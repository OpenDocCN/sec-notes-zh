#  025：课时4 Android 动态代码自修改实现2 🛠️

![](img/5b940375f5810110fc6f34272c6ad053_1.png)

![](img/5b940375f5810110fc6f34272c6ad053_2.png)

在本节课中，我们将学习另一种实现Android动态代码自修改的方法。我们将通过从内存中定位并解析DEX文件，直接修改其指令数据，来实现运行时修改代码逻辑的目标。

![](img/5b940375f5810110fc6f34272c6ad053_4.png)

上一节我们介绍了通过JNI层反射结构体来定位和修改DVM代码的方法。本节中，我们将采用更底层的方式，直接从内存中操作DEX文件。

![](img/5b940375f5810110fc6f34272c6ad053_6.png)

## 概述与原理

![](img/5b940375f5810110fc6f34272c6ad053_8.png)

![](img/5b940375f5810110fc6f34272c6ad053_10.png)

程序加载时，系统会解析DEX文件，最终生成`Method`结构体，其中的`insns`指针指向内存中的指令数据。我们的目标就是定位到这个内存区域并进行修改。

![](img/5b940375f5810110fc6f34272c6ad053_12.png)

![](img/5b940375f5810110fc6f34272c6ad053_14.png)

![](img/5b940375f5810110fc6f34272c6ad053_16.png)

![](img/5b940375f5810110fc6f34272c6ad053_17.png)

核心思路如下：
1.  从进程内存映射中定位到DEX文件。
2.  解析内存中的DEX文件结构。
3.  找到目标类和方法对应的`insns`指针。
4.  修改该指针指向的内存内容。

![](img/5b940375f5810110fc6f34272c6ad053_19.png)

## 代码实现步骤

![](img/5b940375f5810110fc6f34272c6ad053_21.png)

![](img/5b940375f5810110fc6f34272c6ad053_23.png)

以下是实现动态代码修改的具体步骤。

![](img/5b940375f5810110fc6f34272c6ad053_25.png)

### 1. 项目准备与JNI层调用

![](img/5b940375f5810110fc6f34272c6ad053_27.png)

![](img/5b940375f5810110fc6f34272c6ad053_28.png)

首先，在Android Studio中打开项目，并在Java层添加一个用于触发修改的Native方法。

```java
// Java层代码示例
public native void patchMethod2();
```

![](img/5b940375f5810110fc6f34272c6ad053_30.png)

然后在JNI层（C/C++代码）注册并实现这个方法。

![](img/5b940375f5810110fc6f34272c6ad053_32.png)

```c
// JNI层方法注册
JNINativeMethod methods[] = {
    {"patchMethod2", "()V", (void *)patchMethod2}
};
```

![](img/5b940375f5810110fc6f34272c6ad053_34.png)

![](img/5b940375f5810110fc6f34272c6ad053_36.png)

### 2. 定位内存中的DEX模块

![](img/5b940375f5810110fc6f34272c6ad053_37.png)

我们需要获取自身进程的内存映射信息，以找到DEX文件所在的模块基址。这里使用一个遍历`/proc/self/maps`文件的函数`get_module_base`。

![](img/5b940375f5810110fc6f34272c6ad053_39.png)

![](img/5b940375f5810110fc6f34272c6ad053_40.png)

![](img/5b940375f5810110fc6f34272c6ad053_41.png)

![](img/5b940375f5810110fc6f34272c6ad053_42.png)

![](img/5b940375f5810110fc6f34272c6ad053_43.png)

```c
void* get_module_base(pid_t pid, const char* module_name) {
    FILE *fp;
    long addr = 0;
    char *pch;
    char filename[32];
    char line[1024];

    if (pid < 0) {
        // 获取自身进程ID
        snprintf(filename, sizeof(filename), "/proc/self/maps");
    } else {
        snprintf(filename, sizeof(filename), "/proc/%d/maps", pid);
    }

    fp = fopen(filename, "r");
    if (fp != NULL) {
        while (fgets(line, sizeof(line), fp)) {
            if (strstr(line, module_name)) {
                pch = strtok(line, "-");
                addr = strtoul(pch, NULL, 16);
                break;
            }
        }
        fclose(fp);
    }
    return (void *)addr;
}
```

调用该函数，传入模块名（如`/data/app/.../base.apk`）即可获得DEX文件在内存中的起始地址。

![](img/5b940375f5810110fc6f34272c6ad053_45.png)

![](img/5b940375f5810110fc6f34272c6ad053_46.png)

![](img/5b940375f5810110fc6f34272c6ad053_48.png)

### 3. 解析内存中的DEX文件

![](img/5b940375f5810110fc6f34272c6ad053_49.png)

![](img/5b940375f5810110fc6f34272c6ad053_51.png)

![](img/5b940375f5810110fc6f34272c6ad053_53.png)

![](img/5b940375f5810110fc6f34272c6ad053_54.png)

获得内存地址后，我们需要将其作为DEX文件进行解析。Android系统提供的`libdex`库（通常通过`dvmDexFileOpenFromMem`等函数）可以完成此工作，但这里我们使用更直接的`dexFileParse`函数（来自`libdex`或类似实现）来解析内存数据。

![](img/5b940375f5810110fc6f34272c6ad053_56.png)

```c
// 假设 dexData 是获取到的内存起始指针，dataLen 是长度
DexFile* pDexFile = dexFileParse((u1*)dexData, dataLen, kDexParseDefault);
if (pDexFile == NULL) {
    LOGE("Failed to parse dex file.");
    return;
}
```

`DexFile`结构体包含了DEX文件的完整解析信息。

### 4. 查找目标类与方法

解析出`DexFile`后，我们需要遍历其中的类定义，找到我们想要修改的特定类和方法。

```c
// 遍历Dex文件中的类
for (u4 i = 0; i < pDexFile->pHeader->classDefsSize; ++i) {
    const DexClassDef* pClassDef = dexGetClassDef(pDexFile, i);
    const char* className = dexGetClassDescriptor(pDexFile, pClassDef);

    // 比较类名，例如：Lcom/example/myapp/MainActivity;
    if (strcmp(className, "Lcom/example/myapp/MainActivity;") == 0) {
        // 找到目标类
        // 接下来解析该类数据，查找方法
        const u1* pClassData = dexGetClassData(pDexFile, pClassDef);
        DexClassData* pClassDataDecoded = dexReadAndVerifyClassData(&pClassData, NULL);

        // 遍历类中的方法
        for (u4 j = 0; j < pClassDataDecoded->header.directMethodsSize + pClassDataDecoded->header.virtualMethodsSize; ++j) {
            DexMethod* pDexMethod;
            if (j < pClassDataDecoded->header.directMethodsSize) {
                pDexMethod = &pClassDataDecoded->directMethods[j];
            } else {
                pDexMethod = &pClassDataDecoded->virtualMethods[j - pClassDataDecoded->header.directMethodsSize];
            }

            const char* methodName = dexStringById(pDexFile, pDexMethod->methodIdx);
            // 比较方法名，例如：return1
            if (strcmp(methodName, "return1") == 0) {
                // 找到目标方法
                // 获取方法的代码项
                const DexCode* pDexCode = dexGetCode(pDexFile, pDexMethod);
                if (pDexCode != NULL) {
                    // pDexCode->insns 就是指向指令码的指针
                    targetInsnsPtr = (u4*)pDexCode->insns;
                    break;
                }
            }
        }
        break;
    }
}
```

![](img/5b940375f5810110fc6f34272c6ad053_58.png)

### 5. 修改指令内存

![](img/5b940375f5810110fc6f34272c6ad053_60.png)

找到目标指令指针`targetInsnsPtr`后，由于该内存区域可能是只读的，我们需要先修改其页面属性，然后写入新指令，最后恢复属性。

![](img/5b940375f5810110fc6f34272c6ad053_62.png)

![](img/5b940375f5810110fc6f34272c6ad053_63.png)

![](img/5b940375f5810110fc6f34272c6ad053_65.png)

![](img/5b940375f5810110fc6f34272c6ad053_67.png)

```c
#include <sys/mman.h>

![](img/5b940375f5810110fc6f34272c6ad053_69.png)

![](img/5b940375f5810110fc6f34272c6ad053_71.png)

// 计算内存页大小
long page_size = sysconf(_SC_PAGESIZE);
// 计算指令指针所在页的起始地址（按页对齐）
uintptr_t page_start = (uintptr_t)targetInsnsPtr & ~(page_size - 1);

![](img/5b940375f5810110fc6f34272c6ad053_73.png)

// 修改内存页属性为可读可写可执行
if (mprotect((void*)page_start, page_size, PROT_READ | PROT_WRITE | PROT_EXEC) == 0) {
    // 修改成功，写入新指令
    // 例如，将 return 1 (指令码 0x0F01) 改为 return 2 (指令码 0x0F02)
    // 注意字节序，Android通常为小端
    *targetInsnsPtr = 0x0F02; // 假设这是 return 2 的指令码

    // 恢复内存页属性（通常恢复为可读可执行）
    mprotect((void*)page_start, page_size, PROT_READ | PROT_EXEC);
} else {
    LOGE("Failed to change memory protection.");
}
```

### 6. 清理资源

![](img/5b940375f5810110fc6f34272c6ad053_75.png)

修改完成后，需要释放解析DEX文件时分配的资源。

```c
if (pDexFile != NULL) {
    dexFileFree(pDexFile);
}
```

![](img/5b940375f5810110fc6f34272c6ad053_77.png)

![](img/5b940375f5810110fc6f34272c6ad053_79.png)

## 核心概念与公式

![](img/5b940375f5810110fc6f34272c6ad053_81.png)

*   **DEX文件内存映射**：进程通过`mmap`将DEX文件加载到内存，其属性可在`/proc/self/maps`中查看。
*   **指令修改**：本质是修改`DexCode.insns`指针所指向的原始`uint16_t`数组。
*   **内存属性修改**：使用`mprotect`函数修改指定内存区域的访问权限，其原型为：
    `int mprotect(void *addr, size_t len, int prot);`
*   **小端字节序**：Android系统使用小端字节序，多字节数据在内存中低位在前。

![](img/5b940375f5810110fc6f34272c6ad053_83.png)

![](img/5b940375f5810110fc6f34272c6ad053_84.png)

![](img/5b940375f5810110fc6f34272c6ad053_85.png)

## 注意事项与扩展

![](img/5b940375f5810110fc6f34272c6ad053_87.png)

![](img/5b940375f5810110fc6f34272c6ad053_88.png)

![](img/5b940375f5810110fc6f34272c6ad053_90.png)

![](img/5b940375f5810110fc6f34272c6ad053_92.png)

1.  **兼容性**：不同Android版本或厂商定制ROM的`libdex`内部结构可能不同，直接解析需注意适配。
2.  **加固对抗**：许多App加固方案会加密DEX文件，在运行时解密。本方法演示的是一种基础思路，实际加固可能更复杂（如方法级解密、解释执行等）。
3.  **实践建议**：要深入理解DEX格式，建议手动实现一个简单的DEX文件解析器，这有助于掌握其结构细节。
4.  **扩展应用**：此技术不仅可用于代码修改，也是理解Android加壳、脱壳原理的基础。

![](img/5b940375f5810110fc6f34272c6ad053_94.png)

## 总结

![](img/5b940375f5810110fc6f34272c6ad053_96.png)

![](img/5b940375f5810110fc6f34272c6ad053_97.png)

![](img/5b940375f5810110fc6f34272c6ad053_98.png)

![](img/5b940375f5810110fc6f34272c6ad053_99.png)

![](img/5b940375f5810110fc6f34272c6ad053_101.png)

本节课我们一起学习了第二种Android动态代码自修改的实现方法。我们掌握了从进程内存映射中定位DEX文件、解析DEX结构、查找特定方法指令、以及通过`mprotect`修改内存属性并覆写指令的完整流程。这种方法比反射更底层，是理解Android运行时和加固技术的重要实践。