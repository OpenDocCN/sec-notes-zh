# i春秋零基础入门Android逆向 - 课时12：IDA脱壳脚本编写 🛠️

![](img/4a477490ce41e63f66bd84d354bb5aed_1.png)

![](img/4a477490ce41e63f66bd84d354bb5aed_3.png)

![](img/4a477490ce41e63f66bd84d354bb5aed_4.png)

![](img/4a477490ce41e63f66bd84d354bb5aed_6.png)

在本节课中，我们将学习如何编写一个IDA脚本，用于从内存中自动提取和修复被加壳保护的DEX文件。我们将基于Android系统加载DEX文件的原理，通过定位关键数据结构来实现通用脱壳。

![](img/4a477490ce41e63f66bd84d354bb5aed_8.png)

![](img/4a477490ce41e63f66bd84d354bb5aed_10.png)

![](img/4a477490ce41e63f66bd84d354bb5aed_12.png)

![](img/4a477490ce41e63f66bd84d354bb5aed_14.png)

![](img/4a477490ce41e63f66bd84d354bb5aed_16.png)

## 概述

![](img/4a477490ce41e63f66bd84d354bb5aed_18.png)

![](img/4a477490ce41e63f66bd84d354bb5aed_19.png)

![](img/4a477490ce41e63f66bd84d354bb5aed_21.png)

![](img/4a477490ce41e63f66bd84d354bb5aed_22.png)

![](img/4a477490ce41e63f66bd84d354bb5aed_24.png)

上一节我们介绍了在Android Dalvik模式下，系统如何加载一个DEX文件，并重点分析了`gDvm.userDexFiles`这个关键数据结构。本节中，我们将看看如何利用这个结构，编写IDA脚本来自动化提取内存中已加载的DEX文件。

![](img/4a477490ce41e63f66bd84d354bb5aed_26.png)

## 核心原理与目标

![](img/4a477490ce41e63f66bd84d354bb5aed_28.png)

无论是什么壳，只要它需要成功加载一个DEX文件，就必须操作`gDvm.userDexFiles`这个哈希表结构体。因此，我们可以通过遍历这个结构体来获取内存中所有被加载过的DEX文件数据。

![](img/4a477490ce41e63f66bd84d354bb5aed_30.png)

我们的脚本编写目标分为两步：
1.  定位`gDvm.userDexFiles`哈希表的位置。
2.  遍历此表，找到所有加载过的DEX文件数据并将其导出。

## 环境准备与目标分析

首先，我们需要一个目标APK进行实战。本次课件的目标APK使用了某种加密加固。为了编写通用脱壳机，我们将直接挂接进程进行分析，而非单步调试。

以下是操作步骤：

1.  安装目标APK并启动。
2.  在挂接IDA调试器之前，先暂停目标进程，防止其检测到调试器而退出。可以使用`adb shell`命令完成：
    ```bash
    adb shell ps | grep <package_name>  # 查找目标进程PID
    adb shell kill -STOP <PID>          # 暂停进程
    ```
3.  使用IDA挂接到已暂停的进程上。

## 定位关键数据结构

挂接成功后，我们需要在内存中找到`gDvm.userDexFiles`。`gDvm`是一个全局的`DvmGlobals`结构体变量。

1.  在IDA中，使用快捷键`G`跳转到`gDvm`的地址。
2.  我们需要知道`userDexFiles`在`gDvm`结构体中的偏移量。这需要分析`libdvm.so`文件。
3.  从手机中提取`/system/lib/libdvm.so`文件，并用另一个IDA实例进行静态分析。
4.  在静态分析的IDA中，同样跳转到`gDvm`，并搜索或通过交叉引用找到`userDexFiles`的引用点，从而计算出其偏移量。
    *   例如，在某个函数中找到了对`gDvm.userDexFiles`的操作，通过计算该地址与`gDvm`基地址的差值，即可得到偏移量（例如`0x330`）。
    *   **注意**：此偏移量因Android系统版本和编译差异而不同，需要针对自己的环境重新确定。

## 解析DEX文件数据

获得`gDvm.userDexFiles`地址后，我们开始解析这个哈希表。

![](img/4a477490ce41e63f66bd84d354bb5aed_32.png)

1.  `userDexFiles`是一个`HashTable`结构，包含`tableSize`、`numEntries`等字段。
2.  其中`pEntries`指向一个`HashEntry`指针数组，每个`HashEntry`的`data`字段指向一个`DexOrJar`结构体。
3.  `DexOrJar`结构体标识了这是DEX文件还是JAR文件，并包含指向实际文件数据的指针（例如`pDvmDex`）。
4.  对于DEX文件，`pDvmDex`指向一个`DvmDex`结构体，该结构体的`pDexFile`成员指向原始的`DexFile`结构，其中就包含了DEX文件头和数据。
5.  有些壳会抹去或加密原始的DEX文件头。此时，可以检查`DvmDex`结构体中的`memMap`字段，它可能指向一块映射内存，其中保存着未修改的原始DEX数据。

![](img/4a477490ce41e63f66bd84d354bb5aed_34.png)

## 编写IDA脱壳脚本

![](img/4a477490ce41e63f66bd84d354bb5aed_36.png)

![](img/4a477490ce41e63f66bd84d354bb5aed_37.png)

![](img/4a477490ce41e63f66bd84d354bb5aed_38.png)

![](img/4a477490ce41e63f66bd84d354bb5aed_39.png)

基于以上原理，我们可以编写IDA Python脚本来自动化以下流程：

![](img/4a477490ce41e63f66bd84d354bb5aed_41.png)

![](img/4a477490ce41e63f66bd84d354bb5aed_43.png)

1.  通过`gDvm`基地址和偏移量找到`userDexFiles`哈希表。
2.  遍历哈希表中的所有有效条目（`HashEntry`）。
3.  对每个条目，解析出`DexOrJar`结构，判断文件类型。
4.  对于DEX文件，定位到`DvmDex`结构，尝试从`pDexFile`或`memMap`中提取DEX数据。
5.  将提取出的DEX数据修复（例如，重建被抹去的文件头）并保存到本地文件。

![](img/4a477490ce41e63f66bd84d354bb5aed_45.png)

![](img/4a477490ce41e63f66bd84d354bb5aed_46.png)

![](img/4a477490ce41e63f66bd84d354bb5aed_47.png)

脚本的核心逻辑伪代码如下：
```python
# 1. 定位 gDvm 和 userDexFiles
gDvm_addr = idaapi.get_name_ea(idaapi.BADADDR, “gDvm”)
userDexFiles_addr = gDvm_addr + OFFSET_USER_DEX_FILES  # OFFSET_USER_DEX_FILES 需要自行确定

![](img/4a477490ce41e63f66bd84d354bb5aed_49.png)

![](img/4a477490ce41e63f66bd84d354bb5aed_50.png)

![](img/4a477490ce41e63f66bd84d354bb5aed_52.png)

![](img/4a477490ce41e63f66bd84d354bb5aed_54.png)

# 2. 读取 HashTable 结构
table_size = read_memory_dword(userDexFiles_addr)
num_entries = read_memory_dword(userDexFiles_addr + 4)
p_entries = read_memory_pointer(userDexFiles_addr + 8)  # 假设偏移，需根据结构定义调整

![](img/4a477490ce41e63f66bd84d354bb5aed_56.png)

![](img/4a477490ce41e63f66bd84d354bb5aed_57.png)

# 3. 遍历条目
for i in range(table_size):
    entry_addr = p_entries + i * ENTRY_SIZE
    hash_value = read_memory_dword(entry_addr)
    data_ptr = read_memory_pointer(entry_addr + 4)  # data 字段
    if data_ptr != 0:
        # 4. 解析 DexOrJar
        dex_or_jar = parse_dex_or_jar(data_ptr)
        if dex_or_jar.is_dex:
            # 5. 提取并修复 DEX 数据
            dex_data = extract_and_fix_dex(dex_or_jar.pDvmDex)
            save_to_file(dex_data, “dex_%d.dex” % i)
```

![](img/4a477490ce41e63f66bd84d354bb5aed_59.png)

![](img/4a477490ce41e63f66bd84d354bb5aed_61.png)

## 脚本使用与脱壳实战

![](img/4a477490ce41e63f66bd84d354bb5aed_63.png)

![](img/4a477490ce41e63f66bd84d354bb5aed_65.png)

![](img/4a477490ce41e63f66bd84d354bb5aed_67.png)

运行编写好的脚本，它会列出内存中所有加载的DEX/JAR模块。选择目标DEX模块后，脚本会自动提取并修复数据，保存为`.dex`文件。

1.  使用类似JEB、jadx等工具打开脱壳后的DEX文件，验证其完整性。
2.  如果脱壳后的DEX文件在反编译时报告错误（如方法数不对、类找不到），可能还需要进一步的修复。
3.  对于应用程序，还需要修复`AndroidManifest.xml`中的Application类入口。因为加壳程序可能会替换原始的Application。在脱壳后的代码中搜索`android.app.Application`的子类，找到原始的Application类名，并更新到清单文件中。

![](img/4a477490ce41e63f66bd84d354bb5aed_69.png)

![](img/4a477490ce41e63f66bd84d354bb5aed_71.png)

## 总结

![](img/4a477490ce41e63f66bd84d354bb5aed_73.png)

![](img/4a477490ce41e63f66bd84d354bb5aed_75.png)

本节课中我们一起学习了基于`gDvm.userDexFiles`数据结构编写IDA脱壳脚本的方法。我们了解了从定位关键结构、遍历内存中的DEX数据到最终提取和修复文件的完整流程。这种方法具有通用性，但需要注意不同系统版本下的结构偏移差异，以及针对不同壳的修复策略。通过本课的学习，你可以尝试使用或修改脚本，实践对其他壳的脱壳分析。