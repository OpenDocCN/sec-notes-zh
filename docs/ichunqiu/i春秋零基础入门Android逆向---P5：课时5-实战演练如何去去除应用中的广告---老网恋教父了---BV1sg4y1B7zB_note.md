# i春秋零基础入门Android逆向 - P5：实战演练：去除应用广告 🛠️

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_1.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_3.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_5.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_7.png)

在本节课中，我们将学习如何通过逆向分析，定位并去除安卓应用中的广告。课程将分为两部分：首先，我们将回顾并完成上一节课留下的作业；其次，我们将学习针对网络验证型广告的分析与去除方法。

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_9.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_11.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_13.png)

## 回顾与作业讲解 📝

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_15.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_17.png)

上一节我们介绍了使用 `monitor` 工具查看程序日志来定位关键点的方法。本节中，我们来看看如何应用这些技巧来完成“滑雪大冒险”游戏的破解作业。

该作业的目标是破解游戏的内购。核心思路是通过分析程序运行时打印的日志，定位到支付相关的关键代码，并进行修改。

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_19.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_21.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_23.png)

以下是具体的操作步骤：

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_25.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_26.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_28.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_30.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_31.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_33.png)

1.  **运行游戏并监控日志**：使用 `monitor` 工具，在点击购买按钮前后，观察程序打印的日志信息。
2.  **定位关键字符串**：在日志中，可以找到如 `SNSPlay`、`BasePay` 等与支付相关的关键词。
3.  **搜索并分析关键类**：在反编译后的代码中，搜索上述关键词，定位到负责支付的核心类（例如 `SNSPlay` 相关的类）。
4.  **修改支付逻辑**：在该类中，找到开始支付的方法（如 `startPay`）。该方法通常会传入一个支付结果的回调接口（`Listener`）。我们的目标是绕过真实的支付流程，直接调用该回调接口的“支付成功”方法。
    *   回调接口通常包含三个方法：`onSuccess`, `onCancel`, `onFail`。
    *   修改思路是：在 `startPay` 方法中，不执行真实的支付请求，而是直接调用传入的 `Listener` 参数的 `onSuccess` 方法。
    *   关键代码修改示例（伪代码）：
        ```java
        // 原始代码
        public void startPay(..., Listener callback) {
            // 执行复杂的支付网络请求...
            realPayNetworkRequest();
        }

        // 修改后代码
        public void startPay(..., Listener callback) {
            // 直接调用成功回调，模拟支付成功
            callback.onSuccess();
            // 注释或删除原有的支付网络请求代码
            // realPayNetworkRequest();
        }
        ```
5.  **回编译与测试**：保存修改后的代码，重新打包并安装应用。测试购买功能，验证是否已破解成功。

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_35.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_37.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_39.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_41.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_43.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_45.png)

通过这个练习，我们巩固了通过日志分析、字符串搜索来缩小分析范围，并直接修改关键函数逻辑的破解方法。

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_47.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_49.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_50.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_52.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_54.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_56.png)

## 网络广告去除实战 🌐

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_58.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_60.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_62.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_64.png)

上一节我们处理了本地逻辑的修改，本节中我们来看看如何处理依赖网络请求的广告。我们将以优酷、土豆、腾讯视频等App为例，讲解如何去除其视频播放前的广告。

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_66.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_67.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_68.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_70.png)

核心思路是：**通过抓包分析，找到应用获取广告内容的网络请求地址，然后阻止该请求或使其返回空数据**。

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_72.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_73.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_75.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_77.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_79.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_80.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_81.png)

### 1. 抓包环境配置

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_83.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_84.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_85.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_87.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_89.png)

首先，我们需要使用抓包工具来监控应用的所有网络请求。这里推荐使用 `Fiddler` 或 `Charles`。

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_91.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_93.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_95.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_96.png)

以下是配置步骤：

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_98.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_99.png)

1.  确保电脑和手机在同一局域网下。
2.  在电脑上打开抓包工具，记住其代理地址（如 `192.168.1.100:8888`）。
3.  在手机的Wi-Fi设置中，为当前网络连接配置手动代理，填入电脑的IP地址和抓包工具的端口号。

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_101.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_103.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_105.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_107.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_109.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_111.png)

配置完成后，手机的所有HTTP/HTTPS流量都将经过电脑的抓包工具。

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_113.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_115.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_117.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_119.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_120.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_121.png)

### 2. 优酷/土豆广告去除

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_123.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_125.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_126.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_128.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_129.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_130.png)

对于优酷和土豆这类应用，其广告地址特征比较明显。

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_132.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_134.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_136.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_138.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_139.png)

以下是分析流程：

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_141.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_143.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_144.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_146.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_148.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_150.png)

1.  **启动抓包并播放视频**：配置好代理后，在手机上打开优酷App并播放任意视频。此时，抓包工具会捕获到大量网络请求。
2.  **定位广告请求**：在抓包工具的请求列表中，寻找包含明显广告关键词的URL，例如包含 `ad`、`adv` 或特定域名（如 `ad.api.3g.youku.com`）。
3.  **确认广告内容**：点击该请求，查看服务器返回的响应体（Response）。通常会包含指向 `.flv` 或 `.mp4` 视频文件的链接，这些就是广告视频的地址。
4.  **修改应用**：在反编译后的优酷应用代码中，全局搜索上一步找到的广告URL字符串。
5.  **移除或替换**：将搜索到的所有相关URL字符串替换为空字符串 `""`。这样，应用在尝试请求广告时，就无法构造出有效的地址。
    *   关键修改示例（伪代码）：
        ```java
        // 假设原始代码中广告地址是硬编码或拼接的
        String adUrl = "http://ad.api.3g.youku.com/getAd";
        // 修改后，该地址变为空，请求失效
        String adUrl = "";
        ```
6.  **回编译与测试**：重新打包安装修改后的App，测试视频播放，广告应已消失。

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_152.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_154.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_156.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_158.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_159.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_161.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_163.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_165.png)

土豆视频的分析和修改流程与优酷完全一致，因为两者同属一家公司，广告获取机制相同。

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_167.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_169.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_170.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_172.png)

### 3. 腾讯视频广告去除

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_174.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_176.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_178.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_180.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_181.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_183.png)

腾讯视频的广告机制稍复杂，其广告地址可能是动态生成的，直接搜索静态字符串可能无效。

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_184.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_186.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_188.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_189.png)

以下是应对策略：

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_191.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_193.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_195.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_196.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_198.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_199.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_201.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_203.png)

1.  **抓包定位**：同样通过抓包，找到返回广告列表的请求（响应内容可能是XML或JSON格式，包含 `adlist`、`materialurl` 等字段）。
2.  **搜索解析逻辑**：如果直接搜索广告URL无果，可以尝试搜索服务器返回数据中的关键字段，如 `ADLIST`。这有助于我们定位到解析广告数据的代码类。
3.  **分析数据解析类**：找到负责解析广告数据的类和方法。通常，它会有一个循环，用于遍历服务器返回的广告列表，并将每条广告数据添加到一个集合中。
4.  **修改解析逻辑**：我们的目标是让这个解析方法返回一个空的广告列表。可以通过修改代码，**注释掉将广告数据添加到结果列表的那行代码**来实现。
    *   关键修改示例（Smali代码逻辑）：
        *   原始逻辑：解析每条广告数据 -> 调用 `adList.add(adItem)`。
        *   修改后逻辑：解析每条广告数据 -> **跳过添加步骤** -> 返回空的 `adList`。
        *   这相当于在Smali代码中找到 `invoke-virtual` 或 `invoke-interface` 指令来调用 `add` 方法，并将其注释或修改为无效操作。
5.  **回编译与测试**：完成修改后，重新打包测试。视频播放前的广告应该已被去除。

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_205.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_207.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_209.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_211.png)

## 课程总结 🎯

本节课中我们一起学习了两种去除广告的实战方法：

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_213.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_214.png)

1.  **对于有本地逻辑验证的应用（如游戏内购）**：我们通过监控日志、定位关键字符串、分析并修改核心函数逻辑（如直接调用成功回调）来实现破解。
2.  **对于依赖网络请求的广告**：我们掌握了抓包分析这一核心技巧。通过配置代理、捕获网络请求、定位广告链接，然后通过修改应用代码（清空广告URL或破坏广告数据解析逻辑）来达到去广告的目的。

处理网络请求的关键在于耐心和分析。思路比工具更重要，清晰的思路能帮助我们快速缩小分析范围，定位到真正的关键点。

## 课后作业 📚

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_216.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_218.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_220.png)

请尝试对 **PPTV** 应用进行去广告修改，目标包括：
1.  去除视频播放前的贴片广告。
2.  去除应用启动时的开屏广告。
3.  去除退出应用时弹出的图片广告。

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_222.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_224.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_226.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_228.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_230.png)

![](img/22a3055ddffe4a3bf9fdc7190e8131cb_232.png)

请运用本节课所学的抓包和分析方法，独立完成定位和修改。