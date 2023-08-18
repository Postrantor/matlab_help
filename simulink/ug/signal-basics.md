---
url: https://ww2.mathworks.cn/help/simulink/ug/signal-basics.html#bvcdbmy
title: 信号基础知识
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-18 20:59:20
tag: 
summary: 创建、配置、识别和测试信号。
---
## 信号基础知识

_信号_是指在所有时间点都有对应值的时变量。您可以指定各种信号属性，包括：

*   信号名称
    
*   数据类型（例如，8 位、16 位或 32 位整数）
    
*   数值类型（实数或复数）
    
*   维度（一维、二维或多维数组）
    

在 Simulink® 中，信号是指 Simulink 模块图中的模块以及模块图本身所表示的动态系统的输出。[模块图](https://www.mathworks.com/discovery/block-diagram.html)中的线条表示模块图所定义的信号之间的数学关系。例如，连接模块 A 的输出端口和模块 B 的输入端口的线条指明模块 B 的信号输出取决于模块 A 的信号输出。

Simulink 模块图用带有箭头的线条来表示信号。信号的来源是指在计算模块方法（方程）的过程中写入信号的模块。信号的目标是指在计算模块方法（方程）的过程中读取信号的模块。模型中信号的目标位置不一定表示模型中模块的仿真顺序。仿真顺序由 Simulink 自动确定。

**注意**

Simulink 信号是数学概念，不是物理实体。模块图中的线条表示模块之间的数学关系，而不是物理关系。Simulink 信号并不像沿着电线传输的电信号一样沿着连接模块的线条进行传输。模块图并不表示模块之间的物理连接。

您可以通过向模型中添加信源模块来创建信号。例如，可从 Simulink Sources 库向您的模型中添加一个 [Sine, Cosine](https://ww2.mathworks.cn/help/simulink/slref/sine.html) 模块实例，创建一条随时间沿正弦曲线变化的信号。要查看在模型中创建信号的模块的列表，请参阅 [Sources](https://ww2.mathworks.cn/help/simulink/sources.html)。您也可以使用 [Viewers and Generators Manager](https://ww2.mathworks.cn/help/simulink/ug/signal-and-scope-manager.html) 在模型中创建信号，而不使用模块。

### 信号线的线型

Simulink 模型可以包含许多不同类型的信号。当您构造模块图时，所有信号类型都显示为一条细细的实线。当您更新图或开始仿真之后，信号将以指定的线型显示。这些信号类型使您能够区分不同信号类型。在所有信号类型中，您只能自定义非标量信号类型。要了解详细信息，请参阅[信号类型](https://ww2.mathworks.cn/help/simulink/ug/signal-types.html)。

### 信号属性

您可能希望在模型中指定信号属性，以便为信号指定名称或标签，准备要记录的数据，或在模型中自定义信号。使用属性检查器、模型数据编辑器或 “信号属性” 对话框来为以下各项指定属性：

*   信号名称和标签
    
*   信号记录
    
*   Simulink Coder™ 进行代码生成
    
*   对信号进行说明
    

要在属性检查器中访问信号属性，请首先显示属性检查器。在**建模**选项卡上，在**设计**下，点击**属性检查器**。选择信号之后，属性将显示在属性检查器中。

要打开模型数据编辑器，请在**建模**选项卡上，点击**模型数据编辑器**。然后，检查**信号**选项卡并选择一个信号。

要使用 “信号属性” 对话框，请右键点击信号并选择。

要以编程方式指定信号属性，请使用函数（例如 `get_param`）创建一个变量，该变量存储创建信号线的模块输出端口的句柄。然后使用 `set_param` 设置该端口的编程参数。例如：

```
p = get_param(gcb,'PortHandles')
l = get_param(p.Outport,'Line')
set_param(l,'Name','s9')

```

### 信号名称和标签

您可以在模型中以交互方式或编程方式命名信号。信号名称的语法要求取决于您如何使用该名称。最常见的情况包括：

*   请勿使用小于号字符 (`<`) 作为信号名称的开头。
    
*   信号名称可以解析为 `Simulink.Signal` 对象。（请参阅 [`Simulink.Signal`](https://ww2.mathworks.cn/help/simulink/slref/simulink.signal.html)。）这种情况下，信号名称必须是合法的 MATLAB® 标识符。此标识符以字母字符开头，后跟字母数字或下划线字符，长度由函数 [`namelengthmax`](https://ww2.mathworks.cn/help/matlab/ref/namelengthmax.html) 指定。
    
*   信号已经具有名称，因此可在数据记录中按名称进行标识和引用。（请参阅 [使用信号记录导出信号数据](https://ww2.mathworks.cn/help/simulink/ug/exporting-signal-data-using-signal-logging.html)。）此类信号名称可以包含空格和换行符。这些字符可以提高可读性，但有时需要特殊的处理技巧，如[处理记录的名称中的空格和换行符](https://ww2.mathworks.cn/help/simulink/ug/accessing-signal-logging-data.html#budgyrh)中所述。
    
*   信号名称的存在只是为了使模块图更清晰，没有任何计算意义。此类信号名称可以包含任何内容，且不需要特殊处理。
    
*   信号是总线对象的一个元素。使用有效的 C 语言标识符作为信号名称。
    
*   Bus Creator 模块的输入必须具有唯一名称。如果存在重复的名称，Bus Creator 模块将在所有输入信号名称后面追加 `(signal#)`，其中 `#` 是输入端口的索引。
    

确保每个信号名称都是合法的 MATLAB 标识符可以处理各种模型配置。有些特殊的需求可能要求更改信号名称，以符合更严格的语法。您可以使用函数 [`isvarname`](https://ww2.mathworks.cn/help/matlab/ref/isvarname.html) 确定信号名称是否为合法的 MATLAB 标识符。

使用以下选项之一以交互方式命名信号：

默认情况下，信号名称以_信号标签_的形式显示在信号下面。

要以编程方式命名信号，请对信号使用 [`get_param`](https://ww2.mathworks.cn/help/simulink/slref/get_param.html) 和 [`set_param`](https://ww2.mathworks.cn/help/simulink/slref/set_param.html) 函数。下表概述如何在 Simulink 编辑器中使用信号名称和标签。

<table><colgroup><col width="39%"><col width="61%"></colgroup><thead><tr><th>任务</th><th>操作</th></tr></thead><tbody><tr><td>为信号线命名</td><td>双击信号并键入其名称。</td></tr><tr><td>为已命名的信号线的分支命名</td><td>双击分支。</td></tr><tr><td>为一个信号的每个分支命名</td><td>右键点击该信号，选择<strong>属性</strong>，然后使用对话框进行操作。</td></tr><tr><td>删除信号标签和名称</td><td>删除标签中的字符，或者在 “信号属性” 对话框中删除名称。</td></tr><tr><td>只删除信号标签</td><td>右键点击标签，然后选择<strong>删除标签</strong>。</td></tr><tr><td>打开信号标签文本框进行编辑</td><td><p>双击信号线。</p><p>点击标签。</p><p>选择信号线（不是标签），并使用 <strong>F2</strong>。</p><p>在 <span>Mac</span> 上，选择信号线（而不是标签）并使用 <strong>control+return</strong> 键。</p></td></tr><tr><td>移动信号标签</td><td>将标签拖到同一信号线上的新位置。</td></tr><tr><td>复制信号标签</td><td><strong>Ctrl+</strong> 拖动信号标签。</td></tr><tr><td>更改标签字体</td><td>选择信号线（不是标签），然后在<strong>格式</strong>选项卡上，点击<strong>字体属性</strong>按钮箭头，然后点击<strong>模型字体</strong>。</td></tr></tbody></table>

### 信号显示选项

在模型图中显示信号属性可以提高模型的易读性。例如，在 Simulink 编辑器中的**调试**选项卡上，使用菜单在模型布局中包含有关信号属性的信息，例如：

有关详细信息，请参阅[显示信号属性](https://ww2.mathworks.cn/help/simulink/ug/displaying-signal-properties.html)。

您还可以突出显示信号及其源或目标模块。有关详细信息，请参阅[突出显示信号的源和目标](https://ww2.mathworks.cn/help/simulink/ug/displaying-signal-sources-and-destinations.html)。

### 存储信号和状态的设计属性

可以使用模块参数和信号属性指定信号的设计属性，如数据类型、最小值和最大值、物理单位以及数值的复 / 实性。要配置状态，可以使用模块参数。当您使用这些模块参数和信号属性时，可将这些设定存储在模型文件中。

也可以使用存储在工作区或数据字典中的 [`Simulink.Signal`](https://ww2.mathworks.cn/help/simulink/slref/simulink.signal.html) 或 [`Simulink.ValueType`](https://ww2.mathworks.cn/help/simulink/slref/simulink.valuetype.html) 对象的属性来指定这些特性。

请根据您的建模目的选择要采用哪种策略。

<table><colgroup><col width="50%"><col width="50%"></colgroup><thead><tr><th>建模目的</th><th>策略</th></tr></thead><tbody><tr><td><p>提高模型的可移植性、可读性和易维护性</p></td><td><p>将信号属性设定存储在模型文件中。您不需要保存和管理外部对象。可以考虑将模型配置参数<strong>信号解析</strong>设置为 “<code>无</code>”，以禁止模型使用 <code>Simulink.Signal</code> 对象。</p></td></tr><tr><td><p>将信号属性设定与模型分开，以便您可以独立管理每个信号</p></td><td><p>使用 <code>Simulink.Signal</code> 对象。</p></td></tr><tr><td><p>将信号属性设定与模型分开，以便您可以重用特定于应用程序的属性集</p></td><td><p>使用 <code>Simulink.Valuetype</code> 对象。</p></td></tr></tbody></table>

要使用可排序、分组和筛选的列表为信号配置设计属性和代码生成设置，请使用**[模型数据编辑器](https://ww2.mathworks.cn/help/simulink/slref/modeldataeditor.html)**。对于对象，您也可以使用**[模型资源管理器](https://ww2.mathworks.cn/help/simulink/slref/modelexplorer.html)**。

要确定持久性存储 `Simulink.Signal` 或 `Simulink.ValueType` 对象的位置，请参阅[确定在何处存储 Simulink 模型的变量和对象](https://ww2.mathworks.cn/help/simulink/ug/determine-where-to-store-data-for-simulink-models.html)。

### 测试信号

您可以对信号执行以下类型的测试：

*   最小和最大值 - 对于许多 Simulink 模块来说，您可以为输出信号指定有效值范围。Simulink 提供了一项诊断，可对仿真过程模块所生成信号超出指定范围的情况进行检测。有关详细信息，请参阅[指定信号范围](https://ww2.mathworks.cn/help/simulink/ug/signal-ranges.html)。
    
*   连接验证 - 许多 Simulink 模块对于能够接受的信号类型有限制。对模型进行仿真之前，Simulink 会检查所有模块，确保模块可以接受连接的端口所输出的信号类型，并能够在出现不兼容问题时报告错误。要在运行仿真之前检测信号兼容性错误，请更新图。
    

[Signal Editor](https://ww2.mathworks.cn/help/simulink/slref/signaleditorblock.html) 模块显示可互换的方案组。使用 Signal Editor 可显示、创建、编辑和切换可互换方案。

方案可以帮助测试模型。

## 相关主题

*   [控制信号的数据类型](https://ww2.mathworks.cn/help/simulink/ug/control-signal-data-types.html)
*   [信号标签传播](https://ww2.mathworks.cn/help/simulink/ug/signal-label-propagation.html)
*   [Simulink 建模的键盘快捷方式和鼠标操作](https://ww2.mathworks.cn/help/simulink/ug/summary-of-mouse-and-keyboard-actions.html)
*   [Merging Signals](https://ww2.mathworks.cn/help/simulink/slref/merging-signals.html)

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