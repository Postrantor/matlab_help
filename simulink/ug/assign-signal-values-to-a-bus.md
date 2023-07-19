---
tip: translate by openai@2023-07-07 11:31:11
url: https://ww2.mathworks.cn/help/simulink/ug/assign-signal-values-to-a-bus.html
title: Assign Signal Values to Bus Elements - MATLAB & Simulink - MathWorks 中国 --- 将信号值分配给总线元素 - MATLAB & Simulink - MathWorks 中国
date: 2023-07-07 11:27:40
tag:
summary: Use a Bus Assignment block to replace signals assigned to bus elements.
---
This example shows how to use a Bus Assignment block to replace the value of a bus element. You do not need to add Bus Selector and Bus Creator blocks to change the value of a bus element.

> 这个例子展示了如何使用总线分配块来替换总线元素的值。您不需要添加总线选择器和总线创建器块来改变总线元素的值。

Open and simulate the example model.

![](https://ww2.mathworks.cn/help/examples/simulink/win64/AssignSignalValuesToABusExample_01.png)

The Bus Assignment block has two input ports. The first input port receives the bus that contains an element to which you want to assign a new value. The bus can be virtual or nonvirtual. The second input port receives the signal whose value you want to assign to the bus element.

> 车辆分配块有两个输入端口。第一个输入端口接收包含要分配新值的元素的总线。总线可以是虚拟的或非虚拟的。第二个输入端口接收要分配给总线元素的信号的值。

Double-click the Bus Assignment block to open a dialog box with assignment options. The Block Parameters dialog box lists the elements available for assignment in the **Elements in the bus** list.

> 双击“总线分配”块以打开具有分配选项的对话框。“块参数”对话框列出了**总线中的元素**列表中可用于分配的元素。

![](https://ww2.mathworks.cn/help/examples/simulink/win64/xxBusAssignmentBlockParameters.png)

In this model, bus elements `a` and `b` are available for assignment. Element `a` also appears in the **Elements that are being assigned** list, which indicates that it is selected for assignment.

> 在此模型中，总线元素 `a` 和 `b` 可供分配。元素 `a` 也出现在“被分配元素”列表中，这表明它已被选择用于分配。

To assign a new value to the bus element, connect the signal that provides the new value to the corresponding port on the Bus Assignment block. The elements to which you assign values can be nonbus signals or buses. The new values must match the attributes of the elements in the original bus.

> 为总线元素分配新值，请将提供新值的信号连接到总线分配块的相应端口。您可以为非总线信号或总线分配值。新值必须与原始总线中的元素属性匹配。

In this model, signal `c` connects to the port that assigns a new value to element `a`. The port label indicates the bus element. For element `a`, the port label is **:=a**.

> 在这个模型中，信号 `c` 连接到给元素 `a` 分配新值的端口。端口标签表示总线元素。对于元素 `a`，端口标签是**:=a**。

The Bus Assignment block replaces the value of bus element `a`, which is 1, with the value of signal `c`, which is 3.

> 块"Bus Assignment"将总线元素'a'的值 1 替换为信号'c'的值 3。

To display the new value of element `a` and the unchanged value of element `b`, the Bus Selector block selects elements `a` and `b` and connects them to Display blocks. The Display blocks show the value of these elements after assignment.

> 要显示元素 `a` 的新值和元素 `b` 的不变值，Bus Selector 块选择元素 `a` 和 `b`，并将它们连接到 Display 块。显示块显示赋值后这些元素的值。

- Element `a` has a value of 3, which is the new, assigned value from the Bus Assignment block.
- Element `b` has a value of 2, which is its original value.

You can select additional elements for assignment by selecting an element under **Elements in the bus** then clicking **Select**. The Bus Assignment block adds an input port for each additional element to which you want to assign a new value. The new input ports let you connect the signals that you want to assign to the additional bus elements.

> 你可以通过在**总线元素**下选择一个元素并点击**选择**来选择额外的元素进行分配。总线分配块为每个额外元素添加一个输入端口，以便将要分配的新值连接到该元素。新的输入端口可以让您连接要分配给额外总线元素的信号。

## See Also

### Blocks

- [Bus Assignment](https://ww2.mathworks.cn/help/simulink/slref/busassignment.html) | [Bus Creator](https://ww2.mathworks.cn/help/simulink/slref/buscreator.html) | [Bus Selector](https://ww2.mathworks.cn/help/simulink/slref/busselector.html)

## Related Topics

- [Group Signals or Messages into Virtual Buses](https://ww2.mathworks.cn/help/simulink/ug/getting-started-with-buses.html)
