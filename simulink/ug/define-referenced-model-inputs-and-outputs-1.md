---
url: https://ww2.mathworks.cn/help/simulink/ug/define-referenced-model-inputs-and-outputs-1.html#mw_cf67482d-bf21-4aec-8eda-e8d60bd5d580
title: 模型引用接口和边界
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-18 20:59:29
tag: 
summary: 引用模型中的端口与模型引用中的端口相对应。跨越模型边界的信号必须满足特定要求。
---
## 模型引用接口和边界

Model 模块具有输入、输出和控制端口，这些端口与它所引用模型的根级输入、输出和控制端口相对应。引用模型可以包含 Inport、Outport、In Bus Element、Out Bus Element、Trigger 和 Enable 模块，用于从父模型获取输入并向父模型提供输出。Model 模块的输入信号必须对引用模型的对应输入模块有效。Model 模块的输出信号是引用模型的根级输出模块信号。

在 `[sldemo_mdlref_basic](matlab:openExample('simulink_features/ComponentBasedModelingWithModelReferenceExample'))` 中，每个 Model 模块有三个输入：两个 Constant 模块和一个 Pulse Generator 模块。每个 Model 模块都有一个输出信号记录到示波器。由于每个 Pulse Generator 模块的输入信号使用不同的采样时间，因此对于每个模型实例，各个 Model 模块的输出信号也各不相同。

![](https://ww2.mathworks.cn/help/simulink/ug/model_reference_parent.png)

为了与父模型建立连接，引用模型 `sldemo_mdlref_counter` 也包括三个 Inport 模块（**上限**、**下限**和**输入**）和一个 Outport 模块（**输出**）。

![](https://ww2.mathworks.cn/help/simulink/ug/submodel.png)

要查看每个 Model 模块的输出信号有何不同，您可以使用**[仿真数据检查器](https://ww2.mathworks.cn/help/simulink/slref/simulationdatainspector.html)**。

![](https://ww2.mathworks.cn/help/simulink/ug/mdlref_sdi.png)

### 刷新 Model 模块

刷新 Model 模块会更新其内部表示，以反映对引用模型的接口的更改。例如，当引用模型获得或丢失一个端口时，刷新 Model 模块会更新其端口。

默认情况下，在加载引用模型时，引用它的 Model 模块会自动刷新。如果未加载引用模型，则当您执行诸如以下的操作时，对应的 Model 模块会刷新：

*   打开父模型
    
*   选择 Model 模块
    
*   仿真模型层次结构
    
*   为模型层次结构生成代码
    

当您选择 Model 模块时，您可以通过点击**模型模块**选项卡上的**刷新**按钮箭头，然后点击来刷新模型层次结构中的所有 Model 模块。

如果需要在 Simulink® 检测到 Model 模块可能与其引用模型不匹配时收到通知，请更改下列诊断配置参数的默认设置：

当模型的这些配置参数设置为 “`错误`” 时，该模型中的 Model 模块不会自动刷新。要在这些配置参数设置为 “`错误`” 时刷新 Model 模块，请执行以下操作：

### 信号传播

引用模型中的信号属性独立于 Model 模块的上下文。例如，信号维度和数据类型不会跨 Model 模块边界传播。要在引用模型中定义信号属性，请为根级 Inport 和 In Bus Element 模块定义模块参数。

对于连接到 Outport 模块以从引用模型传播到父模型的信号，信号名称必须显式出现在信号线上。

对于接口处的总线，使用 [In Bus Element](https://ww2.mathworks.cn/help/simulink/slref/inbuselement.html) 模块代替 Inport 和 Bus Selector 模块用于输入，用 [Out Bus Element](https://ww2.mathworks.cn/help/simulink/slref/outbuselement.html) 代替 Outport 和 Bus Creator 模块用于输出。与 Inport 和 Outport 模块不同，In Bus Element 和 Out Bus Element 模块支持多速率虚拟总线并且在模型接口处不要求使用 `Simulink.Bus` 对象。它们还提供更干净的总线接口。有关详细信息，请参阅 [Simplify Subsystem and Model Interfaces with Bus Element Ports](https://ww2.mathworks.cn/help/simulink/ug/simplify-subsystem-bus-interfaces.html)。

有关将总线和 Inport 模块结合使用的模型层次结构的示例，请参阅 [Interface Specification Using Bus Objects](https://ww2.mathworks.cn/help/simulink/slref/interface-specification-using-bus-objects.html)。

引用模型只能输入或输出用户定义的定点数据类型或者由 `Simulink.DataType` 或 `Simulink.Bus` 对象定义的数据类型。

### 引用模型中的信号记录

在引用模型中，您可以对配置为信号记录的任何信号进行记录。使用信号记录选择器选择模型层次结构中配置为信号记录的部分或所有信号。有关详细信息，请参阅 [Override Signal Logging Settings](https://ww2.mathworks.cn/help/simulink/ug/overriding-signal-logging-settings.html)。

您可以使用仿真数据检查器来查看和分析在引用模型中记录的信号。您可以在多个绘图上查看信号、缩放和使用数据游标来理解和计算数据。此外，您可以比较来自多个仿真的信号数据。有关查看引用模型中信号的示例，请参阅 [Viewing Signals in Model Reference Instances](https://ww2.mathworks.cn/help/simulink/slref/viewing-signals-in-model-reference-instances.html)。

### 采样时间要求

连接到引用模型根级输入或输出模块的第一个非虚拟模块必须与相关端口具有相同的采样时间。如果采样时间不同，请使用 [Rate Transition](https://ww2.mathworks.cn/help/simulink/slref/ratetransition.html) 模块来匹配输入和输出采样时间，如下图所示。

![](https://ww2.mathworks.cn/help/simulink/ug/mod_ref_match_i_o_rate.png)

### 在引用模型实例之间共享数据

默认情况下，每个 Model 模块实例分别读写模型中不同的信号和模块状态副本。因此，实例之间不会通过共享信号或状态数据进行交互。

要在所有实例之间共享一段数据（例如，累加器或故障指示器），可将数据建模为数据存储。

有关数据存储的详细信息，请参阅[通过创建数据存储对全局数据建模](https://ww2.mathworks.cn/help/simulink/ug/model-global-data-using-data-stores.html)。

## 另请参阅

[Model](https://ww2.mathworks.cn/help/simulink/slref/model.html) | [Inport](https://ww2.mathworks.cn/help/simulink/slref/inport.html) | [Outport](https://ww2.mathworks.cn/help/simulink/slref/outport.html) | [In Bus Element](https://ww2.mathworks.cn/help/simulink/slref/inbuselement.html) | [Out Bus Element](https://ww2.mathworks.cn/help/simulink/slref/outbuselement.html)

## 相关主题

*   [Use Buses at Model Interfaces](https://ww2.mathworks.cn/help/simulink/ug/simplify-subsystem-bus-interfaces.html#btmmqdp)
*   [模型引用的要求和限制](https://ww2.mathworks.cn/help/simulink/ug/model-referencing-limitations.html)
*   [Conditionally Execute Referenced Models](https://ww2.mathworks.cn/help/simulink/ug/create-and-reference-conditional-referenced-models.html)

本页内容对您有帮助吗？

Unrated

1 star

2 stars

3 stars

4 stars

5 stars

 Unrated  1 star  2 stars  3 stars  4 stars  5 stars

您点击的链接对应于以下 MATLAB 命令：

请在 MATLAB 命令行窗口中直接输入以执行命令。Web 浏览器不支持 MATLAB 命令。