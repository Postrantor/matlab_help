---
url: https://ww2.mathworks.cn/help/simulink/slref/goto.html
title: 将模块输入传递给 From 模块 - Simulink
- MathWorks 中国
date: 2023-08-18 20:59:40
tag: 
summary: Goto 模块将其输入传递给其对应的 From 模块。输入可以是任何数据类型的实数值或复数值信号或向量。From 和 Goto 模块允许您将信号从一个模块传递给另一个模块，而无需实际连接它们。
---
*   ![](https://ww2.mathworks.cn/help/simulink/slref/goto_block_icon.png)
    

![](https://ww2.mathworks.cn/help/simulink/slref/goto_block_icon.png)

**库：**  
Simulink / Signal Routing  
HDL Coder / Signal Routing

## 描述

Goto 模块将其输入传递给其对应的 From 模块。输入可以是任何数据类型的实数值或复数值信号或向量。From 和 Goto 模块允许您将信号从一个模块传递给另一个模块，而无需实际连接它们。

一个 Goto 模块可将其输入信号传递到多个 From 模块，尽管一个 From 模块只能接收来自一个 Goto 模块的信号。Goto 模块的输入传递到与它关联的 From 模块，就像两个模块进行了物理连接一样。

例如，此模型使用 Goto 模块和 From 模块。

![](https://ww2.mathworks.cn/help/simulink/slref/from_goto_example.png)

等效模型将 Sine Wave 模块信号直接传递给 Gain 模块。

![](https://ww2.mathworks.cn/help/simulink/slref/from_goto_example_equivalent.png)

Goto 模块和 From 模块通过使用 Goto 标记进行匹配。

**[标记可见性](https://ww2.mathworks.cn/help/simulink/slref/goto.html#mw_050d7be8-5808-43b2-b67e-55c581edaeda)**参数确定 From 模块可以访问信号的位置。

Goto 模块支持信号标签传播。有关详细信息，请参阅[信号标签传播](https://ww2.mathworks.cn/help/simulink/ug/signal-label-propagation.html)。

## 端口

### 输入

全部展开

### **Port_1** — 输入信号  
标量 | 向量 | 矩阵 | N 维数组

要传递给对应的 From 模块的输入信号，指定为标量、向量、矩阵或 N 维数组。

**数据类型：** `single` | `double` | `half` | `int8` | `int16` | `int32` | `int64` | `uint8` | `uint16` | `uint32` | `uint64` | `Boolean` | `fixed point` | `enumerated` | `bus` | `image`

## 参数

全部展开

### **Goto 标记** — 模块标识符  
“`A`” （默认） | ...

Goto 模块标识符。此参数标识定义了作用域的 Goto 模块。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><span><strong>模块参数</strong>：<code>GotoTag</code></span></td></tr><tr><td><span><strong>类型：</strong>字符向量</span></td></tr><tr><td><span><strong>值：</strong><code>'A'</code> | <code>...</code></span></td></tr><tr><td><span><strong>默认值：</strong><code>'A'</code></span></td></tr></tbody></table>

### **标记可见性** — Goto 模块标记的作用域  
“`局部`” （默认） | “`限定作用域`” | “`全局`”

Goto 模块标记的作用域，指定为 “`局部`”、“`限定作用域`” 或 “`全局`”。当您将此参数设置为 “`限定作用域`” 时，您必须使用 [Goto Tag Visibility](https://ww2.mathworks.cn/help/simulink/slref/gototagvisibility.html) 模块来定义标记可见性的作用域。

*   “`局部`”（默认值）- 使用同一个标记的 From 和 Goto 模块必须在同一个子系统中。局部标记名称用方括号 (`[]`) 括起。
    
*   “`限定作用域`” - 使用相同标记的 From 和 Goto 模块必须符合以下条件：
    
    *   在同一子系统中。
        
    *   在模型层次结构中位于 Goto Tag Visibility 模块之下的任何级别且不会超出非虚拟子系统边界。换句话说，它们必须在原子子系统、条件执行子系统或函数调用子系统或模型引用的边界内。
        
    
    范围标记名称用花括号 (`{}`) 括起。
    
*   “`全局`” - 使用同一个标记的 From 和 Goto 模块可以在模型中的任意位置，但超出非虚拟子系统边界的位置除外。
    

From-Goto 模块连接不能穿过非虚拟子系统边界的规则存在以下例外情况。与一个条件执行子系统的状态端口连接的 Goto 模块，对另一个条件执行子系统中的 From 模块是可见的。

**注意**

封装系统中的 “`限定作用域`” Goto 模块仅在该子系统以及它包含的非虚拟子系统中是可见的。如果您运行或更新模块图，而有一个 Goto Tag Visibility 模块在模块图中的级别高于封装子系统中对应的 “`限定作用域`”Goto 模块，Simulink® 将生成错误。

当使用相同标记名称的 Goto 和 From 模块在同一个子系统中时，请使用 local 标记。当使用相同标记名称的 Goto 和 From 模块在不同的子系统中时，您必须使用 global 或 scoped 标记。如果您将某个标记定义为 global，则所有使用该标记的模块都访问同一个信号。定义为 scoped 的标记可在模型中的多个位置使用。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><span><strong>模块参数</strong>：<code>TagVisibility</code></span></td></tr><tr><td><span><strong>类型：</strong>字符向量</span></td></tr><tr><td><span><strong>值：</strong><code>'local' | 'scoped' | 'global'</code></span></td></tr><tr><td><span><strong>默认值：</strong><code>'local'</code></span></td></tr></tbody></table>

### **图标显示** — 要在模块图标上显示的文本  
“`标记`” （默认） | “`信号名称`” | “`标记和信号名称`”

指定要在模块图标上显示的文本。选项包括模块标记、模块所代表的信号的名称，或者同时包含标记和信号名称。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><span><strong>模块参数</strong>：<code>IconDisplay</code></span></td></tr><tr><td><span><strong>类型：</strong>字符向量</span></td></tr><tr><td><span><strong>值：</strong><code>'Signal name' | 'Tag' | 'Tag and signal name'</code></span></td></tr><tr><td><span><strong>默认值：</strong><code>'Tag'</code></span></td></tr></tbody></table>

重命名 Goto 标记。新名称会传播给**对应的模块**框中列出的所有 From 和 Goto Tag Visibility 模块。

或者，当您更改 Goto 模块图标上的标记时，通过按 **Shift+Enter** 键将新名称传播到所有对应的 From 和 Goto Tag Visibility 模块。

### **对应的模块** — 连接到此 Goto 模块的模块  
模块路径 | ...

与此 Goto 模块连接的 From 模块和 Goto Tag Visibility 模块的列表。点击列表中的某个项目，以显示并突出显示对应的 From 和 Goto Tag Visibility 模块。

或者，在 Simulink 编辑器中，选择 Goto 模块以突出显示对应的 From 和 Goto Tag Visibility 模块。

![](https://ww2.mathworks.cn/help/simulink/slref/from_goto_highlighting_goto.png)

当对应的 From 或 Goto Tag Visibility 模块不在当前图中时，包含该模块的 Subsystem 模块会突出显示。

要在打开的图或新选项卡中显示对应的模块，请选择 Goto 模块并在省略号上暂停。然后，从操作栏中选择**相关模块**

![](https://ww2.mathworks.cn/help/simulink/slref/related-blocks-button.png)

。当多个模块对应于所选模块时，将打开一个相关模块列表。您可以通过在文本框中输入搜索词来过滤相关模块列表。从列表中选择相关模块后，窗口焦点转至显示该相关模块的打开的图或新选项卡。

## 模块特性

<table><colgroup><col width="28.70%"><col width="71.30%"></colgroup><tbody><tr><td><p><strong>数据类型</strong></p></td><td><p><code>Boolean</code> | <code>bus</code> | <code>double</code> | <code>enumerated</code> | <code>fixed point</code> | <code>half</code> | <code>integer</code> | <code>single</code> | <code>string</code></p></td></tr><tr><td><p><strong>直接馈通</strong></p></td><td><p><code>否</code></p></td></tr><tr><td><p><strong>多维信号</strong></p></td><td><p><code>是</code></p></td></tr><tr><td><p><strong>可变大小信号</strong></p></td><td><p><code>是</code></p></td></tr><tr><td><p><strong>过零检测</strong></p></td><td><p><code>否</code></p></td></tr></tbody></table>

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