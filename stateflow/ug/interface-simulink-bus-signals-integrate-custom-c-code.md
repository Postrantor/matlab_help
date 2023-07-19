---
tip: translate by openai@2023-07-12 16:29:10
...
---

url: https://ww2.mathworks.cn/help/stateflow/ug/interface-simulink-bus-signals-integrate-custom-c-code.html

> URL：https://ww2.mathworks.cn/help/stateflow/ug/如何将Simulink总线信号集成到自定义C代码中。html
title: Integrate Custom Structures in Stateflow Charts - MATLAB & Simulink - MathWorks 中国 --- 在 Stateflow 图中集成自定义结构 - MATLAB & Simulink - MathWorks 中国
date: 2023-07-12 16:28:07
tag:

summary: This example shows how to use structures from custom code in a Stateflow® chart.

> 这个例子展示了如何在Stateflow®图表中使用自定义代码中的结构。
---


This example shows how to use structures from custom code in a Stateflow® chart. You can define structure typed data in C code and integrate it with Stateflow structures and Simulink® buses. By sharing data with custom code, you can augment the capabilities supported by Stateflow and take advantage of your preexisting code. For more information, see [Reuse Custom Code in Stateflow Charts](https://ww2.mathworks.cn/help/stateflow/ug/share-data-using-custom-c-code.html).

> 这个例子展示了如何在Stateflow®图表中使用自定义代码中的结构。您可以在C代码中定义结构化数据，并将其与Stateflow结构和Simulink®总线集成。通过与自定义代码共享数据，您可以增强Stateflow支持的功能，并利用您现有的代码。有关更多信息，请参阅[在Stateflow图中重用自定义代码](https://ww2.mathworks.cn/help/stateflow/ug/share-data-using-custom-c-code.html)。


In this example, a Stateflow chart processes data from one Simulink bus and outputs the result to another Simulink bus. Both the input and output buses are defined by the `` [`Simulink.Bus`](https://ww2.mathworks.cn/help/simulink/slref/simulink.bus.html) (Simulink) `` object `COUNTERBUS`. In the chart, the Simulink buses interface with the Stateflow structures `inbus` and `outbus`. The chart calls a custom C function to write to the output structure `outbus`.

> 在这个例子中，一个Stateflow图从一个Simulink总线处理数据，并将结果输出到另一个Simulink总线。输入和输出总线都由``[`Simulink.Bus`](https://ww2.mathworks.cn/help/simulink/slref/simulink.bus.html)（Simulink）``对象`COUNTERBUS`定义。在图表中，Simulink总线与Stateflow结构`inbus`和`outbus`进行交互。图表调用自定义C函数来写入输出结构`outbus`。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/InterfaceSimulinkBusSignalsIntegrateCustomCCodeExample_01.png)

### Define Custom Structures in C Code


1. In your C code, define a structure by creating a custom header file. The header file contains `typedef` declarations that define the properties of the custom structure. For example, in this model, the header file `counterbus.h` declares three custom structures:

> 在你的C代码中，通过创建自定义头文件来定义一个结构体。头文件包含定义自定义结构体属性的`typedef`声明。例如，在这个模型中，头文件`counterbus.h`声明了三个自定义结构体：

```
...
typedef struct {
    int input;
} SIGNALBUS;

```

```
typedef struct {
    int upper_saturation_limit;
    int lower_saturation_limit;
} LIMITBUS;

```

```
typedef struct {
    SIGNALBUS inputsignal;
    LIMITBUS limits;
} COUNTERBUS;
...

```


2. In the Type Editor, define a `Simulink.Bus` object that matches each custom structure `typedef` declaration. In the **Header file** field of each `Simulink.Bus` object, enter the name of the header file that contains the matching `typedef` declaration.

> 在类型编辑器中，定义一个与每个自定义结构`typedef`声明匹配的`Simulink.Bus`对象。在每个`Simulink.Bus`对象的**头文件**字段中，输入包含匹配`typedef`声明的头文件的名称。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/xxsf_bus_demo_type_editor.png)


3. Configure the Stateflow chart to include custom C code, as described in [Configure Custom Code for Your Model](https://ww2.mathworks.cn/help/stateflow/ug/share-data-using-custom-c-code.html#brj0vwv).

> 配置Stateflow图，包括根据[配置模型的自定义C代码](https://ww2.mathworks.cn/help/stateflow/ug/share-data-using-custom-c-code.html#brj0vwv)中所述的自定义C代码。

4. Build and run your model.

### Pass Stateflow Structures to Custom Code


When you call custom code functions that take structure pointers as arguments, pass the Stateflow structures by address. To pass the address of a Stateflow structure or one of its fields to a custom function, use the `&` operator and dot notation:

> 当您调用接受结构指针作为参数的自定义代码函数时，请通过地址传递Stateflow结构。 要将Stateflow结构或其中一个字段的地址传递给自定义函数，请使用“&”操作符和点表示法：

- `&outbus` provides the address of the Stateflow structure `outbus`.

- `&outbus.inputsignal` provides the address of the substructure `inputsignal` of the structure `outbus`.

> `-`&outbus.inputsignal提供了结构outbus的子结构inputsignal的地址。

- `&outbus.inputsignal.input` provides the address of the field `input` of the substructure `outbus.inputsignal`.

> `-`&outbus.inputsignal.input`提供了子结构`outbus.inputsignal`的`input`字段的地址。


For more information, see [Index and Assign Values to Stateflow Structures](https://ww2.mathworks.cn/help/stateflow/ug/structure-operations.html).

> 更多信息，请参阅[索引和分配值到Stateflow结构](https://ww2.mathworks.cn/help/stateflow/ug/structure-operations.html)。


For instance, this example contains a custom C function `counterbusFcn` that takes structure pointers as arguments. The custom header file `counterbus.h` contains this function declaration:

> 例如，本示例包含一个自定义C函数`counterbusFcn`，它接受结构指针作为参数。自定义头文件`counterbus.h`包含此函数声明：

```
extern void counterbusFcn(COUNTERBUS *u1, int u2, COUNTERBUS *y1, int *y2);

```


The chart passes the addresses to the Stateflow structures `counterbus_struct` and `outbus` by using this function call:

> 图表通过使用此函数调用将地址传递给 Stateflow 结构 `counterbus_struct` 和 `outbus`：

```
counterbusFcn(&counterbus_struct, u2, &outbus, &y2);


```

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/InterfaceSimulinkBusSignalsIntegrateCustomCCodeExample_02.png)


The function reads the value of the chart input `u2` and the local structure `counterbus_struct`. It writes to the chart output `y2` and the output structure `outbus`.

> 该函数读取图表输入`u2`和本地结构`counterbus_struct`的值，并将写入图表输出`y2`和输出结构`outbus`。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/InterfaceSimulinkBusSignalsIntegrateCustomCCodeExample_03.png)

## See Also


[`Simulink.Bus`](https://ww2.mathworks.cn/help/simulink/slref/simulink.bus.html) (Simulink)

> `Simulink.Bus`（Simulink）

## Related Topics

- [Access Bus Signals Through Stateflow Structures](https://ww2.mathworks.cn/help/stateflow/ug/about-stateflow-structures.html)
- [Index and Assign Values to Stateflow Structures](https://ww2.mathworks.cn/help/stateflow/ug/structure-operations.html)
- [Reuse Custom Code in Stateflow Charts](https://ww2.mathworks.cn/help/stateflow/ug/share-data-using-custom-c-code.html)
- [Access Custom Code Variables and Functions in Stateflow Charts](https://ww2.mathworks.cn/help/stateflow/ug/procedures-for-simulation.html)
- [Integrate External Code by Using Model Configuration Parameters](https://ww2.mathworks.cn/help/rtw/ug/place-external-c-cpp-code-in-generated-code.html#f1144416) (Simulink Coder)

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
