---
url: https://ww2.mathworks.cn/help/simulink/ug/parameterize-referenced-models.html
title: 参数化可重用引用模型的实例
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-18 21:00:51
tag: 
summary: 在将可重用组件建模为引用模型时，要将组件的每个实例配置为使用不同模块参数值，请创建模型参数。
---
## 参数化可重用引用模型的实例

当您使用多个 Model 模块引用同一模型时，您可以将模块参数配置为对模型的每个实例使用相同值或不同值。例如，您可以配置 Gain 模块的**增益**参数。要使用不同值，请创建并使用模型参数来设置模块参数的值。对于某些应用，仅当您可以将每个实例配置为对模块参数使用不同值（例如控制器的设定点或滤波器系数）时，才能重用引用模型。

### 为可重用模型的每个实例指定不同值

对于可重用引用模型中的模块参数，要为模型的每个实例指定不同值，请执行以下操作：

1.  在引用模型的模型工作区中创建一个 MATLAB® 变量或 `Simulink.Parameter` 对象。
    
    *   使用 MATLAB 变量可以方便维护。
        
    *   使用 `Simulink.Parameter` 对象可以更好地控制模型参数的最小值和最大值、数据类型和其他属性。
        
    
2.  使用变量或参数对象设置模块参数值。（可选）使用相同的变量或对象来设置其他模块参数值。
    
3.  通过选择**参数**属性，将变量或对象配置为模型参数。
    
4.  为变量或 `Simulink.Parameter` 对象指定默认值。在您直接仿真此模型时，模块参数使用变量或对象存储在模型工作区中的值。当将此模型作为引用模型进行仿真时，配置为模型参数的形参从其父模型获得其值。
    
    您也可以将 `Simulink.Parameter` 对象的值留空。请参阅[定义模型参数而不指定默认值](https://ww2.mathworks.cn/help/simulink/ug/parameterize-referenced-models.html#mw_8d1158c6-eaed-4d8b-9a34-524caf181f8d)。
    
5.  在引用可重用模型的每个 Model 模块中，为模块参数指定特定于实例的值。特定于实例的值的维度和数据类型必须与模型参数定义的对应项匹配。如果不指定值，该参数将使用模型层次结构中在它下方指定的最后一个值。在顶层模型中，您可以将诊断配置参数**[模型参数没有显式最终值](https://ww2.mathworks.cn/help/simulink/gui/noexplicitfinalvalueformodelarguments.html)**配置为在可设置模型参数值的最顶层 Model 模块使用此默认值（而不是提供显式值）时生成错误或警告。
    
6.  在中间模型中，除了为模块参数指定特定于实例的值之外，您还可以指定是否可以在层次结构的下一级覆盖该参数。
    

### 定义模型参数而不指定默认值

如果使用 `Simulink.Parameter` 对象来设置模块参数值，则只要在模型引用层次结构父级的某个位置提供显式值，就可以在对象的值保留为空（**值**设置为 '`[]`'）的情况下将模型作为引用模型进行编译和仿真。在这种情况下，您无法直接仿真模型。当该值为空时，您必须为该对象提供**数据类型**和**维度**。您还可以指定**最小值**、**最大值**和**复 / 实性**属性。虽然您指定了空值，但 Simulink® 仍会使用您指定的属性合成一个**值**（请参阅 [`Simulink.Parameter`](https://ww2.mathworks.cn/help/simulink/slref/simulink.parameter.html)）。

### 将多个参数组合成一个结构体

当您将一个模型配置为使用多个模型参数时，请考虑在模型工作区中使用结构体而不是单独的变量。当您要添加、重命名或删除参数时，此方法能减少维护工作。您可以使用变量编辑器或命令提示符修改结构体，而不是手动将模型工作区中的参数与 Model 模块中的参数值同步。

如果您拥有 Simulink Coder™ 许可证，此方法还可以减少引用模型函数（如输出 (`step`) 函数）的形参消耗的 ROM。

要创建和使用结构体来设置模块参数值，请参阅[在结构体中组织相关的模块参数定义](https://ww2.mathworks.cn/help/simulink/ug/using-structure-parameters.html)。

### 参数化引用模型

此示例说明如何以交互方式配置一个引用模型的多个实例，以对同一模块参数使用不同值。有关仅使用命令提示符参数化引用模型的示例，请参阅 [Parameterize a Referenced Model Programmatically](https://ww2.mathworks.cn/help/simulink/ug/parameterize-referenced-models-example.html)。有关涉及代码生成的示例，请参阅 [Specify Instance-Specific Parameter Values for Reusable Referenced Model](https://ww2.mathworks.cn/help/rtw/ug/specify-instance-specific-parameter-values-for-reusable-referenced-models.html) (Simulink Coder)。

#### 将引用模型配置为使用模型参数

要为引用模型配置模型参数，必须在模型工作区中创建 MATLAB 变量或 `Simulink.Parameter` 对象。此示例将 `Simulink.Parameter` 对象配置为模型参数，不在对象中存储默认值，而是依赖父模型引用层次结构来提供显式值。没有默认值，模型无法直接仿真，必须作为引用模型进行仿真。

创建包含 Gain 模块和 Discrete Filter 模块的模型 `ex_model_arg_ref`。

![](https://ww2.mathworks.cn/help/simulink/ug/parameterizeareferencedmodelexample_01.png)

要将 Gain 模块的**增益**参数和 Discrete Filter 模块的**分子**参数配置为模型参数，请执行以下步骤：

1.  在模型中，在**建模**选项卡上，点击**模型数据编辑器**。
    
2.  在模型数据编辑器中，选择**参数**选项卡。
    
3.  使用**值**列将**增益**参数的值设置为变量，例如 `gainArg`。
    
4.  在 `gainArg` 的旁边，点击操作按钮
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/prop_action_ctrl.png)
    
    并选择。
    
5.  在 “创建新数据” 对话框中，将**值**设置为 `Simulink.Parameter`，将**位置**设置为 “`模型工作区`”。点击**创建**。
    
6.  在 `Simulink.Parameter` 属性对话框中，将**数据类型**设置为 “`double`” 且**维度**设置为 `1`。将该值留空（**值**设置为 '`[]`'）。为对象指定**最小值**、**最大值**、**复 / 实性**值是可选的。
    
7.  要将对象配置为模型参数，请选择**参数**。
    
8.  点击**确定**。
    
9.  对 Discrete Filter 模块的**分子**参数重复步骤 4 至 9。在本例中，创建一个名为 `coeffArg` 的 `Simulink.Parameter` 对象。
    

请记住，如果没有为参数对象设置值，模型本身无法成功编译。

#### 在父模型中设置模型参数值

在仿真父模型时，可重用引用模型的每个实例都使用您在父模型中指定的参数值。此示例说明如何在模型层次结构的每层上将模型参数作为可调参数在 Model 模块上显示。

创建一个模型 `ex_model_arg`，该模型使用前面示例中可重用模型 `ex_model_arg_ref` 的多个实例。

![](https://ww2.mathworks.cn/help/simulink/ug/parameterizeareferencedmodelexample_02.png)

要为模型引用层次结构中的模型参数设置特定于实例的值，请执行以下操作：

1.  在模型中，在**建模**选项卡上，点击**模型数据编辑器**。
    
2.  在模型数据编辑器中，选择**参数**选项卡。模型数据编辑器显示四行，它们对应于您可以为两个 Model 模块指定的特定于实例的参数。
    
3.  使用模型数据编辑器设置 `Model` 和 `Model1` 中参数的值。默认情况下，模型参数使用模型层次结构中在它下方指定的最后一个值（由值 `<from below>` 指示）。用下图中的值替换默认值。
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/xxmodel_blk_1.png)
    
4.  要在模型层次结构的下一级覆盖这些参数的值，请选中**参数**列中的复选框。默认情况下，该复选框未选中。
    
    您还可以在每个 Model 模块中配置特定于实例的参数。在模块对话框中，选择**实例参数**选项卡。
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/model_block_inst_params.png)
    
    **提示**
    
    当在 [求解器窗格](https://ww2.mathworks.cn/help/simulink/gui/solver-pane.html) 中启用 “`引用模型时使用局部求解器`” 时，不支持模型实例参数。
    
5.  创建模型 `ex_model_arg_top`，该模型包含引用 `ex_model_arg` 的 Model 模块。
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/ex_model_arg_top.png)
    
6.  在模型数据编辑器中，点击**显示 / 刷新其他信息**按钮。在**参数**选项卡上，您可以看到引用模型中作为可调参数显示的每个特定于实例参数。在此处，您可以为模型层次结构中 `coeffArg` 和 `gainArg` 参数的所有实例创建一个参数值集。
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/model_top_inst_params.png)
    
    默认情况下，每个实例都使用模型层次结构中在它下方指定的最后一个值。在本例中，模型数据编辑器显示 `<from_below>`。如果您选中**参数**复选框以向父模型公开参数，**值**显示为 `<inherited>`，表示运行时值现在来自该父模型。
    
    更新模型图后，编辑器还会显示实例的编译值。要导航到此默认值，请点击编译值旁边具有三个纵点的按钮，然后选择 “`从下面导航到默认值`”。最后指定值的引用模型在模型画布的新选项卡中打开，模型数据编辑器突出显示包含模块参数的行。
    

#### 将多个模型参数组合为单个结构体

当您要添加、重命名或删除参数时，可以使用结构体来减少维护工作量。使用结构体时，模型的数学功能是相同的。

要用 `ex_model_arg_ref` 和 `ex_model_arg` 的结构体替换参数值，请执行以下步骤：

1.  在命令提示符下，创建一个结构体。为 `ex_model_arg_ref` 工作区中的每个参数对象添加一个字段。
    
    ```
    structForInst1.gain = 3.17;
    structForInst1.coeff = 1.05;
    
    
    ```
    
2.  将该结构体存储在一个 `Simulink.Parameter` 对象中。
    
    ```
    structForInst1 = Simulink.Parameter(structForInst1);
    
    
    ```
    
3.  打开模型资源管理器。在引用模型 `ex_model_arg_ref` 中，在**建模**选项卡上，点击**模型资源管理器**。
    
4.  使用模型资源管理器将参数对象从基础工作区复制到 `ex_model_arg_ref` 模型工作区中。
    
5.  在模型工作区中，将 `structForInst1` 重命名为 `structArg`。
    
6.  在**目录**窗格中，将 `structArg` 配置为唯一的模型参数。
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/xxstructarg.png)
    
7.  在 `ex_model_arg_ref` 模型中，在模型数据编辑器的**参数**选项卡中，将**增益**参数的值设置为 `structArg.gain` 并将**分子**参数的值设置为 `structArg.coeff`。
    
8.  保存模型。
    
9.  在命令提示符下，将基础工作区中的现有结构体复制为 `structForInst2`。
    
    ```
    structForInst2 = copy(structForInst1);
    
    
    ```
    
10.  使用与在 Model 模块中设置模型参数值相同的数值来设置两个结构体中的字段值。
    
    ```
    structForInst1.Value.gain = 2.98;
    structForInst1.Value.coeff = 0.98;
    
    structForInst2.Value.gain = 3.34;
    structForInst2.Value.coeff = 1.11;
    
    
    ```
    
11.  在顶层模型 `ex_model_arg` 中，使用模型数据编辑器设置参数值，如下图所示。
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/xxmodel_blk_3.png)
    

#### 使用总线对象作为结构体的数据类型

您可以使用 `Simulink.Bus` 对象作为结构体的数据类型。该对象可确保特定于实例的结构体的特征（例如字段的名称和顺序）与模型工作区中结构体的特征相匹配。

1.  在命令提示符下，使用函数 `Simulink.Bus.createObject` 创建一个 `Simulink.Bus` 对象。对象中元素的层次结构与结构体字段的层次结构相匹配。对象的默认名称是 `slBus1`。
    
    ```
    Simulink.Bus.createObject(structForInst1.Value);
    
    
    ```
    
2.  通过复制总线对象，将其重命名为 `myParamStructType`。
    
    ```
    myParamStructType = copy(slBus1);
    
    
    ```
    
3.  在 `ex_model_arg` 的模型数据编辑器中，点击**显示 / 刷新其他信息**按钮。现在，模型数据编辑器包含与基础工作区中的参数对象 `structForInst1` 和 `structForInst2` 对应的行。
    
4.  使用**数据类型**列将 `structForInst1` 和 `structForInst2` 的数据类型设置为 `Bus: myParamStructType`。
    
5.  在 `ex_model_arg_ref` 的模型数据编辑器中，使用模型数据编辑器将 `structArg` 的数据类型设置为 `Bus: myParamStructType`。
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/xxstructarg_type.png)
    

### 更改模型参数名称或值

要在引用模型的上下文中重命名模型参数，请执行以下操作：

*   查找引用该模型的所有 Model 模块，并保存每个模块指定的特定于实例的参数值。使用 `get_param` 函数查询每个模块的 `InstanceParameters` 参数，该参数是一个结构体数组。该结构体包含四个字段：`Name`、`Value`、`Path` 和 `Argument`。
    
    您必须保存特定于实例的参数值，因为重命名操作会丢弃 Model 模块中的值。
    
*   在模型数据编辑器中，右键点击引用模型的模型工作区中的变量或对象，然后选择。重命名操作会更改变量或对象的名称，并在整个模型中更改对它的引用。有关详细信息，请参阅[创建、编辑和管理工作区变量](https://ww2.mathworks.cn/help/simulink/ug/create-edit-and-manage-workspace-variables.html)。
    
*   使用参数的新名称，将参数值重新应用于 Model 模块。要以编程方式在 Model 模块中设置参数值，请参阅[实例参数, Instance parameters 实例参数 Instance parameters](https://ww2.mathworks.cn/help/simulink/slref/model.html#mw_843c73d9-a07f-4591-a223-cbd650d089e2)。
    

### 自定义可重用组件的用户界面

当您设计可重用的引用模型以供团队的其他成员使用时，您可以对整个引用模型应用封装。然后，您可以自定义用户与 Model 模块交互的方式，包括设置特定于实例的值。

使用此方法还可以更轻松地以编程方式指定特定于实例的值。如果您创建并使用名为 `gainMask` 的封装参数，以编程方式将名为 `myModelBlock` 的模型实例的值设置为 `0.98`，则您的用户可以在命令提示符下使用以下命令：

```
set_param('myModelBlock','gainMask','0.98')

```

如果将封装应用于引用模型，则模型封装仅显示直接子级模型中特定于实例的参数。它不显示从后代模型提升的特定于实例的参数。

如果要在不封装模型的情况下设置特定于实例的值，请使用模块的 `InstanceParameters` 参数。有关详细信息，请参阅 [Parameterize a Referenced Model Programmatically](https://ww2.mathworks.cn/help/simulink/ug/parameterize-referenced-models-example.html)。

有关封装模型的信息，请参阅 [Introduction to System Mask](https://ww2.mathworks.cn/help/simulink/ug/systeml-mask.html)。

### 为 `Simulink.LookupTable` 和 `Simulink.Breakpoint` 对象配置特定于实例的数据

当您使用 `Simulink.LookupTable` 对象来存储和配置 ASAP2 或 AUTOSAR 代码生成（例如，STD_AXIS 或 CURVE）的查找表数据时，您可以将对象配置为模型参数。您还可以将 `Simulink.LookupTable` 引用的 `Simulink.Breakpoint` 对象配置为模型参数。然后，您可以为组件的每个实例指定唯一的表数据和断点数据。

您可以将 `Simulink.LookupTable` 参数的实例特定值指定为父模型中的新 `Simulink.LookupTable` 对象或指定为简单的 MATLAB 结构体或数组。

根据**断点设定**属性的值，为 `Simulink.LookupTable` 对象指定特定于实例的值。

*   当**断点设定**属性设置为 “`显式值`” 或 “`等间距`” 时，查找表对象的特定于实例的值可以是：
    
    *   `Simulink.LookupTable` 对象
        
    *   有效的 MATLAB 结构体变量的名称，如 `Model1_LUT2`
        
    *   字面值结构体表达式，如 `struct(‘Table’, …, ‘BP1’, …, ‘BP2’, …)`
        
    *   返回有效结构体的其他表达式，如 `Params.Model1.LUT2` 或对 MATLAB 函数的调用
        
    
*   当**断点设定**属性设置为 “`引用`” 时，`Simulink.LookupTable` 对象的特定于实例的值可以是：
    
    *   `Simulink.LookupTable` 对象。在这种情况下，Simulink 只将查找表对象的**表**属性传递给子模型。这允许您相互独立地配置 `Simulink.LookupTable` 对象和 `Simulink.Breakpoint` 对象的模型参量，并重用这些对象作为多个 Model 模块的实例参数。
        
    *   字面数值数组值，如 `[1 5 7; 2 8 13]`。
        
    *   数值数组变量的名称，如 `Model1_LUT2`。
        
    *   返回有效数值数组的其他表达式，如 `Params.Model1.LUT2` 或对 MATLAB 函数的调用。
        
    
    在这种情况下，如果将 `Simulink.LookupTable` 引用的 `Simulink.Breakpoint` 对象配置为模型参数，则 `Simulink.Breakpoint` 对象的特定于实例的值可以是：
    
    *   `Simulink.Breakpoint` 对象。
        
    *   定义断点的数值数组。数组必须与断点对象的设计定义匹配。Simulink 使用数组中的值合成 `Simulink.Breakpoint` 对象。
        
    *   数值数组变量或结构体字段的名称。
        
    

当您将 `Simulink.LookupTable` 参数的实例特定值指定为结构体时，需要遵循以下规则：

*   模型参数定义的每个字段必须在结构体中指定，并且字段的数目和名称必须匹配。例如，虽然只有来自 “`引用`” 类型的 `Simulink.LookupTable` 对象的表数据传递给子模型，但您仍必须在结构体中指定断点数据，否则 Simulink 会报告错误。
    
*   结构体中表的维度和断点数据必须与模型参数定义的对应项相匹配。
    
*   如果结构体字段的数据类型为 `double`，则该值将转换为对应模型参数字段的数据类型。否则，该值必须与对应模型参数字段的数据类型相匹配。
    

对于任何仿真模式和代码生成，都可以将 `Simulink.LookupTable` 对象的值指定为简单的数值。对于代码生成，如果使用存储类 `Auto` 配置模型参数，则生成的代码中不会保留结构体或数值数组变量。如果将存储类设置为任何其他值，则结构体或数值数组的值用于初始化生成的代码中的可调参数，与其他模型参数类似。

#### 指定 `Simulink.LookupTable` 对象的特定于实例的值

此示例说明如何将 `Simulink.LookupTable` 参数的实例特定值指定为新 `Simulink.LookupTable` 和 MATLAB 结构体。

有关使用查找表和命令提示符参数化引用模型的示例，请参阅 [Configure Instance-Specific Data for Lookup Tables Programmatically](https://ww2.mathworks.cn/help/simulink/ug/configure-instance-specific-data-for-lookup-tables1.html)。

首先，创建一个表示可重用算法的模型。将 `Simulink.LookupTable` 对象配置为您的模型要使用的模型参数。

1.  创建表示可重用算法的模型 `ex_arg_LUT_ref`。
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/configureinstancespecificdataforlookuptablesexample_01.png)
    
2.  使用模型资源管理器，在模型工作区中添加一个 `Simulink.LookupTable` 对象。您可以使用**添加 Simulink LookupTable** 按钮
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/lut_button_me.png)
    
    。为对象 `LUTArg` 命名。
    
3.  在**目录**窗格中，对于 `LUTArg`，选中**参数**列中的复选框。
    
4.  将**表维数**设置为 `2`。在**表**和**断点**表格区域中，对 “`表`”、“`BP1`” 和 “`BP2`” 数据指定值。例如，通过在 MATLAB 表达式框中输入下列值来配置表和断点数据。
    
    *   “`表`” - `[3 4;1 2]`
        
    *   “`BP1`” - `[1 2]`
        
    *   “`BP2`” - `[3 4]`
        
    
    当您直接从 `ex_arg_LUT_ref` 仿真或生成代码时，模型将使用这些值。
    
5.  在**结构体类型定义**下，将**名称**设置为 `LUTArg_Type`。
    
6.  点击**应用**。
    
7.  在**目录**窗格中，对于 `LUTArg`，选中**参数**列中的复选框。
    
8.  在引用模型的 n-D Lookup Table 模块中将**数据设定**设置为 “`查找表对象`”。将**名称**设置为 `LUTArg`。
    
9.  保存模型。
    

接下来，创建一个使用该可重用算法的模型，并为 `Simulink.LookupTable` 对象指定特定于实例的值。

1.  创建一个模型 `ex_arg_LUT`，该模型使用可重用算法两次。
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/configureinstancespecificdataforlookuptablesexample_02.png)
    
2.  在命令提示符下，在模型工作区中创建一个 `Simulink.LookupTable` 对象。您也可以在数据字典或基础工作区中创建 `Simulink.LookupTable` 对象。
    
    ```
    LUTForInst1 = Simulink.LookupTable;
    
    
    ```
    
3.  为对象指定断点和表数据。
    
    ```
    LUTForInst1.Table.Value = [8 7; 6 5];
    LUTForInst1.Breakpoints(1).Value = [5 6];
    LUTForInst1.Breakpoints(2).Value = [3 4];
    
    
    ```
    
4.  指定一个结构体类型名称。将此名称与引用模型工作区中对象指定的名称相匹配。
    
    ```
    LUTForInst1.StructTypeInfo.Name = 'LUTArg_Type';
    
    
    ```
    
5.  使用结构体为第二个 Model 模块创建实例特定的参数值。为结构体指定断点和表数据。
    
    ```
    StructForInst2.Table = [9 8; 7 7];
    StructForInst2.BP1 = [3 4];
    StructForInst2.BP2 = [5 6];
    
    
    ```
    
6.  在 `ex_arg_LUT` 模型中，对于模型实例 `Model`，在**实例参数**选项卡上，将 **LUTArg** 的值设置为 `LUTForInst1`。
    
7.  对于模型实例 `Model1`，将 **LUTArg** 设置为 `StructForInst2`。
    

`ex_arg_LUT_ref` 的一个实例使用存储在模型工作区的 `Simulink.LookupTable` 对象中的表和断点数据。另一个实例使用存储在结构体中的表和断点数据。

#### 为 `Simulink.LookupTable` 和 `Simulink.Breakpoint` 对象指定特定于实例的值

此示例说明如何将 `Simulink.LookupTable` 对象的特定于实例的值指定为新 `Simulink.LookupTable` 对象。该示例还说明如何为 `Simulink.LookupTable` 对象引用的 `Simulink.Breakpoint` 对象指定特定于实例的值。

首先，创建一个表示可重用算法的模型。配置 `Simulink.Breakpoint` 对象和引用该 `Simulink.Breakpoint` 对象的 `Simulink.LookupTable` 作为模型参数。

1.  创建表示可重用算法的模型 `ex_arg_BP_ref`。
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/configureinstancespecificdataforbreakpointsexample_01.png)
    
2.  使用模型资源管理器，在模型工作区中添加一个 `Simulink.LookupTable` 对象。您可以使用**添加 Simulink LookupTable** 按钮
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/lut_button_me.png)
    
    。为对象 `LUTArg` 命名。
    
3.  在**目录**窗格中，对于 `LUTArg`，选中**参数**列中的复选框。
    
4.  将**表维数**设置为 “`1`”。
    
5.  将表数据的值指定为 `[1 2]`。
    
6.  将**断点设定**设置为 “`引用`”。
    
7.  在**断点**部分，将**名称**设置为 `BP1`。
    
8.  在模型工作区中添加一个 `Simulink.Breakpoint` 对象。将该对象命名为 `BP1`，并将值指定为 `[3 4]`。
    
9.  在**目录**窗格中，对于 `BP1`，选中**参数**列中的复选框。
    

接下来，创建一个使用可重用算法的模型，并为 `Simulink.LookupTable` 对象和 `Simulink.Breakpoint` 对象指定特定于实例的值。

1.  创建一个模型 `ex_arg_BP`，该模型使用可重用算法两次。
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/configureinstancespecificdataforbreakpointsexample_02.png)
    
2.  在命令提示符下，对于模型实例 `Model`，在模型工作区中创建一个 `Simulink.LookupTable` 对象和一个 `Simulink.Breakpoint` 对象。您也可以在数据字典或基础工作区中创建这些对象。
    
    ```
    LUTForInst1 = Simulink.LookupTable;
    BPForInst1 = Simulink.Breakpoint;
    
    
    ```
    
3.  为对象指定断点和表数据。您必须为 `LUTForInst1` 指定断点信息，以便特定于实例的值的设定与模型参数设定匹配。但是，只有表数据被推送到子模型。
    
    ```
    LUTForInst1.Table.Value = [7 8];
    LUTForInst1.BreakpointsSpecification = 'Reference';
    LUTForInst1.Breakpoints = {'BPForInst1'};
    BPForInst1.Breakpoints.Value = [5 6];
    
    
    ```
    
4.  在 `ex_arg_BP` 模型中，对于模型实例 `Model`，在**实例参数**选项卡上，将 **LUTArg** 的值设置为 `LUTForInst1`，将 **BP1** 的值设置为 `BPForInst1`。
    
5.  对于模型实例 `Model1`，在模型工作区中创建另一个 `Simulink.LookupTable` 对象，并指定表和断点数据。您也可以在数据字典或基础工作区中创建 `Simulink.LookupTable` 对象。
    
    ```
    LUTForInst2 = Simulink.LookupTable;
    BPForInst2 = Simulink.Breakpoint;
    
    BPForInst2.Breakpoints.Value = [11 12];
    LUTForInst2.Table.Value = [9 10];
    LUTForInst2.BreakpointsSpecification = 'Reference';
    LUTForInst2.Breakpoints = {'BPForInst2'};
    
    
    ```
    
6.  对于此模型实例，使用数组来为 `Simulink.Breakpoint` 对象指定特定于实例的值。
    
    ```
    BPArrayForInst2 = [11 12];
    
    
    ```
    
7.  在 `ex_arg_BP` 模型中，对于模型实例 `Model1`，在**实例参数**选项卡上，将 **LUTArg** 的值设置为 `LUTForInst2`。对于此模型实例，将 BP1 的值设置为数组 `BPArrayForInst2`。
    

## 另请参阅

### 对象

*   [`Simulink.Parameter`](https://ww2.mathworks.cn/help/simulink/slref/simulink.parameter.html) | [`Simulink.LookupTable`](https://ww2.mathworks.cn/help/simulink/slref/simulink.lookuptable-class.html) | [`Simulink.Breakpoint`](https://ww2.mathworks.cn/help/simulink/slref/simulink.breakpoint-class.html)

### 模块

*   [Parameter Writer](https://ww2.mathworks.cn/help/simulink/slref/parameterwriter.html) | [Initialize Function](https://ww2.mathworks.cn/help/simulink/slref/initializefunction.html) | [Reset Function](https://ww2.mathworks.cn/help/simulink/slref/resetfunction.html)

## 相关主题

*   [Parameterize a Referenced Model Programmatically](https://ww2.mathworks.cn/help/simulink/ug/parameterize-referenced-models-example.html)
*   [Configure Instance-Specific Data for Lookup Tables Programmatically](https://ww2.mathworks.cn/help/simulink/ug/configure-instance-specific-data-for-lookup-tables1.html)
*   [Specify Instance-Specific Parameter Values for Reusable Referenced Model](https://ww2.mathworks.cn/help/rtw/ug/specify-instance-specific-parameter-values-for-reusable-referenced-models.html) (Simulink Coder)
*   [在结构体中组织相关的模块参数定义](https://ww2.mathworks.cn/help/simulink/ug/using-structure-parameters.html)
*   [Parameter Interfaces for Reusable Components](https://ww2.mathworks.cn/help/simulink/ug/design-parameter-interface-for-reusable-components.html)
*   [使用模块参数值进行调优和试验](https://ww2.mathworks.cn/help/simulink/ug/using-tunable-parameters.html)