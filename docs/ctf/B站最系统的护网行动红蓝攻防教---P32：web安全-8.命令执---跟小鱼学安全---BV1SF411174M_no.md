# 护网行动红蓝攻防教程：P32：Web安全-8. 命令执行简介、命令执行函数 🛡️💻

![](img/736626bc981b23bce5e68fd7511c8d25_1.png)

![](img/736626bc981b23bce5e68fd7511c8d25_3.png)

![](img/736626bc981b23bce5e68fd7511c8d25_5.png)

在本节课中，我们将要学习Web安全中的一个重要漏洞：命令执行。我们将了解命令执行漏洞产生的原因、危害，并重点学习PHP中可能导致远程代码执行和系统命令执行的关键函数。通过理解这些函数的工作原理，我们可以更好地识别和防范此类漏洞。

## 命令执行漏洞简介 🔍

上一节我们介绍了Web安全的多种攻击方式，本节中我们来看看命令执行漏洞。命令执行漏洞产生的原因是应用程序没有对用户输入做严格的检查过滤，导致用户输入的参数被当成命令来执行。

该漏洞的危害主要包括以下三点：
*   继承Web服务器权限去执行系统命令。
*   反弹Shell以获得目标服务器权限。
*   为进一步的内网渗透打下基础。

![](img/736626bc981b23bce5e68fd7511c8d25_7.png)

![](img/736626bc981b23bce5e68fd7511c8d25_8.png)

命令执行主要分为两种类型：远程代码执行和远程命令执行。它们的区别在于所调用的函数不同。

![](img/736626bc981b23bce5e68fd7511c8d25_10.png)

![](img/736626bc981b23bce5e68fd7511c8d25_12.png)

![](img/736626bc981b23bce5e68fd7511c8d25_14.png)

![](img/736626bc981b23bce5e68fd7511c8d25_16.png)

![](img/736626bc981b23bce5e68fd7511c8d25_18.png)

## 远程代码执行相关函数 📜

![](img/736626bc981b23bce5e68fd7511c8d25_20.png)

![](img/736626bc981b23bce5e68fd7511c8d25_22.png)

![](img/736626bc981b23bce5e68fd7511c8d25_24.png)

![](img/736626bc981b23bce5e68fd7511c8d25_26.png)

![](img/736626bc981b23bce5e68fd7511c8d25_27.png)

由于业务需求，在PHP中有时需要调用执行命令的函数。如果程序使用了这些函数，且未对用户可控的参数进行检查过滤，就可能存在远程代码执行漏洞。

以下是几个关键的远程代码执行函数：

![](img/736626bc981b23bce5e68fd7511c8d25_29.png)

![](img/736626bc981b23bce5e68fd7511c8d25_31.png)

![](img/736626bc981b23bce5e68fd7511c8d25_33.png)

![](img/736626bc981b23bce5e68fd7511c8d25_35.png)

![](img/736626bc981b23bce5e68fd7511c8d25_37.png)

![](img/736626bc981b23bce5e68fd7511c8d25_39.png)

### 1. eval 函数
`eval` 函数会把传入的字符串作为PHP代码执行。其形式如下：
```php
eval($code);
```
例如，一句话木马常利用此函数：
```php
eval($_POST['cmd']);
```
**注意**：`eval` 传入的参数必须是合法的PHP代码（以分号结尾）。该函数允许执行任意PHP代码，因此非常危险。

![](img/736626bc981b23bce5e68fd7511c8d25_41.png)

![](img/736626bc981b23bce5e68fd7511c8d25_43.png)

### 2. assert 函数
`assert` 函数与 `eval` 类似，也会将字符串当作PHP代码执行。
```php
assert($code);
```
它与 `eval` 的主要区别在于，`assert` 直接传入字符串即可执行，不需要以分号结尾。同样可以用于构造一句话木马。

### 3. preg_replace 函数
`preg_replace` 函数用于执行正则表达式的搜索和替换。其结构为：
```php
preg_replace($pattern, $replacement, $subject);
```
当使用 `/e` 修饰符时，替换后的字符串会被当作PHP代码执行（类似于 `eval`）。例如：
```php
preg_replace("/test/e", $_GET['cmd'], "just test");
```
**注意**：`/e` 修饰符在 PHP 5.5.0 后被弃用。

![](img/736626bc981b23bce5e68fd7511c8d25_45.png)

![](img/736626bc981b23bce5e68fd7511c8d25_47.png)

### 4. array_map 函数
`array_map` 函数为数组的每个元素应用回调函数。其形式为：
```php
array_map($callback, $array);
```
如果回调函数名是用户可控的，就可能造成命令执行。例如：
```php
$func = $_GET['func'];
$cmd = $_POST['cmd'];
array_map($func, array($cmd));
```
当用户传入 `func=system` 和 `cmd=whoami` 时，就会执行系统命令。

### 5. create_function 函数
`create_function` 函数用于创建一个匿名函数。其形式为：
```php
create_function($args, $code);
```
传入的 `$code` 参数会被当作PHP代码执行。例如：
```php
$func = create_function('', $_POST['cmd']);
$func();
```
**注意**：参数通常用单引号包裹，以防止变量名被解析。

![](img/736626bc981b23bce5e68fd7511c8d25_49.png)

### 6. call_user_func 函数
`call_user_func` 函数用于调用回调函数。其形式为：
```php
call_user_func($callback, $parameter);
```
第一个参数 `$callback` 是调用的回调函数，其余参数是传入回调函数的参数。如果回调函数名和参数用户可控，就可能造成命令执行。

![](img/736626bc981b23bce5e68fd7511c8d25_51.png)

![](img/736626bc981b23bce5e68fd7511c8d25_53.png)

![](img/736626bc981b23bce5e68fd7511c8d25_54.png)

![](img/736626bc981b23bce5e68fd7511c8d25_56.png)

![](img/736626bc981b23bce5e68fd7511c8d25_58.png)

![](img/736626bc981b23bce5e68fd7511c8d25_60.png)

![](img/736626bc981b23bce5e68fd7511c8d25_62.png)

![](img/736626bc981b23bce5e68fd7511c8d25_64.png)

![](img/736626bc981b23bce5e68fd7511c8d25_66.png)

![](img/736626bc981b23bce5e68fd7511c8d25_68.png)

![](img/736626bc981b23bce5e68fd7511c8d25_70.png)

### 7. array_filter 函数
`array_filter` 函数用回调函数过滤数组中的单元。其形式为：
```php
array_filter($array, $callback);
```
与 `array_map` 类似，如果回调函数名用户可控，就可能造成命令执行。

![](img/736626bc981b23bce5e68fd7511c8d25_72.png)

![](img/736626bc981b23bce5e68fd7511c8d25_74.png)

### 关于双引号的特性
在PHP中，如果双引号内包含变量，解释器会将其替换为变量解析后的值。如果变量值是一段PHP代码，就可能造成代码执行。而单引号内的变量不会被处理。

![](img/736626bc981b23bce5e68fd7511c8d25_76.png)

![](img/736626bc981b23bce5e68fd7511c8d25_78.png)

![](img/736626bc981b23bce5e68fd7511c8d25_80.png)

![](img/736626bc981b23bce5e68fd7511c8d25_82.png)

![](img/736626bc981b23bce5e68fd7511c8d25_84.png)

![](img/736626bc981b23bce5e68fd7511c8d25_86.png)

![](img/736626bc981b23bce5e68fd7511c8d25_88.png)

## 远程系统命令执行相关函数 ⚙️

上一节我们介绍了远程代码执行函数，本节中我们来看看远程系统命令执行。这类漏洞通常出现在提供远程命令操作接口的应用中，如路由器、防火墙的管理界面。

以下是几个常见的系统命令执行函数：

![](img/736626bc981b23bce5e68fd7511c8d25_90.png)

![](img/736626bc981b23bce5e68fd7511c8d25_92.png)

![](img/736626bc981b23bce5e68fd7511c8d25_94.png)

![](img/736626bc981b23bce5e68fd7511c8d25_96.png)

![](img/736626bc981b23bce5e68fd7511c8d25_97.png)

![](img/736626bc981b23bce5e68fd7511c8d25_98.png)

### 1. exec 函数
`exec` 函数用于执行一个外部程序（系统命令）。它不会直接输出结果，而是返回结果的最后一行。如果需要全部结果，可以使用第二个参数（数组）来存储。
```php
exec($command, $output);
```

![](img/736626bc981b23bce5e68fd7511c8d25_100.png)

![](img/736626bc981b23bce5e68fd7511c8d25_102.png)

![](img/736626bc981b23bce5e68fd7511c8d25_104.png)

![](img/736626bc981b23bce5e68fd7511c8d25_106.png)

![](img/736626bc981b23bce5e68fd7511c8d25_108.png)

### 2. system 函数
`system` 函数也是用于执行外部命令。与 `exec` 的主要区别在于，`system` 会直接将结果输出到浏览器。
```php
system($command);
```

![](img/736626bc981b23bce5e68fd7511c8d25_110.png)

### 3. passthru 函数
`passthru` 函数同样用于执行外部命令并直接输出结果。它的特点是能够输出二进制数据，如图像数据，而 `exec` 和 `system` 无法做到。
```php
passthru($command);
```

![](img/736626bc981b23bce5e68fd7511c8d25_112.png)

![](img/736626bc981b23bce5e68fd7511c8d25_114.png)

![](img/736626bc981b23bce5e68fd7511c8d25_116.png)

![](img/736626bc981b23bce5e68fd7511c8d25_118.png)

### 4. shell_exec 函数
`shell_exec` 函数通过Shell环境执行命令，并以字符串的形式返回全部输出结果。
```php
shell_exec($command);
```

## 命令执行中的特殊字符 🔗

![](img/736626bc981b23bce5e68fd7511c8d25_120.png)

![](img/736626bc981b23bce5e68fd7511c8d25_122.png)

![](img/736626bc981b23bce5e68fd7511c8d25_124.png)

![](img/736626bc981b23bce5e68fd7511c8d25_126.png)

![](img/736626bc981b23bce5e68fd7511c8d25_128.png)

![](img/736626bc981b23bce5e68fd7511c8d25_130.png)

在利用命令执行漏洞时，常会使用一些特殊字符来连接或构造命令。

![](img/736626bc981b23bce5e68fd7511c8d25_132.png)

![](img/736626bc981b23bce5e68fd7511c8d25_133.png)

![](img/736626bc981b23bce5e68fd7511c8d25_134.png)

![](img/736626bc981b23bce5e68fd7511c8d25_136.png)

![](img/736626bc981b23bce5e68fd7511c8d25_138.png)

以下是常用的特殊字符及其作用：
*   **竖线 `|`**：无论前一个命令是否成功，后一个命令都会被执行。例如：`a | whoami`。
*   **分号 `;`**：在Unix系统中，用于分隔多个命令，按顺序执行。例如：`pwd; whoami`。
*   **双竖线 `||`**：只有在前一个命令执行失败时，才会执行后一个命令。例如：`nonexistent || whoami`。
*   **与符号 `&&`**：只有在前一个命令执行成功时，才会执行后一个命令。例如：`pwd && whoami`。
*   **反引号 `` ` ``**：用于执行特定的命令，并将输出结果替换到当前位置。例如：`echo `whoami``。
*   **重定向符号 `>` 和 `<`**：用于重定向命令的输入输出。

![](img/736626bc981b23bce5e68fd7511c8d25_140.png)

![](img/736626bc981b23bce5e68fd7511c8d25_142.png)

## 总结 📝

![](img/736626bc981b23bce5e68fd7511c8d25_144.png)

本节课中我们一起学习了命令执行漏洞。我们首先了解了该漏洞的产生原因和巨大危害。然后，我们深入学习了PHP中两类主要的命令执行函数：导致远程代码执行的函数（如 `eval`, `assert`, `preg_replace` 等）和直接执行系统命令的函数（如 `exec`, `system`, `passthru` 等）。理解这些函数的工作原理和利用方式，是发现和防御命令执行漏洞的关键。最后，我们还介绍了在构造攻击命令时常用的一些特殊字符。下节课我们将通过实验来加深对命令执行漏洞利用的理解。