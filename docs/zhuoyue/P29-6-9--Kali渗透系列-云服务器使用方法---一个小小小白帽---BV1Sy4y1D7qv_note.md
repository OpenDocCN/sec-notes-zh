# P29：6.9-【Kali渗透系列】云服务器使用方法 🖥️

在本节课中，我们将学习如何购买并配置一台国外的VPS服务器。这对于后续进行安全或渗透测试至关重要，因为使用个人专属的服务器能避免共享带来的潜在风险。

## 为什么选择国外VPS？🌍

![](img/db7564f0ad90b506f3df3bd7015f111c_1.png)

上一节我们介绍了课程目标，本节中我们来看看选择国外服务器的原因。

![](img/db7564f0ad90b506f3df3bd7015f111c_3.png)

![](img/db7564f0ad90b506f3df3bd7015f111c_5.png)

不建议购买国内服务器，因为国内监管较为严格。而国外服务器（如BandwagonHost）政策相对宽松，且带宽条件通常更好。例如，国外服务器常提供1Gbps带宽，而国内同等带宽价格昂贵。国外服务器通常限制的是每月流量（如1TB），而非带宽速度。

**核心概念：带宽 vs 流量**
*   **带宽**：指网络连接的最大数据传输速率，单位如 Mbps 或 Gbps。`带宽 = 数据传输速率`
*   **流量**：指一段时间内（如每月）通过网络传输的数据总量，单位如 GB 或 TB。`月流量限额 = 1 TB`

**免责声明**：以下内容仅用于教学目的。

![](img/db7564f0ad90b506f3df3bd7015f111c_7.png)

![](img/db7564f0ad90b506f3df3bd7015f111c_9.png)

## 注册与购买流程 🛒

了解了服务器选择后，我们进入具体的购买环节。

### 访问官网与注册

以下是注册步骤：
1.  访问 BandwagonHost 官方网站。
2.  点击页面上的 **Register**（注册）按钮。
3.  填写注册表单。其中，邮箱（Email）必须填写正确，密码需确认（Confirm Password）。其他信息如地址（Address）、邮政编码（Zip Code）等可以酌情填写，不必完全真实。
4.  完成人机验证（Check Browser）后提交注册。

> **提示**：如果英文界面阅读困难，可以使用Chrome浏览器的网页翻译功能。

![](img/db7564f0ad90b506f3df3bd7015f111c_11.png)

### 选择与订购VPS

![](img/db7564f0ad90b506f3df3bd7015f111c_13.png)

注册成功后，即可购买VPS。

以下是推荐的VPS配置选择：
*   **产品**：20G KVM VPS
*   **价格**：$4.99/月
*   **配置**：CPU 2核，内存 1GB，硬盘 20GB，月流量 1TB，带宽 1Gbps。
*   **特点**：KVM虚拟化技术，性能较好；支持30天内新账户退款（实际操作中可能较难）。

![](img/db7564f0ad90b506f3df3bd7015f111c_15.png)

订购时，结算周期（Billing Cycle）选择默认即可，数据中心（Data Center）可选洛杉矶（Los Angeles）或其他。点击“Order Now”或“Add to Cart”加入购物车。

![](img/db7564f0ad90b506f3df3bd7015f111c_17.png)

![](img/db7564f0ad90b506f3df3bd7015f111c_19.png)

在结账（Checkout）页面，可以尝试使用提供的**促销代码**以获得优惠。支付方式支持支付宝（Alipay）和微信支付（WeChat Pay）。

## 服务器管理面板详解 ⚙️

![](img/db7564f0ad90b506f3df3bd7015f111c_21.png)

成功购买服务器后，我们来看看如何管理它。

![](img/db7564f0ad90b506f3df3bd7015f111c_23.png)

![](img/db7564f0ad90b506f3df3bd7015f111c_25.png)

![](img/db7564f0ad90b506f3df3bd7015f111c_27.png)

登录账户后，在“Services -> My Services”中可以找到已购买的VPS，状态为“Active”。点击管理按钮（如“KVM Control Panel”）进入控制面板。

控制面板提供了丰富的管理功能，以下是主要功能的中文说明：

*   **Start/Stop/Reboot**：开机、关机、重启服务器。
*   **Root Shell**：直接在网页端打开服务器的命令行终端，可以执行如 `ifconfig` 等命令。
*   **Install OS**：为服务器重新安装操作系统（会丢失所有数据，操作前建议关机）。
*   **Root Password Modification**：修改服务器的root用户密码。
*   **Snapshots**：创建或恢复服务器快照，用于系统备份和还原。
*   **Statistics**：查看服务器的详细统计信息，如资源使用情况。
*   **Two-Factor Authentication**：设置双重身份认证，提升账户安全性。

> **操作建议**：在进行重要操作（如重装系统）前，务必先创建**快照（Snapshot）**。

## 远程连接服务器 🔗

管理面板虽然方便，但更多操作需要通过SSH远程连接进行。

在控制面板的“Details”或“Statistics”中，可以找到服务器的IP地址和SSH端口号（通常不是默认的22）。

以下是使用SSH客户端（如Xshell）连接的步骤：
1.  打开SSH客户端，新建会话。
2.  主机（Host）填写服务器的IP地址。
3.  端口号（Port）填写控制面板中显示的SSH端口（例如 `28841`）。
4.  身份验证（Authentication）选择“Password”，用户名填写 `root`，密码填写控制面板中显示或你修改后的root密码。
5.  连接即可登录到服务器的命令行界面。

**连接代码示例（Linux/macOS 终端）**:
```bash
ssh root@你的服务器IP -p 你的SSH端口号
# 示例：ssh root@192.168.1.1 -p 28841
```
输入命令后，按提示输入密码即可登录。

![](img/db7564f0ad90b506f3df3bd7015f111c_29.png)

![](img/db7564f0ad90b506f3df3bd7015f111c_31.png)

## 总结 📝

![](img/db7564f0ad90b506f3df3bd7015f111c_33.png)

![](img/db7564f0ad90b506f3df3bd7015f111c_35.png)

本节课中我们一起学习了从零开始获取并使用一台VPS服务器的完整流程。

![](img/db7564f0ad90b506f3df3bd7015f111c_37.png)

![](img/db7564f0ad90b506f3df3bd7015f111c_39.png)

![](img/db7564f0ad90b506f3df3bd7015f111c_41.png)

我们首先探讨了选择国外VPS的原因，主要是出于政策与性价比的考虑。接着，我们一步步完成了在BandwagonHost官网的注册、选购合适配置的VPS以及支付。然后，详细介绍了服务器控制面板的各项核心功能，如开关机、重装系统、创建快照和修改密码等。最后，我们讲解了如何通过SSH工具远程连接到服务器，以便进行后续的深入操作。

![](img/db7564f0ad90b506f3df3bd7015f111c_43.png)

![](img/db7564f0ad90b506f3df3bd7015f111c_45.png)

![](img/db7564f0ad90b506f3df3bd7015f111c_47.png)

掌握独立VPS服务器的使用，是进行安全学习和实践的重要基础步骤。