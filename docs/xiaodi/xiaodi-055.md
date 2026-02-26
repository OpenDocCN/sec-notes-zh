#  055：XSS防御与绕过实战

![](img/5e8e648caf87156c4c655b58f5248e45_1.png)

![](img/5e8e648caf87156c4c655b58f5248e45_3.png)

![](img/5e8e648caf87156c4c655b58f5248e45_5.png)

![](img/5e8e648caf87156c4c655b58f5248e45_7.png)

![](img/5e8e648caf87156c4c655b58f5248e45_9.png)

![](img/5e8e648caf87156c4c655b58f5248e45_11.png)

在本节课中，我们将学习三种主要的XSS安全防御手段：CSP（内容安全策略）、HttpOnly以及代码层面的过滤器。我们将了解它们的工作原理、如何防御XSS攻击，以及在实际场景中是否存在绕过这些防御的可能性。课程内容将结合演示和靶场练习，帮助初学者理解核心概念。

![](img/5e8e648caf87156c4c655b58f5248e45_13.png)

![](img/5e8e648caf87156c4c655b58f5248e45_14.png)

![](img/5e8e648caf87156c4c655b58f5248e45_15.png)

![](img/5e8e648caf87156c4c655b58f5248e45_17.png)

![](img/5e8e648caf87156c4c655b58f5248e45_19.png)

![](img/5e8e648caf87156c4c655b58f5248e45_21.png)

![](img/5e8e648caf87156c4c655b58f5248e45_23.png)

![](img/5e8e648caf87156c4c655b58f5248e45_25.png)

## 📋 概述：三种XSS防御手段

![](img/5e8e648caf87156c4c655b58f5248e45_27.png)

![](img/5e8e648caf87156c4c655b58f5248e45_29.png)

![](img/5e8e648caf87156c4c655b58f5248e45_31.png)

![](img/5e8e648caf87156c4c655b58f5248e45_33.png)

![](img/5e8e648caf87156c4c655b58f5248e45_35.png)

![](img/5e8e648caf87156c4c655b58f5248e45_37.png)

XSS的防御主要有三块内容。本课程主要讲解最后一块，即代码层面的过滤与绕过。前两块（CSP和HttpOnly）虽然也有绕过手法，但在实战中很难实现，因为条件苛刻，即使绕过也不一定能带来实质性的危害。因此，在实战中遇到这类防护，通常选择放弃或简单尝试突破。而代码层面的过滤和过滤性代码，则是可以尝试突破的重点。

![](img/5e8e648caf87156c4c655b58f5248e45_39.png)

![](img/5e8e648caf87156c4c655b58f5248e45_41.png)

![](img/5e8e648caf87156c4c655b58f5248e45_43.png)

![](img/5e8e648caf87156c4c655b58f5248e45_45.png)

![](img/5e8e648caf87156c4c655b58f5248e45_47.png)

![](img/5e8e648caf87156c4c655b58f5248e45_49.png)

![](img/5e8e648caf87156c4c655b58f5248e45_51.png)

## 🔒 第一部分：CSP（内容安全策略）

![](img/5e8e648caf87156c4c655b58f5248e45_53.png)

![](img/5e8e648caf87156c4c655b58f5248e45_55.png)

![](img/5e8e648caf87156c4c655b58f5248e45_57.png)

![](img/5e8e648caf87156c4c655b58f5248e45_59.png)

![](img/5e8e648caf87156c4c655b58f5248e45_60.png)

![](img/5e8e648caf87156c4c655b58f5248e45_62.png)

![](img/5e8e648caf87156c4c655b58f5248e45_64.png)

![](img/5e8e648caf87156c4c655b58f5248e45_66.png)

![](img/5e8e648caf87156c4c655b58f5248e45_68.png)

![](img/5e8e648caf87156c4c655b58f5248e45_70.png)

上一节我们概述了三种防御手段，本节中我们来看看第一种：CSP。

![](img/5e8e648caf87156c4c655b58f5248e45_72.png)

![](img/5e8e648caf87156c4c655b58f5248e45_74.png)

![](img/5e8e648caf87156c4c655b58f5248e45_76.png)

![](img/5e8e648caf87156c4c655b58f5248e45_78.png)

![](img/5e8e648caf87156c4c655b58f5248e45_79.png)

CSP全称为Content Security Policy，即内容安全策略。它并非专为防御XSS而产生，而是一种策略，用于限制页面可以加载哪些外部资源。开启CSP后，会阻止非同源的外部资源加载，这无意中也阻止了大多数XSS攻击，因为XSS攻击载荷通常需要从外部加载。

![](img/5e8e648caf87156c4c655b58f5248e45_81.png)

![](img/5e8e648caf87156c4c655b58f5248e45_83.png)

![](img/5e8e648caf87156c4c655b58f5248e45_85.png)

![](img/5e8e648caf87156c4c655b58f5248e45_87.png)

![](img/5e8e648caf87156c4c655b58f5248e45_89.png)

![](img/5e8e648caf87156c4c655b58f5248e45_91.png)

![](img/5e8e648caf87156c4c655b58f5248e45_93.png)

![](img/5e8e648caf87156c4c655b58f5248e45_95.png)

![](img/5e8e648caf87156c4c655b58f5248e45_97.png)

![](img/5e8e648caf87156c4c655b58f5248e45_99.png)

![](img/5e8e648caf87156c4c655b58f5248e45_101.png)

### CSP的核心概念与演示

![](img/5e8e648caf87156c4c655b58f5248e45_103.png)

![](img/5e8e648caf87156c4c655b58f5248e45_105.png)

![](img/5e8e648caf87156c4c655b58f5248e45_107.png)

![](img/5e8e648caf87156c4c655b58f5248e45_109.png)

![](img/5e8e648caf87156c4c655b58f5248e45_111.png)

在PHP中，可以通过设置响应头来开启CSP。以下代码演示了如何设置只允许加载本地图片：

![](img/5e8e648caf87156c4c655b58f5248e45_113.png)

![](img/5e8e648caf87156c4c655b58f5248e45_115.png)

![](img/5e8e648caf87156c4c655b58f5248e45_117.png)

![](img/5e8e648caf87156c4c655b58f5248e45_119.png)

```php
header("Content-Security-Policy: img-src 'self'");
```

![](img/5e8e648caf87156c4c655b58f5248e45_121.png)

![](img/5e8e648caf87156c4c655b58f5248e45_123.png)

**演示步骤：**
1.  未开启CSP时，页面可以正常加载远程图片。
2.  开启上述CSP策略后，远程图片加载失败，浏览器控制台会显示CSP违规错误。
3.  对于XSS攻击，如果攻击载荷（如窃取Cookie的JS代码）需要请求外部服务器，在CSP开启的情况下，该请求会被阻止，导致攻击失败。

![](img/5e8e648caf87156c4c655b58f5248e45_125.png)

![](img/5e8e648caf87156c4c655b58f5248e45_127.png)

![](img/5e8e648caf87156c4c655b58f5248e45_129.png)

**CSP的影响总结：** CSP通过限制外部资源加载，使得依赖外部JS的XSS攻击难以生效。攻击者只能使用站点内部的脚本，而这些脚本通常无法被攻击者控制。

![](img/5e8e648caf87156c4c655b58f5248e45_131.png)

![](img/5e8e648caf87156c4c655b58f5248e45_133.png)

![](img/5e8e648caf87156c4c655b58f5248e45_135.png)

![](img/5e8e648caf87156c4c655b58f5248e45_137.png)

![](img/5e8e648caf87156c4c655b58f5248e45_139.png)

### CSP的绕过可能性

![](img/5e8e648caf87156c4c655b58f5248e45_141.png)

网上存在一些绕过CSP的文章，但大多依赖于配置不完整或遗漏。例如，可能只在某些页面设置了CSP，而其他页面没有；或者策略设置不够严格，允许了某些非预期的来源。这些绕过条件在实战中很难满足。因此，在实战中遇到完整开启的CSP，通常认为该点无法利用。

![](img/5e8e648caf87156c4c655b58f5248e45_143.png)

![](img/5e8e648caf87156c4c655b58f5248e45_145.png)

![](img/5e8e648caf87156c4c655b58f5248e45_147.png)

![](img/5e8e648caf87156c4c655b58f5248e45_149.png)

![](img/5e8e648caf87156c4c655b58f5248e45_151.png)

![](img/5e8e648caf87156c4c655b58f5248e45_153.png)

## 🍪 第二部分：HttpOnly属性

![](img/5e8e648caf87156c4c655b58f5248e45_155.png)

![](img/5e8e648caf87156c4c655b58f5248e45_157.png)

![](img/5e8e648caf87156c4c655b58f5248e45_159.png)

![](img/5e8e648caf87156c4c655b58f5248e45_161.png)

![](img/5e8e648caf87156c4c655b58f5248e45_163.png)

![](img/5e8e648caf87156c4c655b58f5248e45_165.png)

![](img/5e8e648caf87156c4c655b58f5248e45_167.png)

![](img/5e8e648caf87156c4c655b58f5248e45_169.png)

上一节我们介绍了CSP，本节中我们来看看专门用于保护Cookie的HttpOnly属性。

![](img/5e8e648caf87156c4c655b58f5248e45_171.png)

![](img/5e8e648caf87156c4c655b58f5248e45_173.png)

![](img/5e8e648caf87156c4c655b58f5248e45_175.png)

![](img/5e8e648caf87156c4c655b58f5248e45_177.png)

![](img/5e8e648caf87156c4c655b58f5248e45_179.png)

![](img/5e8e648caf87156c4c655b58f5248e45_181.png)

HttpOnly是设置在Cookie上的一个标志，用于禁止JavaScript通过`document.cookie` API访问该Cookie。这是专门为增强Cookie安全性而设计的。

![](img/5e8e648caf87156c4c655b58f5248e45_183.png)

![](img/5e8e648caf87156c4c655b58f5248e45_185.png)

![](img/5e8e648caf87156c4c655b58f5248e45_187.png)

![](img/5e8e648caf87156c4c655b58f5248e45_189.png)

![](img/5e8e648caf87156c4c655b58f5248e45_191.png)

### HttpOnly的设置与效果

![](img/5e8e648caf87156c4c655b58f5248e45_193.png)

![](img/5e8e648caf87156c4c655b58f5248e45_195.png)

![](img/5e8e648caf87156c4c655b58f5248e45_197.png)

![](img/5e8e648caf87156c4c655b58f5248e45_199.png)

![](img/5e8e648caf87156c4c655b58f5248e45_201.png)

在PHP中，可以通过`setcookie`函数设置HttpOnly属性：

![](img/5e8e648caf87156c4c655b58f5248e45_203.png)

```php
setcookie("name", "xiaodi", time()+3600, "/", "", false, true); // 最后一个参数true表示启用HttpOnly
setcookie("pass", "123456", time()+3600, "/", "", false, false); // false表示不启用HttpOnly
```

**演示步骤：**
1.  在浏览器开发者工具的“应用程序”（Application）标签页中，可以查看Cookie是否启用了HttpOnly（有一个勾选标记）。
2.  对于未启用HttpOnly的Cookie，可以通过控制台执行`document.cookie`读取。
3.  对于启用了HttpOnly的Cookie，`document.cookie`无法读取其值。
4.  在XSS攻击中，如果窃取Cookie的代码尝试读取受HttpOnly保护的Cookie，将无法成功获取其内容。

![](img/5e8e648caf87156c4c655b58f5248e45_205.png)

### HttpOnly的绕过与应对思路

![](img/5e8e648caf87156c4c655b58f5248e45_207.png)

![](img/5e8e648caf87156c4c655b58f5248e45_209.png)

![](img/5e8e648caf87156c4c655b58f5248e45_211.png)

![](img/5e8e648caf87156c4c655b58f5248e45_213.png)

HttpOnly的绕过手段非常有限且条件苛刻（例如利用某些旧的PHP信息页面漏洞），在当今的互联网环境中基本不可行。**主要的应对思路是转换攻击方式**：既然无法窃取Cookie，可以尝试其他利用方式，例如：
*   使用XSS进行钓鱼攻击。
*   利用浏览器攻击框架（如BeEF）控制受害者浏览器。
*   直接执行其他恶意操作（如篡改页面内容、发起请求等）。

![](img/5e8e648caf87156c4c655b58f5248e45_215.png)

![](img/5e8e648caf87156c4c655b58f5248e45_217.png)

![](img/5e8e648caf87156c4c655b58f5248e45_219.png)

![](img/5e8e648caf87156c4c655b58f5248e45_221.png)

![](img/5e8e648caf87156c4c655b58f5248e45_223.png)

![](img/5e8e648caf87156c4c655b58f5248e45_225.png)

![](img/5e8e648caf87156c4c655b58f5248e45_227.png)

HttpOnly只防护了Cookie窃取这一条路径，并未完全封死XSS的利用。

![](img/5e8e648caf87156c4c655b58f5248e45_229.png)

![](img/5e8e648caf87156c4c655b58f5248e45_231.png)

![](img/5e8e648caf87156c4c655b58f5248e45_233.png)

![](img/5e8e648caf87156c4c655b58f5248e45_235.png)

![](img/5e8e648caf87156c4c655b58f5248e45_237.png)

![](img/5e8e648caf87156c4c655b58f5248e45_239.png)

## ⚔️ 第三部分：XSS过滤器与靶场实战

![](img/5e8e648caf87156c4c655b58f5248e45_241.png)

![](img/5e8e648caf87156c4c655b58f5248e45_243.png)

![](img/5e8e648caf87156c4c655b58f5248e45_245.png)

![](img/5e8e648caf87156c4c655b58f5248e45_247.png)

![](img/5e8e648caf87156c4c655b58f5248e45_249.png)

![](img/5e8e648caf87156c4c655b58f5248e45_251.png)

![](img/5e8e648caf87156c4c655b58f5248e45_253.png)

上一节我们介绍了服务端设置的防护，本节中我们来看看应用代码层面最常见的防御：输入过滤与净化，并通过靶场学习绕过技巧。

![](img/5e8e648caf87156c4c655b58f5248e45_255.png)

![](img/5e8e648caf87156c4c655b58f5248e45_257.png)

![](img/5e8e648caf87156c4c655b58f5248e45_259.png)

![](img/5e8e648caf87156c4c655b58f5248e45_261.png)

![](img/5e8e648caf87156c4c655b58f5248e45_263.png)

![](img/5e8e648caf87156c4c655b58f5248e45_265.png)

![](img/5e8e648caf87156c4c655b58f5248e45_267.png)

![](img/5e8e648caf87156c4c655b58f5248e45_269.png)

XSS过滤器指在服务器端代码中对用户输入进行检查、过滤或转义，例如移除`<script>`标签、转义尖括号等。

![](img/5e8e648caf87156c4c655b58f5248e45_271.png)

![](img/5e8e648caf87156c4c655b58f5248e45_273.png)

![](img/5e8e648caf87156c4c655b58f5248e45_275.png)

![](img/5e8e648caf87156c4c655b58f5248e45_277.png)

![](img/5e8e648caf87156c4c655b58f5248e45_279.png)

### 手工测试与绕过思路

![](img/5e8e648caf87156c4c655b58f5248e45_281.png)

![](img/5e8e648caf87156c4c655b58f5248e45_283.png)

![](img/5e8e648caf87156c4c655b58f5248e45_285.png)

![](img/5e8e648caf87156c4c655b58f5248e45_287.png)

![](img/5e8e648caf87156c4c655b58f5248e45_289.png)

![](img/5e8e648caf87156c4c655b58f5248e45_291.png)

手工测试XSS的核心是：**找到页面中所有用户可控的输入点，观察其输出位置，并尝试构造Payload使其中的JS代码得以执行。**

![](img/5e8e648caf87156c4c655b58f5248e45_293.png)

![](img/5e8e648caf87156c4c655b58f5248e45_295.png)

以下是关键的测试与绕过步骤：

![](img/5e8e648caf87156c4c655b58f5248e45_297.png)

![](img/5e8e648caf87156c4c655b58f5248e45_299.png)

![](img/5e8e648caf87156c4c655b58f5248e45_301.png)

![](img/5e8e648caf87156c4c655b58f5248e45_303.png)

1.  **寻找可控输入与输出点**：不仅包括URL参数、表单字段，还包括HTTP请求头（如User-Agent、Referer），以及页面中隐藏的表单字段。
2.  **尝试基础Payload**：输入如`<script>alert(1)</script>`，测试是否被直接执行。
3.  **分析过滤机制**：如果未执行，通过查看网页源代码，分析输出位置。常见情况有：
    *   **输出被HTML实体编码**：如`<`被转义为`&lt;`。这通常难以直接绕过，需寻找未编码的输出点或利用其他HTML特性（如事件处理器）。
    *   **输出被引号包裹在字符串中**：如`value="用户输入"`。需要闭合引号，然后注入HTML/JS代码。
    *   **特定关键字被过滤或删除**：如`script`、`on`等被移除或替换。
4.  **构造绕过Payload**：根据过滤情况，采用不同技巧：
    *   **闭合引号**：`"><script>alert(1)</script>`
    *   **利用HTML事件处理器**：`" onmouseover="alert(1)` （当鼠标划过时触发）
    *   **使用其他标签**：`"><img src=x onerror=alert(1)>`
    *   **大小写绕过**：`<ScRiPt>alert(1)</sCrIpT>`
    *   **双写绕过**：如果`script`被删除，尝试`<scrscriptipt>alert(1)</scrscriptipt>`
    *   **编码绕过**：在某些上下文（如URL）中使用URL编码或Unicode。

![](img/5e8e648caf87156c4c655b58f5248e45_305.png)

![](img/5e8e648caf87156c4c655b58f5248e45_307.png)

![](img/5e8e648caf87156c4c655b58f5248e45_309.png)

![](img/5e8e648caf87156c4c655b58f5248e45_311.png)

![](img/5e8e648caf87156c4c655b58f5248e45_313.png)

### XSS靶场（XSS Lab）实战示例

![](img/5e8e648caf87156c4c655b58f5248e45_315.png)

![](img/5e8e648caf87156c4c655b58f5248e45_317.png)

![](img/5e8e648caf87156c4c655b58f5248e45_319.png)

我们以几关为例，演示上述思路：

![](img/5e8e648caf87156c4c655b58f5248e45_321.png)

![](img/5e8e648caf87156c4c655b58f5248e45_323.png)

*   **第1关**：无过滤，直接注入`<script>alert(1)</script>`即可。
*   **第2关**：输入被放入`value`属性中，且尖括号被实体化。需要闭合引号：`" onmouseover="alert(1)`。
*   **第3关**：输入被放入`value`属性，且引号被实体化。使用事件处理器，且注意引号类型：`' onmouseover='alert(1)`。
*   **第5关**：过滤了`script`和`on`关键字。使用`<a>`标签和`href`属性：`"><a href="javascript:alert(1)">click</a>`。
*   **第10关**：输入被赋给了一个隐藏的输入框（`type="hidden"`）。需要找到该参数名，手动提交并利用事件处理器。

![](img/5e8e648caf87156c4c655b58f5248e45_325.png)

![](img/5e8e648caf87156c4c655b58f5248e45_327.png)

### 自动化测试工具：XSStrike

![](img/5e8e648caf87156c4c655b58f5248e45_329.png)

![](img/5e8e648caf87156c4c655b58f5248e45_331.png)

![](img/5e8e648caf87156c4c655b58f5248e45_333.png)

![](img/5e8e648caf87156c4c655b58f5248e45_335.png)

![](img/5e8e648caf87156c4c655b58f5248e45_337.png)

![](img/5e8e648caf87156c4c655b58f5248e45_339.png)

![](img/5e8e648caf87156c4c655b58f5248e45_341.png)

除了手工测试，可以使用自动化工具如XSStrike。它能自动爬取页面参数，使用大量预定义的Payload进行模糊测试，并尝试识别WAF（Web应用防火墙）和常见的过滤规则，然后生成相应的绕过Payload。

![](img/5e8e648caf87156c4c655b58f5248e45_343.png)

![](img/5e8e648caf87156c4c655b58f5248e45_345.png)

![](img/5e8e648caf87156c4c655b58f5248e45_347.png)

**基本使用命令示例：**
```bash
python xsstrike.py -u "http://target.com/page?name=test"
```
工具会测试`name`参数，并报告可能成功的Payload。

![](img/5e8e648caf87156c4c655b58f5248e45_349.png)

![](img/5e8e648caf87156c4c655b58f5248e45_351.png)

![](img/5e8e648caf87156c4c655b58f5248e45_353.png)

![](img/5e8e648caf87156c4c655b58f5248e45_355.png)

![](img/5e8e648caf87156c4c655b58f5248e45_357.png)

![](img/5e8e648caf87156c4c655b58f5248e45_359.png)

**工具定位**：XSStrike更适合用于辅助发现和验证，尤其是在面对复杂过滤时可以提供绕过思路。但最终Payload的有效性和利用方式，仍需结合手工分析。

![](img/5e8e648caf87156c4c655b58f5248e45_361.png)

![](img/5e8e648caf87156c4c655b58f5248e45_363.png)

![](img/5e8e648caf87156c4c655b58f5248e45_365.png)

![](img/5e8e648caf87156c4c655b58f5248e45_367.png)

![](img/5e8e648caf87156c4c655b58f5248e45_369.png)

![](img/5e8e648caf87156c4c655b58f5248e45_371.png)

## 📝 总结与核心要点

![](img/5e8e648caf87156c4c655b58f5248e45_373.png)

![](img/5e8e648caf87156c4c655b58f5248e45_375.png)

![](img/5e8e648caf87156c4c655b58f5248e45_377.png)

![](img/5e8e648caf87156c4c655b58f5248e45_379.png)

![](img/5e8e648caf87156c4c655b58f5248e45_381.png)

本节课我们一起学习了三种主流的XSS防御手段及其绕过思路：

![](img/5e8e648caf87156c4c655b58f5248e45_383.png)

![](img/5e8e648caf87156c4c655b58f5248e45_385.png)

![](img/5e8e648caf87156c4c655b58f5248e45_387.png)

1.  **CSP（内容安全策略）**：通过限制外部资源加载来防御XSS。配置完整时极难绕过，实战中遇此防护通常放弃。
2.  **HttpOnly属性**：保护Cookie不被JS窃取。无法直接绕过，但攻击者可转向其他利用方式（如钓鱼、浏览器控制）。
3.  **代码层过滤器**：最常见的防护。**核心在于“输入-输出”分析**：找到所有可控输入点，观察其输出上下文，根据过滤类型（实体化、关键字删除、引号包裹）采用相应绕过技巧（闭合、事件处理器、编码、大小写/双写）。

![](img/5e8e648caf87156c4c655b58f5248e45_389.png)

![](img/5e8e648caf87156c4c655b58f5248e45_391.png)

![](img/5e8e648caf87156c4c655b58f5248e45_393.png)

**最重要的思维转变**：XSS漏洞的利用成功与否，不仅取决于能否注入代码，还取决于受害者是否会触发（对于反射型/DOM型），以及是否有严密的防护措施。在实战中，需要综合评估漏洞价值、防护强度及利用条件。