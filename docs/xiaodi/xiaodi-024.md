#  024：PHP应用开发 - 文件管理模块（显示与上传）及安全过滤

![](img/d46d35f9da3b449d82f2d2e69503e40e_1.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_3.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_5.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_7.png)

在本节课中，我们将学习PHP文件管理模块的核心功能，包括文件列表的显示和文件上传的实现。同时，我们会探讨如何通过黑名单、白名单和MIME类型过滤来增强上传功能的安全性，并分析这些功能可能引发的安全漏洞及其防御措施。

![](img/d46d35f9da3b449d82f2d2e69503e40e_9.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_11.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_13.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_15.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_17.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_19.png)

---

![](img/d46d35f9da3b449d82f2d2e69503e40e_21.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_23.png)

## 🔍 上节回顾与本节概述

![](img/d46d35f9da3b449d82f2d2e69503e40e_25.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_27.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_29.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_31.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_33.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_35.png)

上一节我们介绍了使用Token机制防止暴力破解等安全问题的原理。本节中，我们将深入PHP的文件操作，学习如何构建一个基础的文件管理器，并实现安全的上传过滤。

![](img/d46d35f9da3b449d82f2d2e69503e40e_37.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_38.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_40.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_41.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_42.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_44.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_46.png)

文件管理模块通常包含以下功能：**文件上传**、**文件下载**、**文件删除**、**文件编辑**和**文件包含**。今天我们将重点讲解**文件列表显示**和**文件上传**这两个部分。

![](img/d46d35f9da3b449d82f2d2e69503e40e_48.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_50.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_52.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_54.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_56.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_57.png)

---

![](img/d46d35f9da3b449d82f2d2e69503e40e_59.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_61.png)

## 📂 文件上传功能实现

![](img/d46d35f9da3b449d82f2d2e69503e40e_63.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_65.png)

文件上传是Web应用中的常见功能。在PHP中，我们使用全局变量 `$_FILES` 来处理上传的文件。

![](img/d46d35f9da3b449d82f2d2e69503e40e_67.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_69.png)

### 核心代码：接收上传文件信息

![](img/d46d35f9da3b449d82f2d2e69503e40e_71.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_73.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_75.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_77.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_78.png)

以下是接收上传文件基本信息（如文件名、类型、大小、临时路径）的代码：

![](img/d46d35f9da3b449d82f2d2e69503e40e_80.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_82.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_84.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_86.png)

```php
// 接收上传的文件信息
$name = $_FILES['file']['name'];     // 原始文件名
$type = $_FILES['file']['type'];     // 文件MIME类型
$size = $_FILES['file']['size'];     // 文件大小（字节）
$tmp_name = $_FILES['file']['tmp_name']; // 临时文件路径
$error = $_FILES['file']['error'];   // 错误代码
```

![](img/d46d35f9da3b449d82f2d2e69503e40e_88.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_90.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_91.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_93.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_95.png)

**代码说明**：
*   `‘file‘` 对应HTML表单中文件输入框的 `name` 属性。
*   `tmp_name` 是文件上传到服务器后的临时存储路径，脚本执行完毕后会被清除。
*   `error` 为0表示上传成功，其他值代表不同类型的错误。

![](img/d46d35f9da3b449d82f2d2e69503e40e_97.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_99.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_100.png)

### 核心代码：移动临时文件

![](img/d46d35f9da3b449d82f2d2e69503e40e_102.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_104.png)

获取文件信息后，需要使用 `move_uploaded_file()` 函数将临时文件移动到我们指定的目录：

![](img/d46d35f9da3b449d82f2d2e69503e40e_106.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_108.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_110.png)

```php
// 定义目标路径
$target_dir = "uploads/";
$target_file = $target_dir . basename($name);

![](img/d46d35f9da3b449d82f2d2e69503e40e_112.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_114.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_116.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_118.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_120.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_121.png)

// 移动文件
if (move_uploaded_file($tmp_name, $target_file)) {
    echo "文件上传成功！";
} else {
    echo "文件上传失败。";
}
```

![](img/d46d35f9da3b449d82f2d2e69503e40e_123.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_125.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_126.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_128.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_130.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_132.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_134.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_136.png)

---

![](img/d46d35f9da3b449d82f2d2e69503e40e_138.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_140.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_142.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_144.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_146.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_148.png)

## 🛡️ 文件上传安全过滤

![](img/d46d35f9da3b449d82f2d2e69503e40e_150.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_152.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_154.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_156.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_157.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_159.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_161.png)

单纯实现上传功能是不安全的，必须对上传的文件进行严格过滤。常见的过滤机制有三种。

![](img/d46d35f9da3b449d82f2d2e69503e40e_163.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_165.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_167.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_169.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_171.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_173.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_175.png)

### 1. 黑名单过滤

![](img/d46d35f9da3b449d82f2d2e69503e40e_177.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_179.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_181.png)

黑名单机制是**禁止上传特定危险后缀**的文件（如 `.php`, `.asp`, `.jsp`）。

![](img/d46d35f9da3b449d82f2d2e69503e40e_183.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_185.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_187.png)

**核心逻辑**：
1.  定义不允许的后缀列表。
2.  获取用户上传文件的后缀名。
3.  判断该后缀是否在黑名单中。

![](img/d46d35f9da3b449d82f2d2e69503e40e_189.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_191.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_193.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_194.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_196.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_198.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_200.png)

以下是实现代码：

![](img/d46d35f9da3b449d82f2d2e69503e40e_202.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_204.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_206.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_208.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_210.png)

```php
// 定义黑名单后缀
$black_ext = array('php', 'asp', 'jsp', 'aspx');

![](img/d46d35f9da3b449d82f2d2e69503e40e_212.png)

// 获取上传文件的后缀名
$filename = $_FILES['file']['name'];
$ext = strtolower(pathinfo($filename, PATHINFO_EXTENSION));

![](img/d46d35f9da3b449d82f2d2e69503e40e_214.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_216.png)

// 判断是否在黑名单中
if (in_array($ext, $black_ext)) {
    die("非法后缀文件，禁止上传！");
} else {
    // 执行安全的上传操作
    move_uploaded_file($tmp_name, $target_file);
    echo "上传成功！";
}
```

![](img/d46d35f9da3b449d82f2d2e69503e40e_218.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_220.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_222.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_224.png)

**安全隐患**：攻击者可能使用不在名单内的脚本后缀（如 `.php5`, `.phtml`）进行绕过，如果服务器环境支持解析这些后缀，则漏洞依然存在。

![](img/d46d35f9da3b449d82f2d2e69503e40e_226.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_228.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_230.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_232.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_234.png)

### 2. 白名单过滤

白名单机制是**只允许上传特定安全后缀**的文件（如图片格式 `.jpg`, `.png`, `.gif`）。这是比黑名单更安全的策略。

![](img/d46d35f9da3b449d82f2d2e69503e40e_236.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_238.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_240.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_242.png)

**核心逻辑**：
1.  定义允许的后缀列表。
2.  获取用户上传文件的后缀名。
3.  判断该后缀是否在白名单中。

![](img/d46d35f9da3b449d82f2d2e69503e40e_244.png)

以下是实现代码：

```php
// 定义白名单后缀
$white_ext = array('jpg', 'jpeg', 'png', 'gif');

![](img/d46d35f9da3b449d82f2d2e69503e40e_246.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_248.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_250.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_251.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_253.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_255.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_257.png)

// 获取上传文件的后缀名
$filename = $_FILES['file']['name'];
$ext = strtolower(pathinfo($filename, PATHINFO_EXTENSION));

![](img/d46d35f9da3b449d82f2d2e69503e40e_259.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_261.png)

// 判断是否在白名单中
if (!in_array($ext, $white_ext)) {
    die("非法文件类型，仅允许上传图片！");
} else {
    // 执行安全的上传操作
    move_uploaded_file($tmp_name, $target_file);
    echo "上传成功！";
}
```

### 3. MIME类型过滤

![](img/d46d35f9da3b449d82f2d2e69503e40e_263.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_265.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_267.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_269.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_271.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_273.png)

MIME类型是浏览器在HTTP头中声明的文件类型。我们可以通过检查 `$_FILES[‘file‘][‘type‘]` 的值来进行过滤。

![](img/d46d35f9da3b449d82f2d2e69503e40e_275.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_277.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_279.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_281.png)

**核心逻辑**：
1.  定义允许的MIME类型列表（如图片类型）。
2.  获取上传文件的MIME类型。
3.  判断该类型是否在允许列表中。

![](img/d46d35f9da3b449d82f2d2e69503e40e_283.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_285.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_287.png)

以下是实现代码：

![](img/d46d35f9da3b449d82f2d2e69503e40e_289.png)

```php
// 定义允许的MIME类型
$allowed_mime = array('image/jpeg', 'image/png', 'image/gif');

![](img/d46d35f9da3b449d82f2d2e69503e40e_291.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_293.png)

// 获取上传文件的MIME类型
$file_mime = $_FILES['file']['type'];

![](img/d46d35f9da3b449d82f2d2e69503e40e_295.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_297.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_299.png)

// 判断MIME类型是否合法
if (!in_array($file_mime, $allowed_mime)) {
    die("非法文件类型，禁止上传！");
} else {
    // 执行上传操作
    move_uploaded_file($tmp_name, $target_file);
    echo "上传成功！";
}
```

![](img/d46d35f9da3b449d82f2d2e69503e40e_301.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_303.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_305.png)

**安全隐患**：MIME类型完全由客户端控制，攻击者可以通过抓包工具（如Burp Suite）轻易修改 `Content-Type` 头，伪装成合法类型进行绕过。**因此，绝不能仅依赖MIME类型进行验证**，必须结合白名单后缀检查。

![](img/d46d35f9da3b449d82f2d2e69503e40e_307.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_309.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_311.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_313.png)

**最佳实践**：在实际开发中，应组合使用**白名单后缀过滤**和**服务器端文件内容检测**（如使用 `getimagesize()` 函数验证图片文件真伪），以构建多层次防御。

![](img/d46d35f9da3b449d82f2d2e69503e40e_315.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_317.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_319.png)

---

![](img/d46d35f9da3b449d82f2d2e69503e40e_321.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_322.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_323.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_325.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_326.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_328.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_330.png)

## 📁 文件列表显示与目录遍历

文件管理器需要能够读取并显示指定目录下的文件和文件夹。

![](img/d46d35f9da3b449d82f2d2e69503e40e_332.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_334.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_335.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_336.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_338.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_340.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_342.png)

### 核心函数
*   `is_dir($path)`：判断路径是否为目录。
*   `opendir($path)`：打开目录句柄。
*   `readdir($handle)`：从目录句柄中读取条目。
*   `closedir($handle)`：关闭目录句柄。

![](img/d46d35f9da3b449d82f2d2e69503e40e_343.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_345.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_347.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_348.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_350.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_351.png)

### 基础实现代码

![](img/d46d35f9da3b449d82f2d2e69503e40e_353.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_355.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_357.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_359.png)

以下代码实现了读取并显示当前目录下的所有文件和文件夹：

![](img/d46d35f9da3b449d82f2d2e69503e40e_361.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_363.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_365.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_367.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_369.png)

```php
function listFiles($dir = '.') {
    if (is_dir($dir)) {
        if ($dh = opendir($dir)) {
            while (($file = readdir($dh)) !== false) {
                if ($file != '.' && $file != '..') { // 排除当前和上级目录
                    $filepath = $dir . '/' . $file;
                    if (is_dir($filepath)) {
                        echo "[目录] " . $file . "<br>";
                    } else {
                        echo "[文件] " . $file . "<br>";
                    }
                }
            }
            closedir($dh);
        }
    }
}
// 调用函数，显示当前目录
listFiles();
```

![](img/d46d35f9da3b449d82f2d2e69503e40e_371.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_373.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_375.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_377.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_379.png)

### 安全风险：目录遍历漏洞

![](img/d46d35f9da3b449d82f2d2e69503e40e_381.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_383.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_385.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_387.png)

如果文件列表功能接收用户输入的路径参数（如 `?path=./uploads`），并且没有进行严格过滤，攻击者可能通过输入 `../../../` 这样的路径序列来访问服务器上的任意目录，造成**目录遍历漏洞**（或路径穿越漏洞）。

![](img/d46d35f9da3b449d82f2d2e69503e40e_389.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_391.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_393.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_395.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_397.png)

**漏洞示例**：
```php
// 不安全的代码：直接使用用户输入作为路径
$user_path = $_GET['path'];
listFiles($user_path); // 攻击者可传入 ../../../etc/passwd
```

**防御措施**：
1.  **路径固定**：在PHP配置文件（php.ini）中设置 `open_basedir`，将PHP脚本可访问的文件限制在特定目录树内。
    ```ini
    open_basedir = /var/www/html/
    ```
2.  **代码过滤**：在代码中对用户输入的路径进行规范化，并过滤掉 `..`、`../` 等危险字符。
    ```php
    $user_path = $_GET['path'];
    // 过滤目录穿越字符
    if (strpos($user_path, '..') !== false) {
        die('禁止跨目录访问！');
    }
    // 或者将路径限制在特定基础目录下
    $base_dir = '/var/www/html/uploads/';
    $real_path = realpath($base_dir . $user_path);
    // 检查最终路径是否仍在基础目录内
    if (strpos($real_path, $base_dir) === 0) {
        listFiles($real_path);
    } else {
        die('非法路径！');
    }
    ```

![](img/d46d35f9da3b449d82f2d2e69503e40e_399.png)

---

![](img/d46d35f9da3b449d82f2d2e69503e40e_401.png)

## 📝 本节课总结

![](img/d46d35f9da3b449d82f2d2e69503e40e_403.png)

在本节课中，我们一起学习了PHP文件管理模块的两个核心功能：

![](img/d46d35f9da3b449d82f2d2e69503e40e_405.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_407.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_409.png)

1.  **文件上传**：我们掌握了使用 `$_FILES` 全局变量和 `move_uploaded_file()` 函数实现上传的基本方法，并深入探讨了三种安全过滤机制：
    *   **黑名单过滤**：禁止特定危险后缀，但存在被未知危险后缀绕过的风险。
    *   **白名单过滤**：只允许特定安全后缀，是更可靠的策略。
    *   **MIME类型过滤**：检查客户端声明的文件类型，但极易被绕过，需与其他方法结合使用。

![](img/d46d35f9da3b449d82f2d2e69503e40e_411.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_413.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_415.png)

2.  **文件列表显示**：我们学习了使用目录操作函数读取和显示文件列表，并分析了由此可能引发的**目录遍历漏洞**。我们介绍了两种主要的防御思路：通过PHP配置 (`open_basedir`) 进行访问控制，以及在代码层面对用户输入进行严格的路径校验和过滤。

![](img/d46d35f9da3b449d82f2d2e69503e40e_417.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_419.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_421.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_423.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_425.png)

![](img/d46d35f9da3b449d82f2d2e69503e40e_427.png)

理解这些功能的实现原理和安全边界，是进行安全开发、代码审计和漏洞测试的基础。下节课，我们将继续完善文件管理器，实现文件的下载、删除和编辑功能。