---
tip: translate by openai@2023-07-10 09:12:06
...
---
url: [https://www.mathworks.com/help/simulink/slref/hitscheduler.html](https://www.mathworks.com/help/simulink/slref/hitscheduler.html)
title: Schedule major time steps for variable-step solver - Simulink --- 为可变步长求解器安排主要时间步 - Simulink
date: 2023-07-10 09:09:43
tag:

summary: Use the Hit Scheduler block to schedule major time steps for a variable-step solver during simulation......

> 使用 Hit 调度器块在仿真期间为变步长求解器安排主要时间步长。

---

Schedule major time steps for variable-step solver

_Since R2022b_

- ![](https://www.mathworks.com/help/simulink/slref/hit-scheduler-block-icon.png)

**Libraries:**
Simulink / Messages & Events

## Description

Use the Hit Scheduler block to schedule major time steps for a variable-step solver during simulation. With the Hit Scheduler block, you can implement dynamic solver hit event scheduling based on the behavior of your model during simulation.

> 使用 Hit Scheduler 块来调度变步长求解器的主要时间步骤。使用 Hit Scheduler 块，您可以根据模型的行为在仿真期间实现动态求解器 Hit 事件调度。

To schedule a major time step, you provide the block two inputs:

- **En** — The enable input controls when the block schedules a time step. The block schedules a time step when the input is logical `true`.

> 当块调度时间步时，启用输入控制。当输入为逻辑“true”时，块将调度时间步。

- **Δt** — The time interval input specifies when the scheduled time step occurs. The Hit Scheduler block calculates the time hit to schedule, in seconds, as the sum of the current simulation time and the time interval input.

> - **Δt** - 输入的时间间隔指定何时发生定时步骤。Hit Scheduler 块计算定时发生的时间，以秒为单位，是当前仿真时间和时间间隔输入的总和。

When the simulation reaches the scheduled time hit, the simulation takes a major time step and the block output updates. You can configure the block to produce either a signal output or a function-call output using the **Output type** parameter.

> 当模拟达到预定的时间命中时，模拟会进行一个大时间步，块输出将更新。您可以使用**输出类型**参数配置该块以产生信号输出或函数调用输出。

You can use the Hit Scheduler block to schedule multiple future time steps. The block stores the future time steps in a queue you can configure using the **Initial buffer size** and **Use fixed buffer size** parameters.

> 你可以使用 Hit 调度器块来安排多个未来的时间步骤。该块使用**初始缓冲区大小**和**使用固定缓冲区大小**参数将未来的时间步骤存储在队列中。

### Vector Signal Inputs and Outputs

When you configure the block to produce a signal output, you can use a single block to schedule time hits based on multiple signals in the model by using vector inputs. Each element in the **En** input vector indicates when the block should schedule a time hit based on the same element in the **Δt** vector. The block output signal has the same dimensions as the input signals. When the simulation takes a scheduled step, the element in the output vector that corresponds to the input vector element that scheduled the time step changes from logical `false` to logical `true`.

> 当您配置块以产生信号输出时，您可以使用单个块基于模型中的多个信号排定时间击中，通过使用向量输入。 **En** 输入向量中的每个元素表示块应根据 **Δt** 向量中的相同元素安排时间击中的时间。 块输出信号具有与输入信号相同的维度。 当模拟进行计划步骤时，与调度时间步骤的输入向量元素对应的输出向量元素从逻辑 `false` 变更为逻辑 `true`。

## Ports

### Input

expand all

### **En** — Enable signal for scheduling time step

scalar | vector

When the enable input value is logical `true`, the block schedules a simulation time step.

> 当使能输入值逻辑为真时，该块会安排一个仿真时间步骤。

When you set **Output type** to `Signal`, the input can be a scalar or a vector. When the **En** input is a vector, the **Δt** input must be a vector with the same dimensions.

> 当您将**输出类型**设置为 `信号` 时，输入可以是标量或向量。当 **En** 输入为向量时，**Δt** 输入必须是具有相同维度的向量。

When you set **Output type** to `Function call`, the input must be a scalar.

> 当您将**输出类型**设置为“函数调用”时，输入必须是标量。

**Data Types:** `Boolean`

### **Δt** — Interval between current simulation time and time step to schedule

scalar | vector

The block calculates the time step to schedule, in seconds, as the sum of the current simulation time and the current value of the time interval input.

> 块计算定时步骤，以秒为单位，其值为当前仿真时间加上当前时间间隔输入的值之和。

When you set **Output type** to `Signal`, the input can be a scalar or a vector. When the **Δt** input is a vector, the **En** input must be a vector with the same dimensions.

> 当你将输出类型设置为“信号”时，输入可以是标量或向量。当 Δt 输入是一个向量时，En 输入必须是具有相同维度的向量。

When you set **Output type** to `Function call`, the input must be a scalar.

> 当您将**输出类型**设置为“函数调用”时，输入必须是标量。

**Data Types:** `double`

### Output

expand all

### **Port_1** — Indication of scheduled time step

scalar | vector

The block output value indicates when the simulation takes a time step the block scheduled. You can configure the block to produce a signal or a function-call event using the **Output type** parameter. The value of the block output depends on the type of output and the dimensions of the input signal.

> 模拟块输出值指示模拟何时采取时间步长。您可以使用**输出类型**参数配置块以产生信号或函数调用事件。块输出的值取决于输出类型和输入信号的维度。

- When you set **Output type** to `Signal` and the input signal is a scalar, the block output becomes logical `true` when the simulation takes a scheduled time step.

> 当您将**输出类型**设置为 `信号` 并且输入信号为标量时，当仿真采取定期时间步时，块输出变为逻辑 `true`。

- When you set **Output type** to `Signal` and the input signal is a vector, the element in the output vector that corresponds to the input vector element that scheduled the time step changes from logical `false` to logical `true`.

> 当您将**输出类型**设置为“信号”且输入信号为一个向量时，与调度时间步骤相对应的输入向量元素的输出向量元素将从逻辑“假”变为逻辑“真”。

- When you set **Output type** to `Function-Call`, the input must be scalar, and the block generates a function-call event when the simulation takes a scheduled time step.

> 当你将输出类型设置为“函数调用”时，输入必须是标量，当模拟按计划时间步骤进行时，该块会生成一个函数调用事件。

Because the Hit Scheduler block explicitly schedules solver time hits that affect when the block executes, the output has variable sample time for both output types. For more information about variable sample time, see [Types of Sample Time](https://www.mathworks.com/help/simulink/ug/types-of-sample-time.html).

> 因为 Hit Scheduler 块明确地安排求解器时间命中，这会影响块的执行，因此两种输出类型的采样时间都是可变的。有关可变采样时间的更多信息，请参阅[采样时间类型](https://www.mathworks.com/help/simulink/ug/types-of-sample-time.html)。

**Data Types:** `Boolean`

## Parameters

expand all

### **Output type** — Type of block output

`Signal` (default) | `Function Call`

The block output indicates when the simulation takes a time step that the block scheduled. You can configure the block to produce a Boolean signal or a function-call event.

> 模拟块输出表明，当模拟采取模拟块计划的时间步骤时，可以配置该模拟块以产生布尔信号或函数调用事件。

- `Signal` — Block output becomes logical `true` when the simulation takes a scheduled time step. Scalar and vector input and output signals are supported.

> 信号——当模拟进行定时步骤时，块输出变为逻辑“真”。支持标量和矢量输入和输出信号。

- `Function Call` — Block generates a function-call event when the simulation takes a scheduled time step. Only scalar input signals are supported.

> 当模拟进行定时步骤时，函数调用块会生成函数调用事件。仅支持标量输入信号。

#### Programmatic Use

<table summary="Simple list"><tbody><tr><td><span><strong>Block Parameter:</strong> <code>HitSchedulerOutputType</code></span></td></tr><tr><td><span><strong>Type:</strong> string | character vector</span></td></tr><tr><td><span><strong>Values:</strong> <code>'Signal'</code> | <code>'Function-Call'</code></span></td></tr><tr><td><span><strong>Default:</strong> <code>'Signal'</code></span></td></tr></tbody></table>

### **Initial buffer size** — Initial size of buffer for scheduled time steps

`256` (default) | integer

Specify the size of the buffer that stores scheduled time steps as a positive, scalar integer value.

> 请指定存储计划时间步长的缓冲区的大小，以正的标量整数值表示。

By default, the buffer size is dynamic so that the software can increase the size if the buffer fills during simulation. When you select **Use fixed buffer size**, the **Initial buffer size** parameter value specifies the fixed size for the buffer. If the buffer overflows during simulation, the Hit Scheduler block overwrites scheduled time hits on a first-in, first-out basis.

> 默认情况下，缓冲区大小是动态的，以便软件可以在模拟期间增加大小，如果缓冲区满了。当您选择**使用固定缓冲区大小**时，**初始缓冲区大小**参数值指定了缓冲区的固定大小。如果缓冲区在模拟期间溢出，Hit Scheduler 块将按先进先出的方式覆盖安排的时间点击。

#### Programmatic Use

<table summary="Simple list"><tbody><tr><td><span><strong>Block Parameter:</strong> <code>InitialBufferSize</code></span></td></tr><tr><td><span><strong>Type:</strong> string | character vector</span></td></tr><tr><td><span><strong>Values:</strong> positive, whole, numeric scalar</span></td></tr><tr><td><span><strong>Default:</strong> <code>'256'</code></span></td></tr></tbody></table>

### **Use fixed buffer size** — Option to use fixed-size buffer

`off` (default) | `on`

When you select this option, the block uses a fixed buffer size, and the **Initial buffer size** parameter specifies the size of the buffer. If the buffer overflows during simulation, the Hit Scheduler block overwrites scheduled time hits on a first-in, first-out basis. By default, the buffer size is dynamic so that the software can adjust the size during simulation as needed.

> 当您选择此选项时，块使用固定的缓冲区大小，**初始缓冲区大小**参数指定缓冲区的大小。如果在仿真期间缓冲区溢出，Hit Scheduler 块将按先进先出的方式覆盖调度的时间击中。默认情况下，缓冲区大小是动态的，以便软件可以根据需要在仿真期间调整大小。

#### Programmatic Use

<table summary="Simple list"><tbody><tr><td><span><strong>Block Parameter:</strong> <code>FixedBuffer</code></span></td></tr><tr><td><span><strong>Type:</strong> string | character vector</span></td></tr><tr><td><span><strong>Values:</strong> <code>'on'</code> | <code>'off'</code></span></td></tr><tr><td><span><strong>Default:</strong> <code>'off'</code></span></td></tr></tbody></table>

## Block Characteristics

<table><colgroup><col width="28.70%"><col width="71.30%"></colgroup><tbody><tr><td><p><strong>Data Types</strong></p></td><td><p><code>Boolean</code> | <code>double</code></p></td></tr><tr><td><p><strong>Direct Feedthrough</strong></p></td><td><p><code>yes</code></p></td></tr><tr><td><p><strong>Multidimensional Signals</strong></p></td><td><p><code>no</code></p></td></tr><tr><td><p><strong>Variable-Size Signals</strong></p></td><td><p><code>no</code></p></td></tr><tr><td><p><strong>Zero-Crossing Detection</strong></p></td><td><p><code>no</code></p></td></tr></tbody></table>

## Version History

**Introduced in R2022b**

What is one thing we can do to improve this information or the software described on this page?

> 我们可以做什么来改进这个页面上描述的信息或软件？
