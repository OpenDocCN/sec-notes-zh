# 数据库提权技术深度剖析教程 🛡️➡️👑

![](img/f37932b51576497bc406dde1ab8f4f24_1.png)

在本节课中，我们将要学习数据库提权背后的核心原理与实战操作。课程内容源自漏洞银行大咖面对面第58期，由Ph0rse带来的技术分享。我们将从MySql入手，逐步深入，剖析多种数据库的提权玄机。

---

## 第一部分：MySql提权基础 🗄️

![](img/f37932b51576497bc406dde1ab8f4f24_3.png)

![](img/f37932b51576497bc406dde1ab8f4f24_5.png)

上一节我们介绍了课程概述，本节中我们来看看MySql数据库的提权基础。MySql因其环境搭建简单、应用广泛，成为我们学习数据库提权的理想起点。

### 1. MOF提权原理与利用

![](img/f37932b51576497bc406dde1ab8f4f24_7.png)

![](img/f37932b51576497bc406dde1ab8f4f24_8.png)

![](img/f37932b51576497bc406dde1ab8f4f24_9.png)

![](img/f37932b51576497bc406dde1ab8f4f24_10.png)

![](img/f37932b51576497bc406dde1ab8f4f24_11.png)

![](img/f37932b51576497bc406dde1ab8f4f24_13.png)

MOF提权利用了Windows低版本系统（如XP、2000、2003）中“Windows管理规范”的设计缺陷。该系统服务会每隔5秒在特定路径（如`C:\Windows\system32\wbem\mof\`）加载并编译.mof文件。

![](img/f37932b51576497bc406dde1ab8f4f24_15.png)

**核心原理**：通过SQL注入等手段，向该目录写入恶意的.mof文件。由于加载该文件的进程（`svchost.exe`）具有`SYSTEM`权限，因此文件中包含的恶意代码也将以`SYSTEM`权限执行。

![](img/f37932b51576497bc406dde1ab8f4f24_17.png)

![](img/f37932b51576497bc406dde1ab8f4f24_18.png)

以下是利用条件列表：
*   操作系统为Windows XP/2000/2003。
*   数据库用户拥有向系统目录写入文件的权限。
*   数据库允许外联，且账号密码已知。

**关键操作**：写入文件需使用`INTO DUMPFILE`而非`INTO OUTFILE`，因为后者会添加换行符等冗余数据，可能破坏二进制文件（如.dll）或脚本的执行。

![](img/f37932b51576497bc406dde1ab8f4f24_20.png)

**防御与清除**：若服务器已被利用，管理员需停止`winmgmt`服务，删除`C:\Windows\system32\wbem\Repository\`目录下的存储库文件，再重启服务。但根本防御在于杜绝SQL注入漏洞。

![](img/f37932b51576497bc406dde1ab8f4f24_22.png)

![](img/f37932b51576497bc406dde1ab8f4f24_23.png)

![](img/f37932b51576497bc406dde1ab8f4f24_25.png)

![](img/f37932b51576497bc406dde1ab8f4f24_26.png)

### 2. UDF提权详解与演示

![](img/f37932b51576497bc406dde1ab8f4f24_28.png)

UDF（User Defined Function，用户自定义函数）提权是更为常用和灵活的方式。它允许用户通过编写动态链接库（DLL）来扩展MySql的功能。

![](img/f37932b51576497bc406dde1ab8f4f24_30.png)

![](img/f37932b51576497bc406dde1ab8f4f24_32.png)

**核心原理**：将包含自定义命令执行函数的DLL文件写入MySql的插件目录，并通过SQL语句创建对应的UDF。调用此UDF时，即可执行系统命令，且命令权限与启动MySql服务的账户权限相同。

利用步骤如下：
1.  **确定插件目录**：执行SQL查询 `SHOW VARIABLES LIKE ‘%plugin%’;`。
2.  **上传DLL文件**：根据MySql版本（<5.0， 5.0-5.1， >5.1）和操作系统位数（32/64位），将对应的DLL文件写入插件目录。
3.  **创建自定义函数**：使用SQL语句 `CREATE FUNCTION function_name RETURNS STRING SONAME ‘udf.dll’;`。若函数已存在，需先执行 `DROP FUNCTION function_name;`。
4.  **执行命令**：调用创建的函数，例如 `SELECT sys_eval(‘whoami’);`。

**代码示例（创建函数）**：
```sql
CREATE FUNCTION cmdshell RETURNS STRING SONAME ‘udf.dll’;
```

**自动化工具**：除了手动操作，可使用 `sqlmap` 的 `--os-shell` 参数或专用工具如 `pythonscript-fuck-mysql` 进行自动化提权。

![](img/f37932b51576497bc406dde1ab8f4f24_34.png)

![](img/f37932b51576497bc406dde1ab8f4f24_36.png)

---

![](img/f37932b51576497bc406dde1ab8f4f24_38.png)

![](img/f37932b51576497bc406dde1ab8f4f24_39.png)

## 第二部分：MySql新型提权与MSSql提权概览 ⚡

![](img/f37932b51576497bc406dde1ab8f4f24_41.png)

![](img/f37932b51576497bc406dde1ab8f4f24_43.png)

上一节我们探讨了传统的MOF和UDF提权，本节中我们来看看更新颖的提权方式以及其他数据库。

### 3. MySql配置文件提权（CVE-2016-6662/6663/6664）

![](img/f37932b51576497bc406dde1ab8f4f24_45.png)

![](img/f37932b51576497bc406dde1ab8f4f24_47.png)

这套组合漏洞影响MySql 5.5, 5.6, 5.7及衍生版本（如MariaDB, Percona Server），在实战中成功率较高。

**核心原理**：利用MySql配置中的不安全设置，通过漏洞绕过权限限制，修改配置文件（如`my.cnf`）。在MySql服务以高权限重启或重载配置时，即可执行恶意代码，实现从`www-data`权限到`mysql`权限，再到`root`权限的跃升。

![](img/f37932b51576497bc406dde1ab8f4f24_49.png)

![](img/f37932b51576497bc406dde1ab8f4f24_51.png)

**利用流程简述**：
1.  通过Webshell上传漏洞利用的C语言源码（exp.c）。
2.  在服务器上编译该源码：`gcc -g -c exp.c` 和 `gcc -g -shared -o exp.so exp.o`。
3.  执行exp，将当前进程权限提升至`mysql`用户。
4.  进一步利用其他exp（如`.sh`脚本），将`mysql`用户权限提升至`root`。

![](img/f37932b51576497bc406dde1ab8f4f24_52.png)

![](img/f37932b51576497bc406dde1ab8f4f24_53.png)

![](img/f37932b51576497bc406dde1ab8f4f24_55.png)

### 4. MSSql提权简介

![](img/f37932b51576497bc406dde1ab8f4f24_57.png)

![](img/f37932b51576497bc406dde1ab8f4f24_59.png)

![](img/f37932b51576497bc406dde1ab8f4f24_60.png)

![](img/f37932b51576497bc406dde1ab8f4f24_61.png)

MSSql（SQL Server）提权通常通过其丰富的“存储过程”实现。存储过程是为完成复杂功能而预编译的SQL语句集合。

![](img/f37932b51576497bc406dde1ab8f4f24_62.png)

![](img/f37932b51576497bc406dde1ab8f4f24_64.png)

![](img/f37932b51576497bc406dde1ab8f4f24_65.png)

![](img/f37932b51576497bc406dde1ab8f4f24_67.png)

**核心原理**：寻找并利用具有高执行权限的存储过程（如`xp_cmdshell`），让其执行我们的恶意命令。其本质与MySql提权一致：让高权限进程执行我们的代码。

![](img/f37932b51576497bc406dde1ab8f4f24_69.png)

![](img/f37932b51576497bc406dde1ab8f4f24_71.png)

**常见提权存储过程**：
*   `xp_cmdshell`：执行系统命令。
*   `sp_oacreate` / `sp_oamethod`：调用OLE对象，执行命令或写入文件。
*   `xp_dirtree` / `xp_subdirs`：读取目录信息，常用于盲注。
*   `xp_regread` / `xp_regwrite`：读写注册表。
*   `sp_add_job` / `sp_add_jobstep`：创建代理作业执行命令。

![](img/f37932b51576497bc406dde1ab8f4f24_73.png)

![](img/f37932b51576497bc406dde1ab8f4f24_75.png)

![](img/f37932b51576497bc406dde1ab8f4f24_77.png)

![](img/f37932b51576497bc406dde1ab8f4f24_78.png)

**利用示例（启用并调用xp_cmdshell）**：
```sql
EXEC sp_configure ‘show advanced options’, 1;
RECONFIGURE;
EXEC sp_configure ‘xp_cmdshell’, 1;
RECONFIGURE;
EXEC xp_cmdshell ‘whoami’;
```

**自动化工具**：推荐使用 `PowerUpSQL` 工具包，它能自动化探测和利用MSSql中的多种配置缺陷与存储过程进行提权。

![](img/f37932b51576497bc406dde1ab8f4f24_80.png)

---

## 第三部分：提权本质与拓展思维 🧠

上一节我们介绍了MSSql的常见提权方式，本节中我们将总结提权的核心思想并拓展攻击面。

### 5. 提权的核心思想与资源

![](img/f37932b51576497bc406dde1ab8f4f24_82.png)

所有数据库提权，乃至系统提权，其**核心思想**都是相同的：**找到一个以较高权限运行的进程或线程，并诱使或利用它来执行我们的恶意代码，从而继承其权限**。

因此，不要将思路局限于某几种数据库的特定方法。真正的渗透测试人员需要：

![](img/f37932b51576497bc406dde1ab8f4f24_84.png)

1.  **结合操作系统漏洞**：例如，在获得Webshell后，可以尝试使用如Dirty Cow、内核提权EXP等系统漏洞直接提权。资源网站如 `Seclists.org` 的 `Windows-Exploit-Suggester` 和 `Linux-Exploit-Suggester` 是很好的EXP来源。
2.  **扩大知识面**：研究Oracle、NoSQL（如Redis、MongoDB）的提权技术。Oracle可通过Java存储过程执行系统命令，Redis可写SSH密钥或计划任务。
3.  **灵活运用**：根据目标环境，混合使用数据库提权、系统提权、应用软件配置错误等多种技术。

![](img/f37932b51576497bc406dde1ab8f4f24_86.png)

**思维导图**：建议参考国外安全研究者整理的提权思维导图，系统化地梳理从信息收集到获取SYSTEM/ROOT权限的各类可能路径。

### 6. 防御建议

防御提权是一个多层次的过程：
*   **前端**：严格过滤用户输入，根本性解决SQL注入、文件上传等漏洞。
*   **服务配置**：数据库服务应以最低必要权限运行（非SYSTEM/ROOT）；禁用不必要的存储过程或功能组件（如`xp_cmdshell`）；严格控制目录写入权限。
*   **安全假设**：采用“假设已被入侵”的防御心态，实施网络分段、权限细分、日志审计和入侵检测系统，增加攻击者横向移动和提权的难度与成本。

---

## 总结 📚

本节课中我们一起深入学习了数据库提权的核心技术。
我们从MySql的**MOF提权**和**UDF提权**入手，理解了通过系统服务缺陷和自定义函数执行命令的原理。
接着，我们探讨了新型的**MySql配置文件提权**（CVE系列），展示了如何通过修改配置实现权限跃迁。
然后，我们简介了**MSSql**如何利用高权限**存储过程**（如`xp_cmdshell`）进行提权。
最后，我们升华了提权的**核心思想**——“借高权进程之手”，并强调需结合操作系统漏洞、拓宽知识面进行灵活攻击，同时从多层面构建防御体系。

![](img/f37932b51576497bc406dde1ab8f4f24_88.png)

![](img/f37932b51576497bc406dde1ab8f4f24_90.png)

希望本教程能为你打开数据库安全研究的大门。请务必在合法授权的环境下进行实践，不断巩固知识，拓展技能。