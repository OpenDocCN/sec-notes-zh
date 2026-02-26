#  039：IDA脱壳修复脚本编写 🛠️

![](img/d9ab133e6fc4574b5058034085f6ceaa_1.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_2.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_4.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_5.png)

在本节课中，我们将学习如何编写一个脚本，将脱壳得到的ODEX文件修复为可用的DEX文件。上一节我们分析了Android系统如何将DEX文件优化为ODEX文件，并重写了部分Opcode。本节我们将尝试将被重写的Opcode还原回去。这种修复在某些脱壳数据不包含ODEX信息时非常必要。

![](img/d9ab133e6fc4574b5058034085f6ceaa_7.png)

我们将以阿里加固壳为例进行说明。

## 环境准备与脱壳

![](img/d9ab133e6fc4574b5058034085f6ceaa_9.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_11.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_13.png)

首先，安装目标APK并进行调试挂接，使用脚本将内存中的DEX数据提取出来。

![](img/d9ab133e6fc4574b5058034085f6ceaa_15.png)

以下是提取内存DEX数据的核心步骤：

![](img/d9ab133e6fc4574b5058034085f6ceaa_16.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_18.png)

1.  使用IDA附加到目标进程。
2.  运行脱壳脚本，识别并dump出内存中解码后的DEX数据。
3.  脚本会将数据输出到指定目录。

![](img/d9ab133e6fc4574b5058034085f6ceaa_20.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_21.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_22.png)

脱壳完成后，我们得到一个原始的DEX文件（通常称为`dump.dex`）。

![](img/d9ab133e6fc4574b5058034085f6ceaa_23.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_25.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_27.png)

## 初步反编译与问题分析

![](img/d9ab133e6fc4574b5058034085f6ceaa_29.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_30.png)

得到DEX文件后，我们尝试使用反编译工具（如baksmali）将其转换为smali代码。

```
java -jar baksmali.jar dump.dex -o output_dir
```

![](img/d9ab133e6fc4574b5058034085f6ceaa_32.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_34.png)

然而，反编译过程可能会报错，提示“正在尝试反编译一个ODEX代码”或遇到非法值异常。这通常是因为脱出的DEX数据仍包含ODEX的优化结构，部分Opcode未被正确还原。

例如，错误信息可能指向某个类的特定偏移地址，提示该处的值非法。我们需要定位到这个类，并手动修复错误数据。

## 手动修复错误数据

![](img/d9ab133e6fc4574b5058034085f6ceaa_36.png)

当反编译工具在某个类处报错时，我们需要用十六进制编辑器打开`dump.dex`文件，定位到报错的类及其偏移地址。

![](img/d9ab133e6fc4574b5058034085f6ceaa_38.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_40.png)

以下是修复流程：

![](img/d9ab133e6fc4574b5058034085f6ceaa_41.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_43.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_44.png)

1.  在反编译错误日志中找到报错的类名和偏移地址（例如：`0x11687`）。
2.  用十六进制编辑器打开`dump.dex`，跳转到该地址。
3.  观察该地址附近的数据，如果发现明显异常的值（例如，指向文件头的地址 `0x3036`），这很可能是加固壳填充的无效数据。
4.  将这些非法值修改为 `0` 并保存。

![](img/d9ab133e6fc4574b5058034085f6ceaa_46.png)

修改后，再次尝试反编译，之前的异常可能消失，类能被部分反编译。但此时反编译出的smali代码可能包含优化过的Opcode（如 `iget-quick`），这会导致后续重新编译（smali -> dex）失败。

![](img/d9ab133e6fc4574b5058034085f6ceaa_47.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_49.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_50.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_52.png)

## 使用脚本自动化修复ODEX

![](img/d9ab133e6fc4574b5058034085f6ceaa_54.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_55.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_57.png)

手动修复效率低下且不彻底。更优的方案是编写脚本，系统性地将ODEX中的优化Opcode逆向还原为标准DEX的Opcode。

![](img/d9ab133e6fc4574b5058034085f6ceaa_59.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_61.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_63.png)

脚本的核心思路是：遍历DEX文件中的所有方法代码（Code Item），识别出优化过的Opcode（如 `iget-quick`, `invoke-virtual-quick` 等），并根据Android系统的优化规则，将其替换回标准的Opcode（如 `iget`, `invoke-virtual` 等）。

![](img/d9ab133e6fc4574b5058034085f6ceaa_64.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_65.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_67.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_69.png)

以下是一个简化的修复逻辑伪代码：

![](img/d9ab133e6fc4574b5058034085f6ceaa_71.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_72.png)

```python
def repair_opcode(optimized_opcode, operands):
    # 建立优化Opcode到标准Opcode的映射关系
    opcode_map = {
        ‘iget-quick’: revert_iget_quick,
        ‘invoke-virtual-quick’: revert_invoke_virtual_quick,
        # ... 其他优化Opcode
    }
    if optimized_opcode in opcode_map:
        standard_opcode, new_operands = opcode_map[optimized_opcode](operands)
        return standard_opcode, new_operands
    else:
        return optimized_opcode, operands # 无需修复
```

![](img/d9ab133e6fc4574b5058034085f6ceaa_74.png)

运行修复脚本：

```
python repair_odex.py -i dump.dex -o repaired.dex
```

![](img/d9ab133e6fc4574b5058034085f6ceaa_76.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_77.png)

脚本执行后，会生成修复后的 `repaired.dex` 文件。

![](img/d9ab133e6fc4574b5058034085f6ceaa_79.png)

## 验证修复结果

![](img/d9ab133e6fc4574b5058034085f6ceaa_80.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_82.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_83.png)

使用修复后的DEX文件进行反编译和重打包测试。

![](img/d9ab133e6fc4574b5058034085f6ceaa_85.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_86.png)

1.  反编译修复后的DEX：
    ```
    java -jar baksmali.jar repaired.dex -o repaired_smali
    ```
2.  将smali代码重新编译为DEX：
    ```
    java -jar smali.jar assembled repaired_smali -o final.dex
    ```
3.  用 `final.dex` 替换原APK中的DEX文件，并重新签名安装。

![](img/d9ab133e6fc4574b5058034085f6ceaa_88.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_89.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_91.png)

如果应用能正常运行，说明修复成功。可以对比修复前后反编译的smali代码，原本的 `iget-quick` 等指令应被还原为 `iget` 等标准指令。

## 补充：关于反编译工具的 `-x` 参数

![](img/d9ab133e6fc4574b5058034085f6ceaa_93.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_94.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_96.png)

反编译工具（如baksmali）提供了 `-x`（或 `--experimental`）参数来直接处理ODEX文件，但它需要依赖Android系统的启动类路径（boot classpath）文件。

![](img/d9ab133e6fc4574b5058034085f6ceaa_98.png)

以下是使用 `-x` 参数的步骤：

![](img/d9ab133e6fc4574b5058034085f6ceaa_100.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_101.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_103.png)

1.  从设备中导出 `/system/framework/` 目录下的所有BOOTCLASSPATH文件。
    ```
    adb pull /system/framework/ ./framework/
    ```
2.  使用 `-d` 参数指定框架目录进行反编译。
    ```
    java -jar baksmali.jar -x -d ./framework/ dump.odex -o odex_output
    ```

![](img/d9ab133e6fc4574b5058034085f6ceaa_105.png)

此方法虽然有效，但需要提取系统文件，跨设备时可能不通用。相比之下，我们编写的修复脚本更具通用性和灵活性。

![](img/d9ab133e6fc4574b5058034085f6ceaa_107.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_108.png)

## 脚本实现细节探讨

修复脚本的关键在于准确还原优化Opcode。这通常是一对多的逆向映射，需要根据操作数类型等信息进行判断。

例如，优化指令 `iget-quick` 可能对应原始指令 `iget`、`iget-wide`、`iget-object`、`iget-boolean`、`iget-byte`、`iget-char` 或 `iget-short`。脚本需要通过分析该指令操作数所引用的字段类型，来决定还原为哪一种具体的 `iget-*` 指令。

![](img/d9ab133e6fc4574b5058034085f6ceaa_110.png)

目前脚本可能无法处理所有类型的优化指令（如某些 `*quick` 指令或 `volatile` 访问优化）。对于无法修复的情况，一种保守的策略是像之前手动修复一样，将相关优化标志位清零，使其退化为非优化代码，这至少能保证代码可被反编译和重新编译。

![](img/d9ab133e6fc4574b5058034085f6ceaa_112.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_114.png)

![](img/d9ab133e6fc4574b5058034085f6ceaa_116.png)

本节课中我们一起学习了如何通过编写IDA脱壳修复脚本，将包含优化Opcode的ODEX数据修复为标准的DEX文件。我们了解了手动修复的局限性，探讨了自动化脚本的核心修复逻辑，并与反编译工具的 `-x` 参数方法进行了对比。掌握这项技能能有效提高处理加固壳脱壳数据的效率。