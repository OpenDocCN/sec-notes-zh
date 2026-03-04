# CTF夺旗赛教程：P17：文件上传漏洞与一句话木马（下）🔐

![](img/abc8beffe79f0f6df7c900d2bdff2423_1.png)

![](img/abc8beffe79f0f6df7c900d2bdff2423_3.png)

在本节课中，我们将继续学习文件上传漏洞的利用方法，重点探讨如何绕过前端和后端的双重校验机制，并成功上传一句话木马。

![](img/abc8beffe79f0f6df7c900d2bdff2423_4.png)

![](img/abc8beffe79f0f6df7c900d2bdff2423_6.png)

## 绕过前端校验

![](img/abc8beffe79f0f6df7c900d2bdff2423_8.png)

![](img/abc8beffe79f0f6df7c900d2bdff2423_10.png)

上一节我们介绍了文件上传漏洞的基本概念，本节中我们来看看如何绕过常见的校验机制。首先，许多网站会使用前端JavaScript来校验上传文件的类型，这种校验非常容易被绕过。

![](img/abc8beffe79f0f6df7c900d2bdff2423_12.png)

以下是绕过前端校验的步骤：

![](img/abc8beffe79f0f6df7c900d2bdff2423_14.png)

![](img/abc8beffe79f0f6df7c900d2bdff2423_16.png)

1.  准备一个包含一句话木马的PHP文件，例如 `shell.php`。
2.  在本地将文件后缀名临时修改为允许的格式，例如 `.jpg`。
3.  在浏览器中开启开发者工具的“网络”选项卡，并开启请求拦截功能。
4.  提交修改后的文件（如 `shell.jpg`）。
5.  当浏览器拦截到上传请求时，找到请求体中文件名对应的部分，将其从 `shell.jpg` 改回 `shell.php`。
6.  放行被拦截的请求，文件将以 `.php` 后缀上传至服务器。

**核心原理**：前端校验仅在浏览器端生效，通过拦截并修改HTTP请求包，可以完全绕过前端的限制。

## 绕过后端内容类型校验

![](img/abc8beffe79f0f6df7c900d2bdff2423_18.png)

![](img/abc8beffe79f0f6df7c900d2bdff2423_20.png)

有些服务器不仅校验文件后缀，还会校验HTTP请求头中的 `Content-Type` 字段。该字段用于标识文件内容的MIME类型。

![](img/abc8beffe79f0f6df7c900d2bdff2423_22.png)

例如，服务器可能只接受 `image/jpeg` 类型的文件。此时，即使绕过了前端校验，上传也会被后端拒绝。

以下是绕过后端 `Content-Type` 校验的步骤：

![](img/abc8beffe79f0f6df7c900d2bdff2423_24.png)

![](img/abc8beffe79f0f6df7c900d2bdff2423_26.png)

1.  准备一句话木马文件。
2.  在上传时，开启请求拦截。
3.  在拦截到的请求包中，找到 `Content-Type` 字段。
4.  将其值修改为服务器接受的类型，例如将 `application/octet-stream` 或 `text/php` 修改为 `image/jpeg`。
5.  同时，确保文件后缀名也已被修改或绕过。
6.  放行请求，完成上传。

![](img/abc8beffe79f0f6df7c900d2bdff2423_28.png)

**核心概念**：`Content-Type` 是HTTP请求头的一部分，用于告知服务器上传文件的内容类型。其格式为 `type/subtype`，例如 `image/jpeg`。

![](img/abc8beffe79f0f6df7c900d2bdff2423_30.png)

![](img/abc8beffe79f0f6df7c900d2bdff2423_32.png)

## 组合绕过策略

![](img/abc8beffe79f0f6df7c900d2bdff2423_34.png)

当网站同时存在前端和后端校验时，需要组合使用上述两种方法。

![](img/abc8beffe79f0f6df7c900d2bdff2423_36.png)

以下是组合绕过的完整流程：

![](img/abc8beffe79f0f6df7c900d2bdff2423_38.png)

1.  将木马文件后缀改为允许的格式（如 `.jpg`）以通过前端校验。
2.  开启请求拦截，提交文件。
3.  在拦截到的请求中，执行两项修改：
    *   将文件名改回 `.php`。
    *   将 `Content-Type` 修改为服务器接受的类型（如 `image/jpeg`）。
4.  放行请求，文件即可成功上传。

![](img/abc8beffe79f0f6df7c900d2bdff2423_40.png)

![](img/abc8beffe79f0f6df7c900d2bdff2423_42.png)

## 实战练习与注意事项

![](img/abc8beffe79f0f6df7c900d2bdff2423_43.png)

我们提供了一个实验环境供大家练习。访问地址为：`http://10.0.3.143/DVWA`。

![](img/abc8beffe79f0f6df7c900d2bdff2423_45.png)

在练习时，请注意以下事项：

![](img/abc8beffe79f0f6df7c900d2bdff2423_47.png)

*   建议在个人搭建的虚拟机环境中进行练习，避免干扰公共靶场。
*   上传文件时，请使用独特的文件名，例如包含自己姓名缩写的名字（如 `gld1.php`），以便区分不同用户上传的文件。

![](img/abc8beffe79f0f6df7c900d2bdff2423_49.png)

本节课中我们一起学习了如何通过修改请求包来绕过前端和后端的文件上传校验机制。关键在于理解HTTP请求的构成，并利用工具拦截和修改请求数据。掌握这些方法，你就能更有效地利用文件上传漏洞。