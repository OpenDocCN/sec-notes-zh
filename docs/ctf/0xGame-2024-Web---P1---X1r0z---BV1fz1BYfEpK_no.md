# 0xGame 2024 Web 入门 & 环境配置：P1：环境配置与入门指南 🛠️

![](img/a4873707deffb0728a3834e0ec2950b9_0.png)

在本节课中，我们将学习如何为CTF Web方向的学习和解题配置基础环境，并了解Web安全入门的基本路径。内容涵盖必备工具安装、编程语言环境配置以及初步的学习方法。

![](img/a4873707deffb0728a3834e0ec2950b9_2.png)

## 概述 📋

![](img/a4873707deffb0728a3834e0ec2950b9_4.png)

Web安全是CTF竞赛中的重要方向，其核心是发现并利用网站中存在的漏洞。对于初学者而言，搭建一个顺手的工具环境是第一步。本节将系统性地介绍从零开始需要安装的各类软件、工具和插件，并简要说明Web安全的学习框架。

## 学习资源与维基介绍 📚

![](img/a4873707deffb0728a3834e0ec2950b9_6.png)

![](img/a4873707deffb0728a3834e0ec2950b9_8.png)

首先推荐的是我们战队的维基（Wiki），其中包含了战队介绍、招新要求以及一个名为“Introduction”的分类。该分类为零基础新手提供了必要的信息。

![](img/a4873707deffb0728a3834e0ec2950b9_10.png)

![](img/a4873707deffb0728a3834e0ec2950b9_12.png)

![](img/a4873707deffb0728a3834e0ec2950b9_14.png)

以下是该维基包含的主要内容列表：
*   **基础须知**：如何正确提问、如何截图、CTF常见赛制介绍。
*   **入门资料**：其他CTF维基、校科协公开课（往年资料）、《上海交大生存手册》、其他战队的入门指南。
*   **实用信息**：CTF赛事日程、刷题平台、安全社区、计算机类网站以及书籍推荐。
*   **新生赛与学习路线**：近期新生赛信息，以及从零开始的学习路线，涵盖计算机基础知识、浏览器插件、谷歌语法、Markdown、Git、Python、Linux、Docker等。无需一次性学完，可先建立印象，用时再查。
*   **常见问题解答（QA）**。
*   **Web方向专项**：包含入门指南和常用工具列表。

![](img/a4873707deffb0728a3834e0ec2950b9_16.png)

![](img/a4873707deffb0728a3834e0ec2950b9_18.png)

上一节我们介绍了整体的学习资源，本节中我们将重点看看Web方向所需的**常用工具**如何配置。

![](img/a4873707deffb0728a3834e0ec2950b9_20.png)

![](img/a4873707deffb0728a3834e0ec2950b9_22.png)

![](img/a4873707deffb0728a3834e0ec2950b9_24.png)

## 常用工具配置 🧰

![](img/a4873707deffb0728a3834e0ec2950b9_26.png)

![](img/a4873707deffb0728a3834e0ec2950b9_28.png)

![](img/a4873707deffb0728a3834e0ec2950b9_30.png)

![](img/a4873707deffb0728a3834e0ec2950b9_32.png)

![](img/a4873707deffb0728a3834e0ec2950b9_34.png)

我们的维基将工具分成了几个板块，下面将从上到下依次讲解。

![](img/a4873707deffb0728a3834e0ec2950b9_36.png)

### 必备工具

![](img/a4873707deffb0728a3834e0ec2950b9_38.png)

以下是完成CTF学习与解题所必需的基础软件。

![](img/a4873707deffb0728a3834e0ec2950b9_40.png)

![](img/a4873707deffb0728a3834e0ec2950b9_42.png)

![](img/a4873707deffb0728a3834e0ec2950b9_44.png)

![](img/a4873707deffb0728a3834e0ec2950b9_46.png)

*   **Typora**：一款Markdown编辑器。Markdown语法可以方便地进行基础排版，例如使用`#`表示标题，`**文字**`表示加粗，`*文字*`表示斜体，并支持代码高亮。你可以用它来撰写解题报告（Writeup），并最终导出为PDF格式。
*   **Homebrew**（仅限macOS）：macOS的包管理器。使用Homebrew可以快速安装和管理软件，例如通过命令`brew install go`来安装Go语言环境。国内用户建议配置镜像源以加速下载。Windows用户无需关注此工具。

![](img/a4873707deffb0728a3834e0ec2950b9_48.png)

![](img/a4873707deffb0728a3834e0ec2950b9_50.png)

### 虚拟机

如果你学习Pwn（二进制安全）方向，虚拟机是必需的。对于Web或其他方向，初期可能用不到，但可以提前了解。

![](img/a4873707deffb0728a3834e0ec2950b9_52.png)

![](img/a4873707deffb0728a3834e0ec2950b9_54.png)

以下是两种常见的虚拟机方案：
*   **Windows 平台**：VMware Workstation Pro。安装包和破解补丁已存放在招新群文件中。
*   **macOS 平台**：Parallels Desktop (PD)。安装包和激活工具也已存放在招新群文件中。

![](img/a4873707deffb0728a3834e0ec2950b9_56.png)

![](img/a4873707deffb0728a3834e0ec2950b9_58.png)

**安装与激活提示**：
1.  安装完成后，软件会提示需要激活。你可以搜索相关版本的密钥在线激活（VMware），或使用提供的激活工具（PD）。
2.  对于PD，安装后需先注册账号并点击试用，再使用破解补丁。
3.  为避免盗版弹窗，可能需要修改hosts文件。例如在终端执行：`sudo vim /etc/hosts`，添加相应规则后保存（`:wq`）。

![](img/a4873707deffb0728a3834e0ec2950b9_60.png)

![](img/a4873707deffb0728a3834e0ec2950b9_61.png)

![](img/a4873707deffb0728a3834e0ec2950b9_63.png)

![](img/a4873707deffb0728a3834e0ec2950b9_65.png)

### 容器与开发环境

![](img/a4873707deffb0728a3834e0ec2950b9_67.png)

*   **Docker**：用于创建隔离的应用环境。初期可能用不到，但可以提前了解。
    *   Windows用户可安装 Docker Desktop。
    *   macOS用户更推荐安装 OrbStack，它比Docker Desktop更轻量、快速和稳定。
*   **代码编辑器/IDE**：
    *   **VS Code**：轻量级且功能强大的编辑器。推荐安装插件如：
        *   `Code Runner`：右键快速运行代码片段。
        *   `GitHub Copilot`：AI代码辅助（可申请GitHub学生包免费使用）。
    *   **JetBrains 系列 IDE**：集成度更高的开发环境，例如：
        *   `IntelliJ IDEA`：用于Java开发。
        *   `GoLand`：用于Go语言开发。
        *   `PyCharm`：用于Python开发。
        *   `PhpStorm`：用于PHP开发。
    *   这些IDE均可通过学生认证（使用学校邮箱）免费申请授权。

### 编程语言环境

对于CTF Web方向，以下编程语言的运行环境是常用的。

![](img/a4873707deffb0728a3834e0ec2950b9_69.png)

*   **Python**：前往官网下载最新版安装程序（如Windows的`installer 64-bit`）。必装库是`requests`，用于编写HTTP请求脚本。安装命令：`pip3 install requests`。也可安装`ipython`获得更好的交互式命令行体验。
*   **PHP**：
    *   Windows用户推荐使用 `PHPStudy` 这类集成环境面板，方便启动和管理PHP服务。
    *   macOS用户可使用 `MAMP Pro`（有破解版），它支持多版本PHP。
*   **Node.js**：直接官网下载LTS（长期支持）版本安装即可。
*   **Go**：直接官网下载安装。macOS用户也可通过Homebrew安装：`brew install go`。
*   **Java**：对于大多数CTF题目，`JDK 8`版本最常用。可从Oracle官网下载，但需要注册账号。macOS用户可使用`jenv`等工具管理多版本JDK。

![](img/a4873707deffb0728a3834e0ec2950b9_71.png)

![](img/a4873707deffb0728a3834e0ec2950b9_72.png)

### 数据库管理工具

![](img/a4873707deffb0728a3834e0ec2950b9_74.png)

用于连接和操作数据库。

*   **Navicat**：功能全面的数据库管理工具。
*   **DBeaver**：免费开源的数据库管理工具。
两者任选其一或都安装，用于连接题目或本地搭建的数据库（如PHPStudy启动的MySQL，默认账号密码常为`root/root`）。

### 浏览器插件

能极大提升效率的浏览器扩展。

*   **Proxy SwitchyOmega**（或更新版 **SwitchyOmega**）：用于快速切换代理。在后续使用Burp Suite抓包时必备。
*   **Wappalyzer**：识别网站所用技术栈（如JavaScript框架、服务器、编程语言等）。虽然对CTF解题直接帮助不大，但有助于信息收集。
*   **Charset**：修改网页编码。当遇到乱码页面时，可尝试切换为`GBK`或`UTF-8`等编码。
*   **JSON Viewer**：自动格式化网页中的JSON数据，便于阅读。

![](img/a4873707deffb0728a3834e0ec2950b9_76.png)

### Web安全核心工具

![](img/a4873707deffb0728a3834e0ec2950b9_78.png)

这三款工具是CTF Web方向的“利器”。

![](img/a4873707deffb0728a3834e0ec2950b9_80.png)

![](img/a4873707deffb0728a3834e0ec2950b9_81.png)

![](img/a4873707deffb0728a3834e0ec2950b9_83.png)

![](img/a4873707deffb0728a3834e0ec2950b9_85.png)

![](img/a4873707deffb0728a3834e0ec2950b9_87.png)

*   **Burp Suite**：功能强大的HTTP抓包、改包和Web漏洞扫描工具。社区版功能有限，推荐使用专业版（提供破解补丁）。它主要包含以下模块：
    *   `Proxy`：拦截、查看和修改HTTP/HTTPS流量。
    *   `Repeater`：手动重放并修改HTTP请求，用于测试。
    *   `Intruder`：用于自动化攻击，如爆破用户名密码。
    *   `Scanner`：自动化的Web漏洞扫描器（专业版功能）。
*   **蚁剑 (AntSword)**：一款WebShell管理工具。当通过漏洞获取网站后门（WebShell）后，可用它连接并管理服务器。
*   **dirsearch**：网站目录爆破工具。用于发现目标网站上隐藏的目录和文件。通过命令行使用，例如：`python3 dirsearch.py -u http://target.com`。

![](img/a4873707deffb0728a3834e0ec2950b9_89.png)

**Burp Suite 安装激活简要流程**：
1.  从官网下载专业版安装包（JAR文件）。
2.  下载提供的破解补丁（`loader.jar`）。
3.  **Windows**：将`loader.jar`放入Burp安装目录，编辑`vmoptions`文件添加Java参数，然后通过命令行`java -jar loader.jar`启动加载器，按提示完成激活。
4.  **macOS**：将补丁文件放入Burp应用包内（`/Contents/Resources/app`），同样编辑`vmoptions`文件并添加参数，通过终端执行激活命令。若提示“文件已损坏”，需执行特定的系统命令绕过公证。
5.  激活成功后，后续可直接启动Burp Suite。

**Proxy SwitchyOmega 与 Burp 联动**：
1.  在插件中配置一个代理，指向`127.0.0.1:8080`（Burp默认监听端口）。
2.  在Burp的`Proxy` -> `Intercept`选项卡中确保`Intercept is on`。
3.  浏览器切换至该代理模式，此时浏览器流量会被Burp拦截。

![](img/a4873707deffb0728a3834e0ec2950b9_91.png)

![](img/a4873707deffb0728a3834e0ec2950b9_93.png)

![](img/a4873707deffb0728a3834e0ec2950b9_95.png)

![](img/a4873707deffb0728a3834e0ec2950b9_97.png)

## Web安全入门指南 🚀

![](img/a4873707deffb0728a3834e0ec2950b9_99.png)

![](img/a4873707deffb0728a3834e0ec2950b9_101.png)

![](img/a4873707deffb0728a3834e0ec2950b9_103.png)

在配置好环境后，我们来了解一下Web安全的学习路径。我们的维基将入门指南简单分成了三大部分。

![](img/a4873707deffb0728a3834e0ec2950b9_105.png)

![](img/a4873707deffb0728a3834e0ec2950b9_107.png)

![](img/a4873707deffb0728a3834e0ec2950b9_109.png)

### 什么是Web安全？

![](img/a4873707deffb0728a3834e0ec2950b9_111.png)

![](img/a4873707deffb0728a3834e0ec2950b9_113.png)

![](img/a4873707deffb0728a3834e0ec2950b9_115.png)

通俗地说，Web安全就是研究如何攻击（“黑掉”）一个网站（例如百度、学校官网），以及如何防御这种攻击。所有知识点都围绕**漏洞**展开。Web漏洞可能导致：
*   敏感信息泄露（如管理员账号密码、服务器配置文件`/etc/shadow`）。
*   获取服务器控制权限（即`Get Shell` 或 `RCE`）。

![](img/a4873707deffb0728a3834e0ec2950b9_117.png)

![](img/a4873707deffb0728a3834e0ec2950b9_119.png)

### 主要知识点

![](img/a4873707deffb0728a3834e0ec2950b9_121.png)

![](img/a4873707deffb0728a3834e0ec2950b9_123.png)

![](img/a4873707deffb0728a3834e0ec2950b9_124.png)

![](img/a4873707deffb0728a3834e0ec2950b9_125.png)

Web安全涉及但不限于以下常见漏洞类型：
*   HTTP协议基础
*   SQL注入
*   文件上传漏洞
*   文件包含漏洞
*   文件读取漏洞
*   命令执行漏洞
*   跨站脚本攻击（XSS）
*   服务端请求伪造（SSRF）
*   反序列化漏洞
*   等等

### 如何开始学习与解题？

![](img/a4873707deffb0728a3834e0ec2950b9_127.png)

![](img/a4873707deffb0728a3834e0ec2950b9_128.png)

1.  **利用搜索引擎**：遇到不懂的漏洞（如“SQL注入”），使用**Google**搜索“SQL注入 CTF 教程”。多看几篇不同文章或社区（如先知社区、安全客）的讲解，建立整体概念。
2.  **结合题目实践**：CTF Web题主要分两类：
    *   **黑盒测试**：无源代码。需通过信息收集、目录扫描、漏洞猜测等手段进行测试。
    *   **白盒测试**：提供源代码。需进行代码审计，分析源码逻辑寻找漏洞。
3.  **从题目信息入手**：题目名称、描述常包含关键提示。根据提示去搜索对应知识点。
4.  **适当利用AI辅助**：使用ChatGPT、GitHub Copilot等解释代码片段、讲解漏洞原理。但需注意AI可能“胡扯”，不可全信。
5.  **参考他人题解**：阅读他人的解题博客（Writeup），学习思路和方法。例如，可以查看往年的新生赛或CTF赛事的公开题解。
6.  **多刷题**：在CTF刷题平台（如CTFHub、NSSCTF）上练习。从基础题开始，不会就看提示或题解，在实战中学习。
7.  **参加比赛**：积极报名参加各类新生赛、公开赛，以赛促学。

![](img/a4873707deffb0728a3834e0ec2950b9_130.png)

![](img/a4873707deffb0728a3834e0ec2950b9_132.png)

![](img/a4873707deffb0728a3834e0ec2950b9_134.png)

## 总结 📝

![](img/a4873707deffb0728a3834e0ec2950b9_136.png)

![](img/a4873707deffb0728a3834e0ec2950b9_138.png)

![](img/a4873707deffb0728a3834e0ec2950b9_140.png)

本节课中我们一起学习了为CTF Web方向配置开发环境的完整流程，涵盖了从文档编辑、编程语言、数据库工具到核心安全工具（Burp Suite、蚁剑等）的安装与简介。同时，我们也梳理了Web安全的基本概念、主要知识体系以及“边做边学、善用搜索、多练多赛”的实用学习路径。工欲善其事，必先利其器，希望本教程能帮助你顺利搭建起学习Web安全的第一块基石。