![](img/634662718c8d11f70b7e390a5a7f6ca8_1.png)

# i春秋零基础入门Android逆向 - P24：课时3 Android 动态代码自修改实现1 🛠️

在本节课中，我们将学习如何通过编程手段动态修改Android应用中的代码。我们将深入理解Dex文件在运行时的结构转换，并利用JNI接口直接操作内存中的方法结构体，实现代码的自修改。

## 概述：从手动修改到编程实现

在上一节课中，我们探讨了如何手动修改Dex文件中的`DexCode`结构来改变代码。本节中，我们将关注如何通过编程手段实现相同的效果。

Android中的函数代码存储在`DexCode`结构中，该结构包含`insnsSize`（指令长度）和`insns`（指令实际内容）字段。我们的目标就是找到并修改这个结构。

## Android运行时的结构转换

在Android系统加载并执行Dex文件时，并不会直接使用存储格式的结构。为了便于执行，系统会通过特定函数将存储结构转换为运行时结构。

以下是主要的转换关系：
*   `DexCode` 和 `DexMethod` 结构会转换为 `Method` 结构。
*   `DexClassData` 会转换为 `Class` 结构。
*   `DexHeader` 和 `DexMapList` 会转换为 `DvmDex` 结构。

这些转换代码位于 `/dalvik/vm/oo/` 目录下（Dalvik模式）。理解这些转换过程对我们后续的代码修改至关重要。

## 寻找代码修改的切入点

为了修改代码，我们需要找到运行时`Method`结构体。一个直接的想法是解析Dex文件结构，但实现起来较为复杂。

更巧妙的方法是从JNI（Java Native Interface）入手。JNI提供了`GetMethodID`函数，它返回的`jmethodID`实际上就是`Method`结构体的指针。我们可以通过强制类型转换来获取这个指针。

核心概念代码如下：
```c
// 假设 method 是一个 jmethodID
Method* methodStruct = (Method*) method;
```

![](img/634662718c8d11f70b7e390a5a7f6ca8_3.png)

## 编程实现步骤

![](img/634662718c8d11f70b7e390a5a7f6ca8_5.png)

以下是实现动态代码修改的基本步骤。

![](img/634662718c8d11f70b7e390a5a7f6ca8_7.png)

![](img/634662718c8d11f70b7e390a5a7f6ca8_9.png)

![](img/634662718c8d11f70b7e390a5a7f6ca8_11.png)

![](img/634662718c8d11f70b7e390a5a7f6ca8_13.png)

![](img/634662718c8d11f70b7e390a5a7f6ca8_15.png)

![](img/634662718c8d11f70b7e390a5a7f6ca8_17.png)

### 1. 准备项目与JNI环境
首先，创建一个Android项目并配置JNI支持。这包括编写C/C++代码、配置`Android.mk`或`CMakeLists.txt`文件，并确保能正确调用Native方法。

### 2. 导出必要的运行时结构体
Android SDK并未公开`Method`等内部结构体的定义。我们需要从Android源码中手动导出这些结构体的定义，并包含到我们的JNI项目中。

![](img/634662718c8d11f70b7e390a5a7f6ca8_19.png)

![](img/634662718c8d11f70b7e390a5a7f6ca8_20.png)

![](img/634662718c8d11f70b7e390a5a7f6ca8_22.png)

![](img/634662718c8d11f70b7e390a5a7f6ca8_24.png)

![](img/634662718c8d11f70b7e390a5a7f6ca8_26.png)

![](img/634662718c8d11f70b7e390a5a7f6ca8_28.png)

### 3. 获取目标方法的 jmethodID
在Java层，使用反射获取目标方法的`java.lang.reflect.Method`对象。在JNI层，调用`GetMethodID`函数获取其对应的`jmethodID`。

![](img/634662718c8d11f70b7e390a5a7f6ca8_30.png)

![](img/634662718c8d11f70b7e390a5a7f6ca8_32.png)

### 4. 转换并修改 Method 结构体
将获取到的`jmethodID`强制转换为`Method*`指针。通过该指针，我们可以访问`insns`字段，它指向方法指令码的存储位置。直接修改`insns`指针所指向内存的内容，即可改变方法的执行逻辑。

**注意**：直接修改内存可能涉及内存页的访问权限（如只读）。在某些情况下，需要先使用`mprotect`等系统调用修改权限，这将在后续课程中介绍。

![](img/634662718c8d11f70b7e390a5a7f6ca8_34.png)

![](img/634662718c8d11f70b7e390a5a7f6ca8_36.png)

### 5. 验证修改结果
调用被修改的Java方法，观察其行为是否已按预期改变，以验证代码修改是否成功。

## 方法评价与注意事项

![](img/634662718c8d11f70b7e390a5a7f6ca8_38.png)

![](img/634662718c8d11f70b7e390a5a7f6ca8_40.png)

![](img/634662718c8d11f70b7e390a5a7f6ca8_42.png)

本节课介绍的方法原理直接，但存在一个主要问题：**兼容性**。

![](img/634662718c8d11f70b7e390a5a7f6ca8_44.png)

![](img/634662718c8d11f70b7e390a5a7f6ca8_45.png)

由于`Method`等内部结构体是未公开的，其布局可能因Android系统版本或设备制造商的不同而有所差异。这意味着我们的代码可能需要为不同的环境做适配，增加了维护成本。

![](img/634662718c8d11f70b7e390a5a7f6ca8_47.png)

![](img/634662718c8d11f70b7e390a5a7f6ca8_48.png)

![](img/634662718c8d11f70b7e390a5a7f6ca8_50.png)

因此，虽然该方法有助于理解原理，但在实际应用中（如加固壳）使用较少。更通用的方法是直接修复Dex文件，这将在下一节课中详细介绍。

![](img/634662718c8d11f70b7e390a5a7f6ca8_52.png)

![](img/634662718c8d11f70b7e390a5a7f6ca8_54.png)

## 总结

![](img/634662718c8d11f70b7e390a5a7f6ca8_56.png)

本节课我们一起学习了Android动态代码自修改的基本实现。
1.  我们了解了Dex文件从存储结构到运行时结构的转换过程。
2.  我们发现了通过JNI的`GetMethodID`可以获取内部`Method`结构体指针的窍门。
3.  我们实践了通过编程方式定位并修改`Method`结构体中的指令码，从而改变方法行为。
4.  我们也认识到该方法因依赖未公开结构体而存在的兼容性问题。

![](img/634662718c8d11f70b7e390a5a7f6ca8_58.png)

在下一节课中，我们将探讨另一种更为通用和稳定的代码修改方法。