---
url: https://ww2.mathworks.cn/help/simulink/sfg/what-is-an-s-function.html#f6-54075
title: S-Function 概述
- MATLAB & Simulink
- MathWorks 中国
date: 2023-07-31 13:30:30
tag: 
summary: 了解 S-Function 的工作原理。
---
## S-Function 概述

S-Function（系统函数）能够极大地扩充 Simulink® 环境的功能。_S-Function_ 是以 MATLAB®、C、C++ 或 Fortran 语言编写的 Simulink 模块的计算机语言描述。使用 `mex` 实用工具将 C、C++ 和 Fortran S-Function 编译为 MEX 文件（请参阅[编译 C MEX 函数](https://ww2.mathworks.cn/help/matlab/matlab_external/build-an-executable-mex-file.html)）。与其他 MEX 文件一样，S-Function 是动态链接的子例程，MATLAB 执行引擎可以自动加载和执行它们。

S-Function 使用一种称为 S-Function API 的特殊调用语法，使您能够与 Simulink 引擎进行交互。这种交互与该引擎和内置 Simulink 模块之间发生的交互非常相似。

S-Function 遵循一般形式，可以适应连续、离散和混合系统。通过遵循一组简单的规则，您可以在 S-Function 中实现算法，并使用 S-Function 模块将其添加到 Simulink 模型。在编写 S-Function 并将其名称放入 S-Function 模块（在 User-Defined Functions 模块库中提供）后，您可以使用封装来自定义用户界面（请参阅[创建模块封装](https://ww2.mathworks.cn/help/simulink/block-masks.html)）。

如果您有 Simulink Coder™，可以在模型中使用 S-Function 并生成代码。您还可以通过编写目标语言编译器 (TLC) 文件来自定义为 S-Function 生成的代码。有关详细信息，请参阅 [S-Function 和代码生成](https://ww2.mathworks.cn/help/rtw/ug/s-functions-and-code-generation.html) (Simulink Coder)。

### S-Function 的工作原理

S-Function 定义模块在仿真的不同部分（例如初始化、更新、求导、输出和终止）如何工作。在仿真的每一步，仿真引擎都会调用一个方法来完成特定任务。学习 S-Function 基础知识需要基本掌握模块输入、状态和输出之间的数学关系。要了解 S-Function 的工作原理，首先您需要了解 Simulink 如何仿真模型的数学原理，即仿真的各个阶段。有关更多详细信息，请参阅[动态系统的仿真阶段](https://ww2.mathworks.cn/help/simulink/ug/simulating-dynamic-systems.html)。

#### Simulink 模块的数学原理

Simulink 模块由一组输入、一组状态、一组参数和一组输出组成，其中输出是仿真时间、输入、参数和状态的函数。

![](https://ww2.mathworks.cn/help/simulink/sfg/sfun_intro2.png)

以下方程表达了输入、输出、参数、状态和仿真时间之间的数学关系。

其中

#### 仿真阶段

Simulink 模型的执行分阶段进行。在*初始化*阶段，Simulink 引擎将库模块合并到模型中，传播信号宽度、数据类型和采样时间，计算模块参数，确定模块执行顺序，并分配内存。然后，引擎进入*仿真循环*，每经历一次循环就称为一个*仿真步*。在每个仿真步，引擎按照在初始化过程中确定的顺序执行模型中的每个模块。对于每个模块，引擎调用相应函数来计算当前采样时间的模块状态、导数和输出。然后，整个仿真循环继续执行，直到仿真完成。

模型初始化 - 模型为仿真做准备。在此阶段，计算模块参数，确定模块执行顺序，并为每个运算分配内存。在此阶段后，模块经历一个仿真循环。

连续状态和时间的更新 - 仅当模型具有连续状态时才发生。您可以修改子时间步方法（如 `mdlOutputs`、`mdlDerivatives` 和 `mdlZeroCrossing`）来计算输出。

#### S-Function 回调方法

S-Function 由一组 _S-Function 回调方法_ 组成，这些方法执行每个仿真阶段所需的任务。在模型仿真期间的每个仿真阶段，Simulink 引擎都会为模型中的每个 S-Function 模块调用适当的方法。S-Function 回调方法执行的任务包括：

- 编译 - 在此阶段，Simulink 引擎初始化 S-Function。任务包括：

  - 将库模块合并到模型中，并传播信号宽度、数据类型和采样时间
  - 设置输入和输出端口的数目和维度
  - 计算模块参数，并确定模块执行顺序
  - 分配内存和存储区域。
- 计算输出 - 在此状态下，计算输出，直到所有模块输出端口都对当前时间步有效，即所有输出值都在某个误差界限内。
- 更新离散状态 - 在此调用中，模块执行那些在每个时间步执行一次的活动，如更新离散状态。
- 初始化和终止方法 - 这些可选方法只执行一次 S-Function 所需的初始化和终止活动。初始化活动可能包括设置用户数据，或在 S-Function 中初始化状态向量。终止方法执行任何操作，例如当仿真终止或从模型中删除 S-Function 模块时释放所需的内存。
- 积分 - 这适用于具有连续状态和 / 或非采样过零点的模型。如果您的 S-Function 具有连续状态，引擎会在子时间步上调用 S-Function 的输出和导数部分。这是为了让求解器可以计算 S-Function 的状态。如果 S-Function 具有非采样过零点，引擎还会在子时间步上调用 S-Function 的输出和过零点部分，以便能够定位过零点。

要了解关于仿真的术语，尤其关于 S-Function 的术语，请参阅 [S-Function Concepts](https://ww2.mathworks.cn/help/simulink/sfg/s-function-concepts.html)。

### 在模型中使用 S-Function

- [向 S-Function 传递参数](#f6-54365)
- [何时使用 S-Function](#f6-221)

1. 要将 C MEX S-Function 合并到模型中，请从 **Simulink 库浏览器**中拖动一个 S-function 模块。同样，要将 Level-2 MATLAB S-function 合并到模型中，请将 Level-2 MATLAB S-function 模块拖到模型中。
2. 打开**模块参数**对话框，在 **S-Function 名称**字段指定 S-Function 名称，以便为 S-function 模块提供功能。例如，键入 `timestwo` 并点击**应用**以添加一个将传入信号乘以 2 的 C MEX S-Function。

**注意**

如果 MATLAB 路径中包括同名的 C MEX 文件和 MATLAB 文件，则 S-Function 模块在引用该名称时将使用 C MEX 文件。

#### 向 S-Function 传递参数

在 S-function 模块和 Level-2 MATLAB S-Function 的**模块参数**窗口中，都可以指定要传递给对应 S-Function 的参数值。要使用这些字段，您必须知道 S-Function 需要的参数以及函数需要这些参数的顺序。（如果您不知道，请咨询 S-Function 的作者、参考相关文档或源代码。）按照 S-Function 要求的顺序输入各参数，以逗号分隔。参数值可以是常量、在 MATLAB 或模型工作区中定义的变量名称，或是 MATLAB 表达式。

以下示例说明如何使用**参数**字段为 2 级 MATLAB S-Function 输入用户定义的参数。

![](https://ww2.mathworks.cn/help/simulink/sfg/sfun_intro7a.png)

此示例中的模型 `msfcndemo_limintm` 包含示例 S-Function `[msfcn_limintm.m](matlab:edit([matlabroot,'/toolbox/simulink/simdemos/simfeatures/msfcn_limintm.m']);)`。

![](https://ww2.mathworks.cn/help/simulink/sfg/sfun_intro7b.png)

`msfcn_limintm.m` S-Function 接受三个参数：下界、上界和初始条件。如果输入信号的时间积分在下限和上限之间，则 S-Function 输出时间积分；如果时间积分小于下界，则输出下界；如果时间积分大于上界，则输出上界。示例中的对话框指定下界和上界以及初始条件，它们分别为 `-5.0`、`5.0` 和 `0`。示波器显示当输入是振幅为 5 的正弦波时的结果输出。

有关如何在 S-Function 中访问用户指定参数的信息，请参阅 [Processing S-Function Parameters](https://ww2.mathworks.cn/help/simulink/sfg/maintaining-level-1-matlab-s-functions.html#f7-59576) 和 [Handle Errors in S-Functions](https://ww2.mathworks.cn/help/simulink/sfg/error-handling.html)。

您可以使用封装功能为 S-Function 模块创建自定义对话框和图标。通过封装对话框，可以更轻松地为 S-Function 指定其他参数。有关封装的讨论，请参阅[创建模块封装](https://ww2.mathworks.cn/help/simulink/block-masks.html)。

#### 何时使用 S-Function

您可以将 S-Function 用于各种应用，包括：

S-Function 最常见的用途是创建自定义 Simulink 模块（请参阅[模块创建基础知识](https://ww2.mathworks.cn/help/simulink/custom-block-basics.html)）。当使用 S-Function 创建通用模块时，可以在一个模型中多次使用它，并根据模块的每个实例更改参数。

## 另请参阅

[Level-2 MATLAB S-Function](https://ww2.mathworks.cn/help/simulink/slref/level2matlabsfunction.html) | [S-Function Builder](https://ww2.mathworks.cn/help/simulink/slref/sfunctionbuilder.html) | [S-Function](https://ww2.mathworks.cn/help/simulink/slref/sfunction.html) | [MATLAB Function](https://ww2.mathworks.cn/help/simulink/slref/matlabfunction.html)

## 相关主题

- [S-Function Concepts](https://ww2.mathworks.cn/help/simulink/sfg/s-function-concepts.html)
- [S-Function Callback Methods](https://ww2.mathworks.cn/help/simulink/sfg/s-function-callback-methods.html)
- [S-Function Features and Limitations](https://ww2.mathworks.cn/help/simulink/sfg/s-function-features.html)
- [S-Function 和代码生成](https://ww2.mathworks.cn/help/rtw/ug/s-functions-and-code-generation.html) (Simulink Coder)
