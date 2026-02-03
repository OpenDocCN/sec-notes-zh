![](img/b287e42c238d9031ad8f2d08cfce502d_1.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_2.png)

# i春秋零基础入门Android逆向 - P22：课时1 class.dex文件格式讲解 📚

![](img/b287e42c238d9031ad8f2d08cfce502d_4.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_6.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_7.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_9.png)

在本节课中，我们将要学习Android应用的核心文件——`classes.dex`的格式结构。理解DEX文件的格式是进行Android逆向分析、脱壳和修改的基础。

![](img/b287e42c238d9031ad8f2d08cfce502d_11.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_13.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_14.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_15.png)

## 概述

![](img/b287e42c238d9031ad8f2d08cfce502d_16.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_18.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_19.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_21.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_23.png)

对于一个APK文件来说，`classes.dex`文件是最重要的。它存放着整个程序运行所需的字节码。Android系统在运行程序时，会优先加载这个DEX文件，解析并执行其中的代码。我们进行逆向修改，本质上就是对这个DEX文件进行修改的过程。

![](img/b287e42c238d9031ad8f2d08cfce502d_25.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_27.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_29.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_30.png)

因此，了解DEX文件的格式至关重要。

![](img/b287e42c238d9031ad8f2d08cfce502d_32.png)

## DEX文件与Smali字节码

![](img/b287e42c238d9031ad8f2d08cfce502d_34.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_36.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_38.png)

DEX文件存放的是Dalvik字节码。我们可以使用`apktool`工具的`baksmali`命令，将这些字节码反编译成可读的`smali`代码。

![](img/b287e42c238d9031ad8f2d08cfce502d_40.png)

以下是`apktool`的基本功能说明：
*   **`baksmali`**: 用于将DEX文件反编译为`smali`代码。
*   **`smali`**: 用于将`smali`代码汇编回DEX文件。

![](img/b287e42c238d9031ad8f2d08cfce502d_42.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_43.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_44.png)

这两个功能被集成在`apktool`中。其基本用法如下：

![](img/b287e42c238d9031ad8f2d08cfce502d_46.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_47.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_48.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_50.png)

**反编译DEX文件：**
```bash
apktool baksmali <dex文件路径> -o <输出smali文件夹路径>
```
该命令会输出一个包含`smali`代码的文件夹。

![](img/b287e42c238d9031ad8f2d08cfce502d_52.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_54.png)

**汇编Smali代码：**
```bash
apktool smali <smali文件夹路径> -o <输出dex文件路径>
```
该命令会将`smali`代码文件夹重新汇编成一个DEX文件。

![](img/b287e42c238d9031ad8f2d08cfce502d_56.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_58.png)

经过反编译再汇编回来的DEX文件，其大小可能与原文件不同（例如，某些加固壳会对DEX数据进行抽取和动态还原），但其功能是等价的。

![](img/b287e42c238d9031ad8f2d08cfce502d_60.png)

## 使用010 Editor解析DEX结构

![](img/b287e42c238d9031ad8f2d08cfce502d_62.png)

为了深入学习DEX格式，我们需要借助更专业的工具。在Android逆向中，`010 Editor`是一款非常常见且强大的工具，它支持通过模板来解析各种文件格式。

![](img/b287e42c238d9031ad8f2d08cfce502d_64.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_66.png)

我们可以使用其DEX文件模板来直观地解析DEX结构。运行模板后，`010 Editor`会将DEX文件的各个部分（如字符串池、类型定义、方法定义等）清晰地展示出来，这对于分析冗余数据、合法/非法代码以及定位关键信息（如字符串存放位置）非常有帮助。

![](img/b287e42c238d9031ad8f2d08cfce502d_68.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_70.png)

此外，Android系统解析DEX文件的相关源代码位于AOSP的`/dalvik/libdex/`目录下，可供深入学习参考。

![](img/b287e42c238d9031ad8f2d08cfce502d_72.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_73.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_74.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_76.png)

## DEX文件格式详解

Android系统将DEX文件加载到内存后，会将其映射为一个`DexFile`结构体。这个结构体内部包含多个指针，指向文件的不同部分。

![](img/b287e42c238d9031ad8f2d08cfce502d_78.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_79.png)

### DEX文件头 (DexHeader)

![](img/b287e42c238d9031ad8f2d08cfce502d_81.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_83.png)

首先是指向`DexHeader`的指针，它对应DEX文件的开头部分。`DexHeader`结构包含了描述整个文件的关键信息。

![](img/b287e42c238d9031ad8f2d08cfce502d_85.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_87.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_88.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_90.png)

我们通过一张图来了解其核心字段：

![](img/b287e42c238d9031ad8f2d08cfce502d_60.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_92.png)

以下是各字段的详细说明：

*   **`magic[8]`**: 魔数，用于标识这是一个合法的DEX文件，固定为`dex\n035\0`。
*   **`checksum`**: 校验和，用于验证文件数据的完整性。
*   **`signature`**: SHA-1哈希值，同样用于验证文件。
*   **`fileSize`**: 整个DEX文件的大小。
*   **`headerSize`**: `DexHeader`本身的大小，通常为0x70字节，它指向下一个节区的起始位置。
*   **`endianTag`**: 字节序标识，表明文件是大端序还是小端序。
*   **`linkSize` & `linkOff`**: 链接数据段的大小和偏移。
*   **其他`xxxSize` & `xxxOff`**: 这些字段定义了后续各个数据节（如字符串、类型、方法原型等）的大小和偏移地址。系统根据这些信息将对应节区的数据映射到内存中。

### 字符串标识符节 (stringIds)

`stringIds`节区存放了字符串的索引信息。在`DexHeader`中由`stringIdsSize`和`stringIdsOff`指明其位置和大小。

`stringIds`本质上是一个`DexStringId`结构体数组。每个`DexStringId`结构非常简单，只包含一个`stringDataOff`字段，这是一个4字节的偏移量，指向实际字符串数据的存储位置。

实际字符串数据以`ULEB128`变长整数格式存储其长度，后接字符串内容，通常以`\0`结尾。

**ULEB128**是一种变长整数编码，用于节省空间。它使用每个字节的低7位存储数据，最高位作为标志位：如果为1，表示下一个字节仍然属于当前整数；如果为0，则表示整数结束。这样，一个整数可以根据其值的大小占用1到5个字节。

### 类型标识符节 (typeIds)

`typeIds`节区存放了类型的索引信息。它也是一个数组，每个元素是一个`DexTypeId`结构体。

![](img/b287e42c238d9031ad8f2d08cfce502d_94.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_95.png)

每个`DexTypeId`只包含一个`descriptorIdx`字段。这个字段是一个索引值，指向`stringIds`节区中的某个字符串，该字符串就是类型的描述符（例如`“I”`表示整型，`“Ljava/lang/String;”`表示字符串类）。

### 方法原型标识符节 (protoIds)

`protoIds`节区描述了方法的原型（签名），包括返回类型和参数列表。每个`DexProtoId`包含以下字段：
*   `shortyIdx`: 指向`stringIds`，是方法签名的简写。
*   `returnTypeIdx`: 指向`typeIds`，描述返回类型。
*   `parametersOff`: 偏移量，指向一个`DexTypeList`，该列表详细列出了所有参数的类型。

![](img/b287e42c238d9031ad8f2d08cfce502d_97.png)

### 字段标识符节 (fieldIds)

![](img/b287e42c238d9031ad8f2d08cfce502d_99.png)

`fieldIds`节区描述了类中的字段（成员变量）。每个`DexFieldId`包含：
*   `classIdx`: 指向`typeIds`，表示该字段所属的类。
*   `typeIdx`: 指向`typeIds`，表示该字段的类型。
*   `nameIdx`: 指向`stringIds`，表示该字段的名称。

![](img/b287e42c238d9031ad8f2d08cfce502d_101.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_103.png)

### 方法标识符节 (methodIds)

`methodIds`节区描述了类中的方法。每个`DexMethodId`包含：
*   `classIdx`: 指向`typeIds`，表示该方法所属的类。
*   `protoIdx`: 指向`protoIds`，描述该方法的原型（返回类型和参数）。
*   `nameIdx`: 指向`stringIds`，表示该方法的名称。

### 类定义节 (classDefs)

`classDefs`节区存放所有类的定义信息。每个`DexClassDef`结构体定义了一个类的完整信息：
*   `classIdx`: 该类的类型索引。
*   `accessFlags`: 类的访问标志（如public, final）。
*   `superclassIdx`: 父类的类型索引。
*   `interfacesOff`: 实现的接口列表偏移。
*   `sourceFileIdx`: 源文件名索引。
*   `annotationsOff`: 注解数据偏移。
*   `classDataOff`: **指向类数据**的偏移，这是类的核心。

`classDataOff`指向一个`DexClassData`结构，它包含了类的具体内容：
*   静态字段的数量和列表
*   实例字段的数量和列表
*   直接方法的数量和列表
*   虚方法的数量和列表

每个方法项中又包含了方法的索引、访问标志以及最重要的**代码偏移**。该偏移指向`DexCode`结构，其中包含了方法使用的寄存器数量、指令集大小、指令内容以及可能涉及的try-catch块信息。

## 总结

![](img/b287e42c238d9031ad8f2d08cfce502d_105.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_107.png)

![](img/b287e42c238d9031ad8f2d08cfce502d_109.png)

本节课我们一起学习了`classes.dex`文件的基本格式。整个DEX文件通过`DexHeader`头部分引导，串联起`stringIds`（字符串）、`typeIds`（类型）、`protoIds`（方法原型）、`fieldIds`（字段）、`methodIds`（方法）和`classDefs`（类定义）等多个数据节。每个节区通过索引或偏移相互关联，形成一套高效、紧凑的字节码存储结构。Android系统在加载时，正是根据这套结构将DEX文件还原为可执行的类和方法。理解这一结构是后续进行脱壳、代码分析和修改的坚实基础。