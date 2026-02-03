# i春秋学院 进阶篇 PHP代码审计 - P3：第二节 常见的INI配置 🛠️

在本节课中，我们将学习PHP中常见的INI配置文件及其关键设置。理解这些配置对于代码审计、安全分析和环境调试至关重要。

## 配置文件概述 📄

PHP的配置主要通过INI文件进行管理。以下是主要的配置文件类型：

*   **php.ini**：这是PHP的主配置文件，包含了所有默认的配置指令。在某些环境下，也可能命名为 `php-[version].ini`。
*   **.user.ini**：这是一个用户级别的配置文件，类似于Apache的 `.htaccess` 文件。它可以放置在网站目录及其子目录中，用于覆盖该目录范围内的PHP设置。
*   **httpd.conf**：在Apache服务器中，也可以直接在此文件中使用特定的语法来覆盖 `php.ini` 中的值。

不同的配置文件具有不同的配置权限。具体的权限细节可以参考PHP官方文档。

![](img/b4ac35ac54e4f45ee8e934fdeae79711_1.png)

## 配置文件语法 📝

![](img/b4ac35ac54e4f45ee8e934fdeae79711_3.png)

PHP配置文件的语法与常见的INI文件格式相似。基本结构是指令名等于值。

```
指令名 = 值
```

![](img/b4ac35ac54e4f45ee8e934fdeae79711_5.png)

指令名是大小写敏感的。值可以是字符串、数字、PHP常量、INI常量或表达式。

![](img/b4ac35ac54e4f45ee8e934fdeae79711_7.png)

上一节我们介绍了配置文件的基本概念和语法，本节中我们来看看一些常见且重要的具体配置项。

![](img/b4ac35ac54e4f45ee8e934fdeae79711_9.png)

## 变量相关配置 🔧

以下是关于PHP变量处理的两个重要配置。

### 全局变量注册 (`register_globals`)

![](img/b4ac35ac54e4f45ee8e934fdeae79711_11.png)

这个配置项的作用是将 `$_GET`、`$_POST` 等超全局数组中的键值直接注册为全局变量。例如，当 `register_globals = On` 且URL中有 `?id=1` 时，脚本中会直接存在一个 `$id` 变量，其值为 `1`。

![](img/b4ac35ac54e4f45ee8e934fdeae79711_13.png)

**代码示例：**
```php
// 假设 register_globals = On，且请求为 ?test=hello
echo $test; // 输出：hello
```

这个配置在新版PHP中已被移除，因为它带来了严重的安全问题：
1.  **变量来源不明确**：无法判断 `$test` 是来自 `$_GET`、`$_POST` 还是 `$_COOKIE`，导致代码难以阅读和维护。
2.  **变量覆盖风险**：用户可以通过请求参数覆盖程序内部定义的变量，引发安全问题。

因此，该配置默认是关闭的。

### 短标签 (`short_open_tag`)

这个配置允许PHP代码使用缩写形式 `<? ?>` 来代替完整的 `<?php ?>` 标签。

**代码示例：**
```php
<?=$test?> // 短标签，直接输出变量 $test
<?php echo $test; ?> // 标准写法
```

![](img/b4ac35ac54e4f45ee8e934fdeae79711_15.png)

**为何关注此配置？** 在安全审计中，某些上传过滤程序可能只检测 `<?php` 标签。如果服务器开启了短标签，攻击者就可以使用 `<?=` 或 `<?` 来构造变形的WebShell，绕过简单的过滤。

## 安全模式与函数禁用 🛡️

![](img/b4ac35ac54e4f45ee8e934fdeae79711_17.png)

### 安全模式 (`safe_mode`)
安全模式是一个限制环境，旨在解决共享服务器环境下的安全问题。它会限制文件操作、限制环境变量、禁用某些函数等。由于其设计复杂且效率低下，**该特性已在PHP 5.4.0及以上版本中被移除**。我们仅作了解即可。

![](img/b4ac35ac54e4f45ee8e934fdeae79711_19.png)

![](img/b4ac35ac54e4f45ee8e934fdeae79711_21.png)

### 禁用函数 (`disable_functions`)
这是一个极其重要且常用的安全配置。它允许管理员在 `php.ini` 中禁用特定的PHP函数，通常是那些危险函数，如执行系统命令、操作文件系统的函数。

**配置示例：**
```
disable_functions = system, exec, shell_exec, passthru, proc_open, eval
```
配置后，任何尝试调用这些函数的代码都会失败，并抛出“函数被禁用”的警告。这是防止WebShell执行危险操作的有效手段之一。

![](img/b4ac35ac54e4f45ee8e934fdeae79711_23.png)

上一节我们讨论了变量和安全相关的配置，本节中我们来看看与文件操作相关的设置。

## 上传文件及目录权限 📁

![](img/b4ac35ac54e4f45ee8e934fdeae79711_25.png)

以下是控制文件上传和脚本可访问目录的关键配置。

*   **`file_uploads = On`**：控制是否允许HTTP文件上传，默认为开启。
*   **`upload_max_filesize = 8M`**：设置允许上传文件的最大大小。
*   **`upload_tmp_dir`**：指定上传文件的临时存储目录。如果未设置，则使用系统默认的临时目录。
*   **`open_basedir`**：这是最重要的目录限制配置之一。它将PHP脚本可访问的文件系统限制在指定的目录及其子目录内，常用于将脚本限制在网站根目录，防止目录遍历攻击。

![](img/b4ac35ac54e4f45ee8e934fdeae79711_27.png)

**配置示例：**
```
open_basedir = /var/www/html/:/tmp/
```
在Windows系统中，使用分号 `;` 分隔多个目录。此配置能有效阻止脚本访问 `/etc/passwd` 等敏感系统文件。

![](img/b4ac35ac54e4f45ee8e934fdeae79711_29.png)

## 错误信息配置 🐛

错误信息的显示配置对开发和审计至关重要，但在生产环境中应关闭以避免信息泄露。

*   **`display_errors = Off`**：生产环境应设置为 `Off`，不向用户浏览器输出错误详情。
*   **`error_reporting = E_ALL`**：设置错误报告的级别。在开发或审计时，建议设置为 `E_ALL` 以显示所有错误和警告，帮助定位问题。

![](img/b4ac35ac54e4f45ee8e934fdeae79711_31.png)

![](img/b4ac35ac54e4f45ee8e934fdeae79711_33.png)

**审计建议：** 在代码审计时，通常需要在入口文件临时开启错误显示，以便发现隐藏的警告和通知。
```php
ini_set('display_errors', 1);
error_reporting(E_ALL);
```

![](img/b4ac35ac54e4f45ee8e934fdeae79711_35.png)

## 魔术引号及远程文件 ⚙️

![](img/b4ac35ac54e4f45ee8e934fdeae79711_37.png)

### 魔术引号 (`magic_quotes_gpc`)
此特性会对 `$_GET`、`$_POST`、`$_COOKIE` 传入的数据自动进行转义（在特殊字符如单引号前加反斜杠 `\`），初衷是防止SQL注入。但由于其效率低下且转义策略僵化（并非所有数据都需要入库），**该特性已在PHP 5.4.0及以上版本中被移除**。现代应用应使用参数化查询（PDO预处理）等方法来防止注入。

![](img/b4ac35ac54e4f45ee8e934fdeae79711_39.png)

![](img/b4ac35ac54e4f45ee8e934fdeae79711_41.png)

### 远程文件包含 (`allow_url_include`)
此配置控制 `include`、`require` 等函数是否允许包含远程（HTTP/FTP）文件。

![](img/b4ac35ac54e4f45ee8e934fdeae79711_43.png)

![](img/b4ac35ac54e4f45ee8e934fdeae79711_45.png)

*   **`allow_url_include = Off`**（默认）：禁止包含远程文件，这是安全设置。
*   **`allow_url_fopen = On`**（默认）：允许使用 `fopen()` 等函数打开远程文件。

![](img/b4ac35ac54e4f45ee8e934fdeae79711_46.png)

**安全风险：** 如果 `allow_url_include` 被开启，且程序存在文件包含漏洞，攻击者就可能直接包含远程服务器上的恶意代码并执行，造成严重的远程代码执行（RCE）漏洞。**务必确保此配置为 `Off`**。

![](img/b4ac35ac54e4f45ee8e934fdeae79711_48.png)

## 课程总结 📚

![](img/b4ac35ac54e4f45ee8e934fdeae79711_50.png)

![](img/b4ac35ac54e4f45ee8e934fdeae79711_52.png)

本节课我们一起学习了PHP中常见的INI配置：
1.  认识了 `php.ini`、`.user.ini` 等配置文件及其作用。
2.  了解了已废弃的 `register_globals`、`safe_mode`、`magic_quotes_gpc` 等配置的历史与风险。
3.  掌握了 `short_open_tag`、`disable_functions`、`open_basedir`、`allow_url_include` 等关键安全配置的含义和影响。
4.  学会了如何通过 `display_errors` 和 `error_reporting` 来辅助代码调试与审计。

![](img/b4ac35ac54e4f45ee8e934fdeae79711_54.png)

![](img/b4ac35ac54e4f45ee8e934fdeae79711_56.png)

理解这些配置是进行PHP代码审计和安全加固的基础，能帮助你更好地分析代码运行环境、发现潜在的安全隐患。