# P18：18 - Plundervolt - Flipping Bits from Software without Rowhammer - 坤坤武特 - BV1g5411K7fe

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_1.png)

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_2.png)

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_3.png)

## 概述

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_5.png)

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_7.png)

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_9.png)

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_11.png)

在本节课中，我们将学习Plundervolt攻击，这是一种无需Rowhammer即可从软件中翻转位的技术。

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_13.png)

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_15.png)

## Rowhammer攻击

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_17.png)

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_19.png)

Rowhammer攻击是一种利用DRAM行缓冲器刷新机制来翻转位的技术。通过访问特定行，可以放电不仅读取并复制到行缓冲区的单元格，还可以放电附近的单元格，从而造成位翻转。

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_21.png)

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_23.png)

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_25.png)

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_27.png)

## Plundervolt攻击

Plundervolt攻击是一种无需Rowhammer即可从软件中翻转位的技术。它利用了动态电压和频率缩放（DVFS）技术，通过修改内存映射寄存器来改变硬件的电压。

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_29.png)

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_31.png)

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_33.png)

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_35.png)

## 实现Plundervolt攻击

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_37.png)

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_38.png)

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_40.png)

以下是使用C代码实现Plundervolt攻击的示例：

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_42.png)

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_44.png)

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_46.png)

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_48.png)

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_50.png)

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_52.png)

```c
#include <stdio.h>
#include <stdint.h>
#include <unistd.h>

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_54.png)

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_56.png)

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_58.png)

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_60.png)

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_62.png)

int main() {
    // 设置频率为1 GHz
    // ...

    // 加载MSR驱动程序
    // ...

    // 修改电压
    // ...

    // 执行操作并检查故障
    // ...

    return 0;
}
```

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_64.png)

## Plundervolt攻击的应用

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_66.png)

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_68.png)

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_70.png)

![](img/7b957fca6b8ad4c58c4ad990e3f0e20d_72.png)

Plundervolt攻击可以用于以下场景：

* **破解加密算法**：例如，通过注入故障来破解RSA和AES加密算法。
* **读取受保护的数据**：例如，通过注入故障来读取SGX enclave中的秘密数据。
* **导致程序崩溃**：例如，通过注入故障来导致程序崩溃。

## 总结

本节课中，我们学习了Plundervolt攻击，这是一种无需Rowhammer即可从软件中翻转位的技术。Plundervolt攻击可以用于多种场景，包括破解加密算法、读取受保护的数据和导致程序崩溃。