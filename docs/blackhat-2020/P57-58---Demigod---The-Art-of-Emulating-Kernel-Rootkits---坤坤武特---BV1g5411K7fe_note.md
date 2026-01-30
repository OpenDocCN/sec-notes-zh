![](img/9b1674cd9ddcfc06b1f74228db2e8f2d_1.png)

![](img/9b1674cd9ddcfc06b1f74228db2e8f2d_2.png)

# P57：58 - Demigod - The Art of Emulating Kernel Rootkits - 坤坤武特 - BV1g5411K7fe

## 概述

在本节课中，我们将学习如何使用Demigod框架来模拟内核根kit，从而分析内核和rootkit的关键技术。

## 背景和动机

### 康奈尔罗基特

计算机系统由两个主要区域组成，具有不同的权限级别。第一个是用户空间，第二个是操作系统内核，它具有对整个系统的完全控制权。康奈尔罗基特在系统最低级别运行，可以绕过系统的监控和防御机制。

### 分析康奈尔罗基特的挑战

分析康奈尔罗基特非常困难，因为它们可以具有多种技巧来逃避检测。目前有两种方法来分析康奈尔罗基特：

1. **动态分析**：需要两台机器，一台运行罗基特，另一台运行分析工具。这种方法存在许多问题，例如罗基特可以跨机器移动，并且分析工具通常针对用户空间，而不是内核空间。
2. **静态分析**：将康奈尔罗基特文件放入分析工具中，例如IDA Pro或Binary Ninja。然而，静态分析也存在问题，例如康奈尔罗基特可能被打包，或者分析工具无法正确识别它们。

![](img/9b1674cd9ddcfc06b1f74228db2e8f2d_4.png)

### Demigod的解决方案

Demigod使用模拟器来模拟内核，从而在安全沙盒中分析康奈尔罗基特。这种方法允许我们在用户空间中分析内核代码，从而避免了动态分析和静态分析中的许多问题。

## Demigod的设计和实现

![](img/9b1674cd9ddcfc06b1f74228db2e8f2d_6.png)

![](img/9b1674cd9ddcfc06b1f74228db2e8f2d_8.png)

![](img/9b1674cd9ddcfc06b1f74228db2e8f2d_10.png)

![](img/9b1674cd9ddcfc06b1f74228db2e8f2d_12.png)

### Chilling模拟器

![](img/9b1674cd9ddcfc06b1f74228db2e8f2d_14.png)

Demigod基于Chilling模拟器，它是一个开源模拟器，支持多个操作系统和CPU架构。Chilling模拟器允许我们在不同级别进行代码注入和内存访问。

### Demigod的改进

![](img/9b1674cd9ddcfc06b1f74228db2e8f2d_16.png)

![](img/9b1674cd9ddcfc06b1f74228db2e8f2d_18.png)

Demigod对Chilling模拟器进行了改进，以支持内核代码的模拟。这包括：

1. **模拟内核组件**：模拟内核组件，例如shell和API。
2. **模拟系统API**：通过钩子和模拟来模拟系统API。
3. **两阶段模拟**：首先加载驱动程序，然后等待用户请求来执行代码路径。

## Demigod的演示

![](img/9b1674cd9ddcfc06b1f74228db2e8f2d_20.png)

![](img/9b1674cd9ddcfc06b1f74228db2e8f2d_22.png)

![](img/9b1674cd9ddcfc06b1f74228db2e8f2d_24.png)

![](img/9b1674cd9ddcfc06b1f74228db2e8f2d_26.png)

### Windows内核根kit

![](img/9b1674cd9ddcfc06b1f74228db2e8f2d_28.png)

![](img/9b1674cd9ddcfc06b1f74228db2e8f2d_30.png)

![](img/9b1674cd9ddcfc06b1f74228db2e8f2d_32.png)

![](img/9b1674cd9ddcfc06b1f74228db2e8f2d_34.png)

![](img/9b1674cd9ddcfc06b1f74228db2e8f2d_35.png)

![](img/9b1674cd9ddcfc06b1f74228db2e8f2d_37.png)

![](img/9b1674cd9ddcfc06b1f74228db2e8f2d_39.png)

Demigod可以模拟Windows内核根kit，并允许我们分析其行为。以下是一个示例：

```python
# Demigod代码示例
demigod.load_driver("driver.sys")
demigod.run_driver()
```

### macOS内核根kit

Demigod可以模拟macOS内核根kit，并允许我们分析其行为。以下是一个示例：

```python
# Demigod代码示例
demigod.load_kernel_extension("kernel_extension.kext")
demigod.run_kernel_extension()
```

![](img/9b1674cd9ddcfc06b1f74228db2e8f2d_41.png)

![](img/9b1674cd9ddcfc06b1f74228db2e8f2d_43.png)

![](img/9b1674cd9ddcfc06b1f74228db2e8f2d_45.png)

### Linux内核根kit

![](img/9b1674cd9ddcfc06b1f74228db2e8f2d_47.png)

![](img/9b1674cd9ddcfc06b1f74228db2e8f2d_48.png)

![](img/9b1674cd9ddcfc06b1f74228db2e8f2d_50.png)

Demigod可以模拟Linux内核根kit，并允许我们分析其行为。以下是一个示例：

```python
# Demigod代码示例
demigod.load_kernel_module("kernel_module.ko")
demigod.run_kernel_module()
```

## 总结

在本节课中，我们一起学习了如何使用Demigod框架来模拟内核根kit，从而分析内核和rootkit的关键技术。Demigod是一个强大的工具，可以帮助我们更好地理解内核和rootkit的工作原理。