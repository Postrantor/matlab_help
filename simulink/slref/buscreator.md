---
> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [ww2.mathworks.cn](https://ww2.mathworks.cn/help/simulink/slref/buscreator.html)

> Bus Creator 模块可将一组输入元素合并成一条总线。您可以将任何元素类型连接到输入端口，包括其他总线。您可以使用 Bus Selector 模块访问总线中的元素。
---
![](https://ww2.mathworks.cn/help/simulink/slref/bus_creator_block_icon.png)

**库：**
Simulink / Commonly Used Blocks
Simulink / Signal Routing
HDL Coder / Signal Routing

## 描述

Bus Creator 模块可将一组输入元素合并成一条总线。您可以将任何元素类型连接到输入端口，包括其他总线。您可以使用 [Bus Selector](https://ww2.mathworks.cn/help/simulink/slref/busselector.html) 模块访问总线中的元素。

总线元素必须具有唯一名称。默认情况下，总线的每个元素都继承连接到 Bus Creator 模块的元素的名称。如果存在重复名称，Bus Creator 模块会将端口号追加到所有输入元素名称。对于没有名称的元素，Bus Creator 模块会生成 `signaln` 形式的名称，其中 `n` 是连接到元素的端口号。当您搜索元素源或选择元素以连接到其他模块时，您可以按名称引用元素。有关元素命名规范，请参阅[信号名称和标签](https://ww2.mathworks.cn/help/simulink/ug/signal-basics.html#brh77af-1)。

Bus Creator 模块不支持混合使用消息和信号元素作为输入。

## 示例

[](https://ww2.mathworks.cn/help/simulink/ug/getting-started-with-buses.html#bve502j-1)

## 端口

### 输入

全部展开

### **Port_1** — 要包含在总线中的输入元素

标量 | 向量 | 矩阵 | 数组 | 总线

输入端口接受要包含在总线中的元素。输入端口的数量由**输入的数目**参数决定。

**数据类型：** `single` | `double` | `half` | `int8` | `int16` | `int32` | `int64` | `uint8` | `uint16` | `uint32` | `uint64` | `string` | `Boolean` | `fixed point` | `enumerated` | `bus`
**复数支持：** 是

### 输出

全部展开

输出总线由输入元素组成。**[以非虚拟总线输出](https://ww2.mathworks.cn/help/simulink/slref/buscreator.html#mw_6cc56f02-2121-401f-ab41-c9f134af652d)**参数指定输出总线是虚拟总线还是非虚拟总线。有关总线类型的信息，请参阅[合成接口规范](https://ww2.mathworks.cn/help/simulink/ug/composite-signal-techniques.html)。

**数据类型：** `single` | `double` | `half` | `int8` | `int16` | `int32` | `int64` | `uint8` | `uint16` | `uint32` | `uint64` | `string` | `Boolean` | `fixed point` | `enumerated` | `bus`
**复数支持：** 是

## 参数

全部展开

### **输入的数目** — 输入元素的数量

`2` （默认） | 整数

输入元素的数量必须为大于或等于 2 的整数。增加输入的数目会向模块添加空的输入端口。在对模型进行仿真之前，请确保为每个输入端口连接了输入元素。

在修改**输入的数目**参数后，点击**刷新**以更新元素列表。

如果所有输入端口都已连接，您可以通过将另一条线连接到 Bus Creator 模块来添加一个输入端口。

![](https://ww2.mathworks.cn/help/simulink/slref/bus_creator_port_sprouting.png)

以交互方式添加端口会更新**输入的数目**参数，并将新元素添加到总线中的元素列表。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><strong>模块参数</strong>：<code>Inputs</code></td></tr><tr><td><strong>类型：</strong>字符向量</td></tr><tr><td><strong>值：</strong>大于或等于 2 的整数，以逗号分隔的元素名称列表</td></tr><tr><td><strong>默认值：</strong><code>'2'</code></td></tr></tbody></table>

默认情况下，`Inputs` 参数指定输入的数目。当您使用它来指定总线中元素的名称时，输入的数目与您指定的元素名称的数目相匹配。

输入元素列表包括进入模块的所有元素，包括嵌套总线的元素。元素旁边的箭头表示输入元素是总线。要显示该总线的内容，请点击箭头。

要突出显示进入模块的元素的来源，请在列表中选择该元素，然后点击**查找**。

如果在对话框打开时更改了元素名称，请点击**刷新**以更新列表中的名称。

要重新排列输出总线中的元素，请使用**向上**和**向下**按钮。您可以在**总线中的元素**列表中选择多个要进行重新排序或删除的顶层相邻元素。

要添加或删除输入元素，请分别点击**添加**或**删除**。然后，点击**应用**或**确定**来更新模块图标。在对模型进行仿真之前，请确保为每个输入端口连接了输入元素。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><strong>模块参数</strong>：<code>Inputs</code></td></tr><tr><td><strong>类型：</strong>字符向量</td></tr><tr><td><strong>值：</strong>大于或等于 2 的整数，以逗号分隔的元素名称列表</td></tr><tr><td><strong>默认值：</strong><code>'2'</code></td></tr></tbody></table>

默认情况下，`Inputs` 参数指定输入的数目。当您使用它来指定总线中元素的名称时，输入的数目与您指定的元素名称的数目相匹配。

### **按名称过滤** — 用于过滤显示的输入元素的搜索词

文本

要过滤显示的输入元素，请输入搜索词。过滤器可执行部分字符串搜索。请勿将搜索词括在引号内。

要访问过滤选项，请点击**按名称过滤**框右侧的**显示过滤选项** ![](https://ww2.mathworks.cn/help/simulink/slref/filter_by_name_button.png) 按钮。

### **启用正则表达式** — 通过正则表达式过滤显示的输入元素的选项

“`off`” （默认） | “`on`”

选择此参数以使用正则表达式或部分搜索字符串过滤显示的输入元素。默认情况下，您可以仅使用部分搜索字符串过滤显示的输入元素。

正则表达式允许您根据输入元素是否匹配某个模式进行过滤。例如，在**按名称过滤**框中输入 `t$`，以显示名称以小写字母 `t` 结尾的所有元素（及其直接父级）。有关详细信息，请参阅[正则表达式](https://ww2.mathworks.cn/help/matlab/matlab_prog/regular-expressions.html)。

#### 依存关系

要访问此参数，请点击**按名称过滤**框右侧的**显示过滤选项** ![](https://ww2.mathworks.cn/help/simulink/slref/filter_by_name_button.png) 按钮。

### **将过滤结果显示为扁平列表** — 将过滤结果显示为平面列表的选项

“`off`” （默认） | “`on`”

选择此参数以扁平列表形式显示过滤结果，该列表使用圆点表示法来反映总线层次结构。默认情况下，过滤结果显示在层次结构树中。

#### 依存关系

要访问此参数，请点击**按名称过滤**框右侧的**显示过滤选项** ![](https://ww2.mathworks.cn/help/simulink/slref/filter_by_name_button.png) 按钮。

### **输出数据类型** — 输出总线的数据类型

“`'Inherit: auto'`” （默认） | “`'Bus: <object name>'`” | “`<data type expression>`”

指定输出总线的数据类型。

如果选择 “`Bus: <object name>`”，请用 [`Simulink.Bus`](https://ww2.mathworks.cn/help/simulink/slref/simulink.bus.html) 对象的名称替换 `<object name>`。编辑模型时，必须可以访问 `Bus` 对象。

要使用**[类型编辑器](https://ww2.mathworks.cn/help/simulink/slref/typeeditor.html)**定义 `Bus` 对象，请点击**显示数据类型助手**按钮 ![](https://ww2.mathworks.cn/help/simulink/slref/show_data_type_asst_button.png)，将**模式**设置为 “`总线对象`”，然后点击**编辑**按钮。

如果选择 “`<data type expression>`”，请指定一个计算结果为 `Bus` 对象的表达式。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><strong>模块参数</strong>：<code>OutDataTypeStr</code></td></tr><tr><td><strong>类型：</strong>字符向量</td></tr><tr><td><strong>值</strong>：<code>'Inherit: auto'</code> | <code>'Bus: &lt;object name&gt;'</code></td></tr><tr><td><strong>默认值：</strong><code>'Inherit: auto'</code></td></tr></tbody></table>

### **要求输入的名称与以上的名称匹配** — 检查输入元素名称是否与对话框中列出的名称相匹配的选项

“`off`” （默认） | “`on`”

在以后的版本中可能会删除此参数。要强制实施强数据定型，请使用**[使用来自输入的名称，而不是来自总线对象的名称](https://ww2.mathworks.cn/help/simulink/slref/buscreator.html#mw_54a02d9d-ed3f-4d5e-b851-7caa32cc2ddc)**参数。

当选中时，此参数检查输入元素名称是否与 “模块参数” 对话框中列出的名称相匹配。如果元素名称不匹配，Simulink® 将返回错误。

#### 依存关系

- 如果您选中**使用来自输入的名称，而不是来自总线对象的名称**，此参数将被忽略。
- 如果您以编程方式更改**[输入数目](https://ww2.mathworks.cn/help/simulink/slref/buscreator.html#mw_b3ffa862-4ff5-4b40-8ced-dfd7f112ae98)**，此参数将还原为 “`off`”。

### **重命名所选信号** — 所选输入元素的新名称

`''` （默认） | 字符向量

### **使用来自输入的名称，而不是来自总线对象的名称** — 使用来自输入元素（而不是总线对象）的名称的选项

“`on`” （默认） | “`off`”

默认情况下，Bus Creator 模块使用输入元素名称作为输出总线元素名称，即使您将 `Simulink.Bus` 对象指定为数据类型也是如此。

要从 `Bus` 对象继承总线元素名称，请清除此参数。清除此参数将：

- 强制应用强数据定型。
- 避免在 `Bus` 对象和模型中多次输入一个元素名称。多次输入名称可能会无意间创建不匹配的元素名称。
- 支持在数组元素之间必须具有一致的元素名称这一总线数组要求。

您也可以通过检查输入元素名称是否与 `Bus` 对象元素名称匹配来强制实施强数据定型。保持此参数处于选中状态，并将**[元素名称不匹配](https://ww2.mathworks.cn/help/simulink/gui/element-name-mismatch.html)**配置参数设置为 “`错误`”。

#### 依存关系

要启用此参数，请将**[输出数据类型](https://ww2.mathworks.cn/help/simulink/slref/buscreator.html#mw_258f3244-c9b5-421f-bcbf-04948943c193)**设置为 `Bus` 对象。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><strong>模块参数</strong>：<code>InheritFromInputs</code></td></tr><tr><td><strong>类型：</strong>字符向量</td></tr><tr><td><strong>值：</strong> <code>'on'</code> | <code>'off'</code></td></tr><tr><td><strong>默认值：</strong><code>'on'</code></td></tr></tbody></table>

### **以非虚拟总线输出** — 输出非虚拟总线的选项

“`off`” （默认） | “`on`”

选择此参数以输出非虚拟总线而不是虚拟总线。

非虚拟总线中的所有元素都必须具有相同的采样时间，即使关联的 `Bus` 对象的元素为某些元素指定了继承采样时间也是如此。任何导致包含不同采样率元素的非虚拟总线的操作都会生成错误。要更改采样时间不同于其他非虚拟总线输入元素的元素或总线的采样时间，请使用 [Rate Transition](https://ww2.mathworks.cn/help/simulink/slref/ratetransition.html) 模块。有关详细信息，请参阅 [Modify Sample Times for Nonvirtual Buses](https://ww2.mathworks.cn/help/simulink/ug/specify-bus-signal-sample-times.html)。

要生成使用 C 结构体定义此模块创建的总线结构的代码，请启用此参数。

#### 依存关系

要启用此参数，请将**[输出数据类型](https://ww2.mathworks.cn/help/simulink/slref/buscreator.html#mw_258f3244-c9b5-421f-bcbf-04948943c193)**设置为 `Bus` 对象。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><strong>模块参数</strong>：<code>NonVirtualBus</code></td></tr><tr><td><strong>类型：</strong>字符向量</td></tr><tr><td><strong>值：</strong><code>'on'</code> | <code>'off'</code></td></tr><tr><td><strong>默认值：</strong><code>'off'</code></td></tr></tbody></table>

## 模块特性

<table><colgroup><col width="28.70%"><col width="71.30%"></colgroup><tbody><tr><td><p><strong>数据类型</strong></p></td><td><p><code>Boolean</code> | <code>bus</code> | <code>double</code> | <code>enumerated</code> | <code>fixed point</code> | <code>half</code> | <code>integer</code> | <code>single</code> | <code>string</code></p></td></tr><tr><td><p><strong>直接馈通</strong></p></td><td><p><code>否</code></p></td></tr><tr><td><p><strong>多维信号</strong></p></td><td><p><code>是</code></p></td></tr><tr><td><p><strong>可变大小信号</strong></p></td><td><p><code>是</code></p></td></tr><tr><td><p><strong>过零检测</strong></p></td><td><p><code>否</code></p></td></tr></tbody></table>

## 提示

对于位于子系统和模型接口上的总线，您可以使用 [Out Bus Element](https://ww2.mathworks.cn/help/simulink/slref/outbuselement.html) 模块，而不是 Bus Creator 模块和 Outport 模块。Out Bus Element 模块能够：

- 减少模块图中线的复杂度和杂乱无章。
- 使增量更改接口更容易。

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

## 版本历史记录

**在 R2006a 之前推出**

全部展开

### R2014b: 不推荐使用**要求输入的名称与以上的名称匹配**参数
