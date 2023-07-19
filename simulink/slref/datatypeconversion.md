---
> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [ww2.mathworks.cn](https://ww2.mathworks.cn/help/simulink/slref/datatypeconversion.html)
> Data Type Conversion 模块可将任何 Simulink 数据类型的输入信号转换为您指定的数据类型。
---
![](https://ww2.mathworks.cn/help/simulink/slref/data_type_conversion_block_icon.png)

**库：**
Simulink / Commonly Used Blocks
Simulink / Signal Attributes
HDL Coder / Commonly Used Blocks
HDL Coder / HDL Floating Point Operations
HDL Coder / Signal Attributes

## 描述

Data Type Conversion 模块可将任何 Simulink® 数据类型的输入信号转换为您指定的数据类型。

### 转换定点信号

在定点数据类型之间转换时，可以通过**输入和输出具有相等的**参数来控制模块的行为。在以下情况下，此参数不会更改模块的行为：

- 输入和输出没有定点数据类型。
- 输入或输出使用具有平凡定标的定点数据类型。

有关定点数的详细信息，请参阅 [Simulink 中的定点数](https://ww2.mathworks.cn/help/fixedpoint/ug/fixed-point-numbers.html) (Fixed-Point Designer)。

要通过保留输入信号的真实值，将信号从一种数据类型转换为另一种数据类型，请选择默认值 “`真实值(RWV)`”。模块会解释由输入和输出的定标施加的限制，并尝试生成具有相等真实值的输出。

要通过对存储的整数值执行定标重新解释来更改输入信号的真实值，请选择 “`存储的整数(SI)`”。在转换过程中，模块将尝试在指定的数据类型范围内保留为信号存储的整数值。最佳做法是使用相同的字长和符号指定输入和输出数据类型。这样做可确保模块仅更改信号的定标。为输入和输出指定不同的符号或字长可能会产生意外的结果，例如范围丢失或意外的符号扩展。有关示例，请参阅[转换 Simulink 模型中的数据类型](https://ww2.mathworks.cn/help/simulink/slref/convert-data-types-in-simulink-models.html)。

如果选择 “`存储的整数(SI)`”，则模块不会对浮点输入信号执行较低级别的位重新解释。例如，如果输入为 `single` 且值为 `5`，则内存中存储该输入的位将通过下列命令以十六进制方式给出。

但是，Data Type Conversion 模块不会将存储的整数值视为 `40a00000`，而是视为真实值 `5`。转换后，输出的存储的整数值为 `5`。

### 转换枚举信号

使用 Data Type Conversion 模块按如下所示转换枚举信号：

1. 将枚举类型的信号转换为任意数值类型的信号。

   Data Type Conversion 模块的所有枚举值输入的基础整数都必须在该数值类型的范围内。否则，仿真过程中将发生错误。
2. 将任意整数类型的信号转换为枚举类型的信号。

   Data Type Conversion 模块的值输入必须与枚举值的基础值匹配。否则，仿真过程中将发生错误。

   您可以启用**对整数溢出进行饱和处理**参数，这样当模块的值输入与枚举值的基础值不匹配时，Simulink 将使用枚举类型的默认值。请参阅[枚举的类型转换](https://ww2.mathworks.cn/help/rtw/ug/enumerations.html#brs2fu9-1) (Simulink Coder)。

在下列情况下，您不能使用 Data Type Conversion 模块：

- 将非整数数值信号转换为枚举信号。
- 将复信号转换为枚举信号，而不管复信号的实部和虚部的数据类型如何。

有关使用枚举类型的信息，请参阅 [Simulink 枚举](https://ww2.mathworks.cn/help/simulink/ug/about-simulink-enumerations.html)。

## 端口

### 输入

全部展开

### **Port_1** — 输入信号

标量 | 向量 | 矩阵 | N 维数组

输入信号，指定为标量、向量、矩阵或 N 维数组。输入可以是任何实数或复数值信号。如果输入为实数，则输出也是实数。如果输入为复数，则输出也是复数。该模块将输入信号转换为您指定的**输出数据类型**。

在转换定点数据类型时，使用**输入和输出具有相等的**参数确定是基于信号的 “`真实值(RWV)`” 还是 “`存储的整数(SI)`” 值进行转换。有关详细信息，请参阅 [转换定点信号](https://ww2.mathworks.cn/help/simulink/slref/datatypeconversion.html#buptsrd)。

**数据类型：** `single` | `double` | `half` | `int8` | `int16` | `int32` | `int64` | `uint8` | `uint16` | `uint32` | `uint64` | `Boolean` | `fixed point` | `enumerated`

### 输出

全部展开

### **Port_1** — 输出信号

标量 | 向量 | 矩阵 | N 维数组

输出信号，转换为您指定的数据类型，具有与输入信号相同的维度。

**数据类型：** `single` | `double` | `half` | `int8` | `int16` | `int32` | `int64` | `uint8` | `uint16` | `uint32` | `uint64` | `Boolean` | `fixed point` | `enumerated`

## 参数

全部展开

### **输出最小值** — 范围检查的最小输出值

`[]` （默认） | 标量

Simulink 检查的输出范围的下限值。

Simulink 使用最小值执行下列操作：

#### 编程用法

<table summary="Simple list"><tbody><tr><td><strong>模块参数</strong>：<code>OutMin</code></td></tr><tr><td><strong>类型</strong>：字符向量</td></tr><tr><td><strong>值</strong>：<code>'[ ]'</code>| 标量</td></tr><tr><td><strong>默认值</strong>：<code>'[ ]'</code></td></tr></tbody></table>

### **输出最大值** — 范围检查的最大输出值

`[]` （默认） | 标量

Simulink 检查的输出范围的上限值。

Simulink 使用最大值执行下列操作：

#### 编程用法

<table summary="Simple list"><tbody><tr><td><strong>模块参数</strong>：<code>OutMax</code></td></tr><tr><td><strong>类型</strong>：字符向量</td></tr><tr><td><strong>值</strong>：<code>'[ ]'</code>| 标量</td></tr><tr><td><strong>默认值</strong>：<code>'[ ]'</code></td></tr></tbody></table>

### **输出数据类型** — 输出数据类型

“`Inherit: Inherit via back propagation`” （默认） | “`double`” | “`single`” | “`half`” | “`int8`” | “`uint8`” | “`int16`” | “`uint16`” | “`int32`” | “`uint32`” | “`int64`” | “`uint64`” | “`boolean`” | “`fixdt(1,16)`” | “`fixdt(1,16,0)`” | “`fixdt(1,16,2^0,0)`” | “`Enum: <class name>`” | “`<数据类型表达式>`”

为输出选择数据类型。该类型可以继承、直接指定或表示为数据类型对象，如 `Simulink.NumericType`。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><strong>模块参数</strong>：<code>OutDataTypeStr</code></td></tr><tr><td><strong>类型：</strong>字符向量</td></tr><tr><td><strong>值：</strong><code>'Inherit: Inherit via back propagation'</code> | <code>'double'</code> | <code>'single'</code> | <code>'half'</code> | <code>'int8'</code> | <code>'uint8'</code> | <code>'int16'</code> | <code>'uint16'</code> | <code>'int32'</code> | <code>'uint32'</code> | <code>'int64'</code> | <code>'uint64'</code> | <code>'fixdt(1,16)'</code> | <code>'fixdt(1,16,0)'</code> | <code>'fixdt(1,16,2^0,0)'</code> | <code>'Enum: &lt;class name&gt;'</code><code>'&lt;data type expression&gt;'</code></td></tr><tr><td><strong>默认值：</strong><code>'Inherit: Inherit via back propagation'</code></td></tr></tbody></table>

### **锁定输出数据类型设置以防止被定点工具更改** — 防止定点工具覆盖输出数据类型

`off` （默认） | `on`

### **输入和输出具有相等的** — 转换定点数据类型的约束

“`真实值(RWV)`” （默认） | “`存储的整数(SI)`”

在定点数据表示的上下文中，指定哪种类型的输入和输出必须相等。

- “`真实值(RWV)`” - 指定希望输入的 “`真实值(RWV)`” 等于输出的 “`真实值(RWV)`”。
- “`存储的整数(SI)`” - 指定希望输入的 “`存储的整数(SI)`” 值等于输出的 “`存储的整数(SI)`” 值。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><strong>模块参数</strong>：<code>ConvertRealWorld</code></td></tr><tr><td><strong>类型：</strong>字符向量</td></tr><tr><td><strong>值：</strong><code>'Real World Value (RWV)' | 'Stored Integer (SI)'</code></td></tr><tr><td><strong>默认值</strong>：<code>'Real World Value (RWV)'</code></td></tr></tbody></table>

### **整数舍入模式** — 指定定点运算的舍入模式

“`向下`” （默认） | “`向上`” | “`收敛`” | “`最邻近值`” | “`舍入`” | “`最简`” | “`零`”

选择下列舍入模式之一。

“`向上`”

将正值和负值朝正无穷方向舍入。等同于 MATLAB® `ceil` 函数。

“`收敛`”

将数值舍入到最邻近的可表示值。如果出现结值，则舍入到最邻近的偶数整数。等同于 Fixed-Point Designer™ `convergent` 函数。

“`向下`”

将正值和负值朝负无穷方向舍入。等同于 MATLAB `floor` 函数。

“`最邻近值`”

将数值舍入到最邻近的可表示值。如果出现结值，则朝正无穷方向舍入。等同于 Fixed-Point Designer `nearest` 函数。

“`舍入`”

将数值舍入到最邻近的可表示值。如果出现结值，则将正数朝正无穷方向舍入，将负数朝负无穷方向舍入。等同于 Fixed-Point Designer `round` 函数。

“`最简`”

自动选择是向负无穷大方向舍入还是向零舍入，以生成尽可能有效的舍入代码。

“`零`”

将数值向零舍入。等同于 MATLAB `fix` 函数。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><strong>模块参数</strong>：<code>RndMeth</code></td></tr><tr><td><strong>类型</strong>：字符向量</td></tr><tr><td><strong>值</strong>：<code>'Ceiling'</code> | <code>'Convergent'</code> | <code>'Floor'</code> | <code>'Nearest'</code> | <code>'Round'</code> | <code>'Simplest'</code> | <code>'Zero'</code></td></tr><tr><td><strong>默认值</strong>：<code>'Floor'</code></td></tr></tbody></table>

#### 另请参阅

有关详细信息，请参阅[舍入](https://ww2.mathworks.cn/help/fixedpoint/ug/rounding.html) (Fixed-Point Designer)。

### **对整数溢出进行饱和处理** — 溢出操作的方法

`off` （默认） | `on`

指定对溢出是进行饱和处理还是绕回处理。

- `off` - 溢出将绕回到数据类型可以表示的合适值。

  例如，数字 130 不适合一个有符号的 8 位整数，因此绕回 -126。
- `on` - 将溢出饱和处理为数据类型能够表示的最小值或最大值。

  例如，一个有符号的 8 位整数的溢出可以饱和处理为 -128 或 127。

**提示**

- 如果您的模型存在可能的溢出，而您希望在生成的代码中进行显式饱和保护，请考虑选中此复选框。
- 如果您希望优化生成的代码的效率，请考虑清除此复选框。

  清除此复选框还可以帮助您避免过度地指定信号超出范围时的处理方式。有关详细信息，请参阅[信号范围错误故障排除](https://ww2.mathworks.cn/help/simulink/ug/signal-ranges.html#brdiku9)。
- 如果选中此复选框，饱和将应用于模块中的每个内部操作，而不仅仅应用于输出或结果。
- 一般情况下，代码生成进程可以检测到何时不可能发生溢出。在这种情况下，代码生成器不会生成饱和代码。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><strong>模块参数</strong>：<code>SaturateOnIntegerOverflow</code></td></tr><tr><td><strong>类型：</strong>字符向量</td></tr><tr><td><strong>值：</strong><code>'off' | 'on'</code></td></tr><tr><td><strong>默认值：</strong><code>'off'</code></td></tr></tbody></table>

### **采样时间** — `-1` 以外的采样时间值

`-1` （默认） | 标量 | 向量

将采样时间指定为 `-1` 以外的值。有关详细信息，请参阅[指定采样时间](https://ww2.mathworks.cn/help/simulink/ug/how-to-specify-the-sample-time.html)。

#### 依存关系

此参数不可见，除非将其显式设置为 `-1` 以外的值。要了解详细信息，请参阅[不建议设置采样时间的模块](https://ww2.mathworks.cn/help/simulink/ug/sampletimehiding.html)。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><strong>模块参数：</strong><code>SampleTime</code></td></tr><tr><td><strong>类型：</strong>字符串标量或字符向量</td></tr><tr><td><strong>默认值：</strong><code>"-1"</code></td></tr></tbody></table>

### **模式** — 选择数据类型模式

`Inherit` （默认） | `Built in` | `Fixed Point`

选择要指定的数据类别。

- “`继承`” - 数据类型的继承规则。选择 `Inherit` 将在右侧启用另一个菜单 / 文本框，您可以在其中选择继承模式。
- “`内置`” - 内置数据类型。选择 `Built in` 将在右侧启用另一个菜单 / 文本框，您可以在其中选择内置数据类型。
- “`定点`” - 定点数据类型。选择 “`定点`” 将启用可用于指定定点数据类型的其他参数。
- “`表达式`” - 计算结果为数据类型的表达式。选择 `Expression` 将在右侧启用另一个菜单 / 文本框，您可以在其中输入表达式。

有关详细信息，请参阅[使用数据类型助手指定数据类型](https://ww2.mathworks.cn/help/simulink/ug/specify-data-types-using-data-type-assistant.html)。

#### 依存关系

要启用此参数，请点击**显示数据类型助手**按钮。

### **数据类型覆盖** — 为此信号指定数据类型覆盖模式

`Inherit` | `Off`

为此信号选择数据类型覆盖模式。

- 当您选择 “`inherit`” 时，Simulink 从信号的上下文（即：从 Simulink 中使用该信号的模块、`Simulink.Signal` 对象或 Stateflow® 图）中继承数据类型覆盖设置。
- 当您选择 “`off`” 时，Simulink 忽略信号上下文的数据类型覆盖设置，并使用为信号指定的定点数据类型。

有关详细信息，请参阅 Simulink 文档中的[使用数据类型助手指定数据类型](https://ww2.mathworks.cn/help/simulink/ug/specify-data-types-using-data-type-assistant.html)。

#### 依存关系

要启用此参数，请将**模式**设置为 “`内置`” 或 “`定点`”。

#### 提示

由于能够关闭单个数据类型的数据类型覆盖，您可以在应用数据类型覆盖时更好地控制模型中的数据类型。例如，您可以使用此选项确保数据类型满足下游模块的要求，而忽略数据类型覆盖设置。

### **符号性** — 指定有符号或无符号

`Signed` （默认） | `Unsigned`

指定定点数据是有符号还是无符号。有符号数据可以表示正值和负值，无符号数据只表示正值。

- “`有符号`”，将定点数据指定为有符号数据。
- “`无符号`”，将定点数据指定为无符号数据。

有关详细信息，请参阅[使用数据类型助手指定数据类型](https://ww2.mathworks.cn/help/simulink/ug/specify-data-types-using-data-type-assistant.html)。

#### 依存关系

要启用此参数，请将**模式**设置为 `Fixed point`。

### **定标** — 定标定点数据的方法

“`最佳精度`” （默认） | “`二进制小数点`” | “`斜率和偏置`”

指定定点数据的定标方法，以避免发生溢出情况并最大限度地减少量化错误。有关详细信息，请参阅[指定定点数据类型](https://ww2.mathworks.cn/help/simulink/ug/specify-data-types-using-data-type-assistant.html#bsnnyna-1)。

#### 依存关系

要启用此参数，请将**模式**设置为 “`定点`”。

### **字长** — 存储量化整数的字的位大小

`16` （默认） | 从 0 到 32 的整数

指定存储量化整数的字的位大小。有关详细信息，请参阅[指定定点数据类型](https://ww2.mathworks.cn/help/simulink/ug/specify-data-types-using-data-type-assistant.html#bsnnyna-1)。

#### 依存关系

要启用此参数，请将**模式**设置为 “`定点`”。

### **小数长度** — 指定定点数据类型的小数长度

`0` （默认） | 标量整数

将定点数据类型的小数长度指定为正整数或负整数。有关详细信息，请参阅[指定定点数据类型](https://ww2.mathworks.cn/help/simulink/ug/specify-data-types-using-data-type-assistant.html#bsnnyna-1)。

#### 依存关系

要启用此参数，请将**定标**设置为 “`二进制小数点`”。

### **斜率** — 指定定点数据类型的斜率。

`2^0` （默认） | 正实数值标量

指定定点数据类型的斜率。有关详细信息，请参阅[指定定点数据类型](https://ww2.mathworks.cn/help/simulink/ug/specify-data-types-using-data-type-assistant.html#bsnnyna-1)。

#### 依存关系

要启用此参数，请将**定标**设置为 “`斜率和偏置`”。

### **偏置** — 指定定点数据类型的偏置。

`0` （默认） | 实数值标量

将定点数据类型的偏置指定为任意实数。有关详细信息，请参阅[指定定点数据类型](https://ww2.mathworks.cn/help/simulink/ug/specify-data-types-using-data-type-assistant.html#bsnnyna-1)。

#### 依存关系

要启用此参数，请将**定标**设置为 “`斜率和偏置`”。

## 模块特性

<table><colgroup><col width="28.70%"><col width="71.30%"></colgroup><tbody><tr><td><p><strong>数据类型</strong></p></td><td><p><code>Boolean</code> | <code>double</code> | <code>enumerated</code> | <code>fixed point</code> | <code>half</code> | <code>integer</code> | <code>single</code></p></td></tr><tr><td><p><strong>直接馈通</strong></p></td><td><p><code>是</code></p></td></tr><tr><td><p><strong>多维信号</strong></p></td><td><p><code>是</code></p></td></tr><tr><td><p><strong>可变大小信号</strong></p></td><td><p><code>是</code></p></td></tr><tr><td><p><strong>过零检测</strong></p></td><td><p><code>否</code></p></td></tr></tbody></table>

## 扩展功能

### C/C++ 代码生成

使用 Simulink® Coder™ 生成 C 代码和 C++ 代码。

### HDL 代码生成

使用 HDL Coder™ 为 FPGA 和 ASIC 设计生成 Verilog 代码和 VHDL 代码。

HDL Coder™ 提供影响 HDL 实现和综合逻辑的额外配置选项。

**注意**

如果您在模型中使用 `double` 数据类型，请使用此模块在 `double` 和 `single` 数据类型之间进行转换。您不能使用该模块在 `double` 和定点数据类型之间进行转换。

**HDL 模块属性**

<table><colgroup><col width="25%"><col width="75%"></colgroup><thead><tr><th colspan="2">通用</th></tr></thead><tbody><tr><td><strong>ConstrainedOutputPipeline</strong></td><td><p>通过移动设计中现有延迟的方式来放置在输出端的寄存器的数量。分布式流水线处理不会重新分发这些寄存器。默认值为 <code>0</code>。有关详细信息，请参阅 <a href="https://ww2.mathworks.cn/help/hdlcoder/ug/block-implementation-parameters.html#btp8gmp" hreflang="en">ConstrainedOutputPipeline</a> (HDL Coder)。</p></td></tr><tr><td><strong>InputPipeline</strong></td><td><p>要在生成的代码中插入的输入流水线阶段数。分布式流水线处理和受限输出流水线处理可以移动这些寄存器。默认值为 <code>0</code>。有关详细信息，请参阅 <a href="https://ww2.mathworks.cn/help/hdlcoder/ug/block-implementation-parameters.html#bsmj7ju-25" hreflang="en">InputPipeline</a> (HDL Coder)。</p></td></tr><tr><td><strong>OutputPipeline</strong></td><td><p>要在生成的代码中插入的输出流水线阶段数。分布式流水线处理和受限输出流水线处理可以移动这些寄存器。默认值为 <code>0</code>。有关详细信息，请参阅 <a href="https://ww2.mathworks.cn/help/hdlcoder/ug/block-implementation-parameters.html#bsmj7ju-26" hreflang="en">OutputPipeline</a> (HDL Coder)。</p></td></tr></tbody></table>

**注意**

借助 HDL Code Advisor，您可以将使用 “`存储的整数(SI)`” 模式并在浮点和定点数据类型之间转换的 Data Type Conversion 模块替换为 Float Typecast 模块。

<table><colgroup><col width="25%"><col width="75%"></colgroup><thead><tr><th colspan="2">本机浮点</th></tr></thead><tbody><tr><td><strong>LatencyStrategy</strong></td><td><p>指定对于浮点运算符是否将设计中的模块映射到 <code>inherit</code>、<code>Max</code>、<code>Min</code>、<code>Zero</code> 或 <code>Custom</code>。默认值为 “<code>inherit</code>”。另请参阅 <a href="https://ww2.mathworks.cn/help/hdlcoder/ug/hdl-block-properties-native-floating-point.html#mw_a99b0822-f2a6-4f5b-aca9-7f7251a67c7c" hreflang="en">LatencyStrategy</a> (HDL Coder)。</p></td></tr><tr><td><strong>NFPCustomLatency</strong></td><td><p>要指定值，请将 <strong>LatencyStrategy</strong> 设置为 <code>Custom</code>。HDL Coder 会增加延迟，其值等于您为 <strong>NFPCustomLatency</strong> 设置指定的值。另请参阅 <a href="https://ww2.mathworks.cn/help/hdlcoder/ug/hdl-block-properties-native-floating-point.html#mw_e29db696-7117-431a-a8b8-4471b8d24ea8" hreflang="en">NFPCustomLatency</a> (HDL Coder)。</p></td></tr></tbody></table>

**枚举数据支持**

此模块支持枚举信号的代码生成。使用此模块将枚举类型的信号转换为任何整数类型，或将任何整数类型的信号转换为枚举类型。

**限制**

如果将 Data Type Conversion 模块配置为双精度到定点数据类型的转换或定点到双精度数据类型的转换，则在代码生成期间会显示警告。

### PLC 代码生成

使用 Simulink® PLC Coder™ 生成结构化文本代码。

### 定点转换

使用 Fixed-Point Designer™ 设计和仿真定点系统。
