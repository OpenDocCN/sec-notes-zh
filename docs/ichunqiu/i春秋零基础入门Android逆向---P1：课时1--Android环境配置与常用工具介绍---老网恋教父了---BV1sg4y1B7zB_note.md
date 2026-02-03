![](img/0c9d0e4510b9e5dd29583bda9c6138d4_1.png)

# i春秋零基础入门Android逆向 - 课时1：Android环境配置与常用工具介绍 🛠️

在本节课中，我们将学习Android逆向分析的第一步：搭建开发与逆向环境，并介绍后续课程中将使用到的核心工具。这是所有后续学习的基础，请务必认真完成。

## 课程概述

Android逆向是一项综合技能，需要结合开发与逆向分析两方面的能力。拥有良好的开发基础，能帮助你更深刻地理解程序运行原理，从而在逆向分析时快速定位关键代码。本课程将重点讲解逆向中直接接触和修改的语言，如Smali和ARM汇编。

一个典型的Android应用程序通常使用Java语言开发。Java代码被编译成**Dalvik字节码**，存储在`.dex`文件中，并在Dalvik虚拟机上运行。逆向分析的核心对象就是这个`.dex`文件。

上一节我们概述了逆向的基本概念，本节中我们来看看具体需要配置哪些环境。

## 开发环境配置

以下是搭建Android开发与逆向环境所需的步骤。

### 1. 安装与配置JDK

JDK（Java SE Development Kit）是Java开发工具包，是编译和运行Java程序的基础。

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_3.png)

*   **下载地址**：访问Oracle官网或通过搜索引擎下载。
*   **版本选择**：建议选择JDK 8版本。
*   **配置环境变量（Windows 7示例）**：
    1.  右键“计算机” -> “属性” -> “高级系统设置” -> “环境变量”。
    2.  在“系统变量”中，新建变量 `JAVA_HOME`，值为JDK的安装路径（例如 `C:\Program Files\Java\jdk1.8.0_65`）。
    3.  新建变量 `CLASSPATH`，值为 `.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar`。
    4.  编辑 `Path` 变量，在末尾添加 `;%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin`。
*   **验证安装**：打开命令提示符（CMD），输入 `java -version`。若显示版本信息，则配置成功。

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_5.png)

```
java version "1.8.0_65"
Java(TM) SE Runtime Environment (build 1.8.0_65-b17)
Java HotSpot(TM) 64-Bit Server VM (build 25.65-b01, mixed mode)
```

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_7.png)

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_9.png)

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_11.png)

### 2. 安装Android Studio（推荐）

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_13.png)

Android Studio是谷歌官方推荐的集成开发环境（IDE），功能强大且持续更新。

*   **下载**：需要访问谷歌开发者官网，此过程可能需要VPN（科学上网工具）。
*   **安装**：运行安装程序，按照向导完成安装。首次运行时会提示安装Android SDK，请跟随指引完成。
*   **注意**：使用此IDE是后续学习和调试的基础。

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_15.png)

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_17.png)

### 3. 配置Android SDK

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_18.png)

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_20.png)

Android SDK（Software Development Kit）包含开发Android应用所需的库和工具。

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_22.png)

*   **获取方式**：如果安装了Android Studio，SDK通常会一并安装。也可单独下载。
*   **配置环境变量**：
    1.  在“系统变量”中，编辑 `Path` 变量。
    2.  添加SDK中 `platform-tools` 和 `tools` 目录的路径（例如 `D:\Android\sdk\platform-tools` 和 `D:\Android\sdk\tools`）。
*   **验证配置**：在CMD中输入 `adb version` 和 `monitor`，若能正常启动ADB（Android调试桥）和DDMS（Dalvik调试监视器），则配置成功。

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_24.png)

### 4. 安装Android NDK

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_26.png)

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_28.png)

NDK（Native Development Kit）用于开发应用的原生（C/C++）部分，编译生成 `.so` 库文件。在逆向分析涉及原生代码时非常重要。

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_30.png)

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_32.png)

*   **下载**：可通过Android Studio内置的SDK管理器下载，或从官网单独下载（同样可能需要VPN）。
*   **注意**：初期可能用不到，但后续课程中会涉及，建议提前准备。

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_34.png)

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_35.png)

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_36.png)

上一节我们配置了核心的开发环境，接下来看看其他辅助工具。

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_38.png)

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_39.png)

## 辅助工具准备

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_40.png)

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_41.png)

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_43.png)

除了开发环境，逆向分析还需要以下工具。

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_44.png)

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_46.png)

### 1. 代码编辑器（备选）

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_48.png)

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_49.png)

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_51.png)

如果你不习惯使用Android Studio，Eclipse加ADT插件是早期的经典选择。但请注意，谷歌已停止对其支持。

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_53.png)

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_55.png)

*   **下载Eclipse**：从Eclipse官网下载。
*   **安装ADT插件**：在Eclipse中，通过 `Help` -> `Install New Software`，添加ADT插件仓库地址进行安装（此过程可能需要VPN或离线包）。

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_57.png)

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_58.png)

### 2. 安卓模拟器

模拟器用于在电脑上运行和测试Android应用。市面上选择很多，如蓝叠（BlueStacks）、夜神等，选择一个运行流畅的即可。

### 3. 调试用安卓手机

**这是必备设备**。模拟器无法完全替代真机，尤其是在调试原生（SO）代码、进行脱壳等高级操作时。

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_60.png)

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_61.png)

*   **要求**：建议准备一台已Root的安卓手机。
*   **版本**：Android 4.0至7.0版本均可，二手设备即可满足学习需求。

### 4. 科学上网工具（VPN）

由于访问谷歌开发者官网、下载Android Studio/NDK、以及后续查找资料都可能需要，建议准备一个稳定的VPN工具。

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_63.png)

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_65.png)

## 课程总结

本节课我们一起学习了Android逆向分析的入门基础——环境配置。我们详细介绍了JDK、Android Studio/SDK/NDK的安装与配置方法，并列举了模拟器、调试手机和VPN等必备的辅助工具。

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_67.png)

**课后作业**：请根据教程完成以下配置：
1.  成功安装并配置JDK与Android SDK。
2.  安装Android Studio或Eclipse（任选其一）。
3.  安装一款安卓模拟器。
4.  准备一台可用于调试的安卓手机（务必完成）。

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_69.png)

![](img/0c9d0e4510b9e5dd29583bda9c6138d4_71.png)

请务必在课程开始前完成这些环境搭建，这是后续所有学习步骤的基石。