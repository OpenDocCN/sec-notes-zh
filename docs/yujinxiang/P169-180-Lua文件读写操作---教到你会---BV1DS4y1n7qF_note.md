![](img/7cec48798f8140f1c1fa8bfb6c4b5515_1.png)

# 课程 P169：Lua 文件读写操作 📂

![](img/7cec48798f8140f1c1fa8bfb6c4b5515_3.png)

在本节课中，我们将学习 Lua 语言中如何进行文件的基本读写操作。文件操作是编程中处理数据持久化的重要环节，掌握它可以帮助你将程序运行的结果保存下来，或者从外部文件中读取配置和数据。

![](img/7cec48798f8140f1c1fa8bfb6c4b5515_5.png)

---

## 概述

本节课将介绍 Lua 内置的 `io` 库，它提供了打开、读取、写入和关闭文件的功能。我们将通过具体的代码示例，学习如何创建新文件、向文件中写入数据，以及如何从现有文件中读取内容。

---

## 创建与写入文件 ✍️

在 Lua 中，创建和写入文件主要使用 `io.open` 函数。该函数接受两个参数：文件名和打开模式。当模式为 `"w"`（写入）时，如果文件不存在则会创建新文件；如果文件已存在，则会清空原有内容。

![](img/7cec48798f8140f1c1fa8bfb6c4b5515_7.png)

以下是创建并写入文件的核心代码：

![](img/7cec48798f8140f1c1fa8bfb6c4b5515_9.png)

![](img/7cec48798f8140f1c1fa8bfb6c4b5515_11.png)

```lua
local function createFile()
    local myFile = io.open("test.lua", "w")
    if myFile ~= nil then
        myFile:write("第一行文本" .. string.char(10)) -- 使用 string.char(10) 换行
        myFile:write("第二行文本\n") -- 使用转义字符 \n 换行
        myFile:close()
        print("文件创建并写入成功。")
    else
        print("文件创建失败。")
    end
end

![](img/7cec48798f8140f1c1fa8bfb6c4b5515_13.png)

createFile()
```

![](img/7cec48798f8140f1c1fa8bfb6c4b5515_15.png)

**代码解析：**
1.  `io.open("test.lua", "w")` 尝试以写入模式打开文件。成功则返回一个文件句柄，失败则返回 `nil`。
2.  `myFile:write(...)` 方法用于向文件写入字符串。可以使用 `.. string.char(10)` 或 `\n` 实现换行。
3.  操作完成后，必须调用 `myFile:close()` 来关闭文件，确保数据被正确保存。

![](img/7cec48798f8140f1c1fa8bfb6c4b5515_17.png)

![](img/7cec48798f8140f1c1fa8bfb6c4b5515_19.png)

上一节我们介绍了如何创建和写入文件，本节中我们来看看如何读取文件中的内容。

---

## 读取文件内容 📖

读取文件同样使用 `io.open` 函数，但模式需改为 `"r"`（读取）。Lua 提供了多种读取文件内容的方法。

### 方法一：使用 `while` 循环逐行读取

这是一种常见的读取方式，使用 `file:read("*l")` 每次读取一行。

```lua
local function readFileWithWhile()
    local fileName = "test.lua"
    local file = io.open(fileName, "r")
    if file then
        local contentLine = file:read("*l")
        while contentLine do
            print(contentLine)
            contentLine = file:read("*l")
        end
        file:close()
    else
        print("无法打开文件: " .. fileName)
    end
end

readFileWithWhile()
```

### 方法二：使用 `for` 循环与迭代器

![](img/7cec48798f8140f1c1fa8bfb6c4b5515_21.png)

Lua 的 `io.lines` 函数或 `file:lines()` 方法可以返回一个迭代器，使代码更简洁。

![](img/7cec48798f8140f1c1fa8bfb6c4b5515_23.png)

```lua
local function readFileWithFor()
    local fileName = "test.lua"
    local file = io.open(fileName, "r")
    if file then
        for line in file:lines() do
            print(line)
        end
        file:close()
    end
end

![](img/7cec48798f8140f1c1fa8bfb6c4b5515_25.png)

readFileWithFor()
```

![](img/7cec48798f8140f1c1fa8bfb6c4b5515_27.png)

![](img/7cec48798f8140f1c1fa8bfb6c4b5515_29.png)

**两种方法的对比：**
*   `while` 循环方式更基础，可以更灵活地控制读取过程（例如在读取过程中进行条件判断）。
*   `for` 循环方式代码更简洁，是遍历文件每一行的推荐做法。

以下是 `file:read` 函数支持的部分格式化读取模式：

*   `"*l"`： 读取下一行（默认模式）。
*   `"*a"`： 读取整个文件。
*   `"*n"`： 读取一个数字。
*   `number`： 读取指定字节数的字符（例如 `file:read(5)` 读取5个字节）。

---

![](img/7cec48798f8140f1c1fa8bfb6c4b5515_31.png)

## 其他文件操作模式

![](img/7cec48798f8140f1c1fa8bfb6c4b5515_33.png)

除了 `"w"`（写入）和 `"r"`（读取）模式，`io.open` 还支持其他有用的模式：

![](img/7cec48798f8140f1c1fa8bfb6c4b5515_35.png)

*   `"a"`： 追加模式。如果文件存在，则在文件末尾追加内容；如果不存在，则创建新文件。
*   `"r+"`： 读写模式。文件必须存在。
*   `"w+"`： 读写模式。会清空原文件或创建新文件。
*   `"a+"`： 追加读写模式。

![](img/7cec48798f8140f1c1fa8bfb6c4b5515_37.png)

例如，以追加模式写入文件：
```lua
local appendFile = io.open("log.txt", "a")
appendFile:write("这是一条新的日志记录\n")
appendFile:close()
```

![](img/7cec48798f8140f1c1fa8bfb6c4b5515_39.png)

![](img/7cec48798f8140f1c1fa8bfb6c4b5515_41.png)

---

![](img/7cec48798f8140f1c1fa8bfb6c4b5515_42.png)

![](img/7cec48798f8140f1c1fa8bfb6c4b5515_44.png)

## 总结

![](img/7cec48798f8140f1c1fa8bfb6c4b5515_46.png)

本节课中我们一起学习了 Lua 文件操作的核心知识：

![](img/7cec48798f8140f1c1fa8bfb6c4b5515_48.png)

1.  **写入文件**：使用 `io.open(filename, "w")` 打开文件，通过 `file:write()` 写入数据，最后用 `file:close()` 关闭。
2.  **读取文件**：使用 `io.open(filename, "r")` 打开文件，可以通过 `while` 循环配合 `file:read("*l")` 或更简洁的 `for line in file:lines()` 循环来逐行读取内容。
3.  **操作模式**：了解了 `"r"`、`"w"`、`"a"` 等不同文件打开模式的区别和用途。

![](img/7cec48798f8140f1c1fa8bfb6c4b5515_50.png)

文件读写是程序与外部世界交互的基础。掌握这些操作后，你就可以让 Lua 程序保存状态、读取配置或处理文本数据了。在后续课程中，我们会在需要时进一步探讨 `io` 库的其他功能。