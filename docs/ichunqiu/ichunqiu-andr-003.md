#  003：课时3 新版本调试方法及Smali函数文件修改 🛠️

![](img/ec6fffeb29c7be7bf9d552347e87ce56_1.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_3.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_5.png)

在本节课中，我们将学习一款新的反编译工具的使用，并深入掌握Smali代码的调试与静态分析方法。我们将通过一个实际的示例程序，演示如何定位关键函数、动态调试获取关键值，以及如何修改Smali代码来改变程序逻辑。

![](img/ec6fffeb29c7be7bf9d552347e87ce56_7.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_9.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_10.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_11.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_12.png)

## 一、 新反编译工具：Apktool的增强版

![](img/ec6fffeb29c7be7bf9d552347e87ce56_14.png)

上一节我们介绍了APK打包工具。本节中，我们来看看一款基于Apktool改造的、功能更强大的反编译工具。

![](img/ec6fffeb29c7be7bf9d552347e87ce56_16.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_17.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_18.png)

该工具的使用方法与Apktool基本一致，但功能更强大且Bug更少。以下是其基本命令：

![](img/ec6fffeb29c7be7bf9d552347e87ce56_20.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_22.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_24.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_26.png)

*   **反编译**：`java -jar <工具名>.jar d -f <apk文件> -o <输出目录>`
*   **回编译**：`java -jar <工具名>.jar b <反编译目录> -o <输出apk>`

![](img/ec6fffeb29c7be7bf9d552347e87ce56_28.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_30.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_31.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_32.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_34.png)

其中，`-d` 表示反编译，`-b` 表示回编译。

![](img/ec6fffeb29c7be7bf9d552347e87ce56_36.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_37.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_39.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_40.png)

一个特别重要的参数是 `-df`，它代表使用默认的框架资源文件。Android程序在反编译和回编译时需要引用外部的框架资源，这些资源通常来自Android源码编译。如果框架版本过旧，处理新版程序时可能会出错。工具默认会将框架文件安装在用户目录下的 `.apktool` 文件夹中。若需更新或清除旧框架，只需删除该文件夹下的文件即可。

![](img/ec6fffeb29c7be7bf9d552347e87ce56_42.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_43.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_44.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_46.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_47.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_49.png)

该工具还包含许多其他参数，在处理加壳或混淆的APP时非常有用，大家可以根据工具的说明文档自行测试。

![](img/ec6fffeb29c7be7bf9d552347e87ce56_51.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_52.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_53.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_55.png)

## 二、 新版本动态调试方法

![](img/ec6fffeb29c7be7bf9d552347e87ce56_57.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_58.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_59.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_61.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_62.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_64.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_65.png)

上一节我们学习了通过修改Smali代码并回编译的方式进行调试。本节介绍一种更为主流、无需修改原程序即可调试的方法。

![](img/ec6fffeb29c7be7bf9d552347e87ce56_67.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_68.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_70.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_72.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_73.png)

首先，我们使用Android Killer等工具对目标APK进行反编译，得到Smali代码目录。

![](img/ec6fffeb29c7be7bf9d552347e87ce56_75.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_76.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_77.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_79.png)

接着，在Android Studio中导入这个Smali代码目录。为了让Android Studio能识别并高亮显示Smali语法，我们需要安装一个名为 **SmaliIDEA** 的插件。

![](img/ec6fffeb29c7be7bf9d552347e87ce56_81.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_83.png)

安装方法如下：
1.  打开Android Studio，进入 `File -> Settings -> Plugins`。
2.  点击 `Install plugin from disk...`。
3.  选择下载好的 `SmaliIDEA` 插件文件进行安装。
4.  安装完成后重启Android Studio。

重启后，Android Studio就能正确识别和显示Smali代码了。

![](img/ec6fffeb29c7be7bf9d552347e87ce56_85.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_87.png)

调试的准备工作与之前类似：在程序入口（通常是带有 `LAUNCHER` 属性的Activity的 `onCreate` 方法）处下好断点。

## 三、 启动调试会话

准备工作完成后，我们开始启动调试。

首先，确保目标程序已安装到设备或模拟器上。使用命令 `adb install <apk文件路径>` 进行安装。

然后，我们需要以调试模式启动目标应用。不能直接点击图标打开，而需要使用ADB命令：
`adb shell am start -D -n <包名>/<入口Activity全名>`

*   `-D` 表示以调试模式启动。
*   包名和入口Activity名可以在反编译后的 `AndroidManifest.xml` 文件中找到。

执行命令后，应用会启动并显示“Waiting For Debugger”。

![](img/ec6fffeb29c7be7bf9d552347e87ce56_89.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_91.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_92.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_94.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_96.png)

此时，在Android Studio中点击调试按钮（小虫子图标），选择对应的进程，即可附加调试器，程序会在我们下的断点处暂停。

![](img/ec6fffeb29c7be7bf9d552347e87ce56_98.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_100.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_102.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_104.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_106.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_107.png)

在调试过程中，我们可以使用 **Watch** 功能监视寄存器的值（如V1, V2），方便地观察变量变化。常用的调试快捷键有 **F7**（步入）、**F8**（步过）、**F9**（恢复运行）。

![](img/ec6fffeb29c7be7bf9d552347e87ce56_108.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_110.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_112.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_114.png)

## 四、 Smali代码静态分析与动态跟踪

![](img/ec6fffeb29c7be7bf9d552347e87ce56_116.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_118.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_120.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_121.png)

本节我们将结合静态分析和动态调试，来破解一个示例程序的注册算法。

![](img/ec6fffeb29c7be7bf9d552347e87ce56_123.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_125.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_127.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_128.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_130.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_131.png)

首先进行静态分析。在Android Studio中浏览Smali代码，寻找关键字符串（如“恭喜注册码正确”、“抱歉再接再厉”）。我们找到了显示注册成功的方法 `Lcom/example/MainActivity;->showSuccessDialog()`。

![](img/ec6fffeb29c7be7bf9d552347e87ce56_133.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_134.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_136.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_138.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_140.png)

通过右键查看该方法被引用的地方，我们定位到了按钮的点击事件 `onClick`。分析 `onClick` 方法的Smali代码，其逻辑可以翻译如下：

![](img/ec6fffeb29c7be7bf9d552347e87ce56_142.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_144.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_145.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_147.png)

```java
// 获取用户名输入框的文本
String username = ((EditText)findViewById(R.id.username)).getText().toString();
// 获取注册码输入框的文本
String inputCode = ((EditText)findViewById(R.id.password)).getText().toString();
// 根据用户名计算正确的注册码
String realCode = getPasswordFromName(username);
// 比较用户输入与正确注册码
boolean isCorrect = realCode.equals(inputCode);
// 根据比较结果显示对话框
showDialog(isCorrect);
```

![](img/ec6fffeb29c7be7bf9d552347e87ce56_149.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_151.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_153.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_154.png)

由此可知，核心算法在 `getPasswordFromName` 方法中。

![](img/ec6fffeb29c7be7bf9d552347e87ce56_156.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_158.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_160.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_162.png)

我们可以直接在 `getPasswordFromName` 方法返回结果的地方（即 `move-result-object v0` 指令后）下断点。在调试器中运行程序，输入用户名并点击按钮，当断点命中时，在监视窗口中查看寄存器 **V0** 的值，即为该用户名对应的正确注册码。

![](img/ec6fffeb29c7be7bf9d552347e87ce56_164.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_166.png)

如果想深入了解算法细节，可以进入 `getPasswordFromName` 方法内部进行跟踪调试。通过一步步执行，我们发现其算法大致是：将用户名转换为字节数组，然后取每个字节的值，对一个固定的字符串进行取模运算，用模的结果作为索引，从固定字符串中取出字符，拼接成最终的注册码。

![](img/ec6fffeb29c7be7bf9d552347e87ce56_168.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_170.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_172.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_174.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_176.png)

## 五、 修改Smali代码以改变程序逻辑

![](img/ec6fffeb29c7be7bf9d552347e87ce56_178.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_179.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_181.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_182.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_184.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_186.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_188.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_190.png)

如果不想每次都动态调试获取注册码，我们可以直接修改Smali代码，让程序无论输入什么都显示成功。

![](img/ec6fffeb29c7be7bf9d552347e87ce56_192.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_194.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_196.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_198.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_199.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_201.png)

在 `onClick` 事件中，找到判断结果并跳转的关键指令。通常是 `if-eqz` 或 `if-nez` 等条件跳转指令。我们的目标是让程序直接跳转到显示成功的分支。

![](img/ec6fffeb29c7be7bf9d552347e87ce56_203.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_205.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_207.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_208.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_210.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_212.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_213.png)

例如，找到跳转到失败分支的指令 `if-eqz v4, :cond_0`（如果v4为0，则跳转到cond_0标签处，显示失败）。我们可以将这条指令**注释掉**（在Smali中，使用`#`号注释一行），或者直接将其改为无条件跳转到成功标签的指令 `goto :cond_success`。

![](img/ec6fffeb29c7be7bf9d552347e87ce56_215.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_217.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_219.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_221.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_223.png)

修改完成后，使用反编译工具回编译APK并签名安装。再次运行程序，无论输入什么，都会显示注册成功。

![](img/ec6fffeb29c7be7bf9d552347e87ce56_225.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_226.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_227.png)

此外，我们还可以在Smali代码中插入Log输出语句，方便在Logcat中查看运行时的中间值，例如打印出计算出的正确注册码。这需要调用Android的 `Log` 类，并找到一个空闲的寄存器来传递参数。

![](img/ec6fffeb29c7be7bf9d552347e87ce56_229.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_230.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_231.png)

## 六、 使用现成的Java反编译工具

手动分析Smali代码固然能加深理解，但效率较低。实际上，有很多优秀的工具可以将Smali代码反编译成更易读的Java代码，例如 **JD-GUI**、**FernFlower**（集成在Android Studio中）等。

在Android Studio中，对Smali文件右键，选择 `Decompile to Java`，即可看到大致的Java伪代码。这可以作为我们静态分析的**重要参考**。但需要注意的是，反编译得到的Java代码可能不完整或存在错误，特别是程序经过加固或混淆后。因此，深入分析时仍需结合Smali代码和动态调试。

## 总结 🎯

本节课我们一起学习了以下内容：
1.  使用一款功能更强的Apktool改进版进行反编译和回编译。
2.  掌握了无需修改源码、直接对Smali代码进行动态调试的新方法。
3.  实践了从静态分析定位关键函数，到动态调试获取关键值（注册码）的完整流程。
4.  学习了通过修改Smali代码中的跳转逻辑，直接改变程序行为。
5.  了解了插入Log语句辅助分析以及使用Java反编译工具作为参考的方法。

![](img/ec6fffeb29c7be7bf9d552347e87ce56_233.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_235.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_237.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_238.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_239.png)

![](img/ec6fffeb29c7be7bf9d552347e87ce56_241.png)

通过本课的学习，你应该具备了分析简单Android程序逻辑、并运用调试和修改技术解决问题的能力。请务必完成课后练习，通过实践来巩固这些技能。