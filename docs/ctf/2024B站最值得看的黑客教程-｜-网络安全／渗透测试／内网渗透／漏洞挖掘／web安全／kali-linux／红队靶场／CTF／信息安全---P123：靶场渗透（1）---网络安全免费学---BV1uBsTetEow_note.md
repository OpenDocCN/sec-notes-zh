# 网络安全入门：P123：靶场渗透（1）

![](img/a499578bddee1da1dcc078249be48a36_1.png)

![](img/a499578bddee1da1dcc078249be48a36_3.png)

![](img/a499578bddee1da1dcc078249be48a36_4.png)

![](img/a499578bddee1da1dcc078249be48a36_6.png)

![](img/a499578bddee1da1dcc078249be48a36_8.png)

![](img/a499578bddee1da1dcc078249be48a36_10.png)

在本节课中，我们将学习对一个内网靶场进行初步渗透测试的完整流程。我们将从确定目标开始，逐步进行信息收集，并利用发现的敏感信息获取初步访问权限。整个过程将使用Kali Linux自带的工具完成，适合初学者理解和实践。

![](img/a499578bddee1da1dcc078249be48a36_12.png)

![](img/a499578bddee1da1dcc078249be48a36_14.png)

![](img/a499578bddee1da1dcc078249be48a36_15.png)

![](img/a499578bddee1da1dcc078249be48a36_17.png)

## 确定目标 🎯

![](img/a499578bddee1da1dcc078249be48a36_19.png)

首先，我们需要确定攻击目标。本次渗透测试的目标是一台IP地址为 `192.168.80.150` 的服务器。这台服务器位于内网环境中，因此外部无法直接访问。如果你想进行相同的练习，可以在课后获取相同的靶场环境。

![](img/a499578bddee1da1dcc078249be48a36_21.png)

![](img/a499578bddee1da1dcc078249be48a36_23.png)

![](img/a499578bddee1da1dcc078249be48a36_24.png)

![](img/a499578bddee1da1dcc078249be48a36_26.png)

## 信息收集 🔍

![](img/a499578bddee1da1dcc078249be48a36_28.png)

![](img/a499578bddee1da1dcc078249be48a36_30.png)

![](img/a499578bddee1da1dcc078249be48a36_31.png)

![](img/a499578bddee1da1dcc078249be48a36_33.png)

![](img/a499578bddee1da1dcc078249be48a36_35.png)

上一节我们确定了目标，本节中我们来看看如何进行信息收集。对于单一的内网靶机，信息收集的核心是扫描其开放的端口、运行的服务以及操作系统版本。

![](img/a499578bddee1da1dcc078249be48a36_37.png)

![](img/a499578bddee1da1dcc078249be48a36_39.png)

我们将使用Kali Linux自带的强大工具 **Nmap** 来完成这项任务。

![](img/a499578bddee1da1dcc078249be48a36_41.png)

![](img/a499578bddee1da1dcc078249be48a36_43.png)

![](img/a499578bddee1da1dcc078249be48a36_45.png)

以下是执行扫描的命令及其解释：
```bash
nmap -sT -T4 192.168.80.150
```
*   **`nmap`**: 网络扫描工具。
*   **`-sT`**: 指定进行TCP连接扫描，这是最可靠的扫描方式，能发现基于TCP的服务（如网站、数据库）。
*   **`-T4`**: 指定扫描速度，`T4`代表较快的扫描速度。
*   **`192.168.80.150`**: 目标IP地址。

![](img/a499578bddee1da1dcc078249be48a36_47.png)

执行扫描后，我们获得了目标的端口和服务信息。在结果中，我们重点关注两个服务：
1.  **HTTP (端口 80)**: 表示目标运行着一个网站。
2.  **MySQL (端口 3306)**: 表示目标运行着MySQL数据库。

发现网站和数据库服务后，我们下一步自然是想知道网站的具体内容。

## 网站目录枚举 📂

![](img/a499578bddee1da1dcc078249be48a36_49.png)

仅仅知道有网站还不够，我们需要了解这个网站由哪些页面构成，以寻找潜在的脆弱入口。

![](img/a499578bddee1da1dcc078249be48a36_51.png)

![](img/a499578bddee1da1dcc078249be48a36_53.png)

我们将使用Kali自带的另一个工具 **Dirb** 来暴力破解网站的目录和文件。

![](img/a499578bddee1da1dcc078249be48a36_55.png)

![](img/a499578bddee1da1dcc078249be48a36_57.png)

![](img/a499578bddee1da1dcc078249be48a36_59.png)

![](img/a499578bddee1da1dcc078249be48a36_61.png)

![](img/a499578bddee1da1dcc078249be48a36_63.png)

以下是执行目录扫描的命令：
```bash
dirb http://192.168.80.150
```
*   **`dirb`**: 网站目录暴力破解工具。
*   **`http://192.168.80.150`**: 目标网站的地址。

![](img/a499578bddee1da1dcc078249be48a36_65.png)

![](img/a499578bddee1da1dcc078249be48a36_67.png)

扫描完成后，Dirb会列出它发现的所有可能存在的页面路径。从这些结果中，我们筛选出几个关键页面：
1.  网站首页 (`/`)
2.  数据库管理平台 (`/phpmyadmin`)
3.  一个名为Joomla的博客系统后台 (`/administrator`)

![](img/a499578bddee1da1dcc078249be48a36_69.png)

![](img/a499578bddee1da1dcc078249be48a36_71.png)

## 利用信息泄露获取凭证 🔑

![](img/a499578bddee1da1dcc078249be48a36_73.png)

![](img/a499578bddee1da1dcc078249be48a36_75.png)

在目录枚举结果中，我们发现了一个名为 `/.git` 的目录。这通常意味着网站开发者在部署时，不小心将代码版本控制仓库（Git）也上传到了服务器上，造成了 **Git信息泄露**。

![](img/a499578bddee1da1dcc078249be48a36_77.png)

Git信息泄露允许攻击者下载网站的完整后端源代码。我们可以使用一个名为 `git-dump` 的Python脚本来利用此漏洞。

![](img/a499578bddee1da1dcc078249be48a36_79.png)

以下是利用Git泄露下载源代码的命令：
```bash
python3 git-dump.py http://192.168.80.150/.git
```
脚本运行后，会将服务器的源代码下载到本地的一个文件夹中。

![](img/a499578bddee1da1dcc078249be48a36_80.png)

![](img/a499578bddee1da1dcc078249be48a36_82.png)

获取源代码后，我们的目标是寻找其中可能包含的敏感配置信息，例如数据库密码。配置文件通常包含 `config` 或 `configuration` 等关键词。

在下载的源代码中，我们找到了一个名为 `configuration.php` 的文件。查看其内容后，我们成功发现了两处疑似密码的字段：
*   `password`
*   `secret`

通过上下文分析，我们确定 `password` 字段的值 `2166621666` 极有可能是数据库的登录密码。

## 获取初步访问权限 🚪

![](img/a499578bddee1da1dcc078249be48a36_84.png)

现在，我们手头有数据库密码，而之前信息收集时发现了 `/phpmyadmin`（数据库管理平台）。逻辑上，下一步就是尝试用这个密码登录数据库。

![](img/a499578bddee1da1dcc078249be48a36_86.png)

我们访问 `http://192.168.80.150/phpmyadmin`，使用用户名 `root` 和密码 `2166621666` 进行登录。登录成功！这意味着我们获得了目标MySQL数据库的最高管理权限。

## 总结 📝

![](img/a499578bddee1da1dcc078249be48a36_88.png)

本节课中我们一起学习了针对一个内网靶场的初步渗透测试流程：
1.  **目标确定**：明确了要攻击的IP地址。
2.  **信息收集**：使用Nmap扫描目标，发现了HTTP网站和MySQL数据库服务。
3.  **深度枚举**：使用Dirb扫描网站目录，发现了关键的管理后台页面和 `.git` 泄露目录。
4.  **利用漏洞**：通过Git信息泄露漏洞下载网站源代码。
5.  **提取凭证**：在源代码的配置文件中找到了数据库的明文密码。
6.  **获取权限**：使用找到的密码成功登录phpMyAdmin，获得了数据库控制权。

![](img/a499578bddee1da1dcc078249be48a36_90.png)

![](img/a499578bddee1da1dcc078249be48a36_92.png)

![](img/a499578bddee1da1dcc078249be48a36_94.png)

![](img/a499578bddee1da1dcc078249be48a36_96.png)

这个过程清晰地展示了渗透测试中“信息收集-发现漏洞-利用漏洞-获取权限”的基本链条。下一节课，我们将探讨在获得数据库权限后，如何进一步获取服务器的完整控制权。