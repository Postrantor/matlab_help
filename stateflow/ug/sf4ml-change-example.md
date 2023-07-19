---
tip: translate by openai@2023-06-21 15:15:08
...
---
> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [ww2.mathworks.cn](https://ww2.mathworks.cn/help/stateflow/ug/sf4ml-change-example.html)

> Create a MATLAB script or a Simulink model that invokes a standalone Stateflow chart.

---

## Execute Stateflow Chart Objects Through Scripts and Models

A standalone Stateflow® chart is a MATLAB® class that defines the behavior of a finite state machine. Standalone charts implement classic chart semantics with MATLAB as the action language. You can program the chart by using the full functionality of MATLAB, including those functions that are restricted for code generation in Simulink®. For more information, see [Create Stateflow Charts for Execution as MATLAB Objects](https://ww2.mathworks.cn/help/stateflow/ug/create-stateflow-chart-objects.html).

> 一个独立的 Stateflow® 图是一个 MATLAB® 类，定义了有限状态机的行为。独立图表使用 MATLAB 作为动作语言实现经典图表语义。您可以使用 MATLAB 的完整功能来编程图表，包括在 Simulink® 中为代码生成而限制的功能。有关更多信息，请参见[创建作为 MATLAB 对象执行的 Stateflow 图](https://ww2.mathworks.cn/help/stateflow/ug/create-stateflow-chart-objects.html)。

This example shows how to execute a standalone Stateflow chart by using a MATLAB script or a Simulink model.

> 这个例子展示了如何通过使用 MATLAB 脚本或 Simulink 模型来执行独立的 Stateflow 图。

### Count Ways to Make Change for Currency

The file `sf_change.sfx` defines a standalone Stateflow chart that counts the number of ways to make change for a given amount of money. The chart contains these data objects:

> 文件 `sf_change.sfx` 定义了一个独立的 Stateflow 图，用于计算给定金额的零钱变换方式的数量。该图包含以下数据对象：

- `x` is the amount of money to change. The default value is 100.
- `coinValues` is a vector of coin denominations arranged in increasing order. `coinNames` is an array of corresponding coin names. The default values represent standard American coins (pennies, nickels, dimes, and quarters).

> `coinValues` 是一个按递增顺序排列的硬币面值向量。`coinNames` 是相应硬币名称的数组。默认值代表美国标准硬币（便士、五分币、一角币和二角币）。

- `tally` is the number of valid change configurations.
- `tabula` is an array containing the different valid change configurations.
- `chg`, `done`, `i`, and `n` are local data used by the change-counting algorithm.
- `textWidth` and `quietMode` are local data that control how the chart displays its results.

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/CountWaysToMakeChangeChartObjectExample_01.png)

The chart begins with a potential change configuration consisting entirely of the lowest-value coins, specified by an index of 1. At each execution step, the state `exchange` modifies this configuration in one of two ways:

> 图表以一个由最低价值硬币组成的潜在更改配置开始，由索引 1 指定。在每个执行步骤中，状态 `exchange` 以两种方式之一修改此配置：

- The substate `move_up` exchanges some lowest-value coins for a coin with a higher value, specified by the index `i`.

> `move_up` 子状态通过索引 `i` 交换一些最低价值的硬币，以换取价值更高的硬币。

- The substate `move_down` exchanges all of the coins with the value specified by the index `i` for lowest-value coins. Then `move_up` exchanges some lowest-value coins for a coin with a value specified by the index `i+1` or higher.

> - move_down 状态会把索引 i 所指定的价值的所有硬币兑换成价值最低的硬币。然后，move_up 会把一些价值最低的硬币兑换成索引 i+1 或更高价值的硬币。

A potential change configuration is valid when the number of cents represented by the lowest-value coins is divisible by the value of that type of coin. When the chart encounters a new valid configuration, it increments `tally` and appends the new configuration to `tabula`.

> 一个潜在的更改配置有效，当由最低值硬币表示的美分可以被该类型硬币的价值整除时。当图表遇到一个新的有效配置时，它会增加 `tally`，并将新配置附加到 `tabula`。

When no more coin exchanges are possible, the state `stop` becomes active. This state prints the results of the computation, transforms the contents of `tabula` to a table, and sets the value of `done` to `true`.

> 当不再可能进行硬币交换时，状态 `stop` 将变为活动状态。此状态会打印计算结果，将 `tabula` 的内容转换为表格，并将 `done` 的值设置为 `true`。

### Execute Standalone Chart in a MATLAB Script

To run the change-counting algorithm to completion, you must execute the standalone chart multiple times. For example, the MATLAB script `sf_change_script.m` creates a chart object `chartObj` and initializes the value of the local data `x` to 27. The configuration option `'-warningOnUninitializedData'`, which the script sets to `false`, eliminates the warning that `tabula` is an empty array in the new chart object. The `while` loop executes the chart until the local data `done` becomes `true`. Finally, the script displays the value of `tabula`.

> 要完成变化计数算法，您必须多次执行独立图表。例如，MATLAB 脚本 `sf_change_script.m` 创建图表对象 `chartObj` 并将本地数据 `x` 的值初始化为 27。脚本将配置选项 `'-warningOnUninitializedData'` 设置为 `false`，以消除新图表对象中 `tabula` 是空数组的警告。 `while` 循环一直执行图表，直到本地数据 `done` 变为 `true`。最后，脚本显示 `tabula` 的值。

```
chartObj = sf_change('-warningOnUninitializedData',false,x=27);

while ~chartObj.done
    step(chartObj);
end

disp(chartObj.tabula)
```

When you run this script, the standalone chart calculates the number of ways to make change for 27 cents by using standard American coins:

> 当你运行这个脚本时，独立图表将计算使用标准美国硬币为 27 美分找零的方法数量。

```
.............
There are 13 ways to make change for 27 cents.
    Pennies    Nickels    Dimes    Quarters
    _______    _______    _____    ________

      27          0         0         0
      22          1         0         0
      17          2         0         0
      12          3         0         0
       7          4         0         0
       2          5         0         0
      17          0         1         0
      12          1         1         0
       7          2         1         0
       2          3         1         0
       7          0         2         0
       2          1         2         0
       2          0         0         1
```

To determine the number of ways to make change for a different amount, or to use a different system of currency, change the values of `x` and `coinValues`. For example, to use British currency, initialize `coinValues` to `[1 2 5 10 20 25 50]`.

> 要确定换取不同金额的方法数量，或使用不同的货币系统，请更改 `x` 和 `coinValues` 的值。例如，使用英国货币，将 `coinValues` 初始化为 `[1 2 5 10 20 25 50]`。

### Execute Standalone Chart in a Simulink Model

You can execute a standalone Stateflow chart from within a Simulink model. For example, the model `sf_change_model` contains two Stateflow charts that use the standalone chart `sf_change` to count the number of ways to make change for 27 cents in two different currency systems. You can simulate the model, but the functions that execute the standalone chart do not support code generation.

> 你可以在 Simulink 模型中运行独立的 Stateflow 图表。例如，模型 `sf_change_model` 包含两个 Stateflow 图表，它们使用独立图表 `sf_change` 来计算 27 美分在两种不同货币系统中可以换取的方式数量。你可以模拟该模型，但是执行独立图表的函数不支持代码生成。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/ExecuteChartObjectInSimulinkExample_01.png)

Each chart contains these states:

> 每张图表包含这些状态：

- `Initialize` creates a local chart object `chartObj` that implements the change-counting algorithm for the input value `x`.

> 初始化创建一个本地图表对象 chartObj，它实现了对输入值 x 的变化计数算法。

- `Execute` calls the `step` function to execute the standalone chart and stores the result as the output data `tally`.

> 执行调用步骤函数来执行独立图表，并将结果存储为输出数据 `tally`。

- `Finish` displays the results of the algorithm in the Diagnostic Viewer window and sets the value of the output data `done` to `true`.

> 完成后，将算法结果显示在诊断查看器窗口中，并将输出数据的值设置为 `true`。

When both charts reach their respective `Finish` state, the simulation of the model stops and the Display blocks show the final values of the two tallies.

> 当两张图表都达到其各自的“完成”状态时，模型的模拟就会停止，显示块会显示两个计数器的最终值。

**Execution Using MATLAB as the Action Language**

> 使用 MATLAB 作为动作语言执行

The chart `MATLAB syntax` uses MATLAB as the action language. To execute the standalone Stateflow chart, this chart must follow these guidelines:

> 图表 `MATLAB语法` 使用 MATLAB 作为动作语言。要执行独立的 Stateflow 图表，此图表必须遵循以下准则：

- The local variable `chartObj` that contains the handle to the chart object has type `Inherit: From definition in chart`.

> 本地变量 `chartObj` 包含指向图表对象的句柄，其类型为“继承：根据图表中的定义”。

- The `Execute` and `Finish` states access the local data for the standalone chart by calling the `get` function.

> `执行` 和 `完成` 状态通过调用 `get` 函数访问独立图表的本地数据。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/ExecuteChartObjectInSimulinkExample_02.png)

When you simulate this chart with an input of `x` = 27, the Display block `Olde English` shows a tally of 4. The Diagnostic Viewer window shows these results:

> 当您以 `x`=27 为输入模拟此图表时，显示块 `Olde English` 显示计数为 4。诊断查看器窗口显示以下结果：

```
Pennies    Shillings    Florins
   _______    _________    _______
     27           0           0
     15           1           0
      3           2           0
      3           0           1
```

**Execution Using C as the Action Language**

> 使用 C 作为动作语言执行

The chart `C syntax` uses C as the action language. To execute the standalone Stateflow chart, this chart must follow these guidelines:

> 图表 `C语法` 使用 C 作为操作语言。要执行独立的 Stateflow 图表，必须遵循以下准则：

- The local variable `chartObj` that contains the handle to the chart object has type `ml`.
- The `Initialize` state calls the `ml` function to create the chart object.
- The `Execute` and `Finish` states use the `ml` namespace operator to access the `step`, `get`, and `displ` functions to execute the standalone chart, access its local data, and display the results of the algorithm.

> 执行和完成状态使用 `ml` 命名空间操作符访问 `step`、`get` 和 `displ` 函数来执行独立图表，访问其本地数据，并显示算法的结果。

For more information, see [Access MATLAB Functions and Workspace Data in C Charts](https://ww2.mathworks.cn/help/stateflow/ug/calling-built-in-matlab-functions-and-accessing-workspace-data.html).

> 对于更多信息，请参阅 [C 图表中的 MATLAB 函数和工作空间数据访问](https://ww2.mathworks.cn/help/stateflow/ug/calling-built-in-matlab-functions-and-accessing-workspace-data.html)。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/ExecuteChartObjectInSimulinkExample_03.png)

When you simulate this chart with an input of `x` = 27, the Display block `Modern American` shows a tally of 13. The Diagnostic Viewer window shows these results:

> 当您使用输入 `x`=27 模拟此图表时，显示块 `Modern American` 显示 13 个计数。诊断查看器窗口显示以下结果：

```
Safety    FieldGoal    TouchDown
   ______    _________    _________
     12          1            0
      9          3            0
      6          5            0
      3          7            0
      0          9            0
     10          0            1
      7          2            1
      4          4            1
      1          6            1
      5          1            2
      2          3            2
      3          0            3
      0          2            3
```

## Related Topics

- [Create Stateflow Charts for Execution as MATLAB Objects](https://ww2.mathworks.cn/help/stateflow/ug/create-stateflow-chart-objects.html)
- [Execute and Unit Test Stateflow Chart Objects](https://ww2.mathworks.cn/help/stateflow/ug/execute-chart-objects.html)
- [Call Extrinsic MATLAB Functions in Stateflow Charts](https://ww2.mathworks.cn/help/stateflow/ug/call-extrinsic-functions-in-a-stateflow-chart.html)
- [Access MATLAB Functions and Workspace Data in C Charts](https://ww2.mathworks.cn/help/stateflow/ug/calling-built-in-matlab-functions-and-accessing-workspace-data.html)
