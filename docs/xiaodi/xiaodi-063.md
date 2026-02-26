#  063：反调试与代码混淆技术

![](img/6c5a8065f6d2770b551462e8e448eebe_1.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_3.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_5.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_7.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_9.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_11.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_13.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_15.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_17.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_19.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_21.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_23.png)

在本节课中，我们将要学习JavaScript安全中的两个核心概念：**反调试**与**代码混淆**。我们将了解它们的基本概念、实现原理，并重点掌握绕过反调试和还原混淆代码的实用方法。

![](img/6c5a8065f6d2770b551462e8e448eebe_25.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_27.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_29.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_31.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_33.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_35.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_37.png)

## 🔍 反调试技术概述

![](img/6c5a8065f6d2770b551462e8e448eebe_39.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_41.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_43.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_45.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_47.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_49.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_51.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_53.png)

上一节我们介绍了JS安全的基本背景，本节中我们来看看什么是反调试。

![](img/6c5a8065f6d2770b551462e8e448eebe_55.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_57.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_59.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_61.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_63.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_65.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_67.png)

反调试的核心目的是**保护源代码程序**，防止他人通过动态调试手段分析自己的代码。在JS安全领域，反调试技术旨在阻止攻击者利用浏览器开发者工具（如F12）进行调试，从而获取关键逻辑或数据。

![](img/6c5a8065f6d2770b551462e8e448eebe_69.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_71.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_73.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_75.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_77.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_79.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_81.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_83.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_85.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_87.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_89.png)

其检测手段通常包括以下几种：
*   **键盘监听**：监听F12等快捷键的触发。
*   **检测浏览器窗口高度差值**：判断开发者工具面板是否被打开。
*   **检测开发者工具状态**：直接查询`window.console`或`window.devtools`等对象。
*   **检测代码运行时间差**：利用`debugger`语句或复杂计算导致的时间异常来判断是否处于调试状态。
*   **检测运行环境**：判断代码是否在浏览器中运行。

![](img/6c5a8065f6d2770b551462e8e448eebe_91.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_93.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_94.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_96.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_98.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_100.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_101.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_103.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_105.png)

当检测到调试行为时，程序可能会采取**弹出警告**、**跳入无限循环**或**直接终止执行**等策略。

![](img/6c5a8065f6d2770b551462e8e448eebe_107.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_109.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_111.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_113.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_115.png)

## 🚧 常见反调试绕过方法

![](img/6c5a8065f6d2770b551462e8e448eebe_117.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_118.png)

了解了反调试的原理后，我们来看看如何绕过这些防护。以下是五种常见的绕过方法，需要根据目标检测逻辑的强度选择使用。

![](img/6c5a8065f6d2770b551462e8e448eebe_120.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_122.png)

以下是五种常见的绕过方法：

![](img/6c5a8065f6d2770b551462e8e448eebe_124.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_126.png)

1.  **禁用断点法**：在开发者工具的源代码面板，点击**“停用断点”**（Deactivate breakpoints）按钮。这能阻止所有`debugger`语句生效，让代码继续执行，但代价是你也无法主动下断点进行调试了。
2.  **条件断点法**：在触发反调试的`debugger`语句所在行设置条件断点，并将条件设置为`false`。这样该断点永远不会被触发。
    ```javascript
    // 在debugger语句行右键添加条件断点，输入 false
    if (false) { debugger; } // 实际不会执行
    ```
3.  **永不暂停法**：在调用堆栈（Call Stack）中找到反调试函数，右键选择 **“Never pause here”**。效果与条件断点法类似。
4.  **置空函数法（Hook）**：在控制台（Console）中找到并重写触发反调试的关键函数，将其替换为一个空函数。
    ```javascript
    // 假设检测函数是 detectDebug
    detectDebug = function(){}; // 将其“钩住”并置空
    ```
5.  **本地覆盖法**：这是最彻底的方法。利用开发者工具的 **“Overrides”**（覆盖）功能，将网页的JS文件保存到本地，删除或注释掉其中的反调试代码，然后让浏览器加载本地修改后的版本。

![](img/6c5a8065f6d2770b551462e8e448eebe_128.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_129.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_131.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_133.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_135.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_137.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_139.png)

**本地覆盖法操作步骤**：
*   打开开发者工具，进入 **“Sources”**（源代码）面板。
*   找到 **“Overrides”** 选项卡，选择一个本地文件夹用于保存覆盖文件。
*   在 **“Page”** 中找到触发反调试的JS文件（如 `index.js`, `anti-debug.js`）。
*   右键点击该文件，选择 **“Save for overrides”**。
*   在编辑器中打开该本地文件，找到反调试代码（如包含 `debugger`、`setInterval` 循环检测的代码块）并将其注释或删除。
*   保存文件并刷新网页，浏览器将加载你修改后的本地版本，从而绕过反调试。

![](img/6c5a8065f6d2770b551462e8e448eebe_141.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_143.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_145.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_147.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_149.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_150.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_152.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_154.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_156.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_158.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_160.png)

## 🔐 代码混淆技术概述

![](img/6c5a8065f6d2770b551462e8e448eebe_162.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_163.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_164.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_165.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_167.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_168.png)

上一节我们探讨了如何对抗反调试，本节中我们来看看另一种保护手段——代码混淆。

![](img/6c5a8065f6d2770b551462e8e448eebe_170.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_172.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_174.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_176.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_178.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_180.png)

代码混淆是将源代码进行加密和变换，使其变得难以阅读和理解，但功能保持不变。即使攻击者绕过了反调试，如果代码本身是混淆的，分析难度也会极大增加。保护程序的最佳实践通常是**同时使用反调试和代码混淆**。

![](img/6c5a8065f6d2770b551462e8e448eebe_182.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_184.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_186.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_188.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_190.png)

常见的混淆类型包括：
*   **变量/函数名混淆**：将有意义的名称替换为无意义的短字符（如 `a`, `b`, `_0xabc`）。
*   **字符串混淆**：将字符串加密或拆分为数组再拼接。
*   **控制流平坦化**：打乱代码的执行流程，增加分析复杂度。
*   **使用特定混淆工具**：如 `JShaman`、`JScrambler`、`jsfuck` 以及商业版的 `V5`、`V6`、`V7` 加密等。

![](img/6c5a8065f6d2770b551462e8e448eebe_192.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_194.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_196.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_198.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_200.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_202.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_204.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_206.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_208.png)

一个简单的字符串混淆示例：
```javascript
// 混淆前
var apiKey = "secret_key_123";
// 混淆后（一种形式）
var _0x12ab = ["s", "e", "c", "r", "e", "t", /*...*/].join('');
```

![](img/6c5a8065f6d2770b551462e8e448eebe_210.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_211.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_213.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_215.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_217.png)

## 🛠️ 混淆代码还原实战

![](img/6c5a8065f6d2770b551462e8e448eebe_219.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_221.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_223.png)

面对混淆的代码，我们并非无计可施。对于已知的、常见的混淆算法，尤其是开源或流行商业混淆器，往往有现成的解混淆工具或方法。

![](img/6c5a8065f6d2770b551462e8e448eebe_225.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_226.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_228.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_230.png)

**还原思路与步骤**：

![](img/6c5a8065f6d2770b551462e8e448eebe_232.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_234.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_236.png)

1.  **识别混淆类型**：观察代码特征。例如，大量`Array`、`String.fromCharCode`、`eval`、`function`套娃或特定格式的十六进制字符串，可能指向某种已知混淆。
2.  **寻找解密函数**：在混淆代码中，通常存在一个**解密函数**（或入口函数），用于还原被混淆的字符串或逻辑。它的结构可能像 `(function(...){...})(...)`。
3.  **使用自动化工具**：
    *   对于 `jsfuck`、`aaencode`、`jjencode` 等趣味编码，网上有在线解码器。
    *   对于 `JShaman`、`V5` 等商业混淆，可以尝试在GitHub等平台搜索对应的解混淆脚本或工具（例如搜索关键字 `v5 decrypt javascript`）。
    *   使用专业的JS逆向工具或插件辅助分析。
4.  **动态调试提取**：如果静态分析困难，可以结合之前绕过的反调试，在代码运行时下断点，观察内存中变量被解密后的真实值。

![](img/6c5a8065f6d2770b551462e8e448eebe_238.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_240.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_242.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_244.png)

**案例：还原某商业加密（V5）**：
假设遇到一个使用“V5”加密的JS文件，代码完全不可读。
*   搜索发现GitHub上有开源项目 `v5-decryptor`。
*   使用该工具直接对加密的JS文件进行解密。
*   解密后，得到可读的源代码，其中包含核心的加密密钥和算法（例如AES加密的key和iv）。
*   利用还原出的算法，即可对网站传输的加密数据进行解密，或模拟加密过程进行安全测试。

![](img/6c5a8065f6d2770b551462e8e448eebe_246.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_248.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_250.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_252.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_254.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_256.png)

**安全测试意义**：
*   **对于爬虫/数据分析**：还原算法以解密接口数据。
*   **对于安全审计**：
    1.  还原算法后，可分析加密逻辑，构造Payload进行漏洞测试（如参数注入）。
    2.  分析还原后的清晰JS代码，从中发现隐藏的API接口、敏感路径或业务逻辑漏洞。

![](img/6c5a8065f6d2770b551462e8e448eebe_258.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_260.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_262.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_264.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_266.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_268.png)

## 📚 课程总结

![](img/6c5a8065f6d2770b551462e8e448eebe_270.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_272.png)

本节课中我们一起学习了JavaScript安全中反调试与代码混淆两大核心防护技术。

![](img/6c5a8065f6d2770b551462e8e448eebe_274.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_276.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_278.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_280.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_282.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_284.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_286.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_288.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_290.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_292.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_294.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_296.png)

我们首先明确了**反调试**的目的是阻止动态分析，并掌握了五种绕过方法，特别是功能强大的**本地覆盖法**。接着，我们了解了**代码混淆**如何增加静态分析难度，并学习了通过识别类型、寻找工具进行**还原**的实战思路。

![](img/6c5a8065f6d2770b551462e8e448eebe_298.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_300.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_302.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_304.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_305.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_307.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_308.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_309.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_311.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_313.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_315.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_317.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_319.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_321.png)

![](img/6c5a8065f6d2770b551462e8e448eebe_323.png)

关键要点在于：**对抗这些防护是一个持续的过程**。没有一种方法能解决所有问题，需要根据实际情况灵活组合运用所学技巧。掌握这些能力，将为你进行深入的JS逆向、漏洞挖掘或数据抓取打下坚实基础。