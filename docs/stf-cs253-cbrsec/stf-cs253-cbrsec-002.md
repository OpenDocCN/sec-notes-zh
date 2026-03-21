# 002：HTTP、Cookies、会话 🛡️

![](img/0105acc003df6642cebcaaad562a5568_1.png)

![](img/0105acc003df6642cebcaaad562a5568_3.png)

在本节课中，我们将要学习当你在浏览器中输入一个网址并按下回车时，背后发生了什么。我们将深入探讨DNS查询、HTTP协议以及Cookies的工作原理，这些都是构成现代网络体验的基础。理解这些过程对于识别和防范潜在的网络攻击至关重要。

## DNS查询：从域名到IP地址 🌐

上一节我们介绍了课程概述，本节中我们来看看当你在浏览器中输入URL并按下回车后的第一步：DNS查询。DNS系统将用户友好的域名翻译成IP地址。IP地址是互联网上所有连接节点的寻址方式，而域名是我们喜欢在浏览器中输入的人类可读字符串。浏览器需要先完成这个翻译步骤，因为它不知道要联系哪个服务器。

以下是DNS查询过程的详细步骤：

![](img/0105acc003df6642cebcaaad562a5568_5.png)

1.  **客户端查询**：你的浏览器向一个被称为DNS递归解析器的服务器发送一个查询。这个查询本质上是一个问题：“某个特定域名（例如 `stanford.edu`）的IP地址是什么？”
2.  **递归解析**：递归解析器代表客户端承担起查找IP地址的责任。它可能需要进行多步查询。
3.  **查询根域名服务器**：递归解析器首先查询根域名服务器。这些服务器的地址被硬编码在每个DNS解析器中。根服务器不知道具体答案，但它会将权限委托给负责`.edu`这类顶级域名的服务器。
4.  **查询TLD域名服务器**：递归解析器接着向`.edu`顶级域名服务器询问同样的问题。这个服务器同样不保存所有记录，它会指示解析器去询问负责`stanford.edu`的权威域名服务器。
5.  **查询权威域名服务器**：最后，递归解析器向`stanford.edu`的权威域名服务器查询。这个服务器知道确切的答案，并会返回斯坦福大学的IP地址。
6.  **返回结果**：递归解析器将最终得到的IP地址返回给你的浏览器。

现在，我们至少部分回答了“输入URL后会发生什么”这个问题：我们与DNS递归解析器通信，它依次查询根服务器、TLD服务器，最终到达权威服务器并获得IP地址。

![](img/0105acc003df6642cebcaaad562a5568_7.png)

## HTTP协议：客户端与服务器的对话 🔄

![](img/0105acc003df6642cebcaaad562a5568_9.png)

![](img/0105acc003df6642cebcaaad562a5568_11.png)

![](img/0105acc003df6642cebcaaad562a5568_13.png)

现在我们已经有了服务器的IP地址，接下来需要实际连接到它。这就是HTTP协议发挥作用的时候。一旦我们知道了IP地址，浏览器就会向该服务器发起一个HTTP请求并接收响应。

![](img/0105acc003df6642cebcaaad562a5568_15.png)

![](img/0105acc003df6642cebcaaad562a5568_17.png)

![](img/0105acc003df6642cebcaaad562a5568_19.png)

![](img/0105acc003df6642cebcaaad562a5568_21.png)

![](img/0105acc003df6642cebcaaad562a5568_23.png)

给定这个架构，你能想到任何可能的攻击方式吗？例如，攻击一个正在访问`stanford.edu`的访客。

*   **DNS欺骗/劫持**：攻击者可以篡改目标域名的DNS记录，将其指向自己的IP地址。这样，当访客访问该网站时，会被引导到攻击者的服务器。动机包括网络钓鱼（窃取用户密码）、植入JavaScript加密货币挖矿脚本（消耗受害者计算资源牟利），或者仅仅是在页面中插入广告。
*   **HTTP响应篡改**：攻击者可以修改HTTP响应，插入恶意内容。

那么，如何防御这些攻击呢？HTTPS可以提供帮助。HTTPS在HTTP之上增加了TLS加密层，提供了两个关键属性：
*   **完整性**：响应不能被篡改，如果被修改，客户端能够察觉。
*   **真实性**：确保我们正在与我们认为的服务器通信，使用了数字签名。

然而，一个有趣的挑战是：如果攻击者通过DNS劫持控制了域名指向，他们可能能够从证书颁发机构（如Let‘s Encrypt）获得该域名的有效TLS证书。在这种情况下，即使使用了HTTPS，浏览器也会信任攻击者提供的证书，使得攻击难以被察觉。

## DNS攻击与隐私问题 🕵️♂️

除了安全性，DNS也涉及隐私问题。DNS查询是明文传输的。递归解析器可以记录你查询的每一个域名。互联网服务提供商（ISP）曾被发现出售这些查询列表。

你可以通过将计算机的DNS设置更改为承诺不售卖查询数据的服务商（例如Cloudflare的 `1.1.1.1`）来缓解这个问题。但这并不能加密查询本身，你的ISP仍然可以看到你向Cloudflare发送的明文查询。

更彻底的解决方案是 **DNS over HTTPS**。它将DNS查询封装在加密的HTTPS连接中发送，这样网络中的旁观者（包括你的ISP）就无法看到你的查询内容。Firefox等浏览器已开始支持并默认启用此功能。

![](img/0105acc003df6642cebcaaad562a5568_25.png)

## HTTP协议详解 📄

![](img/0105acc003df6642cebcaaad562a5568_27.png)

![](img/0105acc003df6642cebcaaad562a5568_28.png)

让我们更详细地看看HTTP部分。HTTP是一个客户端-服务器模型、基于文本的协议。它的设计简单、可扩展（通过头部字段）且无状态（默认情况下，请求之间没有关联）。

一个HTTP请求的格式如下：
```http
GET /path/to/resource HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
...其他头部...
```
*   **请求行**：包含方法（如GET、POST）、请求的路径和协议版本。
*   **头部字段**：以键值对形式提供附加信息，如`Host`（必需）、`User-Agent`（浏览器标识）、`Referer`（来源页面）等。

服务器响应也包含状态行、头部和正文：
```http
HTTP/1.1 200 OK
Content-Type: text/html
...其他头部...

![](img/0105acc003df6642cebcaaad562a5568_30.png)

![](img/0105acc003df6642cebcaaad562a5568_32.png)

<!DOCTYPE html><html>...</html>
```
*   **状态码**：
    *   `2xx`：成功（如 `200 OK`， `206 Partial Content`用于范围请求）。
    *   `3xx`：重定向（如 `301 Moved Permanently`， `302 Found`， `304 Not Modified`用于缓存验证）。
    *   `4xx`：客户端错误（如 `400 Bad Request`， `403 Forbidden`， `404 Not Found`）。
    *   `5xx`：服务器错误（如 `500 Internal Server Error`， `502 Bad Gateway`， `503 Service Unavailable`）。

HTTP本身是无状态的，但现代网站需要状态（如保持用户登录）。这是通过Cookies实现的。

![](img/0105acc003df6642cebcaaad562a5568_34.png)

## Cookies：在无状态协议上管理状态 🍪

![](img/0105acc003df6642cebcaaad562a5568_36.png)

Cookies是服务器指示浏览器保存的一小段数据。浏览器随后在向同一服务器发送的每个请求中自动附带这个Cookie。

服务器通过响应头 `Set-Cookie` 来设置Cookie：
```http
Set-Cookie: username=alice
```
浏览器收到后，会保存这个Cookie。之后向同一服务器发起请求时，会自动在请求头中加入：
```http
Cookie: username=alice
```
这样，服务器就能识别出用户是“alice”，从而实现会话保持、登录状态管理等功能。

![](img/0105acc003df6642cebcaaad562a5568_38.png)

![](img/0105acc003df6642cebcaaad562a5568_40.png)

## 实践演示：构建一个简单的登录系统 👨💻

为了更直观地理解，我们演示了如何使用Node.js构建一个简单的银行网站登录系统。

![](img/0105acc003df6642cebcaaad562a5568_42.png)

![](img/0105acc003df6642cebcaaad562a5568_44.png)

![](img/0105acc003df6642cebcaaad562a5568_46.png)

![](img/0105acc003df6642cebcaaad562a5568_47.png)

![](img/0105acc003df6642cebcaaad562a5568_49.png)

![](img/0105acc003df6642cebcaaad562a5568_51.png)

![](img/0105acc003df6642cebcaaad562a5568_53.png)

1.  **创建登录页面**：一个包含用户名、密码输入框和提交按钮的HTML表单。
2.  **创建HTTP服务器**：使用Express框架创建一个监听端口4000的服务器。
3.  **处理登录请求**：
    *   服务器在`/login`端点接收POST请求。
    *   验证用户名和密码（在演示中，我们使用一个内存中的“用户数据库”对象）。
    *   如果验证成功，使用`res.cookie(‘username’, username)` 设置一个名为`username`的Cookie，其值为登录的用户名。
    *   发送登录成功或失败的响应。
4.  **识别登录用户**：
    *   在主页请求处理中，使用`req.cookies.username`读取Cookie。
    *   如果存在`username`，则页面显示“Hi, [username]”，否则显示登录表单。

![](img/0105acc003df6642cebcaaad562a5568_55.png)

![](img/0105acc003df6642cebcaaad562a5568_56.png)

![](img/0105acc003df6642cebcaaad562a5568_58.png)

**安全问题暴露**：这个简单实现存在严重漏洞。由于Cookie是存储在客户端的，用户可以轻松地通过浏览器开发者工具修改`username` Cookie的值，例如将`alice`改为`bob`，从而假冒其他用户身份。这清楚地说明了**绝不能信任客户端提供的数据**，尤其是用于身份验证的敏感信息。我们将在后续课程中探讨如何安全地使用Cookies（例如使用签名Cookie、会话ID等）。

![](img/0105acc003df6642cebcaaad562a5568_60.png)

![](img/0105acc003df6642cebcaaad562a5568_62.png)

![](img/0105acc003df6642cebcaaad562a5568_64.png)

![](img/0105acc003df6642cebcaaad562a5568_65.png)

![](img/0105acc003df6642cebcaaad562a5568_67.png)

![](img/0105acc003df6642cebcaaad562a5568_69.png)

## 总结 📝

![](img/0105acc003df6642cebcaaad562a5568_71.png)

![](img/0105acc003df6642cebcaaad562a5568_72.png)

![](img/0105acc003df6642cebcaaad562a5568_74.png)

![](img/0105acc003df6642cebcaaad562a5568_76.png)

本节课中我们一起学习了网络通信的基础流程。我们从输入URL开始，探讨了DNS如何将域名解析为IP地址，并了解了针对DNS的攻击（如劫持）和隐私问题。接着，我们深入研究了HTTP协议，包括请求/响应结构、方法、状态码和头部。最后，我们介绍了Cookies如何用于在无状态的HTTP协议上管理用户状态，并通过一个简单的登录系统演示了其基本用法和潜在的安全风险。理解这些基础组件是学习Web安全的关键第一步。