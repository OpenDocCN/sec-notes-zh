![](img/155a20681598ba0e71ba588653cd80fc_1.png)

![](img/155a20681598ba0e71ba588653cd80fc_3.png)

# 网络安全就业推荐 - P13：第11天：SQL注入漏洞-sqlmap使用，报错注入 🛡️💉

在本节课中，我们将要学习一个非常强大的自动化工具——sqlmap。它能够帮助我们自动检测和利用SQL注入漏洞，极大地简化了手工注入的繁琐过程。我们将从sqlmap的基本介绍开始，逐步学习其常用命令、高级用法，并了解如何通过编写tamper脚本来绕过网站防火墙（WAF）。

![](img/155a20681598ba0e71ba588653cd80fc_5.png)

![](img/155a20681598ba0e71ba588653cd80fc_7.png)

## 什么是sqlmap？🔍

![](img/155a20681598ba0e71ba588653cd80fc_9.png)

sqlmap是一个开源的渗透测试工具，主要用于自动化检测和利用SQL注入漏洞。这个工具历史悠久，但至今仍在持续更新，功能非常强大。

![](img/155a20681598ba0e71ba588653cd80fc_11.png)

![](img/155a20681598ba0e71ba588653cd80fc_13.png)

它的核心作用是解决手工SQL注入的繁琐问题。当你需要从数据库中提取大量数据时，手工编写和执行注入语句几乎是不可能的任务。sqlmap通过自动化这个过程，可以快速、高效地完成数据库信息探测和数据提取。

![](img/155a20681598ba0e71ba588653cd80fc_15.png)

**下载与安装**
sqlmap是一个Python脚本，你可以从其GitHub仓库下载。无论是Python 2还是Python 3环境都可以运行。
```bash
git clone --depth 1 https://github.com/sqlmapproject/sqlmap.git sqlmap-dev
```
解压后，在命令行中进入目录并运行`python sqlmap.py`，如果出现sqlmap的logo，则说明安装成功。

为了方便使用，建议将sqlmap的路径添加到系统的环境变量（PATH）中。这样，你就可以在任何目录下直接使用`sqlmap.py`命令了。

## sqlmap基础使用 🚀

![](img/155a20681598ba0e71ba588653cd80fc_17.png)

上一节我们介绍了sqlmap是什么，本节中我们来看看它的基本使用方法。我们将通过一个简单的靶场来演示。

首先，最基础的命令是使用`-u`参数指定一个可能存在注入的URL。
```bash
python sqlmap.py -u "http://target.com/page.php?id=1"
```
运行后，sqlmap会尝试检测该URL是否存在SQL注入点。如果发现注入点，它会提示你并询问是否跳过检测其他参数。

检测到注入点后，我们就可以按照标准的SQL注入流程来获取数据了。这个流程通常是：获取数据库名 -> 获取表名 -> 获取列名 -> 获取数据。

![](img/155a20681598ba0e71ba588653cd80fc_19.png)

以下是sqlmap中对应这些步骤的常用命令：

![](img/155a20681598ba0e71ba588653cd80fc_21.png)

![](img/155a20681598ba0e71ba588653cd80fc_23.png)

*   **`--dbs`**: 枚举所有数据库。
*   **`-D database_name --tables`**: 枚举指定数据库中的所有表。
*   **`-D database_name -T table_name --columns`**: 枚举指定表中的所有列。
*   **`-D database_name -T table_name -C column1,column2 --dump`**: 导出指定列的数据。

![](img/155a20681598ba0e71ba588653cd80fc_25.png)

![](img/155a20681598ba0e71ba588653cd80fc_27.png)

使用这些命令，你可以在几秒钟内完成对一个简单注入点的完整利用，获取数据库中的所有信息。

![](img/155a20681598ba0e71ba588653cd80fc_29.png)

![](img/155a20681598ba0e71ba588653cd80fc_31.png)

## 处理POST请求与高级参数 🧩

![](img/155a20681598ba0e71ba588653cd80fc_32.png)

![](img/155a20681598ba0e71ba588653cd80fc_34.png)

![](img/155a20681598ba0e71ba588653cd80fc_36.png)

在实际场景中，注入点可能存在于POST请求中，或者需要特定的HTTP头。直接使用`-u`参数可能无法成功检测。

![](img/155a20681598ba0e71ba588653cd80fc_38.png)

![](img/155a20681598ba0e71ba588653cd80fc_40.png)

这里教大家一个万能的方法：配合Burp Suite（BP）使用。

1.  使用BP拦截存在POST注入的HTTP请求包。
2.  在BP中右键点击该请求，选择 `Copy to file`，将请求保存为一个文本文件（例如 `post.txt`）。
3.  使用sqlmap的`-r`参数来读取这个文件。
```bash
python sqlmap.py -r post.txt
```
这样，sqlmap就会根据文件中的完整请求（包括方法、头、参数）进行注入测试。

此外，还有一些重要的高级参数需要了解：

*   **`-p`**: 指定需要测试的参数。例如 `-p “username,password”`。
*   **`--proxy`**: 设置代理，让sqlmap的流量经过BP，方便我们观察其Payload。例如 `--proxy=http://127.0.0.1:8080`。
*   **`--level` 和 `--risk`**: 提高检测的级别和风险等级。默认级别为1，风险为1。提高它们（如 `--level=3 --risk=3`）会让sqlmap进行更深入、更全面的测试，但耗时也更长。
*   **`--os-shell`**: 在特定条件下，尝试获取操作系统的命令行shell。这通常意味着漏洞危害等级从“高危”提升到了“严重”。

## 编写Tamper脚本绕过WAF 🛡️➡️🚪

![](img/155a20681598ba0e71ba588653cd80fc_42.png)

![](img/155a20681598ba0e71ba588653cd80fc_43.png)

在上一节我们学习了基本和高级用法，但在真实网络中，网站往往部署了Web应用防火墙（WAF），会拦截常见的SQL注入Payload。本节中我们来看看如何利用tamper脚本来绕过WAF。

![](img/155a20681598ba0e71ba588653cd80fc_45.png)

![](img/155a20681598ba0e71ba588653cd80fc_47.png)

tamper脚本可以理解为sqlmap的“规则修改器”。它能在sqlmap发送Payload之前，按照我们设定的规则对Payload进行变形，从而绕过WAF的检测。

![](img/155a20681598ba0e71ba588653cd80fc_49.png)

例如，如果WAF过滤了`select`关键字，我们可以编写一个tamper脚本，将`select`替换为`SeLeCt`（大小写混淆）或`SELSELECTECT`（双写绕过）。

![](img/155a20681598ba0e71ba588653cd80fc_51.png)

![](img/155a20681598ba0e71ba588653cd80fc_52.png)

![](img/155a20681598ba0e71ba588653cd80fc_54.png)

**如何使用tamper脚本？**
使用`--tamper`参数指定脚本即可。
```bash
python sqlmap.py -u “http://target.com?id=1” --tamper=my_tamper.py
```

![](img/155a20681598ba0e71ba588653cd80fc_56.png)

![](img/155a20681598ba0e71ba588653cd80fc_58.png)

![](img/155a20681598ba0e71ba588653cd80fc_60.png)

**如何编写tamper脚本？**
sqlmap自带了许多tamper脚本（位于`tamper/`目录），我们可以参考它们进行编写。一个简单的tamper脚本结构如下：
```python
#!/usr/bin/env python
from lib.core.enums import PRIORITY

![](img/155a20681598ba0e71ba588653cd80fc_62.png)

![](img/155a20681598ba0e71ba588653cd80fc_64.png)

![](img/155a20681598ba0e71ba588653cd80fc_66.png)

![](img/155a20681598ba0e71ba588653cd80fc_68.png)

__priority__ = PRIORITY.NORMAL

def dependencies():
    pass

![](img/155a20681598ba0e71ba588653cd80fc_70.png)

![](img/155a20681598ba0e71ba588653cd80fc_72.png)

def tamper(payload, **kwargs):
    # 这里是核心的变形逻辑
    # 例如，将空格替换为注释
    retVal = payload.replace(” “, “/**/“)
    return retVal
```
你可以根据手工测试时发现的绕过方法（例如，用`%0a`换行符代替空格，用`like`代替`=`），将这些规则写入tamper脚本。之后，使用这个脚本运行sqlmap，它就会自动应用这些绕过规则。

![](img/155a20681598ba0e71ba588653cd80fc_74.png)

![](img/155a20681598ba0e71ba588653cd80fc_76.png)

![](img/155a20681598ba0e71ba588653cd80fc_78.png)

![](img/155a20681598ba0e71ba588653cd80fc_80.png)

## 总结 📚

![](img/155a20681598ba0e71ba588653cd80fc_82.png)

![](img/155a20681598ba0e71ba588653cd80fc_84.png)

![](img/155a20681598ba0e71ba588653cd80fc_86.png)

本节课中我们一起学习了自动化SQL注入工具sqlmap的核心用法。

我们首先了解了sqlmap的基本概念和安装方法。接着，学习了其基础命令，掌握了从检测注入点到拖取数据的完整流程。然后，我们探讨了如何处理POST请求等复杂场景，并介绍了一些重要的高级参数。最后，我们深入讲解了tamper脚本的作用和编写方法，这是应对WAF的关键技能。

![](img/155a20681598ba0e71ba588653cd80fc_88.png)

记住，工具虽然强大，但理解SQL注入的原理才是根本。sqlmap是一个帮助你提高效率的工具，而不是替代你思考的“黑盒”。课后请务必使用提供的靶场进行手动练习，熟悉整个流程，并尝试理解sqlmap发送的每一个Payload。