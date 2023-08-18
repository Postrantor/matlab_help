---
url: https://ww2.mathworks.cn/help/simulink/slref/from.html
title: 接受来自 Goto 模块的输入 - Simulink
- MathWorks 中国
date: 2023-08-18 20:59:43
tag: 
summary: From 模块从对应的 Goto 模块接受信号，然后将其作为输出传递出去。
---
*   ![](https://ww2.mathworks.cn/help/simulink/slref/from_block_icon.png)
    

![](https://ww2.mathworks.cn/help/simulink/slref/from_block_icon.png)

**库：**  
Simulink / Signal Routing  
HDL Coder / Signal Routing

## 描述

From 模块从对应的 Goto 模块接受信号，然后将其作为输出传递出去。输出的数据类型与来自 Goto 模块的输入的数据类型相同。From 和 Goto 模块允许您将信号从一个模块传递到另一个模块，而无需实际连接它们。

例如，此模型使用 Goto 模块和 From 模块。

![](https://ww2.mathworks.cn/help/simulink/slref/from_goto_example.png)

等效模型将 Sine Wave 模块信号直接传递给 Gain 模块。

![](https://ww2.mathworks.cn/help/simulink/slref/from_goto_example_equivalent.png)

一个 From 模块只能接收来自一个 Goto 模块的信号，尽管 Goto 模块可以将其信号传递给多个 From 模块。

要将 Goto 模块与 From 模块关联，请在 **Goto 标记**参数中输入 Goto 模块标记。

Goto 模块标记的可见性决定了哪些 From 模块能够接收其信号。有关详细信息，请参阅 [Goto](https://ww2.mathworks.cn/help/simulink/slref/goto.html) 和 [Goto Tag Visibility](https://ww2.mathworks.cn/help/simulink/slref/gototagvisibility.html)。模块通过以下方式指明 Goto 模块标记的可见性：

*   局部标记名称用方括号 (`[]`) 括起。
    
*   范围标记名称用花括号 (`{}`) 括起。
    
*   global 标记名称不带其他任何字符。
    

From 模块支持信号标签传播。有关详细信息，请参阅[信号标签传播](https://ww2.mathworks.cn/help/simulink/ug/signal-label-propagation.html)。

## 端口

### 输出

全部展开

### **Port_1** — 来自连接的 Goto 模块的信号  
标量 | 向量 | 矩阵 | N 维数组

来自连接的 Goto 模块的信号，输出信号的维度和数据类型与 Goto 模块的输入信号的维度和数据类型相同。

**数据类型：** `single` | `double` | `half` | `int8` | `int16` | `int32` | `int64` | `uint8` | `uint16` | `uint32` | `uint64` | `Boolean` | `fixed point` | `enumerated` | `bus` | `image`

## 参数

全部展开

### **Goto 标记** — 将信号转发给此模块的 Goto 模块的标记。  
`A` （默认） | `<More Tags...>` | ...

指定将信号转发给此 From 模块的 Goto 模块的标记。要更改该标记，请从下拉列表中选择新标记。

下拉列表显示 From 模块当前可以看到的 Goto 标记。当您第一次在 Simulink® 会话中显示此列表时，列表的末尾会出现一个标签为 `<More Tags...>` 的列表项。如果您选择此列表项，模块将更新标记列表，以包括库子系统（由包含此 From 模块的模型引用）中的 Goto 模块的标记。Simulink 软件会在生成库标记列表时显示一个进度条。Simulink 会一直保存更新的标记列表直到该 Simulink 会话结束，或者直到您再次选择旁边的**更新标记**按钮为止。仅在您上次更新标记列表后模型所引用的库又发生变化的情况下，才需要再次更新当前会话中的标记列表。

**提示**

如果您使用多个 From 和 Goto Tag Visibility 模块引用同一 Goto 标记，您可以同时在所有这些模块中重命名该标记。为此，请使用 Goto 模块对话框中的**全部重命名**按钮。或者，当您更改 [Goto](https://ww2.mathworks.cn/help/simulink/slref/goto.html) 模块图标上的标记时，通过按 **Shift+Enter** 键将新名称传播到所有对应的 From 和 Goto Tag Visibility 模块。

要找到相关 Goto 模块，请使用 From 模块对话框中的 **Goto 源**超链接。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><span><strong>模块参数</strong>：<code>GotoTag</code></span></td></tr><tr><td><span><strong>类型：</strong>字符向量</span></td></tr><tr><td><span><strong>值：</strong><code>'A'</code> | <code>...</code></span></td></tr><tr><td><span><strong>默认值：</strong><code>'A'</code></span></td></tr></tbody></table>

更新对此 From 模块可见的标记列表，包括包含此 From 模块的模型所引用的库中存在的标记。仅在您上次更新标记列表后模型所引用的库又发生变化的情况下，再次更新当前会话中的标记列表。

### **Goto 源** — 所连接的 Goto 模块的路径  
模块路径

连接到此 From 模块的 Goto 模块的路径。点击该路径将在模型中显示并突出显示 Goto 模块。

在 Simulink 编辑器中，选择 From 模块会突出显示对应的 Goto 和 Goto Tag Visibility 模块。

![](https://ww2.mathworks.cn/help/simulink/slref/from_goto_highlighting_from.png)

当对应的 Goto 或 Goto Tag Visibility 模块不在当前图中时，包含该模块的 Subsystem 模块会突出显示。

要在打开的图或新选项卡中显示对应的模块，请选择 From 模块并在省略号上暂停。然后，从操作栏中选择**相关模块**

![](https://ww2.mathworks.cn/help/simulink/slref/related-blocks-button.png)

。当多个模块对应于所选模块时，将打开一个相关模块列表。您可以通过在文本框中输入搜索词来过滤相关模块列表。从列表中选择相关模块后，窗口焦点转至显示该相关模块的打开的图或新选项卡。

### **图标显示** — 要在模块图标上显示的文本  
“`标记`” | “`标记和信号名称`” | “`信号名称`”

指定要在 From 模块图标上显示的文本。选项包括模块标记、模块所代表的信号的名称，或者同时包含标记和信号名称。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><span><strong>模块参数</strong>：<code>IconDisplay</code></span></td></tr><tr><td><span><strong>类型：</strong>字符向量</span></td></tr><tr><td><span><strong>值：</strong><code>'Signal name' | 'Tag' | 'Tag and signal name'</code></span></td></tr><tr><td><span><strong>默认值：</strong><code>'Tag'</code></span></td></tr></tbody></table>

## 模块特性

<table><colgroup><col width="28.70%"><col width="71.30%"></colgroup><tbody><tr><td><p><strong>数据类型</strong></p></td><td><p><code>Boolean</code> | <code>bus</code> | <code>double</code> | <code>enumerated</code> | <code>fixed point</code> | <code>half</code> | <code>integer</code> | <code>single</code> | <code>string</code></p></td></tr><tr><td><p><strong>直接馈通</strong></p></td><td><p><code>否</code></p></td></tr><tr><td><p><strong>多维信号</strong></p></td><td><p><code>是</code></p></td></tr><tr><td><p><strong>可变大小信号</strong></p></td><td><p><code>是</code></p></td></tr><tr><td><p><strong>过零检测</strong></p></td><td><p><code>否</code></p></td></tr></tbody></table>

## 详细信息

全部展开

### 搜索 Goto 标记

要在打开 From 模块对话框时在模型引用的库中搜索 Goto 标记，请使用 `FollowLinksWhenOpeningFromGotoBlocks` 模型参数和 [`set_param`](https://ww2.mathworks.cn/help/simulink/slref/set_param.html) 函数。

*   `'off'` - 不搜索 Goto 标记（默认值）。
    
*   `'on'` - 搜索 Goto 标记。
    

## 扩展功能

### C/C++ 代码生成  
使用 Simulink® Coder™ 生成 C 代码和 C++ 代码。

### HDL 代码生成  
使用 HDL Coder™ 为 FPGA 和 ASIC 设计生成 Verilog 代码和 VHDL 代码。

### PLC 代码生成  
使用 Simulink® PLC Coder™ 生成结构化文本代码。

### 定点转换  
使用 Fixed-Point Designer™ 设计和仿真定点系统。

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

![](https://ww2.mathworks.cn/images/responsive/global/pic-header-mathworks-logo2.svg)

Select a Web Site

Choose a web site to get translated content where available and see local events and offers. Based on your location, we recommend that you select: .

You can also select a web site from the following list:

[Contact your local office](#)