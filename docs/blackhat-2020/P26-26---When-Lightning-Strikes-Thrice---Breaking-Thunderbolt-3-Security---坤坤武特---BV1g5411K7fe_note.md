![](img/1d3f50dcf93e6f202b7317f1c34a2c66_1.png)

![](img/1d3f50dcf93e6f202b7317f1c34a2c66_2.png)

# P26：26 - 当闪电三次击中时 - 暴破Thunderbolt 3安全 - 坤坤武特 - BV1g5411K7fe

## 概述

在本节课中，我们将学习Thunderbolt 3安全漏洞及其攻击方法。我们将深入了解Thunderbolt协议、安全级别以及如何利用这些漏洞进行攻击。

## Thunderbolt协议简介

Thunderbolt是一种高速接口，由Intel和Apple于2011年开发。它基于PCI Express协议，允许连接外部设备，如外部GPU、显示器和高速存储设备。

## Thunderbolt安全漏洞

Thunderbolt安全漏洞被称为ThunderSpy，它破坏了Thunderbolt协议的安全性。以下是ThunderSpy的主要漏洞：

* **漏洞1**：DROM签名验证漏洞
* **漏洞2**：设备ID和设备名称伪造漏洞
* **漏洞3**：设备UUID伪造漏洞
* **漏洞4**：设备控制器固件漏洞
* **漏洞5**：主机控制器固件漏洞
* **漏洞6**：SPI闪存保护漏洞

## 攻击方法

ThunderSpy攻击方法可以分为以下两类：

* **攻击方法1**：需要短暂访问笔记本电脑，只需5分钟即可重新编程主机控制器固件。
* **攻击方法2**：不需要预先重新编程控制器固件，但需要5分钟物理访问受害者的Thunderbolt设备。

## 影响的系统

所有2011年至今天配备Thunderbolt的设备都容易受到ThunderSpy攻击。这包括所有2011年至2018年间发布的PC、所有运行macOS和Linux的Mac以及一些提供内核到UMA保护的系统。

## Intel的响应

Intel对ThunderSpy的响应是内核DMA保护。然而，这种保护措施存在一些局限性，例如：

![](img/1d3f50dcf93e6f202b7317f1c34a2c66_4.png)

* 它仅是部分缓解措施。
* 它不缓解ThunderSpy漏洞1、2和3。
* 它需要IOMEMU和BIOS支持。

## ThunderSpy 2

ThunderSpy 2是一个实验性保护方案，它将内核DMA保护引入了大约六年的系统。它包括所有具有IOMEMU的系统，包括提供Thunderbolt 2的系统。

## 总结

在本节课中，我们一起学习了Thunderbolt 3安全漏洞及其攻击方法。我们了解到ThunderSpy攻击方法、影响系统和Intel的响应。希望这节课能帮助您更好地了解Thunderbolt安全漏洞。