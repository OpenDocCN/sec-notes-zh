# 018：DNS 重绑定攻击 🔄

![](img/1bb76722691ddd2e6b1a537a70a84c3a_1.png)

在本节课中，我们将深入学习一种名为 **DNS 重绑定** 的攻击技术。这是一种非常微妙且强大的攻击方式，它允许攻击者绕过浏览器的同源策略，直接与受害者本地网络中的设备或服务进行通信。我们将从回顾之前课程中提到的本地服务器安全问题开始，逐步揭示这种攻击的原理，并最终学习如何防御它。

## 课程概述与回顾 📚

上一节我们讨论了本地HTTP服务器（例如Zoom客户端）的安全问题，并尝试通过将请求方法从`GET`改为`PUT`，并利用CORS的预检请求机制来防御攻击。表面上看，这似乎解决了问题，因为攻击者网站无法通过预检请求的检查。

然而，DNS重绑定攻击可以巧妙地绕过这些防御。本节中，我们来看看这种攻击是如何工作的。

## DNS重绑定攻击详解 🎯

DNS重绑定攻击的核心思想是**欺骗浏览器，让它认为一个跨域请求实际上是同源请求**。这样，浏览器就不会执行CORS检查（包括预检请求），从而允许攻击者的代码直接与目标服务器通信。

### 攻击原理

同源策略的定义是：**协议 + 主机名 + 端口**。它**不关心服务器的IP地址**。攻击者正是利用了这一点。

攻击步骤如下：
1.  用户访问攻击者控制的网站 `attacker.com`。
2.  该网站的初始DNS解析指向攻击者自己的服务器IP（例如 `93.184.216.34`）。
3.  网站加载后，其中的JavaScript代码尝试向 `attacker.com` 自身发起一个请求（例如一个`PUT`请求）。由于这是同源请求，浏览器会直接允许。
4.  与此同时，攻击者迅速修改 `attacker.com` 的DNS记录，将其指向目标本地服务器的IP地址（例如 `127.0.0.1` 或 `192.168.1.100`）。
5.  当浏览器的DNS缓存过期后，它再次查询 `attacker.com` 的IP，此时得到的是本地IP地址。
6.  浏览器执行第3步中的请求，但它现在实际连接的是本地服务器。由于浏览器认为这是同源请求（主机名仍是 `attacker.com`），它不会进行任何CORS检查，请求被成功发送到本地服务器。

![](img/1bb76722691ddd2e6b1a537a70a84c3a_3.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_5.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_7.png)

### 攻击的影响范围

![](img/1bb76722691ddd2e6b1a537a70a84c3a_9.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_11.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_12.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_14.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_16.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_18.png)

这种攻击的危害极大：
*   **绕过同源策略**：攻击者可以发送任何类型的请求（`GET`、`PUT`、带自定义头部的请求等）到本地服务。
*   **绕过防火墙**：攻击者可以利用受害者的浏览器作为代理，访问其内部网络中本应受防火墙保护的设备（如打印机、智能摄像头、路由器等）。
*   **广泛存在**：大量IoT设备、本地服务器软件历史上都曾存在或仍然存在此类漏洞。

![](img/1bb76722691ddd2e6b1a537a70a84c3a_20.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_22.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_24.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_26.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_27.png)

以下是部分设备的漏洞统计：
*   66% 的打印机
*   75% 的IP摄像头
*   87% 的路由器、交换机和接入点
*   78% 的流媒体播放器和智能音箱

![](img/1bb76722691ddd2e6b1a537a70a84c3a_29.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_31.png)

## 攻击演示 💻

![](img/1bb76722691ddd2e6b1a537a70a84c3a_33.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_35.png)

让我们通过一个简化示例来理解攻击流程。假设我们有一个不安全的本地服务器，当收到`PUT`请求时会执行某个危险操作（例如打开系统字典）。

![](img/1bb76722691ddd2e6b1a537a70a84c3a_37.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_39.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_41.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_43.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_45.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_47.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_49.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_51.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_53.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_55.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_57.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_59.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_61.png)

**攻击者网站 (`attacker.com`) 的代码可能如下：**
```html
<button id="attackBtn">发送PUT请求</button>
<script>
document.querySelector('#attackBtn').addEventListener('click', () => {
    // 注意：这里请求的是攻击者自己的域名
    fetch('http://attacker.com:8080/launch', { method: 'PUT' })
        .then(response => console.log('请求已发送'))
        .catch(err => console.error('请求失败:', err));
});
</script>
```
**攻击过程：**
1.  用户访问 `http://attacker.com`。此时DNS解析为攻击者服务器IP，页面加载上述代码。
2.  攻击者将 `attacker.com` 的DNS A记录修改为 `127.0.0.1`。
3.  用户点击按钮。浏览器发现这是向 `attacker.com` 发起的同源`PUT`请求，直接放行。
4.  由于DNS记录已更改，该请求实际被发送到本地 `127.0.0.1:8080` 的服务上，攻击成功。

![](img/1bb76722691ddd2e6b1a537a70a84c3a_63.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_65.png)

## 如何防御DNS重绑定攻击 🛡️

理解了攻击原理后，防御方法就变得清晰直接。关键在于：**本地服务器不能仅依赖浏览器的同源策略来验证请求来源**。

![](img/1bb76722691ddd2e6b1a537a70a84c3a_67.png)

最有效的防御措施是：**在服务器端校验 `Host` 头部**。

![](img/1bb76722691ddd2e6b1a537a70a84c3a_69.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_70.png)

`Host` 头部是HTTP请求的一部分，它指明了客户端意图访问的主机名。在DNS重绑定攻击中，尽管请求到达了本地服务器的IP，但 `Host` 头部仍然是 `attacker.com`。

因此，本地服务器应该：
1.  明确知道自己应该服务于哪个主机名（例如 `localhost:8080` 或 `mydevice.local`）。
2.  对于每一个传入的请求，检查其 `Host` 头部是否与预期的主机名**严格匹配**。
3.  如果不匹配，则立即拒绝该请求。

![](img/1bb76722691ddd2e6b1a537a70a84c3a_72.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_74.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_76.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_78.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_79.png)

### 防御代码示例

![](img/1bb76722691ddd2e6b1a537a70a84c3a_81.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_83.png)

以下是一个Node.js Express服务器的中间件示例，用于防御DNS重绑定攻击：

```javascript
const express = require('express');
const app = express();
const PORT = 8080;

![](img/1bb76722691ddd2e6b1a537a70a84c3a_85.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_87.png)

// 防御DNS重绑定的中间件
app.use((req, res, next) => {
    const expectedHost = `localhost:${PORT}`; // 期望服务的主机名
    if (req.headers.host !== expectedHost) {
        // 如果Host头部不匹配，则拒绝请求
        console.warn(`可能的DNS重绑定攻击！来自: ${req.headers.host}`);
        return res.status(403).send('禁止访问：主机头验证失败。\n');
    }
    // 验证通过，继续处理请求
    next();
});

// 你的业务逻辑路由
app.put('/launch', (req, res) => {
    // 执行敏感操作...
    res.send('操作成功\n');
});

![](img/1bb76722691ddd2e6b1a537a70a84c3a_89.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_91.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_93.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_95.png)

app.listen(PORT, () => {
    console.log(`本地服务器运行在 http://localhost:${PORT}`);
});
```

![](img/1bb76722691ddd2e6b1a537a70a84c3a_97.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_98.png)

**核心防御逻辑公式：**
`if (request.headers.host != expected_host) { deny_request(); }`

![](img/1bb76722691ddd2e6b1a537a70a84c3a_100.png)

通过添加这简单的检查，本地服务器可以确保只有明确指向其正确主机名的请求才会被处理，从而彻底免疫DNS重绑定攻击。

## 其他注意事项与总结 📝

![](img/1bb76722691ddd2e6b1a537a70a84c3a_102.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_104.png)

*   **避免使用本地HTTP服务器**：最好的安全实践是尽可能避免在客户端运行本地HTTP服务器。许多功能可以通过注册自定义URL协议处理器等方式实现。
*   **防火墙的局限性**：网络防火墙可以阻止来自互联网的主动入站连接，但DNS重绑定攻击利用的是用户浏览器主动发起的出站请求，从而绕过了这层防护。
*   **攻击的局限性**：虽然DNS重绑定可以用于攻击本地服务和内网设备，但它通常**无法用于直接攻击其他互联网服务**（如银行网站），因为：
    *   浏览器不会发送目标站点的Cookies（同源策略）。
    *   使用HTTPS时会出现证书域名不匹配的错误。

![](img/1bb76722691ddd2e6b1a537a70a84c3a_106.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_108.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_109.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_111.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_113.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_114.png)

![](img/1bb76722691ddd2e6b1a537a70a84c3a_116.png)

**本节课总结：**
本节课我们一起深入学习了**DNS重绑定攻击**。我们了解到，攻击者通过动态操控DNS解析记录，欺骗浏览器将跨域请求误判为同源请求，从而绕过CORS和同源策略的保护。这种攻击对本地服务器和内部网络设备构成严重威胁。最终的防御方案并不复杂，但至关重要：**任何面向网络的服务器（尤其是本地服务）都必须在其业务逻辑中，严格校验HTTP请求的 `Host` 头部，确保它与预期的服务地址完全一致。** 在设计和实现涉及本地网络通信的应用时，务必牢记这一安全原则。