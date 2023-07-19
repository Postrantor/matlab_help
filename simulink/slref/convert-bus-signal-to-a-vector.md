---
url: https://ww2.mathworks.cn/help/simulink/slref/convert-bus-signal-to-a-vector.html
title: Manage Bus-to-Vector Conversions - MATLAB & Simulink - MathWorks 中国 --- 管理总线到向量的转换 - MATLAB & Simulink - MathWorks 中国
date: 2023-06-25 12:55:14
tag:
summary: This example shows how to use a Bus to Vector block to convert a bus to a vector, to provide a signal......
---
This example shows how to find and manage implicit bus-to-vector conversions.

Blocks that do not accept buses may implicitly convert buses to vectors. When a bus is treated as a vector, bus elements become inaccessible.

Some buses cannot convert to vectors. For more information, see [Bus to Vector](https://ww2.mathworks.cn/help/simulink/slref/bustovector.html).

### Identify Implicit Bus-to-Vector Conversions

Open and simulate model `ex_bus_to_vector`.

![](https://ww2.mathworks.cn/help/examples/simulink/win64/ConvertBusSignalToAVectorExample_01.png)

To accept the bus, the Gain blocks implicitly convert the bus to a vector.

To identify buses treated as vectors before simulation, use function `Simulink.BlockDiagram.addBusToVector`.

```
[blocks] = Simulink.BlockDiagram.addBusToVector('ex_bus_to_vector')

```

```
### Processing block diagram 'ex_bus_to_vector'
### Number of blocks left that are connected to a bus being used as a vector: 2
### Done processing block diagram 'ex_bus_to_vector'

blocks =

  1x2 struct array with fields:

    BlockPath
    InputPort
    LibPath
```

To identify buses treated as vectors during simulation, set the **Bus signal treated as vector** configuration parameter to `warning` or `error`. The default setting for **Bus signal treated as vector** is `none`, which generates no warning or error message when a block implicitly converts a bus to a vector.

### Explicitly Define Bus-To-Vector Conversions

To insert Bus to Vector blocks where blocks implicitly convert buses to vectors, use function `Simulink.BlockDiagram.addBusToVector` with `reportOnly` set to `false`. When you use function `Simulink.BlockDiagram.addBusToVector` with `reportOnly` set to `false`, the function saves the model. To create a writable copy of model `ex_bus_to_vector`, this example uses the `save_system` function.

```
save_system('ex_bus_to_vector','ex_bus_to_vector_blocks');
```

```
[blocks,busToVectors] =
Simulink.BlockDiagram.addBusToVector('ex_bus_to_vector_blocks',true,false);
```

![](https://ww2.mathworks.cn/help/examples/simulink/win64/ConvertBusSignalToAVectorExample_02.png)

The Gain blocks no longer implicitly convert the bus to a vector. The inserted Bus to Vector blocks perform the conversion explicitly.

Bus to Vector blocks are virtual and do not affect simulation results, code generation, or performance.

Function `Simulink.BlockDiagram.addBusToVector` returns no remaining implicit bus-to-vector conversions.

```
[blocks] = Simulink.BlockDiagram.addBusToVector('ex_bus_to_vector_blocks')
```

```
###No buses used as vectors left to process

blocks =

  1x0 empty struct array with fields:

    BlockPath
    InputPort
    MixedAttributes
```

By specifying acceptable bus-to-vector conversions with Bus to Vector blocks, you can more easily identify unexpected conversions. Having configuration parameter **Bus signal treated as vector** set to `warning` or `error` alerts you when an unexpected bus-to-vector conversion occurs.

您曾对此示例进行过修改。是否要打开带有您的编辑的示例？

How useful was this information?

Unrated

1 star

2 stars

3 stars

4 stars

5 stars

Unrated 1 star 2 stars 3 stars 4 stars 5 stars

您点击的链接对应于以下 MATLAB 命令：

请在 MATLAB 命令行窗口中直接输入以执行命令。Web 浏览器不支持 MATLAB 命令。
