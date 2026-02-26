#  011：课时3 快速Hook代码搭建之 Xposed 🛠️

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_1.png)

在本节课中，我们将学习如何使用Xposed框架来快速搭建Hook代码。Xposed是一款广泛使用的Hook框架，拥有丰富的插件生态，可用于优化手机功能、添加特性，甚至进行应用破解。我们将通过一个修改颜色的实例，一步步学习如何创建和配置一个Xposed模块。

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_3.png)

## 概述

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_5.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_6.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_7.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_9.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_11.png)

Xposed是一个开源的Hook框架，支持Android 4.0.3到4.4版本（更高版本需使用新版）。与Cydia Substrate（Cydia Impactor）不同，Xposed在程序启动时即可对特定进程进行Hook，这使得它能够更精确地定位目标应用。本节课我们将通过编写一个Xposed模块，Hook一个`getColor`函数并修改其返回值，来掌握Xposed的基本使用方法。

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_13.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_15.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_16.png)

## Xposed框架的安装与配置

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_18.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_20.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_22.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_24.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_26.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_27.png)

上一节我们介绍了Cydia Substrate框架，本节中我们来看看Xposed框架的安装与基础配置。

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_29.png)

首先，需要从Xposed官网下载对应的APK安装包并在手机上安装。安装完成后，在Xposed安装器中安装框架组件，然后重启手机使框架生效。这个过程与Cydia Substrate类似。

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_31.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_32.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_33.png)

## 创建Xposed模块项目

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_35.png)

现在，我们开始创建一个Android项目，并为其添加Xposed支持。

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_37.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_38.png)

### 1. 导入Xposed开发库

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_39.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_40.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_42.png)

首先，需要将Xposed的API库文件（通常是一个JAR文件，如`XposedBridgeApi-54.jar`）导入到项目中。

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_44.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_46.png)

**操作步骤如下：**
1.  在项目的`app`目录下（注意不要放在`libs`文件夹内，以免被编译进DEX文件），新建一个文件夹（例如`provided_libs`）用于存放库文件。
2.  将下载的JAR文件复制到该文件夹。
3.  修改项目的`build.gradle`文件，在`dependencies`区块中以`provided`方式引入该库，以确保它不参与最终编译。

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_47.png)

```gradle
dependencies {
    provided files('provided_libs/XposedBridgeApi-54.jar')
    // ... 其他依赖
}
```
4.  点击同步按钮刷新项目。

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_49.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_50.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_52.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_54.png)

### 2. 修改AndroidManifest.xml文件

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_55.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_57.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_59.png)

接下来，需要在清单文件中声明模块信息，以便Xposed框架能够识别我们的应用为一个模块。

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_61.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_62.png)

**需要添加的元数据如下：**
*   `xposedmodule`: 声明此为Xposed模块。
*   `xposeddescription`: 模块的描述信息，会显示在Xposed安装器的模块列表中。
*   `xposedminversion`: 模块所需的最低Xposed版本号（需与API库版本匹配，例如54）。

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_63.png)

在`<application>`标签内添加以下代码：

```xml
<meta-data
    android:name="xposedmodule"
    android:value="true" />
<meta-data
    android:name="xposeddescription"
    android:value="Hook getColor函数示例" />
<meta-data
    android:name="xposedminversion"
    android:value="54" />
```

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_65.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_66.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_67.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_68.png)

## 编写Xposed模块入口类

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_70.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_71.png)

配置完成后，我们开始编写核心的Hook代码。Xposed模块需要一个入口类来实现特定的接口。

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_73.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_74.png)

### 1. 创建入口类并实现接口

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_76.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_77.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_79.png)

创建一个Java类（例如`MainHook`），并实现`IXposedHookLoadPackage`接口。该接口包含一个`handleLoadPackage`方法，系统每加载一个应用程序包（APK）时都会调用此方法。

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_80.png)

```java
public class MainHook implements IXposedHookLoadPackage {
    @Override
    public void handleLoadPackage(final LoadPackageParam lpparam) throws Throwable {
        // Hook逻辑将写在这里
    }
}
```
`LoadPackageParam`参数包含了当前加载应用的信息，如包名(`lpparam.packageName`)、类加载器(`lpparam.classLoader`)等。

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_82.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_83.png)

### 2. 定位并Hook目标方法

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_85.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_86.png)

在`handleLoadPackage`方法中，我们可以判断当前加载的是否为目标应用，然后进行Hook。Xposed提供了`XposedHelpers.findAndHookMethod`这个便捷方法来查找并Hook指定方法。

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_88.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_89.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_91.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_92.png)

以下是Hook一个`getColor`方法的示例代码框架：

```java
@Override
public void handleLoadPackage(final LoadPackageParam lpparam) throws Throwable {
    // 示例：Hook所有应用的 getColor 方法
    // 实际使用时，可通过 lpparam.packageName 判断特定应用
    try {
        XposedHelpers.findAndHookMethod(
                "android.graphics.Color", // 目标类名
                lpparam.classLoader,      // 目标类所在的ClassLoader
                "getColor",               // 方法名
                String.class,             // 方法的参数类型
                new XC_MethodHook() {     // Hook回调
                    @Override
                    protected void afterHookedMethod(MethodHookParam param) throws Throwable {
                        // 在目标方法执行后，修改返回值
                        param.setResult(0xFFFF0000); // 修改为红色
                    }
                });
    } catch (Exception e) {
        // 必须进行异常捕获，防止Hook失败导致系统崩溃
        XposedBridge.log(e);
    }
}
```
**代码解析：**
*   `findAndHookMethod`：第一个参数是类名（字符串形式），第二个是`ClassLoader`，第三个是方法名，之后是方法的参数类型列表，最后一个参数是`XC_MethodHook`回调实例。
*   `XC_MethodHook`：需要重写`beforeHookedMethod`（方法执行前）和`afterHookedMethod`（方法执行后）来插入我们的逻辑。本例在`afterHookedMethod`中修改了方法的返回值。
*   **异常处理**：Xposed Hook是系统级操作，必须用`try-catch`包裹，避免因找不到类或方法而导致系统崩溃。

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_94.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_95.png)

### 3. 创建并配置xposed_init文件

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_97.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_98.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_99.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_100.png)

最后，需要创建一个资源文件来告诉Xposed框架我们的入口类是哪一个。

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_101.png)

**操作步骤如下：**
1.  在项目的`assets`目录下新建一个文本文件，命名为`xposed_init`（如果没有`assets`目录则新建）。
2.  在文件中写入我们入口类的完整包名和类名，例如：
    ```
    com.example.myxposedmodule.MainHook
    ```
    如果有多个入口类，每行写一个。

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_103.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_105.png)

## 模块的编译、安装与测试

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_106.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_108.png)

完成代码编写后，编译项目生成APK，并将其安装到已激活Xposed框架的手机上。

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_110.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_112.png)

1.  **安装模块**：在Xposed安装器的“模块”选项中，勾选我们刚刚安装的模块。
2.  **重启系统**：根据提示重启手机，使模块生效。
3.  **测试效果**：运行目标应用，观察`getColor`方法的返回值是否已被成功修改（例如，相关颜色是否变成了红色）。

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_114.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_116.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_117.png)

**调试提示**：Xposed模块默认不便于调试。可以在代码中调用`XposedBridge.log(String)`来输出日志，日志可以在Xposed安装器的“日志”页面查看，这有助于排查问题。

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_119.png)

## Xposed与Cydia Substrate的对比

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_121.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_122.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_123.png)

在本节中，我们完成了Xposed模块的搭建。现在我们来总结一下Xposed与上一节课学习的Cydia Substrate的主要区别：

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_125.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_127.png)

以下是两者核心特性的对比：

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_129.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_130.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_132.png)

| 特性 | Xposed | Cydia Substrate |
| :--- | :--- | :--- |
| **Hook粒度** | **进程级**。可以精确针对特定应用包进行Hook。 | **类级**。Hook系统类，对所有使用该类的应用生效。 |
| **目标定位** | 通过`LoadPackageParam`轻松获取包名，易于定位特定应用。 | 难以区分Hook逻辑应用于哪个具体应用。 |
| **类获取方式** | 可通过字符串类名和`ClassLoader`动态查找类，更灵活。 | 通常需要直接引用类（如`TargetClass.class`）。 |
| **代码编写模式** | 提供`beforeHookedMethod`和`afterHookedMethod`两个明确的切入点。 | 直接在`hookMethod`中编写替换逻辑，控制更直接，可多次调用原方法。 |
| **灵活性** | 在`beforeHookedMethod`中调用`param.setResult`可阻止原方法执行，实现“提前返回”。 | 逻辑编写相对自由，但需自行处理调用关系。 |

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_134.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_136.png)

**关于ClassLoader**：在Android中，每个DEX文件（即每个应用）都有自己的`ClassLoader`。Xposed通过传入目标应用的`ClassLoader`，能够在其类加载器中找到并Hook特定的类，这是实现进程级Hook的关键。Java反射操作（如`Class.forName()`）通常也需要指定正确的`ClassLoader`。

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_138.png)

## 总结

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_140.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_142.png)

![](img/dcb60b8ef6cf8f456f4d96369f195b1f_144.png)

本节课中我们一起学习了如何使用Xposed框架搭建Hook代码。我们从一个空白项目开始，逐步完成了导入开发库、配置清单文件、编写实现了`IXposedHookLoadPackage`接口的入口类、使用`XposedHelpers.findAndHookMethod`方法Hook目标函数，以及创建`xposed_init`配置文件的全过程。通过对比Xposed和Cydia Substrate，我们了解到Xposed在针对特定应用进行Hook时更具优势，而Cydia在Hook系统底层类时更为直接。掌握这两种框架的使用，能为Android逆向分析提供强大的工具支持。