# 课程P73：第42天 - Linux权限提升实践指南 🔐

![](img/c8bb69436c0ac629d23ef58dc95dceb4_1.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_3.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_5.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_7.png)

在本节课中，我们将学习Linux环境下几种常见的权限提升方法。课程将涵盖利用公开漏洞、环境变量以及密码哈希等手段，通过实际案例演示如何从普通用户权限提升至root权限。

![](img/c8bb69436c0ac629d23ef58dc95dceb4_9.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_11.png)

## 概述 📋

![](img/c8bb69436c0ac629d23ef58dc95dceb4_13.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_15.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_17.png)

权限提升是渗透测试和系统安全评估中的关键环节。当攻击者获得一个低权限的shell后，下一步就是尝试提升权限，以获取对系统的完全控制。本节课将介绍在Linux系统中实现权限提升的多种技术路径。

![](img/c8bb69436c0ac629d23ef58dc95dceb4_19.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_21.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_23.png)

## 第一部分：利用公开漏洞提权

上一节我们概述了权限提升的基本概念，本节中我们来看看如何利用已知的系统或服务漏洞进行提权。

### 1. 使用SearchSploit查找漏洞利用代码

![](img/c8bb69436c0ac629d23ef58dc95dceb4_25.png)

SearchSploit是Exploit-DB的命令行搜索工具，内置了漏洞数据库，可以方便地查找公开的漏洞利用代码。

以下是使用SearchSploit的基本步骤：

1.  通过`uname -a`命令获取目标系统的内核版本信息。
2.  使用`searchsploit`命令，以内核版本号为关键词进行搜索。例如：`searchsploit 4.15`。
3.  从搜索结果中找到匹配的漏洞利用代码（EXP）。代码通常以C程序形式存在，并附有编号（如47163）。
4.  利用`searchsploit`的`-m`参数，将找到的EXP复制到当前工作目录。命令格式为：`searchsploit -m 47163`。
5.  使用GCC编译器对C代码进行编译：`gcc 47163.c -o exp`。
6.  在目标系统上运行编译生成的`exp`程序，尝试进行提权。

![](img/c8bb69436c0ac629d23ef58dc95dceb4_27.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_29.png)

**核心命令示例：**
```bash
searchsploit -m 47163
gcc 47163.c -o exp
./exp
```

![](img/c8bb69436c0ac629d23ef58dc95dceb4_31.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_33.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_35.png)

> **注意**：提权成功率依赖于目标系统是否存在特定漏洞，具有一定偶然性。并非所有找到的EXP都能成功利用。

![](img/c8bb69436c0ac629d23ef58dc95dceb4_37.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_39.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_41.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_43.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_45.png)

### 2. 脏牛提权漏洞

![](img/c8bb69436c0ac629d23ef58dc95dceb4_47.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_49.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_51.png)

脏牛漏洞是一个影响范围极广的本地提权漏洞，在爆发之初影响了当时几乎全版本的Linux系统，允许低权限用户获取root权限。

![](img/c8bb69436c0ac629d23ef58dc95dceb4_52.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_54.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_55.png)

由于其影响深远，此处不再演示，但建议通过相关实验进行复现和分析，以深入理解其原理。

![](img/c8bb69436c0ac629d23ef58dc95dceb4_57.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_59.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_61.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_62.png)

### 3. CVE-2019-13272 本地提权漏洞

![](img/c8bb69436c0ac629d23ef58dc95dceb4_63.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_64.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_65.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_66.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_67.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_68.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_69.png)

该漏洞影响内核版本4.10至5.1.17的Linux系统。

![](img/c8bb69436c0ac629d23ef58dc95dceb4_71.png)

以下是利用此漏洞的步骤：

![](img/c8bb69436c0ac629d23ef58dc95dceb4_73.png)

1.  根据内核版本，在Exploit-DB上搜索并下载对应的EXP（例如编号47163的C程序）。
2.  将EXP上传至目标机器。
3.  使用GCC编译：`gcc 47163.c -o exp`。
4.  直接运行编译后的程序：`./exp`。
5.  若漏洞存在，执行后将获得一个具有root权限的shell。

![](img/c8bb69436c0ac629d23ef58dc95dceb4_75.png)

### 4. CVE-2019-7304 Snap包管理器提权漏洞

此漏洞影响Ubuntu 14.04, 16.04, 18.04等系统，Snap版本在2.28至2.37之间。

利用方法如下：

1.  在目标系统上发现存在漏洞版本的Snap服务。
2.  从GitHub获取利用脚本。
3.  脚本提供了两种利用方式：
    *   **方法一**：创建SSH密钥，通过特定脚本生成后门账户。
    *   **方法二**：执行脚本后，会创建一个名为`dirty_sock`、密码同为`dirty_sock`的用户，该用户拥有sudo权限。
4.  使用方法二时，执行脚本后，可通过`su dirty_sock`切换用户，然后使用`sudo`命令以root权限执行指令。

**关键点**：某些系统（如实验靶机）的Snap服务会自动更新，从而修复漏洞。因此，在测试时需注意版本，或创建快照以防止自动更新。

## 第二部分：Linux环境变量提权

上一部分我们介绍了利用公开漏洞的方法，本节中我们将探讨一种利用Linux环境变量和SUID权限进行提权的技术。

### 1. 理解PATH环境变量与SUID权限

![](img/c8bb69436c0ac629d23ef58dc95dceb4_77.png)

*   **PATH环境变量**：指定了系统查找可执行程序的目录列表。当用户在终端输入命令时，系统会按照PATH中的目录顺序搜索该命令。
    *   查看命令：`echo $PATH`
*   **SUID权限**：一种特殊的文件权限。当用户执行具有SUID权限的程序时，该进程会继承程序**所有者**的权限，而非执行者的权限。
    *   SUID权限在文件列表中显示为所有者执行权限位的 `s`（例如 `-rwsr-xr-x`）。
    *   查找具有SUID权限的文件：`find / -perm -u=s -type f 2>/dev/null`

### 2. 环境变量提权原理与步骤

![](img/c8bb69436c0ac629d23ef58dc95dceb4_79.png)

提权思路是：找到一个具有SUID权限、且会调用系统命令（如`ps`）的程序。通过劫持PATH环境变量，使其优先执行我们伪造的命令，从而获得root shell。

![](img/c8bb69436c0ac629d23ef58dc95dceb4_81.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_83.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_85.png)

以下是具体操作步骤：

![](img/c8bb69436c0ac629d23ef58dc95dceb4_87.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_89.png)

1.  **寻找目标**：找到一个属主为root且具有SUID权限的程序，例如一个自定义的`/home/kali/shell`程序，其功能是执行`ps`命令。
2.  **创建恶意程序**：在`/tmp`目录下创建一个名为`ps`的文件，内容为反弹shell或直接启动bash。
    ```bash
    echo '/bin/bash' > /tmp/ps
    chmod 777 /tmp/ps
    ```
3.  **劫持PATH环境变量**：将`/tmp`目录添加到当前PATH环境变量的最前面。
    ```bash
    export PATH=/tmp:$PATH
    ```
4.  **触发提权**：执行之前找到的SUID程序（如`./shell`）。此时，该程序以root权限运行，并调用`ps`命令。
    *   系统查找`ps`命令时，根据被修改的PATH，优先在`/tmp`目录下找到了我们创建的恶意`ps`文件。
    *   root权限的进程执行了`/tmp/ps`，即启动了`/bin/bash`。
    *   由于进程继承root权限，因此获得了一个具有root权限的bash shell。

![](img/c8bb69436c0ac629d23ef58dc95dceb4_91.png)

**核心流程公式：**
`SUID程序(root权限) + 被篡改的PATH -> 执行恶意ps -> 获得root shell`

## 第三部分：利用密码哈希提权

![](img/c8bb69436c0ac629d23ef58dc95dceb4_93.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_95.png)

本节我们来看一种最直接的提权方式——获取并破解高权限用户的密码哈希。

![](img/c8bb69436c0ac629d23ef58dc95dceb4_97.png)

### 1. 关键系统文件

![](img/c8bb69436c0ac629d23ef58dc95dceb4_99.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_101.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_103.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_105.png)

Linux系统中用户密码信息主要存储在两个文件中：

![](img/c8bb69436c0ac629d23ef58dc95dceb4_107.png)

*   `/etc/passwd`：所有用户可读，存储用户基本信息（用户名、UID、GID等）。密码字段通常用`x`表示，实际哈希值不在此处。
*   `/etc/shadow`：仅root用户可读，存储加密后的用户密码哈希及其他安全信息。

### 2. 提权方法

![](img/c8bb69436c0ac629d23ef58dc95dceb4_109.png)

在某些配置不当的系统上，`/etc/passwd`文件中可能直接存储了密码哈希（而非`x`）。

![](img/c8bb69436c0ac629d23ef58dc95dceb4_111.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_112.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_114.png)

提权步骤如下：

![](img/c8bb69436c0ac629d23ef58dc95dceb4_116.png)

1.  **查看`/etc/passwd`文件**：`cat /etc/passwd`
2.  **寻找异常条目**：查找密码字段不是`x`，且UID为0（root）的用户行。
    *   例如：`toor:$1$xyz$...:0:0:root:/root:/bin/bash`
3.  **提取哈希值**：复制`$1$xyz$...`这部分哈希值。
4.  **破解哈希**：使用在线网站或本地工具破解哈希。
    *   常用网站：cmd5, somd5
5.  **登录系统**：使用破解得到的明文密码，通过`su`命令切换至对应用户，即可获得root权限。

![](img/c8bb69436c0ac629d23ef58dc95dceb4_118.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_120.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_122.png)

**创建后门账户**：也可以反向操作，在`/etc/passwd`中手动添加一个UID和GID为0的用户，并设置一个已知哈希的密码，从而创建一个具有root权限的后门账户。

![](img/c8bb69436c0ac629d23ef58dc95dceb4_124.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_126.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_127.png)

## 总结与课后作业 🎯

![](img/c8bb69436c0ac629d23ef58dc95dceb4_129.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_131.png)

本节课我们一起学习了Linux系统下多种权限提升技术：
1.  利用SearchSploit查找并利用公开的系统内核或服务漏洞。
2.  利用环境变量PATH和SUID权限的程序进行提权。
3.  通过获取并破解`/etc/passwd`或`/etc/shadow`中的密码哈希来获得高权限账户。

![](img/c8bb69436c0ac629d23ef58dc95dceb4_133.png)

![](img/c8bb69436c0ac629d23ef58dc95dceb4_135.png)

**课后作业**：
1.  复现“脏牛”提权漏洞实验，理解其原理。
2.  在实验环境中，尝试利用CVE-2019-13272或环境变量提权技术完成一次完整的权限提升流程。
3.  动手操作课程PPT中涉及的所有命令和步骤，加深理解。