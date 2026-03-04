# 网络安全教程：P7：5.文件上传漏洞

## 概述
在本节课中，我们将要学习文件上传漏洞的基本概念、攻击流程、WebShell的原理与使用，以及如何绕过客户端与服务端的各种检测机制来上传恶意文件。内容将涵盖WebShell的分类、特点、攻击流程演示，以及文件上传漏洞的检测方式与绕过方法。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_1.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_2.png)

---

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_4.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_6.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_8.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_9.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_11.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_13.png)

## 什么是WebShell？🕵️

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_15.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_17.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_19.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_21.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_23.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_25.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_27.png)

WebShell是一种以ASP、PHP、JSP等网页文件形式存在的命令执行环境。这些网页文件是动态脚本文件，可以被称为网页木马后门。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_29.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_31.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_33.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_34.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_36.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_38.png)

攻击者通常通过这样的网页后门来获取网站服务器的权限，包括控制网站服务器进行文件上传下载、查看数据库以及执行命令。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_40.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_42.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_44.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_46.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_48.png)

### 木马与后门
*   **木马**：全称特洛伊木马（Trojan Horse），指潜伏在计算机中的一种非授权的远程控制程序。其作用是开放系统权限，将用户信息泄露给攻击者，是黑客常用的一种工具。它能在计算机管理员不被发觉的情况下获取系统权限。
*   **后门**：计算机服务器总共有65535个端口，每个端口是计算机与外界通信所开启的“门”。攻击者通过利用这些端口提供的服务（如80端口的Web服务、3389端口的远程桌面、22端口的SSH）来获得服务器权限，并给自己留下进入计算机的后门。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_49.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_51.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_53.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_55.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_57.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_59.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_61.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_63.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_65.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_67.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_69.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_70.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_72.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_73.png)

---

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_75.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_77.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_79.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_81.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_83.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_85.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_87.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_89.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_91.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_93.png)

## WebShell的分类 📂

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_95.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_97.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_99.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_101.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_103.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_105.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_107.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_108.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_110.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_112.png)

以下是WebShell的两种主要分类方式。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_114.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_116.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_118.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_120.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_122.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_124.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_126.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_128.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_130.png)

### 根据文件大小分类
根据文件大小，WebShell可分为以下三种：
1.  **一句话木马**：代码极其简短，通常只有一行代码。优点是使用方便。
2.  **小马**：包含文件上传功能，体积相比于大马较小。
3.  **大马**：体积较大，包含许多功能（如文件管理、数据库操作、命令执行等）。代码通常经过加密隐藏，以绕过对敏感函数的检测。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_132.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_134.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_135.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_137.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_139.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_141.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_143.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_145.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_147.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_149.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_151.png)

### 根据脚本类型分类
根据使用的脚本语言，WebShell可分为以下几种常见类型：
*   ASP
*   ASPS
*   ASPSX
*   JSP
*   PHP
目前，基于PHP语言开发的网站和CMS较为常见。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_153.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_155.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_157.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_158.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_160.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_162.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_164.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_166.png)

---

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_168.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_170.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_172.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_174.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_176.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_178.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_180.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_182.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_184.png)

## WebShell的特点 ✨

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_186.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_188.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_190.png)

WebShell具有以下四个主要特点：
1.  它以动态脚本的形式出现。
2.  它是一个木马后门，攻击者可以通过它获取服务器权限并执行命令。
3.  它可以穿越服务器防火墙。WebShell通常通过80端口（Web服务端口）进行数据传递，因此数据交换隐蔽在正常的Web流量中。
4.  它一般不会在系统日志中留下记录，只会在Web日志中留下数据传递的记录。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_192.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_194.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_196.png)

---

## WebShell攻击流程 🔄

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_198.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_200.png)

上一节我们介绍了WebShell的基本概念，本节中我们来看看如何利用WebShell进行攻击。一个典型的攻击流程如下：

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_202.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_203.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_205.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_207.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_209.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_211.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_213.png)

1.  **利用Web漏洞获取Web权限**：例如，通过SQL注入或网站CMS的漏洞，获得能够执行命令或上传文件的Web权限。
2.  **上传小马**：利用获取的权限上传一个具有文件上传功能的小马。小马的作用是为后续上传大马做准备。
3.  **通过小马上传大马**：由于大马体积大、动静明显，直接上传容易被检测。通过小马上传则更加隐蔽，能更好地绕过检测。
4.  **远程调用WebShell执行命令**：最终通过上传的大马（或一句话木马）来执行命令，获取服务器信息，完成攻击。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_215.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_217.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_218.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_220.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_222.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_224.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_225.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_227.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_229.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_230.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_231.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_232.png)

---

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_234.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_236.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_238.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_240.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_242.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_244.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_246.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_248.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_250.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_251.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_252.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_254.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_256.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_258.png)

## 常见的WebShell示例 💻

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_260.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_262.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_264.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_265.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_267.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_269.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_271.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_273.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_275.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_277.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_279.png)

下面我们来看一些常见的WebShell代码示例。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_281.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_283.png)

### PHP WebShell（一句话木马）
PHP一句话木马通常结构如下：
```php
<?php @eval($_GET[‘pass’]);?>
```
或
```php
<?php @eval($_POST[‘pass’]);?>
```
*   `<?php ... ?>`：PHP代码的开始和结束标记。
*   `eval()`：函数，将字符串作为PHP代码执行。
*   `$_GET[‘pass’]` 或 `$_POST[‘pass’]`：通过GET或POST方式接收名为 `pass` 的参数，该参数的值就是将要被执行的代码。

**攻击演示**：
假设木马文件为 `shell.php`，访问 `http://target.com/shell.php?pass=phpinfo();` 将会执行 `phpinfo()` 函数，显示服务器PHP信息。同理，传递 `pass=system(‘ipconfig’);` 可以执行系统命令。

### 其他脚本的WebShell
ASP、JSP等的一句话木马原理类似，只是语法和函数不同，例如JSP中可能使用 `request.getParameter(“pass”)` 来接收参数。

---

## WebShell的基本原理 ⚙️

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_285.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_287.png)

WebShell的工作原理可以概括为以下三点：
1.  **它是一个可执行脚本**：由PHP、ASP等动态语言编写。
2.  **数据传递**：通过HTTP请求（GET、POST、Cookie）向脚本传递要执行的命令字符串。
3.  **执行传递的数据**：脚本接收到字符串后，调用相应的函数执行。执行方式包括：
    *   **直接执行PHP函数**：如 `eval()`, `system()`。
    *   **文件包含**：通过包含恶意文件来执行其中的代码。
    *   **动态函数执行**：如 `$func = “phpinfo”; $func();`。
    *   **回调函数**：利用如 `call_user_func()` 等函数执行代码。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_289.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_291.png)

---

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_293.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_295.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_297.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_298.png)

## WebShell的传递与连接方式 🔗

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_300.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_302.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_304.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_306.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_308.png)

WebShell可以通过多种方式接收攻击者发送的命令。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_310.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_312.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_314.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_316.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_318.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_320.png)

### 1. GET方式
代码示例：`<?php @eval($_GET[‘pass’]);?>`
使用浏览器或工具直接访问URL并附加参数即可，例如：`shell.php?pass=system(‘dir’);`

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_322.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_324.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_326.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_328.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_330.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_332.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_334.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_336.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_338.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_340.png)

### 2. POST方式
代码示例：`<?php @eval($_POST[‘pass’]);?>`
需要使用Burp Suite (BP)、HackBar等工具构造POST请求，在请求体中传递 `pass=要执行的命令`。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_342.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_344.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_346.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_348.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_350.png)

### 3. Cookie方式
代码示例：`<?php @assert($_COOKIE[‘cmd’]);?>`
需要在HTTP请求头中添加Cookie字段来传递命令，例如：`Cookie: cmd=system(‘whoami’);`。这种方式通常需要借助WebShell管理工具（如菜刀、C刀）来方便地连接和管理。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_352.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_354.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_356.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_357.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_359.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_361.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_363.png)

**工具连接演示**：以C刀为例，添加Shell时填写URL，密码填写代码中定义的参数名（如 `cmd`），并在请求头设置中启用并添加对应的Cookie头及其值。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_365.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_367.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_368.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_370.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_372.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_374.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_375.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_377.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_379.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_381.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_382.png)

---

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_384.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_386.png)

## 从一句话木马到获取完整Shell 🐚

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_388.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_390.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_392.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_394.png)

在实际攻击中，攻击者往往不会直接上传功能齐全的大马，而是采用渐进的方式。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_396.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_398.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_400.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_402.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_404.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_406.png)

### 利用一句话木马写入小马
攻击者可以先上传一个一句话木马，然后通过该木马向服务器写入一个功能更多的小马文件。这通常使用文件操作函数如 `fputs()`、`fopen()` 来实现。
```php
<?php
$file = fopen(“up.php”, “w”);
fputs($file, ‘<?php @eval($_POST[“pass”]);?>’);
fclose($file);
?>
```
访问这个PHP文件，就会在服务器当前目录生成一个包含一句话木马的 `up.php` 文件。在实际利用时，会将小马的完整代码进行Base64编码后写入，以规避特殊字符过滤。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_408.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_410.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_412.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_413.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_415.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_417.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_419.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_421.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_422.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_424.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_426.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_428.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_430.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_432.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_434.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_436.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_438.png)

### 通过小马上传大马
写入小马后，访问小马页面（通常是一个文件上传表单），利用其文件上传功能将功能强大的大马上传到服务器。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_440.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_442.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_444.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_446.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_448.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_450.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_451.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_453.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_455.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_456.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_458.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_460.png)

### 大马的功能
大马通常具有Web界面，功能丰富，包括：
*   文件管理（浏览、上传、下载、编辑、删除）
*   命令执行（虚拟终端）
*   数据库管理
*   信息查看（PHP信息、系统信息）
*   端口扫描
*   等等
通过大马，攻击者可以完全控制服务器，并以此作为跳板进行内网渗透。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_462.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_464.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_466.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_468.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_470.png)

---

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_472.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_474.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_476.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_478.png)

## WebShell管理工具 🛠️

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_480.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_482.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_484.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_486.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_488.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_490.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_492.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_494.png)

为了方便地连接和管理WebShell，安全研究人员开发了多种工具。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_496.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_498.png)

以下是常见的WebShell管理工具：
1.  **中国菜刀 (Chopper)**：经典工具，但已较老，且网络下载版本可能含有后门。
2.  **C刀 (Cknife)**：Java开发，界面简洁，功能齐全，支持文件管理、虚拟终端、数据库连接等。
3.  **Weevely**：Kali Linux自带，命令行工具，用于生成和连接木马。
4.  **中国蚁剑 (AntSword)**：现代、活跃的项目，功能强大，支持插件和脚本。
5.  **冰蝎 (Behinder)**：采用加密通信流量，能有效绕过防火墙和WAF的检测，是当前渗透测试中的“神器”。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_500.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_502.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_504.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_506.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_508.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_510.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_512.png)

这些工具的使用方法大同小异：添加Shell地址、连接密码（即代码中的参数名），即可进行连接和管理。冰蝎等工具因其流量加密特性，在对抗安全设备时更具优势。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_514.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_516.png)

---

## 什么是文件上传漏洞？📤

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_518.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_519.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_521.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_523.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_525.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_527.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_529.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_531.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_533.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_535.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_537.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_538.png)

文件上传功能允许客户端将数据以文件形式封装，并通过HTTP协议发送到服务器端。服务器端解析数据后，将文件存储在硬盘上。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_540.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_542.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_544.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_546.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_547.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_549.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_550.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_552.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_553.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_554.png)

如果服务器端脚本语言没有对上传的文件进行严格的验证和过滤，就容易导致攻击者可以上传任意文件，包括WebShell，从而控制整个网站或服务器。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_556.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_558.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_560.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_562.png)

---

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_564.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_566.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_568.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_570.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_572.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_574.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_575.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_577.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_579.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_581.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_582.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_583.png)

## 文件上传漏洞的危害与常见位置 ⚠️

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_585.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_587.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_588.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_590.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_591.png)

### 危害
攻击者利用文件上传漏洞上传恶意文件（WebShell），从而在服务器上执行恶意代码，最终控制网站或服务器。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_593.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_595.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_597.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_599.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_601.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_602.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_604.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_606.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_608.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_610.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_612.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_613.png)

### 常见存在位置
文件上传漏洞通常出现在具有上传功能的功能点，例如：
*   图片上传（用户头像、帖子图片）
*   头像上传
*   文档上传（Word、Excel等）

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_615.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_617.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_619.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_620.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_622.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_624.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_625.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_627.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_628.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_630.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_631.png)

---

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_632.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_634.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_636.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_638.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_640.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_642.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_644.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_646.png)

## 文件上传的检测方式 🧐

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_647.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_648.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_650.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_652.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_653.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_654.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_656.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_658.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_660.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_662.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_664.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_666.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_668.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_669.png)

服务器和客户端会对上传的文件进行多种检测，主要分为以下几种：

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_671.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_673.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_675.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_677.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_679.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_681.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_682.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_684.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_685.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_687.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_689.png)

### 1. 客户端JavaScript检测
*   **原理**：在前端使用JavaScript代码检测文件扩展名。
*   **绕过方法**：禁用浏览器JS；或先上传一个合法文件（如图片），然后用Burp Suite抓包，修改文件名后缀为 `.php` 再转发。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_691.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_693.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_695.png)

### 2. 服务端MIME类型检测
*   **原理**：检查HTTP请求头中的 `Content-Type` 字段（如 `image/jpeg`, `application/pdf`）。
*   **绕过方法**：使用Burp Suite抓包，将 `Content-Type` 修改为服务器允许的类型（如 `image/jpeg`）。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_697.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_699.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_701.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_703.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_705.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_707.png)

### 3. 服务端文件扩展名检测
*   **原理**：检查文件后缀名（如 `.php`, `.jpg`）。分为两种策略：
    *   **黑名单**：不允许名单内的后缀。
    *   **白名单**：只允许名单内的后缀。
*   **绕过方法**：针对黑名单，可使用大小写混合（`.Php`）、末尾加空格或点（`.php.`、`.php `）、利用Windows特性（`.php::$DATA`）、解析漏洞（`.php.owf.rar`）、上传 `.htaccess` 文件等。针对白名单，可利用 `%00` 截断。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_709.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_711.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_713.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_715.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_717.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_719.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_721.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_723.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_725.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_727.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_729.png)

### 4. 服务端文件内容检测
*   **原理**：检测文件内容的合法性，常见有两种：
    *   **文件幻数 (Magic Number)检测**：检查文件头部的特定字节（如 `GIF89a`）。
    *   **文件加载/渲染检测**：调用图像处理函数对文件进行二次渲染，打乱文件中嵌入的恶意代码。
*   **绕过方法**：
    *   对于幻数检测，在文件开头添加对应的幻数字节（如图片码）。
    *   对于渲染检测，需要将代码插入到图片的注释区等不会被渲染破坏的区域，或攻击图像处理库本身（难度较高）。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_731.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_732.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_734.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_736.png)

---

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_738.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_740.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_742.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_744.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_746.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_748.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_750.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_751.png)

## Web服务器解析漏洞 🗂️

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_753.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_754.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_756.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_758.png)

某些Web服务器在解析文件时存在漏洞，可被用于文件上传攻击。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_760.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_762.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_764.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_765.png)

*   **Apache解析漏洞**：Apache从右向左解析后缀，遇到不可识别后缀则继续向左判断。例如，文件 `shell.php.owf.rar` 可能被解析为 `.php` 文件。
*   **IIS 6.0解析漏洞**：
    *   **目录解析**：`/shell.asp/` 目录下的所有文件都会被当作ASP文件解析。
    *   **文件解析**：`shell.asp;.jpg` 会被当作 `shell.asp` 解析。
*   **IIS 7.0/7.5 & Nginx解析漏洞**：在FastCGI模式下，如果PHP配置不当，访问 `shell.jpg/.php` 可能会将 `shell.jpg` 当作PHP解析。
*   **Nginx `%00` 截断解析漏洞**：在特定版本下，`shell.jpg%00.php` 会被当作PHP文件解析。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_767.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_769.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_771.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_773.png)

---

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_775.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_776.png)

## 文件上传漏洞实战思路与总结 🎯

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_778.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_780.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_782.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_784.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_786.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_788.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_790.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_792.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_794.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_796.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_798.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_800.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_801.png)

### 攻击流程总结
1.  **寻找上传点**：如图片上传、附件上传等。
2.  **判断检测类型**：通过上传正常文件和恶意文件，观察反馈，判断是前端还是后端检测，以及检测了哪些内容（后缀、MIME、内容等）。
3.  **尝试绕过**：根据判断的检测类型，运用对应的绕过方法（修改后缀、MIME、制作图片码、利用解析漏洞等）。
4.  **上传WebShell**：绕过检测后，上传一句话木马或直接上传大马。
5.  **连接WebShell**：使用管理工具连接，获取服务器权限。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_803.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_805.png)

### 防御建议
*   使用白名单验证文件扩展名和MIME类型。
*   对上传文件进行重命名（如使用随机字符串）。
*   将上传文件存储在非Web可访问目录，或禁止目录执行脚本。
*   对图像文件进行二次渲染。
*   使用WAF等安全设备进行防护。

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_807.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_809.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_810.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_812.png)

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_814.png)

---

![](img/3bb2ecc1fb42d873dc7d56a2f0771398_816.png)

## 总结
本节课我们一起学习了WebShell的原理、分类和利用方式，并深入探讨了文件上传漏洞的成因、多种检测机制及相应的绕过技巧。通过理论结合靶场实践，我们掌握了从发现上传点到最终获取服务器权限的完整攻击链条。理解这些原理和手法，对于从事渗透测试和防御加固都至关重要。