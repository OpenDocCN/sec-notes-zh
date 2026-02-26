#  014：第14天 - JS架构&框架识别&泄漏提取&API接口枚举&FUZZ爬虫&插件项目 🔍

在本节课中，我们将要学习前端JavaScript（JS）架构的渗透测试方法。这对于挖掘SRC（安全应急响应中心）漏洞非常有帮助。我们将重点介绍如何识别JS框架、从JS文件中提取敏感信息、枚举API接口，以及使用FUZZ爬虫和各类插件项目进行自动化信息收集。

## 概述 📋

![](img/4f33ebdc4faf5af1d4597311e1389f68_1.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_3.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_5.png)

本节课的核心是理解基于JavaScript开发的Web应用与后端语言（如PHP、Java）开发的应用之间的关键差异，并掌握针对JS应用的特有渗透测试方法。我们将学习三种测试模式：手工分析、半自动工具辅助和全自动工具扫描。

![](img/4f33ebdc4faf5af1d4597311e1389f68_7.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_9.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_11.png)

上一节我们介绍了针对后端技术（如CMS）的识别和源码提取。本节中，我们来看看针对前端JavaScript技术开发的Web应用如何进行渗透测试。

![](img/4f33ebdc4faf5af1d4597311e1389f68_13.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_15.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_17.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_19.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_21.png)

## JS应用的核心概念与差异 🔑

![](img/4f33ebdc4faf5af1d4597311e1389f68_23.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_25.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_27.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_29.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_31.png)

首先，我们需要理解一个核心概念：**JS开发的网站，你在浏览器中看到的代码与其服务器上存储的代码基本一致**。这与后端开发语言（如PHP、Java、.NET）有本质区别。

![](img/4f33ebdc4faf5af1d4597311e1389f68_33.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_35.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_37.png)

**后端语言开发的Web应用**：
*   核心业务逻辑代码运行在服务器端。
*   浏览器无法直接看到真实的源代码，只能看到服务器处理后的输出结果（HTML、CSS、JS）。
*   上节课的CMS识别和源码泄露利用就是针对这类后端应用。

![](img/4f33ebdc4faf5af1d4597311e1389f68_39.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_41.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_43.png)

**前端JavaScript开发的Web应用**：
*   大量业务逻辑、数据验证、API调用等代码以JavaScript形式直接发送到浏览器执行。
*   我们可以在浏览器开发者工具中直接查看和分析这些JS源代码。
*   公式表示：`浏览器端可见代码 ≈ 服务器端业务逻辑代码`

![](img/4f33ebdc4faf5af1d4597311e1389f68_45.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_47.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_48.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_50.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_52.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_54.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_56.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_58.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_60.png)

由于可以直接看到源代码，我们无需像对待后端应用那样去“获取”源码。我们的目标转变为：**从这些可见的JS代码中，分析出更多有价值的信息**，例如：
*   更多的URL地址（接口、管理后台路径等）。
*   代码中的业务逻辑和验证机制。
*   可能泄露的敏感信息（如API密钥、加密算法、硬编码凭证等）。
*   数据传输和加密方式。

![](img/4f33ebdc4faf5af1d4597311e1389f68_62.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_64.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_66.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_68.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_70.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_72.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_74.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_76.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_78.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_80.png)

## 如何识别JS应用 🎯

![](img/4f33ebdc4faf5af1d4597311e1389f68_82.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_84.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_86.png)

在开始测试前，我们需要判断目标网站是否大量使用了JS框架进行开发。以下是判断方法：

![](img/4f33ebdc4faf5af1d4597311e1389f68_88.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_90.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_92.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_93.png)

1.  **使用浏览器插件识别**：安装如`Wappalyzer`这类指纹识别插件。访问目标网站时，插件会显示检测到的技术栈。如果检测到`Vue.js`、`React`、`Angular`等JS框架作为主要技术，则很可能是一个JS应用。
2.  **查看源代码**：在网站页面右键点击“查看网页源代码”或使用开发者工具（F12）。观察加载的JS文件数量和名称。典型的JS应用会加载大量以框架名（如`vue`、`react`）或业务模块（如`login`、`admin`）命名的JS文件。
3.  **对比示例**：
    *   **PHP博客**：插件可能显示主要技术为`PHP`，`JavaScript`仅作为辅助库存在。JS文件可能较少，且功能单一（如表单验证）。
    *   **Vue.js网站**：插件会明确显示`Vue.js`框架。开发者工具的“Sources”或“Network”标签页中会看到大量`.js`、`.vue`文件，其中可能包含`login.js`、`api.js`等包含明确业务逻辑的文件。

![](img/4f33ebdc4faf5af1d4597311e1389f68_95.png)

## JS信息收集的三种模式 🛠️

![](img/4f33ebdc4faf5af1d4597311e1389f68_97.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_99.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_101.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_103.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_105.png)

针对JS应用的信息收集，我们可以分为三种模式：手工分析、半自动工具辅助和全自动工具扫描。每种模式各有优劣。

![](img/4f33ebdc4faf5af1d4597311e1389f68_107.png)

### 模式一：手工分析模式

![](img/4f33ebdc4faf5af1d4597311e1389f68_109.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_111.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_113.png)

手工分析精度高，但耗时较长，需要对JS代码有一定理解。

![](img/4f33ebdc4faf5af1d4597311e1389f68_115.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_117.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_119.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_121.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_123.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_125.png)

以下是手工分析的核心步骤：

![](img/4f33ebdc4faf5af1d4597311e1389f68_127.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_128.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_130.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_132.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_134.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_136.png)

**第一步：筛选关键JS文件**
在开发者工具的“Network”面板中，过滤出所有`.js`文件。根据文件名猜测其功能，优先分析与当前测试功能相关的文件。例如，测试登录功能时，应优先查看名为`login.js`、`auth.js`的文件。

![](img/4f33ebdc4faf5af1d4597311e1389f68_138.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_140.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_142.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_144.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_146.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_148.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_150.png)

**第二步：全局搜索关键字**
在“Sources”面板中，使用`Ctrl+Shift+F`进行全局搜索。搜索的目标是发现隐藏的URL路径、接口地址和敏感信息。

![](img/4f33ebdc4faf5af1d4597311e1389f68_152.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_154.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_156.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_158.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_160.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_162.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_164.png)

以下是需要重点搜索的关键字列表：
*   **URL/路径相关**：`src=`、`href=`、`path=`、`/api/`、`/admin/`、`url:`
*   **HTTP方法相关**：`get(`、`post(`、`fetch(`、`ajax`、`.then(`
*   **敏感信息相关**：`password`、`key`、`secret`、`token`、`access`、`config`、`admin`
*   **API端点相关**：常见的RESTful API路径模式。

![](img/4f33ebdc4faf5af1d4597311e1389f68_166.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_168.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_170.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_172.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_174.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_176.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_178.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_180.png)

搜索到相关代码后，需要跟踪代码逻辑，将可能经过拼接或变量替换的路径还原成完整的URL地址，然后尝试访问，测试是否存在未授权访问等漏洞。

![](img/4f33ebdc4faf5af1d4597311e1389f68_182.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_183.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_185.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_187.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_189.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_191.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_193.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_195.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_197.png)

### 模式二：半自动工具辅助模式（以Burp Suite为例）

![](img/4f33ebdc4faf5af1d4597311e1389f68_199.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_201.png)

半自动模式利用工具插件提高效率，平衡了速度与精度。

![](img/4f33ebdc4faf5af1d4597311e1389f68_203.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_205.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_207.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_209.png)

**Burp Suite 自带功能**：
在`Target`标签页，右键点击目标站点，选择`Engagement tools` -> `Find scripts`。Burp会尝试列出所有发现的JS文件，但功能相对基础。

![](img/4f33ebdc4faf5af1d4597311e1389f68_211.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_213.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_215.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_217.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_219.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_221.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_223.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_225.png)

**推荐使用的Burp插件**：
以下是几个能极大提升JS分析效率的插件：

![](img/4f33ebdc4faf5af1d4597311e1389f68_227.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_229.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_231.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_233.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_235.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_237.png)

1.  **`JS Link Finder` & `JS Miner` (Burp官方商店)**：用于从JS文件中自动发现和提取链接（URL）。
2.  **`HaE` (第三方插件)**：基于自定义规则对数据包进行高亮标记。可以配置规则来标记JS响应中的邮箱、手机号、API密钥、路径等敏感信息，让你快速定位关键数据包。
    *   配置文件(`HaE.yml`)需要单独下载并替换，以加载丰富的预定义规则。
3.  **`Find Something` (第三方插件)**：一个功能强大的信息提取插件，能自动从JS文件中提取URL、子域名、敏感关键字（如`password`）、接口路径等，并以清晰的方式展示。

![](img/4f33ebdc4faf5af1d4597311e1389f68_239.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_241.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_243.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_245.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_247.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_249.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_251.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_253.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_254.png)

安装并配置好这些插件后，你只需正常浏览目标网站，Burp Suite就会自动捕获流量，插件则会自动分析JS响应，并将潜在的风险点高亮或提取出来，大大减少了人工搜索的工作量。

![](img/4f33ebdc4faf5af1d4597311e1389f68_256.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_258.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_260.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_262.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_264.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_266.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_268.png)

### 模式三：全自动工具扫描模式

![](img/4f33ebdc4faf5af1d4597311e1389f68_270.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_272.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_274.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_276.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_278.png)

全自动模式使用命令行工具或专用扫描器，无需人工干预即可完成JS文件的分析和信息提取。

![](img/4f33ebdc4faf5af1d4597311e1389f68_280.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_282.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_284.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_286.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_287.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_288.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_290.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_292.png)

以下是几款推荐的全自动工具：

![](img/4f33ebdc4faf5af1d4597311e1389f68_294.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_296.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_298.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_300.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_301.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_302.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_304.png)

1.  **`JSFinder`**：一款老牌的JS信息提取工具。
    *   命令示例：`python JSFinder.py -u http://example.com`
    *   它会从目标网页的JS中提取URL、子域名等信息。

![](img/4f33ebdc4faf5af1d4597311e1389f68_306.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_307.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_309.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_311.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_313.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_315.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_317.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_319.png)

2.  **`URLFinder` (JSFinder的增强版)**：功能更强大，推荐使用。它提供了Windows可执行文件，无需Python环境。
    *   命令示例：`URLFinder.exe -u http://example.com -s all`
    *   它能深度分析JS，提取URL、敏感信息（邮箱、手机号）、接口路径，并尝试访问以验证有效性，生成详细报告。

![](img/4f33ebdc4faf5af1d4597311e1389f68_321.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_323.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_325.png)

3.  **`FUFF` (Web Fuzzer)**：这不是一个JS分析器，而是一个高性能的模糊测试工具。在JS信息收集中，它用于**爆破潜在的、未在页面中显式引用的JS文件路径**。
    *   **原理**：页面加载的JS文件是有限的。通过使用庞大的JS文件名字典进行爆破，可以发现更多隐藏的JS文件，从而扩大信息收集范围。
    *   **命令示例**：`ffuf -w js_filenames.txt -u http://example.com/FUZZ -t 200`
        *   `-w`：指定字典文件（如包含`admin.js`、`config.js`等常见JS文件名的字典）。
        *   `-u`：目标URL，`FUZZ`是占位符，会被字典中的词替换。
        *   `-t`：设置线程数。
    *   将爆破得到的有效JS文件列表，再交给`URLFinder`等工具进行深度分析。

4.  **`packj` (又名 `packj-audit`)**：这是一款专门针对使用`Webpack`等打包工具的JS应用进行安全检测的工具。许多现代JS框架会使用Webpack打包代码。
    *   **命令示例**：`python3 main.py audit --url http://example.com`
    *   它会自动识别Webpack打包文件，从中提取并分析源代码，检测敏感信息泄露、依赖漏洞等问题。

![](img/4f33ebdc4faf5af1d4597311e1389f68_327.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_329.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_331.png)

## 实战思路与总结 🎓

![](img/4f33ebdc4faf5af1d4597311e1389f68_333.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_335.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_337.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_339.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_341.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_343.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_345.png)

本节课我们一起学习了针对前端JavaScript应用的渗透测试信息收集方法。我们来总结一下核心思路和流程：

![](img/4f33ebdc4faf5af1d4597311e1389f68_347.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_349.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_351.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_353.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_355.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_357.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_359.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_361.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_362.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_364.png)

1.  **识别**：首先使用插件或观察法，判断目标是否为JS密集型应用。
2.  **提取已知信息**：利用手工搜索或`URLFinder`、`JSFinder`、Burp插件等工具，从当前页面加载的JS文件中提取URL、接口、敏感数据。
3.  **扩大攻击面**：使用`FUFF`等模糊测试工具，以字典爆破的方式寻找未直接引用的隐藏JS文件。
4.  **深度分析**：将新发现的JS文件再次送入提取工具进行分析，形成循环，不断发现新信息。
5.  **专项检测**：对于使用Webpack打包的应用，使用`packj`进行专项审计。
6.  **验证与利用**：对所有提取出的URL路径、API接口进行访问测试，寻找未授权访问、信息泄露等漏洞；分析提取出的敏感信息（如密钥、逻辑）的利用可能性。

![](img/4f33ebdc4faf5af1d4597311e1389f68_366.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_368.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_370.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_372.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_374.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_375.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_377.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_379.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_381.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_383.png)

**为什么要进行JS信息收集（打点）？**
*   **发现更多入口点**：找到后台路径、API接口，测试未授权访问。
*   **源码泄露分析**：直接分析业务逻辑、加密算法、验证机制，为绕过防护提供思路。
*   **敏感信息泄露**：在JS代码中可能直接找到API密钥、数据库配置、硬编码账号密码等。
*   **理解应用架构**：清晰了解数据流和功能模块，指导后续更精准的渗透测试。

![](img/4f33ebdc4faf5af1d4597311e1389f68_385.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_387.png)

![](img/4f33ebdc4faf5af1d4597311e1389f68_389.png)

通过本节课的学习，你应该掌握了从前端JS角度对Web应用进行信息收集的完整方法论和工具链。这将使你在面对日益流行的前后端分离架构应用时，具备更强的渗透测试能力。