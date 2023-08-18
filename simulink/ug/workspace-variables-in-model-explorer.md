---
url: https://ww2.mathworks.cn/help/simulink/ug/workspace-variables-in-model-explorer.html
title: 使用模型资源管理器编辑和管理工作区变量
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-18 21:00:41
tag: 
summary: 查找模型或模块使用的工作区变量、查找使用某个变量的模块、查找未使用的变量，以及重命名模块在任意位置使用的某个变量。在单独的文件中保存和加载变量。
---
## 使用模型资源管理器编辑和管理工作区变量

要了解可用于创建、编辑和管理工作区变量的所有方法，请参阅[创建、编辑和管理工作区变量](https://ww2.mathworks.cn/help/simulink/ug/create-edit-and-manage-workspace-variables.html)。

### 查找模型或模块使用的变量

在模型资源管理器中，您可以获取模型或模块使用的变量的列表。下面是获取变量列表的一种方式：

1.  在**目录**窗格中，右键点击您要查找其所用变量的模块。
    
2.  选择菜单项。
    

![](https://ww2.mathworks.cn/help/simulink/ug/me_referenced_variables.png)

模型资源管理器返回如下所示的结果：

![](https://ww2.mathworks.cn/help/simulink/ug/me_referenced_variables_result.png)

为了提高性能，模型资源管理器使用模型的上一个编译版本中缓存的信息。如果您要重新编译模型，可以手动进行，也可以在模型资源管理器中将**更新图**字段设置为 “`是`” 并重复搜索。

您还可以通过以下方式查找模型或模块使用的变量：

*   在模型资源管理器的窗格中，右键点击模块或模型节点，然后选择菜单项。
    
*   在模型资源管理器的搜索栏中，使用 “`引用变量`” 搜索类型选项。
    
*   在 Simulink® 编辑器中，右键点击模块、子系统或画布，然后选择菜单项。点击画布，返回整个模型的结果。
    

[`Simulink.findVars`](https://ww2.mathworks.cn/help/simulink/slref/simulink.findvars.html) 函数提供了更多选项，可以返回从模型资源管理器或 Simulink 编辑器中无法获得的工作区变量信息。

有关查找引用变量时所受限制的信息，请参阅 [`Simulink.findVars`](https://ww2.mathworks.cn/help/simulink/slref/simulink.findvars.html) 说明文档。

#### 使用返回的变量集

对于返回的变量集中的某个变量，您可以查找使用该变量的模块（有关详细信息，请参阅[查找使用特定变量的模块](https://ww2.mathworks.cn/help/simulink/ug/workspace-variables-in-model-explorer.html#bso5cp5)）。此外，您还可以从返回的变量集中导出变量。有关详细信息，请参阅[导出工作区变量](https://ww2.mathworks.cn/help/simulink/ug/workspace-variables-in-model-explorer.html#bso5crg)。

### 查找使用特定变量的模块

此示例说明如何使用模型资源管理器获取使用特定工作区变量的模块列表。

1.  打开模型 `f14`。
    
2.  打开模型资源管理器。
    
3.  在**模型层次结构**窗格中，选择 `Base Workspace` 节点。
    
4.  在**目录**窗格中，右键点击变量 `Mq` 并选择。
    
5.  在**选择系统**对话框中，选择 `f14`。
    
6.  清除**在引用模型中搜索**复选框，因为 `f14` 没有引用任何模型，然后点击**确定**。
    
    选中**在引用模型中搜索**之后，您可以查找在模型引用层次结构中任意位置使用的目标变量。但是，在整个层次结构中查找目标变量可能需要更长时间。
    
    默认情况下，**更新图以包括最近的更改**复选框处于清除状态，通过避免不必要的模型图更新来节省时间。选中该复选框将强制模型图更新以包含您最近对模型所做的更改。
    
7.  点击**确定**以响应更新模型图的消息。
    
    由于您刚刚打开模型，因此至少要更新一次模型图才能查找变量。您可能已经在**选择系统**对话框中选择了**更新图以包括最近的更改**以强制进行初始模型图更新，但通常是在使用**查找使用位置**执行多个搜索的同时对模型进行了更改的情况下，才使用此选项。
    
8.  模型资源管理器显示搜索结果：
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/me_where_used_results.png)
    
    值中包含 `Mq` 的属性列表示使用 `Mq` 变量的模块参数。如果这些属性列尚不在视图中，模型资源管理器会将它们添加到显示的搜索结果的末尾。
    

您还可以通过以下方式之一查找使用特定变量的模块：

*   在搜索栏中，选择 “`变量使用`” 搜索类型选项。
    
*   在**搜索结果**窗格中，右键点击一个变量，然后选择菜单项。
    
*   在模型数据编辑器中，右键点击一个工作区变量并选择菜单项。
    

### 查找未使用的工作区变量

您可以使用模型资源管理器获取在工作区中定义但未被模型或模块使用的变量列表。获取此变量列表的方式之一是在**模型层次结构**窗格中右键点击某个工作区名称，然后选择菜单项。例如：

1.  打开 `f14` 模型。
    
2.  打开模型资源管理器。
    
3.  在搜索工具栏中，将**更新图**字段设置为 “`是`”。
    
4.  在**模型层次结构**窗格中，右键点击 `Base Workspace` 节点，然后选择菜单项。
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/me_unused_variables.png)
    
5.  模型资源管理器显示如下输出：
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/me_unused_variables_result.png)
    
    [`Simulink.findVars`](https://ww2.mathworks.cn/help/simulink/slref/simulink.findvars.html) 函数提供了更多选项，可以返回从模型资源管理器或 Simulink 编辑器中无法获得的有关未使用的工作区变量的信息。
    

### 编辑工作区变量

在模型资源管理器中，您可以使用变量编辑器编辑 MATLAB® 基础工作区或模型工作区中的变量。变量编辑器用于编辑大型数组和结构体。

要打开变量编辑器，请执行下列操作：

1.  在**目录**窗格中，选择变量。
    
2.  在 “对话框” 窗格（右窗格）中，点击变量值附近的按钮
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/prop_action_ctrl.png)
    
    。
    
3.  在菜单中，选择**打开变量编辑器**。
    

或者，要从**目录**窗格而不是 “目录” 窗格中打开“变量编辑器”，请通过点击相应的单元格开始编辑变量的值。该按钮出现在单元格中。

#### 三维或多维数组的表示形式

当变量或 `Simulink.Parameter` 对象的值是一个三维或多维数组时，**值**列会将该数组显示为一个表达式，其中包含对 [`reshape`](https://ww2.mathworks.cn/help/matlab/ref/reshape.html) 函数的调用。

要编辑数组中的值，请修改 `reshape` 调用的第一个参数，该参数将所有数组值包含在串行化向量中。当沿某个维度添加或删除元素时，还必须更正表示修改后的维度的长度的参数。

### 重命名变量

此示例说明如何使用模型资源管理器重命名 Simulink 模型中的模块在任意位置使用的某个变量。

1.  打开[防抱死制动系统建模](https://ww2.mathworks.cn/help/simulink/slref/modeling-an-anti-lock-braking-system.html)示例模型 `[sldemo_absbrake](matlab:openExample('sldemo_absbrake'))`。此模型将数据加载到 MATLAB 基础工作区。
    
2.  打开模型资源管理器。
    
3.  在**模型层次结构**窗格中，选择基础工作区。
    
4.  在**目录**窗格中，右键点击基础工作区变量 `m`，然后选择**全部重命名**。
    
5.  在**选择系统**对话框中，点击模型 `sldemo_absbrake` 的名称，选择它作为重命名变量 `m` 的上下文。
    
6.  清除**在引用模型中搜索**复选框，然后点击**确定**。模型 `sldemo_absbrake` 引用模型 `sldemo_wheelspeed_absbrake`，但只有 `sldemo_absbrake` 使用变量 `m`。
    
    选中**在引用模型中搜索**之后，您可以重命名在模型引用层次结构中任意位置使用的目标变量。但是，在整个层次结构中重命名目标变量可能需要更长时间。
    
    默认情况下，**更新图以包括最近的更改**复选框处于清除状态，通过避免不必要的模型图更新来节省时间。选中该复选框将强制模型图更新以包含您最近对模型所做的更改。
    
7.  点击**确定**以响应更新模型图的消息。
    
    由于您刚刚打开模型，因此至少要更新一次模型图才能重命名变量。您可能已经在**选择系统**对话框中选择了**更新图以包括最近的更改**以强制进行初始模型图更新，但通常是在执行多个变量重命名操作的同时对模型进行了更改的情况下，才使用此选项。
    
8.  在**全部重命名**对话框中，在**新名称**框中键入变量的新名称，然后点击**确定**。
    
    您可以使用**全部重命名**对话框中**对应的模块**部分的超链接来查看目标模块。
    

有关重命名文件的帮助信息，请使用工程。请参阅 [Automatic Updates When Renaming, Deleting, or Removing Files](https://ww2.mathworks.cn/help/simulink/ug/move-rename-copy-or-delete-project-files.html#bu5myet)。

### 比较重复的工作区变量

您可以比较存储在同一工作区或不同工作区中的重复变量。例如，可以将存储在基础工作区中的变量与存储在模型工作区中的重复变量进行比较。

1.  打开模型和模型资源管理器。
    
2.  在搜索工具栏中，搜索重复的变量。选择包含重复项的行。然后，右键点击并选择**比较选定项**。
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/me_comparevariables.png)
    
3.  在**比较查看器**中查看区别。
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/me_comparisonviewer.png)
    

### 导出工作区变量

您可以导出（保存）模型资源管理器中列出的一组变量，既可以导出单个变量，也可以导出基础工作区或模型工作区中的所有变量。

还可以导出使用**查找引用变量**选项或 `Simulink.findVars` 函数返回的变量集。有关详细信息，请参阅[查找模型或模块使用的变量](https://ww2.mathworks.cn/help/simulink/ug/workspace-variables-in-model-explorer.html#bso5b9r)。

要在模型资源管理器中将某个工作区中的所有变量导出到 MATLAB 代码文件或 MAT 文件中，请执行以下操作：

1.  选择要导出的变量。
    
    1.  要选择一个工作区中的所有变量，请右键点击该工作区节点（例如，`Base Workspace`），然后选择菜单项。例如：
        
        ![](https://ww2.mathworks.cn/help/simulink/ug/me_export_variables.png)
        
    2.  要选择单个变量，请在**目录**窗格中选择要导出的变量。右键点击突出显示的某个变量，然后选择菜单项。
        
    
    如果**目录**窗格中的数据按属性分组，则选择一个组中的第一行并不会选择该组中的所有变量。有关分组数据的详细信息，请参阅**[模型资源管理器](https://ww2.mathworks.cn/help/simulink/slref/modelexplorer.html)**。
    
2.  指定是将变量保存到 MATLAB 代码文件还是 MAT 文件中。
    
    MATLAB 代码文件格式更容易读取、可编辑，并且支持版本控制。MAT 文件格式为二进制，在性能上更有优势。
    
    如果指定 MATLAB 代码文件格式，模型资源管理器可能会创建一个关联的 MAT 文件，此文件反映 MATLAB 代码文件的名称，但扩展名为 `.mat` 而不是 `.m`。
    
3.  指定文件的名称和位置。
    
4.  如果该文件已存在，模型资源管理器将显示一个对话框，要求您选择以下选项之一：
    
    *   **覆盖整个文件**
        
        *   用选定的变量替换目标文件中按字母顺序存储的所有变量。
            
        
    *   **更新文件中存在的变量并在文件中追加新变量**
        
    *   **仅更新文件中存在的变量**
        
        *   更新现有变量但不添加任何新变量，这样可以消除潜在的外部变量。
            
        
    

要永久存储模型的工作区变量，请创建数据字典，而不是使用基础工作区。请参阅[什么是数据字典？](https://ww2.mathworks.cn/help/simulink/ug/what-is-a-data-dictionary.html)。

### 导入工作区变量

您可以使用模型资源管理器从文件向基础工作区或模型工作区中导入（加载）一组变量。向工作区中导入变量时，模型资源管理器将覆盖现有变量并添加任何新变量。

要向工作区中导入变量，请执行以下操作：

1.  在**模型层次结构**窗格中，右键点击要向其中导入变量的工作区。
    
2.  选择菜单项。
    
3.  在 “从文件导入” 对话框中，选择要从中导入变量的 MATLAB 代码文件或 MAT 文件。
    
    **注意**
    
    如果导入 MATLAB 代码文件，Simulink 还将导入关联的 MAT 文件。
    

## 另请参阅

[`Simulink.findVars`](https://ww2.mathworks.cn/help/simulink/slref/simulink.findvars.html) | [模型资源管理器](https://ww2.mathworks.cn/help/simulink/slref/modelexplorer.html)

## 相关主题

*   [确定在何处存储 Simulink 模型的变量和对象](https://ww2.mathworks.cn/help/simulink/ug/determine-where-to-store-data-for-simulink-models.html)