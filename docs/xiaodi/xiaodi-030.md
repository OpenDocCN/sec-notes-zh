#  030：Node.js应用开发与安全基础

![](img/a55db4f1f30eb046ad44ad00bbe619f7_1.png)

在本节课中，我们将学习Node.js的基础知识，了解其作为服务端JavaScript的运行环境，并实践如何用它进行Web应用开发。我们还将探讨在开发过程中可能遇到的安全问题，如SQL注入、文件操作漏洞、命令执行以及Node.js特有的原型链污染漏洞。

![](img/a55db4f1f30eb046ad44ad00bbe619f7_3.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_5.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_7.png)

---

![](img/a55db4f1f30eb046ad44ad00bbe619f7_9.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_11.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_13.png)

## 📚 概述：Node.js是什么？

![](img/a55db4f1f30eb046ad44ad00bbe619f7_15.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_17.png)

Node.js是运行在服务端的JavaScript环境。这与我们之前学习的、运行在浏览器客户端（前端）的原生JavaScript有本质区别。

![](img/a55db4f1f30eb046ad44ad00bbe619f7_19.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_21.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_23.png)

**核心差异**：
*   **原生JavaScript**：代码在用户浏览器中执行，用户可以通过查看网页源代码看到所有逻辑。
*   **Node.js**：代码在服务器上执行，用户访问网站时，浏览器接收到的只是代码执行后的结果（如HTML），而看不到服务器端的源代码。

![](img/a55db4f1f30eb046ad44ad00bbe619f7_25.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_27.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_29.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_31.png)

这种差异意味着Node.js开发的网站属于“后端”应用。

![](img/a55db4f1f30eb046ad44ad00bbe619f7_33.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_35.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_37.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_39.png)

---

![](img/a55db4f1f30eb046ad44ad00bbe619f7_41.png)

## 🛠️ 环境搭建与基础开发

![](img/a55db4f1f30eb046ad44ad00bbe619f7_43.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_45.png)

上一节我们介绍了Node.js的基本概念，本节中我们来看看如何搭建环境并进行简单的Web开发。

### 环境安装

首先，需要从Node.js官网下载并安装适合你操作系统的版本。安装完成后，重启电脑以确保环境变量生效。

验证安装：
```bash
node -v
npm -v
```

![](img/a55db4f1f30eb046ad44ad00bbe619f7_47.png)

### 使用Express框架创建Web应用

![](img/a55db4f1f30eb046ad44ad00bbe619f7_49.png)

Express是Node.js中一个简洁的Web应用开发框架。以下是创建一个简单服务器的步骤：

![](img/a55db4f1f30eb046ad44ad00bbe619f7_51.png)

1.  **初始化项目并安装Express**：
    ```bash
    npm init -y
    npm install express
    ```

2.  **创建基础服务器** (`server.js`)：
    ```javascript
    // 1. 引入express库
    const express = require('express');
    // 2. 创建应用实例
    const app = express();
    // 3. 定义路由：当用户访问网站根路径时，返回“首页页面”
    app.get('/', (req, res) => {
        res.send('首页页面');
    });
    // 4. 监听3000端口
    const server = app.listen(3000, () => {
        console.log('Web服务器 3000端口已启动');
    });
    ```

3.  **运行服务器**：
    ```bash
    node server.js
    ```
    访问 `http://localhost:3000` 即可看到“首页页面”。

![](img/a55db4f1f30eb046ad44ad00bbe619f7_53.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_55.png)

---

![](img/a55db4f1f30eb046ad44ad00bbe619f7_57.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_59.png)

## 🔐 实现登录功能与数据接收

![](img/a55db4f1f30eb046ad44ad00bbe619f7_61.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_63.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_65.png)

现在，我们来构建一个具有登录功能的页面，并学习如何接收前端提交的数据。

![](img/a55db4f1f30eb046ad44ad00bbe619f7_67.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_69.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_71.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_73.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_75.png)

### 创建登录页面

![](img/a55db4f1f30eb046ad44ad00bbe619f7_77.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_79.png)

首先，创建一个简单的HTML登录表单 (`login.html`)：
```html
<form action="/login" method="post">
    用户名: <input type="text" name="username"><br>
    密码: <input type="password" name="password"><br>
    <input type="submit" value="登录">
</form>
```

![](img/a55db4f1f30eb046ad44ad00bbe619f7_81.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_83.png)

### 处理GET与POST请求

![](img/a55db4f1f30eb046ad44ad00bbe619f7_85.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_87.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_89.png)

在Node.js中，处理不同HTTP方法的请求数据方式不同。

![](img/a55db4f1f30eb046ad44ad00bbe619f7_91.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_93.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_95.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_97.png)

*   **处理GET请求参数**：使用 `req.query` 对象。
*   **处理POST请求参数**：需要借助 `body-parser` 中间件来解析 `req.body`。

![](img/a55db4f1f30eb046ad44ad00bbe619f7_99.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_101.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_103.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_105.png)

以下是处理登录逻辑的代码示例：

![](img/a55db4f1f30eb046ad44ad00bbe619f7_107.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_109.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_111.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_113.png)

```javascript
const express = require('express');
const bodyParser = require('body-parser'); // 引入body-parser库
const app = express();

![](img/a55db4f1f30eb046ad44ad00bbe619f7_115.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_117.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_119.png)

// 使用body-parser中间件来解析POST请求的`application/x-www-form-urlencoded`数据
app.use(bodyParser.urlencoded({ extended: true }));

![](img/a55db4f1f30eb046ad44ad00bbe619f7_121.png)

// 渲染登录页面 (GET请求)
app.get('/login', (req, res) => {
    res.sendFile(__dirname + '/login.html');
});

// 处理登录提交 (POST请求)
app.post('/login', (req, res) => {
    // 从req.body中获取POST提交的数据
    const username = req.body.username;
    const password = req.body.password;
    
    console.log(`接收到用户名: ${username}, 密码: ${password}`);
    
    // 简单的账号密码验证逻辑
    if (username === 'admin' && password === '123456') {
        res.send('欢迎进入后台管理页面');
    } else {
        res.send('用户名或密码错误');
    }
});

app.listen(3000);
```

![](img/a55db4f1f30eb046ad44ad00bbe619f7_123.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_125.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_127.png)

**关键点**：
*   `req.query` 用于获取URL中的查询参数（如 `?id=1`）。
*   `req.body` 用于获取POST请求体中的数据，但需要 `body-parser` 中间件先行解析。

![](img/a55db4f1f30eb046ad44ad00bbe619f7_129.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_131.png)

---

![](img/a55db4f1f30eb046ad44ad00bbe619f7_133.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_135.png)

## 🗄️ 连接数据库与SQL注入风险

![](img/a55db4f1f30eb046ad44ad00bbe619f7_137.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_139.png)

在实际应用中，用户数据通常存储在数据库中。Node.js可以方便地连接MySQL等数据库。

![](img/a55db4f1f30eb046ad44ad00bbe619f7_141.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_143.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_145.png)

### 连接与查询MySQL

![](img/a55db4f1f30eb046ad44ad00bbe619f7_147.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_149.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_151.png)

1.  安装MySQL驱动库：
    ```bash
    npm install mysql
    ```
2.  编写数据库操作代码：
    ```javascript
    const mysql = require('mysql');
    
    // 创建数据库连接
    const connection = mysql.createConnection({
        host: 'localhost',
        user: 'root',
        password: 'yourpassword',
        database: 'demo01'
    });
    
    connection.connect();
    
    // 执行SQL查询
    const sql = `SELECT * FROM admin WHERE username='admin'`;
    connection.query(sql, (error, results) => {
        if (error) throw error;
        console.log(results); // 打印查询结果
        // 例如，获取第一行数据的用户名：results[0].username
    });
    
    connection.end();
    ```

![](img/a55db4f1f30eb046ad44ad00bbe619f7_153.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_155.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_157.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_159.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_161.png)

### 不安全的SQL拼接与注入风险

![](img/a55db4f1f30eb046ad44ad00bbe619f7_163.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_165.png)

如果直接将用户输入拼接到SQL语句中，就会产生严重的SQL注入漏洞。

![](img/a55db4f1f30eb046ad44ad00bbe619f7_167.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_168.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_170.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_172.png)

**不安全写法示例**：
```javascript
// 假设用户输入的username来自 req.body.username
const userInput = req.body.username;
const sql = `SELECT * FROM users WHERE username='${userInput}'`; // 直接拼接，危险！
connection.query(sql, (error, results) => {
    // ...
});
```
攻击者可以输入 `admin' OR '1'='1` 作为用户名，从而构造出永真条件，绕过登录验证。

![](img/a55db4f1f30eb046ad44ad00bbe619f7_174.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_176.png)

**安全写法（使用参数化查询/预编译）**：
```javascript
const sql = `SELECT * FROM users WHERE username=?`;
connection.query(sql, [userInput], (error, results) => { // 使用占位符 ?
    // ...
});
```
参数化查询能有效防止SQL注入，因为它将用户输入的数据纯粹当作数据处理，而非SQL代码的一部分。

![](img/a55db4f1f30eb046ad44ad00bbe619f7_178.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_180.png)

---

![](img/a55db4f1f30eb046ad44ad00bbe619f7_182.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_184.png)

## 📁 文件系统操作与目录遍历漏洞

![](img/a55db4f1f30eb046ad44ad00bbe619f7_186.png)

Node.js的 `fs` 模块提供了文件系统操作功能。

### 读取目录内容

```javascript
const fs = require('fs');

fs.readdir('./', (err, files) => {
    if (err) throw err;
    console.log(files); // 打印当前目录下的文件和文件夹列表
});
```

![](img/a55db4f1f30eb046ad44ad00bbe619f7_188.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_190.png)

### 不安全的文件路径拼接

如果将用户可控的参数直接用于文件路径操作，可能导致目录遍历漏洞。

![](img/a55db4f1f30eb046ad44ad00bbe619f7_192.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_194.png)

**漏洞示例**：
```javascript
app.get('/readfile', (req, res) => {
    const filePath = req.query.path; // 用户传入路径，如 `../../etc/passwd`
    fs.readFile(filePath, (err, data) => { // 直接使用，危险！
        if (err) return res.send('文件读取失败');
        res.send(data);
    });
});
```
攻击者可以通过构造 `../` 序列来访问服务器上的敏感文件。

**防护措施**：对用户输入进行严格的路径规范化、白名单过滤，或使用安全的API限制访问范围。

![](img/a55db4f1f30eb046ad44ad00bbe619f7_196.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_198.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_200.png)

---

## ⚡ 命令执行与代码执行漏洞

![](img/a55db4f1f30eb046ad44ad00bbe619f7_202.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_204.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_206.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_208.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_210.png)

Node.js中可以执行系统命令或动态执行JavaScript代码，如果使用不当，会导致严重的安全漏洞。

![](img/a55db4f1f30eb046ad44ad00bbe619f7_212.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_214.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_216.png)

### 命令执行（Command Execution）

![](img/a55db4f1f30eb046ad44ad00bbe619f7_218.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_220.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_221.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_223.png)

使用 `child_process` 模块可以执行系统命令。

![](img/a55db4f1f30eb046ad44ad00bbe619f7_225.png)

```javascript
const { exec } = require('child_process');
const userInput = req.query.cmd; // 用户输入命令

// 危险！直接执行用户输入
exec(userInput, (error, stdout, stderr) => {
    // ...
});
```
如果 `userInput` 是 `rm -rf /` 或反弹Shell的命令，将造成灾难性后果。

### 代码执行（Code Evaluation）

使用 `eval()` 或 `Function` 构造函数可以动态执行字符串形式的JavaScript代码。

```javascript
const userInput = req.query.code; // 用户输入代码，如 `process.exit()` 或 `require('child_process').exec('rm -rf /')`
// 危险！直接执行用户输入的代码
const result = eval(userInput);
```
这相当于赋予了攻击者在服务器上执行任意Node.js代码的能力。

![](img/a55db4f1f30eb046ad44ad00bbe619f7_227.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_229.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_231.png)

**防护核心**：**永远不要**将用户可控的输入直接传递给 `exec`、`eval`、`Function` 等函数。必须使用时，应进行严格的命令/代码白名单过滤。

![](img/a55db4f1f30eb046ad44ad00bbe619f7_233.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_235.png)

---

![](img/a55db4f1f30eb046ad44ad00bbe619f7_237.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_239.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_241.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_243.png)

## ⛓️ Node.js特有漏洞：原型链污染

![](img/a55db4f1f30eb046ad44ad00bbe619f7_245.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_247.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_249.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_251.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_252.png)

原型链污染是JavaScript/Node.js中一种特有的漏洞，常出现在CTF比赛中，实战中条件较为苛刻。

### 原型链基础

![](img/a55db4f1f30eb046ad44ad00bbe619f7_254.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_256.png)

JavaScript中每个对象都有一个内部属性 `__proto__`（原型），指向其构造函数的原型对象。当访问一个对象的属性时，如果对象本身没有，JavaScript会沿着原型链向上查找。

![](img/a55db4f1f30eb046ad44ad00bbe619f7_258.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_260.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_262.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_264.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_266.png)

### 污染原理

![](img/a55db4f1f30eb046ad44ad00bbe619f7_268.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_270.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_272.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_274.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_276.png)

如果攻击者能够控制并修改一个对象的原型（`__proto__`），那么所有继承自该原型的对象都会受到影响。

![](img/a55db4f1f30eb046ad44ad00bbe619f7_278.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_280.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_282.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_284.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_286.png)

**污染示例**：
```javascript
const obj1 = {};
const obj2 = {};

![](img/a55db4f1f30eb046ad44ad00bbe619f7_288.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_290.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_292.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_294.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_296.png)

// 正常情况下，obj2没有foo属性
console.log(obj2.foo); // undefined

![](img/a55db4f1f30eb046ad44ad00bbe619f7_298.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_300.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_302.png)

// 通过obj1的原型链污染全局Object原型
obj1.__proto__.foo = 'polluted value';

![](img/a55db4f1f30eb046ad44ad00bbe619f7_304.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_306.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_308.png)

// 此时，所有对象（包括obj2）都有了foo属性
console.log(obj2.foo); // 输出: polluted value
```

![](img/a55db4f1f30eb046ad44ad00bbe619f7_310.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_312.png)

### 漏洞利用场景

![](img/a55db4f1f30eb046ad44ad00bbe619f7_314.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_316.png)

当程序存在对象合并操作（如 `Object.assign`、`lodash.merge` 等），且合并的参数用户可控时，就可能污染原型链。如果后续代码逻辑依赖于某个对象的属性（例如，判断 `if (obj.isAdmin)`），污染可能导致权限绕过等安全问题。

**关键点**：原型链污染本身不是直接执行命令，而是通过修改程序运行时的对象属性，影响程序逻辑，可能与其他漏洞结合产生更大危害。

---

![](img/a55db4f1f30eb046ad44ad00bbe619f7_318.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_320.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_321.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_323.png)

## 🎯 总结与学习意义

![](img/a55db4f1f30eb046ad44ad00bbe619f7_325.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_327.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_329.png)

本节课我们一起学习了Node.js的核心概念和基础开发。

![](img/a55db4f1f30eb046ad44ad00bbe619f7_331.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_333.png)

![](img/a55db4f1f30eb046ad44ad00bbe619f7_335.png)

**我们重点掌握了**：
1.  Node.js作为服务端JavaScript环境的特点。
2.  使用Express框架搭建Web服务器，处理GET/POST请求。
3.  连接数据库进行查询，并认识到不安全的SQL拼接会导致注入漏洞。
4.  使用 `fs` 模块进行文件操作，了解不安全的路径拼接会导致目录遍历。
5.  认识到 `child_process.exec` 和 `eval` 等函数误用会引发命令/代码执行漏洞。
6.  理解了JavaScript原型链机制和原型链污染漏洞的原理。

![](img/a55db4f1f30eb046ad44ad00bbe619f7_337.png)

**学习Node.js安全开发的意义**：
*   **代码审计**：能够审计使用Node.js编写的Web应用源码，发现潜在安全漏洞。
*   **CTF竞赛**：许多CTF题目涉及Node.js代码审计和原型链污染等知识点。
*   **实战渗透**：互联网上存在大量Node.js应用（如YApi接口管理平台），掌握其安全特性有助于在渗透测试或攻防演练中发现独家漏洞。

![](img/a55db4f1f30eb046ad44ad00bbe619f7_339.png)

通过将开发知识与安全问题结合，我们为后续深入学习Web漏洞原理和代码审计打下了坚实的基础。下节课，我们将探讨前端打包工具（如Webpack）和第三方库的安全问题。