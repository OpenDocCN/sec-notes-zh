![](img/008d492789cb1ccdbbfd652c8adc2011_0.png)

![](img/008d492789cb1ccdbbfd652c8adc2011_2.png)

# i春秋零基础入门Android逆向 - 课时3：Android 加壳保护工具编写2 🛡️

![](img/008d492789cb1ccdbbfd652c8adc2011_3.png)

在本节课中，我们将学习如何在Android应用的Native层（JNI层）实现加壳保护。上一节我们介绍了在Java层完成DEX文件的释放、解密和内存加载替换。本节中，我们将看看如何在更底层的JNI层实现相同的功能，这更接近于实际商业壳的加密方式。

## 概述

![](img/008d492789cb1ccdbbfd652c8adc2011_5.png)

![](img/008d492789cb1ccdbbfd652c8adc2011_6.png)

![](img/008d492789cb1ccdbbfd652c8adc2011_7.png)

本节课的核心内容是通过JNI层代码来实现加壳。我们将分析一个参考了阿里早期实验性加壳方案的代码，讲解其基本架构、关键函数以及如何完成DEX文件的解密与内存替换。实现原理与Java层类似，但执行在Native环境，涉及JNI编程和系统底层API调用。

![](img/008d492789cb1ccdbbfd652c8adc2011_9.png)

## JNI层加壳的基本架构

![](img/008d492789cb1ccdbbfd652c8adc2011_11.png)

![](img/008d492789cb1ccdbbfd652c8adc2011_12.png)

![](img/008d492789cb1ccdbbfd652c8adc2011_13.png)

JNI层加壳的基本流程与Java层非常相似。入口点同样在`Application`的`attachBaseContext`方法中，但核心的释放、解密和替换操作将在Native函数中完成。

以下是JNI层实现的核心步骤框架：

1.  **初始化与函数注册**：在`JNI_OnLoad`函数中，判断系统版本并注册需要重写的JNI方法。
2.  **重写`attachBaseContext`**：在Native层实现该方法，这是加壳逻辑的起点。
3.  **调用父类方法**：首先必须调用`super.attachBaseContext`，否则后续操作会导致崩溃。
4.  **释放与解密资源**：从APK资源文件中读取加密的DEX数据到内存，并进行解密。
5.  **内存加载与替换**：将解密后的DEX数据加载，并替换`ClassLoader`中相关的`cookie`数据。

## 关键代码流程分析

下面我们来详细分析JNI层实现中的几个关键部分。

### 1. JNI_OnLoad 函数

这是动态链接库（.so文件）加载时系统调用的函数，用于初始化。

```c
JNIEXPORT jint JNI_OnLoad(JavaVM* vm, void* reserved) {
    JNIEnv* env = NULL;
    // 获取JNI环境
    if (vm->GetEnv((void**) &env, JNI_VERSION_1_6) != JNI_OK) {
        return JNI_ERR;
    }
    // 注册Native方法，例如重写attachBaseContext的方法
    jclass clazz = env->FindClass("com/example/ShellApplication");
    if (clazz == NULL) {
        return JNI_ERR;
    }
    JNINativeMethod methods[] = {
        {"attachBaseContext", "(Landroid/content/Context;)V", (void*)native_attachBaseContext}
    };
    if (env->RegisterNatives(clazz, methods, sizeof(methods)/sizeof(methods[0])) < 0) {
        return JNI_ERR;
    }
    // 初始化其他辅助类或结构体（存放jmethodID, jfieldID等）
    init_helper_class(env);
    return JNI_VERSION_1_6;
}
```

### 2. Native 层 attachBaseContext 实现

这是加壳逻辑的核心函数。

```c
void native_attachBaseContext(JNIEnv* env, jobject thiz, jobject context) {
    // 1. 调用父类的attachBaseContext方法
    jclass super_clazz = env->GetObjectClass(thiz);
    jmethodID super_method = env->GetMethodID(super_clazz, "attachBaseContext", "(Landroid/content/Context;)V");
    env->CallVoidMethod(thiz, super_method, context);

    // 2. 初始化并获取路径信息
    char app_data_dir[PATH_MAX];
    char lib_dir[PATH_MAX];
    // ... 获取应用数据目录、lib目录、APK路径等代码 ...

    // 3. 从资源文件释放加密的DEX到内存
    void* encrypted_dex_data = NULL;
    size_t dex_size = 0;
    release_dex_from_assets(env, context, &encrypted_dex_data, &dex_size);

    // 4. 对内存中的DEX数据进行解密
    // 解密算法需与加密外壳对应
    decrypt_dex_in_memory(encrypted_dex_data, dex_size);

    // 5. 将解密后的DEX数据加载并替换到ClassLoader的cookie中
    replace_cookie_with_dex(env, context, encrypted_dex_data, dex_size);
}
```

### 3. 释放与解密函数

以下是释放资源文件和解密函数的示意代码。

```c
// 从assets读取加密的DEX文件到内存
void release_dex_from_assets(JNIEnv* env, jobject context, void** out_data, size_t* out_size) {
    // 获取AssetManager
    jclass context_clazz = env->GetObjectClass(context);
    jmethodID get_assets_method = env->GetMethodID(context_clazz, "getAssets", "()Landroid/content/res/AssetManager;");
    jobject asset_manager = env->CallObjectMethod(context, get_assets_method);

    // 打开assets中的文件（例如 “encrypted.dex”）
    AAssetManager* mgr = AAssetManager_fromJava(env, asset_manager);
    AAsset* asset = AAssetManager_open(mgr, "encrypted.dex", AASSET_MODE_BUFFER);
    off_t file_size = AAsset_getLength(asset);

    // 分配内存并读取
    *out_data = malloc(file_size);
    *out_size = file_size;
    AAsset_read(asset, *out_data, file_size);
    AAsset_close(asset);
}

// 内存解密函数（示例为简单的异或解密）
void decrypt_dex_in_memory(void* data, size_t size) {
    char key = 0xAA; // 示例密钥，实际应更复杂
    char* p = (char*)data;
    for (size_t i = 0; i < size; ++i) {
        p[i] ^= key;
    }
}
```

### 4. 替换 ClassLoader 的 Cookie

![](img/008d492789cb1ccdbbfd652c8adc2011_15.png)

![](img/008d492789cb1ccdbbfd652c8adc2011_17.png)

这是将解密后的DEX“注入”到运行环境的关键步骤。其原理是找到`PathClassLoader`内部`DexPathList`的`dexElements`数组，并替换其中`DexFile`对象的`mCookie`值。

```c
void replace_cookie_with_dex(JNIEnv* env, jobject context, void* dex_data, size_t dex_size) {
    // 1. 获取当前应用的ClassLoader
    jclass context_clazz = env->GetObjectClass(context);
    jmethodID get_classloader_method = env->GetMethodID(context_clazz, "getClassLoader", "()Ljava/lang/ClassLoader;");
    jobject class_loader = env->CallObjectMethod(context, get_classloader_method);

    // 2. 通过反射获取DexPathList和dexElements数组
    // ... 反射调用代码，获取到jobjectArray dexElements ...

    // 3. 获取第一个（或指定）DexFile对象
    jobject first_dex_file = env->GetObjectArrayElement(dex_elements_array, 0);
    jclass dex_file_clazz = env->GetObjectClass(first_dex_file);

    // 4. 关键：获取mCookie字段（一个long型指针）
    jfieldID cookie_field = env->GetFieldID(dex_file_clazz, "mCookie", "J");
    jlong old_cookie = env->GetLongField(first_dex_file, cookie_field);

    // 5. 使用系统API（如dvmDexFileOpenPartial）将内存dex数据转换为合法的DvmDex结构体
    // 此函数是Android运行时内部函数，通常通过dlsym获取指针调用
    void* new_dvm_dex = NULL;
    // 假设已获取到函数指针 dvmDexFileOpenPartial
    int result = dvmDexFileOpenPartial(dex_data, dex_size, &new_dvm_dex);
    if (result == 0) { // 成功
        // 6. 替换mCookie的值
        env->SetLongField(first_dex_file, cookie_field, (jlong)new_dvm_dex);
        // 注意：需要妥善处理旧的cookie指向的内存，避免泄漏
    }
}
```

**注意**：`dvmDexFileOpenPartial` 或 `openDexFile` 等是系统内部函数。在脱壳分析中，常在此类函数下断点来获取解密后的DEX内存，这就是“内存脱壳”的原理之一。

![](img/008d492789cb1ccdbbfd652c8adc2011_19.png)

## 外壳加密工具对应实现

![](img/008d492789cb1ccdbbfd652c8adc2011_21.png)

有加壳必然有配套的加密外壳工具。外壳工具的工作流程是逆向的：

1.  提取原始APK中的DEX文件。
2.  使用约定的算法（如上面示例的异或）对DEX文件进行加密。
3.  将加密后的DEX文件作为资源（如assets）打包进一个新的“外壳”APK中。
4.  这个外壳APK包含了我们上面编写的JNI层解密和加载代码（`libshell.so`）。
5.  同时，外壳工具需要修改原APK的`AndroidManifest.xml`，将启动的`Application`替换为我们自定义的`ShellApplication`。

## 总结

本节课中我们一起学习了Android加壳在JNI层的实现。我们了解到：

*   **架构相似性**：JNI层加壳与Java层核心思路一致，均在`attachBaseContext`中完成解密和替换。
*   **实现位置**：核心操作移至Native层，提高了逆向分析的难度。
*   **关键技术点**：包括JNI函数注册、资源文件读取、内存解密，以及最关键的一步——通过替换`ClassLoader`内部`DexFile`对象的`mCookie`值来完成DEX的“偷梁换柱”。
*   **与脱壳的关联**：加壳时使用的系统内部函数（如`dvmDexFileOpenPartial`）正是脱壳分析中常用的断点位置。

![](img/008d492789cb1ccdbbfd652c8adc2011_23.png)

![](img/008d492789cb1ccdbbfd652c8adc2011_25.png)

JNI层加壳涉及较多的C/C++编程、JNI接口调用以及Android运行时内部知识，实现复杂度较高。建议结合提供的示例代码，并参考成熟开源壳或商业壳的分析文档进行深入研究。通过本节课的学习，你应该对Android应用加壳保护的整体流程，特别是底层实现，有了更深入的理解。