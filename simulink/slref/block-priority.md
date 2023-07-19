---
tip: translate by openai@2023-07-10 09:38:55
url: https://www.mathworks.com/help/simulink/slref/block-priority.html
title: Block Priority - MATLAB & Simulink --- 块优先级 - MATLAB & Simulink
date: 2023-07-10 09:37:13
tag:
summary: This model shows what happens when blocks are assigned different priorities.
> 总结：这个模型显示了当块被分配不同的优先级时会发生什么。
---
This model shows what happens when blocks are assigned different priorities. The block priority affects the order in which the blocks are executed. You can set the block priority through the Block Properties dialog. A smaller number indicates a higher priority.

> 该模型显示了当块被分配不同的优先级时会发生什么。块优先级影响块的执行顺序。可以通过“块特性”对话框设置块优先级。数字越小表示优先级越高。

The block sorting order is calculated only at the beginning of a simulation. For this reason, changes to a block's Priority property are only updated when the simulation is started. Double click on the green block to change the priority levels for the blocks and see what happens.

> 块排序顺序仅在模拟开始时计算。因此，对块的“优先级”特性所做的更改只有在模拟启动时才会更新。双击绿色区块以更改区块的优先级，然后查看会发生什么。

Inside of each triggered subsystem is an S-Function which controls the coloring of the blocks and also slows down the simulation. The priority level of the block is displayed using the format attribute string that can be set in the Block Properties dialog.

> 每个被触发的子系统内部都有一个 S 函数，它控制块的着色，也减慢模拟速度。块的优先级使用可以在“块特性”对话框中设置的格式属性字符串显示。

![](https://www.mathworks.com/help/examples/simulink_features/win64/BlockPriorityExample_01.png)

## See Also

[Specify Block Execution Priority and Tag](https://www.mathworks.com/help/simulink/ug/block-properties-dialog-box.html#bvcigcz) | [Control and Display Execution Order](https://www.mathworks.com/help/simulink/ug/controlling-and-displaying-the-sorted-order.html)

> [指定块执行优先级和标记](https://www.mathworks.com/help/simulink/ug/block-properties-dialog-box.html#bvcigcz) | [控制和显示执行命令](https://www.mathworks.com/help/simulink/ug/controlling-and-displaying-the-sorted-order.html)

You have a modified version of this example. Do you want to open this example with your edits?

> 您有此示例的修改版本。是否要使用编辑打开此示例？
