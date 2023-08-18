---
url: https://ww2.mathworks.cn/help/simulink/ug/change-model-workspace-data.html
title: 更改模型工作区数据
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-18 21:00:48
tag: 
summary: 当您将数据存储在模型工作区中时，可以选择数据源，例如，模型文件或外部 MAT 文件。要在数据源中修改变量，可以使用不同的过程，具体取决于您选择的数据源类型。
---
## 更改模型工作区数据

当您使用模型工作区来包含模型使用的变量时，可以选择用于存储变量的数据源，例如，模型文件或外部 MAT 文件。要在数据源中修改变量，可以使用不同的过程，具体取决于您选择的数据源类型。

### 更改其数据源是模型文件的工作区数据

如果模型工作区的数据源是模型文件，您可以使用模型资源管理器或 MATLAB® 命令修改存储的变量（请参阅[使用 MATLAB 命令更改工作区数据](https://ww2.mathworks.cn/help/simulink/ug/change-model-workspace-data.html#f4-140177)）。

例如，要在模型工作区中创建变量：

1.  打开模型资源管理器。在**建模**选项卡上，点击**模型资源管理器**或按 **Ctrl+H**。
    
2.  在模型资源管理器的**模型层次结构**窗格中，展开模型的节点，并选择模型工作区。
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/model_workspace.png)
    
3.  选择 > 。
    
    类似地，您可以使用**添加**菜单或工具栏向模型工作区中添加 [`Simulink.Parameter`](https://ww2.mathworks.cn/help/simulink/slref/simulink.parameter.html) 对象。
    

要更改模型工作区变量的值：

1.  打开模型资源管理器。在**建模**选项卡上，点击**模型资源管理器**。
    
2.  在模型资源管理器的**模型层次结构**窗格中，选择模型工作区。
    
3.  在**目录**窗格中，选择变量。
    
4.  在**目录**窗格或**对话框**窗格中，编辑显示的值。
    

要删除模型工作区变量：

1.  打开模型资源管理器。在**建模**选项卡上，点击**模型资源管理器**。
    
2.  在模型资源管理器的**模型层次结构**窗格中，选择模型工作区。
    
3.  在**目录**窗格中，选择变量。
    
4.  选择 > 。
    

### 更改其数据源是 MAT 文件或 MATLAB 文件的工作区数据

您可以使用模型资源管理器或 MATLAB 命令修改其数据源是 MAT 文件或 MATLAB 文件的工作区数据。

要做出永久性更改，请在 “模型工作区” 对话框中，使用**保存到源**按钮，将所做更改保存到 MAT 文件或 MATLAB 文件。

1.  打开模型资源管理器。在**建模**选项卡上，点击**模型资源管理器**。
    
2.  在模型资源管理器的**模型层次结构**窗格中，右键点击工作区。
    
3.  选择菜单项。
    
4.  在 “模型工作区” 对话框中，使用**保存到源**按钮，将所做更改保存到 MAT 文件或 MATLAB 文件。
    

要放弃对工作区所做的更改，请在 “模型工作区” 对话框中，使用**从源重新初始化**按钮。

### 更改其数据源是 MATLAB 代码的工作区数据

更改以 MATLAB 代码为数据源的数据的最安全方式是，编辑数据源然后再重新加载它。编辑 MATLAB 代码，然后在 “模型工作区” 对话框中，使用**从源重新初始化**按钮清空工作区，然后重新执行该代码。

要保存并重新加载因编辑 MATLAB 代码源或工作区变量本身而得到的其他版本的工作区，请参阅[导出工作区变量](https://ww2.mathworks.cn/help/simulink/ug/workspace-variables-in-model-explorer.html#bso5crg)和[导入工作区变量](https://ww2.mathworks.cn/help/simulink/ug/workspace-variables-in-model-explorer.html#bso5csa)。

### 使用 MATLAB 命令更改工作区数据

要使用 MATLAB 命令更改模型工作区中的数据，请首先获取当前所选模型的工作区：

```
hws = get_param(bdroot, 'modelworkspace');


```

此命令将返回 [`Simulink.ModelWorkspace`](https://ww2.mathworks.cn/help/simulink/slref/simulink.modelworkspace.html) 对象的句柄，该对象的属性指定用于初始化模型工作区的数据源。编辑属性以更改数据源。

使用工作区方法执行以下操作：

*   列出、设置和清除变量
    
*   计算工作区中的表达式
    
*   保存和重新加载工作区
    

例如，以下 MATLAB 代码将创建指定模型工作区中的模型参数的变量，保存参数，修改其中一个参数，然后重新加载工作区以将其恢复为之前的状态。

```
hws = get_param(bdroot, 'modelworkspace');
hws.DataSource = 'MAT-File';
hws.FileName = 'params';
hws.assignin('pitch', -10);
hws.assignin('roll', 30);
hws.assignin('yaw', -2);
hws.saveToSource;
hws.assignin('roll', 35);
hws.reload;


```

要以编程方式访问变量以扫描模块参数值，请考虑使用 `Simulink.SimulationInput` 对象，而不是通过模型工作区的编程接口修改变量。请参阅[优化、估计和扫描模块参数值](https://ww2.mathworks.cn/help/simulink/ug/optimize-estimate-and-sweep-block-parameter-values.html)。

### 创建模型封装

通过封装模型，可以控制模型用户如何与模型参数交互。有关详细信息，请参阅 [Introduction to System Mask](https://ww2.mathworks.cn/help/simulink/ug/systeml-mask.html)。

## 相关主题

*   [确定在何处存储 Simulink 模型的变量和对象](https://ww2.mathworks.cn/help/simulink/ug/determine-where-to-store-data-for-simulink-models.html)
*   [模型工作区](https://ww2.mathworks.cn/help/simulink/ug/using-model-workspaces.html)
*   [在模型工作区中指定数据源](https://ww2.mathworks.cn/help/simulink/ug/specify-source-for-data-in-model-workspace.html)

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