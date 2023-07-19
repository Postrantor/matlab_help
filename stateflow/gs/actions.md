---
> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [ww2.mathworks.cn](https://ww2.mathworks.cn/help/stateflow/gs/actions.html)
> 通过使用状态和转移中的动作来控制 Stateflow 图的行为。
---

*状态动作*和*转移动作*是您分别在状态内部或转移上编写的指令，用于定义 Stateflow® 图在仿真期间的行为。例如，下图中的动作定义了一个以试验方式验证 Collatz 猜想实例的状态机。对于给定的数值输入 ![](https://ww2.mathworks.cn/help/stateflow/gs/statetransitionactionsgetstartedexample_eq15012583454694319273_zh_CN.png)，该图通过迭代以下规则来计算冰雹序列 ![](https://ww2.mathworks.cn/help/stateflow/gs/statetransitionactionsgetstartedexample_eq12219811583566878803_zh_CN.png) ![](https://ww2.mathworks.cn/help/stateflow/gs/statetransitionactionsgetstartedexample_eq11325635569159405358_zh_CN.png) ![](https://ww2.mathworks.cn/help/stateflow/gs/statetransitionactionsgetstartedexample_eq10936753333647109350_zh_CN.png) ![](https://ww2.mathworks.cn/help/stateflow/gs/statetransitionactionsgetstartedexample_eq17216054076818118233_zh_CN.png)：

Collatz 猜想指出，每个正整数都有一个最终达到 1 的冰雹序列。

![](https://ww2.mathworks.cn/help/stateflow/gs/statetransitionactionsgetstartedexample_01_zh_CN.png)

该图由三个状态组成。在仿真开始时，`Init` 状态通过以下设置来初始化图数据：

- 将局部数据 `n` 设置为输入 `u` 的值。
- 当 `n` 除以 2 时，将局部数据 `n2` 设置为余数。
- 将输出数据 `y` 设置为 `false`。

根据输入的奇偶性，图转移到 `Even` 或 `Odd` 状态。当状态活动在 `Even` 和 `Odd` 状态之间切换时，图会计算冰雹序列中的数字。当序列达到 1 值时，输出数据 `y` 变为 `true`，并触发 Simulink® 模型中的 [Stop Simulation](https://ww2.mathworks.cn/help/simulink/slref/stopsimulation.html) (Simulink) 模块。

### 状态动作类型

状态动作定义当状态被激活时 Stateflow 图的动作。最常见的状态动作类型是 `entry`、`during` 和 `exit` 动作：

- 当状态被激活时，`entry` 动作发生。
- 当状态已激活并且图未转移出该状态时，`during` 动作在时间步上发生。
- 当图转移出该状态时，`exit` 动作发生。

您可以使用完整关键字（`entry`、`during`、`exit`）或缩写（`en`、`du` 和 `ex`）来指定状态动作的类型。您还可以使用逗号组合各状态动作类型。例如，具有组合类型 `entry, during` 的动作当状态被激活时在时间步上发生，并且在状态保持激活时在每个后续时间步上发生。

冰雹图包含以下状态下的动作：

- `Init` - 如果此状态在仿真开始时被激活，`entry` 动作确定 `n` 的奇偶性并将 `y` 设置为 `false`。当图在一个时间步后转移出 `Init` 时，`exit` 动作确定 `n` 是否等于 1。
- `Even` - 当此状态被激活时，在该状态保持激活的每个后续时间步上，组合的 `entry, during` 动作计算冰雹序列的下一个数字 `n/2` 的值和奇偶性。
- `Odd` - 当此状态被激活时，在该状态保持激活的每个后续时间步上，组合的 `entry, during` 动作检查 `n` 是否大于 1，如果大于 1，则计算冰雹序列的下一个数字 `3*n+1` 的值和奇偶性。

### 转移动作的类型

转移动作定义当激活状态更改时 Stateflow 图执行的动作。最常见的转移动作类型是条件和条件动作。要指定转移动作，请使用采用以下语法的标签：

```
[Condition]{ConditionAction}
```

`Condition` 是布尔表达式，用于确定是否发生转移。如果不指定条件，转移将在源状态被激活后的下一个时间步发生。

`ConditionAction` 是在判断转移的条件为 true 时执行的指令。条件动作发生在条件后，但在任何 `exit` 或 `entry` 状态动作之前。

冰雹图包含发生以下转移时的动作：

- 到 `Init` 的默认转移 - 在仿真开始时，条件动作 `n = u` 将输入值 `u` 赋给局部数据 `n`。
- 从 `Init` 到 `Even` 的转移 - 条件 `n2 == 0` 确定当 `n` 为偶数时发生该转移。此转移起始处的数字 1 表示此转移将先于 `Init` 到 `Odd` 的转移进行计算。
- 从 `Odd` 到 `Even` 的转移 - 条件 `n2 == 0` 确定当 `n` 为偶数时发生该转移。
- 从 `Even` 到 `Odd` 的转移 - 条件 `n2 ~= 0` 确定当 `n` 为奇数时发生该转移。在这种情况下，条件动作 `y = isequal(n,1)` 确定 `n` 是否等于 1。

### 检查图行为

要计算从值 9 开始的冰雹序列，请执行下列步骤：

1. 在 Constant 模块中，输入值 `9`。
2. 在**仿真**选项卡中，点击**运行**。图用下列动作予以响应：

- 在时间 ![](https://ww2.mathworks.cn/help/stateflow/gs/statetransitionactionsgetstartedexample_eq05490811019241878006_zh_CN.png) 处，发生到 `Init` 的默认转移。转移动作将 `n` 的值设置为 9。`Init` 状态被激活。`Init` 中的 `entry` 动作将 `n2` 设置为 1 且将 `y` 设置为 `false`。
- 在时间 ![](https://ww2.mathworks.cn/help/stateflow/gs/statetransitionactionsgetstartedexample_eq12716245961660159691_zh_CN.png) 处，条件 `n2 == 0` 为 false，因此图准备转移到 `Odd`。`Init` 中的 `exit` 动作将 `y` 设置为 `false`。状态 `Init` 变为非激活，状态 `Odd` 被激活。`Odd` 中的 `entry` 动作将 `n` 设置为 28，将 `n2` 设置为 0。
- 在时间 ![](https://ww2.mathworks.cn/help/stateflow/gs/statetransitionactionsgetstartedexample_eq03599317120891285387_zh_CN.png) 处，条件 `n2 == 0` 为 true，因此图准备转移到 `Even`。状态 `Odd` 变为非激活，状态 `Even` 被激活。`Even` 中的 entry 动作将 `n` 设置为 14，将 `n2` 设置为 0。
- 在时间 ![](https://ww2.mathworks.cn/help/stateflow/gs/statetransitionactionsgetstartedexample_eq07738249947809783232_zh_CN.png) 处，条件 `n2 ~= 0` 为 false，因此图不会发生转移。`Even` 状态保持激活。`Even` 中的 `during` 动作将 `n` 设置为 7，将 `n2` 设置为 1。
- 在时间 ![](https://ww2.mathworks.cn/help/stateflow/gs/statetransitionactionsgetstartedexample_eq15359278158169810469_zh_CN.png) 处，条件 `n2 ~= 0` 为 true，因此图准备转移到 `Odd`。转移动作将 `y` 设置为 false。状态 `Even` 变为非激活，状态 `Odd` 被激活。`Odd` 中的 `entry` 动作将 `n` 设置为 22，将 `n2` 设置为 0。
- 该图继续计算冰雹序列，直到它在时间 ![](https://ww2.mathworks.cn/help/stateflow/gs/statetransitionactionsgetstartedexample_eq17315660474862548967_zh_CN.png) 处达到值 `n` ![](https://ww2.mathworks.cn/help/stateflow/gs/statetransitionactionsgetstartedexample_eq13562902622338161563_zh_CN.png)。
- 在时间 ![](https://ww2.mathworks.cn/help/stateflow/gs/statetransitionactionsgetstartedexample_eq05704149810581774953_zh_CN.png) 处，图准备从 `Even` 转移到 `Odd`。转移动作将 `y` 设置为 `true`。状态 `Even` 变为非激活，状态 `Odd` 被激活。`Odd` 中的 `entry` 动作不会修改 `n` 或 `n2`。连接到输出信号 `y` 的 Stop Simulation 模块停止仿真。

3. 在**仿真**选项卡中的**查看结果**下，点击**数据检查器**。
4. 要查看冰雹序列的值，请在仿真数据检查器中，选择记录的信号 `n`。

![](https://ww2.mathworks.cn/help/stateflow/gs/xxsf_collatz-sdi_zh_CN.png)

冰雹序列在 19 次迭代后达到值 1。

## 相关主题

- [通过使用状态来表示工作模式](https://ww2.mathworks.cn/help/stateflow/ug/states.html)
- [定义转移中的动作](https://ww2.mathworks.cn/help/stateflow/ug/transitions.html#f18-8766)

您曾对此示例进行过修改。是否要打开带有您的编辑的示例？

本页内容对您有帮助吗？

Unrated

1 star

2 stars

3 stars

4 stars

5 stars

Unrated 1 star 2 stars 3 stars 4 stars 5 stars

您点击的链接对应于以下 MATLAB 命令：

请在 MATLAB 命令行窗口中直接输入以执行命令。Web 浏览器不支持 MATLAB 命令。
