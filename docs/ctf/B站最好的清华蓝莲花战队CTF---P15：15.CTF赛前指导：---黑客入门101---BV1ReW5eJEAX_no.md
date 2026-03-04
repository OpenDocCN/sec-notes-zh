# CTF赛前指导：15：Web基础与SQL注入入门 🚩

![](img/45cdca6c9df6ea7f02b8641da6bf8529_0.png)

在本节课中，我们将要学习Web应用的基本架构、HTTP协议的基础知识，以及SQL注入攻击的基本原理。通过理解这些核心概念，你将能够应对CTF比赛中常见的Web类题目。

## Web应用基本架构

上一节我们介绍了课程概述，本节中我们来看看Web应用是如何工作的。一个典型的Web应用架构主要分为客户端和服务端两部分。

*   **服务端**：运行着Web服务器软件（如Apache、IIS、Nginx），负责处理客户端请求。它从指定的文件路径读取数据（如HTML页面、图片），并通过HTTP协议发送给客户端。当数据量庞大时，服务端会连接专门的数据库服务器来存储和查询数据，此时HTTP请求中的参数需要被转换成SQL语句与数据库交互。
*   **客户端**：用户通过浏览器访问Web应用。浏览器负责解析服务器返回的HTML、CSS、JavaScript代码，并将其渲染成可视化的网页。早期通过命令行访问网络，后来出现了网景（Netscape）等图形化浏览器，极大地改善了上网体验。

## HTTP协议基础

理解了架构，我们来看看数据是如何在客户端和服务端之间传输的。这依赖于HTTP（超文本传输）协议，它用于从Web服务器传输超文本到本地浏览器，并且基于“请求-响应”模型。

![](img/45cdca6c9df6ea7f02b8641da6bf8529_2.png)

![](img/45cdca6c9df6ea7f02b8641da6bf8529_4.png)

![](img/45cdca6c9df6ea7f02b8641da6bf8529_5.png)

![](img/45cdca6c9df6ea7f02b8641da6bf8529_7.png)

![](img/45cdca6c9df6ea7f02b8641da6bf8529_9.png)

![](img/45cdca6c9df6ea7f02b8641da6bf8529_11.png)

![](img/45cdca6c9df6ea7f02b8641da6bf8529_13.png)

![](img/45cdca6c9df6ea7f02b8641da6bf8529_15.png)

### HTTP请求

![](img/45cdca6c9df6ea7f02b8641da6bf8529_17.png)

![](img/45cdca6c9df6ea7f02b8641da6bf8529_19.png)

![](img/45cdca6c9df6ea7f02b8641da6bf8529_20.png)

![](img/45cdca6c9df6ea7f02b8641da6bf8529_22.png)

一个HTTP请求报文包含三个部分。

![](img/45cdca6c9df6ea7f02b8641da6bf8529_24.png)

*   **请求行**：包含请求方法（如GET或POST）和HTTP协议版本。例如：`GET /index.html HTTP/1.1`。
*   **消息头**：包含一系列键值对，描述了客户端的信息和请求的细节。这些信息可以被工具（如Burp Suite代理）拦截并修改。常见的消息头字段包括：
    *   `Accept`：客户端能够接收的内容类型。
    *   `Accept-Language`：浏览器可以接受的语言。
    *   `Referer`：表示当前请求是从哪个页面链接跳转而来的。
    *   `User-Agent`：包含发出请求的用户信息，如操作系统和浏览器类型。
    *   `X-Forwarded-For` 或 `Client-IP`：当用户通过代理服务器访问时，用于标识用户的真实IP地址。
*   **请求正文**：通常用于POST请求，携带提交的表单数据等。

### HTTP响应

![](img/45cdca6c9df6ea7f02b8641da6bf8529_26.png)

![](img/45cdca6c9df6ea7f02b8641da6bf8529_27.png)

服务器处理请求后，会返回一个HTTP响应报文。

*   **状态行**：包含HTTP协议版本、状态码和状态描述。常见状态码有：
    *   `200 OK`：请求成功。
    *   `302 Found`：请求的资源临时从不同的URI响应，引导用户向其他URL请求资源。
    *   `404 Not Found`：请求的资源未找到。
*   **响应头**：类似于请求头，包含服务器返回的元信息，如 `Content-Type` 指明返回内容的类型。
*   **响应正文**：服务器返回的实际内容，如HTML文档。

## 信息收集与前端分析

![](img/45cdca6c9df6ea7f02b8641da6bf8529_29.png)

![](img/45cdca6c9df6ea7f02b8641da6bf8529_30.png)

在CTF的Web题目中，信息收集是第一步。以下是常见的解题思路。

### 查看页面源代码

![](img/45cdca6c9df6ea7f02b8641da6bf8529_32.png)

![](img/45cdca6c9df6ea7f02b8641da6bf8529_34.png)

![](img/45cdca6c9df6ea7f02b8641da6bf8529_36.png)

许多题目的线索隐藏在HTML、CSS或JavaScript代码中。通过浏览器右键“查看页面源代码”或按F12打开开发者工具，可以找到注释、隐藏的表单、JavaScript验证逻辑等。

**示例**：一个简单的密码验证页面，其JavaScript代码中可能直接包含了正确的密码或验证逻辑。

```javascript
if (x == ‘flag{this_is_password}’) {
    alert(‘Success!’);
}
```

### 使用代理工具分析请求

对于不直接显示在前端的题目，需要使用代理工具（如Burp Suite）拦截和分析HTTP请求与响应包。

**操作步骤**：
1.  配置浏览器代理指向Burp Suite（默认端口8080）。
2.  在Burp Suite中开启拦截功能。
3.  在浏览器中访问目标页面，Burp Suite会捕获请求。
4.  将捕获的请求发送到 **Repeater** 模块，可以方便地修改并重发请求，同时观察请求和响应。

**常见考点**：修改HTTP请求头以符合特定条件。
*   **要求从特定IP访问**：修改 `X-Forwarded-For` 头。
*   **要求从特定国家访问**：修改 `Accept-Language` 头。
*   **要求从特定页面跳转**：修改 `Referer` 头。

![](img/45cdca6c9df6ea7f02b8641da6bf8529_38.png)

![](img/45cdca6c9df6ea7f02b8641da6bf8529_40.png)

![](img/45cdca6c9df6ea7f02b8641da6bf8529_42.png)

## SQL注入原理

![](img/45cdca6c9df6ea7f02b8641da6bf8529_44.png)

![](img/45cdca6c9df6ea7f02b8641da6bf8529_46.png)

当Web应用需要与数据库交互时（如用户登录查询），就可能存在SQL注入漏洞。攻击者通过构造特殊的输入，欺骗服务器执行非预期的SQL命令。

![](img/45cdca6c9df6ea7f02b8641da6bf8529_48.png)

![](img/45cdca6c9df6ea7f02b8641da6bf8529_50.png)

### 数据库基础概念

*   **数据库**：存放数据的集合。
*   **数据库管理系统（DBMS）**：操纵和管理数据库的软件，如MySQL、SQL Server、Oracle。
*   **SQL语句**：用于与数据库通信的语言，最常用的操作是“查询”（SELECT）。

一个典型的登录查询SQL语句如下：
```sql
SELECT * FROM users WHERE username=‘admin’ AND password=‘123456’
```
这条语句在 `users` 表中查找用户名为“admin”且密码为“123456”的记录。

### SQL注入攻击示例

假设一个登录后台的代码直接将用户输入拼接到SQL语句中：
```php
$sql = “SELECT * FROM users WHERE username=‘” . $_POST[‘username’] . “‘ AND password=‘” . $_POST[‘password’] . “‘”;
```

如果用户在用户名输入框中输入 `admin‘ OR ’1‘=’1`，那么拼接后的SQL语句将变为：
```sql
SELECT * FROM users WHERE username=‘admin‘ OR ’1‘=’1’ AND password=‘anything’
```
由于 `’1‘=’1‘` 这个条件永远为真，这条查询很可能返回用户表中的第一条记录，从而绕过密码验证。这就是所谓的“万能密码”。

另一种常见手法是使用注释符 `--` 或 `#` 来注释掉后面的密码检查部分。
输入用户名：`admin‘#`
拼接后的SQL：
```sql
SELECT * FROM users WHERE username=‘admin‘#’ AND password=‘...’
```
`#` 之后的内容被注释，查询变为只检查用户名是否为“admin”，同样可能绕过验证。

![](img/45cdca6c9df6ea7f02b8641da6bf8529_52.png)

![](img/45cdca6c9df6ea7f02b8641da6bf8529_54.png)

![](img/45cdca6c9df6ea7f02b8641da6bf8529_56.png)

### 使用自动化工具进行SQL注入

![](img/45cdca6c9df6ea7f02b8641da6bf8529_58.png)

手动注入效率较低，可以使用自动化工具如 **sqlmap**。
1.  发现注入点后，使用 `sqlmap -u “目标URL”` 进行扫描。
2.  确认存在注入后，可以进一步获取数据库名、表名、字段名和数据。
3.  例如，获取数据库中的所有表：`sqlmap -u “目标URL” --tables`
4.  获取指定表（如users）中的所有数据：`sqlmap -u “目标URL” -D 数据库名 -T users --dump`

通过这种方式，攻击者可以窃取数据库中的敏感信息，如管理员的用户名和加密后的密码。如果密码是弱加密（如MD5），还可以通过彩虹表等方式进行破解。

## 总结

![](img/45cdca6c9df6ea7f02b8641da6bf8529_60.png)

本节课中我们一起学习了Web安全的基础知识。我们首先了解了Web应用的基本架构和HTTP协议的工作机制，这是分析Web题目的基础。接着，我们探讨了通过查看源代码和使用代理工具进行信息收集的方法。最后，我们深入学习了SQL注入的原理，从手动构造“万能密码”到使用自动化工具进行数据库信息窃取。掌握这些知识，是解决CTF中Web类题目和入门Web安全的关键一步。