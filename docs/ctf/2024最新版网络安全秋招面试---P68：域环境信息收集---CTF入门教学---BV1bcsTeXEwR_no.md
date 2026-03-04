# 网络安全入门：P68：域环境信息收集 🔍

在本节课中，我们将学习如何在一个已攻陷的Windows主机上进行域环境信息收集。这是内网渗透测试中的关键一步，能帮助我们判断目标是否处于域环境，并收集关于域结构、用户、组和域控制器的关键信息。

![](img/404d5109e3311eee41f97d1c1a2a2afe_1.png)

上一节我们介绍了域的基本概念和组织架构。本节中，我们来看看如何通过一系列命令来探测和收集域环境信息。

![](img/404d5109e3311eee41f97d1c1a2a2afe_3.png)

![](img/404d5109e3311eee41f97d1c1a2a2afe_4.png)

## 判断是否处于域环境

![](img/404d5109e3311eee41f97d1c1a2a2afe_6.png)

当我们通过漏洞获取到一台主机的权限后，首先需要判断它是否属于一个域。一个简单的方法是使用 `net` 命令。

![](img/404d5109e3311eee41f97d1c1a2a2afe_7.png)

![](img/404d5109e3311eee41f97d1c1a2a2afe_8.png)

![](img/404d5109e3311eee41f97d1c1a2a2afe_10.png)

**命令示例：**
```cmd
net config workstation
```
执行此命令后，查看输出中的“工作站域”或“登录域”信息。如果显示的是域名称（而非本地计算机名），则说明该主机已加入域。

![](img/404d5109e3311eee41f97d1c1a2a2afe_12.png)

![](img/404d5109e3311eee41f97d1c1a2a2afe_14.png)

另一种方法是尝试查询域信息。在非域环境中，相关命令会报错。

![](img/404d5109e3311eee41f97d1c1a2a2afe_15.png)

**命令示例：**
```cmd
net view /domain
```
在域成员主机上，此命令会列出可用的域。在独立主机（工作组环境）上，通常会提示“指定的域不存在，或无法联系”。

![](img/404d5109e3311eee41f97d1c1a2a2afe_17.png)

![](img/404d5109e3311eee41f97d1c1a2a2afe_18.png)

## 收集域内基本信息

![](img/404d5109e3311eee41f97d1c1a2a2afe_20.png)

以下是收集域内用户、组和策略信息的核心命令。

![](img/404d5109e3311eee41f97d1c1a2a2afe_21.png)

![](img/404d5109e3311eee41f97d1c1a2a2afe_23.png)

![](img/404d5109e3311eee41f97d1c1a2a2afe_24.png)

**1. 查询域内所有组**
此命令可以列出域中的所有安全组。
```cmd
net group /domain
```

**2. 查询域内所有用户**
此命令可以列出域中的所有用户账户。
```cmd
net user /domain
```

![](img/404d5109e3311eee41f97d1c1a2a2afe_26.png)

![](img/404d5109e3311eee41f97d1c1a2a2afe_27.png)

![](img/404d5109e3311eee41f97d1c1a2a2afe_28.png)

![](img/404d5109e3311eee41f97d1c1a2a2afe_29.png)

**3. 查看域密码策略**
了解域的密码策略（如长度、复杂性、有效期）有助于后续的密码破解或攻击规划。
```cmd
net accounts /domain
```

![](img/404d5109e3311eee41f97d1c1a2a2afe_30.png)

**4. 查看指定用户的详细信息**
获取特定域用户的详细信息，包括全名、描述、登录时间等。
```cmd
net user [用户名] /domain
```
例如，要查看用户“boss”的详细信息：
```cmd
net user boss /domain
```

![](img/404d5109e3311eee41f97d1c1a2a2afe_32.png)

![](img/404d5109e3311eee41f97d1c1a2a2afe_33.png)

![](img/404d5109e3311eee41f97d1c1a2a2afe_34.png)

**5. 查看当前登录的域**
确认当前会话所属的域。
```cmd
echo %userdomain%
```

![](img/404d5109e3311eee41f97d1c1a2a2afe_36.png)

![](img/404d5109e3311eee41f97d1c1a2a2afe_38.png)

## 定位域控制器

![](img/404d5109e3311eee41f97d1c1a2a2afe_39.png)

![](img/404d5109e3311eee41f97d1c1a2a2afe_40.png)

![](img/404d5109e3311eee41f97d1c1a2a2afe_42.png)

域控制器是域环境的核心，定位它是内网渗透的重要目标。以下是几种定位方法。

![](img/404d5109e3311eee41f97d1c1a2a2afe_44.png)

![](img/404d5109e3311eee41f97d1c1a2a2afe_46.png)

**1. 通过组信息查询**
执行 `net group /domain` 命令时，输出结果顶部的“\\”后面的主机名通常就是域控制器的主机名。

![](img/404d5109e3311eee41f97d1c1a2a2afe_48.png)

**2. 查询域中所有控制器**
使用以下命令可以更直接地列出所有域控制器。
```cmd
net group “domain controllers” /domain
```

![](img/404d5109e3311eee41f97d1c1a2a2afe_50.png)

![](img/404d5109e3311eee41f97d1c1a2a2afe_51.png)

![](img/404d5109e3311eee41f97d1c1a2a2afe_53.png)

**3. 查看DNS后缀和主机名**
域控制器的DNS名称通常与域名相关。
```cmd
ipconfig /all
```
在输出中，查看“DNS 后缀”和“主机名”。域控制器的主机名通常包含“DC”、“AD”等标识，或其完整DNS名为 `主机名.域名` 格式。

![](img/404d5109e3311eee41f97d1c1a2a2afe_55.png)

**4. 通过端口扫描**
域控制器通常会开放一些特定服务端口，通过扫描内网可以辅助定位。
*   **389端口**：LDAP服务，用于目录访问。
*   **53端口**：DNS服务，域控制器常兼任DNS服务器。
*   **88端口**：Kerberos身份验证服务。
*   **445端口**：SMB服务，用于文件共享和通信。

![](img/404d5109e3311eee41f97d1c1a2a2afe_57.png)

![](img/404d5109e3311eee41f97d1c1a2a2afe_59.png)

如果发现内网中某台主机开放了389和53端口，它有很大概率是域控制器。

![](img/404d5109e3311eee41f97d1c1a2a2afe_60.png)

![](img/404d5109e3311eee41f97d1c1a2a2afe_62.png)

## 信息收集的意义

进行详尽的域信息收集是后续攻击的基础。了解清楚域结构、用户权限关系、密码策略以及域控制器的位置，才能有效地规划横向移动路径，最终目标是夺取域控制器的最高权限，控制整个域网络。

![](img/404d5109e3311eee41f97d1c1a2a2afe_64.png)

![](img/404d5109e3311eee41f97d1c1a2a2afe_66.png)

本节课中我们一起学习了在Windows域环境中进行信息收集的关键命令和方法，包括判断域成员身份、枚举域内用户和组、查看安全策略以及定位域控制器。掌握这些基础技能是深入理解内网渗透测试的第一步。请多加练习，熟悉这些命令的输出格式和含义。