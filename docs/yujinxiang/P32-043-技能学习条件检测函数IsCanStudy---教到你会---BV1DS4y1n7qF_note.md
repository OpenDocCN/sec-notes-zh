![](img/e24f545b080a39503ad3bffc99e76d9a_1.png)

# 课程 P32：043 - 技能学习条件检测函数 IsCanStudy 🧠

![](img/e24f545b080a39503ad3bffc99e76d9a_2.png)

在本节课中，我们将学习如何封装一个函数，用于检测游戏中的某个技能是否满足修炼条件。我们将分析所需的数据，编写判断逻辑，并将其集成到现有的技能系统中。

---

上一节我们分析了技能修炼所需的条件数据。本节中，我们将把这些条件判断逻辑封装成一个独立的函数。

这个函数的核心目标是：**判断指定下标的技能当前是否可以被角色学习**。它需要检查三个主要条件：
1.  技能是否已经学习过。
2.  角色的当前历练值是否达到技能要求。
3.  角色的当前等级是否达到技能要求。

以下是实现此函数的具体步骤：

**第一步：在技能对象中添加必要属性**
我们需要在技能对象的数据结构中，补充技能本身的修炼要求数据。
*   `EF6`：技能是否已学习的标志。
*   `268`：技能所需的历练值。
*   `AC`：技能所需的等级。

**第二步：读取角色当前状态**
函数需要获取角色当前的属性，以进行比较。
*   角色当前历练值：从人物属性单元偏移 `0xAC` 处读取。
*   角色当前等级：从人物属性单元偏移 `0x34` 处读取。

**第三步：编写判断函数逻辑**
函数 `IsCanStudy` 接收一个技能下标作为参数，其伪代码逻辑如下：
```c
bool IsCanStudy(int skillIndex) {
    // 1. 读取技能数据
    skillData = GetSkillData(skillIndex);
    // 如果技能ID为0（无效技能），直接返回 false

    // 2. 读取角色当前数据
    currentExp = GetCharacterCurrentExp();
    currentLevel = GetCharacterCurrentLevel();

    // 3. 逐一进行条件判断
    if (skillData.isLearned == true) {
        return false; // 条件1：技能已学习，不可再学
    }
    if (currentExp < skillData.requiredExp) {
        return false; // 条件2：当前历练不足
    }
    if (currentLevel < skillData.requiredLevel) {
        return false; // 条件3：当前等级不足
    }

    // 所有条件均满足
    return true;
}
```

**第四步：集成到技能修炼流程中**
在调用“修炼技能”的功能之前，先使用 `IsCanStudy` 函数进行检测。
```c
void StudySkill(int skillIndex) {
    if (!IsCanStudy(skillIndex)) {
        // 打印调试信息，如“技能已学习”、“历练不足”或“等级不足”
        return -1; // 修炼失败
    }
    // 条件满足，执行原有的修炼技能代码
    ExecuteSkillStudy(skillIndex);
}
```

**第五步：测试与调试**
编写测试代码，循环遍历所有技能下标，调用 `IsCanStudy` 函数，并打印出每个技能的检测结果（例如：“技能1：可学习”、“技能2：历练不足”）。通过信息查看器观察输出，验证函数逻辑是否正确。

---

![](img/e24f545b080a39503ad3bffc99e76d9a_4.png)

本节课中我们一起学习了如何构建一个技能学习条件检测函数。我们通过分析游戏数据，定义了判断逻辑，并将其封装成一个可复用的函数 `IsCanStudy`。最后，我们将此函数集成到技能修炼流程中，并进行了测试验证。掌握这种条件检测的封装方法，对于处理游戏中的各种状态判断非常有帮助。