# 经典15年i春秋渗透测试系统化教程 - P51：课时9 编辑器漏洞讲解（中）🔍

在本节课中，我们将要学习FCKeditor这款编辑器的漏洞利用方法。FCKeditor与之前讲解的eWebEditor一样，都是使用非常广泛的在线编辑器。我们将重点分析其上传漏洞的多种利用方式，并通过实际操作演示如何突破不同版本的安全限制。

![](img/b2e8feda5004e2476b7a06e232ecf96a_1.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_3.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_5.png)

## 概述与编辑器介绍

![](img/b2e8feda5004e2476b7a06e232ecf96a_7.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_9.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_11.png)

上一节我们介绍了eWebEditor编辑器，本节中我们来看看另一款经典编辑器——FCKeditor。FCKeditor在以前是一款使用率相当高的编辑器，与eWebEditor不同的是，它通常没有后台管理界面，因此几乎不存在SQL注入、Cookie伪造等这类漏洞。FCKeditor的大部分漏洞都集中在文件上传功能上。

![](img/b2e8feda5004e2476b7a06e232ecf96a_13.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_15.png)

关于文件上传漏洞，我们在前面的课程中已经讲解过，包括利用解析漏洞和00截断等方法，这里不再赘述。我们直接进入FCKeditor的实战环节。

![](img/b2e8feda5004e2476b7a06e232ecf96a_17.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_18.png)

## 默认路径与版本识别

![](img/b2e8feda5004e2476b7a06e232ecf96a_20.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_22.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_24.png)

首先，我们需要知道FCKeditor的默认访问页面。一般情况下，编辑器会存放在网站的一个独立文件夹内，例如 `FCKeditor` 或由管理员自定义的其他名称。

![](img/b2e8feda5004e2476b7a06e232ecf96a_26.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_28.png)

以下是识别编辑器默认路径和版本的方法：

*   **默认访问页面**：通常是 `http://目标域名/FCKeditor/editor/filemanager/browser/default/browser.html` 这类路径。具体路径可能因版本而异。
*   **版本查看方法**：通过访问特定的测试页面（例如 `/_whatsnew.html`）可以查看版本信息。国内的安全扫描器通常也能很好地识别出编辑器的版本。

![](img/b2e8feda5004e2476b7a06e232ecf96a_30.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_32.png)

为了演示，我们搭建了两个不同版本的环境：FCKeditor 2.4.3 和 FCKeditor 2.6.6。了解版本号至关重要，因为不同版本的漏洞利用方式不同。

![](img/b2e8feda5004e2476b7a06e232ecf96a_34.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_35.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_37.png)

**环境配置提示**：若使用提供的源代码进行实验，需要在配置文件中将 `Config.Enabled` 的值从 `false` 改为 `true` 以启用编辑器。

![](img/b2e8feda5004e2476b7a06e232ecf96a_39.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_41.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_43.png)

## 文件上传功能分析

现在，我们来看看FCKeditor的文件上传功能界面。在编辑器界面中，点击“插入图片”或类似按钮，通常会弹出一个文件浏览对话框。

![](img/b2e8feda5004e2476b7a06e232ecf96a_45.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_47.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_49.png)

以下是文件上传相关的关键路径和功能：

*   **上传路径**：常见的上传处理路径类似 `uploader/upload.php` 或 `filemanager/upload/`。但更重要的是图形化的“浏览服务器”功能。
*   **浏览服务器**：这是一个关键功能，允许用户浏览服务器上的目录和文件。其访问路径通常为 `filemanager/browser/default/connectors/` 目录下的文件，例如用于ASP的 `connector.asp`。

![](img/b2e8feda5004e2476b7a06e232ecf96a_51.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_53.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_55.png)

我们主要关注“浏览服务器”这个页面。如果管理员没有删除此功能，我们就可以直接通过它来管理服务器上的文件。

## FCKeditor 2.4.3 版本漏洞利用

![](img/b2e8feda5004e2476b7a06e232ecf96a_57.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_59.png)

对于FCKeditor 2.4.3及类似的低版本，存在多种漏洞利用方式。

![](img/b2e8feda5004e2476b7a06e232ecf96a_61.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_63.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_64.png)

首先，直接上传`.asp`、`.php`等脚本文件会被拦截。但是，我们可以利用IIS 6.0的解析漏洞进行突破。

![](img/b2e8feda5004e2476b7a06e232ecf96a_65.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_67.png)

**利用解析漏洞**：
1.  准备一个木马文件，将其命名为 `shell.asp;.jpg`。
2.  通过上传功能上传此文件。由于扩展名包含`.jpg`，通常会被允许上传。
3.  上传后，通过“浏览服务器”或返回的路径信息获取文件地址，例如 `http://目标/upload/shell.asp;.jpg`。
4.  IIS 6.0在解析时遇到分号`;`会停止，因此实际执行的是`shell.asp`，我们的木马就被成功解析了。

![](img/b2e8feda5004e2476b7a06e232ecf96a_69.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_71.png)

其次，在2.4.3版本的“浏览服务器”界面中，我们还可以**创建特殊名称的文件夹**来利用解析漏洞。

![](img/b2e8feda5004e2476b7a06e232ecf96a_73.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_75.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_77.png)

**创建特殊文件夹**：
1.  在“浏览服务器”界面，点击“新建文件夹”。
2.  创建一个名为 `shell.asp` 的文件夹（注意，这是一个文件夹）。
3.  然后向这个 `shell.asp` 文件夹内上传一个图片木马，例如 `muma.jpg`。
4.  访问路径 `http://目标/upload/shell.asp/muma.jpg`。IIS 6.0会将 `shell.asp` 这个目录下的所有文件都当做ASP程序来解析，因此 `muma.jpg` 中的ASP代码会被执行。

![](img/b2e8feda5004e2476b7a06e232ecf96a_79.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_81.png)

这种方法非常有效，且操作简单。

![](img/b2e8feda5004e2476b7a06e232ecf96a_83.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_85.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_87.png)

## FCKeditor 2.6.6 版本漏洞利用

![](img/b2e8feda5004e2476b7a06e232ecf96a_89.png)

到了FCKeditor 2.6.6版本，上述创建特殊文件夹的漏洞已被修复。尝试创建 `shell.asp` 文件夹会被阻止。直接使用解析漏洞上传 `shell.asp;.jpg` 也会被拦截。

![](img/b2e8feda5004e2476b7a06e232ecf96a_91.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_93.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_95.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_97.png)

对于这个版本，我们需要采用**白名单绕过**结合**00截断**的方法。

![](img/b2e8feda5004e2476b7a06e232ecf96a_99.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_101.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_103.png)

**利用00截断上传**：
1.  首先，尝试上传一个正常的文本文件 `test.txt`，确认上传功能正常，并记住文件保存的路径格式。
2.  使用抓包工具（如Burp Suite）拦截上传 `test.txt` 的数据包。
3.  在数据包中，找到文件名参数。将文件名修改为 `shell.asp%00.txt`。这里的 `%00` 是空字符的URL编码形式。
4.  将修改后的数据包发送出去。
5.  如果服务器在处理路径时存在漏洞，`%00` 后的内容会被截断，最终服务器保存的文件名将是 `shell.asp`，从而绕过扩展名检查。

![](img/b2e8feda5004e2476b7a06e232ecf96a_105.png)

需要注意的是，00截断的成功与否与服务器环境（PHP版本、配置）密切相关，并非总是有效，需要在实际测试中验证。

![](img/b2e8feda5004e2476b7a06e232ecf96a_107.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_109.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_111.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_113.png)

## 其他注意事项与总结

![](img/b2e8feda5004e2476b7a06e232ecf96a_115.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_116.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_118.png)

在渗透测试过程中，我们还会遇到其他情况：

![](img/b2e8feda5004e2476b7a06e232ecf96a_120.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_122.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_124.png)

*   **路径多样性**：FCKeditor可能存在多个上传和浏览路径，如果默认路径被删，可以尝试其他路径。
*   **浏览器兼容性**：某些编辑器功能可能在特定浏览器下无法正常显示，测试时需准备多种浏览器。
*   **信息收集与总结**：面对各种各样的编辑器及其漏洞，养成良好的信息收集和整理习惯至关重要。建议建立一个自己的知识库，记录不同编辑器、不同版本的默认路径、漏洞类型和利用方法。

![](img/b2e8feda5004e2476b7a06e232ecf96a_126.png)

![](img/b2e8feda5004e2476b7a06e232ecf96a_128.png)

本节课中我们一起学习了FCKeditor编辑器的渗透测试方法。我们掌握了如何识别其版本，并重点讲解了针对2.4.3版本的**解析漏洞利用**（直接上传、创建特殊文件夹）和针对2.6.6版本的**00截断上传绕过**方法。核心在于理解不同版本的安全机制差异，并灵活运用相应的漏洞利用技术。记住，关键在于掌握方法，而非死记硬背路径，在实际测试中结合信息搜索和工具使用，才能高效地发现问题。