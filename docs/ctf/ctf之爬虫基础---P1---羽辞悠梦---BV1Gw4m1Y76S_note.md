# CTF之爬虫基础：P1：爬虫流程与基础实践 🕷️

在本节课中，我们将学习网络爬虫的基本流程，并通过几个简单的实例来实践如何获取和解析网页数据。这对于解决CTF比赛中涉及网页交互的题目至关重要。

## 概述 📋

网络爬虫是一种自动获取网页内容的程序。其核心流程可以概括为：确定需求、找到数据地址、发送请求获取数据、解析数据以及最终保存数据。我们将通过具体的代码示例，一步步演示这个过程。

![](img/481d4e4ae559688feb0a1c916bf9ab26_1.png)

---

![](img/481d4e4ae559688feb0a1c916bf9ab26_3.png)

## 爬虫核心流程 🔄

爬虫工作遵循一个清晰的步骤，理解这个流程是编写有效爬虫程序的基础。

![](img/481d4e4ae559688feb0a1c916bf9ab26_5.png)

### 1. 确定需求 🎯

![](img/481d4e4ae559688feb0a1c916bf9ab26_7.png)

第一步是明确爬虫的目标。例如，我们的需求可能是“爬取某个网页上所有游戏的名称”。明确目标决定了后续所有操作的方向。

### 2. 找到数据所在地址 🌐

确定需求后，需要找到目标数据在网页中的具体位置。数据可能直接存在于初始加载的HTML代码中，也可能通过动态加载（如Ajax请求）获取。

![](img/481d4e4ae559688feb0a1c916bf9ab26_9.png)

*   **静态数据**：直接包含在网页源代码中。
*   **动态数据**：通过额外的网络请求（通常是XHR/Fetch类型）获取。当你在网页上点击“加载更多”或翻页时，浏览器会向服务器发送新的请求，服务器返回新数据，这就是动态加载。

以下是一个动态加载的示意图：
```
用户浏览器  --(请求第2页数据)-->  服务器
用户浏览器  <--(返回第2页数据)--  服务器
```
我们的爬虫需要模拟这个过程，找到并请求正确的数据地址。

![](img/481d4e4ae559688feb0a1c916bf9ab26_11.png)

### 3. 爬取数据 ⬇️

![](img/481d4e4ae559688feb0a1c916bf9ab26_13.png)

找到数据地址后，我们需要编写程序模拟浏览器向服务器发送请求。Python中常用 `requests` 库来完成这个任务。

核心代码是向目标URL发送一个GET请求：
```python
import requests
url = ‘目标网页地址‘
response = requests.get(url)
```
`response` 对象包含了服务器返回的响应，其中 `response.text` 属性就是网页的HTML源代码。

### 4. 解析数据 🔍

![](img/481d4e4ae559688feb0a1c916bf9ab26_15.png)

![](img/481d4e4ae559688feb0a1c916bf9ab26_17.png)

获取到的 `response.text` 是原始的HTML代码，并非我们直接看到的渲染后的页面。我们需要从中提取出所需的具体信息（如标题、链接）。

![](img/481d4e4ae559688feb0a1c916bf9ab26_19.png)

常用的解析方法有正则表达式 (`re` 模块) 和专门的解析库（如 `BeautifulSoup`）。例如，使用正则表达式提取特定模式的内容：
```python
import re
pattern = r‘<a.*?href=“/item/(.*?)“.*?>‘ # 示例：匹配链接
results = re.findall(pattern, response.text, re.S)
```

![](img/481d4e4ae559688feb0a1c916bf9ab26_21.png)

### 5. 保存数据 💾

最后一步是将解析出的有用数据保存下来，可以存入文件（如txt、csv）或数据库。
```python
with open(‘data.txt‘, ‘w‘, encoding=‘utf-8‘) as f:
    for item in results:
        f.write(item + ‘\n‘)
```

---

## 基础实践：静态页面爬取 🧪

![](img/481d4e4ae559688feb0a1c916bf9ab26_23.png)

![](img/481d4e4ae559688feb0a1c916bf9ab26_24.png)

![](img/481d4e4ae559688feb0a1c916bf9ab26_26.png)

上一节我们介绍了爬虫的核心流程，本节中我们通过一个简单实例来实践如何爬取静态页面。

我们的目标是爬取一个简单页面上所有的条目名称。

![](img/481d4e4ae559688feb0a1c916bf9ab26_28.png)

**步骤如下：**

![](img/481d4e4ae559688feb0a1c916bf9ab26_30.png)

1.  **发送请求**：使用 `requests.get()` 获取网页源代码。
2.  **解析数据**：本例使用正则表达式从HTML中提取所需数据。

以下是完整代码示例：
```python
import requests
import re

# 1. 确定目标URL
url = ‘http://127.0.0.1:135/‘ # 替换为你的目标地址

![](img/481d4e4ae559688feb0a1c916bf9ab26_32.png)

# 2. 发送请求，获取响应
response = requests.get(url)
html_code = response.text

![](img/481d4e4ae559688feb0a1c916bf9ab26_34.png)

# 3. 编写正则表达式解析数据
# 假设数据在形如 <li><a href=“/path/01“>Item 1</a></li> 的标签中
pattern = r‘<li><a href=“/path/(.*?)“>.*?</a></li>‘
results = re.findall(pattern, html_code, re.S)

![](img/481d4e4ae559688feb0a1c916bf9ab26_36.png)

![](img/481d4e4ae559688feb0a1c916bf9ab26_38.png)

# 4. 输出结果
print(“提取到的数据：“)
for item in results:
    print(item)
```
运行这段代码，即可提取出网页中符合模式的所有路径编号。

![](img/481d4e4ae559688feb0a1c916bf9ab26_40.png)

![](img/481d4e4ae559688feb0a1c916bf9ab26_41.png)

---

![](img/481d4e4ae559688feb0a1c916bf9ab26_43.png)

## 处理反爬机制：添加请求头 🛡️

![](img/481d4e4ae559688feb0a1c916bf9ab26_44.png)

在上一节的实践中，我们爬取了自己的本地服务器。但当爬取一些商业网站（如当当网）时，直接发送请求可能无法获取正确数据，因为网站有反爬虫机制。

最常见的反爬策略是检查请求头中的 `User-Agent` 字段。该字段标识了客户端的浏览器类型。使用Python的 `requests` 库默认的 `User-Agent` 会暴露爬虫身份，导致被拒绝。

**解决方法**是模拟浏览器的请求头：

![](img/481d4e4ae559688feb0a1c916bf9ab26_46.png)

以下是修改后的代码，添加了请求头信息：
```python
import requests
import re

![](img/481d4e4ae559688feb0a1c916bf9ab26_48.png)

url = ‘https://www.dangdang.com/‘ # 以当当网为例

![](img/481d4e4ae559688feb0a1c916bf9ab26_50.png)

# 定义请求头，模拟Chrome浏览器
headers = {
    ‘User-Agent‘: ‘Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36‘
}

![](img/481d4e4ae559688feb0a1c916bf9ab26_52.png)

# 在get请求中传入headers参数
response = requests.get(url, headers=headers)
html_code = response.text

![](img/481d4e4ae559688feb0a1c916bf9ab26_54.png)

![](img/481d4e4ae559688feb0a1c916bf9ab26_55.png)

# 后续解析步骤...
print(html_code[:500]) # 打印前500字符查看是否成功
```
通过添加正确的 `User-Agent`，服务器会认为请求来自真实的浏览器，从而返回正确的页面数据。对于更复杂的反爬（如Cookie验证、登录状态），可能需要添加更多字段到 `headers` 中，或使用 `session` 保持会话。

---

## 进阶实践：模拟登录 🔑

![](img/481d4e4ae559688feb0a1c916bf9ab26_57.png)

有些数据需要登录后才能访问。本节我们来看如何模拟登录过程。这通常涉及两个步骤：首先获取登录所需的令牌（如 `token`），然后携带账号、密码和该令牌发送POST请求。

![](img/481d4e4ae559688feb0a1c916bf9ab26_59.png)

我们以一个需要 `token` 的登录页面为例。

![](img/481d4e4ae559688feb0a1c916bf9ab26_60.png)

![](img/481d4e4ae559688feb0a1c916bf9ab26_62.png)

**步骤如下：**

![](img/481d4e4ae559688feb0a1c916bf9ab26_64.png)

1.  **获取Token**：先访问登录页，从HTML中解析出隐藏的 `token` 值。
2.  **构造登录数据**：将账号、密码和获取到的 `token` 组合成字典。
3.  **发送POST请求**：使用 `requests.post()` 提交数据，模拟登录。
4.  **保持会话**：使用 `requests.Session()` 对象，可以自动管理Cookies，维持登录状态。

![](img/481d4e4ae559688feb0a1c916bf9ab26_66.png)

以下是使用 `BeautifulSoup` 解析和 `Session` 保持会话的示例代码：
```python
import requests
from bs4 import BeautifulSoup

![](img/481d4e4ae559688feb0a1c916bf9ab26_68.png)

![](img/481d4e4ae559688feb0a1c916bf9ab26_70.png)

login_url = ‘https://example.com/login‘ # 替换为真实的登录地址
target_url = ‘https://example.com/dashboard‘ # 登录后要访问的地址

# 创建Session对象，用于保持会话（自动处理Cookies）
session = requests.Session()

![](img/481d4e4ae559688feb0a1c916bf9ab26_72.png)

# 1. 第一次请求，获取登录页面的HTML和token
login_page_response = session.get(login_url)
soup = BeautifulSoup(login_page_response.text, ‘html.parser‘)

![](img/481d4e4ae559688feb0a1c916bf9ab26_74.png)

# 假设token在一个name=‘token‘的input标签的value属性里
token_input = soup.find(‘input‘, {‘name‘: ‘token‘})
token = token_input[‘value‘] if token_input else ‘‘

# 2. 构造登录数据（账号、密码、token）
login_data = {
    ‘username‘: ‘your_username‘,
    ‘password‘: ‘your_password‘,
    ‘token‘: token
}

![](img/481d4e4ae559688feb0a1c916bf9ab26_76.png)

![](img/481d4e4ae559688feb0a1c916bf9ab26_78.png)

# 3. 发送POST请求进行登录
login_response = session.post(login_url, data=login_data)

# 4. 登录成功后，使用同一个session访问需要登录的页面
profile_response = session.get(target_url)

print(“登录后页面内容片段：“)
print(profile_response.text[:1000]) # 打印部分内容确认
```
**代码关键点说明：**
*   `BeautifulSoup`：用于解析HTML，方便地查找标签和属性。
*   `session`：维持一个会话，自动保存服务器返回的Cookies，并在后续请求中自动发送，这对于保持登录状态至关重要。
*   `find` 方法：查找第一个符合条件的标签。如果需要查找所有，使用 `find_all`。

通过这个流程，爬虫程序就能模拟用户完成登录并访问受保护的页面数据。

![](img/481d4e4ae559688feb0a1c916bf9ab26_80.png)

![](img/481d4e4ae559688feb0a1c916bf9ab26_82.png)

---

## 总结 📝

![](img/481d4e4ae559688feb0a1c916bf9ab26_84.png)

本节课中我们一起学习了网络爬虫的基础知识：
1.  **爬虫五步流程**：确定需求 -> 定位数据地址 -> 发送请求爬取 -> 解析数据 -> 保存数据。
2.  **基础工具**：使用 `requests` 库发送HTTP请求，使用 `re` 或 `BeautifulSoup` 解析HTML数据。
3.  **应对反爬**：通过修改请求头（如 `User-Agent`）来模拟浏览器，绕过基础反爬机制。
4.  **模拟登录**：利用 `requests.Session` 保持会话，并通过解析页面和提交表单数据来模拟用户登录。

![](img/481d4e4ae559688feb0a1c916bf9ab26_86.png)

掌握这些基础技能，你已能够应对许多需要自动化网页交互的CTF挑战。后续课程将探讨更复杂的动态页面爬取和验证码处理等问题。