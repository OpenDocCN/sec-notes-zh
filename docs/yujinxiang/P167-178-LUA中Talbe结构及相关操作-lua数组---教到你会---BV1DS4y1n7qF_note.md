# 课程 P167：Lua 中的 Table 结构及相关操作 - 数组篇 📚

在本节课中，我们将要学习 Lua 语言中一个核心且强大的数据结构：**Table**。我们将重点探讨它作为数组使用时的定义、初始化、遍历以及内置函数操作。通过本教程，你将掌握如何创建和使用 Lua 数组。

---

![](img/d72ad17a76c1c8ed2ba75932b1401348_1.png)

## 概述 📖

![](img/d72ad17a76c1c8ed2ba75932b1401348_3.png)

Table 是 Lua 中唯一的数据结构，它非常灵活，可以模拟数组、字典等多种结构。本节课，我们将聚焦于 Table 作为数组的用法，了解其与 C++ 等语言中数组的异同，并学习相关的操作函数。

---

## Table 的定义与初始化

Table 的定义非常简单。我们可以创建一个空表，也可以直接为其赋予初始值。

```lua
-- 定义一个空表
local myTable = {}

-- 定义一个包含初始值的表（类似数组）
local myArray = {1, 2, 0x15, 4, 5, 6, 7}
```

在 Lua 中，数组的下标**默认从 1 开始**，而不是 0。因此，访问元素的方式如下：

```lua
print(myArray[1]) -- 输出：1
print(myArray[2]) -- 输出：2
print(myArray[3]) -- 输出：21 (0x15的十进制值)
```

---

## 遍历 Table（数组）

为了遍历整个数组，我们可以使用 Lua 内置的 `table.getn` 函数（或 `#` 操作符）来获取数组的长度。

以下是遍历数组的一个示例函数：

```lua
function printTable(t)
    local n = #t -- 或 table.getn(t)，获取表的大小
    print("开始遍历表：")
    for i = 1, n do
        print(t[i])
    end
    print("遍历表结束。")
end

-- 调用函数遍历之前定义的数组
printTable(myArray)
```

执行上述代码，将依次输出数组中的元素：1, 2, 21, 4, 5, 6, 7。

我们也可以先获取表的大小，再进行操作：

```lua
local size = #myArray
print("表的大小是：" .. size) -- 输出：表的大小是：7
```

---

## 动态操作数组元素

Table 作为数组，其大小是动态的。我们可以方便地增加、修改或删除元素。

上一节我们介绍了如何定义和遍历数组，本节中我们来看看如何动态地操作它。

### 使用循环初始化数组

我们可以通过循环来动态地为数组赋值。

```lua
local dynamicArray = {}
for i = 1, 10 do
    dynamicArray[i] = i
end
printTable(dynamicArray) -- 将输出 1 到 10
```

### 在数组末尾追加元素

我们可以在现有数组的基础上继续添加元素。

```lua
local n = #dynamicArray -- 获取当前数组长度
for i = n + 1, n + 10 do
    dynamicArray[i] = i
end
printTable(dynamicArray) -- 将输出 1 到 20
```

---

## Table 的内置函数操作

Lua 为 Table 提供了一些非常实用的内置函数，用于排序、插入和删除元素。

### 排序数组

我们可以使用 `table.sort` 函数对数组进行**升序**排序。**请注意，此函数要求数组中的所有元素都是数字类型。**

```lua
local unsortedArray = {33, 22, 11, 3, 2}
table.sort(unsortedArray)
printTable(unsortedArray) -- 输出：2, 3, 11, 22, 33
```

### 在指定位置插入元素

使用 `table.insert` 函数可以在数组的任意位置插入一个新元素，其后的元素会自动后移。

以下是 `table.insert` 函数的用法：

```lua
-- 语法：table.insert(table, [pos,] value)
-- 在位置`pos`插入`value`，如果省略`pos`，则默认插入到末尾。

local testArray = {1, 3, 4, 5}
table.insert(testArray, 2, 2) -- 在第二个位置插入数字2
printTable(testArray) -- 输出：1, 2, 3, 4, 5
```

### 移除指定位置的元素

使用 `table.remove` 函数可以移除数组中指定位置的元素，其后的元素会自动前移。

以下是 `table.remove` 函数的用法：

```lua
-- 语法：table.remove(table, [pos])
-- 移除位置`pos`的元素并返回它，如果省略`pos`，则默认移除最后一个元素。

local testArray = {1, 2, 9, 3, 4}
table.remove(testArray, 3) -- 移除第三个元素（数字9）
printTable(testArray) -- 输出：1, 2, 3, 4
```

---

## 注意事项与总结

本节课我们一起学习了 Lua 中 Table 结构作为数组的基本用法。我们来回顾一下重点：

1.  **定义与访问**：Table 使用花括号 `{}` 定义，下标从 1 开始。
2.  **遍历**：使用 `#` 操作符获取长度，配合 `for` 循环进行遍历。
3.  **动态性**：数组大小可变，可以随时增删元素。
4.  **内置函数**：
    *   `table.sort(t)`：对数组进行升序排序（要求元素为数字）。
    *   `table.insert(t, pos, value)`：在指定位置插入元素。
    *   `table.remove(t, pos)`：移除指定位置的元素。

**重要提示**：`table.sort` 函数要求数组内的所有元素都是可比较的数字。如果数组中包含字符串或其他类型，排序可能会产生错误或非预期结果。

![](img/d72ad17a76c1c8ed2ba75932b1401348_5.png)

Table 的功能远不止于此，它还可以作为字典（键值对）使用，并支持多维结构。我们将在后续课程中继续探讨 Table 更强大的功能。