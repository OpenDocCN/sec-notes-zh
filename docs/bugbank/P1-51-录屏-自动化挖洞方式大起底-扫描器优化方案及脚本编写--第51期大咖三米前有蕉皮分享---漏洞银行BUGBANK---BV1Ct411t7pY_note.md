# 自动化挖洞方式大起底——扫描器优化方案及脚本编写 🚀

![](img/c9caf156e3d92e984692837ed60f2d4c_0.png)

在本节课中，我们将学习如何通过调用扫描器API和操作其数据库，来优化自动化漏洞挖掘流程，并编写脚本以提高效率。课程内容主要分为两部分：一是通过API批量管理扫描任务，二是连接扫描器数据库以提取和验证漏洞信息。

![](img/c9caf156e3d92e984692837ed60f2d4c_2.png)

---

![](img/c9caf156e3d92e984692837ed60f2d4c_4.png)

## 为什么要学习使用扫描器？🤔

![](img/c9caf156e3d92e984692837ed60f2d4c_6.png)

上一节我们介绍了课程的整体目标，本节中我们来看看掌握扫描器技能的必要性。以下是几个关键原因：

1.  **作为安全人员的必备技能**：许多安全岗位的招聘要求中，明确列出“熟练使用某某扫描器者优先”。
2.  **提高工作效率**：扫描器的开发初衷就是减少人工检测，其扫描速度快且不知疲倦。
3.  **学习扫描思路与规则**：通过研究扫描器内置的检测规则（例如针对备份文件泄露的规则），可以学习前人的经验，并编写有针对性的轻量级脚本。
4.  **发现并完善扫描器的不足**：扫描器可能存在逻辑漏洞检测盲区，或对国内开发者的习惯支持不足，了解其原理有助于我们进行补充和优化。

---

![](img/c9caf156e3d92e984692837ed60f2d4c_8.png)

## 第一部分：调用扫描器API编写脚本 🛠️

![](img/c9caf156e3d92e984692837ed60f2d4c_10.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_12.png)

上一节我们了解了学习扫描器的意义，本节中我们将动手实践，学习如何通过API与扫描器进行交互。

在开始编写脚本前，需要掌握一些Python基础知识。

![](img/c9caf156e3d92e984692837ed60f2d4c_14.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_15.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_16.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_18.png)

### Python基础准备

以下是编写脚本所需的核心知识：

![](img/c9caf156e3d92e984692837ed60f2d4c_20.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_22.png)

*   **列表与字典**：
    *   列表：由中括号 `[]` 括起，元素以逗号分隔。例如：`[‘a‘, ‘b‘, ‘c‘]`
    *   字典：由大括号 `{}` 括起，是键值对集合。例如：`{‘key1‘: ‘value1‘, ‘key2‘: ‘value2‘}`
*   **JSON处理**：
    *   `json.loads()`: 将JSON格式的字符串转换为Python对象（如字典或列表）。
    *   `json.dumps()`: 将Python对象转换为JSON字符串，用于POST提交数据。
*   **Requests库**：用于发送HTTP请求。需要掌握几种基本的请求方法，如GET、POST、PATCH。

![](img/c9caf156e3d92e984692837ed60f2d4c_24.png)

### 环境与配置

![](img/c9caf156e3d92e984692837ed60f2d4c_26.png)

本次演示使用的漏洞测试环境是 `Web for Pen Test`。关于扫描器（AWVS）的安装，请注意以下两点：

![](img/c9caf156e3d92e984692837ed60f2d4c_28.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_30.png)

1.  若安装在远程服务器，需在安装时勾选“允许远程访问”，并配置好防火墙规则。
2.  若浏览器提示证书不安全，需要手动导入扫描器自带的SSL证书（通常位于安装目录下）。

![](img/c9caf156e3d92e984692837ed60f2d4c_32.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_34.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_36.png)

### 获取与验证API密钥

API密钥是调用接口的凭证。

![](img/c9caf156e3d92e984692837ed60f2d4c_38.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_40.png)

1.  在扫描器Web界面，进入 `Configuration` -> `API Keys` 生成令牌。
2.  在HTTP请求头中，必须添加 `X-Auth` 字段进行验证，格式为：`X-Auth: <你的API令牌>`。

![](img/c9caf156e3d92e984692837ed60f2d4c_42.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_44.png)

以下是一个Python请求头构造示例：
```python
headers = {
    ‘X-Auth‘: ‘你的API令牌‘,
    ‘Content-type‘: ‘application/json‘
}
```

![](img/c9caf156e3d92e984692837ed60f2d4c_45.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_47.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_49.png)

### 实践：添加扫描目标

![](img/c9caf156e3d92e984692837ed60f2d4c_50.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_52.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_54.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_56.png)

添加目标的请求方式是POST，URL为 `/api/v1/targets`。

![](img/c9caf156e3d92e984692837ed60f2d4c_58.png)

以下是添加目标的函数示例：
```python
import requests
import json

![](img/c9caf156e3d92e984692837ed60f2d4c_60.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_62.png)

def add_target(api_url, token, target_address, description):
    url = f“{api_url}/api/v1/targets“
    headers = {
        ‘X-Auth‘: token,
        ‘Content-type‘: ‘application/json‘
    }
    data = {
        ‘address‘: target_address,
        ‘description‘: description
    }
    # 忽略SSL证书验证（根据实际情况调整）
    response = requests.post(url, headers=headers, data=json.dumps(data), verify=False)
    return response.json() # 返回响应信息

![](img/c9caf156e3d92e984692837ed60f2d4c_64.png)

# 调用函数
result = add_target(‘https://your-awvs-host‘, ‘your-token-here‘, ‘http://test.com‘, ‘测试目标‘)
print(result)
```
执行成功后，会返回一个包含目标ID的JSON对象。这个ID在后续操作中至关重要。

![](img/c9caf156e3d92e984692837ed60f2d4c_66.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_68.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_70.png)

### 实践：配置扫描参数

![](img/c9caf156e3d92e984692837ed60f2d4c_72.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_74.png)

添加目标后，需要对其进行配置，如扫描速度、站点登录信息等。

配置请求通常使用PATCH方法，URL格式为 `/api/v1/targets/{target_id}/configuration`。

![](img/c9caf156e3d92e984692837ed60f2d4c_76.png)

以下是设置扫描速度的函数示例：
```python
def set_scan_speed(api_url, token, target_id, speed):
    url = f“{api_url}/api/v1/targets/{target_id}/configuration“
    headers = {
        ‘X-Auth‘: token,
        ‘Content-type‘: ‘application/json‘
    }
    # speed 可选 ‘slow‘, ‘moderate‘, ‘fast‘
    data = {
        ‘scan_speed‘: speed
    }
    response = requests.patch(url, headers=headers, data=json.dumps(data), verify=False)
    return response.status_code # 通常成功返回204

![](img/c9caf156e3d92e984692837ed60f2d4c_78.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_80.png)

# 类似地，可以编写设置登录凭证、代理等函数
```
**小技巧**：如果不清楚API提交的具体数据格式，可以使用浏览器的开发者工具（F12），在扫描器Web界面进行操作时，于“网络”标签页中查看实际发送的请求。

![](img/c9caf156e3d92e984692837ed60f2d4c_81.png)

### 实践：启动扫描

![](img/c9caf156e3d92e984692837ed60f2d4c_83.png)

配置完成后，即可启动扫描。请求方式为POST，URL为 `/api/v1/scans`。

![](img/c9caf156e3d92e984692837ed60f2d4c_85.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_86.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_88.png)

启动扫描需要指定目标ID和扫描类型。扫描类型代码可在数据库或官方文档中查询，例如：
*   `11111111-1111-1111-1111-111111111111`: 完全扫描
*   `11111111-1111-1111-1111-111111111112`: 仅高危漏洞

![](img/c9caf156e3d92e984692837ed60f2d4c_90.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_92.png)

以下是启动扫描的函数示例：
```python
def start_scan(api_url, token, target_id, scan_profile_id):
    url = f“{api_url}/api/v1/scans“
    headers = {
        ‘X-Auth‘: token,
        ‘Content-type‘: ‘application/json‘
    }
    data = {
        ‘target_id‘: target_id,
        ‘profile_id‘: scan_profile_id
    }
    response = requests.post(url, headers=headers, data=json.dumps(data), verify=False)
    return response.json()
```

![](img/c9caf156e3d92e984692837ed60f2d4c_94.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_96.png)

---

![](img/c9caf156e3d92e984692837ed60f2d4c_98.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_100.png)

## 第二部分：扫描器数据库的使用 🗃️

![](img/c9caf156e3d92e984692837ed60f2d4c_102.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_104.png)

上一节我们学习了如何通过API控制扫描器，本节中我们来看看如何直接操作扫描器数据库，以更灵活地获取和验证漏洞信息。

![](img/c9caf156e3d92e984692837ed60f2d4c_106.png)

虽然API也能获取漏洞数据，但存在分页限制（每次最多99条）。直接查询数据库则更为高效和全面。

![](img/c9caf156e3d92e984692837ed60f2d4c_108.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_110.png)

### 连接数据库

![](img/c9caf156e3d92e984692837ed60f2d4c_112.png)

扫描器（AWVS）使用PostgreSQL数据库。连接信息可在其配置文件中找到。本地连接通常无需密码。

![](img/c9caf156e3d92e984692837ed60f2d4c_114.png)

以下是使用Python连接数据库的示例：
```python
import psycopg2

![](img/c9caf156e3d92e984692837ed60f2d4c_116.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_117.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_119.png)

# 连接参数
conn = psycopg2.connect(
    database=“wvs“,
    user=“wvs“,
    password=““, # 本地可能为空
    host=“127.0.0.1“,
    port=“5432“
)
cursor = conn.cursor()
```

![](img/c9caf156e3d92e984692837ed60f2d4c_121.png)

### 核心数据表

![](img/c9caf156e3d92e984692837ed60f2d4c_123.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_125.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_127.png)

数据库中有几个重要的表：

![](img/c9caf156e3d92e984692837ed60f2d4c_129.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_131.png)

1.  **`dast_scans` 表**：存储扫描任务信息。
2.  **`vulnerabilities` 表**：**这是最重要的表**，存储了检测出的漏洞详情，包括：
    *   漏洞类型
    *   存在漏洞的URL
    *   攻击参数（`attack`）
    *   完整的HTTP请求数据（`request`）
    *   攻击载荷（`proof`）
    *   服务器响应（`response`）
3.  **`vulnerability_types` 表**：存储漏洞类型的描述、修复方案等。可以修改此表内容以实现报告汉化，但需注意保留HTML标签和字符实体。

### 从数据库提取漏洞信息

![](img/c9caf156e3d92e984692837ed60f2d4c_133.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_135.png)

以下是从 `vulnerabilities` 表中提取关键信息的示例：
```python
# 假设已建立数据库连接 conn
cursor.execute(“SELECT url, attack, proof, request FROM vulnerabilities WHERE scan_id=xxx“)
vuln_records = cursor.fetchall()

![](img/c9caf156e3d92e984692837ed60f2d4c_137.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_138.png)

for record in vuln_records:
    vuln_url = record[0]
    vuln_attack_param = record[1]
    vuln_proof = record[2]
    vuln_raw_request = record[3]
    print(f“漏洞URL: {vuln_url}“)
    print(f“攻击参数: {vuln_attack_param}“)
    # 进一步处理...
cursor.close()
conn.close()
```

![](img/c9caf156e3d92e984692837ed60f2d4c_140.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_142.png)

### 漏洞验证思路

获取漏洞信息后，可以编写脚本进行自动验证，以减少手工确认的工作量。

基本思路如下：
1.  **判断漏洞类型**：根据数据库中的漏洞类型字段。
2.  **选择验证工具**：为不同类型的漏洞选择合适的开源验证工具（如SQLMap用于SQL注入，XSStrike用于XSS）。
3.  **提取并传递参数**：从数据库记录中提取工具所需的参数（如URL、请求数据、攻击点）。
4.  **调用工具并解析结果**：执行验证命令，并判断输出中是否包含成功标识。

**关键点**：对于SQL注入，需要注意 `proof` 字段中的攻击载荷可能过长。有时需要将其替换为数据库 `attack` 字段中记录的原始参数值，才能成功验证。例如，将 `proof` 中的 `1‘ AND ‘1‘=‘1` 替换为 `attack` 中的原始值 `rot`。

**示例：验证XSS漏洞**
可以修改现有XSS工具（如XSStrike），使其支持从命令行接收参数，并优化其对无参数XSS的检测逻辑。
```bash
# 修改后的工具调用方式
python xsstrike.py -u “http://target.com/page?msg=<script>alert(1)</script>“
```

![](img/c9caf156e3d92e984692837ed60f2d4c_144.png)

**示例：验证SQL注入漏洞**
将数据库中的 `request` 字段保存为文件，然后使用SQLMap进行测试。
```bash
# 将请求数据保存到文件 req.txt
# 使用sqlmap测试
sqlmap -r req.txt --batch
```

### 编写自动化验证脚本

![](img/c9caf156e3d92e984692837ed60f2d4c_146.png)

可以编写一个主脚本，自动完成以下流程：
1.  查询数据库，获取新漏洞。
2.  根据漏洞类型分发到不同的验证子函数。
3.  调用相应的安全工具进行验证。
4.  将验证结果（确认的漏洞、误报）记录到报告文件中。

![](img/c9caf156e3d92e984692837ed60f2d4c_148.png)

对于验证失败的案例，可以集成绕过脚本（如将空格替换为注释符`/**/`）进行二次尝试。

---

![](img/c9caf156e3d92e984692837ed60f2d4c_150.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_151.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_153.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_155.png)

## 注意事项与最佳实践 📝

![](img/c9caf156e3d92e984692837ed60f2d4c_157.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_159.png)

在利用扫描器进行自动化挖洞时，请务必注意以下几点：

![](img/c9caf156e3d92e984692837ed60f2d4c_161.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_163.png)

1.  **清除扫描器特征**：扫描器发出的请求头中可能包含特定标识（如`Acunetix`）。这些特征容易被WAF识别并拦截。可以通过代理工具（如Burp Suite）修改或清除这些请求头。
2.  **关注非标准端口**：不要只扫描80/443端口。许多HTTP服务可能运行在其他端口上，它们也可能存在漏洞。
3.  **使用测试账号**：如果配置了自动登录扫描，务必使用测试账号，避免爬虫对真实账户数据造成意外修改或删除。
4.  **控制扫描频率**：避免对目标网站造成过大压力，影响其正常业务。设置较低的扫描速度。
5.  **本地搭建测试环境**：如果目标网站使用开源CMS/框架，可在本地搭建相同环境进行扫描。这样速度更快，且没有WAF干扰，发现漏洞后可直接在真实网站上验证。
6.  **理解误报与漏报**：没有任何扫描器能发现所有漏洞。不要因为扫描器没有报错就认为网站绝对安全。高质量的漏洞往往需要结合手工测试。
7.  **信息收集**：扫描器结果不仅是漏洞，也包含敏感信息泄露（如邮箱、IP、文件路径）。可以利用这些数据进行更深度的信息收集。

---

## 总结 🎯

本节课我们一起学习了自动化漏洞挖掘中扫描器的高级用法。

![](img/c9caf156e3d92e984692837ed60f2d4c_165.png)

![](img/c9caf156e3d92e984692837ed60f2d4c_166.png)

1.  **API自动化**：我们掌握了如何通过Python脚本调用扫描器API，实现目标的批量添加、参数配置和扫描启动，从而将扫描器集成到自动化工作流中。
2.  **数据库操作**：我们学习了如何直接连接扫描器的PostgreSQL数据库，从中提取详细的漏洞信息，并基于这些信息编写自动验证脚本，显著提高了漏洞确认的效率。
3.  **实践技巧**：我们探讨了清除扫描特征、处理非标准端口、使用测试账号、本地测试等一系列最佳实践，以确保自动化过程既高效又负责任。

![](img/c9caf156e3d92e984692837ed60f2d4c_168.png)

记住，扫描器是强大的辅助工具，但无法替代安全研究员的手工测试和深入分析。将自动化与手工测试相结合，才能最大化挖洞的深度和广度。