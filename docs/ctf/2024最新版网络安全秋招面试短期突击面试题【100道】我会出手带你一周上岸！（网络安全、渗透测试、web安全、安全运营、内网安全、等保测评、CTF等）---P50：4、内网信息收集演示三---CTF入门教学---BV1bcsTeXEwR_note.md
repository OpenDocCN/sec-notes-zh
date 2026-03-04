# 网络安全入门：P50：4、内网信息收集演示三 🔍

![](img/c61616bc4597c7ae5e3257a035699404_1.png)

![](img/c61616bc4597c7ae5e3257a035699404_2.png)

![](img/c61616bc4597c7ae5e3257a035699404_4.png)

在本节课中，我们将学习内网信息收集的几种实用方法，包括代理信息、Wi-Fi密码、回收站内容以及浏览器历史记录的收集。这些技巧能帮助我们更深入地了解目标网络环境。

![](img/c61616bc4597c7ae5e3257a035699404_6.png)

上一节我们介绍了基础的内网信息收集概念，本节中我们来看看一些更具体的实操演示。

![](img/c61616bc4597c7ae5e3257a035699404_8.png)

![](img/c61616bc4597c7ae5e3257a035699404_9.png)

## 代理信息收集 🕵️

![](img/c61616bc4597c7ae5e3257a035699404_11.png)

为什么要搜集代理信息？在内网渗透过程中，搜集到代理信息有助于我们判断目标所处的网络环境。通过查看当前代理配置，可以准确了解目标是否使用了代理服务器以及代理的详细信息。

![](img/c61616bc4597c7ae5e3257a035699404_13.png)

![](img/c61616bc4597c7ae5e3257a035699404_14.png)

以下是收集代理信息的命令：

![](img/c61616bc4597c7ae5e3257a035699404_16.png)

*   **查看系统代理**：使用命令 `reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\Internet Settings" /v ProxyServer` 可以读取注册表，获取系统代理服务器的地址和端口。
    *   **示例输出**：`ProxyServer REG_SZ 127.0.0.1:10809` 表示代理地址为 `127.0.0.1`，端口为 `10809`。
*   **查看自动配置脚本(PAC)代理**：使用命令 `reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\Internet Settings" /v AutoConfigURL` 可以查看是否配置了自动代理脚本。
    *   如果未配置，命令会提示“系统找不到指定的注册表项或值”。

## Wi-Fi密码收集 📶

![](img/c61616bc4597c7ae5e3257a035699404_18.png)

在控制目标主机后，我们可以尝试收集其保存的Wi-Fi密码，这有助于我们探测并连接目标内网中的其他无线网络。

以下是收集Wi-Fi密码的步骤：

![](img/c61616bc4597c7ae5e3257a035699404_20.png)

1.  **查看连接过的Wi-Fi记录**：使用命令 `netsh wlan show profiles` 可以列出本机所有保存的Wi-Fi配置文件名称。
2.  **获取特定Wi-Fi的密码**：使用命令 `netsh wlan show profile name="Wi-Fi名称" key=clear` 可以查看指定Wi-Fi的详细配置，其中“关键内容”字段即为保存的密码。
    *   **示例命令**：`netsh wlan show profile name="302" key=clear`

![](img/c61616bc4597c7ae5e3257a035699404_22.png)

## 回收站内容收集 🗑️

![](img/c61616bc4597c7ae5e3257a035699404_24.png)

![](img/c61616bc4597c7ae5e3257a035699404_26.png)

用户删除的文件通常会先进入回收站。如果目标用户未彻底清空回收站，里面可能残留重要文件。我们可以通过命令访问并读取这些文件。

![](img/c61616bc4597c7ae5e3257a035699404_28.png)

以下是收集回收站内容的步骤：

![](img/c61616bc4597c7ae5e3257a035699404_30.png)

![](img/c61616bc4597c7ae5e3257a035699404_32.png)

1.  **查找有回收站内容的用户**：使用命令 `dir /a “C:\$Recycle.Bin”` 或遍历用户目录的方法，可以查看哪些用户的回收站目录下有文件。
    *   每个用户对应一个以SID（安全标识符）命名的文件夹。
2.  **进入目标用户的回收站目录**：回收站文件的实际路径通常为 `C:\$Recycle.Bin\{用户SID}\`。
3.  **解读回收站文件**：
    *   以 `$I` 开头的文件保存了被删除文件的**原始路径信息**。
    *   以 `$R` 开头的文件保存了被删除文件的**实际内容**。
    *   可以使用 `type` 命令查看这些文件的内容。例如：
        *   `type $I5RF0LP.txt` （查看原始路径）
        *   `type $R5RF0LP.txt` （查看文件内容）

![](img/c61616bc4597c7ae5e3257a035699404_34.png)

![](img/c61616bc4597c7ae5e3257a035699404_35.png)

## 浏览器历史与Cookie收集 🌐

![](img/c61616bc4597c7ae5e3257a035699404_37.png)

浏览器历史记录和Cookie中可能包含目标访问过的敏感网址、登录会话等信息。我们可以利用工具进行提取。

![](img/c61616bc4597c7ae5e3257a035699404_39.png)

![](img/c61616bc4597c7ae5e3257a035699404_41.png)

以下是收集浏览器信息的方法：

![](img/c61616bc4597c7ae5e3257a035699404_42.png)

![](img/c61616bc4597c7ae5e3257a035699404_43.png)

![](img/c61616bc4597c7ae5e3257a035699404_45.png)

![](img/c61616bc4597c7ae5e3257a035699404_46.png)

*   **使用Mimikatz工具**：Mimikatz的 `dpapi::chrome` 模块可以提取Chrome浏览器保存的密码、Cookie和历史记录。
    *   **示例命令**（需在Mimikatz环境中执行）：
        ```bash
        dpapi::chrome /in:"%LocalAppData%\Google\Chrome\User Data\Default\Login Data" /unprotect
        ```
*   **使用Metasploit（MSF）自动化收集**：在通过MSF获得目标主机的会话（Session）后，可以使用其内置的后渗透模块进行一键式信息收集。
    *   **常用模块**：
        *   `run post/windows/gather/enum_logged_on_users` （收集登录用户信息）
        *   `run scraper` （运行综合信息收集脚本，包含大量收集项）
    *   **使用方法**：在MSF的`meterpreter`会话中直接输入 `run scraper` 即可。

![](img/c61616bc4597c7ae5e3257a035699404_48.png)

本节课中我们一起学习了内网信息收集的多个实操技巧：从代理配置、Wi-Fi密码到回收站文件和浏览器数据。掌握这些方法能有效提升我们在渗透测试中对目标内网环境的认知深度。请注意，所有技术仅应用于合法授权的安全测试和学习研究。