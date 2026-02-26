#  019：第19天 - 小程序应用信息收集与逆向分析 🕵️

![](img/ec583ecafaea86f54c189669ec5b025a_0.png)

![](img/ec583ecafaea86f54c189669ec5b025a_2.png)

![](img/ec583ecafaea86f54c189669ec5b025a_4.png)

在本节课中，我们将学习如何针对小程序进行信息收集。主要内容包括：如何寻找目标小程序、通过抓包和动态调试获取信息、以及通过解包反编译获取源码并进行静态分析。我们将从外在表现和内部源码两个层面，全面了解小程序的资产信息收集方法。

![](img/ec583ecafaea86f54c189669ec5b025a_6.png)

![](img/ec583ecafaea86f54c189669ec5b025a_8.png)

![](img/ec583ecafaea86f54c189669ec5b025a_10.png)

![](img/ec583ecafaea86f54c189669ec5b025a_12.png)

![](img/ec583ecafaea86f54c189669ec5b025a_14.png)

## 小程序概述与寻找方法

![](img/ec583ecafaea86f54c189669ec5b025a_16.png)

![](img/ec583ecafaea86f54c189669ec5b025a_18.png)

![](img/ec583ecafaea86f54c189669ec5b025a_20.png)

![](img/ec583ecafaea86f54c189669ec5b025a_22.png)

上一节我们介绍了课程的整体目标，本节中我们来看看小程序的基本概念以及如何寻找目标。

![](img/ec583ecafaea86f54c189669ec5b025a_24.png)

![](img/ec583ecafaea86f54c189669ec5b025a_26.png)

![](img/ec583ecafaea86f54c189669ec5b025a_28.png)

![](img/ec583ecafaea86f54c189669ec5b025a_30.png)

![](img/ec583ecafaea86f54c189669ec5b025a_32.png)

小程序是一种无需下载安装即可使用的应用，在国内主要活跃于微信、支付宝、抖音/头条、百度等平台。其中，微信小程序最为常见。

![](img/ec583ecafaea86f54c189669ec5b025a_34.png)

![](img/ec583ecafaea86f54c189669ec5b025a_36.png)

![](img/ec583ecafaea86f54c189669ec5b025a_38.png)

寻找目标小程序的方法非常简单，主要依靠搜索。

![](img/ec583ecafaea86f54c189669ec5b025a_40.png)

以下是具体操作步骤：
1.  打开微信，点击“发现”页面的“小程序”。
2.  在搜索框中输入目标的关键字，如公司名、网站标题、备案信息等。
3.  搜索结果会展示相关的小程序，点击即可进入。

![](img/ec583ecafaea86f54c189669ec5b025a_42.png)

支付宝、抖音等平台的操作方法类似，均通过平台内搜索功能完成。

![](img/ec583ecafaea86f54c189669ec5b025a_43.png)

![](img/ec583ecafaea86f54c189669ec5b025a_45.png)

![](img/ec583ecafaea86f54c189669ec5b025a_47.png)

![](img/ec583ecafaea86f54c189669ec5b025a_49.png)

![](img/ec583ecafaea86f54c189669ec5b025a_51.png)

![](img/ec583ecafaea86f54c189669ec5b025a_53.png)

![](img/ec583ecafaea86f54c189669ec5b025a_55.png)

![](img/ec583ecafaea86f54c189669ec5b025a_57.png)

![](img/ec583ecafaea86f54c189669ec5b025a_59.png)

## 小程序抓包分析 🔍

![](img/ec583ecafaea86f54c189669ec5b025a_61.png)

![](img/ec583ecafaea86f54c189669ec5b025a_63.png)

在了解了如何找到小程序后，本节我们来看看如何通过抓包技术从外在表现收集信息。这与之前Web和APP的抓包思路一致。

![](img/ec583ecafaea86f54c189669ec5b025a_65.png)

![](img/ec583ecafaea86f54c189669ec5b025a_67.png)

![](img/ec583ecafaea86f54c189669ec5b025a_69.png)

![](img/ec583ecafaea86f54c189669ec5b025a_71.png)

![](img/ec583ecafaea86f54c189669ec5b025a_72.png)

![](img/ec583ecafaea86f54c189669ec5b025a_74.png)

抓包目的是获取小程序通信的IP、域名、接口等信息，为后续的漏洞测试（如Web漏洞、接口测试、端口扫描）做准备。

![](img/ec583ecafaea86f54c189669ec5b025a_76.png)

![](img/ec583ecafaea86f54c189669ec5b025a_78.png)

![](img/ec583ecafaea86f54c189669ec5b025a_80.png)

![](img/ec583ecafaea86f54c189669ec5b025a_82.png)

我们使用 **Burp Suite** 配合 **Proxifier** 工具进行联动抓包。确保Burp Suite的CA证书已正确安装到系统的“受信任的根证书颁发机构”和“中间证书颁发机构”存储中。

![](img/ec583ecafaea86f54c189669ec5b025a_84.png)

![](img/ec583ecafaea86f54c189669ec5b025a_86.png)

![](img/ec583ecafaea86f54c189669ec5b025a_88.png)

![](img/ec583ecafaea86f54c189669ec5b025a_90.png)

![](img/ec583ecafaea86f54c189669ec5b025a_92.png)

![](img/ec583ecafaea86f54c189669ec5b025a_94.png)

以下是抓包配置流程：
1.  启动Burp Suite，监听本地的代理端口（如 `127.0.0.1:8080`）。
2.  打开Proxifier，配置代理服务器指向Burp Suite的监听地址。
3.  在Proxifier的“代理规则”中，为微信小程序的专用进程 `WeChatAppEx.exe` 启用代理，使其流量经过Burp Suite。

![](img/ec583ecafaea86f54c189669ec5b025a_96.png)

![](img/ec583ecafaea86f54c189669ec5b025a_98.png)

![](img/ec583ecafaea86f54c189669ec5b025a_100.png)

配置完成后，在微信中操作目标小程序，所有网络请求将在Burp Suite中捕获。通过分析这些请求，可以提取出相关的域名和IP地址资产。

![](img/ec583ecafaea86f54c189669ec5b025a_102.png)

![](img/ec583ecafaea86f54c189669ec5b025a_104.png)

## 小程序逆向与源码获取 ⚙️

![](img/ec583ecafaea86f54c189669ec5b025a_106.png)

![](img/ec583ecafaea86f54c189669ec5b025a_108.png)

![](img/ec583ecafaea86f54c189669ec5b025a_110.png)

通过抓包我们获得了外在的通信信息，本节我们将深入小程序内部，通过逆向工程获取其源代码，这能帮助我们发现更多隐藏资产和配置信息。

![](img/ec583ecafaea86f54c189669ec5b025a_112.png)

![](img/ec583ecafaea86f54c189669ec5b025a_114.png)

![](img/ec583ecafaea86f54c189669ec5b025a_116.png)

小程序逆向主要包括解包和反编译两个步骤，以得到可读的源代码。获取源码后，主要从四个方面进行分析：
1.  **发现更多资产信息**：找到抓包未覆盖的网站或IP。
2.  **查找敏感配置信息**：如API密钥、访问密钥等。
3.  **测试未授权访问**：直接访问需要权限的接口或页面。
4.  **源码安全审计**：分析代码逻辑是否存在安全漏洞。

![](img/ec583ecafaea86f54c189669ec5b025a_118.png)

![](img/ec583ecafaea86f54c189669ec5b025a_120.png)

![](img/ec583ecafaea86f54c189669ec5b025a_122.png)

![](img/ec583ecafaea86f54c189669ec5b025a_124.png)

![](img/ec583ecafaea86f54c189669ec5b025a_126.png)

![](img/ec583ecafaea86f54c189669ec5b025a_128.png)

网上传统的免费逆向方案需要Root的安卓手机、解密和反编译工具，且步骤繁琐，新版本小程序可能不兼容。本教程采用一款集成的图形化工具“小程序多功能助手”（需付费，但操作简便）。

![](img/ec583ecafaea86f54c189669ec5b025a_130.png)

![](img/ec583ecafaea86f54c189669ec5b025a_132.png)

![](img/ec583ecafaea86f54c189669ec5b025a_134.png)

![](img/ec583ecafaea86f54c189669ec5b025a_136.png)

![](img/ec583ecafaea86f54c189669ec5b025a_138.png)

![](img/ec583ecafaea86f54c189669ec5b025a_140.png)

以下是使用该工具获取源码的步骤：
1.  **定位小程序包**：打开电脑版微信，进入 `设置 -> 文件管理 -> 打开文件夹`。在打开的目录中，进入 `WeChat Files\Applet` 文件夹。运行目标小程序后，会生成一个以 `wx` 开头的文件夹，其中的 `.wxapkg` 文件即是小程序包。
2.  **解包**：在“小程序多功能助手”中选择“解包文件”，定位并选择上一步找到的 `.wxapkg` 文件（可能有多个分包，全选即可）。
3.  **反编译**：解包成功后，点击“刷新反编译包”，选中生成的包文件，根据小程序新旧程度选择“新版反编译”或“旧版反编译”。
4.  **修复源码（可选）**：部分源码反编译后可能需要使用工具的“修复源码”功能进行修复。
5.  **导入开发者工具**：使用微信官方“微信开发者工具”，导入反编译得到的源码目录，即可进行预览和动态调试。

![](img/ec583ecafaea86f54c189669ec5b025a_142.png)

![](img/ec583ecafaea86f54c189669ec5b025a_144.png)

![](img/ec583ecafaea86f54c189669ec5b025a_146.png)

![](img/ec583ecafaea86f54c189669ec5b025a_148.png)

## 源码分析与信息挖掘 💎

![](img/ec583ecafaea86f54c189669ec5b025a_150.png)

![](img/ec583ecafaea86f54c189669ec5b025a_152.png)

成功获取并导入源码后，本节我们来看看如何分析这些源码结构并挖掘有价值的信息。

![](img/ec583ecafaea86f54c189669ec5b025a_154.png)

![](img/ec583ecafaea86f54c189669ec5b025a_156.png)

![](img/ec583ecafaea86f54c189669ec5b025a_158.png)

小程序源码有固定的目录结构，主要文件类型如下：
*   `.js` 文件：页面逻辑处理文件，是核心代码所在。
*   `.json` 文件：页面配置文件，可能包含接口地址等。
*   `.wxml` 文件：页面结构文件，类似HTML。
*   `.wxss` 文件：页面样式文件，类似CSS。

![](img/ec583ecafaea86f54c189669ec5b025a_160.png)

![](img/ec583ecafaea86f54c189669ec5b025a_162.png)

![](img/ec583ecafaea86f54c189669ec5b025a_164.png)

在微信开发者工具中，可以进行动态调试：
1.  点击模拟器中的页面元素，调试器会定位到对应的代码文件。
2.  在 `Page` 文件夹下，通常有 `index`（首页）、`list`（列表）等子页面。
3.  结合抓包数据，可以在 `.js` 文件中搜索请求的URL路径，找到对应的业务逻辑代码。

![](img/ec583ecafaea86f54c189669ec5b025a_166.png)

![](img/ec583ecafaea86f54c189669ec5b025a_168.png)

![](img/ec583ecafaea86f54c189669ec5b025a_170.png)

![](img/ec583ecafaea86f54c189669ec5b025a_172.png)

通过源码分析，可以：
*   **发现隐藏接口/资产**：在JS或JSON配置文件中搜索 `http://`、`https://`、域名等关键字。
*   **查找敏感信息**：全局搜索 `access`、`key`、`secret`、`password`、`token` 等敏感词汇，可能会找到云服务（如OSS）的配置密钥。
*   **测试未授权访问**：在开发者工具中尝试直接访问那些在正常小程序中需要登录后才能查看的页面路径，可能会发现未授权访问漏洞。

![](img/ec583ecafaea86f54c189669ec5b025a_174.png)

![](img/ec583ecafaea86f54c189669ec5b025a_176.png)

![](img/ec583ecafaea86f54c189669ec5b025a_178.png)

![](img/ec583ecafaea86f54c189669ec5b025a_180.png)

> **注意**：对获取的源码进行安全测试时，应遵守法律法规，仅用于授权测试或学习研究。

![](img/ec583ecafaea86f54c189669ec5b025a_182.png)

![](img/ec583ecafaea86f54c189669ec5b025a_184.png)

![](img/ec583ecafaea86f54c189669ec5b025a_186.png)

![](img/ec583ecafaea86f54c189669ec5b025a_188.png)

![](img/ec583ecafaea86f54c189669ec5b025a_190.png)

![](img/ec583ecafaea86f54c189669ec5b025a_192.png)

![](img/ec583ecafaea86f54c189669ec5b025a_194.png)

## 课程总结

![](img/ec583ecafaea86f54c189669ec5b025a_196.png)

![](img/ec583ecafaea86f54c189669ec5b025a_197.png)

![](img/ec583ecafaea86f54c189669ec5b025a_199.png)

![](img/ec583ecafaea86f54c189669ec5b025a_201.png)

![](img/ec583ecafaea86f54c189669ec5b025a_203.png)

本节课我们一起学习了针对小程序进行信息收集的全套方法。

![](img/ec583ecafaea86f54c189669ec5b025a_205.png)

![](img/ec583ecafaea86f54c189669ec5b025a_207.png)

![](img/ec583ecafaea86f54c189669ec5b025a_209.png)

![](img/ec583ecafaea86f54c189669ec5b025a_211.png)

![](img/ec583ecafaea86f54c189669ec5b025a_213.png)

![](img/ec583ecafaea86f54c189669ec5b025a_215.png)

我们首先介绍了小程序的概念及其在各大平台的分布，并掌握了通过关键字搜索寻找目标的方法。接着，我们回顾了使用 Burp Suite 和 Proxifier 对小程序进行抓包的技术，以获取其网络通信中的资产信息。

![](img/ec583ecafaea86f54c189669ec5b025a_217.png)

![](img/ec583ecafaea86f54c189669ec5b025a_219.png)

![](img/ec583ecafaea86f54c189669ec5b025a_221.png)

![](img/ec583ecafaea86f54c189669ec5b025a_223.png)

![](img/ec583ecafaea86f54c189669ec5b025a_225.png)

![](img/ec583ecafaea86f54c189669ec5b025a_227.png)

课程的重点在于小程序的逆向分析。我们详细讲解了如何使用专用工具对小程序包进行解包和反编译，从而获得其源代码。获取源码后，我们介绍了如何利用微信开发者工具进行导入和调试，并深入分析了小程序源码的目录结构（`.js`, `.json`, `.wxml`, `.wxss`）。最后，我们探讨了如何从源码中挖掘更多资产、发现敏感配置信息（如API密钥）以及测试未授权访问等安全问题。

![](img/ec583ecafaea86f54c189669ec5b025a_229.png)

![](img/ec583ecafaea86f54c189669ec5b025a_231.png)

![](img/ec583ecafaea86f54c189669ec5b025a_233.png)

![](img/ec583ecafaea86f54c189669ec5b025a_235.png)

![](img/ec583ecafaea86f54c189669ec5b025a_237.png)

![](img/ec583ecafaea86f54c189669ec5b025a_239.png)

通过本课的学习，你应当能够从内外两个维度对小程序进行全面的信息收集，为后续的深入安全测试奠定坚实的基础。