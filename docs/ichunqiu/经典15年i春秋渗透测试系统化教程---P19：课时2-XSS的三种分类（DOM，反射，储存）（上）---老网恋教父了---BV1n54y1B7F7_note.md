# 经典15年i春秋渗透测试系统化教程 - P19：课时2 XSS的三种分类（DOM，反射，储存）（上）🔍

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_1.png)

## 概述

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_3.png)

在本节课中，我们将要学习跨站脚本攻击的三种主要类型：反射型、存储型和DOM型。我们将通过实例演示来理解它们的工作原理、区别以及危害性。

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_5.png)

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_7.png)

---

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_9.png)

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_10.png)

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_12.png)

## 反射型XSS

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_14.png)

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_16.png)

上一节我们介绍了XSS的基本概念，本节中我们来看看反射型XSS。

反射型XSS的特点是，攻击者插入的恶意脚本不会被保存在服务器端。它通常通过钓鱼链接等方式，诱使用户点击一个包含恶意代码的URL来触发。

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_18.png)

以下是反射型XSS的一个关键特点：
*   恶意脚本不保存在服务器端。
*   通常需要配合社会工程学，通过钓鱼方式发送给用户。
*   攻击能否成功具有一定偶然性。

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_20.png)

为了演示，我们可以使用一个国外的测试网站。你需要使用VPN连接才能访问该网站。

该网站提供了各种XSS的测试环境，包括反射型、存储型和DOM型等。我们点击进入反射型测试页面。

在这个页面中，我们可以看到，任何可以输入参数的地方都可能存在XSS漏洞。例如，在`<body>`标签之间插入恶意代码。

我们输入一段简单的XSS代码进行测试：
```html
<script>alert('XSS')</script>
```
页面会弹出一个对话框，这说明此处存在反射型XSS漏洞。

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_22.png)

除了`<body>`标签，在`<head>`标签、`<title>`标签、注释点、`<p>`标签下等几乎所有可以输入的地方，都可以尝试插入XSS代码进行测试。该网站提供了大量HTML、JS、CSS等不同场景下的测试实例，大家可以直接点击进行测试。

---

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_24.png)

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_26.png)

## 存储型XSS

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_28.png)

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_30.png)

了解了反射型XSS后，我们来看看存储型XSS。

与反射型相比，存储型XSS的唯一区别在于，攻击者输入的恶意脚本会被保存到服务器的数据库或文件中。当其他用户（例如管理员）浏览相关页面时，这段恶意脚本会被从服务器读取并执行，危害性更大。

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_32.png)

以下是存储型XSS的一个关键特点：
*   恶意脚本被保存在服务器端（如数据库中）。
*   每次用户浏览相关页面时，脚本都会被读取并执行。
*   常见于论坛、留言板等用户可以持久化输入内容的地方。

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_34.png)

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_36.png)

我们使用DVWA靶场进行演示。在DVWA的存储型XSS模块中，我们设置安全级别为“低”。

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_38.png)

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_40.png)

在留言板中，我们输入一条测试留言，并在消息内容中插入XSS代码：
```html
<script>alert('Stored XSS')</script>
```
点击提交后，页面会立即弹出一个对话框。

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_41.png)

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_43.png)

此时，如果我们关闭页面再重新打开，或者换一个用户（如管理员）登录并查看留言板，这个对话框依然会弹出。这是因为恶意代码已经被存储到了服务器。

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_45.png)

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_47.png)

为了验证这一点，我们可以查看数据库。使用MySQL命令行工具登录数据库，并选择DVWA对应的数据库。

查看数据库中的表，找到存储留言的`guestbook`表。执行SQL查询语句：
```sql
SELECT * FROM guestbook;
```
可以看到，我们刚才插入的XSS代码确实被保存在了数据库的`comment`字段中。这就是为什么每次浏览页面都会触发弹窗的原因。

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_49.png)

如果我们从数据库中删除这条记录，那么再次浏览页面时，弹窗就不会出现了。

存储型XSS的危害之所以更大，是因为它一旦被注入，所有访问受影响页面的用户都会中招。在DVWA的高安全级别下，程序会对输入进行过滤，从而防御此类攻击。

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_51.png)

---

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_53.png)

## 总结

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_55.png)

本节课中我们一起学习了跨站脚本攻击的两种类型：反射型XSS和存储型XSS。

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_57.png)

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_59.png)

反射型XSS的恶意脚本不存储在服务器，需要通过钓鱼链接触发，危害相对可控但需警惕社会工程学攻击。

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_61.png)

存储型XSS的恶意脚本会持久化保存在服务器端，对所有访问者构成持续威胁，危害性更大，常见于有用户交互和内容存储功能的网站。

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_63.png)

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_65.png)

![](img/a430cbe0c33cdfc8edfdec4e50f8b8c8_66.png)

下一节，我们将继续学习第三种类型：DOM型XSS。