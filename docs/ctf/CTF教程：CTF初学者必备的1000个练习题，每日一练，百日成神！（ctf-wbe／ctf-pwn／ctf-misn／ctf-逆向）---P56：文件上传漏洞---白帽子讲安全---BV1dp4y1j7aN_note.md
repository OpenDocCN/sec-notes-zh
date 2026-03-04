# CTF教程：P56：文件上传漏洞

![](img/a6f2321bd7ca2ad933139ea4efe998c2_1.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_2.png)

## 概述

![](img/a6f2321bd7ca2ad933139ea4efe998c2_4.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_6.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_8.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_9.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_11.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_13.png)

在本节课中，我们将要学习文件上传漏洞的基本概念、原理、危害以及常见的绕过检测方法。通过理解服务器和客户端对上传文件的检测机制，我们可以学习如何利用这些机制的缺陷来上传恶意文件（如Webshell），从而获取服务器权限。

![](img/a6f2321bd7ca2ad933139ea4efe998c2_15.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_17.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_19.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_21.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_23.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_25.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_27.png)

---

![](img/a6f2321bd7ca2ad933139ea4efe998c2_29.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_31.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_33.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_35.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_37.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_39.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_41.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_43.png)

## 什么是文件上传

![](img/a6f2321bd7ca2ad933139ea4efe998c2_45.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_46.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_48.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_50.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_52.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_54.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_56.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_58.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_60.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_61.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_63.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_65.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_66.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_68.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_69.png)

文件上传是指客户端将数据以文件形式封装，通过网络协议（通常是HTTP协议）发送到服务器端的过程。服务器端接收并解析数据后，将其存储在服务器的硬盘上。

![](img/a6f2321bd7ca2ad933139ea4efe998c2_71.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_72.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_74.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_76.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_78.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_80.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_82.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_84.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_86.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_88.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_90.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_91.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_93.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_95.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_97.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_98.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_100.png)

文件上传通常通过HTTP协议的POST请求进行。客户端与Web服务器建立连接后，进行数据传输，最终在服务器端保存为文件。

![](img/a6f2321bd7ca2ad933139ea4efe998c2_102.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_104.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_106.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_108.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_110.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_112.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_114.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_116.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_118.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_120.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_122.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_124.png)

---

![](img/a6f2321bd7ca2ad933139ea4efe998c2_126.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_127.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_129.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_131.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_133.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_135.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_137.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_139.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_141.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_143.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_145.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_147.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_149.png)

## 文件上传漏洞产生的原因

![](img/a6f2321bd7ca2ad933139ea4efe998c2_151.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_153.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_155.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_157.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_159.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_161.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_163.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_165.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_167.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_169.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_170.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_172.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_174.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_176.png)

文件上传漏洞的产生通常源于以下几个原因：

![](img/a6f2321bd7ca2ad933139ea4efe998c2_178.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_180.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_182.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_184.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_186.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_188.png)

1.  **服务器配置不当**：服务器未对上传的文件进行有效检测。
2.  **文件上传校验被绕过**：服务端的校验逻辑存在缺陷或不完整，可以被攻击者绕过。
3.  **开源编辑器漏洞**：一些开源的文本编辑器或上传组件本身存在安全漏洞。
4.  **文件解析漏洞**：Web服务器（如Apache、IIS、Nginx）在处理特定文件时存在解析逻辑错误。
5.  **过滤不严**：对上传文件的过滤措施不严格，容易被绕过。

![](img/a6f2321bd7ca2ad933139ea4efe998c2_190.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_192.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_193.png)

---

## 文件上传漏洞的危害

![](img/a6f2321bd7ca2ad933139ea4efe998c2_195.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_197.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_199.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_201.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_203.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_205.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_207.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_208.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_210.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_212.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_214.png)

如果服务器端脚本语言没有对上传的文件进行严格的验证和过滤，攻击者就能够利用文件上传漏洞向服务器上传恶意文件（如Webshell）。通过Webshell，攻击者可以执行任意代码，从而控制整个网站或服务器，进行文件上传下载、查看数据库、执行系统命令等操作。

![](img/a6f2321bd7ca2ad933139ea4efe998c2_216.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_218.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_220.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_222.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_224.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_226.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_228.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_230.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_231.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_233.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_234.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_236.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_238.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_240.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_242.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_244.png)

---

![](img/a6f2321bd7ca2ad933139ea4efe998c2_246.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_248.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_250.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_252.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_254.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_255.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_257.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_259.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_261.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_263.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_265.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_267.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_269.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_271.png)

## 文件上传漏洞可能存在的位置

![](img/a6f2321bd7ca2ad933139ea4efe998c2_273.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_275.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_277.png)

文件上传漏洞通常出现在需要用户上传文件的功能点，例如：

*   图片上传功能（如发帖、修改头像）
*   头像上传功能
*   文档上传功能（如上传Word、Excel文件）

---

![](img/a6f2321bd7ca2ad933139ea4efe998c2_279.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_281.png)

## 文件上传的检测方式

![](img/a6f2321bd7ca2ad933139ea4efe998c2_283.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_285.png)

了解文件上传过程中的检测方式，是学习如何绕过的前提。常见的检测方式包括：

![](img/a6f2321bd7ca2ad933139ea4efe998c2_287.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_289.png)

### 1. 客户端JavaScript检测
通常在前端通过JavaScript脚本检测文件的扩展名。

![](img/a6f2321bd7ca2ad933139ea4efe998c2_291.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_293.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_295.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_297.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_299.png)

### 2. 服务端MIME类型检测
检测HTTP请求包中的 `Content-Type` 字段，判断文件类型。常见的MIME类型有：
*   `image/jpeg`
*   `image/png`
*   `image/gif`
*   `application/pdf`

![](img/a6f2321bd7ca2ad933139ea4efe998c2_301.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_303.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_305.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_307.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_309.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_311.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_313.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_315.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_317.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_319.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_321.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_323.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_325.png)

### 3. 服务端目录路径检测
检测上传文件保存的目录路径。

![](img/a6f2321bd7ca2ad933139ea4efe998c2_327.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_329.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_331.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_333.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_335.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_337.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_339.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_341.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_343.png)

### 4. 服务端文件扩展名检测
检测文件的后缀名（扩展名）。

![](img/a6f2321bd7ca2ad933139ea4efe998c2_345.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_347.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_348.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_350.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_352.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_354.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_356.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_358.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_360.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_361.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_362.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_363.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_365.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_367.png)

### 5. 服务端文件内容检测
检测文件内容的合法性，例如检查文件头（Magic Number）或进行二次渲染。

![](img/a6f2321bd7ca2ad933139ea4efe998c2_369.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_370.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_372.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_373.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_375.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_377.png)

---

![](img/a6f2321bd7ca2ad933139ea4efe998c2_379.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_381.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_383.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_385.png)

## 文件上传绕过的基本流程

![](img/a6f2321bd7ca2ad933139ea4efe998c2_387.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_389.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_391.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_392.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_394.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_396.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_398.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_400.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_402.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_404.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_406.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_407.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_409.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_411.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_413.png)

一个基本的文件上传绕过流程如下：

![](img/a6f2321bd7ca2ad933139ea4efe998c2_415.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_417.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_419.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_421.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_423.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_425.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_427.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_429.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_430.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_432.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_434.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_435.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_437.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_439.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_440.png)

1.  利用Web漏洞获取Web权限（例如通过SQL注入或直接访问上传点）。
2.  上传小马（功能较少，主要用于文件上传）。
3.  通过小马上传大马（功能丰富的Webshell）。
4.  远程调用Webshell执行命令。

![](img/a6f2321bd7ca2ad933139ea4efe998c2_442.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_444.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_445.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_447.png)

在这个过程中，我们主要会使用Burp Suite（BP）进行抓包和改包。

![](img/a6f2321bd7ca2ad933139ea4efe998c2_449.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_451.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_453.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_455.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_457.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_459.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_461.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_463.png)

---

![](img/a6f2321bd7ca2ad933139ea4efe998c2_465.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_467.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_469.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_471.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_473.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_475.png)

## 绕过客户端检测（JS检测）

![](img/a6f2321bd7ca2ad933139ea4efe998c2_477.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_479.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_481.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_483.png)

**原理**：在前端页面有专门的JavaScript代码检测上传文件的类型和扩展名。

![](img/a6f2321bd7ca2ad933139ea4efe998c2_485.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_487.png)

**绕过方法**：因为检测代码在客户端，我们可以禁用JavaScript，或者直接修改发送到服务端的请求包。

![](img/a6f2321bd7ca2ad933139ea4efe998c2_489.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_491.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_493.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_495.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_497.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_499.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_501.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_502.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_504.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_506.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_507.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_509.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_511.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_513.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_514.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_516.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_518.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_520.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_521.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_523.png)

**操作步骤**：
1.  先上传一个允许的文件类型（如图片）。
2.  用Burp Suite抓取上传请求包。
3.  在请求包中，将文件名（如 `shell.png`）修改为恶意文件名（如 `shell.php`）。
4.  将修改后的包发送给服务器。

![](img/a6f2321bd7ca2ad933139ea4efe998c2_525.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_527.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_528.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_529.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_531.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_533.png)

---

![](img/a6f2321bd7ca2ad933139ea4efe998c2_535.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_537.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_539.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_541.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_543.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_544.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_546.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_548.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_550.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_552.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_554.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_556.png)

## 绕过服务端检测

![](img/a6f2321bd7ca2ad933139ea4efe998c2_558.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_559.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_561.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_562.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_563.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_565.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_567.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_569.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_571.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_572.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_574.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_575.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_577.png)

服务端检测主要围绕三个点：MIME类型、文件后缀、文件内容。

![](img/a6f2321bd7ca2ad933139ea4efe998c2_579.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_581.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_583.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_585.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_587.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_589.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_591.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_593.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_594.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_596.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_597.png)

### 1. 绕过MIME类型检测
**原理**：服务端检测HTTP包中的 `Content-Type` 字段值。
**绕过方法**：直接使用Burp Suite修改请求包中的 `Content-Type` 字段，改为服务端允许的类型（如 `image/png`）。

![](img/a6f2321bd7ca2ad933139ea4efe998c2_599.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_600.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_602.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_604.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_606.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_608.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_610.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_612.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_614.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_616.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_618.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_620.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_622.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_624.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_626.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_627.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_628.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_630.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_631.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_633.png)

### 2. 绕过文件后缀检测（黑名单策略）
黑名单策略会列出不允许上传的后缀列表。绕过方法包括：
*   **后缀大小写绕过**：如 `pHp`、`PhP`。
*   **空格绕过**：在文件名后加空格，如 `shell.php `。Windows系统会自动去除末尾空格。
*   **点绕过**：在文件名后加点，如 `shell.php.`。Windows系统会自动去除末尾的点。
*   **特殊字符串绕过**：利用Windows特性，如 `shell.php::$DATA`。
*   **解析漏洞配合**：利用Apache解析漏洞，上传如 `shell.php.xxx` 的文件，Apache从右向左解析，不认识 `.xxx` 则会解析为 `.php`。
*   **.htaccess文件攻击**：上传一个 `.htaccess` 文件，配置将特定文件（如图片）当作PHP文件解析。

![](img/a6f2321bd7ca2ad933139ea4efe998c2_635.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_637.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_639.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_641.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_643.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_645.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_647.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_649.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_651.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_653.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_654.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_656.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_658.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_659.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_661.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_663.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_665.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_667.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_668.png)

### 3. 绕过文件后缀检测（白名单策略）
白名单策略只允许上传列表内的后缀。主要利用 **%00截断** 进行绕过。
*   **GET型%00截断**：在URL参数中，`%00` 会被解码为空字符，截断后面的字符串。例如：`save_path=shell.php%00.jpg`，实际保存为 `shell.php`。
*   **POST型%00截断**：在POST数据中，需要直接使用空字符的十六进制 `00` 进行截断。

![](img/a6f2321bd7ca2ad933139ea4efe998c2_670.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_672.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_674.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_676.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_677.png)

### 4. 绕过文件内容检测
**原理**：服务端检测文件内容，如文件头（Magic Number）。
**绕过方法**：
*   **文件头检测**：在恶意代码前添加合法的图片文件头，如GIF的 `GIF89a`。
    ```php
    GIF89a
    <?php @eval($_POST['pass']);?>
    ```
*   **文件加载/二次渲染检测**：将恶意代码插入到图片文件的注释区等空白区域，保证不破坏文件结构。对于严格的二次渲染，可能需要攻击文件加载器本身，难度较大。

![](img/a6f2321bd7ca2ad933139ea4efe998c2_679.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_681.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_683.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_685.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_687.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_689.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_691.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_693.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_695.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_697.png)

---

![](img/a6f2321bd7ca2ad933139ea4efe998c2_699.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_701.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_703.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_705.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_707.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_709.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_711.png)

## Web服务器解析漏洞

![](img/a6f2321bd7ca2ad933139ea4efe998c2_713.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_715.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_717.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_719.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_721.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_723.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_725.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_727.png)

了解一些历史悠久的Web服务器解析漏洞也有助于文件上传利用。

![](img/a6f2321bd7ca2ad933139ea4efe998c2_729.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_731.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_733.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_735.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_737.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_738.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_739.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_741.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_743.png)

*   **Apache解析漏洞**：从右向左解析后缀，遇到不可识别后缀则继续向左判断，如 `shell.php.xxx` 可能被解析为PHP文件。
*   **IIS 6.0解析漏洞**：
    *   目录解析：`/xx.asp/` 目录下的所有文件都会被当作ASP文件解析。
    *   文件解析：`shell.asp;.jpg` 会被当作 `shell.asp` 解析。
*   **IIS 7.0/7.5 & Nginx解析漏洞**：在Fast-CGI开启情况下，`shell.jpg/.php` 可能被当作PHP文件解析。
*   **Nginx %00解析漏洞**：`shell.jpg%00.php` 会被当作PHP文件解析。

![](img/a6f2321bd7ca2ad933139ea4efe998c2_745.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_747.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_749.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_751.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_753.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_755.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_757.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_759.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_761.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_763.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_765.png)

---

![](img/a6f2321bd7ca2ad933139ea4efe998c2_767.png)

## 总结

![](img/a6f2321bd7ca2ad933139ea4efe998c2_769.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_771.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_773.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_775.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_777.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_778.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_780.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_782.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_784.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_786.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_788.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_790.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_792.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_794.png)

本节课我们一起学习了文件上传漏洞的完整知识体系：

![](img/a6f2321bd7ca2ad933139ea4efe998c2_796.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_798.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_800.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_802.png)

1.  **理解漏洞**：了解了文件上传的基本流程、漏洞成因及其严重危害。
2.  **定位漏洞**：知道了漏洞常出现在图片、头像、文档上传等功能点。
3.  **分析检测**：学习了客户端（JS）和服务端（MIME、后缀、内容）的多种检测机制。
4.  **学习绕过**：掌握了针对不同检测机制的多种绕过方法，包括修改请求包、利用黑/白名单缺陷、%00截断、文件头伪造、解析漏洞利用等。
5.  **利用漏洞**：最终目的是通过绕过检测，上传Webshell（如一句话木马），从而获取服务器控制权。

![](img/a6f2321bd7ca2ad933139ea4efe998c2_804.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_806.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_808.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_810.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_812.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_814.png)

![](img/a6f2321bd7ca2ad933139ea4efe998c2_816.png)

文件上传漏洞是Web安全中常见且危害极大的漏洞。掌握其原理和绕过手法，对于CTF比赛和实际安全测试都至关重要。课后请务必动手实践，加深理解。