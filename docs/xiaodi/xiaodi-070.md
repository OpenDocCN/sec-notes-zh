#  070：反序列化、PYC反编译与格式化字符串 🔐

![](img/a53f930152f1beb30c75097d9275ab2a_1.png)

![](img/a53f930152f1beb30c75097d9275ab2a_3.png)

![](img/a53f930152f1beb30c75097d9275ab2a_5.png)

![](img/a53f930152f1beb30c75097d9275ab2a_7.png)

![](img/a53f930152f1beb30c75097d9275ab2a_9.png)

![](img/a53f930152f1beb30c75097d9275ab2a_11.png)

![](img/a53f930152f1beb30c75097d9275ab2a_13.png)

![](img/a53f930152f1beb30c75097d9275ab2a_15.png)

![](img/a53f930152f1beb30c75097d9275ab2a_17.png)

![](img/a53f930152f1beb30c75097d9275ab2a_19.png)

![](img/a53f930152f1beb30c75097d9275ab2a_21.png)

![](img/a53f930152f1beb30c75097d9275ab2a_23.png)

![](img/a53f930152f1beb30c75097d9275ab2a_25.png)

在本节课中，我们将学习Python语言中三个特有的安全问题：PYC文件反编译、反序列化漏洞利用链以及格式化字符串安全。这些知识点在代码审计和逆向工程中较为常见，有助于我们更深入地理解Python应用的安全风险。

![](img/a53f930152f1beb30c75097d9275ab2a_27.png)

![](img/a53f930152f1beb30c75097d9275ab2a_29.png)

![](img/a53f930152f1beb30c75097d9275ab2a_31.png)

![](img/a53f930152f1beb30c75097d9275ab2a_33.png)

![](img/a53f930152f1beb30c75097d9275ab2a_35.png)

![](img/a53f930152f1beb30c75097d9275ab2a_37.png)

## 一、PYC文件反编译 🔄

![](img/a53f930152f1beb30c75097d9275ab2a_39.png)

![](img/a53f930152f1beb30c75097d9275ab2a_41.png)

![](img/a53f930152f1beb30c75097d9275ab2a_43.png)

![](img/a53f930152f1beb30c75097d9275ab2a_45.png)

![](img/a53f930152f1beb30c75097d9275ab2a_47.png)

![](img/a53f930152f1beb30c75097d9275ab2a_49.png)

![](img/a53f930152f1beb30c75097d9275ab2a_51.png)

上一节我们介绍了课程概述，本节中我们来看看PYC文件反编译。某些编程语言在将源码编译成可执行文件或包时，会生成中间文件（如Java的`.class`、.NET的`.dll`），这些文件可以被反编译以获取源码。Python也不例外，其编译后生成的`.pyc`文件同样可以反编译。

![](img/a53f930152f1beb30c75097d9275ab2a_53.png)

![](img/a53f930152f1beb30c75097d9275ab2a_55.png)

![](img/a53f930152f1beb30c75097d9275ab2a_57.png)

![](img/a53f930152f1beb30c75097d9275ab2a_59.png)

以下是支持反编译的语言及其文件类型：
*   **Java**: `.class`, `.jar` 文件，可使用JD-GUI等工具反编译。
*   **.NET**: `.dll` 文件，可使用 `dnSpy` 等工具反编译。
*   **Python**: `.pyc` 文件，可使用 `uncompyle6` 等模块反编译。

![](img/a53f930152f1beb30c75097d9275ab2a_61.png)

![](img/a53f930152f1beb30c75097d9275ab2a_63.png)

![](img/a53f930152f1beb30c75097d9275ab2a_65.png)

![](img/a53f930152f1beb30c75097d9275ab2a_67.png)

![](img/a53f930152f1beb30c75097d9275ab2a_69.png)

![](img/a53f930152f1beb30c75097d9275ab2a_71.png)

`.pyc`文件是Python源码（`.py`）编译后的字节码文件。当Python程序被打包成独立可执行文件（如`.exe`）时，通常会包含`.pyc`文件。获取这些文件后，即可尝试反编译出原始Python源码。

![](img/a53f930152f1beb30c75097d9275ab2a_73.png)

![](img/a53f930152f1beb30c75097d9275ab2a_75.png)

**如何进行PYC文件反编译？**
1.  安装反编译工具：`pip install uncompyle6`
2.  使用工具进行反编译。例如，对 `gui.pyc` 文件反编译并输出到 `gui.py`：
    ```bash
    uncompyle6 -o gui.py gui.pyc
    ```

![](img/a53f930152f1beb30c75097d9275ab2a_77.png)

![](img/a53f930152f1beb30c75097d9275ab2a_79.png)

![](img/a53f930152f1beb30c75097d9275ab2a_81.png)

![](img/a53f930152f1beb30c75097d9275ab2a_83.png)

![](img/a53f930152f1beb30c75097d9275ab2a_85.png)

![](img/a53f930152f1beb30c75097d9275ab2a_87.png)

![](img/a53f930152f1beb30c75097d9275ab2a_89.png)

![](img/a53f930152f1beb30c75097d9275ab2a_91.png)

此技术主要用于对Python编写的工具进行逆向分析和代码审计。在CTF的PWN（逆向工程）方向中可能出现相关题目，但在常规Web渗透测试中并不常见。

![](img/a53f930152f1beb30c75097d9275ab2a_93.png)

![](img/a53f930152f1beb30c75097d9275ab2a_95.png)

![](img/a53f930152f1beb30c75097d9275ab2a_97.png)

![](img/a53f930152f1beb30c75097d9275ab2a_99.png)

![](img/a53f930152f1beb30c75097d9275ab2a_101.png)

![](img/a53f930152f1beb30c75097d9275ab2a_103.png)

![](img/a53f930152f1beb30c75097d9275ab2a_105.png)

![](img/a53f930152f1beb30c75097d9275ab2a_107.png)

![](img/a53f930152f1beb30c75097d9275ab2a_109.png)

## 二、Python反序列化漏洞 ⛓️

![](img/a53f930152f1beb30c75097d9275ab2a_111.png)

![](img/a53f930152f1beb30c75097d9275ab2a_113.png)

![](img/a53f930152f1beb30c75097d9275ab2a_115.png)

上一节我们介绍了PYC反编译，本节中我们来看看Python中的反序列化漏洞。序列化是将对象状态转换为可存储或传输的字节流的过程，反序列化则是其逆过程。Python中内置的`pickle`模块常用于序列化。

![](img/a53f930152f1beb30c75097d9275ab2a_117.png)

![](img/a53f930152f1beb30c75097d9275ab2a_119.png)

![](img/a53f930152f1beb30c75097d9275ab2a_121.png)

![](img/a53f930152f1beb30c75097d9275ab2a_123.png)

![](img/a53f930152f1beb30c75097d9275ab2a_125.png)

![](img/a53f930152f1beb30c75097d9275ab2a_127.png)

![](img/a53f930152f1beb30c75097d9275ab2a_129.png)

![](img/a53f930152f1beb30c75097d9275ab2a_131.png)

![](img/a53f930152f1beb30c75097d9275ab2a_133.png)

**Python中常见的序列化/反序列化函数及模块：**
*   `pickle.dump()` / `pickle.load()`: 操作文件。
*   `pickle.dumps()` / `pickle.loads()`: 操作字节流（更常用）。
*   `json.load()` / `json.loads()`: JSON模块的反序列化。
*   `yaml.load()`: YAML模块的反序列化（需注意安全风险）。

![](img/a53f930152f1beb30c75097d9275ab2a_135.png)

![](img/a53f930152f1beb30c75097d9275ab2a_137.png)

![](img/a53f930152f1beb30c75097d9275ab2a_139.png)

![](img/a53f930152f1beb30c75097d9275ab2a_141.png)

![](img/a53f930152f1beb30c75097d9275ab2a_143.png)

反序列化漏洞的成因在于，如果程序反序列化了用户可控的恶意数据，并且该数据指向的类中定义了特殊的魔术方法（`__reduce__`, `__setstate__`等），那么在反序列化过程中就可能执行这些方法中的任意代码。

![](img/a53f930152f1beb30c75097d9275ab2a_145.png)

![](img/a53f930152f1beb30c75097d9275ab2a_147.png)

![](img/a53f930152f1beb30c75097d9275ab2a_149.png)

![](img/a53f930152f1beb30c75097d9275ab2a_151.png)

![](img/a53f930152f1beb30c75097d9275ab2a_153.png)

![](img/a53f930152f1beb30c75097d9275ab2a_155.png)

![](img/a53f930152f1beb30c75097d9275ab2a_157.png)

![](img/a53f930152f1beb30c75097d9275ab2a_159.png)

![](img/a53f930152f1beb30c75097d9275ab2a_161.png)

![](img/a53f930152f1beb30c75097d9275ab2a_163.png)

![](img/a53f930152f1beb30c75097d9275ab2a_165.png)

![](img/a53f930152f1beb30c75097d9275ab2a_167.png)

**关键的魔术方法：**
*   `__reduce__()`: 反序列化时自动调用。
*   `__setstate__()`: 反序列化时自动调用。
*   `__getstate__()`: 序列化时自动调用。

![](img/a53f930152f1beb30c75097d9275ab2a_169.png)

![](img/a53f930152f1beb30c75097d9275ab2a_171.png)

![](img/a53f930152f1beb30c75097d9275ab2a_173.png)

![](img/a53f930152f1beb30c75097d9275ab2a_175.png)

![](img/a53f930152f1beb30c75097d9275ab2a_177.png)

![](img/a53f930152f1beb30c75097d9275ab2a_179.png)

![](img/a53f930152f1beb30c75097d9275ab2a_181.png)

![](img/a53f930152f1beb30c75097d9275ab2a_183.png)

![](img/a53f930152f1beb30c75097d9275ab2a_185.png)

**漏洞利用演示：**
1.  **构造恶意类**：创建一个包含`__reduce__`魔术方法的类，该方法返回一个可执行系统命令的元组。
    ```python
    import pickle
    import os

    class Exploit:
        def __reduce__(self):
            # 返回一个元组，第一个元素是可调用对象（os.system），第二个是参数元组
            return (os.system, ('calc.exe', ))

    # 序列化恶意对象
    exp = pickle.dumps(Exploit())
    print(exp) # 输出序列化后的字节流
    ```
2.  **触发漏洞**：当存在漏洞的代码对上述字节流进行反序列化时（例如`pickle.loads(user_controlled_data)`），便会弹出计算器。
    ```python
    pickle.loads(exp) # 反序列化，执行命令
    ```

![](img/a53f930152f1beb30c75097d9275ab2a_187.png)

![](img/a53f930152f1beb30c75097d9275ab2a_188.png)

![](img/a53f930152f1beb30c75097d9275ab2a_190.png)

![](img/a53f930152f1beb30c75097d9275ab2a_192.png)

![](img/a53f930152f1beb30c75097d9275ab2a_194.png)

![](img/a53f930152f1beb30c75097d9275ab2a_196.png)

![](img/a53f930152f1beb30c75097d9275ab2a_198.png)

![](img/a53f930152f1beb30c75097d9275ab2a_200.png)

![](img/a53f930152f1beb30c75097d9275ab2a_202.png)

![](img/a53f930152f1beb30c75097d9275ab2a_204.png)

![](img/a53f930152f1beb30c75097d9275ab2a_206.png)

![](img/a53f930152f1beb30c75097d9275ab2a_208.png)

![](img/a53f930152f1beb30c75097d9275ab2a_210.png)

![](img/a53f930152f1beb30c75097d9275ab2a_212.png)

![](img/a53f930152f1beb30c75097d9275ab2a_214.png)

![](img/a53f930152f1beb30c75097d9275ab2a_216.png)

![](img/a53f930152f1beb30c75097d9275ab2a_217.png)

**黑盒测试特征：**
在传输过程中，`pickle`序列化的数据常经过Base64编码。其编码后的特征是以 `gASV` 或类似变体开头。如果在请求参数（如Cookie）中发现此类特征值，且目标为Python应用，则可尝试反序列化漏洞测试。

![](img/a53f930152f1beb30c75097d9275ab2a_218.png)

![](img/a53f930152f1beb30c75097d9275ab2a_219.png)

![](img/a53f930152f1beb30c75097d9275ab2a_221.png)

![](img/a53f930152f1beb30c75097d9275ab2a_223.png)

![](img/a53f930152f1beb30c75097d9275ab2a_224.png)

![](img/a53f930152f1beb30c75097d9275ab2a_226.png)

![](img/a53f930152f1beb30c75097d9275ab2a_228.png)

![](img/a53f930152f1beb30c75097d9275ab2a_230.png)

![](img/a53f930152f1beb30c75097d9275ab2a_232.png)

![](img/a53f930152f1beb30c75097d9275ab2a_233.png)

## 三、Python格式化字符串漏洞 📝

![](img/a53f930152f1beb30c75097d9275ab2a_235.png)

![](img/a53f930152f1beb30c75097d9275ab2a_237.png)

![](img/a53f930152f1beb30c75097d9275ab2a_239.png)

![](img/a53f930152f1beb30c75097d9275ab2a_241.png)

![](img/a53f930152f1beb30c75097d9275ab2a_243.png)

![](img/a53f930152f1beb30c75097d9275ab2a_244.png)

![](img/a53f930152f1beb30c75097d9275ab2a_245.png)

![](img/a53f930152f1beb30c75097d9275ab2a_247.png)

![](img/a53f930152f1beb30c75097d9275ab2a_249.png)

![](img/a53f930152f1beb30c75097d9275ab2a_250.png)

![](img/a53f930152f1beb30c75097d9275ab2a_252.png)

上一节我们介绍了反序列化漏洞，本节中我们来看看Python特有的格式化字符串安全问题。Python提供了多种字符串格式化方法，其中在3.6版本引入的`f-string`（格式化字符串字面值）若使用不当，可能造成严重的安全问题。

![](img/a53f930152f1beb30c75097d9275ab2a_254.png)

![](img/a53f930152f1beb30c75097d9275ab2a_256.png)

![](img/a53f930152f1beb30c75097d9275ab2a_258.png)

![](img/a53f930152f1beb30c75097d9275ab2a_260.png)

![](img/a53f930152f1beb30c75097d9275ab2a_262.png)

![](img/a53f930152f1beb30c75097d9275ab2a_264.png)

![](img/a53f930152f1beb30c75097d9275ab2a_266.png)

![](img/a53f930152f1beb30c75097d9275ab2a_268.png)

![](img/a53f930152f1beb30c75097d9275ab2a_270.png)

![](img/a53f930152f1beb30c75097d9275ab2a_272.png)

**Python字符串格式化方法：**
1.  **百分号 `%` 格式化**：`"Hello, %s" % name`
2.  **`str.format()` 方法**：`"Hello, {}".format(name)`
3.  **`string.Formatter` 类**
4.  **f-string (Python 3.6+)**：`f"Hello, {name}"`

![](img/a53f930152f1beb30c75097d9275ab2a_274.png)

![](img/a53f930152f1beb30c75097d9275ab2a_276.png)

![](img/a53f930152f1beb30c75097d9275ab2a_278.png)

![](img/a53f930152f1beb30c75097d9275ab2a_280.png)

![](img/a53f930152f1beb30c75097d9275ab2a_282.png)

![](img/a53f930152f1beb30c75097d9275ab2a_284.png)

![](img/a53f930152f1beb30c75097d9275ab2a_286.png)

![](img/a53f930152f1beb30c75097d9275ab2a_288.png)

![](img/a53f930152f1beb30c75097d9275ab2a_290.png)

**漏洞原理：**
当格式化字符串的内容（特别是`f-string`中花括号`{}`内的表达式）可由用户控制时，攻击者可以注入恶意表达式。

![](img/a53f930152f1beb30c75097d9275ab2a_292.png)

![](img/a53f930152f1beb30c75097d9275ab2a_294.png)

![](img/a53f930152f1beb30c75097d9275ab2a_295.png)

![](img/a53f930152f1beb30c75097d9275ab2a_297.png)

![](img/a53f930152f1beb30c75097d9275ab2a_299.png)

![](img/a53f930152f1beb30c75097d9275ab2a_300.png)

![](img/a53f930152f1beb30c75097d9275ab2a_302.png)

![](img/a53f930152f1beb30c75097d9275ab2a_304.png)

![](img/a53f930152f1beb30c75097d9275ab2a_305.png)

![](img/a53f930152f1beb30c75097d9275ab2a_307.png)

**危险示例：**
```python
# 假设模板字符串来自用户输入
user_input = "{__import__('os').system('calc.exe')}"
# 使用f-string进行格式化
result = f"Result: {user_input}" # 这不会执行命令，因为user_input整体被当作字符串

![](img/a53f930152f1beb30c75097d9275ab2a_309.png)

![](img/a53f930152f1beb30c75097d9275ab2a_311.png)

![](img/a53f930152f1beb30c75097d9275ab2a_312.png)

![](img/a53f930152f1beb30c75097d9275ab2a_314.png)

![](img/a53f930152f1beb30c75097d9275ab2a_316.png)

![](img/a53f930152f1beb30c75097d9275ab2a_318.png)

![](img/a53f930152f1beb30c75097d9275ab2a_320.png)

![](img/a53f930152f1beb30c75097d9275ab2a_322.png)

![](img/a53f930152f1beb30c75097d9275ab2a_324.png)

![](img/a53f930152f1beb30c75097d9275ab2a_326.png)

![](img/a53f930152f1beb30c75097d9275ab2a_328.png)

![](img/a53f930152f1beb30c75097d9275ab2a_330.png)

# 危险的正确示例：直接控制f-string表达式部分
code = "__import__('os').system('calc.exe')"
# 如果程序这样构造f-string（极度危险，仅作演示）：
eval(f"f'{{{code}}}'") # 这将执行命令
```
更实际的漏洞场景可能出现在服务端使用`eval()`或`exec()`动态执行包含用户可控内容的f-string模板时。

![](img/a53f930152f1beb30c75097d9275ab2a_332.png)

![](img/a53f930152f1beb30c75097d9275ab2a_334.png)

![](img/a53f930152f1beb30c75097d9275ab2a_336.png)

![](img/a53f930152f1beb30c75097d9275ab2a_338.png)

![](img/a53f930152f1beb30c75097d9275ab2a_340.png)

![](img/a53f930152f1beb30c75097d9275ab2a_342.png)

![](img/a53f930152f1beb30c75097d9275ab2a_344.png)

![](img/a53f930152f1beb30c75097d9275ab2a_346.png)

![](img/a53f930152f1beb30c75097d9275ab2a_348.png)

![](img/a53f930152f1beb30c75097d9275ab2a_350.png)

**`format()`方法的安全问题：**
`str.format()`方法允许通过位置或关键字访问属性。如果格式化模板可控，攻击者可能访问到不应暴露的对象属性。
```python
class User:
    def __init__(self):
        self.secret = "Sensitive Data"
        self.name = "Alice"

![](img/a53f930152f1beb30c75097d9275ab2a_352.png)

![](img/a53f930152f1beb30c75097d9275ab2a_354.png)

![](img/a53f930152f1beb30c75097d9275ab2a_356.png)

![](img/a53f930152f1beb30c75097d9275ab2a_358.png)

user = User()
# 用户可控的格式化字符串
malicious_template = "{0.secret}"
print(malicious_template.format(user)) # 输出: Sensitive Data
```

![](img/a53f930152f1beb30c75097d9275ab2a_360.png)

![](img/a53f930152f1beb30c75097d9275ab2a_362.png)

![](img/a53f930152f1beb30c75097d9275ab2a_364.png)

![](img/a53f930152f1beb30c75097d9275ab2a_366.png)

此漏洞通常需要在代码审计（白盒）中发现，黑盒测试难以直接利用。在审计Python代码时，应重点关注用户输入是否直接用于构造`f-string`或传递给`format()`方法，特别是与敏感函数（如`eval`）结合使用时。

![](img/a53f930152f1beb30c75097d9275ab2a_368.png)

![](img/a53f930152f1beb30c75097d9275ab2a_370.png)

![](img/a53f930152f1beb30c75097d9275ab2a_372.png)

![](img/a53f930152f1beb30c75097d9275ab2a_374.png)

![](img/a53f930152f1beb30c75097d9275ab2a_376.png)

![](img/a53f930152f1beb30c75097d9275ab2a_378.png)

![](img/a53f930152f1beb30c75097d9275ab2a_380.png)

![](img/a53f930152f1beb30c75097d9275ab2a_382.png)

![](img/a53f930152f1beb30c75097d9275ab2a_384.png)

![](img/a53f930152f1beb30c75097d9275ab2a_386.png)

## 总结 📚

![](img/a53f930152f1beb30c75097d9275ab2a_388.png)

![](img/a53f930152f1beb30c75097d9275ab2a_390.png)

![](img/a53f930152f1beb30c75097d9275ab2a_392.png)

![](img/a53f930152f1beb30c75097d9275ab2a_394.png)

![](img/a53f930152f1beb30c75097d9275ab2a_396.png)

![](img/a53f930152f1beb30c75097d9275ab2a_398.png)

![](img/a53f930152f1beb30c75097d9275ab2a_400.png)

![](img/a53f930152f1beb30c75097d9275ab2a_402.png)

![](img/a53f930152f1beb30c75097d9275ab2a_404.png)

本节课我们一起学习了Python安全中的三个关键知识点：
1.  **PYC文件反编译**：了解了`.pyc`文件的作用，以及如何使用`uncompyle6`工具将其反编译为Python源码，这主要用于逆向分析和代码审计。
2.  **Python反序列化漏洞**：深入探讨了`pickle`模块的反序列化机制，理解了如何通过构造包含`__reduce__`等魔术方法的恶意类来实现远程代码执行，并掌握了黑盒测试中的特征识别（Base64编码后的`gASV`前缀）。
3.  **格式化字符串安全**：认识了Python中几种字符串格式化方法，重点分析了`f-string`和`str.format()`在用户输入可控时可能带来的安全风险，如信息泄露或代码执行。

![](img/a53f930152f1beb30c75097d9275ab2a_406.png)

![](img/a53f930152f1beb30c75097d9275ab2a_408.png)

![](img/a53f930152f1beb30c75097d9275ab2a_410.png)

![](img/a53f930152f1beb30c75097d9275ab2a_412.png)

![](img/a53f930152f1beb30c75097d9275ab2a_414.png)

![](img/a53f930152f1beb30c75097d9275ab2a_416.png)

![](img/a53f930152f1beb30c75097d9275ab2a_418.png)

这些知识有助于我们在进行Python应用安全评估时，从代码层面和运行层面发现更深层次的安全隐患。