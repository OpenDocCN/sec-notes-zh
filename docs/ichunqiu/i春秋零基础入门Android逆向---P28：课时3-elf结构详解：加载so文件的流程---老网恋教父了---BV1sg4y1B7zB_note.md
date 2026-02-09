# i春秋零基础入门Android逆向 - P28：课时3 ELF结构详解：加载SO文件的流程 🧩

![](img/b1eef28fe2ab143112f43965d13f5b65_1.png)

![](img/b1eef28fe2ab143112f43965d13f5b65_2.png)

![](img/b1eef28fe2ab143112f43965d13f5b65_4.png)

![](img/b1eef28fe2ab143112f43965d13f5b65_5.png)

在本节课中，我们将深入分析Android系统如何将一个存储在磁盘上的ELF文件（特别是SO库文件）加载到内存中。我们将通过阅读Android源码中的`linker`程序，来理解整个加载、链接和初始化的流程。

![](img/b1eef28fe2ab143112f43965d13f5b65_7.png)

上一节我们介绍了ELF文件在存储中的两种视图（链接视图和执行视图）。本节中我们来看看Android系统如何具体执行加载操作。

![](img/b1eef28fe2ab143112f43965d13f5b65_9.png)

![](img/b1eef28fe2ab143112f43965d13f5b65_10.png)

![](img/b1eef28fe2ab143112f43965d13f5b65_12.png)

![](img/b1eef28fe2ab143112f43965d13f5b65_14.png)

## 加载器：Linker程序

![](img/b1eef28fe2ab143112f43965d13f5b65_16.png)

Android系统通过一个名为`linker`的程序来完成ELF文件的加载和链接操作。该程序的源代码位于Android源码的`bionic/linker`目录下。

`linker`程序的入口点通常由汇编语言编写。以x86版本为例，其入口函数`_start`会调用一系列初始化函数。首先会调用`__linker_init`函数，该函数负责对`linker`自身进行重定位等初始化操作，确保其能正常工作后，才能加载其他SO文件。

我们更关心的是`linker`如何加载外部SO文件。在编程中，我们通过`dlopen`函数来动态加载SO库。在`linker`内部，与之对应的核心函数是`do_dlopen`。

## 核心加载函数：do_dlopen

我们进入`do_dlopen`函数进行分析。该函数接收两个主要参数：SO文件的路径和打开标志位。

![](img/b1eef28fe2ab143112f43965d13f5b65_18.png)

![](img/b1eef28fe2ab143112f43965d13f5b65_19.png)

![](img/b1eef28fe2ab143112f43965d13f5b65_21.png)

以下是`do_dlopen`函数的关键步骤：
1.  **检查标志位**：首先判断传入的标志位是否合法，常见的标志如`RTLD_NOW`表示立即加载并完成重定位。
2.  **操作全局SO信息表**：`linker`维护了一个全局的SO信息表。在加载前，会暂时将该表的访问属性改为可读写，加载完成后再改回只读，以保证线程安全。
3.  **关键调用**：随后，函数会依次调用两个核心函数：
    *   `find_library`: 查找或加载SO库。
    *   `call_constructors`: 调用SO库中的初始化构造函数。

![](img/b1eef28fe2ab143112f43965d13f5b65_23.png)

![](img/b1eef28fe2ab143112f43965d13f5b65_25.png)

## 查找与加载：find_library 函数

`find_library`函数负责查找SO是否已加载，若未加载则从文件系统读取。

![](img/b1eef28fe2ab143112f43965d13f5b65_27.png)

以下是其执行流程：
1.  **查找现有库**：首先根据SO名称，在已加载的SO列表中查找。如果找到，则增加其引用计数并直接返回，避免重复加载。
2.  **加载新库**：如果未找到，则调用`load_library`函数执行真正的加载操作。

![](img/b1eef28fe2ab143112f43965d13f5b65_29.png)

`load_library`函数是加载过程的核心：
*   **打开文件**：获取SO文件的文件描述符（FD）。
*   **读取ELF头**：使用`ElfReader`类读取并验证ELF文件头，确保是合法的ELF格式。
*   **读取程序头表**：根据ELF头中的信息，读取`Program Header Table`到内存中。
*   **加载可加载段**：遍历`Program Header Table`，寻找类型为`PT_LOAD`的段（即可加载段）。通常一个SO文件中只有两个`PT_LOAD`段，它们包含了代码、数据等所有需要加载到内存的内容。然后通过`mmap`等系统调用，将这些段映射到进程的虚拟地址空间。

**重要提示**：在加载过程中，`linker`**只使用了`Program Header Table`（执行视图），而完全忽略了`Section Header Table`（链接视图）**。这就是为什么上节课说链接视图在加载时不起作用。一些加固软件会故意修改或混淆`Section Header Table`来增加分析难度。

## 链接与重定位：soinfo_link_image 函数

在`find_library`的后续流程中，会调用`soinfo_link_image`函数。此函数负责处理动态链接的关键步骤：
1.  **定位动态节区**：从`Program Header Table`中找到类型为`PT_DYNAMIC`的段，它指向`.dynamic`节区。这个节区包含了动态链接所需的所有关键信息。
2.  **解析动态信息**：`.dynamic`节区由一系列`ElfW(Dyn)`结构体组成，每个结构体包含一个标签（`d_tag`）和一个值（`d_un`）。通过这些标签，`linker`可以获取到：
    *   字符串表地址（`.dynstr`）
    *   符号表地址（`.dynsym`）
    *   重定位表地址（`.rel.dyn`, `.rel.plt`等）
    *   初始化函数地址（`.init`, `.init_array`）
    *   依赖的SO库列表（`.needed`）
3.  **加载依赖库**：根据解析出的依赖列表，递归地调用`find_library`来加载所有依赖的SO库。
4.  **执行重定位**：这是链接的核心。`linker`根据重定位表（如`.rel.plt`用于函数延迟绑定，`.rel.dyn`用于数据引用）中的条目，计算符号的真实地址，并修改代码或数据段中相应的引用位置。例如，将`printf`函数的调用地址修改为libc库中的实际地址。

## 初始化：call_constructors 函数

SO文件被加载和链接后，还需要执行一些初始化代码才能正常工作。这是由`call_constructors`函数完成的。

![](img/b1eef28fe2ab143112f43965d13f5b65_31.png)

以下是初始化函数的调用顺序：
1.  **调用依赖库的构造函数**：首先，确保所有依赖的SO库都已调用它们自己的构造函数。
2.  **调用自身的构造函数**：然后，调用本SO库的构造函数。主要包括：
    *   `.init`节区中的代码。
    *   `.init_array`数组中的函数指针列表。

![](img/b1eef28fe2ab143112f43965d13f5b65_33.png)

这些初始化函数通常用于设置全局变量、初始化运行时环境等。在一些加固的SO中，解密代码可能就放在这些初始化函数里。

## 总结

本节课我们一起学习了Android系统中ELF（SO）文件的完整加载流程。我们通过分析`linker`源码，了解了从`dlopen`调用开始，到SO文件最终可用的全过程：

![](img/b1eef28fe2ab143112f43965d13f5b65_35.png)

1.  **加载**：通过`find_library`和`load_library`，将SO文件的`PT_LOAD`段映射到内存。
2.  **链接**：在`soinfo_link_image`函数中，解析动态节区，加载依赖库，并完成复杂的符号查找与重定位工作。
3.  **初始化**：在`call_constructors`函数中，按顺序调用依赖库和自身的构造函数，完成最后的准备工作。

![](img/b1eef28fe2ab143112f43965d13f5b65_37.png)

![](img/b1eef28fe2ab143112f43965d13f5b65_39.png)

![](img/b1eef28fe2ab143112f43965d13f5b65_40.png)

整个过程的核心是**使用`Program Header Table`定位和加载内容，利用`.dynamic`节区进行动态链接**，而`Section Header Table`在加载阶段并不参与。理解这个流程，对于后续分析SO加固、进行动态调试和逆向工程至关重要。