#  009：课时1 Android 结构基础讲解 🏗️

![](img/be42712f3ef1fd95cdc972b1083329d7_1.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_3.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_5.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_7.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_8.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_9.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_11.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_12.png)

在本节课中，我们将学习Android应用程序的基础结构。我们将通过分析一个简单的“Hello World”程序，了解其核心目录、资源文件的作用以及代码与资源之间的关联方式。最后，我们将通过一个实践案例，学习如何修改和插入资源文件。

![](img/be42712f3ef1fd95cdc972b1083329d7_14.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_15.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_17.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_18.png)

## 概述

![](img/be42712f3ef1fd95cdc972b1083329d7_20.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_21.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_22.png)

Android应用程序由代码、资源和配置文件三大部分组成。理解这三者的关系和组织方式，是进行Android逆向分析的基础。本节将详细解析一个标准Android项目的目录结构。

![](img/be42712f3ef1fd95cdc972b1083329d7_24.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_25.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_27.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_29.png)

## Android程序基础结构

![](img/be42712f3ef1fd95cdc972b1083329d7_30.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_31.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_33.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_35.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_36.png)

上一节我们介绍了课程目标，本节中我们来看看一个标准Android项目的具体构成。

![](img/be42712f3ef1fd95cdc972b1083329d7_38.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_40.png)

我们使用Android Studio创建一个最基本的“Hello World”程序作为示例。创建过程遵循默认设置即可。

![](img/be42712f3ef1fd95cdc972b1083329d7_41.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_43.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_45.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_47.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_48.png)

当项目首次构建时，IDE会从网络下载必要的依赖库，这个过程可能较慢。建议保持网络通畅，确保所有依赖下载完成，以避免后续编译问题。

![](img/be42712f3ef1fd95cdc972b1083329d7_50.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_51.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_53.png)

项目创建完成后，我们切换到“Project”视图，这是最接近原始代码结构的布局。在`app`模块下，主要包含三个核心目录：

![](img/be42712f3ef1fd95cdc972b1083329d7_55.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_57.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_58.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_60.png)

1.  **`java`目录**：存放程序的Java/Kotlin源代码。
2.  **`res`目录**：存放程序的所有资源文件，如图片、布局、字符串等。
3.  **`manifests`目录**：存放`AndroidManifest.xml`文件，用于向系统声明应用程序的组件、权限等元信息。

![](img/be42712f3ef1fd95cdc972b1083329d7_62.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_64.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_66.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_67.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_69.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_71.png)

`AndroidManifest.xml`文件至关重要，它注册了代码和资源，告知Android系统应用程序的入口和所需组件。

![](img/be42712f3ef1fd95cdc972b1083329d7_73.png)

## 资源目录详解

![](img/be42712f3ef1fd95cdc972b1083329d7_75.png)

上一节我们介绍了核心目录，本节中我们深入了解一下`res`资源目录下的各个子文件夹。

![](img/be42712f3ef1fd95cdc972b1083329d7_77.png)

以下是`res`目录下常见文件夹及其作用的说明：

![](img/be42712f3ef1fd95cdc972b1083329d7_79.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_81.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_82.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_83.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_84.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_85.png)

*   **`drawable`**：用于存放图片资源（如PNG、JPEG文件）和XML定义的图形。
*   **`layout`**：存放定义用户界面布局的XML文件。
*   **`menu`**：存放定义应用程序菜单（如选项菜单）的XML文件。
*   **`mipmap-<density>`**：专门用于存放应用程序图标，针对不同屏幕密度（如`hdpi`, `xhdpi`）进行了优化，系统会根据设备分辨率自动选择。
*   **`values`**：存放定义简单值的XML文件，如字符串、颜色、尺寸和样式。
*   **`values-<qualifier>`**：这是`values`目录的变体，用于为不同配置（如API版本`v21`、屏幕最小宽度`w820dp`）提供特定的资源值。系统会优先匹配最符合当前设备条件的资源。

![](img/be42712f3ef1fd95cdc972b1083329d7_87.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_89.png)

这种通过文件夹后缀（限定符）进行资源匹配的机制，使得Android应用能够很好地适配各种不同分辨率、系统版本的设备。

![](img/be42712f3ef1fd95cdc972b1083329d7_91.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_93.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_95.png)

## 代码与资源的关联

![](img/be42712f3ef1fd95cdc972b1083329d7_97.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_99.png)

上一节我们了解了资源是如何组织的，本节中我们来看看代码是如何引用这些资源的。

![](img/be42712f3ef1fd95cdc972b1083329d7_101.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_102.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_104.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_106.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_107.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_109.png)

在Android中，源代码和资源文件是分离的。系统通过一个由编译过程自动生成的`R`类来建立两者之间的桥梁。`R`类为每个资源分配一个唯一的`int`型ID（索引）。

![](img/be42712f3ef1fd95cdc972b1083329d7_111.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_112.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_114.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_115.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_117.png)

例如，在Activity的`onCreate`方法中，通过`setContentView(R.layout.activity_main)`设置布局。这里的`R.layout.activity_main`就是一个指向`layout/activity_main.xml`文件的整型ID。

![](img/be42712f3ef1fd95cdc972b1083329d7_119.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_121.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_123.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_124.png)

反编译APK后，可以在`smali`代码中看到类似`const v0, 0x7f0c001c`的指令，这个十六进制数就是资源ID。`R.java`文件则定义了这些ID与资源名称的映射关系，其结构可能如下：

![](img/be42712f3ef1fd95cdc972b1083329d7_126.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_128.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_130.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_131.png)

```java
public final class R {
    public static final class layout {
        public static final int activity_main = 0x7f0c001c;
    }
    public static final class string {
        public static final int app_name = 0x7f1000e1;
    }
    // ... 其他资源类型（id, drawable, color等）
}
```

在代码中，我们通过形如`getString(R.string.app_name)`的方法，传入资源ID来获取具体的资源值。

![](img/be42712f3ef1fd95cdc972b1083329d7_133.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_135.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_137.png)

## 实践：修改与插入资源

上一节我们理解了关联原理，本节中我们通过一个实际案例来学习如何修改APK的资源。

我们的目标是为一个现有的APK插入一个启动屏（Splash Screen），实现“盗用版权”的效果。核心步骤如下：

1.  **反编译目标APK**：使用工具（如Apktool）将APK反编译为`smali`代码和资源文件。
2.  **准备待插入内容**：
    *   **代码**：准备启动屏Activity的`smali`代码。
    *   **资源**：准备启动屏的布局文件（`layout/splash.xml`）和图片资源（放入`drawable`或`mipmap`）。
    *   **配置**：修改`AndroidManifest.xml`，将启动屏Activity设置为新的应用入口（`LAUNCHER`）。
3.  **整合与修复**：
    *   将代码和资源文件复制到反编译目录的对应位置。
    *   关键步骤是修复资源ID冲突。需要更新`res/values/public.xml`和`R$xxx.smali`文件，确保新插入资源的ID唯一且与现有ID范围协调。通常需要将新ID设置为现有同类ID的相邻值。
    *   检查代码中的硬编码资源ID引用（如在`smali`中），确保它们指向正确的资源。
4.  **回编与测试**：使用工具将修改后的文件重新打包成APK，签名并安装测试。

![](img/be42712f3ef1fd95cdc972b1083329d7_139.png)

![](img/be42712f3ef1fd95cdc972b1083329d7_141.png)

这个过程原理不复杂，但操作需细致，尤其要注意资源ID的唯一性和正确性。任何资源ID未定义或冲突都会导致编译失败或运行时崩溃。

## 总结

本节课中我们一起学习了Android应用程序的基础结构。我们明确了`java`、`res`、`manifests`三大核心目录的职责，了解了资源如何通过限定符适配不同设备，并掌握了代码通过`R`类生成的资源ID与资源关联的机制。

![](img/be42712f3ef1fd95cdc972b1083329d7_143.png)

最后，通过一个插入启动屏的实战案例，我们体验了修改APK资源的基本流程和注意事项，特别是资源ID的修复，这是逆向修改中的关键环节。理解这些基础知识，是后续进行更深入逆向分析的重要前提。