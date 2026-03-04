# 护网行动红蓝攻防教程：P34：Web安全-10.命令执行 🛡️

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_1.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_3.png)

在本节课中，我们将学习命令执行漏洞的实战应用，包括对Struts2、ImageMagick以及ThinkPHP等常见组件漏洞的复现与利用。课程将重点讲解如何通过构造Payload执行系统命令、写入Webshell，并理解其背后的核心原理。

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_5.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_7.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_9.png)

---

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_11.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_13.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_15.png)

## 概述

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_17.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_19.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_21.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_22.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_24.png)

命令执行漏洞允许攻击者在目标服务器上执行任意系统命令，危害极大。本节将通过几个经典漏洞案例，演示如何发现、利用此类漏洞，并最终获取服务器控制权。

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_26.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_28.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_29.png)

---

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_31.png)

## Struts2 远程命令执行漏洞复现

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_33.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_35.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_37.png)

上一节我们介绍了命令执行的基本原理，本节中我们来看看如何在实际漏洞环境中应用。首先，我们复现一个经典的Struts2远程命令执行漏洞。

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_39.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_41.png)

### 环境部署与访问

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_43.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_45.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_47.png)

以下是部署Struts2漏洞环境的步骤：

1.  下载漏洞环境War包。
2.  将War包放置于Tomcat的 `webapps` 目录下。
3.  启动Tomcat服务，War包会自动解压。
4.  通过浏览器访问对应的应用路径，例如：`http://target:8080/your_app/`。

**注意**：访问时需注意URL路径，指导书中示例URL末尾多余的 `/` 可能导致访问失败，正确路径应为 `http://target:8080/your_app`。

### 漏洞利用与命令执行

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_49.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_51.png)

当环境部署成功后，即可利用公开的Payload进行漏洞利用。

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_53.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_55.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_56.png)

*   **核心Payload结构**：漏洞利用的核心是向目标URL发送一个包含恶意代码的HTTP请求。Payload通常是一段Java代码，其中定义了要执行的系统命令。
*   **命令执行示例**：一个典型的Payload会调用 `Runtime.getRuntime().exec(“calc”)` 来在Windows服务器上弹出计算器，证明命令执行成功。
*   **自定义命令**：我们可以修改Payload中的命令参数，例如将其改为 `ipconfig` 或 `whoami`，以执行不同的系统命令并查看结果。

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_58.png)

**代码示例：一个简单的命令执行Payload片段**
```java
Runtime.getRuntime().exec(“whoami”);
```

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_60.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_62.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_64.png)

---

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_66.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_68.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_70.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_72.png)

## ThinkPHP 远程命令执行漏洞分析

完成了Struts2的复现后，我们进一步分析一个影响广泛的PHP框架漏洞——ThinkPHP远程命令执行漏洞。

### 漏洞版本与影响

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_74.png)

该漏洞影响ThinkPHP 5.x（例如5.0和5.1）版本。攻击者可以通过构造特定的请求，在目标服务器上执行任意PHP代码和系统命令。

### Payload构造原理

我们不对漏洞的完整触发链进行深入代码分析，而是聚焦于最终执行命令的Payload构造逻辑。其本质是利用了PHP中可以执行代码的危险函数。

*   **核心函数**：`call_user_func_array()` 函数用于调用回调函数，并把参数数组作为回调函数的参数。
*   **Payload示例**：`call_user_func_array(“system”, [“whoami”])`。这相当于直接执行了 `system(“whoami”)`。
*   **写入Webshell**：利用 `file_put_contents()` 函数，我们可以将一句话木马写入服务器。例如：`file_put_contents(“shell.php”, “<?php @eval($_POST[‘cmd’]);?>”)`。

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_76.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_78.png)

**公式表示**：`call_user_func_array([“函数名”, “参数”])` 实现了动态函数调用。

### 漏洞利用实战

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_80.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_82.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_83.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_84.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_85.png)

以下是对ThinkPHP 5.0漏洞的利用过程：

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_87.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_89.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_91.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_93.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_95.png)

1.  **探测漏洞**：使用已知Payload访问特定URL，例如：`http://target/index.php?s=/index/\think\app/invokefunction&function=call_user_func_array&vars[0]=system&vars[1][]=whoami`。
2.  **执行命令**：如果页面返回了当前系统用户（如`www-data`或`apache`），则证明漏洞存在且命令执行成功。
3.  **写入Webshell**：将Payload中的命令替换为写文件操作，即可在网站根目录生成Webshell文件，从而获取持久化控制权。

**注意**：通过 `eval()` 或 `assert()` 执行PHP代码时，传入的字符串**需要以分号结尾**，因为这是PHP代码段的结束标志。而 `system()` 等执行系统命令的函数调用则不需要。

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_97.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_99.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_101.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_103.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_104.png)

---

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_106.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_108.png)

## 命令执行绕过技巧补充

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_110.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_112.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_114.png)

在实战中，经常会遇到对输入进行了过滤或转义的情况。这里补充一个上节课未详细展开的绕过技巧。

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_116.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_118.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_120.png)

### 大小写转换绕过

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_122.png)

考虑以下模拟场景：用户输入被 `preg_replace` 函数处理，并且使用了 `/e` 执行模式，但所有字母在执行前被转换为大写（通过 `strtoupper`）。

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_124.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_126.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_128.png)

*   **直接输入**：如果传入 `<?php phpinfo();?>`，它会被转换为 `<?PHP PHPINFO();?>`。PHP函数名是大小写不敏感的，但`phpinfo()`仍能执行。然而，如果代码中使用了 `system(“whoami”)`，命令本身（`whoami`）被大写后可能无法识别。
*   **利用变量与双引号绕过**：PHP中，双引号字符串会解析其中的变量，而单引号不会。我们可以利用这一点。
    *   **构造Payload**：传入 `“{${phpinfo()}}”`。双引号内的 `{${}}` 结构会被PHP解析，尝试执行 `phpinfo()` 函数，**在大小写转换发生前，代码已被执行**。
    *   **原理**：`strtoupper(“{${phpinfo()}}”)` 在处理时，PHP会先解析 `phpinfo()`，执行成功后，结果被替换到字符串中，然后整个字符串再被转换为大写。由于命令已执行，大小写转换不影响结果。

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_130.png)

这个技巧强调了理解代码执行顺序和PHP字符串解析特性的重要性。

---

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_132.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_134.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_135.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_137.png)

## 总结

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_139.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_141.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_143.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_145.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_147.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_148.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_149.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_151.png)

本节课我们一起学习了命令执行漏洞的实战应用。
1.  我们复现了**Struts2框架**的远程命令执行漏洞，掌握了从环境部署到利用Payload执行命令的全过程。
2.  我们分析了**ThinkPHP框架**的远程命令执行漏洞，理解了如何利用 `call_user_func_array` 等函数构造Payload来执行命令和写入Webshell。
3.  我们补充了**命令执行绕过**的一个技巧，即利用双引号解析变量的特性来绕过输入内容的大写转换过滤。

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_152.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_154.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_156.png)

![](img/8e8eb52a35e7a314a732f4c1f66f64e3_158.png)

命令执行是Web安全中危害极高的漏洞，深入理解其原理和利用方式，对于攻防实战至关重要。建议大家在实验环境中多加练习，并尝试阅读和分析更多公开漏洞的POC代码，以积累经验。