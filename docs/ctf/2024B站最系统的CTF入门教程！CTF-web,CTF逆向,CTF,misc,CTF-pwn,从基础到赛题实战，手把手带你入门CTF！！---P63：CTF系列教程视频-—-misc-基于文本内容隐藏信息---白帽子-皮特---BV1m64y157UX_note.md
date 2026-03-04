# CTF系列教程：P63：基于文本内容隐藏信息 📝

![](img/d63de5b7e8735f2e8ee2af8af338d37f_1.png)

![](img/d63de5b7e8735f2e8ee2af8af338d37f_3.png)

在本节课中，我们将学习CTF杂项（Misc）中一种重要的题型：基于文本内容隐藏信息。我们将探讨如何通过可见或不可见的字符、文件结构以及特殊编码在文本中隐藏信息，并学习相应的分析和提取方法。

---

## 文本隐藏信息的两种主要方式

上一节我们介绍了课程的整体内容，本节中我们来看看基于文本内容隐藏信息的两种主要思路。

### 1. 文字即信息

![](img/d63de5b7e8735f2e8ee2af8af338d37f_5.png)

![](img/d63de5b7e8735f2e8ee2af8af338d37f_7.png)

![](img/d63de5b7e8735f2e8ee2af8af338d37f_9.png)

这种方式利用可见字符本身或其组合来编码信息。它本质上是一种替换密码或古典编码。

以下是常见的几种形式：
*   **古典密码与编码**：例如猪圈密码、银河密码等。
*   **特殊字符编码**：例如佛约编码、与佛论禅等。
*   **抽象文学**：利用特定的文体或格式传递信息。

![](img/d63de5b7e8735f2e8ee2af8af338d37f_11.png)

![](img/d63de5b7e8735f2e8ee2af8af338d37f_13.png)

### 2. 利用不可见字符

![](img/d63de5b7e8735f2e8ee2af8af338d37f_15.png)

![](img/d63de5b7e8735f2e8ee2af8af338d37f_17.png)

这种方式利用在常规阅读中不可见的字符来隐藏信息，对分析工具的要求更高。

![](img/d63de5b7e8735f2e8ee2af8af338d37f_19.png)

![](img/d63de5b7e8735f2e8ee2af8af338d37f_21.png)

以下是几种利用不可见字符的方法：
*   **零宽字符隐写**：利用Unicode中的零宽度字符（如U+200B, U+200C）来编码二进制信息。
*   **软件设置的隐藏**：例如在Word文档中将字体颜色设置为白色，或使用“隐藏文字”功能。
*   **文件损坏或备份**：例如`.php.bak`备份文件，需要用`vim -r`命令恢复查看。
*   **Snow隐写**：利用文本文件末尾的空白区域（行末空格、制表符）隐藏信息，可使用`snow.exe`工具处理。
*   **空白字符编码**：利用空格（Space）和制表符（Tab）代表摩斯电码或二进制（如敲击码、培根密码）。

![](img/d63de5b7e8735f2e8ee2af8af338d37f_23.png)

![](img/d63de5b7e8735f2e8ee2af8af338d37f_25.png)

**核心工具与命令示例**：
*   检查文件隐藏字符：`cat -A 文件名`
*   Snow隐写解密：`snow -C -p 密码 输入文件`
*   零宽字符在线解密工具：`https://330k.github.io/misc_tools/unicode_steganography.html`

![](img/d63de5b7e8735f2e8ee2af8af338d37f_27.png)

![](img/d63de5b7e8735f2e8ee2af8af338d37f_29.png)

---

![](img/d63de5b7e8735f2e8ee2af8af338d37f_31.png)

![](img/d63de5b7e8735f2e8ee2af8af338d37f_33.png)

![](img/d63de5b7e8735f2e8ee2af8af338d37f_35.png)

## 利用文件系统结构隐藏信息

![](img/d63de5b7e8735f2e8ee2af8af338d37f_37.png)

![](img/d63de5b7e8735f2e8ee2af8af338d37f_39.png)

除了直接在文本内容上做文章，出题人还会利用文件系统的目录结构来隐藏信息。这类题目通常需要一定的编程能力来解析结构。

![](img/d63de5b7e8735f2e8ee2af8af338d37f_41.png)

我们以SCTF的一道真题为例进行讲解。题目提供了一个压缩包，解压后得到一系列名为`left`和`right`的文件夹，每个文件夹内有一个`data`文件。

![](img/d63de5b7e8735f2e8ee2af8af338d37f_43.png)

![](img/d63de5b7e8735f2e8ee2af8af338d37f_45.png)

**解题思路分析**：
1.  `left`和`right`的命名暗示这是一个二叉树结构。
2.  每个节点的`data`文件内容（经Base64解码后）是该节点的值。
3.  题目要求我们以正确的顺序（如前序、中序、后序遍历）拼接所有节点的值，才能得到下一步的线索。

![](img/d63de5b7e8735f2e8ee2af8af338d37f_47.png)

![](img/d63de5b7e8735f2e8ee2af8af338d37f_49.png)

**关键Python代码（DFS前序遍历）**：
```python
import os
import base64

![](img/d63de5b7e8735f2e8ee2af8af338d37f_51.png)

![](img/d63de5b7e8735f2e8ee2af8af338d37f_52.png)

![](img/d63de5b7e8735f2e8ee2af8af338d37f_54.png)

def dfs(path):
    # 读取当前节点的数据
    data_path = os.path.join(path, ‘data’)
    if os.path.exists(data_path):
        with open(data_path, ‘r’) as f:
            encoded = f.read().strip()
            decoded = base64.b64decode(encoded).decode(‘utf-8’)
            print(decoded, end=‘’) # 拼接值

    # 优先遍历左子树
    left_path = os.path.join(path, ‘left’)
    if os.path.exists(left_path):
        dfs(left_path)

    # 然后遍历右子树
    right_path = os.path.join(path, ‘right’)
    if os.path.exists(right_path):
        dfs(right_path)

![](img/d63de5b7e8735f2e8ee2af8af338d37f_56.png)

# 从根目录开始遍历
dfs(‘./unzip_folder’)
```
运行脚本后，我们得到一串Base64编码的文本，解码后是一段中文提示，指向“伏羲六十四卦”编码。使用在线工具或特定脚本对其进行解码，即可获得最终的Flag。

这道题综合运用了文件系统分析、Base64解码、古典编码识别以及Python脚本编写，是本节课知识点的完美实践。

---

![](img/d63de5b7e8735f2e8ee2af8af338d37f_58.png)

![](img/d63de5b7e8735f2e8ee2af8af338d37f_60.png)

![](img/d63de5b7e8735f2e8ee2af8af338d37f_62.png)

## 特殊编程语言与编码

![](img/d63de5b7e8735f2e8ee2af8af338d37f_64.png)

在CTF中，我们还会遇到一些利用特殊编程语言或编码规则隐藏信息的题目。识别这些“语言”是解题的第一步。

以下是一些常见的特殊形式：
*   **Ook!**： 一种使用“Ook.”、“Ook?”、“Ook!”组合的图灵完备语言。
*   **Brainfuck**： 仅使用`> < + - . , [ ]`八个字符的极简语言，在Misc和Reverse中都很常见。
*   **JSFuck**： 仅用`[`、`]`、`(`、`)`、`+`、`!`六个字符编写有效JavaScript代码。
*   **Whitespace**： 仅使用空格（Space）、制表符（Tab）和换行（LF）作为有效字符的语言。
*   **Piet**： 一种使用颜色块表示指令的编程语言。
*   **Emoji编程**： 利用Emoji字符编写程序，例如“Emojicode”。

**应对策略**：
1.  **识别特征**：观察文本是否由极有限的特殊字符集构成。
2.  **搜索判断**：将特征字符串（如“Ook.”、“`++++[>++++<-]`”）直接进行网络搜索。
3.  **使用解释器**：找到对应的语言解释器或在线执行环境来运行代码。

---

![](img/d63de5b7e8735f2e8ee2af8af338d37f_66.png)

![](img/d63de5b7e8735f2e8ee2af8af338d37f_68.png)

## 总结与作业

本节课我们一起学习了CTF中基于文本内容隐藏信息的多种技术：
1.  **可见字符编码**：如古典密码、特殊字符编码。
2.  **不可见字符隐写**：如零宽字符、Snow隐写、空白字符编码。
3.  **结构隐藏**：利用文件系统目录树结构承载信息。
4.  **特殊语言**：识别并执行如Brainfuck、Whitespace等特殊编程语言。

**核心要点**：处理文本类题目时，首先要**全面检查**（如用`cat -A`查看所有字符），其次要**大胆联想**（结构、编码、语言），最后要**善用工具和脚本**自动化处理。

![](img/d63de5b7e8735f2e8ee2af8af338d37f_70.png)

![](img/d63de5b7e8735f2e8ee2af8af338d37f_71.png)

![](img/d63de5b7e8735f2e8ee2af8af338d37f_73.png)

**课后作业**：
1.  尝试解密一道零宽字符隐写题目。
2.  复现并理解SCTF中利用文件树隐写的题目。
3.  搜索并尝试运行一段简单的Brainfuck或Ook!代码。

![](img/d63de5b7e8735f2e8ee2af8af338d37f_75.png)

![](img/d63de5b7e8735f2e8ee2af8af338d37f_76.png)

通过掌握这些方法，你将能够更从容地应对CTF杂项中各类文本隐写与编码题目。