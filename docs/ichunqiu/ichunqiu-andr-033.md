#  033：课时8 Android源码定制添加反反调试机制 🛠️

![](img/5cd985d13ae587f697c0b10e27029da1_1.png)

![](img/5cd985d13ae587f697c0b10e27029da1_2.png)

在本节课中，我们将学习如何通过修改Android内核源码，来添加反反调试机制，从而绕过应用程序对调试状态的检测。

![](img/5cd985d13ae587f697c0b10e27029da1_4.png)

上一节我们介绍了应用程序如何通过检测`/proc`文件系统中的特定文件（如`status`和`stat`）来发现调试器。本节中我们来看看如何从系统层面修改这些文件的输出内容，使其无法暴露调试信息。

![](img/5cd985d13ae587f697c0b10e27029da1_6.png)

## 内核源码结构概述

Android系统编译分为内核编译和系统本身编译两部分。本次修改主要针对内核部分。内核源码的结构与Linux内核类似，与进程文件系统（`procfs`）相关的代码位于`fs/proc`目录下。

![](img/5cd985d13ae587f697c0b10e27029da1_8.png)

![](img/5cd985d13ae587f697c0b10e27029da1_9.png)

## 定位关键修改点

我们需要找到生成`/proc/[pid]/status`和`/proc/[pid]/stat`文件的源码位置。

![](img/5cd985d13ae587f697c0b10e27029da1_11.png)

以下是关键步骤：

![](img/5cd985d13ae587f697c0b10e27029da1_13.png)

![](img/5cd985d13ae587f697c0b10e27029da1_15.png)

![](img/5cd985d13ae587f697c0b10e27029da1_17.png)

![](img/5cd985d13ae587f697c0b10e27029da1_19.png)

1.  **导入内核源码**：使用IDE（如CLion、Eclipse等）导入整个内核源码工程，便于分析和搜索。
2.  **定位proc文件系统**：核心代码在`fs/proc`目录中。`base.c`文件是许多proc文件操作的入口。
3.  **查找status文件生成函数**：在`base.c`中搜索`status`，可以找到其对应的操作函数，例如`proc_pid_status`。
4.  **分析stat文件生成函数**：同样，在`base.c`或相关文件中找到`stat`文件的生成函数，如`proc_pid_stat`。

![](img/5cd985d13ae587f697c0b10e27029da1_21.png)

## 修改status文件内容

![](img/5cd985d13ae587f697c0b10e27029da1_22.png)

![](img/5cd985d13ae587f697c0b10e27029da1_23.png)

![](img/5cd985d13ae587f697c0b10e27029da1_25.png)

`/proc/[pid]/status`文件包含了进程的详细信息，其中`State`字段和`TracerPid`字段是反调试检测的重点。

![](img/5cd985d13ae587f697c0b10e27029da1_27.png)

以下是需要修改的核心部分：

![](img/5cd985d13ae587f697c0b10e27029da1_29.png)

![](img/5cd985d13ae587f697c0b10e27029da1_31.png)

*   **修改进程状态字段**：当进程被调试时，其`State`字段会显示为`t`（tracing stop）或`T`（stopped）。我们需要将其修改为`S`（sleeping）。
    *   **定位**：在`fs/proc/array.c`文件中，找到`task_state_array`数组。
    *   **修改**：将该数组中索引为4（对应`t`）和8（对应`T`）的元素值从`"t"`和`"T"`都改为`"S"`。
    *   **代码示例**：
        ```c
        // 修改前
        static const char * const task_state_array[] = {
            "R (running)",        /*   0 */
            "S (sleeping)",       /*   1 */
            "D (disk sleep)",     /*   2 */
            "T (stopped)",        /*   4 */ // 需要修改
            "t (tracing stop)",   /*   8 */ // 需要修改
            ...
        };
        // 修改后
        static const char * const task_state_array[] = {
            "R (running)",        /*   0 */
            "S (sleeping)",       /*   1 */
            "D (disk sleep)",     /*   2 */
            "S (sleeping)",       /*   4 */ // 已修改
            "S (sleeping)",       /*   8 */ // 已修改
            ...
        };
        ```

*   **修改调试进程PID字段**：`TracerPid`字段记录了调试该进程的调试器PID。我们需要让其始终返回0。
    *   **定位**：在`proc_pid_status`函数（通常在`fs/proc/array.c`）中，找到格式化输出`TracerPid`的代码行。
    *   **修改**：将传入格式化字符串的`tracer pid`参数值强制改为0。
    *   **代码示例**：
        ```c
        // 修改前，seq_printf可能会使用task->ptrace_leader等真实值
        seq_printf(m, "TracerPid:\t%d\n", task->pid);
        // 修改后，直接输出0
        seq_printf(m, "TracerPid:\t0\n");
        ```

![](img/5cd985d13ae587f697c0b10e27029da1_33.png)

![](img/5cd985d13ae587f697c0b10e27029da1_34.png)

![](img/5cd985d13ae587f697c0b10e27029da1_36.png)

## 修改stat文件内容

![](img/5cd985d13ae587f697c0b10e27029da1_38.png)

`/proc/[pid]/stat`文件以更紧凑的格式存放类似信息，其第三个字段就是进程状态。

![](img/5cd985d13ae587f697c0b10e27029da1_40.png)

![](img/5cd985d13ae587f697c0b10e27029da1_42.png)

以下是需要修改的部分：

![](img/5cd985d13ae587f697c0b10e27029da1_44.png)

![](img/5cd985d13ae587f697c0b10e27029da1_46.png)

*   **修改进程状态字段**：与`status`文件同理，需要将表示调试状态的字符`t`或`T`改为`S`。
    *   **定位**：在生成`stat`文件的函数（如`do_task_stat`）中，会调用`get_task_state`来获取状态字符串。
    *   **原理**：`get_task_state`函数正是从我们上面修改过的`task_state_array`数组中取值。因此，修改了数组后，`stat`文件的状态字段也会自动变为`S`。

![](img/5cd985d13ae587f697c0b10e27029da1_48.png)

## 其他可能的修改点

![](img/5cd985d13ae587f697c0b10e27029da1_50.png)

![](img/5cd985d13ae587f697c0b10e27029da1_52.png)

某些反调试方案可能会检测`/proc/[pid]/wchan`文件（显示进程当前在哪个内核函数中“睡眠”）。当被调试时，此文件内容可能变为`ptrace_stop`。

![](img/5cd985d13ae587f697c0b10e27029da1_54.png)

以下是修改方法：

![](img/5cd985d13ae587f697c0b10e27029da1_56.png)

![](img/5cd985d13ae587f697c0b10e27029da1_58.png)

*   **定位**：在`fs/proc/base.c`文件中，找到`proc_pid_wchan`函数。
*   **修改**：在函数中，判断如果当前进程处于被跟踪（`ptrace`）状态，则返回一个空字符串或无关字符串，而不是真实的`wchan`信息。
    *   **代码示例**：
        ```c
        static int proc_pid_wchan(struct seq_file *m, struct pid_namespace *ns,
                                  struct pid *pid, struct task_struct *task)
        {
            // 添加判断：如果进程被ptrace跟踪，则输出空值
            if (task->ptrace) {
                seq_putc(m, '0');
                return 0;
            }
            // ... 原有的wchan查询逻辑 ...
        }
        ```

![](img/5cd985d13ae587f697c0b10e27029da1_60.png)

![](img/5cd985d13ae587f697c0b10e27029da1_62.png)

## 编译与测试

![](img/5cd985d13ae587f697c0b10e27029da1_64.png)

![](img/5cd985d13ae587f697c0b10e27029da1_66.png)

完成上述源码修改后，需要重新编译内核并刷入设备。

![](img/5cd985d13ae587f697c0b10e27029da1_68.png)

![](img/5cd985d13ae587f697c0b10e27029da1_69.png)

以下是操作流程：

![](img/5cd985d13ae587f697c0b10e27029da1_71.png)

1.  按照内核编译标准流程，编译生成新的内核镜像（如`zImage`或`boot.img`）。
2.  将新内核刷入Android设备或模拟器。可以使用`fastboot flash boot boot.img`命令。
3.  重启设备，使新内核生效。
4.  测试：使用调试器（如`gdbserver`+`IDA Pro`）附加上一节课中具有反调试功能的示例APK。此时，应用程序应无法检测到调试器的存在。

![](img/5cd985d13ae587f697c0b10e27029da1_73.png)

![](img/5cd985d13ae587f697c0b10e27029da1_74.png)

![](img/5cd985d13ae587f697c0b10e27029da1_76.png)

**注意**：直接刷新内核存在风险，建议在模拟器或专用测试设备上进行。

![](img/5cd985d13ae587f697c0b10e27029da1_77.png)

![](img/5cd985d13ae587f697c0b10e27029da1_79.png)

![](img/5cd985d13ae587f697c0b10e27029da1_80.png)

![](img/5cd985d13ae587f697c0b10e27029da1_82.png)

![](img/5cd985d13ae587f697c0b10e27029da1_83.png)

![](img/5cd985d13ae587f697c0b10e27029da1_84.png)

## 总结

![](img/5cd985d13ae587f697c0b10e27029da1_86.png)

![](img/5cd985d13ae587f697c0b10e27029da1_88.png)

![](img/5cd985d13ae587f697c0b10e27029da1_89.png)

![](img/5cd985d13ae587f697c0b10e27029da1_91.png)

本节课中我们一起学习了如何通过修改Android内核源码来实现反反调试机制。

![](img/5cd985d13ae587f697c0b10e27029da1_93.png)

![](img/5cd985d13ae587f697c0b10e27029da1_95.png)

![](img/5cd985d13ae587f697c0b10e27029da1_96.png)

![](img/5cd985d13ae587f697c0b10e27029da1_98.png)

我们主要完成了以下核心修改：
1.  在`task_state_array`中将调试状态`t`和`T`替换为睡眠状态`S`。
2.  在输出`status`文件时，将`TracerPid`字段固定为0。
3.  可选地，修改了`wchan`文件的输出以隐藏`ptrace_stop`信息。

![](img/5cd985d13ae587f697c0b10e27029da1_100.png)

![](img/5cd985d13ae587f697c0b10e27029da1_102.png)

这些修改从系统层面欺骗了通过读取`/proc`文件系统来检测调试器的应用程序，使得常规的基于`status`和`stat`文件的反调试手段失效。通过实践本课内容，你可以定制一个更有利于逆向分析的系统环境。