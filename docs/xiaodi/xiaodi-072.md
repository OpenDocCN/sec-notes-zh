#  072：支付逻辑漏洞详解

![](img/ba2d0cd505beb11db6ca8a4b411699de_1.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_3.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_5.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_7.png)

在本节课中，我们将要学习支付业务逻辑中常见的安全问题。支付逻辑漏洞主要产生于商品购买、会员开通、升级等涉及支付的环节。这类漏洞通常不涉及复杂的代码层面攻击，而是源于开发者在业务逻辑设计上的疏忽。理解整个支付流程，并分析每个环节可能存在的风险点，是挖掘此类漏洞的关键。

![](img/ba2d0cd505beb11db6ca8a4b411699de_9.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_11.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_13.png)

## 📋 课程概述

![](img/ba2d0cd505beb11db6ca8a4b411699de_15.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_17.png)

支付逻辑的简单流程通常包括：选择商品及数量、选择支付及配送方式、生成订单、完成支付。在这个过程中，涉及商品数据（编号、价格、数量）、订单属性、优惠券、积分、支付方式等多种参数。安全风险可能存在于任何一个环节的参数篡改或逻辑绕过。

![](img/ba2d0cd505beb11db6ca8a4b411699de_19.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_21.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_23.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_25.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_26.png)

上一节我们介绍了逻辑漏洞的通用思路，本节中我们来看看支付场景下的具体应用。

![](img/ba2d0cd505beb11db6ca8a4b411699de_28.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_30.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_32.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_34.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_36.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_38.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_40.png)

## 🔍 支付逻辑常见测试点

![](img/ba2d0cd505beb11db6ca8a4b411699de_42.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_43.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_45.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_47.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_49.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_51.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_53.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_55.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_57.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_59.png)

支付逻辑漏洞的测试可以从以下几个方面入手：

![](img/ba2d0cd505beb11db6ca8a4b411699de_61.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_63.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_65.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_67.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_69.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_71.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_73.png)

### 1. 参数篡改
这包括对商品价格、数量、订单属性等参数的直接修改。

![](img/ba2d0cd505beb11db6ca8a4b411699de_75.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_77.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_79.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_81.png)

### 2. 越权操作
例如，使用他人账户的标识（如`openid`）进行支付或兑换操作。

![](img/ba2d0cd505beb11db6ca8a4b411699de_83.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_85.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_87.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_89.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_91.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_93.png)

### 3. 并发与状态问题
利用并发请求或订单状态管理缺陷进行攻击。

![](img/ba2d0cd505beb11db6ca8a4b411699de_95.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_97.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_99.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_101.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_103.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_105.png)

### 4. 算法与溢出问题
利用数值运算、数据类型溢出等机制实现异常支付。

![](img/ba2d0cd505beb11db6ca8a4b411699de_107.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_109.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_111.png)

以下是针对每个测试点的详细说明：

![](img/ba2d0cd505beb11db6ca8a4b411699de_113.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_114.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_116.png)

#### 1.1 篡改商品价格与数量
在提交订单的数据包中，尝试修改`price`（价格）或`qty`（数量）参数。例如，将高价商品的价格改为低价，或将数量改为小数、负数，观察订单金额是否异常变化。

![](img/ba2d0cd505beb11db6ca8a4b411699de_118.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_120.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_122.png)

**核心操作**：拦截并修改HTTP请求中的价格或数量参数。
```http
POST /buy HTTP/1.1
...
id=product_a&qty=1&price=100  # 原请求
```
修改为：
```http
POST /buy HTTP/1.1
...
id=product_a&qty=-1&price=1  # 篡改后请求
```

![](img/ba2d0cd505beb11db6ca8a4b411699de_124.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_126.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_128.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_130.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_132.png)

#### 1.2 订单对冲与替换支付
**订单对冲**：生成A商品（低价）和B商品（高价）的订单。在支付B商品订单时，替换请求参数，使其指向A商品的订单信息，尝试用低价完成高价商品的支付。
**替换支付**：在支付环节，拦截支付请求，将其替换为另一个订单的支付标识，尝试完成支付。

![](img/ba2d0cd505beb11db6ca8a4b411699de_134.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_136.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_138.png)

#### 1.3 优惠券逻辑漏洞
*   **盗用**：猜测或遍历优惠券ID/兑换码，使用本不属于自己的优惠券。
*   **复用**：对已使用过的优惠券，重复提交使用请求，看是否能再次抵扣金额。
*   **参数分析**：对比使用与不使用优惠券的数据包差异，找到标识优惠券的参数（如`user_coupon_id`），尝试修改或重复使用。

![](img/ba2d0cd505beb11db6ca8a4b411699de_140.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_142.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_143.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_145.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_147.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_149.png)

#### 1.4 积分兑换逻辑漏洞
积分兑换涉及账户余额与积分的数值运算。可能存在以下问题：
*   **负值溢出**：在兑换积分时，传入负值，导致系统执行`余额 = 余额 - (-积分)`，反而增加余额。
*   **运算符号注入**：如果参数值直接参与数据库运算，可尝试传入包含运算符号（如`+`, `-`, `*`）的值，改变运算逻辑。
*   **越权兑换**：修改请求中的用户标识（如`uid`），使用他人的积分进行兑换。

**公式示例**：
假设原兑换逻辑为：`用户余额 = 用户余额 - 兑换积分值`
传入`兑换积分值 = -100`，则变为：`用户余额 = 用户余额 - (-100) = 用户余额 + 100`

#### 1.5 支付方式篡改
在调用第三方支付（微信、支付宝）时，拦截请求，尝试修改支付接口的调用地址或参数，例如重定向到攻击者控制的伪造接口。

![](img/ba2d0cd505beb11db6ca8a4b411699de_151.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_153.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_154.png)

#### 1.6 并发与状态问题
*   **并发签约**：在开通自动续费（签约）服务时，利用多设备或并发请求，绕过“仅限新用户/首月优惠”的限制，以优惠价格连续签约多月。
*   **无限试用**：分析试用服务的截止判断逻辑（如本地时间、服务器时间、试用次数），通过修改设备时间、重放请求等方式绕过限制。
*   **提前/重复签到**：修改签到请求中的日期参数，或并发提交签到请求，获取不应获得的签到奖励。

![](img/ba2d0cd505beb11db6ca8a4b411699de_156.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_158.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_160.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_162.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_164.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_166.png)

#### 1.7 算法与四舍五入问题
*   **四舍五入漏洞**：利用支付平台（如微信、支付宝）最小支付单位为“分”的特性。例如，充值`0.019`元，支付平台实际扣款`0.01`元，但网站后台可能因四舍五入记为`0.02`元，导致账户余额异常增加。
*   **整数溢出**：利用程序变量的最大值限制。例如，`int`类型有最大值（如`2147483647`），超过此值会发生溢出，可能变为很小的值。在积分、金额计算中可能被利用。

![](img/ba2d0cd505beb11db6ca8a4b411699de_168.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_170.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_172.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_174.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_176.png)

## 🛠️ 漏洞挖掘方法总结

![](img/ba2d0cd505beb11db6ca8a4b411699de_178.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_180.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_182.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_184.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_186.png)

1.  **流程梳理**：清晰理解目标业务的完整支付流程。
2.  **参数枚举**：对流程中每个HTTP请求的所有参数进行记录和分析，猜测其含义。
3.  **修改测试**：针对每个参数，尝试进行边界值测试（如极大值、极小值、负数、零、小数）、类型混淆测试（如数字改字符串）、越权测试（替换用户ID）、重复提交测试等。
4.  **状态分析**：关注订单状态、支付状态、优惠券使用状态等，尝试在不同状态间进行非常规跳转或修改。
5.  **多端测试**：在Web端、APP端、小程序端分别测试，可能后端逻辑一致，但前端限制不同。
6.  **思维转换**：始终思考“如果我是开发者，我会如何设计这个逻辑？我可能忽略哪些检查？”。

![](img/ba2d0cd505beb11db6ca8a4b411699de_188.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_190.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_192.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_194.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_196.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_198.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_200.png)

## 💡 修复建议

![](img/ba2d0cd505beb11db6ca8a4b411699de_202.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_204.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_206.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_208.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_210.png)

对于开发者而言，修复支付逻辑漏洞需遵循以下原则：
*   **关键逻辑置于服务端**：所有价格、数量、折扣、用户身份等关键信息的校验必须在服务端进行，不可依赖客户端传递的参数。
*   **使用不可篡改的标识**：订单号、支付流水号等应使用难以预测的强随机数生成，并与用户、商品信息强绑定。
*   **幂等性设计**：支付、优惠券使用、积分兑换等核心操作需具备幂等性，防止重复请求造成多次生效。
*   **状态机管理**：订单、支付等应有明确的状态流转规则，避免状态被非法修改或回退。
*   **业务闭环校验**：支付成功后，回调验证应检查支付金额、订单号、商品信息是否完全匹配，而不仅仅是支付成功状态。
*   **数值运算安全**：对涉及余额、积分的运算，进行严格的符号、范围、溢出检查。

![](img/ba2d0cd505beb11db6ca8a4b411699de_212.png)

## 📝 本节课总结

![](img/ba2d0cd505beb11db6ca8a4b411699de_214.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_216.png)

本节课我们一起学习了支付业务逻辑中常见的多种安全漏洞类型，包括参数篡改、越权盗用、并发签约、算法溢出等。这些漏洞的核心在于攻击者通过修改请求参数、利用逻辑缺陷或状态管理问题，达到“薅羊毛”、越权消费甚至非法获利的目的。

![](img/ba2d0cd505beb11db6ca8a4b411699de_218.png)

挖掘这类漏洞不需要深厚的代码功底，但需要清晰的业务逻辑思维、细致的观察力（分析数据包）和“钻空子”的想象力。请记住，多从开发者角度思考“哪里可能没考虑到”，是发现逻辑漏洞的关键。

![](img/ba2d0cd505beb11db6ca8a4b411699de_220.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_222.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_224.png)

![](img/ba2d0cd505beb11db6ca8a4b411699de_225.png)

下节课，我们将继续学习与状态验证相关的逻辑漏洞，例如接口枚举、验证码绕过等。