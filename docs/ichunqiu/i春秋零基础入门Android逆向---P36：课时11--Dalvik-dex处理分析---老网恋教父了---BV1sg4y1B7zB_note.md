![](img/b214c311bb36ee7dbdaebfc83773a437_1.png)

![](img/b214c311bb36ee7dbdaebfc83773a437_2.png)

# i春秋零基础入门Android逆向 - P36：课时11 Dalvik dex处理分析 🧠

![](img/b214c311bb36ee7dbdaebfc83773a437_4.png)

![](img/b214c311bb36ee7dbdaebfc83773a437_5.png)

在本节课中，我们将要学习Dalvik虚拟机（DVM）如何从内存中加载DEX文件。我们将深入源码，分析关键的加载流程和函数，并探讨这些知识如何应用于脱壳实践。

![](img/b214c311bb36ee7dbdaebfc83773a437_7.png)

## 概述

上一节我们介绍了DEX文件的基本结构。本节中我们来看看DVM系统是如何将内存中的DEX文件数据加载并转换为可执行格式的。我们将分析核心的加载函数，并理解其中可作为脱壳切入点的关键步骤。

## DEX文件加载入口函数分析

整个加载流程的入口是 `dvmDexFileOpenFromMem` 函数。该函数负责从内存中的字节数组打开一个DEX文件。

```c
DvmDex* dvmDexFileOpenFromMem(const u1* addr, size_t len, int flags)
```

该函数接收一个指向DEX文件原始数据的指针 `addr` 和其长度 `len`。它的核心作用是将内存中的DEX字节流转换为DVM内部可识别的 `DvmDex` 结构体。

函数首先会校验传入的参数是否有效。如果数据指针为空，则会抛出异常。接着，它会将传入的DEX数据拷贝到新的内存区域。

拷贝完成后，函数调用 `dexFileParse` 函数对DEX数据进行解析。

```c
DexFile* dexFileParse(const u1* data, size_t length, int flags)
```

![](img/b214c311bb36ee7dbdaebfc83773a437_9.png)

`dexFileParse` 函数会返回一个 `DexFile` 结构体指针。这个结构体非常重要，它内部保存了原始的DEX文件数据、解析后的类定义数据以及方法对象数据。

![](img/b214c311bb36ee7dbdaebfc83773a437_10.png)

![](img/b214c311bb36ee7dbdaebfc83773a437_12.png)

![](img/b214c311bb36ee7dbdaebfc83773a437_14.png)

如果解析成功，函数会继续创建一个 `DvmDex` 结构体，并将 `DexFile` 指针保存其中。最后，将这个 `DvmDex` 结构体添加到全局的DEX文件哈希表 `gDvm.userDexFiles` 中。完成这两步后，DVM就能识别并使用这个DEX文件了。

![](img/b214c311bb36ee7dbdaebfc83773a437_16.png)

## 深入解析流程

![](img/b214c311bb36ee7dbdaebfc83773a437_18.png)

以下是 `dexFileParse` 函数内部的关键步骤：

1.  **校验文件头**：首先调用 `dexFileParseFromMemory`，对内存中的DEX文件进行初步校验。
2.  **验证签名**：通过 `dexComputeChecksum` 和 `dexComputeSHA1Digest` 等函数，验证DEX文件的校验和与SHA1摘要，确保文件完整性。
3.  **解析数据**：调用核心函数 `dexFileParse`，将原始字节数据映射并解析为 `DexFile` 结构体。这个函数会设置DEX文件各部分（如字符串池、类型池、方法池）的偏移量和大小。
4.  **创建优化表**：生成 `pClassLookup` 哈希表。这个表用于加速类的查找过程，是DEX文件优化（odex）的一部分。

其中，`dexFileParse` 函数是内存加载的核心。一些壳程序会直接调用或挂钩此函数来完成解密后的DEX加载，因此它常被用作动态脱壳的断点位置。

## 关键数据结构

在加载过程中，有几个关键的数据结构：

![](img/b214c311bb36ee7dbdaebfc83773a437_20.png)

*   **`DexFile`**：代表一个已解析的DEX文件，包含原始数据指针和解析后的索引信息。
*   **`DvmDex`**：在 `DexFile` 基础上封装而成，是DVM内部表示DEX文件的主要结构。它包含一个 `DexFile*` 指针和一个 `pClassLookup` 表。
*   **`gDvm.userDexFiles`**：一个全局哈希表，保存了所有已加载 `DvmDex` 结构的“cookie”（即其内存地址）。这是实现通用脱壳机思路的关键。

![](img/b214c311bb36ee7dbdaebfc83773a437_22.png)

`DvmDex` 结构体中的 `memMap` 成员也值得注意，它有时会保存解密后的原始DEX文件数据。如果壳程序在加载后没有清除这部分数据，就可能从中直接获取到脱壳后的DEX。

![](img/b214c311bb36ee7dbdaebfc83773a437_24.png)

![](img/b214c311bb36ee7dbdaebfc83773a437_26.png)

## 脱壳思路启示

![](img/b214c311bb36ee7dbdaebfc83773a437_27.png)

通过分析上述流程，我们可以得到一种通用的脱壳思路：

![](img/b214c311bb36ee7dbdaebfc83773a437_29.png)

![](img/b214c311bb36ee7dbdaebfc83773a437_31.png)

![](img/b214c311bb36ee7dbdaebfc83773a437_32.png)

由于所有通过 `dvmDexFileOpenFromMem` 加载的DEX文件，其 `DvmDex` 指针（cookie）最终都会被注册到全局表 `gDvm.userDexFiles` 中。因此，通过遍历这个哈希表，我们就可以枚举出当前进程中所有已加载的DEX文件（包括壳解密后加载的原始DEX）。

![](img/b214c311bb36ee7dbdaebfc83773a437_33.png)

![](img/b214c311bb36ee7dbdaebfc83773a437_34.png)

许多脱壳工具正是基于这一原理。在下一节课中，我们将具体探讨如何编写代码来实现这一脱壳逻辑。

![](img/b214c311bb36ee7dbdaebfc83773a437_36.png)

![](img/b214c311bb36ee7dbdaebfc83773a437_37.png)

![](img/b214c311bb36ee7dbdaebfc83773a437_39.png)

## 总结

![](img/b214c311bb36ee7dbdaebfc83773a437_41.png)

![](img/b214c311bb36ee7dbdaebfc83773a437_43.png)

![](img/b214c311bb36ee7dbdaebfc83773a437_45.png)

本节课我们一起学习了Dalvik虚拟机加载DEX文件的完整流程。我们从 `dvmDexFileOpenFromMem` 入口函数开始，逐步分析了数据校验、解析、结构体转换以及注册到全局表的过程。我们重点了解了 `DexFile`、`DvmDex` 等关键数据结构，并揭示了通过遍历 `gDvm.userDexFiles` 哈希表来实现通用脱壳的核心思路。理解这些底层机制，是进行高级Android逆向与安全分析的重要基础。