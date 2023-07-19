---
url: https://ww2.mathworks.cn/help/simulink/slref/simulink-bus-signals.html
title: 了解 Simulink 总线功能
- MATLAB & Simulink
- MathWorks 中国
date: 2023-06-25 14:06:56
tag: 
summary: 在组件中使用总线，在组件接口上使用总线端口，并更快地执行常见总线工作流。
---
此示例从三个方面向您介绍 Simulink® 总线的功能：

- 在组件中使用总线
- 在组件接口上使用总线端口
- 智能编辑，用于更快地执行常见总线工作流

打开 `slexBusExample` 模型。

![](https://ww2.mathworks.cn/help/simulink/slref/simulinkbussignalsexample_01_zh_CN.png)

### 显示总线线型

打开包含总线的模型时，总线与标量信号具有相同的线型。要更新线型，请在**建模**选项卡上，选择**更新模型**。

![](https://ww2.mathworks.cn/help/examples/simulink_features/win64/xxBusLineStyle.png)

在模型编译后，有几条信号线显示为三条线。这种线型指示该信号线是一条总线。

### 在组件中使用总线

“在组件中使用总线” 方面，子系统的内容说明如何：

- 使用 Bus Creator 模块创建总线。
- 使用 Bus Assignment 模块替换总线中的元素。
- 使用 Bus Selector 模块从总线中提取元素。

![](https://ww2.mathworks.cn/help/examples/simulink_features/win64/SimulinkBusSignalsExample_02.png)

每个 [Bus Creator](https://ww2.mathworks.cn/help/simulink/slref/buscreator.html) 模块将连接到其输入端口的元素组合到一条总线中。一条总线表示一组元素，类似于捆绑在一起的一束电线。例如，由名为 Bus Creator 1 的 Bus Creator 模块创建的总线包含连接到其输入端口的 `sine` 信号和 `chirp` 信号。

要查看总线的层次结构，请点击总线，并在**信号**选项卡上选择**信号层次结构**。

您还可以创建*嵌套*总线。例如，`sinusoidal` 和 `nonsinusoidal` 是由名为 Bus Creator 3 的 Bus Creator 模块创建的总线中的嵌套总线。

[Bus Assignment](https://ww2.mathworks.cn/help/simulink/slref/busassignment.html) 模块替换连接到其**总线**输入端口的总线中的一个或多个元素。例如，Bus Assignment 模块将名为 Bus Creator 3 的 Bus Creator 模块创建的总线中的 `constant` 信号和 `nonsinusoidal` 信号替换为新信号。您可以使用 Bus Assignment 模块来替换嵌套的总线和非总线元素。

[Bus Selector](https://ww2.mathworks.cn/help/simulink/slref/busselector.html) 模块从连接到其输入的总线中提取一个或多个元素。例如，Bus Selector 模块选择 `nonsinusoidal.pulse`、`sinusoidal.sine` 和 `constant` 信号。要在 Scope 模块中显示 `nonsinusoidal.pulse` 和 `sinusoidal.sine` 的值，在 Display 模块中 `constant` 的值，请仿真该模型。

### 在组件接口上使用总线端口

“在组件接口上使用总线端口” 中的子系统演示如何：

- 使用 Out Bus Element 模块在组件的输出端口创建总线。
- 使用 In Bus Element 模块从组件的输入端口提取总线元素。

第一个子系统由五个源模块和五个 Out Bus Element 模块组成。

![](https://ww2.mathworks.cn/help/examples/simulink_features/win64/SimulinkBusSignalsExample_03.png)

[Out Bus Element](https://ww2.mathworks.cn/help/simulink/slref/outbuselement.html) 模块类似于连接到 Outport 模块的 Bus Creator 模块。每个 Out Bus Element 模块都有一个标签，您可以直接编辑该标签来更改输出端口和总线元素的名称。

![](https://ww2.mathworks.cn/help/examples/simulink_features/win64/xxslexBusExampleChangeElement.gif)

您可以用扩展或紧凑表示法显示标签。

- **扩展表示法**：标签显示对应的端口名称和元素层次结构。例如，标签为 `Out1.sinusoidal.sine` 的 Out Bus Element 模块在名为 **Out1** 的输出端口上名为 `sinusoidal` 的嵌套总线中创建一个名为 `sine` 的总线元素信号。
- **紧凑表示法**：标签仅显示叶总线元素名称。例如，标签 `Out1.sinusoidal.sine` 变为 `sine`。

![](https://ww2.mathworks.cn/help/examples/simulink_features/win64/xxslexBusExampleDisplayModes.gif)

在任一模式下，您都可以直接编辑标签的两个部分。

要在总线中创建新元素，请复制并粘贴一个 Out Bus Element 模块。要在接口上创建新输出端口，请右键点击并拖动一个 Out Bus Element 模块，然后选择**创建新端口**。

![](https://ww2.mathworks.cn/help/examples/simulink_features/win64/xxslexBusExampleCreationOutput.gif)

要查看由一组 Out Bus Element 模块创建的总线，请双击其中一个模块以打开端口属性对话框。在该对话框中，您可以：

- 更改端口的名称和编号。
- 突出显示与您选择的元素对应的信号线。
- 按总线或所选元素单独更改模块的颜色。
- 对总线中的元素重新排序。
- 添加或删除总线元素及其对应的模块。
- 指定属性。

![](https://ww2.mathworks.cn/help/examples/simulink_features/win64/xxslexBusExampleOutputDialog.gif)

第二个子系统由两个 Scope 模块、一个 Display 模块和五个 In Bus Element 模块组成。

![](https://ww2.mathworks.cn/help/examples/simulink_features/win64/SimulinkBusSignalsExample_04.png)

[In Bus Element](https://ww2.mathworks.cn/help/simulink/slref/inbuselement.html) 模块类似于连接到 Bus Selector 模块的 Inport 模块。In Bus Element 模块的标签与 Out Bus Element 模块标签的工作方式相同。例如，标签为 `In1.sinusoidal.sine` 的 In Bus Element 模块在名为 `sinusoidal` 的嵌套总线中选择名为 `sine` 的总线元素。

要更改从输入总线中选择的元素，请直接编辑标签文本。当总线连接到对应的输入端口时，您可以从可用信号列表中进行选择。

![](https://ww2.mathworks.cn/help/examples/simulink_features/win64/xxslexBusExampleChangeElementList.gif)

要从总线中选择新元素，请复制并粘贴一个 In Bus Element 模块。要在子系统接口上创建新输入端口，请右键点击并拖动一个 In Bus Element 模块，然后选择**创建新端口**。

要查看由一组 In Bus Element 模块访问的总线，请双击其中一个模块的图标以打开端口属性对话框。在该对话框中，您可以：

- 更改端口的名称和编号。
- 按总线或所选元素单独更改模块的颜色。
- 观察传入总线中任何缺失或未使用的元素。
- 添加或删除与所选元素对应的模块。
- 指定属性。

有关使用 In Bus Element 和 Out Bus Element 模块的详细信息，请参阅 [Simplify Subsystem and Model Interfaces with Bus Element Ports](https://ww2.mathworks.cn/help/simulink/ug/simplify-subsystem-bus-interfaces.html)。

### 更快地执行常见总线工作流

“智能编辑以更快地执行常见总线工作流” 部分的子系统显示如何加快执行常见总线任务的速度：

- 将 Bus Selector 和 Bus Creator 模块转换为 In Bus Element 和 Out Bus Element 模块。
- 在子系统接口上创建总线，并将各个模块的输出捆绑到一条总线中。
- 自动创建端口以将新元素添加到一个 Bus Creator 模块，并从 Bus Selector 模块中选择新元素。

**总线端口**智能编辑提示将子系统接口上的 Bus Selector 和 Bus Creator 模块转换为 In Bus Element 和 Out Bus Element 模块。

1. 点击连接到 Inport 模块的 Bus Selector 模块或连接到 Outport 模块的 Bus Creator 模块。
2. 从操作栏中选择**总线端口**。

![](https://ww2.mathworks.cn/help/examples/simulink_features/win64/xxslexBusExampleBusElementPorts.gif)

**创建总线**智能编辑提示将多个元素捆绑到一条总线中。

1. 拖动以形成一个选择框来选择元素。
2. 从操作栏中选择**创建总线**。

在子系统接口上创建总线时，此操作将所选元素捆绑到一条总线中，将 Inport 和 Outport 模块替换为子系统中的 In Bus Element 和 Out Bus Element 模块，并添加 Bus Creator 和 Bus Selector 模块以保持子系统外部的连接性。

![](https://ww2.mathworks.cn/help/examples/simulink_features/win64/xxslexBusExampleCreateBusForSubsystems.gif)

当在单个模块的输出端创建总线时，此操作会插入一个 Bus Creator 模块，调整其大小，并连接元素。

![](https://ww2.mathworks.cn/help/examples/simulink_features/win64/xxslexBusExampleCreateBusBlockOutputs.gif)

要向总线添加元素，请将一条信号线拖到 Bus Creator 模块。要从总线中选择元素，请将一条信号线拖到 Bus Selector 模块，然后从可用元素列表中选择所需的元素。

![](https://ww2.mathworks.cn/help/examples/simulink_features/win64/xxslexBusExamplePortSprouting.gif)

## 另请参阅

[Bus Assignment](https://ww2.mathworks.cn/help/simulink/slref/busassignment.html) | [Bus Creator](https://ww2.mathworks.cn/help/simulink/slref/buscreator.html) | [Bus Selector](https://ww2.mathworks.cn/help/simulink/slref/busselector.html) | [In Bus Element](https://ww2.mathworks.cn/help/simulink/slref/inbuselement.html) | [Out Bus Element](https://ww2.mathworks.cn/help/simulink/slref/outbuselement.html)

## 相关主题

- [合成接口规范](https://ww2.mathworks.cn/help/simulink/ug/composite-signal-techniques.html)
- [将信号或消息组合到虚拟总线中](https://ww2.mathworks.cn/help/simulink/ug/getting-started-with-buses.html)
- [Simplify Subsystem and Model Interfaces with Bus Element Ports](https://ww2.mathworks.cn/help/simulink/ug/simplify-subsystem-bus-interfaces.html)
- [Display Bus Information](https://ww2.mathworks.cn/help/simulink/ug/view-composite-signals.html)
- [Assign Signal Values to Bus Elements](https://ww2.mathworks.cn/help/simulink/ug/assign-signal-values-to-a-bus.html)

您曾对此示例进行过修改。是否要打开带有您的编辑的示例？

本页内容对您有帮助吗？

Unrated

1 star

2 stars

3 stars

4 stars

5 stars

Unrated 1 star 2 stars 3 stars 4 stars 5 stars

您点击的链接对应于以下 MATLAB 命令：

请在 MATLAB 命令行窗口中直接输入以执行命令。Web 浏览器不支持 MATLAB 命令。
