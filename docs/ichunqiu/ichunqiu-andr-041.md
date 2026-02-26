#  041：课时2 Android 加壳保护工具编写1 🛡️

![](img/01c8fe5c1044a006165dce44b78ae45a_0.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_2.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_4.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_6.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_8.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_10.png)

在本节课中，我们将要学习如何编写一个基于Java层的Android应用加壳保护工具。我们将从分析一个现成的加壳/脱壳代码入手，理清其核心思路、关键方法和操作流程，并最终实现一个简单的加壳工具。

![](img/01c8fe5c1044a006165dce44b78ae45a_12.png)

## 概述

Android应用的加壳保护，其核心思想是在原始应用（“壳内代码”）外部包裹一层保护代码（“外壳”）。外壳程序负责在运行时解密并加载原始的DEX文件，从而保护核心逻辑不被轻易分析。本节课将重点讲解在Java层实现这一过程的基本原理。

## 加壳程序入口点分析

上一节我们介绍了Android应用的基本结构，本节中我们来看看加壳程序的入口点是如何设置的。

每一个被加固的Android程序，其入口点都定义在`AndroidManifest.xml`文件的`<application>`标签的`android:name`属性中。加固后的APK会将该属性指向我们编写的外壳`Application`类。

```xml
<application android:name=".MyShellApplication" ... >
```

![](img/01c8fe5c1044a006165dce44b78ae45a_14.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_16.png)

通过这个入口点，外壳程序就能在应用启动时最先获得控制权，执行解密和加载操作。

![](img/01c8fe5c1044a006165dce44b78ae45a_18.png)

## 外壳Application的关键函数

![](img/01c8fe5c1044a006165dce44b78ae45a_20.png)

外壳`Application`类主要通过重写两个关键函数来完成脱壳工作：
1.  `attachBaseContext(Context base)`
2.  `onCreate()`

![](img/01c8fe5c1044a006165dce44b78ae45a_22.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_23.png)

这两个函数会在程序执行时被自动调用，且有明确的先后顺序：**`attachBaseContext` 会先于 `onCreate` 被调用**。因此，常见的做法是：
*   在 `attachBaseContext` 中完成加密的原始DEX文件的解密、释放和`ClassLoader`的替换。
*   在 `onCreate` 中完成原始`Application`对象的替换和初始化。

下面，我们来详细分析这两个函数中的操作。

### 1. attachBaseContext：解密与加载DEX

这个函数的核心任务是解密并加载原始的DEX文件。

![](img/01c8fe5c1044a006165dce44b78ae45a_25.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_27.png)

**操作流程如下：**

![](img/01c8fe5c1044a006165dce44b78ae45a_28.png)

首先，外壳程序会将原始APK的`classes.dex`文件加密后，作为资源（例如放在`assets`目录下）打包进APK。假设加密后的文件名为`encrypted.dex`。

![](img/01c8fe5c1044a006165dce44b78ae45a_30.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_31.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_32.png)

在`attachBaseContext`中：
1.  **获取上下文与创建目录**：获取应用上下文，并创建一个临时目录用于存放解密后的文件。
    ```java
    File dexDir = context.getDir("dex", Context.MODE_PRIVATE);
    ```
2.  **解密资源文件**：从`assets`目录读取`encrypted.dex`文件，调用解密函数进行解密，将解密后的数据写入临时目录（例如`decrypted.dex`）。
    ```java
    // 伪代码示意
    InputStream is = assetManager.open("encrypted.dex");
    byte[] encryptedData = readStream(is);
    byte[] decryptedData = decrypt(encryptedData); // 调用解密方法
    FileOutputStream fos = new FileOutputStream(new File(dexDir, "decrypted.dex"));
    fos.write(decryptedData);
    ```
3.  **反射获取关键对象**：通过反射机制，获取当前进程的`ActivityThread`对象和对应的`LoadedApk`对象。`LoadedApk`包含了当前已加载APK的信息。
    ```java
    // 伪代码，通过反射获取ActivityThread
    Class<?> activityThreadClass = Class.forName("android.app.ActivityThread");
    Method currentActivityThreadMethod = activityThreadClass.getDeclaredMethod("currentActivityThread");
    Object currentActivityThread = currentActivityThreadMethod.invoke(null);
    ```
4.  **创建并替换ClassLoader**：使用`DexClassLoader`加载解密后的`decrypted.dex`文件，生成一个新的`ClassLoader`。然后，通过反射，将这个新的`ClassLoader`设置到之前获取的`LoadedApk`对象的`mClassLoader`字段中。
    ```java
    DexClassLoader newClassLoader = new DexClassLoader(dexPath, optimizedDirectory, librarySearchPath, parentClassLoader);
    // 通过反射将 newClassLoader 设置到 loadedApk.mClassLoader
    Field mClassLoaderField = loadedApk.getClass().getDeclaredField("mClassLoader");
    mClassLoaderField.setAccessible(true);
    mClassLoaderField.set(loadedApk, newClassLoader);
    ```
完成以上步骤后，后续Java层所有类的加载请求都会由我们这个新的`ClassLoader`来处理，从而执行原始的DEX代码。

![](img/01c8fe5c1044a006165dce44b78ae45a_34.png)

### 2. onCreate：替换原始Application

![](img/01c8fe5c1044a006165dce44b78ae45a_36.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_38.png)

如果原始APK有自己的`Application`类，我们需要在`onCreate`中将其重建并替换回来，以确保原始`Application`的生命周期回调（如`onCreate`）能被正确执行。

![](img/01c8fe5c1044a006165dce44b78ae45a_40.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_41.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_42.png)

**操作流程如下：**

![](img/01c8fe5c1044a006165dce44b78ae45a_44.png)

1.  **读取原始Application信息**：外壳程序需要知道原始`Application`类的全名。这个信息可以预先保存在`AndroidManifest.xml`的MetaData中，或在加壳时保存到外壳的某个字段里。
    ```java
    String originalApplicationName = getMetaDataFromManifest("ORIGINAL_APPLICATION");
    ```
2.  **反射创建原始Application实例**：通过反射，使用上一步获取的类名创建原始`Application`对象。
    ```java
    Class<?> originalAppClass = Class.forName(originalApplicationName);
    Application originalApplication = (Application) originalAppClass.newInstance();
    ```
3.  **替换Application引用**：通过反射，将`ActivityThread`和`LoadedApk`中所有对当前外壳`Application`的引用，替换为我们刚刚创建的原始`Application`对象。这通常涉及修改多个字段，例如`ActivityThread`中的`mInitialApplication`、`mAllApplications`，以及`LoadedApk`中的`mApplication`等。
    ```java
    // 伪代码，替换ActivityThread中的mInitialApplication
    Field mInitialApplicationField = activityThread.getClass().getDeclaredField("mInitialApplication");
    mInitialApplicationField.setAccessible(true);
    mInitialApplicationField.set(activityThread, originalApplication);
    ```
4.  **调用原始Application的attachBaseContext和onCreate**：手动调用原始`Application`对象的`attachBaseContext`和`onCreate`方法，完成其初始化。
    ```java
    Method attachMethod = Application.class.getDeclaredMethod("attach", Context.class);
    attachMethod.setAccessible(true);
    attachMethod.invoke(originalApplication, baseContext);

    originalApplication.onCreate();
    ```
至此，一个完整的Java层加壳/脱壳流程就实现了。

![](img/01c8fe5c1044a006165dce44b78ae45a_46.png)

## 加壳工具编写

![](img/01c8fe5c1044a006165dce44b78ae45a_48.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_50.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_52.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_54.png)

我们编写了加壳程序，还需要一个配套的“加壳工具”来对原始APK进行自动化处理。

![](img/01c8fe5c1044a006165dce44b78ae45a_56.png)

这个加壳工具（通常是一个命令行Java程序）需要完成以下工作：

![](img/01c8fe5c1044a006165dce44b78ae45a_58.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_60.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_62.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_63.png)

以下是加壳工具的主要步骤：

![](img/01c8fe5c1044a006165dce44b78ae45a_65.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_66.png)

1.  **反编译目标APK**：使用Apktool等工具反编译原始APK，但不反编译DEX文件（使用`-s`参数），以便直接操作`classes.dex`。
    ```bash
    apktool d -s original.apk -o temp_dir
    ```
2.  **加密原始DEX**：读取反编译目录中的`classes.dex`文件，使用预定义的加密算法（如简单的异或）进行加密，然后将加密后的文件放入`assets`目录。
    ```java
    byte[] dexData = Files.readAllBytes(Paths.get("temp_dir/classes.dex"));
    byte[] encryptedData = encrypt(dexData);
    Files.write(Paths.get("temp_dir/assets/encrypted.dex"), encryptedData);
    ```
3.  **注入外壳代码**：将我们预先编译好的外壳`Application`的Smali代码，拷贝到反编译目录的对应位置（通常是`smali`或`smali_classes2`等目录）。
4.  **修改AndroidManifest.xml**：修改`AndroidManifest.xml`，将`<application>`标签的`android:name`属性修改为我们外壳`Application`的类名。如果原始APK有自定义`Application`，需要将其类名保存到MetaData中。
    ```xml
    <application android:name="com.shell.MyShellApplication" ...>
        <meta-data android:name="ORIGINAL_APPLICATION" android:value="com.original.OriginalApplication"/>
    </application>
    ```
5.  **回编与签名**：使用Apktool回编修改后的目录，生成新的APK文件，并对其进行签名。
    ```bash
    apktool b temp_dir -o protected.apk
    jarsigner -keystore my.keystore protected.apk alias_name
    ```

## 核心概念：反射（Reflection）

![](img/01c8fe5c1044a006165dce44b78ae45a_68.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_69.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_71.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_73.png)

在整个加壳/脱壳过程中，**反射（Reflection）** 机制起到了至关重要的作用。它允许我们在运行时动态地获取类的信息（字段、方法）、创建对象、调用方法和修改字段值，即使这些字段或方法是`private`的。

![](img/01c8fe5c1044a006165dce44b78ae45a_75.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_77.png)

**代码示例：获取并设置私有字段**
```java
// 获取目标类
Class<?> targetClass = Class.forName("android.app.ActivityThread");
// 获取指定的字段（例如 mInitialApplication）
Field field = targetClass.getDeclaredField("mInitialApplication");
// 设置可访问性，突破private限制
field.setAccessible(true);
// 获取某个ActivityThread实例
Object activityThread = ...;
// 获取该字段的值
Object oldValue = field.get(activityThread);
// 为该字段设置新的值
field.set(activityThread, newValue);
```
正是通过反射，我们才能深入系统内部，修改`ActivityThread`、`LoadedApk`等关键对象的状态，实现`ClassLoader`和`Application`的替换。

![](img/01c8fe5c1044a006165dce44b78ae45a_78.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_80.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_82.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_83.png)

## 总结

![](img/01c8fe5c1044a006165dce44b78ae45a_85.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_87.png)

本节课中我们一起学习了Android Java层加壳保护的基本原理和实现步骤。

![](img/01c8fe5c1044a006165dce44b78ae45a_89.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_90.png)

我们首先分析了加壳程序的入口点，即外壳`Application`。然后深入讲解了其两个核心方法`attachBaseContext`和`onCreate`的分工：前者负责**解密并替换`ClassLoader`**，后者负责**重建并替换原始`Application`**。接着，我们介绍了用于批量处理APK的**加壳工具**的工作流程，包括反编译、加密DEX、注入代码、修改清单文件和回编签名。最后，我们强调了**反射**技术在这一过程中的关键作用。

![](img/01c8fe5c1044a006165dce44b78ae45a_92.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_94.png)

![](img/01c8fe5c1044a006165dce44b78ae45a_96.png)

需要注意的是，这种纯Java层的加壳方案安全性有限，因为解密后的DEX文件会**在磁盘上留有明文副本**，且Java代码容易被反编译分析。但它作为学习加壳原理的起点，是非常合适的。在后续课程中，我们会探讨更安全的Native（C/C++）层加壳方案。