![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_1.png)

# i春秋零基础入门Android逆向 - P6：课时6 分析神器JEB使用方法 🛠️

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_3.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_5.png)

在本节课中，我们将学习安卓逆向分析中一个强大的工具——JEB。我们将通过分析一个Java程序（JEB 2的Demo版）的注册算法，来掌握JEB的基本操作、高级技巧以及如何编写插件来自动化解密流程。

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_7.png)

---

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_9.png)

## 概述

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_11.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_12.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_13.png)

本节课是第一部分课程的最后一节。我们将重点介绍JEB工具在安卓逆向中的应用，目标是分析一个Java程序的注册算法，获取其注册密钥。通过本课，你将学会JEB的基本操作、分析技巧以及编写插件来辅助分析。

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_15.png)

---

## JEB基本操作与目标程序

上一节我们介绍了Smali和Java代码的基础知识，本节中我们来看看如何使用JEB进行实际分析。

首先，我们的目标程序是一个Java程序（JEB 2 Demo版），它需要一个注册密钥才能正常使用。我们的任务是分析其注册算法。

由于JEB不直接识别`.jar`文件，我们需要使用Android SDK中的`dx`工具将其转换为`.dex`文件。

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_17.png)

转换命令如下：
```bash
dx --dex --output=输出文件路径 输入的jar文件路径
```
转换完成后，将生成的`.dex`文件拖入JEB进行分析。

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_19.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_21.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_23.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_25.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_26.png)

JEB的界面和快捷键与IDA类似，熟悉IDA的用户可以快速上手。

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_28.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_30.png)

以下是JEB的一些基本操作：
*   **跳转到入口点**：Java程序的入口点是`main`函数。在JEB中，可以找到`Launcher`类，按`Tab`键进行反编译查看。
*   **导航与跟踪**：双击方法或按`Enter`键可以跳转到其定义。按`Esc`键返回，按`Ctrl+Enter`前进。
*   **添加注释**：对重要的代码行，可以按`C`键或分号`;`来添加注释。
*   **进制转换**：对数字常量，可以按`B`键在十进制和十六进制之间切换。
*   **重命名**：对变量、方法或类，可以按`N`键进行重命名，使代码更易读。

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_32.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_34.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_35.png)

---

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_37.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_39.png)

## 静态分析与字符串解密

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_41.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_43.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_45.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_47.png)

我们开始分析目标程序。在入口类中，我们寻找与注册相关的关键词，如“license”。

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_49.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_50.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_52.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_54.png)

跟踪进去后，我们发现一个类中包含了许多看似加密的数据，其形式类似于`SomeClass.someMethod(...)`。我们猜测这些是运行时动态解密的字符串。

双击跳转到该方法，确认它是一个字符串解密函数。该函数接收三个参数：一个`byte[]`数组和两个`int`型参数，返回解密后的字符串。

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_56.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_57.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_59.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_61.png)

为了验证，我们可以手动编写一个Java程序来调用这个解密函数。将JEB反编译出的代码复制到IDE中，修复一些因反编译导致的语法错误（例如`v1-1`这类寄存器类型转换标记，通常需要手动修正为`v1`）。

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_63.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_64.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_66.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_68.png)

修复后运行解密函数，成功输出了明文字符串，如“release”，证实了我们的猜想。

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_70.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_72.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_73.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_74.png)

然而，程序中存在大量此类加密字符串，手动解密效率低下。

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_76.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_77.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_79.png)

---

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_81.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_82.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_83.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_85.png)

## JEB插件开发与自动化

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_87.png)

这时，JEB的强大之处得以体现：它支持插件开发，允许我们编写脚本来自动化修改反编译结果。

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_89.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_91.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_93.png)

JEB提供了官方的API文档。编写插件的基本步骤是创建一个实现特定接口的Java类。

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_95.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_97.png)

一个最简单的“Hello World”插件示例如下：
```java
import com.pnfsoftware.jeb.client.api.*;
public class HelloPlugin implements IScript {
    public void run(IClientContext ctx) {
        ctx.getOutput().print("Hello World");
    }
}
```
在JEB中通过“File -> Execute Script”即可运行此脚本。

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_99.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_101.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_103.png)

但我们的目标是自动化解密字符串。我们需要编写一个更复杂的插件，其核心逻辑是：
1.  在程序中定位到我们之前找到的字符串解密方法。
2.  查找所有调用该方法的地方。
3.  动态执行解密逻辑，获取明文字符串。
4.  用解密后的字符串替换原有的方法调用代码。

以下是该插件工作流程的关键代码思路：
```java
// 1. 定义目标解密方法的签名
String targetMethodSig = "Lcom/example/Decryptor;->decode([BII)Ljava/lang/String;";
// 2. 获取JEB实例和所有方法
IDexUnit dex = ctx.getMainProject().getDecompiledCode();
int methodCount = dex.getMethodCount();
// 3. 遍历方法，匹配签名
for(int i=0; i<methodCount; i++) {
    IMethodItem method = dex.getMethod(i);
    if(method.getSignature(true).equals(targetMethodSig)) {
        // 4. 找到目标方法，获取其交叉引用（调用点）
        List<IMethodCall> calls = method.getCallers();
        for(IMethodCall call: calls) {
            // 5. 提取调用时的参数值（byte数组和两个int）
            IExpr[] args = call.getArguments();
            // 6. 模拟执行解密算法
            String decryptedString = simulateDecode(args[0], args[1], args[2]);
            // 7. 用字符串常量替换方法调用
            replaceCallWithString(call, decryptedString);
        }
    }
}
```
运行此插件后，程序中所有被加密的字符串都会被自动替换为明文，极大地提高了分析效率。

---

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_105.png)

## 注册算法分析与关键点定位

使用插件解密字符串后，代码变得清晰易读。我们回到注册逻辑的分析。

在`License`相关类中，我们找到了检查注册码的关键函数`checkKey`。该函数会获取用户输入的注册码，并与程序生成的正确注册码进行比对。

我们跟踪`checkKey`函数，发现它调用了一个对比函数。该函数接收两个参数：用户输入的密钥和一个由机器码生成的密钥。我们需要分析这个对比函数的算法。

同时，我们需要找到“机器码”的生成方式。通过交叉引用，我们追踪到机器码的生成类。在Windows系统下，它通过执行`WMIC`命令获取硬件信息（如`csproduct`的`UUID`），然后进行一系列运算（如求MD5值）来生成最终的机器码。

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_107.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_109.png)

注册算法的验证逻辑就建立在这个机器码和用户输入的基础上。具体的比对算法涉及一些运算，这部分将作为课后练习。

---

## 总结与作业

本节课我们一起学习了JEB分析工具的核心使用方法。
*   我们掌握了JEB的基本操作，如导航、注释、重命名。
*   我们学习了如何分析被混淆或加密的代码，特别是字符串的动态解密。
*   我们深入了解了JEB最强大的功能之一——插件开发，并编写了一个自动化解密字符串的插件，大幅提升了逆向效率。
*   最后，我们对一个具体程序的注册算法进行了初步分析，定位了机器码生成和密钥验证的关键代码。

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_111.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_113.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_115.png)

**本节课的作业**：完成对目标程序注册验证函数的完整静态分析，理解其算法逻辑，并尝试推导出有效的注册密钥。

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_117.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_119.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_121.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_123.png)

![](img/a6cfe84a3e48d4ccdd047b2d4f20610d_124.png)

通过前六节课的学习，你应该已经对安卓应用的结构、Smali/Java代码以及基本的静态分析技巧有了初步的了解。接下来可以尝试分析一些简单的应用程序来巩固这些技能。