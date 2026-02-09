# 🚀 课程27：ThinkPHP框架开发与安全分析

![](img/ae2424707d1e4df58a1774928b257bca_1.png)

![](img/ae2424707d1e4df58a1774928b257bca_3.png)

![](img/ae2424707d1e4df58a1774928b257bca_5.png)

![](img/ae2424707d1e4df58a1774928b257bca_7.png)

![](img/ae2424707d1e4df58a1774928b257bca_9.png)

![](img/ae2424707d1e4df58a1774928b257bca_11.png)

在本节课中，我们将学习ThinkPHP（TP）框架的基本使用及其相关的安全问题。我们将从框架的概念入手，逐步了解其路由访问、数据库操作、文件上传等核心功能，并分析其内置的安全机制以及可能存在的安全风险。

![](img/ae2424707d1e4df58a1774928b257bca_13.png)

![](img/ae2424707d1e4df58a1774928b257bca_15.png)

![](img/ae2424707d1e4df58a1774928b257bca_17.png)

![](img/ae2424707d1e4df58a1774928b257bca_19.png)

---

![](img/ae2424707d1e4df58a1774928b257bca_21.png)

![](img/ae2424707d1e4df58a1774928b257bca_23.png)

![](img/ae2424707d1e4df58a1774928b257bca_25.png)

![](img/ae2424707d1e4df58a1774928b257bca_27.png)

## 📦 什么是ThinkPHP框架？

![](img/ae2424707d1e4df58a1774928b257bca_29.png)

![](img/ae2424707d1e4df58a1774928b257bca_31.png)

![](img/ae2424707d1e4df58a1774928b257bca_33.png)

![](img/ae2424707d1e4df58a1774928b257bca_35.png)

ThinkPHP框架是一个封装好的PHP开发工具。它简化了Web应用的开发流程，例如数据库操作，开发者只需下载框架并调用其封装好的方法即可实现功能，无需像原生开发那样从零开始配置和编写大量代码。

![](img/ae2424707d1e4df58a1774928b257bca_37.png)

![](img/ae2424707d1e4df58a1774928b257bca_39.png)

![](img/ae2424707d1e4df58a1774928b257bca_41.png)

![](img/ae2424707d1e4df58a1774928b257bca_43.png)

![](img/ae2424707d1e4df58a1774928b257bca_45.png)

**核心概念**：框架可以理解为将复杂功能封装成模块的工具，开发者使用时直接调用这些模块即可。

![](img/ae2424707d1e4df58a1774928b257bca_47.png)

![](img/ae2424707d1e4df58a1774928b257bca_49.png)

![](img/ae2424707d1e4df58a1774928b257bca_50.png)

![](img/ae2424707d1e4df58a1774928b257bca_52.png)

![](img/ae2424707d1e4df58a1774928b257bca_54.png)

![](img/ae2424707d1e4df58a1774928b257bca_56.png)

![](img/ae2424707d1e4df58a1774928b257bca_58.png)

![](img/ae2424707d1e4df58a1774928b257bca_60.png)

![](img/ae2424707d1e4df58a1774928b257bca_62.png)

---

![](img/ae2424707d1e4df58a1774928b257bca_64.png)

![](img/ae2424707d1e4df58a1774928b257bca_66.png)

![](img/ae2424707d1e4df58a1774928b257bca_68.png)

![](img/ae2424707d1e4df58a1774928b257bca_70.png)

## 🛠️ 为什么要学习ThinkPHP框架？

![](img/ae2424707d1e4df58a1774928b257bca_72.png)

![](img/ae2424707d1e4df58a1774928b257bca_74.png)

![](img/ae2424707d1e4df58a1774928b257bca_76.png)

![](img/ae2424707d1e4df58a1774928b257bca_78.png)

![](img/ae2424707d1e4df58a1774928b257bca_80.png)

![](img/ae2424707d1e4df58a1774928b257bca_82.png)

![](img/ae2424707d1e4df58a1774928b257bca_84.png)

如果采用ThinkPHP框架开发Web应用，就必须遵循其特定的开发流程。这会导致许多方面（如路由访问）与我们之前学习的原生开发存在差异。因此，了解框架的使用方式和安全特性，对于分析和保障Web应用安全至关重要。

![](img/ae2424707d1e4df58a1774928b257bca_86.png)

---

![](img/ae2424707d1e4df58a1774928b257bca_88.png)

![](img/ae2424707d1e4df58a1774928b257bca_90.png)

## 🗂️ ThinkPHP框架的安装与目录结构

![](img/ae2424707d1e4df58a1774928b257bca_92.png)

![](img/ae2424707d1e4df58a1774928b257bca_94.png)

![](img/ae2424707d1e4df58a1774928b257bca_96.png)

ThinkPHP目前有多个版本，其中5.x版本使用较为广泛。以下是基本的安装和配置步骤：

![](img/ae2424707d1e4df58a1774928b257bca_98.png)

![](img/ae2424707d1e4df58a1774928b257bca_100.png)

![](img/ae2424707d1e4df58a1774928b257bca_102.png)

![](img/ae2424707d1e4df58a1774928b257bca_104.png)

![](img/ae2424707d1e4df58a1774928b257bca_106.png)

1.  **下载框架源码**：从官网或指定位置获取ThinkPHP 5.x版本的源码。
2.  **配置Web服务器**：将网站根目录指向源码中的 `public` 文件夹。
3.  **访问入口文件**：默认的入口文件是 `public/index.php`，它定义了应用目录并引导请求。

![](img/ae2424707d1e4df58a1774928b257bca_108.png)

![](img/ae2424707d1e4df58a1774928b257bca_110.png)

![](img/ae2424707d1e4df58a1774928b257bca_112.png)

![](img/ae2424707d1e4df58a1774928b257bca_114.png)

![](img/ae2424707d1e4df58a1774928b257bca_116.png)

![](img/ae2424707d1e4df58a1774928b257bca_118.png)

访问配置好的地址后，若看到默认页面，则说明框架安装成功。

![](img/ae2424707d1e4df58a1774928b257bca_120.png)

![](img/ae2424707d1e4df58a1774928b257bca_122.png)

---

![](img/ae2424707d1e4df58a1774928b257bca_124.png)

![](img/ae2424707d1e4df58a1774928b257bca_126.png)

![](img/ae2424707d1e4df58a1774928b257bca_128.png)

![](img/ae2424707d1e4df58a1774928b257bca_130.png)

![](img/ae2424707d1e4df58a1774928b257bca_132.png)

![](img/ae2424707d1e4df58a1774928b257bca_134.png)

![](img/ae2424707d1e4df58a1774928b257bca_136.png)

## 🔗 路由访问与控制器

![](img/ae2424707d1e4df58a1774928b257bca_138.png)

![](img/ae2424707d1e4df58a1774928b257bca_140.png)

![](img/ae2424707d1e4df58a1774928b257bca_142.png)

ThinkPHP采用MVC（模型-视图-控制器）架构。核心的业务逻辑代码写在控制器（Controller）中。

![](img/ae2424707d1e4df58a1774928b257bca_144.png)

![](img/ae2424707d1e4df58a1774928b257bca_146.png)

![](img/ae2424707d1e4df58a1774928b257bca_148.png)

![](img/ae2424707d1e4df58a1774928b257bca_150.png)

### 路由访问规则

![](img/ae2424707d1e4df58a1774928b257bca_152.png)

![](img/ae2424707d1e4df58a1774928b257bca_154.png)

![](img/ae2424707d1e4df58a1774928b257bca_156.png)

![](img/ae2424707d1e4df58a1774928b257bca_158.png)

访问URL遵循固定的模式：`模块/控制器/操作/参数`。

![](img/ae2424707d1e4df58a1774928b257bca_160.png)

![](img/ae2424707d1e4df58a1774928b257bca_162.png)

![](img/ae2424707d1e4df58a1774928b257bca_164.png)

![](img/ae2424707d1e4df58a1774928b257bca_166.png)

![](img/ae2424707d1e4df58a1774928b257bca_168.png)

![](img/ae2424707d1e4df58a1774928b257bca_170.png)

![](img/ae2424707d1e4df58a1774928b257bca_172.png)

![](img/ae2424707d1e4df58a1774928b257bca_174.png)

*   **模块**：对应 `application` 目录下的子目录（如 `index`）。
*   **控制器**：对应模块下 `controller` 目录中的PHP文件（如 `Index.php`）。
*   **操作**：对应控制器文件中的方法名（如 `index`）。
*   **参数**：传递给操作的变量。

![](img/ae2424707d1e4df58a1774928b257bca_176.png)

![](img/ae2424707d1e4df58a1774928b257bca_178.png)

![](img/ae2424707d1e4df58a1774928b257bca_180.png)

![](img/ae2424707d1e4df58a1774928b257bca_182.png)

![](img/ae2424707d1e4df58a1774928b257bca_184.png)

![](img/ae2424707d1e4df58a1774928b257bca_186.png)

![](img/ae2424707d1e4df58a1774928b257bca_188.png)

**示例**：
假设有一个控制器文件 `application/index/controller/Index.php`，其中有一个方法：
```php
public function xiaodi()
{
    return 'Hello Xiaodi';
}
```
那么访问它的URL为：`http://your-site/index.php/index/index/xiaodi`

![](img/ae2424707d1e4df58a1774928b257bca_190.png)

![](img/ae2424707d1e4df58a1774928b257bca_192.png)

![](img/ae2424707d1e4df58a1774928b257bca_194.png)

![](img/ae2424707d1e4df58a1774928b257bca_196.png)

![](img/ae2424707d1e4df58a1774928b257bca_198.png)

![](img/ae2424707d1e4df58a1774928b257bca_200.png)

![](img/ae2424707d1e4df58a1774928b257bca_202.png)

### 请求参数接收

![](img/ae2424707d1e4df58a1774928b257bca_204.png)

ThinkPHP推荐使用其内置的 `Request` 对象来接收参数，这比原生的 `$_GET` 或 `$_POST` 更安全、统一。

![](img/ae2424707d1e4df58a1774928b257bca_206.png)

![](img/ae2424707d1e4df58a1774928b257bca_208.png)

![](img/ae2424707d1e4df58a1774928b257bca_210.png)

![](img/ae2424707d1e4df58a1774928b257bca_212.png)

![](img/ae2424707d1e4df58a1774928b257bca_214.png)

![](img/ae2424707d1e4df58a1774928b257bca_216.png)

![](img/ae2424707d1e4df58a1774928b257bca_218.png)

![](img/ae2424707d1e4df58a1774928b257bca_220.png)

![](img/ae2424707d1e4df58a1774928b257bca_222.png)

**示例代码**：
```php
use think\Request;
public function show()
{
    $request = Request::instance();
    $name = $request->param('name');
    return $您提交的名字是：' . $name;
}
```
访问URL：`http://your-site/index.php/index/index/show/name/逆风微笑`

![](img/ae2424707d1e4df58a1774928b257bca_224.png)

![](img/ae2424707d1e4df58a1774928b257bca_226.png)

![](img/ae2424707d1e4df58a1774928b257bca_228.png)

![](img/ae2424707d1e4df58a1774928b257bca_230.png)

![](img/ae2424707d1e4df58a1774928b257bca_232.png)

![](img/ae2424707d1e4df58a1774928b257bca_234.png)

---

![](img/ae2424707d1e4df58a1774928b257bca_236.png)

![](img/ae2424707d1e4df58a1774928b257bca_238.png)

![](img/ae2424707d1e4df58a1774928b257bca_240.png)

![](img/ae2424707d1e4df58a1774928b257bca_242.png)

![](img/ae2424707d1e4df58a1774928b257bca_244.png)

![](img/ae2424707d1e4df58a1774928b257bca_246.png)

## 💾 数据库操作

ThinkPHP极大地简化了数据库操作。首先需要在 `application/database.php` 文件中配置数据库连接信息。

![](img/ae2424707d1e4df58a1774928b257bca_248.png)

![](img/ae2424707d1e4df58a1774928b257bca_250.png)

![](img/ae2424707d1e4df58a1774928b257bca_252.png)

![](img/ae2424707d1e4df58a1774928b257bca_254.png)

![](img/ae2424707d1e4df58a1774928b257bca_256.png)

### 安全查询写法（推荐）

![](img/ae2424707d1e4df58a1774928b257bca_258.png)

![](img/ae2424707d1e4df58a1774928b257bca_260.png)

![](img/ae2424707d1e4df58a1774928b257bca_262.png)

![](img/ae2424707d1e4df58a1774928b257bca_264.png)

![](img/ae2424707d1e4df58a1774928b257bca_266.png)

![](img/ae2424707d1e4df58a1774928b257bca_268.png)

![](img/ae2424707d1e4df58a1774928b257bca_270.png)

![](img/ae2424707d1e4df58a1774928b257bca_272.png)

使用框架内置的查询构造器，可以自动进行安全过滤，有效防止SQL注入。

![](img/ae2424707d1e4df58a1774928b257bca_273.png)

![](img/ae2424707d1e4df58a1774928b257bca_274.png)

![](img/ae2424707d1e4df58a1774928b257bca_276.png)

![](img/ae2424707d1e4df58a1774928b257bca_278.png)

![](img/ae2424707d1e4df58a1774928b257bca_280.png)

![](img/ae2424707d1e4df58a1774928b257bca_282.png)

**示例代码（查询）**：
```php
use think\Db;
public function getNews()
{
    // 等价于 SQL: SELECT * FROM news WHERE id = 1
    $data = Db::name('news')->where('id', 1)->find();
    return json($data);
}
```
**示例代码（插入、更新、删除）**：
```php
// 插入
Db::name('user')->insert(['name' => 'test', 'email' => 'test@example.com']);
// 更新
Db::name('user')->where('id', 1)->update(['name' => 'newName']);
// 删除
Db::name('user')->where('id', 1)->delete();
```

![](img/ae2424707d1e4df58a1774928b257bca_284.png)

![](img/ae2424707d1e4df58a1774928b257bca_286.png)

![](img/ae2424707d1e4df58a1774928b257bca_288.png)

![](img/ae2424707d1e4df58a1774928b257bca_290.png)

### 原生查询写法（存在风险）

![](img/ae2424707d1e4df58a1774928b257bca_292.png)

![](img/ae2424707d1e4df58a1774928b257bca_294.png)

![](img/ae2424707d1e4df58a1774928b257bca_296.png)

如果直接使用原生SQL语句，则会绕过框架的内置过滤，存在SQL注入风险。

![](img/ae2424707d1e4df58a1774928b257bca_298.png)

![](img/ae2424707d1e4df58a1774928b257bca_300.png)

![](img/ae2424707d1e4df58a1774928b257bca_302.png)

![](img/ae2424707d1e4df58a1774928b257bca_304.png)

**存在风险的示例代码**：
```php
public function unsafeQuery()
{
    $id = input('id'); // 接收用户输入
    // 直接拼接SQL，存在注入风险！
    $data = Db::query("SELECT * FROM news WHERE id = " . $id);
    return json($data);
}
```

![](img/ae2424707d1e4df58a1774928b257bca_306.png)

![](img/ae2424707d1e4df58a1774928b257bca_308.png)

**实验对比**：
*   使用**安全写法**时，即使传入 `id=1 and 1=2` 这样的参数，查询也会被过滤，不会产生语法错误或异常结果。
*   使用**原生写法**时，传入恶意参数可能导致SQL语句执行错误或产生非预期结果，从而证实漏洞存在。

![](img/ae2424707d1e4df58a1774928b257bca_310.png)

![](img/ae2424707d1e4df58a1774928b257bca_312.png)

---

![](img/ae2424707d1e4df58a1774928b257bca_314.png)

![](img/ae2424707d1e4df58a1774928b257bca_316.png)

![](img/ae2424707d1e4df58a1774928b257bca_318.png)

![](img/ae2424707d1e4df58a1774928b257bca_320.png)

## 📤 文件上传操作

![](img/ae2424707d1e4df58a1774928b257bca_322.png)

![](img/ae2424707d1e4df58a1774928b257bca_324.png)

![](img/ae2424707d1e4df58a1774928b257bca_326.png)

![](img/ae2424707d1e4df58a1774928b257bca_328.png)

![](img/ae2424707d1e4df58a1774928b257bca_330.png)

![](img/ae2424707d1e4df58a1774928b257bca_331.png)

![](img/ae2424707d1e4df58a1774928b257bca_333.png)

ThinkPHP也封装了安全的文件上传处理功能。

![](img/ae2424707d1e4df58a1774928b257bca_335.png)

![](img/ae2424707d1e4df58a1774928b257bca_337.png)

![](img/ae2424707d1e4df58a1774928b257bca_339.png)

![](img/ae2424707d1e4df58a1774928b257bca_341.png)

![](img/ae2424707d1e4df58a1774928b257bca_343.png)

**示例代码**：
```php
public function upload()
{
    $file = request()->file('image');
    if($file){
        // 移动到框架应用根目录的uploads目录下，并自动重命名
        $info = $file->move('./uploads');
        if($info){
            return '上传成功，文件名：' . $info->getFilename();
        }else{
            return $file->getError();
        }
    }
}
```
通过内置的方法，可以方便地进行文件类型、大小等验证，增强了安全性。

![](img/ae2424707d1e4df58a1774928b257bca_345.png)

![](img/ae2424707d1e4df58a1774928b257bca_347.png)

![](img/ae2424707d1e4df58a1774928b257bca_349.png)

![](img/ae2424707d1e4df58a1774928b257bca_351.png)

![](img/ae2424707d1e4df58a1774928b257bca_353.png)

![](img/ae2424707d1e4df58a1774928b257bca_355.png)

![](img/ae2424707d1e4df58a1774928b257bca_357.png)

---

![](img/ae2424707d1e4df58a1774928b257bca_359.png)

![](img/ae2424707d1e4df58a1774928b257bca_361.png)

## 🎨 视图渲染

![](img/ae2424707d1e4df58a1774928b257bca_363.png)

![](img/ae2424707d1e4df58a1774928b257bca_365.png)

![](img/ae2424707d1e4df58a1774928b257bca_367.png)

![](img/ae2424707d1e4df58a1774928b257bca_369.png)

![](img/ae2424707d1e4df58a1774928b257bca_371.png)

![](img/ae2424707d1e4df58a1774928b257bca_373.png)

ThinkPHP使用视图（View）来分离前端页面。控制器负责处理数据，视图负责展示。

![](img/ae2424707d1e4df58a1774928b257bca_375.png)

![](img/ae2424707d1e4df58a1774928b257bca_377.png)

![](img/ae2424707d1e4df58a1774928b257bca_379.png)

![](img/ae2424707d1e4df58a1774928b257bca_381.png)

1.  **创建视图文件**：在对应模块的 `view` 目录下，创建与控制器同名的文件夹，再创建与方法名同名的 `.html` 文件。
    *   例如：`application/index/view/index/index.html`
2.  **控制器中渲染视图并传递数据**：
    ```php
    public function index()
    {
        $this->assign('title', 'ThinkPHP教程');
        $this->assign('content', '欢迎学习安全开发');
        return $this->fetch(); // 渲染 view/index/index.html
    }
    ```
3.  **在视图文件 `index.html` 中使用变量**：
    ```html
    <h1>{$title}</h1>
    <p>{$content}</p>
    ```

![](img/ae2424707d1e4df58a1774928b257bca_383.png)

![](img/ae2424707d1e4df58a1774928b257bca_385.png)

![](img/ae2424707d1e4df58a1774928b257bca_387.png)

![](img/ae2424707d1e4df58a1774928b257bca_389.png)

![](img/ae2424707d1e4df58a1774928b257bca_391.png)

![](img/ae2424707d1e4df58a1774928b257bca_393.png)

![](img/ae2424707d1e4df58a1774928b257bca_395.png)

![](img/ae2424707d1e4df58a1774928b257bca_397.png)

![](img/ae2424707d1e4df58a1774928b257bca_399.png)

---

![](img/ae2424707d1e4df58a1774928b257bca_401.png)

![](img/ae2424707d1e4df58a1774928b257bca_403.png)

![](img/ae2424707d1e4df58a1774928b257bca_405.png)

![](img/ae2424707d1e4df58a1774928b257bca_407.png)

## 🔒 ThinkPHP框架安全分析总结

![](img/ae2424707d1e4df58a1774928b257bca_409.png)

![](img/ae2424707d1e4df58a1774928b257bca_411.png)

![](img/ae2424707d1e4df58a1774928b257bca_413.png)

![](img/ae2424707d1e4df58a1774928b257bca_415.png)

![](img/ae2424707d1e4df58a1774928b257bca_417.png)

![](img/ae2424707d1e4df58a1774928b257bca_418.png)

![](img/ae2424707d1e4df58a1774928b257bca_420.png)

![](img/ae2424707d1e4df58a1774928b257bca_422.png)

通过本节课的学习，我们可以得出以下关于ThinkPHP框架安全的核心结论：

![](img/ae2424707d1e4df58a1774928b257bca_424.png)

![](img/ae2424707d1e4df58a1774928b257bca_426.png)

1.  **安全写法与内置过滤**：严格使用ThinkPHP推荐的数据库操作、文件上传等**安全写法**，可以借助其**内置的安全过滤机制**，有效防御常见的SQL注入、文件上传漏洞等。
2.  **不安全的写法**：如果使用了**原生SQL查询**等不规范写法，或者完全绕过框架使用原生PHP函数，则会**失去框架的保护**，引入安全风险。
3.  **框架版本漏洞**：即使完全使用了安全写法，框架**自身的历史版本也可能存在安全漏洞**（如特定版本的SQL注入、代码执行漏洞）。安全与否还需要关注所使用的框架版本是否受已知漏洞影响。
4.  **分析思路**：在分析或审计一个基于ThinkPHP开发的应用时，应：
    *   识别其使用的框架及版本。
    *   检查关键操作（如数据库查询、文件上传）是否使用了框架推荐的**安全写法**。
    *   若使用了安全写法，则需**核对当前版本是否存在公开的绕过漏洞**。
    *   若使用了不安全的写法（如原生SQL拼接），则直接存在安全隐患。

![](img/ae2424707d1e4df58a1774928b257bca_428.png)

![](img/ae2424707d1e4df58a1774928b257bca_429.png)

![](img/ae2424707d1e4df58a1774928b257bca_431.png)

---

![](img/ae2424707d1e4df58a1774928b257bca_433.png)

![](img/ae2424707d1e4df58a1774928b257bca_435.png)

![](img/ae2424707d1e4df58a1774928b257bca_437.png)

![](img/ae2424707d1e4df58a1774928b257bca_438.png)

本节课中，我们一起学习了ThinkPHP框架的基本开发流程、核心的MVC操作，并重点剖析了其安全机制的双重性：既提供了强大的内置防护，其自身也可能因版本问题存在漏洞。理解这些，是进行PHP框架应用安全测试和代码审计的重要基础。