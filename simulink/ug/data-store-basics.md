---
url: https://ww2.mathworks.cn/help/simulink/ug/data-store-basics.html#bra7wyn
title: 数据存储基础知识
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-18 20:59:26
tag: 
summary: 数据存储是一个存储库，可向其中写入数据和从中读取数据，而无需将输入或输出信号直接连接到数据存储。
---
## 数据存储基础知识

_数据存储_是一个存储库，可向其中写入数据和从中读取数据，而无需将输入或输出信号直接连接到数据存储。由于可从各个模型层级访问数据存储，因此子系统和引用模型都可以使用数据存储来共享数据，而无需使用 I/O 端口。

### 何时使用数据存储

当位于模型不同级别的多个信号需要相同的全局值时，当显式连接所有信号会导致模型异常混乱或需要的时间太长而不可行时，数据存储十分有用。数据存储类似于程序中的全局变量，其优缺点也与全局变量类似，例如，使验证变得更加困难。

要在可重用算法的多个实例（例如，自定义库中的子系统或可重用的引用模型）之间共享数据，可以使用数据存储。有关可重用引用模型的数据共享的详细信息，请参阅[在引用模型实例之间共享数据](https://ww2.mathworks.cn/help/simulink/ug/define-referenced-model-inputs-and-outputs-1.html#mw_cf67482d-bf21-4aec-8eda-e8d60bd5d580)。

#### 数据存储和软件验证

数据存储可能会对软件验证产生巨大影响，尤其是在数据耦合和控制领域。仅使用输入和输出端口来传递数据的模型和子系统会在生成的代码中生成干净、明确且易于验证的接口。

数据存储，与任何类型的全局数据一样，会使验证变得更加困难。如果您的开发过程包括软件验证，请在设计过程的早期考虑好使用数据存储可能带来的影响。

有关详细信息，请参阅 RTCA DO-331 文档 “Model-Based Development and Verification Supplement to DO-178C and DO-278A” 中的第 MB.6.3.3.b 节。

#### Goto 和 From 模块作为信号路由备选方法

在某些情况下，您也许可以使用更简单的方法（[Goto](https://ww2.mathworks.cn/help/simulink/slref/goto.html) 模块和 [From](https://ww2.mathworks.cn/help/simulink/slref/from.html) 模块）实现与数据存储类似的结果。数据 Goto/From 链接的主要缺点是，它们通常不能跨越非虚拟子系统边界访问，而经过正确配置的数据存储可在任何地方访问。有关 Goto/From 链接的详细信息，请参阅 [Goto](https://ww2.mathworks.cn/help/simulink/slref/goto.html) 和 [From](https://ww2.mathworks.cn/help/simulink/slref/from.html) 模块参考页。

### 局部和全局数据存储

您可以定义两种类型的数据存储：

*   _局部数据存储_，从模型层次结构中定义该数据存储的级别或以下级别的任何地方都可访问，但引用模型除外。您可以在模型中以图形方式定义局部数据存储，也可以通过创建模型工作区信号对象 (`Simulink.Signal`) 来定义。
    
*   _全局数据存储_，可从整个模型层次结构中访问，包括引用模型。只能在 MATLAB® 基础工作区中使用信号对象来定义全局数据存储。引用模型唯一可以访问的数据存储类型就是全局数据存储。
    

一般情况下，对于模型中要访问数据存储的所有部分，应将数据存储放在可供这些部分访问的最低级别。[数据存储示例](https://ww2.mathworks.cn/help/simulink/ug/model-global-data-using-data-stores.html#bsvy0vx-1)中给出了一些局部和全局数据存储的示例。

有关使用引用模型的信息，请参阅[模型引用](https://ww2.mathworks.cn/help/simulink/model-reference.html)。

### 数据存储诊断

*   [关于数据存储诊断](#bryz3rq)
    
*   [检测访问顺序错误](#brytn1d-1_2)
    
*   [检测多任务访问错误](#bryz6zx-1)
    
*   [检测重复的名称错误](#brytofr-2)
    
*   [模型顾问中的数据存储诊断](#bry0bel)
    

#### 关于数据存储诊断

Simulink® 提供了各种运行时和编译时诊断，可帮助您避免数据存储问题。这些诊断在 “模型配置参数” 对话框以及 Data Store Memory 模块的参数对话框中提供。Simulink 模型顾问会列出更可能因为禁用诊断而出现数据存储错误的各种情况，以为用户提供支持。

#### 检测访问顺序错误

*   [数据存储诊断和加速模式下引用的模型](#bumr3x8)
    
*   [数据存储诊断和 MATLAB Function 模块](#bshpwqn-1)
    

您可以使用数据存储运行时诊断来检测仿真期间发生的意外数据存储读取和写入顺序。您可以将这些诊断应用于所有数据存储，也可以允许每个 Data Store Memory 模块设置自己的值。这些诊断包括：

这些诊断显示在**模型配置参数 > 诊断 > 数据有效性 > Data Store Memory 模块**窗格中，每个诊断可以具有下列值之一：

*   “`全部禁用`” - 对模型访问的所有数据存储禁用此诊断。
    
*   “`全部启用为警告`” - 在 MATLAB 命令行窗口以警告的形式显示诊断。
    
*   “`全部作为错误`” - 停止仿真并在错误对话框中显示诊断。
    
*   “`使用局部设置`” 允许每个 Data Store Memory 模块为此诊断设置自己的值（默认值）。
    

这些诊断还显示在每个 Data Store Memory 模块参数对话框的**诊断**选项卡上。您可以将每个诊断设置为 “`无`”、“`警告`” 或 “`错误`”。由单个模块指定的值仅在对应的配置参数为 “`使用局部设置`” 时才有效。有关详细信息，请参阅 [模型配置参数：数据有效性诊断](https://ww2.mathworks.cn/help/simulink/gui/diagnostics-pane-data-validity.html) 和 [Data Store Memory](https://ww2.mathworks.cn/help/simulink/slref/datastorememory.html) 文档。

最保守的方法是在 > > > 中将所有数据存储诊断设置为 “`全部作为错误`”。但是，此设置并非在所有情况下都是最佳选择，因为它会将有意设置的行为标记为错误。例如，下图显示一个模型利用模块优先级强制 Data Store Read 模块在 Data Store Write 模块之前执行：

![](https://ww2.mathworks.cn/help/simulink/ug/dsmultitask.png)

![](https://ww2.mathworks.cn/help/simulink/ug/dsreadbeforewriteerror.png)

仿真时发生错误，因为在 Data Store Write 模块更新数据存储 `A` 之前，Data Store Read 模块就读取该数据存储。如果相关延迟是有意设置的，则可以通过将全局参数**检测写前读**设置为 “`使用局部设置`”，然后在 Data Store Memory 模块对话框的**诊断**窗格上将该参数设置为 “`无`” 来抑制此错误。如果使用这种方法，除了有意要从诊断中排除的模块外，可在其他所有 Data Store Memory 模块中将此参数设置为 “`错误`”。

**数据存储诊断和加速模式下引用的模型.** 对于在加速模式下引用的模型，如果您为以下 > > > 参数设置的值不是 “`全部禁用`”，Simulink 将忽略这些参数。

*   **检测写前读** (`ReadBeforeWriteMsg`)
    
*   **检测读后写** (`WriteAfterReadMsg`)
    
*   **检测写后写** (`WriteAfterWriteMsg`)
    

可以使用模型顾问来确定加速模式下引用的特定模型，这些模型的上述配置参数将被 Simulink 忽略。

1.  在 Simulink 编辑器中，在**建模**选项卡上，点击**模型顾问**。
    
2.  选择。
    
3.  运行**检查模型引用加速仿真期间忽略的诊断设置**检查。
    

**数据存储诊断和 MATLAB Function 模块.** 对于 MATLAB Function 模块使用的数据存储内存，诊断可能更保守。例如，如果您为 MATLAB 函数传递数据存储内存数组，`A = foo(A)` 这样的优化可能会导致 MATLAB 将整个数组内容都标记为已读或已写，即使只访问了一些元素。

#### 检测多任务访问错误

如果从一个任务中读取数据存储而写入另一个任务，数据完整性可能会受到损害。例如，假设：

1.  某个任务正在写入数据存储。
    
2.  第二个任务中断第一个任务。
    
3.  第二个任务从该数据存储中读取。
    

如果第一个任务在被第二个任务中断时只更新了部分数据存储，数据存储中的数据将不一致。例如，如果值是向量，则其中的某些元素可能在当前时间步中写入，而其余的元素在上一个时间步中写入。如果值包含多字，则可能会处于不一致的状态，甚至不能保证部分正确。

除非您确定任务抢占不会导致数据完整性问题，否则请将编译时诊断**模型配置参数 > 诊断 > 数据有效性 > Data Store Memory 模块 > 多任务数据存储**设置为 “`警告`”（默认值）或 “`错误`”。此诊断可以标记在不同任务中分别进行数据存储读取和写入操作的情况。下图说明将**多任务数据存储**设置为 “`错误`” 时检测到的问题：

![](https://ww2.mathworks.cn/help/simulink/ug/dsmultitask.png)

![](https://ww2.mathworks.cn/help/simulink/ug/dsmultitaskerror.png)

由于数据存储 `A` 在快速任务中写入，在慢速任务中读取，因此报告错误并提供了解决方案。即使数据存储读取或写入发生在条件子系统内部，此诊断也适用。Simulink 可以正确识别正在其中执行任务的模块，并使用该任务来评估诊断。

下图显示上述问题的一种解决方案：在原来以较慢速度访问数据存储的读取操作后面放置一个速率转移模块。

![](https://ww2.mathworks.cn/help/simulink/ug/image012.png)

通过这一改动，数据存储写入可以以较快的速度继续进行。如果在模型中的其他地方必须以较快的速度读取该数据存储，这样做可能很重要。

**多任务数据存储**诊断还适用于引用模型中的数据存储读取和写入。如果两个不同的子模型在不同的任务中对一个数据存储执行读取和写入，当 Simulink 编译它们的共同父模型时将会检测到错误。

#### 检测重复的名称错误

如果在一个模型中重复使用同一个数据存储名称，可能会发生数据存储错误。例如，当不同嵌套作用域中的两个或多个数据存储内存具有相同的数据存储名称时，会发生数据存储重影情况。这种情况下，位于较低级别的数据存储读取或写入模块所引用的数据存储内存可能不是本来想要的数据存储。

为了防止数据存储名称重复而导致的错误，可将编译时诊断**模型配置参数 > 诊断 > 数据有效性 > Data Store Memory 模块 > 重复数据存储名称**设置为 “`警告`” 或 “`错误`”。默认情况下，此诊断的值为 “`无`”，即禁用重复名称检测。下图说明将**重复数据存储名称**设置为 “`错误`” 时检测到的问题：

![](https://ww2.mathworks.cn/help/simulink/ug/dsduplicate.png)

![](https://ww2.mathworks.cn/help/simulink/ug/dsduptopss.png)

![](https://ww2.mathworks.cn/help/simulink/ug/dsdupbottomss.png)

![](https://ww2.mathworks.cn/help/simulink/ug/dsdupdetecterror.png)

子系统层次结构底层的数据存储读取操作引用名为 `A` 的数据存储，而同一个模型中有两个 Data Store Memory 模块具有该名称，因此会报告错误。此诊断可避免认为数据存储读取操作必定会引用模型顶层的 Data Store Memory 模块的臆断。读取操作实际引用的是中间的 Data Store Memory 模块，它在作用域中更接近 Data Store Read 模块。

#### 模型顾问中的数据存储诊断

模型顾问提供了几种可用于数据存储的诊断。有关模型顾问数据存储诊断的信息，请参阅以下部分：

[检查 Data Store Memory 模块的多任务、强类型化和重影问题](https://ww2.mathworks.cn/help/simulink/slref/simulink-checks.html#brt5usf-1)

[检查数据存储模块采样时间是否存在建模错误](https://ww2.mathworks.cn/help/simulink/slref/simulink-checks.html#brs0e0c)

[检查是否为数据存储模块启用了读 / 写诊断](https://ww2.mathworks.cn/help/simulink/slref/simulink-checks.html#brs0gb0-1)

### 指定数据存储的初始值

通常，要为数据存储指定初始值，可以使用与其他模块相同的方法。请参阅[初始化信号和离散状态](https://ww2.mathworks.cn/help/simulink/ug/initializing-signals-and-discrete-states.html)。

对于大多数模块来说，您可以利用标量扩展最大程度减少为非标量信号指定初始值的工作。如果指定标量初始值，信号中的每个元素都将使用该标量。

但是，如果您将 Data Store Memory 模块中的**维度**参数设置为 `-1`（默认值），则不能使用标量扩展，而必须指定与存储的信号具有相同维度的初始值。要利用初始值标量扩展，请将**维度**参数设置为具体的值，例如 `[1 2]` 或 `[1 myDim]`（对于符号维度）。

## 另请参阅

[Data Store Memory](https://ww2.mathworks.cn/help/simulink/slref/datastorememory.html) | [Data Store Read](https://ww2.mathworks.cn/help/simulink/slref/datastoreread.html) | [Data Store Write](https://ww2.mathworks.cn/help/simulink/slref/datastorewrite.html) | [`Simulink.Signal`](https://ww2.mathworks.cn/help/simulink/slref/simulink.signal.html)

## 相关主题

*   [Data Stores in Generated Code](https://ww2.mathworks.cn/help/rtw/ug/data-stores.html) (Simulink Coder)
*   [通过创建数据存储对全局数据建模](https://ww2.mathworks.cn/help/simulink/ug/model-global-data-using-data-stores.html)
*   [Log Data Stores](https://ww2.mathworks.cn/help/simulink/ug/logging-data-stores.html)
*   [数据对象](https://ww2.mathworks.cn/help/simulink/ug/working-with-data-objects.html)
*   [信号基础知识](https://ww2.mathworks.cn/help/simulink/ug/signal-basics.html)

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