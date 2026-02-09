# 经典15年i春秋渗透测试系统化教程 - P12：课时4 注入式攻击-MySQL手工注入一个站 🔍

![](img/6c01fa539fb0b4319e3cfb3a61c61575_1.png)

在本节课中，我们将学习MySQL手工注入的完整流程。我们将从一个已知的注入点出发，逐步探测数据库结构，最终获取管理员账号和密码，并成功登录网站。整个过程将涵盖注入点确认、字段数判断、联合查询获取关键信息以及数据提取等核心步骤。

---

## 1. 注入点确认与类型判断 🔎

上一节我们介绍了如何通过单引号或`and`语句判断注入点。检测到注入点后，我们需要判断注入类型，例如是数字型、字符型还是搜索型注入。同时，还需确定注入点的提交方式是`GET`、`POST`还是`COOKIE`。

以下是判断字符型注入的典型方式：
```sql
?id=1' and '1'='1
```

---

## 2. 使用ORDER BY判断字段数 📊

确定注入类型后，下一步是使用`ORDER BY`语句查询当前表的字段数（列数）。`ORDER BY`语句用于对查询结果进行排序。通过递增列数进行测试，当指定的列数超出实际范围时，页面会返回错误。

![](img/6c01fa539fb0b4319e3cfb3a61c61575_3.png)

以下是测试字段数的过程：
1.  输入 `?id=1' order by 5 --+`，页面报错，说明不存在第5列。
2.  输入 `?id=1' order by 3 --+`，页面报错，说明不存在第3列。
3.  输入 `?id=1' order by 2 --+`，页面正常显示，说明该表共有2个字段。

![](img/6c01fa539fb0b4319e3cfb3a61c61575_5.png)

**核心要点**：对于字符型注入，必须用单引号闭合前端的查询语句，并用注释符`--+`注释掉后续代码，避免语法错误。

---

## 3. 联合查询获取数据库信息 💡

当我们确定了字段数为2后，就可以使用`UNION SELECT`联合查询来获取当前数据库的关键信息。

以下是查询当前数据库名和用户的语句：
```sql
?id=1' union select database(), user() --+
```
执行后，页面会返回当前数据库名（如`dvwa`）和数据库用户（如`root@localhost`）。

查询数据库版本同样重要，因为MySQL 5.0以上版本包含一个名为`information_schema`的系统数据库，它存储了所有数据库的元数据（库名、表名、列名）。
```sql
?id=1' union select version(), @@version_compile_os --+
```
此语句可以查询数据库版本和操作系统信息。

---

## 4. 查询数据库中的表名 📑

获取数据库名（例如`dvwa`）后，我们可以从`information_schema.tables`表中查询该数据库下所有的表名。

以下是查询`dvwa`数据库所有表名的语句。注意，库名需要转换为十六进制格式（`dvwa`的十六进制是`0x64767761`）以绕过可能的过滤。
```sql
?id=1' union select 1, group_concat(table_name) from information_schema.tables where table_schema=0x64767761 --+
```
`GROUP_CONCAT()`函数会将所有符合条件的表名连接成一个字符串返回。执行后，我们可能会得到类似`guestbook,users`的结果，表明`dvwa`数据库中有`guestbook`和`users`两张表。通常，`users`表更可能存储用户凭证。

---

## 5. 查询表中的列名 🔑

确定了目标表（例如`users`）后，下一步是从`information_schema.columns`表中查询该表的所有列名。

以下是查询`users`表所有列名的语句。同样，表名需要转换为十六进制（`users`的十六进制是`0x7573657273`）。
```sql
?id=1' union select 1, group_concat(column_name) from information_schema.columns where table_name=0x7573657273 --+
```
执行后，可能会返回`user_id, first_name, last_name, user, password`等结果。其中，`user`和`password`列极有可能存储着用户名和密码。

---

## 6. 提取数据并解密登录 🗝️

最后，我们可以直接从`users`表中提取用户名和密码数据。

以下是提取数据的语句：
```sql
?id=1' union select user, password from users --+
```
执行后，页面会显示`user`列和`password`列的数据。密码通常是经过MD5哈希加密的字符串（如`5f4dcc3b5aa765d61d8327deb882cf99`）。

获取到加密的密码后，需要到MD5解密网站进行破解，或使用暴力破解工具。获得明文密码（如`password123`）后，即可尝试在网站登录界面使用对应的用户名和密码进行登录验证。

---

## 总结 📝

![](img/6c01fa539fb0b4319e3cfb3a61c61575_7.png)

本节课我们一起学习了MySQL手工注入的完整链条：
1.  **判断与确认**：确认注入点及其类型。
2.  **探测结构**：使用`ORDER BY`判断字段数。
3.  **信息收集**：利用`UNION SELECT`联合查询获取数据库名、用户、版本等信息。
4.  **枚举数据**：通过查询`information_schema`数据库，逐步获取目标库的表名、列名。
5.  **提取与利用**：最终提取目标表中的敏感数据（如用户名和密码），解密后完成登录。

![](img/6c01fa539fb0b4319e3cfb3a61c61575_9.png)

![](img/6c01fa539fb0b4319e3cfb3a61c61575_11.png)

整个过程层层递进，核心在于熟练运用SQL语句与`information_schema`数据库。掌握这一流程，是理解SQL注入攻击原理的基础。下一节课，我们将继续深入探讨其他注入技巧。