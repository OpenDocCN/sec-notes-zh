![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_1.png)

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_3.png)

# 经典15年i春秋渗透测试系统化教程 - P3：课时3 HTTP方法、消息头讲解 🔍

在本节课中，我们将要学习HTTP响应包的构成、HTTP方法的具体含义与潜在风险，以及URL的结构解析。通过抓包分析，我们将理解服务器如何响应请求，并识别其中可能存在的安全配置信息。

上一节我们介绍了HTTP请求包，本节中我们来看看服务器返回的HTTP响应包。

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_5.png)

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_7.png)

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_9.png)

## 1. 分析HTTP响应包 📦

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_11.png)

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_13.png)

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_14.png)

要查看HTTP响应包，可以使用抓包工具。以百度为例，在抓包工具中右键点击捕获到的请求包，选择“发送到Repeater”进行调试。点击“Go”后，即可在右侧看到服务器返回的响应包。

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_16.png)

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_18.png)

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_20.png)

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_22.png)

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_24.png)

响应包的第一行通常是协议状态，例如：
```
HTTP/1.1 200 OK
```
其中，`200` 代表正常响应。

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_26.png)

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_27.png)

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_29.png)

以下是响应包中一些关键字段的含义：

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_31.png)

*   **Server**：此字段显示了服务器使用的Web服务软件及其版本。例如，`Microsoft-IIS/6.0` 通常对应 Windows Server 2003 操作系统；`Microsoft-IIS/7.5` 则可能对应 Windows Server 2008 R2。
*   **X-Powered-By**：此字段可能指明网站使用的后端技术框架，如 `ASP.NET`、`PHP` 等。但需注意，此信息可能不准确或被隐藏。
*   **Content-Length**：表示响应正文的长度（单位：字节）。在SQL注入测试中，通过对比不同请求返回内容的长度差异，有助于判断是否存在注入点。
*   **Content-Type**：定义了响应内容的媒体类型和字符编码，例如 `text/html; charset=utf-8`。
*   **Set-Cookie**：服务器通过此字段向客户端（浏览器）设置Cookie信息。

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_33.png)

## 2. 缓存与优化机制 💾

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_35.png)

服务器响应头中的某些字段用于控制浏览器缓存行为，以优化性能和节省带宽。

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_37.png)

*   **Cache-Control**：指示浏览器是否以及如何缓存响应内容。例如，`no-cache` 表示不应直接使用缓存的副本。
*   **Expires**：指定响应内容的过期时间。在此时间之前，浏览器可以直接使用本地缓存，而无需向服务器重新请求。

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_39.png)

大型网站（如淘宝）会利用这些机制，将LOGO、样式表等不常变化的静态资源缓存在用户本地。当用户再次访问时，浏览器直接从本地加载这些资源，从而减轻服务器压力并加快页面加载速度。

## 3. HTTP方法详解 ⚙️

HTTP协议定义了多种请求方法，最常用的是 `GET` 和 `POST`。然而，服务器可能支持其他方法，其中一些存在安全风险。

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_41.png)

我们可以使用工具（如Burp Suite的Repeater模块）向目标网站发送 `OPTIONS` 请求，来探测服务器允许的HTTP方法。响应头中的 `Allow` 字段会列出所有支持的方法。

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_43.png)

以下是部分HTTP方法及其潜在风险：

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_45.png)

*   **GET**：安全的请求方法，用于获取资源。
*   **POST**：安全的请求方法，用于提交数据。
*   **PUT**：危险方法。允许客户端向指定位置上传文件。攻击者可利用此方法上传Webshell。
*   **DELETE**：危险方法。允许删除服务器上的指定资源。
*   **MOVE**：危险方法。允许移动/重命名服务器上的资源。攻击者可先通过 `PUT` 上传一个文本文件，再使用 `MOVE` 将其重命名为可执行的脚本文件（如 `.asp`），从而完成入侵。

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_46.png)

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_48.png)

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_49.png)

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_50.png)

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_52.png)

在渗透测试中，若发现目标服务器开启了 `PUT`、`DELETE`、`MOVE` 等危险方法，且未做严格权限控制，则可能直接导致服务器被入侵。

## 4. URL结构解析 🔗

URL（统一资源定位符）用于唯一标识互联网上的资源。一个标准的HTTP URL格式如下：
```
协议://主机[:端口]/路径/[?查询参数]
```
例如：
```
http://www.example.com:8080/news/show.php?id=2
```

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_54.png)

以下是各组成部分的说明：

*   **协议**：如 `http`、`https`、`ftp`。
*   **主机**：域名或IP地址，如 `www.example.com`。
*   **端口**：可选。Web服务默认端口是80（HTTP）或443（HTTPS）。如果使用非默认端口（如8080），则需要显式指定。
*   **路径**：对应服务器上网站目录下的文件路径，如 `/news/show.php`。
*   **查询参数**：可选。以 `?` 开头，使用 `&` 连接多个 `参数名=值` 对，如 `?id=2&page=1`。这些参数会被传递给服务器端的程序进行处理，也是SQL注入等漏洞常见的发生点。

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_56.png)

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_58.png)

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_60.png)

理解URL结构有助于在渗透测试中快速定位资源、分析目录层级和构造测试参数。

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_62.png)

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_64.png)

## 5. 消息头总结 📝

HTTP消息头（Header）在请求和响应中承载了元数据信息。

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_66.png)

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_68.png)

*   **请求头**：由客户端发送，包含如 `Host`、`User-Agent`、`Cookie`、`Referer` 等信息，告知服务器关于本次请求的详细信息。
*   **响应头**：由服务器发送，包含如 `Server`、`Set-Cookie`、`Content-Type`、`Cache-Control` 等信息，指导客户端如何处理响应。

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_70.png)

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_72.png)

消息头中的信息对于渗透测试至关重要，它们可能泄露服务器版本、技术栈、会话状态等，为后续的漏洞探测和利用提供线索。

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_74.png)

---

![](img/ac7b6a9b67744dedde2f75f79d4a4ba1_76.png)

本节课中我们一起学习了HTTP响应包的详细结构，了解了缓存机制的工作原理，深入探讨了各类HTTP方法（特别是危险方法）的安全含义，并解析了URL的组成部分。最后，我们对HTTP消息头在请求和响应中的作用进行了总结。掌握这些基础知识是进行Web渗透测试的重要前提。