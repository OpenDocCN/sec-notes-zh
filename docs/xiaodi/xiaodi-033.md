#  033：JavaEE应用、SQL预编译、Filter过滤器与Listener监听器

![](img/23e3753a0c3bf2e9237690c7ba63441e_1.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_3.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_5.png)

在本节课中，我们将学习Java Web开发中的几个核心概念：SQL预编译、Filter（过滤器）和Listener（监听器）。这些技术不仅是开发中的基础，也与Web安全（如SQL注入防护、内存马原理）紧密相关。我们将通过简单直白的示例，帮助你理解它们的工作原理和应用场景。

![](img/23e3753a0c3bf2e9237690c7ba63441e_7.png)

---

![](img/23e3753a0c3bf2e9237690c7ba63441e_9.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_11.png)

## 📚 概述：今日学习要点

![](img/23e3753a0c3bf2e9237690c7ba63441e_13.png)

本节课内容分为三个主要部分：
1.  **SQL预编译**：深入理解其如何有效防止SQL注入攻击。
2.  **Filter过滤器**：学习如何拦截和过滤HTTP请求，实现访问控制和安全检查。
3.  **Listener监听器**：了解如何监听Web应用中的特定事件（如Session创建）。

![](img/23e3753a0c3bf2e9237690c7ba63441e_15.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_17.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_19.png)

掌握这些知识，将为后续学习高级安全主题（如内存马技术）打下坚实基础。

---

## 🛡️ 第一部分：SQL预编译

![](img/23e3753a0c3bf2e9237690c7ba63441e_21.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_22.png)

上一节我们介绍了SQL注入的基本原理。本节中，我们来看看如何使用SQL预编译来有效防御此类攻击。

![](img/23e3753a0c3bf2e9237690c7ba63441e_24.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_26.png)

SQL预编译的核心思想是**提前固定SQL语句的结构**，将用户输入的数据作为参数传入，而非直接拼接。这样，无论输入内容如何，都不会改变原SQL语句的逻辑，从而杜绝了SQL注入。

![](img/23e3753a0c3bf2e9237690c7ba63441e_28.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_30.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_32.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_34.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_36.png)

### 不安全写法（导致SQL注入）

![](img/23e3753a0c3bf2e9237690c7ba63441e_38.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_40.png)

以下代码直接将用户输入拼接到SQL语句中，存在严重的安全风险。

![](img/23e3753a0c3bf2e9237690c7ba63441e_42.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_44.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_46.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_48.png)

```java
// 接收用户输入
Scanner scanner = new Scanner(System.in);
System.out.println("请输入id：");
String s = scanner.nextLine();
// 将输入直接拼接到SQL语句中（危险！）
String sql = "select * from news where id=" + s;
// 执行查询...
```

![](img/23e3753a0c3bf2e9237690c7ba63441e_50.png)

当用户输入 `1 or 1=1` 时，生成的SQL语句变为 `select * from news where id=1 or 1=1`，会查询出所有数据，造成注入。

![](img/23e3753a0c3bf2e9237690c7ba63441e_52.png)

### 安全写法（使用预编译）

使用 `PreparedStatement` 进行预编译，用 `?` 作为占位符。

![](img/23e3753a0c3bf2e9237690c7ba63441e_54.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_56.png)

```java
// 使用问号作为参数占位符
String safeSql = "select * from news where id=?";
// 获取预编译语句对象
PreparedStatement ps = connection.prepareStatement(safeSql);
// 将用户输入的值安全地设置到第一个参数位置（索引从1开始）
ps.setString(1, s);
// 执行查询...
```

**关键区别**：
*   **不安全写法**：SQL语句结构随用户输入改变。
*   **预编译写法**：SQL语句 `select * from news where id=?` 的结构是固定的。用户输入的值 `1 or 1=1` 会被**整体视为一个字符串参数**传递给 `id` 字段，数据库会寻找 `id` 字段值**等于**字符串 `"1 or 1=1"` 的记录，而不会将其解析为SQL逻辑。因此，注入攻击无法生效。

**简单来说**：预编译将“代码”与“数据”分离，用户输入永远是“数据”，无法变成“代码”来改变程序逻辑。

---

![](img/23e3753a0c3bf2e9237690c7ba63441e_58.png)

## 🔍 第二部分：Filter过滤器

理解了如何防护后端数据库的安全后，我们来看看如何在请求到达业务逻辑之前进行拦截和控制。这就是Filter过滤器的作用。

Filter是Java Web中的核心组件之一，用于**在请求到达Servlet或资源之前，以及响应发送给客户端之前，进行拦截和处理**。它常用于实现：
*   **访问控制与权限验证**（如检查用户是否登录）。
*   **请求/响应的修改**（如统一设置字符编码）。
*   **安全过滤**（如过滤XSS攻击脚本）。
*   **性能优化**（如压缩响应内容）。

其工作流程如下图所示，请求会先经过Filter链，再到达目标Servlet：

```
客户端请求 -> Listener -> Filter -> Servlet -> 数据库
        响应 <- Listener <- Filter <- Servlet <-
```

### 如何创建并使用一个Filter

![](img/23e3753a0c3bf2e9237690c7ba63441e_60.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_62.png)

以下是创建一个用于过滤XSS攻击的Filter的步骤。

![](img/23e3753a0c3bf2e9237690c7ba63441e_64.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_66.png)

**1. 创建Filter类**
创建一个类实现 `javax.servlet.Filter` 接口，并重写其方法。

```java
package com.demo.filter;

![](img/23e3753a0c3bf2e9237690c7ba63441e_68.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_70.png)

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import java.io.IOException;

@WebFilter("/test") // 指定该过滤器拦截的URL路径
public class XssFilter implements Filter {

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        // 初始化方法，服务器启动时执行一次
        System.out.println("XSS过滤器初始化...");
    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
            throws IOException, ServletException {
        // 核心过滤方法，每次匹配的请求都会执行
        System.out.println("XSS过滤器正在工作...");

        // 1. 获取请求参数进行检查
        String code = request.getParameter("code");
        if (code != null && code.contains("script")) {
            // 2. 如果发现攻击载荷，则拦截请求
            response.getWriter().write("检测到XSS攻击，请求被拦截！");
            return; // 注意：这里直接return，不再调用chain.doFilter
        }

        // 3. 如果检查通过，则放行请求，让它继续执行后续的Filter或Servlet
        chain.doFilter(request, response);
    }

    @Override
    public void destroy() {
        // 销毁方法，服务器关闭时执行一次
        System.out.println("XSS过滤器销毁...");
    }
}
```

**2. 配置Filter（如果未使用`@WebFilter`注解）**
在 `web.xml` 文件中配置Filter。

![](img/23e3753a0c3bf2e9237690c7ba63441e_72.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_74.png)

```xml
<filter>
    <filter-name>XssFilter</filter-name>
    <filter-class>com.demo.filter.XssFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>XssFilter</filter-name>
    <url-pattern>/test</url-pattern> <!-- 指定要过滤的URL -->
</filter-mapping>
```

![](img/23e3753a0c3bf2e9237690c7ba63441e_76.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_78.png)

**3. 创建一个测试Servlet**

```java
@WebServlet("/test")
public class TestServlet extends HttpServlet {
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String code = req.getParameter("code");
        resp.getWriter().write("接收到的code是：" + code);
    }
}
```

![](img/23e3753a0c3bf2e9237690c7ba63441e_80.png)

**运行效果**：
*   访问 `http://localhost:8080/test?code=123`，页面正常显示：`接收到的code是：123`。
*   访问 `http://localhost:8080/test?code=<script>alert(1)</script>`，页面显示：`检测到XSS攻击，请求被拦截！`。请求被Filter阻断，不会到达后面的 `TestServlet`。

![](img/23e3753a0c3bf2e9237690c7ba63441e_82.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_84.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_86.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_88.png)

### Filter与内存马

![](img/23e3753a0c3bf2e9237690c7ba63441e_90.png)

在安全领域，Filter有一个重要的“变种”应用——**内存马**。传统Webshell是写在磁盘上的文件（如JSP文件）。而内存马是一种无文件攻击技术，攻击者将恶意代码（作为Filter或Listener）动态注册到正在运行的Java应用服务器内存中。

**为什么危险？**
因为内存马没有实体文件，传统的文件扫描检测方式会失效。它存在于更高的容器层面（Filter链），能够拦截所有请求，危害极大。学习Filter的工作原理，是理解、检测和防御这类高级攻击的基础。

![](img/23e3753a0c3bf2e9237690c7ba63441e_92.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_94.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_96.png)

---

![](img/23e3753a0c3bf2e9237690c7ba63441e_98.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_100.png)

## 👂 第三部分：Listener监听器

![](img/23e3753a0c3bf2e9237690c7ba63441e_102.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_104.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_106.png)

在了解了如何拦截请求之后，我们来看看如何监听Web应用内部发生的事件。Listener监听器用于监听 `ServletContext`、`HttpSession`、`ServletRequest` 等对象的生命周期事件或属性变更事件。

![](img/23e3753a0c3bf2e9237690c7ba63441e_108.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_110.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_112.png)

与Filter主动拦截不同，Listener是**被动触发**的，当特定事件发生时（如Session被创建、销毁），由容器自动调用。

![](img/23e3753a0c3bf2e9237690c7ba63441e_114.png)

Listener主要分为三类：
*   **ServletContext监听器**：监听Web应用的启动和销毁。
*   **HttpSession监听器**：监听用户会话的创建、销毁和属性变更。
*   **ServletRequest监听器**：监听请求的创建、销毁和属性变更。

![](img/23e3753a0c3bf2e9237690c7ba63441e_116.png)

它在安全分析中常用于理解代码流程，同时也是某些类型内存马的载体。

![](img/23e3753a0c3bf2e9237690c7ba63441e_118.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_120.png)

### 如何创建并使用一个Session监听器

以下示例演示如何监听HttpSession的创建和销毁事件。

![](img/23e3753a0c3bf2e9237690c7ba63441e_122.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_124.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_126.png)

**1. 创建Session监听器类**
实现 `HttpSessionListener` 接口。

```java
package com.demo.listener;

import javax.servlet.annotation.WebListener;
import javax.servlet.http.HttpSessionEvent;
import javax.servlet.http.HttpSessionListener;

![](img/23e3753a0c3bf2e9237690c7ba63441e_128.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_130.png)

@WebListener // 标记为监听器，无需配置URL
public class MySessionListener implements HttpSessionListener {

    @Override
    public void sessionCreated(HttpSessionEvent se) {
        // 当有新的Session被创建时触发（如用户首次访问）
        System.out.println("监听器：一个新的Session被创建了，ID: " + se.getSession().getId());
    }

    @Override
    public void sessionDestroyed(HttpSessionEvent se) {
        // 当Session被销毁时触发（如超时、手动失效）
        System.out.println("监听器：一个Session被销毁了，ID: " + se.getSession().getId());
    }
}
```

**2. 创建一个用于操作Session的Servlet**

![](img/23e3753a0c3bf2e9237690c7ba63441e_132.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_133.png)

```java
@WebServlet("/createSession")
public class CreateSessionServlet extends HttpServlet {
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) {
        // 此调用会触发Session创建（如果当前请求没有Session）
        req.getSession();
        resp.getWriter().write("Session Created (or got).");
    }
}

@WebServlet("/invalidateSession")
public class InvalidateSessionServlet extends HttpServlet {
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) {
        HttpSession session = req.getSession(false);
        if (session != null) {
            session.invalidate(); // 手动销毁Session
        }
        resp.getWriter().write("Session Invalidated.");
    }
}
```

**运行效果**：
1.  首次访问 `http://localhost:8080/createSession` 时，控制台会输出：`监听器：一个新的Session被创建了，ID: xxx`。
2.  访问 `http://localhost:8080/invalidateSession` 时，控制台会输出：`监听器：一个Session被销毁了，ID: xxx`。
3.  再次访问 `createSession`，由于之前的Session已销毁，会再次触发 `sessionCreated` 方法。

![](img/23e3753a0c3bf2e9237690c7ba63441e_135.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_137.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_139.png)

**Listener与内存马**：
与Filter类似，攻击者也可以向服务器动态注册一个恶意的Listener（如 `ServletRequestListener`），从而在每一个请求到达时都能执行恶意代码，实现内存马驻留。

![](img/23e3753a0c3bf2e9237690c7ba63441e_141.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_143.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_145.png)

---

![](img/23e3753a0c3bf2e9237690c7ba63441e_147.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_149.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_151.png)

## 📝 总结与回顾

![](img/23e3753a0c3bf2e9237690c7ba63441e_153.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_155.png)

本节课我们一起学习了Java Web中三个至关重要的组件：

![](img/23e3753a0c3bf2e9237690c7ba63441e_157.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_159.png)

![](img/23e3753a0c3bf2e9237690c7ba63441e_161.png)

1.  **SQL预编译**：通过 `PreparedStatement` 固定SQL结构，将用户输入作为参数处理，是防御SQL注入最有效、最根本的手段。其核心公式为：`SQL语句 = 固定结构 + 参数化数据`。
2.  **Filter过滤器**：一个强大的请求/响应拦截器。我们学会了如何创建Filter来检查请求参数（如防XSS）、实现访问控制（如检查Cookie）。更重要的是，理解了它在**内存马**攻击与防御中的关键角色——恶意Filter可以无文件形式驻留内存，拦截所有流量。
3.  **Listener监听器**：用于监听Web应用内部事件（如Session生命周期）。我们创建了一个Session监听器来感知用户会话的创建与销毁。Listener同样可能被用于构造内存马，在事件发生时执行恶意操作。

这些知识不仅是Java EE开发的基础，更是深入理解Web应用安全机制（尤其是近年来流行的无文件攻击、内存马技术）的必经之路。在后续的课程中，当我们探讨代码审计、漏洞分析以及高级攻防技术时，你会不断遇到并应用这些概念。

![](img/23e3753a0c3bf2e9237690c7ba63441e_163.png)

**下节课预告**：我们将开始接触主流的Java开发框架——Spring MVC与MyBatis，并了解它们如何整合，以及在这些框架下的安全考量。