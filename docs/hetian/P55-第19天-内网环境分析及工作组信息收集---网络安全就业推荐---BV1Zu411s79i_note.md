![](img/42c7c9f7d4c9df23e26387e6bf740647_1.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_3.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_5.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_7.png)

# 🛡️ 网络安全就业推荐 - P55：第19天：内网环境分析及工作组信息收集

在本节课中，我们将学习内网渗透测试中的核心环节：内网环境分析与工作组信息收集。我们将从横向与纵向渗透的概念入手，逐步讲解如何利用已获取权限的机器（跳板机）进行内网信息收集，并介绍一系列在Windows环境下常用的信息收集命令和技巧。

---

![](img/42c7c9f7d4c9df23e26387e6bf740647_9.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_11.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_13.png)

## 🔍 横向与纵向渗透概念

![](img/42c7c9f7d4c9df23e26387e6bf740647_15.png)

上一节我们介绍了内网渗透的基本概念，本节中我们来看看横向与纵向渗透的具体含义。

![](img/42c7c9f7d4c9df23e26387e6bf740647_17.png)

横向渗透是指通过已控制的跳板机，访问并渗透该机器能够直接或间接访问到的其他内网主机。其范围不局限于同一C段，而是跳板机网络可达的所有机器。

![](img/42c7c9f7d4c9df23e26387e6bf740647_19.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_21.png)

纵向渗透则是在横向渗透的基础上，通过新控制的内网主机，去访问和渗透原先跳板机无法直接访问的其他网络区域或机器。这通常意味着网络层次的深入。

![](img/42c7c9f7d4c9df23e26387e6bf740647_23.png)

**核心思路**：渗透思路不应局限于单一网段。路由器、交换机等网络设备可能连接着不同的网段，其配置信息是重要的突破口。

![](img/42c7c9f7d4c9df23e26387e6bf740647_25.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_27.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_29.png)

---

![](img/42c7c9f7d4c9df23e26387e6bf740647_31.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_33.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_34.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_36.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_38.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_40.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_42.png)

## 👤 用户信息收集

![](img/42c7c9f7d4c9df23e26387e6bf740647_44.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_46.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_48.png)

在获取目标系统权限后，首要任务是收集用户信息，以了解系统权限结构和潜在的攻击路径。

![](img/42c7c9f7d4c9df23e26387e6bf740647_50.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_52.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_54.png)

以下是常用的用户信息收集命令：

![](img/42c7c9f7d4c9df23e26387e6bf740647_56.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_58.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_60.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_62.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_64.png)

*   **`net user`**：查看本地计算机上的用户账户列表。
*   **`net localgroup administrators`**：查看本地管理员组中的成员，即拥有管理员权限的用户。
*   **`query user` 或 `qwinsta`**：查看当前在线登录的用户会话。
*   **`whoami`**：查看当前命令执行上下文所使用的用户名。
*   **`whoami /priv`**：查看当前用户在目标系统中的具体权限。

![](img/42c7c9f7d4c9df23e26387e6bf740647_65.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_67.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_69.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_71.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_72.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_73.png)

通过以上命令，可以清晰地勾勒出目标主机的用户权限图谱。

![](img/42c7c9f7d4c9df23e26387e6bf740647_75.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_77.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_79.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_81.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_83.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_85.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_87.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_89.png)

---

![](img/42c7c9f7d4c9df23e26387e6bf740647_91.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_93.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_95.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_97.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_99.png)

## 💻 系统信息收集

![](img/42c7c9f7d4c9df23e26387e6bf740647_101.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_103.png)

了解目标系统的详细配置，是寻找漏洞和规划后续攻击步骤的基础。

![](img/42c7c9f7d4c9df23e26387e6bf740647_105.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_107.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_109.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_111.png)

以下是系统信息收集的关键命令：

![](img/42c7c9f7d4c9df23e26387e6bf740647_113.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_115.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_117.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_119.png)

*   **`ipconfig /all`**：获取详细的网络配置信息，包括IP地址、DNS服务器（常指向域控制器）、主机名、DNS后缀（可判断是否加入域）等。
    ```bash
    ipconfig /all
    ```
*   **`systeminfo | findstr /B /C:"OS Name" /C:"OS Version"`**：查找操作系统名称和版本。
*   **`wmic os get Caption,Version,OSArchitecture`**：通过WMI查询系统版本和架构。
    ```bash
    wmic os get Caption,Version,OSArchitecture
    ```
*   **`wmic service list brief`**：列出系统服务及其状态。
*   **`wmic product get name,version`**：查看已安装的软件及版本。
*   **`tasklist`**：查看系统当前运行的进程列表。
*   **`wmic startup get caption,command`**：查看启动程序信息。
*   **`schtasks /query /fo LIST /v`**：查看计划任务（新版系统）。对于旧系统（如Server 2003），可使用 `at` 命令。若执行报错，可尝试先执行 `chcp 437` 更改控制台编码。
*   **`netstat -ano`**：查看网络连接、监听端口及对应进程PID。
    ```bash
    netstat -ano | findstr :445
    ```
*   **`systeminfo`**：查看系统摘要，包括已安装的补丁列表。
*   **`wmic /namespace:\\root\securitycenter2 path antivirusproduct get displayname`**：检查安装了哪些杀毒软件。
*   **`type C:\Windows\System32\drivers\etc\hosts`**：查看hosts文件内容。

收集这些信息有助于识别未打补丁的系统漏洞、存在漏洞的服务或软件，以及安全防护措施（如杀软）。

---

![](img/42c7c9f7d4c9df23e26387e6bf740647_121.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_123.png)

## 📁 共享、网络与其他信息收集

内网中，共享资源、网络代理配置等都可能成为横向移动的跳板。

以下是相关信息的收集方法：

![](img/42c7c9f7d4c9df23e26387e6bf740647_125.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_126.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_128.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_130.png)

*   **`net share`**：查看本机共享列表。
*   **`net use`**：查看当前的网络连接和映射的驱动器。
    *   **磁盘映射示例**：若已知目标机`\\192.168.1.10`的账号密码，可将其C盘映射到本地Z盘。
        ```bash
        net use Z: \\192.168.1.10\C$ /user:username password
        ```
*   **代理信息**：检查系统是否配置了代理。
    ```bash
    reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\Internet Settings" /v ProxyServer
    ```
*   **`netsh wlan show profile name="WiFi名称" key=clear`**：查看已连接Wi-Fi的明文密码。
*   **回收站内容获取**：通过脚本遍历用户SID，查找回收站目录（`$Recycle.Bin`），恢复被删除的文件信息。文件`$I*`存储原路径，`$R*`存储文件内容。
*   **浏览器记录**：可尝试读取Chrome等浏览器的历史记录和Cookie文件（路径如`%LocalAppData%\Google\Chrome\User Data\Default\History`），但需注意加密和权限。可使用Mimikatz等工具的`dpapi`模块解密。

![](img/42c7c9f7d4c9df23e26387e6bf740647_132.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_134.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_136.png)

---

![](img/42c7c9f7d4c9df23e26387e6bf740647_138.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_140.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_142.png)

## ⚙️ 自动化信息收集工具

![](img/42c7c9f7d4c9df23e26387e6bf740647_144.png)

手动执行命令效率较低，可以利用脚本或框架进行批量自动化收集。

![](img/42c7c9f7d4c9df23e26387e6bf740647_146.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_148.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_150.png)

*   **PowerShell脚本**：例如 `Get-Information.ps1` 等脚本，可一键收集大量系统信息。
*   **Metasploit (MSF) 后渗透模块**：
    *   `run post/windows/gather/enum_logged_on_users`
    *   `run post/windows/gather/checkvm`
    *   `run scraper` / `run winenum`：这两个模块能自动运行多个命令，综合收集信息。

![](img/42c7c9f7d4c9df23e26387e6bf740647_152.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_154.png)

![](img/42c7c9f7d4c9df23e26387e6bf740647_156.png)

---

![](img/42c7c9f7d4c9df23e26387e6bf740647_158.png)

## 🏁 总结

本节课中我们一起学习了内网渗透中工作组环境下的信息收集。我们从横向与纵向渗透的概念出发，详细讲解了如何利用已控主机，通过一系列Windows命令收集用户、系统、网络、共享、安全策略等多维度信息。掌握这些基础命令和收集思路，是进行有效内网横向移动和深度渗透的前提。课后请务必在实验环境中多加练习，熟悉每条命令的输出和含义。

![](img/42c7c9f7d4c9df23e26387e6bf740647_160.png)

---
**注意**：本教程所有内容仅用于合法授权的安全测试与学习研究。未经授权对任何系统进行渗透测试均属违法行为。