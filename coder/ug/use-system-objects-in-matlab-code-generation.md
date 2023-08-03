---
url: https://ww2.mathworks.cn/help/coder/ug/use-system-objects-in-matlab-code-generation.html
title: MATLAB 代码生成中的 System object
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-03 11:50:42
tag: 
summary: 在 MATLAB 生成的代码中使用 System object 的特殊注意事项。
---
## MATLAB 代码生成中的 System object

您可以在 MATLAB® 中通过使用 MATLAB Coder™ 从包含 System object 的系统生成 C/C++ 代码。您可以生成高效紧凑的代码以部署在桌面和嵌入式系统中，并提高定点算法的执行速度。

### 代码生成中对 System object 的使用规则和限制

在 MATLAB 生成的代码中使用 System object 时需遵循以下使用规则和限制。

**对象构造和初始化**

*   如果对象存储在持久变量中，则只需将对象句柄嵌入调用 `isempty()` 函数的 `if` 语句中，执行一次 System object 初始化即可。
    
*   将 System object™ 构造函数的参数设置为编译时常量。
    
*   初始化 `releaseImpl` 在 `setupImpl` 结束之前使用的所有 System object 属性。
    
*   在代码生成中，您不能使用其他 MATLAB 类对象将 System object 属性初始化为其默认值。您必须在构造函数中初始化这些属性。
    

**输入和输出**

*   System object 最多可以接受 1024 个输入。每个输入支持最多八个维度。
    
*   输入的数据类型不应更改。
    
*   输入的复 / 实性不应更改。
    
*   如果希望更改输入的大小，则需要确认已启用对可变大小数据的支持。要支持可变大小数据的代码生成，同样需要启用对可变大小数据的支持。默认情况下，在 MATLAB 中启用了对可变大小数据的支持。
    
*   如果可变大小数据超出了 `DynamicMemoryAllocationThreshold` 的值，则软件中预定义的 System object 将不支持可变大小。
    
*   不要将 System object 设置为 [MATLAB Function](https://ww2.mathworks.cn/help/simulink/slref/matlabfunction.html) (Simulink) 模块的输出。
    
*   对于 [MATLAB Function](https://ww2.mathworks.cn/help/simulink/slref/matlabfunction.html) (Simulink) 模块中的任何 System object，不要使用 “将仿真作为工作点保存和恢复” 选项。
    
*   不要将 System object 作为示例输入参数传递给使用 [`codegen`](https://ww2.mathworks.cn/help/coder/ref/codegen.html) 编译的函数。
    
*   不要将 System object 传递给使用 `coder.extrinsic` 函数声明为外部函数（在解释模式下调用的函数）的函数。从外部函数返回的 System object 和自动变为外部对象的示波器 System object 可以用作另一个外部函数的输入。但是，这些函数不会生成代码。
    

**属性**

*   在 MATLAB System 模块中，不能对 System object 的离散状态属性使用可变大小。私有属性可以是可变大小的。
    
*   对象不能用作属性的默认值。
    
*   对不可调属性只能进行一次赋值，包括构造函数中的赋值。
    
*   不可调属性值必须是常量。
    
*   对于定点输入，如果可调属性具有从属数据类型属性，则只能在构造时或锁定对象后设置可调属性。
    
*   对于 `getNumInputsImpl` 和 `getNumOutputsImpl` 方法，如果您基于某个对象属性设置返回参数，则该对象属性必须具有 `Nontunable` 特性。
    

**全局变量**

**方法**

*   仅支持对以下 System object 方法进行代码生成：
    
*   对于您定义的 System object，仅支持对以下方法进行代码生成：
    

### 通过 codegen 使用 System object

您可以在 MATLAB 代码中包含 System object，就像包含任何其他元素一样。然后，您可以使用 [`codegen`](https://ww2.mathworks.cn/help/coder/ref/codegen.html) 命令从 MATLAB 代码编译 MEX 文件（如果您有 MATLAB Coder 许可证，则可以使用此功能）。此编译过程涉及到一些优化，对加速仿真非常有用。有关详细信息，请参阅 [MATLAB Coder 快速入门](https://ww2.mathworks.cn/help/coder/getting-started-with-matlab-coder.html)和 [MATLAB 类](https://ww2.mathworks.cn/help/coder/matlab-classes.html)。

**注意**

大多数（但并非全部）System object 都支持代码生成。有关信息，请参阅特定对象的参考页。

### 通过 MATLAB Function 模块使用 System object

使用 [MATLAB Function](https://ww2.mathworks.cn/help/simulink/slref/matlabfunction.html) (Simulink) 模块，您可以在 Simulink 模型中包含任何 System object 和任何 MATLAB 语言函数。然后，即可基于该模型生成可嵌入代码。System object 为代码生成提供比大多数关联模块更高级别的算法。有关详细信息，请参阅 [用 MATLAB Function 模块在 Simulink 中实现 MATLAB 函数](https://ww2.mathworks.cn/help/simulink/ug/what-is-a-matlab-function-block.html) (Simulink)。

### 通过 MATLAB System 模块使用 System object

使用 [MATLAB System](https://ww2.mathworks.cn/help/simulink/slref/matlabsystem.html) (Simulink) 模块，您可以在 Simulink 模型中包含您使用类定义文件创建的各个 System object。然后，即可基于该模型生成可嵌入代码。有关详细信息，请参阅 [MATLAB System 模块](https://ww2.mathworks.cn/help/simulink/ug/what-is-matlab-system-block.html) (Simulink)。

### System object 和 MATLAB Compiler 软件

MATLAB Compiler™ 软件支持在 MATLAB 函数内使用 System object。编译器产品不支持在 MATLAB 脚本中使用 System object。

## 相关主题

*   [Generate Code That Uses Row-Major Array Layout](https://ww2.mathworks.cn/help/coder/ug/generate-code-that-uses-row-major-data.html)

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