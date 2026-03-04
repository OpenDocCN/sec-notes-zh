# CTF培训：5：杂项的基本解题思路上半部分 🚩

![](img/63bbcc97db7f2721a3b166dc5217eb4b_0.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_2.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_4.png)

在本节课中，我们将要学习CTF比赛中“杂项”题型的基本解题思路。杂项题目内容庞杂，但掌握核心工具和方法后，是拿分的关键。本节课将重点介绍文件操作与隐写的基础知识。

![](img/63bbcc97db7f2721a3b166dc5217eb4b_6.png)

## 文件操作与隐写 📁

![](img/63bbcc97db7f2721a3b166dc5217eb4b_8.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_10.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_12.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_14.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_16.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_18.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_20.png)

上一节我们介绍了CTF的五大题型。本节中，我们来看看杂项题中最基础的部分——文件操作与隐写。很多时候，题目会直接给你一个文件，你需要从中找出隐藏的信息。

![](img/63bbcc97db7f2721a3b166dc5217eb4b_22.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_24.png)

### 文件类型识别

要对文件进行操作，首先需要知道它是什么类型的文件。出题人有时会删除文件的后缀名来增加难度。

![](img/63bbcc97db7f2721a3b166dc5217eb4b_26.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_28.png)

以下是识别文件类型的常用方法：

*   **`file` 命令**：这是Linux下的一个命令行工具，可以识别文件的真实类型。
    ```bash
    file 文件名
    ```
*   **十六进制编辑器**：通过查看文件的头部（文件头）特定字节来判断类型。常见的文件头有：
    *   JPEG: `FF D8 FF E0`
    *   PNG: `89 50 4E 47`
    *   ZIP: `50 4B 03 04`
*   **常用工具**：
    *   `WinHex` / `010 Editor`: 专业的十六进制编辑器。
    *   `Notepad++` (配合 `Hex-Editor` 插件): 轻量级的查看和编辑工具。

![](img/63bbcc97db7f2721a3b166dc5217eb4b_30.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_32.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_34.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_36.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_38.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_39.png)

### 文件头修复

![](img/63bbcc97db7f2721a3b166dc5217eb4b_41.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_43.png)

如果文件头被破坏或错误，`file`命令将无法识别，文件也无法正常打开。此时需要用十六进制编辑器手动修复，将正确的文件头字节补充回去。

### 文件分离

一个文件（如图片）里可能隐藏着其他文件（如压缩包、文本）。我们需要将其分离出来。

![](img/63bbcc97db7f2721a3b166dc5217eb4b_45.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_47.png)

以下是文件分离的几种方法：

*   **`binwalk` 命令**：自动分析并提取文件中嵌入的其他文件。
    ```bash
    binwalk -e 文件名          # 分析并自动提取
    ```
*   **`foremost` 命令**：另一种自动分离工具，会对提取出的文件进行分类。
    ```bash
    foremost -i 文件名 -o 输出目录
    ```
*   **`dd` 命令**：一个更底层的工具，可以精确地从指定位置开始，提取指定大小的数据块。需要配合`binwalk`的分析结果计算偏移量和大小。
    ```bash
    dd if=输入文件 of=输出文件 bs=块大小 skip=跳过块数 count=读取块数
    ```
*   **手动提取**：使用`010 Editor`等工具，直接选中十六进制数据区域，另存为新文件。

### 文件合并

![](img/63bbcc97db7f2721a3b166dc5217eb4b_49.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_51.png)

与分离相反，有时题目会将一个文件分割成多个碎片，需要你将其合并。

![](img/63bbcc97db7f2721a3b166dc5217eb4b_53.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_55.png)

以下是文件合并的方法：

![](img/63bbcc97db7f2721a3b166dc5217eb4b_57.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_59.png)

*   **Linux (`cat`命令)**:
    ```bash
    cat 碎片1 碎片2 碎片3 > 合并后的文件
    ```
*   **Windows (`copy`命令)**:
    ```bash
    copy /B 碎片1 + 碎片2 + 碎片3 合并后的文件
    ```
合并后，可能需要校验文件的MD5值以确保完整性，并使用十六进制编辑器修复可能缺失的文件头。

### 十六进制操作

![](img/63bbcc97db7f2721a3b166dc5217eb4b_61.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_63.png)

有时题目给的不是标准文件，而是一串十六进制文本。你需要将其导入十六进制编辑器（如`010 Editor`的 `Import` 功能），然后另存为正确的文件格式。

### 字符串搜索

在毫无头绪时，可以尝试在文件中直接搜索关键字符串，如`flag`、`key`等。注意不仅要在文本中搜索，也要在十六进制数据中搜索。

## 图片隐写基础 🖼️

上一节我们介绍了通用的文件操作。本节中，我们聚焦于杂项中最常见的载体——图片，学习基础的图片隐写术。

![](img/63bbcc97db7f2721a3b166dc5217eb4b_65.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_67.png)

### 基本概念

![](img/63bbcc97db7f2721a3b166dc5217eb4b_69.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_71.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_73.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_74.png)

图片隐写的方式多种多样，常见的有：
*   **细微颜色差别**：肉眼难以察觉的像素值修改。
*   **GIF多帧隐藏**：信息藏在动图的某一帧中。
*   **颜色通道**：利用R(红)、G(绿)、B(蓝)不同通道存储信息。
*   **EXIF信息**：图片的元数据，可能包含拍摄时间、GPS坐标、提示信息等。
*   **图片修复**：图片长宽、CRC校验值被篡改，导致无法正常显示。
*   **LSB隐写**：将信息隐藏在像素颜色值的最低有效位，对图片观感影响极小。
*   **图片加密**：使用特定算法对图片信息进行加密。

![](img/63bbcc97db7f2721a3b166dc5217eb4b_76.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_78.png)

### 工具使用与实战

**1. EXIF信息查看**
图片属性中可查看详细信息。Linux下可使用`exiftool`命令行工具。
```bash
exiftool 图片名
```

![](img/63bbcc97db7f2721a3b166dc5217eb4b_80.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_82.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_84.png)

**2. 图片差异比较 (Stegsolve)**
当给出两张相似图片时，可能需要对它们进行异或、加减等操作来找出隐藏信息。`Stegsolve` 是一款常用的Java工具，可以方便地进行这些操作。

![](img/63bbcc97db7f2721a3b166dc5217eb4b_86.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_88.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_90.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_92.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_94.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_96.png)

**3. LSB隐写破解**
LSB隐写是高频考点。
*   **Stegsolve**: 在 `Analyse` -> `Data Extract` 功能中，勾选不同颜色通道和位平面进行预览和提取。
*   **zsteg**: 一个命令行工具，能自动尝试多种LSB组合并输出结果。
    ```bash
    zsteg 图片名
    ```
*   **wbStego**: 支持BMP等格式的LSB隐写解密。
*   **Python脚本**：当现成工具无效时，可能需要根据题目特制脚本提取数据。

![](img/63bbcc97db7f2721a3b166dc5217eb4b_98.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_100.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_101.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_103.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_105.png)

**4. 图片修复 (PNG)**
PNG图片无法打开，常因CRC校验错误或图片高度/宽度被篡改。
*   **TweakPNG**: 图形化工具，可打开损坏的PNG并提示CRC错误。
*   **手动修复**：
    *   **CRC错误**：用`TweakPNG`或十六进制编辑器，将错误的CRC值修正为正确的值。
    *   **高度/宽度错误**：通过Python脚本计算正确的图片尺寸，并用十六进制编辑器修改文件中的对应字段。

**5. 图片加密**
需要识别加密方式并使用对应工具解密。
*   **识别工具**: `stegdetect` 可以检测JPG图片是否经过加密（如`jphide`, `outguess`, `F5`等）。
    ```bash
    stegdetect -s 敏感度 图片名
    ```
*   **解密工具**: 根据检测结果使用对应工具（如 `jphide` 的 `jpeg-x`， `outguess` 需自行编译，`F5` 需Java环境）。

![](img/63bbcc97db7f2721a3b166dc5217eb4b_107.png)

**6. 二维码处理**
最终得到的二维码可能被反色、残缺或夹杂图案。
*   **画图工具**：可进行反色（选择后右键“反色”）、裁剪、修补等简单操作。
*   **Stegsolve**：查看颜色通道，可能找到可识别的二维码图层。
*   **在线解码工具**：修复后的二维码可用手机或在线工具扫描。

![](img/63bbcc97db7f2721a3b166dc5217eb4b_109.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_111.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_113.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_115.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_117.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_119.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_121.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_122.png)

![](img/63bbcc97db7f2721a3b166dc5217eb4b_124.png)

本节课中我们一起学习了杂项题目中文件操作与基础图片隐写的核心思路和工具。关键在于熟悉文件结构，掌握各类工具（如`file`, `binwalk`, `Stegsolve`, `zsteg`）的用法，并能根据题目线索灵活运用。下节课我们将继续探讨更复杂的杂项题型。