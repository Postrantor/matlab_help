---
url: https://ww2.mathworks.cn/help/simulink/ug/create-edit-and-manage-workspace-variables.html
title: 创建、编辑和管理工作区变量
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-18 20:59:05
tag: 
summary: 工作区变量使您能够在模块和模型之间共享信息，例如参数值和数据类型。使用不同工具和方法来创建和操作工作区变量。
---
## 创建、编辑和管理工作区变量

要在各模块和模型之间共享参数值和信号数据类型等信息，请使用工作区变量。例如，可以在基础工作区中创建 MATLAB® 数值变量，并使用该变量同时在多个 Gain 模块中设置**增益**参数的值（请参阅[通过创建变量来共享和重用模块参数值](https://ww2.mathworks.cn/help/simulink/ug/share-and-reuse-block-parameter-values-by-creating-variables.html)）。您可以创建一个 `Simulink.Bus` 对象来显式定义总线信号的结构体。

您可以将工作区变量存储在基础工作区、模型工作区或数据字典中。要决定存储变量的位置，请参阅[确定在何处存储 Simulink 模型的变量和对象](https://ww2.mathworks.cn/help/simulink/ug/determine-where-to-store-data-for-simulink-models.html)。

### 变量管理工具

使用以下一种或多种方法来创建、修改、存储、计算和迁移工作区变量：

*   要共享模块参数值并创建 `Simulink.Parameter` 和 `Simulink.Signal` 对象（例如，准备代码生成），可以使用模型数据编辑器。您可以一次性与模型中的所有模块参数、信号线和模块状态交互。您还可以检查列表中的可调参数模块，并对其进行搜索、排序和过滤。
    
    *   要创建一个变量，请在数据表中开始编辑与模块参数值（在**值**列中）对应的单元格或信号或状态名称（在**名称**列中）。输入要创建的变量的名称，然后点击单元格右侧的操作按钮
        
        ![](https://ww2.mathworks.cn/help/simulink/ug/prop_action_ctrl.png)
        
        。
        
        如果模块参数值已设置为简单的数值表达式，则可以为该表达式创建变量。点击对应于该值的单元格右侧的
        
        ![](https://ww2.mathworks.cn/help/simulink/ug/prop_action_ctrl.png)
        
        ，然后选择。在 “创建新数据” 对话框中，设置新变量的名称和位置，然后点击**创建**。单元格现在显示新变量。
        
    *   要使用数据表中的列修改变量，请点击**显示 / 刷新其他信息**按钮。然后，数据表将包含与模型使用的变量和对象对应的行。
        
    *   要一次与一个变量交互（例如，一次检查变量的所有属性），请打开属性检查器（在**建模**选项卡上，在**设计**下点击**属性检查器**）并选择数据表中的相关行。属性检查器显示所选变量的属性。
        
    
    有关详细信息，请参阅**[模型数据编辑器](https://ww2.mathworks.cn/help/simulink/slref/modeldataeditor.html)**。
    
*   要一次与少量参数、信号或状态进行交互，请使用单个模块参数对话框或属性检查器创建用于共享模块参数值的变量，并为代码生成创建和配置参数与信号对象。
    
    在对话框或属性检查器中，点击模块参数值、信号名称或状态名称旁边的操作按钮
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/prop_action_ctrl.png)
    
    。
    
*   要创建和编辑变量或对象的任何类型或类，在各工作区之间移动变量，以及一次性检查工作区中的所有变量，请使用模型资源管理器。您还可以重命名变量并精确分析整个模型或单个模块使用变量的方式。请参阅**[模型资源管理器](https://ww2.mathworks.cn/help/simulink/slref/modelexplorer.html)**和 [使用模型资源管理器编辑和管理工作区变量](https://ww2.mathworks.cn/help/simulink/ug/workspace-variables-in-model-explorer.html)。
    
*   要检查在仿真期间设置为变量或变量表达式的参数值，请打开 “模块参数” 对话框或属性检查器。输入参数值的文本框在左侧显示变量或表达式，在右侧显示值。当您打开 “模块参数” 对话框或属性检查器时，显示的值是仿真中在该时间步的变量或表达式的值。有关详细信息，请参阅 [View Values of Parameters Set as Variables](https://ww2.mathworks.cn/help/simulink/ug/view-variable-values.html)。
    

### 从模块参数编辑变量值或属性

此示例说明如何更改其值由数值变量设置的**增益**参数的值（Gain 模块）。修改变量，而不是模块参数。

1.  打开模型 `f14`。该模型将变量加载到基础工作区中。
    
2.  在模型中，打开属性检查器。在**建模**选项卡上，在**设计**下，点击**属性检查器**。
    
3.  在模型中，选择使用变量 `Mw` 的 Gain 模块。
    
4.  在属性检查器中，点击**增益**参数值旁边的按钮
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/prop_action_ctrl.png)
    
    。选择**打开**。
    
5.  在**数据属性**对话框中，在**值**框中为该变量键入新值，然后点击**确定**。
    

### 以交互方式修改结构体和数组变量

要检查和修改值为结构体或数组的变量，可以点击其旁边的按钮

![](https://ww2.mathworks.cn/help/simulink/ug/prop_action_ctrl.png)

启动变量编辑器。选择以下方法之一：

*   在模型资源管理器中，在**目录**窗格中选择变量。在 “对话框” 窗格（右窗格）中，会出现按钮。
    
*   在模型数据编辑器（在**建模**选项卡上，点击**模型数据编辑器**）中，在**参数**选项卡上，点击**显示 / 刷新其他信息**按钮。在数据表中，找到对应于该变量的行，并在**值**列中开始编辑该变量的值。按钮出现在单元格的右侧。
    
*   在模块对话框或属性检查器中，该按钮出现在使用该变量的模块参数的值旁边。点击该按钮并使用菜单选项打开该变量的属性对话框。然后，在属性对话框中，再次点击该按钮以启动变量编辑器。您只能将此方法用于 `Simulink.Parameter` 等参数对象。
    

### 修改或删除变量的影响

修改或删除变量时，所做的更改可能会影响使用该变量的多个模块和模型。要通过确定变量的使用位置来评估影响，请使用模型资源管理器（请参阅[分析模型中的变量使用情况](https://ww2.mathworks.cn/help/simulink/ug/create-edit-and-manage-workspace-variables.html#mw_f572096f-38ec-4d0b-87c2-100fa05ddeed)）。但是，您只能分析在分析时打开的模型的变量使用情况。在执行分析之前，请打开您认为可能使用该变量的任何模型。

模型和模块通过名称解析使用变量（请参阅[符号解析](https://ww2.mathworks.cn/help/simulink/ug/resolving-symbols.html)）。当您更改变量的名称而未对相关模块和模型进行相应更改时，这些模块和模型会生成错误。在这种情况下，需要在一个或多个模型的上下文中重命名变量，请参阅[在整个模型中重命名变量](https://ww2.mathworks.cn/help/simulink/ug/create-edit-and-manage-workspace-variables.html#mw_93e3e476-dd47-4fcd-bdc7-ae42dcda0b00)。

当模块或模型不能访问它需要的变量时，它会在诊断查看器中生成错误。在某些情况下，您可以使用诊断查看器中的按钮来修复错误（例如，通过还原已删除的变量）。要增加使用诊断查看器恢复缺失变量的可能性，请使用下列方法：

*   将变量存储在数据字典中而不是基础工作区中。使用数据字典，这样可以获得更多恢复选项。有关数据字典的信息，请参阅[什么是数据字典？](https://ww2.mathworks.cn/help/simulink/ug/what-is-a-data-dictionary.html)。
    

*   对于每个模型，请使对应的 Simulink® 缓存文件可用。例如，当您与其他人共享模型时，也要共享该缓存文件。当您从源代码管理系统中提取最新模型设计文件时，请从持续集成系统或最新编译文件夹中提取缓存文件。该缓存文件保留 Simulink Coder™ 可用于帮助您恢复缺失的变量的信息。有关 Simulink 缓存文件的详细信息，请参阅[共享 Simulink 缓存文件以加快仿真速度](https://ww2.mathworks.cn/help/simulink/ug/reuse-simulation-builds-for-faster-simulations.html)。
    

### 分析模型中的变量使用情况

要分析模型使用变量的方式，请使用模型资源管理器。您可以：

*   确定变量在模型中的使用位置。
    
*   确定模型是否使用变量。
    
*   确定工作区中的哪些变量未被模型使用。
    

有关详细信息，请参阅[使用模型资源管理器编辑和管理工作区变量](https://ww2.mathworks.cn/help/simulink/ug/workspace-variables-in-model-explorer.html)。

### 在整个模型中重命名变量

以下示例说明如何在模型数据编辑器中重命名变量。

1.  打开模型 `f14`。该模型将变量加载到基础工作区中。
    
2.  在模型中，在**建模**选项卡上，点击**模型数据编辑器**。在模型数据编辑器中，检查**参数**选项卡。
    
3.  在模型中，点击标签为 `Mw` 的 Gain 模块。
    
    在模型数据编辑器中，**值**列显示该模块使用变量 `Mw`。假设您要重命名此变量。
    
4.  在模型数据编辑器中，点击**显示 / 刷新其他信息**按钮。
    
    现在，数据表包含与模型使用的工作区变量对应的行。
    
5.  激活**更改范围**按钮。
    
    现在，数据表显示有关子系统中数据项的信息。
    
6.  在**过滤内容**框中，输入 `Mw`。
    
    数据表显示对应于该变量的行，以及对应于使用该变量的模块的行。
    
7.  在表示 `Mw` 的行中，右键点击并选择**全部重命名**。
    
8.  在**选择系统**对话框中，点击模型 `f14` 的名称，以选择它作为重命名变量 `Mw` 的上下文。
    
9.  清除**在引用模型中搜索**复选框，因为 `f14` 没有引用任何模型，然后点击**确定**。
    
    选中**在引用模型中搜索**之后，您可以重命名在模型引用层次结构中任意位置使用的目标变量。但是，在整个层次结构中重命名目标变量可能需要更长时间。
    
    默认情况下，**更新图以包括最近的更改**复选框处于清除状态，通过避免不必要的模型图更新来节省时间。选中该复选框将强制模型图更新以包含您最近对模型所做的更改。
    
10.  在**全部重命名**对话框中，在**新名称**框中键入变量的新名称，然后点击**确定**。
    
11.  再次点击**显示 / 刷新其他信息**。由于重命名操作更改了变量的名称和某些模块参数的值，因此要在模型数据编辑器中获取更准确的信息，必须点击**显示 / 刷新其他信息**。
    

### 以编程方式与变量进行交互

在命令提示符下，您可以通过输入 `myVar = 15;` 等命令在基础工作区中创建和修改变量。要以编程方式在其他工作区（例如模型工作区）中创建、修改和存储变量，请使用目标工作区的编程接口。下表显示了可用于以编程方式管理变量的接口和方法。

要以编程方式列出模型使用或不使用的变量，请参阅 [`Simulink.findVars`](https://ww2.mathworks.cn/help/simulink/slref/simulink.findvars.html)。

要以编程方式访问变量以扫描模块参数值，请考虑使用 `Simulink.SimulationInput` 对象，而不是通过编程工作区接口修改变量。请参阅[优化、估计和扫描模块参数值](https://ww2.mathworks.cn/help/simulink/ug/optimize-estimate-and-sweep-block-parameter-values.html)。

## 相关主题

*   [确定在何处存储 Simulink 模型的变量和对象](https://ww2.mathworks.cn/help/simulink/ug/determine-where-to-store-data-for-simulink-models.html)
*   [Partition Data for Model Reference Hierarchy Using Data Dictionaries](https://ww2.mathworks.cn/help/simulink/ug/compose-dictionary-hierarchy.html)
*   [数据对象](https://ww2.mathworks.cn/help/simulink/ug/working-with-data-objects.html)

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