# 🛡️ 课程P56：CSRF漏洞原理、利用与绕过实战

![](img/6d024bf2a8c382c9f13cbb6d870022d3_0.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_2.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_4.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_6.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_8.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_10.png)

在本节课中，我们将要学习跨站请求伪造（CSRF）漏洞。我们将从原理入手，理解其攻击条件，并通过实战案例演示无防护、有来源检测和Token检测三种场景下的利用与绕过方法。

## 📖 概述：什么是CSRF？

![](img/6d024bf2a8c382c9f13cbb6d870022d3_12.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_14.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_16.png)

CSRF，全称跨站请求伪造，是一种利用用户已登录状态，诱骗其访问恶意页面从而执行非预期操作的攻击方式。

![](img/6d024bf2a8c382c9f13cbb6d870022d3_17.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_18.png)

为了便于理解，我们来看一个简化的例子。假设一个支付数据包如下：
```
POST /transfer HTTP/1.1
Host: alipay.com
...
ticket=xxx&name=小迪&account=471656814@qq.com&money=10000
```
这个数据包代表向账户`471656814@qq.com`转账1万元。

![](img/6d024bf2a8c382c9f13cbb6d870022d3_20.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_22.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_24.png)

攻击者将这个数据包嵌入到自己的网站（例如`www.xiaodi8.com`）中。当受害者登录了支付宝，同时又访问了攻击者的网站时，浏览器就会自动向支付宝发送这个转账请求。由于受害者处于登录状态，这个请求就可能被执行，从而造成资金损失。

![](img/6d024bf2a8c382c9f13cbb6d870022d3_26.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_28.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_30.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_32.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_34.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_36.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_38.png)

这就是“跨站请求伪造”的含义：请求由`xiaodi8.com`这个外部站点发出，但伪造的是对`alipay.com`的合法操作请求。

![](img/6d024bf2a8c382c9f13cbb6d870022d3_40.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_42.png)

当然，现实中的支付系统都有严格的防护，此例仅用于说明原理。

![](img/6d024bf2a8c382c9f13cbb6d870022d3_44.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_46.png)

## 🎯 CSRF攻击的三个必要条件

![](img/6d024bf2a8c382c9f13cbb6d870022d3_48.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_50.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_52.png)

从上述原理可以推导出，一次成功的CSRF攻击需要满足以下三个条件：

![](img/6d024bf2a8c382c9f13cbb6d870022d3_54.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_56.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_58.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_60.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_62.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_64.png)

1.  **存在可伪造的请求数据包**：攻击者需要知道目标操作（如添加用户、修改信息）的具体请求格式和参数。
2.  **目标操作缺乏有效防护或防护可被绕过**：目标功能没有设置CSRF防护机制，或者其防护机制（如来源检查、Token验证）存在缺陷。
3.  **受害者会触发恶意请求**：需要通过社交工程等方式，诱使已登录的受害者访问攻击者构造的恶意页面。

![](img/6d024bf2a8c382c9f13cbb6d870022d3_66.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_68.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_70.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_72.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_74.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_76.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_78.png)

其中，**核心在于第二个条件**。如果防护坚不可摧，即使满足其他条件，攻击也无法成功。

![](img/6d024bf2a8c382c9f13cbb6d870022d3_80.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_82.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_84.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_86.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_88.png)

## 🧪 实战一：无任何防护的CSRF利用

![](img/6d024bf2a8c382c9f13cbb6d870022d3_90.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_91.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_93.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_95.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_97.png)

上一节我们介绍了CSRF的原理和攻击条件，本节中我们来看看在一个完全没有防护的系统中，如何实现CSRF攻击。

![](img/6d024bf2a8c382c9f13cbb6d870022d3_99.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_101.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_103.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_105.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_107.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_109.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_111.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_113.png)

我们以一个具有后台管理员添加功能的内容管理系统（CMS）为例。

![](img/6d024bf2a8c382c9f13cbb6d870022d3_115.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_117.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_119.png)

**攻击流程如下：**

![](img/6d024bf2a8c382c9f13cbb6d870022d3_121.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_123.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_125.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_127.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_129.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_131.png)

1.  **抓取请求**：登录后台，进行“添加管理员”操作，并用抓包工具（如Burp Suite）拦截该请求。
2.  **生成POC**：在Burp Suite中，右键点击拦截到的请求，选择 `Engagement tools` -> `Generate CSRF PoC`。在弹出的界面中，勾选 `Include auto-submit script` 以自动提交，然后复制生成的HTML代码。
3.  **部署POC**：将复制的HTML代码保存为一个文件（如`add.html`），并将其上传到攻击者可控的Web服务器上。
4.  **诱导触发**：当已登录该CMS后台的管理员访问了这个`add.html`页面时，其浏览器会自动向CMS后台发送添加管理员的请求。由于管理员处于登录状态，且该功能无任何防护，攻击成功，一个新的管理员账号被悄无声息地创建。

![](img/6d024bf2a8c382c9f13cbb6d870022d3_133.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_135.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_137.png)

这个案例清晰地展示了无防护CSRF的攻击过程：**受害者访问一个恶意页面，该页面携带了伪造的请求，利用受害者的登录凭证完成了攻击**。

![](img/6d024bf2a8c382c9f13cbb6d870022d3_139.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_141.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_143.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_145.png)

## 🛡️ 实战二：绕过来源（Referer）检测防护

![](img/6d024bf2a8c382c9f13cbb6d870022d3_147.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_149.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_151.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_153.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_155.png)

很多系统会通过检查HTTP请求头中的 `Referer` 字段来防御CSRF。该字段表明了请求来源于哪个页面。正常操作来源于本站，而CSRF攻击来源于外部站点。

![](img/6d024bf2a8c382c9f13cbb6d870022d3_157.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_159.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_161.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_163.png)

### 来源检测的原理

![](img/6d024bf2a8c382c9f13cbb6d870022d3_165.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_167.png)

服务器端代码会验证 `Referer` 值是否来源于自身域名。例如，在PHP中，可以通过 `$_SERVER[‘HTTP_REFERER’]` 获取该值并进行判断。

![](img/6d024bf2a8c382c9f13cbb6d870022d3_169.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_171.png)

### 绕过思路分类

![](img/6d024bf2a8c382c9f13cbb6d870022d3_173.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_175.png)

来源检测的绕过主要取决于其验证逻辑的严谨性，可分为“不严谨”和“严谨”两种情况。

![](img/6d024bf2a8c382c9f13cbb6d870022d3_177.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_179.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_181.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_183.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_185.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_187.png)

**以下是几种常见的测试与绕过方法：**

![](img/6d024bf2a8c382c9f13cbb6d870022d3_189.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_191.png)

1.  **空Referer绕过**：某些代码逻辑允许 `Referer` 为空时通过验证（例如，考虑到用户直接输入URL访问功能的情况）。攻击时可以在POC中添加 `<meta name=”referrer” content=”no-referrer”>` 标签，使浏览器发送请求时不携带 `Referer` 头。
2.  **校验缺陷绕过（不严谨校验）**：如果校验逻辑是“检查 `Referer` 中是否包含目标域名”，而非“是否完全等于目标域名”，则可能被绕过。例如，攻击者可以构造一个形如 `http://attacker.com/www.target.com/` 的URL，其 `Referer` 值会包含 `target.com` 字符串。但此方法受限于URL路径规则，实战中较难利用。
3.  **配合其他漏洞（严谨校验）**：如果校验逻辑非常严格，要求 `Referer` 必须完全匹配自身域名，则常规CSRF无法绕过。此时需要配合其他漏洞，如：
    *   **文件上传漏洞**：将CSRF的POC文件（如HTML文件）上传到目标服务器本身，然后诱导用户访问 `https://target.com/upload/add.html`。此时请求的 `Referer` 自然是目标域名。
    *   **XSS漏洞**：在目标站点的页面中注入JavaScript代码来发起伪造请求。由于请求是从目标站点页面本身发出的，`Referer` 校验自然通过。

![](img/6d024bf2a8c382c9f13cbb6d870022d3_193.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_195.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_197.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_199.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_201.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_203.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_205.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_207.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_209.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_211.png)

**黑盒测试建议**：在实战中，可以尝试删除或清空请求中的 `Referer` 头，观察功能是否依然正常执行。如果正常，则说明存在空 `Referer` 绕过的可能。

![](img/6d024bf2a8c382c9f13cbb6d870022d3_213.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_215.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_216.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_218.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_220.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_221.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_223.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_225.png)

## 🔑 实战三：绕过Token检测防护

![](img/6d024bf2a8c382c9f13cbb6d870022d3_227.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_229.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_231.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_233.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_235.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_237.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_239.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_241.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_243.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_245.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_246.png)

另一种更常见的防护措施是使用CSRF Token。其原理是为每个会话或每个表单生成一个不可预测的随机值（Token），服务器在处理请求时校验该Token是否有效。

![](img/6d024bf2a8c382c9f13cbb6d870022d3_248.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_250.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_251.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_253.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_255.png)

### Token防护的原理

![](img/6d024bf2a8c382c9f13cbb6d870022d3_257.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_259.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_261.png)

1.  用户访问表单页面时，服务器生成一个随机Token，并下发给用户（通常藏在表单的隐藏域中）。
2.  用户提交表单时，必须带上这个Token。
3.  服务器验证提交的Token是否与当前会话预期的Token一致。一致则处理请求，否则拒绝。

![](img/6d024bf2a8c382c9f13cbb6d870022d3_263.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_265.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_266.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_267.png)

由于攻击者无法预先得知受害者当前会话的有效Token，因此无法构造出合法的请求。

![](img/6d024bf2a8c382c9f13cbb6d870022d3_269.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_271.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_273.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_275.png)

### Token防护的潜在问题

![](img/6d024bf2a8c382c9f13cbb6d870022d3_277.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_279.png)

Token防护的有效性高度依赖于其实现。不严谨的实现可能导致防护被绕过。

![](img/6d024bf2a8c382c9f13cbb6d870022d3_281.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_283.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_285.png)

**以下是针对Token的几种测试方法：**

![](img/6d024bf2a8c382c9f13cbb6d870022d3_287.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_289.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_291.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_293.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_295.png)

1.  **Token删除测试**：在生成的CSRF POC中，直接删除Token参数，观察请求是否还能成功。如果成功，说明服务器端没有校验Token。
2.  **Token置空测试**：将Token参数的值设置为空（如 `csrftoken=`），观察请求是否成功。
3.  **Token复用测试**：使用同一个Token重复提交多次请求。如果都能成功，说明Token并非一次一用，存在被固定的风险。

![](img/6d024bf2a8c382c9f13cbb6d870022d3_297.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_299.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_301.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_303.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_305.png)

**黑盒测试建议**：在发现请求中存在 `csrftoken`、`authenticity_token` 等类似参数时，应优先使用上述方法进行测试。如果删除、置空后请求依然成功，则证明Token防护存在缺陷，CSRF攻击可能成立。

![](img/6d024bf2a8c382c9f13cbb6d870022d3_307.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_309.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_311.png)

## 📝 总结与防御建议

本节课中我们一起学习了CSRF漏洞。

![](img/6d024bf2a8c382c9f13cbb6d870022d3_313.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_315.png)

*   **原理**：利用用户的登录状态，通过外部站点伪造请求。
*   **利用**：在无防护或防护不严的情况下，可导致用户执行非本意的增删改操作。
*   **绕过**：重点分析了两种主流防护措施的绕过思路：
    *   **来源（Referer）检测**：可通过空Referer、校验逻辑缺陷或配合上传/XSS漏洞进行绕过。
    *   **Token检测**：可通过测试Token的删除、置空、复用等缺陷进行绕过。

![](img/6d024bf2a8c382c9f13cbb6d870022d3_317.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_319.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_321.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_323.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_325.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_327.png)

**对于开发者而言，一个健壮的CSRF防御方案应同时包含以下措施：**

![](img/6d024bf2a8c382c9f13cbb6d870022d3_329.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_331.png)

1.  **使用标准的Anti-CSRF Token**：确保Token足够随机、与用户会话绑定、且一次一用（Critical操作）。
2.  **校验Referer头**：作为辅助手段，严格校验请求来源是否为本站可信域名。
3.  **关键操作使用二次验证**：如修改密码、转账等，要求用户再次输入密码或验证码。
4.  **设置Cookie的SameSite属性**：为Cookie设置 `SameSite=Strict` 或 `Lax`，可以阻止第三方上下文发送Cookie，从根本上缓解CSRF攻击。

![](img/6d024bf2a8c382c9f13cbb6d870022d3_333.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_335.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_337.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_339.png)

![](img/6d024bf2a8c382c9f13cbb6d870022d3_341.png)

通过本课的学习，希望大家能够理解CSRF的危害、掌握其检测与利用方法，并在开发中有效地规避此类风险。