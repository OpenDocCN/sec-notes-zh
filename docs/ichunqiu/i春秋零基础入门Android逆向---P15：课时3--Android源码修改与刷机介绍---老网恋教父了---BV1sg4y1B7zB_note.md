# i春秋零基础入门Android逆向 - P15：课时3 Android源码修改与刷机介绍 🛠️

![](img/56428c97bee3ac6a9371641c8172e954_1.png)

![](img/56428c97bee3ac6a9371641c8172e954_2.png)

![](img/56428c97bee3ac6a9371641c8172e954_4.png)

![](img/56428c97bee3ac6a9371641c8172e954_5.png)

![](img/56428c97bee3ac6a9371641c8172e954_7.png)

在本节课中，我们将学习如何下载和编译Android内核源码，并将其与AOSP源码整合，最终完成刷机操作。这是修改Android系统底层功能的关键步骤。

## 内核源码下载

上一节我们介绍了如何下载基础的AOSP源码。本节中我们来看看如何下载对应设备的内核源码。

![](img/56428c97bee3ac6a9371641c8172e954_9.png)

![](img/56428c97bee3ac6a9371641c8172e954_10.png)

![](img/56428c97bee3ac6a9371641c8172e954_11.png)

![](img/56428c97bee3ac6a9371641c8172e954_13.png)

一个完整的Android系统源码不仅包含AOSP，还包含设备特定的内核代码。内核代码因手机型号或模拟器版本而异，需要根据设备信息选择下载。

![](img/56428c97bee3ac6a9371641c8172e954_15.png)

我们可以通过以下网址查看不同设备对应的内核源码信息：

```
https://source.android.com/source/building-kernels
```

以下是查找和下载内核源码的步骤：

1.  **确定设备信息**：首先需要知道你的设备型号和芯片平台。例如，教程中使用的设备是LG的`hammerhead`（Nexus 5），其芯片平台是高通的`msm`。
2.  **查找对应项目**：在上述网址的表格中，根据设备信息找到对应的`Source`路径。例如，`hammerhead`对应的是`kernel/msm`项目。
3.  **使用代理下载**：由于源码托管在Google服务器，国内访问可能需要使用镜像源。可以将Google的源码地址替换为清华大学的镜像地址进行下载。

例如，下载`msm`内核源码的原始命令是：
```bash
git clone https://android.googlesource.com/kernel/msm.git
```
使用清华镜像的命令则是：
```bash
git clone https://aosp.tuna.tsinghua.edu.cn/kernel/msm.git
```

![](img/56428c97bee3ac6a9371641c8172e954_17.png)

![](img/56428c97bee3ac6a9371641c8172e954_18.png)

## 内核源码编译

![](img/56428c97bee3ac6a9371641c8172e954_20.png)

![](img/56428c97bee3ac6a9371641c8172e954_22.png)

下载完内核源码后，下一步是进行编译。这个过程与编译AOSP类似，但需要特定的环境配置。

首先，需要将下载的内核源码目录（例如`msm`）放置在AOSP源码的根目录下。初始时，该目录可能是空的，因为还没有检出具体的代码分支。

以下是编译内核源码的步骤：

![](img/56428c97bee3ac6a9371641c8172e954_24.png)

1.  **切换内核分支**：进入内核源码目录，查看可用的远程分支，并切换到与你的AOSP版本匹配的分支。
    ```bash
    cd msm
    git branch -a # 查看所有分支
    git checkout android-msm-hammerhead-3.4-kitkat-mr2 # 切换到指定分支（示例）
    ```
2.  **配置编译环境**：编译Android 4.4（KitKat）等旧版本需要JDK 6环境。可以编写一个简单的脚本来快速切换Java版本。
    ```bash
    # 切换至JDK 6环境脚本示例
    export JAVA_HOME=/path/to/jdk1.6.0_45
    export PATH=$JAVA_HOME/bin:$PATH
    ```
3.  **初始化AOSP环境**：在内核目录中，需要导入AOSP的交叉编译工具链和环境变量。
    ```bash
    source ../build/envsetup.sh # 初始化AOSP编译环境
    export PATH=$PATH:../prebuilts/gcc/linux-x86/arm/arm-eabi-4.6/bin # 添加交叉编译器路径
    export ARCH=arm
    export SUBARCH=arm
    export CROSS_COMPILE=arm-eabi-
    ```
4.  **执行编译**：使用`make`命令并指定设备配置文件进行编译。配置文件通常位于`arch/arm/configs/`目录下。
    ```bash
    make hammerhead_defconfig # 载入默认配置（示例）
    make -j4 # 开始编译，-j4表示使用4个线程
    ```
编译成功后，会在`arch/arm/boot/`目录下生成内核镜像文件`zImage`。

![](img/56428c97bee3ac6a9371641c8172e954_26.png)

![](img/56428c97bee3ac6a9371641c8172e954_27.png)

![](img/56428c97bee3ac6a9371641c8172e954_29.png)

![](img/56428c97bee3ac6a9371641c8172e954_31.png)

![](img/56428c97bee3ac6a9371641c8172e954_33.png)

## 整合内核与刷机准备

![](img/56428c97bee3ac6a9371641c8172e954_35.png)

![](img/56428c97bee3ac6a9371641c8172e954_37.png)

编译出新的内核后，需要将其替换到AOSP源码中，并准备好设备驱动，才能进行完整的系统编译和刷机。

![](img/56428c97bee3ac6a9371641c8172e954_39.png)

![](img/56428c97bee3ac6a9371641c8172e954_41.png)

![](img/56428c97bee3ac6a9371641c8172e954_43.png)

1.  **替换内核镜像**：将新编译的`zImage`文件重命名为`kernel`，并复制到AOSP源码中对应设备的预编译内核目录下，覆盖原有文件。
    ```bash
    cp arch/arm/boot/zImage ../device/lge/hammerhead-kernel/kernel
    ```
2.  **下载设备驱动**：许多设备的硬件需要专有驱动（Blob）。这些驱动文件需要从Google的驱动页面单独下载。
    - 驱动下载地址：`https://developers.google.com/android/drivers`
    - 根据你的设备型号和AOSP版本号（如`KTU84P`）下载对应的驱动包。
3.  **提取驱动文件**：将下载的`.tgz`文件解压到AOSP根目录，会得到几个`.sh`脚本文件。给它们添加执行权限并运行，脚本会自动将专有驱动提取到`vendor/`目录下。
    ```bash
    chmod a+x extract-*.sh # 赋予执行权限
    ./extract-broadcom.sh # 执行提取脚本（示例）
    ```

![](img/56428c97bee3ac6a9371641c8172e954_45.png)

## 完整系统编译与刷机

完成内核替换和驱动准备后，就可以重新编译完整的Android系统镜像了。

![](img/56428c97bee3ac6a9371641c8172e954_47.png)

![](img/56428c97bee3ac6a9371641c8172e954_48.png)

![](img/56428c97bee3ac6a9371641c8172e954_50.png)

1.  **设置编译目标**：在AOSP根目录下，初始化环境并选择要编译的设备目标。
    ```bash
    source build/envsetup.sh
    lunch aosp_hammerhead-userdebug # 选择hammerhead设备的userdebug版本（示例）
    ```
2.  **执行完整编译**：使用`make`命令开始编译。这个过程耗时较长。
    ```bash
    make -j8
    ```
3.  **进入刷机模式**：编译完成后，将手机通过USB连接电脑，并重启到`fastboot`（线刷）模式。
    ```bash
    adb reboot bootloader
    ```
4.  **执行刷机命令**：在`fastboot`模式下，使用一条命令刷入所有新编译的镜像文件。
    ```bash
    fastboot flashall -w
    ```
    **注意**：刷机有风险，请确保USB连接稳定，刷机过程中不要断开连接。`-w`选项会清空用户数据。

![](img/56428c97bee3ac6a9371641c8172e954_52.png)

## 常用编译指令

在开发和调试过程中，我们可能不需要每次都完整编译整个系统。以下是一些有用的增量编译指令：

- **`make`**：编译整个系统。
- **`mma`** 或 **`mm`**：编译当前目录下的模块。当你只修改了某个应用的代码时，可以使用此命令快速重新编译该模块。
    ```bash
    cd packages/apps/Settings
    mm # 仅编译Settings应用
    ```
- **`mma`**：编译指定目录下的模块。
    ```bash
    mma packages/apps/Settings
    ```
- **`make snod`**：快速重新生成`system.img`系统镜像。在使用`mm`等命令编译了部分模块后，需要运行此命令将新编译的文件打包进系统镜像，刷机后才能生效。

## 总结

![](img/56428c97bee3ac6a9371641c8172e954_54.png)

本节课中我们一起学习了Android系统修改与刷机的完整流程。我们从下载设备特定的内核源码开始，经历了配置环境、编译内核、整合驱动、完整系统编译，最后完成了刷机操作。同时，我们也掌握了一些高效的增量编译命令，便于日常的开发和调试工作。理解这些步骤是进行Android底层逆向和定制化开发的重要基础。