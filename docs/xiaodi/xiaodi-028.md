#  028：JS应用开发与安全验证处理

![](img/30502ce54eee49ebdbc99a336334433f_0.png)

![](img/30502ce54eee49ebdbc99a336334433f_2.png)

![](img/30502ce54eee49ebdbc99a336334433f_4.png)

![](img/30502ce54eee49ebdbc99a336334433f_6.png)

在本节课中，我们将学习JavaScript的原生开发、JQuery库的使用以及Ajax技术。我们将通过构建文件上传、登录验证和商品购买三个功能模块，来理解前端与后端如何协作，并分析其中可能存在的安全验证问题。

![](img/30502ce54eee49ebdbc99a336334433f_8.png)

![](img/30502ce54eee49ebdbc99a336334433f_10.png)

![](img/30502ce54eee49ebdbc99a336334433f_12.png)

![](img/30502ce54eee49ebdbc99a336334433f_14.png)

---

![](img/30502ce54eee49ebdbc99a336334433f_16.png)

![](img/30502ce54eee49ebdbc99a336334433f_18.png)

![](img/30502ce54eee49ebdbc99a336334433f_20.png)

![](img/30502ce54eee49ebdbc99a336334433f_22.png)

## 📁 原生JS开发：文件上传功能

![](img/30502ce54eee49ebdbc99a336334433f_24.png)

![](img/30502ce54eee49ebdbc99a336334433f_26.png)

![](img/30502ce54eee49ebdbc99a336334433f_28.png)

![](img/30502ce54eee49ebdbc99a336334433f_30.png)

上一节我们介绍了课程的整体安排，本节中我们来看看如何使用原生JavaScript开发一个文件上传功能。

这个功能的核心流程分为四步：
1.  编写前端文件上传页面。
2.  使用JavaScript获取提交的文件数据。
3.  对文件数据进行格式（后缀名）判断。
4.  判断成功后，交由后端PHP处理上传数据。

![](img/30502ce54eee49ebdbc99a336334433f_32.png)

![](img/30502ce54eee49ebdbc99a336334433f_34.png)

因此，本项目可以概括为：**前端JS进行文件格式（后缀）过滤，后端PHP进行文件处理**。

![](img/30502ce54eee49ebdbc99a336334433f_36.png)

![](img/30502ce54eee49ebdbc99a336334433f_38.png)

![](img/30502ce54eee49ebdbc99a336334433f_40.png)

### 代码实现步骤

![](img/30502ce54eee49ebdbc99a336334433f_42.png)

![](img/30502ce54eee49ebdbc99a336334433f_44.png)

![](img/30502ce54eee49ebdbc99a336334433f_46.png)

![](img/30502ce54eee49ebdbc99a336334433f_48.png)

![](img/30502ce54eee49ebdbc99a336334433f_50.png)

以下是实现该功能的具体步骤：

![](img/30502ce54eee49ebdbc99a336334433f_52.png)

![](img/30502ce54eee49ebdbc99a336334433f_54.png)

![](img/30502ce54eee49ebdbc99a336334433f_56.png)

![](img/30502ce54eee49ebdbc99a336334433f_58.png)

![](img/30502ce54eee49ebdbc99a336334433f_60.png)

![](img/30502ce54eee49ebdbc99a336334433f_62.png)

![](img/30502ce54eee49ebdbc99a336334433f_64.png)

1.  **创建项目目录与文件**
    首先，创建一个名为 `js` 的目录，并在其中创建两个文件：
    *   `upload.html`：前端上传页面。
    *   `upload.php`：后端处理文件。

![](img/30502ce54eee49ebdbc99a336334433f_66.png)

![](img/30502ce54eee49ebdbc99a336334433f_68.png)

2.  **编写前端页面 (`upload.html`)**
    页面包含一个表单，用于选择并提交文件。
    ```html
    <form action="upload.php" method="post" enctype="multipart/form-data">
        选择文件：<input type="file" name="f" id="fileInput">
        <input type="submit" value="上传">
    </form>
    ```

![](img/30502ce54eee49ebdbc99a336334433f_70.png)

![](img/30502ce54eee49ebdbc99a336334433f_72.png)

![](img/30502ce54eee49ebdbc99a336334433f_74.png)

![](img/30502ce54eee49ebdbc99a336334433f_76.png)

![](img/30502ce54eee49ebdbc99a336334433f_78.png)

![](img/30502ce54eee49ebdbc99a336334433f_80.png)

3.  **添加JS验证逻辑**
    在 `upload.html` 的 `<script>` 标签内，编写JavaScript代码来验证文件后缀。
    *   **定义允许上传的白名单后缀**：
        ```javascript
        var exts = [‘png‘, ‘gif‘, ‘jpg‘];
        ```
    *   **监听文件选择事件**：当用户选择文件时，触发验证函数。
        ```javascript
        document.getElementById(‘fileInput‘).onchange = function() {
            var fileName = this.value; // 获取文件名
            checkFileExt(fileName);
        };
        ```
    *   **实现验证函数**：提取文件后缀，并与白名单对比。
        ```javascript
        function checkFileExt(fileName) {
            // 获取文件后缀（最后一个点之后的部分）
            var ext = fileName.substring(fileName.lastIndexOf(‘.‘) + 1).toLowerCase();
            var flag = false; // 验证状态，初始为false

            // 遍历白名单进行比对
            for(var i=0; i<exts.length; i++) {
                if(exts[i] == ext) {
                    flag = true; // 匹配成功，状态设为true
                    alert(‘文件后缀正确‘);
                    break;
                }
            }

            // 根据验证状态决定后续操作
            if(!flag) {
                alert(‘文件后缀错误‘);
                location.reload(); // 刷新页面
            }
        }
        ```

![](img/30502ce54eee49ebdbc99a336334433f_82.png)

![](img/30502ce54eee49ebdbc99a336334433f_84.png)

![](img/30502ce54eee49ebdbc99a336334433f_86.png)

![](img/30502ce54eee49ebdbc99a336334433f_88.png)

![](img/30502ce54eee49ebdbc99a336334433f_90.png)

![](img/30502ce54eee49ebdbc99a336334433f_92.png)

![](img/30502ce54eee49ebdbc99a336334433f_94.png)

![](img/30502ce54eee49ebdbc99a336334433f_96.png)

![](img/30502ce54eee49ebdbc99a336334433f_98.png)

![](img/30502ce54eee49ebdbc99a336334433f_100.png)

4.  **编写后端处理 (`upload.php`)**
    后端PHP接收文件并保存到服务器。
    ```php
    <?php
    if ($_FILES[‘f‘][‘error‘] == 0) {
        $tmp_name = $_FILES[‘f‘][‘tmp_name‘];
        $name = $_FILES[‘f‘][‘name‘];
        move_uploaded_file($tmp_name, ‘upload/‘ . $name);
        echo ‘<script>alert("上传成功");</script>‘;
    }
    ?>
    ```

![](img/30502ce54eee49ebdbc99a336334433f_102.png)

![](img/30502ce54eee49ebdbc99a336334433f_104.png)

![](img/30502ce54eee49ebdbc99a336334433f_106.png)

![](img/30502ce54eee49ebdbc99a336334433f_108.png)

![](img/30502ce54eee49ebdbc99a336334433f_110.png)

![](img/30502ce54eee49ebdbc99a336334433f_112.png)

![](img/30502ce54eee49ebdbc99a336334433f_114.png)

![](img/30502ce54eee49ebdbc99a336334433f_116.png)

![](img/30502ce54eee49ebdbc99a336334433f_118.png)

![](img/30502ce54eee49ebdbc99a336334433f_120.png)

### 🔍 安全风险分析

![](img/30502ce54eee49ebdbc99a336334433f_122.png)

![](img/30502ce54eee49ebdbc99a336334433f_124.png)

![](img/30502ce54eee49ebdbc99a336334433f_126.png)

![](img/30502ce54eee49ebdbc99a336334433f_128.png)

![](img/30502ce54eee49ebdbc99a336334433f_130.png)

![](img/30502ce54eee49ebdbc99a336334433f_132.png)

![](img/30502ce54eee49ebdbc99a336334433f_134.png)

![](img/30502ce54eee49ebdbc99a336334433f_136.png)

![](img/30502ce54eee49ebdbc99a336334433f_138.png)

这种实现模式存在明显的安全隐患：
1.  **过滤代码可见**：所有验证逻辑（白名单、判断函数）都直接暴露在浏览器的前端代码中。
2.  **验证可被绕过**：攻击者可以通过禁用浏览器JavaScript、删除或修改前端验证代码、或直接伪造HTTP请求包等方式，轻松绕过前端验证，上传任意类型的文件。

![](img/30502ce54eee49ebdbc99a336334433f_140.png)

![](img/30502ce54eee49ebdbc99a336334433f_142.png)

![](img/30502ce54eee49ebdbc99a336334433f_144.png)

**核心问题在于：前端验证仅服务于用户体验和初步筛选，真正的安全校验必须依赖后端不可篡改的逻辑。**

![](img/30502ce54eee49ebdbc99a336334433f_146.png)

![](img/30502ce54eee49ebdbc99a336334433f_148.png)

![](img/30502ce54eee49ebdbc99a336334433f_150.png)

![](img/30502ce54eee49ebdbc99a336334433f_152.png)

![](img/30502ce54eee49ebdbc99a336334433f_154.png)

![](img/30502ce54eee49ebdbc99a336334433f_156.png)

![](img/30502ce54eee49ebdbc99a336334433f_158.png)

---

![](img/30502ce54eee49ebdbc99a336334433f_160.png)

![](img/30502ce54eee49ebdbc99a336334433f_162.png)

![](img/30502ce54eee49ebdbc99a336334433f_164.png)

## 🔐 使用JQuery与Ajax：登录验证功能

![](img/30502ce54eee49ebdbc99a336334433f_166.png)

![](img/30502ce54eee49ebdbc99a336334433f_168.png)

![](img/30502ce54eee49ebdbc99a336334433f_170.png)

![](img/30502ce54eee49ebdbc99a336334433f_172.png)

上一节我们实现了原生JS的文件上传，本节中我们来看看如何使用JQuery库和Ajax技术构建一个登录验证功能。

![](img/30502ce54eee49ebdbc99a336334433f_174.png)

![](img/30502ce54eee49ebdbc99a336334433f_176.png)

![](img/30502ce54eee49ebdbc99a336334433f_178.png)

![](img/30502ce54eee49ebdbc99a336334433f_180.png)

![](img/30502ce54eee49ebdbc99a336334433f_182.png)

![](img/30502ce54eee49ebdbc99a336334433f_184.png)

![](img/30502ce54eee49ebdbc99a336334433f_186.png)

![](img/30502ce54eee49ebdbc99a336334433f_188.png)

本功能的架构设计为：**前端JS（使用JQuery）处理登录事件和发送数据，后端PHP进行账号密码的实质性校验**。

![](img/30502ce54eee49ebdbc99a336334433f_190.png)

![](img/30502ce54eee49ebdbc99a336334433f_192.png)

![](img/30502ce54eee49ebdbc99a336334433f_194.png)

![](img/30502ce54eee49ebdbc99a336334433f_196.png)

![](img/30502ce54eee49ebdbc99a336334433f_198.png)

### 代码实现步骤

![](img/30502ce54eee49ebdbc99a336334433f_200.png)

![](img/30502ce54eee49ebdbc99a336334433f_202.png)

![](img/30502ce54eee49ebdbc99a336334433f_204.png)

![](img/30502ce54eee49ebdbc99a336334433f_206.png)

![](img/30502ce54eee49ebdbc99a336334433f_208.png)

以下是实现登录验证的具体步骤：

![](img/30502ce54eee49ebdbc99a336334433f_210.png)

![](img/30502ce54eee49ebdbc99a336334433f_212.png)

1.  **创建项目文件**
    创建 `login.html`（前端页面）和 `login_check.php`（后端验证）。

![](img/30502ce54eee49ebdbc99a336334433f_214.png)

![](img/30502ce54eee49ebdbc99a336334433f_216.png)

![](img/30502ce54eee49ebdbc99a336334433f_218.png)

2.  **引入JQuery库**
    在 `login.html` 中引入JQuery库文件，这是使用其Ajax等功能的前提。
    ```html
    <script src=“js/jquery-1.12.4.min.js“></script>
    ```

3.  **编写前端登录页面与逻辑 (`login.html`)**
    *   构建一个简单的登录表单（注意，这里去掉了传统的 `<form>` 标签，改为由JS控制提交）。
    *   使用JQuery为登录按钮绑定点击事件。
    *   在事件处理函数中，使用 `$.ajax` 方法将用户名和密码发送给后端。
    ```html
    <input type=“text“ class=“user“ placeholder=“用户名“>
    <input type=“password“ class=“pass“ placeholder=“密码“>
    <button id=“loginBtn“>登录</button>

    <script>
    $(‘#loginBtn‘).click(function(){
        var my_user = $(‘.user‘).val(); // 获取用户名输入框的值
        var my_pass = $(‘.pass‘).val(); // 获取密码输入框的值

        $.ajax({
            type: ‘POST‘,
            url: ‘login_check.php‘,
            data: {‘my_user‘: my_user, ‘my_pass‘: my_pass}, // 发送的数据
            dataType: ‘json‘, // 期望返回的数据类型
            success: function(res){ // 请求成功后的回调函数
                // res 是后端返回的JSON数据
                if(res.info_code == 1){
                    alert(‘登录成功‘);
                    location.href = ‘index.php‘; // 跳转到首页
                } else {
                    alert(‘登录失败‘);
                }
            }
        });
    });
    </script>
    ```

![](img/30502ce54eee49ebdbc99a336334433f_220.png)

4.  **编写后端验证逻辑 (`login_check.php`)**
    后端接收数据，进行验证，并返回一个JSON格式的结果给前端。
    ```php
    <?php
    $user = $_POST[‘my_user‘];
    $pass = $_POST[‘my_pass‘];

    // 模拟数据库验证（实际应从数据库查询）
    if($user == ‘admin‘ && $pass == ‘123456‘){
        $result = array(‘info_code‘=>1, ‘msg‘=>‘ok‘);
    } else {
        $result = array(‘info_code‘=>0, ‘msg‘=>‘ok‘);
    }
    echo json_encode($result); // 必须输出JSON数据，前端ajax才能接收到
    ?>
    ```

![](img/30502ce54eee49ebdbc99a336334433f_222.png)

![](img/30502ce54eee49ebdbc99a336334433f_224.png)

![](img/30502ce54eee49ebdbc99a336334433f_226.png)

### ⚠️ 关键安全启示

![](img/30502ce54eee49ebdbc99a336334433f_228.png)

![](img/30502ce54eee49ebdbc99a336334433f_230.png)

![](img/30502ce54eee49ebdbc99a336334433f_232.png)

![](img/30502ce54eee49ebdbc99a336334433f_234.png)

这个案例揭示了前端验证的另一个重要安全边界：
*   **不安全的写法**：如上例所示，**登录成功后的关键逻辑（如 `location.href` 跳转）完全由前端根据返回值 `res.info_code` 来决定**。攻击者可以通过抓包工具，在HTTP响应中将 `info_code` 的值从 `0`（失败）修改为 `1`（成功），从而绕过密码验证，直接触发前端的“登录成功”流程。
*   **安全的写法**：应将**核心的业务逻辑处理放在后端**。前端Ajax仅用于交互和提示，例如，登录成功后，后端直接返回一个重定向指令或生成新的授权令牌，而不是让前端JS根据一个可被篡改的标志位来决定后续操作。

![](img/30502ce54eee49ebdbc99a336334433f_236.png)

![](img/30502ce54eee49ebdbc99a336334433f_238.png)

![](img/30502ce54eee49ebdbc99a336334433f_240.png)

![](img/30502ce54eee49ebdbc99a336334433f_242.png)

![](img/30502ce54eee49ebdbc99a336334433f_244.png)

![](img/30502ce54eee49ebdbc99a336334433f_246.png)

**核心原则：前端负责交互和展示，后端负责决策和核心业务逻辑。任何重要的状态改变或权限授予，其判断和执行都必须发生在后端服务器。**

![](img/30502ce54eee49ebdbc99a336334433f_248.png)

![](img/30502ce54eee49ebdbc99a336334433f_250.png)

![](img/30502ce54eee49ebdbc99a336334433f_252.png)

![](img/30502ce54eee49ebdbc99a336334433f_254.png)

---

![](img/30502ce54eee49ebdbc99a336334433f_256.png)

![](img/30502ce54eee49ebdbc99a336334433f_258.png)

![](img/30502ce54eee49ebdbc99a336334433f_260.png)

![](img/30502ce54eee49ebdbc99a336334433f_262.png)

![](img/30502ce54eee49ebdbc99a336334433f_264.png)

![](img/30502ce54eee49ebdbc99a336334433f_266.png)

![](img/30502ce54eee49ebdbc99a336334433f_268.png)

## 🛒 业务逻辑案例：商品购买功能

![](img/30502ce54eee49ebdbc99a336334433f_270.png)

![](img/30502ce54eee49ebdbc99a336334433f_272.png)

![](img/30502ce54eee49ebdbc99a336334433f_274.png)

上一节我们探讨了登录验证中的安全逻辑，本节我们将其原理应用于一个商品购买场景。

![](img/30502ce54eee49ebdbc99a336334433f_276.png)

![](img/30502ce54eee49ebdbc99a336334433f_278.png)

![](img/30502ce54eee49ebdbc99a336334433f_280.png)

![](img/30502ce54eee49ebdbc99a336334433f_282.png)

![](img/30502ce54eee49ebdbc99a336334433f_284.png)

![](img/30502ce54eee49ebdbc99a336334433f_286.png)

我们设计一个简单功能：前端展示商品和购买数量输入框，用户点击购买后，JS发送数量给后端，后端判断总价是否超过用户余额，并返回结果。

![](img/30502ce54eee49ebdbc99a336334433f_288.png)

![](img/30502ce54eee49ebdbc99a336334433f_290.png)

![](img/30502ce54eee49ebdbc99a336334433f_292.png)

![](img/30502ce54eee49ebdbc99a336334433f_294.png)

![](img/30502ce54eee49ebdbc99a336334433f_296.png)

### 代码实现简述

![](img/30502ce54eee49ebdbc99a336334433f_298.png)

![](img/30502ce54eee49ebdbc99a336334433f_300.png)

![](img/30502ce54eee49ebdbc99a336334433f_302.png)

![](img/30502ce54eee49ebdbc99a336334433f_304.png)

![](img/30502ce54eee49ebdbc99a336334433f_306.png)

![](img/30502ce54eee49ebdbc99a336334433f_308.png)

1.  **前端页面 (`shop.html`)**：展示商品，提供数量输入框和购买按钮。使用JQuery Ajax将购买数量 `num` 发送到 `shop.php`。
2.  **后端逻辑 (`shop.php`)**：假设商品单价为888，用户余额为10000。后端计算 `总价 = 888 * num`，判断是否 `<=10000`。
    ```php
    <?php
    $num = $_POST[‘num‘];
    $total = 888 * $num;
    if($total <= 10000){
        echo json_encode(array(‘info_code‘=>1, ‘msg‘=>‘购买成功‘));
    } else {
        echo json_encode(array(‘info_code‘=>0, ‘msg‘=>‘余额不足‘));
    }
    ?>
    ```
3.  **前端回调**：根据返回的 `info_code`，提示用户购买成功或失败。

![](img/30502ce54eee49ebdbc99a336334433f_310.png)

![](img/30502ce54eee49ebdbc99a336334433f_312.png)

![](img/30502ce54eee49ebdbc99a336334433f_314.png)

![](img/30502ce54eee49ebdbc99a336334433f_316.png)

### 💡 业务逻辑安全剖析

![](img/30502ce54eee49ebdbc99a336334433f_318.png)

![](img/30502ce54eee49ebdbc99a336334433f_320.png)

此案例与登录验证同理：
*   如果“扣除余额”、“生成订单”等关键购买成功后的逻辑，是由前端在收到 `info_code=1` 后执行的，那么该逻辑存在被绕过的风险。攻击者可通过修改返回包伪造购买成功。
*   安全的做法是：后端在验证通过后（`总价<=余额`），**同步地、原子化地**执行“扣除余额”和“创建订单”的数据库操作，然后仅将成功的结果通知前端。前端只负责展示“购买成功”的提示，而不负责触发核心业务逻辑。

![](img/30502ce54eee49ebdbc99a336334433f_322.png)

![](img/30502ce54eee49ebdbc99a336334433f_324.png)

![](img/30502ce54eee49ebdbc99a336334433f_326.png)

![](img/30502ce54eee49ebdbc99a336334433f_328.png)

**在审计类似业务（如支付、充值、修改信息、领取优惠券）时，务必关注：成功后的“关键动作”是在前端触发还是在后端同步完成。**

![](img/30502ce54eee49ebdbc99a336334433f_330.png)

![](img/30502ce54eee49ebdbc99a336334433f_332.png)

![](img/30502ce54eee49ebdbc99a336334433f_334.png)

![](img/30502ce54eee49ebdbc99a336334433f_336.png)

---

## 📝 课程总结

![](img/30502ce54eee49ebdbc99a336334433f_338.png)

![](img/30502ce54eee49ebdbc99a336334433f_340.png)

![](img/30502ce54eee49ebdbc99a336334433f_342.png)

本节课中我们一起学习了：
1.  **原生JavaScript开发**：通过文件上传功能，理解了原生JS操作DOM和事件的基本流程，并认识到**前端验证易被绕过**的本质。
2.  **JQuery库与Ajax技术**：学习了如何使用JQuery简化JS代码，并使用Ajax实现前端与后端的数据异步交互。这是现代Web应用常见的通信模式。
3.  **前后端安全边界**：通过登录和购买两个案例，我们深刻理解了**“前端控制交互，后端控制逻辑”** 的安全原则。将核心的业务状态判断和变更逻辑置于后端，是保证应用安全的基础。
4.  **安全漏洞模式**：识别了一种常见漏洞模式——**“基于前端返回值的逻辑决策”**。在安全测试中，应重点关注那些根据HTTP响应内容（如某个`code`或`flag`）来决定后续关键流程的前端代码。

![](img/30502ce54eee49ebdbc99a336334433f_344.png)

![](img/30502ce54eee49ebdbc99a336334433f_346.png)

![](img/30502ce54eee49ebdbc99a336334433f_348.png)

![](img/30502ce54eee49ebdbc99a336334433f_350.png)

![](img/30502ce54eee49ebdbc99a336334433f_352.png)

通过今天的课程，希望大家不仅能掌握一些JS开发技巧，更能建立起对Web应用前后端分工与安全边界的基本认知，为后续分析更复杂的安全问题打下基础。