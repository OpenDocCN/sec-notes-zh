![](img/32df6c3a121be42ac8057efe47760a25_1.png)

![](img/32df6c3a121be42ac8057efe47760a25_3.png)

# 网络安全实战教程：P45 - 第7天：Weblogic、ThinkPHP、JBoss、Struts2历史漏洞讲解 🔍

![](img/32df6c3a121be42ac8057efe47760a25_5.png)

![](img/32df6c3a121be42ac8057efe47760a25_7.png)

![](img/32df6c3a121be42ac8057efe47760a25_9.png)

在本节课中，我们将学习如何利用工具进行SQL注入的深入利用，并重点讲解Weblogic和ThinkPHP框架的历史漏洞原理、识别方法及利用过程。课程内容注重实战，旨在帮助初学者理解常见漏洞的挖掘思路。

![](img/32df6c3a121be42ac8057efe47760a25_11.png)

![](img/32df6c3a121be42ac8057efe47760a25_13.png)

![](img/32df6c3a121be42ac8057efe47760a25_15.png)

![](img/32df6c3a121be42ac8057efe47760a25_17.png)

## SQL注入的深入利用：枚举与拖库

![](img/32df6c3a121be42ac8057efe47760a25_19.png)

上一节我们介绍了使用SQLMap进行基本的SQL注入检测。本节中我们来看看如何利用已发现的注入点，进一步枚举数据库结构并获取数据。

![](img/32df6c3a121be42ac8057efe47760a25_21.png)

![](img/32df6c3a121be42ac8057efe47760a25_23.png)

![](img/32df6c3a121be42ac8057efe47760a25_24.png)

![](img/32df6c3a121be42ac8057efe47760a25_26.png)

![](img/32df6c3a121be42ac8057efe47760a25_28.png)

当我们成功枚举出数据库中的表名（例如 `user`）后，下一步是枚举该表的列名。

![](img/32df6c3a121be42ac8057efe47760a25_30.png)

![](img/32df6c3a121be42ac8057efe47760a25_32.png)

![](img/32df6c3a121be42ac8057efe47760a25_34.png)

以下是枚举指定表（例如 `user`）列名的命令：
```bash
sqlmap.py -u "目标URL" --columns -T user
```
执行该命令后，可能会枚举出 `id`、`username`、`password` 等列。

![](img/32df6c3a121be42ac8057efe47760a25_36.png)

![](img/32df6c3a121be42ac8057efe47760a25_38.png)

![](img/32df6c3a121be42ac8057efe47760a25_40.png)

![](img/32df6c3a121be42ac8057efe47760a25_41.png)

![](img/32df6c3a121be42ac8057efe47760a25_43.png)

获取到列名之后，可以进行拖库操作，即导出表中的具体数据。

以下是拖取指定表（例如 `user`）中数据的命令：
```bash
sqlmap.py -u "目标URL" --dump -T user -C id,username,password
```
此命令会将 `user` 表中 `id`、`username`、`password` 列的数据导出并保存到本地CSV文件中。

**重要提示**：拖库操作（`--dump`）会直接获取数据库中的敏感信息。在漏洞挖掘（如SRC众测）时，仅用于证明漏洞存在即可，切勿实际拖取数据，以免触犯法律。仅在获得明确授权的渗透测试项目中，方可谨慎使用此功能。

![](img/32df6c3a121be42ac8057efe47760a25_45.png)

![](img/32df6c3a121be42ac8057efe47760a25_47.png)

## 绕过WAF：使用Tamper脚本

![](img/32df6c3a121be42ac8057efe47760a25_49.png)

![](img/32df6c3a121be42ac8057efe47760a25_51.png)

在实际测试中，网站可能部署了WAF（Web应用防火墙）来过滤恶意SQL语句。SQLMap的 `--tamper` 参数可以帮助我们绕过这些过滤。

![](img/32df6c3a121be42ac8057efe47760a25_53.png)

Tamper脚本的原理是对注入载荷进行变形，例如：
*   在关键词中插入注释符：`SELECT` -> `SEL/**/ECT`
*   在关键词中添加百分号：`SELECT` -> `S%E%L%E%C%T`
*   对关键词进行随机大小写转换：`SELECT` -> `SeLeCt`

![](img/32df6c3a121be42ac8057efe47760a25_55.png)

![](img/32df6c3a121be42ac8057efe47760a25_57.png)

![](img/32df6c3a121be42ac8057efe47760a25_59.png)

![](img/32df6c3a121be42ac8057efe47760a25_61.png)

以下是使用Tamper脚本的命令示例：
```bash
sqlmap.py -u "目标URL" --tamper=脚本名称.py
```
例如，使用 `randomcase.py` 脚本可以随机转换载荷的大小写，以绕过一些简单的关键词过滤。

![](img/32df6c3a121be42ac8057efe47760a25_63.png)

![](img/32df6c3a121be42ac8057efe47760a25_65.png)

![](img/32df6c3a121be42ac8057efe47760a25_67.png)

![](img/32df6c3a121be42ac8057efe47760a25_69.png)

## Weblogic漏洞讲解

![](img/32df6c3a121be42ac8057efe47760a25_71.png)

![](img/32df6c3a121be42ac8057efe47760a25_73.png)

![](img/32df6c3a121be42ac8057efe47760a25_75.png)

![](img/32df6c3a121be42ac8057efe47760a25_77.png)

上一部分我们结束了SQL注入工具的学习。本节中我们来看看一个常见的中间件漏洞——Weblogic。

### Weblogic简介与特征

Weblogic是美国Oracle公司出品的一款基于JavaEE的应用服务器（中间件）。它广泛用于运行Java Web应用程序。

![](img/32df6c3a121be42ac8057efe47760a25_79.png)

![](img/32df6c3a121be42ac8057efe47760a25_81.png)

识别Weblogic通常有两个特征：
1.  **默认端口**：`7001`
2.  **Web界面特征**：访问其Web根目录通常会返回一个特定的404错误页面。

### Weblogic漏洞利用

![](img/32df6c3a121be42ac8057efe47760a25_83.png)

![](img/32df6c3a121be42ac8057efe47760a25_85.png)

Weblogic历史上存在大量高危漏洞，例如 `CVE-2019-2729`、`CVE-2020-2551` 等。

以下是利用网络空间搜索引擎（如FOFA）寻找Weblogic资产的方法：
搜索语法：`app="Weblogic"` 或 `port="7001"`

发现资产后，可以使用自动化脚本批量检测已知漏洞。例如，使用集成多个PoC的扫描脚本：
```bash
python3 weblogic_scanner.py -f target_urls.txt
```
脚本会批量测试目标是否存在已知的Weblogic漏洞，并输出结果。

![](img/32df6c3a121be42ac8057efe47760a25_87.png)

![](img/32df6c3a121be42ac8057efe47760a25_89.png)

![](img/32df6c3a121be42ac8057efe47760a25_91.png)

![](img/32df6c3a121be42ac8057efe47760a25_93.png)

![](img/32df6c3a121be42ac8057efe47760a25_94.png)

![](img/32df6c3a121be42ac8057efe47760a25_96.png)

对于发现的漏洞，需要进一步利用。以 `CVE-2019-2725` 为例，可以通过搜索引擎找到公开的漏洞利用脚本（Exp）。利用过程通常涉及向特定漏洞路径发送构造好的HTTP请求，从而执行系统命令。
例如，通过Exp执行 `id` 命令可以验证漏洞利用是否成功。

![](img/32df6c3a121be42ac8057efe47760a25_98.png)

![](img/32df6c3a121be42ac8057efe47760a25_100.png)

![](img/32df6c3a121be42ac8057efe47760a25_102.png)

![](img/32df6c3a121be42ac8057efe47760a25_103.png)

![](img/32df6c3a121be42ac8057efe47760a25_105.png)

![](img/32df6c3a121be42ac8057efe47760a25_106.png)

## ThinkPHP漏洞讲解

接下来，我们学习另一个常见的国产开发框架——ThinkPHP的漏洞。

### ThinkPHP简介与特征

ThinkPHP是一个快速、兼容而且简单的轻量级国产PHP开发框架。许多内容管理系统（CMS）如 `ThinkCMF`、`5Ucms` 都是基于其二次开发。

识别ThinkPHP的特征包括：
*   其默认的报错页面会明确显示“ThinkPHP”框架信息。
*   某些版本在开启调试模式时，访问特定路径会暴露框架信息。

![](img/32df6c3a121be42ac8057efe47760a25_108.png)

![](img/32df6c3a121be42ac8057efe47760a25_110.png)

### ThinkPHP漏洞利用

![](img/32df6c3a121be42ac8057efe47760a25_112.png)

![](img/32df6c3a121be42ac8057efe47760a25_113.png)

![](img/32df6c3a121be42ac8057efe47760a25_115.png)

ThinkPHP 5.x 版本曾曝出多个远程代码执行漏洞，例如 `5.0.23` 和 `5.1.x` 版本的相关漏洞。

![](img/32df6c3a121be42ac8057efe47760a25_117.png)

**ThinkPHP 5.0.23 远程代码执行漏洞**
漏洞利用步骤：
1.  捕获访问网站首页的GET请求包。
2.  将请求方法改为 `POST`。
3.  在POST参数中插入Payload，例如执行 `id` 命令的代码。
4.  发送请求，如果响应中返回了命令执行结果（如用户`id`信息），则漏洞利用成功。

**批量检测与利用**
对于未知版本的ThinkPHP站点，可以使用专门的扫描脚本进行批量漏洞检测：
```bash
python3 thinkphp_scan.py -u http://target.com
```
脚本会自动测试多个已知的ThinkPHP漏洞PoC，并返回可用的利用方式。

## 课程总结

本节课中我们一起学习了以下内容：
1.  **SQLMap高级利用**：学习了如何枚举表结构、进行拖库操作，以及使用Tamper脚本绕过WAF过滤。
2.  **Weblogic漏洞**：了解了Weblogic中间件的识别特征，学习了如何寻找资产、使用工具批量扫描漏洞，并对具体漏洞（如CVE-2019-2725）进行了利用演示。
3.  **ThinkPHP漏洞**：掌握了ThinkPHP框架的识别方法，并动手实践了其历史远程代码执行漏洞的利用过程。

![](img/32df6c3a121be42ac8057efe47760a25_119.png)

![](img/32df6c3a121be42ac8057efe47760a25_121.png)

![](img/32df6c3a121be42ac8057efe47760a25_123.png)

通过本课的学习，你应该对这两类常见组件的漏洞挖掘流程有了基本的认识。在实际安全测试中，需要结合信息收集、工具扫描和手动验证，才能有效地发现和利用安全漏洞。请务必在合法授权的范围内进行所有测试。