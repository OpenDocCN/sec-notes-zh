# 🎯 第53课：XSS跨站攻击进阶 - SVG/PDF/Flash文件利用与MXSS/UXSS

![](img/48ad4d267c4e8464accae44b0fceb9f1_1.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_3.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_5.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_7.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_9.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_11.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_13.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_15.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_17.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_19.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_21.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_23.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_25.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_27.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_29.png)

在本节课中，我们将学习几种特殊的跨站脚本攻击方式。这些攻击通常与文件上传功能结合，利用SVG、PDF和Flash等文件格式来执行恶意JavaScript代码。我们还将了解两种较为罕见的XSS变种：MXSS和UXSS。

![](img/48ad4d267c4e8464accae44b0fceb9f1_31.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_33.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_35.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_37.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_39.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_41.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_43.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_45.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_46.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_47.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_49.png)

## 📖 课程概述

![](img/48ad4d267c4e8464accae44b0fceb9f1_51.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_53.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_55.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_57.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_59.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_61.png)

上节课我们介绍了XSS的基础分类。本节我们将深入探讨几种利用特殊文件格式的XSS攻击手法，以及两种由特定上下文触发的XSS变种。

![](img/48ad4d267c4e8464accae44b0fceb9f1_63.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_65.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_67.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_69.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_71.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_73.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_75.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_77.png)

## 🔍 SVG文件中的XSS

![](img/48ad4d267c4e8464accae44b0fceb9f1_79.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_81.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_83.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_85.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_87.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_89.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_91.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_93.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_95.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_97.png)

SVG是一种基于XML的矢量图像格式。与常见的JPG、PNG不同，SVG文件本质上是文本代码，可以被浏览器解析并渲染成图像。

![](img/48ad4d267c4e8464accae44b0fceb9f1_99.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_101.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_103.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_105.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_107.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_109.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_111.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_113.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_115.png)

由于SVG文件可以包含JavaScript代码，攻击者可以制作一个嵌入了恶意脚本的SVG文件。

![](img/48ad4d267c4e8464accae44b0fceb9f1_117.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_119.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_121.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_123.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_125.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_127.png)

以下是SVG文件中嵌入XSS代码的示例：

![](img/48ad4d267c4e8464accae44b0fceb9f1_129.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_131.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_133.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_135.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_137.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_139.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_141.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_143.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_145.png)

```xml
<svg xmlns="http://www.w3.org/2000/svg">
    <script>alert(1)</script>
</svg>
```

![](img/48ad4d267c4e8464accae44b0fceb9f1_147.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_149.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_151.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_153.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_155.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_156.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_158.png)

当这个文件被上传到服务器并获得一个可访问的URL后，任何用户访问该URL时，其中的JavaScript代码就会被执行。

![](img/48ad4d267c4e8464accae44b0fceb9f1_160.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_162.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_164.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_166.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_168.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_170.png)

**利用流程如下：**
1.  攻击者创建一个包含恶意JavaScript代码的SVG文件。
2.  通过网站的文件上传功能，将该SVG文件上传到服务器。
3.  服务器返回该文件的访问地址。
4.  诱导受害者访问该文件地址，从而触发XSS。

![](img/48ad4d267c4e8464accae44b0fceb9f1_172.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_174.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_176.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_178.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_179.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_181.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_183.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_185.png)

## 📄 PDF文件中的XSS

![](img/48ad4d267c4e8464accae44b0fceb9f1_187.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_189.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_191.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_193.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_195.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_197.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_199.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_200.png)

PDF文件同样支持嵌入JavaScript动作。攻击者可以利用PDF编辑器，在PDF文件中添加“打开文档时执行JavaScript”的动作。

![](img/48ad4d267c4e8464accae44b0fceb9f1_202.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_204.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_206.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_208.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_210.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_212.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_214.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_216.png)

**制作恶意PDF的步骤：**
1.  使用PDF编辑软件（如Adobe Acrobat）创建一个PDF文件。
2.  在文档属性或页面动作中，添加一个“打开文档”时运行的JavaScript。
3.  输入的JavaScript代码例如：`app.alert(1)`。
4.  保存PDF文件。

![](img/48ad4d267c4e8464accae44b0fceb9f1_218.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_220.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_222.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_224.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_226.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_228.png)

当受害者使用浏览器在线预览（而非直接下载）这个PDF文件时，其中嵌入的JavaScript代码就会被执行。这种手法常被用于钓鱼攻击。

![](img/48ad4d267c4e8464accae44b0fceb9f1_230.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_232.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_234.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_236.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_238.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_240.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_242.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_244.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_246.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_248.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_249.png)

## ⚡ Flash文件中的XSS

![](img/48ad4d267c4e8464accae44b0fceb9f1_251.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_253.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_255.png)

Flash文件（.swf）也支持ActionScript脚本，并能与JavaScript交互。这里有两种主要的利用方式。

### 方式一：制作恶意Flash文件并上传

![](img/48ad4d267c4e8464accae44b0fceb9f1_257.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_259.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_261.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_263.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_265.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_266.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_268.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_270.png)

攻击者可以使用Adobe Flash等工具创建一个包含恶意ActionScript代码的.swf文件。

![](img/48ad4d267c4e8464accae44b0fceb9f1_272.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_274.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_276.png)

示例ActionScript 2.0代码如下：
```actionscript
// 获取URL参数并执行
var b:String = root.loaderInfo.parameters.m;
eval(b);
```
将文件发布为.swf格式后，通过文件上传功能上传。访问该文件时，通过URL传递参数即可执行代码，例如：`http://example.com/malicious.swf?m=alert(1)`。

![](img/48ad4d267c4e8464accae44b0fceb9f1_278.png)

### 方式二：逆向分析网站现有的Flash文件

![](img/48ad4d267c4e8464accae44b0fceb9f1_280.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_282.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_284.png)

更常见的方式是，攻击者分析目标网站本身使用的.swf文件，寻找可以执行外部传入代码的漏洞点。

**利用步骤如下：**
1.  从目标网站下载其使用的.swf文件。
2.  使用反编译工具（如JPEXS Free Flash Decompiler）对文件进行反编译。
3.  在反编译的代码中搜索可能执行外部输入的关键函数，例如：
    *   `getURL`
    *   `ExternalInterface.call`
    *   `eval`
    *   `loadVariables`
4.  分析代码逻辑，找到可控的输入参数。
5.  直接通过URL向该.swf文件传入恶意参数，触发XSS。

![](img/48ad4d267c4e8464accae44b0fceb9f1_286.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_288.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_290.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_292.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_294.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_296.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_298.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_300.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_302.png)

例如，发现代码中有 `eval(root.loaderInfo.parameters.js)`，那么访问 `http://target.com/flash.swf?js=alert(1)` 即可触发弹窗。

![](img/48ad4d267c4e8464accae44b0fceb9f1_304.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_305.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_307.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_309.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_311.png)

## 🔄 突变型XSS

![](img/48ad4d267c4e8464accae44b0fceb9f1_313.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_315.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_317.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_319.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_321.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_323.png)

MXSS通常发生在内容经过HTML编码过滤后，又被页面某些特定功能（如预览、编辑）错误地解码并执行的情况下。

![](img/48ad4d267c4e8464accae44b0fceb9f1_325.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_327.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_329.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_331.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_333.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_335.png)

**一个经典场景是：**
1.  用户在博客中插入代码 `<script>alert(1)</script>`。
2.  博客系统进行了安全过滤，将其存储为 `&lt;script&gt;alert(1)&lt;/script&gt;`。
3.  但当其他用户通过某些客户端（如旧版QQ的链接预览功能）查看此博客时，预览功能错误地将实体字符解码回原始标签，导致脚本执行。

![](img/48ad4d267c4e8464accae44b0fceb9f1_337.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_339.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_341.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_343.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_344.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_345.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_347.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_349.png)

这类漏洞高度依赖于特定的客户端或功能实现，目前已较为罕见。

![](img/48ad4d267c4e8464accae44b0fceb9f1_351.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_353.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_355.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_357.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_359.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_360.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_361.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_363.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_365.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_367.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_369.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_371.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_373.png)

## 🌐 通用型XSS

![](img/48ad4d267c4e8464accae44b0fceb9f1_375.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_377.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_379.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_381.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_383.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_385.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_386.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_387.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_388.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_389.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_390.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_392.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_393.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_395.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_397.png)

UXSS是利用浏览器或浏览器扩展自身漏洞发起的XSS攻击，与存在漏洞的网站本身无关。

![](img/48ad4d267c4e8464accae44b0fceb9f1_399.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_401.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_402.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_404.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_405.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_407.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_408.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_410.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_412.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_414.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_416.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_418.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_419.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_421.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_423.png)

**一个典型案例是：**
某个版本的Microsoft Edge浏览器内置翻译功能存在漏洞。当用户使用该功能翻译一个看似无害的页面时，翻译引擎的错误处理会导致页面中本已被正确编码的脚本被激活并执行。

![](img/48ad4d267c4e8464accae44b0fceb9f1_424.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_426.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_428.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_430.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_432.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_434.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_436.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_437.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_439.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_441.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_443.png)

攻击者可以将恶意脚本写入大型网站（如社交媒体）的用户可控内容（如昵称）中，这些内容在网站上被安全过滤。但当受害者使用存在漏洞的浏览器访问并触发翻译功能时，XSS就会被触发。

![](img/48ad4d267c4e8464accae44b0fceb9f1_445.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_447.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_448.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_450.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_452.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_454.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_456.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_458.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_460.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_462.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_464.png)

## 📝 本节课总结

![](img/48ad4d267c4e8464accae44b0fceb9f1_466.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_468.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_470.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_472.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_474.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_476.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_478.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_480.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_482.png)

本节课我们一起学习了五种特殊的XSS利用方式：
1.  **SVG XSS**：通过制作并上传包含恶意脚本的SVG图像文件触发。
2.  **PDF XSS**：在PDF文档中嵌入JavaScript动作，利用浏览器在线预览触发。
3.  **Flash XSS**：可通过制作恶意文件上传，或逆向分析网站现有SWF文件找到执行点。
4.  **MXSS**：由内容过滤后的二次解析功能（如预览）触发，现已少见。
5.  **UXSS**：由浏览器或其扩展程序的漏洞导致，可激活其他网站上已被过滤的脚本。

![](img/48ad4d267c4e8464accae44b0fceb9f1_483.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_485.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_487.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_489.png)

![](img/48ad4d267c4e8464accae44b0fceb9f1_491.png)

前三种方式常与“文件上传”功能点结合，作为攻击链的一部分。后两种则依赖于非常特定的环境或已修复的客户端漏洞。理解这些手法有助于我们在安全测试中拓宽思路，识别非常规的攻击面。