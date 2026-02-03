![](img/8663a7a46ddeff5363f8d8fca562af20_1.png)

![](img/8663a7a46ddeff5363f8d8fca562af20_3.png)

# i春秋零基础入门Android逆向 - P20：课时4 ARM汇编代码讲解4 🧠

在本节课中，我们将深入学习ARM与Thumb指令集的区别、识别方法，并掌握如何对SO文件中的汇编代码进行修改和爆破。

![](img/8663a7a46ddeff5363f8d8fca562af20_5.png)

## 概述

上一节我们介绍了ARM汇编的基本指令和调试方法。本节中，我们来看看ARM与Thumb指令集的具体差异，并学习如何在实际逆向工程中识别、修改和爆破SO文件中的代码。

## ARM与Thumb指令集

![](img/8663a7a46ddeff5363f8d8fca562af20_7.png)

![](img/8663a7a46ddeff5363f8d8fca562af20_9.png)

ARM架构在发展过程中，为了提升运算速度，在最初的ARM指令集基础上加入了新的指令集，这就产生了Thumb指令集。

![](img/8663a7a46ddeff5363f8d8fca562af20_11.png)

![](img/8663a7a46ddeff5363f8d8fca562af20_12.png)

![](img/8663a7a46ddeff5363f8d8fca562af20_14.png)

![](img/8663a7a46ddeff5363f8d8fca562af20_16.png)

![](img/8663a7a46ddeff5363f8d8fca562af20_18.png)

以下是两者的核心区别：
*   **ARM指令集**：属于复杂指令集（CISC）。每条指令固定占**4个字节**。
*   **Thumb指令集**：属于精简指令集（RISC）。每条指令固定占**2个字节**，能实现的指令较少，但解码和执行速度更快。

![](img/8663a7a46ddeff5363f8d8fca562af20_20.png)

一个函数内部不能同时存在ARM和Thumb代码，但CPU可以通过特定的跳转指令（如`BX`, `BLX`）在运行时动态切换执行模式。

## 识别ARM与Thumb代码

![](img/8663a7a46ddeff5363f8d8fca562af20_22.png)

IDA等工具对代码模式的识别有时并不准确，特别是混淆过的代码。这时需要人工识别。

![](img/8663a7a46ddeff5363f8d8fca562af20_24.png)

识别方法主要看每条指令对应的机器码长度：
*   **Thumb代码**：每条指令对应**2个字节**的机器码。
*   **ARM代码**：每条指令对应**4个字节**的机器码。

在IDA中，可以按`U`键取消定义，再按`C`键强制转换代码类型，或使用`Alt+G`快捷键修改识别版本。

## 代码修改与爆破实战

![](img/8663a7a46ddeff5363f8d8fca562af20_26.png)

分析代码的最终目的往往是修改其逻辑，例如实现爆破（绕过验证）。这需要对函数的机器码进行修改。

![](img/8663a7a46ddeff5363f8d8fca562af20_28.png)

![](img/8663a7a46ddeff5363f8d8fca562af20_30.png)

![](img/8663a7a46ddeff5363f8d8fca562af20_31.png)

以下是修改SO文件代码的一般步骤：

![](img/8663a7a46ddeff5363f8d8fca562af20_33.png)

1.  **定位目标函数**：通过静态分析或动态调试，找到需要修改的关键函数（如校验函数）。
2.  **分析修改点**：确定需要修改的指令。例如，将返回失败结果的指令改为返回成功。
3.  **计算新机器码**：使用汇编器或相关工具（如“无名小件”），将我们想写入的新指令汇编成对应的机器码。
4.  **应用修改**：在IDA中，通过 `Edit -> Patch program -> Change byte...` 直接修改机器码。
5.  **保存修改**：使用 `Edit -> Patch program -> Apply patches to input file...` 将修改保存到新的SO文件。
6.  **替换文件**：将修改后的SO文件推送到Android设备的对应目录（如 `/data/app/<package_name>/lib/`），替换原文件并赋予执行权限。

![](img/8663a7a46ddeff5363f8d8fca562af20_34.png)

## 跳转指令偏移计算

在修改代码时，经常需要计算或修改跳转指令（如`B`, `BL`）的偏移量。

![](img/8663a7a46ddeff5363f8d8fca562af20_36.png)

![](img/8663a7a46ddeff5363f8d8fca562af20_37.png)

偏移量的计算公式为：
```
偏移量 = (目标地址 - 当前PC值) / 指令长度
```
*   **对于Thumb指令**：`指令长度` 为 **2**。
*   **对于ARM指令**：`指令长度` 为 **4**。

**当前PC值** 的计算需要注意：对于ARM，`PC`指向当前指令地址加**8**；对于Thumb，`PC`指向当前指令地址加**4**。

![](img/8663a7a46ddeff5363f8d8fca562af20_39.png)

![](img/8663a7a46ddeff5363f8d8fca562af20_40.png)

通过修改跳转指令中的偏移字段，可以改变程序的执行流程。

![](img/8663a7a46ddeff5363f8d8fca562af20_42.png)

![](img/8663a7a46ddeff5363f8d8fca562af20_43.png)

![](img/8663a7a46ddeff5363f8d8fca562af20_45.png)

## 总结

![](img/8663a7a46ddeff5363f8d8fca562af20_47.png)

![](img/8663a7a46ddeff5363f8d8fca562af20_49.png)

![](img/8663a7a46ddeff5363f8d8fca562af20_50.png)

![](img/8663a7a46ddeff5363f8d8fca562af20_52.png)

![](img/8663a7a46ddeff5363f8d8fca562af20_54.png)

本节课中我们一起学习了ARM与Thumb指令集的核心区别与识别方法，并掌握了SO文件代码修改和爆破的完整流程，包括定位、分析、计算机器码、应用补丁和替换文件。我们还了解了跳转指令偏移量的计算方法，这是进行更复杂代码流控制修改的基础。掌握这些技能，是进行Android应用逆向分析和修改的关键一步。