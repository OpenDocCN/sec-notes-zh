# 内网横向移动技术详解（P80）🚀

![](img/a6be19247068b456aa8c71b93958e237_0.png)

![](img/a6be19247068b456aa8c71b93958e237_2.png)

![](img/a6be19247068b456aa8c71b93958e237_4.png)

![](img/a6be19247068b456aa8c71b93958e237_5.png)

![](img/a6be19247068b456aa8c71b93958e237_7.png)

![](img/a6be19247068b456aa8c71b93958e237_9.png)

在本节课中，我们将学习内网横向移动的几种核心方法，包括使用smbexec、psexec、wmiexec以及sharpRDP等工具。我们将深入探讨这些工具的原理、使用方法，以及在Metasploit（MSF）和Cobalt Strike（CS）平台下的具体操作。课程内容旨在让初学者能够理解并掌握这些技术。

![](img/a6be19247068b456aa8c71b93958e237_11.png)

![](img/a6be19247068b456aa8c71b93958e237_13.png)

![](img/a6be19247068b456aa8c71b93958e237_15.png)

![](img/a6be19247068b456aa8c71b93958e237_16.png)

![](img/a6be19247068b456aa8c71b93958e237_17.png)

![](img/a6be19247068b456aa8c71b93958e237_18.png)

![](img/a6be19247068b456aa8c71b93958e237_19.png)

![](img/a6be19247068b456aa8c71b93958e237_20.png)

---

![](img/a6be19247068b456aa8c71b93958e237_21.png)

![](img/a6be19247068b456aa8c71b93958e237_22.png)

![](img/a6be19247068b456aa8c71b93958e237_24.png)

![](img/a6be19247068b456aa8c71b93958e237_26.png)

![](img/a6be19247068b456aa8c71b93958e237_28.png)

![](img/a6be19247068b456aa8c71b93958e237_29.png)

## 1. WMIExec脚本使用回顾 🔄

上一节我们介绍了WMIExec脚本的基本使用。本节中，我们来看看如何通过该脚本执行命令并获取结果。

通过脚本的`shell`参数，我们可以直接获得一个CMD会话。以下是其基本使用演示：

*   **执行命令**：脚本能够在远程主机上执行指定的系统命令。
*   **获取回显**：执行命令后，结果会回显到控制台。
*   **反弹Shell**：我们可以将执行的命令更改为反弹Shell的命令。例如，使用PowerShell脚本通过`mshta`方法加载，从而在CS服务端接收到来自目标主机的会话。

**核心命令示例（概念性）**：
```bash
# 假设的wmiexec脚本调用格式
python wmiexec.py 域名/用户名:密码@目标IP "执行的命令"
```

同理，在MSF框架中，也可以利用相应的模块实现类似功能。

---

## 2. MSF中的横向移动模块 🛠️

![](img/a6be19247068b456aa8c71b93958e237_31.png)

![](img/a6be19247068b456aa8c71b93958e237_33.png)

![](img/a6be19247068b456aa8c71b93958e237_35.png)

在MSF框架中，集成了多种用于横向移动的模块，简化了我们的操作。以下是三个关键模块的介绍：

### 2.1 psexec_command
此模块的功能是远程执行命令。其原理与我们之前讨论的psexec类似，但已集成到MSF中。我们只需设置好要执行的命令（例如`whoami`），模块便会通过psexec在目标主机上执行并返回结果。

### 2.2 psexec_payload
此模块与`psexec_command`的不同之处在于，它可以让我们直接获得一个Meterpreter会话。我们通过配置相应的payload，便能在执行后获得目标主机的会话控制权。

![](img/a6be19247068b456aa8c71b93958e237_37.png)

![](img/a6be19247068b456aa8c71b93958e237_39.png)

![](img/a6be19247068b456aa8c71b93958e237_41.png)

### 2.3 smb_ms17_010_command
此模块利用SMB协议（网络文件共享协议）的漏洞（如MS17-010）来执行命令。要访问远程主机的SMB服务，通常需要提供账号密码进行身份验证。在已知凭证的情况下，我们可以利用此模块执行命令或获取会话。

![](img/a6be19247068b456aa8c71b93958e237_43.png)

![](img/a6be19247068b456aa8c71b93958e237_45.png)

**模块调用流程（概念性）**：
1.  `use exploit/windows/smb/psexec`
2.  `set RHOSTS [目标IP]`
3.  `set SMBUser [用户名]`
4.  `set SMBPass [密码或哈希]`
5.  `set payload [选择payload]`
6.  `exploit`

![](img/a6be19247068b456aa8c71b93958e237_47.png)

![](img/a6be19247068b456aa8c71b93958e237_48.png)

![](img/a6be19247068b456aa8c71b93958e237_50.png)

![](img/a6be19247068b456aa8c71b93958e237_52.png)

---

![](img/a6be19247068b456aa8c71b93958e237_54.png)

![](img/a6be19247068b456aa8c71b93958e237_56.png)

## 3. Token窃取原理与应用 🔑

![](img/a6be19247068b456aa8c71b93958e237_58.png)

![](img/a6be19247068b456aa8c71b93958e237_60.png)

Token（令牌）是Windows系统中权限访问控制的核心。掌握Token窃取技术对于权限提升和横向移动至关重要。

![](img/a6be19247068b456aa8c71b93958e237_62.png)

![](img/a6be19247068b456aa8c71b93958e237_64.png)

![](img/a6be19247068b456aa8c71b93958e237_66.png)

### 3.1 Token简介
Windows主要有两种令牌：
*   **委托令牌（Delegation Token）**：用于交互式会话登录（如远程桌面登录）。
*   **模拟令牌（Impersonation Token）**：用于非交互式登录（如通过`net use`访问文件共享）。

![](img/a6be19247068b456aa8c71b93958e237_68.png)

![](img/a6be19247068b456aa8c71b93958e237_70.png)

![](img/a6be19247068b456aa8c71b93958e237_72.png)

![](img/a6be19247068b456aa8c71b93958e237_74.png)

![](img/a6be19247068b456aa8c71b93958e237_76.png)

关键点：用户登录后生成的令牌在系统重启前一直有效。具有委托令牌的用户注销后，其令牌会转变为依然有效的模拟令牌。

### 3.2 在MSF中进行Token窃取
MSF集成了`incognito`模块来实现Token窃取。以下是基本使用步骤：

![](img/a6be19247068b456aa8c71b93958e237_78.png)

首先，加载模块并查看帮助：
```
load incognito
help incognito
```

![](img/a6be19247068b456aa8c71b93958e237_80.png)

![](img/a6be19247068b456aa8c71b93958e237_82.png)

![](img/a6be19247068b456aa8c71b93958e237_83.png)

![](img/a6be19247068b456aa8c71b93958e237_85.png)

常用命令列表：
*   `list_tokens -u`：列出当前主机上存在的用户令牌。
*   `impersonate_token [令牌名]`：模拟指定的令牌，获得其权限。
*   `steal_token [PID]`：窃取指定进程ID（PID）对应的用户令牌权限。
*   `rev2self` / `drop_token`：返回窃取令牌之前的原始权限状态。

**操作示例**：
1.  通过`ps`命令查看进程列表，找到具有高权限（如域管理员）用户令牌的进程。
2.  使用`steal_token [高权限进程PID]`窃取该令牌。
3.  使用`getuid`命令验证当前权限已变更为高权限用户。
4.  此后，便可以此高权限身份进行横向移动等操作。

---

## 4. Cobalt Strike（CS）中的横向移动实践 ⚔️

在CS中，我们可以利用图形化界面和强大的功能进行高效的横向移动。

### 4.1 信息收集：凭证与主机发现
进行横向移动前，需要收集凭证和存活主机信息。

以下是获取凭证的步骤：
1.  在已有的会话上右键，选择 `Access` -> `Dump Hashes` 或直接在Beacon中执行 `hashdump` 命令，获取密码哈希。
2.  执行 `logonpasswords` 命令（或通过 `mimikatz` 模块）尝试获取明文密码。
3.  获取的凭证会存储在 `View` -> `Credentials` 中。

![](img/a6be19247068b456aa8c71b93958e237_87.png)

![](img/a6be19247068b456aa8c71b93958e237_89.png)

![](img/a6be19247068b456aa8c71b93958e237_91.png)

![](img/a6be19247068b456aa8c71b93958e237_92.png)

以下是进行主机探测的步骤：
1.  使用 `portscan` 功能对目标网段进行扫描（如使用ARP扫描方式），探测存活主机及开放端口（如445， 3389）。
2.  扫描结果会在 `Targets` 视图中列出，已获得会话的主机会加粗显示。

![](img/a6be19247068b456aa8c71b93958e237_93.png)

![](img/a6be19247068b456aa8c71b93958e237_95.png)

![](img/a6be19247068b456aa8c71b93958e237_96.png)

![](img/a6be19247068b456aa8c71b93958e237_98.png)

![](img/a6be19247068b456aa8c71b93958e237_100.png)

![](img/a6be19247068b456aa8c71b93958e237_102.png)

### 4.2 使用PsExec横向移动
在发现目标主机后，可以利用获取的凭证进行横向移动。

![](img/a6be19247068b456aa8c71b93958e237_104.png)

![](img/a6be19247068b456aa8c71b93958e237_106.png)

![](img/a6be19247068b456aa8c71b93958e237_108.png)

操作流程如下：
1.  在 `Targets` 视图中的目标主机上右键，选择 `Jump` -> `psexec`。
2.  在弹出的窗口中，选择之前获取到的有效凭证（如域管理员账号和明文密码）。
3.  选择用于接收反弹Shell的监听器（Listener）和当前的跳板机会话（Session）。
4.  点击攻击后，若成功，会在CS中接收到目标主机的新会话，并且通常是System权限。

![](img/a6be19247068b456aa8c71b93958e237_110.png)

### 4.3 在CS中进行Token窃取
CS同样支持Token窃取，通常需要上传`incognito`等工具到目标主机执行。

![](img/a6be19247068b456aa8c71b93958e237_112.png)

![](img/a6be19247068b456aa8c71b93958e237_114.png)

常用操作列表：
*   **列举令牌**：`execute-assembly Incognito.exe list_tokens -u`
*   **制作令牌**：`make_token 域名\用户名 密码` （此命令使用已知明文密码创建令牌）
*   **窃取令牌**：`steal_token [PID]` （需先通过 `ps` 命令查看进程PID）
*   **令牌清除**：`rev2self`

![](img/a6be19247068b456aa8c71b93958e237_116.png)

![](img/a6be19247068b456aa8c71b93958e237_117.png)

![](img/a6be19247068b456aa8c71b93958e237_118.png)

**图形化操作**：在CS的 `Explore` -> `Process List` 中，可以图形化查看进程列表，并在高权限进程上直接选择 `Steal Token`，其本质是执行了上述窃取命令。

![](img/a6be19247068b456aa8c71b93958e237_120.png)

![](img/a6be19247068b456aa8c71b93958e237_122.png)

![](img/a6be19247068b456aa8c71b93958e237_124.png)

通过Token窃取获得高权限（如域管理员）后，可以更方便地访问域内其他资源（如域控共享）或发起新的横向移动。

![](img/a6be19247068b456aa8c71b93958e237_126.png)

---

![](img/a6be19247068b456aa8c71b93958e237_128.png)

![](img/a6be19247068b456aa8c71b93958e237_130.png)

## 5. 使用SharpRDP进行横向移动 🖥️

![](img/a6be19247068b456aa8c71b93958e237_132.png)

![](img/a6be19247068b456aa8c71b93958e237_134.png)

![](img/a6be19247068b456aa8c71b93958e237_136.png)

![](img/a6be19247068b456aa8c71b93958e237_138.png)

SharpRDP是一个利用远程桌面协议（RDP）进行身份验证后命令执行的工具。它提供了一种横向移动的替代方法。

其基本使用方法如下：
```
SharpRDP.exe computername=目标IP command="执行命令" username=域名\用户 password=密码
```

![](img/a6be19247068b456aa8c71b93958e237_140.png)

**使用示例**：
我们可以将`command`参数设置为反弹Shell的命令（例如通过PowerShell或mshta加载CS的Payload），从而在目标主机上执行并获得会话。需要注意的是，该工具本身不提供回显，主要用于在远程主机上执行命令。

**工具编译说明**：
许多安全工具（如SharpRDP）在GitHub上以源代码形式提供。对于C#项目，我们需要使用Visual Studio进行编译：
1.  使用Git克隆项目到本地。
2.  用Visual Studio打开解决方案文件（`.sln`）。
3.  在解决方案资源管理器中右键项目，选择“重新生成解决方案”。
4.  编译后的可执行文件通常位于 `项目目录\bin\Release\` 下。

![](img/a6be19247068b456aa8c71b93958e237_142.png)

![](img/a6be19247068b456aa8c71b93958e237_144.png)

![](img/a6be19247068b456aa8c71b93958e237_146.png)

掌握从源代码编译工具是一项基本且重要的技能。

![](img/a6be19247068b456aa8c71b93958e237_148.png)

![](img/a6be19247068b456aa8c71b93958e237_150.png)

![](img/a6be19247068b456aa8c71b93958e237_152.png)

---

![](img/a6be19247068b456aa8c71b93958e237_154.png)

![](img/a6be19247068b456aa8c71b93958e237_156.png)

![](img/a6be19247068b456aa8c71b93958e237_158.png)

![](img/a6be19247068b456aa8c71b93958e237_160.png)

## 总结 📚

![](img/a6be19247068b456aa8c71b93958e237_162.png)

![](img/a6be19247068b456aa8c71b93958e237_164.png)

本节课我们一起深入学习了内网横向移动的多种关键技术：
1.  **回顾了WMIExec**的使用，包括命令执行和反弹Shell。
2.  **掌握了MSF框架中**的`psexec`、`smb`漏洞利用等横向移动模块。
3.  **深入理解了Token窃取**的原理，并在MSF和CS平台上实践了列举、模拟、窃取令牌的操作。
4.  **系统演练了在Cobalt Strike中**进行信息收集（凭证、主机）、利用PsExec横向移动以及Token窃取的全过程。
5.  **了解了SharpRDP工具**的用途和基本使用方法，并学习了如何编译C#项目源代码。

![](img/a6be19247068b456aa8c71b93958e237_166.png)

这些技术是内网渗透测试中突破边界、扩大战果的核心手段。请务必在授权和合法环境下进行练习，以巩固所学知识。