![](img/b844d515ba4483a8527c7532ee465a29_1.png)

![](img/b844d515ba4483a8527c7532ee465a29_3.png)

![](img/b844d515ba4483a8527c7532ee465a29_5.png)

# 课程 P1：绕过软件WAF的Fuzz技术 🛡️➡️💻

![](img/b844d515ba4483a8527c7532ee465a29_7.png)

![](img/b844d515ba4483a8527c7532ee465a29_9.png)

在本节课中，我们将学习如何通过编写Fuzz脚本来绕过软件Web应用防火墙。课程内容涵盖基础概念、脚本编写思路、具体实现方法以及两个实用的自动化工具彩蛋。我们将从简单的Python基础开始，逐步深入到复杂的WAF绕过技术。

![](img/b844d515ba4483a8527c7532ee465a29_11.png)

![](img/b844d515ba4483a8527c7532ee465a29_13.png)

## 概述

Web应用防火墙通过正则表达式匹配来拦截恶意请求。本节课的核心思路是利用“模糊测试”来生成大量变异的Payload，以绕过这些正则匹配规则。我们将重点学习如何编写Python脚本实现Fuzz，并介绍如何将此技术集成到SQLMap等自动化工具中。

![](img/b844d515ba4483a8527c7532ee465a29_15.png)

## 1. 基础知识准备 🧱

![](img/b844d515ba4483a8527c7532ee465a29_17.png)

在深入Fuzz脚本之前，我们需要了解一些基础概念和准备好测试环境。

![](img/b844d515ba4483a8527c7532ee465a29_19.png)

### 1.1 Python基础入门

![](img/b844d515ba4483a8527c7532ee465a29_21.png)

![](img/b844d515ba4483a8527c7532ee465a29_23.png)

对于不熟悉Python的同学，这里快速介绍几个核心概念。Python是一种动态脚本语言，语法简洁。

*   **变量与输出**：Python中声明变量无需指定类型。
    ```python
    a = 10          # 整数
    b = 3.14        # 浮点数
    c = "Hello"     # 字符串
    print(a)        # 打印变量
    print("World")  # 打印字符串
    ```
*   **条件判断**：使用 `if` 语句，代码块通过缩进来定义。
    ```python
    if a > 5:
        print("a大于5")
    ```
*   **列表与循环**：列表是Python中常用的数据结构，使用 `for` 循环可以遍历列表。
    ```python
    my_list = [‘a‘, ‘b‘, ‘c‘]  # 声明一个列表
    for item in my_list:        # 遍历列表
        print(item)
    ```

### 1.2 所需环境与工具

![](img/b844d515ba4483a8527c7532ee465a29_25.png)

![](img/b844d515ba4483a8527c7532ee465a29_27.png)

![](img/b844d515ba4483a8527c7532ee465a29_29.png)

为了进行实践，需要准备以下环境：
1.  **测试环境**：PHP + MySQL + Apache。推荐使用PHPStudy一键搭建。
2.  **编程语言**：Python 2 或 3，并安装 `requests` 库用于发送HTTP请求。
    ```bash
    pip install requests
    ```
3.  **测试靶场**：一个存在SQL注入漏洞的PHP页面，用于验证绕过效果。
4.  **防护软件**：本次以“安全狗”软件WAF为例进行测试。

## 2. Fuzz脚本的核心思路 💡

![](img/b844d515ba4483a8527c7532ee465a29_31.png)

上一节我们准备好了基础环境，本节中我们来看看Fuzz脚本是如何构思的。

![](img/b844d515ba4483a8527c7532ee465a29_33.png)

![](img/b844d515ba4483a8527c7532ee465a29_35.png)

WAF主要通过匹配请求中的危险关键字（如 `union select`）来进行拦截。Fuzz的核心思想就是**通过大量变异，使Payload的结构变得复杂，从而绕过正则表达式的匹配**。

![](img/b844d515ba4483a8527c7532ee465a29_37.png)

![](img/b844d515ba4483a8527c7532ee465a29_39.png)

一个关键的绕过技巧是利用MySQL的**内联注释**特性。例如，`/*!50000select*/` 在MySQL中会被执行，但可能被WAF规则忽略。我们的脚本将围绕生成这类变异Payload来构建。

![](img/b844d515ba4483a8527c7532ee465a29_41.png)

![](img/b844d515ba4483a8527c7532ee465a29_42.png)

判断绕过是否成功的依据是HTTP响应内容。例如，正常查询的页面包含关键词 `title1`，而被拦截或出错的页面则不包含。脚本通过检查响应中是否存在 `title1` 来判断Payload是否生效。

![](img/b844d515ba4483a8527c7532ee465a29_44.png)

## 3. Fuzz脚本的实现 🛠️

![](img/b844d515ba4483a8527c7532ee465a29_46.png)

理解了核心思路后，现在我们来动手实现Fuzz脚本。脚本的主要任务是生成数百万种Payload组合，并测试它们是否能绕过WAF。

![](img/b844d515ba4483a8527c7532ee465a29_48.png)

![](img/b844d515ba4483a8527c7532ee465a29_49.png)

![](img/b844d515ba4483a8527c7532ee465a29_51.png)

![](img/b844d515ba4483a8527c7532ee465a29_53.png)

以下是脚本的关键步骤：

![](img/b844d515ba4483a8527c7532ee465a29_55.png)

![](img/b844d515ba4483a8527c7532ee465a29_57.png)

![](img/b844d515ba4483a8527c7532ee465a29_59.png)

![](img/b844d515ba4483a8527c7532ee465a29_61.png)

1.  **构建字符库**：定义一个列表，包含各种可用于变异的特殊字符、符号及其URL编码形式。例如：`[‘/*!‘, ‘*/‘, ‘‘, ‘%0a‘, ‘%0b‘]`。
2.  **生成Payload**：通过多层嵌套循环，将字符库中的元素进行排列组合，拼接在SQL关键字（如 `select`）的周围。
    ```python
    # 示例：五层嵌套循环生成组合
    for a in chars:
        for b in chars:
            for c in chars:
                for d in chars:
                    for e in chars:
                        payload = prefix + a + b + c + d + e + “select” + suffix
    ```
3.  **发送请求与判断**：使用 `requests.get()` 方法将拼接好的Payload发送到目标URL。检查返回的网页内容中是否包含成功标志（如 `title1`）。
4.  **保存结果**：将成功绕过的Payload保存到文件中，以备后续分析和使用。

![](img/b844d515ba4483a8527c7532ee465a29_63.png)

![](img/b844d515ba4483a8527c7532ee465a29_65.png)

![](img/b844d515ba4483a8527c7532ee465a29_67.png)

运行脚本后，可能会在短时间内得到成百上千个有效的绕过Payload，这证明了“1000种姿势”并非虚言。

![](img/b844d515ba4483a8527c7532ee465a29_69.png)

![](img/b844d515ba4483a8527c7532ee465a29_71.png)

## 4. 彩蛋一：编写SQLMap的Tamper脚本 🥚

![](img/b844d515ba4483a8527c7532ee465a29_73.png)

![](img/b844d515ba4483a8527c7532ee465a29_75.png)

手工Fuzz得到Payload后，我们希望将其应用到自动化工具中。本节将介绍如何为SQLMap编写一个Tamper脚本，实现自动绕过。

![](img/b844d515ba4483a8527c7532ee465a29_77.png)

![](img/b844d515ba4483a8527c7532ee465a29_79.png)

![](img/b844d515ba4483a8527c7532ee465a29_81.png)

![](img/b844d515ba4483a8527c7532ee465a29_82.png)

![](img/b844d515ba4483a8527c7532ee465a29_84.png)

Tamper是SQLMap的插件，用于在注入前修改Payload。我们可以模仿现有的Tamper脚本（如 `versionedkeywords.py`）进行修改。

![](img/b844d515ba4483a8527c7532ee465a29_86.png)

![](img/b844d515ba4483a8527c7532ee465a29_88.png)

**核心修改点**：将原脚本中用于包裹关键字的内联注释从一层改为两层，以匹配我们Fuzz发现的有效结构。
```python
# 修改前
payload = payload.replace(‘UNION‘, ‘/*!UNION*/‘)
# 修改后
payload = payload.replace(‘UNION‘, ‘/*!/*!UNION*/*/‘)
```

![](img/b844d515ba4483a8527c7532ee465a29_90.png)

![](img/b844d515ba4483a8527c7532ee465a29_92.png)

![](img/b844d515ba4483a8527c7532ee465a29_94.png)

![](img/b844d515ba4483a8527c7532ee465a29_96.png)

使用修改后的Tamper脚本配合SQLMap进行测试：
```bash
sqlmap -u “目标URL“ --tamper=你的脚本名
```
可以发现，SQLMap能够成功检测到注入点并完成数据提取，而不再被WAF拦截。

![](img/b844d515ba4483a8527c7532ee465a29_98.png)

![](img/b844d515ba4483a8527c7532ee465a29_100.png)

![](img/b844d515ba4483a8527c7532ee465a29_102.png)

## 5. 彩蛋二：应对IP频率限制的代理池工具 🥚

![](img/b844d515ba4483a8527c7532ee465a29_104.png)

![](img/b844d515ba4483a8527c7532ee465a29_106.png)

![](img/b844d515ba4483a8527c7532ee465a29_108.png)

许多WAF带有CC攻击防护功能，会限制单个IP的请求频率。即使有有效的Payload，过快请求也会导致IP被封锁。本节介绍一个小工具来解决这个问题。

该工具是一个用Python编写的本地代理服务器，作为SQLMap和目标网站之间的中转。

![](img/b844d515ba4483a8527c7532ee465a29_110.png)

![](img/b844d515ba4483a8527c7532ee465a29_112.png)

**工作原理**：
1.  SQLMap将流量代理到本工具的端口（如9999）。
2.  工具接收到SQLMap的请求后，不是直接发给目标，而是从一个**IP代理池**中随机选取一个代理IP。
3.  通过这个代理IP将请求转发给目标网站，并将响应返回给SQLMap。
4.  每个请求或每几个请求就更换一次代理IP，从而避免因频率过高被封锁。

![](img/b844d515ba4483a8527c7532ee465a29_114.png)

**使用方式**：
1.  运行代理池工具。
2.  在SQLMap命令中设置代理参数指向该工具。
    ```bash
    sqlmap -u “目标URL“ --proxy=“http://127.0.0.1:9999“
    ```
这样，SQLMap的扫描流量就会通过不断变化的代理IP发出，有效绕过WAF的IP频率限制。

![](img/b844d515ba4483a8527c7532ee465a29_116.png)

![](img/b844d515ba4483a8527c7532ee465a29_118.png)

![](img/b844d515ba4483a8527c7532ee465a29_120.png)

## 总结

![](img/b844d515ba4483a8527c7532ee465a29_122.png)

本节课我们一起学习了绕过软件WAF的Fuzz技术。我们从Python基础和环境搭建开始，理解了WAF的拦截原理和Fuzz绕过的核心思路。随后，我们动手编写了一个Fuzz脚本，用于生成海量变异Payload来探测WAF的规则盲点。最后，我们分享了两个提升效率的彩蛋：一是将成果固化为SQLMap的Tamper脚本，实现自动化注入；二是通过代理池工具解决IP请求频率限制的问题。

![](img/b844d515ba4483a8527c7532ee465a29_124.png)

![](img/b844d515ba4483a8527c7532ee465a29_126.png)

![](img/b844d515ba4483a8527c7532ee465a29_128.png)

这套组合拳（Fuzz探测 + Tamper脚本 + 代理池）能极大地提高在存在软件WAF环境下的SQL注入测试成功率。记住，核心思想是**利用自动化生成复杂结构，扰乱基于正则匹配的防御规则**。