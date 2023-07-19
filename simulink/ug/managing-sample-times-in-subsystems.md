---
tip: translate by openai@2023-07-07 22:22:14
url: https://www.mathworks.com/help/simulink/ug/managing-sample-times-in-subsystems.html
title: Sample Times in Subsystems
- MATLAB & Simulink
date: 2023-07-07 22:20:31
tag: 
summary: How Simulink calculates the sample times of virtual and enabled subsystems.
> 概要：Simulink如何计算虚拟子系统和启用子系统的采样时间。
---
## Sample Times in Subsystems

Subsystems fall into two categories: triggered and non-triggered. For triggered subsystems, in general, the subsystem gets its sample time from the triggering signal. One exception occurs when you use a [Trigger](https://www.mathworks.com/help/simulink/slref/trigger.html) block to create a triggered subsystem. If you set the block to and the to , the `SampleTime` parameter becomes active. In this case, _you_ specify the sample time of the Trigger block, which in turn, establishes the sample time of the subsystem.

> 子系统分为两类：触发和非触发。对于触发子系统，通常从触发信号获取采样时间。除此之外，当使用[触发器](https://www.mathworks.com/help/simulink/slref/trigger.html)块创建触发子系统时，会出现一个例外情况。如果您将该块设置为，并将其设置为，则 `SampleTime` 参数将被激活。在这种情况下，*您*指定触发块的采样时间，从而建立子系统的采样时间。

There are four non-triggered subsystems:

- Virtual
- Enabled
- Atomic
- Action

Simulink® calculates the sample times of virtual and enabled subsystems based on the respective sample times of their contents.

> Simulink® 根据虚拟子系统和启用子系统的内容的各自采样时间来计算这些子系统的采样时间。

The atomic subsystem is a special case in that the subsystem block has a `SystemSampleTime` parameter. Moreover, for a sample time other than the default value of –1, the blocks inside the atomic subsystem can have only a value of `Inf`, –1, or the identical (discrete) value of the subsystem `SampleTime` parameter. If the atomic subsystem is left as inherited, Simulink calculates the block sample time in the same manner as the virtual and enabled subsystems. However, the main purpose of the subsystem `SampleTime` parameter is to allow for the simultaneous specification of a large number of blocks, within an atomic subsystem, that are all set to inherited. To obtain the sample time set on an atomic subsystem, use this command at the command prompt:

> 原子子系统是一个特殊的情况，该子系统块具有 `SystemSampleTime` 参数。此外，对于默认值为-1 的采样时间，原子子系统内的块只能具有 Inf，-1 或与子系统 `SampleTime` 参数相同（离散）的值。如果原子子系统保留为继承，则 Simulink 以与虚拟和启用子系统相同的方式计算块采样时间。但是，子系统 `SampleTime` 参数的主要目的是允许同时指定原子子系统中大量设置为继承的块。要获取原子子系统上设置的采样时间，请在命令提示符下使用此命令：

```
get_param(AtomicSubsystemBlock,‘SystemSampleTime’);

```

Finally, the sample time of the action subsystem is set by the [If](https://www.mathworks.com/help/simulink/slref/if.html) block or the [Switch Case](https://www.mathworks.com/help/simulink/slref/switchcase.html) block.

> 最后，动作子系统的采样时间由 [If](https://www.mathworks.com/help/simulink/slref/if.html) 块或 [Switch Case](https://www.mathworks.com/help/simulink/slref/switchcase.html) 块设置。

For non-triggered subsystems where blocks have different sample rates, Simulink returns the Compiled Sample Time for the subsystem as a cell array of all the sample rates present in the subsystem. To see this, use the [`get_param`](https://www.mathworks.com/help/simulink/slref/get_param.html) command at MATLAB prompt.

> 对于未触发的子系统，其中块具有不同的采样率，Simulink 将子系统的编译采样时间返回为所有存在于子系统中的采样率的单元数组。要查看此内容，请在 MATLAB 提示符下使用[`get_param`](https://www.mathworks.com/help/simulink/slref/get_param.html)命令。

```
get_param(subsystemBlock,'CompiledSampleTime')

```

## Related Topics

- [Block Compiled Sample Time](https://www.mathworks.com/help/simulink/ug/determining-the-compiled-sample-time-of-a-block.html)
- [Sample Times in Systems](https://www.mathworks.com/help/simulink/ug/managing-sample-times-in-systems.html)
- [Specify Execution Domain](https://www.mathworks.com/help/simulink/ug/subsystem-domain-specification.html)
