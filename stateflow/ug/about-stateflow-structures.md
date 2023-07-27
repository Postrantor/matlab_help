---
url: https://ww2.mathworks.cn/help/stateflow/ug/about-stateflow-structures.html
title: Access Bus Signals Through Stateflow Structures - MATLAB & Simulink - MathWorks 中国 --- 通过 Stateflow 结构访问总线信号 - MATLAB & Simulink - MathWorks 中国
date: 2023-07-12 15:45:16
tag:
summary: Define Stateflow structures for input, output, and local access to bus signals.
---

> [!NOTE]
> .[在 Stateflow 图中集成自定义结构](https://ww2.mathworks.cn/help/stateflow/ug/interface-simulink-bus-signals-integrate-custom-c-code.html)

## Access Bus Signals Through Stateflow Structures

> 通过 Stateflow 结构访问总线信号

A Stateflow® structure is a data type that you define from a [`Simulink.Bus`](https://ww2.mathworks.cn/help/simulink/slref/simulink.bus.html) (Simulink) object. Using Stateflow structures, you can bundle data of different size and type to create:

> Stateflow ® 结构是您从 `Simulink.Bus` (Simulink) 对象定义的数据类型。使用 Stateflow 结构，您可以捆绑不同大小和类型的数据来创建：

- Inputs and outputs that access Simulink® bus signals from Stateflow charts, Truth Table blocks, and MATLAB Function blocks.
  > 用于访问来自 Stateflow 图、Truth Table 模块和 MATLAB Function 模块的 Simulink ® 总线信号的输入和输出。
- Local data in Stateflow charts, truth tables, graphical functions, MATLAB® functions, and boxes.
  > Stateflow 图、真值表、图形函数、MATLAB ® 函数和方框中的本地数据。
- Temporary data in Stateflow graphical functions, truth tables, and MATLAB functions.
  > Stateflow 图形函数、真值表和 MATLAB 函数中的临时数据。

For more information, see [Create and Specify Simulink.Bus Objects](https://ww2.mathworks.cn/help/simulink/ug/when-to-use-bus-objects.html#brin2jr-1) (Simulink).

> 有关详细信息，请参阅创建和指定 Simulink.Bus 对象 (Simulink)。

### Example of Stateflow Structures

> Stateflow 结构示例

In this example, a Stateflow chart receives a bus input signal by using the structure `inbus` and outputs a bus signal from the structure `outbus`. The input signal comes from the Simulink Bus Creator block `COUNTERBUSCreator`, which bundles signals from two other Bus Creator blocks. The output structure `outbus` connects to a Simulink Bus Selector block. Both `inbus` and `outbus` derive their type from the `Simulink.Bus` object `COUNTERBUS`. For more information about this example, see [Integrate Custom Structures in Stateflow Charts](https://ww2.mathworks.cn/help/stateflow/ug/interface-simulink-bus-signals-integrate-custom-c-code.html).

> 在此示例中，Stateflow 图使用结构体 `inbus` 接收总线输入信号，并从结构体 `outbus` 输出总线信号。输入信号来自 Simulink Bus Creator 模块 `COUNTERBUSCreator` ，该模块捆绑来自其他两个 Bus Creator 模块的信号。输出结构 `outbus` 连接到 Simulink Bus Selector 模块。 `inbus` 和 `outbus` 都从 `Simulink.Bus` 对象 `COUNTERBUS` 派生其类型。有关此示例的更多信息，请参阅在 Stateflow 图中集成自定义结构。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/AccessBusSignalsThroughStructuresExample_01.png)

The elements of a Stateflow structure data type are called _fields_. Fields can be any combination of individual signals, muxed signals, vectors, and other structures (also called _substructures_). Each field has its own data type. The data type does not have to match the type of any other field in the structure. For example, in this model, each of the structures `inbus` and `outbus` has two fields:

> Stateflow 结构数据类型的元素称为字段。字段可以是单个信号、复用信号、向量和其他结构（也称为子结构）的任意组合。每个字段都有自己的数据类型。数据类型不必与结构中任何其他字段的类型匹配。例如，在此模型中，每个结构 `inbus` 和 `outbus` 都有两个字段：

- `inputsignal` is a substructure with one field, `input`.
  > `inputsignal` 是一个具有一个字段 `input` 的子结构。
- `limits` is a substructure with two fields, `upper_saturation_limit` and `lower_saturation_limit`.
  > `limits` 是一个具有两个字段的子结构： `upper_saturation_limit` 和 `lower_saturation_limit` 。

### Define Stateflow Structures

> 定义 Stateflow 结构

1. To define the structure data type, create a Simulink bus object in the base workspace, as described in [Create and Specify Simulink.Bus Objects](https://ww2.mathworks.cn/help/simulink/ug/when-to-use-bus-objects.html#brin2jr-1) (Simulink).

   > 要定义结构数据类型，请在基础工作区中创建 Simulink 总线对象，如创建和指定 Simulink.Bus 对象 (Simulink) 中所述。

2. Add a data object to the chart, as described in [Add Stateflow Data](https://ww2.mathworks.cn/help/stateflow/ug/adding-data.html).

   > 将数据对象添加到图表中，如添加 Stateflow 数据中所述。

   To define temporary structures in truth tables, graphical functions, and MATLAB functions, add a data object _to your function_. For more information, see [Add Data Through the Model Explorer](https://ww2.mathworks.cn/help/stateflow/ug/adding-data.html#bql4jry).

   > 要在真值表、图形函数和 MATLAB 函数中定义临时结构，请将数据对象添加到函数中。有关更多信息，请参阅通过模型资源管理器添加数据。

3. Set the **Scope** property for the structure. Your choices are:

   > 设置结构的 Scope 属性。您的选择是：

   - `Input`
   - `Output`
   - `Local`
   - `Parameter`
   - `Data Store Memory`
   - `Temporary` (Only in charts that use C as the action language)
     `Temporary` （仅在使用 C 作为操作语言的图表中）

4. Set the **Type** property for the structure. Depending on its scope, a Stateflow structure can have one of these data types.

   > 设置结构的 Type 属性。根据其范围，Stateflow 结构可以具有以下数据类型之一。

   <table data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><colgroup data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><col width="25%" data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><col width="75%" data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"></colgroup><thead data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><tr data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><th data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Type<span lang="zh-CN" data-immersive-translate-translation-element-mark="1"><span data-immersive-translate-translation-element-mark="1">&nbsp;</span><span data-immersive-translate-translation-element-mark="1"><span data-immersive-translate-translation-element-mark="1">类型</span></span></span></th><th data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Description<span lang="zh-CN" data-immersive-translate-translation-element-mark="1"><span data-immersive-translate-translation-element-mark="1">&nbsp;</span><span data-immersive-translate-translation-element-mark="1"><span data-immersive-translate-translation-element-mark="1">描述</span></span></span></th></tr></thead><tbody data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><tr data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><td data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><code data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Inherit: Same as Simulink</code></td><td data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><p data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">This option is available for input structures only. The input structure inherits its data type from the Simulink bus signal in your model that connects to it. The Simulink bus signal must be a nonvirtual bus. For more information, see <a href="https://ww2.mathworks.cn/help/stateflow/ug/about-stateflow-structures.html#bqwal0f" data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Virtual and Nonvirtual Buses</a>.<span lang="zh-CN" data-immersive-translate-translation-element-mark="1"><br><span data-immersive-translate-translation-element-mark="1"><span data-immersive-translate-translation-element-mark="1">此选项仅适用于输入结构。输入结构从与其连接的模型中的 Simulink 总线信号继承其数据类型。 Simulink 总线信号必须是非虚拟总线。有关详细信息，请参阅虚拟和非虚拟总线。</span></span></span></p><p data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">In the base workspace, specify a <code data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Simulink.Bus</code> object with the same properties as the bus signal that connects to the <span data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Stateflow</span> input structure. These properties must match:<span lang="zh-CN" data-immersive-translate-translation-element-mark="1"><br><span data-immersive-translate-translation-element-mark="1"><span data-immersive-translate-translation-element-mark="1">在基础工作区中，指定一个 <code data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Simulink.Bus</code> 对象，其属性与连接到 Stateflow 输入结构的总线信号相同。这些属性必须匹配：</span></span></span></p><div data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><ul data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><li data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><p data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Number, name, and type of inputs<span lang="zh-CN" data-immersive-translate-translation-element-mark="1"><br><span data-immersive-translate-translation-element-mark="1"><span data-immersive-translate-translation-element-mark="1">输入的数量、名称和类型</span></span></span></p></li><li data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><p data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Dimension<span lang="zh-CN" data-immersive-translate-translation-element-mark="1"><span data-immersive-translate-translation-element-mark="1">&nbsp;</span><span data-immersive-translate-translation-element-mark="1"><span data-immersive-translate-translation-element-mark="1">方面</span></span></span></p></li><li data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><p data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Sample Time<span lang="zh-CN" data-immersive-translate-translation-element-mark="1"><span data-immersive-translate-translation-element-mark="1">&nbsp;</span><span data-immersive-translate-translation-element-mark="1"><span data-immersive-translate-translation-element-mark="1">采样时间</span></span></span></p></li><li data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><p data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Complexity<span lang="zh-CN" data-immersive-translate-translation-element-mark="1"><span data-immersive-translate-translation-element-mark="1">&nbsp;</span><span data-immersive-translate-translation-element-mark="1"><span data-immersive-translate-translation-element-mark="1">复杂</span></span></span></p></li><li data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><p data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Sampling Mode<span lang="zh-CN" data-immersive-translate-translation-element-mark="1"><span data-immersive-translate-translation-element-mark="1">&nbsp;</span><span data-immersive-translate-translation-element-mark="1"><span data-immersive-translate-translation-element-mark="1">采样方式</span></span></span></p></li></ul></div><p data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">If the input signal comes from a <span data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Bus Creator</span> block, in the Bus Creator dialog box, specify an appropriate bus object for <strong data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Output data type</strong> field. When you specify the bus object, Simulink verifies that the properties of the <code data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Simulink.Bus</code> object in the base workspace match the properties of the Simulink bus signal.<span lang="zh-CN" data-immersive-translate-translation-element-mark="1"><br><span data-immersive-translate-translation-element-mark="1"><span data-immersive-translate-translation-element-mark="1">如果输入信号来自 Bus Creator 模块，则在 Bus Creator 对话框中为 Output data type 字段指定适当的总线对象。当您指定总线对象时，Simulink 会验证基础工作区中 <code data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Simulink.Bus</code> 对象的属性是否与 Simulink 总线信号的属性匹配。</span></span></span></p></td></tr><tr data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><td data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><code data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Bus: &lt;object name&gt;</code></td><td data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><p data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">In the <strong data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Type</strong> field, replace <em data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><code data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">&lt;object name&gt;</code></em> with the name of the <code data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Simulink.Bus</code> object that defines the <span data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Stateflow</span> structure.<span lang="zh-CN" data-immersive-translate-translation-element-mark="1"><br><span data-immersive-translate-translation-element-mark="1"><span data-immersive-translate-translation-element-mark="1">在类型字段中，将 <code data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">&lt;object name&gt;</code> 替换为定义 Stateflow 结构的 <code data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Simulink.Bus</code> 对象的名称。</span></span></span></p><p data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">For input or output structures, you are not required to specify the bus signal in your Simulink model that connects to the <span data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Stateflow</span> structure. If you do specify a bus signal, its properties must match the <code data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Simulink.Bus</code> object that defines the <span data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Stateflow</span> structure.<span lang="zh-CN" data-immersive-translate-translation-element-mark="1"><br><span data-immersive-translate-translation-element-mark="1"><span data-immersive-translate-translation-element-mark="1">对于输入或输出结构，您无需在连接到 Stateflow 结构的 Simulink 模型中指定总线信号。如果您确实指定了总线信号，则其属性必须与定义 Stateflow 结构的 <code data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Simulink.Bus</code> 对象匹配。</span></span></span></p></td></tr><tr data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><td data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><code data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">&lt;date type expression&gt;</code></td><td data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><p data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">In the <strong data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Type</strong> field, replace <em data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><code data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">&lt;data type expression&gt;</code></em> with an expression that evaluates to a data type. For example:<span lang="zh-CN" data-immersive-translate-translation-element-mark="1"><br><span data-immersive-translate-translation-element-mark="1"><span data-immersive-translate-translation-element-mark="1">在 “类型” 字段中，将 <code data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">&lt;data type expression&gt;</code> 替换为计算结果为数据类型的表达式。例如：</span></span></span></p><div data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><ul data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><li data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><p data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Enter the name of the <code data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Simulink.Bus</code> object that defines the <span data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Stateflow</span> structure.<span lang="zh-CN" data-immersive-translate-translation-element-mark="1"><br><span data-immersive-translate-translation-element-mark="1"><span data-immersive-translate-translation-element-mark="1">输入定义 Stateflow 结构的 <code data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Simulink.Bus</code> 对象的名称。</span></span></span></p></li><li data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><p data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">For structures with scopes other than <code data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Output</code>, use the <span data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Stateflow</span> <a href="https://ww2.mathworks.cn/help/stateflow/ug/operations-for-stateflow-data.html#f0-121472" data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f"><code data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">type</code></a> operator to copy the type of another structure. For more information, see <a href="https://ww2.mathworks.cn/help/stateflow/ug/about-stateflow-structures.html#bqw24br" data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Specify Structure Types by Calling the type Operator</a>.<span lang="zh-CN" data-immersive-translate-translation-element-mark="1"><br><span data-immersive-translate-translation-element-mark="1"><span data-immersive-translate-translation-element-mark="1">对于作用域不是 <code data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">Output</code> 的结构，请使用 Stateflow <code data-immersive-translate-effect="1" data-immersive_translate_walked="dae84bab-0d22-4b3c-a98f-e2c32755e76f">type</code> 运算符复制另一个结构的类型。有关详细信息，请参阅通过调用类型运算符指定结构类型。</span></span></span></p></li></ul></div></td></tr></tbody></table>

For example, in the `sf_bus_demo` model, the input structure `inbus` and the output structure `outbus` derive their type through a type specification of the form `Bus: COUNTERBUS`.

> 例如，在 `sf_bus_demo` 模型中，输入结构 `inbus` 和输出结构 `outbus` 通过 `Bus: COUNTERBUS`

![](https://ww2.mathworks.cn/help/stateflow/ug/sf_bus_demo_inbus.png)

### Specify Structure Types by Calling the `type` Operator

> 通过调用 `type` 运算符指定结构类型

To specify structure types, you can use expressions that call the Stateflow [`type`](https://ww2.mathworks.cn/help/stateflow/ug/operations-for-stateflow-data.html#f0-121472) operator. This operator sets the type of one structure to the type of another structure in the Stateflow chart. For example, in the `sf_bus_demo` model, a `type` operator expression specifies the type of the local structure `counterbus_struct` in terms of the input structure `inbus`. Both structures are defined from the `Simulink.Bus` object `COUNTERBUS`. For more information, see [Derive Data Types from Other Data Objects](https://ww2.mathworks.cn/help/stateflow/ug/typing-stateflow-data.html#f7-34857).

> 要指定结构类型，您可以使用调用 Stateflow `type` 运算符的表达式。该运算符将 Stateflow 图中一个结构的类型设置为另一结构的类型。例如，在 `sf_bus_demo` 模型中， `type` 运算符表达式根据输入结构 `inbus` 的类型 > . 这两个结构都是从 `Simulink.Bus` 对象 `COUNTERBUS` 定义的。有关详细信息，请参阅从其他数据对象派生数据类型。

![](https://ww2.mathworks.cn/help/stateflow/ug/sf_bus_demo_counterbus_struct.png)

### Virtual and Nonvirtual Buses

> 虚拟和非虚拟总线

Simulink models support virtual and nonvirtual buses. Nonvirtual buses read their inputs from data structures stored in contiguous memory. Virtual buses read their inputs from noncontiguous memory. For more information, see [Composite Interface Guidelines](https://ww2.mathworks.cn/help/simulink/ug/composite-signal-techniques.html) (Simulink).

> Simulink 模型支持虚拟和非虚拟总线。非虚拟总线从存储在连续内存中的数据结构读取输入。虚拟总线从非连续内存中读取输入。有关详细信息，请参阅复合接口指南 (Simulink)。

Stateflow charts support only nonvirtual buses. Stateflow input structures can accept virtual bus signals and convert them to nonvirtual bus signals. Stateflow input structures cannot inherit properties from virtual bus signals. If the input to a chart is a virtual bus, set the **Type** property of the input structure through a type specification of the form `` Bus: _`<object name>`_ ``.

> Stateflow 图仅支持非虚拟总线。 Stateflow 输入结构可以接受虚拟总线信号并将其转换为非虚拟总线信号。 Stateflow 输入结构无法继承虚拟总线信号的属性。如果图表的输入是虚拟总线，则通过 `` Bus: _`<object name>`_ `` 形式的类型规范设置输入结构的 Type 属性。

### Debug Structures 调试结构

To debug a Stateflow structure, open the Stateflow Breakpoints and Watch window and examine the values of structure fields during simulation. To view the values of structure fields at the command line, use dot notation to index into the structure. For more information, see [Inspect and Modify Data and Messages While Debugging](https://ww2.mathworks.cn/help/stateflow/ug/watching-data-values-during-simulation.html).
要调试 Stateflow 结构，请打开 Stateflow Breakpoints and Watch 窗口并在仿真期间检查结构字段的值。要在命令行查看结构字段的值，请使用点表示法对结构进行索引。有关详细信息，请参阅调试时检查和修改数据和消息。

### Guidelines for Structure Data Types

> 结构数据类型指南

- Define each structure from a `Simulink.Bus` object in the base workspace.
  > 从基础工作区中的 `Simulink.Bus` 对象定义每个结构。
- Structures cannot have a constant scope.
  > 结构不能具有恒定的作用域。
- Structures of parameter scope must be tunable.
  > 参数范围的结构必须是可调的。

## See Also 也可以看看

[`Simulink.Bus`](https://ww2.mathworks.cn/help/simulink/slref/simulink.bus.html) (Simulink) `Simulink.Bus` （Simulink）

## Related Topics 相关话题

- [Index and Assign Values to Stateflow Structures
  > 对 Stateflow 结构进行索引和赋值]([https://ww2.mathworks.cn/help/stateflow/ug/structure-operations.html](https://ww2.mathworks.cn/help/stateflow/ug/structure-operations.html))
- [Integrate Custom Structures in Stateflow Charts
  > 在 Stateflow 图中集成自定义结构]([https://ww2.mathworks.cn/help/stateflow/ug/interface-simulink-bus-signals-integrate-custom-c-code.html](https://ww2.mathworks.cn/help/stateflow/ug/interface-simulink-bus-signals-integrate-custom-c-code.html))
- [Add Stateflow Data 添加 Stateflow 数据](https://ww2.mathworks.cn/help/stateflow/ug/adding-data.html)
- [Derive Data Types from Other Data Objects
  > 从其他数据对象派生数据类型]([https://ww2.mathworks.cn/help/stateflow/ug/typing-stateflow-data.html#f7-34857](https://ww2.mathworks.cn/help/stateflow/ug/typing-stateflow-data.html#f7-34857))
- [Inspect and Modify Data and Messages While Debugging
  > 调试时检查和修改数据和消息]([https://ww2.mathworks.cn/help/stateflow/ug/watching-data-values-during-simulation.html](https://ww2.mathworks.cn/help/stateflow/ug/watching-data-values-during-simulation.html))
- [Specify Bus Properties with Simulink.Bus Object Data Types](https://ww2.mathworks.cn/help/simulink/ug/when-to-use-bus-objects.html) (Simulink)
  > 使用 Simulink.Bus 对象数据类型指定总线属性 (Simulink)
- [Composite Interface Guidelines](https://ww2.mathworks.cn/help/simulink/ug/composite-signal-techniques.html) (Simulink)
  > 复合接口指南 (Simulink)
