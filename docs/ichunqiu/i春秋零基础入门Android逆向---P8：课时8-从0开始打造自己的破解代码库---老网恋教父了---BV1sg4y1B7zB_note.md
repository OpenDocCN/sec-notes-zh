# i春秋零基础入门Android逆向 - P8：课时8 从0开始打造自己的破解代码库 🛠️

![](img/88f1c0c6675b4fc40effcecc05abff56_1.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_3.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_5.png)

在本节课中，我们将学习如何从零开始编写和整合Smali代码，打造属于自己的Android逆向破解代码库。我们将通过手写Smali代码来加深对Android程序底层执行逻辑的理解，并学习如何将这些代码片段封装成可复用的工具。

![](img/88f1c0c6675b4fc40effcecc05abff56_7.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_9.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_10.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_12.png)

## 概述

![](img/88f1c0c6675b4fc40effcecc05abff56_14.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_16.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_18.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_20.png)

上一节我们介绍了如何利用现有工具进行简单的代码修改。本节中，我们将深入底层，学习如何直接编写Smali代码来修改和增强Android应用的功能。掌握这项技能后，你将能进行更高级的流程控制和功能修改。

![](img/88f1c0c6675b4fc40effcecc05abff56_22.png)

## 编写Smali代码的基础

![](img/88f1c0c6675b4fc40effcecc05abff56_24.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_25.png)

与在Windows平台可以用C++或汇编语言编写程序类似，在Android平台上，我们也可以直接使用Smali代码来编写程序。学好Smali代码，能让我们对程序进行更高级的改动，甚至改变整个程序的执行流程。

![](img/88f1c0c6675b4fc40effcecc05abff56_27.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_29.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_30.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_31.png)

首先，我们来回顾一下代码插入。在之前的课程中，我们已经学习过如何插入Log代码来打印程序消息。

![](img/88f1c0c6675b4fc40effcecc05abff56_33.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_34.png)

以下是插入Log代码的基本步骤：
1.  在目标方法中，右键选择“插入代码”。
2.  选择插入Log，并指定要打印的寄存器（例如V0）。
3.  编译运行后，即可在日志监视器中看到输出的字符串。

![](img/88f1c0c6675b4fc40effcecc05abff56_36.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_37.png)

其原理是，Android逆向工具（如Android Killer）在插入这句代码时，实际上在APK中插入了一个包含`Log.d`方法的辅助类。我们可以通过查看插入后的代码来理解其实现。

![](img/88f1c0c6675b4fc40effcecc05abff56_39.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_41.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_43.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_44.png)

## 创建自定义Smali代码库

![](img/88f1c0c6675b4fc40effcecc05abff56_46.png)

我们可以模仿工具内置的代码模板，手写自己的Smali代码，并将其整合到工具中，形成私人的破解代码库。

![](img/88f1c0c6675b4fc40effcecc05abff56_48.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_50.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_51.png)

以下是查看和管理内置代码模板的方法：
1.  点击“插入代码管理器”。
2.  工具内置了三个模板：`loadLibrary`（加载SO文件）、`Log`（打印日志）、`Toast`（显示提示信息）。
3.  这些模板都引用了外部的Smali代码文件。

接下来，我们尝试手写一个Smali类。

![](img/88f1c0c6675b4fc40effcecc05abff56_53.png)

### 手写一个简单的Smali类

![](img/88f1c0c6675b4fc40effcecc05abff56_55.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_56.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_57.png)

我们将创建一个返回特定字符串的Smali类。

![](img/88f1c0c6675b4fc40effcecc05abff56_59.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_60.png)

首先，在合适的位置创建一个新的`.smali`文件，例如 `Hello.smali`。一个基本的Smali文件包含以下部分：

![](img/88f1c0c6675b4fc40effcecc05abff56_62.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_63.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_65.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_67.png)

*   **类声明**：指定类的完整路径。
*   **父类声明**：通常继承自 `Ljava/lang/Object;`。
*   **字段和方法**：定义类的属性和行为。

![](img/88f1c0c6675b4fc40effcecc05abff56_69.png)

以下是一个返回字符串的Smali类示例代码框架：
```smali
.class public Lcom/example/Hello; # 类声明
.super Ljava/lang/Object; # 父类声明

![](img/88f1c0c6675b4fc40effcecc05abff56_71.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_73.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_75.png)

# 接下来可以定义字段(field)和方法(method)
```

![](img/88f1c0c6675b4fc40effcecc05abff56_77.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_79.png)

### 编写一个静态方法

![](img/88f1c0c6675b4fc40effcecc05abff56_81.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_82.png)

我们在类中编写一个静态方法，用于返回字符串“Hello from Smali!”。

![](img/88f1c0c6675b4fc40effcecc05abff56_84.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_85.png)

以下是编写静态方法的步骤和代码：
1.  方法以 `.method` 开始，以 `.end method` 结束。
2.  需要声明方法的访问权限（`public`）、属性（`static`）、名称和描述符。
3.  使用 `.locals` 声明本地寄存器的数量。
4.  使用 `const-string` 将字符串加载到寄存器。
5.  使用 `return-object` 返回寄存器的值。

![](img/88f1c0c6675b4fc40effcecc05abff56_87.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_89.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_91.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_93.png)

具体实现代码如下：
```smali
.method public static getHelloString()Ljava/lang/String;
    .locals 1 # 声明使用1个本地寄存器（V0）

    const-string v0, "Hello from Smali!" # 将字符串存入V0
    return-object v0 # 返回V0中的字符串
.end method
```

编写完成后，在目标方法中调用这个静态方法，并用其返回值替换原有的字符串。调用静态方法使用 `invoke-static` 指令，然后用 `move-result-object` 将结果存入指定寄存器。

![](img/88f1c0c6675b4fc40effcecc05abff56_95.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_96.png)

### 操作静态字段（Field）

![](img/88f1c0c6675b4fc40effcecc05abff56_98.png)

字段（Field）是类中定义的变量。下面我们演示如何声明并操作一个静态字段。

![](img/88f1c0c6675b4fc40effcecc05abff56_99.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_101.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_103.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_105.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_107.png)

首先，在类中声明一个静态的、不可变的（`final`）字符串字段并赋值：
```smali
.field public static final staticField:Ljava/lang/String; = "Static Field Value"
```

![](img/88f1c0c6675b4fc40effcecc05abff56_109.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_111.png)

然后，编写一个静态方法来读取这个字段的值。读取静态字段使用 `sget-object` 指令：
```smali
.method public static getStaticField()Ljava/lang/String;
    .locals 1
    sget-object v0, Lcom/example/Hello;->staticField:Ljava/lang/String; # 读取静态字段到V0
    return-object v0
.end method
```
**注意**：`.locals` 声明的寄存器数量必须与实际使用的数量一致，否则会导致程序崩溃。

![](img/88f1c0c6675b4fc40effcecc05abff56_112.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_113.png)

### 编写非静态（实例）方法

![](img/88f1c0c6675b4fc40effcecc05abff56_115.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_116.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_118.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_120.png)

非静态方法属于类的实例，调用前需要先创建对象。这涉及到构造方法（`<init>`）的编写。

![](img/88f1c0c6675b4fc40effcecc05abff56_122.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_124.png)

每个类都有一个构造方法。如果没有显式编写，编译器会生成一个默认的、调用父类构造方法的版本。我们手动编写一个构造方法：
```smali
.method public constructor <init>()V
    .locals 0
    invoke-direct {p0}, Ljava/lang/Object;-><init>()V # 调用父类Object的构造方法
    return-void
.end method
```
在非静态方法中，寄存器 `p0` 代表 `this` 指针（即对象自身）。

![](img/88f1c0c6675b4fc40effcecc05abff56_126.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_128.png)

接着，我们编写一个普通的实例方法：
```smali
.method public getInstanceString()Ljava/lang/String;
    .locals 1
    const-string v0, "Instance Method String"
    return-object v0
.end method
```

![](img/88f1c0c6675b4fc40effcecc05abff56_130.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_131.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_133.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_134.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_136.png)

要调用这个实例方法，需要先创建对象，然后调用其构造方法，最后才能调用目标方法：
```smali
# 创建对象
new-instance v0, Lcom/example/Hello;
# 调用构造方法
invoke-direct {v0}, Lcom/example/Hello;-><init>()V
# 调用实例方法
invoke-virtual {v0}, Lcom/example/Hello;->getInstanceString()Ljava/lang/String;
move-result-object v0
```

![](img/88f1c0c6675b4fc40effcecc05abff56_138.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_140.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_142.png)

### 操作实例字段

![](img/88f1c0c6675b4fc40effcecc05abff56_144.png)

对于非静态字段，其值属于对象实例。我们通常在构造方法中对它们进行初始化。

![](img/88f1c0c6675b4fc40effcecc05abff56_146.png)

首先声明一个实例字段：
```smali
.field private instanceField:Ljava/lang/String;
```

![](img/88f1c0c6675b4fc40effcecc05abff56_148.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_149.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_151.png)

然后在构造方法中为其赋值。为字段赋值使用 `iput-object` 指令：
```smali
.method public constructor <init>()V
    .locals 1
    invoke-direct {p0}, Ljava/lang/Object;-><init>()V

    const-string v0, "Instance Field Init Value"
    iput-object v0, p0, Lcom/example/Hello;->instanceField:Ljava/lang/String; # 用p0（this）和V0初始化字段
    return-void
.end method
```

![](img/88f1c0c6675b4fc40effcecc05abff56_153.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_155.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_157.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_159.png)

编写一个方法来获取这个字段的值，使用 `iget-object` 指令：
```smali
.method public getInstanceField()Ljava/lang/String;
    .locals 1
    iget-object v0, p0, Lcom/example/Hello;->instanceField:Ljava/lang/String; # 读取字段到V0
    return-object v0
.end method
```

![](img/88f1c0c6675b4fc40effcecc05abff56_161.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_163.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_165.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_167.png)

**提示**：在实际逆向工作中，我们通常不会完全手写Smali代码。更高效的做法是：先用Java写好功能代码，编译成APK后反编译得到Smali代码，再将需要的部分复制出来进行修改和整合。手写练习的主要目的是加深对Smali语法和调用过程的理解。

![](img/88f1c0c6675b4fc40effcecc05abff56_169.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_171.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_172.png)

## 常用破解代码技巧整合

![](img/88f1c0c6675b4fc40effcecc05abff56_174.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_175.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_177.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_179.png)

上一节我们介绍了如何编写基础的Smali类和方法。本节中，我们来看看如何将一些常用的调试和破解技巧封装成代码库，方便日后使用。

![](img/88f1c0c6675b4fc40effcecc05abff56_181.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_183.png)

### 堆栈跟踪（Stack Trace）

![](img/88f1c0c6675b4fc40effcecc05abff56_185.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_187.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_189.png)

堆栈跟踪可以打印出方法调用的完整链，帮助我们快速定位关键代码的调用路径。

![](img/88f1c0c6675b4fc40effcecc05abff56_191.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_192.png)

工具内置的 `StackTrace` 方法就是用于此目的。插入该代码后，当程序执行到该处时，会在日志中输出从该点开始向上的完整调用栈信息。这对于回溯关键方法的调用来源非常有用。

![](img/88f1c0c6675b4fc40effcecc05abff56_194.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_196.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_198.png)

### 方法跟踪（Method Trace）

![](img/88f1c0c6675b4fc40effcecc05abff56_200.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_201.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_203.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_205.png)

方法跟踪可以记录一段时间内所有被调用方法的详细信息，生成跟踪文件。

![](img/88f1c0c6675b4fc40effcecc05abff56_207.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_208.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_210.png)

这需要插入一对代码：`startMethodTracing` 和 `stopMethodTracing`。同时，需要在 `AndroidManifest.xml` 中添加写入外部存储的权限：
```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```
跟踪文件默认保存在 `/sdcard/` 目录下，可以使用 `adb pull` 命令导出，并用性能分析工具（如Traceview）查看。

### 字符串处理

![](img/88f1c0c6675b4fc40effcecc05abff56_212.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_213.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_215.png)

我们还可以封装一些常用的字符串操作方法，例如字符串替换。

![](img/88f1c0c6675b4fc40effcecc05abff56_217.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_219.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_221.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_223.png)

以下是一个调用字符串替换方法的Smali代码示例。它使用 `String.replace` 方法将原字符串中的 ‘L’ 全部替换为 ‘W’：
```smali
const-string v0, "HelloWorld"
const-string v1, "L"
const-string v2, "W"
invoke-virtual {v0, v1, v2}, Ljava/lang/String;->replace(Ljava/lang/CharSequence;Ljava/lang/CharSequence;)Ljava/lang/String;
move-result-object v0
```

![](img/88f1c0c6675b4fc40effcecc05abff56_225.png)

### 等待调试器（Wait For Debugger）

![](img/88f1c0c6675b4fc40effcecc05abff56_227.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_229.png)

`waitForDebugger` 方法会使程序暂停，等待调试器附加。这在动态调试时非常有用，可以确保我们在关键代码执行前挂上调试器。

![](img/88f1c0c6675b4fc40effcecc05abff56_231.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_233.png)

## 实战演练：破解游戏内购

![](img/88f1c0c6675b4fc40effcecc05abff56_235.png)

前面我们学习了代码库的构建。现在，我们综合运用这些技巧来破解一款游戏（以“神庙逃亡”为例）的内购功能。

![](img/88f1c0c6675b4fc40effcecc05abff56_237.png)

### 1. 定位关键代码

运行游戏并尝试购买，发现提示“用户取消操作”。我们可以搜索这个字符串来定位相关的支付逻辑代码。

在反编译后的代码中搜索该字符串，可以找到处理购买取消、成功、失败的回调方法。通常，这些方法在一个与支付相关的类中。

![](img/88f1c0c6675b4fc40effcecc05abff56_239.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_241.png)

### 2. 应对无法直接交叉引用的情况

![](img/88f1c0c6675b4fc40effcecc05abff56_242.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_243.png)

有时，关键方法可能没有被直接引用（例如通过接口或父类回调），导致无法通过简单的交叉引用找到调用点。这时，我们可以使用 **堆栈跟踪** 技巧。

在找到的取消回调方法中插入 `StackTrace` 代码。重新运行游戏并触发购买取消，观察日志中打印出的调用栈，即可一层层向上回溯，找到调用该方法的上层逻辑。

![](img/88f1c0c6675b4fc40effcecc05abff56_245.png)

### 3. 绕过签名验证（Signature Check）

很多应用会验证APK的签名，如果签名不一致（即非原版），则会拒绝运行或提示“盗版软件”。

**破解思路**：
1.  **搜索字符串**：搜索“盗版”、“签名无效”等提示字符串，定位到验证失败时调用的提示方法。
2.  **分析验证逻辑**：向上分析代码，找到进行签名校验并设置布尔值（如 `isPirate = true`）的地方。
3.  **修改验证结果**：找到设置校验结果的地方，将其强制改为通过验证（例如，将 `isPirate` 设为 `false`）。
4.  **深入分析（可选）**：有时验证逻辑较复杂，可能涉及网络验证。可以通过堆栈跟踪找到设置校验结果的地方，并向上分析其验证逻辑（如计算签名的MD5值并与服务器比对）。在实战中，可以抓取正版软件的通信包，分析其合法的签名数据，然后在我们修改的版本中直接返回合法的签名数据，从而绕过验证。

![](img/88f1c0c6675b4fc40effcecc05abff56_247.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_249.png)

### 4. 修改内购逻辑

![](img/88f1c0c6675b4fc40effcecc05abff56_251.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_253.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_255.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_257.png)

在绕过签名验证后，就可以专注于内购逻辑本身。通常，购买成功会调用一个 `onSuccess` 方法，而取消或失败会调用 `onCancel` 或 `onFailure` 方法。

![](img/88f1c0c6675b4fc40effcecc05abff56_259.png)

**破解方法**：
*   **直接跳转**：找到判断购买结果的地方，修改代码逻辑，使其无论支付结果如何，都直接跳转到 `onSuccess` 的处理流程。
*   **修改返回值**：找到检查支付状态的函数，让其始终返回“支付成功”的状态码。

![](img/88f1c0c6675b4fc40effcecc05abff56_261.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_262.png)

**作业**：请根据本节课所学，尝试独立完成对“神庙逃亡”游戏内购功能的破解，并确保能绕过其签名验证。

![](img/88f1c0c6675b4fc40effcecc05abff56_264.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_266.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_268.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_270.png)

## 总结

![](img/88f1c0c6675b4fc40effcecc05abff56_272.png)

本节课中，我们一起学习了如何从零开始打造自己的Android逆向破解代码库。

![](img/88f1c0c6675b4fc40effcecc05abff56_274.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_276.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_277.png)

1.  **Smali代码编写**：我们学习了Smali文件的基本结构，并实践了如何编写静态方法、实例方法、操作静态字段和实例字段。这是进行深度修改的基础。
2.  **代码技巧整合**：我们了解了如何将堆栈跟踪、方法跟踪、字符串处理、等待调试器等常用功能封装成可复用的Smali代码块，提高逆向效率。
3.  **实战应用**：我们通过破解游戏内购的案例，综合运用了字符串搜索、堆栈跟踪、签名验证绕过等技巧，展示了自定义代码库在实际逆向工作中的强大作用。

![](img/88f1c0c6675b4fc40effcecc05abff56_279.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_281.png)

![](img/88f1c0c6675b4fc40effcecc05abff56_283.png)

通过构建属于自己的代码库，你可以将常用的破解模式固化下来，在未来的逆向工程中节省大量时间，并能够处理更加复杂的修改需求。