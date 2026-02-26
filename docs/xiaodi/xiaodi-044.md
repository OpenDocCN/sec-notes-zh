#  044：SQL盲注与增删改查注入方式详解

![](img/19d87e8a5f5e717373ed04be707b3b3c_1.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_3.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_5.png)

在本节课中，我们将学习SQL注入中的盲注技术，以及如何针对数据库的增、删、改、查四种操作方式进行注入。我们将从基本概念入手，通过案例演示，逐步理解盲注的原理、分类及其应用场景。

---

## 📚 概述：SQL操作与盲注的关联

![](img/19d87e8a5f5e717373ed04be707b3b3c_7.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_9.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_11.png)

SQL操作主要分为四种：**增加（INSERT）**、**修改（UPDATE）**、**删除（DELETE）** 和 **查询（SELECT）**。这四种操作对应着网站的不同功能，例如用户注册、新闻发布、信息修改和数据展示等。

![](img/19d87e8a5f5e717373ed04be707b3b3c_13.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_15.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_17.png)

在注入攻击中，查询操作通常会将数据回显到页面上，便于攻击者直接获取信息。而增加、修改和删除操作往往只返回操作成功或失败的结果，不会直接显示数据。这种差异导致了**盲注（Blind SQL Injection）** 技术的产生。

![](img/19d87e8a5f5e717373ed04be707b3b3c_19.png)

---

![](img/19d87e8a5f5e717373ed04be707b3b3c_21.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_23.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_25.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_26.png)

## 🔍 盲注的基本概念

![](img/19d87e8a5f5e717373ed04be707b3b3c_28.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_30.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_32.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_34.png)

盲注是指在注入过程中，某些数据无法直接回显，需要通过其他方法进行判断，从而逐步获取信息的注入方式。盲注主要分为三类：

1.  **基于布尔的盲注（Boolean-based Blind SQL Injection）**
2.  **基于时间的盲注（Time-based Blind SQL Injection）**
3.  **基于报错的盲注（Error-based Blind SQL Injection）**

上一节我们介绍了SQL的四种基本操作，本节中我们来看看盲注是如何在这些操作中应用的。

![](img/19d87e8a5f5e717373ed04be707b3b3c_36.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_38.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_40.png)

---

![](img/19d87e8a5f5e717373ed04be707b3b3c_42.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_44.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_46.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_48.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_50.png)

## 🧩 基于布尔的盲注

基于布尔的盲注依赖于页面返回的真假状态来判断注入条件是否成立。例如，通过判断数据库名的长度或特定字符，逐步猜解数据。

![](img/19d87e8a5f5e717373ed04be707b3b3c_52.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_54.png)

以下是判断数据库名长度的示例：

![](img/19d87e8a5f5e717373ed04be707b3b3c_56.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_58.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_60.png)

```sql
and length(database())=7
```

![](img/19d87e8a5f5e717373ed04be707b3b3c_62.png)

如果条件成立，页面正常显示；如果条件不成立，页面可能显示异常或为空。

**使用条件**：需要页面有数据回显，以便通过内容变化判断真假。

---

![](img/19d87e8a5f5e717373ed04be707b3b3c_64.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_66.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_68.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_70.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_72.png)

## ⏳ 基于时间的盲注

![](img/19d87e8a5f5e717373ed04be707b3b3c_74.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_76.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_78.png)

基于时间的盲注通过注入延时语句，根据页面响应时间来判断条件是否成立。常用的延时函数是 `sleep()`。

![](img/19d87e8a5f5e717373ed04be707b3b3c_80.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_82.png)

以下是结合 `if` 语句的延时注入示例：

![](img/19d87e8a5f5e717373ed04be707b3b3c_84.png)

```sql
and if(1=1,sleep(5),0)
```

![](img/19d87e8a5f5e717373ed04be707b3b3c_86.png)

如果条件成立，页面将延迟5秒响应；否则立即响应。

![](img/19d87e8a5f5e717373ed04be707b3b3c_88.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_90.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_92.png)

**使用条件**：无需页面回显，只需观察响应时间即可。

![](img/19d87e8a5f5e717373ed04be707b3b3c_94.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_96.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_98.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_100.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_102.png)

---

![](img/19d87e8a5f5e717373ed04be707b3b3c_104.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_106.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_108.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_110.png)

## ⚠️ 基于报错的盲注

![](img/19d87e8a5f5e717373ed04be707b3b3c_112.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_114.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_116.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_118.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_120.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_122.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_123.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_125.png)

基于报错的盲注通过触发数据库报错信息，将查询结果直接显示在错误信息中。常用的报错函数包括 `updatexml()` 和 `extractvalue()`。

![](img/19d87e8a5f5e717373ed04be707b3b3c_127.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_129.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_131.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_133.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_135.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_137.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_139.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_141.png)

以下是使用 `updatexml()` 报错注入的示例：

![](img/19d87e8a5f5e717373ed04be707b3b3c_143.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_145.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_147.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_149.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_151.png)

```sql
and updatexml(1,concat(0x7e,(select database()),0x7e),1)
```

![](img/19d87e8a5f5e717373ed04be707b3b3c_153.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_155.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_157.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_159.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_161.png)

执行后，错误信息中会包含数据库名。

![](img/19d87e8a5f5e717373ed04be707b3b3c_163.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_165.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_167.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_169.png)

**使用条件**：需要目标网站开启了数据库报错信息回显。

![](img/19d87e8a5f5e717373ed04be707b3b3c_171.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_173.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_175.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_177.png)

---

![](img/19d87e8a5f5e717373ed04be707b3b3c_179.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_181.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_183.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_185.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_187.png)

## 🛠️ 增删改查操作的注入方式

![](img/19d87e8a5f5e717373ed04be707b3b3c_189.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_191.png)

不同的SQL操作方式对应不同的注入场景。以下是针对四种操作的注入思路：

![](img/19d87e8a5f5e717373ed04be707b3b3c_193.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_195.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_197.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_199.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_201.png)

### 1. 查询（SELECT）注入
查询操作通常伴随数据回显，因此可以直接使用联合查询（UNION）或基于布尔的盲注。

### 2. 增加（INSERT）注入
增加操作通常用于用户注册、新闻发布等功能。由于不直接回显数据，适合使用基于时间或报错的盲注。

![](img/19d87e8a5f5e717373ed04be707b3b3c_203.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_205.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_207.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_209.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_211.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_213.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_215.png)

### 3. 修改（UPDATE）注入
修改操作常用于用户信息更新。同样不直接回显数据，适合使用基于时间或报错的盲注。

![](img/19d87e8a5f5e717373ed04be707b3b3c_217.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_219.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_221.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_223.png)

### 4. 删除（DELETE）注入
删除操作用于删除用户或文章。由于只返回操作结果，适合使用基于时间或报错的盲注。

![](img/19d87e8a5f5e717373ed04be707b3b3c_225.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_227.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_229.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_231.png)

---

![](img/19d87e8a5f5e717373ed04be707b3b3c_233.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_235.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_237.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_239.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_241.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_243.png)

## 🧪 实战案例演示

![](img/19d87e8a5f5e717373ed04be707b3b3c_245.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_247.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_249.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_251.png)

### 案例一：基于报错的盲注（INSERT操作）
在某个CMS的留言功能中，存在INSERT注入漏洞。通过提交恶意留言，触发报错信息，获取数据库版本。

**注入语句**：
```sql
' and updatexml(1,concat(0x7e,(select version()),0x7e),1) and '
```

![](img/19d87e8a5f5e717373ed04be707b3b3c_253.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_255.png)

### 案例二：基于时间的盲注（DELETE操作）
在某个CMS的后台删除功能中，存在DELETE注入漏洞。通过注入延时语句，判断数据库名的长度。

![](img/19d87e8a5f5e717373ed04be707b3b3c_257.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_259.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_261.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_263.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_265.png)

**注入语句**：
```sql
or if(length(database())=6,sleep(3),0)
```

![](img/19d87e8a5f5e717373ed04be707b3b3c_267.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_269.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_271.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_272.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_273.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_275.png)

---

![](img/19d87e8a5f5e717373ed04be707b3b3c_277.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_279.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_281.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_283.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_285.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_287.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_289.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_291.png)

## 📝 总结

![](img/19d87e8a5f5e717373ed04be707b3b3c_293.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_295.png)

本节课中我们一起学习了SQL盲注的基本概念、分类及其在增删改查操作中的应用。通过案例演示，我们了解到：

1.  **基于布尔的盲注** 需要页面回显，通过真假状态判断条件。
2.  **基于时间的盲注** 无需页面回显，通过响应时间判断条件。
3.  **基于报错的盲注** 需要开启报错信息回显，直接获取数据。
4.  **增删改查操作** 中，查询操作适合直接注入，而增加、修改和删除操作更适合盲注。

![](img/19d87e8a5f5e717373ed04be707b3b3c_297.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_299.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_301.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_303.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_305.png)

掌握这些知识后，我们可以在实际渗透测试中更灵活地应对不同类型的注入场景。下节课我们将进一步学习二次注入、宽字节注入等高级技术。

![](img/19d87e8a5f5e717373ed04be707b3b3c_307.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_308.png)

![](img/19d87e8a5f5e717373ed04be707b3b3c_310.png)

---