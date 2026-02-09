# Java安全课程 P65：SQL注入、XXE、SSTI与SPEL表达式注入 🔐

![](img/e5942ddd3dada5b6ba875c20377dfc49_0.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_2.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_4.png)

在本节课中，我们将学习Java安全中几个核心的Web漏洞：SQL注入、XXE、SSTI模板注入以及SPEL表达式注入。我们将通过两个集成漏洞环境来演示这些漏洞在Java中的成因、利用方式以及与PHP环境的差异。

![](img/e5942ddd3dada5b6ba875c20377dfc49_6.png)

## 概述 📋

![](img/e5942ddd3dada5b6ba875c20377dfc49_8.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_10.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_12.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_14.png)

Java安全漏洞在原理上与PHP等语言相似，但具体实现和利用方式存在差异。本节课的重点是理解这些漏洞在Java代码中的表现形式，特别是JDBC、MyBatis框架下的SQL注入，以及Java特有的SPEL表达式注入。

![](img/e5942ddd3dada5b6ba875c20377dfc49_16.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_18.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_20.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_22.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_24.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_26.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_28.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_30.png)

## 环境搭建与准备 🛠️

![](img/e5942ddd3dada5b6ba875c20377dfc49_32.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_34.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_36.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_38.png)

为了便于学习，我们使用两个Java漏洞集成环境：`java-sec` 和 `hello-java-sec`。它们基于Spring Boot框架，集成了多种常见漏洞。

![](img/e5942ddd3dada5b6ba875c20377dfc49_40.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_42.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_44.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_46.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_48.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_49.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_51.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_53.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_55.png)

以下是环境搭建的关键步骤：

![](img/e5942ddd3dada5b6ba875c20377dfc49_57.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_59.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_61.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_63.png)

1.  **数据库配置**：导入项目提供的SQL文件，并确保`application.properties`或相关配置文件中的数据库连接信息（如`jdbc:mysql://localhost:3306/java_sec`）与本地MySQL环境一致。
2.  **Java版本**：建议使用JDK 1.8运行环境，以避免版本兼容性问题。启动命令示例如下：
    ```bash
    "C:\Program Files\Java\jdk1.8.0_xxx\bin\java.exe" -jar java-sec.jar
    ```
3.  **项目启动**：成功启动后，通过浏览器访问对应的端口（如`http://localhost:8000`）即可进入漏洞演示平台。

这两个环境界面清晰，分类明确，并且提供了漏洞代码与安全代码的对比，非常适合初学者理解。

![](img/e5942ddd3dada5b6ba875c20377dfc49_65.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_67.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_69.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_71.png)

## SQL注入（JDBC & MyBatis）💉

![](img/e5942ddd3dada5b6ba875c20377dfc49_73.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_75.png)

上一节我们准备好了实验环境，本节中我们来看看Java中最常见的漏洞之一：SQL注入。在Java中，SQL注入主要出现在直接操作数据库的代码中，尤其是使用JDBC和MyBatis框架时。

![](img/e5942ddd3dada5b6ba875c20377dfc49_77.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_79.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_80.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_82.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_84.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_86.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_88.png)

### JDBC中的SQL注入

![](img/e5942ddd3dada5b6ba875c20377dfc49_90.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_92.png)

JDBC是Java连接数据库的标准API。产生注入的根本原因是**字符串拼接**。

**漏洞代码示例：**
```java
String sql = "SELECT * FROM users WHERE id = " + request.getParameter("id");
Statement stmt = connection.createStatement();
ResultSet rs = stmt.executeQuery(sql);
```
上述代码直接将用户输入的`id`参数拼接到SQL语句中，如果用户输入`1 OR 1=1`，就会改变原语句的逻辑。

![](img/e5942ddd3dada5b6ba875c20377dfc49_94.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_96.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_97.png)

**安全写法（使用预编译PreparedStatement）：**
```java
String sql = "SELECT * FROM users WHERE id = ?";
PreparedStatement pstmt = connection.prepareStatement(sql);
pstmt.setInt(1, Integer.parseInt(request.getParameter("id")));
ResultSet rs = pstmt.executeQuery();
```
使用`?`作为占位符，然后通过`setXXX`方法设置参数值，数据库会对待参数值进行转义，从而避免注入。

![](img/e5942ddd3dada5b6ba875c20377dfc49_99.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_101.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_103.png)

**关键点**：即使使用了`PreparedStatement`，但如果SQL语句本身仍是拼接而成的，预编译也会失效。安全的核心在于 **SQL语句本身必须使用`?`占位符**。

![](img/e5942ddd3dada5b6ba875c20377dfc49_105.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_107.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_109.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_111.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_113.png)

### MyBatis中的SQL注入

![](img/e5942ddd3dada5b6ba875c20377dfc49_115.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_117.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_119.png)

MyBatis是一个优秀的持久层框架，它使用XML或注解来配置和映射SQL。在MyBatis中，需要关注两种参数符号：`#{}`和`${}`。

![](img/e5942ddd3dada5b6ba875c20377dfc49_121.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_123.png)

*   `#{}`：相当于JDBC中的`?`，会进行预编译，是**安全**的。
*   `${}`：直接进行字符串替换，相当于拼接，是**危险**的。

![](img/e5942ddd3dada5b6ba875c20377dfc49_125.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_127.png)

在`ORDER BY`、`LIKE`和`IN`等子句中，如果直接使用`#{}`，MyBatis会报错（因为预编译后参数会被引号包围，如`ORDER BY ‘id’`）。因此，部分开发者会图方便使用`${}`，从而导致注入。

![](img/e5942ddd3dada5b6ba875c20377dfc49_129.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_131.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_133.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_135.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_137.png)

**漏洞代码示例（MyBatis XML映射文件）：**
```xml
<select id="findUser" parameterType="String" resultType="User">
  SELECT * FROM users ORDER BY ${orderBy}
</select>
```
如果`orderBy`参数用户可控，传入`id; DROP TABLE users--`就会导致注入。

![](img/e5942ddd3dada5b6ba875c20377dfc49_139.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_141.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_142.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_144.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_146.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_148.png)

**安全写法**：
1.  在业务层对传入`${}`的参数进行严格的过滤或白名单校验。
2.  尽量避免在`ORDER BY`等子句中使用用户动态传入的列名。

![](img/e5942ddd3dada5b6ba875c20377dfc49_150.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_152.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_154.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_156.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_158.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_160.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_162.png)

### 白盒审计案例

![](img/e5942ddd3dada5b6ba875c20377dfc49_164.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_166.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_168.png)

在一个基于Spring+MyBatis的网校系统源码中，我们通过搜索`${`关键字，发现一处删除文章的功能存在注入。

![](img/e5942ddd3dada5b6ba875c20377dfc49_170.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_172.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_174.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_176.png)

1.  在XML映射文件中找到语句：`DELETE FROM article WHERE id IN (${ids})`。
2.  追踪调用该语句的Java代码，定位到后台的`ArticleController`中的`delete`方法。
3.  该方法接收`ids`参数并直接传递给MyBatis执行。
4.  构造请求`/admin/article/delete?ids=1) OR (1=1`，使用Sqlmap等工具可成功检测出注入点。

![](img/e5942ddd3dada5b6ba875c20377dfc49_178.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_180.png)

**审计技巧**：在Java代码审计中，针对SQL注入，可以优先全局搜索`${`、`ORDER BY`、`LIKE`等关键词，快速定位潜在风险点。

![](img/e5942ddd3dada5b6ba875c20377dfc49_182.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_184.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_185.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_187.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_189.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_191.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_193.png)

## XXE（XML外部实体注入）🗂️

![](img/e5942ddd3dada5b6ba875c20377dfc49_195.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_197.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_199.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_201.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_203.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_205.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_207.png)

SQL注入主要发生在数据库交互层，而XXE则发生在XML解析环节。Java中提供了多种XML解析器，如果配置不当，在解析用户可控的XML数据时，就会导致XXE漏洞。

![](img/e5942ddd3dada5b6ba875c20377dfc49_209.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_211.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_213.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_215.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_217.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_219.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_221.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_223.png)

Java中常见的危险解析类/方法包括：
*   `XMLReader`
*   `SAXReader`
*   `SAXBuilder`
*   `DocumentBuilder`
*   `XMLStreamReader`

![](img/e5942ddd3dada5b6ba875c20377dfc49_225.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_227.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_229.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_231.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_233.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_234.png)

**漏洞代码示例：**
```java
String xmlContent = request.getParameter("content");
SAXReader reader = new SAXReader();
Document document = reader.read(new StringReader(xmlContent));
// ... 处理document
```
如果用户传入的`content`参数包含恶意的外部实体声明，如：
```xml
<?xml version="1.0"?>
<!DOCTYPE test [
  <!ENTITY xxe SYSTEM "file:///etc/passwd">
]>
<root>&xxe;</root>
```
解析器就会读取服务器上的`/etc/passwd`文件内容。

![](img/e5942ddd3dada5b6ba875c20377dfc49_236.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_238.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_240.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_242.png)

**安全写法**：禁用外部实体解析。
```java
SAXReader reader = new SAXReader();
reader.setFeature("http://apache.org/xml/features/disallow-doctype-decl", true);
reader.setFeature("http://xml.org/sax/features/external-general-entities", false);
reader.setFeature("http://xml.org/sax/features/external-parameter-entities", false);
```

![](img/e5942ddd3dada5b6ba875c20377dfc49_244.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_246.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_248.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_250.png)

**黑盒测试**：与语言无关，只要发现请求数据包中包含XML格式数据（如`Content-Type: application/xml`），就可以尝试替换为包含恶意外部实体的Payload，观察响应中是否包含敏感文件内容或是否有对外部网络的请求（如监听端口收到请求）。

![](img/e5942ddd3dada5b6ba875c20377dfc49_252.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_254.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_256.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_258.png)

## SSTI（服务器端模板注入）📄

![](img/e5942ddd3dada5b6ba875c20377dfc49_260.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_262.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_264.png)

XXE处理的是结构化数据，而SSTI涉及的是视图渲染。SSTI发生在使用模板引擎渲染用户输入时。当用户输入被直接拼接进模板字符串，并且该模板被解析执行时，就会导致代码执行。

![](img/e5942ddd3dada5b6ba875c20377dfc49_266.png)

Java中常见的模板引擎有Thymeleaf、FreeMarker、Velocity等。Spring Boot默认支持Thymeleaf。

![](img/e5942ddd3dada5b6ba875c20377dfc49_268.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_270.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_272.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_274.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_276.png)

**漏洞代码示例（Thymeleaf）：**
假设一个控制器接收`language`参数来渲染不同语言页面：
```java
@GetMapping("/greet")
public String greet(@RequestParam String lang, Model model) {
    model.addAttribute("message", "Hello");
    return "greet_" + lang; // 用户可控部分拼接进视图名
}
```
如果用户传入`lang`参数为`__${T(java.lang.Runtime).getRuntime().exec("calc")}__::.x`，在某些特定版本和配置下，Thymeleaf可能会将其解析执行，弹出计算器。

![](img/e5942ddd3dada5b6ba875c20377dfc49_278.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_280.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_282.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_284.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_286.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_288.png)

**漏洞成因**：视图名称（`return`的字符串）或模板内容本身直接包含了未经验证的用户输入。

![](img/e5942ddd3dada5b6ba875c20377dfc49_290.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_292.png)

**安全写法**：
1.  对动态的模板名称或内容片段进行严格的白名单控制。
2.  避免直接将用户输入传递给模板引擎的解析函数。

![](img/e5942ddd3dada5b6ba875c20377dfc49_294.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_296.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_298.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_300.png)

**测试与审计**：
*   **黑盒**：在所有可能影响页面渲染的参数中（如`lang`、`template`、`name`等），尝试插入模板引擎的表达式语句，如`{{7*7}}`、`${7*7}`、`#{7*7}`等，观察响应中是否被运算成`49`。
*   **白盒**：审计代码，查找`return`语句中字符串拼接、或者`model.addAttribute`将用户输入设置为模板变量等模式。

![](img/e5942ddd3dada5b6ba875c20377dfc49_302.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_304.png)

## SPEL（Spring表达式语言）注入⚡

![](img/e5942ddd3dada5b6ba875c20377dfc49_306.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_308.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_310.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_312.png)

SSTI是模板引擎的问题，而SPEL注入是Spring框架特有的表达式注入漏洞。SPEL是一种强大的表达式语言，用于在运行时查询和操作对象图。如果在解析用户可控的SPEL表达式时未做防护，就会造成严重的安全问题。

![](img/e5942ddd3dada5b6ba875c20377dfc49_314.png)

**漏洞代码示例：**
```java
import org.springframework.expression.Expression;
import org.springframework.expression.ExpressionParser;
import org.springframework.expression.spel.standard.SpelExpressionParser;

![](img/e5942ddd3dada5b6ba875c20377dfc49_316.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_318.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_320.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_322.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_324.png)

@GetMapping("/eval")
public String eval(@RequestParam String expr) {
    ExpressionParser parser = new SpelExpressionParser();
    Expression exp = parser.parseExpression(expr); // 用户输入直接被解析
    return exp.getValue().toString();
}
```
用户访问`/eval?expr=T(java.lang.Runtime).getRuntime().exec('calc')`即可执行命令。

![](img/e5942ddd3dada5b6ba875c20377dfc49_326.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_328.png)

**安全写法**：
1.  **根本避免**：不要解析用户可控的字符串作为SPEL表达式。
2.  **沙箱限制**：如果业务必须，应使用`StandardEvaluationContext`的`setRootObject`等限制解析上下文，或使用更安全的`SimpleEvaluationContext`。

**黑名单绕过技巧**：
如果代码中使用了黑名单过滤关键字（如`Runtime`、`exec`），可以利用Java的反射机制进行字符串拼接来绕过。
例如，原Payload为：`T(java.lang.Runtime).getRuntime().exec("calc")`
反射绕过Payload可能形如：
```
T(String).getClass().forName("java.l"+"ang.Ru"+"ntime").getMethod("ex"+"ec", T(String[])).invoke(T(String).getClass().forName("java.l"+"ang.Ru"+"ntime").getMethod("getRu"+"ntime").invoke(null), new String[]{"calc"})
```
这利用了反射动态获取类和方法，将敏感关键字拆散，从而绕过简单的字符串匹配过滤。

![](img/e5942ddd3dada5b6ba875c20377dfc49_330.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_332.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_334.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_336.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_338.png)

## 总结 🎯

![](img/e5942ddd3dada5b6ba875c20377dfc49_340.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_342.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_344.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_346.png)

本节课我们一起学习了Java安全中四个重要的漏洞类型：

![](img/e5942ddd3dada5b6ba875c20377dfc49_348.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_350.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_352.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_354.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_356.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_358.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_360.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_362.png)

1.  **SQL注入**：重点关注JDBC中**字符串拼接**与**预编译占位符`?`**的区别，以及MyBatis中**`${}`**与**`#{}`**的区别。审计时搜索`${`、`ORDER BY`等关键字。
2.  **XXE**：由不安全的XML解析器配置引起。审计时搜索`XMLReader`、`DocumentBuilder`等解析类，并检查是否禁用了外部实体（`setFeature`）。
3.  **SSTI**：发生在模板引擎渲染用户可控的模板名称或内容时。测试时尝试插入模板表达式`{{}}`、`${}`等。
4.  **SPEL注入**：Spring特有的表达式注入。避免直接解析用户输入为SPEL表达式，警惕`SpelExpressionParser`的使用。

![](img/e5942ddd3dada5b6ba875c20377dfc49_364.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_366.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_368.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_369.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_371.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_373.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_374.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_376.png)

![](img/e5942ddd3dada5b6ba875c20377dfc49_378.png)

Java安全漏洞的挖掘，白盒审计依赖于对代码中特定危险模式（如字符串拼接、特定解析函数）的识别；黑盒测试则与语言关系不大，更多依赖于对漏洞原理和常见Payload的掌握。理解这些漏洞在Java中的独特表现，是成为一名合格的Java安全研究员的关键一步。