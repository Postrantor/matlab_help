---
url: https://ww2.mathworks.cn/help/stateflow/ug/overview-of-mealy-and-moore-machines.html
title: Mealy 和 Moore 状态机概述
- MATLAB & Simulink
- MathWorks 中国
date: 2023-07-12 16:01:36
tag: 
summary: 创建实现 Mealy 和 Moore 状态机语义的图。
---
> [!NOTE]
> .[使用 Mealy 和 Moore 图进行序列识别](https://ww2.mathworks.cn/help/stateflow/ug/sequence-recognition-using-mealy-and-moore-charts.html)

## Mealy 和 Moore 状态机概述

在有限状态机中，*状态*是局部数据和图活动的组合。“计算状态” 意味着更新局部数据并产生从当前激活状态到新状态的转移。在状态机模型中，下一状态是当前状态及其输入的函数：

在此方程中：

- X(n) 表示位于时间步 n 的状态。
- X(n+1) 表示位于下一时间步 n+1 的状态。
- u 表示输入。

状态会从一个时间步保留到下一个时间步。

### Mealy 和 Moore 状态机语义

Mealy 状态机和 Moore 状态机经常被视为有限状态机建模的基本业界标准范式。您可以创建实现纯 Mealy 和 Moore 语义的图，作为 Stateflow® 图语义的一部分。您可以使用 Embedded Coder®、Simulink® Coder™ 和 HDL Coder™ 软件在仿真和代码生成中使用 Mealy 和 Moore 图。在 MATLAB® 中的独立 Stateflow 图中不支持 Mealy 和 Moore 语义。

#### Mealy 图的语义

Mealy 状态机是有限状态机，其中转移发生在时钟沿上。Mealy 图的输出是输入和状态的函数：

在每个时间步，Mealy 图都会唤醒，计算自身的输入，然后转移到新的激活状态配置，也称为其*下一个状态*。图在转移到下一个状态时计算其输出。

为了确保输出是输入和状态的函数，Mealy 状态机强制实施以下语义：

- 输出不依赖于下一个状态。
- 图仅在转移时计算输出，而不在状态中计算输出。
- 图根据系统时钟周期性唤醒。

Mealy 状态机在转移时计算其输出。因此，Mealy 图可以在图的默认路径执行时计算其第一个输出。如果为 Mealy 图启用图属性**初始化时执行 (进入) 图**，此计算将在 t = 0（第一个时间步）时发生。否则，计算将在 t = 1（下一个时间步）时发生。有关详细信息，请参阅[初始化时执行 (进入) 图](https://ww2.mathworks.cn/help/stateflow/ug/specifying-chart-properties.html#mw_b5b1852c-6797-48c0-9f6b-754fb1303b08)。

#### Moore 图的语义

Moore 状态机是有限状态机，其中输出在时钟沿上发生更改。Moore 图的输出仅是状态的函数：

在每个时间步，Moore 图都会唤醒，计算自身的输出，然后计算其输入以在下一个时间步重新配置。例如，图在计算其输入后，可以转移到新的激活状态配置。图在计算其输入并更新状态之前计算输出。

为了确保输出仅是当前状态的函数，Moore 状态机强制实施以下语义：

- 输出不依赖于输入。
- 输出不依赖于以前的输出。
- 输出不依赖于时序逻辑。

Moore 状态机在状态中计算其输出。因此，Moore 状态机只能在默认路径执行*后*计算输出。此时，输出会采用默认值。

### 创建 Mealy 和 Moore 图

创建 Stateflow 图时，默认类型是混合状态机模型，称为 _Classic_ 图。Classic 图将 Mealy 和 Moore 图的语义与扩展的 Stateflow 图语义相结合。

要创建 Mealy 图，请在 MATLAB 命令提示符下输入：

![](https://ww2.mathworks.cn/help/stateflow/ug/mm_icon_mealy_chart.png)

要创建 Moore 图，请在 MATLAB 命令提示符下输入：

![](https://ww2.mathworks.cn/help/stateflow/ug/mm_icon_moore_chart.png)

或者，在将 Stateflow 图模块添加到 Simulink 模型后，您可以通过设置**状态机类型**图属性来选择图的语义类型。有关详细信息，请参阅[状态机类型](https://ww2.mathworks.cn/help/stateflow/ug/specifying-chart-properties.html#mw_9e51b7ce-dd23-4367-aa83-2a9fa30d90bd)。

### Mealy 和 Moore 图的优势

与 Classic Stateflow 图相比，Mealy 和 Moore 图具有以下优势：

- 您可以对所创建的 Mealy 和 Moore 图进行验证，确保它们符合相应的形式化定义和语义规则。错误消息会在编译时而非设计时出现。
- 对于 C/C++ 和 HDL 目标，Moore 图可以提供比 Classic 图更高效的实现。
- 可以使用 Moore 图进行反馈回路建模。在 Moore 图中，输入没有直接馈通。您可以设计一个带有从输出端口到输入端口反馈的回路，而又不会引入代数环。Mealy 图和 Classic 图具有直接馈通，因此会在存在代数环时产生错误。

  ![](https://ww2.mathworks.cn/help/stateflow/ug/mm_algebraic_loop.png)

## 另请参阅

[`sfnew`](https://ww2.mathworks.cn/help/stateflow/ref/sfnew.html)

## 相关主题

- [Design Considerations for Mealy Charts](https://ww2.mathworks.cn/help/stateflow/ug/design-considerations-for-mealy-charts.html)
- [Design Considerations for Moore Charts](https://ww2.mathworks.cn/help/stateflow/ug/design-considerations-for-moore-charts.html)
- [Sequence Recognition by Using Mealy and Moore Charts](https://ww2.mathworks.cn/help/stateflow/ug/sequence-recognition-using-mealy-and-moore-charts.html)
- [Model a Vending Machine by Using Mealy Semantics](https://ww2.mathworks.cn/help/stateflow/ug/model-a-vending-machine-using-mealy-semantics.html)
- [Model a Traffic Light by Using Moore Semantics](https://ww2.mathworks.cn/help/stateflow/ug/model-a-traffic-light-using-moore-semantics.html)
- [Convert Charts Between Mealy and Moore Semantics](https://ww2.mathworks.cn/help/stateflow/ug/changing-the-chart-type.html)
