> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [ww2.mathworks.cn](https://ww2.mathworks.cn/help/stateflow/gs/events.html)

> 从一个状态向另一个状态广播事件以同步并行状态。

局部事件允许一个状态触发同一个 Stateflow® 图中另一个状态的转移或动作，从而使您能够协调并行状态。要将事件从一个状态广播到另一个状态，请使用 `[send](https://ww2.mathworks.cn/help/stateflow/ref/send.html)` 运算符以及事件的名称和激活状态的名称：

```
send(eventName,stateName)
```

当您广播事件时，该事件将在接收状态以及该状态层次结构中的任何子状态中生效。

### 家庭安全系统建模

此示例使用局部事件作为家庭安全系统设计的一部分。

![](https://ww2.mathworks.cn/help/stateflow/gs/eventsgetstartedexample_01_zh_CN.png)

该安全系统由一个警报器和三个防入侵传感器（门传感器、窗传感器和运动检测器）组成。在系统检测到入侵后，您可以在较短的一段时间内禁用警报。否则，系统会向警方报告。

该图通过使用以下并行状态之一对每个传感器进行建模：

- 并行状态 `Door` 对门传感器进行建模。输入信号 `D_mode` 在此传感器的 `Active` 和 `Disabled` 模式之间进行选择。当传感器被激活时，输入信号 `Door_sens` 指示可能存在入侵。
- 并行状态 `Win` 对窗传感器进行建模。输入信号 `W_mode` 在此传感器的 `Active` 和 `Disabled` 模式之间进行选择。当传感器被激活时，输入信号 `Win_sens` 指示可能存在入侵。
- 并行状态 `Motion` 对运动检测器进行建模。输入信号 `M_mode` 在此传感器的 `Active` 和 `Disabled` 模式之间进行选择。当传感器被激活时，输入信号 `Mot_sens` 指示可能存在入侵。

为减轻偶发误报的影响，运动检测器采用了一种去抖设计，因此只有持续的正触发信号才会产生警报。相反，门传感器和窗传感器将单个正触发信号解释为入侵并立即发出警报。

称为 `Alarm` 的第四种并行状态对警报系统的工作模式进行建模。输入信号 `Alarm_active` 在警报的 `On` 和 `Off` 模式之间进行选择。如果一个传感器在警报子系统开启时检测到入侵，该传感器会将局部事件 `Alert` 广播到 `Alarm` 状态。在状态 `Alarm` 的 `On` 子状态中，该事件触发从 `Idle` 子状态到 `Pending` 子状态的转移。当 `Pending` 被激活时，会发出警报声，提醒住户可能发生入侵。如果发生误报，则住户可以在较短的一段时间内关闭安全系统。如果在这段时间内没有禁用，系统会向警方报警求助，然后返回 `Idle` 模式。

### 与其他 Simulink 模块协调

Stateflow 图也可以使用事件与 Simulink® 模型中的其他模块进行通信。

![](https://ww2.mathworks.cn/help/stateflow/gs/eventsgetstartedexample_02_zh_CN.png)

**输出事件**

输出事件是在 Stateflow 图中发生但在 Stateflow 图之外的 Simulink 模块中可见的事件。这种类型的事件支持 Stateflow 图将该图中发生的事件通知给其他模块。例如，在此示例中，输出事件 `Sound` 和 `call_police` 负责驱动用来处理警报声和向警方报警的外部模块。当局部事件 `Alert` 触发到状态 `Alarm` 的 `Pending` 子状态的转移时，图会广播这些事件。特别是，在 `Pending` 子状态中，entry 动作会广播 `Sound` 事件。同样，发生从 `Pending` 到 `Idle` 的转移时执行的条件动作会广播 `call_police` 事件。在每种情况下，广播输出事件的动作都使用 `[send](https://ww2.mathworks.cn/help/stateflow/ref/send.html)` 运算符和事件名称：

每个输出事件映射到图上的一个输出端口。根据配置，对应的信号可以控制触发子系统或函数调用子系统。要配置输出事件，请执行以下操作：

1. 在**建模**选项卡的**设计数据**下，选择**符号窗格**和**属性检查器**。
2. 在**符号**窗格中，选择该输出事件。
3. 在**属性检查器**中，将**触发器**设置为以下选项之一：

- `Either edge` - 输出事件广播导致传出信号在 0 和 1 之间切换。
- `Function call` - 输出事件广播导致 Simulink 函数调用事件。

在此示例中，输出事件使用边沿触发器来激活 Simulink 模型中的一对锁存子系统。当每个锁存子系统检测到其输入信号中的值发生变化时，它会短暂输出值 1，然后再返回到 0 输出。

**输入事件**

输入事件是在 Simulink 模块中发生但在 Stateflow 图中可见的事件。这种类型的事件支持其他 Simulink 模块（包括其他 Stateflow 图）将在特定 Stateflow 图之外发生的事件通知该图。例如，在此示例中，输入事件 `sl_call` 控制运动检测器去抖器的时序以及向警方报警之前的短时延迟。在每个实例中，对时序运算符 `after` 的调用内会对该事件计数，并在图接收事件达一定次数后触发转移。

外部 Simulink 模块通过连接到 Stateflow 图上的触发端口的信号发送输入事件。根据具体配置，输入事件是由信号值的变化或通过 Simulink 模块中的函数调用产生的。要配置输入事件，请执行以下操作：

1. 在**建模**选项卡的**设计数据**下，选择**符号窗格**和**属性检查器**。
2. 在**符号**窗格中，选择该输入事件。
3. 在**属性检查器**中，将**触发器**设置为以下选项之一：

- `Rising` - 当输入信号从零或负值变为正值时，图被激活。
- `Falling` - 当输入信号从正值变为零或负值时，图被激活。
- `Either` - 当输入信号在任一方向变化且过零时，图被激活。
- `Function` 调用 - 图通过从 Simulink 模块的函数调用激活。

在此示例中，一个 Simulink Function-Call Generator 模块通过周期函数调用触发输入事件 `sl_call` 来控制安全系统的时序。

### 探索示例

在此示例中，Stateflow 图从几个 Manual Switch 模块接收输入，并输出到一对连接到 Display 模块的锁存子系统。在仿真过程中，您可以：

- 通过点击 Switch 模块启用警报和传感器子系统并触发入侵检测。
- 观看图动画，其中突出显示了图中的各种激活状态。
- 查看 Scope 模块和仿真数据检查器中的输出信号。

例如，假设您打开警报和传感器子系统，关闭传感器触发器，并开始仿真。在仿真过程中，您执行以下动作：

1. 在 ![](https://ww2.mathworks.cn/help/stateflow/gs/eventsgetstartedexample_eq11392407565812330029_zh_CN.png) 秒的时刻，您触发门传感器。警报开始响起 (`Sound = 1`)，因此您立即禁用警报系统。您关闭门传感器触发器，然后重新打开警报。
2. 在 ![](https://ww2.mathworks.cn/help/stateflow/gs/eventsgetstartedexample_eq08815713516279239351_zh_CN.png) 秒的时刻，您触发窗传感器并且警报开始响起 (`Sound = 0`)。这次，您不禁用警报。在大约 ![](https://ww2.mathworks.cn/help/stateflow/gs/eventsgetstartedexample_eq18318288194079829825_zh_CN.png) 秒的时刻，安全系统向警方报警 (`call_police = 1`)。`Sound` 和 `call_police` 信号继续每隔 80 秒在 0 和 1 之间切换一次。
3. 在 ![](https://ww2.mathworks.cn/help/stateflow/gs/eventsgetstartedexample_eq01438897797695050524_zh_CN.png) 秒的时刻，您禁用警报。`Sound` 和 `call_police` 信号停止切换。

仿真数据检查器显示 `Sound` 和 `call_police` 信号对您的动作的响应。

![](https://ww2.mathworks.cn/help/stateflow/gs/xxsf_security-sdi_zh_CN.png)

## 另请参阅

[`send`](https://ww2.mathworks.cn/help/stateflow/ref/send.html)

## 相关主题

- [使用并行分解对同步子系统建模](https://ww2.mathworks.cn/help/stateflow/gs/parallel-decomposition.html)
- [通过发送输入事件激活 Stateflow 图](https://ww2.mathworks.cn/help/stateflow/ug/using-input-events-to-activate-a-stateflow-chart.html)
- [通过发送输出事件激活 Simulink 模块](https://ww2.mathworks.cn/help/stateflow/ug/using-output-events-to-activate-a-simulink-block.html)

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
