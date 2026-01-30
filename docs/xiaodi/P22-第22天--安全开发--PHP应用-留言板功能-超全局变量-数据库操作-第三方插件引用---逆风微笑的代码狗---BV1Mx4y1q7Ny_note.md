![](img/5c22f2a66639ef69bfe4f8581ae405cf_1.png)

# P22：第22天：【安全开发】- PHP应用&留言板功能&超全局变量&数据库操作&第三方插件引用 📝

![](img/5c22f2a66639ef69bfe4f8581ae405cf_3.png)

在本节课中，我们将学习如何使用原生PHP开发一个简单的留言板功能。我们将涵盖从数据库设计、前端表单创建、PHP接收数据、数据库增删改查操作，到引入第三方富文本编辑器的完整流程。通过这个实践，你将理解Web应用的基本数据流和核心开发概念。

![](img/5c22f2a66639ef69bfe4f8581ae405cf_5.png)

---

![](img/5c22f2a66639ef69bfe4f8581ae405cf_7.png)

## 概述

本课程属于“安全开发”系列，旨在通过实际开发Web应用功能，帮助大家理解程序内部逻辑，为后续学习Web安全漏洞（如SQL注入、XSS、未授权访问等）打下坚实基础。我们将从最简单的PHP原生开发开始，逐步深入。

本节课，我们将实现一个具备提交、显示和删除留言功能的留言板。

---

## 开发环境与准备 🛠️

![](img/5c22f2a66639ef69bfe4f8581ae405cf_9.png)

在开始编码前，我们需要搭建开发环境。本次开发使用以下工具组合：

![](img/5c22f2a66639ef69bfe4f8581ae405cf_11.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_13.png)

*   **PHPStorm**： 用于编写PHP代码。
*   **DW (Dreamweaver)**： 用于辅助编写HTML/CSS前端代码（非必需，可用任何文本编辑器替代）。
*   **PHPStudy**： 用于提供集成的PHP运行环境（Apache/Nginx + MySQL）。
*   **Navicat**： 用于连接和管理MySQL数据库。

我们基于 **PHP 7.0** 版本进行开发。请确保你的PHPStudy已启动MySQL服务。

![](img/5c22f2a66639ef69bfe4f8581ae405cf_15.png)

---

## 第一步：数据库设计 🗃️

任何需要存储数据的Web应用，第一步都是设计数据库表结构。我们的留言板需要存储用户昵称、留言内容、IP地址和浏览器信息。

![](img/5c22f2a66639ef69bfe4f8581ae405cf_17.png)

以下是创建数据库和表的步骤：

![](img/5c22f2a66639ef69bfe4f8581ae405cf_19.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_21.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_23.png)

1.  使用Navicat连接到本地的MySQL数据库（通常是 `127.0.0.1`， 用户 `root`， 密码 `root` 或你设置的密码）。
2.  新建一个数据库，命名为 `dm01`， 字符集选择 `utf8`。
3.  在该数据库中新建一张表，命名为 `gb`。
4.  为 `gb` 表添加以下字段（列）：

![](img/5c22f2a66639ef69bfe4f8581ae405cf_25.png)

| 字段名 | 类型 | 长度 | 说明 | 是否为空 |
| :--- | :--- | :--- | :--- | :--- |
| `username` | varchar | 255 | 存储用户昵称 | NOT NULL |
| `content` | text | - | 存储留言内容 | NULL |
| `ipaddr` | varchar | 255 | 存储用户IP地址 | NULL |
| `useragent` | varchar | 255 | 存储浏览器UA信息 | NULL |

![](img/5c22f2a66639ef69bfe4f8581ae405cf_27.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_29.png)

设计好表结构后，我们的数据“仓库”就准备好了。

![](img/5c22f2a66639ef69bfe4f8581ae405cf_31.png)

---

![](img/5c22f2a66639ef69bfe4f8581ae405cf_33.png)

## 第二步：创建前端表单页面 📄

前端页面负责收集用户输入的数据。我们将创建一个简单的HTML表单。

![](img/5c22f2a66639ef69bfe4f8581ae405cf_35.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_37.png)

核心代码如下：

![](img/5c22f2a66639ef69bfe4f8581ae405cf_39.png)

```html
<!-- gb.php -->
<form action="" method="post">
    用户名：<input type="text" name="username"><br><br>
    内容：<textarea name="content"></textarea><br><br>
    <input type="submit" value="提交留言">
</form>
```

![](img/5c22f2a66639ef69bfe4f8581ae405cf_41.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_43.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_45.png)

**代码解释：**
*   `<form action="" method="post">`： 表单标签。`action=""` 表示表单数据提交给当前页面自身（`gb.php`）处理。`method="post"` 表示使用POST方式提交数据，数据在请求体内，相对安全。
*   `name="username"` 和 `name="content"`： 这是字段的“键名”，后端PHP将通过这些键名来获取用户输入的值。

![](img/5c22f2a66639ef69bfe4f8581ae405cf_47.png)

---

![](img/5c22f2a66639ef69bfe4f8581ae405cf_49.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_51.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_53.png)

## 第三步：PHP接收表单数据与超全局变量 🔍

![](img/5c22f2a66639ef69bfe4f8581ae405cf_55.png)

当用户点击提交后，数据会发送到服务器。我们需要在 `gb.php` 文件中编写PHP代码来接收这些数据。

![](img/5c22f2a66639ef69bfe4f8581ae405cf_57.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_59.png)

PHP提供了几个**超全局变量**来获取不同来源的请求数据。最常用的是 `$_POST` 和 `$_GET`。

![](img/5c22f2a66639ef69bfe4f8581ae405cf_61.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_63.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_65.png)

```php
// gb.php 中表单后面的PHP代码
<?php
// 使用 $_POST 超全局变量接收表单数据
$u = $_POST['username']; // 获取名为 ‘username‘ 的输入框的值
$c = $_POST['content'];  // 获取名为 ‘content‘ 的文本域的值

// 为了防止用户未提交表单时直接访问页面导致的报错，可以加判断
if(isset($_POST['username'])){
    // 只有当用户提交了username字段，才执行后续的数据库操作
    echo “接收到的用户名：” . $u;
    echo “<br>接收到的内容：” . $c;
}
?>
```

![](img/5c22f2a66639ef69bfe4f8581ae405cf_67.png)

**核心概念：**
*   **`$_POST`**： 一个关联数组，用于收集来自 `method=”post”` 的表单数据。
*   **`$_GET`**： 一个关联数组，用于收集来自URL参数（如 `?id=1`）的数据。
*   **`isset()`**： 用于检测变量是否已设置并且非 NULL。这里用来判断用户是否提交了表单，避免页面初次加载时报错。

![](img/5c22f2a66639ef69bfe4f8581ae405cf_69.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_71.png)

---

![](img/5c22f2a66639ef69bfe4f8581ae405cf_73.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_75.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_77.png)

## 第四步：连接数据库与执行SQL操作 🗄️

接收到数据后，我们需要将其存入MySQL数据库。PHP使用 `mysqli` 扩展来操作MySQL数据库。

![](img/5c22f2a66639ef69bfe4f8581ae405cf_79.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_81.png)

### 1. 连接数据库

![](img/5c22f2a66639ef69bfe4f8581ae405cf_83.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_85.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_86.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_88.png)

首先，我们需要建立PHP与MySQL数据库的连接。

![](img/5c22f2a66639ef69bfe4f8581ae405cf_90.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_92.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_94.png)

```php
<?php
// 数据库配置信息
$db_ip = “127.0.0.1”; // 数据库服务器地址
$db_user = “root”;    // 数据库用户名
$db_pass = “root”;    // 数据库密码
$db_name = “dm01”;    // 要使用的数据库名

![](img/5c22f2a66639ef69bfe4f8581ae405cf_96.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_98.png)

// 创建数据库连接
$conn = mysqli_connect($db_ip, $db_user, $db_pass, $db_name);

![](img/5c22f2a66639ef69bfe4f8581ae405cf_100.png)

// 检查连接是否成功
if (!$conn) {
    die(“连接失败：” . mysqli_connect_error()); // 连接失败则终止脚本并报错
} else {
    // echo “数据库连接成功！”; // 调试时可打开
}
?>
```

![](img/5c22f2a66639ef69bfe4f8581ae405cf_102.png)

### 2. 获取用户环境信息

![](img/5c22f2a66639ef69bfe4f8581ae405cf_104.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_106.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_108.png)

在插入数据前，我们还可以通过 `$_SERVER` 超全局变量获取用户的一些环境信息，如IP和浏览器类型。

![](img/5c22f2a66639ef69bfe4f8581ae405cf_110.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_112.png)

```php
// 获取用户IP地址
$ip = $_SERVER[‘REMOTE_ADDR’];
// 获取用户浏览器信息 (User-Agent)
$ua = $_SERVER[‘HTTP_USER_AGENT’];
```

![](img/5c22f2a66639ef69bfe4f8581ae405cf_114.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_116.png)

### 3. 构造并执行INSERT语句

![](img/5c22f2a66639ef69bfe4f8581ae405cf_118.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_120.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_122.png)

接下来，我们将接收到的数据和环境信息组合成一条SQL插入语句，并执行它。

![](img/5c22f2a66639ef69bfe4f8581ae405cf_124.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_126.png)

```php
// 构造SQL插入语句
// 使用双引号以便解析变量 $u, $c, $ip, $ua
// 字符串类型的值需要用单引号括起来
$sql = “INSERT INTO gb (username, content, ipaddr, useragent) VALUES (‘$u’, ‘$c’, ‘$ip’, ‘$ua’)”;

![](img/5c22f2a66639ef69bfe4f8581ae405cf_128.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_130.png)

// 执行SQL语句
$result = mysqli_query($conn, $sql);

![](img/5c22f2a66639ef69bfe4f8581ae405cf_132.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_134.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_136.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_138.png)

// 判断执行结果
if ($result) {
    echo “<script>alert(‘留言成功！’);</script>”;
} else {
    echo “留言失败：” . mysqli_error($conn);
}
```

![](img/5c22f2a66639ef69bfe4f8581ae405cf_140.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_142.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_143.png)

**核心概念：**
*   **`mysqli_connect()`**： 用于连接到MySQL数据库。
*   **`mysqli_query()`**： 执行一条SQL查询。
*   **`$_SERVER`**： 另一个超全局变量，包含了服务器和执行环境的信息。

![](img/5c22f2a66639ef69bfe4f8581ae405cf_145.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_147.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_148.png)

---

## 第五步：查询并显示留言列表 📋

![](img/5c22f2a66639ef69bfe4f8581ae405cf_150.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_152.png)

留言提交后，我们还需要将数据库中的所有留言显示在页面上。这需要用到SQL的 `SELECT` 查询语句。

![](img/5c22f2a66639ef69bfe4f8581ae405cf_154.png)

我们在 `gb.php` 页面下方添加显示留言的代码：

![](img/5c22f2a66639ef69bfe4f8581ae405cf_156.png)

```php
<?php
// ... 之前的插入代码 ...

![](img/5c22f2a66639ef69bfe4f8581ae405cf_158.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_160.png)

// 查询所有留言
$sql_select = “SELECT * FROM gb ORDER BY id DESC”; // 假设有自增id字段，按新到旧排序
$result_select = mysqli_query($conn, $sql_select);

![](img/5c22f2a66639ef69bfe4f8581ae405cf_162.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_164.png)

// 判断查询是否成功并有数据
if ($result_select && mysqli_num_rows($result_select) > 0) {
    echo “<hr><h3>留言列表：</h3>”;
    // 循环遍历结果集中的每一行数据
    while($row = mysqli_fetch_assoc($result_select)) {
        // $row 是一个关联数组，键名是字段名
        echo “用户名：” . $row[‘username’] . “<br>”;
        echo “内容：” . $row[‘content’] . “<br>”;
        echo “IP：” . $row[‘ipaddr’] . “ | 浏览器：” . $row[‘useragent’];
        echo “<hr>”;
    }
} else {
    echo “暂无留言。”;
}

![](img/5c22f2a66639ef69bfe4f8581ae405cf_166.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_168.png)

// 关闭数据库连接（非必需，脚本结束会自动关闭，但显式关闭是好习惯）
mysqli_close($conn);
?>
```

![](img/5c22f2a66639ef69bfe4f8581ae405cf_170.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_172.png)

**核心概念：**
*   **`SELECT * FROM table_name`**： SQL查询语句，用于从指定表获取数据。
*   **`mysqli_fetch_assoc()`**： 从结果集中取出一行作为关联数组。每次调用都会获取下一行，直到没有更多行时返回 `false`。
*   **`while` 循环**： 用于遍历查询结果的所有行。

![](img/5c22f2a66639ef69bfe4f8581ae405cf_174.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_176.png)

---

## 第六步：创建后台管理功能（删除留言） 🗑️

通常，我们需要一个后台界面来管理留言（例如删除不当言论）。我们创建一个新文件 `admin/gb.php` 作为后台。

![](img/5c22f2a66639ef69bfe4f8581ae405cf_178.png)

### 1. 显示留言并添加删除按钮

![](img/5c22f2a66639ef69bfe4f8581ae405cf_180.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_182.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_184.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_186.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_188.png)

后台页面也需要显示留言，但在每条留言后增加一个“删除”链接。

```php
// admin/gb.php
<?php
// 包含公共的数据库配置文件，避免重复代码
require_once ‘../config.php’; // 假设config.php里包含了数据库连接代码 $conn

![](img/5c22f2a66639ef69bfe4f8581ae405cf_190.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_192.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_194.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_195.png)

$sql = “SELECT * FROM gb”;
$result = mysqli_query($conn, $sql);

while($row = mysqli_fetch_assoc($result)){
    echo “留言：” . $row[‘content’];
    // 删除按钮：点击后跳转到当前页，并传递一个参数 del_id
    echo “ <a href=’?del_id=“ . $row[‘id’] . “‘>删除</a><br>”;
}
?>
```

![](img/5c22f2a66639ef69bfe4f8581ae405cf_197.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_199.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_201.png)

### 2. 处理删除请求

![](img/5c22f2a66639ef69bfe4f8581ae405cf_203.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_205.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_207.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_209.png)

当点击“删除”链接时，URL会变成 `admin/gb.php?del_id=1`。我们需要在页面顶部编写代码来接收这个参数并执行删除操作。

![](img/5c22f2a66639ef69bfe4f8581ae405cf_211.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_213.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_215.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_217.png)

```php
// admin/gb.php 顶部
<?php
require_once ‘../config.php’;

// 判断是否收到了要删除的ID
if(isset($_GET[‘del_id’])){
    $del_id = $_GET[‘del_id’];
    // 构造删除SQL语句，务必注意条件限制，否则会删除所有数据！
    $sql_del = “DELETE FROM gb WHERE id = $del_id”;
    $result_del = mysqli_query($conn, $sql_del);

    if($result_del){
        echo “<script>alert(‘删除成功！’);</script>”;
        // 可以重定向到当前页，刷新列表
        // echo “<script>location.href=’gb.php’;</script>”;
    } else {
        echo “删除失败！”;
    }
}
// ... 下面是显示留言的代码 ...
?>
```

![](img/5c22f2a66639ef69bfe4f8581ae405cf_219.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_221.png)

**注意：** 这个后台目前没有权限验证，任何人知道地址都可以访问和删除，这构成了一个**未授权访问漏洞**。在实际开发中，必须加入登录验证（如下节课将讲的Session或Cookie验证）。

![](img/5c22f2a66639ef69bfe4f8581ae405cf_223.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_225.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_227.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_229.png)

---

![](img/5c22f2a66639ef69bfe4f8581ae405cf_231.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_233.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_235.png)

## 第七步：引入第三方富文本编辑器 ✨

![](img/5c22f2a66639ef69bfe4f8581ae405cf_237.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_239.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_241.png)

为了让留言板支持图片、字体样式等富文本内容，我们可以引入第三方编辑器，如 UEditor。

![](img/5c22f2a66639ef69bfe4f8581ae405cf_243.png)

### 1. 引入编辑器资源

![](img/5c22f2a66639ef69bfe4f8581ae405cf_245.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_247.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_249.png)

将UEditor的整个文件夹放到项目目录中（例如 `ueditor/`）。

![](img/5c22f2a66639ef69bfe4f8581ae405cf_251.png)

### 2. 修改前端表单

![](img/5c22f2a66639ef69bfe4f8581ae405cf_253.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_255.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_257.png)

将原来的普通 `textarea` 替换为UEditor的容器。

![](img/5c22f2a66639ef69bfe4f8581ae405cf_259.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_261.png)

```html
<!-- 在gb.php的<head>标签内引入UEditor的JS文件 -->
<script type=“text/javascript” src=“ueditor/ueditor.config.js”></script>
<script type=“text/javascript” src=“ueditor/ueditor.all.js”></script>

![](img/5c22f2a66639ef69bfe4f8581ae405cf_263.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_265.png)

<!-- 修改表单中的内容输入框 -->
<form action=“” method=“post”>
    用户名：<input type=“text” name=“username”><br><br>
    内容：
    <!-- 使用UEditor提供的容器替代textarea -->
    <script id=“editor” type=“text/plain” name=“content” style=“width:800px;height:300px;”></script>
    <br><br>
    <input type=“submit” value=“提交留言”>
</form>

![](img/5c22f2a66639ef69bfe4f8581ae405cf_267.png)

<!-- 在页面底部实例化编辑器 -->
<script type=“text/javascript”>
    var ue = UE.getEditor(‘editor’); // ‘editor’ 是上面script标签的id
</script>
```

![](img/5c22f2a66639ef69bfe4f8581ae405cf_269.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_271.png)

### 3. 后端处理

![](img/5c22f2a66639ef69bfe4f8581ae405cf_273.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_275.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_277.png)

后端接收数据的代码完全不变，仍然是通过 `$_POST[‘content’]` 来获取编辑器生成的HTML内容。插入数据库和显示时，直接输出这段HTML即可。

![](img/5c22f2a66639ef69bfe4f8581ae405cf_279.png)

**核心概念：**
*   **第三方插件/库**： 引入他人编写好的代码来快速实现复杂功能。其安全性依赖于插件本身，如果插件存在漏洞（如文件上传漏洞），则会影响到你的应用。

![](img/5c22f2a66639ef69bfe4f8581ae405cf_281.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_283.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_285.png)

---

![](img/5c22f2a66639ef69bfe4f8581ae405cf_287.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_289.png)

## 总结与展望 🎯

![](img/5c22f2a66639ef69bfe4f8581ae405cf_291.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_293.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_295.png)

本节课中，我们一起学习并实践了一个完整PHP留言板应用的开发，涵盖了以下核心知识点：

![](img/5c22f2a66639ef69bfe4f8581ae405cf_297.png)

1.  **数据库设计**： 创建表和字段来定义数据结构。
2.  **前端交互**： 使用HTML表单收集用户输入。
3.  **PHP数据处理**： 利用 `$_POST`、`$_GET`、`$_SERVER` 等超全局变量接收请求和环境数据。
4.  **数据库操作**： 使用 `mysqli` 扩展进行数据库连接、执行 `INSERT`、`SELECT`、`DELETE` 等SQL语句。
5.  **功能模块化**： 通过包含 (`require_once`) 公共配置文件来复用代码。
6.  **第三方集成**： 引入UEditor富文本编辑器来增强前端功能。

![](img/5c22f2a66639ef69bfe4f8581ae405cf_299.png)

通过亲手构建这个应用，你清晰地看到了数据从浏览器表单到服务器PHP脚本，再到MySQL数据库，最后又查询返回到浏览器的完整闭环。理解这个流程是理解Web安全漏洞的基础。例如：

![](img/5c22f2a66639ef69bfe4f8581ae405cf_301.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_303.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_304.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_305.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_306.png)

*   在接收 `$_POST[‘username’]` 时，如果直接拼接到SQL语句中，就可能产生 **SQL注入漏洞**。
*   在将 `$row[‘content’]` 直接输出到页面时，如果内容包含恶意JS代码，就可能产生 **XSS跨站脚本漏洞**。
*   后台管理页面 `admin/gb.php` 缺少权限检查，就构成了 **未授权访问漏洞**。
*   使用的第三方编辑器UEditor，如果其自身有安全缺陷，则会引入 **第三方组件漏洞**。

![](img/5c22f2a66639ef69bfe4f8581ae405cf_308.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_310.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_312.png)

![](img/5c22f2a66639ef69bfe4f8581ae405cf_314.png)

在接下来的课程中，我们将在此基础上，逐步添加用户登录、会话管理、文件上传等功能，并同步分析这些功能中可能出现的各类安全漏洞及其成因。这将帮助你从“开发者”和“安全研究者”双重视角来理解Web应用。