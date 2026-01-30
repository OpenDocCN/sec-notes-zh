![](img/4bccc80ed190dda61de46ed7f90f8716_0.png)

# 课程 P161：在 C/C++ 中获取 Lua 函数返回值 📥

在本节课中，我们将学习如何在 C/C++ 程序中调用 Lua 函数并获取其返回值。我们将重点理解 `lua_pcall` 函数的调用规范、参数与返回值的压栈顺序，并通过一个完整的代码示例来实践。

![](img/4bccc80ed190dda61de46ed7f90f8716_2.png)

---

## 调用 Lua 函数的基本要求

上一节我们介绍了 Lua 虚拟栈的基本操作。本节中我们来看看如何通过 C 代码调用 Lua 函数。

![](img/4bccc80ed190dda61de46ed7f90f8716_4.png)

![](img/4bccc80ed190dda61de46ed7f90f8716_6.png)

要正确调用 Lua 函数并获取其返回值，必须遵循特定的栈操作协议。核心是通过 `lua_pcall` 函数进行调用。

![](img/4bccc80ed190dda61de46ed7f90f8716_8.png)

![](img/4bccc80ed190dda61de46ed7f90f8716_9.png)

以下是调用前必须完成的栈准备工作：
1.  将要调用的函数压入栈顶。
2.  将函数的参数按正序（第一个参数先入栈）压入栈中。

调用完成后，函数的返回值会按正序被推入栈中。

---

## Lua 函数的 C 语言接口规范

为了确保 C 函数能与 Lua 正确通信，所有注册给 Lua 的 C 函数都必须遵循统一的接口定义和参数传递协议。

以下是 C 函数的标准格式和参数获取方式：
*   **函数定义**：`int function_name(lua_State *L)`
*   **参数获取**：函数开始时，参数已按正序压入栈中。第一个参数（如果存在）在索引 **1** 的位置，最后一个参数在索引 **`lua_gettop(L)`** 的位置。`lua_gettop(L)` 返回的值就是传入参数的个数。
*   **返回值**：C 函数通过将返回值压入栈中，并返回一个整数来告知 Lua 返回值个数。例如，`return 1;` 表示有 1 个返回值在栈顶。

---

## 实践：编写一个累加函数

现在，我们通过一个具体的例子来实践上述理论。我们将创建一个 C 函数，在 Lua 中注册它，然后从 C 端调用它并获取计算结果。

首先，我们创建一个新的项目，并复制上一节课的基础代码框架，用于初始化 Lua 状态。

### 1. 定义并注册 C 函数

![](img/4bccc80ed190dda61de46ed7f90f8716_11.png)

我们定义一个名为 `add_number` 的 C 函数，它接收多个整数参数，计算它们的累加和，并将结果返回。

![](img/4bccc80ed190dda61de46ed7f90f8716_13.png)

```c
// 定义给Lua调用的C函数
static int add_number(lua_State *L) {
    int n = lua_gettop(L); // 获取参数个数
    int sum = 0;
    for (int i = 1; i <= n; i++) {
        sum += lua_tointeger(L, i); // 依次获取每个参数并累加
    }
    lua_pushinteger(L, sum); // 将结果压入栈顶
    return 1; // 返回值的个数为1
}

![](img/4bccc80ed190dda61de46ed7f90f8716_15.png)

![](img/4bccc80ed190dda61de46ed7f90f8716_17.png)

int main() {
    lua_State *L = luaL_newstate(); // 创建Lua状态机
    luaL_openlibs(L); // 打开标准库

    // 将C函数注册为Lua的全局函数
    lua_register(L, "add_number", add_number);
    // ... 后续调用代码
}
```

![](img/4bccc80ed190dda61de46ed7f90f8716_19.png)

### 2. 从 C 端调用 Lua 函数

![](img/4bccc80ed190dda61de46ed7f90f8716_21.png)

注册函数后，我们需要在 C 代码中主动调用它。这需要使用 `lua_pcall` 函数。

以下是调用 `add_number` 并传入 8 个参数的完整步骤：
```c
// 1. 将需要调用的函数（add_number）压入栈顶
lua_getglobal(L, "add_number");

// 2. 将8个参数按正序压入栈中
lua_pushinteger(L, 1);
lua_pushinteger(L, 2);
lua_pushinteger(L, 3);
lua_pushinteger(L, 4);
lua_pushinteger(L, 5);
lua_pushinteger(L, 6);
lua_pushinteger(L, 7);
lua_pushinteger(L, 8);

// 3. 调用函数，告知有8个参数，期望1个返回值
if (lua_pcall(L, 8, 1, 0) != LUA_OK) {
    printf("调用错误: %s\n", lua_tostring(L, -1));
}

![](img/4bccc80ed190dda61de46ed7f90f8716_23.png)

![](img/4bccc80ed190dda61de46ed7f90f8716_25.png)

// 4. 调用成功，返回值位于栈顶
int result = lua_tointeger(L, -1);
printf("累加结果为: %d\n", result);

// 5. 将返回值弹出栈，保持栈平衡
lua_pop(L, 1);
```

**关键点说明**：
*   `lua_pcall(L, 8, 1, 0)` 的三个数字参数分别代表：参数个数、期望的返回值个数、错误处理函数索引（0表示无）。
*   **参数个数必须与实际压栈的参数个数严格一致**，否则会导致调用错误或结果异常。
*   **期望的返回值个数必须与C函数实际返回的个数一致**，否则无法正确获取到所有返回值。

![](img/4bccc80ed190dda61de46ed7f90f8716_27.png)

![](img/4bccc80ed190dda61de46ed7f90f8716_29.png)

### 3. 处理多个返回值

Lua 函数可以返回多个值。如果我们的 C 函数返回多个值，并在 `lua_pcall` 中声明了相应的数量，就可以依次从栈中取出。

![](img/4bccc80ed190dda61de46ed7f90f8716_31.png)

![](img/4bccc80ed190dda61de46ed7f90f8716_33.png)

假设 `add_number` 函数修改为返回三个值（和、平均值、参数个数）：
```c
static int add_number(lua_State *L) {
    int n = lua_gettop(L);
    int sum = 0;
    for (int i = 1; i <= n; i++) {
        sum += lua_tointeger(L, i);
    }
    lua_pushinteger(L, sum);      // 第一个返回值：和
    lua_pushnumber(L, sum / (double)n); // 第二个返回值：平均值
    lua_pushinteger(L, n);        // 第三个返回值：参数个数
    return 3; // 声明返回3个值
}
```
调用时，我们需要相应地修改 `lua_pcall` 并获取所有返回值：
```c
// 调用函数，期望3个返回值
lua_pcall(L, 8, 3, 0);

![](img/4bccc80ed190dda61de46ed7f90f8716_35.png)

![](img/4bccc80ed190dda61de46ed7f90f8716_37.png)

// 返回值按正序入栈。栈底是第一个返回值，栈顶是最后一个。
// 索引 -3, -2, -1 分别对应三个返回值。
int sum = lua_tointeger(L, -3);
double avg = lua_tonumber(L, -2);
int count = lua_tointeger(L, -1);

![](img/4bccc80ed190dda61de46ed7f90f8716_39.png)

printf("和:%d, 平均值:%.2f, 参数个数:%d\n", sum, avg, count);

![](img/4bccc80ed190dda61de46ed7f90f8716_41.png)

// 弹出三个返回值
lua_pop(L, 3);
```

![](img/4bccc80ed190dda61de46ed7f90f8716_43.png)

---

## 总结

![](img/4bccc80ed190dda61de46ed7f90f8716_45.png)

本节课中我们一起学习了在 C/C++ 中与 Lua 交互的核心操作——调用 Lua 函数并获取返回值。

![](img/4bccc80ed190dda61de46ed7f90f8716_47.png)

![](img/4bccc80ed190dda61de46ed7f90f8716_49.png)

我们掌握了以下关键知识：
1.  **调用规范**：使用 `lua_pcall` 前，必须先将函数和其参数按规则压入虚拟栈。
2.  **C函数接口**：遵循 `int func(lua_State *L)` 格式，用 `lua_gettop` 获取参数，用 `return n` 声明返回值数量。
3.  **参数与返回值的对应关系**：`lua_pcall` 中声明的参数个数、返回值个数必须与实际压栈的数量、C函数返回的数量严格一致，这是正确交互的基石。
4.  **多返回值处理**：通过调整 `lua_pcall` 的期望返回值个数和正确使用栈索引，可以轻松处理多个返回值。

![](img/4bccc80ed190dda61de46ed7f90f8716_51.png)

通过本课的实践，你已经能够打通 C/C++ 与 Lua 之间的函数调用通道。下一节课，我们将继续探索 Lua 与其他更复杂的数据结构进行交互。