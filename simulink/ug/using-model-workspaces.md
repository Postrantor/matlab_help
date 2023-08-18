---
url: https://ww2.mathworks.cn/help/simulink/ug/using-model-workspaces.html
title: 模型工作区
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-18 20:58:39
tag: 
summary: 将模型使用的变量和对象放置在只有该模型可以访问的工作区中。
---
## 模型工作区

### 模型工作区与 MATLAB 工作区的差异

每个模型都带有自己的工作区以存储变量值。

模型工作区类似于 MATLAB® 基础工作区，不同之处是：

*   模型工作区中的变量仅在该模型的作用域中可见。
    
    如果 MATLAB 工作区和模型工作区都定义了具有相同名称的一个变量，并且该变量不出现在任何中间封装子系统或模型工作区中，则 Simulink® 软件将在模型工作区中使用该变量的值。模型的工作区能够有效地为其提供自己的命名空间，从而允许您为模型创建变量，而不存在与其他模型发生冲突的风险。
    
*   加载模型时，工作区会根据数据源进行初始化。
    
    数据源可以是模型文件、MAT 文件、MATLAB 文件或模型文件中存储的 MATLAB 代码。有关详细信息，请参阅[数据源](https://ww2.mathworks.cn/help/simulink/ug/specify-source-for-data-in-model-workspace.html#f4-140213)。
    
*   您可通过交互方式重新加载和保存 MAT 文件、MATLAB 文件和 MATLAB 代码数据源。
    
*   要将信号对象存储在模型工作区中，需要将该对象的存储类设置为 `Auto`。信号对象包括您创建的 `Simulink.Signal` 和子类。
    
    如果您指定 `Auto` 以外的存储类，则必须将信号对象存储在基础工作区或数据字典中，以确保对象在 Simulink 全局上下文中是唯一的且可供所有模型访问。
    
*   在模型工作区中存储 MATLAB 变量和参数对象（例如 `Simulink.Parameter`）时，存在一些可调性限制。请参阅[特定建模情形下的可调整性注意事项和限制](https://ww2.mathworks.cn/help/simulink/ug/using-tunable-parameters.html#bu1rgli)。此外，如果将 `AUTOSAR.Parameter` 对象存储在模型工作区中，则代码生成器将忽略为该对象指定的存储类。
    

**注意**

当对引用模型中的变量引用进行解析时，其解析方式就如同引用模型的父模型不存在一样。例如，假设引用模型引用的变量同时在父模型的工作区和 MATLAB 工作区中进行了定义，但未在引用模型的工作区中进行定义。在这种情况下，将使用 MATLAB 工作区。

### 内存问题故障排除

当您将工作区变量用作模块参数时，Simulink 会在仿真的编译阶段创建该变量的副本并将变量存储在内存中。这可能会导致您的系统在仿真期间或生成代码过程中将内存耗尽。当您存在以下情形时，便可能出现耗尽系统内存的情况：

此问题不会影响用来表示生成代码中的参数的内存用量。

### 以编程方式操作模型工作区

[`Simulink.ModelWorkspace`](https://ww2.mathworks.cn/help/simulink/slref/simulink.modelworkspace.html) 类的对象可用来描述模型工作区。Simulink 将为您在 Simulink 会话期间打开的每个模型创建此类的一个实例。与此类关联的方法可用于完成与模型工作区相关的各种任务，包括：

*   列出模型工作区中的变量。
    
*   为变量赋值
    
*   对表达式进行计算
    
*   清除模型工作区
    
*   从数据源重新加载模型工作区
    
*   将模型工作区保存到指定的 MAT 文件或 MATLAB 文件
    
*   将工作区保存到工作区所指定的作为其数据源的 MAT 文件或 MATLAB 文件
    

## 另请参阅

[`Simulink.ModelWorkspace`](https://ww2.mathworks.cn/help/simulink/slref/simulink.modelworkspace.html)

## 相关主题

*   [在模型工作区中指定数据源](https://ww2.mathworks.cn/help/simulink/ug/specify-source-for-data-in-model-workspace.html)
*   [更改模型工作区数据](https://ww2.mathworks.cn/help/simulink/ug/change-model-workspace-data.html)
*   [确定在何处存储 Simulink 模型的变量和对象](https://ww2.mathworks.cn/help/simulink/ug/determine-where-to-store-data-for-simulink-models.html)
*   [参数化可重用引用模型的实例](https://ww2.mathworks.cn/help/simulink/ug/parameterize-referenced-models.html)

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