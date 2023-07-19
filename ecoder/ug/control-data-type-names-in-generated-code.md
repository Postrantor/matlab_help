---
tip: translate by openai@2023-07-06 10:51:34
url: [www.mathworks.com](https://www.mathworks.com/help/ecoder/ug/control-data-type-names-in-generated-code.html)
summary: Control the names of primitive, structure, and enumerated data types in the generated code.
---
## Manage Replacement of Simulink Data Types in Generated Code

You can use the **Data type replacement** configuration parameter to control the replacement of Simulink® data types in generated code:

> 您可以使用**数据类型替换**配置参数来控制生成代码中 Simulink® 数据类型的替换：

- If you set the parameter to `Use C data types with fixed-width integers`, the generated code uses data types from the C99 language standard, which includes definitions from `stdint.h` and `stdbool.h`. See [C99 Data Types in Generated Code](https://www.mathworks.com/help/ecoder/ug/control-data-type-names-in-generated-code.html#mw_940b0954-7f63-47c3-bf6b-e9b7cb458b71).

> - 如果将参数设置为“使用具有固定宽度整数的 C 数据类型”，则生成的代码将使用 C99 语言标准中的数据类型，该标准包括“stdint.h”和“stdbool.h”中的定义。请参阅[C99 生成代码中的数据类型]。

The generated code does not include the [rtwtypes.h](https://www.mathworks.com/help/ecoder/ug/build-process-files.html#f1159207) header file. The file `rtwtypes.h` is generated in some cases because it might be needed by static source code located under `matlabroot`. Additionally, if you use custom code that requires Simulink Coder™ data type definitions, you can force the generation of `rtwtypes.h` by selecting the **Coder typedefs compatibility** check box.

> 生成的代码不包括[rtwtypes.h]头文件。在某些情况下会生成文件“rtwtypes.h”，因为位于“matlabroot”下的静态源代码可能需要该文件。此外，如果您使用需要 Simulink 编码器的自定义代码 ™ 数据类型定义，您可以通过选择 **Coder-typedefs 兼容性**复选框来强制生成“rtwtypes.h”。

- If you set the parameter to `Use coder typedefs`, the code generator creates the header file `rtwtypes.h` and generates data types that are based on the C89 language standard. The `rtwtypes.h` file defines the data types through `typedef` statements that build on C primitive types, for example, `float` and `short`. If you also select the **Specify custom data type names** check box, you can specify replacement names for the Simulink Coder data types. See [Control Names of Simulink Coder Primitive Data Types](https://www.mathworks.com/help/ecoder/ug/control-data-type-names-in-generated-code.html#mw_732e00c6-af35-41d1-b4ec-5ed9bb0eceb8).

> -如果将参数设置为“Use coder typedefs”，代码生成器将创建头文件“rtwtypes.h”，并生成基于 C89 语言标准的数据类型。“rtwtypes.h”文件通过“typedef”语句定义数据类型，这些语句建立在 C 基元类型上，例如“float”和“short”。如果同时选中**指定自定义数据类型名称**复选框，则可以指定 Simulink 编码器数据类型的替换名称。参见[Simulink 编码器基本数据类型的控制名称]([https://www.mathworks.com/help/ecoder/ug/control-data-type-names-in-generated-code.html#mw_732e00c6-af35-41d1-b4ec-5ed9b0eceb8](https://www.mathworks.com/help/ecoder/ug/control-data-type-names-in-generated-code.html#mw_732e00c6-af35-41d1-b4ec-5ed9b0eceb8)）。

By default, the generated code aggregates signal, block parameter, and state data into structures (see [How Generated Code Exchanges Data with an Environment](https://www.mathworks.com/help/ecoder/ug/how-generated-code-exchanges-data-with-an-environment.html) and [How Generated Code Stores Internal Signal, State, and Parameter Data](https://www.mathworks.com/help/ecoder/ug/how-generated-code-stores-internal-signal-state-and-parameter-data.html)). You can specify a naming rule for the structure types. You can also place data items into separate custom structures and specify names for the custom structure types. See [Control Names of Structure Types](https://www.mathworks.com/help/ecoder/ug/control-data-type-names-in-generated-code.html#mw_3e879c92-aa82-4535-abde-ad8ec9f0fe4e).

> 默认情况下，生成的代码将信号、块参数和状态数据聚合到结构中（请参阅[生成的代码如何与环境交换数据](https://www.mathworks.com/help/ecoder/ug/how-generated-code-exchanges-data-with-an-environment.html)和[生成的代码如何存储内部信号、状态和参数数据](https://www.mathworks.com/help/ecoder/ug/how-generated-code-stores-internal-signal-state-and-parameter-data.html))。可以为结构类型指定命名规则。还可以将数据项放置到单独的自定义结构中，并指定自定义结构类型的名称。参见[结构类型的控件名称]([https://www.mathworks.com/help/ecoder/ug/control-data-type-names-in-generated-code.html#mw_3e879c92-aa82-4535-abde-ad8ec9f0fe4e](https://www.mathworks.com/help/ecoder/ug/control-data-type-names-in-generated-code.html#mw_3e879c92-aa82-4535-abde-ad8ec9f0fe4e)）。

Use the naming control to:

You can also reuse data type definitions in generated code, for example, `typedef` statements from external code. See [Generate Code That Reuses Data Types From External Code](https://www.mathworks.com/help/ecoder/ug/control-data-type-names-in-generated-code.html#mw_8100ed74-c1dd-494c-af6a-24819f85a848).

> 您还可以在生成的代码中重用数据类型定义，例如外部代码中的“typedef”语句。请参阅[从外部代码生成重用数据类型的代码]([https://www.mathworks.com/help/ecoder/ug/control-data-type-names-in-generated-code.html#mw_8100ed74-c1dd-494c-af6a-24819f85a848](https://www.mathworks.com/help/ecoder/ug/control-data-type-names-in-generated-code.html#mw_8100ed74-c1dd-494c-af6a-24819f85a848)）。

### C99 Data Types in Generated Code

When the **Data type replacement** configuration parameter is set to `Use C data types with fixed-width integers`, the code generator replaces Simulink data types with C99 data types as shown in this table.

> 当**数据类型替换**配置参数设置为“使用具有固定宽度整数的 C 数据类型”时，代码生成器将 Simulink 数据类型替换为 C99 数据类型，如下表所示。

<table><colgroup><col width="50%"><col width="50%"></colgroup><thead><tr><th>Simulink Name</th><th>Code Generation Name</th></tr></thead><tbody><tr><td><p><code>double</code></p></td><td><p><code>double</code></p></td></tr><tr><td><p><code>single</code></p></td><td><p><code>float</code></p></td></tr><tr><td><p><code>int32</code></p></td><td><p><code>int32_t</code></p></td></tr><tr><td><p><code>int16</code></p></td><td><p><code>int16_t</code></p></td></tr><tr><td><p><code>int8</code></p></td><td><p><code>int8_t</code></p></td></tr><tr><td><p><code>uint32</code></p></td><td><p><code>uint32_t</code></p></td></tr><tr><td><p><code>uint16</code></p></td><td><p><code>uint16_t</code></p></td></tr><tr><td><p><code>uint8</code></p></td><td><p><code>uint8_t</code></p></td></tr><tr><td><p><code>boolean</code></p></td><td><p><code>bool</code></p></td></tr><tr><td><p><code>int</code></p></td><td><p><code>int</code></p></td></tr><tr><td><p><code>uint</code></p></td><td><p><code>unsigned int</code></p></td></tr><tr><td><p><code>char</code></p></td><td><p><code>char</code></p></td></tr><tr><td><p><code>uint64</code></p></td><td><p><code>uint64_t</code></p></td></tr><tr><td><p><code>int64</code></p></td><td><p><code>int64_t</code></p></td></tr></tbody></table>

You cannot specify replacements for the generated data type names.

### Control Names of Simulink Coder Primitive Data Types

To rename a Simulink Coder primitive data type such as `int8_T`, create a `Simulink.AliasType` object whose name matches the type name that you want the generated code to use. For example, to create a type named `myType`, at the command prompt, enter:

> 要重命名 Simulink 编码器基元数据类型（如“int8_T”），请创建一个“Simulink.AliasType”对象，该对象的名称与您希望生成的代码使用的类型名称相匹配。例如，要创建名为“myType”的类型，请在命令提示下输入：

```c
myType = Simulink.AliasType;

```

Set the `BaseType` property to the name of the Simulink data type that corresponds to the target Simulink Coder data type. For example, if the target type is `int8_T`, specify `int8`. To identify the Simulink data type, use the information in the table.

> 将“BaseType”属性设置为与目标 Simulink 编码器数据类型相对应的 Simulink 数据类型的名称。例如，如果目标类型为“int8_T”，请指定“int8”。要识别 Simulink 数据类型，请使用表中的信息。

<table><colgroup><col width="50%"><col width="50%"></colgroup><thead><tr><th>Simulink Coder Type Name</th><th>Corresponding Simulink Type Name</th></tr></thead><tbody><tr><td><p><code>real_T</code></p></td><td><p><code>double</code></p></td></tr><tr><td><p><code>real32_T</code></p></td><td><p><code>single</code></p></td></tr><tr><td><p><code>int32_T</code></p></td><td><p><code>int32</code></p></td></tr><tr><td><p><code>int16_T</code></p></td><td><p><code>int16</code></p></td></tr><tr><td><p><code>int8_T</code></p></td><td><p><code>int8</code></p></td></tr><tr><td><p><code>uint32_T</code></p></td><td><p><code>uint32</code></p></td></tr><tr><td><p><code>uint16_T</code></p></td><td><p><code>uint16</code></p></td></tr><tr><td><p><code>uint8_T</code></p></td><td><p><code>uint8</code></p></td></tr><tr><td><p><code>boolean_T</code></p></td><td><p><code>boolean</code></p><p>For configuring data type replacement throughout an entire model (&gt;&gt; ), you can alternatively use one of these types:</p><ul><li><p><code>uint8</code></p></li><li><p><code>int8</code></p></li><li><p><code>int</code><em><code>n*</code></em></p></li></ul></td></tr><tr><td><p><code>int_T</code></p></td><td><p><code>int</code><em><code>n*</code></em></p></td></tr><tr><td><p><code>uint_T</code></p></td><td><p><code>uint</code><em><code>n*</code></em></p></td></tr><tr><td><p><code>char_T</code></p></td><td><p><code>int</code><em><code>n*</code></em></p></td></tr><tr><td><p><code>uint64_T</code></p></td><td><p><code>uint64</code></p></td></tr><tr><td><p><code>int64_T</code></p></td><td><p><code>int64</code></p></td></tr></tbody></table>

- _`n`_ must be 8, 16, 32, or 64.

**Note**

The `boolean_T` `BaseType` must promote to a signed `int`.

For example, to replace the type name `uint8_T` with `myType`, set the `BaseType` property of the `Simulink.AliasType` object to `'int8'`.

> 例如，要将类型名称“uint8_T”替换为“myType”，请将“Simulink.AliasType”对象的“BaseType”属性设置为“uint8”。

```c
myType.BaseType = 'int8';

```

Then, use one or both of these techniques to apply the type to data items in a model:

> 然后，使用以下一种或两种技术将类型应用于模型中的数据项：

- Configure data type replacements. Throughout the code that you generate from a model, you can replace a Simulink Coder data type name with the name of the `Simulink.AliasType` object. You configure data type replacements through model configuration parameters (>> .

> -配置数据类型替换。在从模型生成的整个代码中，可以将 Simulink 编码器数据类型名称替换为“Simulink.AliasType”对象的名称。您可以通过模型配置参数（>>）配置数据类型替换。

For an example that shows how to use data type replacement, see [Replace and Rename Simulink Coder Data Types to Conform to Coding Standards](https://www.mathworks.com/help/ecoder/ug/conform-to-coding-standards-by-renaming-data-types.html).

> 有关如何使用数据类型替换的示例，请参阅[替换和重命名 Simulink 编码器数据类型以符合编码标准](https://www.mathworks.com/help/ecoder/ug/conform-to-coding-standards-by-renaming-data-types.html)。

- Use the name of the `Simulink.AliasType` object to specify the data type of an individual signal, that is, a block output, or block parameter. By default, due to data type propagation and inheritance (see [Data Type Inheritance Rules](https://www.mathworks.com/help/simulink/ug/control-signal-data-types.html#brc7ijf)), the signals, states, and parameters of other downstream blocks typically inherit the same data type. Optionally, you can configure data items in upstream blocks to inherit the type (`Inherit: Inherit via back propagation`) or stop the propagation at an arbitrary block by specifying a noninherited data type setting.

> -使用“Simulink.AliasType”对象的名称指定单个信号的数据类型，即块输出或块参数。默认情况下，由于数据类型传播和继承（请参见[数据类型继承规则](https://www.mathworks.com/help/simulink/ug/control-signal-data-types.html#brc7ijf))，其他下游块的信号、状态和参数通常继承相同的数据类型。或者，您可以将上游块中的数据项配置为继承类型（`inherit:inherit via back propagation `），或者通过指定非继承数据类型设置来停止任意块上的传播。

To specify a data type for an individual data item, use the Model Data Editor (on the **Modeling** tab, click **Model Data Editor**). You can use the name of the same `Simulink.AliasType` object to specify the data types of multiple data items.

> 要为单个数据项指定数据类型，请使用模型数据编辑器（在**建模**选项卡上，单击**模型数据编辑器**）。您可以使用同一个“Simulink.AliasType”对象的名称来指定多个数据项的数据类型。

For an example that shows how to use a `Simulink.AliasType` object directly in a model, see [Create Data Type Alias in the Generated Code](https://www.mathworks.com/help/ecoder/ug/control-data-type-names-in-generated-code.html#buq4r4l-1).

> 有关如何在模型中直接使用“Simulink.AliasType”对象的示例，请参见[在生成的代码中创建数据类型别名]([https://www.mathworks.com/help/ecoder/ug/control-data-type-names-in-generated-code.html#buq4r4l-1](https://www.mathworks.com/help/ecoder/ug/control-data-type-names-in-generated-code.html#buq4r4l-1)） 。

<table><colgroup><col width="50%"><col width="50%"></colgroup><thead><tr><th>Control Names of Primitive Types</th><th>Technique</th></tr></thead><tbody><tr><td>Change the name of a primitive type that the generated code uses to define variables by default (for example, <code>int8_T</code>).</td><td><p>Configure a data type replacement for an entire model.</p><p>To replace a default Simulink Coder data type name with the corresponding Simulink type name, for example, to replace <code>int8_T</code> with <code>int8</code>, you do not need to create a <code>Simulink.AliasType</code> object. See <a href="https://www.mathworks.com/help/ecoder/ug/control-data-type-names-in-generated-code.html#mw_1c27ec0e-c8f7-4b10-b9e4-c1d6d49c5cd9">Use Simulink Data Type Names Instead of Simulink Coder Data Type Names</a>.</p><p>To share data type replacements throughout a model reference hierarchy, use referenced configuration sets (see <a href="https://www.mathworks.com/help/simulink/ug/sharing-a-configuration-set-between-referenced-models.html">Share a Configuration Across Referenced Models</a>).</p></td></tr><tr><td>Configure the generated code to define a particular data item, such as a variable, by using a specific, meaningful type name.</td><td><p>In the model, locate the data item that corresponds to the variable. For example, trace from the code generation report to the model. Use the name of the <code>Simulink.AliasType</code> object to set the data type of the item.</p><p>If necessary, to prevent other data items in upstream and downstream blocks from using the same type name, configure those items to use a data type setting that is not inherited. By default, most signals use the inherited type <code>Inherit: Inherit via internal rule</code>.</p></td></tr><tr><td>Use the same type name for multiple signals and other data items in a data path, which is a series of connected blocks.</td><td><p>In the model, use the name of a <code>Simulink.AliasType</code> object to set the data type of one of the signals in the path, for example, a root-level Inport block or a Constant block. For example, if the path begins with an Inport block at the root level of the model, you can specify the type in that block. By default, due to data type propagation, the data items in the other blocks typically inherit the same type.</p><p>Usually, no matter where on the data path you specify the type, downstream data items inherit the type. You can configure upstream data items to inherit the type, too. Consider specifying the type in a block that you do not expect to remove or change frequently.</p></td></tr><tr><td>Configure the generated code to replace an implementation-dependent type, such as <code>char_T</code> or <code>boolean_T</code>, and another type of equivalent bit length with a single type name that you specify.</td><td>Use the name of the <code>Simulink.AliasType</code> object to configure multiple data type replacements simultaneously. See <a href="https://www.mathworks.com/help/ecoder/ug/control-data-type-names-in-generated-code.html#mw_2d388705-2173-4f87-af6f-44192197453c">Replace Implementation-Dependent Types with the Same Type Name</a>.</td></tr></tbody></table>

#### Use Simulink Data Type Names Instead of Simulink Coder Data Type Names

For consistency between the data type names that you see in a model and in the generated code, you can configure data type replacements so the code uses Simulink type names instead of Simulink Coder names. For each Simulink Coder type that you want to rename, use the Simulink data type name to specify the replacement name. You do not need to create `Simulink.AliasType` objects.

> 为了在模型和生成的代码中看到的数据类型名称之间保持一致，可以配置数据类型替换，以便代码使用 Simulink 类型名称而不是 Simulink 编码器名称。对于要重命名的每个 Simulink 编码器类型，请使用 Simulink 数据类型名称指定替换名称。您不需要创建“Simulink.AliasType”对象。

![](https://www.mathworks.com/help/ecoder/ug/dtr_int8t_int8.png)

To replace `boolean_T`, `int_T`, or `uint_T`, use the information in the table.

> 要替换“boolean_T”、“int_T”或“uint_T”，请使用表中的信息。

<table><colgroup><col width="50%"><col width="50%"></colgroup><thead><tr><th>Simulink Coder Type Name</th><th>Replacement Names to Use</th></tr></thead><tbody><tr><td><p><code>boolean_T</code></p></td><td><p>One of these names:</p><ul><li><p><code>boolean</code></p></li><li><p><code>uint8</code></p></li><li><p><code>int8</code></p></li><li><p><code>int</code><em><code>n*</code></em></p></li></ul></td></tr><tr><td><p><code>int_T</code></p></td><td><p><code>int</code><em><code>n*</code></em></p></td></tr><tr><td><p><code>uint_T</code></p></td><td><p><code>uint</code><em><code>n*</code></em></p></td></tr><tr><td><p><code>char_T</code></p></td><td><p><code>int</code><em><code>n*</code></em></p></td></tr></tbody></table>

- Replace _`n`_ with the number of bits displayed in > for either **Number of bits: int** or **Number of bits: char**. Use the appropriate number of bits for the data type that you want to replace.

> -将*`n`*替换为 > 中显示的位数，表示**位数：int** 或**位数：char**。为要替换的数据类型使用适当的位数。

You cannot use this technique to replace `real_T` with `double` or `real32_T` with `single`.

> 不能使用此技术将“real_T”替换为“double”或将“real32_T”替换为‘single’。

#### Replace Implementation-Dependent Types with the Same Type Name

Some Simulink Coder type names map to C primitives that are implementation-dependent. For example, the Simulink Coder type `int_T` maps to the C type `int`, whose bit length depends on the native integer size of your target hardware. Other implementation-dependent types include `boolean_T`, `char_T`, and `uint_T`.

> 一些 Simulink 编码器类型名称映射到依赖于实现的 C 基元。例如，Simulink 编码器类型“int_T”映射到 C 类型“int”，其比特长度取决于目标硬件的本地整数大小。其他依赖于实现的类型包括“boolean_T”、“char_T”和“uint_T”。

For more readable, simpler code, you can use the same type name to replace multiple Simulink Coder types of the same size. For example, if the native integer size of your hardware is 32 bits, you can replace `int_T` and `int32_T` with the same type name, say, `myInt`.

> 为了获得更可读、更简单的代码，可以使用相同的类型名称来替换相同大小的多个 Simulink 编码器类型。例如，如果硬件的本机整数大小为 32 位，则可以将“int_T”和“int32_T”替换为相同的类型名称，例如“myInt”。

1. Configure your target hardware settings in > .
2. Create a `Simulink.AliasType` object named `myInt`. In this case, because `int_T` and `int32_T` represent a 32-bit signed integer, set `BaseType` to `int32`.

> 2.创建一个名为“myInt”的“Simulink.AliasType”对象。在这种情况下，由于“int_T”和“int32_T”表示 32 位有符号整数，请将“BaseType”设置为“int32”。

```
```

myInt = Simulink.AliasType('int32')

```
```

3. Configure the same data type replacement for `int32_T` and `int_T`.

   ![](https://www.mathworks.com/help/ecoder/ug/dtr_myint.png)

**Note**

Many-to-one data type replacement does not support the `char` (`char_T`) built-in data type. Use `char` only in one-to-one data type replacements.

> 多对一数据类型替换不支持“char”（“char_T”）内置数据类型。仅在一对一的数据类型替换中使用“char”。

Many-to-one data type replacement is not supported for Simulink Coder data types of different sizes.

> 不同大小的 Simulink 编码器数据类型不支持多对一数据类型替换。

#### Define Abstract Numeric Types and Rename Types

This model shows user-defined types, consisting of numeric and alias types. Numeric types allow you to define abstract numeric types, which is particularly useful for fixed-point types. Alias types allow you to rename types, which allows you create a relationship for types.

> 此模型显示用户定义的类型，包括数字类型和别名类型。数字类型允许您定义抽象数字类型，这对于定点类型特别有用。别名类型允许您重命名类型，从而为类型创建关系。

**Explore Example Model**

Open the example model and configure it to show the generated names of blocks.

> 打开示例模型并将其配置为显示生成的块名称。

```c
load_system('UserDefinedDataTypes')
set_param('UserDefinedDataTypes','HideAutomaticNames','off')
open_system('UserDefinedDataTypes')
```

![](https://www.mathworks.com/help/examples/ecoder/win64/UserDefinedDataTypesDemoExample_01.png)

**Key Features of User-Defined Types**

- Displayed and propagated on signal lines
- Used to parameterize a model by type (e.g., In1 specifies its **Output data type** as `ENG_SPEED`)

> -用于按类型参数化模型（例如，In1 将其**输出数据类型**指定为“ENG_SPEED”）

- Types with a common ancestor can be mixed, whereby the common ancestor is propagated (e.g., output of Sum1)

> -具有共同祖先的类型可以混合，从而传播共同祖先（例如，Sum1 的输出）

- Intrinsically supported by the Simulink Model Explorer
- Include an optional header file attribute that is ideal for importing legacy types (ignored for GRT targets)

> -包括一个可选的头文件属性，该属性非常适合导入旧类型（对于 GRT 目标忽略）

- Types used in the generated code (ignored for GRT targets)

**Instructions**

1. Inspect the user-defined types in the Model Explorer by double-clicking the first yellow button below.

> 1.双击下面的第一个黄色按钮，在模型资源管理器中检查用户定义的类型。

2. Inspect the replacement data type mapping by double-clicking the second yellow button below.

> 2.双击下面的第二个黄色按钮，检查替换数据类型映射。

3. Compile the diagram to display the types in this model (Debug> Update Model > Update Model or **Ctrl+D**).

> 3.编译图表以显示此模型中的类型（调试 > 更新模型 > 更新模型或 **Ctrl+D**）。

4. Generate code with the blue button below and inspect model files to see how user-defined types appear in the generated code.

> 4.使用下面的蓝色按钮生成代码，并检查模型文件，以查看用户定义的类型如何出现在生成的代码中。

5. Modify the attributes of `ENG_SPEED` and `ENG_SPEED_OFFSET` and repeat steps 1-4.

> 5.修改“ENG_SPEED”和“ENG_SPEED_OFFSET”的属性，并重复步骤 1-4。

**Notes**

- User-defined types are a feature of Simulink that facilitate parameterization of the data types in a model. Embedded Coder preserves alias data type names (e.g., `ENG_SPEED`) in the generated code, whereas Simulink Coder implements user-defined types as their base type (e.g., `real32_T`).

> -用户定义类型是 Simulink 的一个功能，有助于对模型中的数据类型进行参数化。嵌入式编码器在生成的代码中保留别名数据类型名称（例如“ENG_SPEED”），而 Simulink 编码器实现用户定义的类型作为其基本类型（例如“real32_T”）。

- Embedded Coder also enables you to replace the built-in data types with user-defined data types in the generated code.

> -嵌入式编码器还使您能够在生成的代码中用用户定义的数据类型替换内置的数据类型。

### Control Names of Structure Types

To control the names of the standard structures that Simulink Coder creates by default to store data (``_`model`__P`` for parameter data, for example), use > > > to specify a naming rule. For more information, see [Identifier Format Control](https://www.mathworks.com/help/ecoder/ug/specify-identifier-formats.html).

> 要控制 Simulink 编码器默认创建的用于存储数据的标准结构的名称（例如，参数数据为 `_` model `__P`），请使用 >> 指定命名规则。有关详细信息，请参阅[标识符格式控制](https://www.mathworks.com/help/ecoder/ug/specify-identifier-formats.html)。

When you use nonvirtual buses and parameter structures to aggregate signals and block parameters into a custom structure in the generated code, control the name of the structure type by creating a `Simulink.Bus` object. For more information, see [Organize Data into Structures in Generated Code](https://www.mathworks.com/help/ecoder/ug/organize-data-into-structures-in-generated-code.html).

> 当您使用非虚拟总线和参数结构将信号和块参数聚合到生成代码中的自定义结构中时，请通过创建“Simulink.Bus”对象来控制结构类型的名称。有关更多信息，请参阅[在生成的代码中将数据组织到结构中](https://www.mathworks.com/help/ecoder/ug/organize-data-into-structures-in-generated-code.html)。

### Generate Code That Reuses Data Types From External Code

To generate code that reuses a data type definition from your external C code, specify the data scope of the corresponding data type object or enumeration in Simulink as `Imported`. With this setting, the generated code includes (`#include`) the definition from your code. For more information about controlling the file placement of a custom data type, see [Control File Placement of Custom Data Types](https://www.mathworks.com/help/ecoder/ug/specify-location-of-user-defined-type-definitions.html).

> 要从外部 C 代码生成重用数据类型定义的代码，请将 Simulink 中相应数据类型对象或枚举的数据范围指定为“导入”。使用此设置，生成的代码将包含（`#include`）代码中的定义。有关控制自定义数据类型的文件放置的更多信息，请参见[控制自定义数据类的文件放置](https://www.mathworks.com/help/ecoder/ug/specify-location-of-user-defined-type-definitions.html)。

Instead of creating individual data type objects and enumerated types, and then configuring them, consider creating the objects and types by using the `Simulink.importExternalCTypes` function. By default, the function configures the new objects and types so that the generated code imports (reuses) the type definitions from your code. You can then use the objects and types to set data types in a model and to configure data type replacements. For more information, see [`Simulink.importExternalCTypes`](https://www.mathworks.com/help/simulink/slref/simulink.importexternalctypes.html) and [Exchange Structured and Enumerated Data Between Generated and External Code](https://www.mathworks.com/help/ecoder/ug/exchange-structured-and-enumerated-data-between-generated-and-external-code.html).

> 请考虑使用“Simulink.importExternalCTypes”函数创建对象和类型，而不是创建单独的数据类型对象和枚举类型，然后对其进行配置。默认情况下，该函数配置新的对象和类型，以便生成的代码从代码中导入（重用）类型定义。然后，可以使用对象和类型设置模型中的数据类型，并配置数据类型替换。有关详细信息，请参阅[`Simulink.importExternalCTypes`](https://www.mathworks.com/help/simulink/slref/simulink.importexternalctypes.html)和[在生成的代码和外部代码之间交换结构化和枚举的数据](https://www.mathworks.com/help/ecoder/ug/exchange-structured-and-enumerated-data-between-generated-and-external-code.html)。

### Create Data Type Alias in the Generated Code

This example shows how to configure the generated code to use a data type name (`typedef`) that you specify.

> 此示例显示如何将生成的代码配置为使用您指定的数据类型名称（“typedef”）。

**Export Type Definition**

When you integrate code generated from a model with code from other sources, your model code can create an exported `typedef` statement. Therefore, all of the integrated code can use the type. This example shows how to export the definition of a data type to a generated header file.

> 当您将模型生成的代码与其他源代码集成时，您的模型代码可以创建导出的“typedef”语句。因此，所有的集成代码都可以使用类型。此示例显示如何将数据类型的定义导出到生成的头文件中。

Create a `Simulink.AliasType` object named `mySingleAlias` that acts as an alias for the built-in data type `single`.

> 创建名为“mySingleAlias”的“Simulink.AliasType”对象，该对象用作内置数据类型“single”的别名。

```c
mySingleAlias = Simulink.AliasType('single');
```

Configure the object to export its definition to a header file called `myHdrFile.h`.

> 配置对象以将其定义导出到名为“myHdrFile.h”的头文件中。

```c
mySingleAlias.DataScope = 'Exported';
mySingleAlias.HeaderFile = 'myHdrFile.h';
```

Open the model `rtwdemo_configinterface`.

```c
open_system('rtwdemo_configinterface')
```

![](https://www.mathworks.com/help/examples/ecoder/win64/CreateDataTypeAliasInTheGeneratedCodeExample_01.png)

Configure the model to show the generated names of blocks.

```c
set_param('rtwdemo_configinterface','HideAutomaticNames','off')
```

On the **Modeling** tab, click **Model Data Editor**.

In the model, select the Inport block labeled `In1`.

Use the **Data Type** column to set the data type to `mySingleAlias`.

```c
set_param('rtwdemo_configinterface/In1','OutDataTypeStr','mySingleAlias')
```

Configure `In1` to use default storage.

In the **C Code** tab, select **Code Interface > Default Code Mappings**.

In the Code Mappings editor, under **Inports and Outports**, select category **Inports**. Set the default storage class to `Default`.

> 在代码映射编辑器中，在**输入和输出**下，选择类别**输入**。将默认存储类设置为“default”。

On the **Inports** tab, set **Storage Class** to `Model default`.

```c
cm = coder.mapping.api.get('rtwdemo_configinterface');
setDataDefault(cm,'Inports','StorageClass','Default');
setInport(cm,'In1','StorageClass','Model default');
```

Generate code from the model.

```c
slbuild('rtwdemo_configinterface')
```

```c
### Starting build procedure for: rtwdemo_configinterface
### Successful completion of code generation for: rtwdemo_configinterface

Build Summary

Top model targets built:

Model                    Action           Rebuild Reason
============================================================================================
rtwdemo_configinterface  Code generated.  Code generation information file does not exist.

1 of 1 models built (0 models already up to date)
Build duration: 0h 0m 8.5824s
```

In the code generation report, view the file `rtwdemo_configinterface_types.h`. The code creates a `#include` directive for the generated file `myHdrFile.h`.

> 在代码生成报告中，查看文件 `rtwdemo_configinterface_types.h`。该代码为生成的文件 `myHdrFile.h ` 创建一个 `#include` 指令。

```c
file = fullfile('rtwdemo_configinterface_ert_rtw','rtwdemo_configinterface_types.h');
rtwdemodbtype(file,'#include "myHdrFile.h"',...
    '#include "myHdrFile.h"',1,1)
```

View the file `myHdrFile.h`. The code uses the identifier `mySingleAlias` as an alias for the data type `real32_T`. By default, generated code represents the Simulink data type `single` by using the identifier `real32_T`.

> 查看文件“myHdrFile.h”。该代码使用标识符“mySingleAlias”作为数据类型“real32_T”的别名。默认情况下，生成的代码使用标识符“real32_T”表示 Simulink 数据类型“single”。

The code also provides a macro guard of the form `RTW_HEADER_filename_h_`. When you export a data type definition to integrate generated code with code from other sources, you can use macro guards of this form to prevent unintentional identifier clashes.

> 该代码还提供了形式为 `RTW_HEADER_filename_h_` 的宏保护。当导出数据类型定义以将生成的代码与其他源代码集成时，可以使用此形式的宏保护来防止意外的标识符冲突。

```c
file = fullfile('slprj','ert','_sharedutils','myHdrFile.h');
rtwdemodbtype(file,'#ifndef RTW_HEADER_myHdrFile_h_',...
    ' * File trailer for generated code.',1,0)
```

```c
#ifndef RTW_HEADER_myHdrFile_h_
#define RTW_HEADER_myHdrFile_h_
#include "rtwtypes.h"

typedef real32_T mySingleAlias;
typedef creal32_T cmySingleAlias;

#endif                                 /* RTW_HEADER_myHdrFile_h_ */

/*
```

View the file `rtwdemo_configinterface.h`. The code uses the data type alias `mySingleAlias` to define the structure field `input1`, which corresponds to the Inport block labeled `In1`.

> 查看文件“rtwdemo_configinterface.h”。该代码使用数据类型别名“mySingleAlias”来定义结构字段“input1”，该字段对应于标记为“In1”的 Inport 块。

```c
file = fullfile('rtwdemo_configinterface_ert_rtw','rtwdemo_configinterface.h');
rtwdemodbtype(file,...
    '/* External inputs (root inport signals with default storage) */',...
    '} ExtU_rtwdemo_configinterface_T;',1,1)
```

```c
/* External inputs (root inport signals with default storage) */
typedef struct {
  mySingleAlias input1;                /* '<Root>/In1' */
  MYTYPE input2;                       /* '<Root>/In2' */
  MYTYPE input3;                       /* '<Root>/In3' */
  MYTYPE input4;                       /* '<Root>/In4' */
} ExtU_rtwdemo_configinterface_T;
```

**Import Type Definition**

When you integrate code generated from a model with code from other sources, to avoid redundant `typedef` statements, you can import a data type definition from the external code. This example shows how to import your own definition of a data type from a header file that you create.

> 将模型生成的代码与其他源代码集成时，为了避免多余的“typedef”语句，可以从外部代码导入数据类型定义。此示例显示如何从您创建的头文件中导入您自己的数据类型定义。

Use a text editor to create a header file to import. Name the file `ex_myImportedHdrFile.h`. Place it in your working folder. Copy the following code into the file.

> 使用文本编辑器创建要导入的头文件。将文件命名为 `ex_myImportedHdrFile.h`。将其放在工作文件夹中。将以下代码复制到文件中。

```c
#ifndef HEADER_myImportedHdrFile_h_
#define HEADER_myImportedHdrFile_h_

typedef float myTypeAlias;

#endif
```

The code uses the identifier `myTypeAlias` to create an alias for the data type `float`. The code also uses a macro guard of the form `HEADER_filename_h`. When you import a data type definition to integrate generated code with code from other sources, you can use macro guards of this form to prevent unintentional identifier clashes.

> 代码使用标识符“myTypeAlias”为数据类型“float”创建别名。该代码还使用了形式为“HEADER_filename_h”的宏保护。当导入数据类型定义以将生成的代码与其他源代码集成时，可以使用此表单的宏保护来防止意外的标识符冲突。

At the command prompt, create a `Simulink.AliasType` object named `myTypeAlias` that creates an alias for the built-in type `single`. The Simulink data type `single` corresponds to the C data type `float`.

> 在命令提示符下，创建一个名为“myTypeAlias”的“Simulink.AliasType”对象，该对象为内置类型“single”创建别名。Simulink 数据类型“single”对应于 C 数据类型“float”。

```c
myTypeAlias = Simulink.AliasType('single')
```

```c
myTypeAlias =

  AliasType with properties:

    Description: ''
      DataScope: 'Auto'
     HeaderFile: ''
       BaseType: 'single'

```

Configure the object so that generated code imports the type definition from the header file `ex_myImportedHdrFile.h`.

> 配置对象，以便生成的代码从头文件 `ex_myImportedHdrFile.h ` 导入类型定义。

```c
myTypeAlias.DataScope = 'Imported';
myTypeAlias.HeaderFile = 'ex_myImportedHdrFile.h';
```

Open the model `rtwdemo_configinterface`.

```c
open_system('rtwdemo_configinterface')
```

On the **Modeling** tab, click **Model Data Editor**.

In the model, select the Inport block labeled `In1`.

Use the **Data Type** column to set the data type to `myTypeAlias`.

```c
set_param('rtwdemo_configinterface/In1','OutDataTypeStr','myTypeAlias')
```

Generate code from the model.

```c
slbuild('rtwdemo_configinterface')
```

```c
### Starting build procedure for: rtwdemo_configinterface
### Successful completion of code generation for: rtwdemo_configinterface

Build Summary

Top model targets built:

Model                    Action           Rebuild Reason
===========================================================================
rtwdemo_configinterface  Code generated.  Generated code was out of date.

1 of 1 models built (0 models already up to date)
Build duration: 0h 0m 7.5863s
```

In the code generation report, view the file `rtwdemo_configinterface_types.h`. The code creates a `#include` directive for your header file `ex_myImportedHdrFile.h`.

> 在代码生成报告中，查看文件 `rtwdemo_configinterface_types.h`。该代码为头文件 `ex_myImportedHdrFile.h` 创建一个 `#include` 指令。

```c
file = fullfile('rtwdemo_configinterface_ert_rtw','rtwdemo_configinterface_types.h');
rtwdemodbtype(file,'#include "ex_myImportedHdrFile.h',...
    '/* Forward declaration for rtModel */',1,0)
```

```c
#include "ex_myImportedHdrFile.h"
#include "MYTYPE.h"
#ifndef DEFINED_TYPEDEF_FOR_Table1_Type_
#define DEFINED_TYPEDEF_FOR_Table1_Type_

typedef struct {
  MYTYPE BP[11];
  MYTYPE Table[11];
} Table1_Type;

#endif

#ifndef DEFINED_TYPEDEF_FOR_Table2_Type_
#define DEFINED_TYPEDEF_FOR_Table2_Type_

typedef struct {
  MYTYPE BP1[3];
  MYTYPE BP2[3];
  MYTYPE Table[9];
} Table2_Type;

#endif
#endif                         /* RTW_HEADER_rtwdemo_configinterface_types_h_ */

/*
 * File trailer for generated code.
 *
 * [EOF]
 */
```

View the file `rtwdemo_configinterface.h`. The code uses the data type alias `myTypeAlias` to define the structure field `input1`, which corresponds to the Inport block labeled `In1`.

> 查看文件“rtwdemo_configinterface.h”。该代码使用数据类型别名“myTypeAlias”来定义结构字段“input1”，该字段对应于标记为“In1”的 Inport 块。

```c
file = fullfile('rtwdemo_configinterface_ert_rtw','rtwdemo_configinterface.h');
rtwdemodbtype(file,...
    '/* External inputs (root inport signals with default storage) */',...
    '} ExtU_rtwdemo_configinterface_T;',1,1)
```

```c
/* External inputs (root inport signals with default storage) */
typedef struct {
  myTypeAlias input1;                  /* '<Root>/In1' */
  MYTYPE input2;                       /* '<Root>/In2' */
  MYTYPE input3;                       /* '<Root>/In3' */
  MYTYPE input4;                       /* '<Root>/In4' */
} ExtU_rtwdemo_configinterface_T;
```

**Display Base Data Types and Aliases on Model Diagram**

When you display signal data types on the model diagram, you can choose to display aliases (such as `myTypeAlias`) and base data types (such as `int16`). To display aliases, on the **Debug** tab, select **Information Overlays** > **Alias Data Types**. To display the base types, select **Information Overlays** > **Base Data Types**. For more information, see [Port Data Types](https://www.mathworks.com/help/simulink/ug/displaying-signal-properties.html#f15-90122).

> 在模型图上显示信号数据类型时，可以选择显示别名（如“myTypeAlias”）和基本数据类型（如“int16”）。要显示别名，请在**调试**选项卡上，选择**信息覆盖\*** > **别名数据类型**。要显示基本类型，请选择**信息套印格式\*** > **基本数据类型**。有关更多信息，请参阅[端口数据类型]([https://www.mathworks.com/help/simulink/ug/displaying-signal-properties.html#f15-90122](https://www.mathworks.com/help/simulink/ug/displaying-signal-properties.html#f15-90122)）。

### Create a Named Fixed-Point Data Type in the Generated Code

This example shows how to create and name a fixed-point data type in generated code. You can use the name of the type to specify parameter and signal data types throughout a model and in generated code.

> 此示例显示如何在生成的代码中创建和命名定点数据类型。可以使用类型的名称在整个模型和生成的代码中指定参数和信号数据类型。

The example model `rtwdemo_fixpt1` uses fixed-point data types. So that you can more easily see the fixed-point data type in the generated code, in this example, you create a `Simulink.Parameter` object that appears in the code as a global variable.

> 示例模型“rtwdemo_fixpt1”使用定点数据类型。为了更容易地在生成的代码中看到定点数据类型，在本例中，您创建了一个“Simulink.Parameter”对象，该对象作为全局变量出现在代码中。

Create a `Simulink.AliasType` object that defines a fixed-point data type. Name the object `myFixType`. The generated code uses the name of the object as a data type.

> 创建一个定义定点数据类型的“Simulink.AliasType”对象。将对象命名为 `myFixType`。生成的代码使用对象的名称作为数据类型。

```c
myFixType = Simulink.AliasType('fixdt(1,16,4)');
```

Open the model `rtwdemo_fixpt1`.

```c
open_system('rtwdemo_fixpt1')
```

![](https://www.mathworks.com/help/examples/ecoder/win64/CreateANamedFixedPointDataTypeInTheGeneratedCodeExample_01.png)

Configure the model to show the generated names of the blocks.

```c
set_param('rtwdemo_fixpt1','HideAutomaticNames','off')
```

On the **Modeling** tab, click **Model Data Editor**.

In the Model Data Editor, select the **Parameters** tab.

In the model, select the Gain block.

In the Model Data Editor, for the row that represents the **Gain** parameter of the Gain block, in the **Value** column, specify `myParam`.

> 在模型数据编辑器中，对于表示增益块的**增益**参数的行，在**值**列中，指定“myParam”。

Click the action button (with three vertical dots) next to the parameter value. Select **Create**.

> 单击参数值旁边的操作按钮（带有三个垂直点）。选择**创建**。

In the **Create New Data** dialog box, set `Value` to `Simulink.Parameter(8)`. In this example, for more easily readable code, you set the parameter value to 8 instead of -3.2. Click **Create**. A `Simulink.Parameter` object named `myParam` appears in the base workspace. The object stores the real-world value `8`, which the Gain block uses for the value of the **Gain** parameter.

> 在**创建新数据**对话框中，将“值”设置为“Simulink.Parameter（8）”。在本例中，为了更易于阅读的代码，您将参数值设置为 8，而不是-3.2。单击**创建**。名为“myParam”的“Simulink.Parameter”对象将出现在基本工作区中。该对象存储真实世界的值“8”，增益块将该值用作**增益**参数的值。

In the **Simulink.Parameter** property dialog box, set **Storage class** to `ExportedGlobal`. Click **OK**. With this setting, `myParam` appears in the generated code as a separate global variable.

> 在 **Simulink.Parameter** 属性对话框中，将**存储类**设置为“ExportedGlobal”。单击**确定**。使用此设置，“myParam”将作为单独的全局变量出现在生成的代码中。

In the Model Data Editor, use the **Data Type** column to set the data type of the **Gain** parameter of the Gain block to `myFixType`.

> 在模型数据编辑器中，使用**数据类型**列将增益块的**增益**参数的数据类型设置为“myFixType”。

On the **Signals** tab, use the **Data Type** column to set the data type of the Gain block output to `myFixType`.

> 在**信号**选项卡上，使用**数据类型**列将增益块输出的数据类型设置为“myFixType”。

Use the **Data Type** column to set the data type of the `Conversion` block output to `myFixType`.

> 使用**数据类型**列将“转换”块输出的数据类型设置为“myFixType”。

Alternatively, you can use these commands at the command prompt to configure the blocks and create the object:

> 或者，可以在命令提示下使用以下命令来配置块并创建对象：

```c
set_param('rtwdemo_fixpt1/Gain','Gain','myParam','OutDataTypeStr','myFixType',...
    'ParamDataTypeStr','myFixType')
myParam = Simulink.Parameter(8);
myParam.StorageClass = 'ExportedGlobal';
set_param('rtwdemo_fixpt1/Conversion','OutDataTypeStr','myFixType')
```

In the model, set **Configuration Parameters > Code Generation > System target file** to `ert.tlc`. With this setting, the code generator honors data type aliases such as `myFixType`.

> 在模型中，将**配置参数 > 代码生成 > 系统目标文件**设置为 `ert.tlc`。通过此设置，代码生成器接受数据类型别名，如 `myFixType`。

```c
set_param('rtwdemo_fixpt1','SystemTargetFile','ert.tlc')
```

Select the configuration parameter **Generate code only**.

```c
set_param('rtwdemo_fixpt1','GenCodeOnly','on')
```

Generate code from the model.

```c
slbuild('rtwdemo_fixpt1')
```

```c
### Starting build procedure for: rtwdemo_fixpt1
### Successful completion of code generation for: rtwdemo_fixpt1

Build Summary

Top model targets built:

Model           Action           Rebuild Reason
===================================================================================
rtwdemo_fixpt1  Code generated.  Code generation information file does not exist.

1 of 1 models built (0 models already up to date)
Build duration: 0h 0m 11.166s
```

In the code generation report, view the file `rtwdemo_fixpt1_types.h`. The code defines the type `myFixType` based on an integer type of the specified word length (16).

> 在代码生成报告中，查看文件 `rtwdemo_fixpt1_types.h`。该代码基于指定字长的整数类型定义类型 `myFixType`（16）。

```c
file = fullfile('rtwdemo_fixpt1_ert_rtw','rtwdemo_fixpt1_types.h');
rtwdemodbtype(file,'#ifndef DEFINED_TYPEDEF_FOR_myFixType_',...
    '/* Forward declaration for rtModel */',1,0)
```

```c
#ifndef DEFINED_TYPEDEF_FOR_myFixType_
#define DEFINED_TYPEDEF_FOR_myFixType_

typedef int16_T myFixType;
typedef cint16_T cmyFixType;

#endif
```

View the file `rtwdemo_fixpt1.c`. The code uses the type `myFixType`, which is an alias of the integer type `int16`, to define the variable `myParam`.

> 查看文件 `rtwdemo_fixpt1.c`。该代码使用类型 `myFixType`（整数类型 `int16` 的别名）来定义变量 `myParam`。

```c
file = fullfile('rtwdemo_fixpt1_ert_rtw','rtwdemo_fixpt1.c');
rtwdemodbtype(file,'myFixType myParam = 128;','myFixType myParam = 128;',1,1)
```

```c
myFixType myParam = 128;               /* Variable: myParam
```

The stored integer value `128` of `myParam` is not the same as the real-world value `8` because of the scaling that the fixed-point data type `myFixType` specifies. For more information, see [Scaling](https://www.mathworks.com/help/fixedpoint/ug/scaling.html) (Fixed-Point Designer) in the Fixed-Point Designer documentation.

> 由于定点数据类型“myFixType”指定的缩放，存储的“myParam”的整数值“128”与实际值“8”不同。有关详细信息，请参阅[缩放](https://www.mathworks.com/help/fixedpoint/ug/scaling.html)（固定点设计器）。

The line of code that represents the Gain block applies a right bit shift corresponding to the fraction length specified by `myFixType`.

> 表示增益块的代码行应用与“myFixType”指定的分数长度相对应的右位移位。

```c
rtwdemodbtype(file,...
    'rtwdemo_fixpt1_Y.Out1 = (myFixType)((myParam * rtb_Conversion) >> 4);',...
    'rtwdemo_fixpt1_Y.Out1 = (myFixType)((myParam * rtb_Conversion) >> 4);',1,1)
```

```c
  rtwdemo_fixpt1_Y.Out1 = (myFixType)((myParam * rtb_Conversion) >> 4);
```

### Rename Data Type Object

To rename a data type object such as `Simulink.AliasType` or `Simulink.Bus` after you create it (for example, to rename an alias when coding standards change or when you encounter a naming conflict), you can allow Simulink to rename the object and correct all of the references to the object that appear in a model or models. In the Model Explorer, right-click the variable and select . For more information, see [Rename Variables](https://www.mathworks.com/help/simulink/ug/workspace-variables-in-model-explorer.html#buod58x).

> 要在创建数据类型对象（如“Simulink.AliasType”或“Simulink.Bus”）后重命名该对象（例如，在编码标准更改或遇到命名冲突时重命名别名），可以允许 Simulink 重命名该对象，并更正一个或多个模型中出现的对该对象的所有引用。在模型资源管理器中，在变量上单击鼠标右键，然后选择。有关详细信息，请参阅[重命名变量](https://www.mathworks.com/help/simulink/ug/workspace-variables-in-model-explorer.html#buod58x)。

### Display Signal Data Types on Block Diagram

For readability, you can display signal data types directly on a block diagram. When you use custom names for primitive data types, you can choose to display the custom name (the alias), the underlying primitive, or both. See [Port Data Types](https://www.mathworks.com/help/simulink/ug/displaying-signal-properties.html#f15-90122).

> 为了便于阅读，可以直接在方框图上显示信号数据类型。为基元数据类型使用自定义名称时，可以选择显示自定义名称（别名）、基础基元，或同时显示这两种名称。请参阅[端口数据类型]([https://www.mathworks.com/help/simulink/ug/displaying-signal-properties.html#f15-90122](https://www.mathworks.com/help/simulink/ug/displaying-signal-properties.html#f15-90122)）。

### Data Type Replacement Limitations

- You cannot configure the generated code to use these custom C data types:

  - Array types
  - Pointer types
  - `const` or `volatile` types
- You cannot configure the generated code to use generic C primitive types such as `int` and `short`.

> -不能将生成的代码配置为使用诸如“int”和“short”之类的泛型 C 基元类型。

When you set **Data type replacement** to `Use C data types with fixed-width integers`, these limitations apply:

> 当您将**数据类型替换**设置为“使用具有固定宽度整数的 C 数据类型”时，将应用以下限制：

- The **Specify custom data type names** configuration parameter is not supported.
- Word size values for production integer data types must include the values 8, 16, and 32 bits and each value can only be 8, 16, 32, or 64 bits. If `ProdEqTarget` is set to `'off'`, the same requirement applies to test hardware integer data types.

> -生产整数数据类型的字大小值必须包括值 8、16 和 32 位，并且每个值只能是 8、16、32 或 64 位。如果“ProdEqTarget”设置为“off”，则相同的要求适用于测试硬件整数数据类型。

- You cannot generate AUTOSAR code.
- You cannot run external mode simulations that use TCP/IP or serial communication. Run external mode simulations that use XCP communication.

> -不能运行使用 TCP/IP 或串行通信的外部模式模拟。运行使用 XCP 通信的外部模式模拟。

- You cannot generate code for third-party hardware workflows that use Embedded Coder® support packages.

> -您无法为使用 Embedded Coder® 支持包的第三方硬件工作流生成代码。

- In generated C++ code, statements that include the header files `stdint.h` and `stdbool.h` violate MISRA™ C++:2008 rule 18-0-1.

> -在生成的 C++ 代码中，包含头文件“stdint.h”和“stdbool.h”的语句违反了 MISRA™ C++：2008 年规则 18-0-1。

- If the model includes Simscape™ blocks, some of the generated source files might contain `#include "rtwtypes.h"`. Generated code from Simscape blocks is not intended for production deployment. The build process can compile the generated code because the code generator creates a `rtwtypes.h` file for continuous-time models.

> -如果模型包括 Simscape™ 块，一些生成的源文件可能包含 `#include“rtwtypes.h”`。从 Simscape 块生成的代码不用于生产部署。构建过程可以编译生成的代码，因为代码生成器为连续时间模型创建了一个“rtwtypes.h”文件。

When you set **Data type replacement** to `Use coder typedefs` and select the **Specify custom data type names** configuration parameter, these limitations apply:

> 当您将**数据类型替换**设置为“使用编码器类型定义”并选择**指定自定义数据类型名称**配置参数时，这些限制适用：

- Data type replacement does not support multiple levels of mapping. Each replacement data type name maps directly to one or more built-in data types.

> -数据类型替换不支持多级映射。每个替换数据类型名称直接映射到一个或多个内置数据类型。

- Data type replacement is not supported for simulation target code generation for referenced models.

> -引用模型的模拟目标代码生成不支持数据类型替换。

- If you select the configuration parameter for your model, data type replacement is not supported.

> -如果为模型选择配置参数，则不支持数据类型替换。

- Code generation performs data type replacements while generating `.c`, `.cpp`, and `.h` files. Code generation places these files in build folders (including top and referenced model build folders) and in the `_sharedutils` folder. _Exceptions_ are as follows:

> -代码生成在生成“.c”、“.cpp”和“.h”文件时执行数据类型替换。代码生成将这些文件放置在生成文件夹（包括顶部和引用的模型生成文件夹）和“*sharedutils”文件夹中*例外情况如下：

  <table summary="Simple list"><tbody><tr><td><code>rtwtypes.h</code></td></tr><tr><td><code>multiword_types.h</code></td></tr><tr><td><code>model_reference_types.h</code></td></tr><tr><td><code>builtin_typeid_types.h</code></td></tr><tr><td><code>model_sf.c</code> or <code>.cpp</code> (ERT S-function wrapper)</td></tr><tr><td><code>model_dt.h</code> (C header file supporting external mode)</td></tr><tr><td><code>model_capi.c</code> or <code>.cpp</code></td></tr><tr><td><code>model_capi.h</code></td></tr></tbody></table>

> <table summary=“Simple list”><tbody><tr><td><code>rtwtypes.h</code></td></tr><tr>多字类型.h</code><td></tr></tr><code>model_reference_types.h</code></td></tr><tr>>内置类型.h</code></td>><tr></tr><td><td>model_sf.c</code>或<code>.cpp（ERT S-函数包装器）</td></tr><tr><td><code>model_dt.h</code>（支持外部模式的c头文件）</td></tr><tr><td><code>model_capi.c</code>或<code>.cpp</td><tr><tr></td>model_capi h</code></td></tr></tbody></table>

- Data type replacement is not supported for complex data types.
- Many-to-one data type replacement is not supported for the `char` data type. Attempting to use `char` as part of a many-to-one mapping to a custom data type represents a violation of the MISRA C™ specification. For example, if you map `char` (`char_T`) and either `int8` (`int8_T`) or `uint8` (`uint8_T`) to the same replacement type, the result is a MISRA C violation. If you try to generate C++ code, the code generator makes invalid implicit type casts, resulting in compile-time errors. Use `char` only in one-to-one data type replacements.

> -“char”数据类型不支持多对一数据类型替换。试图使用“char”作为到自定义数据类型的多对一映射的一部分表示违反了 MISRA C™ 规格例如，如果将 `char`（`char_T`）和 `int8`（`int8_T`）或 `uint8`（` uint8_T`。如果尝试生成 C++ 代码，代码生成器会进行无效的隐式类型转换，从而导致编译时错误。仅在一对一的数据类型替换中使用“char”。

Many-to-one data type replacement is not supported for Simulink Coder data types of different sizes. For more information, see [Replace Implementation-Dependent Types with the Same Type Name](https://www.mathworks.com/help/ecoder/ug/control-data-type-names-in-generated-code.html#mw_2d388705-2173-4f87-af6f-44192197453c).

> 不同大小的 Simulink 编码器数据类型不支持多对一数据类型替换。有关更多信息，请参阅[用相同的类型名称替换实现相关类型]([https://www.mathworks.com/help/ecoder/ug/control-data-type-names-in-generated-code.html#mw_2d388705-2173-4f87-af6f-44192197453c](https://www.mathworks.com/help/ecoder/ug/control-data-type-names-in-generated-code.html#mw_2d388705-2173-4f87-af6f-44192197453c)）。

- For ERT S-functions, replace the `boolean` data type with only an 8-bit integer, `int8`, or `uint8`.

> -对于 ERT S 函数，将“boolean”数据类型仅替换为 8 位整数、“int8”或“uint8”。

- Set the `DataScope` property of a `Simulink.AliasType` object to `Auto` (default) or `Imported`.

## Related Topics

- [Exchange Structured and Enumerated Data Between Generated and External Code](https://www.mathworks.com/help/ecoder/ug/exchange-structured-and-enumerated-data-between-generated-and-external-code.html)
- [Replace and Rename Simulink Coder Data Types to Conform to Coding Standards](https://www.mathworks.com/help/ecoder/ug/conform-to-coding-standards-by-renaming-data-types.html)

You have a modified version of this example. Do you want to open this example with your edits?

> 您有此示例的修改版本。是否要使用编辑打开此示例？

How useful was this information?

Unrated

1 star

2 stars

3 stars

4 stars

5 stars

Unrated 1 star 2 stars 3 stars 4 stars 5 stars

You clicked a link that corresponds to this MATLAB command:

Run the command by entering it in the MATLAB Command Window. Web browsers do not support MATLAB commands.

> 通过在 MATLAB 命令窗口中输入命令来运行该命令。Web 浏览器不支持 MATLAB 命令。

Select a Web Site

Choose a web site to get translated content where available and see local events and offers. Based on your location, we recommend that you select: .

> 选择一个网站以获取可用的翻译内容，并查看当地活动和优惠。根据您的位置，我们建议您选择：。

You can also select a web site from the following list:

[Contact your local office](#)
