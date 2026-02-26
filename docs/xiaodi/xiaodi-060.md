#  060：PHP反序列化漏洞详解（第60课）🔐

![](img/4877fe6b504dcf6abe3752ac36d22108_1.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_3.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_5.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_7.png)

在本节课中，我们将要学习PHP反序列化漏洞中的三个核心知识点：属性类型特征、CVE-2016-7124（__wakeup绕过）漏洞、字符串逃逸漏洞以及原生类的利用。这些知识点在CTF比赛和面试中经常出现，是理解PHP反序列化高级利用的关键。

![](img/4877fe6b504dcf6abe3752ac36d22108_9.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_11.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_13.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_15.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_17.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_19.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_21.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_23.png)

## 一、属性类型特征 📝

![](img/4877fe6b504dcf6abe3752ac36d22108_25.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_27.png)

上一节我们介绍了反序列化的基础概念，本节中我们来看看PHP对象属性类型在序列化数据中的特征。

![](img/4877fe6b504dcf6abe3752ac36d22108_29.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_31.png)

在PHP面向对象编程中，类的属性有三种可见性类型：`public`（公有）、`protected`（受保护）和`private`（私有）。这些类型在序列化字符串中有不同的表示方式，了解这一点有助于我们分析流量特征和构造payload。

![](img/4877fe6b504dcf6abe3752ac36d22108_33.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_35.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_37.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_39.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_41.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_43.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_45.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_47.png)

**核心公式：**
*   `public`属性：正常显示属性名，如 `s:3:"age";i:20;`
*   `protected`属性：显示为 `\x00*\x00属性名`，如 `s:6:"\x00*\x00six";s:4:"mail";`
*   `private`属性：显示为 `\x00类名\x00属性名`，如 `s:9:"\x00test\x00age";i:20;`

![](img/4877fe6b504dcf6abe3752ac36d22108_49.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_51.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_53.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_55.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_57.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_59.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_61.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_63.png)

以下是一个示例代码，展示了不同属性类型的序列化结果差异：

![](img/4877fe6b504dcf6abe3752ac36d22108_65.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_67.png)

```php
class test {
    public $name = 'xiaodi';
    private $age = 20;
    protected $six = 'mail';
}
$s = new test();
echo serialize($s);
// 输出类似：O:4:"test":3:{s:4:"name";s:6:"xiaodi";s:9:"\x00test\x00age";i:20;s:6:"\x00*\x00six";s:4:"mail";}
```

![](img/4877fe6b504dcf6abe3752ac36d22108_69.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_71.png)

**关键点：**
序列化时，PHP通过添加 `\x00`（空字符）和类名或 `*` 来区分属性的可见性，以确保反序列化时能正确还原属性类型。这不是错误，而是其内部机制。

## 二、CVE-2016-7124：__wakeup绕过漏洞 🚫

![](img/4877fe6b504dcf6abe3752ac36d22108_73.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_75.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_77.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_79.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_81.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_83.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_85.png)

本节我们将探讨一个历史悠久的PHP反序列化漏洞CVE-2016-7124，它允许攻击者在特定条件下绕过 `__wakeup` 魔术方法的执行。

![](img/4877fe6b504dcf6abe3752ac36d22108_87.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_89.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_91.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_93.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_95.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_97.png)

`__wakeup()` 魔术方法在对象被 `unserialize()` 反序列化时**自动调用**。但该漏洞指出：**当序列化字符串中表示对象属性数量的值大于实际属性数量时，会跳过 `__wakeup()` 方法的执行**。

![](img/4877fe6b504dcf6abe3752ac36d22108_99.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_101.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_103.png)

**影响版本：** PHP 5.6.x < 5.6.25， PHP 7.x < 7.0.10。

### 漏洞原理与演示

以下代码展示了正常的 `__wakeup()` 调用：

```php
class test {
    public function __wakeup() {
        echo "__wakeup被调用！";
    }
}
$s = new test();
$data = serialize($s);
// 正常序列化数据：O:4:"test":0:{}
unserialize($data); // 输出 "__wakeup被调用！"
```

![](img/4877fe6b504dcf6abe3752ac36d22108_105.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_106.png)

要绕过 `__wakeup`，我们需要修改序列化数据中表示属性数量的部分。例如，将 `O:4:"test":0:{}` 中的属性数量 `0` 改为一个更大的数，如 `2`：`O:4:"test":2:{}`。在某些CTF题目中，这可以用于绕过关键的安全检查。

![](img/4877fe6b504dcf6abe3752ac36d22108_108.png)

### 实战案例：极客大挑战2019

![](img/4877fe6b504dcf6abe3752ac36d22108_110.png)

题目源码中有一个类，其 `__wakeup()` 方法会将 `username` 重置为 `guest`，而获取flag的条件是 `username === 'admin'`。通过利用CVE-2016-7124漏洞，我们可以绕过 `__wakeup()`，使 `username` 保持为我们序列化时设置的 `admin`，从而获取flag。

**解题步骤：**
1.  正常序列化一个对象，设置 `username='admin'`, `password=100`。
2.  将序列化字符串中对象属性数量的值（例如`2`）修改为大于2的数（如`3`或`4`）。
3.  提交修改后的序列化字符串，绕过 `__wakeup()` 中的重置操作，满足条件获得flag。

## 三、字符串逃逸漏洞（增减字符）🔀

![](img/4877fe6b504dcf6abe3752ac36d22108_112.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_114.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_116.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_118.png)

字符串逃逸是反序列化中一个精巧的利用技巧，其本质是**利用过滤函数对序列化字符串中特定字符的“增加”或“减少”操作，改变原字符串的结构，从而“逃逸”出新的、被解析的对象属性**。这通常出现在先序列化，后过滤，再反序列化的场景中。

![](img/4877fe6b504dcf6abe3752ac36d22108_120.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_122.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_124.png)

### 1. 字符“增加”场景

假设过滤函数将 `admin` 替换为 `hacker`（5位变6位，增加1位）。

**攻击思路：** 我们通过输入大量 `admin`，使得过滤后增加的字符总长度，恰好“吃掉”后面原本属于值的一部分。被“吃掉”的部分之后的内容，会被当做新的序列化键值对解析。

![](img/4877fe6b504dcf6abe3752ac36d22108_126.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_128.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_130.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_132.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_134.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_136.png)

**构造公式：**
假设我们想让后面隐藏的 `";s:5:"isVIP";i:1;}` 被成功解析。
1.  计算需要“吃掉”的字符长度 `L`（即隐藏payload的长度）。
2.  因为每替换一个 `admin` 增加1位，所以需要输入 `L` 个 `admin`。
3.  最终构造：`username='adminadminadmin...'(共L个) + 隐藏payload`。

![](img/4877fe6b504dcf6abe3752ac36d22108_138.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_140.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_142.png)

过滤后，增加的 `L` 个字符将 `隐藏payload` 之前的字符串部分整体后推，使其恰好成为完整的键值对，而 `隐藏payload` 则被成功解析为新属性。

![](img/4877fe6b504dcf6abe3752ac36d22108_144.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_146.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_148.png)

### 2. 字符“减少”场景

![](img/4877fe6b504dcf6abe3752ac36d22108_150.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_152.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_154.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_156.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_158.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_160.png)

假设过滤函数将 `hacker` 替换为 `admin`（6位变5位，减少1位）。

![](img/4877fe6b504dcf6abe3752ac36d22108_162.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_164.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_166.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_168.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_170.png)

**攻击思路：** 与增加相反，我们需要让过滤后减少的字符，使得后续字符串的一部分“前移”，成为前一个键值对的值的一部分，从而让我们精心构造的后半部分被独立解析。

![](img/4877fe6b504dcf6abe3752ac36d22108_172.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_174.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_176.png)

**构造公式：**
假设我们想逃逸的payload是 `";s:8:"password";s:6:"123456";s:5:"isVIP";i:1;}`。
1.  计算payload长度 `L`。
2.  因为每替换一个 `hacker` 减少1位，所以需要在前面可控的位置（如某个属性值里）填充 `L` 个 `hacker`。
3.  过滤后，减少的 `L` 位使得后续内容前移，恰好让我们的逃逸payload被独立解析出来。

![](img/4877fe6b504dcf6abe3752ac36d22108_178.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_180.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_182.png)

### 实战技巧
*   **关键：** 精确计算长度。需要根据过滤规则（增加/减少位数）和逃逸payload的长度，反推需要输入的原字符数量。
*   **观察：** 注意序列化字符串中的长度标识（如 `s:5:"value"`），必须与实际字符串长度匹配，否则反序列化会失败。

![](img/4877fe6b504dcf6abe3752ac36d22108_184.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_186.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_188.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_190.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_192.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_194.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_196.png)

## 四、原生类（Native Class）利用 🛠️

![](img/4877fe6b504dcf6abe3752ac36d22108_198.png)

当目标代码本身没有定义或触发任何魔术方法时，我们可以利用PHP内置的**原生类**进行攻击。这些原生类本身可能包含有用的方法或会在特定操作（如被 `echo`、调用不存在方法）时触发某些行为。

![](img/4877fe6b504dcf6abe3752ac36d22108_200.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_202.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_204.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_206.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_208.png)

### 利用场景与步骤
1.  **确定触发点：** 分析代码会如何操作反序列化得到的对象（如 `echo $obj` 会触发 `__toString`，调用 `$obj->xxx()` 会触发 `__call`）。
2.  **寻找合适原生类：** 使用脚本遍历在当前PHP环境下所有包含魔术方法（如 `__toString`, `__call`, `__get` 等）的原生类。
    ```php
    // 示例：查找所有包含 __toString 的原生类
    $classes = get_declared_classes();
    foreach ($classes as $class) {
        if (method_exists($class, '__toString')) {
            echo $class . "\n";
        }
    }
    ```
3.  **研究类功能：** 查阅PHP官方手册，了解找到的原生类的功能。常见有用的原生类有：
    *   `Exception` / `Error`：`__toString` 方法会打印栈跟踪信息，可包含文件路径或构造XSS。
    *   `SoapClient`：`__call` 方法可用于发起SSRF（服务器端请求伪造）请求。
    *   `SplFileObject`：可用于读取文件。
4.  **构造利用链：** 实例化选定的原生类，并设置其属性，使其在触发时执行我们期望的操作（如读取文件、发起网络请求）。

![](img/4877fe6b504dcf6abe3752ac36d22108_210.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_212.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_214.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_216.png)

### 实战案例：CTFShow Web259

![](img/4877fe6b504dcf6abe3752ac36d22108_218.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_220.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_222.png)

题目反序列化后调用了不存在的方法 `getFlag()`，这会触发 `__call` 魔术方法。

![](img/4877fe6b504dcf6abe3752ac36d22108_224.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_226.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_228.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_230.png)

**解题步骤：**
1.  扫描发现 `SoapClient` 类具有 `__call` 方法。
2.  `SoapClient` 在调用不可用方法时，会尝试发送SOAP请求。我们可以利用它构造一个访问本地 `flag.php` 的SSRF请求。
3.  在请求头中设置 `X-Forwarded-For: 127.0.0.1` 以满足题目IP限制，并在Cookie中设置 `token=ctfshow` 以满足token条件。
4.  序列化构造好的 `SoapClient` 对象并提交，服务器会代表我们访问 `flag.php`，从而将flag写入文件。

![](img/4877fe6b504dcf6abe3752ac36d22108_232.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_234.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_236.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_238.png)

**关键点：** 原生类的利用高度依赖于PHP环境开启的扩展（如 `soap` 扩展），因为并非所有原生类默认可用。

![](img/4877fe6b504dcf6abe3752ac36d22108_240.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_242.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_244.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_246.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_248.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_250.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_252.png)

## 总结 📚

![](img/4877fe6b504dcf6abe3752ac36d22108_254.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_256.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_258.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_260.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_262.png)

本节课中我们一起学习了PHP反序列化的四个高级主题：
1.  **属性类型特征**：理解了`public`、`protected`、`private`属性在序列化字符串中的不同表示，有助于分析流量和构造payload。
2.  **CVE-2016-7124漏洞**：掌握了通过修改对象属性数量绕过 `__wakeup()` 魔术方法的方法，尽管该漏洞影响版本较老，但在CTF中仍有出现。
3.  **字符串逃逸漏洞**：深入理解了利用过滤函数对字符的“增”或“减”，通过精密的长度计算来改变序列化字符串结构，实现属性注入的利用技巧。这是本章的难点和重点。
4.  **原生类利用**：学习了在无自定义魔术方法可用时，如何寻找并利用PHP内置原生类的魔术方法或功能（如 `SoapClient` 发起SSRF，`Exception` 泄露信息）来达到攻击目的。

![](img/4877fe6b504dcf6abe3752ac36d22108_264.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_266.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_268.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_270.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_272.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_274.png)

![](img/4877fe6b504dcf6abe3752ac36d22108_276.png)

这些知识点构成了PHP反序列化复杂利用的基础，需要结合实践反复练习和思考，尤其是字符串逃逸的长度计算逻辑。