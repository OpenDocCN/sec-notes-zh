#  030：ARM汇编代码讲解5 - 使用Cydia Substrate进行Native Hook 🪝

![](img/9fcfabd8503de28fb6f8e12565251e0b_1.png)

在本节课中，我们将学习如何使用Cydia Substrate框架对Android原生（Native）层的函数进行Hook。我们将了解其基本概念、实现步骤和工作原理。

![](img/9fcfabd8503de28fb6f8e12565251e0b_3.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_5.png)

## 概述

![](img/9fcfabd8503de28fb6f8e12565251e0b_7.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_9.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_10.png)

Hook，即函数钩子，其核心思想是拦截并替换目标函数的执行流程。无论是Java层还是Native层，其思想是一致的：用自己的函数替换原始函数，以达到修改或增强功能的目的。本节课重点讲解在Native层（SO库）使用Cydia Substrate进行Hook的方法。

![](img/9fcfabd8503de28fb6f8e12565251e0b_12.png)

## 环境配置与SDK介绍

上一节我们介绍了ARM汇编的基础知识，本节中我们来看看如何将其应用于实际的Hook操作。首先需要配置Cydia Substrate环境。

![](img/9fcfabd8503de28fb6f8e12565251e0b_14.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_16.png)

Cydia Substrate SDK不仅提供了Java层的API，也提供了Native层的库文件（.so文件）。这意味着我们既可以在Java层挂钩，也可以在Native层对本地函数进行挂钩。需要注意的是，对Java层下钩目前只支持Dalvik虚拟机模式。

![](img/9fcfabd8503de28fb6f8e12565251e0b_17.png)

以下是配置步骤：

![](img/9fcfabd8503de28fb6f8e12565251e0b_19.png)

1.  将Cydia Substrate的`.jar`文件导入项目。
2.  将其提供的`.so`库文件（如`libsubstrate.so`和`libsubstrate-dvm.so`）放入项目的`jniLibs`目录下。Android Studio在打包时会自动将其包含进APK。
3.  修改`build.gradle`文件，在`ndk`模块中指定编译的ABI和链接库。

![](img/9fcfabd8503de28fb6f8e12565251e0b_20.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_22.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_24.png)

```gradle
android {
    ...
    defaultConfig {
        ...
        ndk {
            abiFilters 'armeabi-v7a', 'x86'
        }
    }
}
```

![](img/9fcfabd8503de28fb6f8e12565251e0b_26.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_27.png)

```gradile
// 在模块的build.gradle中
android {
    ...
    sourceSets.main {
        jniLibs.srcDir 'src/main/jniLibs'
    }
}
```

![](img/9fcfabd8503de28fb6f8e12565251e0b_29.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_31.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_33.png)

## Hook的实现流程

![](img/9fcfabd8503de28fb6f8e12565251e0b_35.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_37.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_39.png)

配置好环境后，我们开始编写Hook代码。核心操作在Native层完成。

Cydia Substrate for Android 提供了几个关键函数：
*   `MSGetImageByName` / `MSFindSymbol`: 用于获取SO库的基地址和函数地址。
*   `MSHookFunction`: 用于对函数进行挂钩。

![](img/9fcfabd8503de28fb6f8e12565251e0b_41.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_42.png)

我们通常在SO库加载时（即在初始化函数中）执行Hook操作。可以使用 `__attribute__((constructor))` 或Cydia Substrate提供的 `MSInitialize` 宏来定义初始化函数。

![](img/9fcfabd8503de28fb6f8e12565251e0b_44.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_46.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_48.png)

以下是一个简单的Hook示例，它挂钩一个名为`hello3`的函数，并将其替换为`hello_hook`函数：

![](img/9fcfabd8503de28fb6f8e12565251e0b_50.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_51.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_52.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_53.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_54.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_56.png)

```c
#include <substrate.h>

![](img/9fcfabd8503de28fb6f8e12565251e0b_58.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_60.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_62.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_64.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_66.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_67.png)

// 原始函数声明
int (*old_hello3)(int a, int b);

![](img/9fcfabd8503de28fb6f8e12565251e0b_69.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_71.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_73.png)

// 新的钩子函数
int hello_hook(int a, int b) {
    // 可以在此处添加自定义逻辑
    printf("Hello3 is hooked!\n");
    // 调用原始函数
    return old_hello3(a, b);
}

![](img/9fcfabd8503de28fb6f8e12565251e0b_75.png)

// 初始化函数，在SO加载时执行
MSInitialize {
    MSImageRef image = MSGetImageByName("/path/to/libtarget.so");
    if (image != NULL) {
        // 查找并挂钩hello3函数
        void *hello3_addr = MSFindSymbol(image, "hello3");
        if (hello3_addr != NULL) {
            MSHookFunction(hello3_addr, (void*)&hello_hook, (void**)&old_hello3);
        }
    }
}
```

代码说明：
*   `MSHookFunction` 的第一个参数是**原始函数的地址**。
*   第二个参数是**目标函数（钩子函数）的地址**。
*   第三个参数用于**保存原始函数的指针**，以便在钩子函数中继续调用原功能。

如果要对当前SO自身的函数进行Hook，可以省略获取基地址和查找符号的步骤，直接使用函数指针。

![](img/9fcfabd8503de28fb6f8e12565251e0b_77.png)

## Hook原理分析

仅仅实现功能还不够，理解其底层原理至关重要。下面我们通过调试来分析`MSHookFunction`是如何工作的。

![](img/9fcfabd8503de28fb6f8e12565251e0b_79.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_81.png)

![](img/9fcfabd8503de28fb6f8e12565251e0b_83.png)

当我们对一个函数（例如`hello3`）下钩后，其函数头部的指令会被修改。通常，它会替换为一段跳转代码（trampoline），将执行流导向我们的钩子函数（`hello_hook`）。

![](img/9fcfabd8503de28fb6f8e12565251e0b_85.png)

在ARM架构下，由于存在Thumb和ARM两种指令集，跳转时需要特别注意指令集切换。`MSHookFunction`生成的跳转代码会处理这些细节。例如，它可能先将PC寄存器指向一段保存的原始指令并切换指令集，然后再跳转到钩子函数。

当钩子函数需要调用原始函数时，它会通过之前保存的`old_hello3`指针来调用。这个指针指向一个“桥接”代码，这段代码会执行被Hook时替换掉的那几条原始指令，然后跳转到原始函数的剩余部分继续执行。

这个过程可以简化为以下流程：
1.  程序调用 `hello3`。
2.  控制权转向被修改的跳转指令。
3.  跳转指令将控制权交给 `hello_hook`。
4.  `hello_hook` 执行自定义代码。
5.  通过 `old_hello3()` 调用原始函数逻辑。
6.  原始函数执行完毕，返回 `hello_hook`。
7.  `hello_hook` 返回，整个调用结束。

## 总结

本节课我们一起学习了使用Cydia Substrate框架进行Android Native层函数Hook的完整流程。我们从环境配置开始，逐步实现了对一个本地函数的挂钩，并深入分析了Hook背后的指令替换和跳转原理。

关键点总结：
1.  **Hook本质**：替换函数执行流程，用自定义函数拦截目标函数。
2.  **工具**：使用Cydia Substrate的 `MSHookFunction` 等API可以简化复杂的底层操作。
3.  **时机**：通常在SO库加载的初始化阶段进行挂钩。
4.  **原理**：通过修改目标函数头部的指令为跳转代码，实现执行流重定向。同时保存原指令以便后续调用。

![](img/9fcfabd8503de28fb6f8e12565251e0b_87.png)

理解Native Hook对于Android逆向、安全分析和功能增强具有重要意义。虽然Cydia Substrate封装了大部分细节，但掌握其基本原理能帮助你在遇到问题时进行调试和深度定制。