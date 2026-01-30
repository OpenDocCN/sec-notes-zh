# 🚀 JavaEE 应用开发入门：第32天 - Servlet路由、JDBC与MyBatis数据库操作

在本节课中，我们将学习JavaEE应用开发的基础知识，重点包括Servlet路由技术、JDBC数据库连接以及MyBatis框架的初步介绍。通过本教程，你将掌握如何创建和配置Servlet，理解其生命周期，并学会使用JDBC进行数据库操作。

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_1.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_3.png)

---

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_5.png)

## 📝 概述

JavaEE（Java Platform, Enterprise Edition）是用于企业级Web开发的Java平台。本节课是JavaEE系列的第一讲，我们将从Servlet的基础开始，逐步深入到数据库操作。

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_7.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_9.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_11.png)

---

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_13.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_15.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_17.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_19.png)

## 🛠️ Servlet 的创建与使用

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_21.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_23.png)

上一节我们介绍了JavaEE的基本概念，本节中我们来看看如何创建和使用Servlet。

### 创建Servlet类

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_25.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_27.png)

首先，我们需要创建一个类并继承`HttpServlet`。以下是创建Servlet的基本步骤：

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_29.png)

1. **新建Java类**：在项目中创建一个新的Java类，例如`IndexServlet`。
2. **继承HttpServlet**：让该类继承`javax.servlet.http.HttpServlet`。
3. **重写方法**：根据需要重写`doGet`、`doPost`等方法。

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_31.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_33.png)

```java
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_35.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_37.png)

public class IndexServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        resp.getWriter().println("Hello from doGet");
    }
}
```

### 配置Servlet路由

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_39.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_41.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_43.png)

创建Servlet后，需要在`web.xml`文件中配置路由，以便通过URL访问Servlet。

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_45.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_47.png)

以下是配置Servlet路由的步骤：

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_49.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_51.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_53.png)

1. **定义Servlet**：指定Servlet的名称和对应的类。
2. **映射URL**：将Servlet映射到具体的URL路径。

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_55.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_57.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_59.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_61.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_63.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_65.png)

```xml
<servlet>
    <servlet-name>IndexServlet</servlet-name>
    <servlet-class>com.example.IndexServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>IndexServlet</servlet-name>
    <url-pattern>/index</url-pattern>
</servlet-mapping>
```

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_67.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_69.png)

### 接收和回写数据

Servlet可以接收来自客户端的请求数据，并将处理结果回写到客户端。以下是接收和回写数据的示例：

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_71.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_73.png)

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws IOException {
    String name = req.getParameter("name");
    resp.setContentType("text/html;charset=UTF-8");
    resp.getWriter().println("接收到的name参数值为：" + name);
}
```

---

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_75.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_77.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_79.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_81.png)

## 🔄 Servlet 生命周期

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_83.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_85.png)

上一节我们介绍了Servlet的创建和配置，本节中我们来看看Servlet的生命周期。

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_87.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_89.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_91.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_93.png)

Servlet的生命周期包括以下几个阶段：

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_95.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_97.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_99.png)

1. **初始化（init）**：Servlet被创建时调用，仅执行一次。
2. **服务（service）**：每次请求时调用，根据请求方法（如GET、POST）分发到`doGet`或`doPost`。
3. **销毁（destroy）**：Servlet被销毁时调用，仅执行一次。

以下是生命周期方法的示例：

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_101.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_103.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_105.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_107.png)

```java
public class LifecycleServlet extends HttpServlet {
    @Override
    public void init() {
        System.out.println("Servlet初始化");
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        resp.getWriter().println("处理GET请求");
    }

    @Override
    public void destroy() {
        System.out.println("Servlet销毁");
    }
}
```

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_109.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_111.png)

---

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_113.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_115.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_117.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_119.png)

## 🗃️ JDBC 数据库操作

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_121.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_123.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_125.png)

上一节我们了解了Servlet的生命周期，本节中我们来看看如何使用JDBC进行数据库操作。

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_127.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_129.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_131.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_133.png)

### JDBC 简介

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_135.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_137.png)

JDBC（Java Database Connectivity）是Java语言中用于连接和操作数据库的标准API。它允许Java程序通过SQL语句与数据库进行交互。

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_139.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_141.png)

### JDBC 操作步骤

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_143.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_145.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_147.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_149.png)

以下是使用JDBC操作数据库的基本步骤：

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_151.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_153.png)

1. **注册数据库驱动**：加载数据库驱动类。
2. **建立数据库连接**：通过URL、用户名和密码连接数据库。
3. **创建SQL语句**：编写需要执行的SQL语句。
4. **执行SQL语句**：通过`Statement`或`PreparedStatement`执行SQL。
5. **处理结果集**：遍历结果集并处理数据。
6. **关闭连接**：释放数据库连接资源。

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_155.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_157.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_159.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_161.png)

以下是JDBC操作数据库的示例代码：

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_163.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_165.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_167.png)

```java
import java.sql.*;

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_169.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_171.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_173.png)

public class JdbcExample {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/demo";
        String user = "root";
        String password = "root";

        try {
            // 1. 注册驱动
            Class.forName("com.mysql.cj.jdbc.Driver");

            // 2. 建立连接
            Connection conn = DriverManager.getConnection(url, user, password);

            // 3. 创建SQL语句
            String sql = "SELECT * FROM news";

            // 4. 执行SQL语句
            Statement stmt = conn.createStatement();
            ResultSet rs = stmt.executeQuery(sql);

            // 5. 处理结果集
            while (rs.next()) {
                int id = rs.getInt("id");
                String title = rs.getString("title");
                System.out.println("ID: " + id + ", Title: " + title);
            }

            // 6. 关闭连接
            rs.close();
            stmt.close();
            conn.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 预编译与SQL注入防护

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_175.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_177.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_179.png)

JDBC支持预编译（PreparedStatement），可以有效防止SQL注入攻击。预编译通过提前固定SQL语句的逻辑，防止用户输入改变原有SQL结构。

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_181.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_183.png)

以下是使用预编译的示例：

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_185.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_187.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_189.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_191.png)

```java
String sql = "SELECT * FROM news WHERE id = ?";
PreparedStatement pstmt = conn.prepareStatement(sql);
pstmt.setInt(1, id); // 设置参数
ResultSet rs = pstmt.executeQuery();
```

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_193.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_195.png)

---

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_197.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_199.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_201.png)

## 🧩 MyBatis 框架简介

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_203.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_205.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_207.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_209.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_211.png)

上一节我们介绍了JDBC的基本操作，本节中我们简要介绍MyBatis框架。

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_213.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_215.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_217.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_219.png)

### MyBatis 概述

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_221.png)

MyBatis是一个优秀的持久层框架，它简化了数据库操作，通过XML或注解配置SQL语句，并将结果映射到Java对象中。

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_223.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_225.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_227.png)

### MyBatis 的优势

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_229.png)

以下是MyBatis的主要优势：

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_231.png)

1. **简化SQL操作**：通过配置文件管理SQL语句，减少代码冗余。
2. **灵活的映射**：支持将数据库结果自动映射到Java对象。
3. **动态SQL**：支持动态生成SQL语句，适应复杂查询需求。

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_233.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_235.png)

### MyBatis 的基本使用

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_237.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_239.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_241.png)

以下是MyBatis的基本使用步骤：

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_243.png)

1. **添加依赖**：在项目中引入MyBatis的依赖。
2. **配置数据源**：在配置文件中设置数据库连接信息。
3. **编写Mapper接口**：定义数据库操作方法。
4. **编写SQL映射文件**：配置SQL语句和结果映射。

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_245.png)

---

## 📚 总结

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_247.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_249.png)

本节课中我们一起学习了以下内容：

1. **Servlet的创建与配置**：包括如何创建Servlet类、配置路由以及接收和回写数据。
2. **Servlet的生命周期**：理解了Servlet的初始化、服务和销毁过程。
3. **JDBC数据库操作**：掌握了使用JDBC连接数据库、执行SQL语句以及处理结果集的方法。
4. **MyBatis框架简介**：初步了解了MyBatis框架的优势和基本使用步骤。

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_251.png)

![](img/d270ca8a0ad4fb982f330c718b2f9ad4_253.png)

通过本节课的学习，你已经掌握了JavaEE应用开发的基础知识，为后续深入学习打下了坚实的基础。下节课我们将进一步探讨MyBatis的详细使用和预编译技术。