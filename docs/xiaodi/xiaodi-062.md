#  062：第63天）

在本节课中，我们将要学习JavaScript逆向分析的核心技术，并将其与Web安全测试相结合。我们将重点掌握三种断点调试方法、调用堆栈与作用域的分析，以及如何将逆向出的加密算法集成到BurpSuite插件中进行自动化发包测试。

![](img/e22f7489b3e59a47de9ad9aed20d4336_1.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_3.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_5.png)

---

## 📚 课程概述

从本节课开始，我们将正式进入JavaScript应用安全的学习阶段。我们将探讨JS环境（包括Node.js、Vue等框架）中常见的安全问题。本次JS课程预计包含3-5次直播，内容涵盖JS逆向分析、签名（SIGN）绕过等关键技术。

![](img/e22f7489b3e59a47de9ad9aed20d4336_7.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_9.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_11.png)

学习JS逆向技术对于安全测试者至关重要，主要体现在以下四个方面：
1.  针对前端网站的渗透测试，能提供更多思路。
2.  在密码登录的算法枚举/爆破中起到关键作用。
3.  当测试参数被加密时，通过分析提取加密方式，组合Payload进行漏洞测试。
4.  有助于发现JS文件中泄露的API地址和请求路径，从而测试未授权访问等漏洞。

![](img/e22f7489b3e59a47de9ad9aed20d4336_13.png)

---

![](img/e22f7489b3e59a47de9ad9aed20d4336_15.png)

## 🔍 前置知识：调试面板核心概念

![](img/e22f7489b3e59a47de9ad9aed20d4336_17.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_19.png)

在开始实战前，我们需要了解浏览器开发者工具中的几个核心概念。

![](img/e22f7489b3e59a47de9ad9aed20d4336_21.png)

上一节我们介绍了学习JS逆向的意义，本节中我们来看看调试时必备的工具面板。

![](img/e22f7489b3e59a47de9ad9aed20d4336_23.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_25.png)

打开浏览器（如Chrome/Edge）的开发者工具（检查/审查元素），切换到 **源代码（Sources）** 标签页。你会看到几个关键面板：

*   **作用域（Scope）**：这里显示的是代码**运行后相关的数据值**。你可以在这里查看变量在特定断点处的具体值。
*   **调用堆栈（Call Stack）**：这里显示代码的**执行逻辑顺序**。需要注意的是，它的显示顺序是**由下往上**的。最下面的是起点（最早调用的函数），最上面的是终点（当前暂停处的函数）。分析执行流程时应从下往上看，而逆向溯源时则从上往下看。
*   **XHR/提取断点**：用于监控特定的网络请求（XHR即`XMLHttpRequest`）。

---

![](img/e22f7489b3e59a47de9ad9aed20d4336_27.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_29.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_31.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_33.png)

## 🛠️ 三重断点调试法

![](img/e22f7489b3e59a47de9ad9aed20d4336_35.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_37.png)

我们将介绍三种核心的调试方法，也称为“三重端点”法。每种方法适用于不同的场景。

![](img/e22f7489b3e59a47de9ad9aed20d4336_39.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_41.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_43.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_45.png)

以下是三种主要的调试分析方法：

![](img/e22f7489b3e59a47de9ad9aed20d4336_47.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_49.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_51.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_53.png)

1.  **文件流程断点**：在“网络（Network）”面板中，通过请求的“发起程序（Initiator）”查看参与该请求的JS文件，并在关键文件上下断点。
2.  **代码标签断点**：在“元素（Elements）”面板中，对特定的HTML元素（如提交按钮）设置事件监听断点（如“中断于属性修改”）。
3.  **XHR提交断点**：在“源代码（Sources）”面板的“XHR/提取断点”区域，添加需要监控的请求URL地址，当请求发生时自动断下。

![](img/e22f7489b3e59a47de9ad9aed20d4336_55.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_57.png)

此外，还有一种基础的**全局搜索**方法，即通过搜索关键字（如URL、参数名）来定位相关代码，适合入门，但更专业的分析依赖于断点。

![](img/e22f7489b3e59a47de9ad9aed20d4336_59.png)

---

![](img/e22f7489b3e59a47de9ad9aed20d4336_61.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_63.png)

## 🧪 实战案例一：登录加密算法逆向

![](img/e22f7489b3e59a47de9ad9aed20d4336_65.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_67.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_69.png)

现在，我们通过一个实战案例来演练上述方法。假设目标是一个登录接口，其密码在提交前被加密。

![](img/e22f7489b3e59a47de9ad9aed20d4336_71.png)

### 目标分析
抓取登录请求包，发现`username`和`password`参数均为加密后的密文。如果直接使用明文进行爆破或漏洞测试，请求会因为不符合服务端的解密逻辑而失败。因此，我们需要找出加密算法。

### 方法一：全局搜索
在源代码面板，按 `Ctrl+Shift+F` 全局搜索登录请求的URL或关键参数名（如`loginData`）。找到相关代码段后，逐步向上追溯，找到加密函数的定义和调用位置。例如，可能发现类似 `password = encrypt(plainText)` 的代码。

![](img/e22f7489b3e59a47de9ad9aed20d4336_73.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_75.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_77.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_79.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_81.png)

### 方法二：文件流程断点
1.  在“网络”面板找到登录请求。
2.  点击“发起程序”，查看调用堆栈中列出的JS文件。
3.  选择可能包含加密逻辑的文件（如名称中含`login`的文件），点击行号设置断点。
4.  重新触发登录，代码会在断点处暂停。
5.  结合**作用域**面板观察变量值的变化，在`password`从明文变为密文的那一步前后仔细分析，从而定位加密函数。

![](img/e22f7489b3e59a47de9ad9aed20d4336_83.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_85.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_87.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_89.png)

### 方法三：XHR提交断点
1.  在“源代码”面板的“XHR/提取断点”处，添加登录请求的URL。
2.  触发登录，请求发出前会自动断下。
3.  此时在**调用堆栈**中，你正处于请求发送的最后一环。需要**向下（查看更早的调用）** 分析，寻找在发送前对数据进行处理的代码，即加密发生的位置。

![](img/e22f7489b3e59a47de9ad9aed20d4336_91.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_93.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_95.png)

**核心逻辑**：无论用哪种方法，目标都是定位到 `明文 -> 加密函数 -> 密文` 这个转换过程发生的代码位置。

![](img/e22f7489b3e59a47de9ad9aed20d4336_97.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_99.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_101.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_102.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_104.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_106.png)

---

![](img/e22f7489b3e59a47de9ad9aed20d4336_108.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_110.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_111.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_113.png)

## 🧪 实战案例二：算法提取与本地调用

![](img/e22f7489b3e59a47de9ad9aed20d4336_115.png)

找到加密算法后，我们需要将其提取出来，以便在本地或测试工具中调用。

![](img/e22f7489b3e59a47de9ad9aed20d4336_117.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_119.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_121.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_123.png)

上一节我们介绍了如何定位加密代码，本节中我们来看看如何提取并验证它。

![](img/e22f7489b3e59a47de9ad9aed20d4336_125.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_127.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_129.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_131.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_133.png)

1.  **定位算法文件**：在调试器中，点击加密函数，跳转到其定义所在的JS文件。
2.  **提取关键代码**：将该JS文件保存到本地，或直接复制关键的加密函数代码段。例如，可能是一个自定义的`encrypt()`函数，或是对`CryptoJS`等库的调用。
3.  **本地验证**：
    *   可以在开发者工具的**控制台（Console）** 中，尝试调用提取出的函数，传入明文，看是否输出与抓包一致的密文。
    *   也可以使用在线的JS运行环境或Node.js进行验证。

![](img/e22f7489b3e59a47de9ad9aed20d4336_135.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_137.png)

**示例代码结构**：
```javascript
// 假设提取出的加密函数
function customEncrypt(plainText) {
    // 具体的加密逻辑，可能是AES、RSA或自定义算法
    let key = "固定的密钥或从代码中分析得出";
    let encrypted = ... // 加密过程
    return encrypted;
}
// 测试
console.log(customEncrypt("123456"));
```

![](img/e22f7489b3e59a47de9ad9aed20d4336_139.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_141.png)

---

![](img/e22f7489b3e59a47de9ad9aed20d4336_143.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_145.png)

## 🔗 与安全测试结合：BurpSuite插件集成

![](img/e22f7489b3e59a47de9ad9aed20d4336_147.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_149.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_151.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_153.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_155.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_157.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_159.png)

逆向出算法的最终目的是为了自动化安全测试。我们将加密算法集成到BurpSuite中，实现自动化加密发包。

![](img/e22f7489b3e59a47de9ad9aed20d4336_161.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_163.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_165.png)

以下是使用 `BurpSuite` 插件 `JSRunner` 集成自定义算法的步骤：

![](img/e22f7489b3e59a47de9ad9aed20d4336_167.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_169.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_170.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_172.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_174.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_176.png)

1.  **环境准备**：
    *   安装 `JSRunner` 插件（将 `JSRunner.jar` 放入Burp的扩展目录）。
    *   安装 `Node.js` 环境，用于本地执行JS代码。

![](img/e22f7489b3e59a47de9ad9aed20d4336_178.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_180.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_182.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_184.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_186.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_188.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_190.png)

2.  **修改插件脚本**：
    *   打开 `JSRunner` 的示例脚本（如 `example.js`）。
    *   在脚本中，将我们逆向出来的加密算法函数体，赋值给 `processPayload` 函数。这个函数接收Burp传入的原始Payload（明文），返回加密后的结果。
    ```javascript
    // example.js 修改示例
    function processPayload(payload) {
        // 这里调用我们逆向出来的加密函数
        return customEncrypt(payload);
    }
    ```

![](img/e22f7489b3e59a47de9ad9aed20d4336_192.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_194.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_196.png)

3.  **启动并连接**：
    *   在命令行使用 `node` 运行修改后的JS脚本，它会启动一个本地服务。
    *   在Burp的 `JSRunner` 标签页中，配置连接到此本地服务的地址和端口。

![](img/e22f7489b3e59a47de9ad9aed20d4336_198.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_200.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_202.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_204.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_206.png)

4.  **实战测试**：
    *   在Burp的 `Intruder` 模块，将需要爆破的密码参数值设为 `§明文§`。
    *   在 `Payload Processing` 规则中，添加 `Invoke Burp extension` 处理项，选择配置好的 `JSRunner`。
    *   这样，Intruder在发包时，会自动将每个Payload明文通过我们的JS脚本加密，再发送出去，从而绕过前端加密，实现有效的爆破或漏洞测试。

![](img/e22f7489b3e59a47de9ad9aed20d4336_208.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_210.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_212.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_214.png)

---

![](img/e22f7489b3e59a47de9ad9aed20d4336_216.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_218.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_220.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_222.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_224.png)

## 💡 课程总结与下节预告

![](img/e22f7489b3e59a47de9ad9aed20d4336_226.png)

本节课中我们一起学习了：
1.  **JS逆向的意义**：赋能安全测试，解决加密参数下的爆破、漏洞探测等问题。
2.  **调试基础**：理解了**作用域**（看数据值）和**调用堆栈**（看执行顺序）的核心作用。
3.  **三重断点法**：掌握了**文件流程断点**、**代码标签断点**和**XHR提交断点**的使用场景与方法。
4.  **实战流程**：从定位加密算法、提取代码到本地验证的完整逆向流程。
5.  **安全集成**：学会了将逆向算法集成到BurpSuite插件中，实现自动化加密测试。

![](img/e22f7489b3e59a47de9ad9aed20d4336_228.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_230.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_232.png)

下节课，我们将面对更复杂的挑战：**签名（SIGN）绕过**。在许多应用（尤其是APP和小程序）的请求中，会携带一个动态变化的`sign`参数，用于防止请求重放和篡改。我们将学习如何分析和逆向这种签名算法，让我们的自动化测试工具能够动态生成有效的签名，从而完成更深层次的安全测试。

![](img/e22f7489b3e59a47de9ad9aed20d4336_234.png)

---

![](img/e22f7489b3e59a47de9ad9aed20d4336_236.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_237.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_239.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_241.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_243.png)

![](img/e22f7489b3e59a47de9ad9aed20d4336_244.png)

**记住**：JS逆向的核心在于耐心分析代码执行流和数据流。掌握这些技能，能让你在Web安全测试中突破前端加密的壁垒，发现更多潜在的安全风险。