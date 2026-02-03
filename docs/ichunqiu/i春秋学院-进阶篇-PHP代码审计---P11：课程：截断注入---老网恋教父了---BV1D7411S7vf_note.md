# i春秋学院 进阶篇 PHP代码审计 - P11：课程：截断注入 🔪

在本节课中，我们将要学习一种特殊的SQL注入技术——二次注入。我们将通过分析一个具体的代码案例，理解其原理、利用条件以及修复方法。

![](img/31feb2f3f9262dcd319ca1eac63f3f6e_1.png)

二次注入属于SQL注入的一种。它之所以存在，是因为在一般的字符型注入中，通常需要闭合单引号才能构造有效载荷。如果无法闭合单引号，注入就无法进行。因此，出现了一种新的思路：在存在双条件查询的情况下，可以考虑利用前一个条件去“注释”或“转义”掉一个单引号，从而为后一个条件（即我们的注入载荷）创造机会。第一次查询是前提，第二次注入则基于第一次的前提进行。

![](img/31feb2f3f9262dcd319ca1eac63f3f6e_3.png)

那么，如何“转移”一个单引号呢？这就要考虑到截断、字符集转换或反斜杠转义等技术。这些都可能成为利用的条件。

## 代码审计与漏洞发现 🔍

上一节我们介绍了二次注入的基本概念，本节中我们来看看如何在代码中寻找和利用它。

我们首先在目标代码中寻找漏洞点。在审计了多个页面后，我们在“提交留言”页面发现了一个关键点。

以下是该页面的部分代码：
```php
$sql = "INSERT INTO message (username, content, time) VALUES ('".$_SESSION['username']."', '".clean_input($_POST['content'])."', now())";
```
我们看到，这个SQL语句使用了双条件（实际上是多个值）。`username`来自会话变量，而`content`来自用户输入，并且只经过了`clean_input`函数的简单过滤，没有使用`mysql_real_escape_string`等函数进行安全处理。

这种结构为我们提供了机会。如果`username`的值能“吃掉”它后面的那个单引号，那么`content`部分的内容就可能被当作SQL代码执行。

## 构造利用载荷 💡

理解了漏洞点后，我们来看看如何构造利用载荷。

![](img/31feb2f3f9262dcd319ca1eac63f3f6e_5.png)

我们的目标是：让`username`字段的值以一个反斜杠`\`结尾。这样，在SQL语句中，这个反斜杠会转义掉包裹`username`值的那个单引号，导致单引号的闭合位置发生变化。

![](img/31feb2f3f9262dcd319ca1eac63f3f6e_7.png)

假设我们注册的用户名为 `admin\`，那么构造的SQL语句会变成：
```sql
INSERT INTO message (username, content, time) VALUES ('admin\', 'PAYLOAD', now())
```
在这里，`admin\` 中的反斜杠转义了后面的单引号，使得 `'admin\'` 被数据库视为一个完整的字符串（尽管可能报错或产生非预期行为）。原本用于闭合`username`值的单引号，现在变成了`PAYLOAD`字符串的开头引号。

![](img/31feb2f3f9262dcd319ca1eac63f3f6e_9.png)

![](img/31feb2f3f9262dcd319ca1eac63f3f6e_10.png)

![](img/31feb2f3f9262dcd319ca1eac63f3f6e_12.png)

因此，我们可以在`PAYLOAD`位置插入我们的注入代码。例如，注入获取管理员密码的载荷：
```sql
PAYLOAD: ‘, (SELECT password FROM admin LIMIT 1), ‘1’) --
```
最终拼接的SQL语句会类似于：
```sql
INSERT INTO message (username, content, time) VALUES ('admin\', '', (SELECT password FROM admin LIMIT 1), '1') -- ', 'normal_content', now())
```
这样，`(SELECT password FROM admin LIMIT 1)` 的结果就会被插入到`content`字段中。

![](img/31feb2f3f9262dcd319ca1eac63f3f6e_14.png)

![](img/31feb2f3f9262dcd319ca1eac63f3f6e_16.png)

## 实现前提：控制用户名 🎯

![](img/31feb2f3f9262dcd319ca1eac63f3f6e_18.png)

要实现上述攻击，前提是能控制`username`的值为以反斜杠结尾。这通常发生在用户注册或修改用户名的功能处。

![](img/31feb2f3f9262dcd319ca1eac63f3f6e_20.png)

![](img/31feb2f3f9262dcd319ca1eac63f3f6e_22.png)

我们检查注册功能的代码。发现它对输入进行了处理：
1.  使用`addslashes`添加反斜杠（输入`\`变成`\\`）。
2.  存入数据库时，如果使用`mysql_real_escape_string`等函数，`\\`进入数据库后可能被存储为单个`\`。
3.  当从数据库取出这个用户名并放入`$_SESSION[‘username’]`时，取出的就是单个反斜杠`\`。

![](img/31feb2f3f9262dcd319ca1eac63f3f6e_24.png)

通过这个过程，我们成功地将一个有效的、以反斜杠结尾的用户名设置到了会话中，为二次注入创造了条件。

## 漏洞复现与验证 🧪

![](img/31feb2f3f9262dcd319ca1eac63f3f6e_26.png)

现在，让我们串联起整个攻击流程进行验证。

![](img/31feb2f3f9262dcd319ca1eac63f3f6e_28.png)

以下是完整的攻击步骤：
1.  **注册特殊用户**：注册一个用户名为 `test\`，密码任意的账号。
2.  **登录获取会话**：使用该账号登录，此时 `$_SESSION[‘username’]` 的值为 `test\`（单个反斜杠）。
3.  **发起二次注入**：在留言提交页面，`content`字段填入我们的注入载荷，例如：`‘, (SELECT password FROM admin LIMIT 1), ‘1’) --`。
4.  **查看结果**：提交后，攻击载荷被执行。我们可以通过查看留言列表或数据库，发现`content`字段中出现了管理员的密码哈希值。

![](img/31feb2f3f9262dcd319ca1eac63f3f6e_30.png)

通过这个流程，我们成功利用二次注入漏洞获取了敏感信息。

![](img/31feb2f3f9262dcd319ca1eac63f3f6e_32.png)

## 漏洞修复方案 🛡️

![](img/31feb2f3f9262dcd319ca1eac63f3f6e_34.png)

![](img/31feb2f3f9262dcd319ca1eac63f3f6e_36.png)

学习攻击是为了更好的防御。最后，我们来看看如何修复这个漏洞。

![](img/31feb2f3f9262dcd319ca1eac63f3f6e_38.png)

修复的核心原则是：对所有用户输入进行严格的过滤和转义，特别是在拼接SQL语句时。

![](img/31feb2f3f9262dcd319ca1eac63f3f6e_40.png)

以下是具体的修复建议：
1.  **使用参数化查询（Prepared Statements）**：这是最根本、最安全的解决方案。将SQL语句与数据分离。
2.  **加强输入验证**：在用户注册、修改用户名等处，严格限制用户名的字符集。例如，使用正则表达式只允许字母数字。
    ```php
    // 例如，只允许大小写字母和数字
    if (!preg_match(‘/^[a-zA-Z0-9]+$/’, $username)) {
        die(‘用户名包含非法字符’);
    }
    ```
3.  **统一使用安全的转义函数**：如果暂时不能使用参数化查询，确保对所有插入数据库的变量使用如 `mysqli_real_escape_string()` 等函数进行转义。
4.  **最小化会话信任**：不要盲目信任存储在会话（`$_SESSION`）中的数据，在将其用于数据库操作前也应进行校验或转义。

![](img/31feb2f3f9262dcd319ca1eac63f3f6e_42.png)

![](img/31feb2f3f9262dcd319ca1eac63f3f6e_44.png)

通过实施这些措施，可以有效地杜绝此类二次注入漏洞。

![](img/31feb2f3f9262dcd319ca1eac63f3f6e_46.png)

## 总结 📝

本节课中我们一起学习了二次注入攻击。
*   **原理**：利用第一次查询（如用户注册）将恶意代码片段存入数据库，在第二次查询（如数据展示或插入）时触发执行。
*   **关键**：找到存在双条件/多条件且过滤不严的SQL语句，并控制前一个条件的值来改变SQL语法结构。
*   **利用**：通过注册特殊用户名（如以`\`结尾）为后续注入铺平道路。
*   **修复**：采用参数化查询、加强输入验证、统一安全转义是防御的关键。

![](img/31feb2f3f9262dcd319ca1eac63f3f6e_48.png)

本节的审计课程到此结束。希望通过对这个案例的深入分析，你能更深刻地理解二次注入的成因与危害，并在开发中建立起牢固的安全防线。