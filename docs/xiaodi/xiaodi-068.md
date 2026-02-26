#  068：Java安全之JWT攻防、Swagger自动化与Druid泄漏 🔐

![](img/dab4fc6318f358f64a12ef655193e26c_1.png)

![](img/dab4fc6318f358f64a12ef655193e26c_3.png)

![](img/dab4fc6318f358f64a12ef655193e26c_5.png)

![](img/dab4fc6318f358f64a12ef655193e26c_7.png)

![](img/dab4fc6318f358f64a12ef655193e26c_9.png)

![](img/dab4fc6318f358f64a12ef655193e26c_11.png)

![](img/dab4fc6318f358f64a12ef655193e26c_13.png)

![](img/dab4fc6318f358f64a12ef655193e26c_15.png)

![](img/dab4fc6318f358f64a12ef655193e26c_17.png)

![](img/dab4fc6318f358f64a12ef655193e26c_19.png)

![](img/dab4fc6318f358f64a12ef655193e26c_21.png)

在本节课中，我们将学习三个关键的Java安全知识点：Druid监控泄漏、Swagger接口自动化测试以及JWT（JSON Web Token）的攻击与防御。我们将从原理、识别方法到实战利用进行详细讲解，内容力求简单直白，让初学者能够看懂。

![](img/dab4fc6318f358f64a12ef655193e26c_23.png)

![](img/dab4fc6318f358f64a12ef655193e26c_25.png)

![](img/dab4fc6318f358f64a12ef655193e26c_27.png)

![](img/dab4fc6318f358f64a12ef655193e26c_29.png)

![](img/dab4fc6318f358f64a12ef655193e26c_31.png)

![](img/dab4fc6318f358f64a12ef655193e26c_33.png)

![](img/dab4fc6318f358f64a12ef655193e26c_35.png)

## 1. Druid监控泄漏与未授权访问 🔍

![](img/dab4fc6318f358f64a12ef655193e26c_37.png)

![](img/dab4fc6318f358f64a12ef655193e26c_39.png)

![](img/dab4fc6318f358f64a12ef655193e26c_41.png)

![](img/dab4fc6318f358f64a12ef655193e26c_43.png)

上一节我们介绍了Spring Boot Actuator的监控端点，本节中我们来看看另一个Java中常用的数据库连接池监控组件——Druid。Druid是阿里巴巴开源的一款数据库连接池，用于监控数据库性能。如果配置不当，其内置的监控页面可能造成未授权访问，导致敏感信息泄露。

![](img/dab4fc6318f358f64a12ef655193e26c_45.png)

![](img/dab4fc6318f358f64a12ef655193e26c_47.png)

![](img/dab4fc6318f358f64a12ef655193e26c_49.png)

![](img/dab4fc6318f358f64a12ef655193e26c_51.png)

![](img/dab4fc6318f358f64a12ef655193e26c_53.png)

### 1.1 Druid是什么？
Druid是Java中专门用来监控数据库连接池的组件。它提供了一个Web界面，用于展示SQL监控、Web应用监控、URI监控、Session监控等信息。

![](img/dab4fc6318f358f64a12ef655193e26c_55.png)

![](img/dab4fc6318f358f64a12ef655193e26c_57.png)

![](img/dab4fc6318f358f64a12ef655193e26c_59.png)

![](img/dab4fc6318f358f64a12ef655193e26c_61.png)

![](img/dab4fc6318f358f64a12ef655193e26c_63.png)

![](img/dab4fc6318f358f64a12ef655193e26c_65.png)

### 1.2 安全问题成因
Druid监控页面的安全问题主要源于配置不当。如果在`application.properties`或`application.yml`配置文件中没有设置访问该页面的用户名和密码，或者安全配置存在缺陷，攻击者就可以直接访问监控页面，获取数据库连接信息、执行的SQL语句、Web请求路径等敏感数据。

![](img/dab4fc6318f358f64a12ef655193e26c_67.png)

![](img/dab4fc6318f358f64a12ef655193e26c_69.png)

![](img/dab4fc6318f358f64a12ef655193e26c_71.png)

![](img/dab4fc6318f358f64a12ef655193e26c_73.png)

![](img/dab4fc6318f358f64a12ef655193e26c_75.png)

![](img/dab4fc6318f358f64a12ef655193e26c_77.png)

**配置示例（存在风险）：**
```yaml
# 可能缺少以下安全配置
spring.datasource.druid.stat-view-servlet.enabled=true
spring.datasource.druid.stat-view-servlet.url-pattern=/druid/*
# 未设置loginUsername和loginPassword
```

![](img/dab4fc6318f358f64a12ef655193e26c_79.png)

![](img/dab4fc6318f358f64a12ef655193e26c_81.png)

![](img/dab4fc6318f358f64a12ef655193e26c_83.png)

![](img/dab4fc6318f358f64a12ef655193e26c_85.png)

![](img/dab4fc6318f358f64a12ef655193e26c_87.png)

![](img/dab4fc6318f358f64a12ef655193e26c_89.png)

![](img/dab4fc6318f358f64a12ef655193e26c_91.png)

![](img/dab4fc6318f358f64a12ef655193e26c_93.png)

![](img/dab4fc6318f358f64a12ef655193e26c_95.png)

### 1.3 攻击者视角：黑盒与白盒测试
无论是黑盒测试还是白盒测试，发现和利用Druid泄漏的思路是相通的。

![](img/dab4fc6318f358f64a12ef655193e26c_97.png)

![](img/dab4fc6318f358f64a12ef655193e26c_99.png)

![](img/dab4fc6318f358f64a12ef655193e26c_101.png)

![](img/dab4fc6318f358f64a12ef655193e26c_103.png)

![](img/dab4fc6318f358f64a12ef655193e26c_105.png)

![](img/dab4fc6318f358f64a12ef655193e26c_107.png)

![](img/dab4fc6318f358f64a12ef655193e26c_109.png)

![](img/dab4fc6318f358f64a12ef655193e26c_111.png)

**以下是攻击者常用的步骤：**
1.  **发现端点**：尝试访问常见的Druid监控路径，如 `/druid/index.html`、`/druid/weburi.html`、`/druid/sql.html` 等。
2.  **信息收集**：如果能够未授权访问，则可以直接查看SQL监控、URI监控等信息，获取数据库结构、后台管理路径等。
3.  **权限绕过尝试**：如果页面需要登录，可以尝试：
    *   利用监控页面泄露的Session信息，将其添加到请求Cookie中再次访问。
    *   利用泄露的URI路径，直接访问这些后台功能点，测试是否存在未授权访问。

![](img/dab4fc6318f358f64a12ef655193e26c_113.png)

![](img/dab4fc6318f358f64a12ef655193e26c_115.png)

![](img/dab4fc6318f358f64a12ef655193e26c_117.png)

![](img/dab4fc6318f358f64a12ef655193e26c_119.png)

![](img/dab4fc6318f358f64a12ef655193e26c_121.png)

![](img/dab4fc6318f358f64a12ef655193e26c_123.png)

![](img/dab4fc6318f358f64a12ef655193e26c_125.png)

![](img/dab4fc6318f358f64a12ef655193e26c_127.png)

### 1.4 防御措施
*   **白盒（开发阶段）**：在源码中搜索`druid`相关配置，确保设置了强密码。
    ```properties
    spring.datasource.druid.stat-view-servlet.login-username=admin
    spring.datasource.druid.stat-view-servlet.login-password=StrongPassword123!
    ```
*   **黑盒（运维阶段）**：定期扫描应用是否存在 `/druid/*` 路径的未授权访问，并及时修复。

![](img/dab4fc6318f358f64a12ef655193e26c_129.png)

![](img/dab4fc6318f358f64a12ef655193e26c_131.png)

![](img/dab4fc6318f358f64a12ef655193e26c_133.png)

![](img/dab4fc6318f358f64a12ef655193e26c_135.png)

**总结**：Druid泄漏的本质是**未授权访问导致的信息泄露**。其危害在于暴露了数据库操作、应用接口等内部信息，为后续攻击提供了便利。

![](img/dab4fc6318f358f64a12ef655193e26c_137.png)

![](img/dab4fc6318f358f64a12ef655193e26c_139.png)

![](img/dab4fc6318f358f64a12ef655193e26c_141.png)

![](img/dab4fc6318f358f64a12ef655193e26c_143.png)

![](img/dab4fc6318f358f64a12ef655193e26c_145.png)

## 2. Swagger接口自动化测试与漏洞发现 ⚙️

![](img/dab4fc6318f358f64a12ef655193e26c_147.png)

![](img/dab4fc6318f358f64a12ef655193e26c_149.png)

![](img/dab4fc6318f358f64a12ef655193e26c_151.png)

![](img/dab4fc6318f358f64a12ef655193e26c_153.png)

![](img/dab4fc6318f358f64a12ef655193e26c_155.png)

![](img/dab4fc6318f358f64a12ef655193e26c_157.png)

![](img/dab4fc6318f358f64a12ef655193e26c_159.png)

![](img/dab4fc6318f358f64a12ef655193e26c_161.png)

![](img/dab4fc6318f358f64a12ef655193e26c_163.png)

上一节我们了解了监控组件的风险，本节中我们来看看如何利用开发工具进行攻击。Swagger（现称OpenAPI）是一个用于设计、构建、文档化和使用RESTful Web服务的工具。在Java项目中，它常被集成用于在线测试API接口。

![](img/dab4fc6318f358f64a12ef655193e26c_165.png)

![](img/dab4fc6318f358f64a12ef655193e26c_167.png)

![](img/dab4fc6318f358f64a12ef655193e26c_168.png)

![](img/dab4fc6318f358f64a12ef655193e26c_170.png)

![](img/dab4fc6318f358f64a12ef655193e26c_172.png)

### 2.1 Swagger是什么？
Swagger是一个API接口服务文档工具，归类于RESTful接口。项目上线后，开发或管理员会使用Swagger界面在线测试接口的收发数据是否正常。它会展示项目中所有可调试的API地址和参数。

![](img/dab4fc6318f358f64a12ef655193e26c_174.png)

![](img/dab4fc6318f358f64a12ef655193e26c_176.png)

![](img/dab4fc6318f358f64a12ef655193e26c_178.png)

![](img/dab4fc6318f358f64a12ef655193e26c_180.png)

![](img/dab4fc6318f358f64a12ef655193e26c_182.png)

![](img/dab4fc6318f358f64a12ef655193e26c_184.png)

![](img/dab4fc6318f358f64a12ef655193e26c_186.png)

![](img/dab4fc6318f358f64a12ef655193e26c_188.png)

![](img/dab4fc6318f358f64a12ef655193e26c_190.png)

### 2.2 安全问题：攻击者的“地图”
Swagger本是为开发者提供便利，但也为攻击者提供了一张清晰的“攻击地图”。攻击者可以直接看到所有API端点，并利用其测试功能，批量、自动化地探测漏洞。

![](img/dab4fc6318f358f64a12ef655193e26c_192.png)

![](img/dab4fc6318f358f64a12ef655193e26c_194.png)

![](img/dab4fc6318f358f64a12ef655193e26c_196.png)

![](img/dab4fc6318f358f64a12ef655193e26c_198.png)

![](img/dab4fc6318f358f64a12ef655193e26c_200.png)

![](img/dab4fc6318f358f64a12ef655193e26c_202.png)

![](img/dab4fc6318f358f64a12ef655193e26c_204.png)

![](img/dab4fc6318f358f64a12ef655193e26c_206.png)

![](img/dab4fc6318f358f64a12ef655193e26c_208.png)

![](img/dab4fc6318f358f64a12ef655193e26c_210.png)

![](img/dab4fc6318f358f64a12ef655193e26c_212.png)

![](img/dab4fc6318f358f64a12ef655193e26c_214.png)

![](img/dab4fc6318f358f64a12ef655193e26c_216.png)

![](img/dab4fc6318f358f64a12ef655193e26c_218.png)

**攻击思路如下：**
1.  **发现Swagger UI**：访问常见的Swagger路径，如 `/swagger-ui.html`、`/v2/api-docs`。
2.  **信息收集**：浏览所有接口，寻找敏感功能点（如用户管理、文件上传、系统配置）。
3.  **漏洞探测**：手动或利用工具，对接口进行漏洞测试，如：
    *   **未授权访问**：直接调用标注为需要权限的接口。
    *   **参数注入**：测试SQL注入、命令注入等。
    *   **文件上传**：测试文件上传漏洞。

![](img/dab4fc6318f358f64a12ef655193e26c_220.png)

![](img/dab4fc6318f358f64a12ef655193e26c_222.png)

![](img/dab4fc6318f358f64a12ef655193e26c_224.png)

![](img/dab4fc6318f358f64a12ef655193e26c_226.png)

### 2.3 自动化联动测试
由于接口可能成百上千，手动测试效率低下。我们可以将Swagger与自动化工具联动。

![](img/dab4fc6318f358f64a12ef655193e26c_228.png)

![](img/dab4fc6318f358f64a12ef655193e26c_230.png)

![](img/dab4fc6318f358f64a12ef655193e26c_232.png)

![](img/dab4fc6318f358f64a12ef655193e26c_234.png)

![](img/dab4fc6318f358f64a12ef655193e26c_236.png)

![](img/dab4fc6318f358f64a12ef655193e26c_238.png)

**以下是利用Postman进行批量测试的步骤：**
1.  **导入接口**：在Postman中，通过 `Import` -> `Link`，输入Swagger的API文档地址（通常是 `/v2/api-docs` 或 `/v3/api-docs`）。
2.  **配置测试**：导入后，所有接口会被整理到集合中。可以选中集合，点击 `Run` 进行批量请求。
3.  **分析结果**：查看返回的状态码（如200成功、401未授权、500服务器错误），快速定位可能存在问题的接口。

**进阶：联动漏洞扫描器**
为了更深入地检测漏洞，可以将Postman的流量转发到专业漏洞扫描器（如Burp Suite、Xray）。
1.  在Postman中设置代理，将请求发送到Burp Suite（如 `127.0.0.1:8080`）。
2.  在Burp Suite中配置上游代理，将流量转发给Xray进行被动扫描。
3.  这样，Postman批量测试的每一个请求，都会经过Xray的漏洞检测引擎。

![](img/dab4fc6318f358f64a12ef655193e26c_240.png)

![](img/dab4fc6318f358f64a12ef655193e26c_242.png)

![](img/dab4fc6318f358f64a12ef655193e26c_244.png)

![](img/dab4fc6318f358f64a12ef655193e26c_245.png)

### 2.4 防御与发现
*   **白盒发现**：在源码中搜索 `swagger`、`@EnableSwagger2` 等注解或依赖。
*   **黑盒发现**：使用目录扫描工具探测 `/swagger-ui.html` 等常见路径。在生产环境中，**务必禁用Swagger UI**，或对其访问施加严格的IP白名单和身份验证。

![](img/dab4fc6318f358f64a12ef655193e26c_247.png)

![](img/dab4fc6318f358f64a12ef655193e26c_249.png)

![](img/dab4fc6318f358f64a12ef655193e26c_251.png)

![](img/dab4fc6318f358f64a12ef655193e26c_253.png)

![](img/dab4fc6318f358f64a12ef655193e26c_255.png)

![](img/dab4fc6318f358f64a12ef655193e26c_257.png)

![](img/dab4fc6318f358f64a12ef655193e26c_259.png)

**总结**：Swagger暴露了应用的所有API接口，极大地降低了攻击者的探测成本。安全测试中，它常作为**接口未授权、越权、注入等漏洞的发现入口**。

![](img/dab4fc6318f358f64a12ef655193e26c_261.png)

![](img/dab4fc6318f358f64a12ef655193e26c_263.png)

![](img/dab4fc6318f358f64a12ef655193e26c_265.png)

![](img/dab4fc6318f358f64a12ef655193e26c_267.png)

## 3. JWT（JSON Web Token）攻击与防御 🛡️

![](img/dab4fc6318f358f64a12ef655193e26c_269.png)

![](img/dab4fc6318f358f64a12ef655193e26c_271.png)

前面两节我们讨论了信息泄露和接口暴露的风险，本节我们深入一个更核心的身份验证机制——JWT。JWT是一种用于身份验证的开放标准（RFC 7519），它用于在各方之间安全地传输信息作为JSON对象。

![](img/dab4fc6318f358f64a12ef655193e26c_273.png)

![](img/dab4fc6318f358f64a12ef655193e26c_274.png)

![](img/dab4fc6318f358f64a12ef655193e26c_276.png)

![](img/dab4fc6318f358f64a12ef655193e26c_278.png)

![](img/dab4fc6318f358f64a12ef655193e26c_280.png)

![](img/dab4fc6318f358f64a12ef655193e26c_282.png)

![](img/dab4fc6318f358f64a12ef655193e26c_284.png)

![](img/dab4fc6318f358f64a12ef655193e26c_286.png)

![](img/dab4fc6318f358f64a12ef655193e26c_288.png)

![](img/dab4fc6318f358f64a12ef655193e26c_290.png)

### 3.1 JWT是什么？与Session的对比
JWT旨在取代传统的Cookie+Session身份验证方式。
*   **Session**：用户登录后，服务器创建一个Session ID存储在服务器端，并返回给浏览器一个Cookie。浏览器后续请求携带此Cookie，服务器通过比对Session文件来验证用户身份。
*   **JWT**：用户登录后，服务器生成一个JWT令牌返回给客户端。客户端后续请求在Authorization头中携带此令牌。**服务器无需存储会话状态**，只需验证令牌的签名是否有效、内容是否被篡改即可。

![](img/dab4fc6318f358f64a12ef655193e26c_292.png)

![](img/dab4fc6318f358f64a12ef655193e26c_293.png)

![](img/dab4fc6318f358f64a12ef655193e26c_295.png)

![](img/dab4fc6318f358f64a12ef655193e26c_297.png)

![](img/dab4fc6318f358f64a12ef655193e26c_299.png)

![](img/dab4fc6318f358f64a12ef655193e26c_301.png)

![](img/dab4fc6318f358f64a12ef655193e26c_303.png)

**JWT令牌格式**：`Header.Payload.Signature`
*   **Header（头部）**：包含令牌类型（`typ`）和签名算法（`alg`），如HMAC SHA256或RSA。经过Base64Url编码。
    ```json
    {
      "alg": "HS256",
      "typ": "JWT"
    }
    ```
*   **Payload（负载）**：包含声明（Claims），即需要传递的信息，如用户ID（`sub`）、用户名（`name`）、过期时间（`exp`）等。经过Base64Url编码。
    ```json
    {
      "sub": "1234567890",
      "name": "John Doe",
      "exp": 1516239022
    }
    ```
*   **Signature（签名）**：对编码后的Header和Payload，使用Header中指定的算法和一个密钥（Secret）进行签名，用于验证消息在传递过程中未被篡改。
    ```
    HMACSHA256(
      base64UrlEncode(header) + "." + base64UrlEncode(payload),
      secret)
    ```

![](img/dab4fc6318f358f64a12ef655193e26c_305.png)

![](img/dab4fc6318f358f64a12ef655193e26c_307.png)

![](img/dab4fc6318f358f64a12ef655193e26c_309.png)

![](img/dab4fc6318f358f64a12ef655193e26c_311.png)

![](img/dab4fc6318f358f64a12ef655193e26c_313.png)

![](img/dab4fc6318f358f64a12ef655193e26c_315.png)

**最终形态示例**：
`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c`

![](img/dab4fc6318f358f64a12ef655193e26c_316.png)

![](img/dab4fc6318f358f64a12ef655193e26c_318.png)

![](img/dab4fc6318f358f64a12ef655193e26c_320.png)

![](img/dab4fc6318f358f64a12ef655193e26c_322.png)

![](img/dab4fc6318f358f64a12ef655193e26c_324.png)

![](img/dab4fc6318f358f64a12ef655193e26c_326.png)

![](img/dab4fc6318f358f64a12ef655193e26c_328.png)

![](img/dab4fc6318f358f64a12ef655193e26c_330.png)

![](img/dab4fc6318f358f64a12ef655193e26c_332.png)

### 3.2 如何识别JWT？
1.  **人工识别**：在HTTP请求的 `Authorization` 头或Cookie中，寻找以 `eyJ` 开头，并由两个点（`.`）分隔成三部分的字符串。
2.  **工具识别**：使用Burp Suite插件（如 `JSON Web Tokens`、`AuthMatrix`）或扫描器，它们会自动高亮显示JWT令牌。

![](img/dab4fc6318f358f64a12ef655193e26c_334.png)

![](img/dab4fc6318f358f64a12ef655193e26c_336.png)

![](img/dab4fc6318f358f64a12ef655193e26c_338.png)

![](img/dab4fc6318f358f64a12ef655193e26c_340.png)

![](img/dab4fc6318f358f64a12ef655193e26c_342.png)

![](img/dab4fc6318f358f64a12ef655193e26c_344.png)

![](img/dab4fc6318f358f64a12ef655193e26c_346.png)

![](img/dab4fc6318f358f64a12ef655193e26c_348.png)

### 3.3 JWT的常见攻击手法
JWT的安全依赖于签名的完整性。攻击的核心目标是**伪造一个能被服务器接受的合法令牌**，从而实现身份伪造或越权。

![](img/dab4fc6318f358f64a12ef655193e26c_350.png)

![](img/dab4fc6318f358f64a12ef655193e26c_352.png)

![](img/dab4fc6318f358f64a12ef655193e26c_354.png)

**以下是四种主要攻击手法：**

![](img/dab4fc6318f358f64a12ef655193e26c_356.png)

![](img/dab4fc6318f358f64a12ef655193e26c_358.png)

![](img/dab4fc6318f358f64a12ef655193e26c_360.png)

![](img/dab4fc6318f358f64a12ef655193e26c_362.png)

![](img/dab4fc6318f358f64a12ef655193e26c_364.png)

![](img/dab4fc6318f358f64a12ef655193e26c_366.png)

![](img/dab4fc6318f358f64a12ef655193e26c_368.png)

![](img/dab4fc6318f358f64a12ef655193e26c_370.png)

![](img/dab4fc6318f358f64a12ef655193e26c_372.png)

#### 攻击1：空加密算法（`alg: none`）
*   **原理**：JWT规范支持将算法（`alg`）设置为 `none`，表示不进行签名验证。如果服务器配置不当，接受了 `alg` 为 `none` 的令牌，那么攻击者可以任意修改Payload部分，服务器都会信任。
*   **利用**：将Header中的 `alg` 改为 `none`，删除Signature部分（或保留空签名），然后修改Payload中的用户身份（如将 `user` 改为 `admin`）。
*   **工具**：使用 `jwt_tool` 或Burp插件 `JSON Web Tokens` 可以方便地构造这种令牌。
    ```bash
    python3 jwt_tool.py <原JWT> -X a
    ```

![](img/dab4fc6318f358f64a12ef655193e26c_374.png)

![](img/dab4fc6318f358f64a12ef655193e26c_376.png)

![](img/dab4fc6318f358f64a12ef655193e26c_377.png)

![](img/dab4fc6318f358f64a12ef655193e26c_379.png)

![](img/dab4fc6318f358f64a12ef655193e26c_381.png)

![](img/dab4fc6318f358f64a12ef655193e26c_383.png)

![](img/dab4fc6318f358f64a12ef655193e26c_385.png)

![](img/dab4fc6318f358f64a12ef655193e26c_387.png)

![](img/dab4fc6318f358f64a12ef655193e26c_388.png)

![](img/dab4fc6318f358f64a12ef655193e26c_390.png)

![](img/dab4fc6318f358f64a12ef655193e26c_392.png)

![](img/dab4fc6318f358f64a12ef655193e26c_393.png)

#### 攻击2：弱密钥（Weak Secret）爆破
*   **原理**：对于使用HMAC（如HS256）等对称加密算法的JWT，签名和验证使用同一个密钥（Secret）。如果密钥强度弱（如`123456`、`secret`），攻击者可以暴力破解。
*   **利用**：使用字典对密钥进行爆破，一旦破解成功，就可以用该密钥签发任意Payload的合法令牌。
*   **工具**：使用 `jwt_tool` 或 `hashcat`。
    ```bash
    python3 jwt_tool.py <原JWT> -C -d /path/to/wordlist.txt
    ```

![](img/dab4fc6318f358f64a12ef655193e26c_395.png)

![](img/dab4fc6318f358f64a12ef655193e26c_397.png)

![](img/dab4fc6318f358f64a12ef655193e26c_399.png)

![](img/dab4fc6318f358f64a12ef655193e26c_401.png)

![](img/dab4fc6318f358f64a12ef655193e26c_403.png)

![](img/dab4fc6318f358f64a12ef655193e26c_405.png)

![](img/dab4fc6318f358f64a12ef655193e26c_407.png)

![](img/dab4fc6318f358f64a12ef655193e26c_409.png)

![](img/dab4fc6318f358f64a12ef655193e26c_411.png)

![](img/dab4fc6318f358f64a12ef655193e26c_413.png)

#### 攻击3：算法混淆（Algorithm Confusion）
*   **原理**：服务器可能使用非对称算法（如RS256）验证签名（用公钥），但代码逻辑存在缺陷，未能严格校验Header中的 `alg` 值。攻击者可以将 `alg` 改为对称算法（如HS256），然后使用泄露的或猜测的**公钥**作为HMAC的密钥，来伪造签名。
*   **前提**：需要获取到服务器的公钥（可能通过源码泄露、`/.well-known/jwks.json` 端点等）。
*   **利用**：用公钥作为HS256的密钥，重新签名一个修改过的Payload。
    ```python
    # 示例思路
    import jwt
    forged_token = jwt.encode({"user": "admin"}, public_key, algorithm="HS256")
    ```

![](img/dab4fc6318f358f64a12ef655193e26c_415.png)

![](img/dab4fc6318f358f64a12ef655193e26c_417.png)

![](img/dab4fc6318f358f64a12ef655193e26c_419.png)

![](img/dab4fc6318f358f64a12ef655193e26c_421.png)

![](img/dab4fc6318f358f64a12ef655193e26c_423.png)

![](img/dab4fc6318f358f64a12ef655193e26c_425.png)

![](img/dab4fc6318f358f64a12ef655193e26c_427.png)

![](img/dab4fc6318f358f64a12ef655193e26c_429.png)

![](img/dab4fc6318f358f64a12ef655193e26c_430.png)

#### 攻击4：KID（Key ID）路径注入
*   **原理**：JWT Header中可能包含 `kid`（密钥ID）参数，用于指示服务器使用哪个密钥来验证。如果服务器未对 `kid` 进行过滤，攻击者可以尝试路径遍历、SQL注入等，指向一个攻击者可控的文件或数据库条目，从而控制验证密钥。
*   **示例**：设置 `"kid": "../../../dev/null"`，可能使服务器使用空密钥进行验证，从而绕过签名。

![](img/dab4fc6318f358f64a12ef655193e26c_432.png)

![](img/dab4fc6318f358f64a12ef655193e26c_434.png)

![](img/dab4fc6318f358f64a12ef655193e26c_436.png)

![](img/dab4fc6318f358f64a12ef655193e26c_438.png)

![](img/dab4fc6318f358f64a12ef655193e26c_440.png)

![](img/dab4fc6318f358f64a12ef655193e26c_442.png)

![](img/dab4fc6318f358f64a12ef655193e26c_444.png)

![](img/dab4fc6318f358f64a12ef655193e26c_446.png)

![](img/dab4fc6318f358f64a12ef655193e26c_448.png)

![](img/dab4fc6318f358f64a12ef655193e26c_450.png)

### 3.4 JWT攻击实战流程（黑盒）
1.  **获取令牌**：通过正常登录或信息泄露，获取一个有效的JWT。
2.  **解码分析**：使用 [jwt.io](https://jwt.io) 或插件解码，查看 `alg`、`kid` 及Payload内容。
3.  **顺序测试**：
    a. 测试**空算法绕过**（`alg:none`）。
    b. 测试**删除签名**（直接去掉第三部分），看服务器是否只验证前两部分。
    c. 进行**弱密钥爆破**（针对HS256等）。
    d. 检查是否有 `kid` 等参数，尝试**注入攻击**。
    e. 如果发现是RS256等非对称算法，寻找**公钥泄露**的可能性，尝试**算法混淆**。
4.  **重组发送**：利用成功的方法，伪造一个以目标用户（如`admin`）身份签名的JWT，发送给应用，测试越权。

![](img/dab4fc6318f358f64a12ef655193e26c_452.png)

![](img/dab4fc6318f358f64a12ef655193e26c_454.png)

![](img/dab4fc6318f358f64a12ef655193e26c_456.png)

![](img/dab4fc6318f358f64a12ef655193e26c_458.png)

### 3.5 防御措施
*   **使用强算法**：优先使用非对称算法（如RS256、ES256）。
*   **验证 `alg` 字段**：在代码中严格校验接收到的JWT的 `alg` 头是否与预期一致。
*   **使用强密钥**：确保对称加密的密钥足够复杂，无法被爆破。
*   **设置合理的有效期**：Payload中务必使用 `exp`（过期时间）和 `nbf`（生效时间）声明。
*   **避免敏感信息**：不要在Payload中存放密码等敏感信息，因为它是Base64编码，并非加密。
*   **验证KID**：如果使用 `kid`，必须对其进行严格的白名单校验，防止路径遍历或注入。

![](img/dab4fc6318f358f64a12ef655193e26c_460.png)

![](img/dab4fc6318f358f64a12ef655193e26c_462.png)

![](img/dab4fc6318f358f64a12ef655193e26c_464.png)

![](img/dab4fc6318f358f64a12ef655193e26c_466.png)

![](img/dab4fc6318f358f64a12ef655193e26c_468.png)

![](img/dab4fc6318f358f64a12ef655193e26c_470.png)

![](img/dab4fc6318f358f64a12ef655193e26c_472.png)

![](img/dab4fc6318f358f64a12ef655193e26c_474.png)

![](img/dab4fc6318f358f64a12ef655193e26c_476.png)

![](img/dab4fc6318f358f64a12ef655193e26c_478.png)

**总结**：JWT攻击的核心是**签名绕过和密钥破解**，最终目的是**伪造用户身份**。其危害直接导致**身份验证失效**，造成越权访问。在审计和测试中，JWT是需要重点关注的认证机制。

![](img/dab4fc6318f358f64a12ef655193e26c_480.png)

![](img/dab4fc6318f358f64a12ef655193e26c_482.png)

![](img/dab4fc6318f358f64a12ef655193e26c_484.png)

![](img/dab4fc6318f358f64a12ef655193e26c_486.png)

---
**本节课中我们一起学习了**：
1.  **Druid监控泄漏**：因配置不当导致的未授权访问和信息泄露问题。
2.  **Swagger接口暴露**：开发接口文档工具暴露全部API，成为攻击者的突破口，并学习了如何与Postman、Burp Suite、Xray联动进行自动化测试。
3.  **JWT攻防**：深入理解了JWT的结构和原理，掌握了空算法绕过、弱密钥爆破、算法混淆、KID注入等四种主要攻击手法及其防御方法。

![](img/dab4fc6318f358f64a12ef655193e26c_488.png)

![](img/dab4fc6318f358f64a12ef655193e26c_490.png)

![](img/dab4fc6318f358f64a12ef655193e26c_492.png)

![](img/dab4fc6318f358f64a12ef655193e26c_494.png)

![](img/dab4fc6318f358f64a12ef655193e26c_496.png)

这些知识点涵盖了从信息收集、接口测试到核心身份验证机制攻防的完整链条，是Java应用安全测试中不可或缺的部分。