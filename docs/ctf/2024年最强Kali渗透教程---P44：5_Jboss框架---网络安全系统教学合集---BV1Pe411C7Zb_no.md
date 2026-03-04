# Kali渗透教程：P44：Jboss框架识别与漏洞利用

![](img/b793a0c679a5bbe3dc6cda956e7b015a_1.png)

![](img/b793a0c679a5bbe3dc6cda956e7b015a_2.png)

在本节课中，我们将学习如何识别使用Jboss框架的网站，并了解针对其特定漏洞的检测与利用方法。课程将涵盖Jboss的识别特征、批量检测脚本的使用以及一个命令执行漏洞的利用过程。

![](img/b793a0c679a5bbe3dc6cda956e7b015a_4.png)

![](img/b793a0c679a5bbe3dc6cda956e7b015a_6.png)

## Jboss框架的识别特征 🔍

![](img/b793a0c679a5bbe3dc6cda956e7b015a_8.png)

![](img/b793a0c679a5bbe3dc6cda956e7b015a_10.png)

上一节我们介绍了Jboss框架的基本概念，本节中我们来看看如何识别一个网站是否使用了Jboss。

![](img/b793a0c679a5bbe3dc6cda956e7b015a_12.png)

![](img/b793a0c679a5bbe3dc6cda956e7b015a_13.png)

![](img/b793a0c679a5bbe3dc6cda956e7b015a_15.png)

Jboss应用服务器默认将管理控制台（Web Console）开放于**8080**端口，并使用**HTTP**协议进行通信。其默认访问页面具有特定的样式。

![](img/b793a0c679a5bbe3dc6cda956e7b015a_17.png)

以下是Jboss常见的默认页面示例，不同版本界面有所不同，但通常包含“JBoss”标识：

![](img/b793a0c679a5bbe3dc6cda956e7b015a_1.png)
*（Jboss 6.x 版本默认页面）*

![](img/b793a0c679a5bbe3dc6cda956e7b015a_8.png)
*（Jboss 7.x 版本默认页面）*

通过端口扫描（如使用Nmap）或网络空间搜索引擎（如FoFa、Shodan）搜索特定特征，也可以发现Jboss服务。搜索关键词可包含 `port:8080` 和 `title:“JBoss”`。

![](img/b793a0c679a5bbe3dc6cda956e7b015a_19.png)

## 批量漏洞检测脚本的使用 🛠️

![](img/b793a0c679a5bbe3dc6cda956e7b015a_21.png)

![](img/b793a0c679a5bbe3dc6cda956e7b015a_23.png)

识别出Jboss服务后，下一步是检测其是否存在已知漏洞。我们将使用一个Python编写的批量检测脚本。

![](img/b793a0c679a5bbe3dc6cda956e7b015a_25.png)

该脚本主要包含两个文件：`target.txt` 和 `jboss_scan.py`。使用时，需要将待检测的目标地址和端口按行填入 `target.txt` 文件。

![](img/b793a0c679a5bbe3dc6cda956e7b015a_27.png)

以下是目标文件 `target.txt` 的格式示例：
```
192.168.1.100:8080
10.0.0.5:8080
example.com:8080
```

执行脚本时，需注意Python环境版本。该脚本通常需要 **Python 3** 环境运行。如果系统默认 `python` 命令指向Python 2，则需要使用 `python3` 命令。

以下是执行扫描的命令：
```bash
python3 jboss_scan.py
```
脚本运行后，会输出检测结果，可能提示存在以下漏洞：
1.  **JBoss 控制台未授权访问**
2.  **CVE-2015-7501** 反序列化漏洞
3.  **JMX Console** 未授权访问漏洞
4.  **CVE-2017-12149** 反序列化漏洞

![](img/b793a0c679a5bbe3dc6cda956e7b015a_29.png)

## CVE-2017-12149 漏洞分析与利用 ⚡

![](img/b793a0c679a5bbe3dc6cda956e7b015a_31.png)

![](img/b793a0c679a5bbe3dc6cda956e7b015a_33.png)

在批量检测结果中，如果提示存在 **CVE-2017-12149** 漏洞，则可以进行深入利用。该漏洞是Jboss中一个Java反序列化错误，位于HTTP Invoker组件的过滤器中。

![](img/b793a0c679a5bbe3dc6cda956e7b015a_35.png)

![](img/b793a0c679a5bbe3dc6cda956e7b015a_37.png)

### 漏洞验证
手动验证漏洞是否存在，可以访问以下URL路径：
```
http://<目标IP>:8080/invoker/readonly
```
如果服务器返回 **HTTP 500** 状态码，则表明该端点存在，且很可能存在漏洞。

### 漏洞利用（获取Shell）
此漏洞可导致远程命令执行，攻击者能够获取服务器权限。我们将使用一个公开的漏洞利用脚本进行反弹Shell。

![](img/b793a0c679a5bbe3dc6cda956e7b015a_39.png)

![](img/b793a0c679a5bbe3dc6cda956e7b015a_41.png)

利用步骤如下：
1.  **在攻击机上监听端口**：使用Netcat工具监听一个端口，准备接收反弹回来的Shell连接。
    ```bash
    nc -lvnp 1234
    ```
    *命令解释：`-l` 监听模式，`-v` 详细输出，`-n` 不解析域名，`-p` 指定端口。*

2.  **执行漏洞利用脚本**：运行利用脚本，指定目标URL和反弹Shell的接收地址。
    ```bash
    python2 jboss_CVE-2017-12149.py -u http://<目标IP>:8080
    ```
    *注意：此示例脚本可能需要Python 2环境，部分利用脚本支持Python 3，请根据脚本说明选择正确的解释器。*

![](img/b793a0c679a5bbe3dc6cda956e7b015a_43.png)

![](img/b793a0c679a5bbe3dc6cda956e7b015a_45.png)

3.  **交互与攻击**：脚本执行后，会提示输入反弹Shell的接收IP和端口（即攻击机的公网IP和监听的1234端口）。输入正确信息后，脚本会向目标服务器发送攻击载荷。

![](img/b793a0c679a5bbe3dc6cda956e7b015a_47.png)

![](img/b793a0c679a5bbe3dc6cda956e7b015a_49.png)

![](img/b793a0c679a5bbe3dc6cda956e7b015a_51.png)

4.  **获取Shell**：如果攻击成功，在Netcat监听窗口会接收到来自目标服务器的Shell连接，从而可以执行系统命令。

![](img/b793a0c679a5bbe3dc6cda956e7b015a_53.png)

![](img/b793a0c679a5bbe3dc6cda956e7b015a_55.png)

![](img/b793a0c679a5bbe3dc6cda956e7b015a_57.png)

**重要提示**：在实际渗透测试中，必须在获得明确授权的前提下进行所有操作。未经授权的攻击是违法行为。

![](img/b793a0c679a5bbe3dc6cda956e7b015a_59.png)

![](img/b793a0c679a5bbe3dc6cda956e7b015a_60.png)

## 总结 📝

![](img/b793a0c679a5bbe3dc6cda956e7b015a_62.png)

![](img/b793a0c679a5bbe3dc6cda956e7b015a_64.png)

![](img/b793a0c679a5bbe3dc6cda956e7b015a_66.png)

本节课中我们一起学习了Jboss框架的识别与漏洞利用。
*   我们首先了解了Jboss的默认端口（**8080**）和识别其服务的多种方法。
*   接着，我们学习了如何使用Python脚本对多个Jboss目标进行批量漏洞扫描。
*   最后，我们深入分析了 **CVE-2017-12149** 反序列化漏洞，并演示了从验证到利用该漏洞获取服务器Shell的完整过程。

![](img/b793a0c679a5bbe3dc6cda956e7b015a_68.png)

掌握这些技能有助于在授权的安全评估中，有效地发现和验证Jboss服务器的安全风险。请务必遵守法律法规，将技术用于正当的网络安全防御工作。