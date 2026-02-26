# 🛠️ NMAP 的正确烹饪及食用方法——第 34 期大咖倾旋

![](img/a7df427e55f695eab8b0dd8069b1cc49_1.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_3.png)

在本节课中，我们将要学习如何高效地使用 NMAP 这款强大的网络扫描工具，并深入了解其脚本引擎（NSE）的扩展能力。我们将从资产扫描的基本概念讲起，逐步深入到 NMAP 的内部结构、扫描流程，并最终学习如何编写自定义的 NSE 脚本来实现自动化漏洞检测甚至利用。

![](img/a7df427e55f695eab8b0dd8069b1cc49_5.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_7.png)

## 📖 概述：资产扫描与 NMAP 简介

![](img/a7df427e55f695eab8b0dd8069b1cc49_9.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_11.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_13.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_15.png)

资产扫描是网络安全的基础工作。对于厂商而言，资产扫描有助于评估内部网络的安全等级，发现未修补的漏洞，从而降低办公网络的风险。对于安全研究人员（白帽子）而言，则可以在厂商修复漏洞之前，迅速定位测试目标，寻找突破点。

![](img/a7df427e55f695eab8b0dd8069b1cc49_17.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_19.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_21.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_23.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_25.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_27.png)

一个完整的资产扫描流程通常包括：确定目标（如域名或 IP 段）、收集信息（如子域名）、扫描探测端口、生成报告、分析脆弱点并进一步挖掘漏洞。为了提高效率和数据可操作性，扫描结果通常会存入数据库，以便进行灵活的筛选、汇总和导出。

![](img/a7df427e55f695eab8b0dd8069b1cc49_29.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_31.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_33.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_35.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_37.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_39.png)

在众多扫描工具中，NMAP 因其强大和灵活而广为人知。它是一款开源的网络探测和安全审核工具，以其快速的扫描速度、强大的指纹库和可扩展的脚本引擎而著称。

![](img/a7df427e55f695eab8b0dd8069b1cc49_41.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_43.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_45.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_47.png)

## 🔍 NMAP 核心结构与功能

![](img/a7df427e55f695eab8b0dd8069b1cc49_49.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_51.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_53.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_55.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_57.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_59.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_61.png)

上一节我们介绍了资产扫描的基本概念，本节中我们来看看 NMAP 这款工具的具体构成。

![](img/a7df427e55f695eab8b0dd8069b1cc49_63.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_65.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_67.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_69.png)

NMAP 不仅仅是一个端口扫描器，它的强大源于三个核心部分：**底层的扫描引擎**、**丰富的指纹库** 和 **可扩展的脚本引擎（NSE）**。

![](img/a7df427e55f695eab8b0dd8069b1cc49_71.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_73.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_75.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_77.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_79.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_81.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_83.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_85.png)

### 文件目录结构

![](img/a7df427e55f695eab8b0dd8069b1cc49_87.png)

在 Linux 系统中，NMAP 安装后的目录包含多个关键文件：

![](img/a7df427e55f695eab8b0dd8069b1cc49_89.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_91.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_93.png)

*   `nmap.dtd`: 用于定义 XML 输出格式。
*   `nmap-service-probes`, `nmap-os-db` 等: 这些是**指纹库文件**。它们包含了用于识别服务类型、版本甚至操作系统的特征规则。NMAP 通过向目标端口发送特定探测包，并将返回结果与这些指纹进行正则匹配，从而识别出运行的服务和版本。
*   `nmap.xsl`: 用于格式化 XML 输出报告的模板。
*   `nse_main.lua`: 这是 NSE 脚本引擎的**主加载文件**。在执行任何 NSE 脚本之前，NMAP 会先加载并执行这个文件，完成一些初始化工作。
*   `nselib/` 目录: 这是 NSE 的**扩展库目录**。里面包含了大量封装好的函数模块（如 HTTP、MySQL 请求库），极大方便了脚本编写。如果没有这些库，NMAP 的扩展能力将大打折扣。
*   `scripts/` 目录: 存放了所有内置的 **NSE 脚本**，涵盖了漏洞检测、暴力破解、服务发现等多种功能。

![](img/a7df427e55f695eab8b0dd8069b1cc49_95.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_97.png)

### 基本扫描流程

![](img/a7df427e55f695eab8b0dd8069b1cc49_99.png)

NMAP 执行一次扫描，通常遵循以下流程：
1.  **主机发现**: 判断目标主机是否在线（例如通过 TCP SYN、ACK 包或 ICMP 请求）。
2.  **端口扫描**: 探测目标主机上哪些端口是开放的。
3.  **服务与版本侦测**: 利用指纹库，识别开放端口上运行的服务及其具体版本。
4.  **操作系统侦测**: 通过分析 TCP/IP 协议栈的细微差异（如 TTL 值），猜测目标主机的操作系统。
5.  **脚本扫描**: 根据前面步骤的结果，调用相应的 NSE 脚本进行更深层次的检测或利用。

![](img/a7df427e55f695eab8b0dd8069b1cc49_101.png)

## ⚙️ 深入 NSE 脚本引擎

![](img/a7df427e55f695eab8b0dd8069b1cc49_103.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_105.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_107.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_109.png)

了解了 NMAP 的基本结构后，本节我们将重点探讨其最强大的特性——NSE 脚本引擎。

![](img/a7df427e55f695eab8b0dd8069b1cc49_111.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_113.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_115.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_117.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_119.png)

NSE 允许用户使用 Lua 语言编写脚本，对发现的主机和端口进行自定义操作，包括漏洞发现、验证、利用、暴力破解等。这使得 NMAP 从一个扫描工具转变为一款强大的渗透测试工具。

![](img/a7df427e55f695eab8b0dd8069b1cc49_121.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_123.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_125.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_127.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_129.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_131.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_133.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_135.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_137.png)

### NSE 脚本执行流程与 API

![](img/a7df427e55f695eab8b0dd8069b1cc49_139.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_141.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_143.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_145.png)

一个 NSE 脚本的执行由几个特定的函数（规则）控制，它们按顺序被调用：

![](img/a7df427e55f695eab8b0dd8069b1cc49_147.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_149.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_151.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_153.png)

1.  `prerule()`: 在所有扫描开始**之前**执行。常用于初始化全局变量或检查脚本参数。
2.  `hostrule(host)`: 在**主机发现**后，针对每个被发现的主机执行一次。
3.  `portrule(host, port)`: 在**端口扫描**后，针对每个符合条件的主机和端口组合执行一次。这是最常用的规则。
4.  `action(host, port)`: 当 `hostrule` 或 `portrule` 函数返回 `true` 时，会执行对应的 `action` 函数。这里是脚本主要逻辑所在。
5.  `postrule()`: 在所有扫描**之后**执行。常用于最终的数据输出和清理。

![](img/a7df427e55f695eab8b0dd8069b1cc49_155.png)

**核心概念公式**：
```
如果 (hostrule(host) 返回 true) 或 (portrule(host, port) 返回 true)
则 执行 action(host, port)
```

![](img/a7df427e55f695eab8b0dd8069b1cc49_157.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_159.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_161.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_163.png)

### 编写一个简单的 NSE 脚本

![](img/a7df427e55f695eab8b0dd8069b1cc49_165.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_167.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_169.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_171.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_173.png)

以下是创建一个简单测试脚本的步骤，用于理解执行流程：

![](img/a7df427e55f695eab8b0dd8069b1cc49_175.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_177.png)

```lua
-- 定义一个简单的测试脚本 test.nse
prerule = function()
    print("prerule 执行")
end

![](img/a7df427e55f695eab8b0dd8069b1cc49_179.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_181.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_183.png)

portrule = function(host, port)
    -- 只对开放状态的 TCP 端口执行操作
    if port.state == "open" and port.protocol == "tcp" then
        print("端口扫描到: " .. host.ip .. ":" .. port.number)
        return true -- 返回 true 以触发 action
    end
    return false
end

![](img/a7df427e55f695eab8b0dd8069b1cc49_185.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_187.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_189.png)

action = function(host, port)
    print("action 执行于 " .. host.ip .. ":" .. port.number)
    -- 这里可以插入实际的扫描或利用逻辑
end

![](img/a7df427e55f695eab8b0dd8069b1cc49_191.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_193.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_195.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_197.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_199.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_201.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_203.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_205.png)

postrule = function()
    print("postrule 执行")
end
```

![](img/a7df427e55f695eab8b0dd8069b1cc49_207.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_209.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_211.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_213.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_215.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_217.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_219.png)

使用命令运行该脚本：
```bash
nmap --script test.nse -p 80 127.0.0.1
```

![](img/a7df427e55f695eab8b0dd8069b1cc49_221.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_223.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_225.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_227.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_229.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_231.png)

## 🚀 实战演示：10 秒内 Get Shell

![](img/a7df427e55f695eab8b0dd8069b1cc49_233.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_235.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_237.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_239.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_241.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_243.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_245.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_247.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_249.png)

理论需要实践来验证。本节我们将通过一个真实漏洞案例，演示如何利用 NSE 脚本实现快速检测和利用。

![](img/a7df427e55f695eab8b0dd8069b1cc49_251.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_253.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_255.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_257.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_259.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_261.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_263.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_265.png)

我们以 `Struts2 S2-045` 远程代码执行漏洞为例。NMAP 官方已提供了检测脚本 (`http-vuln-cve2017-5638.nse`)，它可以识别目标是否存在此漏洞。

![](img/a7df427e55f695eab8b0dd8069b1cc49_267.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_269.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_271.png)

**然而，我们可以更进一步**，编写一个自定义的利用脚本，在检测到漏洞的同时，直接上传一个 WebShell，实现“10秒内Get Shell”的目标（实际时间取决于网络和参数）。

![](img/a7df427e55f695eab8b0dd8069b1cc49_273.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_275.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_277.png)

以下是该利用脚本的核心逻辑简述：
1.  **漏洞检测**: 向目标发送一个特殊的 HTTP 请求，该请求在 `Content-Type` 头部包含一个带随机字符串的测试载荷。如果响应头中包含相同的字符串，则证明漏洞存在。
2.  **漏洞利用**: 如果漏洞存在，则构造另一个 HTTP 请求包，其中包含能执行系统命令并写入 WebShell 文件的恶意载荷。
3.  **执行命令**: 脚本会发送上传 WebShell 的请求，如果成功，攻击者就能通过访问特定的 URL 来执行命令。

![](img/a7df427e55f695eab8b0dd8069b1cc49_279.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_281.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_283.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_285.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_287.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_289.png)

**演示命令**：
```bash
# 使用自定义的 Struts2 利用脚本，指定要写入的 Webshell 文件名
nmap -p 80 --script struts2-getshell --script-args webshell=colab.jsp 192.168.1.102
```
通过这种方式，NMAP 不再是单纯的扫描器，而成为了自动化攻击链中的一环。

![](img/a7df427e55f695eab8b0dd8069b1cc49_291.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_293.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_295.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_297.png)

## 💡 高级应用与技巧

![](img/a7df427e55f695eab8b0dd8069b1cc49_299.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_301.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_303.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_305.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_307.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_309.png)

掌握了脚本编写基础后，我们可以探索 NMAP 更高级的用法，将其集成到自动化工作流中。

![](img/a7df427e55f695eab8b0dd8069b1cc49_311.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_313.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_315.png)

### 与数据库集成

将 NMAP 扫描结果实时存入数据库（如 MySQL），可以方便地进行后续分析、生成报告和任务调度。我们可以在 `action` 函数中，使用 `nselib` 中的数据库连接库（如 `nselib/mysql.lua`）来执行 SQL 插入操作。

这样，当进行大规模网络扫描时，所有开放的端口、服务、甚至发现的漏洞信息都能被结构化地保存下来。

![](img/a7df427e55f695eab8b0dd8069b1cc49_317.png)

### 构建自动化扫描平台

结合 Python（Django/Flask）、任务队列（如 Celery）和 NMAP，可以构建一个可视化的自动化漏洞扫描平台。
*   **前端**：用户通过网页提交扫描任务（目标、端口、要使用的 NSE 脚本）。
*   **后端**：使用 Python 调用 NMAP 执行扫描，并解析其输出（XML 或数据库）。
*   **任务调度**：利用 Linux 的 `cron` 或 Python 的定时任务框架，实现定期巡检测试。
*   **优势**：利用 NMAP 稳定的底层扫描引擎和丰富的指纹、脚本库，比完全自己实现扫描逻辑更高效、更准确。

![](img/a7df427e55f695eab8b0dd8069b1cc49_319.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_321.png)

## 📚 总结与资源

![](img/a7df427e55f695eab8b0dd8069b1cc49_323.png)

本节课中我们一起学习了 NMAP 工具的深度使用方法：
1.  **资产扫描**的概念和流程，以及 NMAP 在其中扮演的角色。
2.  **NMAP 的核心结构**，包括其扫描引擎、指纹库和至关重要的 NSE 脚本引擎。
3.  **NSE 脚本的编写**，理解了 `prerule`, `hostrule`, `portrule`, `action`, `postrule` 的执行流程和用法。
4.  **实战应用**，通过修改 Struts2 漏洞脚本，演示了如何将 NMAP 用于自动化漏洞利用。
5.  **高级集成**，探讨了将 NMAP 与数据库、自动化平台结合的可能性。

![](img/a7df427e55f695eab8b0dd8069b1cc49_325.png)

**学习资源**：
*   **官方文档**：[NMAP Official Documentation](https://nmap.org/docs.html) 和 [NSE Documentation](https://nmap.org/nsedoc/)，这是最权威的参考资料，尽管主要是英文。
*   **内置脚本**：多研究 `scripts/` 目录下的脚本，这是最好的学习范例。
*   **中文社区**：可以关注国内安全社区、论坛的相关文章和分享，与其他爱好者共同学习进步。

![](img/a7df427e55f695eab8b0dd8069b1cc49_327.png)

![](img/a7df427e55f695eab8b0dd8069b1cc49_329.png)

NMAP 是一款历经时间考验的经典工具，深入理解并掌握其扩展能力，将极大提升你在网络探测和安全评估方面的效率和深度。希望本教程能为你打开一扇新的大门。