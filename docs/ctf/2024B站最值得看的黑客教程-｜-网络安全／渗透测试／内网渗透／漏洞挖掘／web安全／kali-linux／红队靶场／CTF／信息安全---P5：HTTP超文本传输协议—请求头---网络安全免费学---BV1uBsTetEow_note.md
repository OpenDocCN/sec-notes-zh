# 网络安全入门：P5：HTTP请求头详解 🔍

![](img/04a57c7b44d9f27112c0ab04d868f9d0_1.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_2.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_3.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_4.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_5.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_6.png)

在本节课中，我们将要学习HTTP协议中请求头（Request Headers）的核心概念。请求头是客户端（如浏览器）向服务器发送请求时附带的关键信息，它决定了服务器如何处理这次请求。理解这些字段对于后续学习Web安全、渗透测试至关重要。

![](img/04a57c7b44d9f27112c0ab04d868f9d0_7.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_8.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_9.png)

上一节我们介绍了HTTP协议的基本结构，本节中我们来看看请求头中几个最重要字段的具体含义和作用。

![](img/04a57c7b44d9f27112c0ab04d868f9d0_11.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_12.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_13.png)

## Host字段：指定目标地址 🌐

![](img/04a57c7b44d9f27112c0ab04d868f9d0_15.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_16.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_17.png)

首先，我们来看第一个字段：`Host`。它的中文意思是“主机”或“地址”，用于告诉服务器你想要访问哪个网站。

![](img/04a57c7b44d9f27112c0ab04d868f9d0_18.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_19.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_20.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_21.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_22.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_23.png)

例如，如果你想访问百度，那么`Host`字段的值就是 `www.baidu.com`。其基本格式如下：

![](img/04a57c7b44d9f27112c0ab04d868f9d0_24.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_25.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_26.png)

```
Host: www.target-domain.com
```

![](img/04a57c7b44d9f27112c0ab04d868f9d0_28.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_29.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_30.png)

这个字段是HTTP/1.1协议中必须的，它使得一个服务器能够托管多个域名（虚拟主机）。

![](img/04a57c7b44d9f27112c0ab04d868f9d0_31.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_32.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_33.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_34.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_35.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_36.png)

## User-Agent字段：浏览器身份标识 🖥️📱

![](img/04a57c7b44d9f27112c0ab04d868f9d0_37.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_38.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_39.png)

第二个关键字段是 `User-Agent`。它用于告诉服务器关于客户端浏览器和操作系统的详细信息，以便服务器解决浏览器兼容性问题或提供不同的页面内容。

![](img/04a57c7b44d9f27112c0ab04d868f9d0_41.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_42.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_43.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_44.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_45.png)

以下是`User-Agent`的两个主要应用场景：

![](img/04a57c7b44d9f27112c0ab04d868f9d0_46.png)

1.  **浏览器版本检测**：当服务器发现你的浏览器版本过低时，可能会提示“请更新浏览器”。服务器正是通过解析`User-Agent`字段来获知你的浏览器信息的。
2.  **设备适配**：用手机和电脑访问同一个网站，看到的页面布局可能不同。这是因为手机的`User-Agent`会标明自己是移动设备（如`iPhone`），而电脑的则会标明操作系统（如`Windows NT`）。服务器根据此信息返回适配的网页版本。

![](img/04a57c7b44d9f27112c0ab04d868f9d0_48.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_49.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_50.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_51.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_52.png)

一个典型的`User-Agent`字符串可能如下所示：
```
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36
```

![](img/04a57c7b44d9f27112c0ab04d868f9d0_53.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_54.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_55.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_56.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_57.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_58.png)

## Referer字段：来源追踪与防盗链 🔗

![](img/04a57c7b44d9f27112c0ab04d868f9d0_59.png)

接下来是 `Referer` 字段（注意拼写）。它告诉服务器当前请求是从哪个页面链接过来的。这个字段在开发中作用很大。

![](img/04a57c7b44d9f27112c0ab04d868f9d0_61.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_62.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_63.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_64.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_65.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_66.png)

以下是`Referer`字段的两个核心功能：

![](img/04a57c7b44d9f27112c0ab04d868f9d0_67.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_68.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_69.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_70.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_71.png)

1.  **流量来源统计**：网站运营者可以统计用户是通过哪个渠道（如百度搜索、微博链接）访问过来的，从而评估广告或推广活动的效果。
2.  **防盗链**：这是保护网站资源（如图片、视频）不被其他网站直接盗用的关键技术。例如，腾讯视频的服务器会检查`Referer`值。如果发现视频请求不是来自腾讯视频自己的域名，而是来自某个盗链的电影小站，服务器就会拒绝提供视频流，并可能返回错误提示。

![](img/04a57c7b44d9f27112c0ab04d868f9d0_73.png)

## Cookie字段：维持会话状态的“令牌” 🍪

![](img/04a57c7b44d9f27112c0ab04d868f9d0_74.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_75.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_76.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_77.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_78.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_79.png)

最后，我们讲解至关重要的 `Cookie` 字段。由于HTTP协议本身是无状态的，它无法记录用户的登录状态。`Cookie` 机制就是为了解决这个问题而生的。

![](img/04a57c7b44d9f27112c0ab04d868f9d0_80.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_81.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_82.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_83.png)

你可以把`Cookie`想象成古代皇帝的“令牌”或“玉玺”。客户端（浏览器）在首次登录成功后，服务器会下发一个包含身份信息的`Cookie`。之后，浏览器在访问该网站的其他页面时，都会在请求头中自动带上这个`Cookie`。

![](img/04a57c7b44d9f27112c0ab04d868f9d0_85.png)

服务器通过验证这个`Cookie`，就能识别出用户身份，从而维持登录状态，无需用户每次访问都重新输入账号密码。

![](img/04a57c7b44d9f27112c0ab04d868f9d0_86.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_87.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_88.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_89.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_90.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_91.png)

一个简单的`Cookie`在请求头中如下所示：
```
Cookie: session_id=abc123xyz; username=john_doe
```

![](img/04a57c7b44d9f27112c0ab04d868f9d0_92.png)

> **注**：有同学提到`Token`，它是一个广义的“令牌”概念，在不同协议和应用中有不同实现。而在HTTP上下文中，`Cookie`是维持会话状态最常用的机制之一。

![](img/04a57c7b44d9f27112c0ab04d868f9d0_94.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_95.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_96.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_97.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_98.png)

---

![](img/04a57c7b44d9f27112c0ab04d868f9d0_99.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_100.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_101.png)

![](img/04a57c7b44d9f27112c0ab04d868f9d0_103.png)

本节课中我们一起学习了HTTP请求头中四个核心字段：**`Host`**、**`User-Agent`**、**`Referer`** 和 **`Cookie`**。它们分别负责指定目标、标识客户端、追踪来源和维持状态。理解这些字段是分析网络流量、进行Web安全测试的基础。就像学习语言不需要背完整本词典，掌握这些最常用、最关键的头字段足以让你迈出坚实的第一步。在后续课程中，我们将看到攻击者如何利用这些字段进行安全测试。