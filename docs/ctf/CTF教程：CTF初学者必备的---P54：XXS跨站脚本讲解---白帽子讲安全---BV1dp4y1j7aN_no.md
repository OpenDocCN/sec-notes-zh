# CTF教程：P54：XSS跨站脚本讲解 🛡️

![](img/a5d5f7a5324736ec4594dbde1c7b082f_1.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_2.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_4.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_6.png)

在本节课中，我们将要学习跨站脚本攻击的基本概念、类型、危害、利用方式以及防御方法。XSS是Web安全中一个非常常见且重要的漏洞，理解它对于CTF比赛和实际安全防护都至关重要。

## 什么是跨站脚本攻击？

跨站脚本攻击是攻击者通过网站注入点注入客户端可执行解析的Payload。Payload的意思是脚本代码。

![](img/a5d5f7a5324736ec4594dbde1c7b082f_8.png)

当用户（受害者）访问注入了有害代码的网页时，恶意的Payload就会自动加载并执行，以达到攻击者的目的。攻击者写在代码里的逻辑可能是为了窃取Cookie、进行恶意传播、钓鱼欺骗等。

![](img/a5d5f7a5324736ec4594dbde1c7b082f_10.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_12.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_14.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_16.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_17.png)

为了避免与HTML语言中的CSS相混淆，我们通常称它为XSS。网站注入点需要攻击者自己寻找，并非每个地方都可以注入。注入到客户端可执行解析的脚本代码，在XSS中通常指JavaScript代码。

![](img/a5d5f7a5324736ec4594dbde1c7b082f_19.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_20.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_22.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_24.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_26.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_28.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_30.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_32.png)

## XSS的危害有哪些？

![](img/a5d5f7a5324736ec4594dbde1c7b082f_34.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_36.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_38.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_40.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_42.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_43.png)

以下是XSS攻击可能带来的主要危害：

![](img/a5d5f7a5324736ec4594dbde1c7b082f_45.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_47.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_49.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_51.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_53.png)

1.  **获取用户信息**：例如获取浏览器信息、用户的真实IP地址或Cookie信息。如果得到Cookie，就可以实现无密码登录，进入用户账号并窃取个人信息。
2.  **钓鱼攻击**：利用XSS漏洞构造虚假登录框，骗取用户的账户和密码。攻击者可以用JS代码模拟网站登录框，将用户名和密码发送到攻击者服务器。
3.  **注入木马或广告链接**：在主站注入非法网站链接，可能对主站公司的声誉造成影响。
4.  **后台增删改数据**：可能需要配合CSRF漏洞，骗取用户点击，然后利用JS模拟浏览器发包。
5.  **XSS蠕虫**：例如微博蠕虫（自动关注某人）或贴吧蠕虫（自动回复帖子）。

![](img/a5d5f7a5324736ec4594dbde1c7b082f_55.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_56.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_58.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_60.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_62.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_63.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_65.png)

## XSS漏洞的类型及利用场景

![](img/a5d5f7a5324736ec4594dbde1c7b082f_67.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_69.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_70.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_72.png)

XSS主要分为三种类型：反射型XSS、存储型XSS和DOM型XSS。

上一节我们介绍了XSS的基本概念和危害，本节中我们来看看具体的漏洞类型。

### 反射型XSS

![](img/a5d5f7a5324736ec4594dbde1c7b082f_74.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_76.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_78.png)

反射型XSS的特点是只会执行一次，属于非持久型攻击。它也可以称为参数型XSS。

![](img/a5d5f7a5324736ec4594dbde1c7b082f_80.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_82.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_84.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_86.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_88.png)

它主要存在于攻击者将恶意脚本附加到URL的参数中发送给受害者。服务端未经严格过滤处理而输出在用户浏览器中，导致浏览器可以执行代码数据。

![](img/a5d5f7a5324736ec4594dbde1c7b082f_89.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_91.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_93.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_95.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_97.png)

**攻击流程如下：**
1.  攻击者发现存在反射型XSS的URL。
2.  根据输出点的环境，构造XSS代码。
3.  对URL进行编码或缩短（可选，增加迷惑性）。
4.  将构造的URL发送给受害者。
5.  受害者点击链接后，XSS代码执行，完成攻击。

![](img/a5d5f7a5324736ec4594dbde1c7b082f_99.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_101.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_103.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_105.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_107.png)

**利用场景与测试：**
我们可以尝试在输入框或URL参数中修改内容，插入JS代码，测试是否能注入。

![](img/a5d5f7a5324736ec4594dbde1c7b082f_109.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_111.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_113.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_115.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_117.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_119.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_121.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_122.png)

例如，在一个回显用户输入的页面，如果输入 `<h1>hello`，页面可能会将“hello”显示为一级标题，证明网站过滤不严格。进一步尝试输入 `<script>alert(1)</script>`，如果成功弹窗，则证明存在反射型XSS漏洞。

![](img/a5d5f7a5324736ec4594dbde1c7b082f_124.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_126.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_128.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_130.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_132.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_134.png)

**后端代码示例与防护：**
一个存在漏洞的PHP后端代码可能如下：
```php
<?php
$name = $_GET['name'];
echo "Welcome, " . $name;
?>
```
这段代码直接输出了用户输入的 `$name`，未做任何过滤。

防护方法包括：
*   **黑名单过滤**：使用 `str_replace` 函数替换特定标签，但容易被绕过（如大小写、嵌套标签）。
*   **正则过滤**：使用 `preg_replace` 进行更严格的匹配。
*   **输出编码**：使用 `htmlspecialchars` 函数将预定字符转换为HTML实体，这是最有效的防护手段之一。

### 存储型XSS

![](img/a5d5f7a5324736ec4594dbde1c7b082f_136.png)

存储型XSS属于持久型攻击。它主要存在于攻击者将恶意脚本存储到服务器的数据库中。

![](img/a5d5f7a5324736ec4594dbde1c7b082f_138.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_140.png)

当用户访问包含恶意数据的页面时，服务端未经严格过滤处理而输出在用户浏览器中，导致浏览器执行代码数据。因为数据存储在数据库中，所以每次用户访问该页面都会触发，影响范围更广。

**攻击流程与示例：**
常见于评论、留言、个人信息等需要存入数据库的功能点。攻击者提交包含恶意脚本的留言，该脚本被存入数据库。当任何用户（包括管理员）查看留言板时，恶意脚本从数据库中被取出并输出到页面，从而执行。

![](img/a5d5f7a5324736ec4594dbde1c7b082f_142.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_144.png)

**后端代码示例与防护：**
存在漏洞的代码可能在存储和读取时都未过滤：
```php
// 存储留言（未过滤）
$message = $_POST['message'];
$sql = "INSERT INTO guestbook (message) VALUES ('$message')";

![](img/a5d5f7a5324736ec4594dbde1c7b082f_146.png)

// 读取并显示留言（未过滤）
$result = mysqli_query($conn, "SELECT message FROM guestbook");
while($row = mysqli_fetch_assoc($result)) {
    echo $row['message'];
}
```

防护方法同样可以在**输入时**或**输出时**进行：
*   **输入时过滤**：在数据存入数据库前，使用 `strip_tags` 去除HTML/PHP标签，或使用 `htmlspecialchars` 进行编码。
*   **输出时过滤**：在从数据库取出数据并显示时，使用 `htmlspecialchars` 进行编码。**输出时编码是更推荐的做法**，它保证了数据在存储层是原始的，只在展示时进行安全处理。

![](img/a5d5f7a5324736ec4594dbde1c7b082f_148.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_149.png)

### DOM型XSS

DOM型XSS的特点是可以通过JavaScript操作Document对象实现DOM树的重构。

它主要存在于用户能修改页面的DOM，造成客户端Payload在浏览器中执行。整个过程在客户端完成，不经过服务端处理。

**攻击示例：**
考虑以下前端代码：
```html
<script>
var pos = document.URL.indexOf("name=") + 5;
var userInput = document.URL.substring(pos, document.URL.length);
eval(userInput);
</script>
```
如果URL是 `http://example.com/#name=alert(1)`，那么 `userInput` 的值就是 `alert(1)`，`eval` 函数会执行它，导致弹窗。

![](img/a5d5f7a5324736ec4594dbde1c7b082f_151.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_153.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_155.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_157.png)

DOM型XSS与反射型、存储型的最大区别在于，它的漏洞触发完全依赖于客户端JavaScript对不可信数据的**不安全处理**（如 `innerHTML`、`eval`、`document.write` 等），不依赖于服务端响应。

## XSS漏洞的实操与进阶

上一节我们详细分析了三种XSS类型，本节中我们来看看如何搭建测试平台、进行漏洞利用和绕过防护。

### XSS接收平台搭建

为了更有效地利用XSS漏洞（如窃取Cookie），我们需要一个接收平台。以下是两个常用平台：

1.  **XSSer**：功能强大的XSS平台，需要自行搭建在PHP环境中。
2.  **蓝莲花 (BlueLotus)**：另一个开源的XSS平台，安装相对简单。

![](img/a5d5f7a5324736ec4594dbde1c7b082f_159.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_161.png)

**以蓝莲花为例的简易搭建步骤：**
1.  下载蓝莲花源码，解压到Web服务器根目录（如 `www` 或 `htdocs`）。
2.  通过浏览器访问该目录，按照安装向导完成安装。
3.  记住后台管理密码。
4.  登录后，可以创建项目，生成用于窃取Cookie等信息的恶意JS代码Payload。

![](img/a5d5f7a5324736ec4594dbde1c7b082f_163.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_165.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_166.png)

![](img/a5d5f7a5324736ec4594dbde1c7b082f_167.png)

**平台使用：**
将平台生成的Payload（一段JS代码的URL）插入到存在XSS漏洞的地方。当受害者触发该漏洞时，其Cookie、IP、页面来源等信息就会被发送到接收平台并展示。

### XSS的绕过技巧

![](img/a5d5f7a5324736ec4594dbde1c7b082f_169.png)

当网站存在一些基础的过滤时，我们可以尝试以下绕过方法：

以下是几种常见的过滤绕过思路：

*   **过滤了尖括号 `< >`**：尝试利用现有标签的事件处理器。
    *   `“ onfocus=alert(1) autofocus>`  （闭合 `value` 属性的引号，添加 `onfocus` 事件）
    *   `<img src=1 onerror=alert(1)>` （利用 `img` 标签的 `onerror` 事件）
*   **过滤了 `alert` 等关键词**：使用编码或其它函数。
    *   `prompt(1)`、`confirm(1)`
    *   使用HTML实体编码或JS编码：`javascript:eval(‘al’+’ert(1)’)`
*   **过滤了 `javascript:` 伪协议**：尝试使用编码。
    *   `javasc&#x72;ipt:alert(1)` （对字符进行HTML实体编码）
*   **标签属性间空格被过滤**：用 `/` 代替空格。
    *   `<img/src=1/onerror=alert(1)>`
*   **利用不完整的标签**：某些浏览器有容错机制。
    *   `<m onclick=alert(1)>click me</m>`

### XSS的防御策略

![](img/a5d5f7a5324736ec4594dbde1c7b082f_171.png)

防御XSS主要从两个层面入手：阻止恶意代码注入和阻止恶意操作执行。

![](img/a5d5f7a5324736ec4594dbde1c7b082f_173.png)

以下是关键的防御措施：

1.  **输入验证与过滤**：
    *   **白名单优于黑名单**：只允许特定的、安全的字符或格式（如年龄字段只允许数字）。
    *   **上下文相关的输出编码**：
        *   输出在HTML标签或属性中 → 进行HTML实体编码 (`htmlspecialchars`)。
        *   输出在JavaScript代码中 → 进行JavaScript编码。
        *   输出在URL参数中 → 进行URL编码 (`urlencode`)。
2.  **安全的响应头**：
    *   设置 `Content-Security-Policy` (CSP) 头，限制脚本来源，禁止内联脚本执行，这是非常有效的现代浏览器防护手段。
    *   为Cookie设置 `HttpOnly` 属性，防止通过 `document.cookie` 被JavaScript读取。
3.  **安全的API**：
    *   避免使用 `innerHTML`、`outerHTML`、`document.write()` 等直接插入HTML的API，优先使用 `textContent` 或 `setAttribute`。
    *   避免使用 `eval()`、`setTimeout(string)`、`setInterval(string)` 等可以执行字符串的API。
4.  **使用安全的框架与库**：现代前端框架（如React, Vue, Angular）通常内置了XSS防护机制。同时，确保使用的第三方库没有已知的安全漏洞。

## 总结

![](img/a5d5f7a5324736ec4594dbde1c7b082f_175.png)

本节课中我们一起学习了跨站脚本攻击的完整知识体系。我们从XSS的基本定义和危害入手，深入剖析了反射型、存储型和DOM型三种核心漏洞类型的原理、利用方式及区别。通过搭建XSS接收平台，我们了解了如何实际利用漏洞窃取信息。面对常见的过滤机制，我们探讨了多种巧妙的绕过技巧。最后，我们从输入处理、输出编码、安全HTTP头和使用安全API等多个层面，系统地学习了如何有效防御XSS攻击。理解并掌握这些内容，是成为一名合格安全研究员或开发者的重要一步。