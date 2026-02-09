# 🛡️ 课程 P32：第30天 - XXE漏洞基础

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_0.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_2.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_3.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_4.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_5.png)

在本节课中，我们将学习XML外部实体注入（XXE）漏洞的基础知识。课程将从XML和DTD的基础概念讲起，逐步深入到XXE漏洞的原理与初步利用方式。课程内容旨在让初学者能够理解并掌握核心概念。

---

## 📖 XML基础

上一节我们介绍了课程概述，本节中我们来看看XML的基础知识。

XML是一种用于标记电子文件，使其具有结构性的可扩展标记语言。它的形式与HTML语言类似，但XML没有固定的标签，所有标签都可以自定义。XML的设计宗旨是传输数据，而非像HTML一样用于前端显示。

以下是XML文档处理的基本流程：
1.  客户端生成XML文档并通过HTTP发送给服务端。
2.  服务端接收请求后，使用XML解析库进行解析。
3.  解析后，服务端可能将数据回显给客户端或进行其他处理。

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_7.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_9.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_11.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_12.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_13.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_15.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_16.png)

一个典型的XML文档结构示例如下：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<book id="1">
  <title>网络安全</title>
  <author>作者名</author>
  <year>2023</year>
  <price>100</price>
</book>
```

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_18.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_20.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_22.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_24.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_26.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_28.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_29.png)

XML文档必须遵循以下语法规则：
*   文档必须有一个根元素（如示例中的 `<book>`）。
*   XML元素必须有关闭标签。
*   XML对大小写敏感。
*   元素必须被正确嵌套。
*   属性值必须加引号。

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_31.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_33.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_35.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_37.png)

---

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_39.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_41.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_43.png)

## 📄 XML文档结构与DTD

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_45.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_47.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_49.png)

上一节我们介绍了XML的基本语法，本节中我们来看看XML文档的完整结构以及DTD的作用。

一个完整的XML文档通常包含以下部分：
1.  XML声明
2.  DTD文档类型定义（可选）
3.  文档元素

XML声明用于告知解析器文档的基本信息，例如：
```xml
<?xml version="1.0" encoding="UTF-8"?>
```
其中，`version` 指定XML版本，`encoding` 指定字符编码（如UTF-8）。

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_51.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_53.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_55.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_57.png)

DTD（文档类型定义）的作用是为XML文档定义语义约束。你可以将DTD理解为一个模板，它定义了用户自定义的根元素、子元素及其属性，确保XML文档的规范化。

DTD声明有两种方式：
*   **内部DTD声明**：直接写在XML文档内部。
    ```xml
    <!DOCTYPE note [
      <!ELEMENT note (to,from,heading,body)>
      <!ELEMENT to (#PCDATA)>
      <!ELEMENT from (#PCDATA)>
      <!ELEMENT heading (#PCDATA)>
      <!ELEMENT body (#PCDATA)>
    ]>
    ```
*   **外部DTD声明**：通过SYSTEM关键字引入外部DTD文件。
    ```xml
    <!DOCTYPE note SYSTEM "note.dtd">
    ```
    外部DTD文件（如 `note.dtd`）的内容与内部声明类似。

---

## 🔤 DTD中的PCDATA与CDATA

上一节我们了解了DTD的定义，本节中我们来看看DTD中两种重要的数据类型：PCDATA和CDATA。

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_59.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_61.png)

**PCDATA** 指的是被解析的字符数据。这意味着包含在PCDATA中的文本会被解析器检查实体和标记。如果文本中包含 `<`、`>` 或 `&` 等特殊字符，解析器会将其解释为标记的一部分，从而导致错误。为了避免错误，需要使用预定义的实体引用来替代这些字符，例如：
*   `&lt;` 代表 `<`
*   `&gt;` 代表 `>`
*   `&amp;` 代表 `&`

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_63.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_65.png)

**CDATA** 指的是不应由XML解析器进行解析的文本数据。CDATA部分中的所有内容都会被解析器忽略，原样输出。其语法如下：
```xml
<![CDATA[这里的内容不会被解析，即使包含 <, >, & 等字符]]>
```
需要注意的是，CDATA部分中不能包含其结束标记 `]]>`，也不允许嵌套CDATA。

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_67.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_69.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_71.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_73.png)

---

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_75.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_77.png)

## ⚙️ DTD实体

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_79.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_81.png)

上一节我们区分了PCDATA和CDATA，本节中我们进入核心部分：DTD实体。实体可以理解为可复用的数据单元。

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_83.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_85.png)

以下是DTD实体的主要类型：

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_87.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_88.png)

### 内部普通实体
内部普通实体在DTD内部定义和赋值，在文档元素中引用。
*   **声明方式**：`<!ENTITY 实体名 "实体值">`
*   **引用方式**：`&实体名;`
*   **示例**：
    ```xml
    <!ENTITY hello "World">
    <greeting>Hello &hello;</greeting>
    ```
    解析后输出：`Hello World`

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_90.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_92.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_94.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_96.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_98.png)

### 外部普通实体
外部普通实体通过SYSTEM或PUBLIC关键字引入外部资源。
*   **声明方式**：`<!ENTITY 实体名 SYSTEM "URI/文件路径">`
*   **引用方式**：`&实体名;`
*   **示例（读取文件）**：
    ```xml
    <!ENTITY file SYSTEM "file:///etc/passwd">
    <content>&file;</content>
    ```
    如果服务器存在XXE漏洞，且未做防护，此操作可能将 `/etc/passwd` 文件内容回显。
*   **示例（PHP伪协议）**：
    ```xml
    <!ENTITY file SYSTEM "php://filter/read=convert.base64-encode/resource=/etc/passwd">
    <content>&file;</content>
    ```
    通过Base64编码读取文件，可以避免特殊字符导致的解析错误。

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_100.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_102.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_104.png)

不同语言在引用外部实体时支持的协议可能不同，常见的有 `file://`、`http://`、`php://` 等。

---

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_106.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_108.png)

## 🎯 总结与漏洞原理初探

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_110.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_112.png)

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_114.png)

本节课中我们一起学习了XXE漏洞的基础知识。

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_116.png)

我们首先了解了XML的基本概念、语法和文档结构。然后，我们深入学习了DTD（文档类型定义），包括其作用、声明方式以及内部的PCDATA和CDATA数据类型。最后，我们重点探讨了DTD实体，特别是**外部普通实体**的声明与引用方式。

**XXE漏洞的核心原理**在于：当应用程序在解析用户可控的XML输入时，**未禁用外部实体的加载**，攻击者就可以构造恶意的DTD，通过外部实体声明来读取服务器上的任意文件（如 `file:///etc/passwd`）、发起内部网络请求，甚至在某些配置下导致拒绝服务（DoS）或远程代码执行。

![](img/fb446bdf65a87d4b63b7e5a49a1ad458_118.png)

本节课演示了利用外部实体读取文件的基本利用方式。理解XML、DTD和实体是掌握XXE漏洞的基础。在后续课程中，我们将学习更复杂的利用技巧及防御方法。