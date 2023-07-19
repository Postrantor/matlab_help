---
url: https://ww2.mathworks.cn/help/stateflow/ug/structure-operations.html
title: 对 Stateflow 结构体进行索引并赋值
- MATLAB & Simulink
- MathWorks 中国
date: 2023-07-12 16:20:07
tag: 
summary: 此示例说明如何访问和修改 Stateflow® 结构体的内容或 Stateflow 结构体的数组。
---

> [!NOTE]
> .[在 MATLAB 函数中访问 Simulink 总线信号](https://ww2.mathworks.cn/help/stateflow/ug/working-with-structures-and-bus-signals-in-matlab-functions.html)

此示例说明如何访问和修改 Stateflow® 结构体的内容或 Stateflow 结构体的数组。Stateflow 结构体是您从 `` [`Simulink.Bus`](https://ww2.mathworks.cn/help/simulink/slref/simulink.bus.html) (Simulink) `` 对象定义的一种数据类型。您可以使用 Stateflow 结构体将不同大小和类型的数据一起捆绑到一个数据对象中。有关详细信息，请参阅 [Access Bus Signals Through Stateflow Structures](https://ww2.mathworks.cn/help/stateflow/ug/about-stateflow-structures.html)。

### 对子结构体和字段进行索引

要对 Stateflow 结构体的子结构体和字段进行索引，请使用圆点表示法。名称的第一部分标识父结构体。后续部分标识层次化路径上的各个子级。这些子级可以是单个字段，也可以是包含其他结构体（也称为子结构体）的字段。Stateflow 结构体的字段名称与定义该结构体的 `Simulink.Bus` 对象的元素名称一致。当一个字段包含向量、矩阵或数组时，您可以使用图的动作语言支持的索引表示法来访问其元素。

例如，此模型中的图包含一个输入结构体 (`in`)、一个输出结构体 (`out`)、一个局部结构体 (`localbus`) 和一个局部结构体数组 (`subBusArray`)。

- 该图使用 `Simulink.Bus` 对象 `BusObject` 定义输入结构体 `in`、输出结构体 `out` 和局部结构体 `localbus`。这些结构体有四个字段：`sb`、`a`、`b` 和 `c`。
- 字段 `sb` 是从 `Simulink.Bus` 对象 `SubBus` 定义的子结构体。此子结构体有一个名为 `ele` 的字段。
- 该图使用 `Simulink.Bus` 对象 `SubBus` 定义局部结构体数组 `subBusArray`。该数组的大小为 4。该数组中的每个元素均为结构体，其中包含一个名为 `ele` 的字段。

![](https://ww2.mathworks.cn/help/stateflow/ug/stateflowstructureoperationsexample_01_zh_CN.png)

此列表列举了基于此示例的结构体设定将圆点表示法和数值索引进行组合的表达式：

- `in.c` - 输入结构体 `in` 的字段 `c`
- `in.a(1)` - 输入结构体 `in` 的向量场 `a` 的第一个元素
- `out.sb` - 输出结构体 `out` 的子结构体 `sb`
- `out.sb.ele` - 子结构体 `out.sb` 的字段 `ele`
- `out.sb.ele(2,2)` - 子结构体 `out.sb` 的字段 `ele` 的第二行第二列中的元素
- `subBusArray(1)` - 结构体数组 `subBusArray` 的第一个元素
- `subBusArray(1).ele` - 结构体 `subBusArray(1)` 的字段 `ele`
- `subBusArray(1).ele(3,4)` - 结构体 `subBusArray(1)` 的字段 `ele` 的第三行第四列中的元素

由于图使用 MATLAB 作为动作语言，您可以通过使用由括号分隔的从 1 开始的索引来访问此示例中数组的元素。在使用 C 语言作为动作语言的图中，使用由方括号分隔的从 0 开始的索引。有关详细信息，请参阅 [Stateflow 中向量和矩阵的运算](https://ww2.mathworks.cn/help/stateflow/ug/operations-for-vectors-and-matrices.html)。

### 为结构体和字段赋值

您可以写入作用域不是 `Input` 的任何 Stateflow 结构体。您可以为整个结构体、子结构体或单个字段赋值。

- 要将一个结构体赋给另一个结构体，请在基础工作区中基于同一 `Simulink.Bus` 对象定义这两个结构体。
- 要将一个结构体赋给另一个结构体的一个子结构体（或者反之），请基于同一 `Simulink.Bus` 对象定义该结构体和子结构体。
- 要将一个结构体的字段赋给另一个结构体的字段，这些字段必须具有相同的类型和大小。您可以基于不同 `Simulink.Bus` 对象定义 Stateflow 结构体。

例如，此示例中的图进行以下赋值：

- `localbus = sb2abc(in.sb)` - 结构体 `localbus` 和 MATLAB® 函数 `sb2abc` 的输出参数是基于相同的 `Simulink.Bus` 对象 `BusObject` 定义的。该函数将其输入分解为三个组成部分：一个向量、一个 3×2 矩阵和一个标量。该函数将这些组成部分作为其输出的字段 `a`、`b` 和 `c` 返回。有关此函数的详细信息，请参阅[在 MATLAB 函数中访问 Simulink 总线信号](https://ww2.mathworks.cn/help/stateflow/ug/working-with-structures-and-bus-signals-in-matlab-functions.html)。
- `subBusArray(1) = in.sb` - 结构体 `subBusArray(1)` 和子结构体 `in.sb` 基于相同的 `Simulink.Bus` 对象 `SubBus` 进行定义。
- `subBusArray(2) = abc2sb(in)` - 结构体 `subBusArray(2)` 和图形函数 `abc2sb` 的输出参数是基于相同的 `Simulink.Bus` 对象 `SubBus` 定义的。该函数将来自其输入的字段 `a`、`b` 和 `c` 的值组合在一起，并重排入一个 `int8` 类型的 3×3 矩阵中。它将此矩阵以其输出的字段 `ele` 形式返回。
- `subBusArray(3).ele = transpose(in.sb.ele)` - 字段 `subBusArray(3).ele` 的类型和大小与 `transpose(in.sb.ele)` 的结果相同。两者均为 `int8` 类型的 3×3 矩阵。
- `subBusArray(4).ele = int8(magic(3))` - 字段 `subBusArray(4).ele` 的类型和大小与 `int8(magic(3))` 的结果相同。两者均为 `int8` 类型的 3×3 矩阵。
- `out = localbus` - `out` 和 `localbus` 均基于相同的 `Simulink.Bus` 对象 `BusObject` 进行定义。
- `out.sb = subBusArray(idx)` - 子结构体 `out.sb` 和结构体 `subBusArray(idx)` 基于相同的 `Simulink.Bus` 对象 `SubBus` 进行定义。

![](https://ww2.mathworks.cn/help/stateflow/ug/stateflowstructureoperationsexample_02_zh_CN.png)

### 运行仿真

当您对该示例进行仿真时，Stateflow 图使用输入结构体的字段 `sb` 的值来填充输出结构体的字段 `a`、`b`、`c`。参数 `idx` 选择结构体数组 `subBusArray` 的元素以用作输出的子结构体 `sb`。在此示例中，`idx` 等于 2，因此图使用输入结构体的字段 `a`、`b`、`c` 的值来填充子结构体。

![](https://ww2.mathworks.cn/help/stateflow/ug/stateflowstructureoperationsexample_03_zh_CN.png)

当您对 `idx` 使用其他各值时，子结构体 `out.sb` 将分别包含与 `in.sb`、`in.sb` 的转置或一个 3×3 幻方相同的值。

## 另请参阅

[`Simulink.Bus`](https://ww2.mathworks.cn/help/simulink/slref/simulink.bus.html) (Simulink)

## 相关主题

- [Access Bus Signals Through Stateflow Structures](https://ww2.mathworks.cn/help/stateflow/ug/about-stateflow-structures.html)
- [使用圆点表示法标识数据](https://ww2.mathworks.cn/help/stateflow/ug/identify-data-using-dot-notation.html)
- [Stateflow 中向量和矩阵的运算](https://ww2.mathworks.cn/help/stateflow/ug/operations-for-vectors-and-matrices.html)
- [在 MATLAB 函数中访问 Simulink 总线信号](https://ww2.mathworks.cn/help/stateflow/ug/working-with-structures-and-bus-signals-in-matlab-functions.html)

您曾对此示例进行过修改。是否要打开带有您的编辑的示例？

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
