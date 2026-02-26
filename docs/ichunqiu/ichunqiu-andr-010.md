#  010：课时2 快速Hook代码搭建之 Cydia Substrate 🛠️

![](img/65643bdba9fec7a5ae2f51db64b60baa_1.png)

在本节课中，我们将要学习如何使用 Cydia Substrate 框架来快速搭建 Android 应用的 Hook 代码。我们将了解函数钩子的概念，并动手编写一个简单的插件来修改系统行为。

![](img/65643bdba9fec7a5ae2f51db64b60baa_3.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_5.png)

## 概述：什么是函数钩子？

![](img/65643bdba9fec7a5ae2f51db64b60baa_7.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_9.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_11.png)

函数钩子是一种强大的技术，它允许我们在不修改原始程序的情况下，影响特定函数的执行。通过钩子，我们可以接管整个函数，修改其参数、返回值，甚至改变整个类的行为。Cydia Substrate 就是一个在 Android 平台上实现这种技术的框架。

![](img/65643bdba9fec7a5ae2f51db64b60baa_13.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_14.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_16.png)

上一节我们介绍了逆向工程的基本概念，本节中我们来看看如何利用 Cydia Substrate 进行实际的代码挂钩操作。

![](img/65643bdba9fec7a5ae2f51db64b60baa_18.png)

## Cydia Substrate 简介与安装

![](img/65643bdba9fec7a5ae2f51db64b60baa_20.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_21.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_23.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_24.png)

Cydia Substrate 是一个用于修改系统或应用程序行为的框架。它最初在越狱的 iOS 设备上较为常见，在 Android 上，它同样可以安装插件来修改系统函数或对特定程序进行破解。

![](img/65643bdba9fec7a5ae2f51db64b60baa_25.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_27.png)

首先，我们需要在 Android 设备上安装 Cydia Substrate 的主程序（一个 APK 文件）。安装后，程序会请求 root 权限并对系统进行一些修改，修改完成后需要重启设备才能使插件生效。

![](img/65643bdba9fec7a5ae2f51db64b60baa_29.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_30.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_32.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_33.png)

**注意**：由于兼容性问题，Cydia Substrate 在某些设备上可能无法正常工作，因此在实际应用中可能使用较少，但作为学习 Hook 技术的入门工具，它非常合适。

![](img/65643bdba9fec7a5ae2f51db64b60baa_35.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_37.png)

## 编写第一个 Hook 插件

![](img/65643bdba9fec7a5ae2f51db64b60baa_38.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_39.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_40.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_42.png)

Cydia Substrate 的功能基于插件实现。我们需要创建一个独立的 APK 作为插件，并在其中编写我们的 Hook 逻辑。

![](img/65643bdba9fec7a5ae2f51db64b60baa_44.png)

以下是创建一个简单插件的主要步骤：

![](img/65643bdba9fec7a5ae2f51db64b60baa_45.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_47.png)

### 1. 创建 Android 项目并导入 SDK

首先，创建一个新的 Android 项目。由于插件可能不需要用户界面，我们可以不创建初始的 Activity。

![](img/65643bdba9fec7a5ae2f51db64b60baa_49.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_50.png)

接着，需要导入 Cydia Substrate 提供的 SDK 库（一个 JAR 文件）。将该 JAR 文件拷贝到项目的 `app/libs` 目录下，然后在项目的 `build.gradle` 文件中添加依赖。

![](img/65643bdba9fec7a5ae2f51db64b60baa_52.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_54.png)

```gradle
dependencies {
    implementation files('libs/substrate-api.jar')
}
```

![](img/65643bdba9fec7a5ae2f51db64b60baa_56.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_57.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_59.png)

SDK 中主要的类是 `MS`，它提供了 `hookClassLoad` 和 `hookMethod` 等方法用于挂钩类和方法。

![](img/65643bdba9fec7a5ae2f51db64b60baa_61.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_62.png)

### 2. 修改 AndroidManifest.xml

![](img/65643bdba9fec7a5ae2f51db64b60baa_64.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_66.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_68.png)

为了让 Cydia Substrate 识别我们的 APK 是一个插件，需要在 `AndroidManifest.xml` 文件中添加特定的元数据（meta-data）和权限。

![](img/65643bdba9fec7a5ae2f51db64b60baa_70.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_71.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_73.png)

首先，在 `<application>` 标签内添加以下 `meta-data`：

![](img/65643bdba9fec7a5ae2f51db64b60baa_75.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_76.png)

```xml
<meta-data
    android:name="com.saurik.substrate.main"
    android:value=".Main"/>
```

![](img/65643bdba9fec7a5ae2f51db64b60baa_78.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_79.png)

这里的 `android:value` 指向我们插件的入口类（例如 `.Main`）。

![](img/65643bdba9fec7a5ae2f51db64b60baa_81.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_82.png)

然后，在 `<application>` 标签外、`<manifest>` 标签内添加必要的权限：

![](img/65643bdba9fec7a5ae2f51db64b60baa_84.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_85.png)

```xml
<uses-permission android:name="cydia.permission.SUBSTRATE"/>
```

![](img/65643bdba9fec7a5ae2f51db64b60baa_87.png)

### 3. 编写 Hook 入口类

![](img/65643bdba9fec7a5ae2f51db64b60baa_88.png)

根据 `AndroidManifest.xml` 中的配置，我们创建一个名为 `Main` 的 Java 类作为插件的入口。该类需要包含一个特殊的初始化方法。

![](img/65643bdba9fec7a5ae2f51db64b60baa_90.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_92.png)

```java
public class Main {
    public static void initialize() {
        // 在这里编写我们的 Hook 逻辑
    }
}
```

![](img/65643bdba9fec7a5ae2f51db64b60baa_94.png)

Cydia Substrate 会在插件加载时自动调用 `initialize` 方法。

![](img/65643bdba9fec7a5ae2f51db64b60baa_96.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_97.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_99.png)

### 4. 实现 Hook 逻辑

![](img/65643bdba9fec7a5ae2f51db64b60baa_101.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_102.png)

Hook 的基本流程是：先找到目标类，然后在目标类中找到要挂钩的方法，最后用我们自己的方法替换它。

以下是一个示例，我们挂钩 `android.content.res.Resources` 类的 `getColor` 方法，并修改其返回值：

![](img/65643bdba9fec7a5ae2f51db64b60baa_104.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_106.png)

```java
public class Main {
    public static void initialize() {
        MS.hookClassLoad("android.content.res.Resources",
            new MS.ClassLoadHook() {
                public void classLoaded(Class<?> resources) {
                    // 类加载完成后执行
                    Method getColorMethod = null;
                    try {
                        // 获取 getColor 方法
                        getColorMethod = resources.getMethod("getColor",
                            Integer.TYPE); // int 类型的参数
                    } catch (NoSuchMethodException e) {
                        // 方法没找到，处理异常
                        getColorMethod = null;
                    }

                    if (getColorMethod != null) {
                        // 挂钩找到的方法
                        MS.hookMethod(resources, getColorMethod,
                            new MS.MethodHook() {
                                public Object invoked(Object obj,
                                                      Object... args)
                                                      throws Throwable {
                                    // 调用原始方法获取原返回值
                                    int originalColor = (Integer) MS.invokeOriginalMethod(
                                        getColorMethod, obj, args);
                                    // 修改返回值，例如与 0xFF0000FF 进行按位与操作
                                    int newColor = originalColor & 0xFF0000FF;
                                    return newColor;
                                }
                            }, null);
                    }
                }
            });
    }
}
```

![](img/65643bdba9fec7a5ae2f51db64b60baa_108.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_110.png)

**代码解释**：
*   `MS.hookClassLoad`: 用于在目标类（`android.content.res.Resources`）被加载时得到通知。
*   `classLoaded`: 当目标类加载完成后，会执行此回调函数，参数 `resources` 就是加载好的 Class 对象。
*   `getMethod`: 通过反射获取类中指定的方法（`getColor`），需要指定方法名和参数类型。
*   `MS.hookMethod`: 对找到的方法进行挂钩。
*   `invoked`: 当被挂钩的方法被调用时，会执行此回调。我们可以在这里调用原始方法（`MS.invokeOriginalMethod`）并修改其返回值。

![](img/65643bdba9fec7a5ae2f51db64b60baa_112.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_114.png)

### 5. 编译、安装与测试

![](img/65643bdba9fec7a5ae2f51db64b60baa_115.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_117.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_118.png)

将插件 APK 安装到已部署 Cydia Substrate 的设备上。安装后，Cydia Substrate 会检测到新插件并提示需要重启。重启设备后，Hook 即可生效。

![](img/65643bdba9fec7a5ae2f51db64b60baa_120.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_121.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_122.png)

在我们的例子中，系统内所有调用 `Resources.getColor()` 的地方，其返回的颜色值都会被我们修改，这可能导致应用程序界面的颜色显示异常（例如出现大片红色），从而验证了 Hook 的成功。

## Java 反射在 Hook 中的应用

![](img/65643bdba9fec7a5ae2f51db64b60baa_124.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_125.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_126.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_127.png)

在上面的例子中，我们直接通过方法名和参数类型来获取方法。但在实际逆向中，目标方法可能属于非公开的类，或者参数类型是自定义的复杂类，无法直接引用。

这时，我们就需要用到 **Java 反射** 技术。反射允许我们在运行时动态地获取类的信息并操作类的成员。

![](img/65643bdba9fec7a5ae2f51db64b60baa_129.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_130.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_131.png)

以下是 Hook 中常用的反射操作：

### 获取 Class 对象

![](img/65643bdba9fec7a5ae2f51db64b60baa_133.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_135.png)

```java
Class<?> targetClass = TargetObject.getClass();
// 或者通过类名字符串获取
Class<?> targetClass = Class.forName("com.example.TargetClass");
```

![](img/65643bdba9fec7a5ae2f51db64b60baa_137.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_138.png)

### 获取类中的方法

![](img/65643bdba9fec7a5ae2f51db64b60baa_140.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_142.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_143.png)

```java
// 获取所有 public 方法（包括父类的）
Method[] publicMethods = targetClass.getMethods();
// 获取本类声明的所有方法（包括 private 的，不包括父类的）
Method[] declaredMethods = targetClass.getDeclaredMethods();
// 获取指定的 public 方法
Method specificMethod = targetClass.getMethod("methodName", ParameterType1.class, ParameterType2.class);
// 获取指定的方法（可以是 private 的）
Method specificDeclaredMethod = targetClass.getDeclaredMethod("methodName", ParameterType1.class);
```

### 调用方法

![](img/65643bdba9fec7a5ae2f51db64b60baa_145.png)

```java
// 调用实例方法
Object result = specificMethod.invoke(targetObjectInstance, arg1, arg2);
// 调用静态方法
Object result = staticMethod.invoke(null, arg1, arg2);
```

### 获取和修改字段（Field）

![](img/65643bdba9fec7a5ae2f51db64b60baa_147.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_148.png)

```java
// 获取字段
Field field = targetClass.getDeclaredField("fieldName");
// 设置可访问性（对于 private 字段是必须的）
field.setAccessible(true);
// 获取字段值
Object value = field.get(targetObjectInstance);
// 设置字段值
field.set(targetObjectInstance, newValue);
```

![](img/65643bdba9fec7a5ae2f51db64b60baa_150.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_151.png)

### 设置访问权限

![](img/65643bdba9fec7a5ae2f51db64b60baa_153.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_154.png)

对于非 public 的成员，需要先设置其可访问性：

```java
methodOrField.setAccessible(true);
```

![](img/65643bdba9fec7a5ae2f51db64b60baa_156.png)

熟练掌握 Java 反射是编写高级 Hook 代码的基本功，它让我们能够灵活地分析和操作未知的类结构。

![](img/65643bdba9fec7a5ae2f51db64b60baa_158.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_160.png)

## 注意事项与局限性

![](img/65643bdba9fec7a5ae2f51db64b60baa_162.png)

在使用 Cydia Substrate 时，需要注意以下几点：

![](img/65643bdba9fec7a5ae2f51db64b60baa_164.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_165.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_167.png)

1.  **兼容性**：如前所述，并非所有 Android 设备和系统版本都兼容 Cydia Substrate。
2.  **ClassLoader**：Cydia Substrate 的 `MS.findClass` 方法依赖于正确的 `ClassLoader`。在某些情况下，如果插件与目标应用不在同一个 `ClassLoader` 中，可能无法直接找到类，这时需要结合反射或其他技巧。
3.  **系统稳定性**：错误的 Hook 代码可能导致系统或应用程序崩溃，请在测试设备上进行实验。

![](img/65643bdba9fec7a5ae2f51db64b60baa_169.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_171.png)

尽管有这些局限性，Cydia Substrate 因其 API 简洁明了，依然是初学者理解 Android Hook 原理的优秀工具。

![](img/65643bdba9fec7a5ae2f51db64b60baa_173.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_174.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_175.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_176.png)

## 总结

![](img/65643bdba9fec7a5ae2f51db64b60baa_177.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_178.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_180.png)

本节课中我们一起学习了如何利用 Cydia Substrate 框架快速搭建 Android Hook 代码。

![](img/65643bdba9fec7a5ae2f51db64b60baa_182.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_183.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_184.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_186.png)

我们首先了解了函数钩子的概念及其强大之处。然后，我们一步步完成了插件的创建、SDK 导入、清单文件配置以及核心 Hook 代码的编写，并成功实现了一个修改系统颜色方法的示例。最后，我们探讨了 Java 反射技术在动态挂钩中的关键作用，这是应对复杂逆向场景的必备技能。

![](img/65643bdba9fec7a5ae2f51db64b60baa_188.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_190.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_191.png)

![](img/65643bdba9fec7a5ae2f51db64b60baa_193.png)

通过本课的学习，你应该对 Android 平台上的运行时函数挂钩有了一个初步且直观的认识。请务必亲自动手实践示例代码，并尝试使用反射机制去挂钩更多不同的方法，以巩固所学知识。扎实的 Java 编程和反射基础，是后续深入学习 Android 逆向的基石。