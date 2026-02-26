#  017：Web应用框架与组件识别技术

![](img/bb2771965646bd8880a6d3555d0b2aef_1.png)

在本节课中，我们将学习如何识别Web应用所使用的开发框架和第三方组件。这对于后续的安全测试至关重要，因为许多安全漏洞都源于这些框架和组件本身。

## 📋 课程概述

![](img/bb2771965646bd8880a6d3555d0b2aef_3.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_5.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_7.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_9.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_11.png)

信息收集阶段，我们不仅需要识别网站使用的CMS（内容管理系统），还需要识别其底层的开发框架和关键组件。这是因为：

![](img/bb2771965646bd8880a6d3555d0b2aef_13.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_15.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_17.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_19.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_21.png)

1.  **CMS识别**主要针对PHP等开源程序，对Java、Python等语言开发的应用覆盖有限。
2.  **开发框架**（如ThinkPHP、Django）是代码的“模板”，其安全性直接影响整个应用。
3.  **第三方组件**（如FastJson、Shiro）用于实现特定功能（如数据解析、身份验证），它们自身也可能存在漏洞。

![](img/bb2771965646bd8880a6d3555d0b2aef_23.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_25.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_26.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_28.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_29.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_31.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_33.png)

识别出这些信息，能帮助我们快速定位可能存在的已知漏洞，并理清后续的渗透测试思路。

## 🔍 第一部分：CMS识别工具补充

![](img/bb2771965646bd8880a6d3555d0b2aef_35.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_37.png)

上一节我们介绍了在线CMS识别平台。本节中我们来看看如何在无法连接外网（如内网环境）时进行识别。

针对内网环境，我们可以使用本地工具 `Goby` 或 `CMSeeK` 进行识别。这些工具内置了指纹库，通过比对目标站点特定文件的MD5值或路径是否存在来判断CMS类型。

以下是使用此类工具的基本步骤：
1.  运行工具，指定目标URL。
2.  工具会自动加载本地指纹库进行比对。
3.  输出识别结果。

**示例命令（概念性）**：
```bash
./cmseek -u http://target-site
```

![](img/bb2771965646bd8880a6d3555d0b2aef_39.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_41.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_43.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_45.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_47.png)

## 🧱 第二部分：理解源码的三种组成模式

![](img/bb2771965646bd8880a6d3555d0b2aef_49.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_51.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_53.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_55.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_57.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_59.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_61.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_63.png)

在深入识别之前，我们需要理解一个Web应用源码的三种常见组成模式，这决定了我们的测试重点：

![](img/bb2771965646bd8880a6d3555d0b2aef_65.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_67.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_69.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_71.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_72.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_74.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_76.png)

1.  **纯手写代码**：所有功能均由开发者从头编写。这种模式安全性完全取决于开发者水平，最容易出现逻辑漏洞和编码错误。
2.  **框架开发**：使用成熟的开发框架（如Spring Boot、ThinkPHP）构建应用。应用的安全性很大程度上依赖于框架自身的安全机制。我们的测试重点在于**框架本身是否有公开漏洞**。
3.  **框架+组件**：在框架基础上，引入第三方组件实现特定功能（如用Shiro做权限控制，用FastJson解析数据）。此时，安全风险来源于**框架漏洞**和**组件漏洞**。

![](img/bb2771965646bd8880a6d3555d0b2aef_78.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_80.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_82.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_84.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_86.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_87.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_89.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_91.png)

**核心思路**：识别出框架和组件，就等于找到了可能存在的“漏洞说明书”。

![](img/bb2771965646bd8880a6d3555d0b2aef_93.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_95.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_97.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_99.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_101.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_103.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_105.png)

## 🐍 第三部分：Python Web框架识别

![](img/bb2771965646bd8880a6d3555d0b2aef_107.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_109.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_111.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_113.png)

Python常见的Web开发框架有Django和Flask。识别它们主要依靠一些特征。

![](img/bb2771965646bd8880a6d3555d0b2aef_115.png)

### Django框架识别

![](img/bb2771965646bd8880a6d3555d0b2aef_117.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_119.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_121.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_123.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_125.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_127.png)

以下是Django的常见识别特征：
*   **响应头特征**：查看HTTP响应头，有时会包含 `X-Frame-Options: DENY` 等信息，虽然非独有，但结合其他特征可判断。
*   **Cookie特征**：Django设置的会话Cookie名称默认可能为 `sessionid`。
*   **插件识别**：使用Wappalyzer等浏览器插件可以直接识别。
*   **目录结构**：如果暴露了错误页面或某些路径，可能会泄露Django的典型目录结构。

![](img/bb2771965646bd8880a6d3555d0b2aef_129.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_131.png)

### Flask框架识别

![](img/bb2771965646bd8880a6d3555d0b2aef_133.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_135.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_137.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_139.png)

以下是Flask的常见识别特征：
*   **响应头特征**：较难有唯一标识。
*   **错误信息**：在调试模式下，Flask的错误页面会明确显示“Flask”字样。
*   **插件识别**：同样可借助Wappalyzer等插件。

![](img/bb2771965646bd8880a6d3555d0b2aef_141.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_143.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_145.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_146.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_148.png)

**过渡**：Python框架在实战中遇到的比例相对较低。接下来，我们看看在Web应用中占比极高的PHP框架。

## 🐘 第四部分：PHP开发框架识别

![](img/bb2771965646bd8880a6d3555d0b2aef_150.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_152.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_154.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_156.png)

国内主流的PHP框架包括ThinkPHP、Laravel和Yii。它们都有比较明显的识别特征。

### ThinkPHP框架识别

![](img/bb2771965646bd8880a6d3555d0b2aef_158.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_160.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_162.png)

以下是ThinkPHP的常见识别特征：
*   **固定标识**：访问不存在的路由时，可能返回包含“ThinkPHP”字样的错误页面。
*   **Cookie特征**：可能包含类似 `think_var` 的Cookie名。
*   **图标特征**：默认的favicon.ico图标是特定的。
*   **路由特征**：URL路径可能呈现 `index.php?s=/...` 的格式。

![](img/bb2771965646bd8880a6d3555d0b2aef_164.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_166.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_167.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_169.png)

### Laravel框架识别

![](img/bb2771965646bd8880a6d3555d0b2aef_171.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_173.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_175.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_177.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_179.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_180.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_182.png)

以下是Laravel的常见识别特征：
*   **Cookie特征**：会话Cookie名称默认通常为 `laravel_session`。
*   **响应头特征**：可能包含 `X-Powered-By: Laravel`。
*   **CSRF Token**：表单中常包含名为 `_token` 的隐藏字段，用于防止CSRF攻击。

![](img/bb2771965646bd8880a6d3555d0b2aef_184.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_186.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_188.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_190.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_192.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_194.png)

### Yii框架识别

![](img/bb2771965646bd8880a6d3555d0b2aef_196.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_198.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_200.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_202.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_204.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_206.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_208.png)

以下是Yii的常见识别特征：
*   **Cookie特征**：可能包含 `_csrf` Cookie。
*   **页面内容**：页面HTML中可能包含 `Yii::` 这样的框架标识字符串。

![](img/bb2771965646bd8880a6d3555d0b2aef_210.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_212.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_214.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_216.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_218.png)

**总结**：PHP框架的识别很大程度上依赖于对固定Cookie名、错误页面信息、CSRF Token名称等特征的观察和记忆。

![](img/bb2771965646bd8880a6d3555d0b2aef_220.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_222.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_224.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_226.png)

## ☕ 第五部分：Java应用组件与框架识别

![](img/bb2771965646bd8880a6d3555d0b2aef_228.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_230.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_232.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_234.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_236.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_238.png)

Java生态庞大，大量使用第三方组件和框架，其中许多都曾曝出高危漏洞。识别它们至关重要。

![](img/bb2771965646bd8880a6d3555d0b2aef_240.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_242.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_244.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_246.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_248.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_250.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_252.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_254.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_256.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_258.png)

### 常见Java漏洞组件/框架列表

![](img/bb2771965646bd8880a6d3555d0b2aef_260.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_262.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_264.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_266.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_268.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_270.png)

以下是近年来常出现安全问题的部分Java组件和框架：
*   **FastJson / Jackson**：JSON解析库，曾出现反序列化漏洞。
*   **Shiro**：权限认证框架，RememberMe功能曾出现反序列化漏洞。
*   **Log4j**：日志记录组件，曾出现远程代码执行漏洞。
*   **Spring Boot**：微服务框架，actuator端点等可能泄露信息或导致RCE。
*   **Solr**：搜索服务器，未授权访问、命令执行漏洞。
*   **Struts2**：老牌MVC框架，历史漏洞非常多。

![](img/bb2771965646bd8880a6d3555d0b2aef_272.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_274.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_275.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_277.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_279.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_280.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_282.png)

### 关键组件识别示例

![](img/bb2771965646bd8880a6d3555d0b2aef_284.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_285.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_286.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_288.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_290.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_292.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_293.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_294.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_296.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_298.png)

#### Shiro组件识别

![](img/bb2771965646bd8880a6d3555d0b2aef_300.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_302.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_304.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_306.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_308.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_310.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_312.png)

识别Shiro最直接的方法是检查Cookie。
**特征**：观察浏览器Cookie或HTTP请求包，如果存在名为 `rememberMe` 的Cookie，则很可能使用了Shiro框架。其值通常是加密后的内容，删除时为 `deleteMe`。

![](img/bb2771965646bd8880a6d3555d0b2aef_314.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_316.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_318.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_320.png)

**示例Cookie**：
```
rememberMe=deleteMe
```

#### Spring Boot框架识别

![](img/bb2771965646bd8880a6d3555d0b2aef_322.png)

以下是Spring Boot的常见识别特征：
*   **默认错误页面**：访问不存在的路径时，可能返回一个格式统一的白色错误页面，标题为“Whitelabel Error Page”。
*   **默认图标**：使用绿色的“叶子”图标作为favicon。
*   **端点泄露**：有时可以通过 `/actuator`、`/env` 等路径访问到监控端点。

![](img/bb2771965646bd8880a6d3555d0b2aef_324.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_326.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_328.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_330.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_332.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_333.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_335.png)

#### Solr组件识别

![](img/bb2771965646bd8880a6d3555d0b2aef_337.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_339.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_341.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_343.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_345.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_347.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_349.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_351.png)

以下是Solr的常见识别特征：
*   **默认端口**：Solr控制台通常运行在 `8983` 端口。
*   **特定路径**：通过访问 `http://target:8983/solr/` 可以进入其管理界面。
*   **页面特征**：管理界面具有独特的UI样式和Logo。

![](img/bb2771965646bd8880a6d3555d0b2aef_353.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_355.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_357.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_359.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_361.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_362.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_363.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_364.png)

**过渡**：识别出这些组件后，我们就可以使用对应的漏洞利用工具进行测试。这比盲目扫描效率高得多。

![](img/bb2771965646bd8880a6d3555d0b2aef_366.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_368.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_370.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_372.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_374.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_376.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_378.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_380.png)

## 🎯 第六部分：识别技术的意义与实战关联

![](img/bb2771965646bd8880a6d3555d0b2aef_382.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_384.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_386.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_388.png)

我们进行以上识别的最终目的，是为了指导后续的漏洞利用。

![](img/bb2771965646bd8880a6d3555d0b2aef_390.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_392.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_394.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_396.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_398.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_400.png)

1.  **对于CMS**：识别后，可搜索该CMS的公开漏洞，使用专用EXP工具进行测试。
2.  **对于框架/组件**：识别后，可判断目标是否受某个特定框架漏洞或组件漏洞影响。例如：
    *   识别出Shiro -> 尝试Shiro反序列化漏洞。
    *   识别出FastJson -> 尝试FastJson反序列化漏洞。
    *   识别出Spring Boot且暴露actuator端点 -> 尝试信息泄露或RCE。

![](img/bb2771965646bd8880a6d3555d0b2aef_402.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_404.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_406.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_408.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_410.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_412.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_414.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_416.png)

这使我们的渗透测试从“广撒网”变为“精准打击”，思路更加清晰。

![](img/bb2771965646bd8880a6d3555d0b2aef_418.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_420.png)

## 📚 课程总结

![](img/bb2771965646bd8880a6d3555d0b2aef_422.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_424.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_426.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_427.png)

本节课中我们一起学习了Web应用框架与组件的识别技术。

![](img/bb2771965646bd8880a6d3555d0b2aef_429.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_431.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_433.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_435.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_437.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_439.png)

*   **我们明确了目标**：识别应用是纯手写、基于框架开发还是框架结合组件。
*   **我们掌握了方法**：针对Python、PHP的框架，主要通过响应头、Cookie、错误页面、插件等特征识别。针对Java的组件和框架，则通过特定Cookie（如Shiro）、默认端口（如Solr）、特有页面（如Spring Boot错误页）等方式识别。
*   **我们理解了意义**：这些识别工作是为后续漏洞利用铺路，能够快速关联已知漏洞，提升测试效率。

![](img/bb2771965646bd8880a6d3555d0b2aef_441.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_443.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_444.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_445.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_447.png)

![](img/bb2771965646bd8880a6d3555d0b2aef_449.png)

请将本节课提到的各种框架和组件的特征作为知识要点进行记忆和整理，这将在实战中为你节省大量时间。