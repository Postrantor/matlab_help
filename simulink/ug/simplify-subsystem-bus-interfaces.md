---
tip: translate by openai@2023-06-25 14:05:52
> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [ww2.mathworks.cn](https://ww2.mathworks.cn/help/simulink/ug/simplify-subsystem-bus-interfaces.html)
[](D:\Document\MATLAB\matlab_help\videos\reduced-bus-wiring-bus-element-ports.md)
[](D:\Document\MATLAB\matlab_help\simulink\slref\simulink-bus-signals.md)
---
## Simplify Subsystem and Model Interfaces with Bus Element Ports

Buses simplify subsystem and model interfaces by letting you associate multiple signals or messages with one port. They reduce line complexity and clutter in a block diagram, make it easier to change the interface incrementally, and allow access to elements closer to their point of usage.

> 公共汽车可以通过将多个信号或消息与一个端口相关联，简化子系统和模型接口。它们减少了块图中的线路复杂性和杂乱，使得接口可以逐步改变，并允许访问更接近它们使用点的元素。

For an overview of how to use buses to simplify composite interfaces, see [Reduced Bus Wiring: Bus Element Ports (2 min, 7 sec)](https://www.mathworks.com/videos/reduced-bus-wiring-bus-element-ports-1487889625625.html).

> 要了解如何使用总线来简化复合接口，请参见[减少总线布线：总线元素端口（2 分 7 秒）](https://www.mathworks.com/videos/reduced-bus-wiring-bus-element-ports-1487889625625.html)。

For example, this model contains subsystems where each subsystem interface has multiple ports.

> 例如，这个模型包含多个子系统，每个子系统的接口都有多个端口。

![](https://ww2.mathworks.cn/help/simulink/ug/bus-port-conversion-before.png)

This equivalent model uses buses and has one port per subsystem interface.

> 这个等效模型使用公共汽车，每个子系统接口有一个端口。

![](https://ww2.mathworks.cn/help/simulink/ug/bus-port-conversion-after.png)

For buses at interfaces, use [In Bus Element](https://ww2.mathworks.cn/help/simulink/slref/inbuselement.html) and [Out Bus Element](https://ww2.mathworks.cn/help/simulink/slref/outbuselement.html) blocks instead of Inport and Bus Selector blocks for inputs and Outport and Bus Creator blocks for outputs. In Bus Element and Out Bus Element blocks support multirate virtual buses and do not require `Simulink.Bus` objects at model interfaces, unlike Inport and Outport blocks. They also provide cleaner bus interfaces.

> 对于接口上的总线，请使用 [In Bus Element](https://ww2.mathworks.cn/help/simulink/slref/inbuselement.html) 和 [Out Bus Element](https://ww2.mathworks.cn/help/simulink/slref/outbuselement.html) 块来代替输入的 Inport 和 Bus Selector 块以及输出的 Outport 和 Bus Creator 块。In Bus Element 和 Out Bus Element 块支持多转率虚拟总线，与 Inport 和 Outport 块不同，不需要在模型接口处使用 `Simulink.Bus` 对象。它们还提供更清晰的总线接口。

For example, this model uses Inport, Bus Selector, Bus Creator, and Outport blocks.

> 例如，此模型使用 Inport、Bus Selector、Bus Creator 和 Outport 块。

![](https://ww2.mathworks.cn/help/simulink/ug/bus_port_before.png)

This equivalent model uses In Bus Element and Out Bus Element blocks.

![](https://ww2.mathworks.cn/help/simulink/ug/bus_port_after.png)

At the interface you want to update, the lines and blocks must not have nondefault specifications that could cause conflicts. For example, the line between a Bus Creator block and an Outport block must not be named or marked for signal logging. Similarly, while you can specify a [`Simulink.Bus`](https://ww2.mathworks.cn/help/simulink/slref/simulink.bus.html) object data type for an output port, you must not specify a nondefault data type for an element of the output port, such as `TopBus1.NestedBus1`.

> 在你想要更新的接口上，线和块不能有可能引起冲突的非默认规格。例如，Bus Creator 块和 Outport 块之间的线不能命名或标记为信号记录。同样，尽管你可以为输出端口指定[`Simulink.Bus`](https://ww2.mathworks.cn/help/simulink/slref/simulink.bus.html)对象数据类型，但你不能为输出端口的元素指定非默认数据类型，例如 `TopBus1.NestedBus1`。

The following examples demonstrate how to update interfaces to use In Bus Element and Out Bus Element blocks. The example models are simple, however, buses are most useful when you have many elements to combine.

> 以下示例演示了如何更新接口以使用 In Bus Element 和 Out Bus Element 块。示例模型很简单，但是当您有许多元素要组合时，总线最有用。

### Combine Multiple Subsystem Ports into One Port

This example shows three ways to simplify a subsystem interface by converting multiple ports and their connected signals into one port and a bus. Model interfaces do not support this automated conversion.

> 这个例子展示了三种简化子系统接口的方法，将多个端口及其连接的信号转换为一个端口和一个总线。模型接口不支持这种自动转换。

Open the example model, which contains two subsystems with multiple input and output ports.

> 打开这个示例模型，它包含有多个输入和输出端口的两个子系统。

![](https://ww2.mathworks.cn/help/examples/simulink/win64/ConvertMultiplePortsIntoABusPortExample_01.png)

Drag a selection box around the signal lines between the two subsystems. From the action bar that appears, click **Create Bus**.

> 拖动鼠标在两个子系统之间的信号线上绘制一个选择框。从出现的动作栏中，点击**创建总线**。

![](https://ww2.mathworks.cn/help/examples/simulink/win64/xxVirtualBusPortCreationBetween.png)

Simulink replaces the Inport and Outport blocks in the source and destination subsystems with In Bus Element and Out Bus Element blocks.

> Simulink 会把源子系统和目标子系统中的 Inport 和 Outport 块替换为 In Bus Element 和 Out Bus Element 块。

Drag a selection box around the signal lines between the source blocks and first subsystem. From the action bar that appears, click **Create Bus**.

> 在源块和第一个子系统之间的信号线周围拖动一个选择框。从出现的操作栏中，单击**创建总线**。

![](https://ww2.mathworks.cn/help/examples/simulink/win64/xxVirtualBusPortCreationInput.png)

Simulink adds a Bus Creator block before the first subsystem and replaces the Inport blocks in the first subsystem with In Bus Element blocks.

> Simulink 会在第一个子系统之前添加一个总线创建器块，并用 In Bus 元件块替换第一个子系统中的 Inport 块。

Drag a selection box around the signal lines between the second subsystem and Scope blocks. From the action bar that appears, click **Create Bus**.

> 在第二个子系统和 Scope 块之间的信号线周围拖动选择框。从出现的操作栏中，单击**创建总线**。

![](https://ww2.mathworks.cn/help/examples/simulink/win64/xxVirtualBusPortCreationOutput.png)

Simulink replaces the Outport blocks in the second subsystem with Out Bus Element blocks and adds a Bus Selector block after the second subsystem.

> Simulink 將第二個子系統中的 Outport 塊替換為 Out Bus Element 塊，並在第二個子系統之後添加一個 Bus Selector 塊。

The resulting model uses virtual buses at the subsystem interfaces.

![](https://ww2.mathworks.cn/help/examples/simulink/win64/xxVirtualBusPortCreationFinal.png)

### Simplify Bus Interfaces in Subsystems and Models

This example shows how to convert a subsystem or model interface that uses Inport, Bus Selector, Bus Creator, and Outport blocks to use In Bus Element and Out Bus Element blocks.

> 这个例子展示了如何将使用 Inport、Bus Selector、Bus Creator 和 Outport 块的子系统或模型接口转换为使用 In Bus Element 和 Out Bus Element 块。

Open the example model, which contains a subsystem that modifies an input bus hierarchy using Bus Selector and Bus Creator blocks. The subsystem uses Inport and Outport blocks for input and output.

> 打开示例模型，其中包含一个子系统，使用 Bus Selector 和 Bus Creator 块修改输入总线层次结构。该子系统使用 Inport 和 Outport 块进行输入和输出。

Compile the model to update line styles, which you can use to visually identify buses. In the Simulink Toolstrip, on the **Modeling** tab, click **Update Model** or **Run**.

> 在 Simulink 工具条中，在**建模**选项卡上，单击**更新模型**或**运行**，以编译模型以更新线条样式，以便可以使用它们来直观地识别公共汽车。

![](https://ww2.mathworks.cn/help/examples/simulink/win64/SimplifyBusInterfacesExample_01.png)

Open the subsystem.

![](https://ww2.mathworks.cn/help/examples/simulink/win64/SimplifyBusInterfacesExample_02.png)

To convert Inport and Bus Selector blocks to In Bus Element blocks:

1. Click a Bus Selector block that directly connects to an Inport block.
2. In the action bar that appears when you pause over the ellipsis, click **Bus Ports**.

> 在悬停在省略号上时出现的操作栏中，单击**公共港口**。

![](https://ww2.mathworks.cn/help/examples/simulink/win64/xxBusInterfaceConversionIn1.png)

You can similarly convert an In Bus Element and Bus Selector block.

![](https://ww2.mathworks.cn/help/examples/simulink/win64/xxBusInterfaceConversionIn2.png)

To convert Outport and Bus Creator blocks to Out Bus Element blocks:

1. Click a Bus Creator block that directly connects to an Outport block without branching.

> 点击一个 Bus Creator 块，它直接连接到一个 Outport 块，而不需要分支。

2. In the action bar that appears when you pause over the ellipsis, click **Bus Ports**.

> 在悬停在省略号上时出现的操作栏中，点击**总线端口**。

![](https://ww2.mathworks.cn/help/examples/simulink/win64/xxBusInterfaceConversionOut1.png)

You can similarly convert Out Bus Element and Bus Creator blocks.

![](https://ww2.mathworks.cn/help/examples/simulink/win64/xxBusInterfaceConversionOut2.png)

The resulting model simplifies line routing, makes it easier to incrementally change the interface, and lets you access elements closer to their point of usage.

> 结果模型简化了线路路由，使得可以逐步改变界面，并让您更接近其使用点访问元素。

![](https://ww2.mathworks.cn/help/examples/simulink/win64/xxBusInterfaceConversionResult.png)

You can change the name of a bus and its elements by double-clicking the block labels and editing them.

> 双击区块标签，并编辑它们，可以更改公共汽车及其元素的名称。

To easily identify elements of the same nested bus or bus port, specify block colors.

> 为了容易识别同一嵌套总线或总线端口的元素，请指定块的颜色。

1. Double-click an In Bus Element or Out Bus Element block to open the dialog box for the related port.

> 双击一个 In Bus 元素或 Out Bus 元素块，可以打开与之相关联的端口的对话框。 2. Select an element or the top bus. 3. Specify the background color with the **Set color** dropdown menu.

![](https://ww2.mathworks.cn/help/examples/simulink/win64/xxBusInterfaceConversionFinal.png)

### Use Buses at Model Interfaces

Bus input for a Model block must be consistent with the bus expected by the referenced model.

> 输入到模型块的总线必须与参考模型所期望的总线一致。

If you use a bus as an input to or an output from a referenced model:

- Only a nonvirtual bus can contain variable-size signal elements.
- For code generation, you can only configure the `I/O arguments step method` style of the C++ class interface for the referenced model when using a nonvirtual bus or when using the `Default` style of the C++ class interface.

> 当使用非虚拟总线或使用 C++ 类接口的 `Default` 样式时，您只能为引用的模型配置 `I/O参数步骤方法` 样式的 C++ 类接口来进行代码生成。

- For code generation, you can only configure function prototype control for the referenced model when using a nonvirtual bus.

> 对于使用非虚拟总线时，只能为引用模型配置函数原型控制以用于代码生成。

## See Also

[In Bus Element](https://ww2.mathworks.cn/help/simulink/slref/inbuselement.html) | [Out Bus Element](https://ww2.mathworks.cn/help/simulink/slref/outbuselement.html)

> [入总线元件](https://ww2.mathworks.cn/help/simulink/slref/inbuselement.html) | [出总线元件](https://ww2.mathworks.cn/help/simulink/slref/outbuselement.html)

## Related Topics

- [Composite Interface Guidelines](https://ww2.mathworks.cn/help/simulink/ug/composite-signal-techniques.html)
- [Group Signals or Messages into Virtual Buses](https://ww2.mathworks.cn/help/simulink/ug/getting-started-with-buses.html)
- [Create Nonvirtual Buses](https://ww2.mathworks.cn/help/simulink/ug/create-nonvirtual-buses.html)
- [Load Input Data for a Bus Using In Bus Element Blocks](https://ww2.mathworks.cn/help/simulink/ug/load-input-data-for-a-bus-using-in-bus-element-blocks.html)
- [Load Bus Data to Root-Level Input Ports](https://ww2.mathworks.cn/help/simulink/ug/importing-structures-of-matlab-timeseries-objects-for-bus-signals-to-a-root-level-input-port.html)
