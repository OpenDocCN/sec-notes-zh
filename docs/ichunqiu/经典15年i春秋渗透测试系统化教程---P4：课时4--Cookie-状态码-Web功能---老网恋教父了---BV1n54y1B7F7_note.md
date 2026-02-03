# 经典15年i春秋渗透测试系统化教程 - P4：课时4 Cookie、状态码与Web功能 🔍

在本节课中，我们将学习Web通信中的三个核心概念：Cookie、HTTP状态码以及Web功能。理解这些基础知识对于后续的渗透测试至关重要。

![](img/6167fbf8dedd7790e5b2eede2d2874e3_1.png)

---

![](img/6167fbf8dedd7790e5b2eede2d2874e3_3.png)

## 什么是Cookie？🍪

![](img/6167fbf8dedd7790e5b2eede2d2874e3_5.png)

上一节我们介绍了HTTP协议的基础，本节中我们来看看Cookie。Cookie是服务器发送到用户浏览器并保存在本地的一小段数据。它的主要作用是维持用户与服务器之间的会话状态。

![](img/6167fbf8dedd7790e5b2eede2d2874e3_7.png)

例如，当用户登录一个网站（如163邮箱）并选择“保存登录状态10天”时，服务器会发送一个包含登录凭证的Cookie到浏览器。在接下来的10天内，浏览器每次访问该网站时，都会自动携带这个Cookie，用户就无需重复输入账号密码。

![](img/6167fbf8dedd7790e5b2eede2d2874e3_9.png)

![](img/6167fbf8dedd7790e5b2eede2d2874e3_11.png)

Cookie通常以“名/值”对的形式存在，例如 `sessionid=abc123`。

### Cookie的存储与查看

![](img/6167fbf8dedd7790e5b2eede2d2874e3_13.png)

![](img/6167fbf8dedd7790e5b2eede2d2874e3_15.png)

Cookie信息保存在用户的本地浏览器中。以下是查看方法：

*   **在浏览器中直接查看**：可以在浏览器的设置或开发者工具中查看当前站点的Cookie。
*   **使用抓包工具查看**：通过Burp Suite或浏览器开发者工具的“网络(Network)”面板，可以清晰地看到每个HTTP请求和响应中携带的Cookie信息。

![](img/6167fbf8dedd7790e5b2eede2d2874e3_17.png)

![](img/6167fbf8dedd7790e5b2eede2d2874e3_1.png)

![](img/6167fbf8dedd7790e5b2eede2d2874e3_19.png)

![](img/6167fbf8dedd7790e5b2eede2d2874e3_21.png)

![](img/6167fbf8dedd7790e5b2eede2d2874e3_3.png)

![](img/6167fbf8dedd7790e5b2eede2d2874e3_23.png)

### Cookie的关键属性

一个Cookie不仅仅包含“名”和“值”，还包含几个重要的属性，用于控制其行为。以下是这些属性的作用：

![](img/6167fbf8dedd7790e5b2eede2d2874e3_25.png)

![](img/6167fbf8dedd7790e5b2eede2d2874e3_27.png)

*   **Expires / Max-Age**：此属性定义了Cookie的过期时间。浏览器会在该时间点后自动删除此Cookie。这就是网站“记住我”或“保持登录N天”功能的实现基础。
    *   **示例**：`Expires=Wed, 21 Oct 2025 07:28:00 GMT`
*   **Domain**：此属性指定了Cookie有效的域名。浏览器只会向匹配该域名或其子域名的请求发送此Cookie。这可以用于防止Cookie被其他域恶意利用（例如，在跨站脚本攻击中）。
    *   **示例**：`Domain=.baidu.com` 表示该Cookie对`baidu.com`及其所有子域名（如`tieba.baidu.com`）都有效。
*   **Path**：此属性指定了Cookie有效的URL路径。只有路径匹配的请求才会携带该Cookie。
    *   **示例**：`Path=/admin` 表示只有访问`/admin`及其子路径时，浏览器才会发送此Cookie。
*   **Secure**：如果设置了这个属性，浏览器只会在通过HTTPS协议发送请求时，才会携带此Cookie。这增加了Cookie传输的安全性。
*   **HttpOnly**：如果设置了这个属性，客户端脚本（如JavaScript）将无法通过`document.cookie`访问此Cookie。这能有效缓解跨站脚本攻击窃取用户Cookie的风险。

![](img/6167fbf8dedd7790e5b2eede2d2874e3_13.png)

![](img/6167fbf8dedd7790e5b2eede2d2874e3_29.png)

![](img/6167fbf8dedd7790e5b2eede2d2874e3_15.png)

---

![](img/6167fbf8dedd7790e5b2eede2d2874e3_31.png)

## 理解HTTP状态码 📊

在渗透测试过程中，分析服务器返回的HTTP状态码是判断请求结果的关键。状态码是一个3位数字代码，位于响应的起始行。

以下是常见的状态码分类及含义：

![](img/6167fbf8dedd7790e5b2eede2d2874e3_33.png)

*   **1xx (信息性状态码)**：表示请求已被接收，需要继续处理。
*   **2xx (成功状态码)**：表示请求已成功被服务器接收、理解并处理。
    *   **`200 OK`**：请求成功。这是最常见的状态码。
*   **3xx (重定向状态码)**：表示需要客户端采取进一步的操作才能完成请求。
    *   **`302 Found`**：临时重定向。请求的资源被暂时移动到了另一个URL。
*   **4xx (客户端错误状态码)**：表示客户端请求有错误，服务器无法处理。
    *   **`404 Not Found`**：服务器找不到请求的资源。
    *   **`403 Forbidden`**：服务器理解请求，但拒绝执行（无权限）。
*   **5xx (服务器错误状态码)**：表示服务器在处理请求时发生了错误。
    *   **`500 Internal Server Error`**：服务器内部错误，无法完成请求。
    *   **`503 Service Unavailable`**：服务器暂时无法处理请求（可能由于过载或维护）。

通过观察状态码，我们可以快速判断请求是否成功到达服务器、是否存在权限问题、资源是否存在或服务器端是否出错。

![](img/6167fbf8dedd7790e5b2eede2d2874e3_35.png)

![](img/6167fbf8dedd7790e5b2eede2d2874e3_31.png)

---

## Web功能与安全 🔒

了解了基础组件后，我们来看看它们如何构成Web功能并影响安全。

![](img/6167fbf8dedd7790e5b2eede2d2874e3_37.png)

### HTTPS与加密传输

对于重要的业务（如登录、支付），现代网站普遍采用HTTPS协议。HTTPS在HTTP基础上加入了SSL/TLS加密层，对传输数据进行加密，防止在传输过程中被窃听或篡改。

*   **HTTP**：`http://example.com`，数据明文传输。
*   **HTTPS**：`https://example.com`，数据加密传输。

在渗透测试中，如果目标使用HTTPS，我们通常需要使用能处理SSL的代理工具（如Burp Suite）进行抓包分析。

![](img/6167fbf8dedd7790e5b2eede2d2874e3_35.png)

![](img/6167fbf8dedd7790e5b2eede2d2874e3_39.png)

### 其他HTTP头部与功能

![](img/6167fbf8dedd7790e5b2eede2d2874e3_41.png)

HTTP请求和响应中包含许多头部字段，它们传达了丰富的元数据。学会解读这些信息对渗透测试有帮助：

![](img/6167fbf8dedd7790e5b2eede2d2874e3_43.png)

![](img/6167fbf8dedd7790e5b2eede2d2874e3_45.png)

*   **`Content-Type`**：指示资源的MIME类型（如 `text/html`, `application/json`）。
*   **`Server`**：透露服务器使用的软件信息（如 `nginx/1.18.0`, `Apache/2.4.41`），这有助于寻找已知漏洞。
*   **`Content-Encoding`**：指示响应体使用的压缩格式（如 `gzip`），以加快传输速度。
*   **`Connection`**：指示本次连接是保持活动状态（`keep-alive`）还是请求后关闭（`close`）。

![](img/6167fbf8dedd7790e5b2eede2d2874e3_47.png)

![](img/6167fbf8dedd7790e5b2eede2d2874e3_39.png)

![](img/6167fbf8dedd7790e5b2eede2d2874e3_49.png)

![](img/6167fbf8dedd7790e5b2eede2d2874e3_51.png)

![](img/6167fbf8dedd7790e5b2eede2d2874e3_41.png)

---

![](img/6167fbf8dedd7790e5b2eede2d2874e3_53.png)

![](img/6167fbf8dedd7790e5b2eede2d2874e3_55.png)

## 手工测试：HTTP方法探测 🛠️

在渗透测试中，除了使用自动化工具，掌握手工测试方法同样重要。一个典型的例子是手工探测服务器支持的HTTP方法。

![](img/6167fbf8dedd7790e5b2eede2d2874e3_57.png)

HTTP方法定义了客户端希望对资源执行的操作。常见的有：
*   **GET**：请求获取资源。
*   **POST**：提交数据。
*   **PUT**：上传/替换资源。
*   **DELETE**：删除资源。
*   **OPTIONS**：询问服务器支持哪些方法。

某些不安全的HTTP方法（如PUT、DELETE）如果被不当开启，可能带来安全风险。

**手工测试步骤（使用Telnet或Netcat）：**

![](img/6167fbf8dedd7790e5b2eede2d2874e3_59.png)

![](img/6167fbf8dedd7790e5b2eede2d2874e3_61.png)

1.  连接到目标服务器的80端口（HTTP）或443端口（HTTPS）。
    ```bash
    nc 目标IP 80
    ```
2.  手动构造一个`OPTIONS`请求来询问服务器支持的方法。
    ```
    OPTIONS / HTTP/1.1
    Host: 目标主机名
    （按两次回车）
    ```
3.  分析服务器的响应。在`Allow`头部中，会列出服务器支持的所有HTTP方法。
    ```
    HTTP/1.1 200 OK
    ...
    Allow: GET, POST, HEAD, OPTIONS
    ...
    ```

![](img/6167fbf8dedd7790e5b2eede2d2874e3_63.png)

如果响应中包含了`PUT`、`DELETE`等方法，则需要进一步测试这些方法是否可能被滥用，例如用于上传恶意文件或删除重要资源。

![](img/6167fbf8dedd7790e5b2eede2d2874e3_65.png)

![](img/6167fbf8dedd7790e5b2eede2d2874e3_63.png)

![](img/6167fbf8dedd7790e5b2eede2d2874e3_65.png)

![](img/6167fbf8dedd7790e5b2eede2d2874e3_67.png)

![](img/6167fbf8dedd7790e5b2eede2d2874e3_68.png)

![](img/6167fbf8dedd7790e5b2eede2d2874e3_70.png)

---

![](img/6167fbf8dedd7790e5b2eede2d2874e3_72.png)

## 总结 📝

![](img/6167fbf8dedd7790e5b2eede2d2874e3_74.png)

本节课中我们一起学习了Web渗透测试的基础核心知识：

![](img/6167fbf8dedd7790e5b2eede2d2874e3_76.png)

1.  **Cookie**：理解了其作为会话管理机制的作用、存储位置、查看方式以及`Expires`、`Domain`、`Secure`、`HttpOnly`等关键安全属性的含义。
2.  **HTTP状态码**：掌握了1xx到5xx状态码的分类，并能解读`200`、`302`、`404`、`403`、`500`等常见状态码的含义，从而快速判断请求状态。
3.  **Web功能与安全**：认识了HTTPS对传输数据的加密保护作用，并学会了查看`Content-Type`、`Server`等HTTP头部以获取服务器信息。
4.  **手工测试**：实践了通过手工发送`OPTIONS`请求来探测服务器支持的HTTP方法，这是渗透测试中信息收集的重要一环。

![](img/6167fbf8dedd7790e5b2eede2d2874e3_78.png)

![](img/6167fbf8dedd7790e5b2eede2d2874e3_79.png)

掌握这些概念是读懂HTTP通信、分析Web应用行为并进行有效安全测试的基石。在后续课程中，我们将基于这些知识，深入探讨具体的漏洞原理与利用技术。