#  015：主机架构分析、蜜罐识别、WAF识别、端口扫描、协议识别与服务安全 🔍

![](img/ee062da7355ca7b7bea8e059b19ff28e_1.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_3.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_5.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_6.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_8.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_10.png)

在本节课中，我们将学习如何分析目标主机的架构，识别其是否部署了蜜罐或WAF，并通过端口扫描来识别开放的协议与服务，从而评估其安全状况。这些技能是渗透测试与安全评估中信息收集阶段的关键环节。

![](img/ee062da7355ca7b7bea8e059b19ff28e_12.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_14.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_16.png)

## 概述：主机架构分析技术

![](img/ee062da7355ca7b7bea8e059b19ff28e_18.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_20.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_22.png)

主机架构分析主要围绕三个技术点展开：端口识别、蜜罐识别和WAF识别。上节课我们介绍了Web服务器、应用服务器、数据库类型、操作系统以及各种服务（如FTP、S7等）的识别。本节课我们将重点学习如何识别WAF信息、蜜罐信息，并通过端口扫描来发现主机上运行的各种服务。

![](img/ee062da7355ca7b7bea8e059b19ff28e_24.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_26.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_28.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_30.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_32.png)

简单来说，我们将学习如何通过技术手段，判断目标主机上运行了哪些服务、是否存在防护设备（如WAF），以及目标是否是一个用于诱捕攻击者的陷阱（蜜罐）。

![](img/ee062da7355ca7b7bea8e059b19ff28e_34.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_36.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_38.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_40.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_42.png)

---

![](img/ee062da7355ca7b7bea8e059b19ff28e_44.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_46.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_48.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_50.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_52.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_54.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_56.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_58.png)

## Web服务器与应用服务器识别 🖥️

![](img/ee062da7355ca7b7bea8e059b19ff28e_60.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_62.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_64.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_66.png)

上一节我们介绍了主机架构分析的整体框架，本节中我们来看看如何识别Web服务器和应用服务器。

![](img/ee062da7355ca7b7bea8e059b19ff28e_68.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_70.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_72.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_74.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_76.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_78.png)

### Web服务器识别

![](img/ee062da7355ca7b7bea8e059b19ff28e_80.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_82.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_84.png)

识别Web服务器通常比较简单。当我们访问一个网站时，可以查看HTTP响应头中的 `Server` 字段，该字段通常会标明服务器类型和版本。

![](img/ee062da7355ca7b7bea8e059b19ff28e_86.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_88.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_90.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_92.png)

例如，访问一个网站后，在返回的HTTP数据包中，我们可能会看到如下信息：
```http
Server: Apache/2.4.41 (Ubuntu)
```
这表示该网站运行在Apache服务器上。

### 应用服务器识别

![](img/ee062da7355ca7b7bea8e059b19ff28e_94.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_96.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_98.png)

应用服务器（如Tomcat、JBoss、WebLogic）与Web服务器（如Apache、Nginx、IIS）有所不同。应用服务器的一个显著特点是会开放特定的服务端口。

![](img/ee062da7355ca7b7bea8e059b19ff28e_100.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_102.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_104.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_106.png)

例如：
*   Tomcat 默认开放 **8080** 端口。
*   WebLogic 默认开放 **7001** 端口。
*   JBoss 默认开放 **8080** 或 **9990** 端口。

![](img/ee062da7355ca7b7bea8e059b19ff28e_108.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_110.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_112.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_114.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_116.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_118.png)

因此，识别应用服务器不能仅靠HTTP响应头，而需要借助端口扫描技术，检查这些特定端口是否开放。

![](img/ee062da7355ca7b7bea8e059b19ff28e_120.png)

---

![](img/ee062da7355ca7b7bea8e059b19ff28e_122.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_124.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_126.png)

## 端口扫描：发现主机服务 🚪

![](img/ee062da7355ca7b7bea8e059b19ff28e_128.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_130.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_132.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_134.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_136.png)

上一节我们了解了如何通过特定端口识别应用服务器，本节中我们将系统学习端口扫描技术。

端口扫描是判断目标主机开放了哪些网络端口，从而推断其运行了哪些服务（如网站、数据库、远程连接服务等）的核心技术。

### 常用端口扫描工具

![](img/ee062da7355ca7b7bea8e059b19ff28e_138.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_140.png)

以下是三种常用的端口扫描工具，各有优势：

1.  **网络空间搜索引擎（如Fofa、Quake）**：属于被动信息收集。无需主动向目标发送探测包，直接通过搜索引擎查询目标IP的开放端口和历史记录。优点是快速、隐蔽，但数据可能不全或过时。
2.  **Nmap**：老牌且功能强大的主动扫描工具。扫描准确、信息详细（可探测服务版本、操作系统等），但速度相对较慢。
3.  **Masscan**：高速的主动扫描工具。扫描速度极快，适合大范围IP的端口探测，但在准确性上可能略逊于Nmap。

**主动扫描**与**被动扫描**的区别：
*   **主动扫描**：从你的主机主动向目标发送探测数据包，根据目标的回应进行分析。
*   **被动扫描**：通过第三方平台或搜索引擎查询目标信息，自身不产生直接流量。

### 端口扫描的意义

端口扫描不仅用于识别中间件，还能发现数据库、文件传输、邮件等各种服务。

![](img/ee062da7355ca7b7bea8e059b19ff28e_142.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_144.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_146.png)

以下是常见端口、协议及其可能存在的安全问题示例：

![](img/ee062da7355ca7b7bea8e059b19ff28e_148.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_150.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_152.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_154.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_156.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_158.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_160.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_162.png)

| 端口号 | 协议/服务 | 可能存在的安全问题 |
| :--- | :--- | :--- |
| 21 | FTP | 匿名上传下载、弱口令爆破、软件后门 |
| 22 | SSH | 弱口令爆破、版本漏洞 |
| 80/443 | HTTP/HTTPS | Web应用漏洞（如SQL注入、XSS） |
| 3306 | MySQL | SQL注入、弱口令、权限提升漏洞 |
| 3389 | RDP (Windows远程桌面) | 弱口令爆破、漏洞（如BlueKeep） |
| 6379 | Redis | 未授权访问、弱口令 |

![](img/ee062da7355ca7b7bea8e059b19ff28e_164.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_166.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_168.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_170.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_172.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_174.png)

通过端口扫描，我们可以：
*   **了解服务器角色**：它是Web服务器、数据库服务器还是文件服务器？
*   **拓宽攻击面**：除了Web漏洞，还可以尝试FTP、数据库、远程桌面等其他服务的漏洞。
*   **制定测试策略**：针对不同的开放服务，采用相应的安全测试方法。

![](img/ee062da7355ca7b7bea8e059b19ff28e_175.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_177.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_179.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_181.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_183.png)

### 扫描中的特殊情况

![](img/ee062da7355ca7b7bea8e059b19ff28e_185.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_187.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_189.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_191.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_193.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_195.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_197.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_199.png)

进行端口扫描时，可能会遇到两种特殊情况：

![](img/ee062da7355ca7b7bea8e059b19ff28e_201.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_203.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_205.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_207.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_209.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_211.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_213.png)

1.  **防火墙干扰**：目标服务器的防火墙可能阻止了我们的扫描请求。在Nmap结果中，端口状态可能显示为 **filtered**，表示该端口可能被防火墙过滤，无法确定其真实状态（开放或关闭）。
2.  **内网环境**：目标真实业务可能部署在内网，通过反向代理或NAT映射到公网IP。此时，你扫描公网IP只能发现被映射出来的服务（如Web端口），而内网中其他服务（如数据库）的端口则无法扫描到。这会导致扫描结果与实际情况存在差异。

![](img/ee062da7355ca7b7bea8e059b19ff28e_215.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_217.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_219.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_221.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_223.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_225.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_226.png)

---

![](img/ee062da7355ca7b7bea8e059b19ff28e_228.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_230.png)

## WAF（Web应用防火墙）识别 🛡️

上一节我们学习了如何发现主机服务，本节中我们来看看如何识别保护Web应用的防火墙——WAF。

WAF（Web Application Firewall）用于保护Web应用，防御常见的Web攻击（如SQL注入、XSS）。识别WAF的主要意义在于：如果目标存在强大的WAF，那么绝大多数常规Web攻击手段都会失效，我们需要据此调整测试策略或考虑放弃对该目标的Web层面测试。

![](img/ee062da7355ca7b7bea8e059b19ff28e_232.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_234.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_236.png)

### WAF的类型

![](img/ee062da7355ca7b7bea8e059b19ff28e_238.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_240.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_242.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_244.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_246.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_248.png)

*   **云WAF**：由云服务商（如阿里云、腾讯云、AWS）提供，集成在其云产品中，技术通常较先进。
*   **硬件WAF**：独立的硬件设备，部署在机房网络入口，常见于中大型企业或机构。
*   **软件WAF**：安装在服务器上的软件，如安全狗、云锁、宝塔防火墙等，常见于个人或中小型企业。
*   **代码级WAF**：在网站应用代码中实现的防护规则。

![](img/ee062da7355ca7b7bea8e059b19ff28e_250.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_252.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_254.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_256.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_258.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_260.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_262.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_264.png)

### 如何识别WAF

![](img/ee062da7355ca7b7bea8e059b19ff28e_266.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_268.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_270.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_272.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_274.png)

1.  **人工识别**：主动发送一些恶意请求（如简单的SQL注入语句），观察返回的页面。如果请求被拦截，通常会返回特定的拦截页面，页面上可能包含WAF厂商的名称或特征图标。
2.  **工具识别**：使用自动化工具进行识别。
    *   **WAFW00F**：一款经典的WAF识别工具。
    *   **识别项目**：一些开源项目内置了WAF指纹库，可以自动匹配。
3.  **网络空间搜索引擎**：在Fofa、Quake中搜索目标IP或域名，有时会在资产信息中直接标注WAF类型。

![](img/ee062da7355ca7b7bea8e059b19ff28e_276.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_278.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_280.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_282.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_284.png)

**核心建议**：识别出WAF（尤其是云WAF或知名硬件WAF）后，需要理性评估绕过难度。对于大多数强WAF，常规Web攻击手段成功率极低，应及时调整测试重点，转向其他服务或入口点。

![](img/ee062da7355ca7b7bea8e059b19ff28e_286.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_288.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_290.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_292.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_294.png)

---

![](img/ee062da7355ca7b7bea8e059b19ff28e_296.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_298.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_300.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_302.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_304.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_306.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_308.png)

## 蜜罐识别 🍯

![](img/ee062da7355ca7b7bea8e059b19ff28e_310.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_312.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_314.png)

上一节我们讨论了如何识别防御系统（WAF），本节中我们来看看如何识别攻击陷阱——蜜罐。

![](img/ee062da7355ca7b7bea8e059b19ff28e_316.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_318.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_320.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_322.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_324.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_326.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_327.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_329.png)

蜜罐（Honeypot）是一种安全威胁检测技术，其本质是部署一个充满“漏洞”和“敏感信息”的虚假系统，用于诱骗、记录攻击者的行为，并消耗其时间。作为攻击方（红队），识别蜜罐至关重要，以避免在虚假目标上浪费精力并暴露自身手法。

![](img/ee062da7355ca7b7bea8e059b19ff28e_331.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_333.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_335.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_337.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_339.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_341.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_343.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_345.png)

### 蜜罐的类型

![](img/ee062da7355ca7b7bea8e059b19ff28e_347.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_349.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_351.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_353.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_355.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_357.png)

根据交互程度分为：
*   **低交互蜜罐**：模拟简单服务，交互有限。
*   **中交互蜜罐**：模拟部分服务逻辑，交互性更强。
*   **高交互蜜罐**：模拟一个接近真实的完整系统，交互程度最深。

![](img/ee062da7355ca7b7bea8e059b19ff28e_359.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_361.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_363.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_365.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_367.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_369.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_371.png)

### 如何识别蜜罐

![](img/ee062da7355ca7b7bea8e059b19ff28e_373.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_375.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_377.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_379.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_381.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_383.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_385.png)

1.  **工具识别**：
    *   **Hermit（夸克蜜罐识别工具）**：利用360夸克网络空间引擎的数据进行识别，相对准确。
    *   **浏览器插件**：一些插件可以检测页面特征，提示可能为蜜罐，但误报率可能较高。
2.  **人工分析**：
    *   **端口特征**：蜜罐通常会开放大量端口（数十甚至上百个），且端口号可能呈现有规律的序列（如8001, 8002, 8003...），这与真实业务服务器通常只开放必要端口的特点不同。
    *   **协议访问异常**：尝试用浏览器（HTTP协议）访问非Web服务端口（如数据库端口3306）。真实系统通常会连接失败或无响应，而某些蜜罐为了实现攻击记录，可能会返回一个文件下载，这是其设计架构导致的特征。
    *   **指纹比对**：收集各类蜜罐产品的默认页面、错误信息等指纹，与目标返回的信息进行比对。
3.  **网络空间搜索引擎**：在Fofa、Quake中搜索目标IP，资产标签有时会直接标记为“蜜罐”。

![](img/ee062da7355ca7b7bea8e059b19ff28e_387.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_389.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_391.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_393.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_394.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_396.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_398.png)

**核心建议**：综合运用工具、人工分析和网络空间情报进行判断。一旦高度怀疑目标是蜜罐，最明智的做法是立即停止攻击，更换目标，以免徒劳无功并泄露自身信息。

![](img/ee062da7355ca7b7bea8e059b19ff28e_400.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_402.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_404.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_406.png)

---

![](img/ee062da7355ca7b7bea8e059b19ff28e_408.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_410.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_412.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_414.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_416.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_418.png)

## 总结 📝

![](img/ee062da7355ca7b7bea8e059b19ff28e_420.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_422.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_424.png)

本节课中我们一起学习了主机架构分析的关键技术：

![](img/ee062da7355ca7b7bea8e059b19ff28e_426.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_428.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_430.png)

1.  **端口扫描**：使用Nmap、Masscan等工具或网络空间引擎，发现目标开放的服务端口，从而推断其运行的应用（Web服务器、数据库、远程管理等），并据此拓宽安全测试的范围。
2.  **WAF识别**：通过人工测试、工具（如WAFW00F）或网络空间引擎，判断目标Web应用前是否部署了防火墙。识别WAF有助于我们评估Web层面攻击的难度，及时调整测试策略。
3.  **蜜罐识别**：利用专用工具（如Hermit）、分析端口特征、协议响应异常以及网络空间情报，判断目标是否是一个用于诱捕攻击者的陷阱。识别蜜罐能有效避免时间浪费和信息暴露。

![](img/ee062da7355ca7b7bea8e059b19ff28e_432.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_434.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_436.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_438.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_440.png)

![](img/ee062da7355ca7b7bea8e059b19ff28e_441.png)

掌握这些技能，能够帮助我们在渗透测试和信息收集阶段更清晰地认识目标，做出更明智的决策，从而提高测试效率和成功率。下节课我们将继续学习CDN识别和组件识别等相关内容。