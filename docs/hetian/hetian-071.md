#  071：Cobalt Strike提权模块 🚀

![](img/e2b6ad23cf71015f63c465600c231882_1.png)

在本节课中，我们将学习如何利用Cobalt Strike框架中的提权模块，将已获得的普通用户权限提升至更高权限（如SYSTEM）。课程将涵盖漏洞原理、模块配置、脚本加载以及多种提权方法的具体操作。

![](img/e2b6ad23cf71015f63c465600c231882_2.png)

![](img/e2b6ad23cf71015f63c465600c231882_3.png)

![](img/e2b6ad23cf71015f63c465600c231882_5.png)

![](img/e2b6ad23cf71015f63c465600c231882_7.png)

![](img/e2b6ad23cf71015f63c465600c231882_9.png)

![](img/e2b6ad23cf71015f63c465600c231882_11.png)

---

![](img/e2b6ad23cf71015f63c465600c231882_13.png)

![](img/e2b6ad23cf71015f63c465600c231882_15.png)

## 概述

![](img/e2b6ad23cf71015f63c465600c231882_17.png)

上一节我们介绍了Windows服务路径漏洞的原理。本节中，我们来看看如何在Cobalt Strike中利用相关模块和脚本实现权限提升。我们将重点学习内置提权模块、外部EXP利用以及PowerShell脚本的集成使用。

---

![](img/e2b6ad23cf71015f63c465600c231882_19.png)

![](img/e2b6ad23cf71015f63c465600c231882_21.png)

![](img/e2b6ad23cf71015f63c465600c231882_23.png)

## 服务路径漏洞利用回顾与Cobalt Strike集成

如果目标服务的可执行程序路径没有被引号包裹，系统在启动服务时，会按空格分割路径字符串，并依次查找前面部分对应的可执行程序。它会一层层目录查找，直到找到最终的服务程序。

![](img/e2b6ad23cf71015f63c465600c231882_25.png)

![](img/e2b6ad23cf71015f63c465600c231882_26.png)

![](img/e2b6ad23cf71015f63c465600c231882_28.png)

![](img/e2b6ad23cf71015f63c465600c231882_29.png)

![](img/e2b6ad23cf71015f63c465600c231882_30.png)

利用此漏洞进行提权的方法是：找到该路径中某个我们有写入权限的目录。如果有写入权限，就可以尝试写入一个恶意程序。

![](img/e2b6ad23cf71015f63c465600c231882_32.png)

![](img/e2b6ad23cf71015f63c465600c231882_34.png)

![](img/e2b6ad23cf71015f63c465600c231882_35.png)

例如，尝试写入一个15872字节的程序到 `C:\Windows\Product` 目录下，生成程序名为 `common.exe`。生成此程序后，需要等待服务重新启动。因为漏洞触发需要服务重启并加载可执行程序路径时，才会查找到我们写入的木马程序并加载执行。

![](img/e2b6ad23cf71015f63c465600c231882_37.png)

由于我们是普通用户，没有权限启动该服务，所以利用模块会处于等待状态。我们可以通过模拟服务启动来触发。以下是利用过程的关键步骤：

![](img/e2b6ad23cf71015f63c465600c231882_39.png)

![](img/e2b6ad23cf71015f63c465600c231882_41.png)

1.  **查找可利用服务**：模块会扫描目标机器上存在漏洞的服务。
2.  **写入恶意程序**：将生成的Stage（Payload）写入有权限的目录。
3.  **等待服务重启**：等待服务重启以触发漏洞。

![](img/e2b6ad23cf71015f63c465600c231882_43.png)

![](img/e2b6ad23cf71015f63c465600c231882_45.png)

![](img/e2b6ad23cf71015f63c465600c231882_47.png)

![](img/e2b6ad23cf71015f63c465600c231882_48.png)

**注意**：成功利用后，我们得到的会话（Session）是基于 `common.exe` 这个程序的。由于它并非真正的服务程序，无法与服务控制器正常通信，进程很快会被终止。因此，我们需要在会话建立后立即进行进程迁移（Migrate），将会话迁移到一个稳定的系统进程（如 `explorer.exe`）中，以维持持久访问。

![](img/e2b6ad23cf71015f63c465600c231882_50.png)

![](img/e2b6ad23cf71015f63c465600c231882_52.png)

---

![](img/e2b6ad23cf71015f63c465600c231882_54.png)

![](img/e2b6ad23cf71015f63c465600c231882_56.png)

## Cobalt Strike 高级会话配置

![](img/e2b6ad23cf71015f63c465600c231882_58.png)

![](img/e2b6ad23cf71015f63c465600c231882_60.png)

在Cobalt Strike中，我们可以通过高级配置来优化提权操作。以下是关键配置项：

![](img/e2b6ad23cf71015f63c465600c231882_62.png)

![](img/e2b6ad23cf71015f63c465600c231882_64.png)

*   **`exitonsession`**：此选项默认为 `true`，意味着在得到一个会话后，后台的Job（任务）会自动退出。通常我们将其设置为 `false`，让Job持续运行，确保Payload执行后能稳定建立会话。
*   **`autorunscript`**：此选项用于设置自动运行脚本。在得到新会话后，会自动执行此处设置的命令。例如，我们可以设置为 `migrate -f`，这样在获得会话的瞬间，就会自动将会话迁移到其他进程，防止因进程退出而丢失权限。
*   **`servercms` 模块**：此模块对应“不安全的服务权限”漏洞的利用。
*   **`alwaysinstallelevated` 模块**：此模块对应“始终以高权限安装MSI”策略的漏洞利用。如果目标策略配置了此项，任何用户都能以SYSTEM权限安装MSI文件。我们只需生成一个包含Payload的MSI文件并上传执行，即可获得SYSTEM权限的会话。

![](img/e2b6ad23cf71015f63c465600c231882_66.png)

![](img/e2b6ad23cf71015f63c465600c231882_68.png)

---

![](img/e2b6ad23cf71015f63c465600c231882_70.png)

## 使用外部提权EXP（以CVE-2019-0803为例）

![](img/e2b6ad23cf71015f63c465600c231882_72.png)

![](img/e2b6ad23cf71015f63c465600c231882_74.png)

![](img/e2b6ad23cf71015f63c465600c231882_76.png)

![](img/e2b6ad23cf71015f63c465600c231882_78.png)

除了内置模块，我们还可以上传并使用独立的提权EXP程序。例如，利用CVE-2019-0803（Win32k漏洞）的EXP程序。

![](img/e2b6ad23cf71015f63c465600c231882_80.png)

![](img/e2b6ad23cf71015f63c465600c231882_82.png)

该EXP影响范围很广，包括Windows 7到Windows 10以及多个Server版本。使用方法通常是在命令行后接 `cmd` 选项和要执行的命令。

![](img/e2b6ad23cf71015f63c465600c231882_84.png)

![](img/e2b6ad23cf71015f63c465600c231882_86.png)

**操作步骤**：
1.  在Cobalt Strike的会话中，通过文件管理器将EXP程序（如 `exp.exe`）上传到目标机器。
2.  通过 `execute` 命令或Shell执行该EXP。例如：`exp.exe cmd "whoami"`，如果成功，会返回 `nt authority\system`。
3.  要获得一个SYSTEM权限的Beacon，可以先生成一个反向Shell的Payload（如 `shell.exe`），然后通过EXP以SYSTEM权限执行它：`exp.exe cmd "start C:\path\to\shell.exe"`。执行后，Cobalt Strike的监听器就会收到一个SYSTEM权限的会话。

![](img/e2b6ad23cf71015f63c465600c231882_88.png)

![](img/e2b6ad23cf71015f63c465600c231882_90.png)

---

![](img/e2b6ad23cf71015f63c465600c231882_92.png)

![](img/e2b6ad23cf71015f63c465600c231882_94.png)

![](img/e2b6ad23cf71015f63c465600c231882_96.png)

![](img/e2b6ad23cf71015f63c465600c231882_98.png)

## 集成PowerShell提权脚本（PowerUp）

![](img/e2b6ad23cf71015f63c465600c231882_100.png)

![](img/e2b6ad23cf71015f63c465600c231882_101.png)

![](img/e2b6ad23cf71015f63c465600c231882_103.png)

Cobalt Strike可以通过 `powershell-import` 加载PowerShell脚本。PowerUp是一个著名的提权辅助脚本，能全面检测系统潜在的提权漏洞。

![](img/e2b6ad23cf71015f63c465600c231882_105.png)

![](img/e2b6ad23cf71015f63c465600c231882_107.png)

![](img/e2b6ad23cf71015f63c465600c231882_109.png)

![](img/e2b6ad23cf71015f63c465600c231882_111.png)

![](img/e2b6ad23cf71015f63c465600c231882_113.png)

![](img/e2b6ad23cf71015f63c465600c231882_115.png)

**操作步骤**：
1.  使用 `powershell-import` 命令加载本地的PowerUp脚本。
2.  使用 `powershell` 命令调用脚本中的模块。例如，执行 `Invoke-AllChecks` 命令，脚本会自动运行所有检查模块。
3.  脚本会输出详细的检测结果，例如：
    *   可写的服务路径（`Unquoted Service Paths`）。
    *   启用的“始终以高权限安装”策略（`AlwaysInstallElevated`）。
    *   其他配置错误或弱权限点。
    根据输出信息，我们可以手动或结合其他模块进行利用。

![](img/e2b6ad23cf71015f63c465600c231882_117.png)

![](img/e2b6ad23cf71015f63c465600c231882_119.png)

---

![](img/e2b6ad23cf71015f63c465600c231882_121.png)

![](img/e2b6ad23cf71015f63c465600c231882_122.png)

![](img/e2b6ad23cf71015f63c465600c231882_124.png)

![](img/e2b6ad23cf71015f63c465600c231882_126.png)

![](img/e2b6ad23cf71015f63c465600c231882_128.png)

## 加载与使用第三方Aggressor脚本（如SweetPotato）

![](img/e2b6ad23cf71015f63c465600c231882_130.png)

![](img/e2b6ad23cf71015f63c465600c231882_132.png)

![](img/e2b6ad23cf71015f63c465600c231882_133.png)

![](img/e2b6ad23cf71015f63c465600c231882_135.png)

![](img/e2b6ad23cf71015f63c465600c231882_137.png)

![](img/e2b6ad23cf71015f63c465600c231882_139.png)

Aggressor脚本可以极大扩展Cobalt Strike的功能。例如，`SweetPotato`（土豆家族提权工具）就有对应的Aggressor脚本版本。

![](img/e2b6ad23cf71015f63c465600c231882_141.png)

![](img/e2b6ad23cf71015f63c465600c231882_143.png)

**操作步骤**：
1.  在Cobalt Strike的脚本管理器（`Script Manager`）中，点击 `Load`，选择后缀为 `.cna` 的脚本文件进行加载。
2.  加载成功后，在Beacon的右键菜单 `Access` -> `Elevate` 中，会出现新的提权选项（如 `SweetPotato`）。
3.  选择该选项并指定监听器，如果目标存在相应漏洞，执行后即可获得一个SYSTEM权限的会话。

![](img/e2b6ad23cf71015f63c465600c231882_144.png)

![](img/e2b6ad23cf71015f63c465600c231882_146.png)

![](img/e2b6ad23cf71015f63c465600c231882_147.png)

![](img/e2b6ad23cf71015f63c465600c231882_149.png)

![](img/e2b6ad23cf71015f63c465600c231882_151.png)

![](img/e2b6ad23cf71015f63c465600c231882_152.png)

---

![](img/e2b6ad23cf71015f63c465600c231882_154.png)

![](img/e2b6ad23cf71015f63c465600c231882_156.png)

![](img/e2b6ad23cf71015f63c465600c231882_158.png)

![](img/e2b6ad23cf71015f63c465600c231882_160.png)

![](img/e2b6ad23cf71015f63c465600c231882_162.png)

## Cobalt Strike通信机制简要说明

![](img/e2b6ad23cf71015f63c465600c231882_164.png)

为了更好地理解操作，简要说明Cobalt Strike的通信模型：
*   **Team Server**：服务端，管理者多个监听器（Listener）。
*   **Beacon**：植入在目标机器上的Payload，与Team Server通信。
*   **通信方式**：通常使用HTTP/HTTPS或DNS等协议。Beacon会按照设定的睡眠时间（`sleep`），定期向Team Server发送请求（如GET请求）来获取待执行的命令。执行结果再通过POST等方式回传。设置较短的 `sleep` 时间会使交互更实时，但也更容易被检测。

![](img/e2b6ad23cf71015f63c465600c231882_166.png)

![](img/e2b6ad23cf71015f63c465600c231882_168.png)

![](img/e2b6ad23cf71015f63c465600c231882_170.png)

![](img/e2b6ad23cf71015f63c465600c231882_171.png)

![](img/e2b6ad23cf71015f63c465600c231882_173.png)

![](img/e2b6ad23cf71015f63c465600c231882_175.png)

![](img/e2b6ad23cf71015f63c465600c231882_177.png)

---

![](img/e2b6ad23cf71015f63c465600c231882_179.png)

![](img/e2b6ad23cf71015f63c465600c231882_180.png)

![](img/e2b6ad23cf71015f63c465600c231882_182.png)

![](img/e2b6ad23cf71015f63c465600c231882_184.png)

## 总结

![](img/e2b6ad23cf71015f63c465600c231882_186.png)

本节课我们一起学习了在Cobalt Strike框架中进行权限提升的多种方法：
1.  利用内置的 `elevate` 模块进行提权。
2.  通过高级会话配置（`autorunscript`）实现自动化进程迁移，维持权限稳定。
3.  上传并利用外部提权EXP程序（如CVE-2019-0803）。
4.  集成PowerShell脚本（如PowerUp）进行全面的漏洞检测。
5.  加载第三方Aggressor脚本（如SweetPotato）来扩展提权能力。

![](img/e2b6ad23cf71015f63c465600c231882_188.png)

![](img/e2b6ad23cf71015f63c465600c231882_190.png)

掌握这些方法，能帮助我们在渗透测试中更有效地将普通用户权限提升至SYSTEM权限，为后续的横向移动和权限维持打下基础。课后请大家务必在实验环境中亲自操作一遍，以加深理解。