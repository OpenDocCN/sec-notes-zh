# 🛠️ PHP开发实战：第23天 - 身份验证技术详解 (Session, Cookie, Token)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_1.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_3.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_5.png)

在本节课中，我们将学习PHP后台开发中三种核心的身份验证技术：Cookie、Session和Token。我们将通过编写代码来理解它们的工作原理、区别以及安全影响。

![](img/98cd56e0aa66705d6a7914d44da5fdd7_7.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_9.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_11.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_13.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_15.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_16.png)

## 📖 课程概述

![](img/98cd56e0aa66705d6a7914d44da5fdd7_18.png)

身份验证是Web应用安全的基础。本节课将分别实现基于Cookie、Session和Token的登录验证流程，并分析它们各自的特点和安全考量。

![](img/98cd56e0aa66705d6a7914d44da5fdd7_20.png)

---

![](img/98cd56e0aa66705d6a7914d44da5fdd7_22.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_24.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_26.png)

## 🍪 第一部分：Cookie身份验证

![](img/98cd56e0aa66705d6a7914d44da5fdd7_28.png)

Cookie是一种将用户身份凭证存储在客户端（浏览器）的技术。它常用于实现“记住我”功能。

![](img/98cd56e0aa66705d6a7914d44da5fdd7_30.png)

### Cookie工作流程

![](img/98cd56e0aa66705d6a7914d44da5fdd7_32.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_34.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_36.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_38.png)

以下是Cookie验证的标准流程：

![](img/98cd56e0aa66705d6a7914d44da5fdd7_40.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_41.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_43.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_45.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_47.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_49.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_51.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_53.png)

1.  用户请求登录页面。
2.  服务器验证账号密码。
3.  验证成功后，服务器设置Cookie并发送给浏览器。
4.  浏览器保存Cookie。
5.  后续请求时，浏览器自动携带Cookie。
6.  服务器读取并验证Cookie。
7.  验证通过，返回请求内容。
8.  验证失败，重定向到登录页。

![](img/98cd56e0aa66705d6a7914d44da5fdd7_55.png)

### 代码实现：基于Cookie的登录

![](img/98cd56e0aa66705d6a7914d44da5fdd7_57.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_59.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_61.png)

我们将创建三个文件来实现完整的Cookie登录、首页访问和退出流程。

**1. 创建核心文件**

![](img/98cd56e0aa66705d6a7914d44da5fdd7_63.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_65.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_67.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_69.png)

首先，在项目目录下创建以下文件：
*   `login_c.php`：登录处理文件。
*   `index_c.php`：登录成功后的首页。
*   `logout_c.php`：退出登录文件。

![](img/98cd56e0aa66705d6a7914d44da5fdd7_71.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_73.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_75.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_76.png)

**2. 编写登录页面 (login_c.php)**

登录页面包含一个表单，用于提交用户名和密码。我们使用一个美观的HTML模板。

![](img/98cd56e0aa66705d6a7914d44da5fdd7_78.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_80.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_82.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_84.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_86.png)

```html
<!-- login_c.php 中的表单部分 -->
<form method="post" action="#">
    <input type="text" name="username" placeholder="用户名" required>
    <input type="password" name="password" placeholder="密码" required>
    <button type="submit">登录</button>
</form>
```

![](img/98cd56e0aa66705d6a7914d44da5fdd7_88.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_90.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_92.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_94.png)

**3. 处理登录逻辑 (login_c.php)**

![](img/98cd56e0aa66705d6a7914d44da5fdd7_96.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_98.png)

在`login_c.php`中，我们需要处理以下步骤：

```php
// 1. 接收用户输入的账号密码
$user = $_POST['username'];
$pass = $_POST['password'];

![](img/98cd56e0aa66705d6a7914d44da5fdd7_100.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_102.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_104.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_106.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_108.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_110.png)

// 2. 判定账号密码正确性（连接数据库查询）
include '../config.php'; // 包含数据库配置文件
$sql = "SELECT * FROM admin WHERE username='$user' AND password='$pass'";
$result = mysqli_query($conn, $sql);

![](img/98cd56e0aa66705d6a7914d44da5fdd7_112.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_114.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_116.png)

// 3. 如果正确，生成Cookie并保存
if(mysqli_num_rows($result) > 0) {
    // 设置Cookie，存活时间为30天
    setcookie("username", $user, time() + 60*60*24*30, "/");
    setcookie("password", $pass, time() + 60*60*24*30, "/");
    // 4. 跳转至成功页面
    header("Location: index_c.php");
    exit();
} else {
    // 3.1 如果错误，进行提示
    echo "<script>alert('登录失败');</script>";
}
```

![](img/98cd56e0aa66705d6a7914d44da5fdd7_118.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_120.png)

**4. 受保护的首页 (index_c.php)**

![](img/98cd56e0aa66705d6a7914d44da5fdd7_122.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_124.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_126.png)

首页需要验证Cookie，防止未登录用户直接访问。

```php
// index_c.php
// 1. 接收Cookie值
$cookie_user = $_COOKIE['username'];
$cookie_pass = $_COOKIE['password'];

![](img/98cd56e0aa66705d6a7914d44da5fdd7_128.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_130.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_132.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_134.png)

// 2. 判定Cookie合法性（应与数据库存储值一致）
if($cookie_user != 'admin' || $cookie_pass != '123456'){
    // 非法用户，跳转回登录页
    header("Location: login_c.php");
    exit();
}
// 3. 验证通过，显示首页内容
echo "欢迎您，". $cookie_user;
echo '<a href="logout_c.php">退出登录</a>';
```

![](img/98cd56e0aa66705d6a7914d44da5fdd7_135.png)

**5. 退出登录 (logout_c.php)**

退出功能通过清除或覆盖Cookie来实现。

![](img/98cd56e0aa66705d6a7914d44da5fdd7_137.png)

```php
// logout_c.php
// 清除Cookie（通过设置过期时间为过去）
setcookie("username", "", time() - 3600, "/");
setcookie("password", "", time() - 3600, "/");
// 跳转回登录页
header("Location: login_c.php");
```

![](img/98cd56e0aa66705d6a7914d44da5fdd7_139.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_140.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_142.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_144.png)

### Cookie的安全思考

![](img/98cd56e0aa66705d6a7914d44da5fdd7_146.png)

Cookie存储在用户浏览器中，因此存在安全风险。攻击者如果窃取到Cookie值，就可以在有效期内冒充用户身份登录。这引出了我们常说的攻击面从服务器转向了客户端。

![](img/98cd56e0aa66705d6a7914d44da5fdd7_148.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_150.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_152.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_154.png)

---

![](img/98cd56e0aa66705d6a7914d44da5fdd7_156.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_158.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_160.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_162.png)

## 🔐 第二部分：Session身份验证

![](img/98cd56e0aa66705d6a7914d44da5fdd7_164.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_166.png)

Session（会话）将用户信息存储在服务器端，比Cookie更安全。Session ID通过Cookie传递给浏览器，但关键数据始终在服务器。

![](img/98cd56e0aa66705d6a7914d44da5fdd7_168.png)

### Session与Cookie的区别

![](img/98cd56e0aa66705d6a7914d44da5fdd7_170.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_172.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_174.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_176.png)

以下是两者的核心区别：

![](img/98cd56e0aa66705d6a7914d44da5fdd7_178.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_180.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_182.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_184.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_186.png)

*   **存储位置**：Cookie在客户端，Session在服务端。
*   **安全性**：Session相对更安全，因为敏感数据不直接暴露给客户端。
*   **生命周期**：Cookie可设置长期有效，Session通常随浏览器关闭而失效。
*   **使用场景**：Cookie用于小型网站或需要长期登录的场景；Session用于中大型、对安全性要求高的场景。

![](img/98cd56e0aa66705d6a7914d44da5fdd7_188.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_190.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_192.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_194.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_196.png)

### 代码实现：基于Session的登录

![](img/98cd56e0aa66705d6a7914d44da5fdd7_198.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_200.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_202.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_204.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_206.png)

我们同样创建三个文件：`login_s.php`, `index_s.php`, `logout_s.php`。

![](img/98cd56e0aa66705d6a7914d44da5fdd7_208.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_209.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_211.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_213.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_215.png)

**1. 处理登录并设置Session (login_s.php)**

![](img/98cd56e0aa66705d6a7914d44da5fdd7_217.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_219.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_221.png)

```php
// login_s.php
session_start(); // 启动Session
// ... (数据库验证逻辑与Cookie部分相同) ...
if(mysqli_num_rows($result) > 0) {
    // 设置Session变量
    $_SESSION['username'] = $user;
    $_SESSION['password'] = $pass;
    header("Location: index_s.php");
    exit();
}
```

![](img/98cd56e0aa66705d6a7914d44da5fdd7_223.png)

Session文件默认存储在服务器的临时目录（如`/tmp`），其中包含了序列化的用户数据。

![](img/98cd56e0aa66705d6a7914d44da5fdd7_225.png)

**2. 验证Session的首页 (index_s.php)**

![](img/98cd56e0aa66705d6a7914d44da5fdd7_227.png)

```php
// index_s.php
session_start(); // 每个需要Session的页面都必须先启动
// 判断Session中的用户身份
if($_SESSION['username'] != 'admin' || $_SESSION['password'] != '123456'){
    header("Location: login_s.php");
    exit();
}
echo "欢迎您，". $_SESSION['username'];
```

![](img/98cd56e0aa66705d6a7914d44da5fdd7_229.png)

**3. 销毁Session退出 (logout_s.php)**

![](img/98cd56e0aa66705d6a7914d44da5fdd7_231.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_233.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_235.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_237.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_239.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_241.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_243.png)

```php
// logout_s.php
session_start();
session_unset(); // 释放所有session变量
session_destroy(); // 销毁session
header("Location: login_s.php");
```

![](img/98cd56e0aa66705d6a7914d44da5fdd7_245.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_247.png)

### Session的安全思考

Session数据存储在服务器，攻击者难以直接窃取。但攻击者可能尝试窃取或猜测Session ID。不过，由于Session通常有效期短（如浏览器关闭即失效），且攻击服务器本身难度更大，因此其安全性高于Cookie。

---

![](img/98cd56e0aa66705d6a7914d44da5fdd7_249.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_251.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_253.png)

## 🎫 第三部分：Token身份验证

Token（令牌）用于保证请求的唯一性和防止重复提交。它在防御暴力破解和CSRF（跨站请求伪造）等攻击中起到关键作用。

![](img/98cd56e0aa66705d6a7914d44da5fdd7_255.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_257.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_259.png)

### Token的核心概念

![](img/98cd56e0aa66705d6a7914d44da5fdd7_261.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_263.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_265.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_267.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_269.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_271.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_273.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_275.png)

Token是一个随机生成的、唯一的字符串。每次表单提交或关键请求都必须携带一个有效的Token，服务器会验证该Token是否与预期匹配。**其核心特性是唯一性**。

![](img/98cd56e0aa66705d6a7914d44da5fdd7_277.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_279.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_281.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_283.png)

### 代码实现：集成Token的登录验证

![](img/98cd56e0aa66705d6a7914d44da5fdd7_285.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_287.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_289.png)

我们创建两个文件：`token.php`用于生成Token并显示表单，`token_check.php`用于验证Token和登录信息。

![](img/98cd56e0aa66705d6a7914d44da5fdd7_291.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_293.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_295.png)

**1. 生成Token并嵌入表单 (token.php)**

![](img/98cd56e0aa66705d6a7914d44da5fdd7_297.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_299.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_301.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_303.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_305.png)

```php
// token.php
session_start();
// 生成随机Token
$token = bin2hex(random_bytes(32));
// 将Token存入Session，供验证时使用
$_SESSION['csrf_token'] = $token;
?>
<!-- 在表单中隐藏Token字段 -->
<form method="post" action="token_check.php">
    <input type="hidden" name="csrf_token" value="<?php echo $token; ?>">
    <input type="text" name="username">
    <input type="password" name="password">
    <button type="submit">登录</button>
</form>
```

![](img/98cd56e0aa66705d6a7914d44da5fdd7_307.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_309.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_311.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_313.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_315.png)

**2. 验证Token和登录信息 (token_check.php)**

![](img/98cd56e0aa66705d6a7914d44da5fdd7_317.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_319.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_321.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_323.png)

```php
// token_check.php
session_start();
// 1. 首先验证Token的唯一性
if ($_POST['csrf_token'] !== $_SESSION['csrf_token']) {
    die("非法请求：Token验证失败");
}
// 2. Token验证通过后，再进行常规的账号密码验证
$user = $_POST['username'];
$pass = $_POST['password'];
// ... (数据库验证逻辑) ...
if(验证成功){
    echo "登录成功！";
} else {
    echo "账号或密码错误";
}
// 3. 使用后销毁本次Token，确保一次性使用
unset($_SESSION['csrf_token']);
```

![](img/98cd56e0aa66705d6a7914d44da5fdd7_325.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_327.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_329.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_331.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_333.png)

### Token的安全意义

![](img/98cd56e0aa66705d6a7914d44da5fdd7_335.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_337.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_339.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_341.png)

当攻击者尝试进行暴力破解时，他需要不断向`token_check.php`发送请求。然而，每次合法请求生成的Token都是不同的（`token.php`页面每次刷新都会变）。攻击者无法预知下一次合法的Token值是什么，因此他构造的数据包总会因Token无效而被服务器拒绝。这就有效地阻止了自动化攻击工具。

---

![](img/98cd56e0aa66705d6a7914d44da5fdd7_343.png)

## 📝 课程总结

![](img/98cd56e0aa66705d6a7914d44da5fdd7_345.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_347.png)

本节课我们一起学习了三种PHP后台身份验证技术：

![](img/98cd56e0aa66705d6a7914d44da5fdd7_349.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_351.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_353.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_355.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_357.png)

1.  **Cookie**：将凭证存于客户端，实现简单，可用于“记住登录”，但存在被窃取的风险。
2.  **Session**：将凭证存于服务端，通过Session ID关联，安全性更高，是更通用的会话管理方式。
3.  **Token**：用于保证请求的唯一性，主要防御CSRF和暴力破解等自动化攻击，常与Session或Cookie结合使用。

![](img/98cd56e0aa66705d6a7914d44da5fdd7_359.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_361.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_363.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_365.png)

![](img/98cd56e0aa66705d6a7914d44da5fdd7_367.png)

理解这些技术的实现原理和差异，是构建安全Web应用的基础，也是后续学习Web安全漏洞（如会话劫持、CSRF等）的前提。