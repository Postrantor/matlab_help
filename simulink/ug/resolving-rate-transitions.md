---
tip: translate by openai@2023-07-07 21:46:16
url: https://www.mathworks.com/help/simulink/ug/resolving-rate-transitions.html
title: Resolve Rate Transitions - MATLAB & Simulink --- 解析速率转变 - MATLAB & Simulink
date: 2023-07-07 21:44:51
tag:
summary: How Simulink resolves rate transitions between blocks caused by different block sample times.
> Simulink如何解决由不同块采样时间引起的块之间的速率转换。
---
## Resolve Rate Transitions

In general, a rate transition exists between two blocks if their sample times differ, that is, if either of their sample-time vector components are different. The exceptions are:

> 一般来说，如果两个块的采样时间不同，即它们的采样时间向量组件有所不同，就存在速率转换。例外情况是：

- Blocks that output constant value never have a rate transition with any other rate.
- A continuous sample time (black) and the fastest discrete rate (red) never has a rate transition if you use a fixed-step solver.

> 连续采样时间（黑色）和最快的离散率（红色）如果使用固定步长求解器，永远不会有率转换。

- A variable sample time and fixed in minor step do not have a rate transition.

You can resolve rate transitions manually by inserting rate transition blocks and by using two diagnostic tools. For the single-tasking execution mode, the diagnostic allows you to set the level of Simulink® rate transition messages. The diagnostic serves the same function for multitasking execution mode. These execution modes directly relate to the type of solver in use: Variable-step solvers are always single-tasking; fixed-step solvers may be explicitly set as single-tasking or multitasking.

> 你可以通过插入速率转换块和使用两个诊断工具来手动解决速率转换。对于单任务执行模式，诊断允许您设置 Simulink® 速率转换消息的级别。诊断对多任务执行模式也具有相同的功能。这些执行模式直接关系到使用的求解器类型：可变步长求解器总是单任务；可以明确设置为单任务或多任务的固定步长求解器。

### Automatic Rate Transition

Simulink can detect mismatched rate transitions in a multitasking model during an update diagram and automatically insert Rate Transition blocks to handle them. To enable this, in the **Solver** pane of model configuration parameters, select **Automatically handle rate transition for data transfer**. The default setting for this option is off. When you select this option:

> Simulink 可以在更新图表期间检测多任务模型中的不匹配速率转换，并自动插入速率转换块来处理它们。要启用此功能，请在模型配置参数的**求解器**窗格中选择**自动处理数据传输的速率转换**。此选项的默认设置为关闭。当您选择此选项时：

- Simulink handles transitions between periodic sample times and asynchronous tasks.
- Simulink inserts hidden Rate Transition blocks in the block diagram.
- Automatically inserted Rate Transition blocks operate in protected mode for periodic tasks and asynchronous tasks. You cannot alter this behavior. For periodic tasks, automatically inserted Rate Transition blocks operate with the level of determinism specified by the **Deterministic data transfer** parameter in the **Solver** pane. The default setting is `Whenever possible`, which enables determinism for data transfers between periodic sample-times that are related by an integer multiple. For more information, see [Deterministic data transfer](https://www.mathworks.com/help/simulink/gui/deterministicdatatransfer.html). To use other modes, you must insert Rate Transition blocks and set their modes manually.

> 自动插入的速率转换块以保护模式运行周期任务和异步任务。您无法更改此行为。对于周期性任务，自动插入的速率转换块按照“求解器”窗格中的**确定性数据传输**参数指定的级别运行。默认设置为“可能的情况下”，这使得周期采样时间之间的数据传输具有确定性。有关详细信息，请参阅[确定性数据传输](https://www.mathworks.com/help/simulink/gui/deterministicdatatransfer.html)。要使用其他模式，必须手动插入速率转换块并设置它们的模式。

### Visualize Inserted Rate Transition Blocks

When you select the **Automatically handle rate transition for data transfer** option, Simulink inserts Rate Transition blocks in the paths that have mismatched transition rates. These blocks are hidden by default. To visualize the inserted blocks, update the diagram. Badge labels appear in the model and indicate where Simulink inserted Rate Transition blocks during the compilation phase. For example, in this model, three Rate Transition blocks were inserted between the two Sine Wave blocks and the Multiplexer and Integrator when the model compiled. The ZOH and DbBuf badge labels indicate these blocks.

> 当您选择**自动处理数据传输的速率转换**选项时，Simulink 会在具有不匹配转换率的路径中插入速率转换块。这些块默认情况下是隐藏的。要可视化插入的块，请更新图表。徽章标签出现在模型中，并指示 Simulink 在编译阶段插入了哪些速率转换块。例如，在这个模型中，在模型编译时，在两个正弦波块和多路复用器和积分器之间插入了三个速率转换块。 ZOH 和 DbBuf 徽章标签指示这些块。

![](https://www.mathworks.com/help/simulink/ug/visualize_rtb_badge_label.png)

You can show or hide badge labels. On the **Debug** tab, select > .

To configure the hidden Rate Transition blocks, right click on a badge label and click on **Insert rate transition block** to make the block visible.

> 要配置隐藏的速率转换块，请右键单击徽章标签，然后单击**插入速率转换块**以使块可见。

![](https://www.mathworks.com/help/simulink/ug/visualize_rtb.png)

When you make hidden Rate Transition blocks visible:

- You can see the type of Rate Transition block inserted as well as the location in the model.
- You can set the **Initial Conditions** of these blocks.
- You can change data transfer and sample time block parameters.

Validate the changes to your model by updating your diagram.

![](https://www.mathworks.com/help/simulink/ug/visualize_rtb_final.png)

Displaying inserted Rate Transition blocks is not compatible with export-function models.

> 显示插入的费率转换块与出口功能模型不兼容。

To learn more about the types of Rate Transition blocks, see [Rate Transition](https://www.mathworks.com/help/simulink/slref/ratetransition.html).

> 要了解有关速率转换块类型的更多信息，请参阅[速率转换](https://www.mathworks.com/help/simulink/slref/ratetransition.html)。

**Note**

Suppose you automatically insert rate transition blocks and there is a virtual block specifying sample time upstream of the block you insert. You cannot click the badge of the inserted block to configure the block and make it visible because the sample time on the virtual block causes a rate transition as well. In this case, manually insert a rate transition block before the virtual block. To learn more about virtual blocks, see [Nonvirtual and Virtual Blocks](https://www.mathworks.com/help/simulink/ug/nonvirtual-and-virtual-blocks.html).

> 假设您自动插入速率转换块，并且在您插入的块上游有一个虚拟块指定样本时间。您无法单击插入的块的徽章以配置该块并使其可见，因为虚拟块上的采样时间也会导致速率转换。在这种情况下，在虚拟块之前手动插入一个速率转换块。要了解有关虚拟块的更多信息，请参阅[非虚拟和虚拟块](https://www.mathworks.com/help/simulink/ug/nonvirtual-and-virtual-blocks.html)。

## Related Examples

- [Handle Rate Transitions](https://www.mathworks.com/help/rtw/ug/handle-rate-transitions.html) (Simulink Coder)

## More About

- [Time-Based Scheduling and Code Generation](https://www.mathworks.com/help/rtw/ug/time-based-scheduling-and-code-generation.html) (Simulink Coder)
