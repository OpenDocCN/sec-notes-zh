# CTF教程：P52：Kali软件安装+环境搭建 🛠️

![](img/5c1c12c9e775d27351a1f138ed0935ca_1.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_3.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_5.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_7.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_9.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_11.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_12.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_13.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_15.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_17.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_19.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_21.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_23.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_25.png)

在本节课中，我们将学习如何安装和配置Kali Linux系统，这是进行网络安全学习和CTF竞赛的重要工具平台。我们将从了解Kali是什么开始，逐步完成下载、安装以及基本环境配置。

![](img/5c1c12c9e775d27351a1f138ed0935ca_27.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_29.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_31.png)

## 什么是Kali Linux？ 🐧

![](img/5c1c12c9e775d27351a1f138ed0935ca_33.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_35.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_37.png)

Kali Linux是一个基于Debian的Linux发行版本。我们经常使用Kali Linux，因为它最大的优势和特点是包含了数百种Web安全及渗透测试相关的工具。我们可以直接在这个Linux系统中使用和操作这些工具。它的目的是用于高级的渗透测试和安全审计。它是一个完全免费的Linux系统，并且支持中文。

![](img/5c1c12c9e775d27351a1f138ed0935ca_39.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_41.png)

详细的工具列表可以在以下网址查看：
```
https://tools.kali.org/tools-listing
```
该列表包含了600多个渗透测试工具，每个工具都有详细的介绍和用法。

![](img/5c1c12c9e775d27351a1f138ed0935ca_43.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_45.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_47.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_49.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_51.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_53.png)

## Kali Linux的安装 📥

![](img/5c1c12c9e775d27351a1f138ed0935ca_55.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_57.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_59.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_61.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_63.png)

上一节我们介绍了Kali Linux是什么，本节中我们来看看如何进行安装。

![](img/5c1c12c9e775d27351a1f138ed0935ca_65.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_67.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_69.png)

首先，我们需要下载Kali Linux系统。官方网址是：
```
https://www.kali.org/downloads/
```
打开网址后，可以看到可供下载的版本。新版本不像之前有那么多针对特定桌面环境的镜像。这里提供了几种选择：

![](img/5c1c12c9e775d27351a1f138ed0935ca_70.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_72.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_73.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_75.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_77.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_79.png)

以下是主要的下载选项：
*   **Kali Linux 64位安装版**：用于常规安装的ISO镜像。
*   **Kali Linux 32位安装版**：用于32位系统的安装镜像。
*   **Kali Linux Live版**：可以放到U盘中直接运行的实时系统。
*   **Kali Linux NetInstaller版**：在线安装版本，镜像文件最小，大部分工具需要在线下载安装。

![](img/5c1c12c9e775d27351a1f138ed0935ca_81.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_83.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_85.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_87.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_89.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_91.png)

此外，官网还提供了预配置好的虚拟机版本，方便快速使用。

![](img/5c1c12c9e775d27351a1f138ed0935ca_93.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_95.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_97.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_99.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_100.png)

**注意**：新版本（2020版及以后）取消了默认的root用户登录。默认用户是 `kali`，密码也是 `kali`。使用普通用户登录是更安全的措施。

## 使用虚拟机镜像快速启动 🚀

如果你不想一步步安装系统，可以使用预配置好的虚拟机版本（例如VMware的`.ova`格式文件）。下载后，如果你安装了VMware，双击该文件即可导入并启动虚拟机。

![](img/5c1c12c9e775d27351a1f138ed0935ca_102.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_104.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_106.png)

启动后，会进入登录界面。默认使用 `kali` 用户登录（密码 `kali`）。登录成功后，界面如下图所示。

![](img/5c1c12c9e775d27351a1f138ed0935ca_108.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_110.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_112.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_31.png)

为了方便进行需要高权限的操作（如安装软件），我们可以切换到 `root` 用户。在终端中执行以下命令修改root密码：
```bash
sudo passwd root
```
系统会提示你输入当前用户 (`kali`) 的密码，然后设置新的root密码。之后可以使用 `su root` 命令切换到root用户。

![](img/5c1c12c9e775d27351a1f138ed0935ca_114.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_116.png)

## 使用ISO镜像进行自定义安装 💿

![](img/5c1c12c9e775d27351a1f138ed0935ca_118.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_120.png)

现在，我们来学习如何使用ISO镜像文件进行自定义安装。这种方法可以让你更灵活地配置系统。

![](img/5c1c12c9e775d27351a1f138ed0935ca_122.png)

首先，你需要在官网下载Kali Linux的ISO镜像文件。然后，在虚拟机软件（如VMware）中新建一个虚拟机。

![](img/5c1c12c9e775d27351a1f138ed0935ca_124.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_126.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_128.png)

以下是创建虚拟机的主要步骤：
1.  选择“自定义（高级）”配置。
2.  操作系统选择“稍后安装操作系统”。
3.  客户机操作系统选择“Linux”，版本选择“Debian 10.x 64位”。
4.  为虚拟机命名并选择安装位置（建议不要放在C盘）。
5.  根据电脑配置分配处理器核心数量和内存（例如，2GB内存）。
6.  配置网络类型。

![](img/5c1c12c9e775d27351a1f138ed0935ca_130.png)

关于网络类型，这里做一个简单介绍：
*   **桥接模式**：虚拟机的IP地址与物理主机在同一网段，会占用局域网内的一个IP地址。
*   **NAT模式**：虚拟机通过主机的网络地址转换访问外网，IP地址由虚拟网络分配，不占用物理网络IP。
*   **仅主机模式**：虚拟机只能与主机通信，不能访问外网。

![](img/5c1c12c9e775d27351a1f138ed0935ca_132.png)

对于大多数情况，选择“桥接模式”或“NAT模式”即可。之后，创建虚拟磁盘并完成虚拟机创建。

![](img/5c1c12c9e775d27351a1f138ed0935ca_134.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_136.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_137.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_139.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_141.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_143.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_145.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_147.png)

创建完成后，在虚拟机设置中加载下载好的Kali Linux ISO镜像文件。启动虚拟机，选择“Graphical install”（图形化安装）。

![](img/5c1c12c9e775d27351a1f138ed0935ca_149.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_151.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_152.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_154.png)

安装过程会引导你进行以下配置：
*   选择语言和区域。
*   设置主机名（例如 `kali`）。
*   设置域名（本地使用可留空）。
*   创建普通用户账户和密码。
*   进行磁盘分区。

![](img/5c1c12c9e775d27351a1f138ed0935ca_156.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_158.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_159.png)

在磁盘分区环节，新手可以选择“使用整个磁盘”。如果你熟悉Linux分区，可以选择“手动”分区。常见的分区方案包括：
*   `/boot`：启动分区，约200MB-400MB。
*   `swap`：交换空间，建议为内存大小的1-2倍。
*   `/home`：用户家目录，如果多用户使用，可以分配较大空间。
*   `/`：根目录，分配剩余所有空间。

![](img/5c1c12c9e775d27351a1f138ed0935ca_161.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_163.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_164.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_166.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_168.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_170.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_172.png)

分区完成后，系统将开始安装。安装过程会从网络下载一些文件，耗时较长，请耐心等待。安装完成后，重启系统即可。

## 系统配置与网络设置 🌐

![](img/5c1c12c9e775d27351a1f138ed0935ca_174.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_176.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_178.png)

系统安装完成后，可能需要进行一些基本配置，特别是网络配置，以确保可以正常上网。

![](img/5c1c12c9e775d27351a1f138ed0935ca_180.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_181.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_183.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_185.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_187.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_189.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_190.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_192.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_194.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_196.png)

如果虚拟机无法上网，请检查虚拟机的网络设置。例如，在VMware中，确保网络适配器连接模式正确（如桥接模式），并选择了正确的物理网卡。

![](img/5c1c12c9e775d27351a1f138ed0935ca_198.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_200.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_202.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_204.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_206.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_208.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_210.png)

如果遇到域名无法解析的问题，可能需要修改DNS设置。可以编辑 `/etc/resolv.conf` 文件，添加公共DNS服务器，例如：
```
nameserver 8.8.8.8
nameserver 114.114.114.114
```

![](img/5c1c12c9e775d27351a1f138ed0935ca_212.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_214.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_215.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_217.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_219.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_220.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_222.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_224.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_226.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_228.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_230.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_232.png)

此外，你还可以修改主机名。编辑 `/etc/hostname` 文件，将其中的内容改为你想要的主机名即可。

![](img/5c1c12c9e775d27351a1f138ed0935ca_234.png)

## 总结 📝

![](img/5c1c12c9e775d27351a1f138ed0935ca_236.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_238.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_240.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_242.png)

![](img/5c1c12c9e775d27351a1f138ed0935ca_244.png)

本节课中我们一起学习了Kali Linux的安装与环境搭建。我们首先了解了Kali Linux是什么以及它的用途，然后分别介绍了使用预置虚拟机镜像快速启动的方法，以及使用ISO镜像进行完整自定义安装的详细步骤。最后，我们还学习了基本的系统与网络配置。掌握这些是进行后续网络安全学习和工具使用的基础。