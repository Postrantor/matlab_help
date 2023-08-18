---
url: https://ww2.mathworks.cn/help/simulink/ug/determine-where-to-store-data-for-simulink-models.html
title: 确定在何处存储 Simulink 模型的变量和对象
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-18 20:59:01
tag: 
summary: 模型数据是指您在工作区（例如，基础工作区或数据字典）中创建的对象和变量。选择一种永久存储数据的方式。
---
## 确定在何处存储 Simulink 模型的变量和对象

_模型数据_是指您在工作区（例如，基础工作区或数据字典）中创建的对象和变量。模型数据包括：

*   模块参数（如 `Simulink.Parameter` 对象和 MATLAB® 变量）的数值
    
*   信号，如 `Simulink.Signal` 对象
    
*   由 `Simulink.ValueType` 对象定义的值类型
    
*   数据类型
    
*   模型配置集
    
*   仿真输入和输出数据
    

您可以在适合自己设计的位置存储、分区和共享模型数据。您选择的存储位置可以取决于：

*   您的建模目的。
    
*   模型架构（引用模型、子系统以及其他分区策略）和组件结构。
    
*   您使用的数据类型。
    

### 数据类型

*   _仿真数据_是指您用来驱动仿真的一组输入数据以及仿真生成的一组输出数据。例如，您可以使用变量来存储仿真通过 Inport 模块获取的输入数据。仿真可以通过一些途径（例如 Outport 模块、To Workspace 模块和记录的信号）导出输出数据。
    
    您可以将当前 MATLAB 会话的仿真数据存储到基础工作区中。要永久存储此仿真数据，请将其保存到 MAT 文件或脚本文件中。有关加载、生成和存储仿真数据的详细信息，请参阅 [Comparison of Signal Loading Techniques](https://ww2.mathworks.cn/help/simulink/ug/comparison-of-signal-loading-techniques.html) 和[导出仿真数据](https://ww2.mathworks.cn/help/simulink/ug/export-simulation-data-1.html)。
    
*   _设计数据_是指您用来指定模型中的模块参数和信号特性的一组变量。例如，设计数据包括 MATLAB 数值变量、值类型对象、参数和信号数据对象、数据类型对象和总线对象。
    
    您可以将设计数据存储在基础工作区、模型工作区或数据字典的 “设计数据” 部分。要永久存储模型的本地设计数据，请使用模型工作区。要在模型之间共享设计数据，请使用数据字典或基础工作区。数据字典永久存储数据，您可以控制数据范围以确定所有权、对数据进行分区以提高易读性并简化维护工作，还可以跟踪更改。如果您使用基础工作区，要永久存储数据，则必须将数据保存到 MAT 文件或脚本文件中。
    
*   _配置集_是指一组模型配置参数。默认情况下，配置集位于模型文件中，因此您不需要将配置集与模型分开存储。但是，您不能与其他模型共享配置集。
    
    要在模型之间共享配置集，必须创建 `Simulink.ConfigSet` 对象。每个对象代表一个独立的配置集。您可以将这些对象存储在基础工作区或数据字典的 Configurations 分区。如果使用数据字典，您可以定义每个配置集的范围、比较配置集的不同之处，还可以跟踪更改。数据字典本身会将配置集与其他类型的数据放入不同的分区中。
    

### 存储设计数据

下表显示了存储、分区以及管理设计数据和配置集时可以选择的方式。

### 存储位置

选择以下任一位置来存储数据：

*   MATLAB 基础工作区。当您使用临时模型进行实验时，可使用基础工作区存储变量。
    
*   模型工作区。使用模型工作区永久存储模型的局部数据。
    
*   数据字典。使用数据字典永久存储全局数据、在模型之间共享数据，并跟踪对数据所做的更改。
    

下图显示了每个存储位置的功能和优势。

<table><colgroup><col width="50%"><col width="18%"><col width="17%"><col width="14%"></colgroup><thead><tr><th>功能</th><th>基础工作区</th><th>模型工作区</th><th>数据字典</th></tr></thead><tbody><tr><td>数据 - 模型链接</td><td>隐式</td><td>隐式</td><td>✓</td></tr><tr><td>统一的数据定义界面</td><td>✓</td><td>✓</td><td>✓</td></tr><tr><td>模型 - 数据依存关系</td><td>✓</td><td>✓</td><td>✓</td></tr><tr><td>数据条目比较</td><td>✓</td><td>✓</td><td>✓</td></tr><tr><td>数据条目持久性</td><td>&nbsp;</td><td>✓</td><td>✓</td></tr><tr><td>修复缺失变量的选项</td><td>✓</td><td>✓</td><td>其他选项</td></tr><tr><td>共享数据</td><td>✓</td><td>&nbsp;</td><td>✓</td></tr><tr><td>数据分组</td><td>&nbsp;</td><td>&nbsp;</td><td>✓</td></tr><tr><td>数据条目的更改跟踪</td><td>&nbsp;</td><td>&nbsp;</td><td>✓</td></tr><tr><td>配置集的更改跟踪</td><td>&nbsp;</td><td>&nbsp;</td><td>✓</td></tr><tr><td>数据条目的合并与协调</td><td>&nbsp;</td><td>&nbsp;</td><td>✓</td></tr><tr><td>辅助数据的存储和分区</td><td>&nbsp;</td><td>&nbsp;</td><td>✓</td></tr><tr><td>需求链接</td><td>&nbsp;</td><td>&nbsp;</td><td>✓</td></tr></tbody></table>

有关模型如何与工作区和工作区变量进行交互的信息，请参阅[符号解析](https://ww2.mathworks.cn/help/simulink/ug/resolving-symbols.html)。

#### 临时数据：基础工作区

在以下情况下，请使用基础工作区临时存储数据：

*   当您学习使用 Simulink 时
    
*   当您使用建模技术进行实验需要快速建模变量时
    
*   当您不需要永久存储数据时
    

要在基础工作区中创建变量，您可以使用 MATLAB 命令提示符或模型资源管理器。所有打开的模型都可以使用您在基础工作区中创建的数据。

如果您使用变量来指定模型中的数值模块参数，您可以在仿真过程中通过在命令提示符下使用命令以编程方式更改参数值。要以编程方式更改您存储在模型工作区或数据字典中的参数值，您必须使用这些存储位置的函数接口。

要在结束 MATLAB 会话之前永久存储基础工作区数据，您可以将数据保存到 MAT 文件或脚本文件中。在以后的会话中，您可以从该文件中加载数据。但是，如果您对基础工作区中的数据进行了更改，则必须重新将数据保存到该文件中。可以考虑改用模型工作区或数据字典来永久存储数据。

#### 局部数据：模型工作区

使用模型工作区存储仅在关联模型中使用的数据。这些数据可以包括：

*   常量参数，例如，您用来指定模块参数值的数值变量。
    
*   您用来控制信号和参数特征的数据对象，例如 `Simulink.Signal` 和 `Simulink.Parameter` 对象。但是，模型工作区中的信号对象只能使用 `Auto` 存储类。如果您将 `AUTOSAR.Parameter` 对象存储在模型工作区中，代码生成器将忽略您为该对象指定的存储类。
    
*   您用来指定数据类型的 `Simulink.NumericType` 对象。但是，您不能使用该对象作为数据类型别名。您必须将 `IsAlias` 属性设置为 `false`。
    
*   模型参数。
    

通过将数据存储在模型工作区中，可以提高模型的可移植性并确定数据的所有权。在这种情况下，模型文件永久存储数据。

在模型引用层次结构中，每个模型工作区相当于一个唯一的命名空间。因此，您可以在多个模型工作区中使用同一个变量名称。然后，您可以为每个模型指定一个唯一的变量值。

您可以使用模型资源管理器来操作模型工作区数据。也可以将命令提示符或脚本与模型工作区编程接口结合使用。

有关使用模型工作区存储局部数据的详细信息，请参阅[模型工作区](https://ww2.mathworks.cn/help/simulink/ug/using-model-workspaces.html)。

#### 全局数据和共享数据：数据字典

数据字典是一个独立的文件，用于永久存储数据。可以使用数据字典代替基础工作区来分区数据、跟踪更改、控制访问和共享数据。如果将模型链接到数据字典，仍可以通过配置从模型或字典的访问来使用基础工作区中的变量。

就像模型工作区一样，您可以使用数据字典直接将数据与模型关联。您可以利用这种关联确定数据的范围和所有权。

如果您使用字典，则可以通过将数据存储在其他引用字典中对数据进行分区。但是，字典中的每个条目必须使用一个唯一的名称。您必须将每个字典作为一个单独的文件来管理。

可以使用数据字典存储多个模型或系统组件共享的数据。这些数据可以包括：

*   多个模型用来指定模块参数值的数值变量。
    
*   您用来一次为多个模型指定数据类型的 `Simulink.AliasType` 和 `Simulink.NumericType` 对象。
    
*   数据对象，包括使用 `Auto` 之外的存储类的信号对象（例如 `Simulink.Signal`）。如果您拥有 Simulink Coder™ 许可证，则这些对象可以代表在生成的代码中显示为全局变量的信号和可调参数。
    
*   `Simulink.ValueType` 和 `Simulink.Bus` 对象，用于定义模型组件（如引用模型）的接口。
    
*   您用来在多个模型之间维护配置参数一致性的 `Simulink.ConfigSet` 对象。
    
*   您使用 `Simulink.data.dictionary.EnumTypeDefinition` 对象存储的枚举类型定义。
    

您可以使用模型资源管理器来操作字典数据。也可以将命令提示符或脚本与数据字典编程接口结合使用。

有关数据字典的基本信息，请参阅[什么是数据字典？](https://ww2.mathworks.cn/help/simulink/ug/what-is-a-data-dictionary.html)。

#### 代码生成的注意事项

如果您打算从模型生成 C 代码 (Simulink Coder)，请考虑下列注意事项。

*   如果将 `Auto` 以外的存储类应用于信号对象（例如 `Simulink.Signal`）来控制生成代码中的信号或模块状态的出现，则不能将对象存储在模型工作区中，而应将对象存储在基础工作区或数据字典中。有关信号和状态的存储类的详细信息，请参阅[模型接口元素的 C 代码生成配置](https://ww2.mathworks.cn/help/rtw/ug/c-code-generation-configuration-for-model-interface-elements.html) (Simulink Coder)。
    
*   如果将 `Auto` 以外的存储类应用于参数对象（例如 `Simulink.Parameter`），则可以将对象存储在基础工作区、模型工作区或数据字典中。但是，如果将对象存储在模型工作区中，则代码生成器将假定此模型拥有该参数。有关详细信息，请参阅[存储位置对参数对象的代码生成的影响](https://ww2.mathworks.cn/help/rtw/ug/use-parameter-objects-for-code-generation.html#mw_5f4f922f-63c2-4a3b-a1de-42aa16794eea) (Simulink Coder)。
    

## 相关主题

*   [使用模型资源管理器编辑和管理工作区变量](https://ww2.mathworks.cn/help/simulink/ug/workspace-variables-in-model-explorer.html)
*   [创建、编辑和管理工作区变量](https://ww2.mathworks.cn/help/simulink/ug/create-edit-and-manage-workspace-variables.html)
*   [比较模型组件的功能](https://ww2.mathworks.cn/help/simulink/ug/model-architecture-guidelines.html)
*   [模型工作区](https://ww2.mathworks.cn/help/simulink/ug/using-model-workspaces.html)
*   [数据对象](https://ww2.mathworks.cn/help/simulink/ug/working-with-data-objects.html)
*   [符号解析](https://ww2.mathworks.cn/help/simulink/ug/resolving-symbols.html)

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