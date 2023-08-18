---
url: https://ww2.mathworks.cn/help/simulink/slref/simulink.bus.creatematlabstruct.html
title: 创建与总线使用相同的层次结构和属性的 MATLAB 结构体 - MATLAB Simulink.Bus.createMATLABStruct
- MathWorks 中国
date: 2023-08-18 21:00:11
tag: 
summary: 此 MATLAB 函数 创建一个或多个 MATLAB 结构体，这些结构体具有与指定总线相同的层次结构和属性。生成的结构体使用总线的接地值。使用此语法为多个总线端口创建初始化结构体。
---
Main Content

创建与总线使用相同的层次结构和属性的 MATLAB 结构体

## 说明

[示例](https://ww2.mathworks.cn/help/simulink/slref/simulink.bus.creatematlabstruct.html#btpiwd5-1)

``[`structs`](#btpir6d-1-structFromBus) = Simulink.Bus.createMATLABStruct([`buses`](#btpir6d-1-busSource))`` 创建一个或多个 MATLAB® 结构体，这些结构体具有与指定总线相同的层次结构和属性。生成的结构体使用总线的接地值。使用此语法为多个总线端口创建初始化结构体。

## 示例

全部折叠

打开并仿真模型 `ex_bus_initial_conditions`。

```
open_system('ex_bus_initial_conditions')
sim('ex_bus_initial_conditions');


```

![](https://ww2.mathworks.cn/help/simulink/slref/matlabstructurefrombusobjectexample_01_zh_CN.png)

使用 `ex_bus_initial_conditions` 模型加载的总线对象 `Top` 创建一个 MATLAB 结构体。

```
mStruct = Simulink.Bus.createMATLABStruct('Top');


```

为 `mStruct` 结构体中与总线 `A` 的总线元素 `A1` 对应的字段设置值。

```
mStruct.A.A1 = 3;
mStruct.A


```

```
ans = 

  struct with fields:

    A1: 3
    A2: [5x1 int8]



```

Simulink 会将该结构体中的其他字段设置为对应的总线元素的接地值。

您可以使用 `mStruct` 作为 Unit Delay 模块的初始条件结构体。

为其信号元素使用 `double` 之外的数据类型的总线创建一个 MATLAB 结构体。使用非完全结构体为部分元素指定初始化值。当您创建非完全结构体时，使字段的数据类型与对应元素的数据类型相匹配。

打开并仿真模型 `ex_bus_initial_conditions`。

```
open_system('ex_bus_initial_conditions')
sim('ex_bus_initial_conditions');


```

![](https://ww2.mathworks.cn/help/simulink/slref/initializesignalelementswithanondoubledatatypeexample_01_zh_CN.png)

标签为 `Constant5` 的模块生成的 `C1` 信号元素使用数据类型 `int16`。

查找生成 `Top` 总线信号的 Bus Creator 模块端口的端口句柄。

```
ph = get_param('ex_bus_initial_conditions/TopBus','PortHandles');


```

创建一个非完全结构体，它为 `TopBus` 模块创建的总线信号中的部分元素指定值。要设置 `C.C1` 字段的值，请使用定型表达式。使表达式中使用的数据类型与模型中信号元素的数据类型 (`int16`) 相匹配。

```
PartialstructForK = struct('B',3,'C',struct('C1',int16(5)));


```

使用 `TopBus` 模块的端口句柄 (`ph`) 创建一个完全结构体。覆盖 `C.C1` 和 `B` 元素的接地值。

```
outPort = ph.Outport;
mStruct = Simulink.Bus.createMATLABStruct(outPort,PartialstructForK);


```

输出结构体中的字段 `C.C1` 继续使用数据类型 `int16`。

打开并仿真模型 `ex_bus_initial_conditions`。

```
open_system('ex_bus_initial_conditions')
sim('ex_bus_initial_conditions');


```

![](https://ww2.mathworks.cn/help/simulink/slref/matlabstructwithspecifieddimexample_01_zh_CN.png)

创建一个非完全结构体，其中包含 `TopBus` 模块创建的总线中的部分总线元素。

```
PartialStructForK = struct('A',struct('A1',4),'B',3)


```

```
PartialStructForK = 

  struct with fields:

    A: [1x1 struct]
    B: 3



```

使用总线对象 `Top`、非完全结构体以及生成的结构体的维度创建一个 MATLAB 结构体。

```
structFromBus = Simulink.Bus.createMATLABStruct...
     ('Top',PartialStructForK,[2 3])


```

```
structFromBus = 

  2x3 struct array with fields:

    A
    B
    C



```

要为多个总线端口创建初始化结构体，请将端口句柄指定为 `Simulink.Bus.createMATLABStruct` 的参数。生成的结构体元胞数组使用接地值。

打开并仿真模型 `ex_two_outports_create_struct`。

```
open_system('ex_two_outports_create_struct')
sim('ex_two_outports_create_struct');


```

![](https://ww2.mathworks.cn/help/simulink/slref/createcellarrayofmatlabstructuresexample_01_zh_CN.png)

查找 Bus Creator 模块 `Bus1` 和 `Bus2` 的端口句柄。

```
ph_1 = get_param...
     ('ex_two_outports_create_struct/Bus Creator','PortHandles');
ph_2 = get_param...
     ('ex_two_outports_create_struct/Bus Creator1','PortHandles');


```

使用端口句柄数组创建一个 MATLAB® 结构体。

```
mStruct = Simulink.Bus.createMATLABStruct([ph_1.Outport ph_2.Outport])


```

```
mStruct =

  2x1 cell array

    {1x1 struct}
    {1x1 struct}



```

### 基于总线端口和非完全结构体创建 MATLAB 结构体

基于与总线信号连接的端口创建一个 MATLAB 结构体。使用非完全结构体为连接该端口的总线中的部分总线元素指定值。

打开并仿真模型 `ex_bus_initial_conditions`。

```
open_system('ex_bus_initial_conditions')
sim('ex_bus_initial_conditions');


```

![](https://ww2.mathworks.cn/help/simulink/slref/matlabstructurefrombusportandpartialstructureexample_01_zh_CN.png)

查找生成 `Top` 总线信号的 Bus Creator 模块端口的端口句柄。`Outport` 句柄就是您需要的句柄。

```
ph = get_param('ex_bus_initial_conditions/TopBus','PortHandles')


```

```
ph = 

  struct with fields:

      Inport: [27.0009 31.0009 42.0009]
     Outport: 43.0009
      Enable: []
     Trigger: []
       State: []
       LConn: []
       RConn: []
    Ifaction: []
       Reset: []
       Event: []



```

创建一个非完全结构体，其中包含 `TopBus` 模块创建的总线信号。您可以使用非完全结构体为部分总线元素指定值。

```
PartialstructForK = struct('A',struct('A1',4),'B',3)


```

```
PartialstructForK = 

  struct with fields:

    A: [1x1 struct]
    B: 3



```

由结构体字段 `Top.B` 和 `Top.A` 表示的总线元素在总线层次结构中位于同一级别。您可以使用此非完全结构体覆盖 `B` 和 `A` 总线信号元素的接地值。

基于总线对象或总线端口创建结构体时，您可以使用非完全结构体作为可选参数。

使用 `TopBus` 模块的端口句柄 (`ph`) 创建一个 MATLAB 结构体。覆盖 `A.A1` 和 `B` 总线元素的接地值。

```
outPort = ph.Outport;
mStruct = Simulink.Bus.createMATLABStruct(outPort,PartialstructForK)


```

```
mStruct = 

  struct with fields:

    A: [1x1 struct]
    B: 3
    C: [1x1 struct]



```

## 输入参数

全部折叠

### `buses` — 总线信息的来源  
`Simulink.Bus` 对象名称 | 端口句柄 | `Simulink.Bus` 对象名称的元胞数组 | 端口句柄数组

总线信息的来源，指定为 `Bus` 对象名称、端口句柄、`Bus` 对象名称元胞数组或端口句柄数组。

*   如果使用 `Bus` 对象名称，则 `Bus` 对象必须位于 MATLAB 基础工作区或模型使用的数据字典中。`Bus` 对象名称的数据类型是 `char` 或 `string`。
    
*   如果您使用端口句柄，则在您使用此函数之前，必须成功编译模型。端口句柄的数据类型是 `double`。
    
*   对于总线数组，不能使用端口句柄。
    
*   如果您使用 [`dims`](#btpir6d-1-dims) 参数，则对于 `buses` 参数，请使用 `Bus` 对象或 `Bus` 对象的元胞数组。
    

如果指定 `Bus` 对象名称的元胞数组或端口句柄的数组，则只需一次 `Simulink.Bus.createMATLABStruct` 调用即可创建多个结构体，性能优于使用单独的 `Simulink.Bus.createMATLABStruct` 调用来创建结构体。

**示例：** `struct = Simulink.Bus.createMATLABStruct('BusObject')`

**示例：** `structs = Simulink.Bus.createMATLABStruct({'BusObject','BusObject1'})`

**示例：** `struct = Simulink.Bus.createMATLABStruct(portHandle)`

**示例：** `structs = Simulink.Bus.createMATLABStruct([portHandle,portHandle1])`

**数据类型：** `double` | `char` | `string` | `struct` | `cell`

### `values` — 生成的结构体中元素子集的值  
`[]` | 非完全结构体 | 元胞数组

生成的结构体中元素子集的值，指定为空矩阵 (`[]`)、非完全结构体或元胞数组。对于每个指定的总线信息的来源，元胞数组必须包含一个非完全结构体或空矩阵。

有关创建非完全结构体的信息，请参阅[创建用于初始化的非完全结构体](https://ww2.mathworks.cn/help/simulink/ug/specifying-initial-conditions-for-bus-signals.html#mw_2a97d888-890f-4cd0-a8f9-1d534a514c32)。

要使用接地值，请为空矩阵。

**示例：** `struct = Simulink.Bus.createMATLABStruct('BusObject',PartialStruct)`

**数据类型：** `struct` | `cell`

### `dims` — 生成的结构体的维度  
标量 | 向量 | 元胞数组

生成的结构体的维度，指定为向量。

每个维度元素都必须是大于或等于 1 的整数。如果为 [`values`](#btpir6d-1-partialStructures) 参数指定非完全结构体，则每个维度元素必须大于或等于其在非完全结构体中的对应维度元素。

**示例：** `struct = Simulink.Bus.createMATLABStruct('BusObject',PartialStruct,[2 3])`

**示例：** `structs = Simulink.Bus.createMATLABStruct({'Bus','Bus1','Bus2'},{[],[],[]},{1,2,3})`

**数据类型：** `double` | `cell`

## 输出参数

全部折叠

### `structs` — 与总线具有相同信号层次结构和属性的结构体  
MATLAB 结构体 | MATLAB 结构体的元胞数组

与总线具有相同信号层次结构和属性的结构体，以 MATLAB 结构体或 MATLAB 结构体元胞数组形式返回。

结构体维度取决于您指定的输入参数：

*   如果只指定 [`buses`](#btpir6d-1-busSource) 参数，则维度为 1。
    
*   如果您还指定 [`values`](#btpir6d-1-partialStructures) 参数，则维度与 `values` 的维度相匹配。
    
*   如果您指定 [`dims`](#btpir6d-1-dims) 参数，则维度与 `dims` 的维度相匹配。
    

## 提示

*   如果您对同一个模型重复使用 `Simulink.Bus.createMATLABStruct` 函数（例如，在脚本中的循环中），可以通过避免多次编译模型来提高性能。要提高速度，请在多次使用该函数之前对模型进行编译。例如，要对 `vdp` 模型进行编译，请使用以下命令：
    
    在您创建 MATLAB 结构体后，使用以下命令终止编译：
    
*   您可以使用**[类型编辑器](https://ww2.mathworks.cn/help/simulink/slref/typeeditor.html)**来调用 `Simulink.Bus.createMATLABStruct` 函数。在类型编辑器中，选择您要为其创建 MATLAB 结构体的 `Bus` 对象。然后，在工具条中，点击 **MATLAB 结构体**。
    
    您可以在 MATLAB 编辑器中编辑 MATLAB 结构体，并评估代码以创建或更新此结构体中的值。
    
*   您可以使用 `Simulink.Bus.createMATLABStruct` 函数指定引用模型的输出初始值。
    

您曾对此示例进行过修改。是否要打开带有您的编辑的示例？

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

Select a Web Site

Choose a web site to get translated content where available and see local events and offers. Based on your location, we recommend that you select: .

You can also select a web site from the following list:

[Contact your local office](#)