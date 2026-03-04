# Web安全：P27：XSS跨站脚本讲解 🎯

![](img/0de0e932140d9d4a1ff678a63a8f68ef_1.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_2.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_4.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_6.png)

## 概述
在本节课中，我们将要学习跨站脚本攻击（XSS）的核心概念、类型、危害、利用方式以及防御方法。XSS是一种常见的Web安全漏洞，攻击者通过注入恶意脚本代码，在受害者的浏览器中执行，以达到窃取信息、钓鱼等目的。

---

![](img/0de0e932140d9d4a1ff678a63a8f68ef_8.png)

## 什么是跨站脚本攻击（XSS）❓
跨站脚本攻击是攻击者通过网站注入点注入客户端可执行解析的Payload（脚本代码）。当用户（受害者）访问被注入了有害代码的网页时，恶意的Payload会自动加载并执行，以达到攻击者的目的。攻击者写在代码里的逻辑可能是为了窃取Cookie、进行恶意传播或钓鱼欺骗等。为了避免与HTML语言中的CSS相混淆，我们通常称它为XSS。网站注入点需要攻击者自己寻找，并非每个地方都能进行注入。注入到客户端可执行解析的脚本代码在XSS中通常是JavaScript代码。

![](img/0de0e932140d9d4a1ff678a63a8f68ef_10.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_12.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_14.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_16.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_18.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_20.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_21.png)

---

![](img/0de0e932140d9d4a1ff678a63a8f68ef_23.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_24.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_26.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_28.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_30.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_32.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_34.png)

## XSS的危害 ⚠️
XSS攻击可以造成多种危害，以下是主要的几种：

![](img/0de0e932140d9d4a1ff678a63a8f68ef_36.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_38.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_40.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_42.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_44.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_46.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_47.png)

1.  **获取用户信息**：攻击者可以获取用户的浏览器信息、真实IP地址或Cookie信息。如果得到Cookie信息，就可以实现无密码登录，进入用户账号并窃取个人信息。
2.  **钓鱼攻击**：攻击者可以利用XSS漏洞构造一个登录框，骗取用户的账户和密码。例如，可以提示用户“登录已过期”，然后用JavaScript代码模拟网站的登录框，将用户名和密码发送到攻击者的服务器。
3.  **注入木马或广告链接**：攻击者可能在主站注入非法网站的链接，影响主站公司的声誉。
4.  **后台增删改数据**：这类攻击可能需要配合CSRF漏洞，骗取用户点击，然后利用JavaScript模拟浏览器发送请求。
5.  **XSS蠕虫**：例如微博蠕虫，用户看过某人的微博后会自动关注那个人；或贴吧蠕虫，用户看过某个帖子后会自动回复。

![](img/0de0e932140d9d4a1ff678a63a8f68ef_49.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_51.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_53.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_55.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_57.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_59.png)

---

![](img/0de0e932140d9d4a1ff678a63a8f68ef_60.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_62.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_64.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_66.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_67.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_69.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_71.png)

## XSS漏洞的类型与利用场景 🧩
XSS主要分为三种类型：反射型XSS、存储型XSS和DOM型XSS。

![](img/0de0e932140d9d4a1ff678a63a8f68ef_73.png)

### 反射型XSS 🔍
反射型XSS的特点是只会执行一次，属于非持久型攻击。它也可以称为参数型XSS，主要存在于攻击者将恶意脚本附加到URL的参数中发送给受害者。服务端未经严格过滤处理而输出在用户浏览器中，导致浏览器执行恶意代码。

**攻击流程如下：**
1.  攻击者发现存在反射型XSS的URL。
2.  根据输出点的环境构造XSS代码。
3.  对URL进行编码或缩短以增加迷惑性（此步骤可选）。
4.  将构造的URL发送给受害者。
5.  受害者点击链接后，XSS代码执行，完成攻击。

![](img/0de0e932140d9d4a1ff678a63a8f68ef_75.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_77.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_79.png)

**以下是反射型XSS的一个简单示例：**
假设一个页面接收用户输入的名字并回显。
```html
<!-- 前端表单 -->
<form action="reflect_xss.php" method="get">
    <input type="text" name="name">
    <input type="submit" value="提交">
</form>
```
```php
// 后端处理 (reflect_xss.php)
<?php
    $name = $_GET['name'];
    echo "Welcome, " . $name;
?>
```
如果用户输入 `<script>alert(1)</script>`，且后端未过滤，则页面会弹出警告框。

![](img/0de0e932140d9d4a1ff678a63a8f68ef_81.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_83.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_85.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_87.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_89.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_90.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_92.png)

**如何寻找和测试：**
在输入框或URL参数中尝试插入JS代码，查看是否能执行。

![](img/0de0e932140d9d4a1ff678a63a8f68ef_94.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_96.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_98.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_100.png)

**防御方法：**
对用户输入进行过滤和转义，例如使用PHP的 `htmlspecialchars()` 函数。
```php
<?php
    $name = htmlspecialchars($_GET['name'], ENT_QUOTES);
    echo "Welcome, " . $name;
?>
```

![](img/0de0e932140d9d4a1ff678a63a8f68ef_102.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_104.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_106.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_108.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_110.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_112.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_114.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_116.png)

---

![](img/0de0e932140d9d4a1ff678a63a8f68ef_118.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_120.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_122.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_123.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_125.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_127.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_129.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_131.png)

### 存储型XSS 💾
存储型XSS属于持久型攻击。攻击者将恶意脚本存储到服务器的数据库中。当用户访问包含恶意数据的页面时，服务端未经严格过滤处理而输出在用户浏览器中，导致浏览器执行代码。

![](img/0de0e932140d9d4a1ff678a63a8f68ef_133.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_135.png)

**攻击流程与反射型类似，但涉及数据库：**
1.  攻击者将恶意数据提交到网站（如留言板）。
2.  数据被存入数据库。
3.  其他用户访问该页面时，恶意数据从数据库中被取出并展示，脚本在其浏览器中执行。

**存储型XSS示例（留言板）：**
```php
// 存储留言 (store.php)
<?php
    // 连接数据库...
    $message = $_POST['message'];
    // 未过滤直接存入数据库
    $sql = "INSERT INTO messages (content) VALUES ('$message')";
    // 执行SQL...
?>
```
```php
// 展示留言 (show.php)
<?php
    // 从数据库取出留言
    $result = $db->query("SELECT content FROM messages");
    while($row = $result->fetch_assoc()) {
        echo $row['content']; // 未过滤直接输出
    }
?>
```

![](img/0de0e932140d9d4a1ff678a63a8f68ef_137.png)

**防御方法：**
在数据**存入数据库前**或**从数据库取出后输出前**进行严格的过滤和转义。
```php
// 存入前过滤
$message = htmlspecialchars($_POST['message'], ENT_QUOTES);
// 或输出前转义
echo htmlspecialchars($row['content'], ENT_QUOTES);
```

![](img/0de0e932140d9d4a1ff678a63a8f68ef_139.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_141.png)

---

### DOM型XSS 🌐
DOM型XSS的特点是通过JavaScript操作DOM实现DOM树的重构。它主要存在于用户能修改页面DOM结构的地方，造成客户端Payload在浏览器中执行。整个过程在客户端完成，不经过服务端处理。

![](img/0de0e932140d9d4a1ff678a63a8f68ef_143.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_145.png)

**DOM型XSS示例：**
```html
<script>
    var pos = document.URL.indexOf("name=") + 5;
    var userInput = document.URL.substring(pos, document.URL.length);
    eval("var name = '" + userInput + "';");
    document.write("Hello, " + name);
</script>
```
如果URL为 `http://example.com/page.html?name=Alice`，则正常输出 `Hello, Alice`。
如果URL为 `http://example.com/page.html?name=';alert(1)//`，则代码变为 `eval("var name = '';alert(1)//';")`，从而执行 `alert(1)`。

**防御方法：**
避免使用 `eval()`、`innerHTML`、`document.write()` 等不安全的方法直接处理用户可控的数据。应使用 `textContent` 或经过验证的安全API。

![](img/0de0e932140d9d4a1ff678a63a8f68ef_147.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_148.png)

---

## XSS的绕过与防御 🛡️
上一节我们介绍了XSS的三种主要类型，本节中我们来看看攻击者常用的绕过手法以及如何有效防御。

### 常见的XSS绕过手法
当网站存在一些基础的过滤时，攻击者可能会尝试以下方法绕过：

![](img/0de0e932140d9d4a1ff678a63a8f68ef_150.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_152.png)

1.  **大小写绕过**：如果过滤规则是大小写敏感的。
    *   原始Payload: `<script>alert(1)</script>`
    *   绕过Payload: `<ScRiPt>alert(1)</ScRiPt>`

![](img/0de0e932140d9d4a1ff678a63a8f68ef_154.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_156.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_158.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_159.png)

2.  **双写标签绕过**：如果过滤规则是替换 `script` 为空字符串。
    *   原始Payload: `<script>alert(1)</script>`
    *   绕过Payload: `<scrscriptipt>alert(1)</scrscriptipt>`

3.  **使用其他标签或事件**：如果只过滤了 `<script>` 标签。
    *   `<img src=1 onerror=alert(1)>`
    *   `<body onload=alert(1)>`
    *   `<svg onload=alert(1)>`

4.  **编码绕过**：对关键字符进行HTML实体编码或JS编码。
    *   HTML实体: `"><img src=1 onerror=alert(1)>` (需在属性值中，浏览器会解码)
    *   JS Unicode编码: `\u0061\u006c\u0065\u0072\u0074(1)` 代表 `alert(1)`

5.  **利用伪协议**：在允许的上下文（如`<a>`标签的`href`属性）中使用。
    *   `<a href="javascript:alert(1)">点击</a>`

---

![](img/0de0e932140d9d4a1ff678a63a8f68ef_161.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_163.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_164.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_166.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_168.png)

### XSS的防御策略
有效的XSS防御需要多管齐下，主要从以下两个层面进行：

![](img/0de0e932140d9d4a1ff678a63a8f68ef_170.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_171.png)

1.  **输入验证与过滤**
    *   **白名单优于黑名单**：只允许特定的、安全的字符或格式（如年龄字段只允许数字）。
    *   **对输入进行转义**：根据输出位置，使用对应的转义函数。
        *   输出到HTML正文：使用 `htmlspecialchars()`。
        *   输出到HTML属性：确保属性值用引号包裹，并使用 `htmlspecialchars()`。
        *   输出到JavaScript代码中：使用 `json_encode()` 或专门的JS转义函数。
        *   输出到URL参数：使用 `urlencode()`。

2.  **输出编码**
    *   这是最根本、最有效的防御措施。**不要相信任何用户输入**，在将数据输出到页面时，必须根据上下文进行正确的编码。

3.  **使用安全响应头**
    *   **Content-Security-Policy (CSP)**：通过HTTP头定义页面允许加载哪些资源（脚本、图片、样式等），可以极大地缓解XSS攻击。
        *   示例：`Content-Security-Policy: default-src 'self'; script-src 'self' https://trusted.cdn.com;`
    *   **HttpOnly Cookie**：设置Cookie时添加 `HttpOnly` 标志，防止JavaScript通过 `document.cookie` 读取，可缓解Cookie窃取。
        *   示例（PHP）：`setcookie("sessionid", $value, time()+3600, "/", "", false, true);` (最后一个参数为HttpOnly)

4.  **避免不安全的API**
    *   在JavaScript中，避免直接使用 `innerHTML`、`outerHTML`、`document.write()` 来插入不可信数据。优先使用 `textContent` 或 `setAttribute` 等安全方法。

---

![](img/0de0e932140d9d4a1ff678a63a8f68ef_173.png)

![](img/0de0e932140d9d4a1ff678a63a8f68ef_175.png)

## 总结 📝
本节课中我们一起学习了跨站脚本攻击（XSS）的完整知识体系。

我们首先明确了XSS的定义，即攻击者向网页中注入恶意脚本并在用户浏览器中执行。接着，我们探讨了XSS可能带来的多种危害，包括信息窃取、钓鱼、挂马等。

课程的核心部分是详细讲解了三种XSS类型：
*   **反射型XSS**：非持久化，通过URL参数触发。
*   **存储型XSS**：持久化，恶意代码存储在服务器端（如数据库），影响范围更广。
*   **DOM型XSS**：完全在客户端浏览器中发生，不经过服务端。

![](img/0de0e932140d9d4a1ff678a63a8f68ef_177.png)

最后，我们深入研究了攻击者常用的绕过技术，并系统地学习了如何从**输入处理、输出编码、安全HTTP头和使用安全API**等多个层面构建有效的XSS防御体系。记住，**“所有输入都是有害的”** 是Web安全的基本原则，对用户输入保持警惕并实施严格的输出编码是防范XSS的关键。