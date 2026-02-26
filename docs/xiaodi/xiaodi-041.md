#  041：ASP应用安全攻防基础 🔍

![](img/417bd6c721484af32634ece3a6cedfe1_0.png)

在本节课中，我们将学习ASP（Active Server Pages）应用相关的安全基础知识。ASP是一种较早期的服务器端脚本技术，虽然现在已不常见，但在一些遗留系统中仍可能存在。了解其安全特性有助于我们应对可能遇到的老旧系统。

![](img/417bd6c721484af32634ece3a6cedfe1_2.png)

![](img/417bd6c721484af32634ece3a6cedfe1_4.png)

![](img/417bd6c721484af32634ece3a6cedfe1_6.png)

![](img/417bd6c721484af32634ece3a6cedfe1_8.png)

![](img/417bd6c721484af32634ece3a6cedfe1_9.png)

我们将从ASP与Access数据库的典型组合开始，探讨由此引发的默认数据库泄露问题。接着，我们会分析运行ASP的IIS（Internet Information Services）中间件上的一些经典漏洞，如HTTP.SYS远程蓝屏漏洞、短文件名信息泄露和文件解析漏洞。最后，我们会通过一个实战案例，演示如何利用Access数据库注入漏洞获取后台权限，并结合文件上传与解析漏洞获取服务器控制权。

![](img/417bd6c721484af32634ece3a6cedfe1_11.png)

![](img/417bd6c721484af32634ece3a6cedfe1_13.png)

![](img/417bd6c721484af32634ece3a6cedfe1_15.png)

![](img/417bd6c721484af32634ece3a6cedfe1_17.png)

![](img/417bd6c721484af32634ece3a6cedfe1_18.png)

![](img/417bd6c721484af32634ece3a6cedfe1_20.png)

![](img/417bd6c721484af32634ece3a6cedfe1_22.png)

---

![](img/417bd6c721484af32634ece3a6cedfe1_24.png)

![](img/417bd6c721484af32634ece3a6cedfe1_26.png)

![](img/417bd6c721484af32634ece3a6cedfe1_28.png)

![](img/417bd6c721484af32634ece3a6cedfe1_30.png)

![](img/417bd6c721484af32634ece3a6cedfe1_32.png)

## ASP与Access数据库的默认搭建风险 💾

![](img/417bd6c721484af32634ece3a6cedfe1_34.png)

![](img/417bd6c721484af32634ece3a6cedfe1_36.png)

![](img/417bd6c721484af32634ece3a6cedfe1_38.png)

![](img/417bd6c721484af32634ece3a6cedfe1_40.png)

![](img/417bd6c721484af32634ece3a6cedfe1_42.png)

![](img/417bd6c721484af32634ece3a6cedfe1_43.png)

![](img/417bd6c721484af32634ece3a6cedfe1_45.png)

![](img/417bd6c721484af32634ece3a6cedfe1_47.png)

上一节我们概述了课程内容，本节中我们来看看ASP应用最常见的一种架构组合及其带来的安全问题。

![](img/417bd6c721484af32634ece3a6cedfe1_49.png)

![](img/417bd6c721484af32634ece3a6cedfe1_51.png)

![](img/417bd6c721484af32634ece3a6cedfe1_53.png)

![](img/417bd6c721484af32634ece3a6cedfe1_55.png)

由于ASP程序常与Access数据库搭配使用，这种组合多见于中小型Web应用。Access数据库的一个关键特性是它无需像MySQL那样进行独立的连接配置（如设置账号、密码、数据库名）。开发者通常在脚本文件中直接定义好数据库文件的路径（例如 `database/#data.mdb`），程序启动时便会自动使用该文件。

![](img/417bd6c721484af32634ece3a6cedfe1_57.png)

![](img/417bd6c721484af32634ece3a6cedfe1_59.png)

**代码示例：数据库连接配置**
```asp
<%
db_path = "../database/#data.mdb"
Set conn = Server.CreateObject("ADODB.Connection")
conn.Open "Provider=Microsoft.Jet.OLEDB.4.0;Data Source=" & Server.MapPath(db_path)
%>
```

![](img/417bd6c721484af32634ece3a6cedfe1_61.png)

![](img/417bd6c721484af32634ece3a6cedfe1_63.png)

这种“开箱即用”的便利性也带来了安全隐患。如果开发者在部署时没有修改默认的数据库路径和文件名，攻击者便可能直接通过构造URL来下载数据库文件。

![](img/417bd6c721484af32634ece3a6cedfe1_65.png)

![](img/417bd6c721484af32634ece3a6cedfe1_67.png)

![](img/417bd6c721484af32634ece3a6cedfe1_69.png)

![](img/417bd6c721484af32634ece3a6cedfe1_70.png)

**风险成因：**
1.  数据库文件（.mdb）通常存放在网站目录下。
2.  连接路径在源码中固定，部署时容易被忽略修改。
3.  Access数据库文件无需密码即可打开，其中可能包含管理员账号密码等敏感信息。

![](img/417bd6c721484af32634ece3a6cedfe1_72.png)

![](img/417bd6c721484af32634ece3a6cedfe1_74.png)

![](img/417bd6c721484af32634ece3a6cedfe1_76.png)

以下是利用此风险的基本步骤：
1.  通过信息收集或猜测，确定目标网站使用某款已知的ASP开源程序。
2.  获取该程序的源码，找到其默认的数据库路径（如 `/database/#data.mdb`）。
3.  尝试直接访问该路径（注意URL编码，`#` 需编码为 `%23`），下载数据库文件。
4.  使用Office Access或相关工具打开.mdb文件，查找管理员表（如 `admin`），获取账号密码。

![](img/417bd6c721484af32634ece3a6cedfe1_78.png)

![](img/417bd6c721484af32634ece3a6cedfe1_80.png)

> **注意**：此漏洞的利用前提是网站采用默认配置未作修改。但许多缺乏经验的管理员或开发者确实会保持默认设置。

---

![](img/417bd6c721484af32634ece3a6cedfe1_82.png)

![](img/417bd6c721484af32634ece3a6cedfe1_84.png)

## IIS中间件相关漏洞 ⚙️

![](img/417bd6c721484af32634ece3a6cedfe1_86.png)

![](img/417bd6c721484af32634ece3a6cedfe1_88.png)

![](img/417bd6c721484af32634ece3a6cedfe1_90.png)

![](img/417bd6c721484af32634ece3a6cedfe1_92.png)

了解了ASP应用的自身特点后，我们来看看承载它的IIS服务器本身存在的一些历史漏洞。这些漏洞虽然年份较久，但在未及时更新的老旧系统上依然可能存在。

![](img/417bd6c721484af32634ece3a6cedfe1_94.png)

![](img/417bd6c721484af32634ece3a6cedfe1_96.png)

![](img/417bd6c721484af32634ece3a6cedfe1_98.png)

![](img/417bd6c721484af32634ece3a6cedfe1_100.png)

![](img/417bd6c721484af32634ece3a6cedfe1_102.png)

![](img/417bd6c721484af32634ece3a6cedfe1_104.png)

### HTTP.SYS远程蓝屏漏洞（MS15-034）💥

![](img/417bd6c721484af32634ece3a6cedfe1_106.png)

![](img/417bd6c721484af32634ece3a6cedfe1_108.png)

这是一个2015年披露的Windows系统层漏洞，影响IIS。攻击者可以发送特制的HTTP请求，导致目标服务器系统崩溃、蓝屏并重启。该漏洞属于拒绝服务（DoS）型，虽然不能直接获取权限，但破坏性较强。

![](img/417bd6c721484af32634ece3a6cedfe1_110.png)

![](img/417bd6c721484af32634ece3a6cedfe1_112.png)

![](img/417bd6c721484af32634ece3a6cedfe1_113.png)

![](img/417bd6c721484af32634ece3a6cedfe1_115.png)

![](img/417bd6c721484af32634ece3a6cedfe1_117.png)

![](img/417bd6c721484af32634ece3a6cedfe1_119.png)

![](img/417bd6c721484af32634ece3a6cedfe1_121.png)

![](img/417bd6c721484af32634ece3a6cedfe1_123.png)

![](img/417bd6c721484af32634ece3a6cedfe1_125.png)

![](img/417bd6c721484af32634ece3a6cedfe1_127.png)

**影响版本**：Windows 7, Windows Server 2008 R2, Windows 8, Windows Server 2012等。

![](img/417bd6c721484af32634ece3a6cedfe1_129.png)

![](img/417bd6c721484af32634ece3a6cedfe1_131.png)

**检测与利用**：
1.  **检测**：使用 `curl` 命令发送特定请求头，根据返回状态判断。
    ```bash
    curl -v http://目标IP/ -H "Host: test" -H "Range: bytes=0-18446744073709551615"
    ```
    若返回 `416 Requested Range Not Satisfiable`，则可能存在漏洞。
2.  **利用**：使用Metasploit等渗透测试框架中的对应模块（如 `auxiliary/dos/http/ms15_034_ulonglongadd`）进行攻击。

![](img/417bd6c721484af32634ece3a6cedfe1_133.png)

![](img/417bd6c721484af32634ece3a6cedfe1_135.png)

![](img/417bd6c721484af32634ece3a6cedfe1_137.png)

![](img/417bd6c721484af32634ece3a6cedfe1_139.png)

> **警告**：此漏洞利用会导致目标服务中断，在授权测试中需谨慎使用。

![](img/417bd6c721484af32634ece3a6cedfe1_141.png)

![](img/417bd6c721484af32634ece3a6cedfe1_143.png)

![](img/417bd6c721484af32634ece3a6cedfe1_145.png)

![](img/417bd6c721484af32634ece3a6cedfe1_147.png)

![](img/417bd6c721484af32634ece3a6cedfe1_149.png)

![](img/417bd6c721484af32634ece3a6cedfe1_151.png)

![](img/417bd6c721484af32634ece3a6cedfe1_153.png)

### IIS短文件名泄露漏洞 📁

![](img/417bd6c721484af32634ece3a6cedfe1_155.png)

![](img/417bd6c721484af32634ece3a6cedfe1_157.png)

![](img/417bd6c721484af32634ece3a6cedfe1_159.png)

![](img/417bd6c721484af32634ece3a6cedfe1_161.png)

![](img/417bd6c721484af32634ece3a6cedfe1_163.png)

![](img/417bd6c721484af32634ece3a6cedfe1_165.png)

![](img/417bd6c721484af32634ece3a6cedfe1_167.png)

此漏洞允许攻击者猜解服务器上存在的文件和目录名（仅前6位字符），可用于探测后台管理路径、配置文件或数据库文件等敏感信息。

![](img/417bd6c721484af32634ece3a6cedfe1_169.png)

![](img/417bd6c721484af32634ece3a6cedfe1_171.png)

![](img/417bd6c721484af32634ece3a6cedfe1_173.png)

![](img/417bd6c721484af32634ece3a6cedfe1_175.png)

![](img/417bd6c721484af32634ece3a6cedfe1_177.png)

**漏洞原理**：Windows为了兼容旧的8.3文件名格式，会自动为长文件名创建短文件名（例如 `longfilename.txt` 对应 `LONGFI~1.TXT`）。IIS错误地暴露了通过短文件名访问文件的功能。

![](img/417bd6c721484af32634ece3a6cedfe1_179.png)

![](img/417bd6c721484af32634ece3a6cedfe1_181.png)

![](img/417bd6c721484af32634ece3a6cedfe1_183.png)

![](img/417bd6c721484af32634ece3a6cedfe1_185.png)

![](img/417bd6c721484af32634ece3a6cedfe1_187.png)

![](img/417bd6c721484af32634ece3a6cedfe1_189.png)

**利用工具**：
*   Java版扫描工具：`IIS Shortname Scanner`
*   Python版扫描工具：`iis-shortname-scanner`

![](img/417bd6c721484af32634ece3a6cedfe1_191.png)

![](img/417bd6c721484af32634ece3a6cedfe1_193.png)

![](img/417bd6c721484af32634ece3a6cedfe1_195.png)

![](img/417bd6c721484af32634ece3a6cedfe1_197.png)

![](img/417bd6c721484af32634ece3a6cedfe1_199.png)

![](img/417bd6c721484af32634ece3a6cedfe1_201.png)

以下是使用此类工具的基本流程：
1.  运行扫描工具，指定目标URL。
2.  工具会尝试枚举目录和文件，返回其短文件名形式（如 `ADMINI~1`）。
3.  攻击者根据前6位字符猜测完整的文件名（如 `admin_login.asp`），从而发现潜在的攻击入口。

![](img/417bd6c721484af32634ece3a6cedfe1_203.png)

![](img/417bd6c721484af32634ece3a6cedfe1_205.png)

![](img/417bd6c721484af32634ece3a6cedfe1_207.png)

### IIS文件解析漏洞 🧩

![](img/417bd6c721484af32634ece3a6cedfe1_209.png)

![](img/417bd6c721484af32634ece3a6cedfe1_211.png)

![](img/417bd6c721484af32634ece3a6cedfe1_212.png)

![](img/417bd6c721484af32634ece3a6cedfe1_214.png)

![](img/417bd6c721484af32634ece3a6cedfe1_216.png)

该漏洞主要影响IIS 6.0版本，存在两种利用方式，可将非可执行文件（如图片）当作ASP脚本解析执行。

**漏洞形式一：分号截断**
当文件路径为 `*.asp;.jpg` 时，IIS 6.0会忽略分号后的内容，将文件以 `.asp` 扩展名进行解析。
*   **示例**：上传文件 `shell.asp;.jpg`，服务器会将其作为 `shell.asp` 执行。

**漏洞形式二：目录解析**
在IIS 6.0中，如果目录名包含 `.asp`、`.asa`、`.cer` 等字符串，则该目录下的**所有文件**都会被当作ASP脚本解析。
*   **示例**：创建目录 `test.asp`，其中放入文件 `shell.jpg`。访问 `/test.asp/shell.jpg` 时，`shell.jpg` 会被当作ASP代码执行。

![](img/417bd6c721484af32634ece3a6cedfe1_218.png)

![](img/417bd6c721484af32634ece3a6cedfe1_220.png)

![](img/417bd6c721484af32634ece3a6cedfe1_222.png)

![](img/417bd6c721484af32634ece3a6cedfe1_224.png)

![](img/417bd6c721484af32634ece3a6cedfe1_226.png)

![](img/417bd6c721484af32634ece3a6cedfe1_228.png)

> **注意**：IIS 7.0及以上版本已修复此漏洞，或需要特定条件才能触发。

![](img/417bd6c721484af32634ece3a6cedfe1_230.png)

![](img/417bd6c721484af32634ece3a6cedfe1_232.png)

![](img/417bd6c721484af32634ece3a6cedfe1_234.png)

![](img/417bd6c721484af32634ece3a6cedfe1_236.png)

![](img/417bd6c721484af32634ece3a6cedfe1_238.png)

### IIS写权限漏洞（WebDAV配置不当）✏️

![](img/417bd6c721484af32634ece3a6cedfe1_240.png)

在IIS 5.1/6.0中，如果管理员同时开启了WebDAV扩展和目录的“写入”权限，攻击者可能利用HTTP的PUT方法直接上传文件。

![](img/417bd6c721484af32634ece3a6cedfe1_242.png)

![](img/417bd6c721484af32634ece3a6cedfe1_244.png)

![](img/417bd6c721484af32634ece3a6cedfe1_246.png)

![](img/417bd6c721484af32634ece3a6cedfe1_248.png)

![](img/417bd6c721484af32634ece3a6cedfe1_250.png)

![](img/417bd6c721484af32634ece3a6cedfe1_252.png)

![](img/417bd6c721484af32634ece3a6cedfe1_254.png)

**漏洞利用步骤**：
1.  使用工具（如Postman）发送HTTP PUT请求，在目标目录上传一个文本文件。
2.  利用MOVE方法，结合IIS解析漏洞，将文件重命名为可执行脚本（如 `.asp`）。
3.  访问该脚本文件，获取WebShell。

![](img/417bd6c721484af32634ece3a6cedfe1_256.png)

![](img/417bd6c721484af32634ece3a6cedfe1_258.png)

![](img/417bd6c721484af32634ece3a6cedfe1_260.png)

![](img/417bd6c721484af32634ece3a6cedfe1_262.png)

![](img/417bd6c721484af32634ece3a6cedfe1_263.png)

![](img/417bd6c721484af32634ece3a6cedfe1_265.png)

> **提示**：此漏洞在当前互联网环境中已极为罕见，仅作知识了解。

![](img/417bd6c721484af32634ece3a6cedfe1_267.png)

![](img/417bd6c721484af32634ece3a6cedfe1_269.png)

![](img/417bd6c721484af32634ece3a6cedfe1_271.png)

![](img/417bd6c721484af32634ece3a6cedfe1_273.png)

![](img/417bd6c721484af32634ece3a6cedfe1_275.png)

---

![](img/417bd6c721484af32634ece3a6cedfe1_277.png)

![](img/417bd6c721484af32634ece3a6cedfe1_279.png)

## Access数据库注入实战 🎯

![](img/417bd6c721484af32634ece3a6cedfe1_281.png)

前面我们介绍了多种漏洞，本节我们通过一个模拟实战，串联起信息收集、漏洞利用和权限提升的过程。核心是利用Access数据库的SQL注入漏洞。

![](img/417bd6c721484af32634ece3a6cedfe1_283.png)

![](img/417bd6c721484af32634ece3a6cedfe1_285.png)

![](img/417bd6c721484af32634ece3a6cedfe1_287.png)

![](img/417bd6c721484af32634ece3a6cedfe1_289.png)

![](img/417bd6c721484af32634ece3a6cedfe1_291.png)

![](img/417bd6c721484af32634ece3a6cedfe1_293.png)

### SQL注入原理简述

![](img/417bd6c721484af32634ece3a6cedfe1_294.png)

![](img/417bd6c721484af32634ece3a6cedfe1_296.png)

![](img/417bd6c721484af32634ece3a6cedfe1_298.png)

![](img/417bd6c721484af32634ece3a6cedfe1_300.png)

SQL注入的产生是因为应用程序将用户输入的数据，未经充分过滤或转义，直接拼接到了SQL查询语句中。攻击者可以构造特殊的输入，改变原查询的逻辑，从而执行非预期的数据库操作。

![](img/417bd6c721484af32634ece3a6cedfe1_302.png)

![](img/417bd6c721484af32634ece3a6cedfe1_304.png)

![](img/417bd6c721484af32634ece3a6cedfe1_306.png)

![](img/417bd6c721484af32634ece3a6cedfe1_308.png)

**示例**：
原始查询语句：`SELECT * FROM news WHERE id = 用户输入`
用户输入：`1`
正常查询：`SELECT * FROM news WHERE id = 1`
恶意输入：`1 UNION SELECT username, password FROM admin`
最终查询：`SELECT * FROM news WHERE id = 1 UNION SELECT username, password FROM admin`
这样，攻击者就窃取了管理员表中的账号密码。

![](img/417bd6c721484af32634ece3a6cedfe1_310.png)

![](img/417bd6c721484af32634ece3a6cedfe1_312.png)

![](img/417bd6c721484af32634ece3a6cedfe1_314.png)

![](img/417bd6c721484af32634ece3a6cedfe1_316.png)

![](img/417bd6c721484af32634ece3a6cedfe1_318.png)

### 使用Sqlmap进行Access注入

![](img/417bd6c721484af32634ece3a6cedfe1_320.png)

![](img/417bd6c721484af32634ece3a6cedfe1_322.png)

![](img/417bd6c721484af32634ece3a6cedfe1_324.png)

![](img/417bd6c721484af32634ece3a6cedfe1_326.png)

对于Access数据库的注入，我们通常使用自动化工具 `Sqlmap`，因为它依赖于“猜解”表名和列名，手工注入效率较低。

![](img/417bd6c721484af32634ece3a6cedfe1_328.png)

![](img/417bd6c721484af32634ece3a6cedfe1_330.png)

![](img/417bd6c721484af32634ece3a6cedfe1_332.png)

![](img/417bd6c721484af32634ece3a6cedfe1_334.png)

![](img/417bd6c721484af32634ece3a6cedfe1_336.png)

**实战步骤**：

![](img/417bd6c721484af32634ece3a6cedfe1_338.png)

![](img/417bd6c721484af32634ece3a6cedfe1_340.png)

1.  **寻找注入点**：找到类似 `http://target.com/show.asp?id=123` 这样带有参数的URL。
2.  **使用Sqlmap检测**：
    ```bash
    python sqlmap.py -u "http://target.com/show.asp?id=123"
    ```
3.  **猜解表名**：
    ```bash
    python sqlmap.py -u "http://target.com/show.asp?id=123" --tables
    ```
    工具会使用内置字典猜测数据库中有哪些表。重点关注如 `admin`、`user`、`manager` 等可能存放管理员凭据的表名。
4.  **猜解列名**：
    ```bash
    python sqlmap.py -u "http://target.com/show.asp?id=123" -T admin --columns
    ```
    指定上一步猜到的表名（如 `admin`），猜测该表中有哪些列，通常寻找 `username`、`password` 等列。
5.  **提取数据**：
    ```bash
    python sqlmap.py -u "http://target.com/show.asp?id=123" -T admin -C "username,password" --dump
    ```
    指定表和列，将数据内容导出。密码可能是明文，也可能是MD5等哈希值，需要进一步破解。

![](img/417bd6c721484af32634ece3a6cedfe1_342.png)

![](img/417bd6c721484af32634ece3a6cedfe1_344.png)

![](img/417bd6c721484af32634ece3a6cedfe1_346.png)

![](img/417bd6c721484af32634ece3a6cedfe1_348.png)

![](img/417bd6c721484af32634ece3a6cedfe1_350.png)

### 获取后台权限与WebShell

![](img/417bd6c721484af32634ece3a6cedfe1_352.png)

![](img/417bd6c721484af32634ece3a6cedfe1_354.png)

![](img/417bd6c721484af32634ece3a6cedfe1_356.png)

![](img/417bd6c721484af32634ece3a6cedfe1_358.png)

![](img/417bd6c721484af32634ece3a6cedfe1_360.png)

在通过注入获取管理员账号密码后，下一步是找到后台登录地址并登录。

![](img/417bd6c721484af32634ece3a6cedfe1_362.png)

![](img/417bd6c721484af32634ece3a6cedfe1_364.png)

![](img/417bd6c721484af32634ece3a6cedfe1_366.png)

![](img/417bd6c721484af32634ece3a6cedfe1_368.png)

**寻找后台路径的几种方法**：
*   **目录扫描**：使用字典对网站目录进行暴力猜解（如 `DirBuster`, `御剑` 等工具），寻找 `/admin`, `/manage`, `/login` 等路径。
*   **短文件名探测**：利用前面提到的IIS短文件名漏洞，探测可能存在后台目录。
*   **网页爬虫**：使用爬虫工具分析网站所有链接，从页面源码或链接中可能发现后台地址。

![](img/417bd6c721484af32634ece3a6cedfe1_370.png)

![](img/417bd6c721484af32634ece3a6cedfe1_372.png)

![](img/417bd6c721484af32634ece3a6cedfe1_374.png)

![](img/417bd6c721484af32634ece3a6cedfe1_376.png)

![](img/417bd6c721484af32634ece3a6cedfe1_378.png)

![](img/417bd6c721484af32634ece3a6cedfe1_380.png)

![](img/417bd6c721484af32634ece3a6cedfe1_382.png)

![](img/417bd6c721484af32634ece3a6cedfe1_383.png)

登录后台后，可以寻找文件上传功能。如果存在上传且过滤不严，可以尝试直接上传ASP木马（WebShell）。如果前端对文件后缀做了限制，可以结合 **IIS解析漏洞**（如将文件命名为 `shell.asp;.jpg`）进行绕过。

![](img/417bd6c721484af32634ece3a6cedfe1_385.png)

![](img/417bd6c721484af32634ece3a6cedfe1_387.png)

![](img/417bd6c721484af32634ece3a6cedfe1_389.png)

![](img/417bd6c721484af32634ece3a6cedfe1_391.png)

![](img/417bd6c721484af32634ece3a6cedfe1_393.png)

![](img/417bd6c721484af32634ece3a6cedfe1_395.png)

![](img/417bd6c721484af32634ece3a6cedfe1_397.png)

上传成功后，访问WebShell的URL，即可获得一个与服务器交互的命令执行环境，从而控制网站服务器。

![](img/417bd6c721484af32634ece3a6cedfe1_399.png)

![](img/417bd6c721484af32634ece3a6cedfe1_401.png)

![](img/417bd6c721484af32634ece3a6cedfe1_403.png)

![](img/417bd6c721484af32634ece3a6cedfe1_405.png)

![](img/417bd6c721484af32634ece3a6cedfe1_407.png)

![](img/417bd6c721484af32634ece3a6cedfe1_409.png)

---

![](img/417bd6c721484af32634ece3a6cedfe1_411.png)

![](img/417bd6c721484af32634ece3a6cedfe1_413.png)

![](img/417bd6c721484af32634ece3a6cedfe1_415.png)

![](img/417bd6c721484af32634ece3a6cedfe1_417.png)

![](img/417bd6c721484af32634ece3a6cedfe1_419.png)

![](img/417bd6c721484af32634ece3a6cedfe1_421.png)

![](img/417bd6c721484af32634ece3a6cedfe1_423.png)

![](img/417bd6c721484af32634ece3a6cedfe1_425.png)

## 总结 📝

![](img/417bd6c721484af32634ece3a6cedfe1_427.png)

![](img/417bd6c721484af32634ece3a6cedfe1_429.png)

![](img/417bd6c721484af32634ece3a6cedfe1_430.png)

![](img/417bd6c721484af32634ece3a6cedfe1_432.png)

![](img/417bd6c721484af32634ece3a6cedfe1_434.png)

![](img/417bd6c721484af32634ece3a6cedfe1_436.png)

本节课中我们一起学习了ASP应用安全攻防的基础知识。

![](img/417bd6c721484af32634ece3a6cedfe1_438.png)

![](img/417bd6c721484af32634ece3a6cedfe1_439.png)

我们首先分析了**ASP+Access**这种经典组合因默认配置导致的**数据库泄露风险**。然后，探讨了IIS中间件的几个历史漏洞：可导致服务拒绝的**HTTP.SYS蓝屏漏洞**、用于信息收集的**短文件名泄露漏洞**、可绕过上传限制的**文件解析漏洞**以及配置不当引发的**写权限漏洞**。

![](img/417bd6c721484af32634ece3a6cedfe1_441.png)

![](img/417bd6c721484af32634ece3a6cedfe1_442.png)

![](img/417bd6c721484af32634ece3a6cedfe1_444.png)

![](img/417bd6c721484af32634ece3a6cedfe1_446.png)

![](img/417bd6c721484af32634ece3a6cedfe1_448.png)

![](img/417bd6c721484af32634ece3a6cedfe1_450.png)

![](img/417bd6c721484af32634ece3a6cedfe1_452.png)

![](img/417bd6c721484af32634ece3a6cedfe1_454.png)

最后，我们通过一个实战流程，演示了如何利用**Access数据库注入漏洞**获取后台凭证，并结合**目录扫描**寻找后台，以及利用**文件上传与解析漏洞**上传WebShell，最终获取服务器控制权。

![](img/417bd6c721484af32634ece3a6cedfe1_456.png)

![](img/417bd6c721484af32634ece3a6cedfe1_458.png)

![](img/417bd6c721484af32634ece3a6cedfe1_460.png)

![](img/417bd6c721484af32634ece3a6cedfe1_462.png)

![](img/417bd6c721484af32634ece3a6cedfe1_464.png)

![](img/417bd6c721484af32634ece3a6cedfe1_466.png)

![](img/417bd6c721484af32634ece3a6cedfe1_468.png)

虽然ASP技术已逐渐退出主流，但其中蕴含的安全思想（如默认配置风险、注入原理、解析逻辑等）对于理解整个Web安全体系仍然具有重要价值。在后续课程中，我们将把重点转向PHP、Java等现代主流技术栈的安全问题。