![](img/21ea2d715e714ef665a9d86ba8ccd7ba_1.png)

# 课程 P197：213 - 在线注册控制：注册、充值与激活 🔐

在本节课中，我们将学习如何在一个微分注册系统中实现用户注册、充值激活以及登录功能。我们将使用几个核心函数，并了解如何将它们整合到图形界面中。

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_3.png)

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_5.png)

---

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_6.png)

## 界面与变量准备 🎨

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_8.png)

上一节我们介绍了微分系统的基本概念，本节中我们来看看如何构建用户操作界面。

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_10.png)

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_12.png)

首先，我们需要在图形界面中添加几个按钮和编辑框，用于处理注册、充值和修改密码操作。

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_13.png)

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_15.png)

以下是需要添加的控件：
*   一个用于输入用户名的编辑框。
*   一个用于输入密码的编辑框。
*   一个用于输入充值卡号的编辑框。
*   三个按钮，分别对应“注册”、“充值”和“修改密码”功能。

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_17.png)

接着，我们需要为这些编辑框关联对应的字符串变量，以便在代码中获取用户输入的数据。

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_19.png)

---

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_21.png)

## 实现用户注册 📝

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_23.png)

界面准备就绪后，我们来实现用户注册功能。注册需要使用 `the resist` 函数。

在使用任何功能前，必须确保已成功调用初始化函数。`the resist` 函数的主要参数是用户名和密码，它们都需要 `char` 类型的指针。其他参数如用户类型、是否绑定电脑、通道数和初始点数可以按需设置。

以下是注册功能的核心代码逻辑：
1.  将窗口编辑框中的数据更新到关联的变量中。
2.  调用 `the resist` 函数，传入用户名、密码及其他参数。
3.  处理函数的返回值（例如，0表示成功，8表示用户名重复）。
4.  使用完毕后，释放字符串缓冲区占用的内存。

代码示例：
```c
// 更新数据到变量
UpdateData(TRUE);

// 调用注册函数
int result = the_resist(m_strUsername.GetBuffer(200), m_strPassword.GetBuffer(200), 0, 0, 1, 1000);

// 释放缓冲区
m_strUsername.ReleaseBuffer();
m_strPassword.ReleaseBuffer();

// 处理结果
if(result == 0) {
    // 注册成功
} else if(result == 8) {
    // 用户名重复
}
```
操作成功后，可以在后台管理系统中看到新注册的用户。

---

## 为用户账户充值 💳

用户注册后，账户尚未激活，需要充值才能登录使用。充值功能需要使用 `a t t t` 函数。

以下是充值激活的核心步骤：
1.  用户从开发者或代理商处获得充值卡（加时卡）。
2.  在程序界面输入卡号和要充值的用户名。
3.  调用 `a t t t` 函数，传入卡号、购买者（用户名）等参数。
4.  函数会返回充值成功与否，以及充入的天数和点数。

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_25.png)

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_27.png)

代码示例：
```c
// 更新数据到变量
UpdateData(TRUE);

// 定义变量接收返回的天数和点数
int daysAdded = 0, pointsAdded = 0;

// 调用充值函数
int rechargeResult = a_t_t_t(m_strCardNumber.GetBuffer(200), m_strUsername.GetBuffer(200), NULL, &daysAdded, &pointsAdded);

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_29.png)

// 释放缓冲区
m_strCardNumber.ReleaseBuffer();
m_strUsername.ReleaseBuffer();

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_31.png)

// 处理结果
if(rechargeResult == 0) {
    // 充值成功，daysAdded和pointsAdded已被填充
}
```
充值成功后，该用户的剩余天数和点数会增加，账户即被激活。

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_33.png)

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_35.png)

---

## 登录已激活的账户 🔑

账户充值激活后，用户就可以使用用户名和密码进行登录了。登录功能我们已在上一节课讨论过。

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_37.png)

我们需要修改登录按钮的代码，确保它使用当前界面输入的用户名和密码，而不是固定的测试数据。

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_39.png)

核心注意事项：
*   确保在调用登录函数前，正确地将编辑框内容更新到变量中。
*   登录成功后，可以根据返回值在程序中开启相应的功能或界面。
*   同样，使用完毕后需要释放字符串缓冲区。

代码示例：
```c
// 更新数据到变量
UpdateData(TRUE);

// 调用登录函数
int loginResult = login_function(m_strUsername.GetBuffer(200), m_strPassword.GetBuffer(200));

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_41.png)

// 释放缓冲区
m_strUsername.ReleaseBuffer();
m_strPassword.ReleaseBuffer();

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_42.png)

if(loginResult == 0) {
    // 登录成功，执行后续操作
}
```
务必检查按钮消息映射的函数名是否正确，避免因函数名冲突导致执行错误的代码。

---

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_44.png)

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_46.png)

## 功能测试与流程整合 🧪

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_48.png)

现在，我们将注册、充值、登录三个功能串联起来进行完整测试。

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_50.png)

完整的用户流程如下：
1.  **注册**：新用户输入用户名和密码，点击注册按钮创建账户。
2.  **充值**：用户获得充值卡号，在界面输入卡号和自己的用户名，点击充值按钮为账户激活。
3.  **登录**：账户激活后，用户输入用户名和密码，点击登录按钮即可成功登录并使用软件。

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_52.png)

在测试过程中，请注意每一步的反馈信息，并确保后台管理系统中的数据同步更新。

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_54.png)

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_56.png)

---

## 总结 📚

本节课中我们一起学习了微分注册系统中三个核心功能的实现：用户注册、充值激活和账户登录。

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_58.png)

我们掌握了以下关键点：
*   使用 `the resist` 函数实现注册，并理解其各项参数。
*   使用 `a t t t` 函数通过卡号为指定用户充值，从而激活账户。
*   修改并完善登录逻辑，使其能够使用动态输入的用户信息。
*   理解了注册、充值、登录这一完整的用户生命周期流程。

![](img/21ea2d715e714ef665a9d86ba8ccd7ba_60.png)

此外，还应注意代码中内存的管理（及时释放缓冲区）和界面控件消息映射的准确性。通过本节课的学习，你已经能够构建一个具备完整用户管理流程的软件基础模块。