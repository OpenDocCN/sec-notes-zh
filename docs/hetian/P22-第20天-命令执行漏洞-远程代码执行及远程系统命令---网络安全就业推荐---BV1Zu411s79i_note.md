# 网络安全就业推荐 P22：第20天：命令执行漏洞-远程代码执行及远程系统命令 🛡️💻

在本节课中，我们将要学习命令执行漏洞，特别是远程代码执行和远程系统命令执行。我们将了解其产生原因、危害、常见的利用函数以及基本的防护方法。课程内容将分为简介、漏洞利用、防护方法和实验操作四个部分。

## 命令执行漏洞简介 🔍

上一节我们介绍了课程的整体结构，本节中我们来看看命令执行漏洞的基本概念。

![](img/8339aa24cef9f504b345e3283f978cc3_1.png)

![](img/8339aa24cef9f504b345e3283f978cc3_3.png)

命令执行漏洞产生的原因是：应用没有对用户输入做严格的检查过滤，导致用户输入的参数被当成命令来执行。这与SQL注入漏洞类似，都是由于未对用户输入进行充分验证而引发的安全问题。

![](img/8339aa24cef9f504b345e3283f978cc3_5.png)

![](img/8339aa24cef9f504b345e3283f978cc3_7.png)

![](img/8339aa24cef9f504b345e3283f978cc3_8.png)

![](img/8339aa24cef9f504b345e3283f978cc3_9.png)

![](img/8339aa24cef9f504b345e3283f978cc3_11.png)

![](img/8339aa24cef9f504b345e3283f978cc3_12.png)

关于该漏洞的危害，主要有以下三点：
1.  继承Web服务器权限去执行系统命令。
2.  反弹Shell，获得目标服务器权限。
3.  为进一步的内网渗透提供跳板。

![](img/8339aa24cef9f504b345e3283f978cc3_13.png)

![](img/8339aa24cef9f504b345e3283f978cc3_15.png)

![](img/8339aa24cef9f504b345e3283f978cc3_17.png)

![](img/8339aa24cef9f504b345e3283f978cc3_19.png)

![](img/8339aa24cef9f504b345e3283f978cc3_21.png)

## 远程代码执行（RCE） 🚨

命令执行主要分为远程代码执行和远程系统命令执行两种。它们的区别在于所调用的函数不同。在PHP中，有时因业务需求会调用一些执行命令的函数。

![](img/8339aa24cef9f504b345e3283f978cc3_23.png)

![](img/8339aa24cef9f504b345e3283f978cc3_25.png)

![](img/8339aa24cef9f504b345e3283f978cc3_26.png)

![](img/8339aa24cef9f504b345e3283f978cc3_28.png)

以下是常见的可能导致远程代码执行的PHP函数：

![](img/8339aa24cef9f504b345e3283f978cc3_29.png)

### 1. eval() 函数
`eval()` 函数会把传入的字符串作为PHP代码执行。其基本形式为 `eval($code);`。在一句话木马中常被使用。
```php
eval($_POST[‘cmd‘]);
```
需要注意的是，传入 `eval()` 的参数必须是合法的PHP代码（以分号结尾）。该函数非常危险，因为它允许执行任意PHP代码。

### 2. assert() 函数
`assert()` 函数与 `eval()` 类似，也会将字符串当做PHP代码执行。但与 `eval()` 不同的是，它传入的字符串不需要以分号结尾。
```php
assert($_POST[‘cmd‘]);
```

![](img/8339aa24cef9f504b345e3283f978cc3_31.png)

![](img/8339aa24cef9f504b345e3283f978cc3_33.png)

### 3. preg_replace() 函数
`preg_replace()` 函数用于执行正则表达式的搜索和替换。其利用原理与 `/e` 修饰符有关，该修饰符使得替换后的字符串会被 `eval()` 方式执行。
```php
preg_replace(“/test/e“, $_GET[‘cmd‘], “just test“);
```
需要注意的是，此利用方式在PHP 5.5.0 及以上版本中已被废弃。

### 4. array_map() 函数
`array_map()` 函数的作用是为数组的每个元素应用回调函数。通过构造，可以将用户输入作为函数名来执行。
```php
$func = $_GET[‘func‘];
$cmd = $_POST[‘cmd‘];
array_map($func, array($cmd));
```
例如，传递 `func=system&cmd=whoami` 即可执行系统命令。

### 5. create_function() 函数
`create_function()` 函数用于创建一个匿名函数。通过向该函数传递参数，可以导致代码执行。
```php
$func = create_function(‘‘, $_POST[‘cmd‘]);
$func();
```

![](img/8339aa24cef9f504b345e3283f978cc3_35.png)

![](img/8339aa24cef9f504b345e3283f978cc3_37.png)

![](img/8339aa24cef9f504b345e3283f978cc3_39.png)

![](img/8339aa24cef9f504b345e3283f978cc3_41.png)

![](img/8339aa24cef9f504b345e3283f978cc3_43.png)

![](img/8339aa24cef9f504b345e3283f978cc3_45.png)

![](img/8339aa24cef9f504b345e3283f978cc3_47.png)

### 6. call_user_func() 函数
`call_user_func()` 函数用于调用回调函数。第一个参数是回调函数，其余参数是回调函数的参数。
```php
call_user_func($_GET[‘func‘], $_POST[‘cmd‘]);
```

![](img/8339aa24cef9f504b345e3283f978cc3_48.png)

![](img/8339aa24cef9f504b345e3283f978cc3_49.png)

![](img/8339aa24cef9f504b345e3283f978cc3_51.png)

![](img/8339aa24cef9f504b345e3283f978cc3_53.png)

![](img/8339aa24cef9f504b345e3283f978cc3_55.png)

### 7. array_filter() 函数
`array_filter()` 函数用回调函数过滤数组中的单元。其利用方式与 `array_map()` 类似。
```php
$func = $_GET[‘func‘];
$cmd = $_POST[‘cmd‘];
array_filter(array($cmd), $func);
```

![](img/8339aa24cef9f504b345e3283f978cc3_57.png)

![](img/8339aa24cef9f504b345e3283f978cc3_59.png)

![](img/8339aa24cef9f504b345e3283f978cc3_61.png)

![](img/8339aa24cef9f504b345e3283f978cc3_63.png)

![](img/8339aa24cef9f504b345e3283f978cc3_65.png)

### 关于双引号的特性
在PHP中，双引号包裹的字符串中的变量会被解析。如果变量值包含PHP代码，该代码会被执行。
```php
$cmd = “phpinfo();“;
echo “$cmd“; // 会输出 phpinfo() 的执行结果
```
而单引号包裹的字符串中的变量不会被解析。

![](img/8339aa24cef9f504b345e3283f978cc3_67.png)

![](img/8339aa24cef9f504b345e3283f978cc3_69.png)

## 远程系统命令执行 ⚙️

上一节我们介绍了远程代码执行，本节中我们来看看远程系统命令执行。这种漏洞通常出现在应用提供了调用系统命令的接口时，例如网络设备的管理界面中的Ping测试功能。

![](img/8339aa24cef9f504b345e3283f978cc3_71.png)

![](img/8339aa24cef9f504b345e3283f978cc3_73.png)

以下是常见的用于执行系统命令的PHP函数：

![](img/8339aa24cef9f504b345e3283f978cc3_75.png)

![](img/8339aa24cef9f504b345e3283f978cc3_77.png)

![](img/8339aa24cef9f504b345e3283f978cc3_79.png)

![](img/8339aa24cef9f504b345e3283f978cc3_81.png)

### 1. exec() 函数
`exec()` 函数用于执行一个外部程序。它不会直接输出结果，而是返回结果的最后一行。如果需要全部结果，可以使用第二个参数（数组）来存储输出。
```php
exec($_POST[‘cmd‘], $output);
print_r($output);
```

![](img/8339aa24cef9f504b345e3283f978cc3_83.png)

![](img/8339aa24cef9f504b345e3283f978cc3_85.png)

![](img/8339aa24cef9f504b345e3283f978cc3_87.png)

![](img/8339aa24cef9f504b345e3283f978cc3_89.png)

### 2. system() 函数
`system()` 函数用于执行外部程序，并直接输出结果到浏览器。这是一句话木马中常用的函数。
```php
system($_POST[‘cmd‘]);
```

![](img/8339aa24cef9f504b345e3283f978cc3_91.png)

![](img/8339aa24cef9f504b345e3283f978cc3_93.png)

### 3. passthru() 函数
`passthru()` 函数与 `system()` 类似，也会直接将结果输出到浏览器。它的特点是能够输出二进制数据，例如图像流。
```php
passthru($_POST[‘cmd‘]);
```

![](img/8339aa24cef9f504b345e3283f978cc3_95.png)

![](img/8339aa24cef9f504b345e3283f978cc3_97.png)

### 4. shell_exec() 函数
`shell_exec()` 函数通过Shell环境执行命令，并以字符串的形式返回完整的输出。
```php
echo shell_exec($_POST[‘cmd‘]);
```

## 命令执行中的特殊字符 🔗

![](img/8339aa24cef9f504b345e3283f978cc3_99.png)

![](img/8339aa24cef9f504b345e3283f978cc3_101.png)

![](img/8339aa24cef9f504b345e3283f978cc3_103.png)

![](img/8339aa24cef9f504b345e3283f978cc3_105.png)

![](img/8339aa24cef9f504b345e3283f978cc3_106.png)

在利用命令执行漏洞时，常会使用一些特殊字符来连接或构造命令。

![](img/8339aa24cef9f504b345e3283f978cc3_107.png)

![](img/8339aa24cef9f504b345e3283f978cc3_109.png)

![](img/8339aa24cef9f504b345e3283f978cc3_111.png)

![](img/8339aa24cef9f504b345e3283f978cc3_113.png)

![](img/8339aa24cef9f504b345e3283f978cc3_114.png)

![](img/8339aa24cef9f504b345e3283f978cc3_116.png)

![](img/8339aa24cef9f504b345e3283f978cc3_118.png)

![](img/8339aa24cef9f504b345e3283f978cc3_120.png)

![](img/8339aa24cef9f504b345e3283f978cc3_122.png)

以下是常见的命令连接符：
*   **分号 `;`**：无论前一个命令是否成功，后面的命令都会执行。例如：`a; whoami`
*   **管道符 `|`**：将前一个命令的输出作为后一个命令的输入。例如：`a | whoami`
*   **双管道符 `||`**：只有在前一个命令执行失败时，才执行后面的命令。例如：`a || whoami`
*   **双与号 `&&`**：只有在前一个命令执行成功时，才执行后面的命令。例如：`pwd && whoami`
*   **反引号 `` ` ``**：用于执行特定的命令。例如：`` `whoami` ``
*   **重定向符号 `>` 和 `<`**：用于重定向命令的输入输出。例如：`whoami > test.txt`

![](img/8339aa24cef9f504b345e3283f978cc3_124.png)

![](img/8339aa24cef9f504b345e3283f978cc3_126.png)

![](img/8339aa24cef9f504b345e3283f978cc3_128.png)

![](img/8339aa24cef9f504b345e3283f978cc3_130.png)

![](img/8339aa24cef9f504b345e3283f978cc3_132.png)

![](img/8339aa24cef9f504b345e3283f978cc3_134.png)

## 总结 📝

![](img/8339aa24cef9f504b345e3283f978cc3_136.png)

![](img/8339aa24cef9f504b345e3283f978cc3_138.png)

本节课中我们一起学习了命令执行漏洞。我们首先了解了漏洞的成因与危害。然后，我们深入探讨了远程代码执行，学习了一系列危险的PHP函数（如 `eval()`, `assert()`, `array_map()` 等）及其利用方式。接着，我们介绍了远程系统命令执行，分析了 `exec()`, `system()`, `passthru()` 等函数的特点。最后，我们了解了在命令拼接中常用的特殊字符。

![](img/8339aa24cef9f504b345e3283f978cc3_140.png)

理解这些函数和原理，有助于我们在进行代码审计时，快速定位潜在的漏洞点，并通过构造特定输入来验证漏洞。实际环境中，命令执行漏洞往往需要通过白盒测试（代码审计）来发现。下节课我们将通过实验来进一步巩固对这些漏洞的理解和利用。