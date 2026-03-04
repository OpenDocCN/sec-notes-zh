# CTF入门教学：P10：排序函数 🧮

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_1.png)

在本节课中，我们将要学习PHP中的关联数组以及如何对数组进行排序。排序是数据处理中的基础操作，在CTF的Web题目中，理解数据如何被组织和处理至关重要。

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_2.png)

## 关联数组的定义与访问

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_4.png)

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_5.png)

上一节我们介绍了数组的基本概念，本节中我们来看看PHP中一种特殊的数组——关联数组。

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_7.png)

关联数组是使用开发者自定义的字符串作为键（key）的数组。它与普通索引数组不同，其键名具有具体的含义。

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_9.png)

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_10.png)

以下是创建关联数组的两种常见方法：

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_12.png)

1.  使用 `array()` 函数并指定键值对。
2.  使用简化的数组语法 `[]` 并指定键值对。

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_14.png)

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_15.png)

**代码示例：**
```php
// 方法一：使用array()函数
$age = array("Peter"=>"35", "Ben"=>"37", "Joe"=>"43");

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_17.png)

// 方法二：使用简写语法
$age = ["Peter"=>"35", "Ben"=>"37", "Joe"=>"43"];
```

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_19.png)

定义数组后，可以通过在数组变量后使用方括号 `[]` 并传入键名来访问对应的值。

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_21.png)

**代码示例：**
```php
echo $age['Peter']; // 输出：35
echo $age['Ben'];   // 输出：37
```

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_23.png)

## 遍历关联数组

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_25.png)

要遍历关联数组中的所有键值对，`foreach` 循环是最合适的方法。

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_27.png)

以下是使用 `foreach` 循环遍历关联数组的语法：

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_29.png)

```php
foreach ($array as $key => $value) {
    // 处理 $key 和 $value
}
```

**代码示例：**
```php
$age = ["Peter"=>"35", "Ben"=>"37", "Joe"=>"43"];
foreach ($age as $name => $age_value) {
    echo "Key=" . $name . ", Value=" . $age_value;
    echo "<br>";
}
```
这段代码会依次输出每一对键和值，例如：`Key=Peter, Value=35`。

## 数组排序函数

对数组元素进行排序是常见的操作。PHP提供了多种排序函数，可以根据值或键进行升序或降序排列。

我们可以将这些函数分为以下几对：
*   `sort()` / `rsort()` – 对数组的值进行升序/降序排序（重置数字索引）。
*   `asort()` / `arsort()` – 根据数组的值进行升序/降序排序（保持键值关联）。
*   `ksort()` / `krsort()` – 根据数组的键进行升序/降序排序。

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_31.png)

### 1. 按值排序（不保持键关联）

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_33.png)

`sort()` 函数对数组元素按值进行升序排序，并重新分配数字索引。

**代码示例：**
```php
$cars = array("Volvo", "BMW", "Toyota");
sort($cars);
// 排序后：$cars = ["BMW", "Toyota", "Volvo"]
```

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_35.png)

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_37.png)

`rsort()` 函数功能相反，进行降序排序。

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_38.png)

**代码示例：**
```php
$cars = array("Volvo", "BMW", "Toyota");
rsort($cars);
// 排序后：$cars = ["Volvo", "Toyota", "BMW"]
```

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_40.png)

### 2. 按值排序（保持键关联）

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_42.png)

`asort()` 函数根据数组的值进行升序排序，同时保持键和值之间的关联。

**代码示例：**
```php
$age = ["Peter"=>"35", "Ben"=>"37", "Joe"=>"43"];
asort($age);
// 排序后：$age = ["Peter"=>"35", "Ben"=>"37", "Joe"=>"43"]
// 注意：此例中值已是升序，若值乱序则会按值重新排列，但键跟随值移动。
```

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_44.png)

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_45.png)

`arsort()` 函数则根据值进行降序排序，同样保持键值关联。

### 3. 按键排序

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_47.png)

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_48.png)

`ksort()` 函数根据数组的键进行升序排序。

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_50.png)

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_52.png)

**代码示例：**
```php
$age = ["Peter"=>"43", "Ben"=>"37", "Joe"=>"35"];
ksort($age);
// 按键名字母升序排序后：$age = ["Ben"=>"37", "Joe"=>"35", "Peter"=>"43"]
```

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_54.png)

`krsort()` 函数根据键进行降序排序。

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_56.png)

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_58.png)

**代码示例：**
```php
$age = ["Peter"=>"43", "Ben"=>"37", "Joe"=>"35"];
krsort($age);
// 按键名字母降序排序后：$age = ["Peter"=>"43", "Joe"=>"35", "Ben"=>"37"]
```

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_60.png)

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_61.png)

## 总结

![](img/7e94cd4a89ab0e9f7f877b4a6f62298e_63.png)

本节课中我们一起学习了PHP关联数组的核心操作。
我们首先了解了如何使用键值对定义和访问关联数组，然后掌握了使用 `foreach` 循环遍历它的方法。
最后，我们系统学习了PHP中各类数组排序函数：`sort()`/`rsort()` 用于纯值排序，`asort()`/`arsort()` 用于在排序时保持键值关联，`ksort()`/`krsort()` 则用于根据键名排序。
理解这些数据操作是分析Web应用逻辑、发现潜在漏洞（如因排序逻辑不当导致的信息泄露）的重要基础。