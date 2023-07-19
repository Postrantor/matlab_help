---
tip: translate by openai@2023-07-18 09:31:58
url: https://ww2.mathworks.cn/help/stateflow/ug/tutorial-using-fixed-point-parameters-and-local-data.html
title: Build a Low-Pass Filter by Using Fixed-Point Data - MATLAB & Simulink - MathWorks 中国
date: 2023-07-18 09:30:40
tag:
summary: Model a low-pass Butterworth filter that uses fixed-point arithmetic.
> 模拟低通布特沃斯滤波器，使用定点算法。
---

Main Content

This example shows how to build a Stateflow® chart that uses fixed-point data to implement a low-pass Butterworth filter. By designing the filter with fixed-point data instead of floating-point data, you can simulate your model using less memory. For more information, see [Fixed-Point Data in Stateflow Charts](https://ww2.mathworks.cn/help/stateflow/ug/what-is-fixed-point-data.html).

> 这个例子展示了如何构建一个使用定点数据实现低通 Butterworth 滤波器的 Stateflow® 图表。 通过使用定点数据而不是浮点数据设计滤波器，您可以使用更少的内存来模拟模型。 有关更多信息，请参见[Stateflow 图表中的定点数据](https://ww2.mathworks.cn/help/stateflow/ug/what-is-fixed-point-data.html)。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/LowPassButterworthFilterExample_01.png)

### Build the Fixed-Point Butterworth Filter

The Low-Pass Filter chart is a stateless flow chart that accepts one input and provides one output. The chart contains these data symbols:

> 低通滤波器图是一个无状态流程图，它接受一个输入并提供一个输出。该图包含以下数据符号：

- `x` — **Scope**: `Input`, **Type**: `Inherit:Same as Simulink`
- `y` — **Scope**: `Output`, **Type**: `fixdt(1,16,10)`
- `x_n1` — **Scope**: `Local`, **Type**: `fixdt(1,16,12)`
- `y_n1` — **Scope**: `Local`, **Type**: `fixdt(1,16,10)`
- `b0` — **Scope**: `Parameter`, **Type**: `fixdt(1,16,15)`
- `b1` — **Scope**: `Parameter`, **Type**: `fixdt(1,16,15)`
- `a1` — **Scope**: `Parameter`, **Type**: `fixdt(1,16,15)`

The values of `b0`, `b1`, and `a1` are the coefficients of the low-pass Butterworth filter.

> b0、b1 和 a1 的值是低通布特沃斯滤波器的系数。

To build the Low-Pass Filter chart:

1.  Create a Simulink® model with an empty Stateflow chart by entering [`sfnew`](https://ww2.mathworks.cn/help/stateflow/ref/sfnew.html) at the MATLAB® command prompt.

> 在 MATLAB® 命令提示符中输入[`sfnew`](https://ww2.mathworks.cn/help/stateflow/ref/sfnew.html)，创建一个带有空状态流图的 Simulink® 模型。

2.  In the Stateflow chart, add a flow chart with a single branch that assigns values to `y`, `x_n1`, and `y_n1`.

> 在 Stateflow 图中，添加一个具有单个分支的流程图，用于将值分配给`y`，`x_n1`和`y_n1`。

3.  Add input, output, local, and parameter data to the chart, as described in [Add Stateflow Data](https://ww2.mathworks.cn/help/stateflow/ug/adding-data.html).

> 在图表中添加输入、输出、局部和参数数据，如[添加 Stateflow 数据](https://ww2.mathworks.cn/help/stateflow/ug/adding-data.html)中所述。

### Define the Model Callback Function

Before loading the model, MATLAB calls the [`butter`](https://ww2.mathworks.cn/help/signal/ref/butter.html) (Signal Processing Toolbox) function to compute the values for the parameters `b0`, `b1`, and `a1`. The function constructs a first-order low-pass Butterworth filter with a normalized cutoff frequency of `(2*pi*Fc/(Fs/2))` radians per second, where:

> 在加载模型之前，MATLAB 会调用[`butter`](https://ww2.mathworks.cn/help/signal/ref/butter.html)（信号处理工具箱）函数来计算参数`b0`、`b1`和`a1`的值。该函数构造一个一阶低通 Butterworth 滤波器，其归一化截止频率为`(2*pi*Fc/(Fs/2))`赫兹每秒，其中：

- The sampling frequency is `Fs` = 1000 Hz.
- The cutoff frequency is `Fc` = 50 Hz.

The function output `B` contains the numerator coefficients of the filter in descending powers of `z`. The function output `A` contains the denominator coefficients of the filter in descending powers of `z`.

> 函数输出`B`包含以`z`为降序的滤波器的分子系数。函数输出`A`包含以`z`为降序的滤波器的分母系数。

```
Fs = 1000;
Fc = 50;
[B,A] = butter(1,2*pi*Fc/(Fs/2));
b0 = B(1);
b1 = B(2);
a1 = A(2);


```

To define the preload callback for the model:

1.  In the **Modeling** tab, under **Setup**, select **Model Settings** > **Model Properties**.

> 在**建模**选项卡下，在**设置**下，选择**模型设置** > **模型属性**。

2.  In the Model Properties dialog box, on the **Callbacks** tab, select **PreLoadFcn**.

> 在模型属性对话框中，在**回调**选项卡上，选择**PreLoadFcn**。 3. Enter the MATLAB code for the preload function call. 4. Click **OK**.

To load the parameter values to the MATLAB workspace, save, close, and reopen the model.

> 将参数值加载到 MATLAB 工作空间，保存，关闭并重新打开模型。

### Add Other Blocks to the Model

To complete the model, add a [Sine Wave](https://ww2.mathworks.cn/help/simulink/slref/sinewave.html) (Simulink) block, a [Data Type Conversion](https://ww2.mathworks.cn/help/simulink/slref/datatypeconversion.html) (Simulink) block, and a [Scope](https://ww2.mathworks.cn/help/simulink/slref/scope.html) (Simulink) block. Connect and label the blocks according to this diagram.

> 完成模型，添加[正弦波](https://ww2.mathworks.cn/help/simulink/slref/sinewave.html)（Simulink）块、[数据类型转换](https://ww2.mathworks.cn/help/simulink/slref/datatypeconversion.html)（Simulink）块和[示波器](https://ww2.mathworks.cn/help/simulink/slref/scope.html)（Simulink）块。根据此图连接和标记块。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/LowPassButterworthFilterExample_02.png)

**Sine Wave block**

The Sine Wave block outputs a floating-point signal. The block has these settings:

> 该正弦波块输出一个浮点信号。该块具有以下设置：

- **Sine type**: `Time based`
- **Time**: `Use simulation time`
- **Amplitude**: `1`
- **Bias**: `0`
- **Frequency**: `2*pi*Fc`
- **Phase**: `0`
- **Sample time**: `1/Fs`
- **Interpret vector parameters as 1-D**: `On`

**Data Type Conversion block**

The Data Type Conversion block converts the floating-point signal from the Sine Wave block to a fixed-point signal. By converting the signal to a fixed-point type, you can simulate your model using less memory. The block has these settings:

> 数据类型转换块将正弦波块中的浮点信号转换为定点信号。通过将信号转换为定点类型，您可以使用更少的内存来模拟模型。该块具有以下设置：

- **Output minimum**: `[]`
- **Output maximum**: `[]`
- **Output data type**: `fixdt(1,16,14)`
- **Lock output data type setting against changes by the fixed-point tools**: `Off`
- **Input and output to have equal**: `Real World Value (RWV)`
- **Integer rounding mode**: `Floor`
- **Saturate on integer overflow**: `Off`
- **Sample time**: `-1`

**Scope block**

The Scope block has two input ports that connect to the input and output signals for the Low-Pass Filter chart. To display the two signals separately, select a scope layout with two rows and one column.

> 范围块有两个输入端口，可连接到低通滤波器图表的输入和输出信号。要单独显示两个信号，请选择具有两行一列的范围布局。

### Set Model Configuration Parameters

Because none of the blocks in the model have a continuous sample time, use a discrete solver with these configuration parameters:

> 由于模型中的所有块都没有连续采样时间，因此使用具有以下配置参数的离散求解器：

- **Stop time**: `0.1`
- **Type**: `Fixed-step`
- **Solver**: `discrete (no continuous states)`
- **Fixed-step size (fundamental sample time)**: `1/Fs`

To configure the model:

1.  In the **Modeling** tab, under **Setup**, select **Model Settings**.
2.  In the **Solver** pane, set the discrete solver parameters.
3.  Click **OK**.

### Run the Model

When you simulate the model, the Scope block displays two signals. The top signal shows the fixed-point version of the sine wave input to the chart. The bottom signal corresponds to the filtered output from the chart. The filter removes high-frequency values from the signal but allows low-frequency values to pass through the chart unchanged.

> 当您模拟模型时，Scope 块将显示两个信号。顶部信号显示图表输入的固定点版本的正弦波。底部信号对应于图表中的过滤输出。该滤波器从信号中移除高频值，但允许低频值通过图表不变地传递。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/LowPassButterworthFilterExample_03.png)

## See Also

[`sfnew`](https://ww2.mathworks.cn/help/stateflow/ref/sfnew.html) | [`butter`](https://ww2.mathworks.cn/help/signal/ref/butter.html) (Signal Processing Toolbox) | [Sine Wave](https://ww2.mathworks.cn/help/simulink/slref/sinewave.html) (Simulink) | [Data Type Conversion](https://ww2.mathworks.cn/help/simulink/slref/datatypeconversion.html) (Simulink) | [Scope](https://ww2.mathworks.cn/help/simulink/slref/scope.html) (Simulink)

> sfnew（Stateflow）| butter（信号处理工具箱）| 正弦波（Simulink）| 数据类型转换（Simulink）| 波形示波器（Simulink）

## Related Topics

- [Fixed-Point Data in Stateflow Charts](https://ww2.mathworks.cn/help/stateflow/ug/what-is-fixed-point-data.html)
- [Fixed-Point Operations in Stateflow Charts](https://ww2.mathworks.cn/help/stateflow/ug/how-fixed-point-data-works-in-stateflow-charts.html)
- [Operations for Fixed-Point Data in Stateflow](https://ww2.mathworks.cn/help/stateflow/ug/operations-for-fixed-point-data.html)
