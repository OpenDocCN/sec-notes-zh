# 课程 01: AppleStorm - 揭露 Apple Intelligence 的隐私风险 🍎⚡

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_1.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_3.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_4.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_5.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_7.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_9.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_11.png)

在本节课中，我们将学习 Apple Intelligence 这一苹果公司新推出的 AI 套件，并深入探讨其背后可能存在的隐私风险。我们将通过实际的技术分析，了解用户数据在本地设备与苹果服务器之间是如何流动的。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_13.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_15.png)

---

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_16.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_18.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_19.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_21.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_22.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_24.png)

## 概述

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_26.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_27.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_29.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_30.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_31.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_33.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_34.png)

Apple Intelligence 是苹果公司于去年九月推出的新 AI 功能套件，旨在提升生产力并承诺保护用户数据隐私。它包含写作工具、图像工具，并大幅增强了 Siri 的能力。然而，这些新功能在实际运作中，用户数据是否真的如宣传所言得到妥善保护？本节课将带你一探究竟。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_35.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_37.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_39.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_40.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_41.png)

上一节我们介绍了课程主题，本节中我们来看看 Apple Intelligence 的基本架构。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_43.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_44.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_45.png)

## Apple Intelligence 的架构

根据苹果的隐私政策，其 AI 架构主要包含两组模型：

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_47.png)

1.  **设备端模型**：这些模型直接在用户的 Mac 或 iOS 设备上运行，无需网络通信。
    *   `模型推理(用户设备)`
2.  **云端模型**：这些是更强大的语言和图像模型，运行在苹果的“私有云计算”服务器上。
    *   `模型推理(苹果服务器)`

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_49.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_50.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_52.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_54.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_56.png)

这两组模型协同工作。一个运行在用户设备上的“路由模型”会判断当前任务能否完全在本地处理。如果不能，它会将**相关数据**发送到云端，利用更大的模型来完成用户请求。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_58.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_60.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_62.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_63.png)

然而，苹果的 AI 生态系统不仅包含私有云计算。以下是其三大核心基础设施组件：

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_64.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_66.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_68.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_69.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_71.png)

*   **私有云计算**：处理复杂的 AI 任务。
*   **Siri 基础设施**：
    *   **听写服务器** (`guzzoni.apple.com`)：负责语音听写。
    *   **搜索服务** (`smoothth.apple.com`)：处理在线或苹果生态内的搜索请求。
*   **Apple Intelligence 扩展**：用于集成第三方服务，例如 ChatGPT。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_73.png)

了解了基本架构后，接下来我们将聚焦于潜在的风险。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_75.png)

## 隐私风险研究方法

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_77.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_79.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_81.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_82.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_84.png)

为了评估风险，我们需要明确两点：

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_86.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_87.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_89.png)

1.  哪些功能完全在设备端运行，无需网络通信。
2.  哪些功能需要借助私有云计算或其他苹果基础设施（如 Siri 服务器）来完成。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_91.png)

当功能需要联网时，最关键的问题是：**哪些数据从我们的设备发送到了苹果的服务器？**

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_93.png)

我的研究方法是通过技术手段，拦截 Siri、写作工具、图像工具与苹果服务器之间的所有通信，并分析外传的数据内容。

但这并非易事。虽然 Apple Intelligence 核心功能会记录发送到私有云计算的请求，便于在设备上查看，但 Siri 的通信分析则困难得多。

Siri 使用了一种称为 **证书锁定** 或 **SSL Pinning** 的安全技术。这项技术使应用程序能够验证远程服务器确实是苹果的官方服务器，从而拒绝任何其他服务器（例如用于中间人攻击的服务器）的连接。这使得直接解密和分析 Siri 的通信流量变得非常困难。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_95.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_97.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_99.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_100.png)

经过数月的努力，我成功绕过了苹果的证书锁定机制（仅在我自己的设备上），并解密了其背后的所有通信协议。在接下来的演示中，我们将看到发送到苹果服务器的实际数据。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_102.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_103.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_104.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_105.png)

在开始演示前，需要设定一个基线：在研究的初始阶段，我确保关闭了所有“从此 App 学习”的设置选项，以避免预期之外的数据共享。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_107.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_109.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_110.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_111.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_112.png)

## 风险演示与分析

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_113.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_115.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_116.png)

### 1. Siri 与上下文数据泄露

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_118.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_120.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_121.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_122.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_123.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_124.png)

我们首先测试 Siri。借助 Apple Intelligence，Siri 新增了打字输入功能。我尝试了一个简单的、必须联网才能回答的问题：“拉斯维加斯今天的天气怎么样？”

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_126.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_127.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_128.png)

以下是发送到搜索服务 (`smoothth.apple.com`) 的数据包内容分析：

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_130.png)

*   **精确位置信息**：即使用户只是询问天气，Siri 也会在**每个请求**中默认发送设备的位置信息。用户可以在设置中关闭此功能。
*   **已安装的应用列表**：Siri 会在设备上进行“主题建模”，根据用户询问的话题（如天气、邮件、编码），查找设备上所有相关的应用（**包括虚拟机内的应用**），并将应用名称发送给服务器。
*   **“正在播放”队列的元数据**：如果用户正在通过任何应用（如 YouTube、VLC）播放音频或视频，该媒体文件的元数据（如歌曲名、视频标题）也会被发送到苹果的听写服务器和搜索服务。即使你只是暂停播放并切换到其他应用，只要该媒体仍在“正在播放”队列中，其信息就可能被发送。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_132.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_133.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_134.png)

**总结**：一个简单的天气查询，触发了大量与核心问题无关的上下文数据（位置、应用列表、媒体元数据）被发送至苹果服务器。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_136.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_138.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_140.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_141.png)

### 2. Siri 与消息应用集成

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_143.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_145.png)

接下来测试 Siri 与第三方应用的集成，例如通过 Siri 发送 WhatsApp 消息。

研究发现，当使用 Siri 发送 WhatsApp 或 iMessage 时，**消息内容、联系人姓名和电话号码会被发送到苹果的听写服务器**。然而，当我完全阻断设备与苹果服务器的通信后，Siri 仍然能成功发送消息。这表明，向服务器发送消息数据**并非核心功能所必需**。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_147.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_148.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_149.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_151.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_152.png)

相比之下，使用 Siri 撰写邮件或查看日历时，邮件正文或日历事件内容**并未**被发送到服务器。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_154.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_155.png)

### 3. 写作工具与图像工具

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_157.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_158.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_159.png)

与 Siri 相比，Apple Intelligence 的写作工具和图像工具在隐私保护方面表现更好。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_161.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_162.png)

*   **写作工具**：大部分功能（如重写、总结）都在设备端完成。少数需要联网的功能（如“从主题生成”）会明确标识，并且只将必要数据发送至私有云计算，没有发现无关数据泄露。
*   **图像工具**：所有图像生成和编辑功能都完全在设备端运行，未发现任何图像或提示词被发送至苹果服务器。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_164.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_165.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_167.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_168.png)

### 4. ChatGPT 扩展

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_170.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_171.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_173.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_175.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_176.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_177.png)

当通过 Apple Intelligence 使用 ChatGPT 时，需要注意两点：

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_179.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_180.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_181.png)

1.  所有请求并非直接发送给 OpenAI，而是先经过苹果的“Apple Intelligence 扩展”服务器。
2.  即使用户已登录 ChatGPT 账户，所有通信仍需经过苹果服务器。
3.  更令人担忧的是，发送给 ChatGPT 的提示词**会被同时发送给两个端点**：Apple Intelligence 扩展服务器和 Siri 的搜索服务。用户只能看到前者的回复，不清楚后者为何需要这些数据。

## 与苹果的沟通及现状

我将所有发现披露给了苹果公司。他们的回应可以概括为：

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_183.png)

1.  承认部分 Siri 行为异常（如发送不必要的数据），并承诺由工程团队调查修复。
2.  强调许多问题（如听写服务器、搜索服务的数据收集）属于 **Siri 核心服务** 的范畴，而非 **Apple Intelligence** 的一部分，尽管在用户体验上二者已深度整合。
3.  表示将改进关于 ChatGPT 集成和私有云计算架构的文档清晰度。

## 风险缓解建议

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_185.png)

在苹果彻底解决问题之前，用户可以采取以下措施来降低风险：

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_187.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_188.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_190.png)

1.  **阻断听写服务器**：由于听写服务器的部分数据收集似乎非必需，用户可以通过网络设置阻止设备与 `guzzoni.apple.com` 的通信，这能在不影响核心功能的情况下减少部分数据泄露。
2.  **审阅隐私设置**：立即检查“Siri 与 Apple Intelligence”设置，关闭所有你不想共享数据的应用下方的“从此 App 学习”选项。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_191.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_192.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_194.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_195.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_197.png)

## 总结与思考

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_199.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_201.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_203.png)

本节课中我们一起学习了 Apple Intelligence 的架构，并通过实际技术分析，揭示了其生态中（特别是 Siri 服务）存在的隐私风险，包括不必要的位置信息、应用列表、媒体元数据乃至消息内容的泄露。

本次研究带来几点重要启示：

1.  **隐私政策的重要性**：在 AI 时代，我们必须更关注我们共享了哪些数据。可以尝试利用 AI 工具快速总结冗长的隐私政策。
2.  **企业治理与透明度**：组织需要工具来监控离开其网络的数据。而像**证书锁定**这样的安全技术，在保护通信的同时，也阻碍了研究和企业进行必要的安全监控。在涉及 AI 和数据上传的场景中，需要在安全与透明度之间找到平衡。
3.  **功能界线的模糊性**：苹果将 Siri 与 Apple Intelligence 在技术上区分开来，但用户体验上它们是一体的。用户可能无法区分一次查询是由“旧版 Siri”处理，还是由“新版 Apple Intelligence”或“集成的 ChatGPT”处理，而这背后对应着不同的数据处理策略和隐私条款。

最终，作为用户，我们需要在享受 AI 带来的便利时，保持对数据隐私的清醒认识。作为行业，则需要推动更高的透明度和更清晰的责任界定。