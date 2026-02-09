# i春秋零基础入门Android逆向 - P18：课时2 ARM汇编代码讲解2 🧠

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_1.png)

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_2.png)

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_4.png)

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_5.png)

在本节课中，我们将深入学习ARM汇编代码的分析，重点探讨SO文件的加载顺序、栈与内存数据段的区别、指针概念、进制转换以及函数调用规则。我们将通过一个具体的`hellonet`程序实例，结合静态分析与动态调试，详细拆解其汇编逻辑。

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_7.png)

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_9.png)

---

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_11.png)

## 概述

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_13.png)

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_14.png)

本节课我们将分析一个名为`hellonet`的Android应用程序。该程序的Java层会调用一个名为`hello3`的Native函数，并传入一个整型参数`100`。我们的目标是深入分析对应的SO文件（`libhello.so`）中的`hello3`函数，理解其汇编实现、参数传递、局部变量存储以及函数调用栈的平衡原理。

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_16.png)

---

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_17.png)

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_19.png)

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_20.png)

## SO文件分析工具介绍

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_22.png)

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_23.png)

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_25.png)

在深入分析汇编代码之前，首先介绍两款用于分析ELF格式SO文件的工具。

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_27.png)

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_29.png)

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_30.png)

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_32.png)

### 1. readelf工具

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_34.png)

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_35.png)

`readelf`是Linux环境下强大的ELF文件格式查看工具，可以显示SO文件的头部信息、程序头、节头、动态链接信息等。

以下是使用`readelf`查看SO文件所有信息的命令示例：
```bash
readelf -a libhello.so > log.txt
```
此命令将分析结果输出到`log.txt`文件中，便于详细查看。

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_37.png)

### 2. SO Helper工具

`SO Helper`是一款图形化分析工具，提供符号表查看、区段信息、反编译等功能，适合在图形界面下进行初步分析。

---

## SO文件加载与初始化函数

SO文件在加载到内存时，系统会主动调用一些特定的函数来完成初始化工作。

*   **init系列函数**：以`.init`或`_init`开头的函数，在SO加载时最早被调用，用于全局变量或环境的初始化。
*   **JNI_OnLoad函数**：Android系统特有的函数，在SO被`System.loadLibrary`加载时调用，用于注册Native方法。
*   **fini系列函数**：以`.fini`或`_fini`开头的函数，在SO卸载时调用，用于清理资源。但在程序运行时通常不会执行。

---

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_39.png)

## 栈与内存数据段

理解栈和内存数据段的区别对于分析汇编代码至关重要。

*   **栈**：用于存储函数内的**局部变量**、函数参数、返回地址等。栈空间由系统自动分配和释放，遵循“后进先出”原则。函数执行时栈顶下移（地址减小）分配空间，函数返回时栈顶上移（地址增大）释放空间。
*   **内存数据段**：用于存储**全局变量**和**静态变量**。这些变量在程序整个生命周期内都存在，可以被所有函数访问。

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_41.png)

例如，在C代码中：
```c
int global_var = 10; // 存储在内存数据段

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_43.png)

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_44.png)

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_45.png)

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_46.png)

void func() {
    int local_var = 20; // 存储在栈上
}
```

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_47.png)

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_49.png)

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_51.png)

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_52.png)

---

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_54.png)

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_56.png)

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_58.png)

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_60.png)

## 指针概念

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_62.png)

指针是C/C++语言的核心概念，它存储的是内存地址。通过指针，程序可以直接访问和操作内存中的数据，这使得编程非常灵活但也容易出错。在汇编层面，指针通常体现为对某个寄存器所存地址的加载和存储操作。

---

## 进制转换

计算机底层使用二进制存储数据。但在分析和调试时，我们更常使用十六进制表示，因为它与二进制转换直观（每4位二进制对应1位十六进制）。

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_64.png)

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_66.png)

转换关系遵循“8421”编码规则：
*   十六进制 `0xF` = 二进制 `1111` (8+4+2+1=15)
*   十六进制 `0x5` = 二进制 `0101` (4+1=5)
*   十六进制 `0x3` = 二进制 `0011` (2+1=3)

在IDA等调试器中，内存数据默认以十六进制显示。例如，十进制`100`等于十六进制`0x64`。

---

## 函数参数传递规则（ATPCS）

ARM架构遵循ATPCS规范进行函数参数传递：
*   前4个参数通过寄存器 **R0、R1、R2、R3** 传递。
*   第5个及之后的参数通过**栈**来传递。
*   函数的返回值通常通过寄存器 **R0** 返回。

在我们的例子中，`hello3`函数有三个参数（JNIEnv*、jobject、int），因此分别通过R0、R1、R2传递。

---

## 动态调试实战分析

上一节我们介绍了基础概念，本节中我们来看看如何对`hello3`函数进行动态调试与逐行分析。

我们使用IDA Pro同时进行静态分析和动态调试。一份IDA实例用于静态查看代码结构，另一份用于附加到运行中的进程进行动态跟踪。

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_68.png)

### 函数入口与栈帧建立

在`hello3`函数开头，我们观察到典型的栈帧建立代码：

```
PUSH    {R7, LR}
SUB     SP, SP, #0x18
MOV     R7, SP
```

以下是上述代码的详细解释：

1.  `PUSH {R7, LR}`：将R7（帧指针）和LR（链接寄存器，保存返回地址）的值压入栈中保存。
2.  `SUB SP, SP, #0x18`：将栈指针SP下移`0x18`（24）字节，为局部变量分配空间。
3.  `MOV R7, SP`：将当前栈指针SP的值赋给R7。之后函数可以通过R7来访问局部变量（因为SP可能在函数执行中变化，而R7是固定的）。

### 参数保存与局部变量

分配栈空间后，函数将传入的参数保存到栈上的局部变量区域：

```
STR     R0, [R7,#0x14]    ; 保存 JNIEnv* 参数
STR     R1, [R7,#0x10]    ; 保存 jobject 参数
STR     R2, [R7,#0xC]     ; 保存 int 参数 (100 -> 0x64)
```

此时，栈布局如下（地址从高到低增长）：
```
SP+0x18 | ...          |
        | 保存的 LR     |
        | 保存的 R7     | <- R7 指向这里
        | JNIEnv* (R0) | (R7+0x14)
        | jobject (R1) | (R7+0x10)
        | int 参数 (R2) | (R7+0xC)
        | 局部变量 sum  | (R7-0x8)
        | 局部变量 i    | (R7-0x4)
SP      | ...          |
```

### 循环累加逻辑分析

函数的核心是一个循环，对从0到传入参数（100）的整数进行累加。其汇编逻辑对应以下C代码：
```c
int sum = 0;
for(int i = 0; i < input; i++) {
    sum += i;
}
```

以下是循环部分的汇编代码分析：

```
; 初始化循环变量 i 为 0
MOVS    R3, #0
STR     R3, [R7,#-0x4]

; 循环开始标签
loc_xxxx
; 加载全局变量 base (本例中为0) 和 循环变量 i
LDR     R2, =g_base
LDR     R3, [R7,#-0x4]
LDR     R2, [R2]        ; R2 = g_base
CMP     R2, R3          ; 比较 g_base 和 i
BGE     exit_loop       ; 如果 i >= g_base? 这里实际是比较 i 和输入参数

; 累加操作：sum += i
LDR     R2, [R7,#-0x8]  ; 加载 sum
LDR     R3, [R7,#-0x4]  ; 加载 i
ADDS    R2, R2, R3      ; R2 = sum + i
STR     R2, [R7,#-0x8]  ; 存回 sum

; 循环变量 i 自增
LDR     R3, [R7,#-0x4]
ADDS    R3, #1
STR     R3, [R7,#-0x4]
B       loc_xxxx        ; 跳回循环开始

exit_loop:
```

### 函数调用与返回

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_70.png)

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_71.png)

循环结束后，函数将累加结果`sum`作为参数，调用另一个函数进行处理：

```
LDR     R0, [R7,#-0x8]  ; 将 sum 加载到 R0 作为参数
BL      sub_xxxx        ; 调用函数
MOV     R1, R0          ; 将返回值保存到 R1
```

最后，函数准备返回，需要清理栈帧：

```
ADD     R7, R7, #0x18   ; R7 指向栈帧底部，准备释放局部变量空间
POP     {R7, PC}        ; 从栈中恢复旧的 R7，并将保存的返回地址弹出到 PC，实现函数返回
```

**栈平衡原理**：函数调用前后，栈指针（SP）必须恢复到相同的位置。这是通过`SUB SP, SP, #xxx`分配空间，并在返回前通过`ADD R7, R7, #xxx`（或直接操作SP）释放等量空间来实现的。如果栈不平衡，会导致后续函数调用出错或程序崩溃。

---

## 总结

本节课我们一起深入分析了ARM汇编代码。我们学习了如何使用`readelf`和`SO Helper`分析SO文件，理解了SO的初始化函数、栈与内存数据段的区别、指针概念、进制转换以及ATPCS参数传递规则。

通过动态调试`hello3`函数，我们实战演练了：
1.  函数栈帧的建立与销毁过程。
2.  参数和局部变量在栈上的布局与访问。
3.  循环结构的汇编实现。
4.  函数调用时的参数传递与返回值处理。
5.  **栈平衡原理**的重要性。

![](img/dfd3e2abf134b8f4fe4f9b057fe8803e_73.png)

这些概念是逆向分析的基础，虽然琐碎但至关重要。请务必结合实操练习，加深理解。