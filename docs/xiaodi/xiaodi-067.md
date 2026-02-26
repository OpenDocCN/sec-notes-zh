#  067：Java原生反序列化与SpringBoot攻防实战 🛡️

![](img/d530a3c126cf1e4ed71a17381936fe47_0.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_2.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_4.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_6.png)

在本节课中，我们将要学习Java安全中的两个核心主题：Java原生反序列化漏洞的简单利用，以及SpringBoot框架的常见安全问题与攻防。课程内容旨在让初学者能够理解基本概念并掌握实用工具。

![](img/d530a3c126cf1e4ed71a17381936fe47_8.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_10.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_11.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_12.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_14.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_16.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_18.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_20.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_22.png)

## 一、Java原生反序列化漏洞概述

![](img/d530a3c126cf1e4ed71a17381936fe47_24.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_26.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_28.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_30.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_32.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_34.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_36.png)

上一节我们介绍了Java安全的基础知识，本节中我们来看看Java原生反序列化漏洞。序列化是将对象状态转换为可存储或传输格式的过程，反序列化则是其逆过程。Java原生反序列化漏洞通常源于对不可信数据的反序列化操作。

![](img/d530a3c126cf1e4ed71a17381936fe47_38.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_40.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_42.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_44.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_46.png)

### 1.1 漏洞特征与发现

![](img/d530a3c126cf1e4ed71a17381936fe47_48.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_50.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_52.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_54.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_56.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_58.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_60.png)

此类漏洞在流量中通常具有明显特征，有助于黑盒测试中的识别。

![](img/d530a3c126cf1e4ed71a17381936fe47_62.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_64.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_66.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_68.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_70.png)

*   **序列化数据特征**：原始序列化数据通常以 `AC ED 00 05` 开头。
*   **Base64编码特征**：经过Base64编码后，特征通常为 `rO0AB` 或 `RO0AB`。
*   **发现方式**：
    *   **黑盒测试**：通过流量分析捕获上述特征。
    *   **白盒审计**：直接审查代码中是否使用了 `ObjectInputStream.readObject()` 等反序列化接口，以及是否引用了存在漏洞的组件。

![](img/d530a3c126cf1e4ed71a17381936fe47_72.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_73.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_75.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_77.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_79.png)

### 1.2 常见利用链与工具

![](img/d530a3c126cf1e4ed71a17381936fe47_81.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_83.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_85.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_87.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_89.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_91.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_93.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_95.png)

Java原生反序列化漏洞的利用依赖于环境中存在的特定类库（gadget chain）。以下是主流的利用工具和项目。

![](img/d530a3c126cf1e4ed71a17381936fe47_97.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_99.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_101.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_103.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_105.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_107.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_109.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_111.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_113.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_115.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_117.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_119.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_121.png)

以下是三个常用的Java反序列化利用工具：

![](img/d530a3c126cf1e4ed71a17381936fe47_123.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_124.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_125.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_127.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_129.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_131.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_133.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_135.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_137.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_139.png)

1.  **ysoserial**：经典的命令行工具，需要根据目标环境选择对应的gadget。
    ```bash
    java -jar ysoserial.jar CommonsCollections5 "calc.exe" > payload.ser
    ```
2.  **Yakit**：集成了图形化界面的安全工具，包含Java反序列化利用模块，方便生成Payload。
3.  **Java反序列化利用在线平台**：一个Web项目，支持生成PHP、.NET、Python和Java等多种语言的反序列化Payload，对Java支持良好。

![](img/d530a3c126cf1e4ed71a17381936fe47_141.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_143.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_145.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_147.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_149.png)

**关键点**：工具的使用成功与否，取决于目标应用`ClassPath`中是否存在工具所依赖的特定库（如`commons-collections`）及其兼容版本。如果不存在，则无法成功利用。

![](img/d530a3c126cf1e4ed71a17381936fe47_151.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_153.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_155.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_157.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_159.png)

## 二、SpringBoot框架攻防实战 🚀

![](img/d530a3c126cf1e4ed71a17381936fe47_161.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_163.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_165.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_167.png)

上一节我们介绍了Java原生反序列化的利用，本节中我们来看看目前最流行的Java Web开发框架——SpringBoot的安全问题。

![](img/d530a3c126cf1e4ed71a17381936fe47_169.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_171.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_173.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_175.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_177.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_179.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_181.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_183.png)

SpringBoot的Actuator模块是一个监控和管理应用的工具，配置不当会导致严重的信息泄露，进而可能引发远程代码执行（RCE）。

![](img/d530a3c126cf1e4ed71a17381936fe47_185.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_187.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_189.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_191.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_193.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_195.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_197.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_199.png)

### 2.1 信息泄露与利用

![](img/d530a3c126cf1e4ed71a17381936fe47_201.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_203.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_205.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_207.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_209.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_211.png)

Actuator模块暴露的端点（如`/env`, `/heapdump`）可能泄露敏感信息。

![](img/d530a3c126cf1e4ed71a17381936fe47_213.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_215.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_217.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_219.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_221.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_223.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_225.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_227.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_229.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_231.png)

以下是信息泄露的利用流程：

![](img/d530a3c126cf1e4ed71a17381936fe47_233.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_235.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_237.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_238.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_240.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_242.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_244.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_246.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_248.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_250.png)

1.  **探测与发现**：访问常见的Actuator端点，如 `/actuator`， `/env`， `/heapdump`。
2.  **下载Heapdump文件**：如果 `/heapdump` 端点可访问，可以下载内存转储文件。
3.  **分析Heapdump文件**：使用工具从heapdump文件中提取敏感信息（如数据库密码、API密钥）。
    *   **工具推荐**：
        *   **heapdump_tool**：自动化分析工具，能自动提取多种敏感信息。
        *   **Eclipse Memory Analyzer (MAT)**：功能强大的内存分析工具，支持自定义搜索和分析。

![](img/d530a3c126cf1e4ed71a17381936fe47_252.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_254.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_256.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_258.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_260.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_262.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_264.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_266.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_268.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_270.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_272.png)

### 2.2 漏洞检测与利用

![](img/d530a3c126cf1e4ed71a17381936fe47_274.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_276.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_278.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_280.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_282.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_284.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_286.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_288.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_290.png)

除了信息泄露，SpringBoot本身或其组件也可能存在远程代码执行漏洞。

![](img/d530a3c126cf1e4ed71a17381936fe47_292.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_294.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_296.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_298.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_300.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_302.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_304.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_306.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_308.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_310.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_312.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_314.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_316.png)

以下是两个用于SpringBoot漏洞检测与利用的项目：

![](img/d530a3c126cf1e4ed71a17381936fe47_318.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_320.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_322.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_324.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_326.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_328.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_330.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_332.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_334.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_336.png)

1.  **SpringBoot Exploit Tools**：此类工具通常先通过信息泄露端点搜集信息，然后检测并利用已知的RCE漏洞（如`logback JNDI RCE`）。
2.  **专门针对Spring Cloud Function SpEL RCE等漏洞的扫描器**：用于检测SpringBoot特定组件的CVE漏洞。

![](img/d530a3c126cf1e4ed71a17381936fe47_338.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_340.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_342.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_344.png)

**实战技巧**：
*   **黑盒识别**：SpringBoot应用通常有独特的错误页面和 favicon 图标，可通过这些特征进行初步识别。
*   **BurpSuite插件**：使用如 `APIKit` 等Burp插件，可以自动在流量中检测并标记SpringBoot Actuator端点及其他API接口，极大提升信息收集效率。
*   **白盒审计**：在代码中检查是否引入了`spring-boot-starter-actuator`依赖，并检查配置文件（如`application.yml`）中`management.endpoints.web.exposure.include`的配置值，如果设置为`*`或包含`sensitive`端点（如`env`, `heapdump`），则存在风险。

![](img/d530a3c126cf1e4ed71a17381936fe47_346.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_348.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_350.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_352.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_354.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_356.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_358.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_360.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_361.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_363.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_365.png)

## 三、课程总结与回顾

![](img/d530a3c126cf1e4ed71a17381936fe47_367.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_369.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_371.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_373.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_375.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_377.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_379.png)

本节课中我们一起学习了Java安全中两个重要的实战方向。

![](img/d530a3c126cf1e4ed71a17381936fe47_380.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_381.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_383.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_385.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_387.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_389.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_391.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_393.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_395.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_397.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_399.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_400.png)

我们首先探讨了**Java原生反序列化漏洞**，了解了其流量特征、发现方式，并实践了如何使用`ysoserial`、`Yakit`等工具生成和利用Payload，关键点是利用成功依赖于目标环境中的特定类库。

![](img/d530a3c126cf1e4ed71a17381936fe47_402.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_404.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_406.png)

接着，我们深入研究了**SpringBoot框架的攻防**。核心在于其Actuator监控模块，配置不当会导致敏感信息泄露（如`/env`、`/heapdump`）。我们学习了如何探测这些端点、下载并分析heapdump文件以提取数据库密码等关键信息，并介绍了如何利用自动化工具检测和利用由此信息泄露引发的或框架自身的远程代码执行漏洞。

![](img/d530a3c126cf1e4ed71a17381936fe47_408.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_410.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_412.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_414.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_415.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_417.png)

![](img/d530a3c126cf1e4ed71a17381936fe47_419.png)

通过本课的学习，希望大家在遇到Java反序列化流量或SpringBoot应用时，能够有效地进行安全性测试与评估。下节课我们将继续探讨Java生态中其他常见组件的安全问题。