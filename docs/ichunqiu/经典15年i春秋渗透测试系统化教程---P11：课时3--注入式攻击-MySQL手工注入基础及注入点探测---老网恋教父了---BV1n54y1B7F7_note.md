# 经典15年i春秋渗透测试系统化教程 - P11：课时3 注入式攻击-MySQL手工注入基础及注入点探测 🔍

![](img/d676bcc3f188c51d567e3d11a27e92d8_1.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_2.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_4.png)

在本节课中，我们将要学习MySQL数据库手工注入的基础知识，并详细讲解如何探测一个网站是否存在SQL注入点。MySQL是Web应用程序中广泛使用的数据库，理解其注入原理对于渗透测试至关重要。

## 概述 📋

![](img/d676bcc3f188c51d567e3d11a27e92d8_6.png)

MySQL数据库在Web应用程序中应用非常广泛，无论是中小型企业还是大型企业。如果在编写Web程序时，对用户提交的参数未进行过滤或过滤不严，就会导致SQL注入攻击。本节课将重点讲解MySQL 5.0及以上版本的注入攻击方法。

![](img/d676bcc3f188c51d567e3d11a27e92d8_8.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_10.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_12.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_14.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_16.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_18.png)

## MySQL注入背景

![](img/d676bcc3f188c51d567e3d11a27e92d8_20.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_22.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_23.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_25.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_27.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_29.png)

MySQL数据库通常与PHP网页程序结合搭建网站。许多主流网站，如新浪、网易等，都采用PHP + MySQL的模式。因此，学习MySQL注入是掌握SQL注入攻击的重要环节。

![](img/d676bcc3f188c51d567e3d11a27e92d8_31.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_33.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_34.png)

MySQL版本主要分为两类：
*   **MySQL 5.0及以上版本**：新增了`information_schema`数据库，其中存储了所有数据库、表、列的信息，使得注入攻击（如爆库、爆表、爆字段）变得相对简单。
*   **MySQL 5.0以下版本**：不支持`information_schema`，注入时通常需要采用暴力猜解的方式，类似于Access数据库的注入。

![](img/d676bcc3f188c51d567e3d11a27e92d8_36.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_38.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_40.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_42.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_43.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_45.png)

**注意**：PHP配置文件`php.ini`中的`magic_quotes_gpc`参数如果设置为`On`，会对单引号等特殊字符进行转义（如`'`变为`\'`），增加注入难度。本节课主要针对MySQL 5.0及以上版本进行讲解。

![](img/d676bcc3f188c51d567e3d11a27e92d8_47.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_49.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_51.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_53.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_55.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_57.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_59.png)

## 注入点探测方法 🎯

![](img/d676bcc3f188c51d567e3d11a27e92d8_61.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_63.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_65.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_67.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_69.png)

探测一个URL参数是否存在SQL注入漏洞，是手工注入的第一步。

![](img/d676bcc3f188c51d567e3d11a27e92d8_71.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_73.png)

以下是基本的探测方法：
1.  **单引号法**：在参数值后添加一个单引号`‘`，观察页面是否返回数据库错误信息。
2.  **逻辑判断法**：通过构造`and 1=1`和`and 1=2`的逻辑判断，观察页面返回内容是否不同。

![](img/d676bcc3f188c51d567e3d11a27e92d8_75.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_77.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_79.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_81.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_83.png)

**示例**：
假设目标URL为：`http://target.com/page.php?id=1`
*   测试：`http://target.com/page.php?id=1‘` （观察是否报错）
*   测试：`http://target.com/page.php?id=1 and 1=1` （正常页面）
*   测试：`http://target.com/page.php?id=1 and 1=2` （错误或内容缺失的页面）

![](img/d676bcc3f188c51d567e3d11a27e92d8_85.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_87.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_89.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_90.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_92.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_94.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_96.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_98.png)

如果`and 1=1`返回正常页面，而`and 1=2`返回异常页面，则基本可以判断该参数存在注入点。

![](img/d676bcc3f188c51d567e3d11a27e92d8_100.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_102.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_104.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_105.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_107.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_108.png)

## 判断注入点类型 🔧

![](img/d676bcc3f188c51d567e3d11a27e92d8_110.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_111.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_113.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_115.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_117.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_119.png)

发现注入点后，需要判断其类型，因为不同类型的注入点，构造注入语句的方式不同。主要分为三类：数字型、字符型和搜索型。

![](img/d676bcc3f188c51d567e3d11a27e92d8_121.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_123.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_124.png)

### 1. 数字型注入
数字型注入点的SQL语句原型通常如下，参数值**没有**被引号包围：
```sql
SELECT * FROM users WHERE id = $id
```
在这种情况下，`$id`直接代入查询。注入时无需考虑闭合引号，可以直接拼接SQL语句。
*   **判断**：参数后直接加`and 1=1`和`and 1=2`，页面反应不同。
*   **注入示例**：`id=1 and 1=1`

![](img/d676bcc3f188c51d567e3d11a27e92d8_126.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_128.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_130.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_132.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_134.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_135.png)

### 2. 字符型注入
字符型注入点的SQL语句原型中，参数值**被单引号**包围：
```sql
SELECT * FROM users WHERE username = ‘$name‘
```
在这种情况下，`$name`被包裹在引号内。注入时，**必须首先闭合前面的单引号**，才能让后续的SQL语句生效，并且通常需要注释掉原语句后面的部分。
*   **判断**：参数后加单引号`‘`会引发错误。需要构造如`‘ and ‘1‘=‘1`和`‘ and ‘1‘=‘2`进行测试。
*   **注入示例**：`name=admin‘ and ‘1‘=‘1`

![](img/d676bcc3f188c51d567e3d11a27e92d8_137.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_139.png)

### 3. 搜索型注入
搜索型注入通常出现在搜索功能中，使用`LIKE`关键字，参数值**被百分号和单引号**包围：
```sql
SELECT * FROM users WHERE password LIKE ‘%$key%‘
```
注入时，需要**闭合前面的`%‘`**，并注释掉后面的`%‘`。
*   **判断与注入示例**：构造如`key=test%‘ and ‘1‘=‘1`进行测试。

**核心要点**：分析源代码中的SQL语句结构，确定参数被何种符号包围，并在注入时正确闭合它们。

![](img/d676bcc3f188c51d567e3d11a27e92d8_141.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_143.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_145.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_147.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_149.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_151.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_153.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_155.png)

## 判断注入点提交方式 📤

![](img/d676bcc3f188c51d567e3d11a27e92d8_157.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_159.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_161.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_163.png)

注入点的参数可以通过不同的HTTP方法提交，主要分为三种：GET、POST和Cookie。探测方式有所不同。

![](img/d676bcc3f188c51d567e3d11a27e92d8_165.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_166.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_168.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_169.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_171.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_172.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_174.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_176.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_178.png)

以下是三种提交方式的判断与测试方法：
1.  **GET注入**：参数直接出现在URL中（如`?id=1`）。这是最常见的形式，直接在浏览器地址栏修改URL进行测试即可。
2.  **POST注入**：参数通过表单正文提交，不会显示在URL中。常见于登录框、搜索框等。需要使用抓包工具（如Burp Suite）拦截请求，在POST数据部分修改参数进行测试。
3.  **Cookie注入**：参数存放在Cookie中发送。同样需要使用抓包工具拦截请求，修改Cookie中的值进行测试。

![](img/d676bcc3f188c51d567e3d11a27e92d8_180.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_182.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_184.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_186.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_188.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_190.png)

**技巧**：有时，即使后端程序主要接收POST请求，也可能同时处理GET请求。可以尝试将POST数据以`?参数=值`的形式附加在URL后进行测试，但这取决于后端代码的具体实现。

![](img/d676bcc3f188c51d567e3d11a27e92d8_192.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_194.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_195.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_197.png)

## 总结 📝

![](img/d676bcc3f188c51d567e3d11a27e92d8_199.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_201.png)

本节课我们一起学习了MySQL手工注入的基础核心步骤。
1.  **探测注入点**：使用单引号法和逻辑判断法（`and 1=1` / `and 1=2`）初步判断是否存在SQL注入漏洞。
2.  **判断注入类型**：通过分析或猜测SQL语句结构，确定是数字型、字符型还是搜索型注入，这对于后续正确构造注入语句至关重要。
3.  **判断提交方式**：识别参数是通过GET、POST还是Cookie提交，并选择相应的工具和方法进行注入测试。

![](img/d676bcc3f188c51d567e3d11a27e92d8_203.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_205.png)

![](img/d676bcc3f188c51d567e3d11a27e92d8_207.png)

理解并掌握这三个步骤，是进行有效手工SQL注入的前提。在后续课程中，我们将在此基础上，学习如何利用注入点获取数据库信息、表数据乃至系统权限。