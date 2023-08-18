---
url: https://ww2.mathworks.cn/help/simulink/ug/model-referencing-limitations.html
title: 模型引用的要求和限制
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-18 20:59:34
tag: 
summary: 模型引用在可重用性、仿真模式、封装和调试等功能方面有一定的要求和限制。
---
## 模型引用的要求和限制

在引用模型之前，请考虑模型引用要求和限制。通过提前了解要求和限制，您可以做好相关准备以便成功引用模型。

### 模型重用

您可以在模型层次结构中多次引用一个模型，除非引用模型具有以下任一属性：

*   模型引用另一个设置为单实例的模型。
    
*   模型包含 To File 模块。
    
*   模型包含内部信号或状态，其存储类不支持多实例模型。内部信号和状态的存储类必须设置为 `Auto` 或 `Model default`，并且内部数据的默认存储类必须是多实例存储类。
    
*   模型使用以下任一 Stateflow® 结构体：
    
    *   导出的 Stateflow 图形函数
        
    *   机器级别的数据
        
    
*   引用模型在加速模式下执行，并且包含未内联或已内联但未设置选项 `SS_OPTION_WORKS_WITH_CODE_REUSE` 的 S-Function。
    
*   模型包含函数调用子系统，且该子系统：
    

如果引用模型具有任一上述属性，则模型层次结构中只能显示模型的一个实例。模型必须将**[每个顶层模型允许的实例总数](https://ww2.mathworks.cn/help/simulink/gui/total-number-of-instances-allowed-per-top-model.html)**设置为 “`一个`”。

### 模型封装

您可以在引用模型中使用封装模块。此外，您可以封装引用模型（请参阅 [Create and Reference a Masked Model](https://ww2.mathworks.cn/help/simulink/ug/create-and-reference-a-masked-model.html)）。

要成功使用封装，请考虑以下要求和限制：

*   如果封装指定引用模型的名称，则封装必须直接提供引用模型的名称。您不能使用工作区变量来提供名称。
    
*   Model 模块的封装工作区不可用于引用模型。引用模型使用的任何变量都必须解析为以下任一工作区：
    
*   封装回调不能添加 Model 模块、更改 Model 模块名称或更改 Model 模块仿真模式。
    

### 引用模型中的 S-Function

不同类型的 S-Function 为模型引用提供不同级别的支持。

<table><colgroup><col width="33%"><col width="33%"><col width="33%"></colgroup><thead><tr><th>S-Function 类型</th><th>普通模式下的模型引用</th><th>加速模式下的模型引用和受保护模型</th></tr></thead><tbody><tr><td>1 级 MATLAB S-Function</td><td>不支持</td><td>不支持</td></tr><tr><td>2 级 MATLAB S-Function</td><td>支持</td><td>支持 - 需要 TLC 文件</td></tr><tr><td>手写 C MEX S-Function</td><td><p>支持 - 可以与 TLC 文件内联</p></td><td>支持 - 可以与 TLC 文件内联</td></tr><tr><td>S-Function Builder</td><td>支持</td><td>支持</td></tr><tr><td>代码继承工具</td><td>支持</td><td>支持</td></tr></tbody></table>

当您在引用模型中使用 S-Function 时，请考虑以下要求和限制。

### 模型架构要求和限制

<table><colgroup><col width="35%"><col width="66%"></colgroup><thead><tr><th>元素</th><th>要求和限制</th></tr></thead><tbody><tr><td><span>Goto</span> 和 <span>From</span> 模块</td><td><p><span>Goto</span> 和 <span>From</span> 模块不能跨越模型引用边界。</p></td></tr><tr><td>迭代子系统</td><td><p>如果引用模型包含 <span>Assignment</span> 模块，则仅当 <span>Assignment</span> 模块也包含在一个迭代子系统中时，才能将此 <span>Model</span> 模块放入另一个迭代子系统中。</p></td></tr><tr><td>可配置子系统</td><td><p>在包含 <span>Model</span> 模块的可配置子系统中，请勿在更新过程中更改可配置子系统选择的子系统。</p></td></tr><tr><td><code>InitFcn</code> 回调</td><td><p>顶层模型中的 <code>InitFcn</code> 回调无法更改引用模型使用的参数。</p></td></tr><tr><td>打印引用模型</td><td><p>您不能从顶层模型打印引用模型。</p></td></tr></tbody></table>

### 信号要求和限制

<table><colgroup><col width="35%"><col width="66%"></colgroup><thead><tr><th>信号</th><th>要求和限制</th></tr></thead><tbody><tr><td>从 0 或 1 开始的索引信息的传播</td><td><p>在两种情况下，Simulink 不会将从 0 开始或从 1 开始的索引信息传播到连接至以下模块的引用模型根级端口：</p><div><ul><li><p>接受索引的模块（如 <span>Assignment</span> 模块）</p></li><li><p>生成索引的模块（如 <span>For Iterator</span> 模块）</p></li></ul></div><p><span>Assignment</span> 是接受索引的模块。<span>For Iterator</span> 是生成索引的模块。</p><p>这两种情况会导致缺少传播，从而使 Simulink 无法检测不兼容的索引连接。在这两种情况下：</p><div><ul><li><p>如果引用模型的根级输入端口连接到模型中具有不同的从 0 开始或从 1 开始的索引设置的索引输入，Simulink 不会设置根级 <a href="https://ww2.mathworks.cn/help/simulink/slref/inport.html"><span>Inport</span></a> 模块的从 0 开始或从 1 开始的索引属性。</p></li><li><p>如果引用模型的根级输出端口连接到模型中具有不同的从 0 开始或从 1 开始的索引设置的索引输出，Simulink 不会设置根级 <a href="https://ww2.mathworks.cn/help/simulink/slref/outport.html"><span>Outport</span></a> 模块的从 0 开始或从 1 开始的索引属性。</p></li></ul></div></td></tr><tr><td>异步采样率</td><td><p>仅当引用模型<span><em>同时</em></span>满足以下条件时，才能使用异步采样率：</p><div><ul><li><p>外部源通过根级 <span>Inport</span> 模块驱动异步采样率。</p></li><li><p>根级 <span>Inport</span> 模块输出函数调用信号。请参阅 <a href="https://ww2.mathworks.cn/help/rtw/ref/asynchronoustaskspecification.html"><span>Asynchronous Task Specification</span></a><span role="cross_prod"> (Simulink Coder)</span>。</p></li></ul></div></td></tr><tr><td>用户定义的输入或输出数据类型</td><td><p>引用模型只能输入或输出用户定义的定点数据类型或者由 <code>Simulink.DataType</code> 或 <code>Simulink.Bus</code> 对象定义的数据类型。</p></td></tr><tr><td>总线</td><td><p>如果使用虚拟总线作为引用模型的输入或输出，则总线不能包含可变大小信号元素。请参阅 <a href="https://ww2.mathworks.cn/help/simulink/ug/simplify-subsystem-bus-interfaces.html#btmmqdp" hreflang="en">Use Buses at Model Interfaces</a>。</p></td></tr><tr><td>信号对象</td><td><p>连接到 <span>Model</span> 模块的信号从功能上讲在模块内部和外部都是同一个信号。因此，该信号必须满足一个给定信号最多只能有一个关联信号对象的限制条件。有关详细信息，请参阅 <a href="https://ww2.mathworks.cn/help/simulink/slref/simulink.signal.html"><code>Simulink.Signal</code></a>。</p></td></tr></tbody></table>

### 仿真要求和限制

<table><colgroup><col width="35%"><col width="66%"></colgroup><thead><tr><th>仿真属性</th><th>要求和限制</th></tr></thead><tbody><tr><td>连续采样时间传播</td><td><p>连续采样时间不能传播到未要求明确指定采样时间的 <span>Model</span> 模块。</p></td></tr><tr><td>采样时间和求解器</td><td><p>顶层模型的求解器控制模型层次结构中的所有连续采样时间。例如，对于定步长求解器，引用模型中的所有连续速率以顶层模型的定步长运行。有关采样时间如何影响求解器的信息，请参阅<a href="https://ww2.mathworks.cn/help/simulink/ug/types-of-sample-time.html">采样时间的类型</a>。</p></td></tr><tr><td>状态初始化</td><td><p>如果一个模型引用了其他具有状态的模型，要初始化该模型的状态，请以结构体或带时间的结构体格式指定初始状态。</p></td></tr><tr><td>参数可调性</td><td><p>当您仿真引用了其他模型的模型时，在某些情况下，您将失去模块参数（例如，<span>Gain</span> 模块的<strong>增益</strong>参数）的某些可调性。有关详细信息，请参阅<a href="https://ww2.mathworks.cn/help/simulink/ug/using-tunable-parameters.html#bu1rgli">特定建模情形下的可调整性注意事项和限制</a>。</p></td></tr></tbody></table>

### 代码生成要求和限制

通过预先了解代码生成要求和限制，您可以做好准备，以便为代码生成正确设置模型层次结构。请参阅 [Set Configuration Parameters for Code Generation of Model Hierarchies](https://ww2.mathworks.cn/help/rtw/ug/set-configuration-parameters-for-code-generation-of-model-hierarchies.html) (Simulink Coder) 和 [Code Generation Limitations for Model Reference](https://ww2.mathworks.cn/help/rtw/ug/code-generation-limitations-for-model-reference.html) (Simulink Coder)。

## 相关主题

*   [比较模型组件的功能](https://ww2.mathworks.cn/help/simulink/ug/model-architecture-guidelines.html)
*   [设置模型层次结构的配置参数](https://ww2.mathworks.cn/help/simulink/ug/set-configuration-parameters-for-model-referencing-1.html)
*   [Code Generation Limitations for Model Reference](https://ww2.mathworks.cn/help/rtw/ug/code-generation-limitations-for-model-reference.html) (Simulink Coder)

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