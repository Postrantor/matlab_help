---
url: https://ww2.mathworks.cn/help/simulink/ug/specify-source-for-data-in-model-workspace.html
title: 在模型工作区中指定数据源
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-18 21:00:44
tag: 
summary: 将模型使用的变量和对象存储在模型文件或单独文件中。（可选）将变量和对象存储为您可修改的代码。
---
## 在模型工作区中指定数据源

当您使用模型工作区来包含模型使用的变量时，您可以选择将变量存储在以下某个数据源中：

*   模型文件，它可以存储静态变量定义。
    
*   单独的 MAT 文件或 MATLAB® 文件。您可以随时将变量从外部文件重新加载到模型工作区中。
    
*   您自己的用于创建变量的自定义 MATLAB 代码。您可以将代码保存为模型文件的一部分，并随时重新加载代码。
    

要为模型工作区指定数据源，请在模型资源管理器中使用 “模型工作区” 对话框。要显示模型工作区的对话框，请执行下列操作：

1.  打开模型资源管理器。在**建模**选项卡上，点击**模型数据编辑器**。
    
2.  在**模型层次结构**窗格中，右键点击模型工作区。
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/data_sources.png)
    
3.  选择菜单项，将打开 “模型工作区” 对话框。
    

要使用 MATLAB 命令更改模型工作区中的数据，请参阅[使用 MATLAB 命令更改工作区数据](https://ww2.mathworks.cn/help/simulink/ug/change-model-workspace-data.html#f4-140177)。

### 数据源

“模型工作区” 对话框中的**数据源**字段包含工作区的以下数据源选项：

### MAT 文件和 MATLAB 文件源代码管理

选择 “`MAT 文件`” 或 “`MATLAB 文件`” 作为工作区的**数据源**会导致 “模型工作区” 对话框显示其他控制项。

![](https://ww2.mathworks.cn/help/simulink/ug/mat_file_workspace.png)

#### 文件名

指定作为所选工作区的数据源的 MAT 文件或 MATLAB 文件的文件名或路径名称。如果指定文件名，则名称必须位于 MATLAB 路径上。

#### 从数据源重新初始化

清空工作区并从**文件名**字段指定的 MAT 文件或 MATLAB 文件重新加载数据。

#### 保存到数据源

将工作区保存在**文件名**字段指定的 MAT 文件或 MATLAB 文件中。

### MATLAB 代码源代码管理

选择 “`MATLAB 代码`” 作为工作区的**数据源**会导致 “模型工作区” 对话框显示其他控制项。

![](https://ww2.mathworks.cn/help/simulink/ug/matlab_code_workspace.png)

#### MATLAB 代码

指定用于初始化所选工作区的 MATLAB 代码。要更改初始化代码，请编辑此字段，然后在对话框中选择**从源重新初始化**按钮以清空工作区并执行修改后的代码。

#### 从数据源重新初始化

清空工作区并执行 **MATLAB 代码**字段的内容。

#### 创建模型封装

通过封装模型，可以控制模型用户如何与模型参数交互。有关详细信息，请参阅 [Introduction to System Mask](https://ww2.mathworks.cn/help/simulink/ug/systeml-mask.html)。

## 相关主题

*   [确定在何处存储 Simulink 模型的变量和对象](https://ww2.mathworks.cn/help/simulink/ug/determine-where-to-store-data-for-simulink-models.html)
*   [模型工作区](https://ww2.mathworks.cn/help/simulink/ug/using-model-workspaces.html)
*   [更改模型工作区数据](https://ww2.mathworks.cn/help/simulink/ug/change-model-workspace-data.html)

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