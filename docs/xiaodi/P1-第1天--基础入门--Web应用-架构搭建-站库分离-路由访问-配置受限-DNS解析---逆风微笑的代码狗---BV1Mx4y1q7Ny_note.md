# P1：第1天：【基础入门】- Web应用&架构搭建&站库分离&路由访问&配置受限&DNS解析 🚀

![](img/ee936a7b609552bbac906cec97a2ced8_1.png)

![](img/ee936a7b609552bbac906cec97a2ced8_3.png)

![](img/ee936a7b609552bbac906cec97a2ced8_5.png)

在本节课中，我们将要学习Web应用的基础知识，包括网站的组成、搭建架构、站库分离的概念、路由访问方式、中间件配置限制以及DNS解析的原理。这些知识是理解后续Web安全测试的基础。

![](img/ee936a7b609552bbac906cec97a2ced8_7.png)

![](img/ee936a7b609552bbac906cec97a2ced8_9.png)

![](img/ee936a7b609552bbac906cec97a2ced8_11.png)

![](img/ee936a7b609552bbac906cec97a2ced8_13.png)

![](img/ee936a7b609552bbac906cec97a2ced8_15.png)

![](img/ee936a7b609552bbac906cec97a2ced8_17.png)

## 概述

![](img/ee936a7b609552bbac906cec97a2ced8_19.png)

![](img/ee936a7b609552bbac906cec97a2ced8_21.png)

![](img/ee936a7b609552bbac906cec97a2ced8_23.png)

网站通常由四个核心部分组成：**操作系统**、**中间件**、**数据库**和**程序员编写的源码**。我们将从真实的服务器环境搭建开始，逐步解析网站的多种架构模式及其对安全测试的影响。

![](img/ee936a7b609552bbac906cec97a2ced8_25.png)

![](img/ee936a7b609552bbac906cec97a2ced8_27.png)

## 网站搭建基础

![](img/ee936a7b609552bbac906cec97a2ced8_29.png)

![](img/ee936a7b609552bbac906cec97a2ced8_31.png)

![](img/ee936a7b609552bbac906cec97a2ced8_33.png)

![](img/ee936a7b609552bbac906cec97a2ced8_35.png)

要搭建一个网站，首先需要购买云服务器和域名。购买流程在各大云服务商（如阿里云、腾讯云）的平台上完成。为了节省成本，可以选择按量付费的服务器。

![](img/ee936a7b609552bbac906cec97a2ced8_37.png)

![](img/ee936a7b609552bbac906cec97a2ced8_39.png)

购买完成后，需要在服务器上安装中间件。以Windows Server 2012为例，可以使用系统自带的IIS（Internet Information Services）作为中间件。其安装过程相对简单，可通过服务器管理器的“添加角色和功能”向导完成。

![](img/ee936a7b609552bbac906cec97a2ced8_41.png)

![](img/ee936a7b609552bbac906cec97a2ced8_43.png)

![](img/ee936a7b609552bbac906cec97a2ced8_45.png)

![](img/ee936a7b609552bbac906cec97a2ced8_47.png)

## 域名解析与子域名模式 🗺️

![](img/ee936a7b609552bbac906cec97a2ced8_49.png)

![](img/ee936a7b609552bbac906cec97a2ced8_51.png)

![](img/ee936a7b609552bbac906cec97a2ced8_53.png)

![](img/ee936a7b609552bbac906cec97a2ced8_55.png)

域名解析是将域名指向服务器IP地址的过程。在域名管理后台，可以添加A记录来实现。

![](img/ee936a7b609552bbac906cec97a2ced8_57.png)

![](img/ee936a7b609552bbac906cec97a2ced8_59.png)

![](img/ee936a7b609552bbac906cec97a2ced8_61.png)

![](img/ee936a7b609552bbac906cec97a2ced8_63.png)

![](img/ee936a7b609552bbac906cec97a2ced8_65.png)

以下是域名解析的配置示例：
```
记录类型：A
主机记录：www
记录值：8.218.70.35
```
这表示访问 `www.xiaodi123.fun` 将指向IP `8.218.70.35`。

![](img/ee936a7b609552bbac906cec97a2ced8_67.png)

![](img/ee936a7b609552bbac906cec97a2ced8_69.png)

我们可以在一个IP上绑定多个子域名（如 `www.xiaodi123.fun` 和 `xiaodi123.fun`），它们都指向同一个网站。更常见的情况是，不同的子域名可能指向服务器上完全不同的网站程序。

![](img/ee936a7b609552bbac906cec97a2ced8_71.png)

![](img/ee936a7b609552bbac906cec97a2ced8_73.png)

例如：
*   `blog.xiaodi123.fun` -> Z-Blog 博客程序
*   `oa.xiaodi123.fun` -> 通达OA 办公系统
*   `bbs.xiaodi123.fun` -> Discuz! 论坛程序

![](img/ee936a7b609552bbac906cec97a2ced8_75.png)

![](img/ee936a7b609552bbac906cec97a2ced8_77.png)

![](img/ee936a7b609552bbac906cec97a2ced8_79.png)

**安全意义**：在信息收集阶段，发现目标的子域名意味着可能发现了新的、独立的攻击面。任何一个子域名上的程序出现漏洞，都可能危及主站安全。

![](img/ee936a7b609552bbac906cec97a2ced8_81.png)

## 端口模式与目录模式 🔧

![](img/ee936a7b609552bbac906cec97a2ced8_83.png)

![](img/ee936a7b609552bbac906cec97a2ced8_85.png)

![](img/ee936a7b609552bbac906cec97a2ced8_87.png)

![](img/ee936a7b609552bbac906cec97a2ced8_89.png)

除了子域名，网站还可以通过**端口**和**目录**进行区分。

![](img/ee936a7b609552bbac906cec97a2ced8_91.png)

**端口模式**：在同一服务器的不同端口上运行不同的网站应用。
例如：
*   `xiaodi123.fun:80` -> 主站
*   `xiaodi123.fun:8080` -> 另一个独立应用

![](img/ee936a7b609552bbac906cec97a2ced8_93.png)

![](img/ee936a7b609552bbac906cec97a2ced8_95.png)

![](img/ee936a7b609552bbac906cec97a2ced8_97.png)

**目录模式**：在同一网站根目录下的不同子目录中放置不同的网站源码。
例如：
*   `xiaodi123.fun/` -> 主站程序
*   `xiaodi123.fun/blog/` -> 另一个博客程序

![](img/ee936a7b609552bbac906cec97a2ced8_99.png)

![](img/ee936a7b609552bbac906cec97a2ced8_101.png)

![](img/ee936a7b609552bbac906cec97a2ced8_103.png)

**安全意义**：端口扫描和目录爆破是信息收集的重要手段，目的就是发现这些“隐藏”的、独立的应用，从而增加攻击点。

![](img/ee936a7b609552bbac906cec97a2ced8_105.png)

![](img/ee936a7b609552bbac906cec97a2ced8_107.png)

![](img/ee936a7b609552bbac906cec97a2ced8_109.png)

![](img/ee936a7b609552bbac906cec97a2ced8_111.png)

![](img/ee936a7b609552bbac906cec97a2ced8_113.png)

![](img/ee936a7b609552bbac906cec97a2ced8_115.png)

## 源码类型与结构 📁

![](img/ee936a7b609552bbac906cec97a2ced8_116.png)

![](img/ee936a7b609552bbac906cec97a2ced8_118.png)

![](img/ee936a7b609552bbac906cec97a2ced8_120.png)

![](img/ee936a7b609552bbac906cec97a2ced8_122.png)

![](img/ee936a7b609552bbac906cec97a2ced8_124.png)

![](img/ee936a7b609552bbac906cec97a2ced8_126.png)

网站源码根据其开放程度，主要分为三类：
1.  **开源**：代码公开，可免费获取和使用（如 WordPress, Discuz!）。
2.  **商业**：需要付费购买，代码通常不公开或经过混淆。
3.  **自研**：企业或组织内部开发，代码不对外公开。

![](img/ee936a7b609552bbac906cec97a2ced8_128.png)

![](img/ee936a7b609552bbac906cec97a2ced8_130.png)

![](img/ee936a7b609552bbac906cec97a2ced8_132.png)

源码的结构也有规律可循，通过目录和文件名通常可以判断其功能：
*   `admin/`, `manage/` -> 后台管理目录
*   `database/`, `data/`, `db/` -> 数据库相关文件或配置
*   `upload/`, `files/` -> 文件上传目录
*   `config.php`, `web.config`, `application.properties` -> 配置文件

![](img/ee936a7b609552bbac906cec97a2ced8_134.png)

![](img/ee936a7b609552bbac906cec97a2ced8_136.png)

**安全意义**：能否获取到源码直接影响测试方式。拥有源码可以进行白盒审计（代码审计），更容易发现逻辑漏洞。对于加密（如PHP的ionCube、Zend Guard）或编译型语言（如Java的.class文件，.NET的.dll文件）的源码，需要先进行反编译才能分析。

![](img/ee936a7b609552bbac906cec97a2ced8_138.png)

![](img/ee936a7b609552bbac906cec97a2ced8_140.png)

![](img/ee936a7b609552bbac906cec97a2ced8_142.png)

![](img/ee936a7b609552bbac906cec97a2ced8_144.png)

![](img/ee936a7b609552bbac906cec97a2ced8_146.png)

![](img/ee936a7b609552bbac906cec97a2ced8_148.png)

![](img/ee936a7b609552bbac906cec97a2ced8_150.png)

## 站库分离与数据库角色 🗃️

![](img/ee936a7b609552bbac906cec97a2ced8_152.png)

![](img/ee936a7b609552bbac906cec97a2ced8_154.png)

![](img/ee936a7b609552bbac906cec97a2ced8_156.png)

![](img/ee936a7b609552bbac906cec97a2ced8_158.png)

![](img/ee936a7b609552bbac906cec97a2ced8_160.png)

“站库分离”是指网站服务器（存放源码）和数据库服务器（存放数据）部署在不同的机器上。

![](img/ee936a7b609552bbac906cec97a2ced8_162.png)

![](img/ee936a7b609552bbac906cec97a2ced8_164.png)

![](img/ee936a7b609552bbac906cec97a2ced8_166.png)

![](img/ee936a7b609552bbac906cec97a2ced8_168.png)

![](img/ee936a7b609552bbac906cec97a2ced8_170.png)

![](img/ee936a7b609552bbac906cec97a2ced8_172.png)

**传统模式**：网站和数据库在同一台服务器。
**站库分离模式**：网站服务器通过配置文件中的数据库连接信息（IP、端口、用户名、密码）远程访问另一台数据库服务器。
**云数据库模式**：直接使用云服务商提供的数据库服务（如RDS），管理更便捷，安全策略（如IP白名单）通常更严格。

![](img/ee936a7b609552bbac906cec97a2ced8_174.png)

![](img/ee936a7b609552bbac906cec97a2ced8_176.png)

![](img/ee936a7b609552bbac906cec97a2ced8_178.png)

![](img/ee936a7b609552bbac906cec97a2ced8_180.png)

数据库配置文件通常包含以下关键信息：
```xml
<!-- 示例：.NET 配置文件中的数据库连接字符串 -->
<connectionStrings>
    <add name="DefaultConnection" connectionString="Server=8.218.72.88;Database=MyCmsDB;User Id=sa;Password=YourPassword;" />
</connectionStrings>
```

![](img/ee936a7b609552bbac906cec97a2ced8_182.png)

![](img/ee936a7b609552bbac906cec97a2ced8_184.png)

![](img/ee936a7b609552bbac906cec97a2ced8_186.png)

![](img/ee936a7b609552bbac906cec97a2ced8_188.png)

**安全意义**：站库分离提高了攻击门槛。即使获取了网站权限，要直接访问或控制数据库服务器也需要突破额外的网络边界或认证限制。云数据库服务的安全策略可能使传统的数据库攻击手段失效。

![](img/ee936a7b609552bbac906cec97a2ced8_190.png)

![](img/ee936a7b609552bbac906cec97a2ced8_192.png)

## 中间件配置与安全限制 ⚙️

![](img/ee936a7b609552bbac906cec97a2ced8_194.png)

![](img/ee936a7b609552bbac906cec97a2ced8_196.png)

![](img/ee936a7b609552bbac906cec97a2ced8_198.png)

![](img/ee936a7b609552bbac906cec97a2ced8_200.png)

![](img/ee936a7b609552bbac906cec97a2ced8_202.png)

![](img/ee936a7b609552bbac906cec97a2ced8_204.png)

中间件（如IIS, Apache, Nginx）的配置会直接影响网站的安全性和可测试性。

![](img/ee936a7b609552bbac906cec97a2ced8_206.png)

以下是几种常见的配置及其影响：

![](img/ee936a7b609552bbac906cec97a2ced8_208.png)

![](img/ee936a7b609552bbac906cec97a2ced8_210.png)

1.  **目录权限**：可以设置某个目录（如图片目录`/upload/`）禁止执行脚本。即使攻击者上传了Webshell到该目录，也无法被执行。
    *   **配置项**：`执行权限` -> `无`

2.  **身份验证**：可以为特定目录启用身份验证（如Windows身份验证），要求访问者提供系统账号密码。
    *   **配置项**：`身份验证` -> `启用Windows身份验证`
    *   **影响**：未授权的用户无法访问该目录，可能阻断测试路径。

![](img/ee936a7b609552bbac906cec97a2ced8_212.png)

![](img/ee936a7b609552bbac906cec97a2ced8_214.png)

![](img/ee936a7b609552bbac906cec97a2ced8_216.png)

![](img/ee936a7b609552bbac906cec97a2ced8_217.png)

![](img/ee936a7b609552bbac906cec97a2ced8_219.png)

3.  **MIME类型/处理程序映射**：决定了不同扩展名的文件如何被服务器处理。例如，将`.asp`文件映射为纯文本，则即使访问该文件，其中的代码也不会被执行。
    *   **影响**：可能用于防御webshell，也可能因配置错误导致安全问题。

![](img/ee936a7b609552bbac906cec97a2ced8_221.png)

![](img/ee936a7b609552bbac906cec97a2ced8_223.png)

![](img/ee936a7b609552bbac906cec97a2ced8_225.png)

![](img/ee936a7b609552bbac906cec97a2ced8_227.png)

![](img/ee936a7b609552bbac906cec97a2ced8_229.png)

## 路由访问与路径解析 🛣️

![](img/ee936a7b609552bbac906cec97a2ced8_231.png)

![](img/ee936a7b609552bbac906cec97a2ced8_233.png)

![](img/ee936a7b609552bbac906cec97a2ced8_235.png)

![](img/ee936a7b609552bbac906cec97a2ced8_237.png)

![](img/ee936a7b609552bbac906cec97a2ced8_239.png)

![](img/ee936a7b609552bbac906cec97a2ced8_241.png)

![](img/ee936a7b609552bbac906cec97a2ced8_243.png)

![](img/ee936a7b609552bbac906cec97a2ced8_245.png)

大多数网站的访问路径与服务器上的文件目录是一一对应的（**常规访问**）。例如，访问 `http://xiaodi123.fun/post/1.html` 对应服务器上 `网站根目录/post/1.html` 这个文件。

![](img/ee936a7b609552bbac906cec97a2ced8_247.png)

![](img/ee936a7b609552bbac906cec97a2ced8_249.png)

![](img/ee936a7b609552bbac906cec97a2ced8_251.png)

![](img/ee936a7b609552bbac906cec97a2ced8_253.png)

![](img/ee936a7b609552bbac906cec97a2ced8_255.png)

![](img/ee936a7b609552bbac906cec97a2ced8_257.png)

然而，现代Web框架（如Spring MVC, Django, Laravel）广泛使用**路由（Routing）** 机制。访问的URL由路由规则解析，并不直接对应物理文件。

![](img/ee936a7b609552bbac906cec97a2ced8_259.png)

![](img/ee936a7b609552bbac906cec97a2ced8_261.png)

![](img/ee936a7b609552bbac906cec97a2ced8_263.png)

![](img/ee936a7b609552bbac906cec97a2ced8_265.png)

例如，URL `http://example.com/user/profile/123` 可能被路由到 `UserController` 的 `profile` 方法，并传入参数 `123`，而服务器上根本不存在 `/user/profile/123` 这个目录或文件。

![](img/ee936a7b609552bbac906cec97a2ced8_267.png)

![](img/ee936a7b609552bbac906cec97a2ced8_269.png)

![](img/ee936a7b609552bbac906cec97a2ced8_271.png)

![](img/ee936a7b609552bbac906cec97a2ced8_273.png)

![](img/ee936a7b609552bbac906cec97a2ced8_275.png)

![](img/ee936a7b609552bbac906cec97a2ced8_277.png)

![](img/ee936a7b609552bbac906cec97a2ced8_279.png)

![](img/ee936a7b609552bbac906cec97a2ced8_281.png)

![](img/ee936a7b609552bbac906cec97a2ced8_283.png)

**安全意义**：理解路由机制对于漏洞利用至关重要。例如，一个文件上传漏洞的利用点，可能不是通过直接访问上传后的文件路径触发，而是需要通过某个特定的控制器路由来访问。

![](img/ee936a7b609552bbac906cec97a2ced8_285.png)

![](img/ee936a7b609552bbac906cec97a2ced8_287.png)

![](img/ee936a7b609552bbac906cec97a2ced8_289.png)

## 总结

![](img/ee936a7b609552bbac906cec97a2ced8_290.png)

本节课我们一起学习了Web应用的基础架构知识。

![](img/ee936a7b609552bbac906cec97a2ced8_292.png)

![](img/ee936a7b609552bbac906cec97a2ced8_294.png)

![](img/ee936a7b609552bbac906cec97a2ced8_296.png)

![](img/ee936a7b609552bbac906cec97a2ced8_298.png)

![](img/ee936a7b609552bbac906cec97a2ced8_300.png)

我们了解了网站由操作系统、中间件、数据库和源码构成。探讨了通过**子域名**、**端口**、**目录**来部署多个独立网站的模式，这些是信息收集阶段的关键目标。

![](img/ee936a7b609552bbac906cec97a2ced8_302.png)

![](img/ee936a7b609552bbac906cec97a2ced8_304.png)

![](img/ee936a7b609552bbac906cec97a2ced8_306.png)

![](img/ee936a7b609552bbac906cec97a2ced8_308.png)

![](img/ee936a7b609552bbac906cec97a2ced8_310.png)

![](img/ee936a7b609552bbac906cec97a2ced8_312.png)

![](img/ee936a7b609552bbac906cec97a2ced8_314.png)

我们分析了**源码**的三种类型（开源、商业、自研）及其对安全测试的影响，认识了源码的基本结构。

![](img/ee936a7b609552bbac906cec97a2ced8_316.png)

![](img/ee936a7b609552bbac906cec97a2ced8_318.png)

我们重点讲解了**站库分离**的概念及其安全意义，以及数据库配置文件的常见位置和内容。

![](img/ee936a7b609552bbac906cec97a2ced8_320.png)

我们还学习了**中间件配置**（如权限、身份验证、MIME类型）如何直接产生安全限制，影响渗透测试的进行。

![](img/ee936a7b609552bbac906cec97a2ced8_322.png)

![](img/ee936a7b609552bbac906cec97a2ced8_324.png)

![](img/ee936a7b609552bbac906cec97a2ced8_326.png)

![](img/ee936a7b609552bbac906cec97a2ced8_328.png)

![](img/ee936a7b609552bbac906cec97a2ced8_330.png)

![](img/ee936a7b609552bbac906cec97a2ced8_332.png)

![](img/ee936a7b609552bbac906cec97a2ced8_334.png)

![](img/ee936a7b609552bbac906cec97a2ced8_336.png)

最后，我们对比了**常规路径访问**和基于**路由**的访问方式，后者在现代Web应用中非常普遍。

![](img/ee936a7b609552bbac906cec97a2ced8_338.png)

![](img/ee936a7b609552bbac906cec97a2ced8_340.png)

![](img/ee936a7b609552bbac906cec97a2ced8_342.png)

![](img/ee936a7b609552bbac906cec97a2ced8_343.png)

![](img/ee936a7b609552bbac906cec97a2ced8_345.png)

![](img/ee936a7b609552bbac906cec97a2ced8_347.png)

![](img/ee936a7b609552bbac906cec97a2ced8_349.png)

理解这些基础架构和配置，是后续分析Web安全漏洞、制定渗透测试策略的基石。下节课我们将探讨其他搭建模式（如集成软件、Docker容器）带来的不同安全考量。