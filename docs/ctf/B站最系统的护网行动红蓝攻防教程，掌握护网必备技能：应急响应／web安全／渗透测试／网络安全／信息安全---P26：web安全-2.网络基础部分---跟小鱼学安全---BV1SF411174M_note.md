# 护网行动红蓝攻防教程：P26：Web安全-2.网络基础部分 🛡️

在本节课中，我们将要学习网络基础部分的核心内容，特别是HTTP协议、URL/URI、HTTP请求与响应报文，以及Cookie和Session等概念。这些知识是理解Web安全的基础。

![](img/0dacc5eae12477c9e7c51eaea381c0a6_1.png)

## 什么是HTTP？ 🌐

![](img/0dacc5eae12477c9e7c51eaea381c0a6_3.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_5.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_7.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_9.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_11.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_13.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_15.png)

HTTP，全称超文本传输协议，是用于分布式、协作式和超媒体信息系统的应用层协议。它是一种基于请求与响应、无状态的应用层协议，基于TCP/IP协议来传输数据。

HTTP的设计初衷是为了提供一种发布和接收HTML页面的方法。当我们浏览网站时，就是使用HTTP协议发送请求，并接收服务器返回的HTML页面，从而查看网页内容。

![](img/0dacc5eae12477c9e7c51eaea381c0a6_17.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_19.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_21.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_23.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_25.png)

## HTTP的发展历史 📜

HTTP的发展历史主要经历了多个版本。目前最常见的版本是HTTP/1.1，而HTTP/2版本也逐渐覆盖市场。了解其历史有助于理解协议的演进。

## HTTP的工作流程 🔄

![](img/0dacc5eae12477c9e7c51eaea381c0a6_27.png)

HTTP的工作流程如下：

![](img/0dacc5eae12477c9e7c51eaea381c0a6_29.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_31.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_32.png)

1.  **建立连接**：客户端通过TCP的三次握手与服务器建立连接。
2.  **发送请求**：客户端向服务器发送HTTP请求。
3.  **处理与响应**：服务器解释请求并处理后，向客户端发送HTTP响应。
4.  **断开连接**：客户端通过TCP的四次挥手与服务器断开连接。

关于TCP的三次握手和四次挥手是网络基础协议，如需深入了解，可查阅相关资料。

![](img/0dacc5eae12477c9e7c51eaea381c0a6_34.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_36.png)

## HTTP的特性与持续连接机制 ⚙️

![](img/0dacc5eae12477c9e7c51eaea381c0a6_38.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_40.png)

在HTTP/0.9和1.0版本中，每次请求和响应后TCP连接就会关闭。而在HTTP/1.1版本中，引入了持续连接（Keep-Alive）机制。

该机制允许在多个请求-响应之间重复使用同一个TCP连接，减少了因频繁建立和断开连接而产生的等待时间，从而提升了效率。

![](img/0dacc5eae12477c9e7c51eaea381c0a6_42.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_44.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_46.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_48.png)

## 统一资源定位符（URL） 🔗

![](img/0dacc5eae12477c9e7c51eaea381c0a6_50.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_52.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_54.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_56.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_58.png)

URL，即我们常说的网址，用于定位互联网上的资源。一个完整的URL包含以下几个部分：

以下是URL的组成部分：

*   **协议方案名**：如 `http://`、`https://`、`ftp://`。
*   **登录信息（可选）**：用于认证，格式为 `用户名:密码@`。
*   **服务器地址**：通常是域名，如 `www.example.com`。
*   **服务器端口号（可选）**：Web服务器默认端口是80，可省略。
*   **文件路径**：指定服务器上资源的位置，如 `/path/to/file.html`。
*   **查询字符串（可选）**：以 `?` 开头，传递参数，格式为 `参数名=值&参数名2=值2`。
*   **片段标识符（可选）**：以 `#` 开头，用于定位页面内的特定片段。

例如，一个完整的URL可能看起来像这样：`https://www.example.com:8080/path/index.html?name=value#section`

## 统一资源标识符（URI） 🏷️

![](img/0dacc5eae12477c9e7c51eaea381c0a6_60.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_62.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_64.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_66.png)

URI是用来标识抽象或物理资源的紧凑字符串。URL是URI的一个子集。简单来说，URI是一个更广泛的概念，而URL是用于定位资源的一种特定类型的URI。

![](img/0dacc5eae12477c9e7c51eaea381c0a6_68.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_70.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_72.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_74.png)

HTTP协议使用URI来传输数据和建立连接。

![](img/0dacc5eae12477c9e7c51eaea381c0a6_76.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_78.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_80.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_82.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_84.png)

## HTTP请求报文 📨

客户端向服务器发送的请求消息称为HTTP请求报文，它由四个部分组成：

以下是请求报文的构成：

![](img/0dacc5eae12477c9e7c51eaea381c0a6_86.png)

1.  **请求行**：包含请求方法、请求的URI和HTTP协议版本。格式为：`请求方法 URI 协议版本`。例如：`GET /index.html HTTP/1.1`。
2.  **请求头部**：包含多个字段，传递客户端信息及对响应的期望。常见字段有：
    *   `Host`: 请求的服务器域名和端口。
    *   `Content-Length`: 请求主体的长度。
    *   `Referer`: 表示当前请求是从哪个页面链接过来的。
    *   `User-Agent`: 客户端的浏览器和操作系统信息。
    *   `Cookie`: 发送给服务器的Cookie值。
3.  **空行**：表示请求头部的结束，也是请求正文的开始。
4.  **请求数据（正文）**：可选部分，例如在POST请求中提交的表单数据。

![](img/0dacc5eae12477c9e7c51eaea381c0a6_88.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_89.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_91.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_93.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_95.png)

**GET请求示例**：
```
GET /search?q=keyword HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0
```

**POST请求示例**：
```
POST /login HTTP/1.1
Host: www.example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 27

![](img/0dacc5eae12477c9e7c51eaea381c0a6_97.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_99.png)

username=admin&password=123456
```

## HTTP响应报文 📤

![](img/0dacc5eae12477c9e7c51eaea381c0a6_101.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_103.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_105.png)

服务器返回给客户端的消息称为HTTP响应报文，同样由四个部分组成：

以下是响应报文的构成：

1.  **状态行**：包含HTTP协议版本、状态码和状态描述。例如：`HTTP/1.1 200 OK`。
2.  **响应头部**：包含服务器信息、响应数据的描述等。常见字段有：
    *   `Server`: 服务器软件信息。
    *   `Content-Type`: 响应主体的媒体类型，如 `text/html`。
    *   `Set-Cookie`: 服务器向客户端设置Cookie。
3.  **空行**：表示响应头部的结束。
4.  **响应正文**：服务器返回的实际数据，如HTML页面、JSON数据等。

![](img/0dacc5eae12477c9e7c51eaea381c0a6_107.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_108.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_110.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_112.png)

**响应示例**：
```
HTTP/1.1 200 OK
Server: nginx/1.18.0
Content-Type: text/html; charset=UTF-8
Set-Cookie: sessionId=abc123; Path=/

<!DOCTYPE html><html>...</html>
```

## HTTP状态码 📊

![](img/0dacc5eae12477c9e7c51eaea381c0a6_114.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_116.png)

状态码由三位数字组成，第一位数字定义了响应的类别：

![](img/0dacc5eae12477c9e7c51eaea381c0a6_118.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_120.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_122.png)

*   **1xx（信息）**：请求已被接收，继续处理。
*   **2xx（成功）**：请求已成功被服务器接收、理解并处理。如 `200 OK`。
*   **3xx（重定向）**：需要客户端进一步操作以完成请求。如 `302 Found`（临时重定向）。
*   **4xx（客户端错误）**：请求包含语法错误或无法完成。如 `404 Not Found`。
*   **5xx（服务器错误）**：服务器在处理请求时发生错误。如 `500 Internal Server Error`。

## 其他HTTP请求方法 🛠️

除了常见的GET和POST，HTTP还有其他方法：

![](img/0dacc5eae12477c9e7c51eaea381c0a6_124.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_126.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_128.png)

*   **HEAD**：与GET类似，但只返回响应头部，不返回正文。用于获取资源的元信息。
*   **OPTIONS**：用于查询服务器支持的请求方法。
*   **PUT**：请求服务器存储一个资源，用请求主体内容创建或覆盖目标URI指定的资源。
*   **DELETE**：请求服务器删除指定URI的资源。
*   **TRACE**：用于回显服务器收到的请求，主要用于测试或诊断。

![](img/0dacc5eae12477c9e7c51eaea381c0a6_130.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_132.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_134.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_136.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_138.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_140.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_142.png)

**PUT方法示例（上传文件）**：
```
PUT /uploads/info.php HTTP/1.1
Host: target.com
Content-Length: 20

![](img/0dacc5eae12477c9e7c51eaea381c0a6_144.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_146.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_148.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_150.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_152.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_154.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_156.png)

<?php phpinfo(); ?>
```

![](img/0dacc5eae12477c9e7c51eaea381c0a6_158.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_160.png)

## Cookie与Session 🍪

![](img/0dacc5eae12477c9e7c51eaea381c0a6_162.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_163.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_165.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_167.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_169.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_171.png)

HTTP是无状态协议，服务器不记得用户上一次的操作。Cookie和Session机制用于解决此问题，维持用户状态。

![](img/0dacc5eae12477c9e7c51eaea381c0a6_173.png)

**Cookie**：
*   由服务器生成，发送并存储在客户端（浏览器）。
*   用于辨别用户身份、记录购物车、自动登录等。
*   每次请求会自动附加在请求头中发送给服务器。
*   安全性较低，因为存储在客户端且可能被窃取。

![](img/0dacc5eae12477c9e7c51eaea381c0a6_175.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_177.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_179.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_181.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_182.png)

**Session**：
*   数据存储在服务器端。
*   每个用户会话有一个唯一的Session ID，通常通过Cookie传递给服务器来关联。
*   安全性相对较高，敏感信息不存储在客户端。

![](img/0dacc5eae12477c9e7c51eaea381c0a6_184.png)

**工作原理**：
1.  用户首次访问，服务器创建Session并生成唯一Session ID。
2.  服务器通过 `Set-Cookie` 响应头将Session ID发送给客户端。
3.  客户端保存此Cookie。
4.  后续请求，客户端自动在Cookie头中带上Session ID。
5.  服务器通过Session ID找到对应的Session数据，识别用户。

![](img/0dacc5eae12477c9e7c51eaea381c0a6_186.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_188.png)

## HTTPS 🔒

![](img/0dacc5eae12477c9e7c51eaea381c0a6_190.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_191.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_193.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_195.png)

HTTPS是HTTP的安全版，在HTTP下加入了SSL/TLS层，用于对通信进行加密和身份认证。

*   **加密**：防止数据在传输过程中被窃听或篡改。
*   **认证**：确认通信对方的身份。
*   网址以 `https://` 开头，并且浏览器地址栏通常有锁形图标。

![](img/0dacc5eae12477c9e7c51eaea381c0a6_197.png)

## 总结 📝

![](img/0dacc5eae12477c9e7c51eaea381c0a6_199.png)

![](img/0dacc5eae12477c9e7c51eaea381c0a6_201.png)

本节课中，我们一起学习了Web安全的网络基础核心知识。我们了解了HTTP协议的工作原理、URL/URI的构成、HTTP请求与响应报文的详细结构、各种HTTP方法的应用，以及Cookie和Session如何维持用户状态。最后，我们还简要介绍了HTTPS的重要性。掌握这些基础概念，是进一步学习Web安全漏洞（如XSS、CSRF、SQL注入等）和进行渗透测试的基石。