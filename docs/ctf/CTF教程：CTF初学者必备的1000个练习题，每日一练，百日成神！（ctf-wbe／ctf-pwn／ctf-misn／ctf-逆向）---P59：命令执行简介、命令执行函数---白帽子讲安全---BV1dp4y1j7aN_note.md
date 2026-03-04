# CTF教程：P59：命令执行简介、命令执行函数 🚀

在本节课中，我们将要学习命令执行漏洞的基本概念，并深入了解在PHP中可能导致远程代码执行和系统命令执行的关键函数。理解这些函数的工作原理是发现和利用此类漏洞的基础。

![](img/961dcdb42817b6f3cf6036b20679e3ce_1.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_3.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_5.png)

## 命令执行漏洞简介 🔍

上一节我们介绍了CTF的多种题型，本节中我们来看看命令执行漏洞。

命令执行漏洞产生的原因是应用程序没有对用户输入做严格的检查过滤，导致用户输入的参数被当成命令来执行。这与SQL注入漏洞的原理有相似之处，都是由于对用户输入缺乏验证。

该漏洞的危害主要包括以下三点：
*   继承Web服务器权限执行系统命令。
*   反弹Shell获得目标服务器权限。
*   为进一步的内网渗透打下基础。

命令执行主要分为两种类型：远程代码执行和远程命令执行。它们的区别在于所调用的函数不同。

![](img/961dcdb42817b6f3cf6036b20679e3ce_7.png)

## 远程代码执行相关函数 💻

由于业务需求，PHP中有时需要调用执行命令的函数。如果程序使用这些函数时，没有对用户可控的参数进行检查过滤，就可能存在远程代码执行漏洞。

![](img/961dcdb42817b6f3cf6036b20679e3ce_9.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_10.png)

以下是常见的可能导致远程代码执行的函数及其利用方式。

![](img/961dcdb42817b6f3cf6036b20679e3ce_12.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_14.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_16.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_18.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_20.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_22.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_24.png)

### 1. eval 函数

![](img/961dcdb42817b6f3cf6036b20679e3ce_26.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_28.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_29.png)

`eval` 函数会把传入的字符串作为PHP代码执行。其基本形式为：
```php
eval($code);
```
常见的一句话木马就是利用此函数构造的：
```php
eval($_POST['cmd']);
```
需要注意的是，`eval` 传入的参数必须是合法的PHP代码（例如以分号结尾）。该函数功能非常危险，因为它允许执行任意PHP代码。

### 2. assert 函数

`assert` 函数与 `eval` 类似，也会将字符串当作PHP代码执行。
```php
assert($code);
```
它与 `eval` 的一个区别是，`assert` 直接传入字符串即可执行，不一定需要是完整的PHP代码形式。其常见利用形式为：
```php
assert($_POST['cmd']);
```
在代码前使用 `@` 符号可以忽略执行过程中产生的错误或警告。

![](img/961dcdb42817b6f3cf6036b20679e3ce_31.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_33.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_35.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_37.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_39.png)

### 3. preg_replace 函数

![](img/961dcdb42817b6f3cf6036b20679e3ce_41.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_43.png)

`preg_replace` 函数用于执行正则表达式的搜索和替换。
```php
preg_replace($pattern, $replacement, $subject);
```
当使用 `/e` 修饰符时，该函数在完成反向引用替换后，会将 `$replacement` 参数当作PHP代码执行（执行方式类似 `eval`）。其利用示例如下：
```php
// 当匹配到‘test’时，用$_POST[‘cmd’]的值替换，并执行该值作为PHP代码
preg_replace("/test/e", $_POST['cmd'], "test");
```
**注意**：`/e` 修饰符在 PHP 5.5.0 之后已被弃用。

### 4. array_map 函数

`array_map` 函数为数组的每个元素应用回调函数。
```php
array_map($callback, $array);
```
如果回调函数名可控，就可能造成代码执行。示例如下：
```php
$func = $_GET['func']; // 例如传入 ‘assert’
$cmd = $_POST['cmd'];   // 例如传入 ‘phpinfo();’
array_map($func, array($cmd));
```
这段代码会使用 `assert` 函数来处理 `$cmd` 数组中的值，从而执行 `phpinfo()`。

![](img/961dcdb42817b6f3cf6036b20679e3ce_45.png)

### 5. create_function 函数

![](img/961dcdb42817b6f3cf6036b20679e3ce_47.png)

`create_function` 函数用于创建一个匿名函数。
```php
create_function($args, $code);
```
其利用方式如下，传递的 `$code` 参数会被当作PHP代码执行：
```php
$func = create_function('', $_POST['cmd']);
$func();
```
**注意**：参数通常用单引号包裹，以防止变量名被解析。这与双引号中变量会被解析的特性不同。

### 6. call_user_func 函数

`call_user_func` 函数把第一个参数作为回调函数调用。
```php
call_user_func($callback, $parameter);
```
其利用方式如下：
```php
call_user_func($_GET['func'], $_POST['cmd']);
// 例如：func=assert&cmd=phpinfo();
```

![](img/961dcdb42817b6f3cf6036b20679e3ce_49.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_51.png)

### 7. array_filter 函数

![](img/961dcdb42817b6f3cf6036b20679e3ce_53.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_54.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_56.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_58.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_60.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_62.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_64.png)

`array_filter` 函数用回调函数过滤数组中的单元。
```php
array_filter($array, $callback);
```
其利用原理与 `array_map` 类似：
```php
$cmd = $_POST['cmd']; // 例如传入 ‘whoami’
$array = array($cmd);
$func = $_GET['func']; // 例如传入 ‘system’
array_filter($array, $func);
```
这段代码会使用 `system` 函数处理数组中的 `whoami` 命令。

![](img/961dcdb42817b6f3cf6036b20679e3ce_66.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_68.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_70.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_72.png)

### 关于双引号的特性

![](img/961dcdb42817b6f3cf6036b20679e3ce_74.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_76.png)

在PHP中，如果双引号包裹的字符串中包含变量，解释器会将其替换为变量解析后的值。如果变量值是一段可执行的PHP代码，就可能造成代码执行。
```php
$code = “phpinfo();”;
echo “$code”; // 输出 phpinfo() 的执行结果
```
而单引号中的内容则会被原样输出。

![](img/961dcdb42817b6f3cf6036b20679e3ce_78.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_80.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_82.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_84.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_86.png)

## 远程系统命令执行相关函数 ⚙️

![](img/961dcdb42817b6f3cf6036b20679e3ce_88.png)

上一节我们介绍了远程代码执行，本节中我们来看看远程系统命令执行。这类漏洞常出现在提供远程命令操作接口的应用中，例如路由器的ping测试功能。

以下是常见的系统命令执行函数。

### 1. exec 函数

![](img/961dcdb42817b6f3cf6036b20679e3ce_90.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_92.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_94.png)

`exec` 函数执行一个外部程序，但默认不输出结果，而是返回结果的最后一行。如需获取全部输出，可以使用第二个参数。
```php
exec($command, $output);
// $output 数组将保存命令执行的每一行结果
```

![](img/961dcdb42817b6f3cf6036b20679e3ce_96.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_97.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_98.png)

### 2. system 函数

![](img/961dcdb42817b6f3cf6036b20679e3ce_100.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_102.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_104.png)

`system` 函数执行外部命令，并直接将结果输出到浏览器。这是我们最常用来执行系统命令的函数。
```php
system($command);
```

![](img/961dcdb42817b6f3cf6036b20679e3ce_106.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_108.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_110.png)

### 3. passthru 函数

`passthru` 函数与 `system` 类似，也直接输出结果。它的特点是可用于输出二进制数据，如图像流。
```php
passthru($command);
```

![](img/961dcdb42817b6f3cf6036b20679e3ce_112.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_114.png)

### 4. shell_exec 函数

![](img/961dcdb42817b6f3cf6036b20679e3ce_116.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_118.png)

`shell_exec` 函数通过Shell环境执行命令，并以字符串的形式返回全部输出。
```php
shell_exec($command);
```

## 命令连接符 🔗

![](img/961dcdb42817b6f3cf6036b20679e3ce_120.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_122.png)

在利用命令执行漏洞时，常使用一些特殊符号来连接多条命令，实现更复杂的操作。

![](img/961dcdb42817b6f3cf6036b20679e3ce_124.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_126.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_128.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_130.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_132.png)

以下是常用的命令连接符：
*   **`|` (竖线)**：无论前一个命令是否成功，后一个命令都会执行。例如：`a | whoami`
*   **`;` (分号)**：在Unix系统中，用于顺序执行多条命令，无论前一条是否成功。例如：`pwd; whoami`
*   **`||` (双竖线)**：只有前一个命令执行失败时，才执行后一个命令。例如：`nonexistent || whoami`
*   **`&&` (双and号)**：只有前一个命令执行成功时，才执行后一个命令。例如：`pwd && whoami`
*   **`` ` ` `` (反引号) 或 `$()`**：用于执行特定的命令，并将其输出作为值。例如：echo \`whoami\`
*   **`>` 和 `<` (重定向符号)**：用于重定向命令的输入输出。

![](img/961dcdb42817b6f3cf6036b20679e3ce_134.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_136.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_137.png)

## 总结 📝

![](img/961dcdb42817b6f3cf6036b20679e3ce_139.png)

![](img/961dcdb42817b6f3cf6036b20679e3ce_141.png)

本节课中我们一起学习了命令执行漏洞的核心知识。

我们首先了解了命令执行漏洞的产生原因与危害。接着，深入探讨了PHP中可能导致**远程代码执行**的一系列函数，如 `eval`、`assert`、`preg_replace`、`array_map` 等，并通过示例理解了其利用原理。然后，我们学习了用于**远程系统命令执行**的函数，如 `exec`、`system`、`passthru`，这些函数常出现在需要调用系统命令的功能点中。最后，我们介绍了一些在拼接和利用命令时常用的**命令连接符**。

![](img/961dcdb42817b6f3cf6036b20679e3ce_143.png)

理解这些函数是进行代码审计、发现命令执行漏洞的关键。在实际场景中，我们需要在代码中寻找这些危险函数，并分析其参数是否用户可控且未经过滤，从而判断漏洞是否存在。