# 课程P6：第4天 - HTML与JavaScript前端基础 🧱

在本节课中，我们将要学习跨站脚本攻击（XSS）漏洞所依赖的前端基础知识。理解HTML和JavaScript是分析XSS漏洞的前提，因此本节课将系统性地介绍这两种核心的Web前端技术。

## 概述：Web请求与前后端

一次完整的Web请求是从浏览器发送到服务器端的过程。我们日常访问网站，就是在浏览器地址栏输入一个URL，然后向服务器发送请求。

在浏览器这一端，我们能看到并与之交互的界面，就是由前端代码构成的。例如，打开一个网页（如“和天实验室”主页），右键“查看源代码”，所看到的代码就是其前端代码。这些代码主要涉及HTML和JavaScript语言。

服务器端则可能使用PHP、Java、.NET等语言进行业务逻辑处理，并可能与数据库交互以实现数据持久化。例如，SQL注入漏洞就主要发生在与数据库交互的环节。

跨站脚本攻击（XSS）漏洞主要发生在前端，因此我们首先需要打好前端基础。

## 第一部分：HTML概述 📄

上一节我们了解了Web应用的基本架构，本节中我们来看看构成网页骨架的HTML。

### 什么是HTML？

![](img/6029b55545ce7a3305a9405db6353212_1.png)

![](img/6029b55545ce7a3305a9405db6353212_3.png)

HTML（HyperText Markup Language，超文本标记语言）是一种用于创建网页的标准标记语言。它通过一系列“标签”来标记网页中的不同部分（如标题、段落、图片），从而定义网页的结构和内容。

![](img/6029b55545ce7a3305a9405db6353212_5.png)

HTML文件是文本文件，但其后缀名为 `.html` 或 `.htm`。这种文件可以直接由浏览器打开并执行。

![](img/6029b55545ce7a3305a9405db6353212_7.png)

![](img/6029b55545ce7a3305a9405db6353212_9.png)

![](img/6029b55545ce7a3305a9405db6353212_11.png)

### HTML文档基础骨架

![](img/6029b55545ce7a3305a9405db6353212_13.png)

![](img/6029b55545ce7a3305a9405db6353212_15.png)

![](img/6029b55545ce7a3305a9405db6353212_17.png)

一个最基本的HTML文档结构如下：

![](img/6029b55545ce7a3305a9405db6353212_19.png)

![](img/6029b55545ce7a3305a9405db6353212_21.png)

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>页面标题</title>
    </head>
    <body>
        <p>这是一个段落。</p>
    </body>
</html>
```

![](img/6029b55545ce7a3305a9405db6353212_23.png)

以下是各部分说明：
*   `<!DOCTYPE html>`： 文档类型声明，告诉浏览器这是一个HTML5文档。
*   `<html>` 和 `</html>`： 根标签，包裹了整个HTML文档。
*   `<head>` 和 `</head>`： 文档头部，包含页面的元信息（如字符编码、标题），不会直接显示在页面主体中。
*   `<body>` 和 `</body>`： 文档主体，包含所有会显示在网页上的内容。

![](img/6029b55545ce7a3305a9405db6353212_25.png)

![](img/6029b55545ce7a3305a9405db6353212_27.png)

### Head 头部标签详解

![](img/6029b55545ce7a3305a9405db6353212_29.png)

![](img/6029b55545ce7a3305a9405db6353212_31.png)

![](img/6029b55545ce7a3305a9405db6353212_33.png)

![](img/6029b55545ce7a3305a9405db6353212_35.png)

头部（`<head>`）主要包含页面的元信息。以下是两个常用标签：

![](img/6029b55545ce7a3305a9405db6353212_37.png)

![](img/6029b55545ce7a3305a9405db6353212_39.png)

1.  **`<title>` 标签**： 定义浏览器标签页上显示的标题。
    ```html
    <title>和天安实验室 - 国内大型在线实验室</title>
    ```
2.  **`<meta>` 标签**： 提供关于HTML文档的元数据。最常用的功能是设置字符编码，防止中文乱码。
    ```html
    <meta charset="UTF-8">
    ```

![](img/6029b55545ce7a3305a9405db6353212_41.png)

### Body 主体常用标签

主体（`<body>`）内的标签定义了网页的可见内容。以下是部分常用标签：

*   **段落与文本**：
    `<p>` 标签定义一个段落。
    ```html
    <p>这是一个段落文本。</p>
    ```
*   **超链接**：
    `<a>` 标签定义一个超链接。`href` 属性指定链接地址，`target="_blank"` 表示在新标签页打开。
    ```html
    <a href="https://www.baidu.com" target="_blank">百度一下</a>
    ```
*   **图片**：
    `<img>` 标签用于嵌入图像。`src` 属性指定图片地址，`alt` 属性提供图片加载失败时的替代文本。
    ```html
    <img src="image.jpg" alt="图片加载失败提示">
    ```
*   **布局容器**：
    `<div>` 是块级元素，常用于布局容器。`<span>` 是行内元素，用于对文本的一部分进行样式设置。
*   **列表**：
    HTML支持有序列表、无序列表和定义列表。
    ```html
    <!-- 无序列表 -->
    <ul>
        <li>Coffee</li>
        <li>Milk</li>
    </ul>
    <!-- 有序列表 -->
    <ol>
        <li>第一步</li>
        <li>第二步</li>
    </ol>
    ```
*   **表格**：
    `<table>` 标签定义表格。`<tr>` 定义行，`<td>` 定义单元格。
    ```html
    <table border="1">
        <tr>
            <td>第一行，第一列</td>
            <td>第一行，第二列</td>
        </tr>
    </table>
    ```

![](img/6029b55545ce7a3305a9405db6353212_43.png)

### 表单（Form）与输入控件

![](img/6029b55545ce7a3305a9405db6353212_45.png)

![](img/6029b55545ce7a3305a9405db6353212_47.png)

![](img/6029b55545ce7a3305a9405db6353212_49.png)

![](img/6029b55545ce7a3305a9405db6353212_51.png)

表单用于收集用户输入，并将数据提交到服务器进行处理，例如登录功能。

![](img/6029b55545ce7a3305a9405db6353212_53.png)

![](img/6029b55545ce7a3305a9405db6353212_55.png)

![](img/6029b55545ce7a3305a9405db6353212_57.png)

以下是表单的基本结构：

![](img/6029b55545ce7a3305a9405db6353212_59.png)

![](img/6029b55545ce7a3305a9405db6353212_61.png)

```html
<form action="submit.php" method="get">
    <label for="username">用户名：</label>
    <input type="text" id="username" name="user"><br>
    <label for="pwd">密码：</label>
    <input type="password" id="pwd" name="password"><br>
    <input type="submit" value="登录">
</form>
```

![](img/6029b55545ce7a3305a9405db6353212_63.png)

![](img/6029b55545ce7a3305a9405db6353212_65.png)

*   **`<form>` 标签**： 定义表单区域。`action` 属性指定数据提交到的服务器地址，`method` 属性定义提交方法（GET或POST）。
*   **`<input>` 标签**： 定义输入控件，其形态由 `type` 属性决定。
*   **`<label>` 标签**： 为输入控件定义标签，提升用户体验。点击标签文字也能选中对应的输入框。

**GET与POST提交方式的区别**：
*   **GET**： 表单数据会附加在URL之后，格式为 `?参数1=值1&参数2=值2`。在地址栏可见，长度有限制。
*   **POST**： 表单数据被嵌入在HTTP请求的请求体中，在地址栏不可见，更适合提交敏感或大量数据。

以下是几种常见的输入控件类型：

```html
文本输入框: <input type="text" name="fname">
密码输入框: <input type="password" name="pwd">
单选按钮: <input type="radio" name="gender" value="male"> 男
复选框: <input type="checkbox" name="hobby" value="reading"> 阅读
下拉列表:
<select name="city">
    <option value="gz">广州</option>
    <option value="sz" selected>深圳</option>
</select>
提交按钮: <input type="submit" value="提交">
重置按钮: <input type="reset" value="重置">
多行文本域: <textarea name="message" rows="5" cols="30"></textarea>
```

### HTML 实体字符

![](img/6029b55545ce7a3305a9405db6353212_67.png)

在HTML中，一些字符具有特殊含义（如 `<` 和 `>` 用于定义标签），不能直接在浏览器中显示。为了安全地显示这些字符，需要使用实体字符。

例如，我们希望页面显示文本“`<h1>标签`”，而不是将其解析为标题标签。如果直接写入：
```html
<p>我是<h1>标签哦</p>
```
浏览器会将其中的 `<h1>` 解析为标题标签。为了正确显示，需要将其编码为实体字符：
```html
<p>我是&lt;h1&gt;标签哦</p>
```
*   `<` 的实体名称是 `&lt;`
*   `>` 的实体名称是 `&gt;`

实体字符有两种表示方式：实体名称（如 `&lt;`）和实体编号（如 `&#60;`）。这在XSS防御中非常重要，通过将用户输入的特殊字符转换为实体字符，可以防止其被浏览器解析为可执行的HTML代码。

## 第二部分：JavaScript概述 ⚙️

上一节我们学习了构成网页结构的HTML，本节中我们来看看让网页“动”起来的JavaScript。

JavaScript是一种在浏览器中运行的脚本语言，主要用于实现网页的动态效果和与用户的交互。它与Java没有直接关系，是一种解释型语言，代码直接嵌入在HTML中。

![](img/6029b55545ce7a3305a9405db6353212_69.png)

### JavaScript 在 HTML 中的引入方式

![](img/6029b55545ce7a3305a9405db6353212_71.png)

![](img/6029b55545ce7a3305a9405db6353212_73.png)

![](img/6029b55545ce7a3305a9405db6353212_74.png)

有三种主要方式将JavaScript代码引入HTML页面：

![](img/6029b55545ce7a3305a9405db6353212_75.png)

![](img/6029b55545ce7a3305a9405db6353212_77.png)

1.  **内嵌脚本**： 直接在 `<script>` 标签内编写代码。
    ```html
    <script>
        var test = "1234";
        alert(test); // 弹出警告框，显示“1234”
    </script>
    ```
2.  **外部脚本**： 通过 `<script>` 标签的 `src` 属性引入外部的 `.js` 文件。
    ```html
    <script src="myScript.js"></script>
    ```
3.  **事件属性**： 在HTML标签的事件属性（如 `onclick`）中直接编写少量代码。
    ```html
    <button onclick="alert('你好！')">点击我</button>
    ```

### 文档对象模型（DOM）操作

DOM（Document Object Model）是将HTML文档表示为树形结构（节点树）的标准模型。JavaScript可以通过DOM来访问、操作HTML元素。

以下是使用JavaScript操作DOM的常见示例：

*   **获取元素内容**：
    ```html
    <p id="demo">JavaScript基础</p>
    <script>
        var x = document.getElementById("demo"); // 获取id为“demo”的元素
        alert("id为demo的内容是：" + x.innerHTML); // 获取其内部HTML内容并弹出
    </script>
    ```
*   **修改元素内容**：
    ```javascript
    var x = document.getElementById("demo");
    x.innerHTML = "JavaScript真好玩！"; // 将元素内容修改为新文本
    ```
*   **动态写入内容**：
    ```javascript
    document.write("<p>当前时间是：" + Date() + "</p>"); // 向文档直接写入HTML
    ```
*   **响应用户事件**：
    ```html
    <h1 id="text">点击下方链接</h1>
    <a href="#" onclick="changeText(this)">点击这里</a>
    <script>
        function changeText(element) {
            document.getElementById("text").innerHTML = "欢迎参加Web安全工程师的学习！";
        }
    </script>
    ```
    常用事件还包括 `onmouseover`（鼠标悬停）、`onchange`（内容改变）等。

### 浏览器对象模型（BOM）操作

BOM（Browser Object Model）允许JavaScript与浏览器窗口进行交互。

![](img/6029b55545ce7a3305a9405db6353212_79.png)

![](img/6029b55545ce7a3305a9405db6353212_81.png)

![](img/6029b55545ce7a3305a9405db6353212_83.png)

以下是BOM的一些常见应用：

![](img/6029b55545ce7a3305a9405db6353212_84.png)

*   **弹出对话框**：
    ```javascript
    alert("这是一个警告框"); // 警告框，只有一个确定按钮
    confirm("确定要删除吗？"); // 确认框，有确定和取消按钮
    prompt("请输入你的名字", "默认值"); // 提示框，可输入信息
    ```
    在XSS漏洞验证中，能够弹出这些对话框是证明漏洞存在的常见方式。
*   **获取Cookie信息**：
    ```javascript
    alert(document.cookie); // 弹出当前页面的Cookie信息
    ```
    Cookie常用于身份验证，获取他人Cookie可能导致“会话劫持”。
*   **获取浏览器及页面信息**：
    ```javascript
    console.log(screen.width); // 获取屏幕宽度
    console.log(location.href); // 获取当前页面的完整URL
    ```

## 总结 🎯

本节课中我们一起学习了XSS漏洞的前端技术基础。

我们首先了解了Web请求的基本流程，区分了前端（浏览器）和后端（服务器）的角色。然后，我们系统学习了**HTML**，包括其文档结构、常用标签（特别是表单`<form>`和输入控件`<input>`）以及用于安全显示特殊字符的**实体字符**。接着，我们学习了**JavaScript**，掌握了其在HTML中的引入方式，以及如何通过**DOM**操作网页元素、通过**BOM**与浏览器交互。

![](img/6029b55545ce7a3305a9405db6353212_86.png)

![](img/6029b55545ce7a3305a9405db6353212_87.png)

理解这些前端知识是后续深入学习跨站脚本攻击（XSS）原理、利用及防御的基石。建议大家动手实践，编写简单的HTML页面并尝试用JavaScript进行交互，以巩固所学内容。