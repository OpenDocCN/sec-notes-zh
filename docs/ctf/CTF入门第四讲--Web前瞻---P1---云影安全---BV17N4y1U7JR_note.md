# CTF入门第四讲：Web前瞻 - P1

![](img/adf960b94d65ac7e1e4c7c2940554db5_1.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_3.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_5.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_7.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_8.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_10.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_12.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_14.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_16.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_18.png)

在本节课中，我们将要学习Web应用的基础构成、核心的请求方式以及一些安全开发的基本概念。课程内容旨在为零基础的同学提供一个清晰的Web技术概览，为后续的CTF Web方向学习打下基础。

![](img/adf960b94d65ac7e1e4c7c2940554db5_20.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_22.png)

## Web基础构成

![](img/adf960b94d65ac7e1e4c7c2940554db5_24.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_26.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_28.png)

一个完整的Web应用主要由前端、后端和数据库三部分构成。

![](img/adf960b94d65ac7e1e4c7c2940554db5_30.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_32.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_34.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_35.png)

### 前端：内容展示层

![](img/adf960b94d65ac7e1e4c7c2940554db5_37.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_39.png)

前端负责将内容展示给用户。例如，你在B站看到的轮播图、按钮、视频和文字链接，都属于前端范畴。

![](img/adf960b94d65ac7e1e4c7c2940554db5_41.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_43.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_45.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_47.png)

前端技术核心包括：
*   **HTML**：超文本标记语言，用于构建网页的基本结构。例如，一个按钮可以用 `<button>` 标签表示。
*   **CSS**：层叠样式表，用于美化网页，设置颜色、布局等样式。
*   **JavaScript**：实现网页的交互功能，例如点击按钮跳转、提交表单、播放视频等。

以下是一个简单的HTML按钮示例，并通过CSS将其文字颜色设置为红色：
```html
<!DOCTYPE html>
<html>
<head>
    <style>
        .btn {
            color: red;
        }
    </style>
</head>
<body>
    <button class="btn">点击我</button>
</body>
</html>
```

![](img/adf960b94d65ac7e1e4c7c2940554db5_49.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_51.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_53.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_55.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_57.png)

### 后端：数据处理层

![](img/adf960b94d65ac7e1e4c7c2940554db5_59.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_61.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_63.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_65.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_67.png)

后端负责处理业务逻辑和数据。例如，当你在百度翻译输入“你好”并点击翻译时，前端会将“你好”发送给后端，后端处理后再将“Hello”返回给前端展示。

![](img/adf960b94d65ac7e1e4c7c2940554db5_69.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_71.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_73.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_75.png)

目前流行的后端语言和框架包括：
*   **Java** + **Spring Boot**：目前企业级开发中最流行的组合之一。
*   **Go**：以其高性能和简洁的语法近年来备受关注。
*   **PHP**：虽然在大厂中应用减少，但它是CTF Web题目中最常出现的语言。
*   **Python** + **Flask/Django**：常用于快速开发和脚本编写。

![](img/adf960b94d65ac7e1e4c7c2940554db5_77.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_79.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_81.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_83.png)

### 数据库：数据存储层

![](img/adf960b94d65ac7e1e4c7c2940554db5_85.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_87.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_89.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_91.png)

数据库用于持久化存储数据，如用户信息、视频链接、评论内容等。

![](img/adf960b94d65ac7e1e4c7c2940554db5_93.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_94.png)

常见的数据库类型包括：
*   **关系型数据库**：如 **MySQL**、**Oracle**。它们使用SQL语言进行增删改查操作。
*   **非关系型数据库**：如 **Redis**（缓存）、**MongoDB**（文档存储）。

![](img/adf960b94d65ac7e1e4c7c2940554db5_96.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_98.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_100.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_101.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_103.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_105.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_107.png)

## HTTP请求方式

![](img/adf960b94d65ac7e1e4c7c2940554db5_109.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_111.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_113.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_114.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_116.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_118.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_120.png)

客户端（如浏览器）与服务器之间通过HTTP协议通信，其中请求方式（Method）定义了操作的类型。以下是几种核心的请求方式。

![](img/adf960b94d65ac7e1e4c7c2940554db5_122.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_124.png)

### GET 与 POST

![](img/adf960b94d65ac7e1e4c7c2940554db5_126.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_128.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_129.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_131.png)

这是最常用的两种请求方式。

![](img/adf960b94d65ac7e1e4c7c2940554db5_133.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_135.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_137.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_139.png)

*   **GET请求**：用于向服务器**获取**资源。参数会直接附加在URL之后，格式为 `?key1=value1&key2=value2`。
    *   **特点**：参数明文可见，长度有限制。
    *   **示例**：`https://www.baidu.com/s?wd=CTF`

*   **POST请求**：用于向服务器**提交**数据，常用于表单提交、文件上传等。
    *   **特点**：数据放在请求体（Request Body）中，不可见，长度无限制。
    *   **数据格式**：通常使用JSON格式。JSON是一种轻量级的数据交换格式，使用“键值对”来组织数据。
    *   **示例**：
        ```json
        {
          "username": "zhangsan",
          "password": "123456"
        }
        ```

![](img/adf960b94d65ac7e1e4c7c2940554db5_141.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_143.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_145.png)

### RESTful API 与 增删改查

![](img/adf960b94d65ac7e1e4c7c2940554db5_147.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_149.png)

在标准的RESTful架构中，HTTP请求方式与对数据的操作（增删改查）有明确的对应关系：
*   **POST** -> **增** (Create)：新增数据。
*   **DELETE** -> **删** (Delete)：删除数据。
*   **PUT** -> **改** (Update)：更新数据。
*   **GET** -> **查** (Retrieve)：查询数据。

![](img/adf960b94d65ac7e1e4c7c2940554db5_151.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_153.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_155.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_157.png)

这种设计使API接口清晰且规范。在CTF题目中，最常见的是GET和POST请求。

![](img/adf960b94d65ac7e1e4c7c2940554db5_159.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_161.png)

## Web安全开发前瞻

![](img/adf960b94d65ac7e1e4c7c2940554db5_163.png)

上一节我们介绍了Web的基本构成和通信方式，本节中我们来看看在开发中需要考虑的一些安全措施。

![](img/adf960b94d65ac7e1e4c7c2940554db5_165.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_167.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_169.png)

### 1. 数据加密

![](img/adf960b94d65ac7e1e4c7c2940554db5_171.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_173.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_175.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_177.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_179.png)

在数据传输和存储过程中，加密至关重要。
*   **非对称加密（如RSA）**：使用公钥加密，私钥解密。常用于保护传输过程中的敏感数据，确保即使公钥泄露，攻击者也无法解密信息。
*   **代码混淆**：为了防止前端JavaScript代码（如加密逻辑、公钥）被轻易审计，可以使用Webpack等工具进行代码混淆和压缩，大幅降低可读性。

![](img/adf960b94d65ac7e1e4c7c2940554db5_181.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_183.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_185.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_187.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_189.png)

### 2. 用户认证与授权

![](img/adf960b94d65ac7e1e4c7c2940554db5_191.png)

如何安全地识别用户身份并控制其访问权限是关键。
*   **Session-Cookie 机制**：用户登录后，服务器创建Session并生成唯一Session ID返回给客户端，客户端将其存入Cookie。后续请求携带此Cookie，服务器通过Session ID验证用户身份。
    *   **优点**：Session数据存储在服务器，相对安全。
    *   **缺点**：在分布式集群环境中，Session共享较麻烦。
*   **JWT (JSON Web Token) 机制**：用户登录后，服务器生成一个加密的Token（包含用户信息）返回给客户端。客户端后续在请求头中携带此Token。
    *   **优点**：Token本身包含信息，无需服务器存储，非常适合分布式系统。
    *   **缺点**：Token一旦签发，在有效期内无法主动废止。

![](img/adf960b94d65ac7e1e4c7c2940554db5_193.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_195.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_196.png)

### 3. 其他安全实践

![](img/adf960b94d65ac7e1e4c7c2940554db5_198.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_200.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_202.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_204.png)

*   **动态口令**：如短信验证码、扫码登录、基于时间的动态口令（如Google Authenticator）。即使静态密码泄露，没有动态口令也无法登录，有效防止“撞库”。
*   **设备指纹**：通过收集浏览器版本、屏幕分辨率、字体等设备信息，生成一个唯一标识。可用于识别异常登录行为（例如同一账号从多个完全不同设备登录）。
*   **防范钓鱼邮件**：注意检查发件人邮箱是否伪造、邮件内容格式是否正规、链接是否为可疑短链。对于短链，可以使用抓包工具（如Burp Suite）分析其真实跳转地址。

![](img/adf960b94d65ac7e1e4c7c2940554db5_206.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_207.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_209.png)

## 总结

![](img/adf960b94d65ac7e1e4c7c2940554db5_211.png)

![](img/adf960b94d65ac7e1e4c7c2940554db5_213.png)

本节课我们一起学习了Web应用的基础架构，包括负责展示的前端、处理逻辑的后端和存储数据的数据库。我们深入探讨了HTTP协议中的GET与POST请求，以及它们在RESTful API中与增删改查操作的对应关系。最后，我们前瞻了Web安全开发中的几个重要概念：数据加密、用户认证（Session vs JWT）以及动态口令、设备指纹等防护措施。理解这些基础知识，将帮助我们更好地后续学习CTF Web方向的各种漏洞与利用技术。