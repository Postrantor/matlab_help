---
tip: translate by openai@2023-07-09 10:23:20
url: https://www.mathworks.com/help/simulink/ug/types-of-sample-time.html#brrdmmw-7
title: Types of Sample Time - MATLAB & Simulink --- 采样时间的类型 - MATLAB & Simulink
date: 2023-07-07 22:35:45
tag:
summary: Understand how Simulink represents and categorizes sample times.
---
## Types of Sample Time

### Discrete Sample Time

Given a block with a discrete sample time, Simulink® executes the block output or update method at times, where the sample time period is always greater than zero and less than the simulation time . The number of `periods()` is an integer that must satisfy.

> 给定一个具有离散采样时间的块，Simulink® 在采样时间周期大于零且小于仿真时间的时间执行块输出或更新方法。**`periods()` 的数量必须满足整数**。

As simulation progresses, Simulink computes block outputs only once at each of these fixed time intervals of . These simulation times at which Simulink executes the output method of a block for a given sample time are called _sample time hits_. Discrete sample times are the only type for which sample time hits are known _a priori_.

> 随着仿真进行，Simulink 仅在这些固定的时间间隔.上计算块输出一次。 Simulink 为给定采样时间执行块的输出方法的仿真时间称为*采样时间命中*。离散采样时间是唯一可以预先知道采样时间命中的类型。

If you need to delay the initial sample hit time, you can define an offset, .

> 如果你需要延迟初始样本的击中时间，你可以定义一个偏移量。

The [**Unit Delay**](https://www.mathworks.com/help/simulink/slref/unitdelay.html) block is an example of a block with a discrete sample time.

> 单位延迟块是一个具有离散采样时间的块的示例。

### Continuous Sample Time

Continuous sample time hits are divided into major time steps and minor time steps. Minor time steps are subdivisions of the major time steps. The solver produces a result at each major time step. The solver uses results at the minor time steps to improve the accuracy of the result at the major time step.

> 连续采样时间命中分为**主时间步和次时间步**。次时间步是主时间步的细分。求解器在每个主时间步产生一个结果。求解器利用次时间步的结果来提高主时间步结果的准确性。

The ODE solver you choose integrates all continuous states from the simulation start time to a given major or minor time step. The solver determines the times of the minor steps and uses the results at the minor time steps to improve the accuracy of the results at the major time steps. You see the block output only at the major time steps.

> 选择的 ODE 求解器从仿真开始时间到给定的主要或次要时间步长积分所有连续状态。该求解器确定次要步长的时间，并使用次要时间步长的结果来提高主要时间步长的结果的准确性。您只能在主要时间步长处看到块输出。

To specify continuous sample time for a block, such as the Derivative block, for the **Sample time** parameter, enter `[0, 0]` or `0`.

> 为块（例如微分块）指定连续采样时间，可在**采样时间**参数中输入 **`[0, 0]` 或 `0`**。

### Inherited Sample Time

If a block sample time is set to `[–1 0]` or `–1`, the sample time is _inherited_, and Simulink determines the best sample time for the block based on the context of the block within the model. Simulink determines the sample time for blocks with inherited sample time during compilation. Because the inherited setting is overwritten in compilation, the Sample Time Legend never shows inherited sample time [`-1 0`] in a compiled model. For more information, see [View Sample Time Information](https://www.mathworks.com/help/simulink/ug/how-to-view-sample-time-information.html).

> 如果块样本时间设置为 `[-1 0]` 或 `-1`，则样本时间是继承的，Simulink 根据块在模型中的上下文确定最佳样本时间。Simulink 在编译期间确定具有继承样本时间的块的样本时间。由于**继承设置在编译期间被覆盖**，因此编译模型中的样本时间图例永远不会显示继承样本时间[-1 0]。有关详细信息，请参阅[查看样本时间信息]。

Some blocks inherit sample time by default. For these blocks, the parameter is not visible unless you specify a noninherited value. For example, the [Gain](https://www.mathworks.com/help/simulink/slref/gain.html) and [Rounding Function](https://www.mathworks.com/help/simulink/slref/roundingfunction.html) blocks do not have a visible sample time parameter and have inherited sample time by default. As a best practice, do not change the **Sample time** parameter for these blocks. For more information, see [Blocks for Which Sample Time Is Not Recommended](https://www.mathworks.com/help/simulink/ug/sampletimehiding.html).

> 一些模块默认继承采样时间。对于这些模块，除非您指定非继承的值，否则参数是不可见的。例如，[Gain]和[Rounding Function]模块没有可见的采样时间参数，而是默认继承采样时间。作为最佳实践，不要更改这些模块的**采样时间**参数。有关更多信息，请参阅[不建议使用采样时间的模块]。

All inherited blocks are subject to the process of sample time propagation. For more information, see [How Propagation Affects Inherited Sample Times](https://www.mathworks.com/help/simulink/ug/how-propagation-affects-inherited-sample-times.html).

> 所有继承的块都受到样本时间传播的过程的影响。有关更多信息，请参阅[传播如何影响继承的样本时间]。

### Fixed-in-Minor-Step

If the sample time of a block is [`0 1`], the block has _fixed-in-minor-step_ sample time. For this sample time, the block does not execute at the minor time steps. The block executes only at major time steps. Fixed-in-minor-step sample time eliminates unnecessary computations of blocks with outputs that cannot change between major steps.

> 如果一个块的采样时间为[`0 1`]，那么该块具有*固定在小步骤*的采样时间。对于这个采样时间，该块不会在小时间步骤中执行。块仅在主时间步骤中执行。固定在小步骤中的采样时间可以消除不必要的计算，其输出不能在主步骤之间改变。

While you can explicitly set a block to fixed-in-minor-step sample time, more often the software sets this condition as either an inherited sample time or as an alteration to a specification of continuous sample time. Fixed-in-minor-step sample time is equivalent to the fastest discrete rate in a system that uses a fixed-step solver. When you use a fixed-step solver, fixed-in-minor-step sample time is converted to the fastest discrete sample time.

> 当您可以显式设置一个块为固定小步骤采样时间时，软件通常将此条件设置为继承的采样时间或作为对连续采样时间规范的更改。固定小步骤采样时间等同于使用固定步长求解器的系统中最快的离散率。当您使用固定步长求解器时，固定小步骤采样时间将转换为最快的离散采样时间。

### Constant Sample Time

In Simulink, a _constant_ is a symbolic name or expression whose value you can change only outside the algorithm or through supervisory control. Blocks whose outputs do not change during normal execution of the model, such as the Constant block, are always considered to be constant.

> 在 Simulink 中，常量是一个符号名称或表达式，您只能在算法外部或通过监督控制改变它的值。输出在模型正常执行期间不会改变的块，如常量块，总是被认为是常量。

Simulink assigns constant sample time to these blocks. They run the block output method:

> Simulink 为这些块分配恒定采样时间。它们运行块输出方法：

- At the start of a simulation
- In response to runtime changes in the environment, such as tuning a parameter

For constant sample time, the block sample time assignment is `[inf 0]` or `inf`.

> 对于恒定采样时间，块采样时间分配为 `[inf 0]` 或 `inf`。

For a block to allow constant sample time, the block must not have continuous or discrete states and must not drive an output port of a conditionally executed subsystem. For more information, see [Using Enabled Subsystems](https://www.mathworks.com/help/simulink/ug/enabled-subsystems.html).

> 为了允许块使用恒定采样时间，该块不能具有连续或离散状态，也不能驱动条件执行子系统的输出端口。有关更多信息，请参阅[使用启用子系统]。

The Simulink block library includes several blocks whose ports can produce outputs at different sample rates, such as the MATLAB S-Function block, the Level-2 MATLAB S-Function block, and the C S-Function block. Some ports of these blocks can have a constant sample time.

> 模拟链接块库包括几个块，其端口可以以不同的采样率生成输出，如 MATLAB S-Function 块、Level-2 MATLAB S-Function 块和 C S-Function 块。这些块的某些端口可以具有恒定的采样时间。

### Variable Sample Time

Blocks that use a variable sample time have an implicit sample time parameter that the block specifies. The block tells the software when it executes. The compiled sample time is [`–2` ``_`Tvo`_``], where ``_`Tvo`_`` is a unique variable offset.

> 使用可变采样时间的块具有一个隐含的采样时间参数，由块指定。块告诉软件何时执行。编译后的采样时间为[`-2` ``_`Tvo`_``]，其中 ``_`Tvo`_`` 是唯一的可变偏移量。

The [Hit Scheduler](https://www.mathworks.com/help/simulink/slref/hitscheduler.html) block and the [Pulse Generator](https://www.mathworks.com/help/simulink/slref/pulsegenerator.html) block both have variable sample time. Variable sample time is supported only for variable-step solvers. The Hit Scheduler block is not supported for fixed-step solvers. When you use a fixed-step solver to simulate a model that contains a Pulse Generator block, the block specifies a discrete sample time.

> 该[Hit Scheduler] 块和[Pulse Generator] 块均具有可变采样时间。可变采样时间仅支持可变步长求解器。不支持固定步长求解器的 Hit Scheduler 块。当使用固定步长求解器仿真包含 Pulse Generator 块的模型时，该块指定离散采样时间。

To learn how to write your own block that uses a variable sample time, see [C MEX S-Function Examples](https://www.mathworks.com/help/simulink/sfg/c-mex-s-function-examples.html).

> 要学习如何编写使用变量采样时间的自定义块，请参阅 [C MEX S-Function Examples](https://www.mathworks.com/help/simulink/sfg/c-mex-s-function-examples.html)。

### Controllable Sample Time

You can configure a block to use a controllable sample time with a resolution _Tbase_. _Tbase_ is the smallest allowable time interval between block executions. To set _Tbase_ in your own C S-Function block, use the [`ssSetControllableSampleTime`](https://www.mathworks.com/help/simulink/sfg/sssetcontrollablesampletime.html) function.

> 您可以配置一个块以使用具有分辨率 *Tbase* 的可控采样时间。 *Tbase* 是块执行之间允许的最小时间间隔。要在自己的 C S-Function 块中设置 *Tbase*，请使用[`ssSetControllableSampleTime`](https://www.mathworks.com/help/simulink/sfg/sssetcontrollablesampletime.html)函数。

When a block uses controllable sample time, you can dynamically configure the block to execute at _n_ multiples of _Tbase_. The time of the next block execution is

> 当一个块使用可控采样时间时，您可以动态配置该块以 _n_ 个 _Tbase_ 的倍数执行。下一个块执行的时间是

_Tnext_ = _n_ _Tbase_ + _T_

You can set _n_ in your C S-Function block using the [`ssSetNumTicksToNextHitForControllableSampleTime`](https://www.mathworks.com/help/simulink/sfg/sssetnumtickstonexthitforcontrollablesampletime.html) function.

> 你可以使用[`ssSetNumTicksToNextHitForControllableSampleTime`](https://www.mathworks.com/help/simulink/sfg/sssetnumtickstonexthitforcontrollablesampletime.html) 函数在你的 C S-Function 块中设置 *n*。

### Triggered Sample Time

If a block is inside a triggered subsystem, such as a function-call- or enabled subsystem, the block may be constant or have a triggered sample time, except in the case of an asynchronous function call. You cannot specify the triggered sample time type explicitly. To achieve a triggered sample time during compilation, set the block sample time to inherited (`–1`). The software then determines the specific times at which the block executes during simulation.

> 如果一个块位于触发子系统，例如函数调用或启用子系统内，该块可以是恒定的或具有触发采样时间，除了在异步函数调用的情况下。您不能明确指定触发采样时间类型。要在编译期间实现触发采样时间，请将块采样时间设置为继承（`-1`）。然后，软件确定块在模拟期间执行的具体时间。

### Asynchronous Sample Time

An asynchronous sample time is similar to a triggered sample time. In both cases, you need to specify an inherited sample time because the Simulink engine does not regularly execute the block. Instead, a runtime condition determines when the block executes. For an asynchronous sample time, an S-function makes an asynchronous function call.

> 异步采样时间类似于触发采样时间。在这两种情况下，您都需要指定继承的采样时间，因为 Simulink 引擎不会定期执行该块。相反，运行时条件确定块何时执行。对于异步采样时间，S-函数进行异步函数调用。

The differences between these sample time types are:

- Only a function-call subsystem can have an asynchronous sample time. See [Using Function-Call Subsystems](https://www.mathworks.com/help/simulink/ug/using-function-call-subsystems.html).

> 只有函数调用子系统才能具有异步采样时间。参见[使用函数调用子系统](https://www.mathworks.com/help/simulink/ug/using-function-call-subsystems.html)。

- The source of the function-call signal is an S-function that has the option `SS_OPTION_ASYNCHRONOUS`.

> 来源于具有 `SS_OPTION_ASYNCHRONOUS` 选项的 S-函数的函数调用信号。

- The asynchronous sample time can also occur when a virtual block is connected to an asynchronous S-function or an asynchronous function-call subsystem.

> 异步采样时间也可能发生在将虚拟块连接到异步 S-函数或异步函数调用子系统时。

- The asynchronous sample time is important to certain code generation applications. See [Asynchronous Events](https://www.mathworks.com/help/rtw/ug/asynchronous-events.html) (Simulink Coder).

> 异步采样时间对某些代码生成应用非常重要。参见[异步事件](https://www.mathworks.com/help/rtw/ug/asynchronous-events.html)（Simulink Coder）。

- The sample time is .

For an explanation of how to use blocks to model and generate code for asynchronous event handling, see [Rate Transitions and Asynchronous Blocks](https://www.mathworks.com/help/rtw/ug/rate-transitions-and-asynchronous-blocks.html) (Simulink Coder).

> 对于如何使用块来模拟和生成用于异步事件处理的代码的解释，请参阅[速率转换和异步块](https://www.mathworks.com/help/rtw/ug/rate-transitions-and-asynchronous-blocks.html)（Simulink Coder）。

## Related Topics

- [What Is Sample Time?](https://www.mathworks.com/help/simulink/ug/what-is-sample-time.html)
- [View Sample Time Information](https://www.mathworks.com/help/simulink/ug/how-to-view-sample-time-information.html)
- [Specify Sample Time](https://www.mathworks.com/help/simulink/ug/how-to-specify-the-sample-time.html)
