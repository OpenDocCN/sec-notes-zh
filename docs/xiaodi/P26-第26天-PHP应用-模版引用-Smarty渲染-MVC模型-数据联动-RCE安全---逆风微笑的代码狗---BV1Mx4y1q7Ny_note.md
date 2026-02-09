# 🧩 P26：PHP模板技术、Smarty渲染与MVC模型安全教程

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_1.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_3.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_5.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_7.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_9.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_11.png)

在本节课中，我们将学习PHP中的模板技术应用、Smarty模板引擎的使用、MVC模型的基本概念，以及由此可能引发的安全漏洞（如RCE）。课程将从简单的新闻列表功能实现开始，逐步深入到模板技术的原理、应用和安全问题。

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_13.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_15.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_17.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_19.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_21.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_23.png)

---

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_25.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_27.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_29.png)

## 📰 新闻列表功能实现

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_31.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_33.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_35.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_37.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_39.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_41.png)

上一节我们介绍了课程的整体目标，本节中我们来看看如何实现一个基础的新闻列表功能。这个功能类似于之前留言板项目的数据查询与展示，核心步骤包括创建数据库表、连接数据库、查询数据并在页面显示。

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_43.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_45.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_47.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_49.png)

以下是实现新闻列表功能的主要步骤：

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_51.png)

1.  **创建数据库表**：在数据库中创建一个存储新闻数据的表。
    ```sql
    CREATE TABLE news (
        id INT(10) NOT NULL AUTO_INCREMENT,
        title VARCHAR(255) NOT NULL,
        content TEXT,
        author VARCHAR(255),
        image VARCHAR(255),
        PRIMARY KEY (id)
    );
    ```

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_53.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_55.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_57.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_59.png)

2.  **插入测试数据**：向`news`表中插入一条示例数据。
    ```sql
    INSERT INTO news (title, content, author, image) VALUES ('小迪安全培训', '培训内容...', '小迪', 'images/xiaodi.png');
    ```

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_61.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_63.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_65.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_67.png)

3.  **编写PHP代码查询并显示**：创建一个PHP文件（如`news.php`）来连接数据库，执行查询并输出结果。
    ```php
    <?php
    include('config.php'); // 包含数据库配置文件
    $id = $_GET['id'] ?? 1; // 获取URL参数，默认为1
    $sql = "SELECT * FROM news WHERE id = $id";
    $result = mysqli_query($conn, $sql);
    $row = mysqli_fetch_assoc($result);

    echo "<h1>" . $row['title'] . "</h1>";
    echo "<p>作者: " . $row['author'] . "</p>";
    echo "<p>" . $row['content'] . "</p>";
    echo "<img src='" . $row['image'] . "' width='300'>";
    ?>
    ```

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_69.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_71.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_73.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_75.png)

通过以上步骤，我们实现了一个基础的新闻展示页面。然而，这种将PHP逻辑与HTML标签混合编写的方式，在需要复杂页面样式时会变得难以维护。

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_77.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_79.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_81.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_83.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_85.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_87.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_89.png)

---

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_91.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_93.png)

## 🎨 模板技术的引入与原理

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_95.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_97.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_99.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_101.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_103.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_105.png)

上一节我们完成了基础的新闻展示，但页面缺乏美观的样式。本节中我们来看看如何使用模板技术将页面显示（视图）与业务逻辑（控制）分离，从而提升开发效率和可维护性。

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_107.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_109.png)

模板技术的核心思想是：**预先编写好带有占位符的HTML页面（模板文件），然后在PHP程序中用实际的数据替换这些占位符，最后输出完整的HTML页面。**

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_111.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_113.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_115.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_117.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_119.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_121.png)

以下是实现一个简单自定义模板的步骤：

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_123.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_125.png)

1.  **创建HTML模板文件**：创建一个`news.html`文件，使用特定标记（如`{$title}`）作为占位符。
    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <title>{$title}</title>
    </head>
    <body>
        <h1>{$title}</h1>
        <p>作者: {$author}</p>
        <div>{$content}</div>
        <img src="{$image}">
    </body>
    </html>
    ```

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_127.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_129.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_131.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_133.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_135.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_137.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_139.png)

2.  **编写PHP渲染逻辑**：在`news.php`中读取模板文件内容，并用数据库查询结果替换占位符。
    ```php
    <?php
    // ... 数据库查询代码，获取 $row 数组 ...
    $template = file_get_contents('news.html'); // 读取模板
    $template = str_replace('{$title}', $row['title'], $template);
    $template = str_replace('{$author}', $row['author'], $template);
    $template = str_replace('{$content}', $row['content'], $template);
    $template = str_replace('{$image}', $row['image'], $template);
    echo $template; // 输出渲染后的页面
    ?>
    ```

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_141.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_143.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_145.png)

**这样做的好处是**：前端设计师可以独立修改`news.html`的样式，而PHP开发者只需关注数据获取和替换逻辑。这种分离正是**MVC（Model-View-Controller）模型**中“视图(View)”与“控制器(Controller)”分离的体现。
*   **Model（模型）**：负责数据操作，本例中的数据库查询。
*   **View（视图）**：负责显示，即我们的`news.html`模板文件。
*   **Controller（控制器）**：负责业务逻辑，即`news.php`中协调数据和视图的部分。

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_147.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_149.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_151.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_153.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_155.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_157.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_159.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_161.png)

在实际项目（如Z-Blog等开源程序）中，模板文件通常存放在`/template/`或`/theme/`目录下，通过包含和替换机制来渲染不同页面。

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_163.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_165.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_167.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_169.png)

---

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_171.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_173.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_175.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_177.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_179.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_181.png)

## ⚠️ 自定义模板的安全风险（RCE）

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_183.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_185.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_187.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_189.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_191.png)

上一节我们了解了模板技术带来的便利，本节中我们来看看如果实现不当会引入怎样的安全风险。**自定义模板最大的风险在于可能造成远程代码执行（Remote Code Execution, RCE）。**

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_193.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_195.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_197.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_199.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_201.png)

风险产生的原因在于：**我们使用了`str_replace`等函数将用户可控的数据直接替换到模板中，如果这些数据包含PHP代码，并且模板文件被当作PHP代码解析执行，就会导致代码执行。**

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_203.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_205.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_207.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_209.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_211.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_213.png)

以下是两种危险场景：

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_215.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_217.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_219.png)

1.  **风险一：数据库数据中包含PHP代码**
    如果攻击者能够向`news`表的`title`或`content`字段插入PHP代码（例如通过后台发布文章），当这些数据被替换到模板并输出时，代码将被执行。
    ```php
    // 假设 $row['title'] 被恶意设置为：小迪安全培训<?php phpinfo();?>
    $template = str_replace('{$title}', $row['title'], $template);
    // 最终 $template 内容包含：<h1>小迪安全培训<?php phpinfo();?></h1>
    // 当该字符串被输出时，<?php phpinfo();?> 会被服务器解析执行。
    ```

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_221.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_223.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_225.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_227.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_229.png)

2.  **风险二：模板文件本身被写入PHP代码**
    如果攻击者有权修改模板文件（例如通过后台模板编辑功能），他可以直接在`news.html`中写入PHP代码。
    ```html
    <!-- news.html 被修改 -->
    <h1>{$title}</h1>
    <?php system($_GET['cmd']); ?> <!-- 恶意代码 -->
    ```
    当`news.php`加载并输出此模板时，其中的PHP代码同样会被执行。

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_231.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_233.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_235.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_237.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_239.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_241.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_243.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_245.png)

**漏洞核心**：自定义模板引擎通常使用`eval()`或类似机制来解析包含PHP代码的模板字符串，或者直接将模板文件包含（`include`）进来。如果替换的内容或模板文件本身用户可控，且未经过滤，就会导致RCE。

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_247.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_249.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_251.png)

---

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_253.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_255.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_257.png)

## 🛡️ 第三方模板引擎（Smarty）及其安全

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_259.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_261.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_263.png)

上一节我们看到了自定义模板的风险，本节中我们来看看如何使用更安全、更强大的第三方模板引擎——**Smarty**。Smarty是一个流行的PHP模板引擎，它提供了丰富的功能，并且默认情况下会对输出进行转义，提升了安全性。

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_265.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_267.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_269.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_270.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_272.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_274.png)

以下是使用Smarty的基本步骤：

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_276.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_278.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_280.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_282.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_284.png)

1.  **下载并引入Smarty**：将Smarty库文件放入项目目录（例如`smarty/`）。
2.  **创建PHP控制器文件**：
    ```php
    <?php
    require_once('smarty/libs/Smarty.class.php');
    $smarty = new Smarty();
    $smarty->setTemplateDir('templates/');
    $smarty->setCompileDir('templates_c/');

    $smarty->assign('title', '欢迎使用Smarty');
    $smarty->assign('content', '这是通过Smarty渲染的内容。');
    $smarty->display('index.tpl');
    ?>
    ```
3.  **创建Smarty模板文件**：在`templates/`目录下创建`index.tpl`，使用Smarty语法`{$variable}`。
    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <title>{$title}</title>
    </head>
    <body>
        <h1>{$title}</h1>
        <p>{$content}</p>
        <!-- 尝试插入PHP代码不会被执行 -->
        <?php phpinfo(); ?>
    </body>
    </html>
    ```

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_286.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_288.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_290.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_292.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_294.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_296.png)

**Smarty的安全性提升**：在默认配置下，Smarty会将`{$variable}`中的HTML特殊字符进行转义，并且模板文件（`.tpl`）中的原生PHP代码**不会**被服务器执行。这有效防止了上一节提到的直接RCE漏洞。

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_298.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_300.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_302.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_304.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_306.png)

---

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_308.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_310.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_312.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_314.png)

## 🔓 Smarty的历史漏洞与启示

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_316.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_318.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_320.png)

上一节我们提到Smarty提升了安全性，但本节中我们必须认识到：**第三方组件本身也可能存在漏洞**。安全是相对的，依赖于具体版本和配置。

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_322.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_324.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_326.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_328.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_330.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_332.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_334.png)

例如，Smarty 3.1.42之前版本中存在一个代码执行漏洞（CVE-2021-26119）。在特定配置下，攻击者可能通过构造特殊的模板内容或参数实现RCE。我们使用有漏洞的旧版本（如3.1.31）进行演示：

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_336.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_338.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_340.png)

```php
// 使用存在漏洞的Smarty版本
$smarty->assign('x', $_GET['x']);
$smarty->display('exploit.tpl');
```
如果模板`exploit.tpl`内容为`{php}echo `id`;{/php}`，且在旧版本中未禁用`{php}`标签，则可能执行系统命令。

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_342.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_344.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_346.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_348.png)

**这个案例给我们至关重要的启示**：
1.  **安全是一个整体**：即使自身代码严谨，所依赖的**第三方组件（框架、模板引擎、插件、库）** 若存在已知漏洞，整个应用也会面临威胁。
2.  **代码审计需全面**：在进行安全审计时，不仅要审查自定义的业务逻辑代码，还必须识别和检查所有引用的第三方组件的**名称和版本**，并排查是否有公开漏洞。
3.  **持续更新与维护**：保持所有组件更新到安全版本是至关重要的安全实践。

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_350.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_352.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_354.png)

在PHP中，常见的风险点包括：
*   **自身代码漏洞**：业务逻辑缺陷、SQL注入、XSS等。
*   **框架漏洞**：如ThinkPHP、Laravel的历史RCE漏洞。
*   **模板引擎漏洞**：如Smarty、Twig的历史漏洞。
*   **组件/插件漏洞**：如图像处理库、PDF生成库、编辑器的漏洞。

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_356.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_358.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_359.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_361.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_363.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_365.png)

---

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_367.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_369.png)

## 📝 课程总结

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_371.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_373.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_375.png)

本节课中我们一起学习了PHP开发中的几个核心概念：

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_377.png)

1.  **新闻列表功能**：通过数据库查询与PHP结合，实现基础的数据展示功能。
2.  **模板技术**：为了分离逻辑与表现层，提高代码可维护性，引入了模板技术。我们演示了如何通过简单的字符串替换实现自定义模板。
3.  **MVC模型**：模板技术是MVC架构中“视图”与“控制器”分离的实践。
4.  **安全风险**：**自定义模板如果实现不当，极易因未过滤的用户输入导致远程代码执行（RCE）漏洞。**
5.  **Smarty模板引擎**：使用成熟的第三方模板引擎可以提升开发效率并增强默认安全性（如输出转义）。
6.  **第三方组件安全**：**即使使用第三方组件，也必须关注其安全公告和版本更新**，因为组件本身也可能存在漏洞，从而危及整个应用。

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_379.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_381.png)

![](img/e55b0bbc28b9a8e0f34f1e454ceab4d1_383.png)

最终，我们建立起一个重要的安全观念：现代Web应用的安全是“木桶效应”，需要关注**自身代码**、**框架**、**模板引擎**、**各类组件/插件**等多个层面，任何一处的短板都可能导致严重的安全问题。