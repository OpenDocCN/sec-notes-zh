# 护网行动红蓝攻防教程：P46：SSRF简介

![](img/ca1281ffca33d8bf60a85b197d3035f7_1.png)

在本节课中，我们将要学习服务端请求伪造（SSRF）漏洞。这是一种攻击者能够诱使服务器向任意地址发起网络请求的漏洞，常被用作攻击内网服务的跳板。我们将从基本概念、漏洞成因、常见场景、利用方法以及PHP中的相关函数等方面进行系统介绍。

![](img/ca1281ffca33d8bf60a85b197d3035f7_3.png)

## 什么是SSRF漏洞？

上一节我们介绍了课程概述，本节中我们来看看SSRF漏洞的具体定义。

SSRF是Server-Side Request Forgery的缩写，中文译为服务端请求伪造。该漏洞的形成是因为服务器提供了从其他服务器应用获取数据的功能，但没有对用户提交的目标URL地址进行严格的过滤与限制。

攻击者可以构造一个恶意的请求链接，传递给存在漏洞的服务器执行。服务器会以自身的身份和权限，向攻击者指定的内部或外部地址发起请求，从而造成危害。

**核心公式**可以概括为：
`漏洞成因 = 服务器信任用户输入的URL + 缺乏对目标地址的过滤/限制`

![](img/ca1281ffca33d8bf60a85b197d3035f7_5.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_7.png)

## SSRF与CSRF的区别

![](img/ca1281ffca33d8bf60a85b197d3035f7_9.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_11.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_13.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_15.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_17.png)

了解了SSRF的基本概念后，我们来看看它和另一个常见漏洞CSRF有何不同。

![](img/ca1281ffca33d8bf60a85b197d3035f7_19.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_21.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_22.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_24.png)

SSRF与CSRF（跨站请求伪造）名称相似，但有本质区别：
*   **CSRF**：攻击发生在**客户端（浏览器）**。利用的是**服务器对用户浏览器身份的过度信任**。攻击者诱骗已认证的用户访问恶意页面，从而以该用户的身份执行非本意的操作（如转账、改密）。
*   **SSRF**：攻击发生在**服务端**。利用的是**服务器对用户提供的URL地址的过度信任**。攻击者控制服务器向任意地址发起请求，常被用作攻击内网、探测端口的跳板。

简单来说，CSRF是“借用户之手”，SSRF是“借服务器之手”。

## SSRF的攻击危害

![](img/ca1281ffca33d8bf60a85b197d3035f7_26.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_28.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_30.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_32.png)

SSRF漏洞的危害性很大，主要体现在以下几个方面：

![](img/ca1281ffca33d8bf60a85b197d3035f7_34.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_36.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_38.png)

以下是SSRF漏洞可能造成的几种危害：
1.  **探测内网信息**：利用存在漏洞的服务器作为代理，扫描和探测目标内网的IP、端口及服务Banner信息，绘制内网拓扑。
2.  **攻击内网应用**：直接攻击运行在内网中、但对外不可达的脆弱系统或应用（如Redis未授权访问、Struts2命令执行等），获取服务器权限。
3.  **读取本地文件**：利用`file://`等协议，读取服务器本地的敏感文件（如`/etc/passwd`）。
4.  **绕过访问控制**：作为穿越网络防火墙的“通行证”，访问原本受保护的内网资源。

![](img/ca1281ffca33d8bf60a85b197d3035f7_40.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_41.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_43.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_45.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_47.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_49.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_51.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_53.png)

## SSRF的常见场景

![](img/ca1281ffca33d8bf60a85b197d3035f7_55.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_56.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_58.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_60.png)

那么，在实际的Web应用中，我们可以在哪些功能点寻找SSRF漏洞呢？

![](img/ca1281ffca33d8bf60a85b197d3035f7_62.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_64.png)

以下是可能产生SSRF漏洞的几种常见功能场景：
*   **网页内容分享功能**：例如“分享到微博/QQ空间”等功能，后端会抓取用户提供的URL的标题、图片等内容。如果对URL未加限制，就可能存在SSRF。
*   **在线翻译功能**：例如百度翻译的“翻译网页”功能，后端需要抓取用户输入的URL内容进行翻译。
*   **图片/文件加载与下载**：通过URL地址远程加载或下载图片、文档等资源的功能。
*   **文章收藏功能**：例如收藏到印象笔记，后端会抓取目标文章的内容。
*   **从URL关键字寻找**：在请求参数中寻找如`url`、`link`、`src`、`target`等关键字，其后面跟随的地址可能就是服务器会请求的目标。

![](img/ca1281ffca33d8bf60a85b197d3035f7_66.png)

**总结**：任何**能够对外发起网络请求**或**从远程服务器请求资源**的地方，都可能存在SSRF漏洞。

![](img/ca1281ffca33d8bf60a85b197d3035f7_68.png)

## SSRF漏洞的利用

![](img/ca1281ffca33d8bf60a85b197d3035f7_70.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_72.png)

上一节我们介绍了漏洞可能出现的场景，本节中我们来看看如何利用这些漏洞，以及会用到哪些关键协议。

![](img/ca1281ffca33d8bf60a85b197d3035f7_74.png)

### 常用协议介绍

在SSRF利用中，我们经常借助一些URL协议来达到不同的攻击目的。

以下是SSRF利用中常用的几种协议：
1.  **`dict://`协议**：字典服务器协议。可用于探测目标主机端口的Banner信息，获取服务版本。
    *   **本地示例**：`curl -v 'dict://127.0.0.1:3306'` 可以探测本地MySQL服务的版本信息。
    *   **远程利用**：在存在SSRF的点，提交 `dict://<内网IP>:<端口>`，服务器会返回该端口服务的Banner信息。
2.  **`file://`协议**：文件系统协议。用于读取服务器本地文件系统的内容。
    *   **示例**：`file:///etc/passwd` 可以尝试读取Linux系统的密码文件。
3.  **`gopher://`协议**：一个功能强大的协议，支持发送多种格式的请求数据包，是攻击内网服务的“万金油”。
    *   **示例**：`curl 'gopher://your-vps-ip:6789/_HI%20Everyone%0AThis%20is%20just%20a%20test'` 可以向你的服务器(6789端口)发送一段文本。在SSRF中，常用来构造攻击Redis等服务的Payload。

### PHP中易导致SSRF的函数

![](img/ca1281ffca33d8bf60a85b197d3035f7_76.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_77.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_79.png)

在PHP开发中，某些函数的不当使用极易引发SSRF漏洞。

![](img/ca1281ffca33d8bf60a85b197d3035f7_81.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_82.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_84.png)

以下是三个需要重点关注的PHP函数：
1.  **`curl_exec()`**：执行一个cURL会话。cURL支持多种协议（`http`、`https`、`file`、`gopher`、`dict`、`ftp`等）。如果用户可控的URL直接传入此函数，且未做过滤，则攻击者可以利用cURL支持的所有协议进行攻击。
    *   **测试**：可先用`curl -V`查看当前cURL支持的协议列表。
2.  **`file_get_contents()`**：将整个文件读入一个字符串。此函数也支持PHP伪协议。
    *   **PHP伪协议利用**：当目标文件是`.php`等会被解析的文件时，直接读取只会得到解析后的输出。使用 `php://filter/convert.base64-encode/resource=index.php` 可以以Base64编码形式读取PHP文件的源代码，避免被解析。
3.  **`fsockopen()`**：打开一个网络连接或Unix套接字连接。此函数主要用于对域名或IP地址发起原始的网络连接，常用于端口探测。

![](img/ca1281ffca33d8bf60a85b197d3035f7_86.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_88.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_90.png)

### 无回显SSRF的探测

![](img/ca1281ffca33d8bf60a85b197d3035f7_92.png)

有时，服务器虽然发起了请求，但不会将响应内容返回给前端页面，形成“盲SSRF”。这时我们需要其他方法来判断请求是否成功。

![](img/ca1281ffca33d8bf60a85b197d3035f7_94.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_95.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_97.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_99.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_101.png)

以下是两种探测盲SSRF的常用方法：
1.  **HTTP带外通道 (OAST)**：利用外部平台接收请求记录。
    *   **Burp Suite Collaborator**：Burp Suite提供的服务，会生成一个临时域名（如 `xxxxxx.oastify.com`）。将此类域名作为SSRF的测试目标，如果漏洞存在，服务器会向该域名发起DNS解析和HTTP请求，这些记录会在Burp Collaborator客户端显示。
2.  **DNSLog**：利用DNS查询记录来验证。
    *   **原理**：让存在漏洞的服务器去解析一个我们独有的子域名（如 `payload.dnslog.cn`）。只要服务器尝试解析该域名，就会在DNSLog平台上留下查询记录，从而证明漏洞存在。

![](img/ca1281ffca33d8bf60a85b197d3035f7_103.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_105.png)

## 本节课总结

![](img/ca1281ffca33d8bf60a85b197d3035f7_107.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_109.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_111.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_113.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_115.png)

![](img/ca1281ffca33d8bf60a85b197d3035f7_117.png)

本节课中我们一起学习了服务端请求伪造（SSRF）漏洞。
我们首先了解了SSRF的定义、成因及其与CSRF的区别。然后，探讨了SSRF可能造成的多种危害，并梳理了在Web应用中寻找该漏洞的常见功能场景。
在利用部分，我们介绍了`dict`、`file`、`gopher`等关键协议的作用，分析了PHP中`curl_exec()`、`file_get_contents()`、`fsockopen()`等易导致SSRF的函数。最后，针对无回显的“盲SSRF”，介绍了利用Burp Collaborator和DNSLog进行探测的方法。
SSRF是进入内网的重要突破口，理解其原理和利用方式对于红队攻防至关重要。下节课我们将继续学习如何绕过服务端对SSRF的限制，并进行更深入的实战利用。