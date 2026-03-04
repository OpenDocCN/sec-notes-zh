# 网络安全面试突击：P17：应急响应之Linux系统中毒处理 🛡️

在本节课中，我们将学习当Linux系统疑似中毒或被入侵时，如何进行应急响应。我们将按照一系列清晰的步骤，从用户检查到日志分析，系统地排查和处理安全威胁。

---

## 检查用户与密码 🔍

首先，我们需要检查系统的用户和密码文件。这一步的目的是确保系统中不存在攻击者创建的隐藏用户，并且所有账户的密码都是安全的。

**操作步骤如下：**

1.  **检查用户文件**：查看 `/etc/passwd` 文件，确认所有用户都是已知且合法的。特别注意UID为0（root权限）的用户。
    ```bash
    cat /etc/passwd
    ```
2.  **检查密码文件**：查看 `/etc/shadow` 文件（需要root权限），确保密码哈希是安全的。检查是否有空密码或弱密码账户。
    ```bash
    sudo cat /etc/shadow
    ```
3.  **清理与加固**：如果发现异常用户或弱密码账户，应立即删除未知用户或修改密码。

---

## 查看当前登录用户 👥

上一节我们检查了静态的用户账户，本节中我们来看看当前有哪些用户正在登录系统。这有助于发现正在进行的可疑会话。

我们可以使用 `w` 或 `who` 命令来查看。

**操作步骤如下：**

*   **使用 `w` 命令**：该命令能显示更详细的信息，包括用户、登录时间、运行的进程和终端类型。
    ```bash
    w
    ```
*   **使用 `who` 命令**：此命令更简洁，主要显示当前登录的用户名、终端和时间。
    ```bash
    who
    ```

通过对比已知的正常登录记录，可以快速识别出异常登录活动。

---

## 修改配置文件与检查历史记录 📝

在确认用户状态后，我们需要加固系统并追溯攻击者的行为。修改配置文件可以增强日志记录，而检查命令历史则可能发现攻击痕迹。

**操作步骤如下：**

1.  **增强登录审计**：在 `/etc/profile` 或用户家目录的 `.bashrc` 文件末尾添加代码，使每次用户登录时都记录其IP、时间等信息到日志中。
    ```bash
    echo ‘echo “$(whoami) logged in from $(echo $SSH_CLIENT | awk ‘{print $1}’) at $(date)” >> /var/log/login.log’ | sudo tee -a /etc/profile
    ```
2.  **检查命令历史**：查看用户执行过的命令，寻找可疑操作。`history` 命令显示当前用户的命令历史。
    ```bash
    history
    ```
    检查所有用户的命令历史（需要root权限）：
    ```bash
    sudo find /home -name ‘.bash_history’ -exec cat {} \;
    ```

---

## 分析网络连接与进程 🌐

攻击者通常会开放端口或运行恶意进程。本节我们将使用网络和进程分析工具来发现这些异常。

**操作步骤如下：**

1.  **分析网络连接**：使用 `netstat` 或 `ss` 命令查看所有网络连接、监听端口和对应的进程。
    ```bash
    sudo netstat -tulnp
    # 或
    sudo ss -tulnp
    ```
    重点关注不熟悉的IP地址、非常用端口以及可疑的进程名。
2.  **检查进程与文件路径**：对 `netstat` 中发现的可疑进程，使用 `ps` 命令进一步检查其详细信息，特别是其可执行文件的路径。
    ```bash
    ps aux | grep [可疑进程PID或名称]
    ls -la /proc/[可疑进程PID]/exe
    ```
    确认该进程是否为预期的系统服务或合法应用程序。

---

## 检查系统运行级别与定时任务 ⏰

系统启动项和定时任务是恶意软件实现持久化驻留的常见位置。我们需要对这些地方进行排查。

**操作步骤如下：**

1.  **检查运行级别与服务**：查看系统当前运行级别以及在该级别下启动的服务。
    ```bash
    runlevel
    sudo systemctl list-unit-files --type=service | grep enabled
    # 对于使用SysVinit的系统
    chkconfig --list
    ```
2.  **检查定时任务**：攻击者常利用cron设置后门。检查系统及所有用户的定时任务。
    ```bash
    sudo cat /etc/crontab
    sudo ls -la /etc/cron.*/
    crontab -l # 查看当前用户的cron任务
    sudo crontab -u [用户名] -l # 查看指定用户的cron任务
    ```
    仔细检查是否存在可疑的脚本或任务。

---

## 使用安全工具扫描与日志分析 🛠️

手动检查后，我们可以借助专业工具进行更全面的扫描，并深入分析系统日志，寻找攻击证据。

**操作步骤如下：**

1.  **使用安全工具扫描**：使用如 `rkhunter`、`chkrootkit`、`ClamAV` 等工具，检查系统是否存在已知的后门、rootkit或病毒。
    ```bash
    sudo rkhunter --check
    sudo chkrootkit
    sudo clamscan -r /
    ```
2.  **分析安全日志**：集中分析系统日志，特别是安全相关日志（如 `/var/log/auth.log`、`/var/log/secure`），寻找失败登录、特权提升等异常事件。
    ```bash
    sudo grep -i “failed” /var/log/auth.log
    sudo grep -i “accepted” /var/log/auth.log
    sudo journalctl -xe # 查看系统日志
    ```

---

## Web后门检测 🕸️

如果系统运行了Web服务（如Apache、Nginx），必须检查Web目录中是否被植入了Webshell等后门。

**操作步骤如下：**

1.  **使用扫描工具**：可以使用 `webshell` 扫描工具（如 `find` 命令结合特征码）进行检测。
    ```bash
    find /var/www/html -name “*.php” -exec grep -l “eval(.*base64_decode\|system(.*\$\|shell_exec” {} \;
    ```
2.  **手动代码审查**：在Web根目录中搜索常见的恶意函数或关键词，如 `eval()`、`assert()`、`system()`、`passthru()`、`shell_exec()` 以及 `base64_decode` 等。

---

## 总结 📚

本节课中我们一起学习了Linux系统中毒应急响应的完整流程。我们从**检查用户和密码**开始，逐步排查了**当前会话**、**系统配置**、**网络连接与进程**、**启动项与定时任务**，最后利用**安全工具扫描**、**深度日志分析**以及**Web后门检测**，形成了一套系统性的排查方法。记住，应急响应的核心是快速控制事态、消除威胁并收集证据，每一步操作都应谨慎记录，以便后续溯源和恢复。