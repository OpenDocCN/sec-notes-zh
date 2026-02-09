# 课程 P1：CSRF漏洞原理与组合攻击实战 🎯

![](img/7137fc91cc91db1244bc898ca9d827f9_1.png)

在本节课中，我们将学习跨站请求伪造（CSRF）漏洞的基础知识、攻击原理、利用方法，并深入探讨如何将CSRF与其他类型漏洞（如XSS、文件上传）组合使用，形成更具威胁的攻击链。课程内容从基础到进阶，旨在帮助初学者理解并掌握CSRF的核心概念。

---

## CSRF入门：原理与特点 🔍

上一节我们介绍了课程概述，本节中我们来看看CSRF的基础知识。

![](img/7137fc91cc91db1244bc898ca9d827f9_3.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_5.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_7.png)

**CSRF**，全称为跨站请求伪造。其核心特点是：攻击者诱导受害者的浏览器，向目标网站发送一个伪造的HTTP请求。这个请求会携带受害者在该目标网站的身份凭证（如Cookie或Session），从而以受害者的身份执行非预期的操作。

![](img/7137fc91cc91db1244bc898ca9d827f9_9.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_11.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_13.png)

它的产生过程可以概括为：
1.  攻击者构造一个恶意请求（Payload）。
2.  攻击者诱使受害者访问包含此Payload的页面。
3.  受害者的浏览器向目标网站发送该伪造请求。
4.  目标网站验证请求中携带的受害者凭证，并执行相应操作。

---

![](img/7137fc91cc91db1244bc898ca9d827f9_15.png)

## POC编写：手工与自动化 🛠️

![](img/7137fc91cc91db1244bc898ca9d827f9_17.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_19.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_21.png)

理解了CSRF的原理后，本节我们来看看如何构造攻击证明（POC）。

![](img/7137fc91cc91db1244bc898ca9d827f9_23.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_25.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_27.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_29.png)

POC的编写分为手工和自动化两种方式。

**手工编写POC**通常使用HTML表单或JavaScript来发起请求。
*   对于修改数据等操作，常用HTML表单。
*   对于图片上传等复杂请求，可能需要使用JavaScript。

以下是一个手工编写POST请求POC的模板：
```html
<form action="目标URL" method="POST">
    <input type="hidden" name="参数1" value="恶意值1" />
    <input type="hidden" name="参数2" value="恶意值2" />
    <input type="submit" value="提交" />
</form>
```
编写时，需要确定目标URL、请求方法（GET/POST）以及要伪造的请求参数。

**自动化编写POC**可以借助工具提高效率。例如，在Burp Suite中，对抓取到的数据包右键点击，选择 “Engagement tools” -> “Generate CSRF PoC”，即可自动生成POC代码。但自动生成的POC有时可能需要手动调整。

---

## 攻击方法：GET与POST请求利用 ⚔️

学会了编写POC，本节中我们来看看如何实际利用CSRF发起攻击。

CSRF的攻击方法取决于目标请求的类型（GET或POST）。

![](img/7137fc91cc91db1244bc898ca9d827f9_31.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_33.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_35.png)

**对于GET型请求**，利用非常简单。攻击者只需让受害者访问一个包含恶意请求的链接或页面即可。例如，通过 `<img src="恶意URL">` 标签，浏览器会自动发起GET请求。

![](img/7137fc91cc91db1244bc898ca9d827f9_37.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_38.png)

以下是部分可以发起GET请求的HTML标签：
*   `<img src="...">`
*   `<link href="...">`
*   `<script src="...">`
*   `<iframe src="...">`

![](img/7137fc91cc91db1244bc898ca9d827f9_40.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_42.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_44.png)

**对于POST型请求**，通常需要构造一个表单页面，并诱导受害者提交。为了更隐蔽和自动化，可以使用JavaScript自动提交表单，无需受害者点击。

![](img/7137fc91cc91db1244bc898ca9d827f9_46.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_48.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_50.png)

例如，在表单后添加如下JS代码：
```javascript
document.forms[0].submit();
```
这段代码能自动提交页面中的第一个表单，实现“静默”攻击。

![](img/7137fc91cc91db1244bc898ca9d827f9_52.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_54.png)

为了进一步降低被发现的概率，可以将攻击页面嵌入 `<iframe>` 标签并隐藏，这样请求完成后页面不会发生跳转。

![](img/7137fc91cc91db1244bc898ca9d827f9_56.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_58.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_60.png)

---

![](img/7137fc91cc91db1244bc898ca9d827f9_62.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_64.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_66.png)

## 漏洞进阶：CSRF蠕虫攻击 🐛

![](img/7137fc91cc91db1244bc898ca9d827f9_68.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_70.png)

掌握了基础攻击方法后，本节我们进入更高级的利用方式——CSRF蠕虫。

![](img/7137fc91cc91db1244bc898ca9d827f9_72.png)

CSRF蠕虫是指一种能够自我传播的CSRF攻击。当一名受害者触发攻击后，该攻击会利用受害者的身份继续向其他用户发起相同的CSRF攻击，从而实现“一传十，十传百”的大规模感染效果，类似于网络蠕虫病毒。

其核心思路是：将CSRF攻击Payload本身作为攻击成功后发布的新内容（如文章、评论）。当用户A访问恶意内容时，会触发一个CSRF请求，以其身份发布一条新的、包含相同攻击Payload的内容。随后，访问这条新内容的用户B又会成为新的受害者并继续传播。

构造蠕虫的关键在于，需要将生成CSRF请求的完整HTML/JS代码，作为数据提交到存在CSRF漏洞的功能点（如发帖框）中。

---

![](img/7137fc91cc91db1244bc898ca9d827f9_74.png)

## 组合拳实战：CSRF与XSS联动 🥊

![](img/7137fc91cc91db1244bc898ca9d827f9_76.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_78.png)

单一的漏洞可能危害有限，但组合利用往往能产生巨大威力。本节我们探讨CSRF与存储型XSS的组合利用。

![](img/7137fc91cc91db1244bc898ca9d827f9_80.png)

**场景**：一个网站同时存在一个“鸡肋”的存储型XSS（例如，仅在用户个人简介处，且输出在`<script>`标签内）和一个“鸡肋”的CSRF漏洞（例如，修改个人信息处无Token验证）。

![](img/7137fc91cc91db1244bc898ca9d827f9_82.png)

单独来看，这两个漏洞可能只能影响攻击者自身账户，危害很低。但组合起来：
1.  **利用CSRF伪造请求**：攻击者构造一个POC，其功能是向受害者的“个人简介”字段写入一个窃取Cookie的XSS攻击代码。
2.  **诱导受害者触发**：攻击者将CSRF的POC链接发送给其他用户。
3.  **后果**：受害者触发CSRF后，其个人资料被修改，植入了XSS代码。此后，任何查看该受害者资料的用户，其Cookie都可能被窃取。

![](img/7137fc91cc91db1244bc898ca9d827f9_84.png)

通过这种方式，两个低危漏洞被串联，实现了对其他用户的攻击，显著放大了危害。

---

![](img/7137fc91cc91db1244bc898ca9d827f9_86.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_88.png)

## 组合拳实战：CSRF与文件上传联动 📤

![](img/7137fc91cc91db1244bc898ca9d827f9_90.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_92.png)

本节我们看看CSRF如何与文件上传功能结合。

![](img/7137fc91cc91db1244bc898ca9d827f9_94.png)

直接用传统的HTML表单进行CSRF文件上传比较困难，因为浏览器对文件上传字段（`<input type="file">`）有安全限制，无法通过纯HTML表单预填文件内容。

![](img/7137fc91cc91db1244bc898ca9d827f9_96.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_98.png)

**解决方案是利用JavaScript和CORS（跨域资源共享）**。
现代浏览器允许通过JS发起带有`File`对象的跨域POST请求（需服务器配置CORS策略）。攻击者可以在恶意页面中，用JS构造一个包含文件数据的`FormData`对象，并发送到目标的上传接口。

![](img/7137fc91cc91db1244bc898ca9d827f9_100.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_102.png)

一个简化的利用思路如下：
```javascript
// 1. 在恶意页面中创建一个文件对象（内容可为恶意脚本）
var blob = new Blob(["<?php phpinfo(); ?>"], { type: "text/plain"});
var file = new File([blob], "shell.php");

![](img/7137fc91cc91db1244bc898ca9d827f9_104.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_106.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_108.png)

// 2. 构造FormData并附加文件
var formData = new FormData();
formData.append("file", file, "shell.php");
formData.append("other_param", "value");

// 3. 发起跨域请求
fetch("https://target.com/upload", {
    method: "POST",
    body: formData,
    // 注意：此请求能否成功，取决于目标服务器的CORS配置
    mode: "cors",
    credentials: "include" // 携带Cookie
});
```
当受害者访问该恶意页面时，JS会自动执行，尝试以其身份上传一个恶意文件。

![](img/7137fc91cc91db1244bc898ca9d827f9_110.png)

---

## 修复建议 🛡️

学习了多种攻击方式后，本节我们了解如何防御CSRF攻击。

以下是常见的CSRF防护措施：
1.  **验证Token**：在请求中添加一个随机的、不可预测的Token（通常存在Session中），服务端验证该Token是否有效。这是最有效的方法之一。
2.  **验证Referer/Origin头部**：检查请求是否来自合法的源（域名）。但需注意，Referer可能被篡改或为空，且需处理好站内资源的引用。
3.  **使用SameSite Cookie属性**：将Cookie设置为`SameSite=Strict`或`Lax`，可以限制第三方上下文发送Cookie，从而缓解CSRF。但需要注意对用户体验的影响。
4.  **关键操作增加二次验证**：对于修改密码、转账等敏感操作，要求用户再次输入密码或验证码。

需要注意的是，任何单一的防御措施都可能存在绕过方式（例如，Token若通过XSS泄露则防护失效），因此建议采用纵深防御策略。

---

## 课程总结 📚

![](img/7137fc91cc91db1244bc898ca9d827f9_112.png)

本节课中，我们一起学习了CSRF漏洞的完整知识体系。
我们从**基础原理**出发，理解了CSRF是跨站请求伪造。
接着学习了**POC的编写与自动化生成**，以及针对**GET和POST请求的具体攻击方法**。
然后，我们进入了进阶主题，探讨了具有传播能力的**CSRF蠕虫攻击**。
最后，我们重点研究了两种强大的组合攻击：**CSRF与存储型XSS联动**，将两个低危漏洞变为高危威胁；以及**CSRF与文件上传功能联动**，利用JS和CORS实现文件上传的伪造请求。

![](img/7137fc91cc91db1244bc898ca9d827f9_114.png)

![](img/7137fc91cc91db1244bc898ca9d827f9_116.png)

希望本课程能帮助你深刻理解CSRF的危害及其与其他漏洞组合产生的“化学反应”，并在安全测试与防御中加以应用。