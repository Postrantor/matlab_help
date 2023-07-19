---
tip: translate by openai@2023-07-07 21:00:18
url: https://www.mathworks.com/help/simulink/ug/managing-sample-times-in-systems.html
title: Sample Times in Systems - MATLAB & Simulink --- 系统中的采样时间 - MATLAB & Simulink
date: 2023-07-07 20:57:08
tag:
summary: How Simulink calculates the sample times of discrete and hybrid systems.
> 概要：Simulink如何计算离散和混合系统的采样时间。
---
## Sample Times in Systems

### Purely Discrete Systems

A purely discrete system is composed solely of discrete blocks and can be modeled using either a fixed-step or a variable-step solver. Simulating a discrete system requires that the simulator take a simulation step at every sample time hit. For a _multirate discrete system_—a system whose blocks Simulink® samples at different rates—the steps must occur at integer multiples of each of the system sample times. Otherwise, the simulator might miss key transitions in the states of the system. The step size that the Simulink software chooses depends on the type of solver you use to simulate the multirate system and on the fundamental sample time.

> 一个纯粹的离散系统由离散块组成，可以使用固定步长或可变步长求解器来建模。模拟离散系统需要模拟器在每个采样时刻取一个模拟步长。对于*多重率离散系统*——一个系统块在不同速率下由 Simulink® 采样的系统——步长必须以系统采样时间的整数倍发生。否则，模拟器可能会错过系统状态的关键转换。**Simulink 软件选择的步长取决于您用于模拟多重率系统的求解器类型以及基本采样时间**。

The _fundamental sample time_ of a multirate discrete system is the largest double that is an integer divisor of the actual sample times of the system. For example, suppose that a system has sample times of 0.25 and 0.50 seconds. The fundamental sample time in this case is 0.25 seconds. Suppose, instead, the sample times are 0.50 and 0.75 seconds. The fundamental sample time is again 0.25 seconds.

> 基本采样时间是多重率离散系统中最大的双整数，它能够被系统的实际采样时间整除。例如，假设一个系统的采样时间为 0.25 秒和 0.50 秒。在这种情况下，基本采样时间是 0.25 秒。假设采样时间是 0.50 秒和 0.75 秒，基本采样时间仍然是 0.25 秒。

The importance of the fundamental sample time directly relates to whether you direct the Simulink software to use a fixed-step or a variable-step discrete solver to solve your multirate discrete system. A fixed-step solver sets the simulation step size equal to the fundamental sample time of the discrete system. In contrast, a variable-step solver varies the step size to equal the distance between actual sample time hits.

> 基本采样时间的重要性直接关系到您是否指示 Simulink 软件使用固定步长还是变步长离散求解器来求解多率离散系统。固定步长求解器将仿真步长设置为离散系统的基本采样时间。相比之下，变步长求解器会根据实际采样时间命中之间的距离而变化步长。

The following diagram illustrates the difference between a fixed-step and a variable-step solver.

> 下图说明了固定步长和可变步长求解器之间的区别。

![](https://www.mathworks.com/help/simulink/ug/how_simulink_works3.gif)

In the diagram, the arrows indicate simulation steps and circles represent sample time hits. As the diagram illustrates, a variable-step solver requires fewer simulation steps to simulate a system, if the fundamental sample time is less than any of the actual sample times of the system being simulated. On the other hand, a fixed-step solver requires less memory to implement and is faster if one of the system sample times is fundamental. This can be an advantage in applications that entail generating code from a Simulink model (using Simulink Coder™). In either case, the discrete solver provided by Simulink is optimized for discrete systems; however, you can simulate a purely discrete system with any one of the solvers and obtain equivalent results.

> 在图中，箭头表示仿真步骤，圆圈表示样本时间命中。正如图所示，如果基本样本时间小于要仿真系统的任何实际样本时间，可变步长求解器需要较少的仿真步骤。另一方面，如果系统样本时间中有一个是基本的，则固定步长求解器实现起来需要更少的内存，而且速度更快。在使用 Simulink Coder™ 从 Simulink 模型生成代码的应用中，这可能是一个优势。无论哪种情况，Simulink 提供的离散求解器都是针对离散系统进行优化的；但是，您可以使用任何一种求解器来仿真纯离散系统，并获得相同的结果。

Consider the following example of a simple multirate system. For this example, the DTF1 Discrete Transfer Fcn block **Sample time** is set to `[1 0.1]` [], which gives it an offset of `0.1`. The **Sample time** of the DTF2 Discrete Transfer Fcn block is set to `0.7` , with no offset. The solver is set to a variable-step discrete solver.

> 考虑以下简单多重比率系统的示例。对于此示例，DTF1 离散传递函数块的**采样时间**设置为 `[1 0.1]`[]，这给它带来了 `0.1` 的偏移量。DTF2 离散传递函数块的**采样时间**设置为 `0.7`，没有偏移量。求解器设置为变步长离散求解器。

![](https://www.mathworks.com/help/simulink/ug/multiratemodel.png)

Running the simulation and plotting the outputs using the `stairs` function

> 运行模拟并使用 `stairs` 函数绘制输出

```matlab
set_param(bdroot,'SolverType','Variable-Step','SolverName','VariableStepDiscrete','SaveFormat','Array');
simOut = sim(bdroot,'Stoptime','3');
stairs(simOut.tout,simOut.yout,'-*','LineWidth',1.2);
xlabel('Time (t)');
ylabel('Outputs (out1,out2)');
legend('t_s = [1, 0.1]','t_s = 0.7','location','best')
```

produces the following plot.

![](https://www.mathworks.com/help/simulink/ug/how_simulink_works_14.png)

(For information on the `sim` command. see [Run Simulations Programmatically](https://www.mathworks.com/help/simulink/ug/using-the-sim-command.html). )

> 要了解关于“sim”命令的信息，请参阅[以编程方式运行仿真](https://www.mathworks.com/help/simulink/ug/using-the-sim-command.html)。

As the figure demonstrates, because the DTF1 block has a `0.1` offset, the DTF1 block has no output until `t = 0.1`. Similarly, the initial conditions of the transfer functions are zero; therefore, the output of DTF1, y(1), is zero before this time.

> 如图所示，由于 DTF1 块具有“0.1”偏移，因此在“t = 0.1”之前 DTF1 块没有输出。同样，传递函数的初始条件为零；因此，在此之前，DTF1 的输出 y（1）为零。

### Hybrid Systems

_Hybrid systems_ contain both discrete and continuous blocks and thus have both discrete and continuous states. However, Simulink solvers treat any system that has both continuous and discrete sample times as a hybrid system. For information on modeling hybrid systems, see [Modeling Hybrid Systems](https://www.mathworks.com/help/simulink/slref/simulink-concepts-models.html#f7-21139).

> 系统混合包含有离散和连续的模块，因此具有离散和连续的状态。然而，Simulink 求解器将具有连续和离散采样时间的任何系统都视为混合系统。有关建模混合系统的信息，请参阅[建模混合系统](https://www.mathworks.com/help/simulink/slref/simulink-concepts-models.html#f7-21139)。

In block diagrams, the term hybrid applies to both hybrid systems (mixed continuous-discrete systems) and systems with multiple sample times (multirate systems). Such systems turn yellow in color when you perform an with Sample Time Display turned '`on`'. As an example, consider the following model that contains an atomic subsystem, “Discrete Cruise Controller”, and a virtual subsystem, “Car Dynamics”.

> 在块图中，“混合”一词适用于混合系统（混合连续离散系统）和具有多个采样时间（多采样系统）的系统。当您执行具有样本时间显示转换为'on'时，这些系统会变成黄色。例如，考虑以下包含原子子系统“离散巡航控制器”和虚拟子系统“汽车动力学”的模型。

**Car Model**

![](https://www.mathworks.com/help/simulink/ug/car_system_orig.png)

With the **Sample Time** option set to , an turns the virtual subsystem yellow, indicating that it is a hybrid subsystem. In this case, the subsystem is a true hybrid system since it has both continuous and discrete sample times. As shown below, the discrete input signal, D1, combines with the continuous velocity signal, v, to produce a continuous input to the integrator.

> 当**采样时间**选项设置为时，虚拟子系统变为黄色，表明它是一个混合子系统。在这种情况下，子系统是一个真正的混合系统，因为它具有连续和离散采样时间。如下图所示，离散输入信号 D1 与连续速度信号 v 结合，产生连续输入到积分器。

**Car Model after an Update Diagram**

![](https://www.mathworks.com/help/simulink/ug/car_system_colors.png)

**Car Dynamics Subsystem after an Update Diagram**

![](https://www.mathworks.com/help/simulink/ug/car_hybrid_subsystem.png)

Now consider a multirate subsystem that contains three Sine Wave source blocks, each of which has a unique sample time — 0.2, 0.3, and 0.4, respectively.

> 现在考虑一个多率子系统，其中包含三个正弦波源块，每个块都具有独特的采样时间——分别为 0.2、0.3 和 0.4。

**Multirate Subsystem after an Update Diagram**

![](https://www.mathworks.com/help/simulink/ug/multirate_subsystem.png)

An turns the subsystem yellow because the subsystem contains more than one sample time. As shown in the block diagram, the Sine Wave blocks have discrete sample times D1, D2, and D3 and the output signal is fixed in minor step.

> 由于子系统包含多个采样时间，因此 An 将子系统变为黄色。如图所示，正弦波块具有离散采样时间 D1、D2 和 D3，输出信号以小步骤固定。

In assessing a system for multiple sample times, Simulink does not consider either constant [inf, 0] or asynchronous [–1, –n] sample times. Thus a subsystem consisting of one block that outputs constant value and one block with a discrete sample time will not be designated as hybrid.

> 在评估具有多个采样时间的系统时，Simulink 不考虑恒定[inf，0]或异步[-1，-n]采样时间。因此，由一个输出恒定值的块和一个具有离散采样时间的块组成的子系统不会被指定为混合系统。

The hybrid annotation and coloring are very useful for evaluating whether or not the subsystems in your model have inherited the correct or expected sample times.

> 融合注释和着色非常有用，可以用来评估您模型中的子系统是否继承了正确或预期的采样时间。

## Related Topics

- [Blocks for Which Sample Time Is Not Recommended](https://www.mathworks.com/help/simulink/ug/sampletimehiding.html)
- [View Sample Time Information](https://www.mathworks.com/help/simulink/ug/how-to-view-sample-time-information.html)
