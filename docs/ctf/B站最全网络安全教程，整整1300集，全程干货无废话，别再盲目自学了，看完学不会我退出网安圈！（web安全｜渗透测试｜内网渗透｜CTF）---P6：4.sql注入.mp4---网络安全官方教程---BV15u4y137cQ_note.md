# 网络安全教程：P6：SQL注入基础与实战

![](img/511593d0cc2c54851ca12baf32923ee9_1.png)

![](img/511593d0cc2c54851ca12baf32923ee9_3.png)

![](img/511593d0cc2c54851ca12baf32923ee9_5.png)

## 概述

![](img/511593d0cc2c54851ca12baf32923ee9_7.png)

![](img/511593d0cc2c54851ca12baf32923ee9_9.png)

在本节课中，我们将要学习SQL注入的基础知识。SQL注入是Web安全中最常见且危害极大的漏洞之一。我们将从数据库基础开始，逐步理解SQL注入的原理、检测方法以及利用手段。课程内容力求简单直白，确保初学者能够看懂。

![](img/511593d0cc2c54851ca12baf32923ee9_11.png)

![](img/511593d0cc2c54851ca12baf32923ee9_13.png)

---

![](img/511593d0cc2c54851ca12baf32923ee9_15.png)

![](img/511593d0cc2c54851ca12baf32923ee9_17.png)

![](img/511593d0cc2c54851ca12baf32923ee9_19.png)

## 第一章：数据库基础与网站结构

![](img/511593d0cc2c54851ca12baf32923ee9_21.png)

![](img/511593d0cc2c54851ca12baf32923ee9_23.png)

![](img/511593d0cc2c54851ca12baf32923ee9_25.png)

![](img/511593d0cc2c54851ca12baf32923ee9_27.png)

![](img/511593d0cc2c54851ca12baf32923ee9_29.png)

![](img/511593d0cc2c54851ca12baf32923ee9_31.png)

![](img/511593d0cc2c54851ca12baf32923ee9_33.png)

在讲解SQL注入之前，我们先了解一下网站的基本结构和数据库的基础知识。

![](img/511593d0cc2c54851ca12baf32923ee9_35.png)

![](img/511593d0cc2c54851ca12baf32923ee9_37.png)

![](img/511593d0cc2c54851ca12baf32923ee9_39.png)

![](img/511593d0cc2c54851ca12baf32923ee9_41.png)

![](img/511593d0cc2c54851ca12baf32923ee9_43.png)

![](img/511593d0cc2c54851ca12baf32923ee9_45.png)

![](img/511593d0cc2c54851ca12baf32923ee9_47.png)

![](img/511593d0cc2c54851ca12baf32923ee9_49.png)

![](img/511593d0cc2c54851ca12baf32923ee9_51.png)

![](img/511593d0cc2c54851ca12baf32923ee9_52.png)

![](img/511593d0cc2c54851ca12baf32923ee9_54.png)

![](img/511593d0cc2c54851ca12baf32923ee9_56.png)

上一节我们介绍了课程概述，本节中我们来看看一个普通网站的结构。

![](img/511593d0cc2c54851ca12baf32923ee9_58.png)

![](img/511593d0cc2c54851ca12baf32923ee9_60.png)

![](img/511593d0cc2c54851ca12baf32923ee9_62.png)

![](img/511593d0cc2c54851ca12baf32923ee9_64.png)

![](img/511593d0cc2c54851ca12baf32923ee9_66.png)

![](img/511593d0cc2c54851ca12baf32923ee9_68.png)

![](img/511593d0cc2c54851ca12baf32923ee9_70.png)

一个典型的网站处理流程如下：
1.  用户（攻击者）向Web服务器提交一个请求。
2.  Web服务器解析该请求，并将其传递给数据库。
3.  数据库处理请求，并将响应返回给Web服务器。
4.  Web服务器将处理后的结果返回给用户。

![](img/511593d0cc2c54851ca12baf32923ee9_72.png)

这个过程可以简单地理解为：**用户 -> Web服务器 -> 数据库 -> Web服务器 -> 用户**。

![](img/511593d0cc2c54851ca12baf32923ee9_74.png)

接下来，我们开始了解数据库。

---

![](img/511593d0cc2c54851ca12baf32923ee9_76.png)

![](img/511593d0cc2c54851ca12baf32923ee9_78.png)

![](img/511593d0cc2c54851ca12baf32923ee9_80.png)

![](img/511593d0cc2c54851ca12baf32923ee9_82.png)

## 什么是数据库？

![](img/511593d0cc2c54851ca12baf32923ee9_84.png)

![](img/511593d0cc2c54851ca12baf32923ee9_86.png)

![](img/511593d0cc2c54851ca12baf32923ee9_88.png)

![](img/511593d0cc2c54851ca12baf32923ee9_90.png)

![](img/511593d0cc2c54851ca12baf32923ee9_92.png)

数据库，简单理解就是一个存放数据的地方。网站的关键数据，如用户信息、文章内容等，都存储在数据库中。当需要查找或调用数据时，程序会从数据库中读取。

![](img/511593d0cc2c54851ca12baf32923ee9_94.png)

![](img/511593d0cc2c54851ca12baf32923ee9_96.png)

![](img/511593d0cc2c54851ca12baf32923ee9_98.png)

![](img/511593d0cc2c54851ca12baf32923ee9_100.png)

![](img/511593d0cc2c54851ca12baf32923ee9_102.png)

![](img/511593d0cc2c54851ca12baf32923ee9_104.png)

---

![](img/511593d0cc2c54851ca12baf32923ee9_106.png)

![](img/511593d0cc2c54851ca12baf32923ee9_108.png)

![](img/511593d0cc2c54851ca12baf32923ee9_110.png)

![](img/511593d0cc2c54851ca12baf32923ee9_112.png)

![](img/511593d0cc2c54851ca12baf32923ee9_114.png)

![](img/511593d0cc2c54851ca12baf32923ee9_116.png)

![](img/511593d0cc2c54851ca12baf32923ee9_118.png)

![](img/511593d0cc2c54851ca12baf32923ee9_120.png)

![](img/511593d0cc2c54851ca12baf32923ee9_122.png)

## SQL是什么？

![](img/511593d0cc2c54851ca12baf32923ee9_124.png)

![](img/511593d0cc2c54851ca12baf32923ee9_126.png)

![](img/511593d0cc2c54851ca12baf32923ee9_128.png)

![](img/511593d0cc2c54851ca12baf32923ee9_130.png)

![](img/511593d0cc2c54851ca12baf32923ee9_132.png)

![](img/511593d0cc2c54851ca12baf32923ee9_134.png)

![](img/511593d0cc2c54851ca12baf32923ee9_136.png)

SQL（Structured Query Language）是用来操作数据库的语言。网站通过编写SQL语句来与数据库进行交互，例如查询、增加、修改或删除数据。

![](img/511593d0cc2c54851ca12baf32923ee9_138.png)

![](img/511593d0cc2c54851ca12baf32923ee9_140.png)

![](img/511593d0cc2c54851ca12baf32923ee9_142.png)

![](img/511593d0cc2c54851ca12baf32923ee9_144.png)

![](img/511593d0cc2c54851ca12baf32923ee9_146.png)

![](img/511593d0cc2c54851ca12baf32923ee9_148.png)

![](img/511593d0cc2c54851ca12baf32923ee9_149.png)

![](img/511593d0cc2c54851ca12baf32923ee9_151.png)

![](img/511593d0cc2c54851ca12baf32923ee9_153.png)

![](img/511593d0cc2c54851ca12baf32923ee9_155.png)

![](img/511593d0cc2c54851ca12baf32923ee9_157.png)

![](img/511593d0cc2c54851ca12baf32923ee9_159.png)

![](img/511593d0cc2c54851ca12baf32923ee9_161.png)

![](img/511593d0cc2c54851ca12baf32923ee9_163.png)

以下是常见的SQL数据库类型：
*   MySQL
*   SQL Server
*   Oracle
*   Access

![](img/511593d0cc2c54851ca12baf32923ee9_165.png)

![](img/511593d0cc2c54851ca12baf32923ee9_167.png)

![](img/511593d0cc2c54851ca12baf32923ee9_169.png)

![](img/511593d0cc2c54851ca12baf32923ee9_171.png)

![](img/511593d0cc2c54851ca12baf32923ee9_173.png)

![](img/511593d0cc2c54851ca12baf32923ee9_175.png)

![](img/511593d0cc2c54851ca12baf32923ee9_177.png)

![](img/511593d0cc2c54851ca12baf32923ee9_179.png)

![](img/511593d0cc2c54851ca12baf32923ee9_180.png)

![](img/511593d0cc2c54851ca12baf32923ee9_182.png)

![](img/511593d0cc2c54851ca12baf32923ee9_184.png)

![](img/511593d0cc2c54851ca12baf32923ee9_186.png)

虽然数据库种类不同，但其操作原理基本相似。本课程将以 **MySQL** 为例进行讲解。

![](img/511593d0cc2c54851ca12baf32923ee9_188.png)

![](img/511593d0cc2c54851ca12baf32923ee9_190.png)

![](img/511593d0cc2c54851ca12baf32923ee9_192.png)

![](img/511593d0cc2c54851ca12baf32923ee9_194.png)

![](img/511593d0cc2c54851ca12baf32923ee9_196.png)

![](img/511593d0cc2c54851ca12baf32923ee9_198.png)

![](img/511593d0cc2c54851ca12baf32923ee9_200.png)

![](img/511593d0cc2c54851ca12baf32923ee9_202.png)

![](img/511593d0cc2c54851ca12baf32923ee9_204.png)

![](img/511593d0cc2c54851ca12baf32923ee9_206.png)

![](img/511593d0cc2c54851ca12baf32923ee9_208.png)

---

![](img/511593d0cc2c54851ca12baf32923ee9_210.png)

![](img/511593d0cc2c54851ca12baf32923ee9_212.png)

![](img/511593d0cc2c54851ca12baf32923ee9_214.png)

![](img/511593d0cc2c54851ca12baf32923ee9_216.png)

![](img/511593d0cc2c54851ca12baf32923ee9_217.png)

![](img/511593d0cc2c54851ca12baf32923ee9_219.png)

## 数据库的结构

理解数据库的结构对于学习SQL注入至关重要。我们可以将其想象为一个多层级的容器。

![](img/511593d0cc2c54851ca12baf32923ee9_221.png)

数据库的结构层级如下：
*   **数据库（Database）**：最大的容器。
*   **表（Table）**：数据库中包含的多个数据表。
*   **字段（Column）**：表中的列，定义了数据的属性（如用户名、密码）。
*   **数据（Data/Row）**：字段中存储的具体值。

![](img/511593d0cc2c54851ca12baf32923ee9_223.png)

例如，一个名为 `users` 的表可能包含 `user_id`、`username`、`password` 等字段，而 `admin` 和其对应的密码就是存储在字段中的具体数据。

![](img/511593d0cc2c54851ca12baf32923ee9_225.png)

![](img/511593d0cc2c54851ca12baf32923ee9_227.png)

![](img/511593d0cc2c54851ca12baf32923ee9_229.png)

![](img/511593d0cc2c54851ca12baf32923ee9_231.png)

---

![](img/511593d0cc2c54851ca12baf32923ee9_233.png)

![](img/511593d0cc2c54851ca12baf32923ee9_235.png)

![](img/511593d0cc2c54851ca12baf32923ee9_237.png)

## MySQL基本操作

![](img/511593d0cc2c54851ca12baf32923ee9_239.png)

![](img/511593d0cc2c54851ca12baf32923ee9_241.png)

![](img/511593d0cc2c54851ca12baf32923ee9_243.png)

![](img/511593d0cc2c54851ca12baf32923ee9_245.png)

![](img/511593d0cc2c54851ca12baf32923ee9_247.png)

![](img/511593d0cc2c54851ca12baf32923ee9_249.png)

![](img/511593d0cc2c54851ca12baf32923ee9_251.png)

![](img/511593d0cc2c54851ca12baf32923ee9_253.png)

为了便于理解SQL注入，我们需要掌握一些简单的SQL语句。重点是理解其作用，而非死记硬背。

![](img/511593d0cc2c54851ca12baf32923ee9_255.png)

![](img/511593d0cc2c54851ca12baf32923ee9_257.png)

![](img/511593d0cc2c54851ca12baf32923ee9_259.png)

![](img/511593d0cc2c54851ca12baf32923ee9_261.png)

![](img/511593d0cc2c54851ca12baf32923ee9_263.png)

![](img/511593d0cc2c54851ca12baf32923ee9_265.png)

![](img/511593d0cc2c54851ca12baf32923ee9_267.png)

![](img/511593d0cc2c54851ca12baf32923ee9_269.png)

以下是操作数据库时最核心的四条语句，即“增删改查”：

![](img/511593d0cc2c54851ca12baf32923ee9_271.png)

![](img/511593d0cc2c54851ca12baf32923ee9_273.png)

![](img/511593d0cc2c54851ca12baf32923ee9_275.png)

![](img/511593d0cc2c54851ca12baf32923ee9_277.png)

![](img/511593d0cc2c54851ca12baf32923ee9_278.png)

![](img/511593d0cc2c54851ca12baf32923ee9_280.png)

![](img/511593d0cc2c54851ca12baf32923ee9_282.png)

![](img/511593d0cc2c54851ca12baf32923ee9_284.png)

### 1. 查询数据（SELECT）
用于从数据库中检索数据。
```sql
SELECT * FROM users;
```
这条语句的意思是：查询 `users` 表中的所有（`*` 代表所有）内容。

### 2. 增加数据（INSERT）
用于向数据库表中插入新的数据行。
```sql
INSERT INTO users (id, username, password) VALUES (10, ‘test‘, ‘pass123‘);
```
这条语句向 `users` 表的 `id`， `username`， `password` 字段插入对应的值。

### 3. 更新数据（UPDATE）
用于修改数据库中已有的数据。
```sql
UPDATE users SET username=‘newname‘, email=‘new@email.com‘ WHERE id=10;
```
这条语句将 `users` 表中 `id` 为 10 的记录的 `username` 和 `email` 字段进行更新。

![](img/511593d0cc2c54851ca12baf32923ee9_286.png)

![](img/511593d0cc2c54851ca12baf32923ee9_287.png)

![](img/511593d0cc2c54851ca12baf32923ee9_289.png)

![](img/511593d0cc2c54851ca12baf32923ee9_291.png)

### 4. 删除数据（DELETE）
用于从数据库表中删除数据行。
```sql
DELETE FROM users WHERE id=10;
```
这条语句将删除 `users` 表中 `id` 为 10 的记录。

![](img/511593d0cc2c54851ca12baf32923ee9_293.png)

![](img/511593d0cc2c54851ca12baf32923ee9_295.png)

![](img/511593d0cc2c54851ca12baf32923ee9_297.png)

![](img/511593d0cc2c54851ca12baf32923ee9_299.png)

![](img/511593d0cc2c54851ca12baf32923ee9_301.png)

![](img/511593d0cc2c54851ca12baf32923ee9_303.png)

![](img/511593d0cc2c54851ca12baf32923ee9_304.png)

![](img/511593d0cc2c54851ca12baf32923ee9_306.png)

![](img/511593d0cc2c54851ca12baf32923ee9_308.png)

![](img/511593d0cc2c54851ca12baf32923ee9_310.png)

**核心概念**：掌握这四条语句是理解SQL注入的基础。

![](img/511593d0cc2c54851ca12baf32923ee9_312.png)

![](img/511593d0cc2c54851ca12baf32923ee9_314.png)

![](img/511593d0cc2c54851ca12baf32923ee9_316.png)

![](img/511593d0cc2c54851ca12baf32923ee9_318.png)

![](img/511593d0cc2c54851ca12baf32923ee9_320.png)

![](img/511593d0cc2c54851ca12baf32923ee9_322.png)

---

![](img/511593d0cc2c54851ca12baf32923ee9_324.png)

![](img/511593d0cc2c54851ca12baf32923ee9_326.png)

![](img/511593d0cc2c54851ca12baf32923ee9_328.png)

![](img/511593d0cc2c54851ca12baf32923ee9_330.png)

## 联合查询（UNION）

![](img/511593d0cc2c54851ca12baf32923ee9_332.png)

![](img/511593d0cc2c54851ca12baf32923ee9_334.png)

![](img/511593d0cc2c54851ca12baf32923ee9_336.png)

联合查询是SQL注入中获取数据的关键技术之一。`UNION` 操作符用于合并两个或多个 `SELECT` 语句的结果集。

![](img/511593d0cc2c54851ca12baf32923ee9_338.png)

![](img/511593d0cc2c54851ca12baf32923ee9_340.png)

![](img/511593d0cc2c54851ca12baf32923ee9_342.png)

![](img/511593d0cc2c54851ca12baf32923ee9_344.png)

![](img/511593d0cc2c54851ca12baf32923ee9_346.png)

![](img/511593d0cc2c54851ca12baf32923ee9_348.png)

![](img/511593d0cc2c54851ca12baf32923ee9_350.png)

例如：
```sql
SELECT * FROM users WHERE username=‘admin‘ UNION SELECT 1,2,3;
```
这条语句会先查询 `username` 为 `admin` 的用户，然后在其结果下方追加一行 `1,2,3` 的数据。

![](img/511593d0cc2c54851ca12baf32923ee9_352.png)

![](img/511593d0cc2c54851ca12baf32923ee9_354.png)

![](img/511593d0cc2c54851ca12baf32923ee9_355.png)

![](img/511593d0cc2c54851ca12baf32923ee9_357.png)

![](img/511593d0cc2c54851ca12baf32923ee9_359.png)

在SQL注入中，我们可以利用 `UNION` 来执行我们自定义的查询语句，从而获取数据库中的敏感信息。

---

![](img/511593d0cc2c54851ca12baf32923ee9_361.png)

![](img/511593d0cc2c54851ca12baf32923ee9_362.png)

![](img/511593d0cc2c54851ca12baf32923ee9_364.png)

![](img/511593d0cc2c54851ca12baf32923ee9_366.png)

## 判断SQL注入点

![](img/511593d0cc2c54851ca12baf32923ee9_368.png)

![](img/511593d0cc2c54851ca12baf32923ee9_370.png)

![](img/511593d0cc2c54851ca12baf32923ee9_372.png)

![](img/511593d0cc2c54851ca12baf32923ee9_374.png)

![](img/511593d0cc2c54851ca12baf32923ee9_376.png)

![](img/511593d0cc2c54851ca12baf32923ee9_378.png)

![](img/511593d0cc2c54851ca12baf32923ee9_380.png)

![](img/511593d0cc2c54851ca12baf32923ee9_382.png)

![](img/511593d0cc2c54851ca12baf32923ee9_384.png)

上一节我们介绍了SQL的基本操作，本节中我们来看看如何发现一个网站是否存在SQL注入漏洞。

![](img/511593d0cc2c54851ca12baf32923ee9_386.png)

![](img/511593d0cc2c54851ca12baf32923ee9_388.png)

![](img/511593d0cc2c54851ca12baf32923ee9_390.png)

![](img/511593d0cc2c54851ca12baf32923ee9_392.png)

![](img/511593d0cc2c54851ca12baf32923ee9_394.png)

![](img/511593d0cc2c54851ca12baf32923ee9_396.png)

![](img/511593d0cc2c54851ca12baf32923ee9_398.png)

检测SQL注入最基本的方法是在可能的输入点（如URL参数、表单字段）尝试插入特殊字符，观察服务器的反应。

![](img/511593d0cc2c54851ca12baf32923ee9_400.png)

![](img/511593d0cc2c54851ca12baf32923ee9_402.png)

![](img/511593d0cc2c54851ca12baf32923ee9_404.png)

![](img/511593d0cc2c54851ca12baf32923ee9_406.png)

最常见的测试方法是输入一个**单引号（‘）**。
*   如果页面返回数据库错误信息，则很可能存在SQL注入漏洞。因为单引号破坏了原SQL语句的语法结构。
*   如果输入单引号后报错，再输入一个单引号页面又恢复正常，则进一步确认存在字符型注入。

另一种测试方法是使用**逻辑判断**，例如：
*   输入 `and 1=1`，页面正常显示。
*   输入 `and 1=2`，页面显示异常或为空。
这是因为 `1=1` 永远为真，而 `1=2` 永远为假，影响了SQL语句的执行逻辑。

![](img/511593d0cc2c54851ca12baf32923ee9_408.png)

---

![](img/511593d0cc2c54851ca12baf32923ee9_410.png)

![](img/511593d0cc2c54851ca12baf32923ee9_412.png)

## 注释符与常用函数

在构造SQL注入语句时，注释符用于注释掉原SQL语句中后续不必要的部分，使我们的注入语句能正确执行。

MySQL中常用的注释符有：
*   `#`
*   `-- ` （注意后面有一个空格）
*   `/* */`

![](img/511593d0cc2c54851ca12baf32923ee9_414.png)

![](img/511593d0cc2c54851ca12baf32923ee9_416.png)

![](img/511593d0cc2c54851ca12baf32923ee9_418.png)

![](img/511593d0cc2c54851ca12baf32923ee9_420.png)

![](img/511593d0cc2c54851ca12baf32923ee9_422.png)

![](img/511593d0cc2c54851ca12baf32923ee9_424.png)

![](img/511593d0cc2c54851ca12baf32923ee9_426.png)

![](img/511593d0cc2c54851ca12baf32923ee9_428.png)

![](img/511593d0cc2c54851ca12baf32923ee9_430.png)

![](img/511593d0cc2c54851ca12baf32923ee9_432.png)

![](img/511593d0cc2c54851ca12baf32923ee9_434.png)

![](img/511593d0cc2c54851ca12baf32923ee9_435.png)

![](img/511593d0cc2c54851ca12baf32923ee9_437.png)

![](img/511593d0cc2c54851ca12baf32923ee9_439.png)

此外，需要了解一些常用函数，它们在注入时用于截取、连接信息：
*   `concat()`：连接字符串。
*   `group_concat()`：将分组中的多个值连接成一个字符串。
*   `substr()` / `mid()`：截取字符串。
*   `length()`：返回字符串长度。
*   `database()`：返回当前数据库名。
*   `version()`：返回数据库版本。

---

## 系统表：information_schema

在MySQL中，`information_schema` 是一个特殊的数据库，它保存了关于MySQL服务器所维护的所有其他数据库的信息（元数据）。我们可以通过查询这个数据库来获取其他数据库的表名、列名等信息。

![](img/511593d0cc2c54851ca12baf32923ee9_441.png)

![](img/511593d0cc2c54851ca12baf32923ee9_442.png)

![](img/511593d0cc2c54851ca12baf32923ee9_444.png)

![](img/511593d0cc2c54851ca12baf32923ee9_446.png)

例如，查询当前数据库的所有表名：
```sql
SELECT table_name FROM information_schema.tables WHERE table_schema = database();
```
这条语句从 `information_schema.tables` 表中查询当前数据库（`database()`）的所有表名（`table_name`）。

![](img/511593d0cc2c54851ca12baf32923ee9_448.png)

![](img/511593d0cc2c54851ca12baf32923ee9_450.png)

![](img/511593d0cc2c54851ca12baf32923ee9_451.png)

![](img/511593d0cc2c54851ca12baf32923ee9_453.png)

![](img/511593d0cc2c54851ca12baf32923ee9_455.png)

![](img/511593d0cc2c54851ca12baf32923ee9_457.png)

![](img/511593d0cc2c54851ca12baf32923ee9_458.png)

---

![](img/511593d0cc2c54851ca12baf32923ee9_460.png)

![](img/511593d0cc2c54851ca12baf32923ee9_462.png)

![](img/511593d0cc2c54851ca12baf32923ee9_464.png)

![](img/511593d0cc2c54851ca12baf32923ee9_466.png)

![](img/511593d0cc2c54851ca12baf32923ee9_468.png)

![](img/511593d0cc2c54851ca12baf32923ee9_470.png)

![](img/511593d0cc2c54851ca12baf32923ee9_472.png)

## SQL注入利用流程

![](img/511593d0cc2c54851ca12baf32923ee9_474.png)

![](img/511593d0cc2c54851ca12baf32923ee9_476.png)

理解了以上基础后，一个典型的手工SQL注入利用流程如下：

![](img/511593d0cc2c54851ca12baf32923ee9_478.png)

![](img/511593d0cc2c54851ca12baf32923ee9_480.png)

1.  **判断注入点**：使用单引号、`and 1=1`、`and 1=2`等方法确认漏洞存在及注入类型（数字型/字符型）。
2.  **判断字段数**：使用 `ORDER BY` 子句。`ORDER BY 3` 正常而 `ORDER BY 4` 错误，说明当前查询的字段数为3。
3.  **确定回显位**：使用 `UNION SELECT` 联合查询，如 `UNION SELECT 1,2,3`，观察页面中哪个位置显示了数字，这些位置就是可以回显查询结果的位置。
4.  **获取数据库信息**：在回显位替换为想要查询的信息。例如：
    ```sql
    UNION SELECT 1, database(), version()
    ```
    可以获取当前数据库名和版本。
5.  **获取表名**：查询 `information_schema.tables` 来获取表名。
    ```sql
    UNION SELECT 1, group_concat(table_name),3 FROM information_schema.tables WHERE table_schema=database()
    ```
6.  **获取字段名**：查询 `information_schema.columns` 来获取指定表的字段名。
    ```sql
    UNION SELECT 1, group_concat(column_name),3 FROM information_schema.columns WHERE table_name=‘users‘
    ```
7.  **获取数据**：最后，从目标表中提取数据。
    ```sql
    UNION SELECT 1, username, password FROM users
    ```

---

![](img/511593d0cc2c54851ca12baf32923ee9_482.png)

![](img/511593d0cc2c54851ca12baf32923ee9_484.png)

![](img/511593d0cc2c54851ca12baf32923ee9_486.png)

![](img/511593d0cc2c54851ca12baf32923ee9_488.png)

## 总结

![](img/511593d0cc2c54851ca12baf32923ee9_490.png)

![](img/511593d0cc2c54851ca12baf32923ee9_492.png)

本节课我们一起学习了SQL注入的基础知识。我们从网站结构和数据库基础讲起，理解了SQL语言的核心操作“增删改查”。我们重点探讨了如何判断SQL注入点，并学习了联合查询、注释符、系统表等关键概念。最后，我们梳理了一个完整的手工SQL注入利用流程：从判断注入点到最终获取敏感数据。

![](img/511593d0cc2c54851ca12baf32923ee9_494.png)

![](img/511593d0cc2c54851ca12baf32923ee9_496.png)

![](img/511593d0cc2c54851ca12baf32923ee9_498.png)

![](img/511593d0cc2c54851ca12baf32923ee9_500.png)

![](img/511593d0cc2c54851ca12baf32923ee9_502.png)

SQL注入的原理在于程序将用户输入的数据直接拼接到了SQL语句中并执行，导致攻击者可以构造恶意输入来操纵数据库查询。掌握这些基本原理是进一步学习各类SQL注入变种（如报错注入、盲注）和自动化工具（如SQLmap）的基础。请务必理解每一步的含义，而不仅仅是记住命令。