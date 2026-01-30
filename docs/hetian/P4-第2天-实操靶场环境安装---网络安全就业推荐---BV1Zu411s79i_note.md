# 网络安全就业推荐 P4：第2天：实操靶场环境安装 🛠️

## 概述
在本节课中，我们将学习如何搭建一个用于网络安全学习和测试的漏洞靶场环境。主要内容包括在Linux系统上部署LAMP环境，以及在Windows系统上使用PHPStudy集成环境，并最终部署DVWA漏洞靶场。同时，我们也会补充Java和Python环境的安装，为后续使用安全工具做好准备。

---

## LAMP环境简介与安装

上一节我们概述了课程目标，本节中我们来看看什么是LAMP环境以及如何在Kali Linux中安装它。

LAMP指的是一组常用于搭建动态网站或服务器的开源软件组合的首字母缩写。它们本身是独立的程序，但由于经常被一起使用，所以被统称为LAMP。

以下是LAMP各组成部分的含义：
*   **L**：指 **Linux** 操作系统。它是一类Unix-like操作系统的统称，在服务器领域应用非常广泛。常见的发行版有CentOS、Debian、Ubuntu、Red Hat等。我们的Kali Linux就是基于Debian的发行版。
*   **A**：指 **Apache**，一个网页服务器。
*   **M**：指 **MySQL** 或 **MariaDB** 等数据库管理系统。
*   **P**：指 **PHP** 脚本语言。

我们主要讲解的是 **Linux + Apache + MySQL + PHP** 的架构。当然，也存在使用Nginx替代Apache的LNMP架构。两者的主要区别在于，Nginx占用系统资源更少，擅长处理静态页面请求，常作为前端服务器；而Apache则更擅长处理PHP等动态脚本请求，常作为后端服务器。

![](img/84a685713de25dd648421bd595768518_1.png)

![](img/84a685713de25dd648421bd595768518_3.png)

### 在Kali Linux中安装LAMP
在Linux系统中安装软件通常比Windows更方便。以下是安装LAMP环境的核心命令：

```bash
# 安装 Apache2
apt install apache2 -y

# 安装 MariaDB (在Kali中替代MySQL)
apt install mariadb mariadb-server -y

# 安装 PHP 及常用扩展
apt install php libapache2-mod-php php-mysql php-gd php-curl php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip -y
```

![](img/84a685713de25dd648421bd595768518_5.png)

**命令解析**：
*   `apt`：Debian系Linux的包管理工具。
*   `install`：安装软件。
*   `-y`：在安装过程中对所有提示自动回答“yes”。

安装完成后，需要启动服务：

![](img/84a685713de25dd648421bd595768518_7.png)

![](img/84a685713de25dd648421bd595768518_9.png)

```bash
# 启动 Apache2 服务
service apache2 start
# 或使用 systemctl (更推荐)
systemctl start apache2

# 设置 Apache2 开机自启
systemctl enable apache2

# 启动 MariaDB 服务
systemctl start mariadb
```

### 卸载LAMP环境
如果你需要卸载已安装的环境，可以使用以下命令：

![](img/84a685713de25dd648421bd595768518_11.png)

![](img/84a685713de25dd648421bd595768518_13.png)

```bash
# 自动移除 MariaDB 相关包
apt autoremove mariadb mariadb-server -y
# 移除 Apache2
apt remove apache2 -y
# 移除 PHP
apt remove php* -y

# 查找并删除残留的配置文件 (以mysql为例)
find /etc -name "*mysql*" -exec rm -rf {} \;
find /etc -name "*mariadb*" -exec rm -rf {} \;
```

---

## 部署DVWA漏洞靶场

![](img/84a685713de25dd648421bd595768518_15.png)

![](img/84a685713de25dd648421bd595768518_16.png)

上一节我们安装了基础的LAMP环境，本节中我们将利用这个环境来部署一个具体的漏洞靶场——DVWA。

### DVWA简介
DVWA (Damn Vulnerable Web Application) 是一个基于PHP/MySQL的Web应用程序。它集成了OWASP Top 10中常见的Web漏洞（如SQL注入、XSS、文件上传等），并分为低、中、高、不可能四个安全等级，主要用于安全人员学习和测试漏洞利用技术。

![](img/84a685713de25dd648421bd595768518_18.png)

![](img/84a685713de25dd648421bd595768518_20.png)

### 部署步骤
以下是部署DVWA的具体步骤：

![](img/84a685713de25dd648421bd595768518_22.png)

![](img/84a685713de25dd648421bd595768518_23.png)

![](img/84a685713de25dd648421bd595768518_25.png)

![](img/84a685713de25dd648421bd595768518_26.png)

1.  **下载DVWA**：从GitHub官方仓库下载源码压缩包。
2.  **解压文件**：使用 `unzip` 命令解压下载的ZIP包。
    ```bash
    unzip DVWA-master.zip
    ```
3.  **移动到Web根目录**：Apache的默认Web根目录通常是 `/var/www/html/`。将解压后的DVWA文件夹移动到此目录。
    ```bash
    mv DVWA-master /var/www/html/dvwa
    ```
4.  **配置DVWA**：
    *   进入DVWA的配置目录。
        ```bash
        cd /var/www/html/dvwa/config/
        ```
    *   复制配置文件模板。
        ```bash
        cp config.inc.php.dist config.inc.php
        ```
    *   编辑 `config.inc.php` 文件，主要设置数据库密码（如果MariaDB的root用户密码为空，则留空）。
        ```php
        $_DVWA[ 'db_password' ] = '';
        ```
5.  **配置PHP**：启用`allow_url_include`功能，以支持DVWA的某些模块。
    *   编辑PHP配置文件（路径可能为 `/etc/php/7.3/apache2/php.ini`）。
    *   找到 `allow_url_include` 这一行，将其值改为 `On`。
    *   保存文件并重启Apache服务。
        ```bash
        systemctl restart apache2
        ```
6.  **安装PHP扩展**：确保已安装`php-gd`扩展（用于验证码功能）。
    ```bash
    apt install php-gd -y
    systemctl restart apache2
    ```
7.  **设置文件权限**：为DVWA所需的目录赋予写权限。
    ```bash
    chmod -R 777 /var/www/html/dvwa/hackable/uploads/
    chmod -R 777 /var/www/html/dvwa/external/phpids/0.6/lib/IDS/tmp/phpids_log.txt
    chmod 777 /var/www/html/dvwa/config/
    ```
8.  **创建数据库用户**：由于MariaDB的安全限制，建议不使用root用户连接DVWA。创建一个专用用户。
    ```bash
    # 登录MariaDB
    mysql -u root
    # 在MariaDB命令行中执行
    CREATE DATABASE dvwa;
    CREATE USER 'dvwa'@'localhost' IDENTIFIED BY '';
    GRANT ALL PRIVILEGES ON dvwa.* TO 'dvwa'@'localhost';
    FLUSH PRIVILEGES;
    exit;
    ```
    *   同时，记得在 `config.inc.php` 中将数据库用户改为 `dvwa`。
9.  **访问并安装**：在浏览器中访问 `http://localhost/dvwa/`。按照页面提示，点击“Create / Reset Database”按钮。完成后，使用默认账号（admin）和密码（password）登录即可。

---

![](img/84a685713de25dd648421bd595768518_28.png)

![](img/84a685713de25dd648421bd595768518_30.png)

## 在Windows上使用PHPStudy部署环境

![](img/84a685713de25dd648421bd595768518_32.png)

在Linux上部署环境需要对系统有一定了解。对于初学者，在Windows上使用集成环境会更加简单快捷。

### PHPStudy简介
PHPStudy是一个Windows下的PHP调试环境程序集成包。它集成了Apache、Nginx、MySQL、PHP等多种环境，一键即可启动，极大简化了环境搭建流程。

![](img/84a685713de25dd648421bd595768518_34.png)

![](img/84a685713de25dd648421bd595768518_36.png)

![](img/84a685713de25dd648421bd595768518_37.png)

**注意**：请务必从官网下载最新版本，旧版本可能存在安全后门。

![](img/84a685713de25dd648421bd595768518_39.png)

![](img/84a685713de25dd648421bd595768518_41.png)

![](img/84a685713de25dd648421bd595768518_43.png)

### 部署DVWA（Windows版）
1.  **安装并启动PHPStudy**：安装后，在面板上启动Apache和MySQL服务。
2.  **放置源码**：将DVWA源码文件夹放入PHPStudy的WWW根目录（例如 `D:\phpstudy_pro\WWW\`），并重命名为 `dvwa`。
3.  **配置**：
    *   重命名 `config.inc.php.dist` 为 `config.inc.php`。
    *   在PHPStudy面板中，打开PHP配置文件（php.ini），将 `allow_url_include` 设置为 `On`，并重启服务。
    *   在 `config.inc.php` 中配置数据库密码（PHPStudy默认root密码为 `root`）。
4.  **访问安装**：浏览器访问 `http://localhost/dvwa/`，点击创建数据库，然后登录。

![](img/84a685713de25dd648421bd595768518_45.png)

![](img/84a685713de25dd648421bd595768518_47.png)

![](img/84a685713de25dd648421bd595768518_49.png)

### 部署其他CMS（如熊海CMS）
步骤与DVWA类似：
1.  将CMS源码放入WWW根目录。
2.  访问该目录，通常会进入安装界面。
3.  根据提示输入数据库信息（数据库名、用户名、密码）。
4.  **关键点**：如果安装程序提示数据库不存在，需要手动创建。可以通过PHPStudy自带的数据库管理工具或命令行创建。
    ```sql
    CREATE DATABASE `cms_db_name`;
    ```
5.  完成安装。

---

## Java与Python环境安装

为了运行后续课程中涉及的安全工具（如Burp Suite），我们需要配置Java和Python环境。

![](img/84a685713de25dd648421bd595768518_51.png)

![](img/84a685713de25dd648421bd595768518_53.png)

### Java环境安装 (Windows)
1.  **下载JDK**：从Oracle官网下载JDK 8的Windows安装包。
2.  **安装**：运行安装程序，记下安装路径（如 `C:\Program Files\Java\jdk1.8.0_291`）。
3.  **配置环境变量**：
    *   **JAVA_HOME**：新建系统变量，变量值为JDK安装路径（如 `C:\Program Files\Java\jdk1.8.0_291`）。
    *   **CLASSPATH**：新建系统变量，变量值为 `.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;`。
    *   **Path**：编辑系统变量，**新增**两条：`%JAVA_HOME%\bin` 和 `%JAVA_HOME%\jre\bin`。
4.  **验证**：打开CMD，输入 `java -version` 和 `javac`，能显示版本信息即成功。

![](img/84a685713de25dd648421bd595768518_55.png)

![](img/84a685713de25dd648421bd595768518_57.png)

![](img/84a685713de25dd648421bd595768518_58.png)

![](img/84a685713de25dd648421bd595768518_60.png)

### Java环境安装 (Linux - Kali)
Kali Linux通常已安装Java。如需安装或确认，可使用包管理器：
```bash
# 查看可用版本
apt list openjdk*
# 安装 OpenJDK 8
apt install openjdk-8-jdk -y
# 验证
java -version
```

![](img/84a685713de25dd648421bd595768518_62.png)

![](img/84a685713de25dd648421bd595768518_64.png)

### Python环境安装 (Windows)
1.  **下载安装包**：从Python官网下载安装程序（推荐3.7或3.8版本）。
2.  **安装**：运行安装程序。**务必勾选 “Add Python 3.x to PATH”**，这样安装程序会自动配置环境变量。建议选择“Customize installation”自定义安装路径。
3.  **验证**：打开CMD，输入 `python`，能进入交互式命令行即成功。

![](img/84a685713de25dd648421bd595768518_66.png)

![](img/84a685713de25dd648421bd595768518_67.png)

![](img/84a685713de25dd648421bd595768518_69.png)

### Python环境安装 (Linux - Kali)
Kali Linux默认已安装Python 2和Python 3。可直接使用 `python` 或 `python3` 命令。

![](img/84a685713de25dd648421bd595768518_71.png)

![](img/84a685713de25dd648421bd595768518_73.png)

![](img/84a685713de25dd648421bd595768518_75.png)

---

![](img/84a685713de25dd648421bd595768518_77.png)

![](img/84a685713de25dd648421bd595768518_79.png)

![](img/84a685713de25dd648421bd595768518_81.png)

![](img/84a685713de25dd648421bd595768518_83.png)

## 安全工具Burp Suite社区版激活

![](img/84a685713de25dd648421bd595768518_85.png)

![](img/84a685713de25dd648421bd595768518_87.png)

![](img/84a685713de25dd648421bd595768518_88.png)

![](img/84a685713de25dd648421bd595768518_90.png)

Burp Suite是Web安全测试中不可或缺的抓包、改包、扫描工具。社区版免费但功能有限，专业版需要许可证。以下是一种激活社区版的方法（仅供学习交流，请支持正版）。

**激活步骤简述**：
1.  下载提供的Burp Suite文件包。
2.  运行破解补丁 (`burp-loader-keygen.jar`)。
3.  点击 `Run` 启动Burp，在许可证窗口将生成的许可证复制进去。
4.  手动激活：将 `License Request` 复制到破解补丁，生成 `Activation Response` 并复制回Burp。
5.  激活成功后，可编写批处理脚本方便日后启动。

**注意**：Kali Linux中通常预装了更新版本的Burp Suite社区版，可直接使用。

![](img/84a685713de25dd648421bd595768518_92.png)

---

![](img/84a685713de25dd648421bd595768518_94.png)

![](img/84a685713de25dd648421bd595768518_96.png)

![](img/84a685713de25dd648421bd595768518_98.png)

## 总结
本节课中我们一起学习了网络安全学习环境的搭建。我们从LAMP基础环境讲起，分别在Linux和Windows系统上成功部署了DVWA漏洞靶场，为后续的漏洞原理学习和实操打下了基础。同时，我们也配置了Java和Python运行环境，并介绍了核心安全工具Burp Suite的获取与基本使用。掌握这些环境的搭建，是你迈向Web安全实践的第一步。