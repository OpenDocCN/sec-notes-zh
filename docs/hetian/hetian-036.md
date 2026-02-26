#  036：SSRF漏洞 - 简介、利用与防御 🛡️

在本节课中，我们将要学习服务端请求伪造漏洞。这是一种允许攻击者诱使服务器向任意目标发起网络请求的漏洞，常被用作“穿越网络防火墙的通行证”。

## 什么是SSRF？🤔

SSRF，全称 Server-Side Request Forgery，即服务端请求伪造。攻击者可以构造一个由服务器执行的恶意请求链接，从而造成漏洞。该漏洞通常用于外网探测或攻击内网服务。

![](img/77e0765b4691c75db3dabec028e17de8_1.png)

![](img/77e0765b4691c75db3dabec028e17de8_3.png)

![](img/77e0765b4691c75db3dabec028e17de8_4.png)

![](img/77e0765b4691c75db3dabec028e17de8_6.png)

![](img/77e0765b4691c75db3dabec028e17de8_8.png)

![](img/77e0765b4691c75db3dabec028e17de8_10.png)

![](img/77e0765b4691c75db3dabec028e17de8_12.png)

其形成原因大多是：服务端提供了从其他服务器应用获取数据的功能，但没有对目标地址进行严格的过滤和限制。

![](img/77e0765b4691c75db3dabec028e17de8_14.png)

![](img/77e0765b4691c75db3dabec028e17de8_16.png)

![](img/77e0765b4691c75db3dabec028e17de8_18.png)

![](img/77e0765b4691c75db3dabec028e17de8_19.png)

![](img/77e0765b4691c75db3dabec028e17de8_21.png)

![](img/77e0765b4691c75db3dabec028e17de8_22.png)

![](img/77e0765b4691c75db3dabec028e17de8_23.png)

![](img/77e0765b4691c75db3dabec028e17de8_24.png)

![](img/77e0765b4691c75db3dabec028e17de8_26.png)

例如，一个网站加载托管在其他服务器上的图片时，如果未对请求图片资源的链接（URL）进行限制和过滤，就可能存在SSRF漏洞。攻击者可以伪造请求目标，让存在漏洞的服务器向指定的内网IP和端口发起请求。

![](img/77e0765b4691c75db3dabec028e17de8_28.png)

![](img/77e0765b4691c75db3dabec028e17de8_29.png)

最常见的例子是：攻击者传入一个未经验证的URL，后端代码直接请求该URL，从而造成SSRF漏洞。

![](img/77e0765b4691c75db3dabec028e17de8_31.png)

![](img/77e0765b4691c75db3dabec028e17de8_33.png)

## SSRF与CSRF的区别 ⚖️

![](img/77e0765b4691c75db3dabec028e17de8_35.png)

上一节我们介绍了SSRF的基本概念，本节中我们来看看它与另一个常见漏洞CSRF的区别。

![](img/77e0765b4691c75db3dabec028e17de8_37.png)

![](img/77e0765b4691c75db3dabec028e17de8_39.png)

![](img/77e0765b4691c75db3dabec028e17de8_40.png)

![](img/77e0765b4691c75db3dabec028e17de8_42.png)

![](img/77e0765b4691c75db3dabec028e17de8_43.png)

![](img/77e0765b4691c75db3dabec028e17de8_44.png)

![](img/77e0765b4691c75db3dabec028e17de8_46.png)

![](img/77e0765b4691c75db3dabec028e17de8_48.png)

![](img/77e0765b4691c75db3dabec028e17de8_50.png)

![](img/77e0765b4691c75db3dabec028e17de8_52.png)

*   **CSRF**：跨站请求伪造。发生在用户登录网站A后，攻击者诱使用户访问恶意网站B，网站B利用用户的登录状态（如Cookie）向网站A发起非用户本意的请求。其根源是服务端没有对用户提交的数据进行随机值校验，或没有校验HTTP请求头中的Referer字段。
*   **SSRF**：服务端请求伪造。利用的是服务器对用户提供的URL过于信任，没有对该URL进行充分的限制和恶意性检测。攻击者可以利用存在SSRF漏洞的服务器作为跳板，攻击内网或其他服务器。

![](img/77e0765b4691c75db3dabec028e17de8_54.png)

![](img/77e0765b4691c75db3dabec028e17de8_56.png)

![](img/77e0765b4691c75db3dabec028e17de8_58.png)

![](img/77e0765b4691c75db3dabec028e17de8_59.png)

![](img/77e0765b4691c75db3dabec028e17de8_61.png)

## SSRF的攻击危害 💥

![](img/77e0765b4691c75db3dabec028e17de8_63.png)

![](img/77e0765b4691c75db3dabec028e17de8_65.png)

SSRF漏洞的危害主要体现在以下几个方面：

![](img/77e0765b4691c75db3dabec028e17de8_67.png)

![](img/77e0765b4691c75db3dabec028e17de8_69.png)

以下是SSRF漏洞的主要危害：

![](img/77e0765b4691c75db3dabec028e17de8_71.png)

![](img/77e0765b4691c75db3dabec028e17de8_73.png)

![](img/77e0765b4691c75db3dabec028e17de8_75.png)

![](img/77e0765b4691c75db3dabec028e17de8_77.png)

![](img/77e0765b4691c75db3dabec028e17de8_79.png)

1.  **信息收集**：攻击者可以利用SSRF漏洞获取Web应用可达服务器的Banner信息，收集内网服务的指纹信息，以及探测内部网络结构和开放的端口。
2.  **攻击内网服务**：攻击者可以攻击运行在内网的系统或应用程序。由于内网服务的安全防护可能不如外网严格，可能存在未授权访问等漏洞。通过SSRF攻击这些漏洞，可能直接获取Web Shell，从而绕过防火墙进入目标内网。
3.  **读取内部资源与执行攻击**：通过特定的URL协议，攻击者可以读取服务器内部的敏感文件。同时，可以结合内网中存在的其他漏洞（如Redis未授权访问、Struts2命令执行等）执行相应的攻击动作，例如写入Webshell或反弹Shell。

## SSRF漏洞的常见场景 🔍

了解了SSRF的危害后，我们来看看在哪些地方可能找到这种漏洞。

![](img/77e0765b4691c75db3dabec028e17de8_81.png)

![](img/77e0765b4691c75db3dabec028e17de8_83.png)

以下是从Web功能点寻找SSRF漏洞的常见场景：

![](img/77e0765b4691c75db3dabec028e17de8_85.png)

![](img/77e0765b4691c75db3dabec028e17de8_87.png)

![](img/77e0765b4691c75db3dabec028e17de8_89.png)

1.  **通过URL地址分享网页内容**：例如文章分享到微博、QQ空间等功能。这些功能会通过用户提供的URL链接获取目标网站的标题、图片等内容。如果未对用户提供的链接进行过滤，就可能存在SSRF漏洞。
2.  **在线翻译**：例如百度翻译、有道翻译等提供的“翻译网页”功能。该功能会请求用户输入的URL以获取网页内容进行翻译。如果未对URL进行限制，就可能存在SSRF漏洞。
3.  **通过URL地址加载与下载图片**：网站通过用户提供的URL远程加载或下载图片资源。
4.  **图片/文章收藏功能**：例如印象笔记的收藏功能，它会请求用户提供的URL以保存文章内容。

![](img/77e0765b4691c75db3dabec028e17de8_91.png)

![](img/77e0765b4691c75db3dabec028e17de8_93.png)

![](img/77e0765b4691c75db3dabec028e17de8_95.png)

![](img/77e0765b4691c75db3dabec028e17de8_97.png)

![](img/77e0765b4691c75db3dabec028e17de8_99.png)

此外，还可以通过URL中的关键字（如 `url`、`link`、`src` 等）来寻找可能存在SSRF漏洞的参数。

![](img/77e0765b4691c75db3dabec028e17de8_101.png)

![](img/77e0765b4691c75db3dabec028e17de8_103.png)

![](img/77e0765b4691c75db3dabec028e17de8_105.png)

![](img/77e0765b4691c75db3dabec028e17de8_107.png)

**总结来说，SSRF漏洞常出现在两类场景：**
*   能够对外发起网络请求的地方。
*   从远程服务器请求资源（如图片、文本）的功能点。

![](img/77e0765b4691c75db3dabec028e17de8_109.png)

![](img/77e0765b4691c75db3dabec028e17de8_111.png)

![](img/77e0765b4691c75db3dabec028e17de8_113.png)

## SSRF漏洞的利用与测试 🛠️

![](img/77e0765b4691c75db3dabec028e17de8_115.png)

![](img/77e0765b4691c75db3dabec028e17de8_117.png)

![](img/77e0765b4691c75db3dabec028e17de8_119.png)

![](img/77e0765b4691c75db3dabec028e17de8_121.png)

发现潜在的SSRF漏洞后，我们需要利用一些特定的URL协议来进行测试和利用。

以下是SSRF利用中常用的几种URL协议：

1.  **`dict://` 协议**：字典服务器协议。可用于获取目标服务器上特定端口运行服务的版本等Banner信息。
    *   **本地利用示例**：`curl -v “dict://127.0.0.1:3306”` 可以探测本地MySQL服务的版本信息。
    *   **远程利用**：在存在SSRF漏洞的参数中，构造 `dict://内网IP:端口` 的Payload，服务器会请求该端口并返回服务信息。
2.  **`file://` 协议**：文件系统协议。用于读取服务器本地文件系统的内容。
    *   **利用示例**：构造 `file:///etc/passwd` 可以尝试读取Linux系统的密码文件。
3.  **`gopher://` 协议**：一个功能强大的协议，堪称“万金油”。它可以发送预先构造好的数据包，常用于攻击内网服务，如Redis未授权访问。
    *   **利用示例**：`curl “gopher://127.0.0.1:6789/_HI%20Everyone%0AThis%20is%20just%20a%20test”` 会向本地的6789端口发送一段文本数据。

![](img/77e0765b4691c75db3dabec028e17de8_123.png)

## PHP中易导致SSRF的函数 ⚙️

![](img/77e0765b4691c75db3dabec028e17de8_125.png)

![](img/77e0765b4691c75db3dabec028e17de8_127.png)

![](img/77e0765b4691c75db3dabec028e17de8_129.png)

![](img/77e0765b4691c75db3dabec028e17de8_131.png)

![](img/77e0765b4691c75db3dabec028e17de8_133.png)

![](img/77e0765b4691c75db3dabec028e17de8_135.png)

![](img/77e0765b4691c75db3dabec028e17de8_137.png)

在PHP开发中，某些函数使用不当极易引发SSRF漏洞。

![](img/77e0765b4691c75db3dabec028e17de8_139.png)

![](img/77e0765b4691c75db3dabec028e17de8_141.png)

![](img/77e0765b4691c75db3dabec028e17de8_143.png)

以下是三个需要重点关注的PHP函数：

![](img/77e0765b4691c75db3dabec028e17de8_145.png)

![](img/77e0765b4691c75db3dabec028e17de8_146.png)

![](img/77e0765b4691c75db3dabec028e17de8_148.png)

![](img/77e0765b4691c75db3dabec028e17de8_150.png)

![](img/77e0765b4691c75db3dabec028e17de8_152.png)

![](img/77e0765b4691c75db3dabec028e17de8_154.png)

![](img/77e0765b4691c75db3dabec028e17de8_156.png)

![](img/77e0765b4691c75db3dabec028e17de8_158.png)

![](img/77e0765b4691c75db3dabec028e17de8_160.png)

![](img/77e0765b4691c75db3dabec028e17de8_161.png)

![](img/77e0765b4691c75db3dabec028e17de8_162.png)

1.  **`curl_exec()`**：用于执行cURL会话，获取URL内容。如果前端传入的URL参数未经处理直接传给此函数，攻击者可以构造恶意URL进行利用。
2.  **`file_get_contents()`**：将整个文件读入一个字符串。同样，如果参数可控，攻击者可以利用它读取文件或请求URL。**特别需要注意的是**，此函数支持PHP伪协议 `php://filter`，可用来读取网站源码（如 `php://filter/convert.base64-encode/resource=index.php`），避免PHP代码被直接解析执行。
3.  **`fsockopen()`**：打开一个网络连接或Unix套接字连接。此函数主要用于对IP地址或域名进行原始Socket连接，并读取返回内容。它只能处理IP/域名，不能直接处理完整的HTTP URL。

![](img/77e0765b4691c75db3dabec028e17de8_164.png)

![](img/77e0765b4691c75db3dabec028e17de8_166.png)

![](img/77e0765b4691c75db3dabec028e17de8_168.png)

![](img/77e0765b4691c75db3dabec028e17de8_170.png)

![](img/77e0765b4691c75db3dabec028e17de8_172.png)

![](img/77e0765b4691c75db3dabec028e17de8_174.png)

![](img/77e0765b4691c75db3dabec028e17de8_175.png)

![](img/77e0765b4691c75db3dabec028e17de8_177.png)

![](img/77e0765b4691c75db3dabec028e17de8_179.png)

![](img/77e0765b4691c75db3dabec028e17de8_181.png)

![](img/77e0765b4691c75db3dabec028e17de8_183.png)

## 无回显的SSRF（Blind SSRF）与检测方法 🕵️♂️

![](img/77e0765b4691c75db3dabec028e17de8_185.png)

![](img/77e0765b4691c75db3dabec028e17de8_187.png)

![](img/77e0765b4691c75db3dabec028e17de8_189.png)

![](img/77e0765b4691c75db3dabec028e17de8_191.png)

前面介绍的案例都有明显的回显（即请求结果会显示在页面上）。但存在一种情况，服务端虽然发起了请求，却不将结果返回给前端页面，这就是“无回显SSRF”。

![](img/77e0765b4691c75db3dabec028e17de8_193.png)

![](img/77e0765b4691c75db3dabec028e17de8_195.png)

![](img/77e0765b4691c75db3dabec028e17de8_196.png)

![](img/77e0765b4691c75db3dabec028e17de8_197.png)

![](img/77e0765b4691c75db3dabec028e17de8_198.png)

对于无回显的SSRF，我们可以通过以下两种带外通道技术进行检测：

![](img/77e0765b4691c75db3dabec028e17de8_200.png)

![](img/77e0765b4691c75db3dabec028e17de8_202.png)

![](img/77e0765b4691c75db3dabec028e17de8_204.png)

![](img/77e0765b4691c75db3dabec028e17de8_206.png)

![](img/77e0765b4691c75db3dabec028e17de8_207.png)

![](img/77e0765b4691c75db3dabec028e17de8_209.png)

![](img/77e0765b4691c75db3dabec028e17de8_211.png)

1.  **HTTP带外通道**：利用Burp Suite Collaborator客户端或自己的公网服务器。将带有唯一子域名的Collaborator URL或自己的服务器地址作为Payload，如果目标服务器发起了请求，我们就能在Collaborator或服务器日志中看到记录，从而证实漏洞存在。
2.  **DNSLog**：利用DNS解析记录。使用 `dnslog.cn` 等平台提供的子域名作为Payload。如果目标服务器尝试解析该域名，就会在平台留下DNS解析日志，从而判断漏洞存在。

![](img/77e0765b4691c75db3dabec028e17de8_213.png)

![](img/77e0765b4691c75db3dabec028e17de8_215.png)

![](img/77e0765b4691c75db3dabec028e17de8_217.png)

## SSRF的限制与绕过方法 🚧

![](img/77e0765b4691c75db3dabec028e17de8_219.png)

![](img/77e0765b4691c75db3dabec028e17de8_221.png)

![](img/77e0765b4691c75db3dabec028e17de8_223.png)

![](img/77e0765b4691c75db3dabec028e17de8_225.png)

![](img/77e0765b4691c75db3dabec028e17de8_227.png)

服务端可能会对SSRF进行一些防御性限制，例如过滤内网IP、特定协议等。攻击者则需要掌握一些绕过技巧。

![](img/77e0765b4691c75db3dabec028e17de8_229.png)

![](img/77e0765b4691c75db3dabec028e17de8_231.png)

![](img/77e0765b4691c75db3dabec028e17de8_233.png)

![](img/77e0765b4691c75db3dabec028e17de8_235.png)

![](img/77e0765b4691c75db3dabec028e17de8_237.png)

![](img/77e0765b4691c75db3dabec028e17de8_239.png)

以下是几种常见的SSRF限制绕过方法：

![](img/77e0765b4691c75db3dabec028e17de8_241.png)

![](img/77e0765b4691c75db3dabec028e17de8_243.png)

![](img/77e0765b4691c75db3dabec028e17de8_245.png)

![](img/77e0765b4691c75db3dabec028e17de8_247.png)

![](img/77e0765b4691c75db3dabec028e17de8_249.png)

![](img/77e0765b4691c75db3dabec028e17de8_251.png)

![](img/77e0765b4691c75db3dabec028e17de8_253.png)

1.  **利用 `@` 符号**：URL格式 `http://example.com@192.168.1.1`。实际请求的是`@`符号后的IP地址，但前端显示的是`@`前的域名，可用于绕过一些基于域名的过滤。
2.  **IP地址转换进制**：将IP地址转换为八进制、十进制或十六进制格式。例如，`192.168.1.1` 可转换为：
    *   八进制：`0300.0250.01.01`
    *   十进制：`3232235777`
    *   十六进制：`0xC0A80101`
    浏览器在解析时通常会将其还原为标准IP。
3.  **添加端口**：在某些正则匹配不严谨的情况下，为URL添加端口可能绕过过滤，例如 `http://192.168.1.1:80`。
4.  **利用短地址/重定向服务**：使用 `xip.io`、`nip.io` 或URL短链接服务。例如 `192.168.1.1.xip.io` 会解析到 `192.168.1.1`，可以绕过对直接IP地址的过滤。
5.  **利用句号替代点**：在某些解析场景下，`127。0。0。1`（使用中文句号）可能被等价解析为 `127.0.0.1`。

![](img/77e0765b4691c75db3dabec028e17de8_255.png)

![](img/77e0765b4691c75db3dabec028e17de8_257.png)

## SSRF漏洞的防御 🛡️

![](img/77e0765b4691c75db3dabec028e17de8_259.png)

![](img/77e0765b4691c75db3dabec028e17de8_261.png)

![](img/77e0765b4691c75db3dabec028e17de8_263.png)

![](img/77e0765b4691c75db3dabec028e17de8_265.png)

![](img/77e0765b4691c75db3dabec028e17de8_267.png)

![](img/77e0765b4691c75db3dabec028e17de8_269.png)

最后，我们从防御角度总结一下应对SSRF漏洞的措施。

![](img/77e0765b4691c75db3dabec028e17de8_271.png)

![](img/77e0765b4691c75db3dabec028e17de8_272.png)

![](img/77e0765b4691c75db3dabec028e17de8_273.png)

![](img/77e0765b4691c75db3dabec028e17de8_275.png)

![](img/77e0765b4691c75db3dabec028e17de8_277.png)

以下是防御SSRF漏洞的几种有效方法：

1.  **过滤返回信息**：对服务端请求返回的内容进行严格检查，如果包含敏感信息（如内网IP、端口Banner等），则不予显示。
2.  **统一错误信息**：避免通过请求的成功或失败状态（如响应时间、布尔值True/False）来推断内网信息。
3.  **限制请求端口**：只允许访问Web应用需要的常用端口，如80、443、8080等。
4.  **设置IP黑名单**：严格禁止访问内网IP地址段（如 `10.0.0.0/8`， `172.16.0.0/12`， `192.168.0.0/16`）和回环地址（`127.0.0.0/8`）。
5.  **禁用不必要的协议**：仅允许使用HTTP和HTTPS协议，禁用 `file://`、`gopher://`、`dict://` 等危险协议。
6.  **进行白名单校验**：对用户输入的URL进行严格的白名单校验，只允许访问预期的、可信的域名或IP。

![](img/77e0765b4691c75db3dabec028e17de8_279.png)

![](img/77e0765b4691c75db3dabec028e17de8_281.png)

![](img/77e0765b4691c75db3dabec028e17de8_283.png)

![](img/77e0765b4691c75db3dabec028e17de8_285.png)

![](img/77e0765b4691c75db3dabec028e17de8_287.png)

## 总结 📚

![](img/77e0765b4691c75db3dabec028e17de8_289.png)

![](img/77e0765b4691c75db3dabec028e17de8_291.png)

![](img/77e0765b4691c75db3dabec028e17de8_293.png)

![](img/77e0765b4691c75db3dabec028e17de8_295.png)

![](img/77e0765b4691c75db3dabec028e17de8_297.png)

![](img/77e0765b4691c75db3dabec028e17de8_299.png)

![](img/77e0765b4691c75db3dabec028e17de8_301.png)

![](img/77e0765b4691c75db3dabec028e17de8_303.png)

![](img/77e0765b4691c75db3dabec028e17de8_305.png)

![](img/77e0765b4691c75db3dabec028e17de8_307.png)

![](img/77e0765b4691c75db3dabec028e17de8_309.png)

本节课中我们一起学习了SSRF漏洞。我们从SSRF的基本概念和形成原因讲起，理解了它作为“内网通行证”的危害。我们区分了SSRF与CSRF，并列举了SSRF常见的攻击场景和利用方式，包括使用 `dict`、`file`、`gopher` 等协议进行信息探测和攻击。我们还探讨了PHP中易引发SSRF的危险函数，以及如何检测无回显的SSRF。最后，我们学习了攻击者可能使用的绕过技巧，并从开发角度总结了多种有效的防御方案。掌握这些知识，对于挖掘和防范SSRF漏洞至关重要。