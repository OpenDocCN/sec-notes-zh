# 0基础WEB安全教学：P2：第二节 前端篇（上）HTML与CSS：如何用10000原石诱惑朋友 🎣

![](img/0eaf6cc2642fb725f3ece907c075ba6b_1.png)

在本节课中，我们将学习HTML和CSS的基础知识。你将了解网页的基本结构，学会使用最核心的标签和样式属性，并最终能够制作一个简单的网页。对于安全学习而言，掌握这些基础知识已经足够。

## 概述：网页的骨架与皮肤 🦴

![](img/0eaf6cc2642fb725f3ece907c075ba6b_3.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_5.png)

一个网页主要由两部分构成：HTML负责搭建结构（骨架），CSS负责美化样式（皮肤）。我们将从最基础的HTML标签开始，逐步引入CSS来装饰我们的网页。

![](img/0eaf6cc2642fb725f3ece907c075ba6b_7.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_8.png)

## HTML基础结构

![](img/0eaf6cc2642fb725f3ece907c075ba6b_10.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_11.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_13.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_15.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_16.png)

HTML文档的基本结构是一个大的`<html>`标签，它包含两个主要部分：`<head>`和`<body>`。

![](img/0eaf6cc2642fb725f3ece907c075ba6b_18.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_20.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_22.png)

*   `<head>`标签：写在文档开头，用于定义网页的元信息，这些信息通常对用户不可见，但对浏览器至关重要。例如，可以设置网页的编码、标题等。
*   `<body>`标签：网页的主体部分，所有用户能看到的内容，如文字、图片、链接等，都写在这里。

![](img/0eaf6cc2642fb725f3ece907c075ba6b_23.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_25.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_27.png)

HTML标签通常成对出现，格式为`<标签名>内容</标签名>`。例如：`<title>看名笑</title>`。也有少数标签可以自闭合，例如`<img>`。

![](img/0eaf6cc2642fb725f3ece907c075ba6b_29.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_30.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_32.png)

## 核心HTML标签介绍

![](img/0eaf6cc2642fb725f3ece907c075ba6b_34.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_35.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_37.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_38.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_40.png)

上一节我们介绍了HTML的基本结构，本节中我们来看看构成网页内容的具体标签。以下是几个最常用且足够应对安全场景的HTML标签。

![](img/0eaf6cc2642fb725f3ece907c075ba6b_42.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_43.png)

*   **`<div>`标签**：这是一个容器标签，用于将页面内容划分为独立的区块。后续学习CSS时，可以方便地对每个`<div>`区块进行单独的样式控制，例如改变颜色或大小。
*   **`<a>`标签（链接）**：用于创建超链接。它有一个关键属性`href`，用于指定链接指向的地址。例如：`<a href=“https://www.baidu.com”>看名笑</a>`会创建一个显示为“看名笑”的链接，点击后将跳转到百度。
*   **`<input>`标签（输入框）**：用于创建用户输入区域。其`type`属性决定了输入框的类型，例如`type=“text”`是普通文本框，`type=“password”`是密码框（输入内容显示为圆点）。
*   **`<img>`标签（图片）**：用于在网页中嵌入图像。其`src`属性指定了图片来源的路径。路径可以是相对路径（如`img/logo.png`），也可以是绝对路径（如`C:\Users\Pictures\logo.png`）。
*   **`<form>`标签（表单）**：用于包裹一组输入控件（如多个`<input>`），并将用户填写的数据提交到服务器。它有两个重要属性：`action`（指定数据提交到的后端程序地址）和`method`（指定提交方式，如GET或POST）。

![](img/0eaf6cc2642fb725f3ece907c075ba6b_45.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_46.png)

## HTML标签的关键属性

![](img/0eaf6cc2642fb725f3ece907c075ba6b_48.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_50.png)

理解了标签的用途后，我们还需要知道如何配置它们。以下是所有HTML标签都可能拥有的一些通用属性，以及部分标签特有的关键属性。

*   **通用属性**：
    *   `class`：为元素定义一个或多个类名。CSS可以通过类名来批量设置同一类元素的样式。
    *   `id`：为元素指定一个唯一的标识符。CSS可以通过ID来精确地设置单个元素的样式。
*   **特定属性**：
    *   `src`：常见于`<img>`、`<script>`等标签，意为“来源”(source)，用于指定外部资源的路径。
    *   `name`：常见于`<input>`标签。当表单提交时，后端程序通过这个`name`值来识别和接收对应的用户输入数据。例如，`<input type=“text” name=“username”>`提交后，后端会收到一个名为`username`的变量及其值。
    *   `type`：用于`<input>`标签，定义输入框的类型。
    *   `action` 和 `method`：用于`<form>`标签，定义表单数据的提交目标和方式。

![](img/0eaf6cc2642fb725f3ece907c075ba6b_52.png)

## CSS基础：为网页添加样式 🎨

![](img/0eaf6cc2642fb725f3ece907c075ba6b_54.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_56.png)

现在我们的网页有了内容，但看起来可能很简陋。接下来，我们使用CSS来美化它。CSS代码通常写在`<head>`标签内的`<style>`标签中。

![](img/0eaf6cc2642fb725f3ece907c075ba6b_58.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_60.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_61.png)

CSS的基本语法是通过选择器选中HTML元素，然后为其设置样式属性和值。选择元素主要依靠我们之前提到的`class`和`id`属性。

*   **通过类选择**：在CSS中使用 `.类名` 来选中所有具有该`class`的元素。
*   **通过ID选择**：在CSS中使用 `#ID名` 来选中具有该`id`的唯一元素。

![](img/0eaf6cc2642fb725f3ece907c075ba6b_63.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_65.png)

例如：
```css
.background {
    /* 这里写样式 */
}
#kanmingxiao {
    /* 这里写样式 */
}
```

![](img/0eaf6cc2642fb725f3ece907c075ba6b_67.png)

## 核心CSS属性

CSS有非常多的属性，对于初学者，掌握以下几个最基础的属性就能实现很多效果。以下是几个控制元素位置、大小和层叠顺序的核心属性。

![](img/0eaf6cc2642fb725f3ece907c075ba6b_69.png)

*   **定位与布局**：
    *   `position`：设置元素的定位方式。
        *   `relative`：相对定位，相对于元素原本的位置进行偏移。
        *   `absolute`：绝对定位，相对于最近一个设置了定位（非`static`）的父元素进行偏移。
        *   `fixed`：固定定位，相对于浏览器窗口进行定位，滚动页面时元素位置不变。
    *   `left` / `top`：与`position`配合使用，定义元素距离左侧(`left`)或顶部(`top`)的偏移量，单位常用像素(`px`)。
*   **尺寸控制**：
    *   `width`：设置元素的宽度。
    *   `height`：设置元素的高度。
*   **层叠控制**：
    *   `z-index`：设置元素的堆叠顺序。数值越大，元素越靠前显示。默认值为0。

![](img/0eaf6cc2642fb725f3ece907c075ba6b_70.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_72.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_73.png)

## 实战演示：组合运用HTML与CSS

![](img/0eaf6cc2642fb725f3ece907c075ba6b_75.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_77.png)

理论介绍完毕，让我们看一个简单的实战例子。以下是如何利用所学知识，制作一个模仿常见登录界面的网页。

![](img/0eaf6cc2642fb725f3ece907c075ba6b_79.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_80.png)

![](img/0eaf6cc2642fb725f3ece907c075ba6b_82.png)

1.  **HTML结构**：使用`<div>`划分区域，用`<img>`设置背景和Logo，用`<form>`包裹`<input>`创建账号密码输入框。
2.  **CSS美化**：
    *   为背景`<div>`设置`position: fixed;`使其铺满全屏。
    *   为登录框`<div>`设置`width`、`height`、`background-color`（背景色）和`border-radius`（圆角）。
    *   使用`position: absolute;`配合`left`和`top`，将登录框精确放置在页面中央。
    *   为输入框设置`padding`（内边距）、`margin`（外边距）和`border`（边框）使其更美观。

通过组合这些基础的HTML标签和CSS属性，你已经可以制作出功能完整、外观不错的网页。在实际操作中，遇到不懂的样式效果（如阴影、渐变），随时可以搜索“CSS如何实现XXX”来查找代码并应用。

![](img/0eaf6cc2642fb725f3ece907c075ba6b_84.png)

## 总结

本节课中我们一起学习了WEB前端的基础——HTML和CSS。
*   **HTML**方面，我们掌握了文档的基本结构（`<html>`, `<head>`, `<body>`），以及`<div>`, `<a>`, `<input>`, `<img>`, `<form>`等核心标签及其关键属性（`class`, `id`, `src`, `name`）。
*   **CSS**方面，我们学会了如何通过`class`和`id`选择元素，并使用了`position`, `left/top`, `width/height`, `z-index`等属性来控制元素的样式和布局。

![](img/0eaf6cc2642fb725f3ece907c075ba6b_86.png)

记住，对于安全学习，目标不是成为前端专家，而是具备读懂和编写简单网页代码的能力。这足以让你理解网页的构成，并为后续学习WEB安全技术（如漏洞分析）打下坚实基础。