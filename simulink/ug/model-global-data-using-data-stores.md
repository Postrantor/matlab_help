---
url: https://ww2.mathworks.cn/help/simulink/ug/model-global-data-using-data-stores.html
title: 通过创建数据存储对全局数据建模
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-18 20:59:48
tag: 
summary: 通过创建数据存储明确对全局内存中一个单独的块进行建模。
---
## 通过创建数据存储对全局数据建模

_数据存储_是一个存储库，可向其中写入数据和从中读取数据，而无需将输入或输出信号直接连接到数据存储。由于可从各个模型层级访问数据存储，因此子系统和引用模型都可以使用数据存储来共享数据，而无需使用 I/O 端口。要决定是否使用数据存储，请参阅[数据存储基础知识](https://ww2.mathworks.cn/help/simulink/ug/data-store-basics.html)。

### 数据存储示例

#### 概述

以下示例说明用于定义和访问数据存储的方法。请参阅[对数据存储访问进行排序](https://ww2.mathworks.cn/help/simulink/ug/model-global-data-using-data-stores.html#brytnz0)，了解控制一段时间内的数据存储访问的方法，例如确保始终先写入然后再读取给定的数据存储。请参阅[数据存储诊断](https://ww2.mathworks.cn/help/simulink/ug/data-store-basics.html#brytn1d-1_1)，了解在不运行任何仿真的情况下用来检测和更正潜在数据存储错误的方法。

#### 局部数据存储示例

以下模型说明了如何创建和访问局部数据存储，该数据存储只在模型或特定子系统中可见。

![](https://ww2.mathworks.cn/help/simulink/ug/local_data_store_example.png)

此模型使用数据存储，以允许子系统 A 发出信号来通知其输出是无效的。

![](https://ww2.mathworks.cn/help/simulink/ug/local_data_store_example_suba.png)

如果子系统 A 的输出无效，模型将使用子系统 B 的输出。

![](https://ww2.mathworks.cn/help/simulink/ug/local_data_store_example_subb.png)

#### 全局数据存储示例

下面的模型将前面示例中的子系统替换为在功能上相同的引用模型，以说明如何使用全局数据存储在模型引用层次结构中共享数据。

![](https://ww2.mathworks.cn/help/simulink/ug/global_data_store_example.png)

在此示例中，顶层模型使用 MATLAB® 工作区中的信号对象定义错误数据存储。该操作十分有必要，因为仅当数据存储由 MATLAB 工作区或数据字典中的信号对象定义时，它们才跨模型边界可见。模型为创建信号对象的 `PreLoadFcn` 模型回调参数指定代码。此代码在模型加载之前执行。

![](https://ww2.mathworks.cn/help/simulink/ug/global_data_store_example_callbacks.png)

### 创建和应用数据存储

以下是用于配置数据存储的常规工作流。您可以按不同的顺序执行这些任务，或者与其他部分分开执行，具体取决于您如何使用数据存储。

要创建数据存储，您可以创建 [Data Store Memory](https://ww2.mathworks.cn/help/simulink/slref/datastorememory.html) 模块或 [`Simulink.Signal`](https://ww2.mathworks.cn/help/simulink/slref/simulink.signal.html) 对象。模块或信号对象表示数据存储，并指定其属性。每个数据存储必须有唯一的名称。

使用 Data Store Memory 模块实现的数据存储：

*   支持数据存储初始化
    
*   在模型层次结构中的特定级别提供对数据存储范围和选项的控制
    
*   需要使用模块来表示数据存储
    
*   无法在引用模型内访问
    
*   不能位于 For Each 子系统模块代表的子系统内。
    

使用 `Simulink.Signal` 对象实现的数据存储：

*   在整个模型范围内提供对数据存储范围和选项的控制
    
*   不需要使用模块来表示数据存储
    
*   如果数据存储是全局数据存储，则可以在引用模型中访问
    

注意不要将局部数据存储与 Data Store Memory 模块划等号，也不要将全局数据存储与 [`Simulink.Signal`](https://ww2.mathworks.cn/help/simulink/slref/simulink.signal.html) 对象划等号。两种方法均可定义局部数据存储，而信号对象可以定义局部或全局数据存储。

### 包含 Data Store Memory 模块的数据存储

*   [创建数据存储](#br2ifzv)
    
*   [指定 Data Store Memory 模块的属性](#brytn1e-1)
    

#### 创建数据存储

要使用 Data Store Memory 模块定义数据存储，请将模块的一个实例拖到您希望数据存储在其中可见的顶层模型。结果是一个局部数据存储，无法在引用模型内访问。

*   要定义一个在给定模型内的每个级别（Model 模块除外）均可见的数据存储，请将 Data Store Memory 模块拖到模型的根级。
    
*   要定义一个仅在特定子系统及其包含的子系统内可见、但在 Model 模块内不可见的数据存储，请将 Data Store Memory 模块拖到该子系统中。
    

添加 Data Store Memory 模块后，请使用其参数定义数据存储的属性。**数据存储名称**属性指定 Data Store Write 和 Data Store Read 模块可以访问的数据存储的名称。有关详细信息，请参阅 [Data Store Memory](https://ww2.mathworks.cn/help/simulink/slref/datastorememory.html) 文档。

除了那些可以使用 Data Store Memory 模块参数定义的数据存储属性外，您还可以通过选择**数据存储名称必须解析为 Simulink 信号对象**选项并将信号对象用作数据存储名称，来定义数据存储属性。有关详细信息，请参阅[使用信号对象指定属性](https://ww2.mathworks.cn/help/simulink/ug/model-global-data-using-data-stores.html#brytogf-2)。

#### 指定 Data Store Memory 模块的属性

Data Store Memory 模块可以从其对应的 Data Store Read 和 Data Store Write 模块继承三个数据属性。可继承属性包括：

但是，允许这些属性成为继承属性可能会导致难以调试的意外结果。要避免出现此类错误，请使用 Data Store Memory 模块对话框或 `Simulink.Signal` 对象显式指定属性。

**使用模块参数指定属性**

您可以使用 Data Store Memory 模块对话框或模型数据编辑器的**数据存储**选项卡（在**建模**选项卡上，点击**模型数据编辑器**）指定数据存储的数据类型和复 / 实性。在下图中，模块对话框将**数据类型**设置为 `uint16`，并将**信号类型**设置为 `real`。

![](https://ww2.mathworks.cn/help/simulink/ug/data_store_memory.png)

**使用信号对象指定属性**

您可以使用 `Simulink.Signal` 对象为 Data Store Memory 模块指定数据存储属性。

**提示**

要按照[包含信号对象的数据存储](https://ww2.mathworks.cn/help/simulink/ug/model-global-data-using-data-stores.html#bryzv43-2)中的描述建立隐式数据存储，请使用与您将信号对象与 Data Store Memory 模块显式关联在一起时使用的同一种常规方法。

下图显示了为名为 `A` 的 `Simulink.Signal` 对象指定解析的 Data Store Memory 模块。要对数据存储使用信号对象，请将**数据存储名称**设置为信号对象的名称。要执行编译时检查，请打开**信号属性**选项卡，然后选择**数据存储名称必须解析为 Simulink 信号对象**参数。

![](https://ww2.mathworks.cn/help/simulink/ug/data_store_memory_main.png)

或者，在模型数据编辑器的**数据存储**选项卡（在**建模**选项卡上，点击**模型数据编辑器**）上编辑数据存储名称时，点击附近的操作按钮

![](https://ww2.mathworks.cn/help/simulink/ug/prop_action_ctrl.png)

，然后选择。在 “创建新数据” 对话框中，将**值**设置为 `Simulink.Signal`。

信号对象指定数据存储原本会继承的所有三个数据属性的值。在用于定义局部数据存储的此示例中，`Simulink.Signal` 对象 `A` 具有以下继承的属性：`DataType`、`Complexity` 和 `SampleTime`。

```
A =
 
Simulink.Signal (handle)
         CoderInfo: [1x1 Simulink.CoderInfo]
       Description: ''
          DataType: 'auto'
               Min: []
               Max: []
              Unit: ''
        Dimensions: 1
    DimensionsMode: 'auto'
        Complexity: 'auto'
        SampleTime: -1
      InitialValue: 0

```

有关指定局部和全局数据存储的信号对象属性的详细信息，请参阅[数据存储的信号对象属性](https://ww2.mathworks.cn/help/simulink/ug/model-global-data-using-data-stores.html#br2ifcw-1)。

**使用模型数据编辑器来配置列表中的 Data Store Memory 模块.** 使用模型数据编辑器中的**数据存储**选项卡配置 Data Store Memory 模块的参数。使用这种方法，无需定位数据存储即可对其进行配置，还可将数据存储与其他接口元素（例如 Inport 和 Outport 模块）一起配置。模型数据编辑器还在同一列表中显示 Data Store Read 和 Data Store Write 模块的信息。

要打开**[模型数据编辑器](https://ww2.mathworks.cn/help/simulink/slref/modeldataeditor.html)**，请在**建模**选项卡中，点击**模型数据编辑器**。

### 包含信号对象的数据存储

*   [创建数据存储](#br2ifox)
    
*   [局部和全局数据存储](#br2ifox_1)
    
*   [数据存储的信号对象属性](#br2ifcw-1)
    

#### 创建数据存储

若要使用 `Simulink.Signal` 对象定义数据存储，而不使用 Data Store Memory 模块，请在工作区中创建对需要访问该数据存储的每个组件均可见的信号对象。相关联的数据存储的名称就是该信号对象的名称。您可以在 Data Store Read 和 Data Store Write 模块中使用此名称，就好像它是 Data Store Memory 模块的**数据存储名称**一样。当您对数据存储使用该信号对象时，Simulink® 会创建关联的数据存储。

#### 局部和全局数据存储

您可以使用 `Simulink.Signal` 对象来定义局部或全局数据存储。

*   如果您在 MATLAB 基础工作区或数据字典中定义该对象，则结果是一个全局数据存储，可在 Simulink 内的每个模型（包括所有引用模型）中访问该数据存储。
    
*   如果您在模型工作区中创建对象，则结果是一个局部数据存储，可在模型（引用模型除外）的每个级别进行访问。
    

#### 数据存储的信号对象属性

信号对象未定义的那些数据存储属性具有它们在 Data Store Memory 模块中拥有的相同默认值。用作数据存储的信号对象的属性值具有不同的要求，具体取决于数据存储是本地存储还是全局存储。

创建对象后，您可以将信号对象的属性设置为您希望相应的数据存储属性具有的值。例如，以下命令在 MATLAB 基础工作区中定义一个名为 `Error` 的数据存储：

```
Error = Simulink.Signal;
Error.Description = 'Use to signal that subsystem output is invalid';
Error.DataType = 'boolean';
Error.Complexity = 'real';
Error.Dimensions = 1;
Error.SampleTime = 0.1;

```

**局部数据存储的属性.** 对于局部数据存储，您可为下面列出的每个参数显式设置值，也可以让数据存储从 Data Store Write 和 Data Store Read 模块继承值。

*   `DataType`
    
*   `Complexity`
    
*   `SampleTime`
    

要使用 Data Store Memory 模块定义局部数据存储，您可以对**数据存储名称**参数使用信号对象。要进行编译时检查，请在**信号属性**选项卡上，选择**数据存储名称必须解析为 Simulink 信号对象**参数。如果 Simulink 找不到信号对象或信号对象属性与信号对象属性不一致，**数据存储名称必须解析为 Simulink 信号对象**参数会导致 Simulink 显示错误并停止编译。

**全局数据存储的属性.** 下表介绍了全局数据存储的参数要求。

<table><colgroup><col width="46%"><col width="54%"></colgroup><thead><tr><th>参数</th><th>全局数据存储值</th></tr></thead><tbody><tr><td><code>DataType</code></td><td>必须显式设置</td></tr><tr><td><code>Complexity</code></td><td>必须显式设置</td></tr><tr><td><code>Dimensions</code></td><td>可以设置或继承</td></tr><tr><td><code>SampleTime</code></td><td>可以设置或继承</td></tr></tbody></table>

**修改由信号对象定义的数据存储的属性.** 您可以使用模型数据编辑器（在**建模**选项卡中，点击**模型数据编辑器**）修改和检查数据存储、Data Store Read 和 Data Store Write 模块的属性。在**数据存储**选项卡上，要显示使用信号对象（例如 `Simulink.Signal`）定义的数据存储的属性，请点击**显示 / 刷新其他信息**按钮。然后，如果数据表中所示的 Data Store Read 或 Data Store Write 模块引用由某信号对象定义的数据存储，则表包含与该对象对应的行。

有关详细信息，请参阅**[模型数据编辑器](https://ww2.mathworks.cn/help/simulink/slref/modeldataeditor.html)**。

### 使用 Simulink 模块访问数据存储

*   [写入数据存储](#bsvjgxl)
    
*   [从数据存储读取](#bsvjgyz)
    
*   [访问全局数据存储](#bsvjg0j)
    

#### 写入数据存储

要在每个时间步设置数据存储的值：

1.  在计算值的模型级别创建 [Data Store Write](https://ww2.mathworks.cn/help/simulink/slref/datastorewrite.html) 模块的一个实例。
    
2.  将 Data Store Write 模块的**数据存储名称**参数设置为您要向其中写入数据的数据存储的名称。
    
3.  将计算值的模块的输出连接到 Data Store Write 模块的输入。
    

![](https://ww2.mathworks.cn/help/simulink/ug/data_store_ex1b.png)

#### 从数据存储读取

要在每个时间步获取数据存储的值：

1.  在需要该值的模型级别创建 [Data Store Read](https://ww2.mathworks.cn/help/simulink/slref/datastoreread.html) 模块的一个实例。
    
2.  将 Data Store Read 模块的**数据存储名称**参数设置为您要从中读取数据的数据存储的名称。
    
3.  将 Data Store Read 模块的输出连接到需要数据存储值的模块的输入。
    

![](https://ww2.mathworks.cn/help/simulink/ug/data_store_ex1a.png)

#### 访问全局数据存储

连接到全局数据存储（由 MATLAB 工作区中的信号对象定义）时，Data Store Read 或 Data Store Write 模块将在数据存储名称上方显示文字 `global`。

![](https://ww2.mathworks.cn/help/simulink/ug/creating_model44.png)

### 对数据存储访问进行排序

*   [关于数据存储访问顺序](#bryzuht)
    
*   [使用函数调用子系统对访问进行排序](#brytoeq-1)
    
*   [使用模块优先级对访问进行排序](#brytoeq-2)
    

#### 关于数据存储访问顺序

要从数据存储获取正确的结果，您必须控制数据存储读取和写入操作的执行顺序。如果数据存储的读取操作发生在写入操作之前，将会向算法中引入延迟：读取操作获取的值是在上一个时间步（而非当前时间步）中计算和存储的值。

这种延迟可能会导致系统行为与设计初衷不符，有些情况下还可能导致系统不稳定。即使不会发生这些问题，不受控制的访问顺序在不同版本的 Simulink 之间可能也会发生变化。

本节将介绍用于显式控制数据存储读取和写入执行顺序的几种策略。请参阅[数据存储诊断](https://ww2.mathworks.cn/help/simulink/ug/data-store-basics.html#brytn1d-1_1)，了解在不运行仿真的情况下检测和更正潜在数据存储错误可以采用的一些方法。

#### 使用函数调用子系统对访问进行排序

您可以使用函数调用子系统来控制访问数据存储的模型组件的执行顺序。下图显示了这种方法：

![](https://ww2.mathworks.cn/help/simulink/ug/image003model.png)

![](https://ww2.mathworks.cn/help/simulink/ug/image003chart.png)

![](https://ww2.mathworks.cn/help/simulink/ug/image003before.png)

![](https://ww2.mathworks.cn/help/simulink/ug/image003after.png)

子系统 `Before` 包含 Data Store Write，Stateflow® 图先调用该子系统，然后再调用子系统 `After`，后者包含 Data Store Read。

#### 使用模块优先级对访问进行排序

您可以将数据存储读取操作和写入操作嵌入到按优先级指定相对执行顺序的原子子系统内或 Model 模块内。

![](https://ww2.mathworks.cn/help/simulink/ug/image005_model.png)

![](https://ww2.mathworks.cn/help/simulink/ug/image005_before.png)

![](https://ww2.mathworks.cn/help/simulink/ug/image005_after.png)

Model 模块 `beforeDSM` 的优先级低于 `afterDSM`，因此能保证它先执行。由于 `beforeDSM` 是原子子系统，它的所有操作（包括 Data Store Write）都将在 `afterDSM` 及其所有操作（包括 Data Store Read）之前执行。

### 包含总线和总线数组的数据存储

使用包含总线和总线数组的数据存储的好处包括：

*   通过将多个信号与单个数据存储相关联，可以简化模型布局
    
*   能够生成特定的代码，这些代码可以将存储数据中的数据表示为反映总线层次结构的结构体
    
*   写入数据存储和从数据存储中读取时无需创建数据副本，因而可以提高数据访问效率
    

您不能使用包含以下项的总线或总线数组：

#### 设置模型以使用包含总线和总线数组的数据存储

此过程适用于局部和全局数据存储，以及使用 Data Store Memory 模块或 `Simulink.Signal` 对象定义的数据存储。在执行此过程之前，您必须了解如何在模型中使用数据存储，如[创建和应用数据存储](https://ww2.mathworks.cn/help/simulink/ug/model-global-data-using-data-stores.html#br2h68s-1)中所述。

若要将总线和总线数组与数据存储结合使用：

### 访问特定的总线和矩阵元素

#### 选择特定的总线或矩阵元素

默认情况下，模型会向数据存储写入和从中读取所有总线和矩阵元素。

要选择要写入数据存储或从中读取的特定总线或矩阵元素，请使用 [Data Store Write](https://ww2.mathworks.cn/help/simulink/slref/datastorewrite.html) 模块的**元素赋值**窗格和 [Data Store Read](https://ww2.mathworks.cn/help/simulink/slref/datastoreread.html) 模块的**元素选择**窗格。选择特定的总线或矩阵元素可提供以下优点：

*   减少模型中模块的数量。例如，您可以对要访问的每个特定总线元素省略 Data Store Read 和 Bus Selector 模块对组，或 Data Store Write 和 Bus Assignment 模块对组。
    
*   使用大型总线和总线数组更快地对模型进行仿真。
    

#### 将特定元素写入数据存储

**注意**

以下过程介绍了如何使用 Data Store Write 模块接口将特定元素写入数据存储。您还可以在命令行中执行此任务，方法是使用 `DataStoreElements` 参数来指定元素。有关详细信息，请参阅[使用命令行指定](https://ww2.mathworks.cn/help/simulink/ug/model-global-data-using-data-stores.html#bsvla4x)。

要指定特定的总线或矩阵元素以写入数据存储：

1.  选择 Data Store Write 模块，然后在参数对话框中，选择**元素赋值**窗格。例如，假设您使用的一条总线具有名为 `DSM` 的数据存储：
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/data_store_write_unexpanded.png)
    
2.  展开**总线中的信号**列表中的所有元素。
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/data_store_write_expanded.png)
    
3.  指定您要写入数据存储的元素。例如：
    
    *   在**总线中的信号**列表中，点击 `B`。然后点击**选择 >>** 以选择元素 `B`。
        
    *   要输出 `A2` 的所有元素（在 `A` 嵌套总线中），请选择 `A2[5,1]`。然后点击**选择 >>**。
        
    *   要输出 `C2` 嵌套总线中的第二个元素 `A2`，请选择 `A2[5,1]` 元素。在**指定要赋值的元素**文本框中，编辑文本，使其显示 `DSM.C.C2.A2(2,1)`。
        
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/data_store_write_with_assignments.png)
    
    有关更多示例，请参阅[指定要分配或选择的元素](https://ww2.mathworks.cn/help/simulink/ug/model-global-data-using-data-stores.html#bsvlask-1)。
    
4.  （可选）对分配的元素重新排序，这会更改 Data Store Write 模块的端口顺序。
    
    *   要对分配的元素重新排序，请在**赋值的元素**列表中，选择您要移动的元素，然后点击**向上**或**向下**。
        
    *   要删除指定的元素，请点击**删除**。
        
    
5.  要应用分配的元素，请点击**确定**。
    
    Data Store Write 模块对每个分配的元素都有一个端口。与每个端口对应的选定元素的名称将显示在模块图标中。如果您分配多个信号，则增加的这些内容可能会降低模型的易读性。要改善易读性，您可以扩展模块的大小，或创建多个 Data Store Write 模块。
    

#### 从数据存储读取特定元素

从数据存储读取特定元素涉及的步骤与[将特定元素写入数据存储](https://ww2.mathworks.cn/help/simulink/ug/model-global-data-using-data-stores.html#bsvjg3c)中所述的步骤非常相似。Data Store Read 模块与 Data Store Write 模块略有不同。Data Store Read 模块具有：

*   **元素选择**窗格，而不是**元素赋值**窗格
    
*   **所选元素**列表，而不是**赋值的元素**列表
    

#### 指定要分配或选择的元素

使用 MATLAB 矩阵元素语法来指定特定元素。有关在 MATLAB 中指定矩阵的详细信息，请参阅[创建、串联和扩展矩阵](https://ww2.mathworks.cn/help/matlab/math/creating-and-concatenating-matrices.html)。

**有效元素指定.** 下表显示了用于指定要分配或选择的元素的有效语法示例。这些元素使用 `A` 总线的 `A2` 嵌套总线，如[将特定元素写入数据存储](https://ww2.mathworks.cn/help/simulink/ug/model-global-data-using-data-stores.html#bsvjg3c)中使用的总线层次结构所示。

![](https://ww2.mathworks.cn/help/simulink/ug/data_store_write_expanded.png)

<table><colgroup><col width="50%"><col width="50%"></colgroup><thead><tr><th>有效语法</th><th>描述</th></tr></thead><tbody><tr><td><code>DSM.A.A2(:,:)</code></td><td><p>选择每个维度中的所有元素</p></td></tr><tr><td><code>DSM.A.A2([1,3,5],1)</code></td><td><p>选择第一、第三和第五个元素</p></td></tr><tr><td><code>DSM.A.A2(2:5,1)</code></td><td><p>选择第二至第五个元素</p></td></tr></tbody></table>

**无效元素指定.** 下表显示了用于指定要分配或选择的元素的无效语法示例。这些元素使用 `A` 总线的 `A2` 嵌套总线，如[将特定元素写入数据存储](https://ww2.mathworks.cn/help/simulink/ug/model-global-data-using-data-stores.html#bsvjg3c)中使用的总线层次结构所示。

<table><colgroup><col width="50%"><col width="50%"></colgroup><thead><tr><th>无效语法</th><th>语法无效的原因</th></tr></thead><tbody><tr><td><code>DSM.A.A2(:)</code></td><td><p>您必须为每个维度指定冒号。对于这些示例中使用的总线层次结构，您必须使用两个冒号。</p></td></tr><tr><td><code>DSM.A.A2(2:end,1)</code></td><td><p>不能使用 <code>end</code> 运算符。</p></td></tr><tr><td><code>DSM.A.A2(idx,1)</code></td><td><p>不能使用变量来指定索引。考虑通过在模块参数对话框的<strong>元素选择</strong> / <strong>元素赋值</strong>窗格中选择<strong>启用索引</strong>来使用动态索引。请参阅 <a href="https://ww2.mathworks.cn/help/simulink/slref/datastoreread.html"><span>Data Store Read</span></a> 和 <a href="https://ww2.mathworks.cn/help/simulink/slref/datastorewrite.html"><span>Data Store Write</span></a>。</p></td></tr><tr><td><code>DSM.A.A2(-1,1)</code></td><td><p>维度 <code>–1</code> 不在有效维度范围内。</p></td></tr></tbody></table>

**使用命令行指定.** 要设置要写入或读取的元素，请使用 `DataStoreElements` 参数。使用井号 (#) 分隔多个元素。例如，选择您要为其指定元素的 Data Store Write 或 Data Store Read 模块，并输入命令，如：

```
set_param(gcb, 'DataStoreElements', 'DSM.A#DSM.B#DSM.C(3,4)')

```

此指定会使模块现在具有三个对应于您指定的元素的端口。

### 重命名数据存储

*   [重命名模块定义的数据存储](#buoeaau)
    
*   [重命名信号对象定义的数据存储](#buoeaa4)
    

#### 重命名模块定义的数据存储

将模型中 Data Store Read 和 Data Store Write 模块用到的某个数据存储全部进行重命名。

1.  在 Data Store Memory 模块对话框的**数据存储名称**框中键入新名称，然后点击**全部重命名**。
    
2.  在**全部重命名**对话框中，确认**新名称**字段中的新数据存储名称，然后点击**确定**
    

**注意**

如果您在工作区中创建 `Simulink.Signal` 对象以控制为数据存储生成的代码，则不能使用**全部重命名**来重命名数据存储，而是必须使用模型资源管理器重命名相应的 `Simulink.Signal` 对象。有关示例，请参阅[重命名信号对象定义的数据存储](https://ww2.mathworks.cn/help/simulink/ug/model-global-data-using-data-stores.html#buoeaa4)。

#### 重命名信号对象定义的数据存储

此示例说明了如何重命名由 `Simulink.Signal` 对象定义的数据存储。您可以使用模型资源管理器，在模型或模型引用层次结构中的 Data Store Read 和 Data Store Write 模块使用该对象的所有位置重命名该对象。

1.  打开模型 `sldemo_mdlref_dsm`。该模型会在 MATLAB 基础工作区中创建 `Simulink.Signal` 对象 `ErrorCond`，并将该对象用作模型引用层次结构中的全局数据存储。
    
    ```
    openExample('sldemo_mdlref_dsm')
    
    
    
    ```
    
2.  打开模型资源管理器。
    
3.  在**模型层次结构**窗格中，选择基础工作区。
    
4.  在**目录**窗格中，右键点击数据存储 `ErrorCond`，并选择**全部重命名**。
    
5.  在**选择系统**对话框中，点击模型 `sldemo_mdlref_dsm` 的名称，以选择它作为重命名数据存储 `ErrorCond` 的上下文。
    
6.  由于 `ErrorCond` 是在引用模型中使用的全局数据存储，因此应选择**在引用模型中搜索**。点击**确定**。
    
    默认情况下，**更新图以包括最近的更改**复选框处于清除状态，通过避免不必要的模型图更新来节省时间。选中该复选框将强制模型图更新以包含您最近对模型所做的更改。
    
7.  点击**确定**以响应更新模型图的消息。
    
    由于您刚刚打开模型，因此至少要更新一次模型图后才能重命名变量（例如 `ErrorCond`）。您可能已经在**选择系统**对话框中选择了**更新图以包括最近的更改**以强制进行初始模型图更新，但通常是在执行多个变量重命名操作的同时对模型进行了更改的情况下，才使用此选项。
    
8.  在**全部重命名**对话框的**新名称**框中键入数据存储的新名称，然后点击**确定**。
    

### 生成的代码中的自定义数据存储访问函数

Embedded Coder® 提供了存储类，您可以使用该类在生成的代码中指定自定义数据存储访问函数。请参阅[使用 Struct 存储类将参数数据组织为结构体](https://ww2.mathworks.cn/help/ecoder/ug/configure-parameters-for-code-generation.html#br0debj) (Embedded Coder) 和 [Access Data Through Functions with Storage Class GetSet](https://ww2.mathworks.cn/help/ecoder/ug/getset-custom-storage-classes.html) (Embedded Coder)。

## 另请参阅

### 模块

*   [Data Store Memory](https://ww2.mathworks.cn/help/simulink/slref/datastorememory.html) | [Data Store Read](https://ww2.mathworks.cn/help/simulink/slref/datastoreread.html) | [Data Store Write](https://ww2.mathworks.cn/help/simulink/slref/datastorewrite.html)

### 对象

*   [`Simulink.Signal`](https://ww2.mathworks.cn/help/simulink/slref/simulink.signal.html)

## 相关主题

*   [Data Stores in Generated Code](https://ww2.mathworks.cn/help/rtw/ug/data-stores.html) (Simulink Coder)
*   [Log Data Stores](https://ww2.mathworks.cn/help/simulink/ug/logging-data-stores.html)
*   [数据存储基础知识](https://ww2.mathworks.cn/help/simulink/ug/data-store-basics.html)
*   [数据对象](https://ww2.mathworks.cn/help/simulink/ug/working-with-data-objects.html)
*   [信号基础知识](https://ww2.mathworks.cn/help/simulink/ug/signal-basics.html)
*   [合成接口规范](https://ww2.mathworks.cn/help/simulink/ug/composite-signal-techniques.html)