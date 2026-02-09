# i春秋零基础入门Android逆向 - P2：课时2 调试方法及Smali文件结构 📱

![](img/cf495bfa417e2038cf8bee4cba13d2a2_1.png)

在本节课中，我们将学习Android开发的基础知识，并重点介绍如何调试Android应用以及理解Smali文件的结构。我们将从创建一个简单的“Hello World”应用开始，逐步深入到代码的反编译、修改和调试过程。

## 概述

![](img/cf495bfa417e2038cf8bee4cba13d2a2_3.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_4.png)

本节课将分为几个部分：首先，我们将解决上节课遗留的Android SDK安装问题；接着，我们将通过Android Studio创建一个基础的应用程序；然后，我们会分析APK文件的构成，并介绍Smali语言；最后，我们将学习如何对应用进行反编译、修改和调试。

![](img/cf495bfa417e2038cf8bee4cba13d2a2_6.png)

---

![](img/cf495bfa417e2038cf8bee4cba13d2a2_8.png)

## Android SDK安装问题解决 🛠️

![](img/cf495bfa417e2038cf8bee4cba13d2a2_10.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_12.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_13.png)

上一节我们介绍了课程概述，本节中我们来看看如何解决Android SDK安装过程中可能遇到的问题。

![](img/cf495bfa417e2038cf8bee4cba13d2a2_14.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_15.png)

安装Android SDK时，并非所有包都需要安装。通常，只需安装默认选中的包即可。等待绿色进度条完成后，系统会自动检测并勾选缺失的组件，点击安装即可。

![](img/cf495bfa417e2038cf8bee4cba13d2a2_17.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_19.png)

如果因网络或代理设置问题导致下载失败，可以尝试设置代理。在Android Studio的 `Tools` -> `Options` 中，可以配置代理地址和端口。

---

![](img/cf495bfa417e2038cf8bee4cba13d2a2_21.png)

## 创建第一个Android应用：Hello World 🌍

![](img/cf495bfa417e2038cf8bee4cba13d2a2_23.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_24.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_26.png)

现在，我们正式开始教程。绝大多数程序员的第一个程序都是“Hello World”。本节我们将使用Android Studio来创建它。

![](img/cf495bfa417e2038cf8bee4cba13d2a2_28.png)

我们统一使用Android Studio进行开发。请确保网络环境良好，以便顺利下载依赖包。

![](img/cf495bfa417e2038cf8bee4cba13d2a2_30.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_32.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_33.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_35.png)

以下是创建新项目的步骤：

1.  打开Android Studio，选择 `Start a new Android Studio project`。
2.  在引导窗口中，填写应用名称（Application Name），例如“HelloWorld”。
3.  填写包名（Package Name），这是应用在系统中的唯一标识符。
4.  选择项目存放路径。
5.  选择最低支持的Android版本（Min SDK），例如API 15（Android 4.0.3）。
6.  选择一个初始的Activity模板，然后点击 `Finish`。

![](img/cf495bfa417e2038cf8bee4cba13d2a2_37.png)

项目构建完成后，可以看到一个包含“Hello World”文本和按钮的界面。点击运行按钮（绿色三角形）即可在模拟器或真机上运行应用。

![](img/cf495bfa417e2038cf8bee4cba13d2a2_39.png)

---

## Android项目结构解析 🗂️

上一节我们创建了第一个应用，本节中我们来分析一下Android项目的目录结构。

![](img/cf495bfa417e2038cf8bee4cba13d2a2_41.png)

一个Android项目主要由以下几个部分组成：

![](img/cf495bfa417e2038cf8bee4cba13d2a2_43.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_45.png)

*   **`java` 目录**：存放项目的Java源代码。
*   **`res` 目录**：存放资源文件，如布局（`layout`）、字符串（`values`）、图片（`drawable`）等。
*   **`AndroidManifest.xml` 文件**：每个APK都必须有的核心文件，用于声明应用的基本属性、权限和组件（如Activity）。

编译生成的APK文件本质上是一个ZIP压缩包。我们可以用压缩工具或反编译工具查看其内容。

编译前后文件的对应关系如下：
*   `java` 目录的代码被编译成 `classes.dex` 文件。
*   `res` 目录的资源被编译到 `resources.arsc` 等文件中。
*   `AndroidManifest.xml` 被编译为二进制格式。

![](img/cf495bfa417e2038cf8bee4cba13d2a2_47.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_49.png)

使用反编译工具（如Android Killer）打开APK，可以看到 `classes.dex` 被反编译成了 **Smali** 代码。Smali是Android Dalvik虚拟机的寄存器语言，是Java字节码的一种人类可读的表示形式。在Android逆向中，我们主要分析和修改的就是Smali代码。

![](img/cf495bfa417e2038cf8bee4cba13d2a2_51.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_53.png)

---

## Activity与布局基础 🖼️

理解了项目结构后，本节我们来看看构成用户界面的核心组件：Activity。

![](img/cf495bfa417e2038cf8bee4cba13d2a2_55.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_57.png)

Activity代表一个应用窗口。一个Activity要运行，必须在 `AndroidManifest.xml` 中注册。

![](img/cf495bfa417e2038cf8bee4cba13d2a2_59.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_61.png)

注册Activity的格式如下：
```xml
<activity android:name=".MainActivity">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```
其中，`android:name` 指定Activity的类名。`<intent-filter>` 中的 `MAIN` 和 `LAUNCHER` 表示该Activity是应用的入口。

![](img/cf495bfa417e2038cf8bee4cba13d2a2_63.png)

Activity的界面布局由 `res/layout` 目录下的XML文件定义。在Activity的Java代码中，通过 `setContentView(R.layout.activity_main)` 方法将布局与Activity关联起来。

![](img/cf495bfa417e2038cf8bee4cba13d2a2_65.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_67.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_69.png)

---

![](img/cf495bfa417e2038cf8bee4cba13d2a2_71.png)

## 动手实践：创建与修改Activity ✨

![](img/cf495bfa417e2038cf8bee4cba13d2a2_73.png)

理论需要结合实践，本节我们将动手创建一个新的Activity并修改其内容。

![](img/cf495bfa417e2038cf8bee4cba13d2a2_75.png)

以下是创建一个新Activity的步骤：

![](img/cf495bfa417e2038cf8bee4cba13d2a2_77.png)

1.  在 `res/layout` 目录下右键，创建新的布局文件（如 `activity_hello.xml`）。
2.  在 `java` 目录下创建对应的Java类（如 `HelloActivity`），并继承 `AppCompatActivity`。
3.  在新Activity的 `onCreate` 方法中，调用 `setContentView(R.layout.activity_hello)` 关联布局。
4.  在 `AndroidManifest.xml` 中注册这个新的Activity。

![](img/cf495bfa417e2038cf8bee4cba13d2a2_79.png)

若想在主Activity中启动新的Activity，可以使用以下代码：
```java
Intent intent = new Intent(MainActivity.this, HelloActivity.class);
startActivity(intent);
```

![](img/cf495bfa417e2038cf8bee4cba13d2a2_81.png)

我们还可以修改界面元素，例如为一个按钮添加点击事件来修改文本框的内容：
```java
Button myButton = findViewById(R.id.my_button);
EditText myEditText = findViewById(R.id.my_edit_text);

![](img/cf495bfa417e2038cf8bee4cba13d2a2_83.png)

myButton.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        myEditText.setText("新文本内容");
    }
});
```

![](img/cf495bfa417e2038cf8bee4cba13d2a2_85.png)

---

## Smali文件结构详解 📄

开发基础已经介绍完毕，现在我们将重点转向逆向分析的核心——Smali代码。

![](img/cf495bfa417e2038cf8bee4cba13d2a2_87.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_88.png)

Smali文件有固定的格式。我们以之前创建的 `HelloActivity` 对应的Smali文件为例进行分析。

一个典型的Smali文件结构如下：

![](img/cf495bfa417e2038cf8bee4cba13d2a2_90.png)

```
.class <访问权限> <类名>;
.super <父类>;
.source <源文件名>

![](img/cf495bfa417e2038cf8bee4cba13d2a2_91.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_93.png)

# 字段定义
.field ...

![](img/cf495bfa417e2038cf8bee4cba13d2a2_95.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_97.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_99.png)

# 方法定义
.method <访问权限> <方法名>(<参数类型>)<返回类型>
    .registers <寄存器数量> # 声明本方法中使用的寄存器总数
    <指令集> # 方法的实际执行指令
.end method
```

![](img/cf495bfa417e2038cf8bee4cba13d2a2_101.png)

**关键元素解析：**

![](img/cf495bfa417e2038cf8bee4cba13d2a2_103.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_105.png)

*   **`.class`**：定义类，例如 `.class public Lcom/example/HelloActivity;`。在Smali中，类名以 `L` 开头，包名中的点用 `/` 代替，最后以 `;` 结尾。
*   **`.super`**：指定父类。
*   **`.source`**：对应的源Java文件名。
*   **`.method` / `.end method`**：定义一个方法块。
*   **寄存器**：Smali使用寄存器（v0, v1, p0等）存储数据。p0通常代表 `this` 对象。
*   **类型描述符**：
    *   基本类型：`Z` (boolean), `B` (byte), `S` (short), `C` (char), `I` (int), `J` (long), `F` (float), `D` (double), `V` (void)
    *   对象类型：`Lpackage/name/ObjectName;` (例如 `Ljava/lang/String;`)
    *   数组类型：`[` (一维数组，例如 `[I` 表示 `int[]`)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_107.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_109.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_110.png)

**方法调用示例：**
`invoke-virtual {v0}, Landroid/widget/TextView;->setText(Ljava/lang/CharSequence;)V`
这行代码表示调用 `v0` 寄存器所指向对象（一个TextView）的 `setText` 方法，参数类型为 `CharSequence`，返回类型为 `void`。

![](img/cf495bfa417e2038cf8bee4cba13d2a2_112.png)

---

![](img/cf495bfa417e2038cf8bee4cba13d2a2_114.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_116.png)

## 修改与调试Smali代码 🔧

![](img/cf495bfa417e2038cf8bee4cba13d2a2_118.png)

理解了Smali结构后，本节我们学习如何修改并调试它，这是逆向工程的关键步骤。

![](img/cf495bfa417e2038cf8bee4cba13d2a2_120.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_122.png)

**1. 修改Smali代码**
我们可以直接修改反编译后的Smali文件。例如，找到设置文本的代码，修改其字符串常量。字符串在Smali中通常以UTF-8编码形式存在。可以使用工具（如Android Killer内的字符串转换工具）进行编码和解码。
修改后，需要回编译并签名APK才能安装测试。

![](img/cf495bfa417e2038cf8bee4cba13d2a2_124.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_126.png)

**2. 调试Smali代码（传统方法）**
调试允许我们动态跟踪代码执行流程。一个经典的方法是使用 `apktool` 对APK进行反编译并注入调试等待代码。

![](img/cf495bfa417e2038cf8bee4cba13d2a2_128.png)

以下是调试步骤概要：

![](img/cf495bfa417e2038cf8bee4cba13d2a2_130.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_132.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_134.png)

*   **反编译**：`apktool d -d target.apk -o output_dir` (使用 `-d` 参数生成可调试的Smali)。
*   **修改清单文件**：在 `AndroidManifest.xml` 的 `<application>` 标签内添加 `android:debuggable="true"`。
*   **注入调试等待**：在入口Activity的 `onCreate` 方法开始处，添加Smali代码 `invoke-static {}, Landroid/os/Debug;->waitForDebugger()V`。
*   **回编译与签名**：使用 `apktool b` 回编译，并对生成的APK进行签名。
*   **配置IDE调试**：在Android Studio中配置远程调试（Remote），主机为 `localhost`，端口为 `8700`。
*   **启动调试**：安装并运行修改后的APK，应用会卡在等待调试器界面。使用 `DDMS` 或 `Monitor` 工具选中对应进程，然后在Android Studio中启动调试连接。

![](img/cf495bfa417e2038cf8bee4cba13d2a2_136.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_137.png)

连接成功后，就可以在Smali代码中设置断点，单步执行（F8），观察寄存器值的变化，从而深入理解程序逻辑。

![](img/cf495bfa417e2038cf8bee4cba13d2a2_139.png)

---

![](img/cf495bfa417e2038cf8bee4cba13d2a2_141.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_143.png)

## 总结与作业 📚

本节课我们一起学习了Android开发的基础、项目结构、Smali语法以及如何对APK进行反编译、修改和调试。

![](img/cf495bfa417e2038cf8bee4cba13d2a2_145.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_146.png)

**核心要点总结：**
1.  Android应用由Java代码、资源文件和清单文件构成，编译后生成APK。
2.  Smali是Dalvik字节码的汇编语言，是逆向分析的主要对象。
3.  Smali文件有固定的结构，包括类定义、字段和方法。
4.  通过修改Smali代码并回编译，可以改变应用行为。
5.  通过给APK添加调试属性并注入等待代码，可以实现动态调试。

![](img/cf495bfa417e2038cf8bee4cba13d2a2_148.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_150.png)

**课后作业：**
1.  仿照教程，创建一个包含按钮并能启动另一个Activity的APP。
2.  将第一个页面中的“Hello World”文本修改为“你好世界”。
3.  尝试使用 `apktool` 和 Android Studio 调试一个简单的应用，熟悉Smali指令的执行过程。

![](img/cf495bfa417e2038cf8bee4cba13d2a2_152.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_154.png)

![](img/cf495bfa417e2038cf8bee4cba13d2a2_156.png)

熟练掌握这些基础，将为后续更复杂的Android逆向分析打下坚实的基础。