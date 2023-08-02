---
tip: translate by openai@2023-07-31 14:42:57
url: https://ww2.mathworks.cn/help/simulink/sfg/how-the-simulink-engine-interacts-with-c-s-functions.html
title: Simulink Engine Interaction with C S-Functions - MATLAB & Simulink - MathWorks 中国 --- Simulink 引擎与 C S-Function 的交互 - MATLAB & Simulink - MathWorks 中国
date: 2023-07-31 14:40:46
tag:
summary: Learn how the Simulink engine interacts with a C S-function to create and debug your own C S-function......
> 摘要：学习Simulink引擎如何与C S-function交互，以创建和调试您自己的C S-function......
---
## Simulink Engine Interaction with C S-Functions

You can examine how the Simulink® engine interacts with S-functions from two perspectives:

- **Process perspective**, i.e., at which points in a simulation the engine invokes the S-function.
- **Data perspective**, i.e., how the engine and the S-function exchange information during a simulation.

> 你可以从两个角度检查 Simulink® 引擎如何与 S-函数交互：
>
> - 过程视角，即引擎在模拟中何时调用 S-函数。
> - 数据视角，即引擎和 S-函数在模拟过程中如何交换信息。

### Process View

The following figures show the order in which the Simulink engine invokes the callback methods in an S-function. Solid rectangles indicate callbacks that always occur during model initialization or at every time step. Dotted rectangles indicate callbacks that may occur during initialization and/or at some or all time steps during the simulation loop. See the documentation for each callback method to determine the exact circumstances under which the engine invokes the callback.

> 以下图表显示了 S-函数中引擎调用回调方法的顺序。**实心矩形**表示模型初始化期间或每个时间步骤期间总是发生的回调。**虚线矩形**表示可能在初始化期间和/或模拟循环的某些或所有时间步骤期间发生的回调。有关引擎调用回调的确切情况，请参阅每个回调方法的文档。

**Note**

The process view diagram represents the execution of S-functions that contain continuous and discrete states, enable zero-crossing detection, and reside in a model that uses a variable-step solver. Different solvers omit certain steps in the diagram. For a better understanding of how the Simulink engine executes your particular S-function, run the model containing the S-function using the Simulink debugger.

> 流程视图图表表示了包含连续和离散状态、启用零交叉检测以及位于使用可变步长求解器的模型中的 S 函数的执行。**不同的求解器会省略图表中的某些步骤。** 为了更好地理解 Simulink 引擎如何执行您的特定 S 函数，请使用 Simulink 调试器运行包含 S 函数的模型。

In the following model initialization loop, the Simulink engine configures the S-function for an upcoming simulation. The engine always makes the required calls to `mdlInitializeSizes` and `mdlInitializeSampleTime` to set up the fundamental attributes of the S-function, including input and output ports, S-function dialog parameters, work vectors, sample times, etc.

> 在以下模型初始化循环中，Simulink 引擎为即将到来的仿真配置 S 函数。引擎总是调用所需的 `mdlInitializeSizes` 和 `mdlInitializeSampleTime`，以设置 S 函数的基本属性，包括输入和输出端口、S 函数对话框参数、工作向量、采样时间等。

The engine calls additional methods, as needed, to complete the S-function initialization. For example, if the S-function uses work vectors, the engine calls `mdlSetWorkWidths`. Also, if the `mdlInitializeSizes` method deferred setting up input and output port attributes, the engine calls any methods necessary to complete the port initialization, such as `mdlSetInputPortWidth`, during signal propagation. The `mdlStart` method calls the `mdlCheckParameters` and `mdlProcessParameters` methods if the S-function uses dialog parameters.

> **引擎根据需要调用额外的方法来完成 S 函数的初始化**。
>
> - 例如，如果 S 函数使用工作向量，引擎会调用 `mdlSetWorkWidths`。
> - 此外，如果 `mdlInitializeSizes` 方法推迟设置输入和输出端口属性，引擎会在信号传播期间调用任何必要的方法来完成端口初始化，例如 `mdlSetInputPortWidth`。
> - 如果 S 函数使用对话框参数，`mdlStart` 方法会调用 `mdlCheckParameters` 和 `mdlProcessParameters` 方法。

![](https://ww2.mathworks.cn/help/simulink/sfg/sfun_model_initialization_1.png)

**Note**

The `mdlInitializeSizes` callback method also runs when you enter the name of a compiled S-function into the S-Function Block Parameters dialog box.

> 回调方法 `mdlInitializeSizes` 也会在您输入编译后的 S 函数名称到 S 函数块参数对话框时运行。

After initialization, the Simulink engine executes the following simulation loop. If the simulation loop is interrupted, either manually or when an error occurs, the engine jumps directly to the [`mdlTerminate`](https://ww2.mathworks.cn/help/simulink/sfg/mdlterminate.html) method. If the simulation was manually halted, the engine first completes the current time step before invoking `mdlTerminate`.

> 在初始化之后，Simulink 引擎会执行以下仿真循环。如果仿真循环被手动中断或发生错误，引擎会直接跳转到[`mdlTerminate`]方法。如果仿真被手动停止，引擎会先完成当前时间步长，然后调用 `mdlTerminate`。

![](https://ww2.mathworks.cn/help/simulink/sfg/sfunc_simulation_loop.png)

If your model contains multiple S-Function blocks at a given level of model hierarchy, the engine invokes a particular method for every S-function before proceeding to the next method. For example, the engine calls all the `mdlInitializeSizes` methods before calling any `mdlInitializeSampleTimes` methods. The engine uses the block sorted order to determine the order to execute the S-functions. To learn more about how the engine determines the block execution order, see [Control and Display Execution Order](https://ww2.mathworks.cn/help/simulink/ug/controlling-and-displaying-the-sorted-order.html).

> 如果您的模型在给定的模型层次中包含多个 S-Function 块，引擎将为每个 S-function 调用特定的方法，然后再调用下一个方法。例如，引擎会先调用所有的 `mdlInitializeSizes` 方法，然后再调用任何 `mdlInitializeSampleTimes` 方法。引擎使用块排序顺序来确定执行 S-functions 的顺序。要了解有关引擎如何确定块执行顺序的更多信息，请参阅[控制和显示执行顺序]。

#### Calling Structure for Code Generation

If you use the Simulink Coder™ product to generate code for a model containing S-functions, the Simulink engine does not execute the entire calling sequence outlined above. Initialization proceeds as outlined above until the engine reaches the `mdlStart` method. The engine then calls the S-function methods shown in the following figure, where the `mdlRTW` method is unique to the Simulink Coder product.

> 如果您**使用 Simulink Coder™ 产品为包含 S 函数的模型生成代码**，则 Simulink 引擎不会执行上述概述中的整个调用序列。初始化按照上述方式进行，直到引擎到达 `mdlStart` 方法。然后，引擎调用下图中显示的 S 函数方法，其中 `mdlRTW` 方法对 Simulink Coder 产品是唯一的。

![](https://ww2.mathworks.cn/help/simulink/sfg/sfunc_callingstructureforcodegeneration.png)

If the S-function resides in a conditionally executed subsystem, it is possible for the generated code to interleave calls to `mdlInitializeConditions` and `mdlStart`. Consider the following Simulink model.

> 如果 S-函数位于条件执行子系统中，则可能会对 `mdlInitializeConditions` 和 `mdlStart` 进行交错调用。考虑以下 Simulink 模型。

![](https://ww2.mathworks.cn/help/simulink/sfg/sfunc_c27.gif)

The model contains two [nonvirtual subsystems](https://ww2.mathworks.cn/help/simulink/slref/subsystem.html), the conditionally executed enabled subsystem named Reset and the atomic subsystem named Atomic. Each subsystem contains an S-Function block that calls the S-function `dsfunc.c`, which models a discrete state-space system with two states. The enabled subsystem Reset resets the state values when the subsystem is enabled, and the output values when the subsystem is disabled.

> 模型包含两个条件执行启用子系统(nonvirtual subsystems)，分别为名为 Reset 的启用子系统和名为 Atomic 的原子子系统。每个子系统都包含一个调用 S-函数 `dsfunc.c` 的 S-函数块，该函数模拟具有两个状态的离散状态空间系统。启用子系统 Reset 在子系统启用时重置状态值，在子系统禁用时重置输出值。

Using the generic real-time (GRT) target, the generated code for the model-wide `Start` function calls the `Start` functions of the two subsystems before calling the model-wide `MdlInitialize` function, as shown in the following code:

> 使用通用实时(GRT)目标，为模型范围生成的“Start”函数代码在调用模型范围的“MdlInitialize”函数之前，先调用两个子系统的“Start”函数，如下所示代码：

```c
void MdlStart(void) {
  /* snip */

  /* Start for enabled SubSystem: '<Root>/Reset' */
  sfcndemo_enablesub_Reset_Start();
  /* end of Start for SubSystem: '<Root>/Reset' */

  /* Start for atomic SubSystem: '<Root>/Atomic' */
  sfcndemo_enablesub_Atomic_Start();
  /* end of Start for SubSystem: '<Root>/Atomic' */

MdlInitialize();
}
```

The `Start` function for the enabled subsystem calls the subsystem's `InitializeConditions` function:

> 启用的子系统的 `Start` 函数调用子系统的 `InitializeConditions` 函数。

```c
void sfcndemo_enablesub_Reset_Start(void) {
   sfcndemo_enablesub_Reset_Init();
   /* snip */
}
```

The `MdlInitialize` function, called in `MdlStart`, contains a call to the `InitializeConditions` function for the atomic subsystem:

> `MdlStart` 中调用的 `MdlInitialize` 函数包含对原子子系统的 `InitializeConditions` 函数的调用。

```c
void MdlInitialize(void) {
   /* InitializeConditions for atomic SubSystem:
        '<Root>/Atomic' */

   sfcndemo_enablesub_Atomic_Init();
}
```

Therefore, the model-wide `Start` function interleaves calls to the `Start` and `InitializeConditions` functions for the two subsystems and the S-functions they contain.

> 因此，模型范围内的 `Start` 功能会交叉调用两个子系统及其包含的 S-函数的 `Start` 和 `InitializeConditions` 功能。

For more information about the Simulink Coder product and how it interacts with S-functions, see [S-Functions and Code Generation](https://ww2.mathworks.cn/help/rtw/ug/s-functions-and-code-generation.html) (Simulink Coder).

> 了解有关 Simulink Coder 产品及其与 S-函数的交互方式的更多信息，请参阅[S-函数和代码生成]。

#### Alternate Calling Structure for External Mode

When you are running a Simulink model in external mode, the calling sequence for S-function routines changes as shown in the following figure.

> 当您在外部模式下运行 Simulink 模型时，S-函数例程的调用序列如下图所示发生变化。

![](https://ww2.mathworks.cn/help/simulink/sfg/sfunc_c24.gif)

The engine calls `mdlRTW` once when it enters external mode and again each time a parameter changes or when you click **Update Model** on the **Modeling** tab.

> 引擎在进入外部模式时调用 `mdlRTW` 一次，每当参数发生变化或点击**建模**选项卡上的**更新模型**时，又会再次调用一次。

### Data View

S-function blocks have input and output signals, parameters, and internal states, plus other general work areas. In general, block inputs and outputs are written to, and read from, a block I/O vector. Inputs can also come from

> **S 函数块具有输入和输出信号、参数和内部状态，以及其他一般工作区域**。一般来说，块输入和输出写入块 I/O 向量，并从中读取。输入也可以来自块内部。

- External inputs via the root Inport blocks
- Ground if the input signal is unconnected or grounded

Block outputs can also go to the external outputs via the root Outport blocks. In addition to input and output signals, S-functions can have

> 块输出也可以通过根 Outport 块发送到外部输出。除了输入和输出信号，S 函数还可以拥有

- Continuous states
- Discrete states
- Other working areas such as real, integer, or pointer work vectors

You can parameterize S-function blocks by passing parameters to them using the S-Function Block Parameters dialog box.

> 你可以通过使用 S 函数块参数对话框将参数传递给它们来参数化 S 函数块。

The following figure shows the general mapping between these various types of data.

> 下图展示了这些不同类型数据之间的一般映射关系。

![](https://ww2.mathworks.cn/help/simulink/sfg/sfunc_c19.gif)

An S-function's [`mdlInitializeSizes`](https://ww2.mathworks.cn/help/simulink/sfg/mdlinitializesizes.html) routine sets the sizes of the various signals and vectors. S-function methods called during the simulation loop can determine the sizes and values of the signals.

> S-函数的[`mdlInitializeSizes`]例程设置各种信号和向量的大小。在模拟循环期间调用的 S-函数方法可以确定信号的大小和值。

An S-function method can access input signals in two ways:

- Via pointers
- Using contiguous inputs

#### Accessing Signals Using Pointers

During the simulation loop, access the input signals using

```c
InputRealPtrsType uPtrs = ssGetInputPortRealSignalPtrs(S, portIndex)
```

This returns an array of pointers for the input port with index _`portIndex`_, where _`portIndex`_ starts at 0. There is one array of pointers for each input port. To access an element of this array you must use

> 这将返回具有索引 _`portIndex`_ 的输入端口的指针数组，其中*`portIndex`*从 0 开始。每个输入端口有一个指针数组。要访问此数组的元素，必须使用

The following figure describes how to access the input signals of an S-function with two inputs.

> 下图描述了如何访问具有两个输入的 S 函数的输入信号。

![](https://ww2.mathworks.cn/help/simulink/sfg/sfunc_c20.gif)

As shown in the previous figure, the input array pointers can point at noncontiguous places in memory.

> 如前图所示，输入数组指针可以指向内存中不连续的位置。

You can retrieve the output signal by using this code.

```
real_T *y = ssGetOutputPortSignal(S,outputPortIndex);


```

#### Accessing Contiguous Input Signals

An S-function's `mdlInitializeSizes` method can specify that the elements of its input signals must occupy contiguous areas of memory, using [`ssSetInputPortRequiredContiguous`](https://ww2.mathworks.cn/help/simulink/sfg/sssetinputportrequiredcontiguous.html). If the inputs are contiguous, other methods can use [`ssGetInputPortSignal`](https://ww2.mathworks.cn/help/simulink/sfg/ssgetinputportsignal.html) to access the inputs.

> S-函数的 `mdlInitializeSizes` 方法可以指定其输入信号的元素必须占据连续的内存区域，使用[`ssSetInputPortRequiredContiguous`](https://ww2.mathworks.cn/help/simulink/sfg/sssetinputportrequiredcontiguous.html)。如果输入是连续的，其他方法可以使用[`ssGetInputPortSignal`](https://ww2.mathworks.cn/help/simulink/sfg/ssgetinputportsignal.html)来访问输入。

#### Accessing Input Signals of Individual Ports

This section describes how to access all input signals of a particular port and write them to the output port. The preceding figure shows that the input array of pointers can point to noncontiguous entries in the block I/O vector. The output signals of a particular port form a contiguous vector. Therefore, the correct way to access input elements and write them to the output elements (assuming the input and output ports have equal widths) is to use this code.

> 这一节描述了如何访问特定端口的所有输入信号，并将它们写入输出端口。前面的图表显示，输入指针数组可以指向块 I/O 向量中的非连续条目。特定端口的输出信号形成一个连续的向量。因此，访问输入元素并将它们写入输出元素的正确方法(假设输入和输出端口具有相同的宽度)是使用此代码。

```
int_T element;
int_T portWidth = ssGetInputPortWidth(S,inputPortIndex);
InputRealPtrsType uPtrs = ssGetInputPortRealSignalPtrs(S,inputPortIndex);
real_T *y = ssGetOutputPortSignal(S,outputPortIdx);

for (element=0; element<portWidth; element++) {
  y[element] = *uPtrs[element];
}


```

A common mistake is to try to access the input signals via pointer arithmetic. For example, if you were to place

> 一个常见的错误是试图通过指针算术访问输入信号。例如，如果你要放置

```
real_T *u = *uPtrs; /* Incorrect */


```

just below the initialization of `uPtrs` and replace the inner part of the above loop with

> 在 uPtrs 的初始化之下，用以下循环替换上面循环的内部部分：

for (int i = 0; i < uPtrs.size(); ++i)
uPtrs[i]->translate(简体中文);

```
*y++ = *u++; /* Incorrect */


```

the code compiles, but the MEX file might crash the Simulink software. This is because it is possible to access invalid memory (which depends on how you build your model). When accessing the input signals incorrectly, a crash occurs when the signals entering your S-function block are not contiguous. Noncontiguous signal data occurs when signals pass through virtual connection blocks such as the Mux or Selector blocks.

> 代码编译成功，但是 MEX 文件可能会使 Simulink 软件崩溃。这是因为有可能访问无效的内存(这取决于你如何构建模型)。当不正确访问输入信号时，当信号进入 S-function 块时不连续时会发生崩溃。当信号经过虚拟连接块(如 Mux 或 Selector 块)时，会出现非连续的信号数据。

To verify that your S-function correctly accesses wide input signals, pass a replicated signal to each input port of your S-function. To do this, create a Mux block with the number of input ports equal to the width of the desired signal entering your S-function. Then, connect the driving source to each S-function input port, as shown in the following figure. Finally, run your S-function using this input signal to verify that it does not crash and produces expected results.

> 要验证您的 S-函数正确访问宽输入信号，请将复制信号传递给 S-函数的每个输入端口。要做到这一点，请使用等于所需信号进入 S-函数的宽度创建一个 Mux 块。然后，将驱动源连接到 S-函数的每个输入端口，如下图所示。最后，使用此输入信号运行您的 S-函数，以验证它不会崩溃并产生预期的结果。

![](https://ww2.mathworks.cn/help/simulink/sfg/sfunc_c17.gif)

## See Also

[Level-2 MATLAB S-Function](https://ww2.mathworks.cn/help/simulink/slref/level2matlabsfunction.html) | [S-Function Builder](https://ww2.mathworks.cn/help/simulink/slref/sfunctionbuilder.html) | [S-Function](https://ww2.mathworks.cn/help/simulink/slref/sfunction.html) | [MATLAB Function](https://ww2.mathworks.cn/help/simulink/slref/matlabfunction.html)

> 二级 MATLAB S-函数 | S-函数构建器 | S-函数 | MATLAB 函数

## Related Topics

- [S-Function Concepts](https://ww2.mathworks.cn/help/simulink/sfg/s-function-concepts.html)
- [Use S-Functions in Models](https://ww2.mathworks.cn/help/simulink/sfg/what-is-an-s-function.html#f6-54075)
- [S-Function Callback Methods](https://ww2.mathworks.cn/help/simulink/sfg/s-function-callback-methods.html)
