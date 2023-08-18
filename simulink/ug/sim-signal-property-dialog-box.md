---
url: https://ww2.mathworks.cn/help/simulink/ug/sim-signal-property-dialog-box.html
title: 使用 Simulink.Signal 对象指定和控制信号属性
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-18 21:00:07
tag: 
summary: 使用 Simulink.Signal 对象可以为信号或离散状态分配或验证属性，如数据类型、数值类型、维度等。
---
## 使用 `Simulink.Signal` 对象指定和控制信号属性

使用 [`Simulink.Signal`](https://ww2.mathworks.cn/help/simulink/slref/simulink.signal.html) 对象可以为信号或离散状态分配或验证属性，如数据类型、数值类型、维度等。

### 使用信号对象分配或验证信号属性

您可以使用信号对象来分配或验证信号属性。同样的方法也适用于离散状态。要使用信号对象分配或验证信号属性值，请执行下列操作：

1.  创建与要对其分配或验证属性的信号同名的 `Simulink.Signal` 对象。
    
    1.  打开模型资源管理器。
        
    2.  在 “模型层次结构” 窗格中，选择基础工作区或模型工作区节点，具体取决于所需的信号对象上下文。如果在模型工作区中创建信号对象，则必须将**存储类**参数设置为 “`自动`”。
        
    3.  选择 > 。
        
    
2.  设置与信号源未指定的属性对应的对象的属性，或与要验证的属性对应的对象的属性。
    
3.  启用显式或隐式信号解析：
    
    *   **显式解析:** 在信号的 “信号属性” 对话框中，启用**信号名称必须解析为 Simulink 信号对象**。这是首选方法。有关详细信息，请参阅[显式和隐式符号解析](https://ww2.mathworks.cn/help/simulink/ug/resolving-symbols.html#brjnb77-1)。
        
        使用这种方法时，请将设置为 “`无`” 以外的值。要仅使用显式解析（一种最佳做法），请将参数设置为 “`仅显式`”。
        
    *   **隐式解析:** 将模型的选项设置为 “`显式和隐式`” 或 “`显式和隐式(警告)`”。显式解析是首选方法。
        
    
4.  将信号对象赋给一个工作区变量。
    
5.  将该信号对象与源信号相关联。
    

### 验证

信号与信号对象不匹配时的结果可能取决于几个因素。当您更新图时、运行仿真时或同时进行两者时，Simulink® 软件可以验证信号属性。何时以及如何进行验证取决于内部规则（这些规则可能发生变化），有时还取决于配置参数设置。

并非所有信号验证都会将信号源属性与信号对象属性进行比较。例如，如果使用信号对象指定**最小值**和**最大值**信号值，则信号源必须指定与信号对象相同的值（或者从该对象继承值），但这种验证仅与源和目标之间的协议有关，与在仿真期间强制执行的最小值和最大值无关。

如果**配置参数 > 诊断 > 数据有效性 > 仿真范围检查**的值为 “`无`”（默认值），则 Simulink 不会在仿真过程中强制执行任何最小和最大信号值，即使信号对象提供或验证了它们也是如此。要在仿真过程中强制执行最小和最大信号值，请将**仿真范围检查**设置为 “`警告`” 或 “`错误`”。有关详细信息，请参阅[指定信号范围](https://ww2.mathworks.cn/help/simulink/ug/signal-ranges.html)和[模型配置参数：数据有效性诊断](https://ww2.mathworks.cn/help/simulink/gui/diagnostics-pane-data-validity.html)。

### 多个信号对象

如果信号对象的存储类为 `Auto` 或 `Reusable`，则可以将给定的_信号对象_与多个信号相关联。如果存储类是 `Auto` 并且您清除**信号存储重用**等优化以便生成的代码为所有关联的信号分配内存，则每个信号都显示为一个包含信号和状态数据的全局结构体的唯一命名字段。如果对象的存储类不是 `Auto` 或 `Reusable`，则可以将信号对象与不超过一个信号相关联。

您可以将给定的_信号_与不超过一个信号对象相关联。信号可以多次引用信号对象，但每次引用必须解析为同一个信号对象。引用两个具有完全相同属性的不同信号对象会导致编译时错误。

如果模型将多个信号对象与任一信号关联，就会发生编译时错误。为防止出现该错误，请决定信号使用哪个对象，然后删除或重新配置对任何其他信号对象的所有引用，以便所有其余引用都解析为所选信号对象。有关可用于跟踪信号全部范围的方法的描述，请参阅[突出显示信号的源和目标](https://ww2.mathworks.cn/help/simulink/ug/displaying-signal-sources-and-destinations.html)。

### Signal Specification 模块：`Simulink.Signal` 的替代方法

您可以使用 [Signal Specification](https://ww2.mathworks.cn/help/simulink/slref/signalspecification.html) 模块而不是 `Simulink.Signal` 对象来指定信号源未指定的属性。每种方法都有优点和缺点：

*   使用信号对象可简化模型，并允许您在不编辑模型的情况下更改信号属性值，但不直接在模块图中显示信号属性值。
    
*   使用 Signal Specification 模块可直接在模块图中显示信号属性值，但会使模型复杂化，并需要对其进行编辑才能更改信号属性值。
    

以下两个模型说明了向信号赋予属性的两种方式的相应优点。

在第一个示例中，名为 `Sig1` 的信号对象指定输入端口 `In1` 发出的信号的采样时间和数据类型。

![](https://ww2.mathworks.cn/help/simulink/ug/simulink_signal_with_object.png)

要确定 `Sig1` 信号的属性，可以在模型资源管理器中查看信号对象。在此模型中，采样时间为 `-1`，数据类型为 `auto`。

![](https://ww2.mathworks.cn/help/simulink/ug/simulink_signal_model_explorer.png)

如果使用信号对象指定信号 `Sig1` 的采样时间和数据类型属性，则可以在不编辑模型的情况下更改采样时间或数据类型。例如，您可以使用模型资源管理器、MATLAB® 命令行或 MATLAB 程序更改这些属性。

第二个示例使用 Signal Specification 模块指定输入端口 `In2` 发出的信号的采样时间和数据类型。Signal Specification 模块直接在图中显示数据类型和信号采样时间属性，在本例中分别为 `uint8` 和 `4`。

![](https://ww2.mathworks.cn/help/simulink/ug/signal_with_signal_specification.png)

### 总线支持

#### 使用总线对象作为数据类型

`Simulink.Signal` 支持使用非虚拟总线作为输出数据类型。

如果将信号对象的**数据类型**设置为总线对象，则不能将信号对象与非总线信号关联。

#### 使用结构体作为初始值

如果使用总线对象作为数据类型，请将**初始值**设置为 `0` 或者与该总线对象匹配的 MATLAB 结构体。

您指定的结构体中必须为总线对象所代表的总线中的每个元素包含一个值。

您可以使用 [`Simulink.Bus.createMATLABStruct`](https://ww2.mathworks.cn/help/simulink/slref/simulink.bus.creatematlabstruct.html) 创建一个与总线对应的完全结构体。

也可以使用 [`Simulink.Bus.createObject`](https://ww2.mathworks.cn/help/simulink/slref/simulink.bus.createobject.html) 从 MATLAB 结构体中创建一个总线对象。

## 另请参阅

[`Simulink.Signal`](https://ww2.mathworks.cn/help/simulink/slref/simulink.signal.html)

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