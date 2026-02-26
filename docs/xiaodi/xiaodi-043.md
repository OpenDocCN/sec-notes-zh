#  043：PHP应用与SQL注入进阶（2） - 数据请求类型、方法、头部与编码

![](img/bc030ddf02bf33193a2eb1a349a1ac30_1.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_3.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_5.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_7.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_9.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_11.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_13.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_15.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_17.png)

在本节课中，我们将深入学习SQL注入中容易被忽略的细节，包括数据请求类型、不同的HTTP请求方法、HTTP头部信息以及数据编码格式如何影响注入的成功与否。理解这些内容将帮助你更全面地识别和利用SQL注入漏洞。

![](img/bc030ddf02bf33193a2eb1a349a1ac30_19.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_21.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_23.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_25.png)

---

![](img/bc030ddf02bf33193a2eb1a349a1ac30_27.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_29.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_31.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_33.png)

## 📊 数据请求类型与SQL语句拼接

![](img/bc030ddf02bf33193a2eb1a349a1ac30_35.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_37.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_39.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_41.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_43.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_45.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_47.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_49.png)

上一节我们介绍了PHP中SQL注入的基本原理。本节中我们来看看，为什么有时明明存在注入点，但工具或手工测试却无法成功注入。一个核心原因在于后端代码中SQL语句的拼接方式。

![](img/bc030ddf02bf33193a2eb1a349a1ac30_51.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_53.png)

由于开发者编写SQL语句的习惯和框架的差异，接收到的参数在拼接到SQL语句中时，可能会被单引号、括号、百分号等符号包裹。这导致我们注入的Payload必须考虑如何“闭合”原有的语句结构，才能让注入的SQL代码生效。

![](img/bc030ddf02bf33193a2eb1a349a1ac30_55.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_57.png)

以下是几种常见的SQL语句拼接方式及其对应的注入思路：

![](img/bc030ddf02bf33193a2eb1a349a1ac30_59.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_61.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_63.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_65.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_67.png)

*   **数字型**：参数直接以数字形式拼接，如 `id=$id`。注入时无需考虑引号闭合。
*   **字符型**：参数被单引号包裹，如 `id='$id'`。注入时需要用 `'` 闭合前面的引号，并用 `--+` 或 `#` 注释掉后面的内容。
*   **搜索型**：参数用于 `LIKE` 语句，如 `username LIKE '%$name%'`。注入时需先闭合前面的 `%'`，再注入。
*   **括号包裹型**：条件被括号包裹，如 `(id='$id')`。注入时需先闭合括号和引号。
*   **多重包裹型**：条件可能被多层括号或引号包裹，需要根据实际情况尝试闭合。

![](img/bc030ddf02bf33193a2eb1a349a1ac30_69.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_71.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_73.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_75.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_77.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_79.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_81.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_83.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_85.png)

**核心概念**：成功的注入本质上是将我们输入的Payload与原始SQL语句**正确拼接**，形成一条新的、语法正确且能执行我们意图的SQL语句。公式可以概括为：
`原始SQL框架` + `精心构造的Payload` = `可执行的新SQL语句`

![](img/bc030ddf02bf33193a2eb1a349a1ac30_87.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_89.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_91.png)

由于我们无法直接看到后端代码（黑盒测试），因此只能通过尝试常见的闭合方式（如添加 `'`、`)`、`%'` 等）来猜测其SQL结构。

![](img/bc030ddf02bf33193a2eb1a349a1ac30_93.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_95.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_97.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_99.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_101.png)

---

![](img/bc030ddf02bf33193a2eb1a349a1ac30_103.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_105.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_107.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_109.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_111.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_113.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_115.png)

## 🔄 HTTP请求方法对注入的影响

![](img/bc030ddf02bf33193a2eb1a349a1ac30_117.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_119.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_121.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_123.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_125.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_127.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_129.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_131.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_133.png)

除了URL中的GET参数，SQL注入点可能隐藏在HTTP请求的任何部分。不同的请求方法决定了数据提交的位置和方式。

![](img/bc030ddf02bf33193a2eb1a349a1ac30_135.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_136.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_138.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_140.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_142.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_144.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_146.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_148.png)

### GET 请求注入
数据通过URL参数传递，形式为 `?id=1`。这是最常见、最容易被发现的注入点，测试直接在URL后添加Payload即可。

![](img/bc030ddf02bf33193a2eb1a349a1ac30_150.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_152.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_154.png)

### POST 请求注入
数据通过请求体（Body）传递，在URL中不可见。常见于登录、搜索、文件上传等表单提交。
*   **测试方法**：需要使用抓包工具（如Burp Suite）拦截请求，在POST数据部分（如 `username=admin`）修改参数值进行注入测试。
*   **示例**：一个登录框的POST请求，注入点可能在 `username` 或 `password` 字段。

![](img/bc030ddf02bf33193a2eb1a349a1ac30_156.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_158.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_160.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_162.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_164.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_166.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_168.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_170.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_172.png)

### Cookie 注入
应用程序有时会将参数（如用户ID、会话标识）存放在Cookie中，并直接用于数据库查询。
*   **测试方法**：通过抓包工具修改Cookie中的值，尝试注入。
*   **示例**：`Cookie: user_id=1`， 尝试将其改为 `user_id=1' and '1'='1`。

![](img/bc030ddf02bf33193a2eb1a349a1ac30_174.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_176.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_178.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_180.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_182.png)

### HTTP 头部注入
应用程序可能从HTTP请求头部获取信息（如 `User-Agent`、`X-Forwarded-For`、`Referer`），并记录或用于查询。
*   **常见注入点**：
    *   `User-Agent`：记录客户端浏览器信息。
    *   `X-Forwarded-For`：常用于获取客户端真实IP，如果将其存入数据库，可能产生注入。
    *   `Referer`：记录请求来源页面。
*   **测试方法**：使用抓包工具修改对应头部的值进行注入测试。

![](img/bc030ddf02bf33193a2eb1a349a1ac30_184.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_186.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_188.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_190.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_192.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_194.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_196.png)

**示例场景**：一个记录登录日志的功能，代码使用 `$_SERVER[‘HTTP_X_FORWARDED_FOR’]` 获取IP并存入数据库。攻击者可以伪造该头部为SQL注入Payload。

![](img/bc030ddf02bf33193a2eb1a349a1ac30_198.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_200.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_202.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_204.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_206.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_208.png)

```php
// 存在注入风险的代码示例
$user_ip = $_SERVER['HTTP_X_FORWARDED_FOR']; // 攻击者可控制此值
$sql = "INSERT INTO login_log (ip, username) VALUES ('$user_ip', '$username')";
```

![](img/bc030ddf02bf33193a2eb1a349a1ac30_210.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_212.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_213.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_215.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_217.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_219.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_221.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_223.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_224.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_226.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_228.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_230.png)

---

![](img/bc030ddf02bf33193a2eb1a349a1ac30_232.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_234.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_236.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_238.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_240.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_242.png)

## 📝 数据编码与格式的影响

![](img/bc030ddf02bf33193a2eb1a349a1ac30_244.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_245.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_247.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_249.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_251.png)

有时，前端提交的数据会经过编码或使用特定格式（如JSON），如果后端处理不当，也会影响注入测试。

![](img/bc030ddf02bf33193a2eb1a349a1ac30_253.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_255.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_257.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_259.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_261.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_263.png)

### Base64 编码
参数在传输前被Base64编码。
*   **现象**：URL参数看起来像 `?id=MQ==`（即 `1` 的Base64编码）。
*   **测试方法**：需要先将构造好的注入Payload进行Base64编码，再提交。直接提交明文Payload无效。
*   **流程**：`1' and 1=1--+` -> Base64编码 -> `MScgYW5kIDE9MS0tKw==` -> 提交。

![](img/bc030ddf02bf33193a2eb1a349a1ac30_265.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_267.png)

### JSON 格式
数据以JSON格式在请求体中传输。
*   **现象**：请求头 `Content-Type: application/json`， 请求体为 `{"username":"admin","password":"123456"}`。
*   **测试方法**：在抓包工具中，直接修改JSON字符串中对应值的部分进行注入测试。注意保持JSON格式有效（如字符串的引号）。
*   **示例**：将 `{"username":"admin"}` 改为 `{"username":"admin' or '1'='1"}`。

![](img/bc030ddf02bf33193a2eb1a349a1ac30_269.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_271.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_273.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_275.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_277.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_279.png)

---

![](img/bc030ddf02bf33193a2eb1a349a1ac30_281.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_283.png)

## 🎯 课程总结

![](img/bc030ddf02bf33193a2eb1a349a1ac30_285.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_287.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_289.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_291.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_293.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_295.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_297.png)

本节课我们一起深入探讨了SQL注入中几个关键的上下文因素：

![](img/bc030ddf02bf33193a2eb1a349a1ac30_299.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_301.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_303.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_305.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_307.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_309.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_311.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_313.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_315.png)

1.  **数据请求类型与拼接**：理解了SQL语句的多种拼接方式（字符型、数字型、搜索型等）是成功注入的前提，需要根据情况尝试闭合。
2.  **HTTP请求方法**：注入点不仅存在于GET参数中，还可能隐藏在POST数据、Cookie以及HTTP头部（如X-Forwarded-For）中，测试时需要全面检查。
3.  **数据编码与格式**：面对Base64编码或JSON格式的数据，需要先将注入Payload进行相应处理（编码或嵌入JSON结构），才能被后端正确解析并触发漏洞。

![](img/bc030ddf02bf33193a2eb1a349a1ac30_317.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_319.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_320.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_322.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_324.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_326.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_328.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_330.png)

![](img/bc030ddf02bf33193a2eb1a349a1ac30_332.png)

掌握这些知识，你就能更系统地在黑盒或白盒测试中，从各个维度寻找和验证SQL注入漏洞，避免因测试姿势不对而遗漏高危风险点。下节课，我们将探讨更复杂的注入场景：无回显注入（盲注）及其利用技巧。