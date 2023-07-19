---
url: https://ww2.mathworks.cn/help/simulink/slref/busassignment.html
title: 为指定的总线元素赋予新值 - Simulink
- MathWorks 中国
date: 2023-07-07 11:09:32
tag: #matlab #busassignment #bus
summary: Bus Assignment 模块将输入信号的值赋给所选总线元素。
---
Main Content

- ![](https://ww2.mathworks.cn/help/simulink/slref/bus_assignment_block_icon.png)

**库：**
Simulink / Signal Routing
HDL Coder / Signal Routing

## 描述

Bus Assignment 模块**将输入信号的值赋给所选总线元素**。使用 Bus Assignment 模块可以更改总线元素值，而无需添加 Bus Selector 和 Bus Creator 模块来选择总线元素并将这些元素重新组合为总线。Bus Assignment 模块简化了总线的更新，以反映在一个单独的组件（如子系统或引用模型）中发生的处理。

Bus Assignment 模块将连接到其 Assignment 输入端口的元素赋给连接到其总线输入端口的总线的指定元素。该模块将替换先前分配给这些元素的元素。**此更改不影响总线的构成，只影响元素本身的值**。未替换的信号不受其他元素替换的影响。

被赋值的元素可以是非总线信号或总线，包括总线数组。**新值必须与原始总线中元素的属性匹配**。

默认情况下，Simulink® 修复 Bus Assignment 模块中由于上游总线层次结构发生变化而失效的选择。Simulink 会生成一个警告以强调它修改了模型。要防止 Simulink 自动进行这些修复，请执行下列步骤：

1. 在 Simulink 工具条的**建模**选项卡上，点击**模型设置**。
2. 导航到**诊断** > **连接**窗格。
3. 将**[修复总线选择](https://ww2.mathworks.cn/help/simulink/gui/repair-bus-selections.html)**配置参数设置为 “`未修复的错误`”。

## 限制

- Bus Assignment 模块不支持消息。
- **Bus Assignment 模块不能取代总线数组中的总线**。请改用 [Assignment](https://ww2.mathworks.cn/help/simulink/slref/assignment.html) 模块。有关详细信息，请参阅 [总线数组赋值](https://ww2.mathworks.cn/help/simulink/ug/work-with-array-of-buses-signals.html#bvg276u)。
- Bus Assignment 模块不能取代总线数组中的总线元素。要选择要用 Bus Assignment 模块修改的总线索引，请使用 [Selector](https://ww2.mathworks.cn/help/simulink/slref/selector.html) 模块。然后，将选定的总线用于 Bus Assignment 模块。

## 端口

### 输入

全部展开

输入虚拟或非虚拟总线可以包含 Simulink **支持的任何数据类型的实数值或复数值元素，包括总线对象、定点数据类型和枚举数据类型**。总线也**可以包含总线数组**。

**数据类型：** `single` | `double` | `half` | `int8` | `int16` | `int32` | `int64` | `uint8` | `uint16` | `uint32` | `uint64` | `Boolean` | `fixed point` | `enumerated` | `bus` | `string`
**复数支持：** 是

### **:=** — 要赋给总线元素的新值

标量 | 向量 | 矩阵 | 数组 | 总线

**[要赋值的元素](https://ww2.mathworks.cn/help/simulink/slref/busassignment.html#mw_da988914-b3a5-49dc-9c9f-9e1b3bd160a9)**列表中的每个元素都对应一个赋值端口。端口标签指示对应于该端口的总线元素。对于名为 `signal1` 的元素，端口标签为**:= signal1**。

将要赋给总线元素的信号连接到其对应的赋值端口。连接到赋值端口的信号必须与对应的总线元素具有相同的结构体、数据类型和采样时间。**要更改一个或多个元素的采样时间，请使用 Rate Transition 模块**。有关详细信息，请参阅 [Modify Sample Times for Nonvirtual Buses](https://ww2.mathworks.cn/help/simulink/ug/specify-bus-signal-sample-times.html)。

**数据类型：** `single` | `double` | `half` | `int8` | `int16` | `int32` | `int64` | `uint8` | `uint16` | `uint32` | `uint64` | `Boolean` | `fixed point` | `enumerated` | `bus` | `string`
**复数支持：** 是

### 输出

全部展开

输出虚拟或非虚拟总线包含所选元素所赋予的总线元素值和其他元素的未修改的总线元素值。

**数据类型：** `single` | `double` | `half` | `int8` | `int16` | `int32` | `int64` | `uint8` | `uint16` | `uint32` | `uint64` | `Boolean` | `fixed point` | `enumerated` | `bus` | `string`
**复数支持：** 是

## 参数

全部展开

此 参数 为只读。

选择输入总线中要对其进行操作的元素。

元素名称旁边的箭头表示该元素是嵌套总线。要显示嵌套总线中的元素，请点击该箭头。

选择一个或多个元素后，点击：

- **查找** - 查找所选元素的源。Simulink 将打开并突出显示包含该元素源的系统。
- **选择** - 将所选元素添加到要赋值的元素列表中。有关详细信息，请参阅**[要赋值的元素](https://ww2.mathworks.cn/help/simulink/slref/busassignment.html#mw_da988914-b3a5-49dc-9c9f-9e1b3bd160a9)**。

要刷新列表以反映对输入总线的修改，请点击**刷新**。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><span><strong>模块参数：</strong><code>InputSignals</code></span></td></tr><tr><td><span><strong>类型：</strong>元胞数组 | 由元胞数组组成的元胞数组</span></td></tr><tr><td><span><strong>值：</strong>输入总线元素的名称</span></td></tr><tr><td><span><strong>默认值：</strong>无</span></td></tr></tbody></table>

指定用于过滤输入元素长列表的搜索词。请勿将搜索词括在引号内。过滤器执行部分字符串搜索。

要访问过滤选项，例如使用正则表达式来指定搜索词，请点击**按名称过滤**框右侧的**显示过滤选项**按钮

![](https://ww2.mathworks.cn/help/simulink/slref/filter_by_name_button.png)

### **启用正则表达式** — 使用正则表达式过滤输入元素的选项

off （默认） | on

允许使用 MATLAB® 正则表达式来过滤元素名称。例如，在**按名称过滤**框中输入 `t$`，以显示名称以小写字母 `t` 结尾的所有元素及其直接父级。有关详细信息，请参阅 [正则表达式](https://ww2.mathworks.cn/help/matlab/matlab_prog/regular-expressions.html)。

#### 依存关系

要访问此参数，请点击**按名称过滤**框右侧的**显示过滤选项**

![](https://ww2.mathworks.cn/help/simulink/slref/filter_by_name_button.png)

按钮。

### **将过滤结果显示为扁平列表** — 在扁平列表中显示过滤后的输入元素的选项

off （默认） | on

默认情况下，输入元素的列表以层次结构树形式显示元素。要在使用圆点表示法来反映总线层次结构的扁平列表中显示过滤后的元素，请选择此参数。

#### 依存关系

要访问此参数，请点击**按名称过滤**框右侧的**显示过滤选项**按钮

![](https://ww2.mathworks.cn/help/simulink/slref/filter_by_name_button.png)

。

### **要赋值的元素** — 要被赋予新值的总线元素

元素名称列表

对于此列表中的每个元素，模块都有一个对应的赋值端口。端口标签包含对应元素的名称。

要为元素添加赋值端口，请执行以下操作：

1. 从**总线中的元素**列表中选择一个或多个元素。

   如果您从**总线中的元素**列表中选择了多个元素，则您选择它们的顺序将确定它们在**要赋值的元素**列表中的顺序。
2. （可选）指定希望元素出现在**要赋值的元素**列表中的位置。选择希望添加的元素出现在其下方的元素。如果不选择元素，添加的元素会出现在列表的末尾。
3. 点击**选择**。

要更改赋值端口的顺序，请选择列表中的一个元素或多个连续元素，然后点击**上移**或**下移**。当您更改元素顺序时，端口会保持连接。

要删除赋值端口，请在列表中选择对应的元素，然后点击**删除**。

如果列表中的某个元素不在输入总线中，则该元素名称以三个问号 (`???`) 开头。请修改输入总线以包含指定名称的元素，或从列表中删除该元素。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><span><strong>模块参数</strong>：<code>AssignedSignals</code></span></td></tr><tr><td><span><strong>类型：</strong>字符向量 | 字符串标量</span></td></tr><tr><td><span><strong>值：</strong>以逗号分隔的元素名称列表</span></td></tr><tr><td><span><strong>默认值：</strong>无</span></td></tr></tbody></table>

## 模块特性

<table><colgroup><col width="28.70%"><col width="71.30%"></colgroup><tbody><tr><td><p><strong>数据类型</strong></p></td><td><p><code>Boolean</code> | <code>bus</code> | <code>double</code> | <code>enumerated</code> | <code>fixed point</code> | <code>half</code> | <code>integer</code> | <code>single</code> | <code>string</code></p></td></tr><tr><td><p><strong>直接馈通</strong></p></td><td><p><code>是</code></p></td></tr><tr><td><p><strong>多维信号</strong></p></td><td><p><code>是</code></p></td></tr><tr><td><p><strong>可变大小信号</strong></p></td><td><p><code>是</code></p></td></tr><tr><td><p><strong>过零检测</strong></p></td><td><p><code>否</code></p></td></tr></tbody></table>

## 扩展功能

### C/C++ 代码生成

使用 Simulink® Coder™ 生成 C 代码和 C++ 代码。

实际数据类型或功能支持取决于模块实现。

### HDL 代码生成

使用 HDL Coder™ 为 FPGA 和 ASIC 设计生成 Verilog 代码和 VHDL 代码。

### PLC 代码生成

使用 Simulink® PLC Coder™ 生成结构化文本代码。

### 定点转换

使用 Fixed-Point Designer™ 设计和仿真定点系统。

实际数据类型或功能支持取决于模块实现。

本页内容对您有帮助吗？

Unrated

1 star

2 stars

3 stars

4 stars

5 stars

Unrated 1 star 2 stars 3 stars 4 stars 5 stars

您点击的链接对应于以下 MATLAB 命令：

请在 MATLAB 命令行窗口中直接输入以执行命令。Web 浏览器不支持 MATLAB 命令。

![](https://ww2.mathworks.cn/images/responsive/global/pic-header-mathworks-logo2.svg)

Select a Web Site

Choose a web site to get translated content where available and see local events and offers. Based on your location, we recommend that you select: .

You can also select a web site from the following list:

[Contact your local office](#)
