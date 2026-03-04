# 渗透测试：P24：1.渗透测试-域名信息收集 🎯

在本节课中，我们将要学习渗透测试信息收集的第一步：域名信息收集。信息收集是渗透测试成功的关键保障，通过广泛收集目标资产信息，可以显著增加发现漏洞的可能性。本节课将详细介绍域名、Whois查询、备案信息以及多种子域名收集的方法和工具。

## 概述

信息收集是指通过各种方式获取目标信息的过程，为后续渗透测试打下基础。我们需要收集的信息包括但不限于：目标站点的IP地址、服务器中间件（如Apache、Tomcat）、脚本语言（如PHP、JSP、ASP）、开放端口、管理员邮箱等。信息收集主要分为主动信息收集和被动信息收集两类。

## 信息收集的分类

上一节我们介绍了信息收集的重要性，本节中我们来看看信息收集的具体分类。

信息收集主要分为主动信息收集和被动信息收集。
*   **主动信息收集**：通过直接与目标系统交互来获取信息，例如对网站进行漏洞扫描、端口扫描。这种方式会产生直接流向目标服务器的网络流量，并可能在目标服务器的访问日志中留下记录。如果目标部署了防火墙或入侵检测系统（IDS），可能会触发警报或导致IP被封禁。
*   **被动信息收集**：利用公开渠道获取信息，不与目标系统直接交互。例如，使用搜索引擎或网络空间搜索引擎（如Fofa、Shodan）来查询已被网络爬虫公开收录的信息。这种方式隐蔽性更高，不易被目标察觉。

![](img/54eb29deb36a00b889f3c8ceda73da21_1.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_2.png)

## 需要收集的信息

![](img/54eb29deb36a00b889f3c8ceda73da21_4.png)

无论是刚入门的同学还是经验丰富的渗透测试工程师，在项目中通常都需要收集以下几类信息：

以下是渗透测试中需要收集的核心信息类别：
1.  **服务器信息**：包括IP地址、开放的端口及其对应的服务。
2.  **网站信息**：这是Web渗透的重点。需要了解网站的架构，例如：
    *   服务器操作系统
    *   中间件类型（如Nginx, Apache）
    *   数据库类型（如MySQL, Redis）
    *   编程语言（如PHP, Java）
    *   其他指纹信息（如WAF类型）
    *   敏感目录或文件
    *   是否存在源码泄露
    *   同C段IP的其他站点（旁站）
3.  **域名信息**：通过Whois查询、备案信息查询和子域名搜索来获取。
4.  **人员信息**：收集目标组织相关人员的姓名、职务、生日、联系电话或邮件地址等。这些信息可用于生成针对性密码字典进行爆破攻击，因为很多人会使用此类个人信息设置密码。

## 域名与Whois查询

![](img/54eb29deb36a00b889f3c8ceda73da21_6.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_8.png)

在了解了需要收集的信息后，我们首先从最基础的域名概念开始。

![](img/54eb29deb36a00b889f3c8ceda73da21_10.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_11.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_13.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_14.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_16.png)

**域名**是由点分隔的一组名称，用于在互联网上标识计算机或计算机组。域名通过DNS（域名系统）与IP地址相互映射，使人更方便地访问互联网，例如 `baidu.com`。

![](img/54eb29deb36a00b889f3c8ceda73da21_18.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_19.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_21.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_22.png)

域名分为不同级别，例如顶级域名（`.com`, `.gov`, `.edu`）、二级域名（如 `mail.baidu.com` 中的 `baidu`）等。`.gov` 和 `.edu` 分别代表政府机构和教育机构，未经授权不得对其进行安全测试。

**Whois** 是一种用于查询域名注册信息的协议和数据库。通过Whois查询，可以获取域名的注册者、注册商、注册日期、过期日期、DNS服务器和联系人邮箱等信息。对于中小型网站，域名注册者可能就是网站管理员。

![](img/54eb29deb36a00b889f3c8ceda73da21_24.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_26.png)

以下是进行Whois查询的两种常用方法：
*   **Web接口查询**：使用如阿里云Whois或站长之家等在线平台进行查询。例如，在站长之家输入 `hetianlab.com` 即可查看其注册信息。
*   **命令行查询**：在Kali Linux等系统中，可以直接使用 `whois` 命令进行查询。例如，在终端输入：
    ```bash
    whois baidu.com
    ```

![](img/54eb29deb36a00b889f3c8ceda73da21_28.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_30.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_32.png)

## 备案信息查询

在中国大陆，搭建网站并进行域名解析需要进行**ICP备案**。备案后会获得一个备案号，通常显示在网站底部。

![](img/54eb29deb36a00b889f3c8ceda73da21_34.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_35.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_36.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_37.png)

备案信息可用于反查同一主办单位注册的其他网站（旁站），从而发现更多关联资产。可以通过工信部备案管理系统或第三方网站查询备案信息。

![](img/54eb29deb36a00b889f3c8ceda73da21_39.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_41.png)

## 子域名收集

![](img/54eb29deb36a00b889f3c8ceda73da21_43.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_45.png)

子域名收集是信息收集环节中至关重要的一步。收集到的子域名越多，发现脆弱点的面就越广，渗透测试成功的可能性就越大。

![](img/54eb29deb36a00b889f3c8ceda73da21_47.png)

子域名是顶级域名的下一级，例如 `mail.hetianlab.com` 和 `bbs.hetianlab.com` 都是 `hetianlab.com` 的子域名。

![](img/54eb29deb36a00b889f3c8ceda73da21_49.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_50.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_52.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_54.png)

以下是几种常用的子域名收集方法：

![](img/54eb29deb36a00b889f3c8ceda73da21_56.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_58.png)

### 1. 搜索引擎收集
利用搜索引擎（如Google）的特定语法进行搜索。例如，使用 `site:` 语法可以查找指定域名的所有相关内容。
```
site:hetianlab.com
```
此外，还可以结合其他语法寻找潜在漏洞点，例如搜索可能存在SQL注入的URL模式。

![](img/54eb29deb36a00b889f3c8ceda73da21_60.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_62.png)

### 2. 第三方网站查询
利用一些在线服务平台（如站长之家）提供的子域名查询功能。这些平台聚合了DNS记录等信息。

![](img/54eb29deb36a00b889f3c8ceda73da21_64.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_66.png)

### 3. 网络空间搜索引擎
使用如 **Fofa**、**Shodan**、**ZoomEye（钟馗之眼）** 等网络空间搜索引擎。这些引擎通过持续扫描全网设备并建立索引，可以高效地发现目标的子域名及相关服务信息。部分功能可能需要付费。

![](img/54eb29deb36a00b889f3c8ceda73da21_68.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_70.png)

### 4. SSL证书查询
网站启用HTTPS（443端口）时会部署SSL证书。通过查询证书颁发机构的记录或利用证书透明度（CT）日志，可以发现关联的域名。有些在线工具专门提供通过SSL证书查找子域名的服务。

![](img/54eb29deb36a00b889f3c8ceda73da21_72.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_74.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_76.png)

### 5. JS文件分析
网页的JavaScript（JS）文件中常常会引用或包含其他子域名的链接。通过爬取和分析目标网站的JS文件，可以提取出隐藏的子域名。可以使用工具如 **JSFinder** 来自动化这一过程。
```bash
# 示例：使用JSFinder（具体命令可能因工具版本而异）
python JSFinder.py -u http://www.baidu.com
```

![](img/54eb29deb36a00b889f3c8ceda73da21_78.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_80.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_82.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_84.png)

### 6. 工具自动化收集
以下是三个常用的自动化子域名收集工具：

![](img/54eb29deb36a00b889f3c8ceda73da21_86.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_88.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_90.png)

*   **子域名挖掘机（Layer）**：通过加载字典进行子域名爆破。原理是尝试将字典中的前缀与目标域名拼接，然后解析DNS记录。缺点是速度较慢，且久未更新。
*   **OneForAll**：一个功能强大的子域名收集工具，它集成了多种收集方式（如证书查询、DNS查询、搜索引擎等）。其效果很大程度上依赖于配置的各类API密钥（如搜索引擎API、DNS查询API等）。配置越完善，收集结果越好。
    ```bash
    # 基本使用示例
    python3 oneforall.py --target hetianlab.com run
    ```
*   **Sublist3r**：一个用于枚举子域的Python工具，它利用多个公开源进行查询，使用简单。
    ```bash
    # 基本使用示例
    python3 sublist3r.py -d hetianlab.com
    ```

![](img/54eb29deb36a00b889f3c8ceda73da21_92.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_94.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_95.png)

**最佳实践**：在实际渗透测试中，通常不会只依赖一种工具或方法。建议结合多种工具（如OneForAll配置部分免费API，再使用Sublist3r或搜索引擎作为补充）进行收集，最后对结果进行去重和整理，以获得最全面的子域名列表。

![](img/54eb29deb36a00b889f3c8ceda73da21_97.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_99.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_101.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_103.png)

![](img/54eb29deb36a00b889f3c8ceda73da21_104.png)

## 总结

![](img/54eb29deb36a00b889f3c8ceda73da21_106.png)

本节课中我们一起学习了渗透测试信息收集中的域名信息收集部分。我们首先明确了信息收集的目的和分类（主动与被动），然后列出了需要收集的关键信息类型。接着，我们详细讲解了域名的概念、Whois查询和备案信息查询的方法。最后，我们深入探讨了子域名收集的重要性，并介绍了多种实用的收集方法，包括搜索引擎语法、网络空间搜索引擎、SSL证书查询、JS文件分析以及几款主流自动化工具（如OneForAll、Sublist3r）的使用。

![](img/54eb29deb36a00b889f3c8ceda73da21_108.png)

记住，广泛而细致的资产信息收集是成功渗透的基石。课后请务必动手实践，使用至少两种工具尝试收集 `hetianlab.com` 和 `baidu.com` 的子域名，在实践中熟悉工具并解决问题。