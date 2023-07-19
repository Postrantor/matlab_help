---
url: https://ww2.mathworks.cn/help/stateflow/ug/working-with-structures-and-bus-signals-in-matlab-functions.html
title: 在 MATLAB 函数中访问 Simulink 总线信号
- MATLAB & Simulink
- MathWorks 中国
date: 2023-07-12 16:26:11
tag: 
summary: 此示例说明如何使用 MATLAB 和 Stateflow® 结构体在 MATLAB® 函数中读取和写入 Simulink® 总线信号。
---
此示例说明如何使用 MATLAB 和 Stateflow® 结构体在 MATLAB® 函数中读取和写入 Simulink® 总线信号。MATLAB 结构体使您能够将不同大小和类型的数据捆绑到一个变量中。您可以创建一个 MATLAB 结构体来实现以下目的：

- 将相关数据存储在 MATLAB 函数的一个局部变量或持久变量中
- 读取或写入局部 Stateflow 结构体
- 在输入或输出端口与 Simulink 总线信号对接

MATLAB 函数仅支持非虚拟总线。有关详细信息，请参阅[合成接口规范](https://ww2.mathworks.cn/help/simulink/ug/composite-signal-techniques.html) (Simulink)。

### 在 MATLAB 函数中定义结构体

在此示例中，Stateflow 图处理来自一个 Simulink 总线信号的数据，并将结果输出到另一个 Simulink 总线信号。输入和输出总线信号都由 [`Simulink.Bus`](https://ww2.mathworks.cn/help/simulink/slref/simulink.bus.html) (Simulink) 对象 `BusObject` 定义。这些总线有四个字段：`sb`、`a`、`b` 和 `c`。字段 `sb` 也是由 `Simulink.Bus` 对象 `SubBus` 定义的总线信号。它有一个名为 `ele` 的字段。

![](https://ww2.mathworks.cn/help/stateflow/ug/connectmatlabstructstosimulinkbusexample_01_zh_CN.png)

在图中，Simulink 总线信号与 Stateflow 结构体 `in` 和 `out` 对接。函数 `sb2abc` 从输入结构体中提取信息，并将其存储在局部 Stateflow 结构体 `localbus` 中。然后，该图将局部结构体的值和结构体数组 `subBusArray` 的元素之一组合起来，写入输出结构体。有关访问和修改 Stateflow 结构体或 Stateflow 结构体数组的内容的详细信息，请参阅[对 Stateflow 结构体进行索引并赋值](https://ww2.mathworks.cn/help/stateflow/ug/structure-operations.html)。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/ConnectMATLABStructsToSimulinkBusExample_02.png)

MATLAB® 函数 `sb2abc` 接受 `SubBus` 类型的 Stateflow 结构体，并返回 `BusObject` 类型的 Stateflow 结构体。该函数将来自其输入的字段 `ele` 的值分解为三个组成部分：一个向量、一个 3×2 矩阵和一个标量。该函数将这些组成部分存储在一个局部 MATLAB ``[`struct`](https://ww2.mathworks.cn/help/matlab/ref/struct.html)`` 中，后者与 `Simulink.Bus` 对象 `BusObject` 具有相同的字段。然后，该函数将 MATLAB `struct` 的值赋给输出结构体 `y`。

```
% extract data from input structure
A = double(u.ele(1:2,1));
B = uint8(u.ele(:,2:3));
C = double(u.ele(3,1));

X = struct(ele=int8(zeros(3)));
Y = struct(sb=X,a=A,b=B,c=C);

% assign value to output structure
```

### 定义输入和输出结构体

在 MATLAB 函数中，您可以通过定义函数的输入和输出结构体来访问局部 Stateflow 结构体或对接 Simulink 总线信号：

1. 在基础工作区中，创建一个定义结构体数据类型的 `Simulink.Bus` 对象。
2. 在**符号**窗格中，选择函数输入或输出。
3. 在**属性检查器**中，将**类型**属性设置为 `Bus: <object name>`。用定义 Stateflow 结构体的 `Simulink.Bus` 对象的名称替换 < 对象名称 >。

例如，在函数 `sb2abc` 中：

- 输入结构体 `u` 的**类型**属性指定为 `Bus: SubBus`。
- 输出结构体 `y` 的**类型**属性指定为 `Bus: BusObject`。

有关详细信息，请参阅 [Define Stateflow Structures](https://ww2.mathworks.cn/help/stateflow/ug/about-stateflow-structures.html#bquvi77)。

### 定义局部和持久结构体变量

要将相关数据存储在 MATLAB 函数内的单个变量中，您可以创建一个 MATLAB `struct` 类型的局部或持久变量。例如，函数 `sb2abc` 定义了两个局部 MATLAB 结构体，用于在写入输出结构体 `y` 之前临时存储从输入结构体 `u` 提取的数据：

- `X` 是标量 `struct`，具有名为 `ele` 的单个字段。此字段包含一个类型为 `int8` 的 3×3 矩阵，此矩阵与 `Simulink.Bus` 对象 `SubBus` 的结构体相匹配。
- `Y` 是具有以下四个字段的标量 `struct`：`sb` 是 `SubBus` 类型的子结构体，`a` 是 `double` 类型的二维向量，`b` 是 `uint8` 类型的 3×2 矩阵，`c` 是 `double` 类型的标量。这些字段与 `Simulink.Bus` 对象 `BusObject` 的结构体相匹配。

有关详细信息，请参阅[为代码生成定义标量结构体](https://ww2.mathworks.cn/help/simulink/ug/defining-scalar-structures-for-code-generation.html) (Simulink)。

## 另请参阅

[`struct`](https://ww2.mathworks.cn/help/matlab/ref/struct.html) | [`Simulink.Bus`](https://ww2.mathworks.cn/help/simulink/slref/simulink.bus.html) (Simulink)

## 相关主题

- [Access Bus Signals Through Stateflow Structures](https://ww2.mathworks.cn/help/stateflow/ug/about-stateflow-structures.html)
- [对 Stateflow 结构体进行索引并赋值](https://ww2.mathworks.cn/help/stateflow/ug/structure-operations.html)
- [使用圆点表示法标识数据](https://ww2.mathworks.cn/help/stateflow/ug/identify-data-using-dot-notation.html)
- [为代码生成定义标量结构体](https://ww2.mathworks.cn/help/simulink/ug/defining-scalar-structures-for-code-generation.html) (Simulink)
