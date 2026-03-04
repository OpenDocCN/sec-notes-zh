# 0基础WEB安全教学，从开机到拿奖：P5：第五节 后端篇（下）php：阿巴阿巴，前端你在说什么 🧑‍💻

在本节课中，我们将要学习一门重要的后端语言——PHP。我们将了解后端的基本概念，学习PHP的核心语法，并通过一个完整的实例演示如何使用PHP连接数据库、处理用户请求，从而理解后端如何工作。

## 概述：什么是后端？

上一节我们介绍了前端技术，本节中我们来看看后端。后端是一系列处理用户数据和服务器数据的软件集合。这些软件互相传递和处理信息，最终将结果发送给客户端（例如网页前端）。

![](img/c635577666de1f24584abb3eeb42676b_1.png)

可以作为后端的语言非常多，例如C、Java、Go等。选择哪种语言取决于项目需求、开发效率、性能等多种因素。后端的基本思想可以概括为三个步骤：
1.  **接收请求**：从前端获取数据。
2.  **处理请求**：根据程序逻辑处理接收到的数据。
3.  **返回响应**：将处理结果发送回前端。

![](img/c635577666de1f24584abb3eeb42676b_3.png)

![](img/c635577666de1f24584abb3eeb42676b_4.png)

![](img/c635577666de1f24584abb3eeb42676b_5.png)

![](img/c635577666de1f24584abb3eeb42676b_7.png)

将后端代码与前端分离，主要出于安全性和维护性的考虑。后端代码运行在服务器上，只要没有漏洞，用户通常无法直接看到源代码，这增加了安全性。

![](img/c635577666de1f24584abb3eeb42676b_9.png)

## PHP语言简介

PHP是一门解释型语言。解释型语言没有编译器，而是通过一个解释器程序将我们编写的代码（本质上是字符串）转换成计算机可以执行的指令。

![](img/c635577666de1f24584abb3eeb42676b_11.png)

PHP文件以 `.php` 结尾。当你浏览网页时，如果网址以 `.php` 结尾，就说明该页面使用了PHP。

PHP代码写在特定的标签 `<?php ... ?>` 中。在这个标签内，既可以写PHP代码，也可以嵌入HTML、JavaScript等前端代码。但经过服务器解析后，用户只能看到最终生成的HTML，而看不到原始的PHP代码，这保证了代码的私密性。

PHP的语句以分号 `;` 结尾。它使用 `$` 符号来定义变量，并且是一门弱类型语言，变量可以存储任何类型的数据（字符串、整数等），无需预先声明类型。

![](img/c635577666de1f24584abb3eeb42676b_13.png)

![](img/c635577666de1f24584abb3eeb42676b_15.png)

**定义变量示例：**
```php
$a = 1; // 整数
$b = "hello"; // 字符串
$c = true; // 布尔值
```

## PHP核心概念与函数

以下是PHP中一些核心且常用的概念与函数。

### 1. 引入库文件
类似于C语言的 `#include`，PHP可以通过 `include` 或 `require` 语句引入外部文件（库），以使用预定义的函数和类。PHP安装后，许多扩展库需要在配置文件 `php.ini` 中启用。

### 2. 信息函数：`phpinfo()`
`phpinfo()` 函数会输出当前PHP环境的详细信息，包括版本、安装的扩展、配置文件路径等。这个函数在调试时很有用，但也会暴露敏感信息，因此在生产环境中应避免使用。在安全领域，攻击者成功入侵后，常会执行此函数来验证权限。

### 3. 判断函数：`is_*` 系列
PHP内置了许多以 `is_` 开头的函数，用于判断变量类型或状态，例如 `isset()`（检查变量是否已设置）、`is_numeric()`（检查是否为数字）。这些函数常用于表单验证等逻辑判断。

**示例：**
```php
if (isset($_POST[‘username‘])) {
    // 如果用户提交了username字段，则执行这里的代码
}
```

### 4. 数据库操作函数
PHP与MySQL数据库交互是后端开发的核心。以下是三个最基本的函数：
*   **`mysqli_connect()`**: 用于连接到MySQL数据库。需要提供主机名、用户名、密码、数据库名等参数。它返回一个数据库连接对象。
*   **`mysqli_query()`**: 执行一条SQL语句。需要传入数据库连接对象和SQL语句字符串。
*   **`mysqli_close()`**: 关闭之前打开的数据库连接，释放资源。

**注意**：`mysqli_*` 是MySQL改进扩展，用于较新版本（如5.0+）。旧版本使用 `mysql_*` 函数（不带 `i`），但已不推荐使用。

### 5. 接收请求数据：`$_GET` 与 `$_POST`
这是PHP中两个非常重要的超全局变量，用于获取前端通过表单提交的数据。
*   **`$_GET`**: 用于获取通过URL参数（GET请求）传递的数据。
*   **`$_POST`**: 用于获取通过表单体（POST请求）传递的数据。

它们的用法是：`$_GET[‘参数名‘]` 或 `$_POST[‘字段名‘]`，其中参数名/字段名对应前端HTML表单中 `input` 标签的 `name` 属性。

### 6. 输出函数：`echo`
`echo` 用于向网页输出字符串或HTML内容。它功能强大，不仅可以输出文本，还可以输出包含HTML、JavaScript的完整代码块。

### 7. 正则表达式函数
正则表达式用于匹配和操作复杂的字符串模式。
*   **`preg_match()`**: 用正则表达式匹配字符串，返回匹配的次数。
*   **`preg_replace()`**: 用正则表达式搜索并替换字符串。
这些函数常用于输入验证、数据过滤和文本处理，是构建简单“防火墙”的基础。

### 8. 文件操作函数
*   **`file_get_contents()`**: 读取整个文件的内容到一个字符串中。
*   **`file_put_contents()`**: 将一个字符串写入文件。

### 9. 对象操作符：`->`
在PHP中，使用 `->` 来访问对象的属性和方法。例如，从 `mysqli_connect()` 返回的数据库连接对象，可以通过 `->` 来调用其方法或访问属性。

![](img/c635577666de1f24584abb3eeb42676b_17.png)

**示例：**
```php
$link = mysqli_connect(...);
if ($link->connect_error) { // 访问对象的connect_error属性
    die("连接失败: " . $link->connect_error);
}
```

![](img/c635577666de1f24584abb3eeb42676b_19.png)

### 10. 定义函数：`function`
PHP使用 `function` 关键字定义函数，语法与JavaScript类似。由于是弱类型语言，无需指定参数和返回值的类型。

![](img/c635577666de1f24584abb3eeb42676b_21.png)

### 11. 安全过滤函数：`htmlspecialchars()`
这个函数将字符串中的特殊字符（如 `<`, `>`, `&`, `"`）转换为HTML实体（如 `&lt;`, `&gt;`）。这可以防止用户输入被误解为HTML或JavaScript代码，是一种简单的XSS（跨站脚本）攻击防护手段。

![](img/c635577666de1f24584abb3eeb42676b_23.png)

## 实战演练：用PHP实现表单数据入库

![](img/c635577666de1f24584abb3eeb42676b_25.png)

![](img/c635577666de1f24584abb3eeb42676b_27.png)

现在，我们将通过一个完整的例子，演示如何使用PHP接收前端表单数据，并将其存入MySQL数据库。

![](img/c635577666de1f24584abb3eeb42676b_28.png)

### 第一步：准备数据库
首先，我们需要在MySQL中创建一个数据库和表。

![](img/c635577666de1f24584abb3eeb42676b_30.png)

![](img/c635577666de1f24584abb3eeb42676b_31.png)

1.  打开命令行或MySQL客户端，登录数据库。
2.  创建一个数据库（例如 `test_db`）并选择使用它。
3.  创建一张表，包含两个字段（例如 `fans` 和 `blogger`）。

![](img/c635577666de1f24584abb3eeb42676b_33.png)

**SQL命令示例：**
```sql
CREATE DATABASE test_db;
USE test_db;
CREATE TABLE yyds (
    fans VARCHAR(50),
    blogger VARCHAR(50)
);
```

![](img/c635577666de1f24584abb3eeb42676b_35.png)

### 第二步：编写前端HTML表单
创建一个HTML文件（例如 `form.html`），包含一个表单。表单的 `action` 属性指向处理数据的PHP文件，`method` 属性设为 `post`。

![](img/c635577666de1f24584abb3eeb42676b_37.png)

![](img/c635577666de1f24584abb3eeb42676b_38.png)

**HTML代码示例：**
```html
<form action="process.php" method="post">
    粉丝名: <input type="text" name="fans_name"><br>
    博主名: <input type="text" name="blogger_name"><br>
    <input type="submit" value="提交">
</form>
```

![](img/c635577666de1f24584abb3eeb42676b_40.png)

![](img/c635577666de1f24584abb3eeb42676b_42.png)

### 第三步：编写PHP处理脚本
创建一个名为 `process.php` 的文件，编写后端逻辑。

**PHP代码示例：**
```php
<?php
// 1. 连接数据库
$link = mysqli_connect("localhost", "root", "你的密码", "test_db");
// 设置字符集为UTF-8，防止中文乱码
mysqli_set_charset($link, "utf8");

![](img/c635577666de1f24584abb3eeb42676b_44.png)

![](img/c635577666de1f24584abb3eeb42676b_45.png)

// 检查连接是否成功
if ($link->connect_error) {
    die("连接失败: " . $link->connect_error);
}

![](img/c635577666de1f24584abb3eeb42676b_47.png)

// 2. 接收前端POST过来的数据
$fans = $_POST[‘fans_name‘];
$blogger = $_POST[‘blogger_name‘];

![](img/c635577666de1f24584abb3eeb42676b_49.png)

// 3. 构造SQL插入语句
// 注意：变量值需要用单引号包裹，以满足SQL语法
$sql = "INSERT INTO yyds (fans, blogger) VALUES (‘$fans‘, ‘$blogger‘)";

![](img/c635577666de1f24584abb3eeb42676b_51.png)

// 4. 执行SQL语句
if (mysqli_query($link, $sql)) {
    echo "数据插入成功！粉丝是：" . $fans . "，博主是：" . $blogger;
} else {
    echo "错误: " . $sql . "<br>" . mysqli_error($link);
}

![](img/c635577666de1f24584abb3eeb42676b_53.png)

![](img/c635577666de1f24584abb3eeb42676b_55.png)

![](img/c635577666de1f24584abb3eeb42676b_57.png)

// 5. 关闭数据库连接
mysqli_close($link);
?>
```

![](img/c635577666de1f24584abb3eeb42676b_59.png)

![](img/c635577666de1f24584abb3eeb42676b_60.png)

![](img/c635577666de1f24584abb3eeb42676b_62.png)

![](img/c635577666de1f24584abb3eeb42676b_64.png)

**关键点说明：**
*   **连接数据库**：使用 `mysqli_connect` 并检查错误。
*   **获取数据**：通过 `$_POST[‘字段名‘]` 获取表单数据。
*   **构造SQL**：将变量嵌入SQL字符串时，**必须手动添加单引号**。这不是PHP自动完成的，而是SQL语法要求。这也是SQL注入漏洞产生的常见原因之一。
*   **字符串连接**：在PHP中，连接字符串使用点号 `.`，而不是加号 `+`。
*   **字符编码**：使用 `mysqli_set_charset($link, "utf8")` 可以避免在不同操作系统间传输中文数据时产生乱码。

![](img/c635577666de1f24584abb3eeb42676b_66.png)

![](img/c635577666de1f24584abb3eeb42676b_67.png)

![](img/c635577666de1f24584abb3eeb42676b_69.png)

![](img/c635577666de1f24584abb3eeb42676b_71.png)

### 第四步：测试运行
1.  将 `form.html` 和 `process.php` 放在Web服务器的目录下（例如小皮面板的 `www` 目录）。
2.  确保Apache（或Nginx）和MySQL服务已启动。
3.  在浏览器中访问 `form.html`，填写表单并提交。
4.  如果一切正常，页面会显示“数据插入成功”，并且数据已经保存到数据库的 `yyds` 表中。

![](img/c635577666de1f24584abb3eeb42676b_73.png)

![](img/c635577666de1f24584abb3eeb42676b_74.png)

![](img/c635577666de1f24584abb3eeb42676b_75.png)

## 总结

![](img/c635577666de1f24584abb3eeb42676b_76.png)

![](img/c635577666de1f24584abb3eeb42676b_78.png)

![](img/c635577666de1f24584abb3eeb42676b_80.png)

![](img/c635577666de1f24584abb3eeb42676b_82.png)

本节课中我们一起学习了后端的基本概念和PHP语言的核心知识。我们了解到后端负责接收、处理和响应请求，而PHP是一门非常适合Web开发的后端语言。

![](img/c635577666de1f24584abb3eeb42676b_84.png)

![](img/c635577666de1f24584abb3eeb42676b_86.png)

![](img/c635577666de1f24584abb3eeb42676b_87.png)

![](img/c635577666de1f24584abb3eeb42676b_88.png)

我们重点掌握了：
*   PHP的基本语法和变量定义。
*   如何使用 `$_GET` 和 `$_POST` 接收前端数据。
*   如何通过 `mysqli_*` 系列函数连接和操作MySQL数据库。
*   完成了一个从表单提交到数据入库的完整后端流程实例。

![](img/c635577666de1f24584abb3eeb42676b_90.png)

![](img/c635577666de1f24584abb3eeb42676b_92.png)

![](img/c635577666de1f24584abb3eeb42676b_93.png)

![](img/c635577666de1f24584abb3eeb42676b_95.png)

记住，学习编程语言的关键在于理解其核心思想并动手实践。PHP的核心就是处理HTTP请求和数据库交互。掌握了这些，你就已经具备了用PHP构建简单后端服务的能力。在后续的课程中，我们将基于这些知识，深入探讨Web安全相关的议题。