# i春秋零基础入门Android逆向 - P13：课时1 安装部署Android源码编译环境 🛠️

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_1.png)

在本节课中，我们将学习如何安装和部署一个用于Android源码编译、修改的环境。我们将基于谷歌官方的AOSP项目进行配置，确保初学者能够跟随步骤完成环境的搭建。

## 概述 📋

编译Android源码是进行系统级修改和逆向分析的基础。本节课将指导你完成从零开始配置一个Ubuntu系统下的AOSP编译环境，包括安装必要的工具、配置JDK、下载源码以及执行首次编译。

## 环境配置准备

首先，我们需要访问谷歌官方的AOSP编译指南网站，获取最权威的配置信息。官方网址提供了初始化编译环境、设置和下载源码的详细步骤。

上一节我们介绍了课程目标，本节中我们来看看具体的环境配置步骤。

### 1. 确定源码版本

我们需要确定要下载和编译的Android源码版本。本节课以Android 4.4.3_r1 (KTU84L) 版本为例。该版本支持Nexus 7和Nexus 4设备。如果你使用的是Nexus 5，则需要下载对应的KTU84N分支代码。

### 2. 安装JDK

不同版本的Android源码需要不同版本的JDK。以下是版本对应关系：
*   Android 2.3 至 4.0.x： 需要 **JDK 5**
*   Android 4.1.x 至 5.0.x： 需要 **JDK 6**
*   Android 5.1.x 至 6.0.x： 需要 **OpenJDK 7**
*   Android 6.0 以上： 需要 **OpenJDK 8**

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_3.png)

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_4.png)

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_5.png)

由于我们编译的是Android 4.4.3，因此需要安装**甲骨文(Oracle)的JDK 6**。如果JDK版本不正确，编译过程会报错。安装方法可自行搜索。

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_6.png)

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_8.png)

### 3. 安装编译依赖库

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_10.png)

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_12.png)

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_14.png)

以下是在Ubuntu 14.04系统上安装编译所需依赖库的命令。请根据你的Ubuntu版本选择对应的命令。

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_16.png)

```bash
# 对于 Ubuntu 14.04
sudo apt-get install git-core gnupg flex bison gperf build-essential zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev libgl1-mesa-dev libxml2-utils xsltproc unzip
```

### 4. 配置USB权限

为了在后续刷机时能够连接手机，需要下载一个规则文件并放到指定目录。这一步是必须的，否则手机在Recovery模式下无法被系统识别。

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_18.png)

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_20.png)

文件需要放置在 `/etc/udev/rules.d/` 目录下，并且需要将文件中的 `<username>` 替换为你自己的用户名。

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_22.png)

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_23.png)

## 虚拟机环境搭建演示

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_24.png)

考虑到大家可能只有一台计算机，我们将使用VirtualBox虚拟机进行演示。你也可以使用VMware。

以下是创建虚拟机的关键步骤：
1.  创建新的虚拟机，选择Linux (Ubuntu 64-bit) 系统。
2.  分配足够的内存，建议**至少4GB**，官方推荐实体机8GB，虚拟机16GB以获得较好体验。
3.  创建虚拟硬盘，**建议分配200GB以上**的空间。
4.  加载Ubuntu 14.04的ISO镜像文件并启动安装。

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_26.png)

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_28.png)

安装过程基本是全自动的。安装完成后，需要为虚拟机安装“增强功能”，以获得更好的显示效果和剪贴板共享等功能。安装后重启系统。

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_30.png)

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_32.png)

## 在Ubuntu系统中进行配置

系统启动后，我们开始具体配置。

### 1. 安装JDK 6

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_34.png)

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_36.png)

根据之前确定的版本，下载并安装Oracle JDK 6。

### 2. 安装编译库

打开终端 (Ctrl+Alt+T)，粘贴并运行之前提到的安装编译依赖库的命令。系统会自动下载并安装所有必需的软件包。

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_38.png)

### 3. 配置USB规则

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_39.png)

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_40.png)

将下载好的 `51-android.rules` 文件复制到 `/etc/udev/rules.d/` 目录，并确保其中的用户名已修改正确。

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_42.png)

完成以上三步后，基础编译环境就配置好了。

## 下载Android源码

接下来，我们开始下载Android源码。官方下载需要使用 `repo` 工具，并且需要谷歌账号。为了加速下载，我们使用清华大学镜像源。

以下是下载步骤：

1.  安装 `git` 并配置用户名和邮箱。
    ```bash
    sudo apt-get install git
    git config --global user.email "you@example.com"
    git config --global user.name "Your Name"
    ```
2.  下载 `repo` 工具，并放到 `~/bin` 目录，赋予执行权限。
    ```bash
    mkdir ~/bin
    PATH=~/bin:$PATH
    curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
    chmod a+x ~/bin/repo
    ```
3.  修改 `repo` 文件，将源地址替换为清华镜像。
    *   将 `REPO_URL` 一行修改为：`REPO_URL = 'https://mirrors.tuna.tsinghua.edu.cn/git/git-repo/'`
4.  创建源码目录并初始化仓库。
    ```bash
    mkdir android_source
    cd android_source
    repo init -u https://aosp.tuna.tsinghua.edu.cn/platform/manifest -b android-4.4.3_r1
    ```
5.  同步代码（下载）。这是一个漫长的过程，取决于网络状况。
    ```bash
    repo sync
    ```

如果下载中断，可以重新执行 `repo sync` 命令继续下载。

## 编译Android源码

源码下载完成后，就可以开始编译了。

以下是编译步骤：

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_44.png)

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_45.png)

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_47.png)

1.  进入源码根目录，初始化编译环境。
    ```bash
    cd android_source
    source build/envsetup.sh
    ```
2.  选择编译目标。我们选择模拟器版本进行测试。
    ```bash
    lunch aosp_arm-eng
    ```
3.  （可选）设置编译缓存以加速后续编译。
    ```bash
    export USE_CCACHE=1
    export CCACHE_DIR=/home/username/.ccache # 替换为你的路径
    prebuilts/misc/linux-x86/ccache/ccache -M 100G
    ```
4.  开始编译。`-j4` 表示使用4个线程，请根据你的CPU核心数调整。
    ```bash
    make -j4
    ```

编译过程可能需要数小时。如果出现错误（如JDK版本错误），请根据错误提示进行修正。

编译成功后，输出的系统镜像文件位于 `out/target/product/generic/` 目录下。

## 运行编译结果

编译完成后，可以使用内置的模拟器运行我们编译的系统。

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_49.png)

```bash
emulator
```

这个模拟器比Windows上的AVD性能更好，适合用于初步测试。测试无误后，才能刷入实体手机。

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_51.png)

## 总结 📝

本节课中我们一起学习了如何从零开始搭建Android AOSP源码的编译环境。我们完成了以下关键步骤：
1.  确定了Android 4.4.3_r1作为目标版本。
2.  安装了正确版本的Oracle JDK 6。
3.  在Ubuntu 14.04上安装了所有必要的编译依赖包。
4.  配置了USB连接规则。
5.  使用清华镜像下载了Android源码。
6.  初始化了编译环境，并成功执行了首次编译。
7.  使用模拟器运行了编译出的系统镜像。

![](img/0f1a7816bd97de0f3b3cdf6365dcf0a7_53.png)

这个过程是进行Android系统修改和深入逆向分析的基础，请务必动手实践，遇到问题多查阅官方文档和错误日志。