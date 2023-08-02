---
url: https://www.mathworks.com/help/rtw/ug/modeling-for-single-tasking-execution.html
title: Modeling for Single-Tasking Execution - MATLAB & Simulink --- 单任务执行建模 - MATLAB & Simulink
date: 2023-07-21 10:26:58
tag: 
summary: Use the base sample rate of a model to define the time interval during which blocks in the model exec......
---
## Modeling for Single-Tasking Execution

### Single-Tasking Mode

You can execute model code in a strictly single-tasking manner. While this mode is less efficient with regard to execution speed, in certain situations, it can simplify your model.

In single-tasking mode, the base sample rate must define a time interval that is long enough to allow the execution of all blocks within that interval.

The next figure illustrates the inefficiency inherent in single-tasking execution.

![](https://www.mathworks.com/help/rtw/ug/single_tasking_system_exe.png)

Single-tasking system execution requires a base sample rate that is long enough to execute one step through the entire model.

### Build a Program for Single-Tasking Execution

To use single-tasking execution, clear model configuration parameter **Treat each discrete rate as a separate task**. If you select the parameter, single-tasking mode is used in the following cases:

* If your model contains one sample time
* If your model contains a continuous and a discrete sample time and the fixed step size is equal to the discrete sample time

### Single-Tasking Execution

This example examines how a simple multirate model executes in both real time and simulation, using a fixed-step solver. It considers operation in both single-tasking and multitasking modes, as determined by setting model configuration parameter **Treat each discrete rate as a separate task**.

The example model is shown in the next figure. The discussion refers to the six blocks of the model as A through F, as labeled in the block diagram.

The execution order of the blocks (indicated in the upper right of each block) has been forced into the order shown by assigning higher priorities to blocks F, E, and D. The ordering shown is one possible valid execution ordering for this model. For more information, see [Simulation Phases in Dynamic Systems](https://www.mathworks.com/help/simulink/ug/simulating-dynamic-systems.html).

The execution order is determined by data dependencies between blocks. In a real-time system, the execution order determines the order in which blocks execute within a given time interval or task. This discussion treats the model's execution order as a given, because it is concerned with the allocation of block computations to tasks, and the scheduling of task execution.

![](https://www.mathworks.com/help/rtw/ug/multi10.png)

**Note**

The discussion and timing diagrams in this section are based on the assumption that the Rate Transition blocks are used in the default (protected) mode, with block parameters **Ensure data integrity during data transfer** and **Ensure deterministic data transfer (maximum delay)** selected

This example considers the execution of the above model when model configuration parameter **Treat each discrete rate as a separate task** is cleared, which indicates the single-tasking mode.

In a single-tasking system, if you select model configuration parameter **Block reduction**, fast-to-slow Rate Transition blocks are optimized out of the model. The default case is shown (parameter **Block reduction** selected), so block B does not appear in the timing diagrams in this section. For more information, see [Block reduction](https://www.mathworks.com/help/simulink/gui/block-reduction.html).

The following table shows, for each block in the model, the execution order, sample time, and whether the block has an output or update computation. Block A does not have discrete states, and accordingly does not have an update computation.

**Execution Order and Sample Times (Single-Tasking)**

<table summary="Execution Order and Sample Times (Single-Tasking)"><colgroup><col width="25%"><col width="25%"><col width="25%"><col width="25%"></colgroup><thead><tr><th><p><strong>Blocks<br>(in Execution Order)</strong></p></th><th><p><strong>Sample Time<br>(in Seconds)</strong></p></th><th><p><strong>Output</strong></p></th><th><p><strong>Update</strong></p></th></tr></thead><tbody><tr><td><p>E</p></td><td><p>0.1</p></td><td><p>Y</p></td><td><p>Y</p></td></tr><tr><td><p>F</p></td><td><p>0.1</p></td><td><p>Y</p></td><td><p>Y</p></td></tr><tr><td><p>D</p></td><td><p>1</p></td><td><p>Y</p></td><td><p>Y</p></td></tr><tr><td><p>A</p></td><td><p>0.1</p></td><td><p>Y</p></td><td><p>N</p></td></tr><tr><td><p>C</p></td><td><p>1</p></td><td><p>Y</p></td><td><p>Y</p></td></tr></tbody></table>

#### Real-Time Single-Tasking Execution

The next figure shows the scheduling of computations when the generated code is deployed in a real-time system. The generated program is shown running in real time, under control of interrupts from a 10 Hz timer.

![](https://www.mathworks.com/help/rtw/ug/real_time_system_single_tasking_exe_of_model.png)

At time 0.0, 1.0, and every second thereafter, both the slow and fast blocks execute their output computations; this is followed by update computations for blocks that have states. Within a given time interval, output and update computations are sequenced in block execution order.

The fast blocks execute on every tick, at intervals of 0.1 second. Output computations are followed by update computations.

The system spends some portion of each time interval (labeled “wait”) idling. During the intervals when only the fast blocks execute, a larger portion of the interval is spent idling. This illustrates an inherent inefficiency of single-tasking mode.

#### Simulated Single-Tasking Execution

The next figure shows the execution of the model during the Simulink® simulation loop.

![](https://www.mathworks.com/help/rtw/ug/simulated_single_tasking_exe_of_model.png)

Because time is simulated, the placement of ticks represents the iterations of the simulation loop. Blocks execute in exactly the same order as in the previous figure, but without the constraint of a real-time clock. Therefore there is no idle time between simulated sample periods.

## Related Topics

* [Time-Based Scheduling and Code Generation](https://www.mathworks.com/help/rtw/ug/time-based-scheduling-and-code-generation.html)
* [Configure Time-Based Scheduling](https://www.mathworks.com/help/rtw/ug/configure-time-based-scheduling.html)
* [Time-Based Scheduling Example Models](https://www.mathworks.com/help/rtw/ug/time-based-scheduling-example-models-1.html)
