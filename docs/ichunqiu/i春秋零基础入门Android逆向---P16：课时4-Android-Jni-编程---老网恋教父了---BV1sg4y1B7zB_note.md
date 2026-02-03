![](img/f3f1012c50c8250de4910c7f1f5b1600_0.png)

# 课程4：Android JNI 编程入门教程 🛠️

在本节课中，我们将学习Android JNI编程的基础知识。JNI是连接Java层和Native层的桥梁，理解它是进行Android逆向分析的关键。我们将从环境配置开始，逐步学习如何编写、编译和调用Native代码。

![](img/f3f1012c50c8250de4910c7f1f5b1600_2.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_3.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_5.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_7.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_8.png)

## 概述

![](img/f3f1012c50c8250de4910c7f1f5b1600_9.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_11.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_13.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_14.png)

Android应用的代码分为两层：Java层和Native层。Java层代码编译为DEX文件，相对容易分析。而Native层代码由C/C++编写，编译为SO文件（动态链接库），其运行的是ARM或x86汇编指令，在安全性和隐蔽性上远高于Java层。因此，许多核心的加密算法和操作都放在Native层。学习JNI编程，是我们逆向分析Native层代码的第一步。

![](img/f3f1012c50c8250de4910c7f1f5b1600_15.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_17.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_19.png)

## 开发环境配置

![](img/f3f1012c50c8250de4910c7f1f5b1600_20.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_22.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_23.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_25.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_26.png)

上一节我们概述了JNI的重要性，本节中我们来看看如何配置开发环境。我们使用Android Studio进行开发，因为它内置了对原生NDK代码的支持。

![](img/f3f1012c50c8250de4910c7f1f5b1600_28.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_29.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_31.png)

以下是配置步骤：

![](img/f3f1012c50c8250de4910c7f1f5b1600_33.png)

1.  修改项目根目录下的 `build.gradle` 文件，将Gradle插件版本更新至2.10或更高。
    ```groovy
    classpath 'com.android.tools.build:gradle:2.10'
    ```

![](img/f3f1012c50c8250de4910c7f1f5b1600_35.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_36.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_38.png)

2.  在App模块的 `build.gradle` 文件中，应用新的插件并配置NDK。
    ```groovy
    apply plugin: 'com.android.application'

    android {
        ...
        defaultConfig {
            ...
            ndk {
                moduleName "hello" // 指定生成的SO库名称
            }
        }
    }
    ```

![](img/f3f1012c50c8250de4910c7f1f5b1600_40.png)

3.  同步Gradle项目，完成环境配置。

## 静态注册JNI方法

环境配置好后，我们开始编写第一个JNI方法。静态注册是JNI最基础的绑定方式，其函数名遵循特定规则。

首先，在Java类中声明一个Native方法。
```java
public class MainActivity extends AppCompatActivity {
    // 声明一个静态的Native方法
    public static native String helloFromJNI();
    // 声明一个非静态的Native方法
    public native String helloFromJNI2();
}
```

接着，在 `src/main/jni` 目录下创建对应的C/C++源文件（例如 `hello.c`），并实现函数。函数名由 `Java_`、包名、类名和方法名拼接而成。
```c
#include <jni.h>

![](img/f3f1012c50c8250de4910c7f1f5b1600_42.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_44.png)

// 对应静态方法 helloFromJNI
JNIEXPORT jstring JNICALL
Java_com_example_app_MainActivity_helloFromJNI(JNIEnv *env, jclass clazz) {
    return (*env)->NewStringUTF(env, "Hello from static JNI!");
}

![](img/f3f1012c50c8250de4910c7f1f5b1600_46.png)

// 对应实例方法 helloFromJNI2
JNIEXPORT jstring JNICALL
Java_com_example_app_MainActivity_helloFromJNI2(JNIEnv *env, jobject thiz) {
    return (*env)->NewStringUTF(env, "Hello from instance JNI!");
}
```

最后，在Java代码加载对应的SO库并调用Native方法。
```java
public class MainActivity extends AppCompatActivity {
    static {
        System.loadLibrary("hello"); // 加载名为 libhello.so 的库
    }
    // ... 调用 helloFromJNI() 和 helloFromJNI2()
}
```

![](img/f3f1012c50c8250de4910c7f1f5b1600_48.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_49.png)

## 动态注册JNI方法

![](img/f3f1012c50c8250de4910c7f1f5b1600_51.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_53.png)

静态注册的函数名冗长且不易维护。因此，我们可以使用动态注册来更灵活地关联Java方法和Native函数。

首先，Java层的Native声明与静态注册相同。
```java
public native String helloDynamic();
```

![](img/f3f1012c50c8250de4910c7f1f5b1600_55.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_56.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_58.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_60.png)

然后，在C/C++层，我们不再使用复杂的固定函数名，而是实现一个函数映射表和一个初始化函数。
```cpp
#include <jni.h>
#include <string>

![](img/f3f1012c50c8250de4910c7f1f5b1600_62.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_64.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_66.png)

// 实际执行的Native函数
jstring native_hello(JNIEnv *env, jobject thiz) {
    return env->NewStringUTF("Hello from dynamic JNI!");
}

// 方法映射表，将Java方法名、签名与C++函数指针关联
static JNINativeMethod gMethods[] = {
        {"helloDynamic", "()Ljava/lang/String;", (void *)native_hello}
};

![](img/f3f1012c50c8250de4910c7f1f5b1600_68.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_70.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_72.png)

// SO库加载时自动调用的初始化函数
JNIEXPORT jint JNI_OnLoad(JavaVM* vm, void* reserved) {
    JNIEnv* env = nullptr;
    if (vm->GetEnv((void**)&env, JNI_VERSION_1_6) != JNI_OK) {
        return -1;
    }

    // 找到要注册Native方法的Java类
    jclass clazz = env->FindClass("com/example/app/MainActivity");
    if (clazz == nullptr) {
        return -1;
    }

    // 进行动态注册
    if (env->RegisterNatives(clazz, gMethods, sizeof(gMethods)/sizeof(gMethods[0])) < 0) {
        return -1;
    }

    return JNI_VERSION_1_6;
}
```
动态注册通过 `JNI_OnLoad` 函数在SO库加载时完成方法绑定，使得代码更清晰，也增加了逆向分析的难度。

![](img/f3f1012c50c8250de4910c7f1f5b1600_74.png)

## 总结

本节课我们一起学习了Android JNI编程的核心内容。

我们首先了解了Java层与Native层的区别以及JNI的作用。然后，我们一步步配置了Android Studio的NDK开发环境。接着，我们通过实例学习了**静态注册**和**动态注册**两种将Java方法与C/C++函数关联的方式。静态注册简单直接，但命名繁琐；动态注册则更加灵活高效，是实际开发中更常用的技术。

![](img/f3f1012c50c8250de4910c7f1f5b1600_76.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_78.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_79.png)

![](img/f3f1012c50c8250de4910c7f1f5b1600_81.png)

掌握JNI编程是分析Android应用Native层逻辑的基石，请大家务必熟悉这两种注册方式，并尝试在真机上进行调试。