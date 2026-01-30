# 🚀 JavaEE应用与SpringBoot框架安全入门 - 第38课

![](img/8ee6635d5b991869a10fd77d40be2da5_1.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_3.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_5.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_7.png)

在本节课中，我们将学习当前主流的Java Web开发框架——SpringBoot，并重点探讨其与MyBatis数据库框架集成时可能产生的SQL注入问题，以及Thymeleaf模板引擎的模板注入漏洞。课程内容将兼顾框架的简单使用与安全风险分析，旨在让初学者能够理解并识别这些常见的安全隐患。

## 📦 SpringBoot框架简介

![](img/8ee6635d5b991869a10fd77d40be2da5_9.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_11.png)

SpringBoot是一个Java Web开发框架。它之所以成为主流，是因为其开发过程更加智能、快捷和高效，符合当前的技术潮流。相较于早期的Spring MVC或Spring Framework，SpringBoot极大地简化了配置和部署流程。

![](img/8ee6635d5b991869a10fd77d40be2da5_13.png)

### 基本Web应用开发

![](img/8ee6635d5b991869a10fd77d40be2da5_15.png)

上一节我们介绍了SpringBoot是什么，本节中我们来看看如何使用它创建一个简单的Web应用。

![](img/8ee6635d5b991869a10fd77d40be2da5_17.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_19.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_21.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_23.png)

首先，在IDE中创建一个新的SpringBoot项目，选择`Maven`作为构建工具，并添加`Web`依赖。

![](img/8ee6635d5b991869a10fd77d40be2da5_25.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_26.png)

创建完成后，我们来编写一个控制器（Controller），处理HTTP请求。

![](img/8ee6635d5b991869a10fd77d40be2da5_28.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_30.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_32.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_34.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_36.png)

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

![](img/8ee6635d5b991869a10fd77d40be2da5_38.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_40.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_42.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_44.png)

@RestController
@RequestMapping("/demo")
public class IndexController {

    @GetMapping("/test")
    public String test() {
        return "Hello, SpringBoot!";
    }
}
```

启动应用后，访问 `http://localhost:8080/demo/test`，页面将显示“Hello, SpringBoot!”。

![](img/8ee6635d5b991869a10fd77d40be2da5_46.png)

以下是SpringBoot中几种常见的请求映射方式：

![](img/8ee6635d5b991869a10fd77d40be2da5_48.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_50.png)

*   **无参GET请求**：使用 `@GetMapping` 注解。
*   **带参GET请求**：在方法参数中使用 `@RequestParam` 注解接收参数。
*   **POST请求**：使用 `@PostMapping` 注解，并通过 `@RequestParam` 或对象接收参数。

![](img/8ee6635d5b991869a10fd77d40be2da5_52.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_54.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_56.png)

**`@RestController` 与 `@Controller` 的区别**：`@RestController` 是 `@Controller` 和 `@ResponseBody` 的组合注解，其方法返回值会直接写入HTTP响应体，通常用于返回JSON或XML数据。而 `@Controller` 通常用于返回视图名称，需要配合 `@ResponseBody` 注解才能返回数据。

![](img/8ee6635d5b991869a10fd77d40be2da5_58.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_60.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_62.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_64.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_66.png)

## 🗃️ SpringBoot集成MyBatis与SQL注入风险

![](img/8ee6635d5b991869a10fd77d40be2da5_68.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_70.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_72.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_74.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_76.png)

上一节我们介绍了SpringBoot的基本Web开发，本节中我们来看看如何集成MyBatis进行数据库操作，并分析其SQL注入风险。

![](img/8ee6635d5b991869a10fd77d40be2da5_78.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_80.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_82.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_84.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_86.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_88.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_90.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_92.png)

MyBatis是Java中主流的持久层框架之一。在Java中分析SQL注入，需要关注所使用的数据库操作方式（如JDBC、MyBatis、Hibernate），不同方式的安全问题各异。

![](img/8ee6635d5b991869a10fd77d40be2da5_94.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_96.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_98.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_100.png)

### 集成MyBatis步骤

![](img/8ee6635d5b991869a10fd77d40be2da5_102.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_104.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_106.png)

以下是集成MyBatis并进行数据库查询的基本步骤：

![](img/8ee6635d5b991869a10fd77d40be2da5_108.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_110.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_112.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_114.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_116.png)

1.  **准备数据库**：确保MySQL服务已启动，并创建好数据库和表（例如 `user` 表，包含id, username, password字段）。
2.  **添加依赖**：在创建SpringBoot项目时，勾选 `MyBatis Framework` 和 `MySQL Driver` 依赖。
3.  **配置数据库连接**：在 `application.yml` 文件中配置数据库连接信息。
    ```yaml
    spring:
      datasource:
        url: jdbc:mysql://localhost:3306/demo01
        username: root
        password: root
        driver-class-name: com.mysql.cj.jdbc.Driver
    ```
4.  **创建实体类**：创建 `User` 类，用于映射数据库表中的数据。
    ```java
    public class User {
        private Integer id;
        private String username;
        private String password;
        // 省略 getter, setter, toString 方法
    }
    ```
5.  **创建Mapper接口**：创建 `UserMapper` 接口，并使用 `@Mapper` 注解。在其中定义SQL映射方法。
    ```java
    @Mapper
    public interface UserMapper {
        @Select("SELECT * FROM user")
        List<User> findAll();
    }
    ```
6.  **创建Controller进行查询**：在Controller中注入 `UserMapper` 并调用其方法。
    ```java
    @RestController
    public class UserController {
        @Autowired
        private UserMapper userMapper;

        @GetMapping("/users")
        public List<User> getUsers() {
            return userMapper.findAll();
        }
    }
    ```
启动应用，访问 `http://localhost:8080/users`，即可获取用户列表的JSON数据。

![](img/8ee6635d5b991869a10fd77d40be2da5_118.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_120.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_121.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_123.png)

### MyBatis中的SQL注入风险

![](img/8ee6635d5b991869a10fd77d40be2da5_125.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_127.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_129.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_131.png)

MyBatis的SQL语句编写有两种参数占位符：`#{}`（预编译，安全）和 `${}`（拼接，危险）。SQL注入通常发生在以下三种使用 `${}` 进行字符串拼接的场景中：

![](img/8ee6635d5b991869a10fd77d40be2da5_133.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_135.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_137.png)

*   **模糊查询 (`like`)**
*   **IN 条件语句**
*   **ORDER BY 排序字段**

![](img/8ee6635d5b991869a10fd77d40be2da5_139.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_141.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_143.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_145.png)

以下是易产生注入的写法示例（使用 `${}`）：

![](img/8ee6635d5b991869a10fd77d40be2da5_147.png)

```xml
<!-- 在Mapper XML文件中 -->
<select id="findByUsername" resultType="User">
    SELECT * FROM user WHERE username LIKE '%${username}%'
</select>
```

当攻击者传入 `username` 参数值为 `' OR '1'='1` 时，会导致SQL注入。

![](img/8ee6635d5b991869a10fd77d40be2da5_149.png)

**安全的写法应使用 `#{}`**：

![](img/8ee6635d5b991869a10fd77d40be2da5_151.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_153.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_155.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_157.png)

```xml
<select id="findByUsername" resultType="User">
    SELECT * FROM user WHERE username LIKE CONCAT('%', #{username}, '%')
</select>
```

![](img/8ee6635d5b991869a10fd77d40be2da5_159.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_161.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_163.png)

在代码审计时，可以全局搜索 `$` 符号来定位潜在的SQL注入点。

![](img/8ee6635d5b991869a10fd77d40be2da5_165.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_167.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_169.png)

## 🎭 Thymeleaf模板注入（SSTI）

![](img/8ee6635d5b991869a10fd77d40be2da5_171.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_172.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_174.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_176.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_178.png)

上一节我们讨论了数据库层的安全风险，本节中我们来看看视图层的风险——模板注入。

![](img/8ee6635d5b991869a10fd77d40be2da5_180.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_182.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_184.png)

模板引擎（如Thymeleaf）用于动态渲染HTML页面。Thymeleaf是SpringBoot推荐的模板引擎之一。

### Thymeleaf基本使用

![](img/8ee6635d5b991869a10fd77d40be2da5_186.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_188.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_190.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_192.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_194.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_196.png)

1.  创建SpringBoot项目时，添加 `Thymeleaf` 依赖。
2.  在 `resources/templates` 目录下创建模板文件，例如 `index.html`。
    ```html
    <!DOCTYPE html>
    <html xmlns:th="http://www.thymeleaf.org">
    <head>
        <title>Home</title>
    </head>
    <body>
        <h1 th:text="${message}">Default Message</h1>
    </body>
    </html>
    ```
3.  创建Controller，向模板传递数据。
    ```java
    @Controller // 注意这里使用 @Controller，不是 @RestController
    public class PageController {
        @GetMapping("/")
        public String index(Model model) {
            model.addAttribute("message", "Hello, Thymeleaf!");
            return "index"; // 返回模板名称，对应 index.html
        }
    }
    ```
访问 `http://localhost:8080/`，页面将显示“Hello, Thymeleaf!”。

![](img/8ee6635d5b991869a10fd77d40be2da5_198.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_200.png)

### Thymeleaf模板注入漏洞

![](img/8ee6635d5b991869a10fd77d40be2da5_202.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_204.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_206.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_207.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_209.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_211.png)

模板注入（SSTI）发生在当用户输入被直接拼接进模板表达式并执行时。一个常见的触发场景是“视图名”可控。

考虑以下存在缺陷的代码：

![](img/8ee6635d5b991869a10fd77d40be2da5_213.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_215.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_217.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_219.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_221.png)

```java
@GetMapping("/path")
public String getPage(@RequestParam String lang) {
    // 用户可控的 lang 参数直接拼接到视图名中
    return "user/" + lang + "/index"; // 危险！
}
```

![](img/8ee6635d5b991869a10fd77d40be2da5_223.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_225.png)

攻击者可以构造特殊的 `lang` 参数值，使其成为一段Thymeleaf表达式，例如：
`__${T(java.lang.Runtime).getRuntime().exec("calc")}__::.x`

![](img/8ee6635d5b991869a10fd77d40be2da5_227.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_229.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_231.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_233.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_235.png)

当Thymeleaf版本较低（例如受影响的旧版本）时，访问 `/path?lang=__${T(java.lang.Runtime).getRuntime().exec("calc")}__::.x` 可能会在服务器上执行计算器程序。

![](img/8ee6635d5b991869a10fd77d40be2da5_237.png)

**漏洞修复**：
*   **升级Thymeleaf**：及时更新到已修复该漏洞的安全版本。
*   **避免视图名拼接**：不要将用户输入直接用于动态视图解析。应使用白名单机制校验参数。

![](img/8ee6635d5b991869a10fd77d40be2da5_239.png)

## 📚 本节课总结

![](img/8ee6635d5b991869a10fd77d40be2da5_241.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_243.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_245.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_247.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_249.png)

本节课我们一起学习了SpringBoot框架的基础使用，并深入探讨了两个关键的安全问题：

![](img/8ee6635d5b991869a10fd77d40be2da5_251.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_253.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_255.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_257.png)

1.  **MyBatis SQL注入**：核心在于区分 `#{}`（安全）和 `${}`（危险）的使用场景。在模糊查询、IN语句、ORDER BY子句中错误使用 `${}` 会导致SQL注入。审计时应重点搜索 `${}` 的使用。
2.  **Thymeleaf模板注入（SSTI）**：当用户输入被直接拼接到模板路径或表达式中时，在特定低版本Thymeleaf下可能造成远程代码执行。防范措施包括升级框架和避免用户输入控制视图逻辑。

![](img/8ee6635d5b991869a10fd77d40be2da5_259.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_261.png)

![](img/8ee6635d5b991869a10fd77d40be2da5_263.png)

理解这些框架的特性和常见误用模式，是进行Java Web应用安全测试和代码审计的基础。下节课我们将继续探讨SpringBoot生态中的其他安全议题，如Actuator端点信息泄露和Swagger接口文档安全问题。