# 第39课：JavaEE应用、SpringBoot框架、Actuator监控泄漏与Swagger自动化 🛡️

![](img/1e9f7407050e870238cd05cf7a895fa6_1.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_3.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_5.png)

在本节课中，我们将学习JavaEE应用开发中两个非常常见且重要的组件：Spring Boot Actuator监控系统和Swagger接口文档工具。我们将了解它们的基本用途、配置方法，并重点探讨因配置不当可能引发的安全问题，例如敏感信息泄露和接口暴露。通过学习，你将理解这些工具在开发中的作用以及如何安全地使用它们。

![](img/1e9f7407050e870238cd05cf7a895fa6_7.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_9.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_11.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_13.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_15.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_17.png)

## Spring Boot Actuator 监控系统 📊

![](img/1e9f7407050e870238cd05cf7a895fa6_19.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_21.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_23.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_25.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_27.png)

上一节我们介绍了本课程的两个核心主题。本节中，我们来看看第一个：Spring Boot Actuator。在Spring Boot（Java EE开发框架）中，监控系统是经常使用的一个组件。

![](img/1e9f7407050e870238cd05cf7a895fa6_29.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_31.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_33.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_35.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_37.png)

它的作用是提供代码的健康检查、审计指标收集、访问路由跟踪，可以监控应用运行的内存、状态等信息。简单来说，它就是用来监控当前项目（即Web应用或任何应用）运行状态的工具。这包括运行占用的内存、调用链、访问过的网址及次数、出现的异常等。本质上，它是一个维护类项目，便于你对项目的动态进行实时监控。

![](img/1e9f7407050e870238cd05cf7a895fa6_39.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_41.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_43.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_44.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_46.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_48.png)

### Actuator 的基本使用

![](img/1e9f7407050e870238cd05cf7a895fa6_50.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_52.png)

以下是Actuator的基本使用步骤。

![](img/1e9f7407050e870238cd05cf7a895fa6_54.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_56.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_58.png)

1.  **创建项目与引入依赖**
    首先，在IDE（如IntelliJ IDEA）中新建一个Spring Boot项目。在创建时，于依赖选择界面勾选 `Spring Boot Actuator`。如果创建时未勾选，也可以在项目创建后，在 `pom.xml` 文件中手动添加以下依赖：
    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
    ```

2.  **配置监控端点**
    Actuator的监控信息通过一系列HTTP端点（endpoints）暴露。配置在 `application.properties` 或 `application.yml` 文件中进行。如果什么都不配置，默认只开放少数几个端点（如 `/actuator/health`）。
    要开放所有端点，可以在配置文件中添加：
    ```properties
    management.endpoints.web.exposure.include=*
    ```
    启动应用后，访问 `http://localhost:8080/actuator`（端口可能不同），即可看到所有可用的监控端点列表，例如 `/env`（环境变量）、`/info`（应用信息）、`/heapdump`（堆内存转储）等。

![](img/1e9f7407050e870238cd05cf7a895fa6_60.png)

### Actuator 的安全问题：信息泄露

![](img/1e9f7407050e870238cd05cf7a895fa6_62.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_64.png)

Actuator本身是为了方便监控，但如果配置不当（如未授权即可访问所有端点），就会导致严重的信息泄露。其中，`/heapdump` 端点的风险尤为突出。

![](img/1e9f7407050e870238cd05cf7a895fa6_66.png)

`heapdump` 文件是JVM堆内存的转储文件，可以理解为项目运行时的一个内存快照。这个文件中可能包含应用程序运行时的各种数据，例如：

![](img/1e9f7407050e870238cd05cf7a895fa6_68.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_70.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_72.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_74.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_76.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_78.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_80.png)

*   **数据库连接信息**：配置在项目中的数据库URL、用户名和密码。
*   **源代码片段**：部分类和方法信息。
*   **内存中的敏感数据**：如会话信息、加密密钥等。

![](img/1e9f7407050e870238cd05cf7a895fa6_82.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_84.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_86.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_88.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_90.png)

**攻击者如果能够访问并下载 `heapdump` 文件，就可以使用分析工具从中提取出这些敏感信息。**

![](img/1e9f7407050e870238cd05cf7a895fa6_92.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_94.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_96.png)

以下是利用 `heapdump` 文件提取信息的演示过程：

![](img/1e9f7407050e870238cd05cf7a895fa6_98.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_100.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_102.png)

1.  **构造存在数据库配置的项目**：创建一个Spring Boot项目，在 `application.properties` 中配置数据库连接信息。
    ```properties
    spring.datasource.url=jdbc:mysql://localhost:3306/demo_db
    spring.datasource.username=root
    spring.datasource.password=123456
    ```
2.  **启用Actuator并暴露端点**：确保Actuator依赖已引入，并配置 `management.endpoints.web.exposure.include=*`。
3.  **访问并下载heapdump**：启动应用，访问 `http://localhost:8080/actuator/heapdump`，浏览器会自动下载一个 `.hprof` 文件。
4.  **使用工具分析heapdump**：
    *   **使用JDK自带工具**：`jhat` 或 `jvisualvm` 可以打开 `.hprof` 文件进行分析，但需要一定的查询技巧。
    *   **使用自动化提取工具**：网上存在一些专门用于从heapdump中提取敏感信息的开源工具（如某些Java编写的提取器）。运行这些工具，指定heapdump文件路径，即可自动扫描并输出可能的敏感信息，如上面配置的数据库密码。

![](img/1e9f7407050e870238cd05cf7a895fa6_104.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_106.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_108.png)

除了 `heapdump`，其他端点也可能泄露信息：
*   `/env`：泄露所有环境变量和配置属性。
*   `/mappings`：泄露所有Spring MVC控制器映射（即API接口路径）。
*   `/loggers`：显示和修改日志配置。

### 安全加固建议

![](img/1e9f7407050e870238cd05cf7a895fa6_110.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_112.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_114.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_116.png)

为了防止Actuator导致的信息泄露，可以采取以下措施：

![](img/1e9f7407050e870238cd05cf7a895fa6_118.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_120.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_122.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_124.png)

1.  **限制暴露的端点**：在生产环境中，只开放必要的端点（如 `/health`）。
    ```properties
    management.endpoints.web.exposure.include=health,info
    ```
2.  **禁用高风险端点**：明确关闭 `heapdump`、`env` 等端点。
    ```properties
    management.endpoint.heapdump.enabled=false
    management.endpoint.env.enabled=false
    ```
3.  **集成安全框架**：将Actuator端点纳入Spring Security的管理之下，配置访问权限，只允许授权用户（如管理员）访问。
4.  **使用独立的管理端口**：为Actuator端点配置一个独立的管理端口，并与应用主端口隔离，并在防火墙层面进行限制。

![](img/1e9f7407050e870238cd05cf7a895fa6_126.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_128.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_130.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_132.png)

---

![](img/1e9f7407050e870238cd05cf7a895fa6_134.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_136.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_138.png)

## Swagger 接口文档与自动化测试 🔧

![](img/1e9f7407050e870238cd05cf7a895fa6_140.png)

上一节我们介绍了Actuator监控系统及其安全隐患。本节中，我们来看看第二个主题：Swagger。Swagger（现称为OpenAPI）是一个流行的接口文档生成工具，在Java（特别是Spring Boot）和其他语言开发中广泛应用。

![](img/1e9f7407050e870238cd05cf7a895fa6_142.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_144.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_146.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_148.png)

它的主要作用是在开发前后，帮助开发者和测试人员对项目接口的正确性进行验证和测试。开发者集成Swagger后，它会自动扫描代码中的注解，生成一个可视化的Web页面，清晰地列出所有API接口、参数、请求方法等信息，并允许直接在页面上发起接口调用测试。

![](img/1e9f7407050e870238cd05cf7a895fa6_150.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_152.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_154.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_156.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_158.png)

### Swagger 的基本使用

![](img/1e9f7407050e870238cd05cf7a895fa6_160.png)

以下是Swagger在Spring Boot中的基本集成步骤。

![](img/1e9f7407050e870238cd05cf7a895fa6_162.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_164.png)

1.  **引入依赖**
    Swagger有2.x和3.x（OpenAPI 3）等版本。以常用的Swagger 2为例，需要在 `pom.xml` 中添加两个依赖：
    ```xml
    <dependency>
        <groupId>io.springfox</groupId>
        <artifactId>springfox-swagger2</artifactId>
        <version>2.9.2</version>
    </dependency>
    <dependency>
        <groupId>io.springfox</groupId>
        <artifactId>springfox-swagger-ui</artifactId>
        <version>2.9.2</version>
    </dependency>
    ```
    **注意**：Spring Boot 2.6+ 版本在路径匹配上有所变化，可能需要额外配置 `spring.mvc.pathmatch.matching-strategy=ant_path_matcher` 来避免启动报错。

2.  **启用Swagger**
    在Spring Boot的主应用类上添加 `@EnableSwagger2` 注解。
    ```java
    @SpringBootApplication
    @EnableSwagger2
    public class DemoApplication {
        public static void main(String[] args) {
            SpringApplication.run(DemoApplication.class, args);
        }
    }
    ```

![](img/1e9f7407050e870238cd05cf7a895fa6_166.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_168.png)

3.  **访问Swagger UI**
    启动应用后，访问 `http://localhost:8080/swagger-ui.html`（Swagger 2）或 `http://localhost:8080/swagger-ui/index.html`（Swagger 3/OpenAPI 3）。页面上会展示所有被扫描到的Controller及其接口，你可以查看每个接口的详细信息，并点击“Try it out”按钮直接发起请求进行测试。

![](img/1e9f7407050e870238cd05cf7a895fa6_170.png)

### Swagger 的安全问题：接口暴露与自动化测试

![](img/1e9f7407050e870238cd05cf7a895fa6_172.png)

Swagger本身不是漏洞，但它会带来一个重要的安全问题：**接口信息过度暴露**。

![](img/1e9f7407050e870238cd05cf7a895fa6_174.png)

对于一个进行安全测试的网站，测试者通常需要花费大量精力进行信息收集，才能发现潜在的API接口（如后台管理接口、文件上传接口、数据查询接口）。然而，如果该网站部署了Swagger且未做访问控制，那么所有接口信息就会一目了然地展示在Swagger UI页面上。

![](img/1e9f7407050e870238cd05cf7a895fa6_176.png)

这相当于直接向测试者（或攻击者）提供了网站的“API地图”，带来了以下风险：

![](img/1e9f7407050e870238cd05cf7a895fa6_178.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_180.png)

*   **发现测试入口**：直接获知文件上传、数据提交、用户信息查询等功能的接口地址。
*   **未授权访问测试**：尝试调用一些本应需要权限的接口，测试是否存在未授权访问漏洞。
*   **敏感功能探测**：发现后台管理、用户删除、配置修改等敏感接口。
*   **自动化测试基础**：为后续的自动化漏洞扫描提供了清晰的接口列表和参数结构。

![](img/1e9f7407050e870238cd05cf7a895fa6_182.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_184.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_186.png)

**利用Swagger进行（半）自动化测试：**

面对Swagger暴露的大量接口，手动逐个测试效率低下。可以借助API测试工具（如Postman）实现半自动化测试。

![](img/1e9f7407050e870238cd05cf7a895fa6_188.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_190.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_192.png)

1.  **导入Swagger文档**：在Swagger UI页面上，通常可以找到一个JSON格式的文档链接（如 `/v2/api-docs`）。复制此URL。
2.  **在Postman中导入**：打开Postman，点击“Import” -> “Link”，粘贴Swagger文档的URL。Postman会自动根据文档创建所有接口的请求集合。
3.  **批量运行测试**：在Postman的集合运行器（Collection Runner）中，选择需要测试的接口，设置可能的参数或变量，即可批量发送请求。通过观察返回结果，可以快速筛选出存在异常（如报错信息泄露、未授权返回数据等）的接口进行深入分析。
4.  **联动安全扫描工具**：更进一步，可以将Postman的代理设置为Burp Suite或Xray等漏洞扫描器，这样在自动化接口测试的同时，流量会经过扫描器，自动检测SQL注入、XSS等常见漏洞。

![](img/1e9f7407050e870238cd05cf7a895fa6_194.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_196.png)

### 安全加固建议

![](img/1e9f7407050e870238cd05cf7a895fa6_198.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_200.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_202.png)

1.  **访问控制**：使用Spring Security等安全框架，对Swagger的访问路径（如 `/swagger-ui.html`， `/v2/api-docs`）进行权限控制，只允许内部或授权用户访问。
2.  **环境隔离**：仅在开发、测试环境启用Swagger，生产环境务必禁用。
3.  **使用注解控制暴露范围**：在Swagger配置中，可以通过注解精细控制哪些接口需要生成文档，避免全部暴露。

![](img/1e9f7407050e870238cd05cf7a895fa6_204.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_206.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_208.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_210.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_212.png)

---

![](img/1e9f7407050e870238cd05cf7a895fa6_214.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_216.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_218.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_220.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_222.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_224.png)

## 总结 📝

![](img/1e9f7407050e870238cd05cf7a895fa6_226.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_228.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_230.png)

本节课中我们一起学习了JavaEE应用开发中两个重要组件：Spring Boot Actuator和Swagger。

![](img/1e9f7407050e870238cd05cf7a895fa6_232.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_234.png)

*   **Actuator**：是一个强大的应用监控系统，用于收集指标、检查健康状态。但其配置不当（特别是暴露`/heapdump`、`/env`等端点）会导致**敏感信息（如数据库密码、配置属性）严重泄露**。我们演示了如何下载和分析`heapdump`文件来提取这些信息。
*   **Swagger**：是一个优秀的接口文档生成与测试工具，极大提升了开发效率。但它会**完整暴露应用的所有API接口**，为攻击者提供了清晰的攻击面地图，可能辅助发现未授权访问、文件上传等漏洞点。我们介绍了如何利用Postman等工具对Swagger暴露的接口进行自动化测试。

![](img/1e9f7407050e870238cd05cf7a895fa6_236.png)

![](img/1e9f7407050e870238cd05cf7a895fa6_238.png)

两者本身都是优秀的开发辅助工具，其“安全问题”根源在于**运维和配置的疏忽**。作为安全人员，了解它们的工作原理和常见配置，有助于在渗透测试和信息收集中快速识别此类风险点。作为开发者，则应在生产环境中严格遵循最小化暴露原则，做好访问控制。