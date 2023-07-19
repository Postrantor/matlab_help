---
url: https://ww2.mathworks.cn/help/stateflow/ug/watching-data-values-during-simulation.html
title: 在调试模式下检查和修改数据和消息
- MATLAB & Simulink
- MathWorks 中国
date: 2023-07-12 16:18:03
tag: 
summary: 向您说明在仿真过程中跟踪 Stateflow 数据和自主活动值的各种方法。
---
## 在调试模式下检查和修改数据和消息

当您的 Stateflow® 图处于调试模式时，您可以通过检查数据、消息和时序逻辑表达式的值来检查图的状态。您还可以通过修改数据值以及发送局部和输出消息来测试图的设计。下表总结了可用于执行这些调试任务的接口。有关详细信息，请参阅 [Set Breakpoints to Debug Charts](https://ww2.mathworks.cn/help/stateflow/ug/set-breakpoints-to-debug-charts.html)。

<table><colgroup><col width="33%"><col width="17%"><col width="17%"><col width="17%"><col width="17%"></colgroup><thead><tr><th>调试任务</th><th>Stateflow 编辑器</th><th>符号窗格</th><th>断点和监视窗口</th><th>MATLAB<sup>®</sup> 命令行窗口</th></tr></thead><tbody><tr><td>检查数据和消息的值</td><td><a href="https://ww2.mathworks.cn/help/stateflow/ug/watching-data-values-during-simulation.html#bugsdu8-2">可以</a></td><td><a href="https://ww2.mathworks.cn/help/stateflow/ug/watching-data-values-during-simulation.html#mw_bf8b3e7c-6a3b-4754-90d8-b2fb26c6b1d9">可以</a></td><td><a href="https://ww2.mathworks.cn/help/stateflow/ug/watching-data-values-during-simulation.html#bugsdu8-1">可以</a></td><td><a href="https://ww2.mathworks.cn/help/stateflow/ug/watching-data-values-during-simulation.html#bugsdu8-3">可以</a></td></tr><tr><td>检查时序逻辑表达式</td><td><a href="https://ww2.mathworks.cn/help/stateflow/ug/watching-data-values-during-simulation.html#bugsdu8-2">可以</a></td><td>不可以</td><td>不可以</td><td>不可以</td></tr><tr><td>修改数据和消息的值</td><td>不可以</td><td><a href="https://ww2.mathworks.cn/help/stateflow/ug/watching-data-values-during-simulation.html#mw_bf8b3e7c-6a3b-4754-90d8-b2fb26c6b1d9">可以</a></td><td>不可以</td><td><a href="https://ww2.mathworks.cn/help/stateflow/ug/watching-data-values-during-simulation.html#bsq1usn">可以</a></td></tr><tr><td>发送消息</td><td>不可以</td><td>不可以</td><td>不可以</td><td><a href="https://ww2.mathworks.cn/help/stateflow/ug/watching-data-values-during-simulation.html#mw_043c8165-a185-45e9-9e07-5a78b5d2811f">可以</a></td></tr></tbody></table>

### 在 Stateflow 编辑器中查看数据

当仿真在断点处暂停时，您可以通过指向图中的状态、转移或函数来检查数据值。工具提示会显示所选对象使用的数据和消息的值。

<table><colgroup><col width="44%"><col width="56%"></colgroup><thead><tr><th>对象类型</th><th>工具提示信息</th></tr></thead><tbody><tr><td>状态和转移</td><td>对象使用的数据、消息和时序逻辑表达式的值</td></tr><tr><td>图形函数、真值表函数和 MATLAB 函数</td><td>局部数据、消息、函数作用域内的输入和输出的值</td></tr></tbody></table>

例如，当 `second` 状态计算其 `during` 动作时，此图中的断点会暂停仿真。指向父状态 `gear` 会显示工具提示，该工具提示显示以下各项的值：

- 时序逻辑表达式 `duration(speed >= up_threshold)` 和 `duration(speed <= down_threshold)`。
- 数据，包括 `speed`、`up_threshold` 和 `up`。

![](https://ww2.mathworks.cn/help/stateflow/ug/sf_car_using_duration_tooltip.png)

**注意**

如果选择图属性**导出图级别函数**和**将导出的函数视为全局可见**，则工具提示不显示时序逻辑数据。

### 在 “符号” 窗格中查看和修改数据

当图处于调试模式时，**符号**窗格会显示图中每个数据和消息对象的值。例如，当此图在断点处暂停时，您可以在**值**列中看到列出的所有图数据的值。突出显示的值是在上一个时间步中发生了更改的值。

![](https://ww2.mathworks.cn/help/stateflow/ug/sf_car_using_duration_symbols_pane.png)

在**符号**窗格中，您可以更改以下各项的值：

- 数据存储内存、局部数据和输出数据。
- 局部消息和输出消息。

点击数据或消息对象的**值**字段以输入新值。

在仿真过程中，无法更改常量、参数或输入数据和消息的值。

有关详细信息，请参阅 [Manage Symbols in the Stateflow Editor](https://ww2.mathworks.cn/help/stateflow/ug/use-the-symbols-window-with-stateflow-data-events-and-messages.html)。

### 查看 “断点和监视” 窗口中的数据

当仿真在断点处暂停时，您可以在 Stateflow 的 “断点和监视” 窗口中查看当前的数据和消息值。要打开 “断点和监视” 窗口，请在**调试**选项卡上点击**断点列表**。或者，打开 “断点” 对话框，然后点击**断点列表**链接。

#### 跟踪观察列表中的数据

您可以使用 “断点和监视” 窗口实现以下目的：

- 将数据和消息对象添加到观察列表中。
- 跟踪自上一个时间步以来发生更改的值。
- 展开消息以查看消息队列和消息数据值。

例如，您可以将 `speed`、`up_threshold` 和 `up` 添加到观察列表中，并在步进仿真时跟踪其值。高亮的部分指示 `speed` 和 `up_threshold` 的值在上一个时间步期间发生变化。

![](https://ww2.mathworks.cn/help/stateflow/ug/sf_car_using_duration_watch.png)

要将数据或消息对象添加到观察列表，请打开**属性检查器**或模型资源管理器。选择您要观察的数据或消息对象，然后点击链接。

或者，在 Stateflow 编辑器中，右键点击使用数据或消息的状态或转移。选择**添加到监视窗口**，并从下拉列表中选择变量名称。

#### 格式化观察显示

要更改用于显示观察数据的格式，请选择窗口顶部的齿轮图标

![](https://ww2.mathworks.cn/help/stateflow/ug/breakpoint-watch-gear.png)

。使用下拉列表为每个数据类型选择一种 MATLAB 格式。

![](https://ww2.mathworks.cn/help/stateflow/ug/configure_watch_data_format.png)

#### 从观察列表中删除数据

要从观察列表中删除数据或消息对象，请指向观察数据的路径，然后点击变量名称左侧的**删除此监视项**图标。

![](https://ww2.mathworks.cn/help/stateflow/ug/sf_car_using_duration_remove_watch.png)

#### 保存和还原观察数据

观察数据在 MATLAB 会话期间持续存在。当您关闭模型时，它的观察数据列表仍保留在 “断点和监视窗口” 中。如果在同一 MATLAB 会话期间重新打开一个模型，该模型的观察数据列表将被还原。

您可以保存断点和观察数据列表，并在以后的 MATLAB 会话中重新加载它们。要保存断点和观察数据列表的快照，请在 “断点和监视” 窗口顶部，点击**保存当前断点和监视项**图标。要还原快照，请点击**加载断点和监视项**图标。

### 在 MATLAB 命令行窗口中查看和修改数据

当仿真在断点处暂停时，MATLAB 命令提示符变为 `debug>>`。在此提示符下，您可以检查和更改 Stateflow 数据的值，发送局部和输出消息，并与 MATLAB 工作区交互。

例如，假设上一个图已到达某断点。要查看在当前作用域下可见的数据，请使用 [`whos`](https://ww2.mathworks.cn/help/matlab/ref/whos.html) 命令。

```
  Name                Size            Bytes  Class       Attributes

  TWAIT               1x1                 1  uint8
  down                1x1                 1  logical
  down_th             1x1                 8  double
  down_threshold      1x1                 8  double
  gear                1x1                 4  gearType
  speed               1x1                 8  double
  throttle            1x1                 8  double
  up                  1x1                 1  logical
  up_th               1x1                 8  double
  up_threshold        1x1                 8  double


```

要检查 `speed` 和 `up_threshold` 的值，请输入：

#### 使用调试提示符修改数据

在调试提示符下，您可以更改数据存储内存、局部和输出数据的值。例如，在上一个图中，您可以更改 `up_threshold`、`up` 和 `gear` 的值：

在调试提示符下修改数据时，请遵循以下规则。

- 要修改向量和矩阵，请使用 MATLAB 语法进行索引，而不考虑图的动作语言属性。请参阅[索引表示法](https://ww2.mathworks.cn/help/stateflow/ug/operations-for-vectors-and-matrices.html#mw_bab98d0e-f45e-4dff-ad88-8fab2668747a)。

  例如，要更改 2×2 矩阵 `u` 的对角线上的元素，请输入：

  ```
  u(1,1) = 6.022e23;
  u(2,2) = 6.626e-34

  ```
- 对于可变大小的数据，可以在指定的维度范围内更改其维度。例如，假设 `v` 是可变大小数组，其最大大小为 `[16 16]`。要将 `v` 的值更改为由 1 组成的 5×7 数组，请输入：
- 要修改枚举数据，请使用带前缀的标识符显式指定枚举类型。请参阅 [Notation for Enumerated Values](https://ww2.mathworks.cn/help/stateflow/ug/using-enumerated-data.html#brqmyi3)。

  例如，假设 `w` 具有枚举数据类型 `Colors`。要将 `w` 的值更改为枚举值 `Red`，请输入：
- 要修改数值数据，请使用 MATLAB 类型转换函数将其转换为显式数据类型。`double` 类型的数据不需要显式转换。请参阅[类型转换运算](https://ww2.mathworks.cn/help/stateflow/ug/operations-for-stateflow-data.html#f0-61218)。

  例如，假设 `x` 的类型为 `single`，`y` 的类型为 `int32`，`z` 的类型为 `fixdt(1,16,12)`。要更改这些数据对象的值，请输入：

  ```
  x = single(98.6);
  y = int32(100);
  z = fi(0.5413,1,16,12);

  ```
- 您无法在调试提示符下更改常量、参数或输入数据的值。

#### 使用调试提示符发送消息

在调试提示符下，您可以发送局部和输出消息。例如，在下图中，局部消息 `M` 确定在 `DecisionPoint` 后哪个状态变为激活状态。如果该图收到带有正值的消息 `M`，则状态 `Received` 变为激活状态，图输出值 `true`。否则，状态 `Missed` 变为激活状态，图输出值 `false`。

![](https://ww2.mathworks.cn/help/stateflow/ug/sf_message_debug.png)

消息的初始值为零。要将数据字段的值更改为正数并将消息发送到其局部队列，请输入：

当您前进到仿真的下一步时，该消息将触发向 `Received` 状态的转移。有关详细信息，请参阅 [Control Chart Execution After a Breakpoint](https://ww2.mathworks.cn/help/stateflow/ug/control-chart-execution-after-breakpoint.html)。

从调试提示符发送消息时，请遵循以下规则：

- 要读取或写入有效消息的消息数据字段，请使用消息对象的名称。不要使用圆点表示法语法。
- 仅当图通过调用 [`send`](https://ww2.mathworks.cn/help/stateflow/ref/send.html) 运算符显式发送消息时，才能从调试提示符发送消息。
- 您无法从调试提示符发送输入消息。

有关详细信息，请参阅 [Control Message Activity in Stateflow Charts](https://ww2.mathworks.cn/help/stateflow/ug/message-operations.html)。

#### 在调试模式下访问 MATLAB 工作区

您可以在调试提示符下输入其他 MATLAB 命令，但结果会在 Stateflow 工作区中执行。例如，您可以使用 [`save`](https://ww2.mathworks.cn/help/matlab/ref/save.html) 函数将所有图变量保存在 MAT 文件中：

要在 MATLAB 基础工作区中输入命令，请使用 [`evalin`](https://ww2.mathworks.cn/help/matlab/ref/evalin.html) 命令和第一个参数 `"base"`。例如，要列出 MATLAB 工作区中的变量，请使用以下命令：

## 另请参阅

[`whos`](https://ww2.mathworks.cn/help/matlab/ref/whos.html) | [`save`](https://ww2.mathworks.cn/help/matlab/ref/save.html) | [`evalin`](https://ww2.mathworks.cn/help/matlab/ref/evalin.html) | [`send`](https://ww2.mathworks.cn/help/stateflow/ref/send.html)

## 相关主题

- [Set Breakpoints to Debug Charts](https://ww2.mathworks.cn/help/stateflow/ug/set-breakpoints-to-debug-charts.html)
- [Control Chart Execution After a Breakpoint](https://ww2.mathworks.cn/help/stateflow/ug/control-chart-execution-after-breakpoint.html)
- [Manage Symbols in the Stateflow Editor](https://ww2.mathworks.cn/help/stateflow/ug/use-the-symbols-window-with-stateflow-data-events-and-messages.html)
