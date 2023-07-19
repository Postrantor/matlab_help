---
> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [ww2.mathworks.cn](https://ww2.mathworks.cn/help/stateflow/gs/active-state-data.html)

> 自动跟踪仿真期间哪个状态被激活。
---
如果 Stateflow® 图包含与图层次结构相关的数据，则可以使用激活状态数据来简化设计。通过启用激活状态数据，您可以：

- 避免手动更新反映图活动的数据。
- 在仿真数据检查器中记录并监视图活动。
- 使用图活动数据来控制其他子系统。
- 将图活动数据导出到其他 Simulink 模块。

例如，在以下交通信号模型中，被激活的状态决定输出信号 `color` 的值。您可以通过启用激活状态数据来简化图的设计。在本例中，Stateflow 图可以通过跟踪状态活动来提供交通信号的颜色，因此您不必显式更新 `color` 的值。

![](https://ww2.mathworks.cn/help/stateflow/gs/activestatedatagetstartedexample_01_zh_CN.png)

要启用激活状态数据，请选择一个要监控的状态。然后，在**属性检查器**中：

1. 选择**创建监控输出**。
2. 选择以下活动类型之一：

- `Self activity` - 指示状态是否激活的布尔值
- `Child activity` - 指示哪个子状态被激活的枚举值
- `Leaf state activity` - 指示哪个叶状态被激活的枚举值

3. 输入激活状态数据符号的**数据名称**。

4.（可选）对于 `Child activity` 或 `Leaf state activity`，输入激活状态数据类型的**枚举名称**。

默认情况下，Stateflow 图将状态活动作为输出数据报告给 Simulink 模型。要将激活状态数据符号的作用域更改为局部数据，请使用**符号**窗格。

### 交通信号控制器建模

此示例使用激活状态数据为一对交通信号灯的控制器系统建模。

![](https://ww2.mathworks.cn/help/stateflow/gs/activestatedatagetstartedexample_02_zh_CN.png)

在 `Traffic Controller` 图中，由两个平行的子图管理着控制交通信号灯的逻辑。这两个子图具有相同的层次结构，即包含三个子状态：`Red`、`Yellow` 和 `Green`。输出数据 `Light1` 和 `Light2` 对应于子图中的激活子状态。这些信号：

- 确定动画交通信号灯的阶段。
- 帮助统计在每个信号灯下等待的汽车数量。
- 驱动一个 Safety Assertion 子系统，确认两个交通信号灯永远不会同时呈绿色。

要查看 Traffic Controller 图中的子图，请点击图左下角的箭头。

![](https://ww2.mathworks.cn/help/stateflow/gs/activestatedatagetstartedexample_03_zh_CN.png)

每个交通控制器循环遍历其子状态，从 `Red` 到 `Green` 到 `Yellow`，然后再返回到 `Red`。每个状态对应于交通信号灯循环中的一个阶段。输出信号 `Light1` 和 `Light2` 指示在任意给定时刻哪个状态被激活。

**红灯**

当 `Red` 状态被激活时，交通信号灯循环开始。经过短暂的延迟后，控制器检查在十字路口等待的汽车。如果检测到有至少一辆汽车或者经过了一定的时间，则控制器会将 `greenLightRequest` 设置为 `true`，以此来请求绿灯。在发出请求后，控制器继续保持 `Red` 状态较短的一段时间，直到检测到另一个交通信号为红色，控制器才会将状态转移到 `Green`。

**绿灯**

当 `Green` 状态被激活时，控制器会将 `greenLightRequest` 设置为 `false`，以此来取消其绿灯请求。控制器将 `greenLightLocked` 设置为 `true`，以防止另一个交通信号变绿。在短暂延迟后，控制器会检查是否有来自另一个控制器的绿灯请求。如果它收到请求或经过了一定的时间，则控制器会转移到 `Yellow` 状态。

**黄灯**

当 `Yellow` 状态变为非激活时，控制器会将 `greenLightLocked` 设置为 false，指示另一个交通信号灯可以安全地变绿。在转移到 `Red` 状态之前，控制器将保持 `Yellow` 状态一定的时间。然后开始下一个交通信号灯循环。

**交通信号灯的时序**

交通信号灯循环的时序通过几个参数来定义。要更改交通信号灯的计时，请双击 `Traffic Controller` 图，并在对话框中输入以下参数的新值：

- `REDDELAY` - 从红灯激活到控制器检查十字路口等待车辆之间的时间长度。此值也是控制器发出绿灯请求后，交通信号灯可以变绿的最短时间长度。默认值为 6 秒。
- `MAXREDDELAY` - 控制器在发出绿灯请求前检查车辆的最长时间。默认值为 360 秒。
- `GREENDELAY` - 交通信号灯保持绿色的最长时间。默认值为 180 秒。
- `MINGREENDELAY` - 交通信号灯保持绿色的最短时间长度。默认值为 120 秒。
- `YELLOWDELAY` - 交通信号灯保持黄色的时间长度。默认值为 15 秒。

### 探索示例

1. 点击左下角的箭头打开图。
2. 在**符号**窗格中，选择 `greenLightRequested`。然后，在**属性检查器**中的**记录**下，选择**记录信号数据**。
3. 对 `greenLightLocked`、`Light1` 和 `Light2` 重复上一步骤。
4. 在**仿真**选项卡中，点击**运行**。
5. 在**仿真**选项卡中的**查看结果**下，点击**数据检查器**。
6. 在仿真数据检查器中，将记录的信号显示在单独的坐标区中。布尔信号 `greenLightRequested` 和 `greenLightLocked` 显示为数值 0 或 1。状态活动信号 `Light1` 和 `Light2` 为枚举数据，其值为 `Green`、`Yellow`、`Red` 和 `None`。

![](https://ww2.mathworks.cn/help/stateflow/gs/xxsf_trafficlight-sdi_zh_CN.png)

要在仿真过程中跟踪图活动，可以使用仿真数据检查器中的缩放和光标按钮。例如，下面列出了仿真前 300 秒内的关键时刻：

- ![](https://ww2.mathworks.cn/help/stateflow/gs/activestatedatagetstartedexample_eq05490811019241878006_zh_CN.png) - 在仿真开始时，两个交通信号灯都为红色。`Light1` 和 `Light2` 为 `Red`，`greenLightRequested` 为 `false`，`greenLightLocked` 为 `false`。
- ![](https://ww2.mathworks.cn/help/stateflow/gs/activestatedatagetstartedexample_eq09978131082994638030_zh_CN.png) - 6 秒（`REDDELAY` 的默认值）后，两条街道上都有汽车在等待，因此两个交通信号灯都请求绿灯。`Light1` 和 `Light2` 仍为 `Red`，`greenLightRequested` 为 `true`，`greenLightLocked` 为 `false`。
- ![](https://ww2.mathworks.cn/help/stateflow/gs/activestatedatagetstartedexample_eq11951804155291995028_zh_CN.png) - 再过 6 秒（`REDDELAY` 的默认值）后，信号灯 1 变为绿色，取消绿灯请求，并将 `greenLightLocked` 设置为 `true`。然后，信号灯 2 请求绿灯。`Light1` 为 `Green`，`Light2` 为 `Red`，`greenLightRequested` 变为 `false` 然后为 `true`，`greenLightLocked` 为 `true`。
- ![](https://ww2.mathworks.cn/help/stateflow/gs/activestatedatagetstartedexample_eq15887113488953338383_zh_CN.png) - 120 秒（`MINGREENDELAY` 的默认值）后，信号灯 1 变为黄色。`Light1` 为 `Yellow`，`Light2` 为 `Red`，`greenLightRequested` 为 `true`，`greenLightLocked` 为 `true`。
- ![](https://ww2.mathworks.cn/help/stateflow/gs/activestatedatagetstartedexample_eq13311095447044303409_zh_CN.png) - 15 秒（`YELLOWDELAY` 的默认值）后，信号灯 1 变为红色，并将 `greenLightLocked` 设置为 `false`。然后，信号灯 2 变为绿色，取消绿灯请求，并将 `greenLightLocked` 设置为 `true`。`Light1` 为 `Red`，`Light2` 为 `Green`，`greenLightRequested` 为 `false`，`greenLightLocked` 变为 `false` 然后为 `true`。
- ![](https://ww2.mathworks.cn/help/stateflow/gs/activestatedatagetstartedexample_eq01350805154886934534_zh_CN.png) - 6 秒（`REDDELAY` 的默认值）后，信号灯 1 请求绿灯。`Light1` 为 `Red`，`Light2` 为 `Green`，`greenLightRequested` 为 `true`，`greenLightLocked` 为 `true`。
- ![](https://ww2.mathworks.cn/help/stateflow/gs/activestatedatagetstartedexample_eq10304234090842083408_zh_CN.png) - 在信号灯 2 为绿色 120 秒（`MINGREENDELAY` 的默认值）后，信号灯 2 变为黄色。`Light1` 为 `Red`，`Light2` 为 `Yellow`，`greenLightRequested` 为 `true`，`greenLightLocked` 为 `true`。
- ![](https://ww2.mathworks.cn/help/stateflow/gs/activestatedatagetstartedexample_eq05922833764625562942_zh_CN.png) - 15 秒（`YELLOWDELAY` 的默认值）后，信号灯 2 变为红色，并将 `greenLightLocked` 设置为 `false`。然后，信号灯 1 变为绿色，取消绿灯请求，并将 `greenLightLocked` 设置为 `true`。`Light1` 为 `Green`，`Light2` 为 `Red`，`greenLightRequested` 为 `false`，`greenLightLocked` 变为 `false` 然后为 `true`。
- ![](https://ww2.mathworks.cn/help/stateflow/gs/activestatedatagetstartedexample_eq17718326725850168440_zh_CN.png) - 6 秒（`REDDELAY` 的默认值）后，信号灯 2 请求绿灯。`Light1` 为 `Green`，`Light2` 为 `Red`，`greenLightRequested` 为 `true`，`greenLightLocked` 为 `true`。

重复循环，直到仿真于 ![](https://ww2.mathworks.cn/help/stateflow/gs/activestatedatagetstartedexample_eq01657301476922979099_zh_CN.png) 秒处结束。

## 相关主题

- [创建层次结构来管理复杂系统](https://ww2.mathworks.cn/help/stateflow/gs/hierarchy.html)
- [Monitor State Activity Through Active State Data](https://ww2.mathworks.cn/help/stateflow/ug/about-active-state-data.html)
- [Simplify Stateflow Charts by Incorporating Active State Output](https://ww2.mathworks.cn/help/stateflow/ug/use-active-state-output.html)
- [View State Activity by Using the Simulation Data Inspector](https://ww2.mathworks.cn/help/stateflow/ug/view-state-activity-using-simulation-data-inspector.html)

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
