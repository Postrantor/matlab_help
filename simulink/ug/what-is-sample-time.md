---
tip: translate by openai@2023-07-07 22:19:01
url: https://www.mathworks.com/help/simulink/ug/what-is-sample-time.html
title: What Is Sample Time? - MATLAB & Simulink --- 什么是采样时间？ - MATLAB 和 Simulink
date: 2023-07-07 22:17:10
tag:
summary: The sample time of a block indicates when the block generates outputs or updates its internal state.
> 示例时间块表明该块何时产生输出或更新其内部状态。
---
## What Is Sample Time?

The _sample time_ of a block is a parameter that indicates when, during simulation, the block produces outputs and if appropriate, updates its internal state. The internal state includes but is not limited to continuous and discrete states that are logged.

> 样本时间是一个参数，它指示模拟过程中块的输出何时产生，如果适当，还会更新其内部状态。内部状态包括但不限于记录的连续和离散状态。

**Note**

Do not confuse the Simulink® usage of the term sample time with the engineering sense of the term. In engineering, sample time refers to the rate at which a discrete system samples its inputs. Simulink allows you to model single-rate and multirate discrete systems and hybrid continuous-discrete systems through the appropriate setting of block sample times that control the rate of block execution (calculations).

> 不要把 Simulink® 中使用的“采样时间”这个术语和工程技术领域中的“采样时间”混淆在一起。在工程技术中，采样时间指的是离散系统采样输入的频率。Simulink 可以通过适当设置块采样时间来模拟单速率和多速率离散系统以及混合连续-离散系统，这种设置可以控制块执行（计算）的频率。

For many engineering applications, you need to control the rate of block execution. In general, Simulink provides this capability by allowing you to specify an explicit `SampleTime` parameter in the block dialog or at the command line. Blocks that do not have a `SampleTime` parameter have an implicit sample time. You cannot specify implicit sample times. Simulink determines them based upon the context of the block in the system. The [Integrator](https://www.mathworks.com/help/simulink/slref/integrator.html) block is an example of a block that has an implicit sample time. Simulink automatically sets its sample time to `0`.

> 对于许多工程应用，您需要控制块执行的速率。通常，Simulink 通过允许您在块对话框中或命令行中指定显式“SampleTime”参数来提供此功能。没有“SampleTime”参数的块具有隐式采样时间。您无法指定隐式采样时间。Simulink 根据块在系统中的上下文确定它们。[Integrator](https://www.mathworks.com/help/simulink/slref/integrator.html) 块就是具有隐式采样时间的块的示例。Simulink 会自动将其采样时间设置为“0”。

Sample times can be port based or block based. For block-based sample times, all of the inputs and outputs of the block run at the same rate. For port-based sample times, the input and output ports can run at different rates. To learn more about rates of execution, see [Types of Sample Time](https://www.mathworks.com/help/simulink/ug/types-of-sample-time.html).

> 样本时间可以基于端口或块。对于基于块的样本时间，块的所有输入和输出以相同的速率运行。对于基于端口的样本时间，输入和输出端口可以以不同的速率运行。要了解有关执行速率的更多信息，请参阅[样本时间类型](https://www.mathworks.com/help/simulink/ug/types-of-sample-time.html)。

## Related Topics

- [Specify Sample Time](https://www.mathworks.com/help/simulink/ug/how-to-specify-the-sample-time.html)
- [Types of Sample Time](https://www.mathworks.com/help/simulink/ug/types-of-sample-time.html)
- [Sample Times in Systems](https://www.mathworks.com/help/simulink/ug/managing-sample-times-in-systems.html)
- [Sample Times in Subsystems](https://www.mathworks.com/help/simulink/ug/managing-sample-times-in-subsystems.html)
