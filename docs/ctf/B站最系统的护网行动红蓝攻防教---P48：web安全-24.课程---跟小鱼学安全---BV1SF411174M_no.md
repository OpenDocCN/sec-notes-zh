# 护网行动红蓝攻防教程：P48：Web安全-24.课程考核讲解 🎯

![](img/bb2868721acf3cb84984bb0920c759ed_1.png)

![](img/bb2868721acf3cb84984bb0920c759ed_3.png)

![](img/bb2868721acf3cb84984bb0920c759ed_5.png)

![](img/bb2868721acf3cb84984bb0920c759ed_6.png)

![](img/bb2868721acf3cb84984bb0920c759ed_8.png)

![](img/bb2868721acf3cb84984bb0920c759ed_10.png)

![](img/bb2868721acf3cb84984bb0920c759ed_12.png)

![](img/bb2868721acf3cb84984bb0920c759ed_13.png)

![](img/bb2868721acf3cb84984bb0920c759ed_15.png)

![](img/bb2868721acf3cb84984bb0920c759ed_17.png)

![](img/bb2868721acf3cb84984bb0920c759ed_19.png)

在本节课中，我们将对之前布置的两个SSRF漏洞实战考核题目进行详细讲解。我们将一起回顾SSRF漏洞的利用思路，学习如何探测内网服务，并掌握针对特定服务（如Struts2、Redis）的进阶攻击方法。

![](img/bb2868721acf3cb84984bb0920c759ed_21.png)

![](img/bb2868721acf3cb84984bb0920c759ed_23.png)

![](img/bb2868721acf3cb84984bb0920c759ed_25.png)

![](img/bb2868721acf3cb84984bb0920c759ed_27.png)

![](img/bb2868721acf3cb84984bb0920c759ed_29.png)

![](img/bb2868721acf3cb84984bb0920c759ed_31.png)

![](img/bb2868721acf3cb84984bb0920c759ed_32.png)

![](img/bb2868721acf3cb84984bb0920c759ed_34.png)

---

## 概述

![](img/bb2868721acf3cb84984bb0920c759ed_36.png)

![](img/bb2868721acf3cb84984bb0920c759ed_38.png)

SSRF（服务器端请求伪造）漏洞允许攻击者诱使服务器向内部或外部的任意地址发起请求。本节课我们将通过两个实战题目，系统性地学习如何利用SSRF漏洞进行内网探测、服务识别和漏洞利用，最终获取目标系统的控制权。

![](img/bb2868721acf3cb84984bb0920c759ed_40.png)

![](img/bb2868721acf3cb84984bb0920c759ed_42.png)

上一节我们介绍了SSRF漏洞的基本原理和常见利用协议，本节中我们来看看如何将这些知识应用于复杂的实战场景。

![](img/bb2868721acf3cb84984bb0920c759ed_43.png)

![](img/bb2868721acf3cb84984bb0920c759ed_45.png)

![](img/bb2868721acf3cb84984bb0920c759ed_47.png)

![](img/bb2868721acf3cb84984bb0920c759ed_48.png)

![](img/bb2868721acf3cb84984bb0920c759ed_49.png)

---

![](img/bb2868721acf3cb84984bb0920c759ed_51.png)

![](img/bb2868721acf3cb84984bb0920c759ed_52.png)

![](img/bb2868721acf3cb84984bb0920c759ed_53.png)

## 题目一：有回显的SSRF利用

第一个题目是一个存在SSRF漏洞的Web应用，目标是读取服务器上的`flag`文件。题目过滤了`file://`协议，因此需要寻找其他利用路径。

### 漏洞探测与信息收集

首先，我们需要测试服务器支持的协议。输入一个公网URL（如`http://www.baidu.com`）测试，发现服务器会返回该URL的页面源码，这证实了SSRF漏洞的存在。

以下是探测服务器支持协议和收集内网信息的步骤：

![](img/bb2868721acf3cb84984bb0920c759ed_55.png)

![](img/bb2868721acf3cb84984bb0920c759ed_57.png)

![](img/bb2868721acf3cb84984bb0920c759ed_59.png)

![](img/bb2868721acf3cb84984bb0920c759ed_61.png)

1.  **测试Dict协议**：使用`dict://`协议探测服务器的端口和服务信息。例如，`dict://<公网IP>:22`可以测试SSH服务。
2.  **探测内网端口**：确认支持`dict`协议后，可以构造请求探测内网存活端口。例如：`dict://127.0.0.1:22`。
3.  **批量端口扫描**：手动测试效率低下，可以使用Burp Suite的Intruder模块对内网IP的常用端口进行爆破，通过响应差异判断端口开放情况。

通过爆破，我们发现了目标内网开放了以下关键端口：
*   **22端口**：SSH服务
*   **3306端口**：MySQL数据库
*   **5002端口**：未知Web服务
*   **8080端口**：Tomcat服务 (运行Struts2框架)

![](img/bb2868721acf3cb84984bb0920c759ed_63.png)

![](img/bb2868721acf3cb84984bb0920c759ed_65.png)

![](img/bb2868721acf3cb84984bb0920c759ed_67.png)

![](img/bb2868721acf3cb84984bb0920c759ed_69.png)

![](img/bb2868721acf3cb84984bb0920c759ed_71.png)

![](img/bb2868721acf3cb84984bb0920c759ed_73.png)

![](img/bb2868721acf3cb84984bb0920c759ed_74.png)

### 漏洞利用与攻击链构建

![](img/bb2868721acf3cb84984bb0920c759ed_76.png)

![](img/bb2868721acf3cb84984bb0920c759ed_78.png)

![](img/bb2868721acf3cb84984bb0920c759ed_80.png)

探测到8080端口运行着Struts2框架后，我们的攻击思路变得清晰。

1.  **识别具体漏洞**：访问`http://127.0.0.1:8080`，根据页面特征确认是Struts2。查找已知的Struts2漏洞POC进行测试。
2.  **命令执行**：使用Struts2 S2-032漏洞的POC，成功在目标服务器上执行了`whoami`命令，确认当前用户为`root`，具备高权限。
    *   **POC示例**：
        ```http
        GET /index.action?method:%23_memberAccess%3d@ognl.OgnlContext@DEFAULT_MEMBER_ACCESS,%23res%3d%40org.apache.struts2.ServletActionContext%40getResponse(),%23res.setCharacterEncoding(%23parameters.encoding[0]),%23w%3d%23res.getWriter(),%23s%3dnew+java.util.Scanner(@java.lang.Runtime@getRuntime().exec(%23parameters.cmd[0]).getInputStream()).useDelimiter(%23parameters.pp[0]),%23str%3d%23s.hasNext()%3f%23s.next()%3a%23parameters.ppp[0],%23w.print(%23str),%23w.close(),1?%23xx:%23request.toString&pp=%5C%5CA&ppp=%20&encoding=UTF-8&cmd=whoami HTTP/1.1
        ```
3.  **获取Shell**：由于是内网服务，我们无法直接连接。因此，需要利用命令执行功能反弹一个Shell到我们的公网服务器。
    *   在公网服务器监听端口：`nc -lvnp 4444`
    *   通过SSRF发送Struts2 POC，执行反弹Shell命令（例如使用bash或python）。

**核心思路总结**：本题通过SSRF漏洞的`dict`协议进行内网端口探测，发现Struts2服务后，利用其远程命令执行漏洞反弹Shell，从而穿透网络边界，实现对内网系统的控制。

![](img/bb2868721acf3cb84984bb0920c759ed_82.png)

![](img/bb2868721acf3cb84984bb0920c759ed_84.png)

![](img/bb2868721acf3cb84984bb0920c759ed_86.png)

![](img/bb2868721acf3cb84984bb0920c759ed_88.png)

---

## 题目二：无回显SSRF与Redis未授权访问

第二个题目同样存在SSRF，但无直接回显。目标是利用该漏洞攻击内网的Redis服务。

![](img/bb2868721acf3cb84984bb0920c759ed_90.png)

### 信息收集与协议确认

![](img/bb2868721acf3cb84984bb0920c759ed_92.png)

首先重复信息收集步骤，通过爆破发现内网开放了80、3306和**6379（Redis）**端口。

![](img/bb2868721acf3cb84984bb0920c759ed_94.png)

![](img/bb2868721acf3cb84984bb0920c759ed_96.png)

![](img/bb2868721acf3cb84984bb0920c759ed_98.png)

1.  **测试Gopher协议**：Redis攻击常利用Gopher协议发送Payload。需要先测试目标服务器是否支持`gopher://`协议。
2.  **确认Redis未授权访问**：通过`dict`协议连接`dict://127.0.0.1:6379`，如果返回Redis版本信息，则可能存在未授权访问漏洞。

![](img/bb2868721acf3cb84984bb0920c759ed_100.png)

![](img/bb2868721acf3cb84984bb0920c759ed_101.png)

![](img/bb2868721acf3cb84984bb0920c759ed_103.png)

### Redis未授权访问利用方法

确认Redis可被攻击后，有以下几种常见的GetShell方法：

![](img/bb2868721acf3cb84984bb0920c759ed_105.png)

![](img/bb2868721acf3cb84984bb0920c759ed_107.png)

![](img/bb2868721acf3cb84984bb0920c759ed_109.png)

![](img/bb2868721acf3cb84984bb0920c759ed_110.png)

![](img/bb2868721acf3cb84984bb0920c759ed_112.png)

**1. 写入SSH公钥**
   前提：目标Redis服务运行在root用户下，且`~/.ssh/`目录存在。
   *   在攻击机生成SSH密钥对：`ssh-keygen -t rsa`
   *   将公钥写入一个文件（如`key.txt`），构造Redis命令，将公钥内容写入目标服务器的`authorized_keys`文件。
   *   使用Gopher协议发送构造好的Redis命令Payload。

![](img/bb2868721acf3cb84984bb0920c759ed_114.png)

![](img/bb2868721acf3cb84984bb0920c759ed_116.png)

![](img/bb2868721acf3cb84984bb0920c759ed_118.png)

![](img/bb2868721acf3cb84984bb0920c759ed_120.png)

**2. 写入WebShell**
   前提：知道目标Web服务器的绝对路径。
   *   构造Redis命令，将一句话木马写入Web目录下的文件（如`shell.php`）。
   *   使用Gopher协议发送Payload。

![](img/bb2868721acf3cb84984bb0920c759ed_122.png)

**3. 写入定时任务（Crontab）反弹Shell**
   这是最通用有效的方法。
   *   构造Redis命令，将反弹Shell的命令写入`/var/spool/cron/root`（或`/var/spool/cron/crontabs/root`）文件。
   *   使用Gopher协议发送Payload。

![](img/bb2868721acf3cb84984bb0920c759ed_124.png)

![](img/bb2868721acf3cb84984bb0920c759ed_126.png)

### Payload构造与Gopher协议利用

![](img/bb2868721acf3cb84984bb0920c759ed_128.png)

![](img/bb2868721acf3cb84984bb0920c759ed_130.png)

![](img/bb2868721acf3cb84984bb0920c759ed_132.png)

![](img/bb2868721acf3cb84984bb0920c759ed_134.png)

Gopher协议可以发送封装好的TCP数据包。攻击Redis的关键在于将Redis命令转换为Gopher可发送的格式。

![](img/bb2868721acf3cb84984bb0920c759ed_136.png)

![](img/bb2868721acf3cb84984bb0920c759ed_138.png)

![](img/bb2868721acf3cb84984bb0920c759ed_139.png)

![](img/bb2868721acf3cb84984bb0920c759ed_141.png)

![](img/bb2868721acf3cb84984bb0920c759ed_143.png)

![](img/bb2868721acf3cb84984bb0920c759ed_144.png)

以下是转换步骤的简要说明：
1.  将Redis命令行交互过程抓包保存。
2.  对抓取的数据进行处理：
    *   删除无关行（如以`>`、`<`、`+OK`开头的行）。
    *   将换行符`\r\n`替换为URL编码`%0D%0A`。
    *   将所有行连接成一行字符串。
3.  最终Payload格式为：`gopher://127.0.0.1:6379/_[处理后的Payload字符串]`

![](img/bb2868721acf3cb84984bb0920c759ed_146.png)

![](img/bb2868721acf3cb84984bb0920c759ed_148.png)

![](img/bb2868721acf3cb84984bb0920c759ed_150.png)

![](img/bb2868721acf3cb84984bb0920c759ed_152.png)

![](img/bb2868721acf3cb84984bb0920c759ed_154.png)

**工具辅助**：可以使用现成的脚本（如`Gopherus`）自动生成攻击Redis的Gopher Payload，支持写入SSH密钥、WebShell、定时任务等多种方式。
*   **命令示例**：`python gopherus.py --exploit redis`

![](img/bb2868721acf3cb84984bb0920c759ed_155.png)

![](img/bb2868721acf3cb84984bb0920c759ed_157.png)

![](img/bb2868721acf3cb84984bb0920c759ed_159.png)

**本题总结**：本题利用SSRF配合Gopher协议，攻击内网存在未授权访问漏洞的Redis服务。通过写入定时任务的方式反弹Shell，是内网穿透中非常经典的攻击链。

---

## 课程总结与进阶学习

本节课中我们一起学习了SSRF漏洞在复杂场景下的综合利用。我们掌握了以下核心技能：
1.  **系统化的探测**：从协议测试到内网端口扫描，逐步绘制内网资产地图。
2.  **服务识别与漏洞利用**：针对识别出的服务（如Struts2、Redis），快速查找并利用已知漏洞。
3.  **攻击链构建**：将多个漏洞串联，从外网SSRF突破边界，到内网横向移动，最终获取权限。

![](img/bb2868721acf3cb84984bb0920c759ed_161.png)

Web安全是渗透测试的入口，而完整的渗透测试还涉及后续的内网渗透、权限提升、权限维持和痕迹清理等更广阔的领域。希望本课程为你打下了坚实的基础，助你在网络安全道路上继续深入探索。

![](img/bb2868721acf3cb84984bb0920c759ed_163.png)

![](img/bb2868721acf3cb84984bb0920c759ed_165.png)

---
**注**：本文档根据课程视频内容整理，侧重于思路讲解和方法论。实战中题目环境可能发生变化，但核心原理不变。所有技术仅用于合法安全测试和学习研究。