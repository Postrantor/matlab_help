---
tip: translate by openai@2023-07-12 16:10:47
url: https://ww2.mathworks.cn/help/stateflow/ug/sequence-recognition-using-mealy-and-moore-charts.html
title: Sequence Recognition by Using Mealy and Moore Charts - MATLAB & Simulink - MathWorks 中国
date: 2023-07-12 16:08:17
tag:
summary: Use Mealy and Moore machines for a sequence recognition application in signal processing.
> 使用Mealy和Moore机器来实现信号处理中的序列识别应用。
---
This example shows how to use Mealy and Moore machines for a sequence recognition application in signal processing. For more information, see [Overview of Mealy and Moore Machines](https://ww2.mathworks.cn/help/stateflow/ug/overview-of-mealy-and-moore-machines.html).

> 这个例子展示了如何在信号处理中使用 Mealy 和 Moore 机器来识别序列。更多信息，请参阅 [Mealy 和 Moore 机器概述](https://ww2.mathworks.cn/help/stateflow/ug/overview-of-mealy-and-moore-machines.html)。

In this model, two Stateflow® charts use a different set of semantics to find the sequence `1`, `2`, `1`, `3` in the input signal from a [Signal Editor](https://ww2.mathworks.cn/help/simulink/slref/signaleditorblock.html) (Simulink) block.

> 在这个模型中，两个 Stateflow® 图使用不同的语义集来从[信号编辑器](https://ww2.mathworks.cn/help/simulink/slref/signaleditorblock.html)（Simulink）块的输入信号中找到序列 `1`，`2`，`1`，`3`。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/SequenceRecognitionUsingMealyAndMooreChartExample_01.png)

Each chart contains an input data `u` and two output data:

- `seqFound` indicates when the chart finds the sequence. A value of `false` means that the chart is still searching for the sequence. A value of `true` means that the chart has found the sequence.

> `- ` seqFound `表示图表发现序列的时间。值为` false `意味着图表仍在搜索序列。值为` true` 意味着图表已经找到了序列。

- `status` records the status of the sequence recognition. This value ranges from 0 to 4 and indicates the number of symbols detected by the chart.

> `状态` 记录序列识别的状态。该值的范围从 0 到 4，表示图表检测到的符号的数量。

The Moore chart outputs `seqFound` and `status` based on the current state of the chart. At each time step, the chart executes the actions for the current state, evaluates the input `u`, and transitions to a new state. For example, when the chart receives the sequence of input values `1`, `2`, `1`, `3` from the Signal Editor block, it transitions from state `s0` to state `s1` to state `s12` to state `s121` to state `s1213` in four time steps. The chart sets the value of `seqFound` to `true` in a state action after state `s1213` becomes active.

> Moore 图根据当前图表状态输出 `seqFound` 和 `status`。在每个时间步骤中，图表执行当前状态的操作，评估输入 `u`，并转换到新状态。例如，当图表从信号编辑器块接收到输入值 `1`，`2`，`1`，`3` 的序列时，它从状态 `s0` 转换到状态 `s1`，再转换到状态 `s12`，再转换到状态 `s121`，最后转换到状态 `s1213`，总共四个时间步骤。在状态 `s1213` 激活后，图表在状态动作中将 `seqFound` 的值设置为 `true`。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/SequenceRecognitionUsingMealyAndMooreChartExample_02.png)

The Mealy chart outputs `seqFound` and `status` based on the current state of the chart and the value of the input. At each time step, the chart evaluates the input `u`, makes the transition to a new state, and executes the corresponding condition actions. Because this chart computes its output values in the condition actions of its transitions, these actions are taken before the state becomes active. For example, when the chart receives the sequence of input values `1`, `2`, `1`, `3` from the Signal Editor block, it transitions from state `s0` to state `s1` to state `s12` to state `s121` to state `s1213` in four time steps. The chart sets the value of `seqFound` to `true` in a condition action in the same time step that state `s1213` becomes active.

> 根据当前图表的状态和输入值，Mealy 图表输出 `seqFound` 和 `status`。每一步时间，图表评估输入 `u`，进行过渡到新状态，并执行相应的条件动作。由于这张图表在其转换的条件动作中计算其输出值，因此这些动作是在状态激活之前执行的。例如，当图表从 Signal Editor 块接收到输入值 `1`，`2`，`1`，`3` 的序列时，它从状态 `s0` 转换到状态 `s1`，再转换到状态 `s12`，再转换到状态 `s121`，再转换到状态 `s1213`，在四个时间步骤中。在状态 `s1213` 激活的同一时间步骤中，图表将 `seqFound` 的值设置为 `true`。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/SequenceRecognitionUsingMealyAndMooreChartExample_03.png)

When you simulate the model, the `seqFound` scope shows that the output of the Moore chart lags one time step behind the output of the Mealy chart. The delay is a result of the Moore semantics, in which the output is based on the state of the chart at the start of each time step and not on the current input.

> 当你模拟模型时，`seqFound` 范围表明 Moore 图的输出比 Mealy 图的输出滞后一个时间步骤。这种延迟是 Moore 语义的结果，其中输出基于每个时间步骤开始时图表的状态，而不是当前的输入。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/SequenceRecognitionUsingMealyAndMooreChartExample_04.png)

### Reference

Katz, Bruce F. _Digital Design: From Gates to Intelligent Machines_, 2006.

> 《数字设计：从门到智能机器》，Katz Bruce F.，2006 年。

## See Also

[Signal Editor](https://ww2.mathworks.cn/help/simulink/slref/signaleditorblock.html) (Simulink)

> 信号编辑器（Simulink）

## Related Topics

- [Overview of Mealy and Moore Machines](https://ww2.mathworks.cn/help/stateflow/ug/overview-of-mealy-and-moore-machines.html)
- [Design Considerations for Mealy Charts](https://ww2.mathworks.cn/help/stateflow/ug/design-considerations-for-mealy-charts.html)
- [Design Considerations for Moore Charts](https://ww2.mathworks.cn/help/stateflow/ug/design-considerations-for-moore-charts.html)
