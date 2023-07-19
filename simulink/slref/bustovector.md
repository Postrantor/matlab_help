---
url: https://ww2.mathworks.cn/help/simulink/slref/bustovector.html
title: 将虚拟总线转换为向量 - Simulink MathWorks 中国
date: 2023-06-25 12:55:09
tag:
summary: Bus to Vector 模块将虚拟总线转换为向量信号。
---
#bus #bus_to_vector

Main Content

![](https://ww2.mathworks.cn/help/simulink/slref/bus_to_vector_block_icon.png)

**库：**
Simulink / Signal Attributes
HDL Coder / Signal Attributes

## 描述

Bus to Vector 模块将虚拟总线转换为向量信号。输入总线必须由具有相同的数据类型、信号类型和采样模式的标量或一维、行或列向量组成。如果输入总线包含行向量或列向量，则将分别输出一个行向量或一个列向量。否则，输出是一维数组。

Bus to Vector 模块仅用于将隐式的总线到向量转换替换为显式转换。如果想在不手动插入 Bus to Vector 模块的情况下识别和纠正用作向量的总线，您可以使用模型顾问检查[检查被视为向量的总线信号](https://ww2.mathworks.cn/help/simulink/slref/simulink-checks.html#bvjmey9-1)。您也可以使用 [`Simulink.BlockDiagram.addBusToVector`](https://ww2.mathworks.cn/help/simulink/slref/simulink.blockdiagram.addbustovector.html) 函数，它会在所需的位置自动插入 Bus to Vector 模块。

**注意**

如果您在 R2007a 之前的 Simulink® 产品版本中使用**另存为**保存模型，则每个 Bus to Vector 模块会被替换为不输出任何内容的空子系统。要在这种情况下使用模型，您必须重新连接，否则就需要纠正每个用来包含 Bus to Vector 模块但现在被空子系统中断的路径。

## 端口

### 输入

全部展开

### **Port_1** — 要转换为向量的总线

标量 | 向量 | 总线

输入总线必须由具有相同的数据类型、信号类型和采样模式的标量或一维、行或列向量组成。如果输入是非总线信号，则该模块不进行转换。

**数据类型：** `single` | `double` | `half` | `int8` | `int16` | `int32` | `int64` | `uint8` | `uint16` | `uint32` | `uint64` | `Boolean` | `fixed point` | `enumerated`
**复数支持：** 是

### 输出

全部展开

输出向量的维数取决于输入总线元素的维数。如果输入总线包含行向量或列向量，此模块将分别输出一个行向量或一个列向量。否则，输出是一维向量。

**数据类型：** `single` | `double` | `half` | `int8` | `int16` | `int32` | `int64` | `uint8` | `uint16` | `uint32` | `uint64` | `Boolean` | `fixed point` | `enumerated`
**复数支持：** 是

## 模块特性

<table><colgroup><col width="28.70%"><col width="71.30%"></colgroup><tbody><tr><td><p><strong>数据类型</strong></p></td><td><p><code>Boolean</code> | <code>bus</code> | <code>double</code> | <code>enumerated</code> | <code>fixed point</code> | <code>half</code> | <code>integer</code> | <code>single</code></p></td></tr><tr><td><p><strong>直接馈通</strong></p></td><td><p><code>否</code></p></td></tr><tr><td><p><strong>多维信号</strong></p></td><td><p><code>否</code></p></td></tr><tr><td><p><strong>可变大小信号</strong></p></td><td><p><code>否</code></p></td></tr><tr><td><p><strong>过零检测</strong></p></td><td><p><code>否</code></p></td></tr></tbody></table>

## 扩展功能

### C/C++ 代码生成

使用 Simulink® Coder™ 生成 C 代码和 C++ 代码。

实际数据类型或功能支持取决于模块实现。

### HDL 代码生成

使用 HDL Coder™ 为 FPGA 和 ASIC 设计生成 Verilog 代码和 VHDL 代码。

### 定点转换

使用 Fixed-Point Designer™ 设计和仿真定点系统。

实际数据类型或功能支持取决于模块实现。

## 另请参阅

模块
Bus Creator | Bus Selector | Mux | Data Type Conversion
函数
Simulink.BlockDiagram.addBusToVector
主题
Identify Automatic Bus Conversions
合成接口规范
