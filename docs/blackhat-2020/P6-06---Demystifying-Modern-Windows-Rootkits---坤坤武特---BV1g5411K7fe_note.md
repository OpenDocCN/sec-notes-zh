![](img/bdba59d6094c80ebd9d4e04bf7ae4360_1.png)

# P6：06 - 现代Windows Rootkit揭秘 - 坤坤武特 - BV1g5411K7fe

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_3.png)

感谢大家来参加我的演讲。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_1.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_5.png)

首先，介绍一下我自己。我今年18岁，是罗切斯特理工学院的大二学生。我对Windows内部结构很感兴趣。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_7.png)

我主要自学，同时有一群强大的导师团队在帮助我。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_9.png)

我还有丰富的游戏破解背景，这也是我涉足Windows内部结构的原因。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_11.png)

那么，这次演讲的主题是什么呢？简单来说。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_13.png)

在本演讲中，我们将探讨加载Rootkit、与Rootkit通信、滥用合法网络通信、我编写的一个名为Spectre的Rootkit示例，以及其背后的设计选择。使用内核命令和技巧来掩盖Rootkit的文件系统跟踪。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_15.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_3.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_17.png)

首先，我们来了解一下Windows Rootkit概述。在本演讲中，当我说Rootkit时，我将指的是Windows上的内核级Rootkit。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_19.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_21.png)

那么，为什么你想使用Rootkit呢？因为内核驱动程序对机器有显著的访问权限。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_23.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_25.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_27.png)

与用户模式不同，你几乎可以访问任何东西。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_29.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_31.png)

内核驱动程序在与其他内核防病毒软件相同的权限级别上运行。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_33.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_35.png)

这意味着，一般来说，你拥有相同的访问权限和资源。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_37.png)

针对内核恶意软件的缓解措施和安全解决方案较少。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_39.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_41.png)

如果你可以加载内核代码，那么你很可能可以做一些事情来掩盖你的行踪。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_43.png)

防病毒软件通常对内核驱动程序的操作可见性和操作较少。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_45.png)

这是因为防病毒软件通常依赖于使用用户模式钩子来获取对某些可疑操作的可见性。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_47.png)

但在内核中，由于补丁保护，你不能直接钩子任务内核。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_49.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_51.png)

最后，内核驱动程序通常会被防病毒软件忽略。

让我们看看一个例子。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_53.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_55.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_5.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_57.png)

所以，无论是运行消费者防病毒软件还是企业级EDR，你使用的应用程序都可能对内核驱动程序抱有相当大的信任。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_59.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_61.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_63.png)

例如，这里我们有来自Malwarebytes和Carbon Black的预处理线程回调。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_65.png)

所以，每当创建或复制到处理器或线程的句柄时，这些函数就会被调用。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_67.png)

从Malwarebytes开始，他们在回调中会检查进程ID是否小于8。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_69.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_71.png)

如果内核句柄是内核句柄，那么它将直接返回0，并停止处理。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_73.png)

对于Carbon Black，如果句柄是内核句柄或前一个模式不是用户模式，那么它将不会处理该句柄创建。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_75.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_77.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_7.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_79.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_81.png)

现在，我们来谈谈加载Rootkit。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_83.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_9.png)

有很多易受攻击的驱动程序。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_85.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_87.png)

一些逆向工程知识可以帮助你找到驱动程序中的零日漏洞，这也可以很容易地做到。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_89.png)

一些例子包括Capcom的反作弊驱动程序和TellNow驱动程序，甚至包括微软自己。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_91.png)

现在，我把易受攻击和零日放在引号中，是因为很多时候驱动程序需要管理员权限才能与其通信。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_93.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_95.png)

遵循微软标准，从ring 3（管理员）到ring 0（内核）不是有效的安全边界。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_97.png)

所以，从技术上讲，这甚至不是一个漏洞。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_99.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_11.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_101.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_103.png)

但这并不意味着我们不能滥用它。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_105.png)

所以，使用合法的驱动程序也有很多好处，因为你只需要几个原语来提升权限，并且找到易受攻击的驱动程序相对简单。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_107.png)

一个很好的起点是OEM驱动程序。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_109.png)

由于兼容性原因，很难检测到一些操作，这也很困难。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_111.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_113.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_115.png)

例如，假设一个驱动程序通过其I/Octl接口公开了一些可疑的操作。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_117.png)

特别是如果合法应用程序使用了这些可疑功能，那么防病毒软件很难判断操作是否来自合法应用程序，或者这是预期用途，或者恶意应用程序正在滥用该驱动程序功能。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_119.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_121.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_13.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_123.png)

但是，滥用合法驱动程序也有一些严重的缺点。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_125.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_127.png)

我之所以不太喜欢这种方法，是因为假设你使用一个合法的驱动程序来加载你的驱动程序。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_129.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_131.png)

问题是，你经常会遇到兼容性问题，特别是跨操作系统版本。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_133.png)

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_135.png)

即使你试图消除所有错误，你也可能会遇到一些边缘情况，这会导致蓝屏。

最后，我不想让受害者蓝屏。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_137.png)

对于一些Rad Teamer来说，一个选项是为自己公司购买自己的证书。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_139.png)

这对于定向攻击来说很棒。

![](img/bdba59d6094c80ebd9d4e04bf7ae4360_141.png)

你不会担心稳定性问题，但这也可能暴露你的身份，并且可能会被列入黑名单。

现在，这种黑名单并不