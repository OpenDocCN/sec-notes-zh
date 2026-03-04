# 网络安全教程：P5：3.XSS跨站脚本攻击原理与防御

![](img/93121f90f1878bd4839b0b524980b94f_1.png)

![](img/93121f90f1878bd4839b0b524980b94f_2.png)

![](img/93121f90f1878bd4839b0b524980b94f_4.png)

![](img/93121f90f1878bd4839b0b524980b94f_6.png)

## 概述
在本节课中，我们将要学习**跨站脚本攻击**的原理、类型、危害以及如何进行防御。XSS是一种常见的Web安全漏洞，理解它对于学习网络安全至关重要。

---

![](img/93121f90f1878bd4839b0b524980b94f_8.png)

![](img/93121f90f1878bd4839b0b524980b94f_10.png)

## 什么是跨站脚本攻击？
跨站脚本攻击是攻击者通过网站注入点注入客户端可执行解析的**Payload**。Payload的意思是脚本代码。

![](img/93121f90f1878bd4839b0b524980b94f_12.png)

![](img/93121f90f1878bd4839b0b524980b94f_14.png)

![](img/93121f90f1878bd4839b0b524980b94f_16.png)

![](img/93121f90f1878bd4839b0b524980b94f_18.png)

![](img/93121f90f1878bd4839b0b524980b94f_20.png)

![](img/93121f90f1878bd4839b0b524980b94f_21.png)

![](img/93121f90f1878bd4839b0b524980b94f_23.png)

![](img/93121f90f1878bd4839b0b524980b94f_24.png)

当用户（受害者）访问被注入了有害代码的网页时，恶意的Payload就会自动加载并执行，以达到攻击者的目的。攻击者写在代码里的逻辑可能是为了窃取Cookie、进行恶意传播或钓鱼欺骗等。

![](img/93121f90f1878bd4839b0b524980b94f_26.png)

![](img/93121f90f1878bd4839b0b524980b94f_28.png)

![](img/93121f90f1878bd4839b0b524980b94f_30.png)

![](img/93121f90f1878bd4839b0b524980b94f_32.png)

![](img/93121f90f1878bd4839b0b524980b94f_34.png)

![](img/93121f90f1878bd4839b0b524980b94f_36.png)

![](img/93121f90f1878bd4839b0b524980b94f_38.png)

![](img/93121f90f1878bd4839b0b524980b94f_40.png)

为了避免与HTML语言中的CSS相混淆，我们通常称它为**XSS**。网站注入点需要攻击者自己寻找，不是每一个地方都能进行注入。注入到客户端可执行解析的脚本代码，在XSS中通常指**JavaScript代码**。

![](img/93121f90f1878bd4839b0b524980b94f_42.png)

![](img/93121f90f1878bd4839b0b524980b94f_44.png)

![](img/93121f90f1878bd4839b0b524980b94f_46.png)

![](img/93121f90f1878bd4839b0b524980b94f_47.png)

![](img/93121f90f1878bd4839b0b524980b94f_49.png)

![](img/93121f90f1878bd4839b0b524980b94f_51.png)

---

![](img/93121f90f1878bd4839b0b524980b94f_53.png)

![](img/93121f90f1878bd4839b0b524980b94f_55.png)

![](img/93121f90f1878bd4839b0b524980b94f_57.png)

![](img/93121f90f1878bd4839b0b524980b94f_59.png)

![](img/93121f90f1878bd4839b0b524980b94f_60.png)

![](img/93121f90f1878bd4839b0b524980b94f_62.png)

![](img/93121f90f1878bd4839b0b524980b94f_64.png)

![](img/93121f90f1878bd4839b0b524980b94f_66.png)

![](img/93121f90f1878bd4839b0b524980b94f_67.png)

![](img/93121f90f1878bd4839b0b524980b94f_69.png)

## XSS的危害有哪些？
以下是XSS攻击可能带来的主要危害：

![](img/93121f90f1878bd4839b0b524980b94f_71.png)

![](img/93121f90f1878bd4839b0b524980b94f_73.png)

1.  **获取用户信息**：例如浏览器信息、用户的真实IP地址或Cookie信息。如果得到Cookie，就可以实现无密码登录，进入用户账号并窃取个人信息。
2.  **钓鱼攻击**：利用XSS漏洞构造一个登录框，骗取用户的账户和密码。例如，可以提示用户“登录已过期”，然后用JS代码模拟网站的登录框，将用户名和密码发送到攻击者的服务器。
3.  **注入木马或广告链接**：在主站注入非法网站的链接，可能对主站公司的声誉造成影响。
4.  **后台增删改网络数据等操作**：这可能需要配合后面学习的CSRF漏洞，骗取用户点击，然后利用JS模拟浏览器发包。
5.  **XSS蠕虫**：例如微博蠕虫（自动关注某人）或贴吧蠕虫（自动回复某个帖子）。

---

![](img/93121f90f1878bd4839b0b524980b94f_75.png)

![](img/93121f90f1878bd4839b0b524980b94f_77.png)

![](img/93121f90f1878bd4839b0b524980b94f_79.png)

## XSS漏洞的类型及利用场景
XSS主要分为三种类型：反射型XSS、存储型XSS和DOM型XSS。

![](img/93121f90f1878bd4839b0b524980b94f_81.png)

![](img/93121f90f1878bd4839b0b524980b94f_83.png)

![](img/93121f90f1878bd4839b0b524980b94f_85.png)

![](img/93121f90f1878bd4839b0b524980b94f_87.png)

![](img/93121f90f1878bd4839b0b524980b94f_89.png)

![](img/93121f90f1878bd4839b0b524980b94f_90.png)

*   **反射型XSS**：主要用于钓鱼、引流或配合其他漏洞（如CSRF）。它的特点是只会执行一次，属于非持久型攻击。
*   **存储型XSS**：攻击范围比较广，流量传播大。它也可以配合其他漏洞。
*   **DOM型XSS**：属于客户端的XSS，其Payload长度大小不受限制，也可以配合其他漏洞。

![](img/93121f90f1878bd4839b0b524980b94f_92.png)

![](img/93121f90f1878bd4839b0b524980b94f_94.png)

![](img/93121f90f1878bd4839b0b524980b94f_96.png)

![](img/93121f90f1878bd4839b0b524980b94f_98.png)

![](img/93121f90f1878bd4839b0b524980b94f_100.png)

上一节我们介绍了XSS的基本概念和危害，本节中我们来看看具体的XSS类型。

![](img/93121f90f1878bd4839b0b524980b94f_102.png)

![](img/93121f90f1878bd4839b0b524980b94f_104.png)

![](img/93121f90f1878bd4839b0b524980b94f_106.png)

![](img/93121f90f1878bd4839b0b524980b94f_108.png)

![](img/93121f90f1878bd4839b0b524980b94f_110.png)

![](img/93121f90f1878bd4839b0b524980b94f_112.png)

![](img/93121f90f1878bd4839b0b524980b94f_114.png)

![](img/93121f90f1878bd4839b0b524980b94f_116.png)

![](img/93121f90f1878bd4839b0b524980b94f_118.png)

---

![](img/93121f90f1878bd4839b0b524980b94f_120.png)

![](img/93121f90f1878bd4839b0b524980b94f_122.png)

![](img/93121f90f1878bd4839b0b524980b94f_123.png)

![](img/93121f90f1878bd4839b0b524980b94f_125.png)

![](img/93121f90f1878bd4839b0b524980b94f_127.png)

![](img/93121f90f1878bd4839b0b524980b94f_129.png)

![](img/93121f90f1878bd4839b0b524980b94f_131.png)

![](img/93121f90f1878bd4839b0b524980b94f_133.png)

![](img/93121f90f1878bd4839b0b524980b94f_135.png)

### 反射型XSS
反射型XSS的特点是只会执行一次，属于非持久型攻击，也可以称为参数型跨站脚本。

它主要存在于攻击者将恶意脚本附加到URL的参数中发送给受害者。服务端未经严格过滤处理而输出在用户浏览器中，导致浏览器可以执行该代码。

以下是反射型XSS的攻击流程：
1.  攻击者发现存在反射型XSS的URL。
2.  根据输出点的环境，构造XSS代码（即能够触发XSS执行的代码）。
3.  对URL进行编码或缩短（此步可选，仅为了增加迷惑性）。
4.  将构造好的URL发送给受害者。
5.  受害者点击链接后，XSS代码执行，完成攻击。

![](img/93121f90f1878bd4839b0b524980b94f_137.png)

![](img/93121f90f1878bd4839b0b524980b94f_139.png)

![](img/93121f90f1878bd4839b0b524980b94f_141.png)

#### 反射型XSS利用场景与示例
我们如何寻找XSS漏洞？可以尝试在输入框或URL参数中修改内容，插入JS代码，看看能否注入。

例如，一个简单的输入名字并回显的页面：
```html
<!-- 前端表单 -->
<form action="reflect_xss.php" method="get">
    <input type="text" name="name">
    <input type="submit" value="提交">
</form>
```
```php
<!-- 后端PHP (reflect_xss.php) -->
<?php
    $name = $_GET['name'];
    echo "Welcome, " . $name;
?>
```
如果用户输入`<script>alert(1)</script>`，后端没有过滤直接输出，就会弹窗。

![](img/93121f90f1878bd4839b0b524980b94f_143.png)

![](img/93121f90f1878bd4839b0b524980b94f_145.png)

**防御方法**：在输出用户数据前进行过滤或编码。
```php
<?php
    $name = $_GET['name'];
    // 方法1：过滤特定标签（不推荐，易绕过）
    $name = str_replace("<script>", "", $name);
    // 方法2：使用正则匹配过滤（更好，但需注意大小写）
    $name = preg_replace("/<script>/i", "", $name);
    // 方法3：进行HTML实体编码（推荐）
    $name = htmlspecialchars($name, ENT_QUOTES);
    echo "Welcome, " . $name;
?>
```
即使进行了过滤，攻击者也可能尝试绕过，例如使用大小写、嵌套标签或利用其他HTML标签的事件属性（如`<img src=1 onerror=alert(1)>`）。因此，最可靠的防御是在**输出时对所有不可信数据进行HTML实体编码**。

---

![](img/93121f90f1878bd4839b0b524980b94f_147.png)

![](img/93121f90f1878bd4839b0b524980b94f_148.png)

### 存储型XSS
存储型XSS属于持久型攻击。它与反射型的最大区别在于，它涉及**数据库**。

攻击者将恶意脚本存储到服务器的数据库中。当用户访问包含恶意数据的页面时，服务端未经严格过滤处理而输出在用户浏览器中，导致浏览器执行代码。

存储型XSS常见于评论、留言、个人信息等需要将数据存入数据库的功能点。

![](img/93121f90f1878bd4839b0b524980b94f_150.png)

![](img/93121f90f1878bd4839b0b524980b94f_152.png)

#### 存储型XSS示例与防御
一个留言板功能，用户提交的留言存入数据库，并在页面展示。
```php
<!-- 存储留言的PHP -->
<?php
    // 连接数据库...
    $nickname = $_POST['nickname'];
    $message = $_POST['message'];
    // 不安全的做法：直接存入数据库
    $sql = "INSERT INTO guestbook (nickname, message) VALUES ('$nickname', '$message')";
    // ...执行SQL
?>
<!-- 展示留言的PHP -->
<?php
    // 从数据库取出留言
    $result = $conn->query("SELECT * FROM guestbook");
    while($row = $result->fetch_assoc()) {
        // 不安全的做法：直接输出
        echo "<div>".$row['nickname'].": ".$row['message']."</div>";
    }
?>
```
如果用户在留言中插入`<script>alert(1)</script>`，那么所有访问留言板的用户都会触发弹窗。

![](img/93121f90f1878bd4839b0b524980b94f_154.png)

![](img/93121f90f1878bd4839b0b524980b94f_156.png)

![](img/93121f90f1878bd4839b0b524980b94f_158.png)

![](img/93121f90f1878bd4839b0b524980b94f_159.png)

**防御方法**：防御点可以在**数据存入数据库前**或**从数据库取出展示时**。
1.  **输入过滤**：在存入数据库前，对数据进行过滤或编码。
    ```php
    $message = htmlspecialchars($_POST['message'], ENT_QUOTES);
    // 然后再执行SQL插入
    ```
2.  **输出编码**：在从数据库取出数据展示时，再进行编码（更推荐，保证存储原始数据）。
    ```php
    while($row = $result->fetch_assoc()) {
        $safe_nickname = htmlspecialchars($row['nickname'], ENT_QUOTES);
        $safe_message = htmlspecialchars($row['message'], ENT_QUOTES);
        echo "<div>".$safe_nickname.": ".$safe_message."</div>";
    }
    ```
推荐使用**输出编码**，因为它不影响数据的原始存储，并且能更灵活地应对不同的输出上下文。

---

### DOM型XSS
DOM型XSS的特点是通过JavaScript操作**Document Object Model**实现DOM树的重构。

它主要存在于用户能修改页面的DOM，造成客户端Payload在浏览器中执行。整个过程在**客户端**完成，不经过服务端处理。

#### DOM型XSS示例
```html
<script>
    var pos = document.URL.indexOf("name=") + 5;
    var name = document.URL.substring(pos, document.URL.length);
    document.write(name);
</script>
```
如果访问的URL是`http://example.com/page.html?name=<script>alert(1)</script>`，那么`document.write`会直接将`<script>alert(1)</script>`写入页面，导致脚本执行。

![](img/93121f90f1878bd4839b0b524980b94f_161.png)

![](img/93121f90f1878bd4839b0b524980b94f_163.png)

![](img/93121f90f1878bd4839b0b524980b94f_164.png)

![](img/93121f90f1878bd4839b0b524980b94f_166.png)

![](img/93121f90f1878bd4839b0b524980b94f_168.png)

![](img/93121f90f1878bd4839b0b524980b94f_170.png)

![](img/93121f90f1878bd4839b0b524980b94f_171.png)

**防御方法**：避免使用`innerHTML`、`outerHTML`、`document.write`等不安全的方法直接插入不可信数据。应使用`textContent`或`setAttribute`等安全的API，或者对要插入的数据进行编码。

---

## XSS的绕过与高级防御
攻击者会尝试各种方法绕过过滤。以下是一些常见的绕过思路和对应的防御原则：

### 常见绕过方式
1.  **大小写绕过**：`<ScRiPt>alert(1)</sCrIpT>`
2.  **标签嵌套绕过**：`<scr<script>ipt>alert(1)</scr</script>ipt>`
3.  **使用其他标签或事件**：`<img src=1 onerror=alert(1)>`， `<svg onload=alert(1)>`
4.  **编码绕过**：使用HTML实体、URL编码或JS编码。
    *   例如：`<a href="javascript:alert(1)">click</a>` 可以编码为 `<a href="javascript:alert(1)">click</a>`
5.  **利用伪协议**：`<a href="javascript:alert(1)">click</a>`

### 防御原则
1.  **输入验证**：使用白名单原则，只允许特定的、安全的字符或格式。
2.  **输出编码**：根据输出位置进行正确的编码。
    *   输出在HTML标签之间：使用HTML实体编码（`htmlspecialchars`）。
    *   输出在HTML属性值中：使用HTML实体编码，并注意引号。
    *   输出在JavaScript代码中：使用JavaScript编码。
    *   输出在URL参数中：使用URL编码。
3.  **使用安全的API**：在JavaScript中，使用`textContent`代替`innerHTML`。
4.  **设置HttpOnly Cookie**：防止JavaScript读取敏感Cookie。
    ```php
    setcookie("sessionid", "value", time()+3600, "/", "", false, true); // 最后一个参数为HttpOnly
    ```
5.  **使用内容安全策略**：通过HTTP头`Content-Security-Policy`来定义页面可以加载和执行哪些资源，这是非常有效的现代浏览器防御手段。

![](img/93121f90f1878bd4839b0b524980b94f_173.png)

![](img/93121f90f1878bd4839b0b524980b94f_175.png)

---

## 总结
本节课中我们一起学习了跨站脚本攻击的方方面面。

*   **XSS本质**：攻击者注入恶意脚本，在受害者浏览器中执行。
*   **三种类型**：
    *   **反射型XSS**：Payload在URL中，非持久，需要诱骗点击。
    *   **存储型XSS**：Payload存储在服务器（如数据库），持久，危害范围大。
    *   **DOM型XSS**：由前端JavaScript不安全地操作DOM引发，不经过服务端。
*   **核心防御**：对**所有不可信的数据**在**输出到相应上下文时**进行严格的编码或转义。**不要相信任何来自用户的输入**。

![](img/93121f90f1878bd4839b0b524980b94f_177.png)

理解XSS的原理和防御方法，是构建安全Web应用的基石。请务必通过实践来加深理解。