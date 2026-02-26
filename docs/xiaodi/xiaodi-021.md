#  021：信息打点补充篇 - 公众号服务、Github监控、供应链与网盘泄漏等进阶资产收集 🎯

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_1.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_3.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_5.png)

在本节课中，我们将学习信息收集的进阶技巧。这些方法主要针对网络空间中的非直接攻击面，例如供应链、社会工程学与漏洞的联动利用。课程将围绕四个真实案例展开，并详细讲解其中涉及的核心知识点。

## 概述

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_7.png)

本节课是信息打点部分的最后一讲，内容是对之前知识的额外补充。虽然知识点较为零散，但在实战中，尤其是在护网等场景下，这些方法对于发现非常规攻击路径至关重要。我们将重点讲解四个方向：**微信公众号服务资产收集**、**Github监控与信息泄露利用**、**供应链信息收集**以及**网盘泄漏、证书、图标、邮箱等资产发现**。

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_9.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_11.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_13.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_14.png)

---

## 一、 实战案例引入

在讲解具体知识点前，我们先回顾四个来自真实护网环境的案例，它们展示了如何利用这些进阶技巧打开突破口。

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_16.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_18.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_20.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_21.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_22.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_24.png)

以下是四个案例的简要说明：

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_26.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_28.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_30.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_32.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_34.png)

1.  **案例一：招标平台供应链攻击**
    *   在某央企项目的采购网上收集到一个邮箱。
    *   通过历史泄露库查询到该邮箱的旧密码。
    *   利用旧密码成功碰撞登录该邮箱。
    *   从邮箱附件中发现内部资产信息（如APP地址），进而通过反编译和漏洞利用获取Webshell。

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_36.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_38.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_40.png)

2.  **案例二：OA系统人员信息利用**
    *   目标是一个OA系统，常规漏洞利用失败。
    *   通过“爱企查”等平台查询公司关键人物（如董事长、法人）。
    *   尝试使用“人名+弱口令”的组合进行爆破，成功登录OA系统。

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_42.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_44.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_46.png)

3.  **案例三：邮箱弱口令爆破**
    *   信息收集阶段整理出目标相关的一批邮箱。
    *   生成“邮箱+常见弱口令”的字典。
    *   利用该字典批量尝试登录各类业务系统，获得突破。

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_48.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_50.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_52.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_54.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_56.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_58.png)

4.  **案例四：Github泄露导致内网沦陷**
    *   目标IP范围不准，且禁止钓鱼。
    *   在Github上发现目标泄露的邮箱和密码。
    *   登录邮箱后，发现内部系统账号密码及VPN信息。
    *   通过VPN进入内网，进行后续渗透。

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_60.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_62.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_64.png)

这些案例的共同点是：**当正面攻击失效时，转而从人员、供应链、历史泄露信息等侧面寻找切入点**。接下来，我们将分解这些案例中用到的关键技术。

---

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_66.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_68.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_70.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_72.png)

## 二、 微信公众号服务资产收集 🔍

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_74.png)

微信公众号可能成为资产发现的入口。核心思路是：**寻找目标企业的微信公众号，并检查其是否使用了第三方（非腾讯官方）的服务接口**。

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_76.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_78.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_80.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_82.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_84.png)

**操作分为两步：**

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_86.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_88.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_90.png)

1.  **获取目标微信公众号途径：**
    *   **企业信息平台：** 在“爱企查”等平台的公司信息中，有时会列出其微信公众号。
    *   **搜索引擎：** 使用搜狗微信搜索（`weixin.sogou.com`）直接搜索公司名称或相关关键词。
    *   **微信内搜索：** 直接在微信“搜一搜”中搜索公司名，查看其公众号及历史文章。

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_92.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_94.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_96.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_98.png)

2.  **判断是否存在第三方服务资产：**
    *   关注找到的公众号，查看其菜单栏、自动回复等功能。
    *   关键点：**如果点击某个功能后，跳转的域名不是 `qq.com`、`weixin.qq.com` 等腾讯官方域名，而是跳转到其他独立域名，那么这个域名就是你需要收集的资产**。
    *   例如，公众号的“会员绑定”、“在线商城”、“预约服务”等，常常会跳转到企业自有的网站。

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_100.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_102.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_104.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_106.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_108.png)

**核心概念：**
*   资产判定公式：`跳转域名 ∉ 腾讯官方域名集合` => `该域名为目标资产`

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_110.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_112.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_114.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_116.png)

---

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_118.png)

## 三、 Github监控与信息泄露利用 💻

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_120.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_122.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_124.png)

Github是全球最大的开源代码托管平台。开发或运维人员可能无意中将包含敏感信息（如密码、密钥、内部架构图）的代码上传至此。这是信息收集的“富矿”。

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_126.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_128.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_130.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_132.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_134.png)

上一节我们介绍了Github搜索，本节我们来看看如何对其进行监控。

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_136.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_138.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_140.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_142.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_144.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_146.png)

### 1. 主动搜索（基于筛选）

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_148.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_150.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_152.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_154.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_156.png)

你可以使用Github的搜索语法，针对特定目标进行筛选。

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_158.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_160.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_162.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_164.png)

**常用搜索语法示例：**
*   按域名搜索敏感信息：`in:file “target.com” password`
*   按邮箱搜索：`“target@domain.com” password`
*   按公司名/人名搜索：`“公司名称” OR “人名” config`

**操作流程：**
1.  在Github搜索框输入构造好的语法。
2.  在结果中仔细查看代码片段、配置文件（如 `.env`、`config.php`、`application.yml`），寻找数据库连接字符串、API密钥、邮箱密码等。
3.  下载可疑项目到本地，进行全局关键词搜索，追踪敏感信息的来源和用途。

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_166.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_168.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_170.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_172.png)

### 2. 被动监控（长期监控）

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_174.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_176.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_178.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_180.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_182.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_184.png)

对于重点目标，可以搭建监控系统，持续监控Github上新出现的相关泄露。

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_186.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_188.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_190.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_192.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_194.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_196.png)

**以下是两个推荐的监控项目：**

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_198.png)

*   **项目A（Web界面）：** 一个用于监控Github代码泄露的系统。
    *   **功能：** 可添加多个监控任务（关键词），设定爬取间隔（如每小时），通过Web界面或配置钉钉/微信机器人接收告警。
    *   **应用场景：** 监控目标公司域名、邮箱、关键词，以及监控新爆出的CVE漏洞情报。
    *   **搭建简述：**
        ```bash
        # 根据项目README，通常需要安装Python依赖
        pip install -r requirements.txt
        # 配置数据库和告警设置
        # 启动Web服务
        python app.py
        ```

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_200.png)

*   **项目B（命令行工具）：** 一个专注于监控特定Github仓库变动的工具。
    *   **功能：** 监控指定仓库的代码提交，当代码发生变更或出现预设规则（如包含`password`明文）的关键词时告警。
    *   **应用场景：** 更适合蓝队或代码审计场景，用于防止内部代码违规上传或监控第三方组件安全更新。

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_202.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_204.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_206.png)

**核心概念：**
*   监控逻辑：`持续执行(Github搜索API(关键词))` -> `发现新结果` -> `触发告警`
*   意义：**将一次性的信息收集，转变为持续性的威胁感知。**

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_208.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_210.png)

---

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_212.png)

## 四、 网盘泄漏搜索 📁

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_214.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_216.png)

一些员工可能将工作文件（如招标书、项目计划、通讯录、系统手册）上传到公共网盘并设置了分享。通过网盘搜索工具，有可能发现这些泄露信息。

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_218.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_220.png)

**操作方法：**
1.  使用网盘搜索工具（课程提供了相关工具）。
2.  以目标公司名称、项目名称、产品名称等作为关键词进行搜索。
3.  在结果中筛选，查看是否有包含敏感信息的文档、表格、源代码压缩包等。

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_222.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_224.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_226.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_228.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_230.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_232.png)

**注意：** 此方法成功率不稳定，严重依赖于是否有员工确实进行了公开分享操作，但作为一种补充手段值得尝试。

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_234.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_236.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_238.png)

---

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_240.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_242.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_244.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_246.png)

## 五、 网络空间搜索引擎进阶用法 🛰️

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_248.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_250.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_252.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_254.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_256.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_258.png)

除了基本的IP、域名搜索，网络空间搜索引擎（如Fofa、Quake）还提供了一些进阶资产关联能力。

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_260.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_262.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_264.png)

### 1. 证书（SSL/TLS）资产关联
同一个SSL证书可能被多个域名或IP使用。通过搜索证书特征，可以发现关联资产。
*   **语法示例（Fofa）：** `cert="target.com"` 或 `cert.subject="Organization Name"`

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_266.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_268.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_270.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_272.png)

### 2. 图标（Favicon）资产关联
网站图标（favicon.ico）的哈希值是唯一的。通过上传目标网站的图标，可以搜索到使用了同一图标的所有资产。
*   **操作：** 在Fofa、Quake的“图标搜索”功能中，上传从目标网站获取的 `favicon.ico` 文件即可。

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_274.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_276.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_278.png)

### 3. 邮箱资产关联
在搜索引擎中直接搜索邮箱地址，有时能发现该邮箱注册过的其他网站或系统，这些都可能成为攻击面。
*   **操作：** 在搜索框中直接输入邮箱地址进行搜索。

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_280.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_282.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_284.png)

---

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_286.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_287.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_289.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_291.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_293.png)

## 六、 敏感目录与文件扫描 📂

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_295.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_297.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_299.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_301.png)

这是一种传统但依然有效的信息收集手段，旨在发现网站暴露的备份文件、配置文件、管理后台、测试页面等。

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_303.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_305.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_307.png)

**核心工具：**
*   **目录扫描器：** 如 `Dirsearch`、`Dirb`、`ffuf`。
*   **爬虫工具：** 如 `Burp Suite` 的爬虫功能、`AWVS` 等扫描器内置的爬虫。

**基本原理：**
工具会按照一个庞大的字典，向目标网站发送形如 `http://target.com/{字典词条}` 的请求，根据HTTP状态码（如200、403、500）和响应内容来判断该路径是否存在且有价值。

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_309.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_311.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_313.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_315.png)

**注意：** 对于采用现代前端框架（如React、Vue）或严格路由控制的网站，此方法效果会大打折扣。

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_317.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_319.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_321.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_323.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_325.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_327.png)

**代码示例（Dirsearch 基本用法）：**
```bash
python3 dirsearch.py -u http://target.com -e php,html,js,bak,zip
```

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_328.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_329.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_330.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_332.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_334.png)

---

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_336.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_338.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_340.png)

## 总结

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_342.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_344.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_346.png)

本节课我们一起学习了信息打点的多种进阶技巧：

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_348.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_350.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_352.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_354.png)

1.  **微信公众号资产收集**：关键在于发现并利用其跳转至第三方服务的链接。
2.  **Github信息泄露利用**：这是本节课的重点，包括主动搜索敏感代码和搭建系统进行长期监控，是发现数据库密码、API密钥、内部系统地址的绝佳途径。
3.  **网盘泄漏搜索**：作为补充手段，寻找员工意外公开的内部文件。
4.  **网络空间引擎进阶**：利用证书、图标、邮箱进行资产关联，扩大攻击面。
5.  **敏感目录扫描**：使用自动化工具探测隐藏的路径和文件。

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_356.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_358.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_360.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_362.png)

这些方法的核心思想是 **“正面不行，走侧面”** 。当直接的技术漏洞难以发现时，从人员、供应链、运维习惯、历史数据等角度切入，往往能打开新的突破口。掌握这些技巧，能让你的信息收集工作更加立体和全面。

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_364.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_366.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_368.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_370.png)

![](img/df3a7073d30f8efbe49c06e84a5e1f2b_372.png)

从下节课开始，我们将进入新的篇章——**开发基础**的学习，为后续的漏洞分析与利用打下坚实基础。