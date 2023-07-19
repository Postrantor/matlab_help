---
> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [ww2.mathworks.cn](https://ww2.mathworks.cn/help/stateflow/gs/parallel-decomposition.html)

> 通过使用并行状态来实现同时被激活的多个工作模式。
---
要实现并发运行的工作模式，请在 Stateflow® 图中使用并行状态。例如，作为复杂系统设计的一部分，您可以使用并行状态对同时被激活的独立组件或子系统建模。

### 状态分解

图或状态的分解类型指定图或状态包含互斥状态还是并行状态：

- 互斥状态表示互斥的工作模式。在同一层级上不能有两个互斥状态同时被激活或执行。Stateflow 图用实线矩形表示每个互斥状态。

![](https://ww2.mathworks.cn/help/stateflow/gs/paralleldecompositiongetstartedexample_01_zh_CN.png)

- 并行状态表示独立的工作模式。虽然并行状态会依次执行，但可以有两个或多个并行状态同时处于激活状态。Stateflow 图用虚线矩形表示每个并行状态，其中的数字表示其执行顺序。

![](https://ww2.mathworks.cn/help/stateflow/gs/paralleldecompositiongetstartedexample_02_zh_CN.png)

通过在状态层次结构的不同层级设置状态分解，您可以在 Stateflow 图中组合互斥和并行状态。默认的状态分解类型是 `Exclusive (OR)`。要将分解类型更改为 `AND (Parallel)`，请右键点击父状态并选择**分解** > **AND (并行)**。

### 气温控制器建模

此示例使用并行分解对一个空调控制器进行建模，该控制器将物理被控对象中的气温保持在 120 度。

![](https://ww2.mathworks.cn/help/stateflow/gs/paralleldecompositiongetstartedexample_03_zh_CN.png)

在顶层，空调控制器图有两个互斥状态：`PowerOff` 和 `PowerOn`。该图使用互斥 (OR) 分解，因为控制器无法同时处于打开和关闭状态。

控制器使用两个风扇来工作。当气温升至 120 度以上时，启动第一个风扇。当气温升至 150 度以上时，启动第二个风扇以提供额外的冷却。图将这些风扇建模为顶层状态 `PowerOn` 的并行子状态 `FAN1` 和 `FAN2`。由于风扇作为独立的组件运行，根据所需冷却量的多少打开或关闭，`PowerOn` 使用并行 (AND) 分解来确保在控制器打开时两个子状态都被激活。

除工作阈值不同以外，这些风扇由具有相同子状态和转移配置的状态建模，表示两种风扇工作模式 `On` 和 `Off`。由于两个风扇都不能同时处于打开和关闭状态，`FAN1` 和 `FAN2` 具有互斥 (OR) 分解。

在 `PowerOn` 中，称为 `SpeedValue` 的第三个并行状态表示一个独立子系统，它计算在每个时间步内循环启动的风扇数。当 `FAN1` 的 `On` 状态被激活时，布尔表达式 `in(FAN1.On)` 的值为 1。否则，`in(FAN1.On)` 等于 0。同样，`in(FAN2.On)` 的值指示 `FAN2` 是处于循环打开还是关闭状态。这些表达式的总和表示在每个时间步中打开的风扇的数量。

### 指定并行状态的执行顺序

即使 `FAN1`、`FAN2` 和 `SpeedValue` 同时处于激活状态，这些状态在仿真过程中也是依次执行的。状态右上角的数字指定执行顺序。以下是此执行顺序的基本原理：

- `FAN1` 应首先执行，因为它循环打开的温度低于 `FAN2` 打开的温度。无论 `FAN2` 是打开还是关闭，它都可以打开。
- `FAN2` 以第二顺位执行，因为它循环打开的温度高于 `FAN1` 打开的温度。它仅在 `FAN1` 已打开时才能打开。
- `SpeedValue` 最后执行，因而可以观测 `FAN1` 和 `FAN2` 的最新状态。

默认情况下，Stateflow 根据您将并行状态添加到图的顺序来分配并行状态的执行顺序。要更改某并行状态的执行顺序，请右键点击该状态并从**执行顺序**下拉列表中选择值。

### 探索示例

此示例包含一个名为 `Air Controller` 的 Stateflow 图和一个名为 `Physical Plant` 的 Simulink® 子系统。

![](https://ww2.mathworks.cn/help/stateflow/gs/paralleldecompositiongetstartedexample_04_zh_CN.png)

根据物理被控对象的气温，图打开风扇，并向子系统输出正在运行的风扇的数量 `airflow`。此值根据以下规则确定冷却活动因子 ![](https://ww2.mathworks.cn/help/stateflow/gs/paralleldecompositiongetstartedexample_eq03091110158048648509_zh_CN.png)：

- `airflow` = 0 - 没有风扇运行。气温不会降低，因为 ![](https://ww2.mathworks.cn/help/stateflow/gs/paralleldecompositiongetstartedexample_eq14394746751260252312_zh_CN.png)。
- `airflow` = 1 - 一个风扇运行。气温降低，冷却活动因子 ![](https://ww2.mathworks.cn/help/stateflow/gs/paralleldecompositiongetstartedexample_eq16298905221389238741_zh_CN.png)。
- `airflow` = 2 - 两个风扇运行。气温降低，冷却活动因子 ![](https://ww2.mathworks.cn/help/stateflow/gs/paralleldecompositiongetstartedexample_eq12844725938971953643_zh_CN.png)。

Physical Plant 子系统块根据以下公式更新工厂内的气温 ![](https://ww2.mathworks.cn/help/stateflow/gs/paralleldecompositiongetstartedexample_eq02137513487127062277_zh_CN.png)：

其中：

- ![](https://ww2.mathworks.cn/help/stateflow/gs/paralleldecompositiongetstartedexample_eq01572441086277914966_zh_CN.png) 是初始温度。默认值为 70°。
- ![](https://ww2.mathworks.cn/help/stateflow/gs/paralleldecompositiongetstartedexample_eq10604485442030496953_zh_CN.png) 是环境温度。默认值为 160°。
- ![](https://ww2.mathworks.cn/help/stateflow/gs/paralleldecompositiongetstartedexample_eq07996253399464447700_zh_CN.png) 是被控对象的热传递因子。默认值为 0.01。
- ![](https://ww2.mathworks.cn/help/stateflow/gs/paralleldecompositiongetstartedexample_eq07628547966429477945_zh_CN.png) 是对应于 `airflow` 的冷却活动因子。

新温度决定仿真的下一个时间步的冷却量。

## 相关主题

- [通过使用状态分解定义互斥和并行模式](https://ww2.mathworks.cn/help/stateflow/ug/state-decomposition.html)
- [并行状态的执行顺序](https://ww2.mathworks.cn/help/stateflow/ug/execution-order-for-parallel-states.html)
- [Check State Activity by Using the in Operator](https://ww2.mathworks.cn/help/stateflow/ug/checking-state-activity.html)

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
