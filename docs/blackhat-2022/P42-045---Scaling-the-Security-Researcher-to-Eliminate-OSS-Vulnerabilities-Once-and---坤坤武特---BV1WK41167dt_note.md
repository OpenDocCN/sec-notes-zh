![](img/63657aa9d34513faa3f9d11e07419eb0_0.png)

# 课程 P42：045 - 扩展安全研究员以消除开源漏洞 🛡️

在本节课中，我们将学习如何利用自动化工具和技术，大规模地识别并修复开源软件中的安全漏洞。我们将探讨从发现漏洞到生成修复补丁的完整流程，并了解如何通过协作和先进工具来扩展安全研究工作的影响力。

---

![](img/63657aa9d34513faa3f9d11e07419eb0_2.png)

## 概述与背景

![](img/63657aa9d34513faa3f9d11e07419eb0_4.png)

本次演讲旨在分享如何扩展安全研究员的能力，以消除开源软件中的安全漏洞。演讲者Jonathan Lu是一名软件安全工程师和研究员，也是Human Security的首位Dan Kaminsky研究员。他的工作得到了GitHub和Dan Kaminsky奖学金的支持。

![](img/63657aa9d34513faa3f9d11e07419eb0_6.png)

Dan Kaminsky因在2008年发现DNS关键漏洞并推动全行业修复而闻名。Dan Kaminsky奖学金旨在纪念他的遗产，通过资助改善世界安全性的开源工作。

![](img/63657aa9d34513faa3f9d11e07419eb0_8.png)

---

![](img/63657aa9d34513faa3f9d11e07419eb0_10.png)

## 从一个简单的漏洞开始

![](img/63657aa9d34513faa3f9d11e07419eb0_12.png)

我们的故事从一个简单的漏洞开始：在Gradle或Maven构建文件中使用HTTP协议下载依赖项。

**漏洞核心**：使用HTTP协议解析依赖关系，会使构建过程面临中间人攻击的风险，攻击者可能篡改下载的构件（artifact）。在Java生态系统中，下载的构件通常没有额外的身份验证或验证机制。

这个漏洞不仅存在于Gradle，也存在于Maven构建文件中。这意味着恶意代码可能在CI/CD管道中被执行，甚至被用来上传构件，通常会导致网络凭证暴露。

当开始审查开源项目时，发现这个漏洞无处不在，存在于Spring、Apache、Red Hat、Kotlin等众多知名组织的项目中。Maven Central（Java生态系统的核心构件仓库）报告称，即使在多次沟通后，仍有大量流量使用HTTP。

---

![](img/63657aa9d34513faa3f9d11e07419eb0_14.png)

## 如何大规模解决问题

![](img/63657aa9d34513faa3f9d11e07419eb0_16.png)

面对如此广泛存在的漏洞，我们需要一种系统性的方法来解决。以下是解决问题的步骤。

![](img/63657aa9d34513faa3f9d11e07419eb0_18.png)

### 识别易受攻击的项目

![](img/63657aa9d34513faa3f9d11e07419eb0_20.png)

首先，需要识别哪些项目存在此漏洞。我们使用GitHub的CodeQL代码查询语言来达成此目的。CodeQL会在每次提交时尝试构建开源项目并创建数据库，安全研究员可以大规模地对这些数据库运行查询。

![](img/63657aa9d34513faa3f9d11e07419eb0_22.png)

![](img/63657aa9d34513faa3f9d11e07419eb0_24.png)

以下是用于查找易受攻击项目的CodeQL查询示例（概念性表示）：
```ql
// 示例：查找构建文件中使用HTTP的语句
from GradleBuildFile gbf
where gbf.usesHttpProtocol()
select gbf
```

通过此方法，GitHub安全实验室提供了包含易受攻击项目的列表。

### 自动生成修复补丁

![](img/63657aa9d34513faa3f9d11e07419eb0_26.png)

获得项目列表后，下一步是自动生成修复的拉取请求（Pull Request）。我们编写了一个Python机器人，它封装了GitHub CLI，用于自动创建PR。

![](img/63657aa9d34513faa3f9d11e07419eb0_28.png)

![](img/63657aa9d34513faa3f9d11e07419eb0_30.png)

最初尝试使用XML解析器直接修改构建文件，但会导致代码格式发生巨大变化，维护者通常不愿接受这种“破坏性”的修改。因此，我们转而使用正则表达式进行精准替换。

**核心修复逻辑**（概念性代码）：
```python
# 使用正则表达式将 http:// 替换为 https://
import re
vulnerable_code = re.sub(r'http://', 'https://', original_build_file_content)
```

虽然“使用正则表达式解决问题会带来两个问题”是一句业内玩笑，但这种方法在此场景下是有效的。

![](img/63657aa9d34513faa3f9d11e07419eb0_32.png)

通过这项技术，我们为大量项目生成了修复补丁。例如，针对此HTTP漏洞，生成了1596个拉取请求，合并率约为40%。这项工作也获得了GitHub安全实验室的奖金。

---

![](img/63657aa9d34513faa3f9d11e07419eb0_34.png)

## 面临的挑战与自动化需求

![](img/63657aa9d34513faa3f9d11e07419eb0_36.png)

成功修复一个漏洞后，我们意识到开源世界中存在太多类似的安全漏洞。例如，ZIP Slip（路径遍历）漏洞在CodeQL查询结果中就有海量的存在实例。

手动报告和修复每一个漏洞是不现实的。我们需要更强大的自动化方案，不仅能发现漏洞，还能生成格式正确、易于被接受的修复代码。

---

![](img/63657aa9d34513faa3f9d11e07419eb0_38.png)

## 引入OpenRewrite进行精确代码转换

![](img/63657aa9d34513faa3f9d11e07419eb0_40.png)

为了应对自动化修复的挑战，我们引入了OpenRewrite框架。OpenRewrite是一个用于查询和转换源代码的框架和API，它基于编译器的抽象语法树（AST）。

![](img/63657aa9d34513faa3f9d11e07419eb0_42.png)

### 什么是抽象语法树（AST）？

![](img/63657aa9d34513faa3f9d11e07419eb0_44.png)

AST是源代码的树状结构表示，反映了代码的语法结构。然而，编译器生成的原始AST是精简的，它不包含格式（如缩进、空格）和注释信息，类型信息也有限。

![](img/63657aa9d34513faa3f9d11e07419eb0_46.png)

### OpenRewrite的解决方案

OpenRewrite通过以下方式解决了这些限制：

1.  **保留格式的AST**：OpenRewrite在解析时，会将AST与源文件同步，为每个树节点保留其前缀（空格）、后缀和注释。这使得转换后的代码能完美匹配项目的代码风格。
2.  **语义感知**：OpenRewrite的AST不仅是语法上的，也是语义感知的，它提供了完整的类型信息，这对于进行安全、精确的转换至关重要。
3.  **模板引擎**：OpenRewrite提供了模板引擎，允许开发者用熟悉的编程语言（如Java）来构建AST片段，并指定参数替换。框架会自动处理格式化，确保生成的代码无缝融入现有代码库。

![](img/63657aa9d34513faa3f9d11e07419eb0_48.png)

**示例模板**（概念性表示）：
```java
// 定义一个在指定语句后插入安全防护代码的模板
template InsertSecurityCheck {
    afterStatement(@AfterStatement Statement stmt) {
        // 要插入的防护代码逻辑
        if (!isSafe(path)) {
            throw new SecurityException();
        }
    }
}
```

![](img/63657aa9d34513faa3f9d11e07419eb0_50.png)

有了OpenRewrite，我们可以定义复杂的修复逻辑（“食谱”-Recipe），并大规模、高质量地应用到代码库中。

![](img/63657aa9d34513faa3f9d11e07419eb0_52.png)

![](img/63657aa9d34513faa3f9d11e07419eb0_54.png)

---

## 实战：修复三类安全漏洞

![](img/63657aa9d34513faa3f9d11e07419eb0_56.png)

![](img/63657aa9d34513faa3f9d11e07419eb0_58.png)

利用OpenRewrite，我们可以自动化修复多种复杂漏洞。下面介绍三个例子。

![](img/63657aa9d34513faa3f9d11e07419eb0_60.png)

### 1. 临时目录劫持

![](img/63657aa9d34513faa3f9d11e07419eb0_62.png)

**漏洞原理**：在Unix-like系统上，`/tmp`目录是所有用户共享的。旧版Java代码中，常用“创建临时文件->删除->创建目录”的模式来创建临时目录，这存在竞争条件，可能导致目录被劫持。

![](img/63657aa9d34513faa3f9d11e07419eb0_64.png)

**易受攻击代码模式**：
```java
File temp = File.createTempFile("prefix", "");
temp.delete();
temp.mkdir(); // 存在竞争条件
```

**安全修复**：使用Java 7引入的`Files.createTempDirectory` API。
```java
Path tempDir = Files.createTempDirectory("prefix");
```

![](img/63657aa9d34513faa3f9d11e07419eb0_66.png)

![](img/63657aa9d34513faa3f9d11e07419eb0_68.png)

使用OpenRewrite，我们能够精准定位这种模式，并将其替换为安全的API调用，同时清理不必要的代码块。我们为此漏洞生成了64个拉取请求。

![](img/63657aa9d34513faa3f9d11e07419eb0_70.png)

### 2. 部分路径遍历

![](img/63657aa9d34513faa3f9d11e07419eb0_72.png)

![](img/63657aa9d34513faa3f9d11e07419eb0_74.png)

**漏洞原理**：当使用字符串比较来检查路径是否在某个安全目录内时，如果攻击者提供的路径与安全路径有共同前缀，可能绕过检查。例如，安全路径是`/home/sam`，攻击者可能传入`/home/samantha/../sam`，规范化后可能绕过检查。

![](img/63657aa9d34513faa3f9d11e07419eb0_76.png)

**易受攻击代码**：
```java
String userPath = new File(input).getCanonicalPath(); // 丢失尾部分隔符
if (userPath.startsWith("/home/sam")) { // 不安全的字符串比较
    // 允许访问
}
```

**安全修复**：使用`java.nio.file.Path`对象进行比较，它更能理解路径结构。
```java
Path safeBase = Paths.get("/home/sam").toRealPath();
Path userPath = Paths.get(input).toRealPath();
if (userPath.startsWith(safeBase)) {
    // 允许访问
}
```

![](img/63657aa9d34513faa3f9d11e07419eb0_78.png)

![](img/63657aa9d34513faa3f9d11e07419eb0_80.png)

**检测挑战**：漏洞代码可能以多种形式出现（如变量提取）。为了准确检测，OpenRewrite需要**数据流分析**能力。

![](img/63657aa9d34513faa3f9d11e07419eb0_82.png)

**数据流分析**：跟踪变量在程序运行时的可能取值和传递路径。这允许我们发现跨越多行代码、经过复杂调用的漏洞模式，并减少误报。

OpenRewrite集成了数据流分析API，其设计类似于CodeQL，便于安全研究员将知识迁移过来。

![](img/63657aa9d34513faa3f9d11e07419eb0_84.png)

![](img/63657aa9d34513faa3f9d11e07419eb0_86.png)

### 3. ZIP Slip漏洞

![](img/63657aa9d34513faa3f9d11e07419eb0_88.png)

**漏洞原理**：解压ZIP文件时，如果未对压缩包内的条目名称进行校验，攻击者可能通过构造包含`../`的路径名，将文件写入预期目录之外的位置，可能导致远程代码执行。

![](img/63657aa9d34513faa3f9d11e07419eb0_90.png)

**易受攻击代码**：
```java
ZipEntry entry = (ZipEntry) entries.nextElement();
File file = new File(destinationDir, entry.getName()); // entry.getName()可能包含“../”
// ... 解压操作
```

![](img/63657aa9d34513faa3f9d11e07419eb0_92.png)

**安全修复**：在写入文件前，检查规范化的目标路径是否仍在安全目录内。
```java
String entryName = entry.getName();
Path targetPath = destinationDir.toPath().resolve(entryName).normalize();
if (!targetPath.startsWith(destinationDir.toPath().toRealPath())) {
    throw new SecurityException("Invalid entry");
}
```

![](img/63657aa9d34513faa3f9d11e07419eb0_94.png)

**修复的复杂性**：修复代码可能以不同形式存在。我们需要判断项目中是否已经存在足够的防护逻辑，避免对已修复的代码进行重复“修复”。

**控制流分析**：为了做出准确判断，OpenRewrite引入了**控制流分析**。它构建程序执行路径的图表，显示基本块（顺序执行的语句序列）和条件跳转。通过遍历控制流图，我们可以分析防护检查（如`if`语句）是否确实保护了易受攻击的代码段。如果防护存在且逻辑正确，则漏洞已修复，无需再次干预。

结合数据流分析和控制流分析，OpenRewrite能够智能、准确地修复像ZIP Slip这样的复杂漏洞。

---

## 大规模生成拉取请求的工程挑战

即使有了完美的修复代码，大规模生成拉取请求也面临工程挑战，主要是GitHub API的速率限制。

![](img/63657aa9d34513faa3f9d11e07419eb0_96.png)

生成一个PR通常涉及多个步骤，每个步骤都可能消耗API调用额度：
1.  **Fork仓库**：调用GitHub API创建分支（需处理命名冲突）。
2.  **提交更改**：在分支上提交修复。
3.  **创建PR**：向原仓库发起拉取请求。

![](img/63657aa9d34513faa3f9d11e07419eb0_98.png)

为了高效、可持续地运行，需要精心设计逻辑来处理速率限制和错误重试。

![](img/63657aa9d34513faa3f9d11e07419eb0_100.png)

---

![](img/63657aa9d34513faa3f9d11e07419eb0_102.png)

## 利用Moderne平台进行规模化操作

![](img/63657aa9d34513faa3f9d11e07419eb0_104.png)

OpenRewrite团队提供了Moderne平台。该平台已索引了数千个开源项目，并免费提供服务。安全研究员可以将自己编写的OpenRewrite“食谱”（修复逻辑）提交到该平台，平台便能自动在这些被索引的项目上运行食谱，检测变更，并生成格式良好的拉取请求。

![](img/63657aa9d34513faa3f9d11e07419eb0_106.png)

这解决了从“拥有修复能力”到“实际为成千上万项目提交修复”的“最后一公里”问题。研究员只需运行食谱，平台便会处理fork、提交、创建PR等一系列繁琐操作。

![](img/63657aa9d34513faa3f9d11e07419eb0_108.png)

![](img/63657aa9d34513faa3f9d11e07419eb0_110.png)

---

## 发现更多目标项目

![](img/63657aa9d34513faa3f9d11e07419eb0_112.png)

Moderne目前索引了约7000个项目，但世界上有更多的开源仓库。如何找到其他易受攻击的项目呢？

![](img/63657aa9d34513faa3f9d11e07419eb0_114.png)

我们可以再次利用CodeQL。CodeQL索引了超过10万个开源项目（其中约3.5万个是Java项目）。我们可以编写CodeQL查询来发现特定漏洞，然后将易受攻击的项目列表贡献给OpenRewrite社区或相关存储库。这样，OpenRewrite/Moderne就能意识到这些新项目，从而可以用相应的食谱去修复它们。

---

![](img/63657aa9d34513faa3f9d11e07419eb0_116.png)

## 成果与最佳实践

![](img/63657aa9d34513faa3f9d11e07419eb0_118.png)

![](img/63657aa9d34513faa3f9d11e07419eb0_120.png)

通过结合CodeQL（发现）、OpenRewrite（修复）、Moderne（规模化执行）以及自定义机器人（处理API），我们已经取得了显著成果。

![](img/63657aa9d34513faa3f9d11e07419eb0_122.png)

**成果示例**：我们已经针对临时目录劫持、部分路径遍历、ZIP Slip等漏洞，生成了数百个拉取请求。在2022年，仅个人就生成了超过5000个拉取请求，对开源安全产生了实质性影响。

![](img/63657aa9d34513faa3f9d11e07419eb0_124.png)

在与开源维护者互动时，遵循一些最佳实践至关重要：
1.  **签署提交（GPG签名）**：这能验证提交者身份，增加可信度，并可能避免一些法律层面的顾虑。
2.  **使用规范的提交信息**：考虑使用类似“Conventional Commits”的格式，清晰说明修复的安全问题。
3.  **协调沟通**：在开始大规模提交PR前，最好与GitHub或相关平台协调，告知你的计划，避免账户因自动化行为被限制。
4.  **考虑影响**：大规模披露漏洞可能对维护者团队造成短期负担。需要权衡“修复大量漏洞的长期收益”与“给维护者带来的短期压力”。清晰、 actionable（可操作）的沟通信息可以减轻这种负担。
5.  **使用个人账户**：进行此类自动化操作可能对账户有风险，建议使用专门的或个人账户。

![](img/63657aa9d34513faa3f9d11e07419eb0_126.png)

---

## 总结与呼吁

在本节课中，我们一起学习了如何通过自动化工具链扩展安全研究员的能力，以大规模消除开源软件漏洞。我们从**发现**（CodeQL）开始，到**精确修复**（OpenRewrite），再到**规模化执行**（Moderne/自定义机器人），形成了一个完整的解决方案。

作为安全研究员，我们知道这些漏洞的存在。面对开发者与安全研究员人数比例的悬殊（例如，每500名开发者对应1名安全研究员），传统手动方法无法应对海量的漏洞。自动化批量修复技术，是扩展我们专业知识、在开源世界产生最大积极影响的最佳途径。

正如Dan Kaminsky所说：“我们能修复它，我们有技术。” 现在，我们需要创造并利用这种技术。

**你可以参与其中**：
*   为OpenRewrite项目贡献安全修复“食谱”。
*   加入GitHub Security Lab的Slack频道，讨论需要修复的漏洞。
*   考虑加入Linux基金会的开源安全基金会（OpenSSF），参与开源安全的讨论和行动。

通过协作和先进工具，我们可以让开源软件变得更加安全。

![](img/63657aa9d34513faa3f9d11e07419eb0_128.png)

---
*致谢：感谢Human Security、Moderne、OpenRewrite团队、GitHub Security Lab以及实习生Sham的工作，使得这项技术成为可能。特别感谢演讲教练Lydia Juliano对本次演讲的指导。*