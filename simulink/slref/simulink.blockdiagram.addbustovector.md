---
> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [ww2.mathworks.cn](https://ww2.mathworks.cn/help/simulink/slref/simulink.blockdiagram.addbustovector.html)
> This MATLAB function searches a model, excluding any library blocks, for bus signals used implicitly ......
---
Convert virtual bus signals into vector signals by adding Bus to Vector blocks

## Description

[example](https://ww2.mathworks.cn/help/simulink/slref/simulink.blockdiagram.addbustovector.html#mw_06434ccf-941a-46f7-8251-b6d58768b12e)

``[[`destBlocks`](#bvh0gqa-1-destBlocks),[`busToVectorBlocks`](#bvh0gqa-1-busToVectorBlocks),[`ignoredBlocks`](#bvh0gqa-1-ignoredBlocks)] = Simulink.BlockDiagram.addBusToVector([`model`](#bvh0gqa-1-model),[`includeLibs`](#bvh0gqa-1-includeLibs),[`reportOnly`](#bvh0gqa-1-reportOnly))`` searches a model, and if `reportOnly` is set to `false`, then the function inserts a [Bus to Vector](https://ww2.mathworks.cn/help/simulink/slref/bustovector.html) block into each bus that is used as a vector in any block that it searches. The insertion replaces the implicit use of a bus as a vector with an explicit conversion of the bus to a vector. The source and destination blocks of the signal do not change.

If `Simulink.BlockDiagram.addBusToVector` adds Bus to Vector blocks to the model or any library, the function changes the saved copy of the diagram.

If `Simulink.BlockDiagram.addBusToVector` changes a library block, the change affects every instance of that block in every model that uses the library.

``[[`destBlocks`](#bvh0gqa-1-destBlocks),[`busToVectorBlocks`](#bvh0gqa-1-busToVectorBlocks),[`ignoredBlocks`](#bvh0gqa-1-ignoredBlocks)] = Simulink.BlockDiagram.addBusToVector([`model`](#bvh0gqa-1-model),[`includeLibs`](#bvh0gqa-1-includeLibs),[`reportOnly`](#bvh0gqa-1-reportOnly),[`strictOnly`](#bvh0gqa-1-strictOnly))`` searches a model, and if `strictOnly` is `true`, the function checks for input bus signals used implicitly as vectors that are fed into one of these blocks. These blocks cannot take virtual bus signals, but they can accept nonvirtual bus signals:

- Delay
- Selector
- Assignment
- Vector Concatenate
- Reshape
- Permute Dimensions

## Examples

collapse all

### Identify Bus-To-Vector Conversions

Model `ex_bus_to_vector` simulates correctly, but the input to the Gain block is a bus, while the output is a vector. The Gain block implicitly converts the bus to a vector.

Open the model.

```
openExample('simulink/ConvertBusSignalToAVectorExample',...
'supportingFile','ex_bus_to_vector.slx')

```

![](https://ww2.mathworks.cn/help/simulink/slref/bus_to_vector_before.png)

Identify buses treated as vectors.

```
[blocks] = Simulink.BlockDiagram.addBusToVector(...
'ex_bus_to_vector')
```

```
### Processing block diagram 'ex_bus_to_vector'
### Number of blocks left that are connected to a bus being used as a vector: 2
### Done processing block diagram 'ex_bus_to_vector'

blocks =

  1×2 struct array with fields:

    BlockPath
    InputPort
    LibPath
```

To understand the relationship between `Simulink.BlockDiagram.addBusToVector` and the **Bus signal treated as vector** configuration parameter, see [Manage Bus-to-Vector Conversions](https://ww2.mathworks.cn/help/simulink/slref/convert-bus-signal-to-a-vector.html).

Model `ex_bus_to_vector` simulates correctly, but the input to the Gain block is a bus, while the output is a vector. The Gain block implicitly converts the bus to a vector.

Open the model.

```
openExample('simulink/ConvertBusSignalToAVectorExample',...
'supportingFile','ex_bus_to_vector.slx')
```

![](https://ww2.mathworks.cn/help/simulink/slref/bus_to_vector_before.png)

Insert Bus to Vector blocks.

When you use function `Simulink.BlockDiagram.addBusToVector` with `reportOnly` set to `false`, the function saves the model. To create a writable copy of model `ex_bus_to_vector`, this example uses the `save_system` function.

```
save_system('ex_bus_to_vector','ex_bus_to_vector_blocks');
[blocks,busToVectors] = Simulink.BlockDiagram.addBusToVector(...
'ex_bus_to_vector_blocks',true,false);
```

![](https://ww2.mathworks.cn/help/simulink/slref/bus_to_vector_inserted.png)

The Gain block no longer implicitly converts the bus to a vector. The inserted Bus to Vector block performs the conversion explicitly. The Bus to Vector block is virtual and does not affect simulation results, code generation, or performance.

To understand the relationship between `Simulink.BlockDiagram.addBusToVector` and the **Bus signal treated as vector** configuration parameter, see [Manage Bus-to-Vector Conversions](https://ww2.mathworks.cn/help/simulink/slref/convert-bus-signal-to-a-vector.html).

## Input Arguments

collapse all

### `model` — Model name or handle

character vector | string scalar | numeric scalar

Model name or handle, specified as a character vector, string scalar, or numeric scalar.

**Data Types:** `double` | `char` | `string`

### `includeLibs` — Search library blocks

`false` (default) | `true`

Search library blocks, specified as `false` or `true`.

- `false` — Search only the blocks in the model.
- `true` — Search library blocks for bus signals used implicitly as vectors.

Specify as the second argument.

**Data Types:** `logical`

### `reportOnly` — Report results without changing model

`true` (default) | `false`

Choice to report results without changing the model, specified as `false` or `true`.

- `false` — Update the model by inserting Bus to Vector blocks for bus signals that are implicitly used as vectors.
- `true` — Report search results, but do not change the model.

Specify as the third argument. Also specify the [`model`](#bvh0gqa-1-model) and [`includeLibs`](#bvh0gqa-1-includeLibs) arguments.

**Data Types:** `logical`

### `strictOnly` — Check input bus signals used implicitly as vectors that feed blocks that can accept nonvirtual, but not virtual, bus signals

`false` (default) | `true`

Check input bus signals used implicitly as vectors that feed blocks that can accept nonvirtual, but not virtual, bus signals, specified as `false` or `true`. If `strictOnly` is `true`, the function checks for input bus signals used implicitly as vectors that are fed into one of these blocks. These blocks cannot take virtual bus signals, but they can accept nonvirtual bus signals.

- Delay
- Selector
- Assignment
- Vector Concatenate
- Reshape
- Permute Dimensions

Specify as the fourth argument. You must also specify the [`model`](#bvh0gqa-1-model), [`includeLibs`](#bvh0gqa-1-includeLibs), and [`reportOnly`](#bvh0gqa-1-reportOnly) arguments.

**Data Types:** `logical`

## Output Arguments

collapse all

### `destBlocks` — Blocks connected to buses but that treat buses as vectors

array of structures

Blocks connected to buses that treat buses as vectors, returned as an array of structures. Each structure in the array contains these fields:

- `BlockPath` — Character vector specifying the path to the block to which the bus connects.
- `InputPort` — Integer specifying the input port to which the bus connects.
- `LibPath` — If the block is a library block instance, and if [`includeLibs`](#bvh0gqa-1-includeLibs) is `true`, the field value is the path to the source library block. Otherwise, `LibPath` is empty (`[]`).

### `busToVectorBlocks` — Bus to Vector blocks added by function

cell array

Bus to Vector blocks added by function, specified as a cell array. If [`reportOnly`](#bvh0gqa-1-reportOnly) is set to `false`, the cell array contains the paths to each Bus to Vector block that the function added to replace buses used as vectors. Otherwise, `busToVectorBlocks` is empty (`[]`).

### `ignoredBlocks` — Cases where function cannot insert Bus to Vector block

array of structures

Cases where function cannot insert Bus to Vector block, specified as an array of structures. Each structure in the array contains these fields:

- `BlockPath` — Character vector specifying the path to the block to which the bus connects.
- `InputPort` — Integer specifying the input port to which the bus connects.

These cases occur when a Bus to Vector cannot be inserted because the input virtual bus signal consists of elements with mixed attributes.

## Tips

- Before you execute this function:

  1. Ensure that the model compiles without error.
  2. Save the model.
- Back up the model and any libraries before calling the function with [`reportOnly`](#bvh0gqa-1-reportOnly) set to `false`.
- To preview the effects of the change on blocks in all models, call `Simulink.BlockDiagram.addBusToVector` with [`includeLibs`](#bvh0gqa-1-includeLibs) set to `true` and `reportOnly` set to `true`. Then, examine the information returned in the [`destBlocks`](#bvh0gqa-1-destBlocks) output argument.

## Version History

**Introduced in R2007a**

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

Select a Web Site

Choose a web site to get translated content where available and see local events and offers. Based on your location, we recommend that you select: .

You can also select a web site from the following list:

[Contact your local office](#)
