# i春秋零基础入门Android逆向 - 课时13：Odex修复方法 🔧

![](img/bd320506450f7ed91192c7df69bd661f_1.png)

![](img/bd320506450f7ed91192c7df69bd661f_2.png)

![](img/bd320506450f7ed91192c7df69bd661f_4.png)

在本节课中，我们将学习Android系统如何将DEX文件优化为ODEX文件，并探讨如何修复因优化导致的脱壳问题。课程将从源码层面分析优化过程，并讲解如何通过修改系统设置来获取原始的、未经优化的DEX数据。

![](img/bd320506450f7ed91192c7df69bd661f_6.png)

![](img/bd320506450f7ed91192c7df69bd661f_7.png)

---

## 脚本脱壳方法的补充与扩展 🔍

![](img/bd320506450f7ed91192c7df69bd661f_9.png)

上一节我们介绍了使用脚本进行脱壳的方法。本节中，我们来看看如何将该方法应用于更复杂的壳，例如360加固，并指出脚本的局限性。

![](img/bd320506450f7ed91192c7df69bd661f_11.png)

以下是使用脚本脱360加固壳的尝试过程：

![](img/bd320506450f7ed91192c7df69bd661f_13.png)

![](img/bd320506450f7ed91192c7df69bd661f_14.png)

![](img/bd320506450f7ed91192c7df69bd661f_15.png)

1.  按照上节课的方法，在程序运行后挂接IDA进行脱壳。
2.  运行脚本后，发现脚本无法找到某些类的原始代码数据，提示 `is past` 并跳过对这些类的提取。
3.  分析导出的DEX文件，发现许多类的 `class data` 数据为0，表明其真实数据已被清空。
4.  尝试用IDA打开导出的DEX文件，会提示为无效的DEX文件。通过先反编译再重新编译可以修复文件结构错误，但函数内容仍然是空的或异常的。

![](img/bd320506450f7ed91192c7df69bd661f_17.png)

面对这种情况，与手工脱壳一样，使用脚本也需要找到正确的脱壳时机点。

![](img/bd320506450f7ed91192c7df69bd661f_19.png)

我们通过手动调试的方式来定位这个时机点：

![](img/bd320506450f7ed91192c7df69bd661f_20.png)

![](img/bd320506450f7ed91192c7df69bd661f_22.png)

1.  使用 `am start -D -n` 选项以调试模式启动应用，并用IDA挂接。
2.  在程序运行初期（如 `JNI_OnLoad` 处），运行脚本查看当前只加载了3个DEX文件。
3.  单步执行，直到经过某个关键函数（该函数负责解密并加载真实的DEX）后，再次运行脚本。
4.  此时发现内存中加载了新的DEX文件。在这个时机点（即刚加载完真实DEX时）下断点并运行脱壳脚本，即可成功导出未被清空数据的原始DEX文件。

![](img/bd320506450f7ed91192c7df69bd661f_24.png)

![](img/bd320506450f7ed91192c7df69bd661f_25.png)

![](img/bd320506450f7ed91192c7df69bd661f_27.png)

![](img/bd320506450f7ed91192c7df69bd661f_28.png)

这个补充说明强调了脚本脱壳同样依赖于准确的脱壳点。

![](img/bd320506450f7ed91192c7df69bd661f_30.png)

![](img/bd320506450f7ed91192c7df69bd661f_31.png)

![](img/bd320506450f7ed91192c7df69bd661f_33.png)

![](img/bd320506450f7ed91192c7df69bd661f_34.png)

---

![](img/bd320506450f7ed91192c7df69bd661f_36.png)

![](img/bd320506450f7ed91192c7df69bd661f_38.png)

![](img/bd320506450f7ed91192c7df69bd661f_40.png)

![](img/bd320506450f7ed91192c7df69bd661f_41.png)

## DEX文件优化机制分析 📖

![](img/bd320506450f7ed91192c7df69bd661f_43.png)

上一节我们成功脱壳，得到了两份DEX文件：一份是加载前的原始数据，另一份是加载后的内存数据。理论上它们应该相同，但实际上存在差异。这是因为Android系统在加载DEX时会对其进行优化和校验。

优化和校验的目的：
*   **提高运行速度**：将部分指令替换为更高效的指令。
*   **减少崩溃概率**：预先检测代码中的潜在问题。

然而，经过优化的DEX文件无法直接用于逆向分析，因为其代码逻辑已被改变，反编译后无法得到原始代码，运行也会异常。

我们通过对比优化前后的DEX反编译的Smali代码来观察差异：

**优化前的Smali代码示例：**
```smali
invoke-direct {v0}, Ljava/lang/Object;-><init>()V
return v1
```

**优化后的Smali代码示例：**
```smali
invoke-direct/range {v0 .. v5}, Ljava/lang/Object;-><init>()V
return-object-barrier v1
```

![](img/bd320506450f7ed91192c7df69bd661f_45.png)

![](img/bd320506450f7ed91192c7df69bd661f_46.png)

可以看到，`invoke-direct` 被优化为 `invoke-direct/range`，`return` 被优化为 `return-object-barrier`。这些优化指令在反编译回Java代码时会导致逻辑错误。

![](img/bd320506450f7ed91192c7df69bd661f_48.png)

![](img/bd320506450f7ed91192c7df69bd661f_50.png)

---

## 从源码理解优化与校验过程 🛠️

接下来，我们从Android源码（以 `dexopt` 相关代码为例）分析优化和校验的具体实现。

![](img/bd320506450f7ed91192c7df69bd661f_52.png)

![](img/bd320506450f7ed91192c7df69bd661f_54.png)

### 代码优化过程

优化的入口函数是 `OptimizeClass`。系统在加载类时会调用此方法。其核心流程如下：

1.  获取类中每个方法的指令起始地址和长度。
2.  遍历每条指令，读取其操作码（Opcode）。
3.  根据Opcode类型判断是否可以进行优化（例如，将 `iget`、`iput` 系列指令优化为 `iget-quick`、`iput-quick`）。
4.  在 `RewriteInsns` 函数中，通过一个大的 `switch-case` 结构，将可优化的Opcode替换为对应的优化后Opcode。

**核心代码逻辑示意：**
```cpp
// 伪代码，示意优化过程
for (each instruction in method) {
    uint8_t opcode = *instruction_ptr;
    switch (opcode) {
        case OP_IGET:
            *instruction_ptr = OP_IGET_QUICK; // 替换为优化指令
            break;
        case OP_INVOKE_DIRECT:
            *instruction_ptr = OP_INVOKE_DIRECT_RANGE;
            break;
        // ... 其他可优化指令
    }
}
```

### 代码校验过程

![](img/bd320506450f7ed91192c7df69bd661f_56.png)

校验的入口函数是 `VerifyClass`。其流程与优化类似，但目的是检查指令的合法性：

![](img/bd320506450f7ed91192c7df69bd661f_57.png)

![](img/bd320506450f7ed91192c7df69bd661f_59.png)

1.  同样遍历方法的每条指令及其Opcode。
2.  检查指令格式、寄存器使用等是否符合规范。
3.  如果校验失败（例如，使用了非法寄存器），函数返回 `false`。
4.  系统可能会因此拒绝加载该类，甚至将出错的指令替换为抛出异常的指令（如 `throw-verification-error`），这会导致脱壳得到的代码出现无意义的异常抛出。

---

## 如何禁用优化以获取原始DEX 🔧

对于逆向分析，我们需要获取未经优化的原始DEX代码。这可以通过修改Android系统的编译选项来实现。

优化和校验的开关由系统属性控制，例如 `dalvik.vm.dexopt-flags`。我们可以在Android系统源码的编译配置中修改这些标志位。

**关键配置位置示例（具体路径可能因版本而异）：**
```
# 在 device.mk 或类似编译配置文件中
PRODUCT_PROPERTY_OVERRIDES += \
    dalvik.vm.dexopt-flags=v=n,o=v  # v=n 表示不校验，o=v 表示不优化
```

修改并重新编译系统后，系统加载DEX时将跳过优化和校验步骤。此时再从内存中脱壳导出的DEX文件，就是原始的、未经修改的代码数据。

你可以尝试在修改系统设置前后分别进行脱壳，并对比导出的DEX文件差异，以直观理解优化的影响。

---

![](img/bd320506450f7ed91192c7df69bd661f_61.png)

## 总结 📝

![](img/bd320506450f7ed91192c7df69bd661f_63.png)

![](img/bd320506450f7ed91192c7df69bd661f_64.png)

本节课我们一起学习了以下内容：

![](img/bd320506450f7ed91192c7df69bd661f_66.png)

1.  **脚本脱壳的补充**：使用脚本脱壳也需要寻找准确的脱壳时机点，并以360加固为例进行了演示。
2.  **DEX优化机制**：了解了Android系统为了提升性能和稳定性，会对DEX指令进行优化（如替换Opcode）和校验。
3.  **优化带来的问题**：经过优化的DEX文件会改变代码逻辑，导致无法正确反编译和运行，给逆向分析造成障碍。
4.  **源码级分析**：从 `dexopt` 源码层面剖析了优化 (`OptimizeClass`) 和校验 (`VerifyClass`) 的具体实现过程。
5.  **解决方案**：通过修改Android系统的编译配置（如 `dalvik.vm.dexopt-flags`），可以禁用DEX的优化和校验，从而在脱壳时获取到原始的、未被修改的DEX代码数据。

![](img/bd320506450f7ed91192c7df69bd661f_68.png)

![](img/bd320506450f7ed91192c7df69bd661f_70.png)

掌握这些原理，能帮助你更好地处理脱壳后遇到的代码异常问题，并理解Android运行时底层的一部分工作机制。