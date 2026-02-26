#  075：Fuzz模糊测试篇&JS算法口令&隐藏参数&盲Payload&未知文件目录 (2) 🔍

![](img/3e12d43bed5d839ef458fb7361185fa4_1.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_3.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_5.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_7.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_9.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_11.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_13.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_15.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_16.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_18.png)

在本节课中，我们将要学习模糊测试（Fuzz）的核心概念及其在Web渗透测试中的具体应用。Fuzz本质上是一种基于黑盒的自动化模糊测试技术，它通过大量、批量的测试来枚举各种可能性，从而发现潜在的漏洞点、弱口令、隐藏参数和可利用的Payload。我们将从口令爆破、目录文件发现、参数枚举等多个方面，结合具体案例来深入理解这一技术。

![](img/3e12d43bed5d839ef458fb7361185fa4_20.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_22.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_24.png)

---

## 概述：什么是模糊测试（Fuzz）？

![](img/3e12d43bed5d839ef458fb7361185fa4_26.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_28.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_30.png)

模糊测试，专业上称为Fuzz Testing，等同于我们常说的“爆破”概念。它是一种通过大量、批量的测试数据，对软件或网站的安全措施进行探测的技术。其核心思想是“盲猜”，即在未知目标内部结构的情况下，通过枚举各种可能的输入（如账号、密码、目录、参数、Payload等），并根据响应差异来判断是否存在可利用的点。

![](img/3e12d43bed5d839ef458fb7361185fa4_32.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_34.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_36.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_38.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_40.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_41.png)

在Web渗透测试中，Fuzz技术主要体现在以下四个方面：
*   **弱口令爆破**
*   **漏洞点发现**
*   **隐藏参数枚举**
*   **Payload利用测试**

![](img/3e12d43bed5d839ef458fb7361185fa4_43.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_45.png)

接下来，我们将通过具体案例，逐一剖析Fuzz在这些方面的应用。

![](img/3e12d43bed5d839ef458fb7361185fa4_47.png)

---

![](img/3e12d43bed5d839ef458fb7361185fa4_49.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_51.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_53.png)

## 一、Fuzz在用户口令爆破中的应用 🔑

![](img/3e12d43bed5d839ef458fb7361185fa4_55.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_57.png)

上一节我们介绍了Fuzz的基本概念，本节中我们来看看它在用户口令爆破中的具体体现。根据密码的传输和加密方式不同，Fuzz的策略也有所区别。

以下是几种常见的口令爆破场景及应对方法：

![](img/3e12d43bed5d839ef458fb7361185fa4_59.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_61.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_62.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_64.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_66.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_68.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_70.png)

1.  **明文传输口令的爆破**
    这是最简单的情况。抓取登录数据包后，在Burp Suite的Intruder模块中，将用户名和/或密码字段设置为变量，加载相应的字典（如用户名字典、密码字典）进行爆破即可。
    *   **单变量爆破**：只对密码字段进行替换。
    *   **多变量爆破**：同时对用户名和密码字段进行替换，需使用“Cluster bomb”攻击类型。

![](img/3e12d43bed5d839ef458fb7361185fa4_72.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_74.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_76.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_78.png)

2.  **使用常见算法（如MD5、Base64）加密的口令爆破**
    当密码被MD5、Base64等常见算法加密后传输时，我们不能直接发送明文字典。需要在Burp Suite的Payload Processing中添加对应的编码模块。
    *   **操作示例**：在Intruder的Payload位置，添加规则 **`Hash -> MD5`**。这样，字典中的每个密码在发送前都会先被MD5加密，从而符合服务器的验证逻辑。

![](img/3e12d43bed5d839ef458fb7361185fa4_80.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_82.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_84.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_86.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_88.png)

3.  **使用自定义JS算法加密的口令爆破**
    当密码被网站自定义的JavaScript算法加密时（算法可能包含密钥、偏移量等），我们需要先逆向分析出加密逻辑，然后通过插件让Burp Suite在爆破时模拟该加密过程。
    *   **步骤简介**：
        *   通过浏览器开发者工具调试JS，找到加密函数。
        *   将关键的JS代码（加密函数及其依赖）提取出来。
        *   使用 **`JSRC`** 或 **`Fake`** 等Burp插件，创建一个能执行该JS加密逻辑的模块。
        *   在Intruder模块中，选择使用该自定义插件作为Payload Processing规则。
    *   **核心代码/公式概念**：这通常涉及调用一个自定义的JavaScript函数，例如 **`function encrypt(pwd) { return custom_js_algorithm(pwd); }`**，并在Burp插件中调用它。

![](img/3e12d43bed5d839ef458fb7361185fa4_90.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_92.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_94.png)

**过渡**：掌握了针对各种登录框的Fuzz爆破后，我们来看看Fuzz如何应用于发现网站上那些“看不见”的资源和入口点。

![](img/3e12d43bed5d839ef458fb7361185fa4_96.png)

---

![](img/3e12d43bed5d839ef458fb7361185fa4_98.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_100.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_102.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_104.png)

## 二、Fuzz在发现未知目录与文件中的应用 📁

![](img/3e12d43bed5d839ef458fb7361185fa4_106.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_108.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_110.png)

很多时候，一个网站的主页可能看起来空空如也，或者只展示了有限的内容。Fuzz思路在这里的体现，就是通过暴力猜测，寻找未在页面上公开引用的目录和文件，这些往往是后台入口、备份文件、配置文件等高价值目标。

![](img/3e12d43bed5d839ef458fb7361185fa4_112.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_114.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_116.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_118.png)

以下是利用Fuzz探测未知资源的典型流程：

![](img/3e12d43bed5d839ef458fb7361185fa4_120.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_122.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_124.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_126.png)

1.  **探测未知目录与文件**
    将目标URL的路径部分设置为变量，使用庞大的目录和文件名字典进行爆破。通过观察HTTP状态码（如200、403）或响应长度差异，来判断该路径是否存在。
    *   **常用字典**：`directory-list-*.txt`, `fuzzdb` 项目中的发现类字典。

![](img/3e12d43bed5d839ef458fb7361185fa4_128.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_130.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_132.png)

2.  **探测隐藏参数**
    当发现一个可访问的文件（如 `ping.php`）后，该文件可能需要参数才能触发功能或漏洞。此时，我们需要对参数名进行Fuzz。
    *   **操作**：在URL后添加 `?FUZZ=1` 之类的结构，将 `FUZZ` 设为变量，加载参数字典进行爆破。
    *   **常用字典**：`parameter-names.txt` 等。

![](img/3e12d43bed5d839ef458fb7361185fa4_134.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_136.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_138.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_140.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_142.png)

3.  **探测有效Payload（参数值）**
    在找到有效的参数名（如 `cmd`）后，下一步是Fuzz该参数的值，即测试哪些Payload可以成功利用（如命令执行、SQL注入等）。
    *   **操作**：固定参数名，将其值设为变量，加载对应的漏洞Payload字典进行爆破。
    *   **常用字典**：`command-injection.txt`, `sqli.txt`, `xss.txt` 等。

![](img/3e12d43bed5d839ef458fb7361185fa4_144.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_145.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_147.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_148.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_150.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_152.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_153.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_154.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_156.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_157.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_159.png)

**关键前提**：Fuzz成功依赖于**响应差异**。目标对“正确”输入和“错误”输入的响应（如状态码、页面长度、内容）必须有可区分的不同，否则我们无法判断猜测是否成功。

![](img/3e12d43bed5d839ef458fb7361185fa4_160.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_162.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_164.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_166.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_168.png)

**过渡**：从目录扫描到参数爆破，Fuzz展现了一种系统性的黑盒探测方法。为了加深理解，我们最后来看几个真实的安全案例，看看Fuzz思路如何在实际漏洞挖掘中发挥作用。

![](img/3e12d43bed5d839ef458fb7361185fa4_170.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_172.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_174.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_176.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_178.png)

---

![](img/3e12d43bed5d839ef458fb7361185fa4_180.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_182.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_184.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_186.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_188.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_190.png)

## 三、Fuzz实战案例分享 🎯

![](img/3e12d43bed5d839ef458fb7361185fa4_192.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_194.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_196.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_198.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_200.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_202.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_204.png)

以下是几个通过Fuzz思路发现并利用漏洞的真实案例：

![](img/3e12d43bed5d839ef458fb7361185fa4_206.png)

*   **案例一：手机号枚举与验证码爆破**
    *   **场景**：某登录系统提示“手机号未绑定”，无法触发短信验证码。
    *   **Fuzz应用**：利用手机号段字典（如1380000-1389999）对手机号字段进行Fuzz，通过响应差异（如“该用户不存在”与“验证码已发送”）找出已注册的手机号。随后对该号码进行验证码爆破，最终成功登录。

![](img/3e12d43bed5d839ef458fb7361185fa4_208.png)

*   **案例二：后台路径未授权访问**
    *   **场景**：某系统后台路径为 `/admin/index.php`。
    *   **Fuzz应用**：对 `index.php` 部分进行Fuzz，尝试替换为其他常见后台文件名（如 `view.php`, `manage.php`）。发现 `/admin/view.php` 可直接访问，存在未授权访问漏洞。

![](img/3e12d43bed5d839ef458fb7361185fa4_210.png)

*   **案例三：基于信息收集的密码组合爆破**
    *   **场景**：某学校系统提示“账号为学号，密码为特定字符+身份证后六位”。
    *   **Fuzz应用**：
        1.  通过社工或网络搜索，获取学号生成规则，生成学号字典。
        2.  密码固定部分已知，变动部分是“身份证后六位”，生成六位数字字典。
        3.  在Burp Suite中使用“Cluster bomb”模式，同时爆破学号和密码后六位，成功破解大量账号。

![](img/3e12d43bed5d839ef458fb7361185fa4_212.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_214.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_216.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_218.png)

这些案例表明，Fuzz不仅是一种技术工具，更是一种解决问题的思维模式，可以灵活应用于信息收集、漏洞探测和利用的各个环节。

![](img/3e12d43bed5d839ef458fb7361185fa4_220.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_222.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_224.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_226.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_228.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_230.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_232.png)

---

![](img/3e12d43bed5d839ef458fb7361185fa4_234.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_236.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_238.png)

## 总结 📝

本节课中我们一起学习了模糊测试（Fuzz）的核心理念与实战应用。

![](img/3e12d43bed5d839ef458fb7361185fa4_240.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_241.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_243.png)

我们首先明确了Fuzz是一种**基于黑盒的、通过大量枚举进行测试**的技术，其本质是“爆破”思维的延伸。接着，我们深入探讨了Fuzz在三个关键场景下的应用：
1.  **用户口令爆破**：针对明文、通用加密（MD5/Base64）、自定义JS加密等不同情况，采取不同的策略和工具进行Fuzz。
2.  **未知资源发现**：通过Fuzz目录、文件名、参数名和参数值，系统性地探测网站隐藏的入口点和潜在漏洞点，并强调了**响应差异**是成功的关键。
3.  **实战案例思维**：通过多个SRC漏洞案例，我们看到Fuzz思路如何与信息收集相结合，在漏洞挖掘中发挥关键作用。

![](img/3e12d43bed5d839ef458fb7361185fa4_245.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_247.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_249.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_251.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_253.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_255.png)

![](img/3e12d43bed5d839ef458fb7361185fa4_257.png)

Fuzz的成功离不开高质量的字典和对目标响应行为的准确判断。这种“盲猜”但“有据”的测试方法，是渗透测试人员在面对未知黑盒系统时的重要武器。希望你能理解并掌握这种思路，将其灵活运用到更广泛的安全测试领域中去。