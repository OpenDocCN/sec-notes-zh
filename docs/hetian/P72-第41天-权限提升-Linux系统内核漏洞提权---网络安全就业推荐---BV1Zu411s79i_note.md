# 课程P72：Linux系统内核漏洞提权 🔓

在本节课中，我们将学习Linux系统下的权限提升技术。课程将涵盖信息收集、内核漏洞利用、环境变量滥用以及密码哈希破解等方法，并通过实验进行巩固。

---

## 第一部分：Linux提权信息收集 📊

上一节我们介绍了课程的整体框架，本节中我们来看看如何进行有效的信息收集。这与Windows提权类似，第一步都是收集目标系统的各类信息，为后续寻找提权突破口做准备。

以下是Linux提权信息收集的关键方面：

*   **系统与内核版本**：`uname -a` 命令用于获取内核版本，这是寻找内核漏洞提权的关键依据。
*   **环境变量**：`echo $PATH` 命令用于查看环境变量配置，与环境变量提权相关。
*   **网络信息**：`ifconfig` 或 `ip addr` 命令用于查看网卡信息。
*   **应用程序与服务**：`ps aux` 或 `systemctl list-units` 命令用于查看运行中的服务及其版本，寻找存在漏洞的服务。
*   **计划任务**：`crontab -l` 命令用于查看当前用户的计划任务。
*   **配置文件**：检查如 `/etc/passwd`、`/etc/shadow`、Web服务配置文件等，可能包含敏感信息。

为了提高效率，我们通常使用自动化脚本进行批量信息收集。

![](img/22e87a87a487476b10e17bbe5b8ae6c8_1.png)

![](img/22e87a87a487476b10e17bbe5b8ae6c8_3.png)

### 自动化信息收集脚本

![](img/22e87a87a487476b10e17bbe5b8ae6c8_5.png)

以下是一个常用的Linux提权信息收集脚本，它集成了上述大部分命令：

```bash
# 从GitHub获取脚本
curl -L https://github.com/your-repo/linenum.sh -o linenum.sh
# 赋予执行权限并运行
chmod +x linenum.sh
./linenum.sh
```

脚本执行后会输出大量信息，主要包括：

![](img/22e87a87a487476b10e17bbe5b8ae6c8_7.png)

![](img/22e87a87a487476b10e17bbe5b8ae6c8_9.png)

*   **基础信息**：系统版本、主机名、当前用户等。
*   **可写目录**：当前用户拥有写入权限的目录。
*   **可用命令**：系统中已安装的实用命令，如 `ping`、`nc`、`wget`、`gcc`、`python3` 等。
*   **系统详情**：发行版信息、Sudo版本、环境变量 `$PATH`、磁盘与CPU信息。
*   **进程与服务**：正在运行的服务、定时任务 (`crontab`) 等。
*   **配置文件**：各种服务的配置详情。

分析脚本输出的结果，可以帮助我们快速定位潜在的提权路径。

---

## 第二部分：查找漏洞利用代码 🔍

![](img/22e87a87a487476b10e17bbe5b8ae6c8_11.png)

上一节我们学习了如何收集信息，本节中我们来看看如何根据收集到的信息查找可用的漏洞利用代码。拥有漏洞利用代码是成功提权的前提。

以下是查找漏洞利用代码的常用网站：

*   **Exploit Database**：著名的漏洞信息库，提供大量漏洞的详细说明和利用代码。
*   **SecurityFocus**：提供全面的漏洞公告、解决方案和引用链接。
*   **Rapid7**：Metasploit框架背后的公司，其数据库包含丰富的漏洞模块信息。
*   **安全客**：国内知名的安全社区，实时更新最新的漏洞资讯和分析报告。

---

![](img/22e87a87a487476b10e17bbe5b8ae6c8_13.png)

![](img/22e87a87a487476b10e17bbe5b8ae6c8_14.png)

## 第三部分：Linux系统内核漏洞提权 ⚙️

上一节我们介绍了如何寻找利用代码，本节我们聚焦于最常见的提权方式——利用Linux内核漏洞。这类漏洞通常影响广泛，利用方式直接。

### Linux平台提权漏洞集合

![](img/22e87a87a487476b10e17bbe5b8ae6c8_16.png)

![](img/22e87a87a487476b10e17bbe5b8ae6c8_18.png)

安全研究人员整理了历史Linux内核提权漏洞的集合，例如在GitHub上的项目。我们可以根据内核版本号快速查找对应的漏洞和利用代码。

例如，对于一个漏洞，其利用代码通常是一个C语言文件：
```c
// 示例：一个本地提权漏洞的EXP代码片段
#include <stdio.h>
#include <stdlib.h>
int main() {
    // ... 漏洞利用逻辑 ...
    system("/bin/bash -p"); // 获取root shell
    return 0;
}
```
我们需要在目标机器上用 `gcc` 编译后执行：
```bash
gcc exploit.c -o exploit
./exploit
```

![](img/22e87a87a487476b10e17bbe5b8ae6c8_20.png)

![](img/22e87a87a487476b10e17bbe5b8ae6c8_22.png)

![](img/22e87a87a487476b10e17bbe5b8ae6c8_23.png)

### 使用SearchSploit工具

在Kali Linux等渗透测试环境中，可以使用 `searchsploit` 工具快速搜索本地漏洞数据库。

以下是 `searchsploit` 的基本用法：

1.  **更新数据库**：`searchsploit -u`
2.  **搜索漏洞**：`searchsploit linux kernel 4.15 local privilege`
3.  **查看详情**：`searchsploit -p 12345` (其中12345是漏洞ID)

例如，搜索适用于内核版本4.10至5.1.17的本地提权漏洞：
```bash
searchsploit linux kernel 4.15 local privilege
```
该命令会返回匹配的漏洞列表及其本地存储路径，我们可以直接复制利用代码到目标机器进行编译和测试。

![](img/22e87a87a487476b10e17bbe5b8ae6c8_25.png)

![](img/22e87a87a487476b10e17bbe5b8ae6c8_27.png)

![](img/22e87a87a487476b10e17bbe5b8ae6c8_29.png)

---

![](img/22e87a87a487476b10e17bbe5b8ae6c8_31.png)

![](img/22e87a87a487476b10e17bbe5b8ae6c8_33.png)

## 第四部分：Linux环境变量提权与密码哈希破解 🔑

![](img/22e87a87a487476b10e17bbe5b8ae6c8_35.png)

![](img/22e87a87a487476b10e17bbe5b8ae6c8_37.png)

![](img/22e87a87a487476b10e17bbe5b8ae6c8_39.png)

上一节我们深入探讨了内核漏洞提权，本节我们来看看另外两种提权方法：滥用环境变量和破解密码哈希。

![](img/22e87a87a487476b10e17bbe5b8ae6c8_41.png)

![](img/22e87a87a487476b10e17bbe5b8ae6c8_43.png)

![](img/22e87a87a487476b10e17bbe5b8ae6c8_45.png)

### 环境变量提权

Linux中的 `PATH` 环境变量定义了系统查找可执行文件的目录顺序。如果我们将一个包含恶意代码的目录添加到 `PATH` 的前端，并且该目录下有一个与系统命令同名的可执行文件，那么当特权用户（如root）或特权服务执行该命令时，就会运行我们的恶意代码。

基本利用步骤：
1.  编写一个反向Shell或添加用户的C程序，命名为 `ls`、`cat` 等常见命令名。
2.  编译它：`gcc -o ls malicious.c`
3.  将当前目录（或恶意文件所在目录）添加到 `PATH` 的最前面：`export PATH=/tmp:$PATH` (假设恶意文件在 `/tmp`)
4.  等待或诱导特权上下文执行 `ls` 命令。

### 密码哈希破解

![](img/22e87a87a487476b10e17bbe5b8ae6c8_47.png)

如果通过信息收集获取到了 `/etc/shadow` 文件中的密码哈希，我们可以尝试使用工具如 `John the Ripper` 或 `hashcat` 进行破解。

![](img/22e87a87a487476b10e17bbe5b8ae6c8_49.png)

基本流程：
1.  从 `/etc/shadow` 中提取目标用户的哈希值（格式通常为 `$id$salt$hash`）。
2.  使用破解工具和字典进行离线破解。
```bash
# 使用John the Ripper示例
unshadow /etc/passwd /etc/shadow > hashes.db
john --wordlist=/usr/share/wordlists/rockyou.txt hashes.db
```

![](img/22e87a87a487476b10e17bbe5b8ae6c8_51.png)

![](img/22e87a87a487476b10e17bbe5b8ae6c8_53.png)

---

![](img/22e87a87a487476b10e17bbe5b8ae6c8_55.png)

![](img/22e87a87a487476b10e17bbe5b8ae6c8_57.png)

## 第五部分：提权实验 🧪

![](img/22e87a87a487476b10e17bbe5b8ae6c8_59.png)

在本节中，我们将通过两个实验来巩固所学知识。请根据实验环境完成以下任务：

**实验一：内核漏洞提权**
1.  使用信息收集脚本枚举目标机器。
2.  根据内核版本，使用 `searchsploit` 查找可用的本地提权漏洞。
3.  将利用代码上传到目标机器，编译并执行，尝试获取root权限。

![](img/22e87a87a487476b10e17bbe5b8ae6c8_61.png)

![](img/22e87a87a487476b10e17bbe5b8ae6c8_63.png)

**实验二：环境变量滥用提权**
1.  在目标机器上，找到一个以root权限运行的脚本或服务，其中调用了未使用绝对路径的命令（如 `system(“ls”)`）。
2.  编写一个恶意的 `ls` 程序，实现提权（如启动Setuid Shell）。
3.  修改当前用户的 `PATH` 变量，使恶意 `ls` 优先被找到。
4.  触发该脚本或服务的执行，验证是否获得root权限。

---

![](img/22e87a87a487476b10e17bbe5b8ae6c8_65.png)

## 总结 📝

![](img/22e87a87a487476b10e17bbe5b8ae6c8_67.png)

![](img/22e87a87a487476b10e17bbe5b8ae6c8_69.png)

本节课中我们一起学习了Linux系统下的权限提升技术。我们从信息收集开始，了解了自动化脚本的使用；接着学习了如何查找漏洞利用代码；然后重点探讨了利用Linux内核漏洞进行提权的方法，并介绍了 `searchsploit` 工具；最后，我们简要了解了环境变量提权和密码哈希破解的思路。通过课程末尾的实验，希望大家能够将理论应用于实践，深刻理解Linux提权的核心过程。记住，深入理解系统原理和保持对最新漏洞的关注是安全研究的基石。