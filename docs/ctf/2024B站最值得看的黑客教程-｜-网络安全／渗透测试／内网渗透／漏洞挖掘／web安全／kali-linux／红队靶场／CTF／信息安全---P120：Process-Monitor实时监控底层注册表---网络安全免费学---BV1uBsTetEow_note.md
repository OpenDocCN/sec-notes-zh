# 网络安全：P120：Process Monitor实时监控底层注册表

## 概述
在本节课中，我们将学习如何利用白名单程序进行UAC提权，并掌握使用Process Monitor工具监控程序底层行为的方法。我们将通过一个具体的实例，演示如何发现并篡改白名单程序加载的注册表命令，从而绕过用户账户控制，获得系统最高权限。

---

## 白名单提权原理
上一节我们介绍了白名单程序的概念，本节中我们来看看其提权的核心原理。

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_1.png)

白名单程序在运行时，除了启动我们看到的界面，其底层还会加载操作系统的组件。这些行为包括加载动态链接库、进行文件操作或访问注册表。关键在于，某些白名单程序在启动时会调用注册表中的特定命令。

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_3.png)

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_5.png)

如果该白名单程序以高权限运行，那么它调用的命令也会以高权限执行。因此，我们只需将注册表中该命令的值篡改为我们想要执行的命令（例如添加用户或执行木马），当用户再次运行该白名单程序时，我们的恶意命令就会以最高管理员权限被执行，从而实现提权。

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_7.png)

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_9.png)

**核心逻辑公式**：
```
白名单程序启动 -> 调用注册表命令 -> 命令以高权限执行
篡改注册表命令为恶意命令 -> 白名单程序启动 -> 恶意命令以高权限执行 -> 提权成功
```

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_11.png)

---

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_13.png)

## Process Monitor工具介绍
理解了原理后，我们需要一个工具来观察程序的底层行为。Process Monitor正是这样一个由微软官方提供的强大工具。

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_15.png)

Process Monitor可以实时监控系统中的文件、注册表、进程和线程活动。在渗透测试或应急响应中，它常被用来分析软件或木马的具体行为，例如查看其修改了哪些注册表项、创建了哪些文件。

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_17.png)

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_19.png)

---

## 实战演示：监控白名单程序行为
接下来，我们通过一个完整的实验来演示如何发现可利用的白名单程序。

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_21.png)

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_22.png)

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_23.png)

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_24.png)

### 环境准备
首先，确保我们处于一个受UAC保护的普通管理员账户下，而不是最高权限的Administrator账户。然后，运行Process Monitor工具。

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_26.png)

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_27.png)

### 操作步骤
以下是使用Process Monitor进行分析的关键步骤：

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_28.png)

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_30.png)

1.  **启动监控并运行目标程序**
    打开Process Monitor，让其开始记录。然后运行我们选择测试的白名单程序（例如Windows设置 `ms-settings:`）。

2.  **设置进程过滤器**
    由于系统活动很多，我们需要过滤出只与目标程序相关的记录。
    *   点击工具栏上的“过滤器”(Filter)按钮（图标为漏斗状）。
    *   在弹出窗口中，选择条件为“Process Name”（进程名）。
    *   在下拉列表或输入框中，选择或填入目标进程的名称（例如`SystemSettings.exe`）。
    *   点击“Add”（添加）后，再点击“Apply”（应用）或“OK”（确定）。

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_32.png)

3.  **搜索关键注册表项**
    过滤后，列表将只显示该进程的活动。我们的目标是查找它是否访问了包含命令的注册表项。
    *   在Process Monitor中按下 `Ctrl + F` 打开查找框。
    *   搜索关键词 **`command`**。
    *   在结果中，寻找`RegOpenKey`或`RegQueryValue`操作，且路径中包含`command`的项。特别注意结果是否为`NAME NOT FOUND`，这表示程序试图访问该键值但未找到，这正是我们可以利用的地方。

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_34.png)

4.  **定位并分析注册表路径**
    在找到的`command`相关项上右键，选择“Jump to...”（跳转到），工具会自动打开注册表编辑器并定位到该路径。确认该键值是否为空或包含默认值，这验证了其可被篡改。

---

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_36.png)

## 实施提权
通过监控，我们假设发现了以下可利用的注册表路径：`HKCU\Software\Classes\ms-settings\shell\open\command`。

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_38.png)

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_40.png)

以下是实施提权的具体操作：

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_42.png)

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_44.png)

1.  **篡改注册表命令**
    我们将上述注册表路径下的默认值和数据值篡改为我们想要执行的命令。例如，篡改为启动`cmd.exe`。
    **操作代码（在CMD中执行）**：
    ```cmd
    reg add "HKCU\Software\Classes\ms-settings\shell\open\command" /d "cmd.exe" /f
    reg add "HKCU\Software\Classes\ms-settings\shell\open\command" /v "DelegateExecute" /f
    ```
    第一行将默认值设置为`cmd.exe`。第二行设置`DelegateExecute`键（某些程序需要），其值为空。

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_46.png)

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_47.png)

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_48.png)

2.  **触发提权**
    现在，以普通管理员身份再次运行该白名单程序（如`ms-settings:`）。此时，系统不会正常打开设置，而是会以**高级权限**启动一个命令提示符窗口。
    在这个新的CMD窗口中执行`net user`等需要高权限的命令，将会成功。

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_50.png)

3.  **实战化利用**
    在真实渗透中，可以将`cmd.exe`替换为木马或后门程序的路径。当用户运行白名单程序时，木马便会以最高权限静默运行。

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_52.png)

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_54.png)

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_56.png)

---

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_58.png)

## 总结
本节课中我们一起学习了白名单程序提权的核心原理与实战方法。

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_60.png)

我们了解到，某些白名单程序在启动时会尝试执行注册表中的命令。利用Process Monitor工具，我们可以监控并发现这一行为。通过篡改对应的注册表键值为恶意命令，当用户再次运行该白名单时，即可实现绕过UAC，以系统最高权限执行我们的命令。

**关键步骤总结**：
1.  使用Process Monitor监控目标白名单程序。
2.  过滤并搜索其对`command`相关注册表项的访问。
3.  篡改该注册表项的值为恶意命令（如`cmd.exe`或木马路径）。
4.  诱导或等待用户运行该白名单程序，触发提权。

![](img/dbd147cdcd05b28663e5ce57a60e3e7f_62.png)

这种方法在实战中非常有效，且由于修改的是当前用户可写的注册表区域，通常不会触发杀毒软件的警报。掌握这一技术，是深入理解Windows权限安全的重要一步。