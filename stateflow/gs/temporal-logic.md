---
> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [ww2.mathworks.cn](https://ww2.mathworks.cn/help/stateflow/gs/temporal-logic.html)

> 通过使用时序逻辑运算符来定义图在仿真时间的行为。
---
要定义 Stateflow 图在仿真时间的行为，请在图的状态和转移动作中包含时序逻辑运算符。时序逻辑运算符是内置函数，告知状态保持激活的时间长度或布尔条件保持为 true 的时间长度。使用时序逻辑，您可以控制以下各项的时序：

以下是最常见的绝对时间时序逻辑运算符：

- `[after](https://ww2.mathworks.cn/help/stateflow/ref/after.html)` - 如果自包含该运算符的状态或包含该运算符的转移的源状态被激活以来经过的仿真时间达到 `n` 秒，则 `after(n,sec)` 返回 `true`。否则，运算符返回 `false`。此运算符支持以秒 (`sec`)、毫秒 (`msec`) 和微秒 (`usec`) 计的基于事件的时序逻辑和绝对时间时序逻辑。
- `[elapsed](https://ww2.mathworks.cn/help/stateflow/ref/elapsed.html)` - `elapsed(sec)` 返回自关联状态激活以来经过的仿真时间的秒数。
- `[duration](https://ww2.mathworks.cn/help/stateflow/ref/duration.html)` - `duration(C)` 返回自布尔条件 `C` 变为 `true` 以来经过的仿真时间的秒数。

### Bang-Bang 温度控制器建模

此示例使用时序逻辑对调节锅炉内部温度的 Bang-Bang 控制器建模。

![](https://ww2.mathworks.cn/help/stateflow/gs/temporallogicgetstartedexample_01_zh_CN.png)

该示例由 Stateflow 图和 Simulink® 子系统组成。`Bang-Bang Controller` 图将当前锅炉温度与参考设定值进行比较，并确定是否开启锅炉。`Boiler Plant Model` 子系统通过根据控制器的状态升高或降低温度，对锅炉内部的动态特性进行建模。然后，图使用锅炉温度进行下一步仿真。

`Bang-Bang Controller` 图使用时序逻辑运算符 `after` 来实现以下目的：

- 当锅炉在开启和关闭之间切换时，调节 Bang-Bang 循环的时序。
- 根据锅炉的工作模式控制以不同速率闪烁的状态 LED。

定义锅炉行为和 LED 子系统行为的计时器彼此独立运行，并不阻断或中断控制器的仿真。

### Bang-Bang 循环的时序

`Bang-Bang Controller` 图包含一对代表锅炉两种工作模式的子状态：`On` 和 `Off`。该图使用激活状态输出数据 `boiler` 来指示哪个子状态为激活状态。

![](https://ww2.mathworks.cn/help/stateflow/gs/temporallogicgetstartedexample_02_zh_CN.png)

`On` 和 `Off` 子状态之间转移上的条件定义 Bang-Bang 控制器的行为：

- 在发生从 `On` 到 `Off` 的第一个转移时，根据条件 `after(20,sec)`，锅炉在开启 20 秒后关闭。
- 在发生从 `Off` 到 `On` 的转移时，如果图形函数 `cold()` 指示锅炉温度低于参考设定值至少 40 秒，则条件 `after(40,sec)[cold()]` 开启锅炉。
- 在发生从 `On` 到 `Off` 的第二个转移时，`On` 状态中的内部转移逻辑确定在锅炉温度等于或高于参考设定值时关闭锅炉。

由于存在这些转移动作，Bang-Bang 循环的时序取决于锅炉的当前温度。在仿真开始时，锅炉较冷，控制器在 `Off` 状态下保持 40 秒，在 `On` 状态下保持 20 秒。在时间到达 ![](https://ww2.mathworks.cn/help/stateflow/gs/temporallogicgetstartedexample_eq05317045536900750900_zh_CN.png) 秒时，锅炉的温度达到参考设定值。自这一时刻起，锅炉仅需补偿在 `Off` 状态下的热损失。因而控制器在 `Off` 状态下保持 40 秒，在 `On` 状态下保持 4 秒。

![](https://ww2.mathworks.cn/help/stateflow/gs/xxsf_boiler-sdi-temp_zh_CN.png)

### 状态 LED 的时序

`Off` 状态包含子状态 `Flash`，其自环转移由动作 `after(5,sec)` 触发。由于这种转移，当 `Off` 状态被激活时，子状态将执行其 `entry` 动作并每隔 5 秒就调用一次图形函数 `flash_LED`。该函数使输出符号 `LED` 的值在 0 和 1 之间切换。

![](https://ww2.mathworks.cn/help/stateflow/gs/temporallogicgetstartedexample_03_zh_CN.png)

`On` 状态调用图形函数 `flash_LED` 作为 `entry, during` 组合状态的动作。当 `On` 状态被激活时，此动作在仿真的每个时间步调用该函数，以在 0 和 2 之间切换输出符号 `LED` 的值。

![](https://ww2.mathworks.cn/help/stateflow/gs/temporallogicgetstartedexample_04_zh_CN.png)

因此，状态 LED 的时序取决于锅炉的工作模式。例如：

- 从 ![](https://ww2.mathworks.cn/help/stateflow/gs/temporallogicgetstartedexample_eq05490811019241878006_zh_CN.png) 秒到 ![](https://ww2.mathworks.cn/help/stateflow/gs/temporallogicgetstartedexample_eq03650817669656050065_zh_CN.png) 秒，锅炉关闭，并且 LED 信号每隔 5 秒就在 0 和 1 之间交替一次。
- 从 ![](https://ww2.mathworks.cn/help/stateflow/gs/temporallogicgetstartedexample_eq03650817669656050065_zh_CN.png) 秒到 ![](https://ww2.mathworks.cn/help/stateflow/gs/temporallogicgetstartedexample_eq01915627481382864964_zh_CN.png) 秒，锅炉开启，并且 LED 信号每隔一秒就在 0 和 2 之间交替一次。
- 从 ![](https://ww2.mathworks.cn/help/stateflow/gs/temporallogicgetstartedexample_eq01915627481382864964_zh_CN.png) 秒到 ![](https://ww2.mathworks.cn/help/stateflow/gs/temporallogicgetstartedexample_eq09033346722860809468_zh_CN.png) 秒，锅炉关闭，并且 LED 信号每隔 5 秒就在 0 和 1 之间交替一次。

![](https://ww2.mathworks.cn/help/stateflow/gs/xxsf_boiler-sdi-led_zh_CN.png)

### 探索示例

使用其他时序逻辑来研究随着锅炉温度接近参考设定值，Bang-Bang 循环的时序如何变化。

1. 输入调用 `elapsed` 和 `duration` 运算符的新状态动作：

- 在 `On` 状态下，将 `Timer1` 设置为 `On` 状态被激活的时间长度：

```
en,du,ex: Timer1 = elapsed(sec);
```

- 在 `Off` 状态下，将 `Timer2` 设置为锅炉温度等于或高于参考设定值的时间长度：

```
en,du,ex: Timer2 = duration(temp>=reference);
```

2. 在**符号**窗格中，点击**解析未定义的符号**。Stateflow 编辑器将符号 `Timer1` 和 `Timer2` 解析为输出数据。
3. 对 `Timer1` 和 `Timer2` 启用记录。在**符号**窗格中，选择每个符号。然后，在**属性检查器**中的**记录**下，选择**记录信号数据**。
4. 在**仿真**选项卡中，点击**运行**。
5. 在**仿真**选项卡中的**查看结果**下，点击**数据检查器**。
6. 在仿真数据检查器中，在同一坐标区中显示信号 `boiler` 和 `Timer1`。绘图表明：

- 通常情况下，在锅炉冷时 Bang-Bang 循环的 `On` 阶段持续 20 秒，在锅炉热时持续 4 秒。
- 在锅炉第一次达到参考温度时，循环会提前中断，控制器在 `On` 状态仅保持 18 秒。
- 在锅炉变热后，第一个循环的时间比随后的循环时间略短，在该循环中控制器在 `On` 状态仅保持了 3 秒。

![](https://ww2.mathworks.cn/help/stateflow/gs/xxsf_boiler-sdi-timer1_zh_CN.png)

7. 在仿真数据检查器中，在同一坐标区中显示信号 `boiler` 和 `Timer2`。绘图表明：

- 在锅炉变热后，Bang-Bang 循环的 `Off` 阶段通常需要 9 秒才能冷却。
- 锅炉第一次达到参考温度时，需要 19 秒来冷却，这是其他循环冷却时间的两倍多。

![](https://ww2.mathworks.cn/help/stateflow/gs/xxsf_boiler-sdi-timer2_zh_CN.png)

较短的循环和较长的冷却时间是由于 `On` 状态内的子状态层次结构导致的。当锅炉首次达到参考温度时，从 `HIGH` 到 `NORM` 的转移会使控制器在一个额外的时间步内保持开启，导致锅炉温度超过正常值。而在后面的循环中，历史结点导致 `On` 阶段以激活的 `NORM` 子状态开始。然后，控制器在锅炉达到参考温度后立即关闭，从而冷却锅炉。

## 相关主题

- [通过使用状态和转移动作来定义图行为](https://ww2.mathworks.cn/help/stateflow/gs/actions.html)
- [使用时序逻辑控制图的执行](https://ww2.mathworks.cn/help/stateflow/ug/using-temporal-logic-in-state-actions-and-transitions.html)
- [通过定义图形函数重用逻辑模式](https://ww2.mathworks.cn/help/stateflow/ug/graphical-functions-for-reusing-logic-patterns-and-iterative-loops.html)
- [Resume Prior Substate Activity by Using History Junctions](https://ww2.mathworks.cn/help/stateflow/ug/recording-state-activity-with-history-junctions.html)

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
