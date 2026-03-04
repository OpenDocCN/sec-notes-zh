# 网络安全：P10：命令执行简介与函数

![](img/e287933d7cb8536f931a5caf2ff4c3d5_1.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_3.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_5.png)

在本节课中，我们将学习命令执行漏洞的基本概念，并深入了解在PHP中可能导致远程代码执行和系统命令执行的关键函数。理解这些函数的工作原理是识别和利用此类漏洞的基础。

## 命令执行漏洞简介

命令执行漏洞产生的原因是应用程序未对用户输入进行严格的检查与过滤，导致用户输入的参数被服务器当作命令来执行。这与SQL注入的原理有相似之处，都是由于对用户输入信任过度而引发的安全问题。

该漏洞的危害主要包括：
*   **继承Web服务器权限**：攻击者可以以Web服务的权限执行系统命令。
*   **获取服务器控制权**：通常用于反弹Shell，从而完全控制目标服务器。
*   **内网渗透的跳板**：在获得服务器权限后，可以此为基础，进一步探测和攻击内网其他设备。

命令执行主要分为两种类型：
1.  **远程代码执行**：攻击者能够注入并执行由服务器语言（如PHP）解释的代码。
2.  **远程系统命令执行**：攻击者能够直接调用并执行操作系统层面的命令。

![](img/e287933d7cb8536f931a5caf2ff4c3d5_7.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_8.png)

## 远程代码执行函数

![](img/e287933d7cb8536f931a5caf2ff4c3d5_10.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_12.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_14.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_16.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_18.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_20.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_22.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_24.png)

上一节我们介绍了命令执行漏洞的基本概念，本节中我们来看看在PHP中哪些函数可能导致远程代码执行。当业务需求需要调用执行命令的函数时，如果开发者未对用户可控的参数进行过滤，就可能引发此类漏洞。

![](img/e287933d7cb8536f931a5caf2ff4c3d5_26.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_27.png)

以下是几个常见的危险函数及其利用方式：

**1. eval() 函数**
`eval()` 函数会将传入的字符串参数当作PHP代码来执行。其基本形式为 `eval($code)`。
*   **注意**：传入的字符串必须是有效的PHP代码（通常以分号结尾）。
*   **示例（一句话木马）**：
    ```php
    <?php eval($_POST[‘cmd’]); ?>
    ```
    攻击者通过POST方式传递`cmd`参数（如 `system(‘whoami’);`），该代码就会被执行。

![](img/e287933d7cb8536f931a5caf2ff4c3d5_29.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_31.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_33.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_35.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_37.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_39.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_41.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_43.png)

**2. assert() 函数**
`assert()` 在PHP中主要用于调试断言，但它同样会将传入的字符串当作PHP代码执行。
*   **与eval的区别**：`assert()` 处理的字符串不需要以分号结尾。
*   **示例**：
    ```php
    <?php assert($_POST[‘cmd’]); ?>
    ```

**3. preg_replace() 函数**
`preg_replace()` 函数用于执行正则表达式的搜索和替换。当使用 `/e` 修饰符时，替换后的字符串会被 `eval()` 执行。
*   **版本限制**：此用法在 PHP 5.5.0 后被废弃。
*   **示例**：
    ```php
    <?php preg_replace(“/test/e”, $_GET[‘cmd’], “just test”); ?>
    ```
    访问 `?cmd=phpinfo()`，字符串 “test” 被匹配后，`phpinfo()` 会被当作代码执行。

![](img/e287933d7cb8536f931a5caf2ff4c3d5_45.png)

**4. array_map() 函数**
`array_map()` 为数组的每个元素应用回调函数。如果回调函数名用户可控，就可能造成代码执行。
*   **示例**：
    ```php
    <?php
    $func = $_GET[‘func’];
    $cmd = $_POST[‘cmd’];
    array_map($func, array($cmd));
    ?>
    ```
    访问 `?func=system` 并以POST方式传递 `cmd=whoami`，则会执行系统命令。

![](img/e287933d7cb8536f931a5caf2ff4c3d5_47.png)

**5. create_function() 函数**
`create_function()` 用于创建匿名函数。其参数中的代码会被执行。
*   **示例**：
    ```php
    <?php
    $func = create_function(‘’, $_POST[‘cmd’]);
    $func();
    ?>
    ```
    通过POST传递 `cmd=echo ‘test’;` 即可执行。

**6. call_user_func() 函数**
`call_user_func()` 用于调用回调函数。第一个参数为回调函数，其余参数为回调函数的参数。
*   **示例**：
    ```php
    <?php call_user_func(“assert”, $_POST[‘cmd’]); ?>
    ```

![](img/e287933d7cb8536f931a5caf2ff4c3d5_49.png)

**7. 双引号变量解析**
在PHP中，双引号包裹的字符串中的变量会被解析。如果变量值包含可执行代码，可能引发问题。
*   **示例**：
    ```php
    <?php
    $code = “phpinfo()”;
    eval(“\$result = \”$code\”;”); // $code 被解析并执行
    echo $result;
    ?>
    ```

![](img/e287933d7cb8536f931a5caf2ff4c3d5_51.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_53.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_54.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_56.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_58.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_60.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_62.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_64.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_66.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_68.png)

## 远程系统命令执行函数

![](img/e287933d7cb8536f931a5caf2ff4c3d5_70.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_72.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_74.png)

上一节我们探讨了远程执行PHP代码的函数，本节我们转向更直接的层面——执行操作系统命令的函数。这类漏洞常出现在需要调用系统功能的应用中，如网络设备的ping测试、路由追踪等。

![](img/e287933d7cb8536f931a5caf2ff4c3d5_76.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_78.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_80.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_82.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_84.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_86.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_88.png)

以下是PHP中常见的系统命令执行函数：

![](img/e287933d7cb8536f931a5caf2ff4c3d5_90.png)

**1. exec() 函数**
`exec()` 用于执行外部程序，但默认不输出结果，只返回最后一行。
*   **语法**：`exec(string $command, array &$output, int &$return_var)`
*   **示例**：
    ```php
    <?php
    $cmd = $_POST[‘cmd’];
    exec($cmd, $output);
    print_r($output);
    ?>
    ```
    传递 `cmd=ls -la`，命令结果会存储在 `$output` 数组中。

**2. system() 函数**
`system()` 执行外部命令并直接输出结果到浏览器。这是我们之前演示中最常用的函数。
*   **示例**：
    ```php
    <?php system($_GET[‘cmd’]); ?>
    ```
    访问 `?cmd=whoami`，结果直接显示在页面上。

![](img/e287933d7cb8536f931a5caf2ff4c3d5_92.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_94.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_96.png)

**3. passthru() 函数**
`passthru()` 与 `system()` 类似，直接输出结果。它特别适用于输出二进制数据，如图像。
*   **示例**：
    ```php
    <?php passthru(‘ls -la’); ?>
    ```

![](img/e287933d7cb8536f931a5caf2ff4c3d5_98.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_99.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_100.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_102.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_104.png)

**4. shell_exec() 函数**
`shell_exec()` 通过Shell环境执行命令，并以字符串形式返回全部输出。
*   **示例**：
    ```php
    <?php echo shell_exec(‘whoami’); ?>
    ```

![](img/e287933d7cb8536f931a5caf2ff4c3d5_106.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_108.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_110.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_112.png)

## 命令连接符与特殊用法

在利用命令执行漏洞时，我们常常需要拼接多个命令或绕过一些简单的过滤。以下是一些常用的命令连接符和特殊符号：

![](img/e287933d7cb8536f931a5caf2ff4c3d5_114.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_116.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_118.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_120.png)

*   **分号 `;`**：按顺序执行多个命令，无论前一个命令是否成功。
    *   `命令A ; 命令B`
*   **管道符 `|`**：将前一个命令的输出作为后一个命令的输入。
    *   `命令A | 命令B`
*   **逻辑与 `&&`**：只有前一个命令执行成功，才执行后一个命令。
    *   `命令A && 命令B`
*   **逻辑或 `||`**：只有前一个命令执行失败，才执行后一个命令。
    *   `命令A || 命令B`
*   **反引号 `` ` `` 或 `$()`**：执行括号或反引号内的命令。
    *   `echo “当前目录是：`pwd`”`
    *   `echo “当前用户是：$(whoami)”`

## 漏洞防护方法

![](img/e287933d7cb8536f931a5caf2ff4c3d5_122.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_124.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_126.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_128.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_130.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_132.png)

了解攻击方式后，防御就变得清晰。核心原则是：**不要信任任何用户输入**。
1.  **尽量避免使用危险函数**：如非必要，不要使用 `eval()`, `assert()`, `system()` 等函数。
2.  **严格过滤用户输入**：使用白名单机制，只允许符合预期格式（如纯数字、特定选项）的输入。
3.  **转义或过滤特殊字符**：对用户输入中的命令连接符（`;`, `|`, `&`, `` ` `` 等）进行转义或过滤。
4.  **使用安全的替代函数**：例如，用 `escapeshellarg()` 或 `escapeshellcmd()` 函数来处理传入系统命令的参数，它们会对参数进行转义，确保其被当作一个整体参数处理，而非命令的一部分。
    ```php
    <?php
    $user_input = $_GET[‘ip’];
    // 不安全
    system(“ping -c 4 “ . $user_input);
    // 相对安全
    system(“ping -c 4 “ . escapeshellarg($user_input));
    ?>
    ```
5.  **降低权限运行**：Web服务器进程应以最低必要权限运行，避免使用root等高权限账户。

![](img/e287933d7cb8536f931a5caf2ff4c3d5_134.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_135.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_136.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_138.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_140.png)

## 总结

![](img/e287933d7cb8536f931a5caf2ff4c3d5_142.png)

![](img/e287933d7cb8536f931a5caf2ff4c3d5_144.png)

本节课中，我们一起学习了命令执行漏洞的完整知识链。我们从漏洞的**基本定义和危害**出发，深入剖析了导致**远程代码执行**（如 `eval`, `assert`）和**远程系统命令执行**（如 `system`, `exec`）的关键PHP函数，并通过示例理解了其利用原理。接着，我们介绍了攻击中常用的**命令连接符**，最后探讨了基础的**防护思路**，核心在于对用户输入进行不可信处理和最小权限原则。

![](img/e287933d7cb8536f931a5caf2ff4c3d5_146.png)

理解这些函数和原理，不仅有助于我们在渗透测试中识别和利用此类漏洞，更重要的是能在开发过程中主动避免引入此类安全风险。下节课我们将通过实验来具体操作，加深理解。