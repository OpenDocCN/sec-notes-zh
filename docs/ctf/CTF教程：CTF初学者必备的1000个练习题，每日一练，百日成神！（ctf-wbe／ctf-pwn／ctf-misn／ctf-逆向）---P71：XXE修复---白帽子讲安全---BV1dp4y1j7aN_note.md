# CTF教程：P71：XXE修复 🔧

![](img/949a84e60517a3ab4f99d5a5885c3e6c_1.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_3.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_5.png)

在本节课中，我们将学习XML外部实体注入漏洞的修复方法。上一节我们介绍了通过上传Excel文件触发XXE漏洞的利用方式，本节中我们来看看如何从开发和安全防护的角度修复此类漏洞，并完成相关练习。

![](img/949a84e60517a3ab4f99d5a5885c3e6c_7.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_9.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_11.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_13.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_15.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_17.png)

## 概述
本节课将介绍XXE漏洞的两种主要修复方案，并通过代码示例演示如何在Java、PHP和Python中实施修复。课程最后将布置相关的实战练习。

![](img/949a84e60517a3ab4f99d5a5885c3e6c_19.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_20.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_22.png)

## XXE漏洞修复方案

![](img/949a84e60517a3ab4f99d5a5885c3e6c_24.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_26.png)

### 方案一：过滤用户输入
此方案的核心思想是对用户提交的XML数据进行严格的过滤和检查。以下是需要过滤的关键词和符号：

![](img/949a84e60517a3ab4f99d5a5885c3e6c_28.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_30.png)

*   **特殊符号**：例如尖括号 `<`、`>` 和连接符 `&`。
*   **DTD声明关键词**：例如 `<!DOCTYPE`、`<!ENTITY`。
*   **外部实体声明关键词**：例如 `SYSTEM`、`PUBLIC`。

![](img/949a84e60517a3ab4f99d5a5885c3e6c_32.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_34.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_36.png)

**注意**：基于黑名单的过滤方式可能存在被绕过风险，因此不是最推荐的修复方案。

### 方案二：禁用外部实体解析（推荐）
更安全有效的方式是在解析XML时，直接禁用外部实体的加载功能。不同编程语言的实现方式如下：

**Java修复代码示例**
在Java中，通常通过设置解析器的属性来实现。
```java
documentBuilderFactory.setFeature("http://apache.org/xml/features/disallow-doctype-decl", true);
documentBuilderFactory.setFeature("http://xml.org/sax/features/external-general-entities", false);
documentBuilderFactory.setFeature("http://xml.org/sax/features/external-parameter-entities", false);
```
将 `DocumentBuilderFactory` 的相关特性设置为 `true` 或 `false`，即可禁用DTD和外部实体。

![](img/949a84e60517a3ab4f99d5a5885c3e6c_38.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_40.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_42.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_44.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_45.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_47.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_49.png)

**PHP修复代码示例**
在PHP中，使用 `libxml_disable_entity_loader` 函数。
```php
libxml_disable_entity_loader(true);
```
将参数设置为 `true`，表示禁止加载外部实体。

![](img/949a84e60517a3ab4f99d5a5885c3e6c_51.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_53.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_55.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_56.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_58.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_59.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_61.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_62.png)

**Python修复代码示例**
在Python的`lxml`库中，解析XML时设置相关参数。
```python
parser = etree.XMLParser(resolve_entities=False)
tree = etree.parse(xml_source, parser)
```
将 `resolve_entities` 参数设置为 `False`，解析器将不会解析实体。

## 修复效果验证
以Java代码为例，修复后再次发送包含恶意外部实体引用的Payload，解析器会抛出异常（如“文件提前结束”或“无法加载外部实体”），而不会发起外部网络请求或读取本地文件，从而成功防御了XXE攻击。

![](img/949a84e60517a3ab4f99d5a5885c3e6c_64.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_66.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_68.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_70.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_72.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_74.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_76.png)

## 课程练习 🎯
以下是本节课需要完成的实战练习，用于巩固XXE漏洞的利用与修复知识：

![](img/949a84e60517a3ab4f99d5a5885c3e6c_78.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_79.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_80.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_82.png)

*   **靶场练习**：完成靶场中关于XXE有回显和无回显漏洞的题目（例如题目ID涉及47、005、75、177等）。成功标志通常是获取到敏感信息或触发“登录成功”状态。
*   **实验回顾**：如果之前的“Java中常见解析Excel引入的XXE组件漏洞分析”实验未完成，请继续完成。

![](img/949a84e60517a3ab4f99d5a5885c3e6c_84.png)

![](img/949a84e60517a3ab4f99d5a5885c3e6c_85.png)

## 总结
本节课我们一起学习了XXE漏洞的修复方法。核心要点是，相较于过滤用户输入，更优的方案是在代码层面直接禁用XML解析器的外部实体加载功能。我们在Java、PHP和Python中分别看到了具体的实现代码。通过修复，可以有效地阻止攻击者利用XXE漏洞读取敏感文件或发起网络请求。