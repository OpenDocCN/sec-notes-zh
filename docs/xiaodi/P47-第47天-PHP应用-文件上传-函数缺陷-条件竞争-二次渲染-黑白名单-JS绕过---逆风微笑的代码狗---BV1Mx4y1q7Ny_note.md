![](img/f56eec4b2eecd255b828fea6faa23268_1.png)

# 🛡️ 文件上传安全基础教程（第47课）

在本节课中，我们将学习文件上传功能中常见的安全问题及其原理。课程将通过一个PHP靶场环境，演示多种文件上传漏洞的成因、利用方式和防御思路。请注意，虽然演示环境为PHP，但其中涉及的许多逻辑和验证缺陷在其他编程语言中也同样存在。

![](img/f56eec4b2eecd255b828fea6faa23268_3.png)

---

## 📚 课程概述与核心概念

在学习具体漏洞之前，必须理解两个核心概念，这是后续所有内容的基础。

**第一，文件解析的对应关系。**
在没有解析漏洞或配置错误的情况下，文件格式与解析方式是一一对应的。例如，上传一个包含PHP后门代码的`.jpg`图片，直接访问该图片地址是无法执行其中PHP代码的。只有当服务器存在解析漏洞或错误配置时，才可能将图片格式错误地解析为脚本格式并执行其中的代码。

**第二，文件上传攻击的本质。**
攻击者利用网站或应用的文件上传功能，尝试将恶意文件（如Webshell）上传到服务器。如果上传成功并获得访问地址，攻击者便能通过该恶意文件控制服务器。防御者则通过各种检测手段（如检查文件后缀、类型、内容等）来阻止恶意文件的上传。

然而，安全问题不仅出现在验证代码本身，还可能源于：
1.  开发语言、函数或中间件的固有缺陷。
2.  使用了存在漏洞的第三方组件或编辑器。
3.  文件上传后的不同存储逻辑（如存放到其他服务器、对象存储等），这会影响攻击的实际效果。

![](img/f56eec4b2eecd255b828fea6faa23268_5.png)

本节课将聚焦于最原始、最常见的文件上传安全架构问题。

![](img/f56eec4b2eecd255b828fea6faa23268_7.png)

![](img/f56eec4b2eecd255b828fea6faa23268_9.png)

![](img/f56eec4b2eecd255b828fea6faa23268_11.png)

![](img/f56eec4b2eecd255b828fea6faa23268_13.png)

---

![](img/f56eec4b2eecd255b828fea6faa23268_15.png)

![](img/f56eec4b2eecd255b828fea6faa23268_17.png)

## 🧪 实验环境搭建

![](img/f56eec4b2eecd255b828fea6faa23268_19.png)

![](img/f56eec4b2eecd255b828fea6faa23268_21.png)

![](img/f56eec4b2eecd255b828fea6faa23268_23.png)

![](img/f56eec4b2eecd255b828fea6faa23268_25.png)

我们将使用一个基于Docker的PHP文件上传漏洞靶场进行演示。该靶场包含了十余种不同类型的文件上传漏洞场景。

![](img/f56eec4b2eecd255b828fea6faa23268_27.png)

![](img/f56eec4b2eecd255b828fea6faa23268_29.png)

![](img/f56eec4b2eecd255b828fea6faa23268_31.png)

![](img/f56eec4b2eecd255b828fea6faa23268_33.png)

以下是搭建步骤：

![](img/f56eec4b2eecd255b828fea6faa23268_35.png)

![](img/f56eec4b2eecd255b828fea6faa23268_37.png)

![](img/f56eec4b2eecd255b828fea6faa23268_39.png)

1.  使用 `F8X` 项目自动化部署Docker环境（需要Linux系统）。
    ```bash
    ./F8X -D
    ```
2.  下载靶场源码并进入项目目录。
3.  使用Docker Compose启动靶场。
    ```bash
    docker-compose up -d
    ```
4.  启动后，靶场将在本机开放从30001到30013的端口，每个端口对应一个漏洞关卡。

![](img/f56eec4b2eecd255b828fea6faa23268_41.png)

![](img/f56eec4b2eecd255b828fea6faa23268_43.png)

![](img/f56eec4b2eecd255b828fea6faa23268_45.png)

![](img/f56eec4b2eecd255b828fea6faa23268_47.png)

在测试时，建议开启Burp Suite等抓包工具，因为许多漏洞需要通过修改HTTP请求数据包来验证。

![](img/f56eec4b2eecd255b828fea6faa23268_49.png)

![](img/f56eec4b2eecd255b828fea6faa23268_51.png)

---

![](img/f56eec4b2eecd255b828fea6faa23268_53.png)

![](img/f56eec4b2eecd255b828fea6faa23268_55.png)

![](img/f56eec4b2eecd255b828fea6faa23268_57.png)

## 🔓 漏洞类型与利用演示

![](img/f56eec4b2eecd255b828fea6faa23268_59.png)

![](img/f56eec4b2eecd255b828fea6faa23268_61.png)

![](img/f56eec4b2eecd255b828fea6faa23268_63.png)

### 1. 前端JS验证绕过 🎯

![](img/f56eec4b2eecd255b828fea6faa23268_65.png)

![](img/f56eec4b2eecd255b828fea6faa23268_67.png)

上一节我们介绍了文件上传的基本概念，本节中我们来看看最常见也是最简单的漏洞类型：前端验证绕过。

![](img/f56eec4b2eecd255b828fea6faa23268_69.png)

![](img/f56eec4b2eecd255b828fea6faa23268_71.png)

**漏洞原理：** 文件格式校验仅由客户端的JavaScript代码完成。攻击者可以禁用浏览器JS、修改前端代码或拦截HTTP请求，绕过校验。

![](img/f56eec4b2eecd255b828fea6faa23268_73.png)

![](img/f56eec4b2eecd255b828fea6faa23268_75.png)

**如何判断：**
*   查看网页源代码，寻找校验文件后缀的JavaScript函数。
*   上传非法文件时，抓包工具未捕获到任何发往服务器的请求，说明验证在请求发出前已被浏览器拦截。

![](img/f56eec4b2eecd255b828fea6faa23268_77.png)

![](img/f56eec4b2eecd255b828fea6faa23268_79.png)

**利用步骤：**
1.  先上传一个合法的图片文件（如`.jpg`），并拦截该请求。
2.  在抓包工具中，将请求中的文件名和后缀修改为恶意脚本（如`shell.php`）。
3.  将修改后的请求发送到服务器。由于服务器端没有校验或校验不严，恶意文件便上传成功。

![](img/f56eec4b2eecd255b828fea6faa23268_81.png)

![](img/f56eec4b2eecd255b828fea6faa23268_83.png)

![](img/f56eec4b2eecd255b828fea6faa23268_85.png)

**核心要点：** 前端验证仅用于提升用户体验，不能作为安全依赖，必须辅以后端校验。

![](img/f56eec4b2eecd255b828fea6faa23268_87.png)

![](img/f56eec4b2eecd255b828fea6faa23268_89.png)

![](img/f56eec4b2eecd255b828fea6faa23268_91.png)

![](img/f56eec4b2eecd255b828fea6faa23268_93.png)

---

![](img/f56eec4b2eecd255b828fea6faa23268_95.png)

### 2. Apache `.htaccess` 解析漏洞 🔧

![](img/f56eec4b2eecd255b828fea6faa23268_97.png)

![](img/f56eec4b2eecd255b828fea6faa23268_99.png)

![](img/f56eec4b2eecd255b828fea6faa23268_101.png)

![](img/f56eec4b2eecd255b828fea6faa23268_103.png)

![](img/f56eec4b2eecd255b828fea6faa23268_105.png)

前端验证绕过后，服务器如何解析文件就成了关键。本节我们来看一个由服务器配置文件导致的解析漏洞。

![](img/f56eec4b2eecd255b828fea6faa23268_107.png)

![](img/f56eec4b2eecd255b828fea6faa23268_109.png)

![](img/f56eec4b2eecd255b828fea6faa23268_111.png)

**漏洞原理：** Apache服务器的`.htaccess`文件可以强制指定特定后缀的文件使用某种解析方式。如果攻击者能上传自定义的`.htaccess`文件，就能操控服务器对文件的解析规则。

![](img/f56eec4b2eecd255b828fea6faa23268_113.png)

![](img/f56eec4b2eecd255b828fea6faa23268_115.png)

![](img/f56eec4b2eecd255b828fea6faa23268_117.png)

![](img/f56eec4b2eecd255b828fea6faa23268_119.png)

![](img/f56eec4b2eecd255b828fea6faa23268_121.png)

![](img/f56eec4b2eecd255b828fea6faa23268_123.png)

**利用步骤：**
1.  准备一个`.htaccess`文件，内容为：`AddType application/x-httpd-php .png`。这行配置表示将所有`.png`文件当作PHP脚本解析。
2.  利用其他方法（如抓包改包）将该`.htaccess`文件上传到服务器。
3.  再上传一个包含PHP代码的图片文件（如`shell.png`）。
4.  访问`shell.png`，其中的PHP代码将被执行。

![](img/f56eec4b2eecd255b828fea6faa23268_125.png)

![](img/f56eec4b2eecd255b828fea6faa23268_127.png)

![](img/f56eec4b2eecd255b828fea6faa23268_129.png)

![](img/f56eec4b2eecd255b828fea6faa23268_130.png)

**核心要点：** 此漏洞利用了中间件（Apache）的配置特性，与具体编程语言无关。关键在于攻击者能否控制配置文件。

![](img/f56eec4b2eecd255b828fea6faa23268_132.png)

![](img/f56eec4b2eecd255b828fea6faa23268_133.png)

![](img/f56eec4b2eecd255b828fea6faa23268_135.png)

![](img/f56eec4b2eecd255b828fea6faa23268_137.png)

![](img/f56eec4b2eecd255b828fea6faa23268_139.png)

---

![](img/f56eec4b2eecd255b828fea6faa23268_141.png)

![](img/f56eec4b2eecd255b828fea6faa23268_143.png)

![](img/f56eec4b2eecd255b828fea6faa23268_145.png)

### 3. Content-Type 校验绕过 📄

![](img/f56eec4b2eecd255b828fea6faa23268_147.png)

![](img/f56eec4b2eecd255b828fea6faa23268_149.png)

![](img/f56eec4b2eecd255b828fea6faa23268_151.png)

服务器端开始介入验证了。首先我们来看基于HTTP请求头中`Content-Type`的校验如何被绕过。

![](img/f56eec4b2eecd255b828fea6faa23268_153.png)

![](img/f56eec4b2eecd255b828fea6faa23268_155.png)

**漏洞原理：** 服务器通过检查HTTP请求头中的 `Content-Type` 字段（如 `image/jpeg`， `application/x-php`）来判断文件类型。此值可由客户端控制。

![](img/f56eec4b2eecd255b828fea6faa23268_157.png)

![](img/f56eec4b2eecd255b828fea6faa23268_159.png)

![](img/f56eec4b2eecd255b828fea6faa23268_161.png)

**如何判断：** 分别上传图片和PHP脚本并抓包，观察 `Content-Type` 字段的差异。

![](img/f56eec4b2eecd255b828fea6faa23268_163.png)

![](img/f56eec4b2eecd255b828fea6faa23268_165.png)

![](img/f56eec4b2eecd255b828fea6faa23268_167.png)

![](img/f56eec4b2eecd255b828fea6faa23268_169.png)

![](img/f56eec4b2eecd255b828fea6faa23268_171.png)

**利用步骤：**
1.  上传一个图片文件并拦截请求。
2.  将请求中的文件名改为`shell.php`，同时将 `Content-Type` 字段修改为图片类型（如 `image/jpeg`）。
3.  发送请求。服务器仅校验了`Content-Type`，因此恶意文件被放行。

![](img/f56eec4b2eecd255b828fea6faa23268_173.png)

![](img/f56eec4b2eecd255b828fea6faa23268_175.png)

![](img/f56eec4b2eecd255b828fea6faa23268_177.png)

![](img/f56eec4b2eecd255b828fea6faa23268_179.png)

**核心要点：** 单独依赖`Content-Type`进行验证是极不安全的，因为它完全由客户端控制。

![](img/f56eec4b2eecd255b828fea6faa23268_181.png)

![](img/f56eec4b2eecd255b828fea6faa23268_183.png)

---

![](img/f56eec4b2eecd255b828fea6faa23268_185.png)

![](img/f56eec4b2eecd255b828fea6faa23268_187.png)

![](img/f56eec4b2eecd255b828fea6faa23268_189.png)

![](img/f56eec4b2eecd255b828fea6faa23268_191.png)

![](img/f56eec4b2eecd255b828fea6faa23268_193.png)

### 4. 文件头（Magic Bytes）校验绕过 🖼️

![](img/f56eec4b2eecd255b828fea6faa23268_195.png)

![](img/f56eec4b2eecd255b828fea6faa23268_197.png)

![](img/f56eec4b2eecd255b828fea6faa23268_199.png)

![](img/f56eec4b2eecd255b828fea6faa23268_201.png)

既然`Content-Type`不可信，开发者可能会检查文件内容的实际开头字节（魔数）。本节我们看看如何伪造文件头。

![](img/f56eec4b2eecd255b828fea6faa23268_203.png)

![](img/f56eec4b2eecd255b828fea6faa23268_205.png)

![](img/f56eec4b2eecd255b828fea6faa23268_207.png)

![](img/f56eec4b2eecd255b828fea6faa23268_209.png)

![](img/f56eec4b2eecd255b828fea6faa23268_211.png)

![](img/f56eec4b2eecd255b828fea6faa23268_213.png)

**漏洞原理：** 每种文件格式在文件开头都有特定的字节序列（魔数），例如`JPEG`是`FF D8 FF`，`PNG`是`89 50 4E 47`。服务器通过检查这些字节来判断文件真实类型。

![](img/f56eec4b2eecd255b828fea6faa23268_215.png)

![](img/f56eec4b2eecd255b828fea6faa23268_217.png)

![](img/f56eec4b2eecd255b828fea6faa23268_219.png)

![](img/f56eec4b2eecd255b828fea6faa23268_220.png)

![](img/f56eec4b2eecd255b828fea6faa23268_222.png)

**利用步骤：**
1.  使用十六进制编辑器（如010 Editor）在一个PHP脚本文件的开头插入图片的魔数字节（例如`GIF89a`）。
2.  上传这个修改后的文件（后缀可为`.php`）。
3.  在抓包时，可能需要同时将`Content-Type`修改为对应的图片类型以通过多重校验。
4.  服务器检查文件头符合图片特征，便允许上传。但由于文件后缀是`.php`，它最终仍会被当作PHP脚本执行。

![](img/f56eec4b2eecd255b828fea6faa23268_224.png)

![](img/f56eec4b2eecd255b828fea6faa23268_226.png)

![](img/f56eec4b2eecd255b828fea6faa23268_228.png)

**核心要点：** 结合文件头和文件后缀进行校验会更安全，但攻击者仍可能通过精心构造文件来绕过。

![](img/f56eec4b2eecd255b828fea6faa23268_230.png)

---

![](img/f56eec4b2eecd255b828fea6faa23268_232.png)

![](img/f56eec4b2eecd255b828fea6faa23268_234.png)

### 5. 黑名单校验：双写后缀绕过 ✏️

![](img/f56eec4b2eecd255b828fea6faa23268_236.png)

![](img/f56eec4b2eecd255b828fea6faa23268_238.png)

![](img/f56eec4b2eecd255b828fea6faa23268_240.png)

从本节开始，我们进入更常见的后缀校验环节。首先看黑名单机制及其一种绕过方式。

![](img/f56eec4b2eecd255b828fea6faa23268_242.png)

![](img/f56eec4b2eecd255b828fea6faa23268_244.png)

**漏洞原理：** 黑名单列出禁止上传的后缀（如`.php`， `.asp`）。如果校验逻辑存在缺陷，例如只进行一次字符串替换且未递归处理，则可能被绕过。

![](img/f56eec4b2eecd255b828fea6faa23268_246.png)

![](img/f56eec4b2eecd255b828fea6faa23268_248.png)

![](img/f56eec4b2eecd255b828fea6faa23268_250.png)

**示例缺陷代码：**
```php
$file_name = str_ireplace($blacklist, '', $file_name);
```
这段代码将黑名单中的后缀替换为空。如果上传文件名为`shell.pHp`，替换后变为`shell.`，无效。但如果上传`shell.pphphp`，替换中间的`php`后，文件名变为`shell.php`，成功绕过。

![](img/f56eec4b2eecd255b828fea6faa23268_252.png)

![](img/f56eec4b2eecd255b828fea6faa23268_254.png)

**利用步骤：**
1.  将恶意文件命名为类似`shell.pphphp`的形式。
2.  服务器过滤掉中间的`php`字符串，最终存储的文件名变为`shell.php`。

![](img/f56eec4b2eecd255b828fea6faa23268_256.png)

![](img/f56eec4b2eecd255b828fea6faa23268_258.png)

**核心要点：** 安全过滤逻辑必须严谨，考虑递归过滤和边界情况。黑名单机制很难穷尽所有危险后缀。

![](img/f56eec4b2eecd255b828fea6faa23268_260.png)

![](img/f56eec4b2eecd255b828fea6faa23268_262.png)

---

### 6. 黑名单校验：大小写绕过 🔠

继续探讨黑名单的另一种经典绕过方式。

![](img/f56eec4b2eecd255b828fea6faa23268_264.png)

**漏洞原理：** 在Windows服务器上，文件系统不区分大小写。如果后端校验代码是大小写敏感的，且黑名单只包含了小写后缀（如`php`），那么上传大写后缀文件（如`PHP`， `Php`）即可绕过。

![](img/f56eec4b2eecd255b828fea6faa23268_266.png)

**利用步骤：**
1.  将恶意文件后缀改为大写或大小写混合，如`shell.PHP`。
2.  在Windows服务器上，该文件仍可被当作`shell.php`访问并执行。

![](img/f56eec4b2eecd255b828fea6faa23268_268.png)

![](img/f56eec4b2eecd255b828fea6faa23268_270.png)

![](img/f56eec4b2eecd255b828fea6faa23268_272.png)

**核心要点：** 校验逻辑应使用统一的大小写（如`strtolower`）进行处理。同时需注意，此绕过方式依赖于服务器操作系统。

![](img/f56eec4b2eecd255b828fea6faa23268_274.png)

![](img/f56eec4b2eecd255b828fea6faa23268_276.png)

![](img/f56eec4b2eecd255b828fea6faa23268_278.png)

---

![](img/f56eec4b2eecd255b828fea6faa23268_280.png)

![](img/f56eec4b2eecd255b828fea6faa23268_282.png)

![](img/f56eec4b2eecd255b828fea6faa23268_284.png)

### 7. 截断上传（%00截断）⛔

![](img/f56eec4b2eecd255b828fea6faa23268_286.png)

![](img/f56eec4b2eecd255b828fea6faa23268_288.png)

![](img/f56eec4b2eecd255b828fea6faa23268_290.png)

![](img/f56eec4b2eecd255b828fea6faa23268_292.png)

这是一种较老但原理重要的漏洞，存在于特定版本的PHP中。

![](img/f56eec4b2eecd255b828fea6faa23268_294.png)

![](img/f56eec4b2eecd255b828fea6faa23268_296.png)

![](img/f56eec4b2eecd255b828fea6faa23268_298.png)

![](img/f56eec4b2eecd255b828fea6faa23268_300.png)

![](img/f56eec4b2eecd255b828fea6faa23268_302.png)

**漏洞原理：** 在PHP版本 `< 5.3.4` 时，存在空字符（`%00`）截断漏洞。当`magic_quotes_gpc`为`Off`时，攻击者可以在文件名中插入`%00`，其后的内容会被截断。

![](img/f56eec4b2eecd255b828fea6faa23268_304.png)

![](img/f56eec4b2eecd255b828fea6faa23268_306.png)

**利用场景：** 当上传路径或文件名用户可控时。
*   **GET请求截断：** `upload_path=shell.php%00.jpg` （`%00`在URL中会自动解码）
*   **POST请求截断：** 需要手动将`%00`进行URL解码，在数据包中写入空字节的原始字符。

![](img/f56eec4b2eecd255b828fea6faa23268_308.png)

![](img/f56eec4b2eecd255b828fea6faa23268_310.png)

![](img/f56eec4b2eecd255b828fea6faa23268_312.png)

![](img/f56eec4b2eecd255b828fea6faa23268_314.png)

![](img/f56eec4b2eecd255b828fea6faa23268_316.png)

![](img/f56eec4b2eecd255b828fea6faa23268_318.png)

**利用步骤（POST示例）：**
1.  上传一个图片，拦截请求。
2.  将保存文件名的参数值改为`shell.php` + 一个空字节（Hex `00`） + `.jpg`，例如在Burp Suite的Hex视图中修改。
3.  服务器接收后，`%00`后的`.jpg`被截断，最终保存为`shell.php`。

**核心要点：** 这是PHP特定版本的历史漏洞，强调了依赖环境（语言、版本、配置）本身也可能引入风险。

![](img/f56eec4b2eecd255b828fea6faa23268_320.png)

![](img/f56eec4b2eecd255b828fea6faa23268_322.png)

![](img/f56eec4b2eecd255b828fea6faa23268_324.png)

---

![](img/f56eec4b2eecd255b828fea6faa23268_326.png)

![](img/f56eec4b2eecd255b828fea6faa23268_328.png)

![](img/f56eec4b2eecd255b828fea6faa23268_330.png)

![](img/f56eec4b2eecd255b828fea6faa23268_332.png)

![](img/f56eec4b2eecd255b828fea6faa23268_334.png)

### 8. 白名单校验与扩展名Fuzzing 📋

![](img/f56eec4b2eecd255b828fea6faa23268_336.png)

![](img/f56eec4b2eecd255b828fea6faa23268_338.png)

![](img/f56eec4b2eecd255b828fea6faa23268_340.png)

![](img/f56eec4b2eecd255b828fea6faa23268_342.png)

白名单（只允许指定的后缀，如`.jpg`， `.png`）通常比黑名单更安全。但若实现不完整，仍可能被绕过。

![](img/f56eec4b2eecd255b828fea6faa23268_344.png)

![](img/f56eec4b2eecd255b828fea6faa23268_346.png)

**漏洞原理：** 服务器可能未考虑到所有可执行的脚本扩展名。例如，PHP除了`.php`，还可能支持`.php5`， `.phtml`， `.phps`等。

![](img/f56eec4b2eecd255b828fea6faa23268_348.png)

**如何利用：** 使用Fuzzing（模糊测试）技术，尝试大量可能的脚本后缀。

**利用步骤：**
1.  准备一个包含各种PHP扩展名（如`.php3`， `.php4`， `.php5`， `.phtml`）的字典文件。
2.  使用Burp Suite的Intruder模块，对上传文件的后缀参数进行暴力猜解。
3.  根据HTTP响应长度或内容的不同，判断哪些扩展名被成功上传并可能被执行。

![](img/f56eec4b2eecd255b828fea6faa23268_350.png)

![](img/f56eec4b2eecd255b828fea6faa23268_352.png)

**核心要点：** 白名单需要尽可能精确和完整。同时，服务器应配置禁止执行上传目录中的任何脚本文件。

![](img/f56eec4b2eecd255b828fea6faa23268_354.png)

---

![](img/f56eec4b2eecd255b828fea6faa23268_356.png)

![](img/f56eec4b2eecd255b828fea6faa23268_358.png)

![](img/f56eec4b2eecd255b828fea6faa23268_360.png)

![](img/f56eec4b2eecd255b828fea6faa23268_362.png)

### 9. 条件竞争漏洞 ⏱️

![](img/f56eec4b2eecd255b828fea6faa23268_364.png)

![](img/f56eec4b2eecd255b828fea6faa23268_366.png)

这是一种逻辑层面的漏洞，与代码执行顺序密切相关。

![](img/f56eec4b2eecd255b828fea6faa23268_368.png)

![](img/f56eec4b2eecd255b828fea6faa23268_369.png)

**漏洞原理：** 不安全的代码逻辑可能是“先保存文件，再检查其合法性，不合法则删除”。这会在“保存”和“删除”之间留下一个极短的时间窗口。

**利用思路：** 攻击者需要在这个时间窗口内访问并触发上传的文件。如果该文件能在此瞬间创建一个新的持久化恶意文件，那么即使原文件被删除，攻击也成功了。

![](img/f56eec4b2eecd255b828fea6faa23268_371.png)

**典型利用代码（上传的恶意文件内容）：**
```php
<?php fputs(fopen(‘shell.php‘， ‘w‘)， ‘<?php @eval($_POST[pass]);?>‘);?>
```
这段代码的意思是：访问此文件，就会在同目录下生成一个包含Webshell的`shell.php`文件。

![](img/f56eec4b2eecd255b828fea6faa23268_373.png)

![](img/f56eec4b2eecd255b828fea6faa23268_375.png)

![](img/f56eec4b2eecd255b828fea6faa23268_377.png)

**利用步骤：**
1.  编写上述PHP文件并上传（由于后缀不合法，它会被删除，但会短暂存在）。
2.  利用多线程工具，疯狂地、并发地进行两个操作：
    *   线程A：不断上传该恶意文件。
    *   线程B：不断访问该恶意文件预期的URL地址。
3.  一旦线程B在文件被删除前访问成功，就会触发代码，生成永久的`shell.php`。

![](img/f56eec4b2eecd255b828fea6faa23268_379.png)

**核心要点：** 必须遵循“先校验，后保存”的安全顺序。任何相反的逻辑都可能引入条件竞争漏洞。

![](img/f56eec4b2eecd255b828fea6faa23268_381.png)

![](img/f56eec4b2eecd255b828fea6faa23268_383.png)

![](img/f56eec4b2eecd255b828fea6faa23268_385.png)

![](img/f56eec4b2eecd255b828fea6faa23268_387.png)

![](img/f56eec4b2eecd255b828fea6faa23268_389.png)

---

![](img/f56eec4b2eecd255b828fea6faa23268_391.png)

![](img/f56eec4b2eecd255b828fea6faa23268_393.png)

![](img/f56eec4b2eecd255b828fea6faa23268_395.png)

![](img/f56eec4b2eecd255b828fea6faa23268_397.png)

### 10. 二次渲染绕过 🎨

许多应用（如社交网站）会对用户上传的图片进行“二次渲染”（调整尺寸、压缩质量等），这可能导致植入的恶意代码被清除。

![](img/f56eec4b2eecd255b828fea6faa23268_399.png)

![](img/f56eec4b2eecd255b828fea6faa23268_401.png)

![](img/f56eec4b2eecd255b828fea6faa23268_403.png)

**漏洞原理：** 渲染过程会改变图片的二进制内容。如果恶意代码被插入到会被渲染修改的部分，上传后代码就会丢失。

![](img/f56eec4b2eecd255b828fea6faa23268_405.png)

![](img/f56eec4b2eecd255b828fea6faa23268_407.png)

![](img/f56eec4b2eecd255b828fea6faa23268_409.png)

**利用步骤：**
1.  正常上传一个图片，下载服务器处理后的图片。
2.  使用工具（如010 Editor的“比较文件”功能）对比原始图片和渲染后图片，找出未被修改的二进制数据块。
3.  将PHP代码写入到原始图片中这些“稳定”的数据块内。
4.  重新上传修改后的图片。由于恶意代码位于不会被渲染破坏的区域，因此得以保留。
5.  结合其他漏洞（如文件包含）来执行图片中的代码。

![](img/f56eec4b2eecd255b828fea6faa23268_411.png)

![](img/f56eec4b2eecd255b828fea6faa23268_413.png)

![](img/f56eec4b2eecd255b828fea6faa23268_415.png)

![](img/f56eec4b2eecd255b828fea6faa23268_417.png)

![](img/f56eec4b2eecd255b828fea6faa23268_419.png)

**核心要点：** 二次渲染是一种有效的安全措施，但并非绝对。攻击者可以通过分析渲染规律，找到“安全区”植入代码。

---

![](img/f56eec4b2eecd255b828fea6faa23268_421.png)

![](img/f56eec4b2eecd255b828fea6faa23268_423.png)

![](img/f56eec4b2eecd255b828fea6faa23268_425.png)

### 11. 移动函数缺陷绕过 🚚

![](img/f56eec4b2eecd255b828fea6faa23268_427.png)

![](img/f56eec4b2eecd255b828fea6faa23268_429.png)

某些文件上传函数在特定使用方式下存在缺陷，本节以PHP的`move_uploaded_file`函数为例。

![](img/f56eec4b2eecd255b828fea6faa23268_431.png)

![](img/f56eec4b2eecd255b828fea6faa23268_433.png)

![](img/f56eec4b2eecd255b828fea6faa23268_435.png)

**漏洞原理：** 当使用`move_uploaded_file($tmp_name, $destination)`函数时，如果`$destination`参数用户部分可控，且服务器PHP版本存在特性或配置问题，可能造成绕过。

![](img/f56eec4b2eecd255b828fea6faa23268_437.png)

**一个CTF常见场景：**
代码先将后缀转为小写并检查白名单（如`jpg`），然后使用`move_uploaded_file($_FILES[‘file‘][‘tmp_name‘]， “upload/” . $_POST[‘save_name‘])`保存。`save_name`用户可控。
攻击者可以提交`save_name=shell.php`（注意这里是`POST`参数，不受之前对`$_FILES[‘file‘][‘name‘]`的后缀检查影响），从而将文件保存为`.php`后缀。

![](img/f56eec4b2eecd255b828fea6faa23268_439.png)

![](img/f56eec4b2eecd255b828fea6faa23268_441.png)

![](img/f56eec4b2eecd255b828fea6faa23268_443.png)

![](img/f56eec4b2eecd255b828fea6faa23268_445.png)

**利用步骤：**
1.  上传一个图片文件。
2.  在请求中，修改另一个控制最终保存文件名的参数（如`save_name`），将其值设为`shell.php`。
3.  文件将以`.php`后缀保存。

![](img/f56eec4b2eecd255b828fea6faa23268_447.png)

**核心要点：** 必须对所有用户可控的、会影响最终存储结果的参数进行严格的校验和过滤。

---

![](img/f56eec4b2eecd255b828fea6faa23268_449.png)

## 🎓 课程总结

![](img/f56eec4b2eecd255b828fea6faa23268_451.png)

本节课我们一起学习了文件上传安全的基础知识，涵盖了十余种常见的漏洞类型：

![](img/f56eec4b2eecd255b828fea6faa23268_453.png)

1.  **前端JS验证绕过**：验证完全在客户端，可被直接绕过。
2.  **服务器解析漏洞**（如`.htaccess`）：利用中间件配置特性控制解析方式。
3.  **Content-Type校验绕过**：伪造请求头字段。
4.  **文件头校验绕过**：在文件内容开头伪造合法魔数。
5.  **黑名单双写绕过**：利用过滤逻辑不严谨，构造特殊文件名。
6.  **黑名单大小写绕过**：利用操作系统和校验逻辑对大小写处理差异。
7.  **%00截断上传**：利用PHP历史版本的空字符处理漏洞。
8.  **白名单扩展名Fuzzing**：尝试其他可执行的脚本扩展名。
9.  **条件竞争漏洞**：利用“先存后删”逻辑的时间差。
10. **二次渲染绕过**：分析渲染规律，将代码插入稳定数据区。
11. **移动函数缺陷**：利用保存函数参数用户可控的缺陷。

需要强调的是，这些漏洞中：
*   **与语言/环境相关**：解析漏洞（`.htaccess`）、特定函数缺陷（`move_uploaded_file`）、历史版本漏洞（`%00`截断）。
*   **与验证逻辑相关（通用）**：前端验证、各种校验绕过（类型、文件头、黑名单/白名单策略、条件竞争、二次渲染对抗）。这些逻辑缺陷在任何语言的开发中都需要注意。

![](img/f56eec4b2eecd255b828fea6faa23268_455.png)

![](img/f56eec4b2eecd255b828fea6faa23268_457.png)

![](img/f56eec4b2eecd255b828fea6faa23268_459.png)

![](img/f56eec4b2eecd255b828fea6faa23268_461.png)

![](img/f56eec4b2eecd255b828fea6faa23268_463.png)

文件上传漏洞在实际挖掘中，往往出现在那些**不易被发现的上传点**。随着安全架构的演进（如对象存储、严格的目录权限、WAF等），传统的攻击方式可能逐渐失效，但理解这些基础原理，是学习更高级攻防技术、适应未来安全架构变化的基石。下节课我们将探讨文件上传引发的其他安全问题及更复杂的安全架构。