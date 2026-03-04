# CTF培训网络安全基础入门：P14：杂项（上）教程

![](img/e653577dfee86da28bdf170ba8b40ec5_1.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_3.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_5.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_7.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_9.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_11.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_13.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_15.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_16.png)

## 概述
在本节课中，我们将要学习CTF比赛中“杂项”（Misc）类题目的基础入门知识，特别是文件操作与信息隐藏（隐写术）的相关内容。我们将学习如何识别、分析和处理包含隐藏信息的文件，并掌握相关的工具和方法。

![](img/e653577dfee86da28bdf170ba8b40ec5_17.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_19.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_21.png)

---

![](img/e653577dfee86da28bdf170ba8b40ec5_23.png)

## 文件操作与隐写

![](img/e653577dfee86da28bdf170ba8b40ec5_25.png)

### 文件类型识别
上一节我们介绍了杂项题目的特点，本节中我们来看看如何分析下载的文件。每个文件都有其特定的结构，可以通过文件头部（文件头）来识别其类型。

**核心概念：文件头**
文件头是文件起始处的特定字节序列，用于标识文件格式。例如：
*   JPEG 文件头：`FF D8 FF E0`
*   PNG 文件头：`89 50 4E 47`
*   ZIP 文件头：`50 4B 03 04`

**识别方法：**
1.  **手工分析（使用十六进制编辑器）**
    *   **工具**：Notepad++（需安装Hex Editor插件）或 010 Editor。
    *   **操作**：用编辑器以十六进制模式打开文件，查看文件起始的几个字节，并与已知的文件头进行比对。
    *   **示例（010 Editor中查看PNG文件头）**：
        ```
        地址: 00000000  89 50 4E 47 0D 0A 1A 0A ...
        对应ASCII: . P N G ........
        ```
        可以看到起始字节 `89 50 4E 47` 对应 ASCII 字符 “.PNG”，表明这是一个PNG图片。

![](img/e653577dfee86da28bdf170ba8b40ec5_27.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_29.png)

2.  **工具分析（使用 `file` 命令）**
    *   **环境**：Linux（如Kali）。
    *   **命令**：`file <文件名>`
    *   **示例**：`file example` 命令会输出该文件的类型信息。

![](img/e653577dfee86da28bdf170ba8b40ec5_31.png)

**当文件无法打开时：**
如果文件扩展名丢失或损坏，导致无法正常打开，通常需要检查并修复文件头。

![](img/e653577dfee86da28bdf170ba8b40ec5_33.png)

**文件头修复示例（以ZIP文件为例）：**
如果一个文件实际是ZIP压缩包，但文件头损坏，可以使用十六进制编辑器（如010 Editor）在文件起始处插入正确的ZIP文件头：`50 4B 03 04`。

![](img/e653577dfee86da28bdf170ba8b40ec5_35.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_37.png)

---

![](img/e653577dfee86da28bdf170ba8b40ec5_39.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_41.png)

### 文件分离
有时，一个文件中可能隐藏（附加）了另一个文件。我们需要将其分离出来。

![](img/e653577dfee86da28bdf170ba8b40ec5_43.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_45.png)

以下是几种分离方法：

![](img/e653577dfee86da28bdf170ba8b40ec5_47.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_49.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_51.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_53.png)

**1. 自动化分离工具**
这些工具可以自动识别并分离出文件中嵌入的其他文件。
*   **`binwalk` 命令**：
    *   **分析文件**：`binwalk <文件名>` （用于分析文件结构）
    *   **分离文件**：`binwalk -e <文件名>` （`-e` 参数用于提取嵌入的文件）
*   **`foremost` 命令**：
    *   当 `binwalk` 无法有效分离时，可以尝试使用 `foremost`。
    *   **命令**：`foremost <文件名> -o <输出目录>` （`-o` 指定输出目录）

**2. 半自动化/手动分离**
当自动化工具失效时，需要手动分析并分离。
*   **使用 `dd` 命令**：`dd` 命令可以按指定位置和大小从文件中提取数据块。
    *   **语法**：`dd if=输入文件 of=输出文件 bs=块大小 count=块数 skip=跳过的块数`
    *   **示例**：从一个JPEG文件（内含ZIP）中提取ZIP部分。
        1.  先用 `binwalk` 分析出ZIP部分的起始偏移量（例如 22895）和大小。
        2.  使用命令：`dd if=隐藏文件.jpg of=提取出的.zip bs=1 skip=22895`。此命令从偏移量22895字节处开始，提取数据到新文件。

*   **使用十六进制编辑器手动分离**：
    1.  用010 Editor等工具打开文件。
    2.  搜索第二个文件的文件头（如 `50 4B 03 04`）。
    3.  从该位置开始，选中直到文件末尾的数据。
    4.  将选中的数据另存为一个新文件。

![](img/e653577dfee86da28bdf170ba8b40ec5_55.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_57.png)

---

![](img/e653577dfee86da28bdf170ba8b40ec5_59.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_60.png)

### 文件合并与校验
有时题目会将一个文件分割成多个部分，需要合并后才能得到完整信息。

**文件合并（Linux下）**：
使用 `cat` 命令可以合并多个文件。
*   **命令**：`cat 文件1 文件2 文件3 ... > 合并后的文件`
*   **示例**：`cat part1.gif part2.gif part3.gif > complete.gif`

![](img/e653577dfee86da28bdf170ba8b40ec5_62.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_64.png)

**文件校验（MD5校验）**：
下载或合并文件后，常需要验证其完整性，确保与出题方提供的文件一致。
*   **命令**：`md5sum <文件名>`
*   **操作**：计算本地文件的MD5哈希值，与题目给出的MD5值进行比对。如果一致，则文件完整无误。

![](img/e653577dfee86da28bdf170ba8b40ec5_66.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_68.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_69.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_71.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_73.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_74.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_76.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_78.png)

**注意**：MD5校验用于验证文件完整性，与MD5加密（不可逆的哈希函数）是不同的概念，不能通过MD5值反推原文件内容。

![](img/e653577dfee86da28bdf170ba8b40ec5_80.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_82.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_84.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_86.png)

---

## 图片隐写术
图片是信息隐藏的常见载体。本节介绍几种常见的图片隐写术及破解方法。

### 1. 文件结构隐写
*   **Fireworks (PNG) 图层/帧隐藏**：使用Adobe Fireworks编辑的PNG图片可能包含多个图层或动画帧，其中可能隐藏信息。可以用Fireworks软件或某些图片查看工具检查图层和帧。
*   **EXIF信息隐藏**：数码照片的EXIF数据中存储了拍摄参数、时间、GPS位置等信息，有时flag就藏在这里。
    *   **查看方法**：
        *   Windows：右键图片 -> 属性 -> 详细信息。
        *   Linux命令：`exiftool <图片文件名>`

![](img/e653577dfee86da28bdf170ba8b40ec5_88.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_90.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_92.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_94.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_95.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_96.png)

### 2. 图片对比分析
当两张图片看起来几乎完全相同时，可以考虑进行像素级对比分析，通过数学运算发现差异。
*   **工具**：Stegsolve（Java图像分析工具）。
*   **操作**：
    1.  用 Stegsolve 打开图片A。
    2.  选择菜单 `Analyze` -> `Image Combiner`，并选择图片B进行对比。
    3.  尝试不同的运算模式（如 XOR, ADD, SUB等），观察生成的图像中是否出现隐藏的二维码或文字。

![](img/e653577dfee86da28bdf170ba8b40ec5_98.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_100.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_102.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_104.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_106.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_108.png)

### 3. LSB（最低有效位）隐写
这是最常见的隐写术之一。原理是修改像素颜色值（RGB）的最低有效位（Least Significant Bit）来存储信息，因为人眼很难察觉这种微小的颜色变化。

![](img/e653577dfee86da28bdf170ba8b40ec5_110.png)

**提取LSB隐藏信息的方法：**

![](img/e653577dfee86da28bdf170ba8b40ec5_112.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_114.png)

![](img/e653577dfee86da28bdf170ba8b40ec5_116.png)

*   **使用 Stegsolve**：
    1.  打开图片，选择 `Analyze` -> `Data Extract`。
    2.  勾选RGB通道的最低有效位（LSB）。
    3.  尝试不同的位平面顺序（如RGB, RBG等）。
    4.  预览提取出的数据，可能会直接看到flag或一段可读文本。

*   **使用 `zsteg` 工具（针对PNG/BMP）**：
    *   **安装**：`gem install zsteg` （需先安装Ruby环境）
    *   **使用**：`zsteg <图片文件名>` 该命令会自动尝试多种LSB提取方式并显示结果。

![](img/e653577dfee86da28bdf170ba8b40ec5_118.png)

*   **使用 `stegolsb` 工具**：
    *   这是一个命令行工具，可以用于LSB隐写的编码和解码。
    *   **示例（提取）**：`stegolsb steglsb -r -i 隐藏的图片.png -o 提取出的.txt -n 1` （`-n 1` 表示使用最低1个位平面）

*   **使用脚本（Python示例）**：
    可以编写Python脚本，使用PIL/Pillow库读取图片像素，并提取每个颜色通道LSB位的值，组合成隐藏信息。
    ```python
    from PIL import Image
    import sys

    def lsb_extract(img_path):
        img = Image.open(img_path)
        pixels = img.load()
        width, height = img.size
        binary_data = []
        for y in range(height):
            for x in range(width):
                r, g, b = pixels[x, y][:3] # 取RGB值
                binary_data.append(r & 1) # 取R通道LSB
                binary_data.append(g & 1) # 取G通道LSB
                binary_data.append(b & 1) # 取B通道LSB
        # 将二进制数据转换为字节
        # ... (后续处理代码)
        return extracted_data
    ```

### 4. 二维码修复与识别
杂项题中经常出现二维码，但可能被故意损坏、遮挡或颜色反转。
*   **修复工具**：简单的画图工具即可进行涂抹、裁剪、颜色反转等操作。
*   **识别工具**：手机扫码软件、`zbarimg`命令行工具 (`zbarimg 二维码图片.png`)。

![](img/e653577dfee86da28bdf170ba8b40ec5_120.png)

---

## 总结
本节课中我们一起学习了CTF杂项题目上半部分的核心内容：
1.  **文件操作**：掌握了通过文件头识别文件类型、修复损坏文件头、以及使用 `binwalk`、`dd`、`foremost` 等工具进行文件分离与合并的技巧。
2.  **图片隐写术**：了解了多种在图片中隐藏信息的方式，包括检查EXIF数据、分析图片图层/帧、使用Stegsolve进行图片对比和LSB分析，以及使用 `zsteg` 等专用工具。
3.  **核心方法**：处理杂项题目的通用思路是：先检查文件属性，尝试常规工具自动分析，若不成功则进行手动深度分析（十六进制编辑、像素操作等），并善于利用题目给出的任何潜在提示。

![](img/e653577dfee86da28bdf170ba8b40ec5_122.png)

通过本节课的学习，你应该具备了处理基础文件隐写和图片隐写题目的能力。下节课我们将继续学习杂项的其他类型，如流量分析、压缩包处理等。