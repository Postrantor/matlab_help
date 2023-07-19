---
tip: translate by openai@2023-06-21 15:37:31
> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [ww2.mathworks.cn](https://ww2.mathworks.cn/help/stateflow/ug/create-stateflow-chart-objects.html)
> Save standalone Stateflow charts outside of Simulink models.https://ww2.mathworks.cn/help/stateflow/ug/create-stateflow-chart-objects.html
---
## Create Stateflow Charts for Execution as MATLAB Objects

To combine the advantages of state machine programming with the full functionality of MATLAB®, create a standalone Stateflow® chart outside of a Simulink® model. Save the standalone chart with the extension `.sfx` and execute it as a MATLAB object. Refine your design by using chart animation and graphical debugging tools.

> 将状态机编程的优势与 MATLAB® 的全部功能结合在一起，在 Simulink® 模型之外创建一个独立的 Stateflow® 图表。将独立图表以 `.sfx` 扩展名保存，并将其作为 MATLAB 对象执行。通过使用图表动画和图形调试工具来完善您的设计。

With standalone charts, you can create MATLAB applications such as:

> 使用独立图表，您可以创建 MATLAB 应用程序，如：

These applications can be shared and executed without requiring a Stateflow license. For more information, see [Share Standalone Charts](https://ww2.mathworks.cn/help/stateflow/ug/create-stateflow-chart-objects.html#mw_4a31edbd-ef7a-4ff9-be39-471f3a245da5).

> 这些应用程序可以共享和执行，而不需要 Stateflow 许可证。有关更多信息，请参阅[共享独立图表](https://ww2.mathworks.cn/help/stateflow/ug/create-stateflow-chart-objects.html#mw_4a31edbd-ef7a-4ff9-be39-471f3a245da5)。

### Construct a Standalone Chart

To construct a standalone Stateflow chart, open the Stateflow Editor by using the [`edit`](https://ww2.mathworks.cn/help/matlab/ref/edit.html) function. For example, at the MATLAB Command Window, enter:

> 使用[`edit`](https://ww2.mathworks.cn/help/matlab/ref/edit.html)函数在 MATLAB 命令窗口中打开 Stateflow 编辑器来构建独立的 Stateflow 图，例如：输入：

If the file `chart.sfx` does not exist, the Stateflow Editor opens an empty chart with the name `chart`. Otherwise, the editor opens the chart defined by the `sfx` file.

> 如果文件 `chart.sfx` 不存在，Stateflow 编辑器将打开一个名为 `chart` 的空图表。否则，编辑器将打开由 `sfx` 文件定义的图表。

In the Stateflow Editor, create a standalone chart by combining states, transitions, data, and other elements. For more information, see [Construct and Run a Stateflow Chart](https://ww2.mathworks.cn/help/stateflow/gs/stateflow-charts.html).

> 在 Stateflow 编辑器中，通过组合状态、转换、数据和其他元素来创建独立图表。有关更多信息，请参阅[构建和运行 Stateflow 图表](https://ww2.mathworks.cn/help/stateflow/gs/stateflow-charts.html)。

After you save the standalone chart, the [`help`](https://ww2.mathworks.cn/help/matlab/ref/help.html) function displays information about executing it in MATLAB:

> 保存独立图表后，[`帮助`](https://ww2.mathworks.cn/help/matlab/ref/help.html)函数将显示有关在 MATLAB 中执行它的信息。

To close the standalone chart from the MATLAB Command Window, use the [`sfclose`](https://ww2.mathworks.cn/help/stateflow/ref/sfclose.html) function:

> 使用[`sfclose`](https://ww2.mathworks.cn/help/stateflow/ref/sfclose.html)函数可以从 MATLAB 命令窗口关闭独立图表：

### Create a Stateflow Chart Object

To execute a standalone chart in MATLAB, first create a Stateflow chart object. Use the name of the `sfx` file for the standalone chart as a function. Specify the initial values of data as name-value pairs. For example, suppose that you defined a standalone chart with data objects called `data1` and `data2`. Then this command creates the chart object `chartObject`, initializes `data1` and `data2`, and executes its default transition:

> 在 MATLAB 中执行独立图表，首先创建一个 Stateflow 图表对象。使用独立图表的 `sfx` 文件名作为函数。指定数据的初始值作为名称值对。例如，假设您定义了名为 `data1` 和 `data2` 的独立图表。然后，此命令创建图表对象 `chartObject`，初始化 `data1` 和 `data2`，并执行其默认转换：

```
chartObject = chart(data1=value1,data2=value2)
```

To display chart information, such as the syntax for execution, the values of the chart data, and the list of active states, use the [`disp`](https://ww2.mathworks.cn/help/matlab/ref/disp.html) function:

> 使用[`disp`](https://ww2.mathworks.cn/help/matlab/ref/disp.html)函数来显示图表信息，如执行语法，图表数据的值和活动状态的列表：

### Execute a Standalone Chart

After you define a Stateflow chart object, you can execute the standalone chart by calling the `step` function (with data values, if necessary):

> 在定义了 Stateflow 图表对象之后，您可以通过调用 `step` 函数（如果需要，可以带上数据值）来执行独立图表：

```
step(chartObject,data1=value1,data2=value2)
```

Alternatively, you can call one of the input event functions:

> 你也可以调用一个输入事件函数：

```
event_name(chartObject,data1=value1,data2=value2)
```

In either case, the values are assigned to local data before the chart executes.

> 无论哪种情况，在图表执行之前，这些值都会被分配给本地数据。

If your chart has graphical or MATLAB functions, you can call them directly in the MATLAB Command Window. Calling a chart function does not execute the standalone chart.

> 如果您的图表具有图形或 MATLAB 函数，您可以在 MATLAB 命令窗口中直接调用它们。调用图表函数不会执行独立图表。

```
function_name(chartObject,u1,u2)
```

**Note**

If you use [`nargin`](https://ww2.mathworks.cn/help/matlab/ref/nargin.html) in a graphical or MATLAB function in your chart, `nargin` counts the chart object as one of the input arguments. The value of `nargin` is the same whether you call the function from the chart or from the MATLAB Command Window.

> 如果在图形或 MATLAB 函数中使用 `nargin`，`nargin` 将图表对象视为输入参数之一。无论是从图表还是从 MATLAB 命令窗口调用函数，`nargin` 的值都是相同的。

You can execute a standalone chart without opening the Stateflow Editor. If the chart is open, then the Stateflow Editor highlights active states and transitions through chart animation.

> 你可以在不打开 Stateflow 编辑器的情况下执行独立图表。如果图表已打开，则 Stateflow 编辑器会通过图表动画突出显示活动状态和转换。

For the purposes of debugging and unit testing, you can execute a standalone chart directly from the Stateflow Editor. During execution, you enter data values and broadcast events from the user interface. For more information, see [Execute and Unit Test Stateflow Chart Objects](https://ww2.mathworks.cn/help/stateflow/ug/execute-chart-objects.html).

> 为了调试和单元测试，您可以直接从 Stateflow Editor 中执行独立图表。在执行过程中，您可以从用户界面输入数据值和广播事件。有关详细信息，请参阅[执行和单元测试 Stateflow 图表对象](https://ww2.mathworks.cn/help/stateflow/ug/execute-chart-objects.html)。

You can execute a standalone chart from a MATLAB script, a Simulink model, or an App Designer user interface. For more information, see:

> 您可以从 MATLAB 脚本、Simulink 模型或 App Designer 用户界面执行独立图表。有关更多信息，请参阅：

### Stop Chart Execution

To stop executing a chart, destroy the chart object by calling the [`delete`](https://ww2.mathworks.cn/help/matlab/ref/handle.delete.html) function:

> 调用[`delete`](https://ww2.mathworks.cn/help/matlab/ref/handle.delete.html)函数销毁图表对象，以停止执行图表：

After the chart object is deleted, any handles to the chart object remain in the workspace, but become invalid. To remove the invalid handle from the workspace, use the command [`clear`](https://ww2.mathworks.cn/help/matlab/ref/clear.html):

> 删除图表对象后，工作空间中仍然存在对图表对象的句柄，但是已经失效。要从工作空间中移除失效的句柄，请使用命令[`clear`](https://ww2.mathworks.cn/help/matlab/ref/clear.html)：

If you clear a valid chart object handle and there are other handles to the same chart object, the chart object is not destroyed. For example, when you are executing a chart, the Stateflow Editor contains internal handles to the chart object. Clearing the chart object handle from the workspace does not destroy the chart object or remove the chart animation highlighting in the editor. To reset the animation highlighting, right-click the chart canvas and select .

> 如果您清除了有效的图表对象句柄，并且还有其他句柄指向同一个图表对象，则图表对象不会被销毁。例如，当您正在执行图表时，Stateflow 编辑器内部包含指向图表对象的句柄。从工作区清除图表对象句柄不会销毁图表对象或删除编辑器中的图表动画高亮显示。要重置动画高亮显示，请右键单击图表画布，然后选择。

### Share Standalone Charts

You can share standalone charts with collaborators who do not have a Stateflow license.

> 你可以与没有 Stateflow 许可证的合作者共享独立图表。

If your collaborators have the same or a later version of MATLAB than you have, they can execute your standalone charts as MATLAB objects without opening the Stateflow Editor. Chart animation and debugging are not supported. Run-time error messages do not link to the state or transition in the chart where the error occurs.

> 如果你的合作者拥有与你相同或更新版本的 MATLAB，他们可以在不打开 Stateflow 编辑器的情况下将你的独立图表执行为 MATLAB 对象。不支持图表动画和调试。运行时错误消息不会链接到发生错误的图表中的状态或转换。

**Note**

To run standalone charts that you saved in R2019a or R2019b, your collaborators must have the same version of MATLAB.

> 要运行您在 R2019a 或 R2019b 中保存的独立图表，您的合作伙伴必须具有相同版本的 MATLAB。

If your collaborators have an earlier version of MATLAB, export a standalone chart to a format that they can use. You can only export to R2019a and later releases. To complete the export process, you need access to the versions of Stateflow from which and to which you are exporting.

> 如果您的合作者拥有早期版本的 MATLAB，请将独立图表导出到他们可以使用的格式。您只能导出到 R2019a 及更高版本。要完成导出过程，您需要访问您正在导出和导入的 Stateflow 版本。

1. Using the later version of Stateflow, open the standalone chart.

> 使用较新版本的 Stateflow 打开独立图表。

2. On the **State Chart** tab, select > .

> 在状态图表标签上，选择 > 。

3. In the Export to Previous Version dialog box, specify a file name for the exported chart.

> 在“导出到先前版本”对话框中，为导出的图表指定一个文件名。

4. From the **Save as type** list, select the earlier version to which to export the chart.

> 从"另存为类型"列表中，选择要导出图表的较早版本。

5. Click **Save**.
6. Using the earlier version of Stateflow, open and resave the exported chart.

> 使用较早版本的 Stateflow 打开并重新保存导出的图表。

To export a chart from the MATLAB Command Window, call the Stateflow function [`exportToVersion`](https://ww2.mathworks.cn/help/stateflow/ref/stateflow.exporttoversion.html). For more information, see [Export Chart to an Earlier Version of MATLAB](https://ww2.mathworks.cn/help/stateflow/ref/stateflow.exporttoversion.html#mw_2bcc4b83-9ae6-4df9-9848-0b4447c73d79).

> 从 MATLAB 命令窗口导出图表，请调用 Stateflow 函数[`exportToVersion`](https://ww2.mathworks.cn/help/stateflow/ref/stateflow.exporttoversion.html)。有关更多信息，请参见[将图表导出到较早版本的 MATLAB](https://ww2.mathworks.cn/help/stateflow/ref/stateflow.exporttoversion.html#mw_2bcc4b83-9ae6-4df9-9848-0b4447c73d79)。

**Note**

Attempting to execute an exported chart before resaving it will result in an error.

> 尝试在重新保存之前执行导出的图表会导致错误。

### Properties and Functions of Stateflow Chart Objects

A Stateflow chart object encapsulates data and operations in a single structure by providing:

> Stateflow 图形对象通过提供以下内容将数据和操作封装在单个结构中：

- Private properties that contain the internal state variables for the standalone chart.
- A `step` function that calls the various operations implementing the chart semantics.

A chart object can have other properties and functions that correspond to the various elements present in the chart.

> 图表对象可以具有与图表中存在的各个元素相对应的其他属性和函数。

<table><colgroup><col width="50%"><col width="50%"></colgroup><thead><tr><th>Standalone Chart Elements</th><th>Chart Object Elements</th></tr></thead><tbody><tr><td>Local and constant data</td><td>Public properties</td></tr><tr><td>Input events</td><td>Functions that execute the chart</td></tr><tr><td>Graphical and MATLAB functions</td><td>Functions that you can call from the MATLAB Command Window</td></tr></tbody></table>

#### Chart Object Configuration Options

When you create a chart object, you can specify chart behavior by including these configuration options as name-value pairs.

> 当你创建一个图表对象时，你可以通过将这些配置选项作为名称-值对来指定图表行为。

<table><colgroup><col width="20%"><col width="40%"><col width="40%"></colgroup><thead><tr><th>Configuration Option</th><th>Description</th><th>Example</th></tr></thead><tbody><tr><td><code>-animationDelay</code></td><td>Specify the delay that the chart animation uses to highlight each transition segment. The default value is <code>0.01</code> seconds. To produce a chart with no animation delays, set to zero.</td><td><p>Create a chart object that has slow animation by specifying one-second delays.</p><pre class="hljs ini">chartObject = chart('-animationDelay',1)</pre></td></tr><tr><td><code>-enableAnimation</code></td><td>Enable chart animation and debugging instrumentation. The default value is <code>true</code>.</td><td><p>Create a chart object that has animation and debugging instrumentation disabled.</p><pre class="hljs ini">chartObject = chart('-enableAnimation',false)</pre></td></tr><tr><td><code>-eventQueueSize</code></td><td>Specify the size of the queue used for events and temporal logic operations. The default value is <code>20</code>. To disable the queuing of events, set to zero. For more information, see <a href="https://ww2.mathworks.cn/help/stateflow/ug/how-events-drive-chart-execution.html#mw_5eb311dc-c2d5-43c3-a199-9a5174bded16">Events in Standalone Charts</a>.</td><td><p>Create a chart object that ignores all events without warning if they occur when the chart is processing another operation.</p><pre class="hljs ini">chartObject = chart('-eventQueueSize',0)</pre></td></tr><tr><td><code>-executeInitStep</code></td><td>Enable the initial execution of default transitions. The default value is <code>true</code>.</td><td><p>Create a chart object but do not execute the default transition.</p><pre class="hljs ini">chartObject = chart('-executeInitStep',false)</pre></td></tr><tr><td><code>-warningOnUninitializedData</code></td><td>Enable the warning about empty chart data after initializing the chart object. The default value is <code>true</code>.</td><td><p>Eliminate the warning when creating a chart object.</p><pre class="hljs ini">chartObject = chart('-warningOnUninitializedData',false)</pre></td></tr></tbody></table>

#### Initialization of Chart Data

In the Stateflow Editor, you can use the **Symbols** pane to specify initial values for chart data. When you create a chart object, chart data is initialized in alphabetical order according to its scope. Constant data is initialized first. Local data is initialized last.

> 在 Stateflow 编辑器中，您可以使用**符号**窗格指定图表数据的初始值。创建图表对象时，将按其作用域的字母顺序对图表数据进行初始化。首先初始化常量数据。最后初始化本地数据。

If you use an expression to specify an initial value, then the chart attempts to resolve the expression by:

> 如果您使用表达式来指定初始值，那么图表会尝试通过以下方式解析表达式：

- Using the values of other data in the chart.
- Calling functions on the search path.

For example, suppose that you specify an initial value for the local data `x` by using the expression `y`. Then:

> 例如，假设您使用表达式 `y` 为本地数据 `x` 指定初始值。那么：

- If the chart has a constant called `y`, `y` is initialized before `x`. The local data `x` is assigned the same initial value as `y`.

> 如果图表有一个常量叫做 `y`，`y` 在 `x` 之前被初始化。本地数据 `x` 被赋予与 `y` 相同的初始值。

- If the chart has a local data called `y`, `x` is initialized before `y`. The local data `x` is assigned to an empty array. If the configuration option `-warningOnUninitializedData` is set to `true`, a warning occurs.

> 如果图表有一个本地数据叫做 `y`，`x` 在 `y` 之前被初始化。本地数据 `x` 被分配给一个空数组。如果配置选项 `-warningOnUninitializedData` 设置为 `true`，则会发生警告。

- If the chart has no data named `y`, `x` is initialized by calling the function `y`. If the file `y.m` is not on the search path, this error occurs:

> 如果图表没有叫做'y'的数据，就会通过调用函数'y'来初始化'x'。如果搜索路径上没有'y.m'文件，就会出现这个错误。

```
Undefined function or variable 'y'.
```

Stateflow does not search the MATLAB workspace to resolve initial values, so this error occurs even if there is a variable called `y` in the MATLAB workspace.

> Stateflow 不会搜索 MATLAB 工作空间来解析初始值，因此即使 MATLAB 工作空间中存在一个叫做 `y` 的变量，也会发生这个错误。

### Capabilities and Limitations

#### Supported Features

- Classic chart semantics with MATLAB as the action language. You can use the full functionality of MATLAB, including those functions that are restricted for code generation in Simulink. See [Execute Stateflow Chart Objects Through Scripts and Models](https://ww2.mathworks.cn/help/stateflow/ug/sf4ml-change-example.html).

> 使用 MATLAB 作为操作语言的经典图表语义。您可以使用 MATLAB 的全部功能，包括在 Simulink 中限制用于代码生成的功能。请参见[通过脚本和模型执行 Stateflow 图表对象](https://ww2.mathworks.cn/help/stateflow/ug/sf4ml-change-example.html)。

**Note**

In standalone Stateflow charts, the operating system command symbol `!` is not supported. To execute operating system commands, use the function [`system`](https://ww2.mathworks.cn/help/matlab/ref/system.html).

> 在独立的 Stateflow 图中，不支持操作系统命令符号 `！`。要执行操作系统命令，请使用函数[`system`](https://ww2.mathworks.cn/help/matlab/ref/system.html)。

- Exclusive (OR) and Parallel (AND) state decomposition with hierarchy. See [Define Exclusive and Parallel Modes by Using State Decomposition](https://ww2.mathworks.cn/help/stateflow/ug/state-decomposition.html) and [Use State Hierarchy to Design Multilevel State Complexity](https://ww2.mathworks.cn/help/stateflow/ug/state-hierarchy.html).

> 独占（或）和并行（与）状态分解与层次结构。参见[使用状态分解定义独占和并行模式](https://ww2.mathworks.cn/help/stateflow/ug/state-decomposition.html)和[使用状态层次结构设计多级状态复杂性](https://ww2.mathworks.cn/help/stateflow/ug/state-hierarchy.html)。

- Flow charts, graphical functions, and MATLAB functions. See [Reusable Components in Charts](https://ww2.mathworks.cn/help/stateflow/reusable-components-in-charts.html).

> 流程图、图形函数和 MATLAB 函数。参见[图表中的可重用组件](https://ww2.mathworks.cn/help/stateflow/reusable-components-in-charts.html)。

- Conversion of MATLAB code to graphical functions by using the Pattern Wizard. See [Convert MATLAB Code into Stateflow Flow Charts](https://ww2.mathworks.cn/help/stateflow/ug/convert-MATLAB-to-flow-chart.html).

> 使用模式向导将 MATLAB 代码转换为图形函数。请参阅[将 MATLAB 代码转换为 Stateflow 流程图](https://ww2.mathworks.cn/help/stateflow/ug/convert-MATLAB-to-flow-chart.html)。

- Chart local and constant data without restriction to type. See [Execute and Unit Test Stateflow Chart Objects](https://ww2.mathworks.cn/help/stateflow/ug/execute-chart-objects.html).

> 在没有限制类型的情况下图表化本地和常量数据。请参阅[执行和测试 Stateflow 图表对象](https://ww2.mathworks.cn/help/stateflow/ug/execute-chart-objects.html)。

- Input events. See [Design Human-Machine Interface Logic by Using Stateflow Charts](https://ww2.mathworks.cn/help/stateflow/ug/sf4ml-lamp-example.html).

> 输入事件。参见[使用 Stateflow 图设计人机界面逻辑](https://ww2.mathworks.cn/help/stateflow/ug/sf4ml-lamp-example.html)。

- Operators [`hasChanged`](https://ww2.mathworks.cn/help/stateflow/ref/haschanged.html), [`hasChangedFrom`](https://ww2.mathworks.cn/help/stateflow/ref/haschangedfrom.html), and [`hasChangedTo`](https://ww2.mathworks.cn/help/stateflow/ref/haschangedto.html) that detect changes in the values of local data.

> - 操作符[`hasChanged`](https://ww2.mathworks.cn/help/stateflow/ref/haschanged.html)、[`hasChangedFrom`](https://ww2.mathworks.cn/help/stateflow/ref/haschangedfrom.html)和[`hasChangedTo`](https://ww2.mathworks.cn/help/stateflow/ref/haschangedto.html)可检测局部数据值的变化。

**Note**

Standalone Stateflow charts do not support change detection on an element of a matrix or a field in a structure.

> 独立状态流图不支持对矩阵元素或结构中的字段进行变化检测。

- Temporal logic operators:

  - [`after`](https://ww2.mathworks.cn/help/stateflow/ref/after.html), [`at`](https://ww2.mathworks.cn/help/stateflow/ref/at.html), and [`every`](https://ww2.mathworks.cn/help/stateflow/ref/every.html) operate on the number of input events, chart invocations (`tick`), and absolute time (`sec`). Use these operators in state `on` actions and as transition triggers.

> - [`after`](https://ww2.mathworks.cn/help/stateflow/ref/after.html)、[`at`](https://ww2.mathworks.cn/help/stateflow/ref/at.html)和[`every`](https://ww2.mathworks.cn/help/stateflow/ref/every.html)都是基于输入事件的数量、图表调用（`tick`）和绝对时间（`sec`）操作的。在状态 `on` 动作和转换触发器中使用这些操作符。

- [`count`](https://ww2.mathworks.cn/help/stateflow/ref/count.html) operates on the number of chart invocations (`tick`).

> - [`count`](https://ww2.mathworks.cn/help/stateflow/ref/count.html) 对图表调用（`tick`）的次数进行操作。

- [`temporalCount`](https://ww2.mathworks.cn/help/stateflow/ref/temporalcount.html) operates on absolute time (`sec`, `msec`, and `usec`).

> `temporalCount` 操作绝对时间（`sec`、`msec` 和 `usec`）。

- [`elapsed`](https://ww2.mathworks.cn/help/stateflow/ref/elapsed.html) operates on absolute time (`sec`).

> `elapsed` 操作绝对时间（`秒`）。

Standalone charts define absolute-time temporal logic in terms of wall-clock time, which is limited to 1 millisecond precision.

> 独立图表使用绝对时间的时间逻辑定义，其精度仅限于 1 毫秒。

- Function `getActiveStates` to access the states that are active during execution of the chart. To store the active states as a cell array, enter:

> 使用函数 `getActiveStates` 访问在图表执行期间处于活动状态的状态。要将活动状态存储为单元数组，请输入：

```
states = getActiveStates(chartObject)
```

- Stateflow function [`exportAsClass`](https://ww2.mathworks.cn/help/stateflow/ref/stateflow.exportasclass.html) that exports the standalone chart as the equivalent MATLAB class. Use this function to debug run-time errors that are otherwise difficult to diagnose. For example, suppose that you encounter an error while executing a Stateflow chart that controls a MATLAB application. If you export the chart as a MATLAB class file, you can replace the chart with the class in your application and diagnose the error by using the MATLAB debugger. To export the chart `chart.sfx` as a class file `chart.m`, enter:

> 使用 Stateflow 函数[`exportAsClass`](https://ww2.mathworks.cn/help/stateflow/ref/stateflow.exportasclass.html)将独立图表导出为等效的 MATLAB 类。使用此函数可以调试其他难以诊断的运行时错误。例如，假设您在执行控制 MATLAB 应用程序的 Stateflow 图时遇到错误。如果将图表导出为 MATLAB 类文件，则可以在应用程序中用类文件替换图表，并通过 MATLAB 调试器诊断错误。要将图表 `chart.sfx` 导出为类文件 `chart.m`，请输入：

```
Stateflow.exportAsClass("chart.sfx")
```

When you execute the MATLAB class, the Stateflow Editor does not animate the original chart.

> 当您执行 MATLAB 类时，Stateflow 编辑器不会对原始图表进行动画处理。

#### Limitations

Content specific to Simulink:

> 内容特定于 Simulink：

- Sample time and continuous-time semantics.
- C action language.
- Simulink functions and Simulink subsystems as states.
- Input, output, and parameter data.
- Data store memory data.
- Output and local events.
- Input, output, and local messages.

Other limitations:

- No Mealy or Moore semantics.
- No State Transition Tables.
- No Truth Table functions.
- No state-parented local data or functions.
- No transition actions (actions that execute after the source state for the transition is exited but before the destination state is entered).

> 没有过渡动作（在退出源状态后执行，但在进入目标状态之前执行的动作）。

## See Also

[`disp`](https://ww2.mathworks.cn/help/matlab/ref/disp.html) | [`edit`](https://ww2.mathworks.cn/help/matlab/ref/edit.html) | [`exportAsClass`](https://ww2.mathworks.cn/help/stateflow/ref/stateflow.exportasclass.html) | [`exportToVersion`](https://ww2.mathworks.cn/help/stateflow/ref/stateflow.exporttoversion.html) | [`help`](https://ww2.mathworks.cn/help/matlab/ref/help.html) | [`sfclose`](https://ww2.mathworks.cn/help/stateflow/ref/sfclose.html)

> [`disp`](https://ww2.mathworks.cn/help/matlab/ref/disp.html) | [`edit`](https://ww2.mathworks.cn/help/matlab/ref/edit.html) | [`exportAsClass`](https://ww2.mathworks.cn/help/stateflow/ref/stateflow.exportasclass.html) | [`exportToVersion`](https://ww2.mathworks.cn/help/stateflow/ref/stateflow.exporttoversion.html) | [`help`](https://ww2.mathworks.cn/help/matlab/ref/help.html) | [`sfclose`](https://ww2.mathworks.cn/help/stateflow/ref/sfclose.html)

显示 | 编辑 | 导出为类 | 导出到版本 | 帮助 | 关闭状态流

## Related Topics

- [Execute and Unit Test Stateflow Chart Objects](https://ww2.mathworks.cn/help/stateflow/ug/execute-chart-objects.html)
- [Execute Stateflow Chart Objects Through Scripts and Models](https://ww2.mathworks.cn/help/stateflow/ug/sf4ml-change-example.html)
- [Design Human-Machine Interface Logic by Using Stateflow Charts](https://ww2.mathworks.cn/help/stateflow/ug/sf4ml-lamp-example.html)
- [Implement a Financial Strategy by Using Stateflow](https://ww2.mathworks.cn/help/stateflow/ug/sf4ml-stockticker-example.html)
- [Model a Communications Protocol by Using Chart Objects](https://ww2.mathworks.cn/help/stateflow/ug/sf4ml-comm-example.html)
