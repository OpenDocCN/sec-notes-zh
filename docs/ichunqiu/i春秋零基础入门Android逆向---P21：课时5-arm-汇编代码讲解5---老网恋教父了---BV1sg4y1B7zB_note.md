![](img/336ec5bc5e232e9587a64d31161a675e_1.png)

# i春秋零基础入门Android逆向 - P21：课时5 ARM汇编代码讲解5 - 函数Hook原理与实践 🎯

在本节课中，我们将要学习Android Native层（SO库）函数Hook（挂钩）的原理与实现方法。我们将使用Cydia Substrate框架，通过一个简单的例子，演示如何替换一个SO库中的函数，并深入分析其底层运作机制。

![](img/336ec5bc5e232e9587a64d31161a675e_3.png)

## 概述

![](img/336ec5bc5e232e9587a64d31161a675e_5.png)

![](img/336ec5bc5e232e9587a64d31161a675e_7.png)

![](img/336ec5bc5e232e9587a64d31161a675e_9.png)

![](img/336ec5bc5e232e9587a64d31161a675e_10.png)

Hook，即函数挂钩，其核心思想是拦截并替换目标函数的执行流程。无论是Java层还是Native层，其目的都是用自己的函数来“代替”原函数，从而达到监控、修改或增强原功能的效果。本节课将重点讲解在Native层（SO库）对C/C++函数进行Hook的方法。

![](img/336ec5bc5e232e9587a64d31161a675e_12.png)

## Hook的基本概念与Cydia Substrate框架

上一节我们介绍了ARM汇编的基础知识，本节中我们来看看如何利用这些知识实现函数Hook。

![](img/336ec5bc5e232e9587a64d31161a675e_14.png)

Cydia Substrate（简称Substrate或Cydia）是一个强大的代码修改框架。它不仅提供了Java层的Hook能力，也提供了Native层的Hook支持。其SDK中包含用于Native层Hook的头文件（`.h`）和库文件（`.so`）。

![](img/336ec5bc5e232e9587a64d31161a675e_16.png)

![](img/336ec5bc5e232e9587a64d31161a675e_17.png)

**核心区别**：
*   **Java层Hook**：依赖于Java虚拟机，目前Substrate主要支持Dalvik虚拟机（即`/system/bin/app_process`），对ART模式（Android 5.0+默认）的支持有限。
*   **Native层Hook**：直接操作ELF（SO库）文件，与虚拟机模式无关，通用性更强。

本节课我们专注于Native层Hook。

![](img/336ec5bc5e232e9587a64d31161a675e_19.png)

![](img/336ec5bc5e232e9587a64d31161a675e_20.png)

![](img/336ec5bc5e232e9587a64d31161a675e_22.png)

![](img/336ec5bc5e232e9587a64d31161a675e_24.png)

## 项目配置与代码编写

![](img/336ec5bc5e232e9587a64d31161a675e_26.png)

![](img/336ec5bc5e232e9587a64d31161a675e_27.png)

以下是配置项目以使用Cydia Substrate进行Native Hook的步骤。

![](img/336ec5bc5e232e9587a64d31161a675e_29.png)

1.  **导入库文件**：将Substrate SDK中的`substrate.h`头文件以及`libsubstrate.so`、`libsubstrate-dvm.so`（如需Java Hook）库文件放入项目的`jni`目录下。通常，`libsubstrate.so`需要放置在`jni/libs`目录中，Android构建系统会自动将其打包进APK。

![](img/336ec5bc5e232e9587a64d31161a675e_31.png)

![](img/336ec5bc5e232e9587a64d31161a675e_33.png)

![](img/336ec5bc5e232e9587a64d31161a675e_35.png)

![](img/336ec5bc5e232e9587a64d31161a675e_37.png)

![](img/336ec5bc5e232e9587a64d31161a675e_39.png)

2.  **配置构建脚本**：在项目的`Android.mk`或`build.gradle`（NDK配置部分）中添加必要的链接标志和库。
    ```makefile
    # 示例 Android.mk 片段
    LOCAL_LDLIBS += -L$(LOCAL_PATH)/libs -lsubstrate -ldl
    LOCAL_CFLAGS := -DANDROID_ABI=\"armeabi-v7a\" # 指定ABI
    ```

3.  **编写Hook代码**：Hook的执行时机通常选择在SO库被加载的初始化阶段。我们可以使用Substrate提供的宏`MSInitialize`或类似`__attribute__((constructor))`的特性来定义初始化函数。

以下是Hook的核心代码示例：
```c
#include <substrate.h>

![](img/336ec5bc5e232e9587a64d31161a675e_41.png)

![](img/336ec5bc5e232e9587a64d31161a675e_43.png)

![](img/336ec5bc5e232e9587a64d31161a675e_45.png)

// 1. 声明原函数和目标函数
int (*old_hello3)(int); // 用于保存原函数指针
int new_hello3(int x) { // 新的钩子函数
    // 可以在此处添加自定义逻辑，例如修改参数、记录日志等
    int result = old_hello3(x * 2); // 调用原函数，但传入参数加倍
    return result + 100; // 修改返回值
}

![](img/336ec5bc5e232e9587a64d31161a675e_47.png)

![](img/336ec5bc5e232e9587a64d31161a675e_48.png)

![](img/336ec5bc5e232e9587a64d31161a675e_49.png)

![](img/336ec5bc5e232e9587a64d31161a675e_50.png)

![](img/336ec5bc5e232e9587a64d31161a675e_52.png)

// 2. 在初始化函数中进行挂钩
MSInitialize {
    MSImageRef image = MSGetImageByName("/data/app/com.example.hellojni-1/lib/arm/libhello-jni.so");
    if (image != NULL) {
        // 查找原函数地址并进行替换
        void *target = MSFindSymbol(image, "Java_com_example_hellojni_MainActivity_hello3");
        if (target != NULL) {
            MSHookFunction(target, (void*)&new_hello3, (void**)&old_hello3);
        }
    }
}
```
**代码解释**：
*   `MSGetImageByName`：获取指定SO库的镜像引用。
*   `MSFindSymbol`：在镜像中根据符号名查找函数地址。
*   `MSHookFunction`：核心Hook函数。它接受三个参数：
    1.  原函数的地址（`target`）。
    2.  新函数（钩子函数）的地址（`(void*)&new_hello3`）。
    3.  用于保存原函数地址的指针（`(void**)&old_hello3`），以便在钩子函数中能够调用原功能。

![](img/336ec5bc5e232e9587a64d31161a675e_54.png)

![](img/336ec5bc5e232e9587a64d31161a675e_56.png)

![](img/336ec5bc5e232e9587a64d31161a675e_58.png)

![](img/336ec5bc5e232e9587a64d31161a675e_59.png)

![](img/336ec5bc5e232e9587a64d31161a675e_61.png)

## Hook原理深度分析

![](img/336ec5bc5e232e9587a64d31161a675e_63.png)

![](img/336ec5bc5e232e9587a64d31161a675e_65.png)

当我们调用`MSHookFunction`后，究竟发生了什么？我们可以使用IDA Pro等反汇编工具进行动态调试来观察。

![](img/336ec5bc5e232e9587a64d31161a675e_67.png)

**Hook前**：目标函数（例如`hello3`）的代码段是完整的、未被修改的机器指令。

**Hook后**：目标函数的**头部指令被替换**。通常会被替换为一段“跳板代码”（Trampoline）。这段代码的主要作用是：
1.  将执行流无条件跳转到我们定义的**钩子函数**（`new_hello3`）。
2.  可能会保存被覆盖的原始指令，以便后续执行。

**执行流程**：
1.  程序调用原函数 `hello3`。
2.  CPU执行到被修改的函数头部，立即跳转至 `new_hello3`。
3.  在 `new_hello3` 中，我们可以执行自定义逻辑。
4.  如果需要调用原函数功能，则通过之前保存的 `old_hello3` 指针进行调用。`old_hello3` 指向的是一段“存根”（Stub），它包含了被覆盖的原始指令，并最终跳回 `hello3` 函数被修改处之后继续执行。
5.  原函数执行完毕后，控制权返回 `new_hello3`。
6.  `new_hello3` 可以处理原函数的返回值，最后将结果返回给最初的调用者。

![](img/336ec5bc5e232e9587a64d31161a675e_69.png)

**关键点**：这种直接修改函数代码的方式，需要处理ARM/Thumb指令集切换、指令对齐、相对跳转计算等复杂细节。Cydia Substrate等成熟框架封装了这些底层操作，使我们能够专注于Hook逻辑本身。

## 实践演示与调试

![](img/336ec5bc5e232e9587a64d31161a675e_71.png)

![](img/336ec5bc5e232e9587a64d31161a675e_73.png)

![](img/336ec5bc5e232e9587a64d31161a675e_75.png)

在视频中，讲师通过修改一个简单的JNI示例程序，演示了Hook过程：
1.  **未Hook时**：调用 `hello3(7)` 返回 `1403`（假设是 `7 * 200 + 3` 的结果）。
2.  **Hook后**：在 `new_hello3` 中将输入参数加倍（`7*2=14`），调用原函数后再给结果加100。最终调用 `hello3(7)` 返回了不同的结果，证明Hook成功。

![](img/336ec5bc5e232e9587a64d31161a675e_77.png)

通过IDA Pro动态调试，可以清晰地看到：
*   `hello3` 函数头部的指令被修改为 `B.W PC, #?` 等形式的跳转指令。
*   执行流如何从 `hello3` 跳转到 `new_hello3`。
*   在 `new_hello3` 中，通过 `old_hello3` 调用原函数存根的过程。

## 其他Hook方法简介

除了上述直接修改函数体的“Inline Hook”外，还有其他Hook方法：
*   **导入表Hook（IAT Hook）**：修改ELF文件的导入表（`.plt.got`等），将外部函数调用指向自己的函数。这种方法通常用于Hook动态链接库中的函数调用。
*   **异常处理Hook**：通过制造异常并接管异常处理流程来实现Hook，更为隐蔽但实现复杂。

这些方法各有优缺点和适用场景，`MSHookFunction` 属于Inline Hook，是功能最强大、最通用的一种。

## 总结

本节课中我们一起学习了Android Native层函数Hook的核心知识：
1.  **Hook概念**：用自己的函数替换目标函数，以拦截、监控或修改其行为。
2.  **使用Cydia Substrate**：学习了如何配置项目并使用 `MSHookFunction` 等API实现简单的Native Hook。
3.  **Hook原理**：深入分析了Inline Hook通过修改目标函数头部指令，实现执行流劫持和跳转的底层机制。
4.  **调试与分析**：通过反汇编和动态调试，直观地验证了Hook的执行流程。

![](img/336ec5bc5e232e9587a64d31161a675e_79.png)

掌握函数Hook技术是Android逆向和安全分析中的关键技能，它允许我们深入理解应用程序的运行机制，并实现强大的动态分析、漏洞检测或功能增强。