---
url: https://ww2.mathworks.cn/help/simulink/slref/simulink.bus.createobject.html
title: 从模块或 MATLAB 结构体创建 Simulink.Bus 对象 - MATLAB Simulink.Bus.createObject
- MathWorks 中国
date: 2023-08-18 21:00:15
tag: 
summary: 此 MATLAB 函数 为指定的模块创建 Simulink.Bus 对象，并返回有关创建的 Bus 对象的信息。
---
从模块或 MATLAB 结构体创建 `Simulink.Bus` 对象

## 说明

[示例](https://ww2.mathworks.cn/help/simulink/slref/simulink.bus.createobject.html#bvibcah)

``[`busInfo`](#bvh2b1i-1-busInfo) = Simulink.Bus.createObject([`model`](#bvh2b1i-1-model),[`blocks`](#bvh2b1i-1-blocks))`` 为指定的模块创建 `Simulink.Bus` 对象，并返回有关创建的 `Bus` 对象的信息。

如果指定与总线层次结构对应的模块，此函数将为层次结构中的每个总线创建 `Bus` 对象。

如果模型使用数据字典，则在数据字典中创建 `Bus` 对象。否则，将在基础工作区中创建。

[示例](https://ww2.mathworks.cn/help/simulink/slref/simulink.bus.createobject.html#mw_603e9ecf-e85b-456a-847f-aae28338506a)

``[`busInfo`](#bvh2b1i-1-busInfo) = Simulink.Bus.createObject([`struct`](#bvh2b1i-1-struct))`` 基于数值结构体或其中可包含 MATLAB® `timeseries`、MATLAB `timetable` 和 `matlab.io.datastore.SimulationDatastore` 对象的结构体创建 `Bus` 对象。

如果指定具有层次结构的结构体，此函数将为层次结构中的每个结构体创建 `Bus` 对象。

`Bus` 对象是在基础工作区中创建的。

[示例](https://ww2.mathworks.cn/help/simulink/slref/simulink.bus.createobject.html#mw_29863384-d669-46fb-ab47-91e4c428ad35)

``[`busInfo`](#bvh2b1i-1-busInfo) = Simulink.Bus.createObject(___,[`file`](#bvh2b1i-1-file),[`format`](#bvh2b1i-1-format))`` 将 `Bus` 对象保存在具有指定格式的函数中。该函数可以使用元胞数组或数组来定义对象属性。

## 示例

全部折叠

### 基于 Bus Creator 模块创建 `Simulink.Bus` 对象

打开示例模型。

```
open_system('BusObjectCreationModel')

```

![](https://ww2.mathworks.cn/help/simulink/slref/createsimulinkbusobjectfromblockexample_01_zh_CN.png)

创建与 Bus Creator 模块创建的总线对应的 `Bus` 对象。

```
busInfo = Simulink.Bus.createObject('BusObjectCreationModel',...
    'BusObjectCreationModel/Bus Creator');

```

### 在函数中创建和保存 `Simulink.Bus` 对象

基于两个 Bus Creator 模块创建 `Bus` 对象，并将 `Bus` 对象定义保存在一个函数中。

打开示例模型。

```
open_system('BusObjectCreationModel');

```

![](https://ww2.mathworks.cn/help/simulink/slref/createandsavesimulinkbusobjectsinafunctionexample_01_zh_CN.png)

使用 `getSimulinkBlockHandle` 函数将 Bus Creator 模块的模块句柄赋给一个变量。

```
bc = getSimulinkBlockHandle('BusObjectCreationModel/Bus Creator');

```

您也可以在模型中选择一个 Bus Creator 模块，然后使用 [`gcbh`](https://ww2.mathworks.cn/help/simulink/slref/gcbh.html) 函数获取其模块句柄。

将 Bus Creator1 模块的模块句柄赋给一个变量。

```
bc1 = getSimulinkBlockHandle('BusObjectCreationModel/Bus Creator1');

```

要创建一个 `Bus` 对象，请在向量中指定模块句柄变量。要保存 `Bus` 对象定义，还要指定文件名。

```
busInfo = Simulink.Bus.createObject('BusObjectCreationModel',...
    [bc bc1], 'BusObjectFunction');

```

由于这些 Bus Creator 模块会创建一个总线层次结构，因此仅指定 Bus Creator1 模块即可在工作区中和函数中创建两个 `Bus` 对象。

将 `BusObjectFunction` 与此命令创建的函数进行比较。

```
topBusInfo = Simulink.Bus.createObject('BusObjectCreationModel',...
    bc1, 'BusObjectFunctionFromHierarchy');

```

对于采用更易于读取的格式的函数，请将函数格式指定为 `object`。

```
topBusInfo1 = Simulink.Bus.createObject('BusObjectCreationModel',...
    bc1, 'BusObjectFunctionFormatted','object');

```

### 从 MATLAB 结构体创建 `Simulink.Bus` 对象

当您使用 Constant 模块创建非虚拟总线时，您必须为**常量值**指定一个 MATLAB 结构体，并将 `Simulink.Bus` 对象指定为**输出数据类型**。

对于此示例，创建一个包含其他结构体的结构体。

```
bus_struct.A.A1 = 0;
bus_struct.A.A2 = [0 + 0i;0 + 0i;0 + 0i;0 + 0i;0 + 0i];
bus_struct.B = 5;
bus_struct.C.C1 = 0;
bus_struct.C.C2.A1 = 0;
bus_struct.C.C2.A2 = [0 + 0i;0 + 0i;0 + 0i;0 + 0i;0 + 0i];

```

创建与该结构体对应的 `Bus` 对象。

```
busInfo = Simulink.Bus.createObject(bus_struct);

```

该函数创建四个 `Bus` 对象。名为 `slBus1` 的 `Bus` 对象对应于顶层结构体，并使用默认 `Bus` 对象名称。名为 `A`、`C` 和 `C2` 的 `Bus` 对象对应于嵌套结构体。

要查看 `Bus` 对象，请打开**类型编辑器**。

## 输入参数

全部折叠

模型名称或句柄，指定为字符向量。

您指定的模型必须编译成功。

### `blocks` — 与总线相关联的模块  
字符向量 | 由模块路径名称组成的元胞数组 | 由模块句柄组成的向量

与总线相关联的模块，指定为字符向量、模块路径名称的元胞数组或模块句柄的向量。如果有一个模块，则指定模块的完整路径名称。如果有多个模块，则指定模块路径名称元胞数组或模块句柄向量。

此函数可以基于下列模块创建 `Bus` 对象：

*   Bus Creator 模块
    
*   子系统 Inport 模块
    
*   子系统 Outport 模块
    

如果指定与总线层次结构关联的模块，该函数还会为该层次结构中的所有嵌套总线创建 `Bus` 对象。

### `struct` — 对象结构体或数值结构体  
MATLAB `timeseries`、MATLAB `timetable` 和 `matlab.io.datastore.SimulationDatastore` 对象的结构体 | 数值结构体

数值结构体或对象结构体，指定为数值结构体或其中可以包含 MATLAB `timeseries`、MATLAB `timetable` 和 `matlab.io.datastore.SimulationDatastore` 对象的结构体。

要生成的函数的名称，指定为字符向量。文件名必须唯一。

### `format` — 要生成的函数的格式  
`'cell'` （默认） | `'object'`

要生成的函数的格式，指定为 `'cell'` 或 `'object'`。`'cell'` 格式更紧凑，但 `'object'` 格式更易于读取。

`'cell'` 格式将 `Bus` 对象定义保存在由元胞数组构成的元胞数组中，并通过调用 [`Simulink.Bus.cellToObject`](https://ww2.mathworks.cn/help/simulink/slref/simulink.bus.celltoobject.html) 创建 `Bus` 对象。每个子元胞数组表示一个 `Bus` 对象并包含以下属性：

元素字段是一个元胞数组，该元胞数组包含 `Bus` 对象引用的每个 [`Simulink.BusElement`](https://ww2.mathworks.cn/help/simulink/slref/simulink.buselement.html) 对象的信息：

`'object'` 格式将 `Bus` 对象定义保存为数组。该函数使用数组索引来访问数组元素，并使用圆点表示法为属性赋值。

### `scope` — 要包含 Simulink.Bus 对象的数据字典  
`Simulink.data.Dictionary` 对象

## 输出参数

全部折叠

总线对象信息，以结构体数组形式返回。

当您指定 [`blocks`](#bvh2b1i-1-blocks) 时，`busInfo` 结构体数组的每个元素对应于一个模块，并且包含以下字段：

*   `block` - 模块的句柄
    
*   `busName` - 与模块关联的 `Bus` 对象的名称
    

当您指定 [`struct`](#bvh2b1i-1-struct) 时，`busInfo` 结构体包含以下字段：

*   `block` - 空矩阵 (`[]`)
    
*   `busName` - 对应于该结构体的 `Bus` 对象的名称
    

## 版本历史记录

**在 R2006a 之前推出**

全部展开

### R2020b: `Simulink.BusElement` 对象不再支持 `SampleTime` 属性

不再支持 `Simulink.BusElement` 对象的 `SampleTime` 属性。

指定采样时间的 `BusElement` 对象会在编译期间导致错误。要从 `BusElement` 对象中删除采样时间设定，请将其 `SampleTime` 设置为 `-1`。

`Simulink.Bus.cellToObject` 继续接受为总线元素指定采样时间的元胞数组。`Simulink.Bus.objectToCell`、`Simulink.Bus.save` 和 `Simulink.Bus.createObject` 继续返回包含非继承采样时间的元胞数组或数组。当继承采样时间 (`-1`) 时，这些函数会忽略它。同样，**类型编辑器**和**模型资源管理器**在继承时会忽略采样时间。

要指定总线元素的采样时间，请使用对应模块的 `SampleTime` 模块参数。例如，您可以使用 In Bus Element、Out Bus Element 和 Signal Specification 模块来指定采样时间。

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

![](https://ww2.mathworks.cn/images/responsive/global/pic-header-mathworks-logo2.svg)

Select a Web Site

Choose a web site to get translated content where available and see local events and offers. Based on your location, we recommend that you select: .

You can also select a web site from the following list:

[Contact your local office](#)