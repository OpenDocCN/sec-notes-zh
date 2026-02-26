#  002：第2天 - Web架构与建站方式详解

![](img/67a86e9ea94c69769dae42399aff995c_1.png)

![](img/67a86e9ea94c69769dae42399aff995c_3.png)

![](img/67a86e9ea94c69769dae42399aff995c_5.png)

![](img/67a86e9ea94c69769dae42399aff995c_7.png)

![](img/67a86e9ea94c69769dae42399aff995c_9.png)

在本节课中，我们将要学习Web应用的不同架构和建站方式，并重点分析它们对安全测试思路和结果的影响。理解这些差异是成为一名合格安全测试人员的基础。

## 📚 课程概述

上节课我们介绍了常规化搭建（源码与数据库在同一服务器）和站库分离（源码与数据库在不同服务器）的原理及影响。本节中我们来看看其他几种常见的Web架构和建站方式。

![](img/67a86e9ea94c69769dae42399aff995c_11.png)

## 🔗 前后端分离架构

![](img/67a86e9ea94c69769dae42399aff995c_13.png)

![](img/67a86e9ea94c69769dae42399aff995c_15.png)

![](img/67a86e9ea94c69769dae42399aff995c_17.png)

前后端分离是一种现代Web开发架构。它的原理是：**前端界面（用户看到的页面）和后端逻辑（数据处理与业务管理）完全独立开发、部署和运行**。

![](img/67a86e9ea94c69769dae42399aff995c_19.png)

![](img/67a86e9ea94c69769dae42399aff995c_20.png)

![](img/67a86e9ea94c69769dae42399aff995c_22.png)

![](img/67a86e9ea94c69769dae42399aff995c_24.png)

![](img/67a86e9ea94c69769dae42399aff995c_26.png)

![](img/67a86e9ea94c69769dae42399aff995c_27.png)

![](img/67a86e9ea94c69769dae42399aff995c_29.png)

![](img/67a86e9ea94c69769dae42399aff995c_31.png)

![](img/67a86e9ea94c69769dae42399aff995c_33.png)

![](img/67a86e9ea94c69769dae42399aff995c_34.png)

*   **前端**：通常使用Vue.js、React等JavaScript框架开发，负责展示和用户交互。数据通过API接口从后端获取。
*   **后端**：使用PHP、Java、Python等后端语言开发，提供API接口，负责业务逻辑、数据处理和数据库交互。
*   **通信方式**：前后端通过**API接口**（通常是RESTful API）进行数据交换，格式常为JSON。

![](img/67a86e9ea94c69769dae42399aff995c_35.png)

![](img/67a86e9ea94c69769dae42399aff995c_37.png)

![](img/67a86e9ea94c69769dae42399aff995c_39.png)

![](img/67a86e9ea94c69769dae42399aff995c_41.png)

![](img/67a86e9ea94c69769dae42399aff995c_43.png)

![](img/67a86e9ea94c69769dae42399aff995c_45.png)

![](img/67a86e9ea94c69769dae42399aff995c_47.png)

![](img/67a86e9ea94c69769dae42399aff995c_49.png)

![](img/67a86e9ea94c69769dae42399aff995c_50.png)

![](img/67a86e9ea94c69769dae42399aff995c_52.png)

**核心概念公式**：
`前端 (JS框架) <-- API (JSON/XML) --> 后端 (PHP/Java/Python等)`

![](img/67a86e9ea94c69769dae42399aff995c_54.png)

![](img/67a86e9ea94c69769dae42399aff995c_56.png)

![](img/67a86e9ea94c69769dae42399aff995c_58.png)

![](img/67a86e9ea94c69769dae42399aff995c_60.png)

### 🔍 对安全测试的影响

![](img/67a86e9ea94c69769dae42399aff995c_62.png)

![](img/67a86e9ea94c69769dae42399aff995c_64.png)

![](img/67a86e9ea94c69769dae42399aff995c_66.png)

![](img/67a86e9ea94c69769dae42399aff995c_68.png)

![](img/67a86e9ea94c69769dae42399aff995c_70.png)

![](img/67a86e9ea94c69769dae42399aff995c_72.png)

![](img/67a86e9ea94c69769dae42399aff995c_74.png)

![](img/67a86e9ea94c69769dae42399aff995c_76.png)

![](img/67a86e9ea94c69769dae42399aff995c_78.png)

以下是针对前后端分离架构进行安全测试时需要注意的几点：

![](img/67a86e9ea94c69769dae42399aff995c_80.png)

![](img/67a86e9ea94c69769dae42399aff995c_82.png)

![](img/67a86e9ea94c69769dae42399aff995c_84.png)

![](img/67a86e9ea94c69769dae42399aff995c_86.png)

![](img/67a86e9ea94c69769dae42399aff995c_88.png)

1.  **前端漏洞减少**：由于前端页面主要由HTML和JS渲染，不直接处理业务逻辑和数据库操作，因此传统的SQL注入、文件上传等漏洞点大幅减少。
2.  **后台地址隐蔽**：后端管理地址可能与前端主站域名完全不同（例如，`admin.another-domain.com`），传统的目录扫描方法可能无法发现真实后台。
3.  **攻击面转移**：安全测试的重点应放在**后端API接口**上。漏洞可能存在于API的身份验证、参数处理、业务逻辑等方面。即使通过前端漏洞获取了部分权限，也可能无法直接影响后端服务器或数据库。

![](img/67a86e9ea94c69769dae42399aff995c_90.png)

![](img/67a86e9ea94c69769dae42399aff995c_92.png)

![](img/67a86e9ea94c69769dae42399aff995c_94.png)

![](img/67a86e9ea94c69769dae42399aff995c_96.png)

![](img/67a86e9ea94c69769dae42399aff995c_97.png)

![](img/67a86e9ea94c69769dae42399aff995c_99.png)

![](img/67a86e9ea94c69769dae42399aff995c_101.png)

![](img/67a86e9ea94c69769dae42399aff995c_103.png)

![](img/67a86e9ea94c69769dae42399aff995c_105.png)

![](img/67a86e9ea94c69769dae42399aff995c_107.png)

**识别方法**：观察网站页面是否非常简洁、交互流畅，且URL变化时页面主体是局部刷新而非整体跳转（单页应用特性）。后期信息收集时，需关注API域名和可能的子域名。

![](img/67a86e9ea94c69769dae42399aff995c_109.png)

![](img/67a86e9ea94c69769dae42399aff995c_111.png)

![](img/67a86e9ea94c69769dae42399aff995c_113.png)

![](img/67a86e9ea94c69769dae42399aff995c_115.png)

![](img/67a86e9ea94c69769dae42399aff995c_117.png)

![](img/67a86e9ea94c69769dae42399aff995c_119.png)

![](img/67a86e9ea94c69769dae42399aff995c_121.png)

## 🧩 集成软件建站（如宝塔、PHPStudy）

![](img/67a86e9ea94c69769dae42399aff995c_123.png)

![](img/67a86e9ea94c69769dae42399aff995c_125.png)

![](img/67a86e9ea94c69769dae42399aff995c_127.png)

![](img/67a86e9ea94c69769dae42399aff995c_129.png)

![](img/67a86e9ea94c69769dae42399aff995c_131.png)

![](img/67a86e9ea94c69769dae42399aff995c_133.png)

![](img/67a86e9ea94c69769dae42399aff995c_135.png)

![](img/67a86e9ea94c69769dae42399aff995c_137.png)

这类方式使用集成的环境软件（如宝塔面板、PHPStudy）来快速搭建网站环境。其原理是：**软件将Web服务器（如Apache/Nginx）、数据库（如MySQL）、编程语言环境（如PHP）等打包，提供图形化界面进行一键安装和管理**。

![](img/67a86e9ea94c69769dae42399aff995c_139.png)

![](img/67a86e9ea94c69769dae42399aff995c_141.png)

![](img/67a86e9ea94c69769dae42399aff995c_143.png)

![](img/67a86e9ea94c69769dae42399aff995c_145.png)

![](img/67a86e9ea94c69769dae42399aff995c_147.png)

![](img/67a86e9ea94c69769dae42399aff995c_149.png)

![](img/67a86e9ea94c69769dae42399aff995c_151.png)

![](img/67a86e9ea94c69769dae42399aff995c_153.png)

### ⚖️ 不同集成软件的安全差异

![](img/67a86e9ea94c69769dae42399aff995c_155.png)

![](img/67a86e9ea94c69769dae42399aff995c_157.png)

![](img/67a86e9ea94c69769dae42399aff995c_159.png)

![](img/67a86e9ea94c69769dae42399aff995c_161.png)

![](img/67a86e9ea94c69769dae42399aff995c_163.png)

![](img/67a86e9ea94c69769dae42399aff995c_165.png)

我们通过实验对比了不同搭建方式下，获取WebShell权限后的差异：

![](img/67a86e9ea94c69769dae42399aff995c_167.png)

![](img/67a86e9ea94c69769dae42399aff995c_169.png)

![](img/67a86e9ea94c69769dae42399aff995c_171.png)

| 建站方式 | 命令执行能力 | 文件目录访问权限 | 默认用户权限 | 安全性分析 |
| :--- | :--- | :--- | :--- | :--- |
| **宝塔面板搭建** | **通常被禁用** | **仅限网站目录** | 较低权限用户 | 软件自身做了较多安全加固，限制了WebShell的权限。 |
| **PHPStudy搭建** | **通常可以执行** | **可访问服务器其他目录** | 较高权限用户（如Administrator） | 默认安全设置较弱，获取WebShell后权限较高。 |
| **手动安装环境（常规）** | **取决于配置** | **取决于配置** | 取决于Web服务运行账户 | 安全性完全由搭建者控制，可能很强也可能很弱。 |

![](img/67a86e9ea94c69769dae42399aff995c_173.png)

![](img/67a86e9ea94c69769dae42399aff995c_175.png)

![](img/67a86e9ea94c69769dae42399aff995c_177.png)

**核心结论**：使用不同的集成软件或手动搭建，会导致网站运行在**不同的权限上下文**中，这直接影响了攻击者获取WebShell后能做什么。宝塔等较新的面板软件往往内置了更多安全限制。

![](img/67a86e9ea94c69769dae42399aff995c_179.png)

![](img/67a86e9ea94c69769dae42399aff995c_181.png)

![](img/67a86e9ea94c69769dae42399aff995c_183.png)

![](img/67a86e9ea94c69769dae42399aff995c_184.png)

![](img/67a86e9ea94c69769dae42399aff995c_186.png)

![](img/67a86e9ea94c69769dae42399aff995c_188.png)

![](img/67a86e9ea94c69769dae42399aff995c_190.png)

## 🐳 Docker容器化部署

![](img/67a86e9ea94c69769dae42399aff995c_192.png)

![](img/67a86e9ea94c69769dae42399aff995c_194.png)

![](img/67a86e9ea94c69769dae42399aff995c_196.png)

![](img/67a86e9ea94c69769dae42399aff995c_198.png)

![](img/67a86e9ea94c69769dae42399aff995c_200.png)

![](img/67a86e9ea94c69769dae42399aff995c_202.png)

![](img/67a86e9ea94c69769dae42399aff995c_204.png)

![](img/67a86e9ea94c69769dae42399aff995c_206.png)

Docker是一种容器化技术。其原理是：**将应用及其依赖环境打包成一个独立的镜像，然后在隔离的“容器”中运行。每个容器都是宿主机系统中的一个轻量级、隔离的进程空间**。

![](img/67a86e9ea94c69769dae42399aff995c_208.png)

![](img/67a86e9ea94c69769dae42399aff995c_210.png)

![](img/67a86e9ea94c69769dae42399aff995c_212.png)

![](img/67a86e9ea94c69769dae42399aff995c_214.png)

![](img/67a86e9ea94c69769dae42399aff995c_216.png)

![](img/67a86e9ea94c69769dae42399aff995c_218.png)

**核心概念代码**（拉取并运行一个Tomcat容器）：
```bash
docker pull tomcat:8.0
docker run -d -p 8080:8080 --name my_tomcat tomcat:8.0
```

![](img/67a86e9ea94c69769dae42399aff995c_220.png)

![](img/67a86e9ea94c69769dae42399aff995c_222.png)

![](img/67a86e9ea94c69769dae42399aff995c_224.png)

![](img/67a86e9ea94c69769dae42399aff995c_226.png)

### 🎭 对安全测试的独特影响：容器逃逸

![](img/67a86e9ea94c69769dae42399aff995c_228.png)

![](img/67a86e9ea94c69769dae42399aff995c_230.png)

当攻击者成功入侵一个Docker容器内的应用时：

1.  **权限局限**：攻击者获得的权限被限制在**当前容器内部**。他看到的文件系统、进程列表都是容器内部的，与宿主机隔离。
2.  **影响范围**：即使拿到容器内的root权限，也无法直接访问宿主机上的其他应用或敏感文件。
3.  **新的攻击路径**：安全测试的目标可能从“获取服务器权限”转变为“实现**容器逃逸**”，即突破容器隔离，获取宿主机权限。这需要利用Docker或宿主机系统的特定漏洞。

![](img/67a86e9ea94c69769dae42399aff995c_232.png)

![](img/67a86e9ea94c69769dae42399aff995c_234.png)

**简单理解**：攻击者被困在了一个精心布置的“房间”（容器）里，虽然他可能完全控制了这个房间，但想进入整栋大楼（宿主机）还需要找到特别的“门”或“漏洞”。

![](img/67a86e9ea94c69769dae42399aff995c_236.png)

![](img/67a86e9ea94c69769dae42399aff995c_238.png)

## 🌐 建站平台/分配站

![](img/67a86e9ea94c69769dae42399aff995c_240.png)

![](img/67a86e9ea94c69769dae42399aff995c_242.png)

![](img/67a86e9ea94c69769dae42399aff995c_244.png)

有些网站并非自己购买服务器和域名搭建，而是通过第三方**建站平台**（如凡科建站）申请获得。其原理是：**用户在建站平台上注册，平台分配一个子域名或允许绑定自有域名，用户通过平台提供的模板和工具在线设计、管理网站，但底层技术和服务器完全由平台控制**。

![](img/67a86e9ea94c69769dae42399aff995c_246.png)

### 🎯 对安全测试的关键影响

![](img/67a86e9ea94c69769dae42399aff995c_248.png)

![](img/67a86e9ea94c69769dae42399aff995c_250.png)

![](img/67a86e9ea94c69769dae42399aff995c_252.png)

![](img/67a86e9ea94c69769dae42399aff995c_254.png)

![](img/67a86e9ea94c69769dae42399aff995c_256.png)

![](img/67a86e9ea94c69769dae42399aff995c_257.png)

![](img/67a86e9ea94c69769dae42399aff995c_259.png)

1.  **资产归属混淆**：你测试的`company.example-platform.com`这个网站，其真实资产所有者是**建站平台**，而非`company`本身。攻击成功影响的是平台及其所有用户。
2.  **测试目标偏差**：针对这类站点的渗透测试，实际上是在测试**建站平台的安全性**。如果目标是某个具体公司，那么在此类站点上花费精力可能是徒劳的。
3.  **信息收集关键**：通过**域名Whois查询、备案信息**等手段，对比网站宣称的主体和域名注册/备案主体，可以快速识别出此类“分配站”。

![](img/67a86e9ea94c69769dae42399aff995c_261.png)

![](img/67a86e9ea94c69769dae42399aff995c_263.png)

![](img/67a86e9ea94c69769dae42399aff995c_265.png)

![](img/67a86e9ea94c69769dae42399aff995c_267.png)

![](img/67a86e9ea94c69769dae42399aff995c_269.png)

![](img/67a86e9ea94c69769dae42399aff995c_271.png)

![](img/67a86e9ea94c69769dae42399aff995c_273.png)

![](img/67a86e9ea94c69769dae42399aff995c_275.png)

![](img/67a86e9ea94c69769dae42399aff995c_276.png)

![](img/67a86e9ea94c69769dae42399aff995c_277.png)

![](img/67a86e9ea94c69769dae42399aff995c_278.png)

![](img/67a86e9ea94c69769dae42399aff995c_279.png)

## 📝 课程总结

![](img/67a86e9ea94c69769dae42399aff995c_281.png)

![](img/67a86e9ea94c69769dae42399aff995c_283.png)

![](img/67a86e9ea94c69769dae42399aff995c_284.png)

![](img/67a86e9ea94c69769dae42399aff995c_286.png)

![](img/67a86e9ea94c69769dae42399aff995c_287.png)

![](img/67a86e9ea94c69769dae42399aff995c_288.png)

![](img/67a86e9ea94c69769dae42399aff995c_290.png)

本节课我们一起学习了四种重要的Web架构与建站方式：
1.  **前后端分离**：重点测试后端API，前台漏洞少，后台地址可能隐蔽。
2.  **集成软件建站**：不同软件（宝塔/PHPStudy）的默认安全配置差异巨大，直接影响渗透深度。
3.  **Docker容器化**：攻击被限制在容器内，需关注容器逃逸漏洞。
4.  **建站平台分配站**：需识别资产真实归属，避免测试目标错误。

![](img/67a86e9ea94c69769dae42399aff995c_291.png)

![](img/67a86e9ea94c69769dae42399aff995c_293.png)

![](img/67a86e9ea94c69769dae42399aff995c_295.png)

![](img/67a86e9ea94c69769dae42399aff995c_297.png)

![](img/67a86e9ea94c69769dae42399aff995c_299.png)

理解目标网站的架构是制定正确测试策略的第一步。不同的架构决定了漏洞可能出现的位点、攻击的影响范围以及防御的难点。在后续的实际信息收集和漏洞探测中，务必首先尝试判断目标的架构类型。