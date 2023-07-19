---
tip: translate by openai@2023-07-07 22:05:55
url: https://www.mathworks.com/help/rtw/ug/time-based-scheduling-and-code-generation.html
title: Time-Based Scheduling and Code Generation - MATLAB & Simulink --- 基于时间的调度和代码生成 - MATLAB & Simulink
date: 2023-07-07 22:04:04
tag:
summary: Generate code that meets real-time execution requirements after reviewing sample time and tasking mod......
> 总结：在检查样本时间和任务模型后，生成符合实时执行要求的代码。
---
## Time-Based Scheduling and Code Generation

### Sample Time Considerations

Simulink® models run at one or more sample times. The Simulink product provides considerable flexibility in building multirate systems, that is, systems with more than one sample time. However, this same flexibility also allows you to construct models for which the code generator cannot generate real-time code for execution in a multitasking environment. To make multirate models operate as expected in real time (that is, to give the right answers), you sometimes must modify your model or instruct the Simulink engine to modify the model for you. In general, the modifications involve placing Rate Transition blocks between blocks that have unequal sample times. The following sections discuss issues you must address to use a multirate model in a multitasking environment. For a comprehensive discussion of sample times, including rate transitions, see [What Is Sample Time?](https://www.mathworks.com/help/simulink/ug/what-is-sample-time.html), [Sample Times in Subsystems](https://www.mathworks.com/help/simulink/ug/managing-sample-times-in-subsystems.html), [Sample Times in Systems](https://www.mathworks.com/help/simulink/ug/managing-sample-times-in-systems.html), [Resolve Rate Transitions](https://www.mathworks.com/help/simulink/ug/resolving-rate-transitions.html), and associated topics.

> Simulink® 模型可以以一个或多个采样时间运行。Simulink 产品在构建多采样系统（即，具有多个采样时间的系统）方面提供了相当大的灵活性。但是，这种同样的灵活性也允许您构建代码生成器无法为多任务环境中的执行生成实时代码的模型。为了使多采样模型在实时中正常运行（即，给出正确的答案），您有时必须修改模型或指示 Simulink 引擎为您修改模型。通常，修改涉及在具有不等采样时间的块之间放置速率转换块。以下部分讨论您必须解决的问题，以在多任务环境中使用多采样模型。有关采样时间的全面讨论，包括速率转换，请参阅[什么是采样时间？]，[子系统中的采样时间]，[系统中的采样时间]，[解决速率转换]以及相关主题。

### Tasking Modes

There are two execution modes for a fixed-step model: single-tasking and multitasking. These modes are available only for fixed-step solvers. To select an execution mode, select model configuration parameter **Treat each discrete rate as a separate task**. When you select this parameter, multitasking execution is applied for a multirate model. When you clear this parameter, single-tasking execution is applied.

> 固定步长模型有两种执行模式：单任务和多任务。这些模式仅适用于固定步长求解器。要选择执行模式，请选择模型配置参数**将每个离散率单独处理**。选择此参数后，多步长模型将采用多任务执行。清除此参数时，将应用单任务执行。

**Note**

A model that is multirate and uses multitasking cannot reference a multirate model that uses single-tasking.

> 一个多率模型和使用多任务的模型不能引用一个使用单任务的多率模型。

Execution of models in a real-time system can be done with the aid of a real-time operating system, or it can be done on _bare-metal_ target hardware, where the model runs in the context of an interrupt service routine (ISR).

> 模型在实时系统中的执行可以通过实时操作系统来完成，也可以在裸机目标硬件上完成，在这种情况下，模型运行在中断服务例程（ISR）的上下文中。

The fact that a system (such as The Open Group UNIX® or Microsoft® Windows® systems) is multitasking does not imply that your program can execute in real time. This is because the program might not preempt other processes when required.

> 事实上，一个系统（比如 The Open Group UNIX® 或者 Microsoft® Windows® 系统）是多任务的，并不意味着你的程序可以实时执行。这是因为程序可能不会在必要时预占其他进程。

In operating systems (such as PC-DOS) where only one process can exist at a given time, an interrupt service routine (ISR) must perform the steps of saving the processor context, executing the model code, collecting data, and restoring the processor context.

> 在只能同时存在一个进程的操作系统（如 PC-DOS）中，中断服务程序（ISR）必须执行保存处理器上下文、执行模型代码、收集数据和恢复处理器上下文的步骤。

Other operating systems, such as POSIX-compliant ones, provide automatic context switching and task scheduling. This simplifies the operations performed by the ISR. In this case, the ISR simply enables the model execution task, which is normally blocked. The next figure illustrates this difference.

> 其他操作系统，例如符合 POSIX 标准的系统，提供自动上下文切换和任务调度。这简化了中断服务程序（ISR）所执行的操作。在这种情况下，ISR 只需启用通常处于阻塞状态的模型执行任务。下图说明了这种区别。

![](https://www.mathworks.com/help/rtw/ug/real_time_program_exe.png)

### Model Execution and Rate Transitions

To generate code that executes as expected in real time, you (or the Simulink engine) might need to identify and handle sample rate transitions within the model. In multitasking mode, by default the Simulink engine flags errors during simulation if the model contains invalid rate transitions. You can use the model configuration parameter **Multitask data transfer** to alter this behavior. Parameter **Single task data transfer** is available for the same purpose for single-tasking mode.

> 要生成在实时中执行预期的代码，您（或 Simulink 引擎）可能需要在模型中识别和处理采样率转换。在多任务模式下，默认情况下，如果模型包含无效的速率转换，Simulink 引擎在仿真过程中会标记错误。您可以使用模型配置参数**多任务数据传输**来改变这种行为。**单任务数据传输**参数也可用于单任务模式。

To avoid raising rate transition errors, insert Rate Transition blocks between tasks. You can request that the Simulink engine handle rate transitions automatically by inserting hidden Rate Transition blocks. See [Automatic Rate Transition](https://www.mathworks.com/help/rtw/ug/handle-rate-transitions.html#f1022163) for an explanation of this option.

> 为了避免出现率转换错误，在任务之间插入率转换块。您可以要求 Simulink 引擎自动处理率转换，方法是插入隐藏的率转换块。有关此选项的详细信息，请参阅[自动率转换]。

To understand such problems, first consider how Simulink simulations differ from real-time programs.

> 要理解这些问题，首先要考虑 Simulink 模拟与实时程序有何不同。

### Execution During Simulink Model Simulation

Before the Simulink engine simulates a model, it orders the blocks based upon their topological dependencies. This includes expanding virtual subsystems into the individual blocks they contain and flattening the entire model into a single list. Once this step is complete, each block is executed in order.

> 在 Simulink 引擎模拟模型之前，它根据拓扑依赖性对块进行排序。这包括将虚拟子系统扩展为其包含的个别块，并将整个模型平坦化为单个列表。一旦这一步骤完成，每个块都按顺序执行。

The key to this process is the ordering of blocks. A block whose output is directly dependent on its input (that is, a block with direct feedthrough) cannot execute until the block driving its input executes.

> 这个过程的关键是块的排序。一个输出直接取决于其输入（即具有直接穿透的块），在驱动其输入的块执行之前不能执行。

Some blocks set their outputs based on values acquired in a previous time step or from initial conditions specified as a block parameter. The output of such a block is determined by a value stored in memory, which can be updated independently of its input. During simulation, computations are performed prior to advancing the variable corresponding to time. This results in computations occurring instantaneously (that is, no computational delay).

> 一些块根据上一时间步骤中获得的值或作为块参数指定的初始条件来设置其输出。这样的块的输出由存储在内存中的值确定，该值可以独立于其输入更新。在模拟期间，在推进与时间相关的变量之前执行计算。这导致计算立即发生（即没有计算延迟）。

### Model Execution in Real Time

A real-time program differs from a Simulink simulation in that the program must execute the model code synchronously with real time. Every calculation results in some computational delay. This means the sample intervals cannot be shortened or lengthened (as they can be in a Simulink simulation), which leads to less efficient execution.

> 一个实时程序与 Simulink 模拟的不同之处在于，程序必须与实时同步执行模型代码。每次计算都会导致一些计算延迟。这意味着样本间隔不能缩短或延长（就像在 Simulink 模拟中一样），这导致执行效率较低。

Consider the following timing figure.

![](https://www.mathworks.com/help/rtw/ug/unused_time_in_sample_interval.png)

Note the processing inefficiency in the sample interval `t1`. That interval cannot be compressed to increase execution speed because, by definition, sample times are clocked in real time.

> 注意样本间隔 `t1` 中的处理效率不高。由于样本时间是以实时计时的，因此无法将该间隔压缩以提高执行速度。

You can circumvent this potential inefficiency by using the multitasking mode. The multitasking mode defines tasks with different priorities to execute parts of the model code that have different sample rates.

> 你可以通过使用多任务模式来避免这种潜在的低效率。多任务模式定义了具有不同优先级的任务，以执行具有不同采样率的模型代码的部分。

See [Multitasking and Pseudomultitasking Modes](https://www.mathworks.com/help/rtw/ug/modeling-for-multitasking-execution.html#f997668) for a description of how this works. It is important to understand that section before proceeding here.

> 请参阅[多任务和伪多任务模式]，了解其工作原理。在继续此处之前，重要的是要理解该部分。

### Single-Tasking Versus Multitasking Operation

Single-tasking programs require longer sample intervals, because all computations must be executed within each clock period. This can result in inefficient use of available CPU time, as shown in the previous figure.

> 单任务程序需要更长的采样间隔，因为所有计算必须在每个时钟周期内完成。这可能会导致可用 CPU 时间的不高效利用，如前面图所示。

Multitasking mode can improve the efficiency of your program if the model is large and has many blocks executing at each rate.

> 模型较大且每个速率有许多块正在执行时，多任务模式可以提高程序的效率。

However, if your model is dominated by a single rate, and only a few blocks execute at a slower rate, multitasking can actually degrade performance. In such a model, the overhead incurred in task switching can be greater than the time required to execute the slower blocks. In this case, it is more efficient to execute all blocks at the dominant rate.

> 然而，如果您的模型由单个速率主导，且只有少数块以较慢的速率执行，多任务实际上可能会降低性能。在这种模型中，任务切换所产生的开销可能大于执行较慢块所需的时间。在这种情况下，以主导速率执行所有块更有效率。

If you have a model that can benefit from multitasking execution, you might need to modify your model by adding Rate Transition blocks (or instruct the Simulink engine to do so) to generate expected results.

> 如果您有一个可以从多任务执行中受益的模型，您可能需要通过添加速率转换块（或指示 Simulink 引擎这样做）来修改模型，以生成预期的结果。

For more information about the two modes of execution and examples, see [Modeling for Single-Tasking Execution](https://www.mathworks.com/help/rtw/ug/modeling-for-single-tasking-execution.html) and [Modeling for Multitasking Execution](https://www.mathworks.com/help/rtw/ug/modeling-for-multitasking-execution.html).

> 要了解有关两种执行模式及示例的更多信息，请参阅[单任务执行建模](https://www.mathworks.com/help/rtw/ug/modeling-for-single-tasking-execution.html)和[多任务执行建模](https://www.mathworks.com/help/rtw/ug/modeling-for-multitasking-execution.html)。

## Related Topics

- [What Is Sample Time?](https://www.mathworks.com/help/simulink/ug/what-is-sample-time.html)
- [Sample Times in Subsystems](https://www.mathworks.com/help/simulink/ug/managing-sample-times-in-subsystems.html)
- [Sample Times in Systems](https://www.mathworks.com/help/simulink/ug/managing-sample-times-in-systems.html)
- [Configure Time-Based Scheduling](https://www.mathworks.com/help/rtw/ug/configure-time-based-scheduling.html)
- [Sample Times in Subsystems](https://www.mathworks.com/help/simulink/ug/managing-sample-times-in-subsystems.html)
- [Sample Times in Systems](https://www.mathworks.com/help/simulink/ug/managing-sample-times-in-systems.html)
- [Resolve Rate Transitions](https://www.mathworks.com/help/simulink/ug/resolving-rate-transitions.html)
- [Handle Rate Transitions](https://www.mathworks.com/help/rtw/ug/handle-rate-transitions.html)
- [Modeling for Single-Tasking Execution](https://www.mathworks.com/help/rtw/ug/modeling-for-single-tasking-execution.html)
- [Modeling for Multitasking Execution](https://www.mathworks.com/help/rtw/ug/modeling-for-multitasking-execution.html)
- [Time-Based Scheduling Example Models](https://www.mathworks.com/help/rtw/ug/time-based-scheduling-example-models-1.html)
