# 019：Snort快速入门（补充资源）🚀

![](img/1594245c822936da05a7782aeeb8f11f_1.png)

![](img/1594245c822936da05a7782aeeb8f11f_3.png)

![](img/1594245c822936da05a7782aeeb8f11f_4.png)

![](img/1594245c822936da05a7782aeeb8f11f_5.png)

在本教程中，我们将学习如何在Security Onion虚拟机中运行Snort，以完成模块4的作业。我们将涵盖从下载数据包捕获文件到配置Snort、运行分析，并最终解读结果的完整流程。

![](img/1594245c822936da05a7782aeeb8f11f_7.png)

## 准备工作与环境

![](img/1594245c822936da05a7782aeeb8f11f_8.png)

![](img/1594245c822936da05a7782aeeb8f11f_10.png)

![](img/1594245c822936da05a7782aeeb8f11f_11.png)

上一节我们介绍了本教程的目标，本节中我们来看看具体的操作环境。首先，你需要启动Security Onion虚拟机。作业中要求使用Snort对一个特定的PCAP文件进行分析。

![](img/1594245c822936da05a7782aeeb8f11f_13.png)

![](img/1594245c822936da05a7782aeeb8f11f_15.png)

以下是开始前的准备工作：

![](img/1594245c822936da05a7782aeeb8f11f_17.png)

1.  **下载PCAP文件**：作业PDF中提供了文件的下载链接。你可以在虚拟机中打开Chromium浏览器直接下载，或者使用命令行工具`wget`。
    *   使用`wget`命令的格式为：`wget [文件的URL]`。
2.  **解压文件**：下载的文件通常是压缩格式（.gz）。你需要使用`gunzip`命令进行解压。
    *   解压命令为：`gunzip [文件名].gz`。
    *   解压后，你将得到一个名为`inside.tcpdump`的PCAP文件。

![](img/1594245c822936da05a7782aeeb8f11f_19.png)

![](img/1594245c822936da05a7782aeeb8f11f_20.png)

![](img/1594245c822936da05a7782aeeb8f11f_22.png)

## 配置Snort规则文件

![](img/1594245c822936da05a7782aeeb8f11f_23.png)

![](img/1594245c822936da05a7782aeeb8f11f_25.png)

![](img/1594245c822936da05a7782aeeb8f11f_27.png)

在运行Snort之前，我们需要根据作业要求修改其配置文件，以确保它能正确读取PCAP文件。

![](img/1594245c822936da05a7782aeeb8f11f_29.png)

![](img/1594245c822936da05a7782aeeb8f11f_31.png)

Snort的配置文件位于`/etc/nsm/`目录下，通常以你的网络适配器命名，例如`securityonion-eth1-snort.conf`。

你需要使用文本编辑器（如`vi`或`nano`）打开此文件，并找到大约第170行附近的`config daq:`配置行。

![](img/1594245c822936da05a7782aeeb8f11f_33.png)

以下是需要进行的修改：

![](img/1594245c822936da05a7782aeeb8f11f_35.png)

![](img/1594245c822936da05a7782aeeb8f11f_37.png)

*   将`config daq:`行的值从`pfring`更改为`pcap`。
*   （可选）你可以通过在该行前面添加`#`号来注释掉后续几行相关的配置。

完成修改并保存文件后，Snort的配置就准备好了。

## 运行Snort分析PCAP文件

![](img/1594245c822936da05a7782aeeb8f11f_39.png)

![](img/1594245c822936da05a7782aeeb8f11f_40.png)

现在，我们可以运行Snort来分析之前下载的PCAP文件了。运行Snort需要指定几个关键的**命令行参数**。

![](img/1594245c822936da05a7782aeeb8f11f_42.png)

![](img/1594245c822936da05a7782aeeb8f11f_44.png)

![](img/1594245c822936da05a7782aeeb8f11f_46.png)

通过查阅Snort的手册页（`man snort`），我们可以了解到以下重要参数：

![](img/1594245c822936da05a7782aeeb8f11f_48.png)

![](img/1594245c822936da05a7782aeeb8f11f_50.png)

*   `-c`：用于指定规则配置文件（即我们刚刚修改的那个文件）。
*   `-r`：用于指定要读取的PCAP文件。
*   `-v`：启用详细模式，将处理过程输出到屏幕，便于观察。

![](img/1594245c822936da05a7782aeeb8f11f_52.png)

![](img/1594245c822936da05a7782aeeb8f11f_54.png)

因此，完整的运行命令结构如下：
```bash
sudo snort -c /etc/nsm/securityonion-eth1-snort.conf -r /path/to/inside.tcpdump -v
```
请将`/path/to/inside.tcpdump`替换为你解压后的PCAP文件的实际路径。

![](img/1594245c822936da05a7782aeeb8f11f_56.png)

![](img/1594245c822936da05a7782aeeb8f11f_57.png)

执行该命令后，Snort会先初始化并加载规则，这个过程可能需要一分钟左右。初始化完成后，你将看到数据包开始在屏幕上滚动显示，这表明Snort正在处理文件。

![](img/1594245c822936da05a7782aeeb8f11f_59.png)

![](img/1594245c822936da05a7782aeeb8f11f_60.png)

## 解读分析结果

![](img/1594245c822936da05a7782aeeb8f11f_62.png)

![](img/1594245c822936da05a7782aeeb8f11f_63.png)

![](img/1594245c822936da05a7782aeeb8f11f_64.png)

当Snort处理完整个PCAP文件后，它会在最后输出一系列的统计信息。作业要求你找出Snort在此次分析中检测到了多少个警报（Alert）。

![](img/1594245c822936da05a7782aeeb8f11f_66.png)

![](img/1594245c822936da05a7782aeeb8f11f_67.png)

![](img/1594245c822936da05a7782aeeb8f11f_68.png)

在输出的“Action Stats”部分，你可以找到“Alerts”这一项，其对应的数值就是检测到的警报数量。根据提示，这个数字应该在几百左右。如果你的结果远小于或远大于这个范围，可能需要检查配置并重新运行一次。

## 总结

![](img/1594245c822936da05a7782aeeb8f11f_70.png)

![](img/1594245c822936da05a7782aeeb8f11f_71.png)

![](img/1594245c822936da05a7782aeeb8f11f_72.png)

![](img/1594245c822936da05a7782aeeb8f11f_73.png)

本节课中我们一起学习了在Security Onion环境中使用Snort进行入侵检测分析的基本流程。我们完成了从下载数据包、配置Snort、使用`-c`、`-r`、`-v`参数运行分析，到最终在输出统计信息中查找警报数量的全过程。希望本教程能帮助你顺利完成模块4的作业。