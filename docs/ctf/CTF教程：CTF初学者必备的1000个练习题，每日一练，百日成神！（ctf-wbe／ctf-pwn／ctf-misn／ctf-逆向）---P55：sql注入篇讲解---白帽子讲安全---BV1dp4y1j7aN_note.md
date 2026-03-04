# CTF教程：P55：SQL注入篇讲解 🎯

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_1.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_3.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_5.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_7.png)

## 概述
在本节课中，我们将要学习SQL注入的基础知识。我们将从理解网站与数据库的交互原理开始，逐步深入到SQL语句的核心操作，并最终掌握SQL注入漏洞的产生原理和利用方法。课程内容力求简单直白，让初学者能够轻松理解。

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_9.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_11.png)

---

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_13.png)

## 第一章：数据库基础与网站交互结构

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_15.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_17.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_19.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_21.png)

上一节我们介绍了课程的整体安排，本节中我们来看看一个网站与数据库交互的基本结构。

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_23.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_25.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_27.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_29.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_31.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_33.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_35.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_37.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_39.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_41.png)

一个典型的网站处理用户请求的过程如下：

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_43.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_45.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_47.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_49.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_51.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_52.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_54.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_56.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_58.png)

1.  用户通过浏览器向Web服务器提交一个请求。
2.  Web服务器解析这个请求，并将其传递给后端的数据库。
3.  数据库接收到请求后，执行相应的查询操作。
4.  数据库将查询结果返回给Web服务器。
5.  Web服务器对结果进行处理后，再返回给用户的浏览器。

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_60.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_62.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_64.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_66.png)

这个过程可以简单地理解为：**用户 -> Web服务器 -> 数据库 -> Web服务器 -> 用户**。

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_68.png)

---

## 第二章：什么是数据库？

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_70.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_72.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_74.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_76.png)

上一节我们介绍了网站的交互流程，本节中我们来具体看看其中的核心组件——数据库。

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_78.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_80.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_82.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_84.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_86.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_88.png)

数据库，简单理解就是一个存放数据的地方。网站的关键数据，如用户信息、文章内容等，通常都存储在数据库中。当需要查找或使用这些数据时，程序就从数据库中调用。

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_90.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_92.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_94.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_96.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_98.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_100.png)

常见的数据库有：
*   MySQL
*   SQL Server
*   Oracle
*   Access

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_102.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_104.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_106.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_108.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_110.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_112.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_114.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_116.png)

**注意**：所有数据库的操作原理基本相通。本课程将以 **MySQL** 为例进行讲解。

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_118.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_120.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_122.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_124.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_126.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_128.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_130.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_132.png)

---

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_134.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_136.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_138.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_140.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_141.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_143.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_145.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_147.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_149.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_151.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_153.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_155.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_157.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_159.png)

## 第三章：数据库的结构

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_160.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_162.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_164.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_166.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_168.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_170.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_171.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_173.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_175.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_177.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_179.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_181.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_183.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_185.png)

理解了数据库是什么之后，我们来看看数据在数据库内部是如何组织的。

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_187.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_189.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_191.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_193.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_195.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_197.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_199.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_201.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_203.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_205.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_207.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_208.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_210.png)

数据库的结构可以形象地理解为：
*   **数据库** 是一个大的“仓库”。
*   **表** 是仓库里的“货架”。
*   **字段** 是货架上的“标签”（用于定义存储什么类型的数据，如用户名、密码）。
*   **数据** 就是货架上按照标签存放的“货物”。

**核心结构关系**：**数据库 -> 表 -> 字段 -> 数据**。

---

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_212.png)

## 第四章：SQL——操作数据库的语言

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_214.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_216.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_218.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_220.png)

知道了数据库的结构，我们如何与它沟通呢？这就需要用到SQL。

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_222.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_224.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_226.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_228.png)

SQL（Structured Query Language）是用来管理和操作数据库的标准语言。无论是MySQL、SQL Server还是Oracle，都通过SQL语句来执行查询、更新、删除等操作。

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_230.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_232.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_234.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_236.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_238.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_240.png)

以下是SQL中最核心的四种操作，称为“增删改查”：

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_242.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_244.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_246.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_248.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_250.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_252.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_254.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_256.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_258.png)

1.  **查询数据**
    ```sql
    SELECT * FROM users;
    ```
    （`*` 表示所有，这条语句的意思是：从 `users` 表中查询所有数据。）

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_260.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_262.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_264.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_266.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_268.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_270.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_271.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_273.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_275.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_277.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_279.png)

2.  **增加数据**
    ```sql
    INSERT INTO users (id, username, password) VALUES (10, ‘admin’, ‘123456’);
    ```
    （向 `users` 表的指定字段插入一条新数据。）

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_281.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_283.png)

3.  **更新数据**
    ```sql
    UPDATE users SET username=‘new_admin’, email=‘new@example.com’ WHERE id=10;
    ```
    （更新 `users` 表中 `id` 为10的记录的 `username` 和 `email` 字段。）

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_285.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_287.png)

4.  **删除数据**
    ```sql
    DELETE FROM users WHERE id=10;
    ```
    （从 `users` 表中删除 `id` 为10的记录。）

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_289.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_291.png)

**重点**：理解这四条语句的含义是学习SQL注入的关键。

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_293.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_295.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_297.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_299.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_301.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_303.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_305.png)

---

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_307.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_309.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_311.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_313.png)

## 第五章：SQL注入漏洞原理

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_315.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_317.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_319.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_321.png)

上一节我们掌握了操作数据库的核心语句，本节中我们来看看这些语句如何被恶意利用，即SQL注入的原理。

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_323.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_325.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_327.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_329.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_331.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_333.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_335.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_337.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_339.png)

回顾网站的处理流程：用户输入 -> 拼接成SQL语句 -> 数据库执行。如果网站在将用户输入拼接到SQL语句前没有进行严格的过滤，就会产生漏洞。

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_341.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_343.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_345.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_347.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_349.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_351.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_353.png)

**漏洞产生模拟**：
假设网站查询用户信息的原始语句是：
```sql
SELECT * FROM articles WHERE id = 用户输入的值;
```
正常情况，用户输入 `1`，语句变为：
```sql
SELECT * FROM articles WHERE id = 1;
```
但如果用户输入 `1‘ UNION SELECT database() — `，语句就变成了：
```sql
SELECT * FROM articles WHERE id = 1‘ UNION SELECT database() — ‘;
```
由于 `—` 在SQL中是注释符，后面的内容会被忽略。最终执行的语句是：
```sql
SELECT * FROM articles WHERE id = 1‘ UNION SELECT database();
```
这样，攻击者自己构造的 `SELECT database()` 语句也被执行，从而泄露了数据库名。

**核心原理**：**将用户输入的数据，当作了SQL代码来执行**。

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_355.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_356.png)

---

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_358.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_360.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_362.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_364.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_366.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_368.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_370.png)

## 第六章：如何发现SQL注入点

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_372.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_374.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_376.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_378.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_380.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_382.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_384.png)

理解了原理，我们如何在实践中发现SQL注入漏洞呢？

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_386.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_388.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_390.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_392.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_394.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_396.png)

最常用的方法是插入“测试字符”，观察网站的响应是否异常。

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_398.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_400.png)

**探测方法**：
1.  在可能的输入点（如URL参数、登录框）尝试输入一个**单引号** `‘`。
2.  如果页面返回了数据库错误信息（如包含 `You have an error in your SQL syntax`），则很可能存在注入点。
3.  为了进一步确认，可以再输入 `‘‘`（两个单引号）。如果页面恢复正常，则基本可以确定存在字符型注入漏洞。

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_402.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_404.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_406.png)

**原理**：单引号在SQL中用于包裹字符串。额外插入的单引号会破坏SQL语句的语法结构，导致数据库报错。如果程序将报错信息显示给用户，我们就发现了漏洞。

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_408.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_410.png)

---

## 第七章：SQL注入的利用流程

发现注入点后，接下来我们学习如何系统地利用它获取数据。这是一个标准化的流程。

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_412.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_414.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_416.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_418.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_420.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_422.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_424.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_426.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_428.png)

以下是手工利用SQL注入的典型步骤：

1.  **判断注入类型**
    *   使用 `and 1=1` 和 `and 1=2` 测试。
    *   如果 `and 1=1` 页面正常，`and 1=2` 页面异常，则很可能是数字型注入。
    *   如果都需要用引号闭合，则是字符型注入。

2.  **确定字段数**
    *   使用 `ORDER BY` 语句。
    *   例如：`ORDER BY 3` 正常，`ORDER BY 4` 报错，说明当前查询结果有3列。

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_430.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_432.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_434.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_436.png)

3.  **获取数据库信息**
    *   使用联合查询 `UNION SELECT`。
    *   例如：`UNION SELECT 1, database(), 3` 可以获取当前数据库名。

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_438.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_439.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_441.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_443.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_445.png)

4.  **获取数据库中的表名**
    *   MySQL中，有一个名为 `information_schema.tables` 的系统表，存储了所有表的信息。
    *   使用语句：`UNION SELECT 1, group_concat(table_name), 3 FROM information_schema.tables WHERE table_schema = database()`

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_447.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_449.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_451.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_453.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_455.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_457.png)

5.  **获取表中的字段名**
    *   同样查询 `information_schema.columns` 表。
    *   使用语句：`UNION SELECT 1, group_concat(column_name), 3 FROM information_schema.columns WHERE table_name = ‘users‘`

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_459.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_461.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_463.png)

6.  **获取数据**
    *   最后，直接查询目标表中的数据。
    *   例如：`UNION SELECT 1, username, password FROM users`

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_465.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_467.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_469.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_471.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_473.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_475.png)

**关键表**：`information_schema` 是MySQL的信息数据库，其中 `tables` 和 `columns` 表是注入时查询表名和列名的关键。

---

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_477.png)

## 总结

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_479.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_481.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_483.png)

本节课中我们一起学习了SQL注入的基础知识。我们从网站与数据库的交互模型讲起，认识了数据库的结构和操作它的SQL语言。我们深入剖析了SQL注入漏洞产生的根本原因：**程序未过滤用户输入，导致其被拼接为SQL代码执行**。接着，我们学习了如何通过插入单引号等测试字符来发现注入点。最后，我们掌握了一个系统化的手工注入利用流程，从判断类型、确定字段数，到一步步获取数据库名、表名、列名乃至最终的数据。

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_485.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_487.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_489.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_491.png)

![](img/f03aea37cb6c4009c7ee2a46693d2c8a_493.png)

SQL注入是Web安全中最经典也最危险的漏洞之一。理解本节课的内容，是成为一名安全研究员的重要基石。请务必动手实践，加深理解。