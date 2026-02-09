# 课程P3：Web架构与安全组件详解 🔐

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_0.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_2.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_4.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_6.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_7.png)

在本节课中，我们将学习五种常见的Web架构组件：WAF、OSS存储、CDN加速、反向代理和负载均衡。我们将了解它们的基本原理、作用，并重点分析它们对网站安全测试可能产生的影响。

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_9.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_10.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_12.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_14.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_16.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_18.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_20.png)

## 1. WAF（Web应用防火墙）🛡️

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_22.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_24.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_26.png)

上一节我们介绍了课程概述，本节中我们来看看第一种组件：WAF。

WAF的全称是Web Application Firewall，即网站应用防火墙。它的核心作用是提供防护和保护，主要防止针对Web应用的攻击。

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_28.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_30.png)

WAF主要分为以下几类：
*   **硬件型WAF**：公司产品大多以此为主，属于商业产品。
*   **软件型WAF**：个人使用较多。
*   **云WAF**：类似阿里云、腾讯云等厂商提供的WAF产品。

WAF对安全测试的影响非常直接：它会拦截常规的Web攻击手法。下面我们通过一个简单的演示来理解。

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_32.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_34.png)

### WAF防护效果演示

以下是演示WAF开启前后对比的步骤：

1.  **环境准备**：在Windows Server上搭建IIS，并安装一款免费的WAF防护软件。
2.  **关闭WAF**：首先移除防护，访问网站上的后门文件（例如 `1.asp`），可以正常访问。使用连接工具也能成功连接。
3.  **开启WAF**：安装并开启WAF防护。
4.  **再次测试**：尝试访问后门文件时，页面提示“禁止脚本访问”。使用连接工具尝试连接，会提示“初始化失败”，连接被阻断。

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_36.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_38.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_40.png)

这个演示清晰地展示了WAF的核心作用：**部署后，它会根据规则拦截对网站的后门、漏洞利用等攻击行为**。因此，在后续的安全测试中，如果目标网站部署了WAF，我们的攻击手法很可能会受到拦截。

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_42.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_44.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_46.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_48.png)

> **关于WAF绕过**：网上存在WAF绕过的专题技术。但需要理解，WAF本身就是防护产品，能绕过的通常是一些防护能力较弱或规则存在空子的情况。对于主流的商业WAF产品，从正面完全绕过非常困难。

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_50.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_51.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_53.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_55.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_57.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_59.png)

## 2. CDN（内容分发网络）⚡

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_61.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_63.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_65.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_67.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_69.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_71.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_73.png)

上一节我们介绍了WAF的防护机制，本节中我们来看看用于加速访问的CDN。

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_75.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_77.png)

CDN的全称是Content Delivery Network，即内容分发网络。它的原理是**内容分发服务**，存在意义是**提高网站的访问速度**。

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_79.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_81.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_83.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_85.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_87.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_89.png)

其工作原理是：在全球或全国各地部署多个节点服务器。当用户访问网站时，CDN会调度到离用户最近的节点来提供服务，从而减少网络延迟，提升访问速度。

然而，CDN的引入也带来了一个对安全测试至关重要的影响：**它隐藏了网站的真实IP地址**。用户和测试者访问到的都是CDN节点的IP，而非网站源站服务器的真实IP。

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_91.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_93.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_95.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_97.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_99.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_101.png)

### CDN效果演示

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_103.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_104.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_106.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_108.png)

我们通过一个已备案的域名来演示开启CDN前后的区别：

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_110.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_111.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_113.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_115.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_116.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_117.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_118.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_120.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_121.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_123.png)

1.  **未开启CDN**：域名直接解析到源站服务器IP（例如 `17.1.1.1`）。此时，通过 `ping` 命令或信息收集工具，获取到的就是真实IP。
2.  **开启CDN加速**：
    *   在CDN服务商控制台添加域名（如 `www.demo.com`），并设置源站IP。
    *   在域名DNS解析处，将域名（如 `www`）以CNAME记录方式指向CDN提供的域名。
3.  **开启后效果**：配置生效后，不同地区的用户 `ping` 该域名，会得到不同的IP地址（均为CDN节点IP）。使用“超级Ping”等工具进行多地检测，会发现解析出的IP遍布全国各地，且都非源站真实IP。

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_125.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_127.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_129.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_131.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_133.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_134.png)

此时，如果你对这个域名进行信息收集或端口扫描，你的目标实际上是CDN节点，而非真实的网站服务器。**这会导致后续的安全测试方向错误**。因此，针对使用了CDN的网站，信息收集阶段的一个重要任务就是尝试寻找其真实IP地址。

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_136.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_138.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_140.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_142.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_144.png)

## 3. OSS（对象存储）📦

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_146.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_148.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_149.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_150.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_152.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_153.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_155.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_157.png)

上一节我们了解了CDN如何隐藏真实IP，本节中我们来看看另一种影响文件安全的组件：OSS。

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_159.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_161.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_163.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_165.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_167.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_169.png)

OSS的全称是Object Storage Service，即对象存储服务。它是一种海量、安全、低成本的云存储服务，**主要用于存储网站的图片、视频、文档等静态资源**。它也能提高资源文件的加载速度。

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_171.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_173.png)

OSS对安全测试，特别是**文件上传漏洞**，有着显著的影响。当网站程序配置了OSS存储后，用户上传的文件不会保存在网站自身的服务器目录下，而是直接存储到独立的OSS资源桶中。

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_175.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_177.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_179.png)

### OSS效果演示

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_181.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_183.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_185.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_187.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_189.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_191.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_193.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_195.png)

我们通过一个网盘程序来演示：

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_197.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_198.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_200.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_202.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_204.png)

1.  **未配置OSS**：在网盘上传一个图片或文本文件，文件会存储在网站服务器的本地目录中。
2.  **配置OSS**：
    *   在阿里云OSS创建存储空间（Bucket）。
    *   在网盘程序后台配置OSS的访问端点（Endpoint）、访问密钥（AccessKey）等信息，并设置存储策略为OSS。
3.  **配置后效果**：再次上传文件。此时，网站服务器本地目录不会出现该文件。登录阿里云OSS控制台，可以在对应的Bucket中找到刚上传的文件。通过OSS提供的链接访问该文件，它仅作为静态文件被下载或展示，**服务器不会将其作为脚本解析执行**。

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_206.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_208.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_210.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_212.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_214.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_216.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_218.png)

这意味着，即使网站存在文件上传漏洞，攻击者成功上传了Webshell（如 `.asp`, `.php` 文件），由于文件被存储在了OSS上，**它无法被网站服务器执行**，从而直接消除了上传漏洞的危害。

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_220.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_222.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_224.png)

> **新的安全隐患**：配置OSS需要用到AccessKey等密钥。如果这些密钥泄露，攻击者可能直接操作OSS资源，带来新的安全风险。这体现了安全是“矛”与“盾”的持续博弈。

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_226.png)

## 4. 反向代理与正向代理 🔄

上一节我们看到了OSS如何改变文件存储逻辑，本节我们来学习代理技术，其中反向代理对测试目标有重大影响。

首先区分两个概念：
*   **正向代理**：**代理客户端**。客户端通过代理服务器去访问目标服务器。例如，VPN或科学上网工具就是典型的正向代理，它帮助用户访问原本无法直接到达的资源。
*   **反向代理**：**代理服务器端**。服务器端将客户端的请求，转发到内部或后端的其他服务器。客户端感知不到真实的后端服务器。

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_228.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_230.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_232.png)

反向代理对安全测试的影响在于：**它可以将流量转发到另一个毫不相干的站点**。

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_234.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_236.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_238.png)

### 反向代理效果演示

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_240.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_242.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_243.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_245.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_247.png)

在宝塔面板或Nginx中配置反向代理规则非常简单：
```nginx
# 示例：将访问本网站根目录的请求，全部代理到百度
location / {
    proxy_pass https://www.baidu.com;
}
```
配置后，访问你的网站（如 `test.com`），浏览器显示的实际上是百度的内容，地址栏可能仍是你的域名。如果你被要求测试 `test.com` 这个网站，你的所有测试实际上都落在了百度上，与 `test.com` 的真实后端无关。

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_249.png)

因此，在进行测试时，需要识别目标是否使用了反向代理，否则测试活动可能完全偏离真实目标。

## 5. 负载均衡 ⚖️

上一节我们探讨了反向代理的流量转发，本节我们来看另一种提升服务能力的架构：负载均衡。

负载均衡的原理是将访问请求**分摊到多个操作单元（服务器）上执行**，共同完成任务。其主要目的是：
1.  **提高处理能力与访问速度**。
2.  **实现高可用与容灾**：当一台服务器故障时，其他服务器可以继续提供服务。

### 负载均衡效果演示

以Nginx的负载均衡配置为例：
```nginx
upstream backend_servers { # 定义后端服务器组
    server 192.168.1.101:80 weight=3; # 服务器1，权重3
    server 192.168.1.102:80 weight=1; # 服务器2，权重1
}

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_251.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_253.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_255.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_257.png)

server {
    location / {
        proxy_pass http://backend_servers; # 将请求转发至服务器组
    }
}
```
假设 `101` 服务器页面显示“Server A”，`102` 服务器页面显示“Server B”。配置负载均衡后，访问网站，刷新页面，内容可能会在“Server A”和“Server B”之间切换（根据权重，出现A的概率更高）。

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_259.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_261.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_263.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_265.png)

这对安全测试的影响是：**目标网站可能由多台服务器集群共同支撑**。你的攻击可能只针对其中一台，需要思考如何扩大战果，或者你的攻击行为是否会被分摊到不同服务器上。

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_267.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_269.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_270.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_271.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_273.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_275.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_277.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_278.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_279.png)

## 总结 📝

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_280.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_282.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_284.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_285.png)

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_287.png)

本节课中我们一起学习了五种常见的Web架构与安全组件：
1.  **WAF**：防护产品，会拦截攻击流量，增加测试难度。
2.  **CDN**：加速服务，会隐藏网站真实IP，导致信息收集目标错误。
3.  **OSS**：对象存储，将上传文件独立存储，可能使文件上传漏洞失效，但引入密钥管理风险。
4.  **反向代理**：流量转发，可能将测试者引向非真实的业务服务器。
5.  **负载均衡**：多服务器分摊请求，使目标环境变得复杂。

![](img/625cefca01c2a81dfe9c0be5a8f4d4eb_289.png)

理解这些架构组件，是进行有效Web安全测试的重要基础。它帮助我们明确信息收集的方向，理解某些攻击手法为何失效，并在后续学习中，为掌握诸如“CDN真实IP查找”、“WAF绕过”等进阶技术做好铺垫。