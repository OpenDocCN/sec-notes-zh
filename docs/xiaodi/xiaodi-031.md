#  031：JavaScript应用、WebPack打包器与第三方库JQuery的安全检测

![](img/e90e96ef59b334500d0ec0f65bc6619c_0.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_2.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_4.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_6.png)

在本节课中，我们将学习JavaScript开发中的两个重要拓展部分：打包器（以WebPack为例）和第三方库（以JQuery为例）。我们将了解它们的基本概念、使用方法，并重点探讨在使用不当时可能引发的安全问题，特别是源码泄露和版本漏洞。

![](img/e90e96ef59b334500d0ec0f65bc6619c_8.png)

---

![](img/e90e96ef59b334500d0ec0f65bc6619c_10.png)

## 🧩 什么是打包器（WebPack）？

![](img/e90e96ef59b334500d0ec0f65bc6619c_12.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_14.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_16.png)

上一节我们介绍了JavaScript开发的基础。本节中，我们来看看什么是打包器。

![](img/e90e96ef59b334500d0ec0f65bc6619c_18.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_20.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_22.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_24.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_26.png)

WebPack是一个模块打包器。在WebPack中，前端的所有资源文件（如JavaScript、CSS、图片）都被当作模块处理，最终会生成优化后的静态资源。

### 为什么需要打包器？

![](img/e90e96ef59b334500d0ec0f65bc6619c_28.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_30.png)

在传统开发中，如果项目有多个JavaScript文件，我们需要在HTML中逐一引用它们，并且必须注意引用的顺序（例如，函数声明必须在调用之前）。当文件数量很多时，这会变得非常繁琐且容易出错。

![](img/e90e96ef59b334500d0ec0f65bc6619c_32.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_34.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_36.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_38.png)

WebPack可以解决这个问题。它能够分析模块间的依赖关系，将多个有依赖关系的模块打包成一个或多个文件。这样做的好处是：
1.  **简化引用**：只需在HTML中引用最终打包好的文件。
2.  **实现模块化**：代码组织更清晰，便于维护和开发。
3.  **优化资源**：可以进行代码压缩、混淆等优化操作。

### WebPack简单使用演示

![](img/e90e96ef59b334500d0ec0f65bc6619c_40.png)

以下是使用WebPack的基本步骤：

![](img/e90e96ef59b334500d0ec0f65bc6619c_42.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_44.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_46.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_47.png)

1.  **创建项目文件结构**
    我们创建两个模块文件和一个入口文件来演示依赖关系。

    **src/sum.js**
    ```javascript
    export function sum(x, y) {
        return x + y;
    }
    ```

    **src/subtract.js**
    ```javascript
    export function subtract(x, y) {
        return x - y;
    }
    ```

    **src/main.js** （入口文件）
    ```javascript
    import { sum } from './sum.js';
    import { subtract } from './subtract.js';

    console.log(sum(1, 2)); // 输出：3
    console.log(subtract(2, 1)); // 输出：1
    ```

    注意：这里使用的是ES6模块语法（`import/export`），浏览器原生不支持这种模块化语法直接运行，需要打包器处理。

![](img/e90e96ef59b334500d0ec0f65bc6619c_49.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_51.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_53.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_55.png)

2.  **安装WebPack**
    在项目目录下，通过npm安装WebPack。
    ```bash
    npm install webpack webpack-cli --save-dev
    ```

![](img/e90e96ef59b334500d0ec0f65bc6619c_57.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_59.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_61.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_63.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_65.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_67.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_69.png)

3.  **创建配置文件**
    在项目根目录创建 `webpack.config.js` 文件，配置入口、输出和模式。

    **webpack.config.js**
    ```javascript
    const path = require('path');

    module.exports = {
        entry: './src/main.js', // 入口文件
        output: {
            filename: 'bundle.js', // 输出文件名
            path: path.resolve(__dirname, 'dist'), // 输出目录
        },
        mode: 'development', // 模式：开发模式
    };
    ```
    配置文件主要包含：
    *   `entry`：打包的入口起点。
    *   `output`：打包后的资源输出位置和名称。
    *   `mode`：打包模式，常见的有 `development`（开发）和 `production`（生产）。

![](img/e90e96ef59b334500d0ec0f65bc6619c_71.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_73.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_75.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_77.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_79.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_81.png)

4.  **运行打包命令**
    执行以下命令，WebPack会根据配置进行打包。
    ```bash
    npx webpack
    ```
    打包完成后，会在 `dist` 目录下生成 `bundle.js` 文件。在HTML中只需引用这一个文件即可。

![](img/e90e96ef59b334500d0ec0f65bc6619c_83.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_85.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_86.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_88.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_90.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_92.png)

---

![](img/e90e96ef59b334500d0ec0f65bc6619c_94.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_95.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_97.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_99.png)

## 🔒 WebPack打包模式与安全问题

![](img/e90e96ef59b334500d0ec0f65bc6619c_101.png)

上一节我们演示了WebPack的基本用法。本节中我们来关注其核心的安全问题：打包模式的选择。

WebPack的 `mode` 配置直接影响输出代码的形态和安全性。

![](img/e90e96ef59b334500d0ec0f65bc6619c_103.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_105.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_107.png)

*   **开发模式 (`development`)**：
    *   **特点**：代码未经过深度压缩和优化，包含完整的源码映射（Source Map）和注释。
    *   **目的**：便于开发者调试。
    *   **安全风险**：如果错误地将此模式用于生产环境，会导致**前端源码完全泄露**。攻击者可以通过浏览器开发者工具直接查看近乎原始的代码逻辑、API接口、敏感配置信息等。

![](img/e90e96ef59b334500d0ec0f65bc6619c_108.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_110.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_112.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_114.png)

*   **生产模式 (`production`)**：
    *   **特点**：代码被高度压缩、混淆、优化，移除了不必要的代码和注释。
    *   **目的**：减少文件体积，提高执行效率，并保护代码逻辑。
    *   **安全性**：能有效防止源码的直接暴露。

![](img/e90e96ef59b334500d0ec0f65bc6619c_116.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_118.png)

### 安全对比实验

![](img/e90e96ef59b334500d0ec0f65bc6619c_120.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_122.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_124.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_126.png)

1.  使用 `mode: 'development'` 打包后，生成的 `bundle.js` 文件较大，在浏览器中可清晰看到 `src/` 目录下的原始模块文件和代码逻辑。
2.  使用 `mode: 'production'` 打包后，生成的 `bundle.js` 文件很小，代码被压缩成单行或几行，变量名被替换，无法直接还原业务逻辑。

![](img/e90e96ef59b334500d0ec0f65bc6619c_128.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_130.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_132.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_134.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_136.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_138.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_140.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_142.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_143.png)

**安全结论**：在项目上线（生产环境）时，务必确保WebPack配置中 `mode` 设置为 `'production'`。配置不当是导致前端源码泄露的常见原因。

![](img/e90e96ef59b334500d0ec0f65bc6619c_145.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_147.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_149.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_151.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_153.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_155.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_157.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_159.png)

### 真实案例

![](img/e90e96ef59b334500d0ec0f65bc6619c_160.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_162.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_164.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_166.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_168.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_170.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_172.png)

某些大型网站曾因误将开发模式的WebPack构建包发布上线，导致其基于Vue.js或React的前端应用源码完全暴露。攻击者可以从中分析出API调用方式、敏感路径、甚至硬编码的密钥，极大地增加了安全风险。

![](img/e90e96ef59b334500d0ec0f65bc6619c_174.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_176.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_178.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_180.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_182.png)

---

![](img/e90e96ef59b334500d0ec0f65bc6619c_184.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_186.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_188.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_190.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_192.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_194.png)

## 📚 第三方库（以JQuery为例）的安全问题

![](img/e90e96ef59b334500d0ec0f65bc6619c_196.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_198.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_200.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_202.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_204.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_206.png)

除了打包工具，项目中引用的第三方库也是安全的关键点。我们以广泛使用的JQuery库为例。

![](img/e90e96ef59b334500d0ec0f65bc6619c_208.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_210.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_212.png)

JQuery是一个快速、简洁的JavaScript库，它简化了HTML文档遍历、事件处理、动画和Ajax交互。许多网站都会直接引用公共CDN或本地存储的JQuery文件。

![](img/e90e96ef59b334500d0ec0f65bc6619c_214.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_216.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_218.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_220.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_222.png)

### 第三方库的通用风险

![](img/e90e96ef59b334500d0ec0f65bc6619c_224.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_226.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_228.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_230.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_232.png)

第三方库的安全风险主要来自：
1.  **库本身存在已知漏洞**：库的某个版本被披露存在安全漏洞（如XSS、CSRF等）。
2.  **使用过时的有漏洞版本**：项目引用了存在漏洞的旧版本，未及时更新。
3.  **不安全的调用方式**：即使库本身安全，开发者以不安全的方式调用其API（例如，将未过滤的用户输入直接传递给某些方法），也会引入风险。

![](img/e90e96ef59b334500d0ec0f65bc6619c_234.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_236.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_238.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_240.png)

### JQuery版本漏洞实例

![](img/e90e96ef59b334500d0ec0f65bc6619c_242.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_244.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_246.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_248.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_250.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_252.png)

JQuery历史上多个版本存在XSS（跨站脚本）漏洞。例如，在特定版本范围内（如1.x至3.x的某些子版本），其`.html()`、`.append()`等方法在处理用户可控的输入时，可能绕过内部的安全过滤，导致XSS攻击。

![](img/e90e96ef59b334500d0ec0f65bc6619c_254.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_256.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_258.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_260.png)

**漏洞演示概要**：
1.  一个页面引用了存在漏洞的JQuery版本（例如v3.4.1）。
2.  页面中有一段使用JQuery方法（如`.html()`）来动态更新DOM元素的代码，且更新的内容来自用户可控的输入。
3.  攻击者可以构造特殊的Payload（如``），当该内容被`.html()`方法处理并插入页面时，会触发XSS攻击，执行恶意脚本。
4.  如果将JQuery升级到已修复该漏洞的版本（例如v3.5.0+），同样的Payload则会被正确过滤或处理，不会执行。

![](img/e90e96ef59b334500d0ec0f65bc6619c_262.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_264.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_266.png)

**安全建议**：
1.  **定期更新**：关注所使用的第三方库的安全公告，及时更新到安全版本。
2.  **子资源完整性检查（SRI）**：如果从CDN引用库，使用SRI哈希来确保获取的库文件未被篡改。
    ```html
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"
            integrity="sha384-KyZXEAg3QhqLMpG8r+Knujsl5/5v8F5i5M5J5J5J5/f5F5F5F5F5F5F5F5F5F5F5F5"
            crossorigin="anonymous"></script>
    ```
3.  **安全编码**：即使使用安全的库版本，在处理用户输入并将其插入DOM时，也应进行适当的转义或使用安全的API（例如，使用`.text()`而非`.html()`来设置纯文本内容）。

![](img/e90e96ef59b334500d0ec0f65bc6619c_268.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_270.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_272.png)

---

## 📝 本节课总结

在本节课中，我们一起学习了JavaScript生态中两个重要的组成部分及其安全问题：

![](img/e90e96ef59b334500d0ec0f65bc6619c_274.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_276.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_278.png)

1.  **WebPack打包器**：我们了解了它的作用是将多个模块打包成少数资源文件以简化管理和优化。其核心安全点在于**打包模式的选择**，错误地使用开发模式上线会导致**前端源码完全泄露**。
2.  **第三方库（JQuery）**：我们认识到使用第三方库可以提升开发效率，但也引入了依赖风险。**使用存在已知漏洞的旧版本库**是主要威胁，可能导致XSS等攻击。维护策略包括及时更新和使用SRI。

![](img/e90e96ef59b334500d0ec0f65bc6619c_280.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_282.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_284.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_286.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_288.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_290.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_292.png)

![](img/e90e96ef59b334500d0ec0f65bc6619c_294.png)

理解这些工具和库的工作原理及安全配置，是构建安全前端应用的基础。在后续的Web安全课程中，我们将学习如何利用这些知识进行安全测试和漏洞挖掘。