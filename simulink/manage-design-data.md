---
url: https://ww2.mathworks.cn/help/simulink/manage-design-data.html
title: 管理设计数据
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-18 20:58:57
tag: 
summary: 使用模型工作区、符号、数据对象和数据类来指定变量值
---
使用模型工作区、符号、数据对象和数据类来指定变量值

## 主题

### 数据存储在模型工作区中

*   **[模型工作区](https://ww2.mathworks.cn/help/simulink/ug/using-model-workspaces.html)**  
    将模型使用的变量和对象放置在只有该模型可以访问的工作区中。
*   **[更改模型工作区数据](https://ww2.mathworks.cn/help/simulink/ug/change-model-workspace-data.html)**  
    当您将数据存储在模型工作区中时，可以选择数据源，例如，模型文件或外部 MAT 文件。要在数据源中修改变量，可以使用不同的过程，具体取决于您选择的数据源类型。
*   **[在模型工作区中指定数据源](https://ww2.mathworks.cn/help/simulink/ug/specify-source-for-data-in-model-workspace.html)**  
    将模型使用的变量和对象存储在模型文件或单独文件中。（可选）将变量和对象存储为您可修改的代码。

### 对象和变量中的数据存储

*   **[确定在何处存储 Simulink 模型的变量和对象](https://ww2.mathworks.cn/help/simulink/ug/determine-where-to-store-data-for-simulink-models.html)**  
    模型数据是指您在工作区（例如，基础工作区或数据字典）中创建的对象和变量。选择一种永久存储数据的方式。
*   **[创建、编辑和管理工作区变量](https://ww2.mathworks.cn/help/simulink/ug/create-edit-and-manage-workspace-variables.html)**  
    工作区变量使您能够在模块和模型之间共享信息，例如参数值和数据类型。使用不同工具和方法来创建和操作工作区变量。
*   **[使用模型资源管理器编辑和管理工作区变量](https://ww2.mathworks.cn/help/simulink/ug/workspace-variables-in-model-explorer.html)**  
    查找模型或模块使用的工作区变量、查找使用某个变量的模块、查找未使用的变量，以及重命名模块在任意位置使用的某个变量。在单独的文件中保存和加载变量。
*   **[数据对象](https://ww2.mathworks.cn/help/simulink/ug/working-with-data-objects.html)**  
    通过使用外部数据对象，在模块图外部指定参数、信号和状态的属性，包括参数值。
*   **[符号解析](https://ww2.mathworks.cn/help/simulink/ug/resolving-symbols.html)**  
    您可以控制模型中的模块如何将符号解析为您在工作区中创建的变量和对象。
*   **[定义数据类](https://ww2.mathworks.cn/help/simulink/ug/simulink-data-class-extension-using-matlab-class-syntax.html)**  
    通过创建您自己的数据对象类，自定义您的模型与数据（信号、参数和状态）之间的交互方式。
*   **[Upgrade Level-1 Data Classes](https://ww2.mathworks.cn/help/simulink/ug/upgrading-level-1-data-classes-to-level-2.html)**  
    Simulink® no longer supports level-1 data classes. You must upgrade data classes that you created using the level-1 data class infrastructure, which was removed in a previous release.

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