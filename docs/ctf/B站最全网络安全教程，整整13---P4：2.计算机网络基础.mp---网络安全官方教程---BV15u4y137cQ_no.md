# 网络安全基础：P4：计算机网络基础

## 概述
在本节课中，我们将要学习计算机网络的基础知识，特别是HTTP协议、URL/URI、HTTP请求与响应报文的结构，以及Cookie和Session等核心概念。这些知识是理解Web安全、渗透测试等后续内容的重要基石。

---

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_1.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_3.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_5.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_7.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_9.png)

## 什么是HTTP？ 🌐
HTTP，全称超文本传输协议，是我们浏览网站时最常遇到的协议。标准网址（URL）通常以`http://`或`https://`开头。浏览器在输入域名（如`baidu.com`）时，会自动补全协议部分。

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_11.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_13.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_15.png)

HTTP是一种基于请求与响应、无状态的应用层协议，设计初衷是为了提供发布和接收HTML页面的方法。它基于TCP/IP协议来传输数据。

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_17.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_19.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_21.png)

### HTTP发展历史
HTTP主要版本包括0.9、1.0、1.1和2.0。目前最常见的是HTTP/1.1版本，而HTTP/2.0正逐渐普及。

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_23.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_25.png)

### HTTP工作流程
HTTP的工作遵循客户端-服务器模型。以下是其基本工作流程：

1.  **建立连接**：客户端通过TCP三次握手与服务器建立连接。
2.  **发送请求**：客户端向服务器发送HTTP请求报文。
3.  **处理与响应**：服务器解释请求并处理，然后向客户端发送HTTP响应报文。
4.  **展示内容**：客户端接收响应，并将内容（如HTML页面）展示给用户。
5.  **断开连接**：完成数据传递后，客户端通过TCP四次挥手与服务器断开连接。

**TCP三次握手与四次挥手**是网络基础协议，用于建立和终止可靠连接。

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_27.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_29.png)

### HTTP特性：持续连接
在HTTP/0.9和1.0中，每次请求-响应后TCP连接就会关闭。HTTP/1.1引入了持续连接（Keep-Alive）机制。

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_31.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_32.png)

*   **优点**：减少了重复建立TCP连接所需的等待时间。
*   **机制**：在同一个TCP连接内，可以连续发送多个请求和接收多个响应，而无需重新握手。

---

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_34.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_36.png)

## 统一资源定位符（URL）📍
URL，俗称网址，用于定位互联网上的资源。一个完整的URL包含以下部分：

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_38.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_40.png)

以下是URL各组成部分的详细说明：

*   **协议方案名**：如 `http`、`https`、`ftp`、`file`、`mailto`。
*   **登录信息（认证）**：可选的用户名和密码，格式为 `username:password@`。
*   **服务器地址**：域名或IP地址，如 `www.example.com`。
*   **服务器端口号**：可选，Web服务器默认端口是80（HTTP）或443（HTTPS）。若使用默认端口，可省略。
*   **文件路径**：服务器上资源的具体路径，如 `/path/to/file.html`。
*   **查询字符串**：以 `?` 开头，用于向服务器传递参数，格式为 `key1=value1&key2=value2`。
*   **片段标识符**：以 `#` 开头，用于定位页面内的特定锚点。

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_42.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_44.png)

**示例**：
`https://www.example.com:8080/path/page.html?name=value#section`

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_46.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_48.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_50.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_52.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_54.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_56.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_58.png)

---

## 统一资源标识符（URI）🔍
URI是用来标识抽象或物理资源的紧凑字符串。URL是URI的一个子集。简单理解：URI是一个更大的概念，它包含了URL（定位符）和URN（统一资源名称）等。

**关键点**：HTTP协议使用URI来传输参数和建立连接。

### URI vs URL 辨析
以下是一些例子，帮助区分URI和URL：

*   `ftp://example.com/resource.txt` - **是URL**（有协议和路径）。
*   `mailto:user@example.com` - **是URL**（使用mailto协议）。
*   `http://[2001:db8::1]:8080/index.html` - **是URL**（包含IPv6地址）。
*   `urn:isbn:0451450523` - **是URN（属于URI，但不是URL）**。
*   `/index.html` - **不是完整的URL**（缺少协议和主机）。
*   `index.html` - **不是URL**。

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_60.png)

---

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_62.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_64.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_66.png)

## HTTP请求报文（客户端 -> 服务器）📤
HTTP请求报文由四个部分组成：请求行、请求头部、空行、请求数据。

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_68.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_70.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_72.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_74.png)

### 1. 请求行
请求行格式为：`请求方法 空格 URI 空格 协议版本 CRLF`

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_76.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_78.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_80.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_82.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_84.png)

*   **请求方法**：定义对资源的操作，常见的有：
    *   `GET`：请求获取资源。
    *   `POST`：向指定资源提交数据。
    *   `HEAD`：获取资源的响应头，不返回主体。
    *   `PUT`：上传资源到指定位置。
    *   `DELETE`：删除指定资源。
    *   `OPTIONS`：查询服务器支持的请求方法。
    *   `TRACE`：用于诊断，回显收到的请求。
*   **URI**：请求的资源标识。
*   **协议版本**：如 `HTTP/1.1`。
*   **CRLF**：回车换行符，表示行结束。

### 2. 请求头部
请求头部允许客户端传递关于自身的信息及对响应的期望。格式为：`头部字段: 值 CRLF`

除了 `Host` 头部在HTTP/1.1中是必需的，其他头部都是可选的。

以下是一些常见的请求头部字段：

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_86.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_88.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_89.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_91.png)

*   `Host`：请求的服务器域名和端口。
*   `Content-Length`：请求主体的字节长度。
*   `Content-Type`：请求主体的媒体类型（如 `application/x-www-form-urlencoded`, `multipart/form-data`）。
*   `User-Agent`：客户端软件信息（浏览器、操作系统等）。
*   `Referer`：当前请求页面的来源页地址。
*   `Cookie`：发送给服务器的Cookie信息。
*   `X-Forwarded-For`：常用于标识客户端的原始IP地址，特别是在经过代理时。

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_93.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_95.png)

### 3. 空行
一个空行（CRLF）标志着请求头部的结束，也是请求主体开始的标志。

### 4. 请求数据（主体）
请求数据仅在部分方法（如POST、PUT）中存在。

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_97.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_99.png)

*   **GET请求**：参数通常附加在URL的查询字符串中，格式如 `?key1=value1&key2=value2`。
*   **POST请求**：数据放在请求主体中。常见的 `Content-Type` 有：
    *   `application/x-www-form-urlencoded`：标准表单提交，格式如 `username=admin&password=123`。
    *   `multipart/form-data`：用于文件上传，内容由随机生成的 `boundary` 字符串分隔。

---

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_101.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_103.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_105.png)

## HTTP响应报文（服务器 -> 客户端）📥
HTTP响应报文同样由四个部分组成：状态行、响应头部、空行、响应数据。

### 1. 状态行
状态行格式为：`协议版本 空格 状态码 空格 状态描述 CRLF`

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_107.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_109.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_111.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_112.png)

*   **状态码**：三位数字，表示请求结果。
    *   **1xx**：信息性状态码，请求已被接收，继续处理。
    *   **2xx**：成功状态码，请求已成功被服务器接收、理解、并处理。如 `200 OK`。
    *   **3xx**：重定向状态码，需要后续操作才能完成这一请求。如 `302 Found`（临时重定向）。
    *   **4xx**：客户端错误状态码，请求含有语法错误或无法完成。如 `404 Not Found`。
    *   **5xx**：服务器错误状态码，服务器在处理请求的过程中发生了错误。如 `500 Internal Server Error`。

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_114.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_115.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_117.png)

### 2. 响应头部
格式与请求头部类似，服务器通过它传递附加信息。

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_119.png)

常见的响应头部字段包括：

*   `Server`：服务器软件信息。
*   `Content-Type`：响应主体的媒体类型（如 `text/html`, `application/json`）。
*   `Content-Length`：响应主体的字节长度。
*   `Set-Cookie`：服务器向客户端设置Cookie。
*   `Location`：用于重定向，指定新地址。

### 3. 空行
空行分隔响应头部和响应主体。

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_121.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_123.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_125.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_127.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_129.png)

### 4. 响应数据（主体）
服务器返回的实际内容，如HTML页面、JSON数据等。

---

## HTTP请求方法实践 🔧
上一节我们介绍了HTTP报文的结构，本节中我们通过工具（如Burp Suite）来实际查看和操作这些请求方法。

### 工具准备：Burp Suite
Burp Suite 是一个用于Web安全测试的集成平台。常用模块：
*   **Proxy**：拦截和修改浏览器流量。
*   **Repeater**：手动重放和修改HTTP请求。
*   **HTTP History**：记录所有经过代理的HTTP请求历史。

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_131.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_133.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_135.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_137.png)

**设置代理**：将浏览器代理设置为Burp Suite监听的地址（默认 `127.0.0.1:8080`）。

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_139.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_141.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_143.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_145.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_147.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_149.png)

### 方法演示
1.  **GET**：用于获取资源。请求数据在URL中，无请求主体。
2.  **POST**：用于提交数据。常见于表单登录和文件上传。
    *   **表单登录**：`Content-Type: application/x-www-form-urlencoded`，数据在请求主体中。
    *   **文件上传**：`Content-Type: multipart/form-data`，数据由 `boundary` 分隔。
3.  **HEAD**：与GET类似，但服务器只返回响应头，不返回响应主体。用于检查资源状态。
4.  **OPTIONS**：用于查询目标资源支持的通信选项（请求方法）。响应头中的 `Allow` 字段会列出支持的方法。
5.  **PUT**：请求服务器存储请求主体中的内容到指定的URI位置。
    *   如果URI不存在，则创建文件（状态码201）。
    *   如果URI存在，则替换文件内容（状态码200或204）。
    *   **安全风险**：若服务器配置不当，允许PUT方法可能导致任意文件上传漏洞。
6.  **DELETE**：请求服务器删除指定的资源。
7.  **TRACE**：回显服务器收到的请求，主要用于测试或诊断。

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_151.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_153.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_155.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_157.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_159.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_161.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_163.png)

---

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_165.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_167.png)

## Cookie 与 Session 🍪
HTTP是无状态协议，服务器无法记住用户的上一次请求。这不利于需要状态维持的应用（如购物车、登录状态）。

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_169.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_170.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_172.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_174.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_176.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_178.png)

### Cookie
Cookie是服务器发送到用户浏览器并保存在本地的一小块数据。

*   **作用**：在下一次向同一服务器发起请求时，浏览器会携带Cookie，从而让服务器识别用户身份。
*   **类型**：
    *   **会话Cookie**：存储在内存，浏览器关闭即失效。
    *   **持久Cookie**：存储在硬盘，有过期时间。
*   **工作原理**：
    1.  客户端首次访问，服务器在响应中通过 `Set-Cookie` 头部设置Cookie。
    2.  浏览器保存Cookie。
    3.  后续向同一服务器发起请求时，浏览器自动在请求头 `Cookie` 字段中携带该信息。
*   **缺陷**：明文传输（除非使用HTTPS）、大小限制（约4KB）、可能被窃取。

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_180.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_182.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_184.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_186.png)

### Session
Session是一种在服务端保存用户会话信息的机制。

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_188.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_189.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_191.png)

*   **作用**：在服务端存储特定用户会话所需的信息。
*   **工作原理**：
    1.  客户端首次访问，服务器创建唯一的Session ID。
    2.  服务器通过 `Set-Cookie` 将Session ID发送给客户端。
    3.  客户端后续请求携带此Session ID（通常在Cookie中）。
    4.  服务器通过Session ID找到对应的会话数据。
*   **与Cookie的区别**：
    | 特性 | Cookie | Session |
    | :--- | :--- | :--- |
    | 存储位置 | 客户端浏览器 | 服务器端 |
    | 安全性 | 较低（客户端可见） | 较高（信息在服务器） |
    | 存储内容 | 只能是字符串 | 可以是任意数据类型 |
    | 性能影响 | 不占用服务器资源 | 占用服务器资源 |

---

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_193.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_195.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_197.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_198.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_200.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_202.png)

## HTTPS 🔒
HTTPS是HTTP的安全版本，全称超文本传输安全协议。

*   **构成**：HTTP over SSL/TLS。即在HTTP下层加入了SSL/TLS安全套接层。
*   **目的**：提供加密通信、身份认证和数据完整性保护。
*   **与HTTP的区别**：HTTP是明文传输，HTTPS对传输的数据进行加密。

---

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_204.png)

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_206.png)

## 总结
本节课中我们一起学习了计算机网络的基础知识，重点包括：
1.  **HTTP协议**：理解了其无状态、基于请求-响应的特性，以及工作流程。
2.  **URL与URI**：掌握了网址的完整结构和两者的区别。
3.  **HTTP报文**：深入分析了请求报文（请求行、头、体）和响应报文（状态行、头、体）的构成，以及常见方法（GET, POST, PUT等）和状态码的含义。
4.  **实践工具**：初步了解了使用Burp Suite进行抓包、改包和重放请求的方法。
5.  **状态管理**：学习了Cookie和Session的工作原理及其在维持用户会话中的作用。
6.  **安全传输**：了解了HTTPS的基本概念及其与HTTP的区别。

![](img/684aa0fa15cc9e4d8e93b26de5a3b608_208.png)

这些知识是后续学习Web安全漏洞（如SQL注入、XSS、CSRF等）和进行渗透测试的重要基础。