---
tip: translate by openai@2023-07-31 14:29:25
url: https://ww2.mathworks.cn/help/simulink/sfg/writing-level-2-matlab-s-functions.html
title: Write Level-2 MATLAB S-Functions - MATLAB & Simulink - MathWorks 中国 --- 编写 2 级 MATLAB S-Function - MATLAB & Simulink - MathWorks 中国
date: 2023-07-31 14:27:19
tag:
summary: Explains how to create MATLAB S-functions based on the current Level-2 MATLAB S-function application ......
> 概要：解释如何基于当前的Level-2 MATLAB S-function应用程序创建MATLAB S-functions。
---
## Write Level-2 MATLAB S-Functions

### Setup Working Environment to Write Level-2 MATLAB S-Function

This example opens up a directory containing the following files required fro this topic.

> 这个例子打开了一个包含此主题所需文件的目录。

1. `msfuntmpl_basic.m`
2. `msfuntmpl.m`
3. `msfcn_unit_delay.m`
4. `msfcndemo_sfundsc2.slx`

### About Level-2 MATLAB S-Functions

The Level-2 MATLAB® S-function API allows you to use the MATLAB language to create custom blocks with multiple input and output ports and capable of handling any type of signal produced by a Simulink® model, including matrix and frame signals of any data type. The Level-2 MATLAB S-function API corresponds closely to the API for creating C MEX S-functions. Much of the documentation for creating C MEX S-functions applies also to Level-2 MATLAB S-functions. To avoid duplication, this section focuses on providing information that is specific to writing Level-2 MATLAB S-functions.

> 支持级别 2 的 MATLAB® S-Function API 允许您使用 MATLAB 语言创建具有多个输入和输出端口的自定义块，**并能够处理 Simulink® 模型产生的任何类型的信号，包括任何数据类型的矩阵和帧信号**。级别 2 的 MATLAB S-Function API 与创建 C MEX S-Function 的 API 密切相关。创建 C MEX S-Function 的大部分文档也适用于级别 2 的 MATLAB S-Function。为了避免重复，本节专注于提供与编写级别 2 的 MATLAB S-Function 相关的特定信息。

A Level-2 MATLAB S-function is MATLAB function that defines the properties and behavior of an instance of a [Level-2 MATLAB S-Function](https://ww2.mathworks.cn/help/simulink/slref/level2matlabsfunction.html) block that references the MATLAB function in a Simulink model. The MATLAB function itself comprises a set of callback methods (see [Level-2 MATLAB S-Function Callback Methods](https://ww2.mathworks.cn/help/simulink/sfg/writing-level-2-matlab-s-functions.html#f7-68222)) that the Simulink engine invokes when updating or simulating the model. The callback methods perform the actual work of initializing and computing the outputs of the block defined by the S-function.

> 一个 Level-2 MATLAB S-Function 是一种 MATLAB 函数，它定义了一个[Level-2 MATLAB S-Function]块的属性和行为，该块引用了 Simulink 模型中的 MATLAB 函数。MATLAB **函数本身由一组回调方法**(参见[Level-2 MATLAB S-Function 回调方法])组成，**当更新或模拟模型时，Simulink 引擎会调用这些回调方法**。回调方法执行由 S-Function 定义的块的初始化和计算输出的实际工作。

To facilitate these tasks, the engine passes a run-time object to the callback methods as an argument. The run-time object effectively serves as a MATLAB proxy for the S-Function block, allowing the callback methods to set and access the block properties during simulation or model updating.

> 为了便于完成这些任务，**引擎将运行时对象作为参数传递给回调方法**。运行时对象有效地充当 S-Function 块的 MATLAB 代理，允许回调方法在仿真或模型更新期间设置和访问块属性。

### About Run-Time Objects

When the Simulink engine invokes a Level-2 MATLAB S-function callback method, it passes an instance of the [`Simulink.MSFcnRunTimeBlock`](https://ww2.mathworks.cn/help/simulink/slref/simulink.msfcnruntimeblock.html) class to the method as an argument. This instance, known as the run-time object for the S-Function block, serves the same purpose for Level-2 MATLAB S-function callback methods as the `SimStruct` structure serves for C MEX S-function callback methods. The object enables the method to provide and obtain information about various elements of the block ports, parameters, states, and work vectors. The method does this by getting or setting properties or invoking methods of the block run-time object. See the documentation for the [`Simulink.MSFcnRunTimeBlock`](https://ww2.mathworks.cn/help/simulink/slref/simulink.msfcnruntimeblock.html) class for information on getting and setting run-time object properties and invoking run-time object methods.

> 当 Simulink 引擎调用第二级 MATLAB S-function 回调方法时，它将[`Simulink.MSFcnRunTimeBlock`]类的一个实例作为参数传递给该方法。这个实例，即 S-Function 块的运行时对象，为第二级 MATLAB S-function 回调方法提供了与 C MEX S-function 回调方法相同的目的。该对象使得该方法可以提供和获取有关块端口、参数、状态和工作向量的各个元素的信息。该方法通过获取或设置属性或调用块运行时对象的方法来实现这一点。有关获取和设置运行时对象属性以及调用运行时对象方法的信息，请参阅[`Simulink.MSFcnRunTimeBlock`]类的文档。

Run-time objects do not support MATLAB sparse matrices. For example, if the variable `block` is a run-time object, the following line in a Level-2 MATLAB S-function produces an error:

> 运行时对象不支持 MATLAB 稀疏矩阵。例如，如果变量 `block` 是一个运行时对象，则 Level-2 MATLAB S-Function 中的以下行会产生错误：

```matlab
block.Outport(1).Data = speye(10);
```

where the `speye` command forms a sparse identity matrix.

**Note**

Other MATLAB programs besides MATLAB S-functions can use run-time objects to obtain information about a MATLAB S-function in a model that is simulating. See [Access Block Data During Simulation](https://ww2.mathworks.cn/help/simulink/ug/accessing-block-data-during-simulation.html) in _Using Simulink_ for more information.

> 其他除了 MATLAB S-Function 之外的 MATLAB 程序可以使用运行时对象来获取模拟中 MATLAB S-Function 的信息。有关更多信息，请参阅《使用 Simulink》中的[在仿真期间访问块数据]。

### Level-2 MATLAB S-Function Template

Use the basic Level-2 MATLAB S-function template `[msfuntmpl_basic.m](matlab:edit('msfuntmpl_basic.m'))` to get a head start on creating a new Level-2 MATLAB S-function. The template contains skeleton implementations of the required callback methods defined by the Level-2 MATLAB S-function API. To write a more complicated S-function, use the annotated template `[msfuntmpl.m](matlab:edit('msfuntmpl.m'))`.

> 使用基本的 Level-2 MATLAB S-function 模板[msfuntmpl_basic.m]来开始创建一个新的 Level-2 MATLAB S-function。该模板包含 Level-2 MATLAB S-function API 定义的所需回调方法的骨架实现。要编写更复杂的 S-function，请使用带注释的模板[msfuntmpl.m]。

To create a MATLAB S-function, make a copy of the template and edit the copy as necessary to reflect the desired behavior of the S-function you are creating. The following two sections describe the contents of the MATLAB code template. The section [Example of Writing a Level-2 MATLAB S-Function](https://ww2.mathworks.cn/help/simulink/sfg/writing-level-2-matlab-s-functions.html#brgtux6) describes how to write a Level-2 MATLAB S-function that models a unit delay.

> 要创建一个 MATLAB S 函数，复制模板并根据需要编辑副本以反映您正在创建的 S 函数的所需行为。下面的两节描述了 MATLAB 代码模板的内容。[示例：编写第 2 级 MATLAB S 函数] 描述了如何编写一个模拟单位延迟的第 2 级 MATLAB S 函数。

### Level-2 MATLAB S-Function Callback Methods

The Level-2 MATLAB S-function API defines the signatures and general purposes of the callback methods that constitute a Level-2 MATLAB S-function. The S-function itself provides the implementations of these callback methods. The implementations in turn determine the block attributes (e.g., ports, parameters, and states) and behavior (e.g., the block outputs as a function of time and the block inputs, states, and parameters). By creating an S-function with an appropriate set of callback methods, you can define a block type that meets the specific requirements of your application.

> 第二级 MATLAB S-Function API 定义了构成第二级 MATLAB S-Function 的回调方法的签名和一般用途。S-Function 本身提供了这些回调方法的实现。这些实现反过来又确定了块属性(例如，端口、参数和状态)和行为(例如，块输出随时间变化以及块输入、状态和参数)。通过创建具有适当的回调方法集的 S-Function，您可以定义满足特定应用程序要求的块类型。

A Level-2 MATLAB S-function must include the following callback methods:

- A `setup` function to initialize the basic S-function characteristics
- An `Outputs` function to calculate the S-function outputs

Your S-function can contain other methods, depending on the requirements of the block that the S-function defines. The methods defined by the Level-2 MATLAB S-function API generally correspond to similarly named methods defined by the C MEX S-function API. For information on when these methods are called during simulation, see [Process View](https://ww2.mathworks.cn/help/simulink/sfg/how-the-simulink-engine-interacts-with-c-s-functions.html#f8-83424) in [Simulink Engine Interaction with C S-Functions](https://ww2.mathworks.cn/help/simulink/sfg/how-the-simulink-engine-interacts-with-c-s-functions.html).

> 您的 S-Function 可以包含其他方法，取决于 S-Function 定义的块的要求。 Level-2 MATLAB S-Function API 定义的方法通常对应于 C MEX S-Function API 定义的类似命名的方法。 有关模拟期间调用这些方法的信息，请参阅 [Simulink 引擎与 C S-Function 的交互](how-the-simulink-engine-interacts-with-c-s-functions.md)中的[处理视图]。

The following table lists all the Level-2 MATLAB S-function callback methods and their C MEX counterparts.

> 以下表格列出了所有第二级 MATLAB S-Function 回调方法及其 C MEX 对应物。

### Using the `setup` Method

The body of the `setup` method in a Level-2 MATLAB S-function initializes the instance of the corresponding Level-2 MATLAB S-Function block. In this respect, the `setup` method is similar to the [`mdlInitializeSizes`](https://ww2.mathworks.cn/help/simulink/sfg/mdlinitializesizes.html) and [`mdlInitializeSampleTimes`](https://ww2.mathworks.cn/help/simulink/sfg/mdlinitializesampletimes.html) callback methods implemented by C MEX S-functions. The `setup` method performs the following tasks:

> `setup` 方法的本体初始化相应的 Level-2 MATLAB S-Function 块的实例。在这方面，`setup` 方法类似于由 C MEX S-Function 实现的[`mdlInitializeSizes`]和[`mdlInitializeSampleTimes`]回调方法。`setup` 方法执行以下任务：

- Initializing the number of input and output ports of the block.
- Setting attributes such as dimensions, data types, complexity, and sample times for these ports.
- Specifying the block sample time. See [Specify Sample Time](https://ww2.mathworks.cn/help/simulink/ug/how-to-specify-the-sample-time.html) in _Using Simulink_ for more information on how to specify valid sample times.

> 指定块采样时间。有关如何指定有效采样时间的更多信息，请参阅《使用 Simulink》中的[指定采样时间]。

- Setting the number of S-function dialog parameters.
- Registering S-function callback methods by passing the handles of local functions in the MATLAB S-function to the `RegBlockMethod` method of the S-Function block's run-time object. See the documentation for [`Simulink.MSFcnRunTimeBlock`](https://ww2.mathworks.cn/help/simulink/slref/simulink.msfcnruntimeblock.html) for information on using the `RegBlockMethod` method.

> 通过将 MATLAB S 函数中的本地函数句柄传递给 S 函数块的运行时对象的 RegBlockMethod 方法来注册 S 函数回调方法。有关使用 RegBlockMethod 方法的信息，请参阅[Simulink.MSFcnRunTimeBlock]的文档。

### Example of Writing a Level-2 MATLAB S-Function

The following steps illustrate how to write a simple Level-2 MATLAB S-function. When applicable, the steps include examples from the S-function example `[msfcn_unit_delay.m](matlab:edit('msfcn_unit_delay.m'))` used in the model `[msfcndemo_sfundsc2](matlab:open_system('msfcndemo_sfundsc2');)`. All lines of code use the variable name `block` for the S-function run-time object.

> 以下步骤演示了如何编写一个简单的 Level-2 MATLAB S-Function。在适用的情况下，步骤包括来自模型[msfcndemo_sfundsc2]中使用的 S-Function 示例[msfcn_unit_delay.m]的示例。所有代码行都使用变量名 `block` 表示 S-Function 运行时对象。

1. Open MATLAB S-function template `[msfuntmpl_basic.m](matlab:edit('msfuntmpl_basic.m'))` from the working folder. If you change the file name when you copy the file, change the function name in the `function` line to the same name.

> 打开 MATLAB S-Function 模板 `[msfuntmpl_basic.m](matlab:edit('msfuntmpl_basic.m'))`，该文件位于当前工作文件夹中。如果复制文件时更改了文件名，请将 `function` 行中的函数名更改为相同的名称。

2. Modify the `setup` method to initialize the S-function's attributes. For this example:

   - Set the run-time object's `NumInputPorts` and `NumOutputPorts` properties to `1` in order to initialize one input port and one output port.
   - Invoke the run-time object's [SetPreCompInpPortInfoToDynamic](https://ww2.mathworks.cn/help/simulink/slref/simulink.msfcnruntimeblock.html#f29-99863) and [SetPreCompOutPortInfoToDynamic](https://ww2.mathworks.cn/help/simulink/slref/simulink.msfcnruntimeblock.html#f29-99987) methods to indicate that the input and output ports inherit their compiled properties (dimensions, data type, complexity, and sampling mode) from the model.
   - Set the `DirectFeedthrough` property of the run-time object's `InputPort` to `false` in order to indicate the input port does not have direct feedthrough. Retain the default values for all other input and output port properties that are set in your copy of the template file. The values set for the `Dimensions`, `DatatypeID`, and `Complexity` properties override the values inherited using the `SetPreCompInpPortInfoToDynamic` and `SetPreCompOutPortInfoToDynamic` methods.
   - Set the run-time object's `NumDialogPrms` property to `1` in order to initialize one S-function dialog parameter.
   - Specify that the S-function has an inherited sample time by setting the value of the runtime object's `SampleTimes` property to `[-1 0]`.
   - Call the run-time object's `RegBlockMethod` method to register the following four callback methods used in this S-function.

     - `PostPropagationSetup`
     - `InitializeConditions`
     - `Outputs`
     - `Update`

   > - 将运行时对象的 `NumInputPorts` 和 `NumOutputPorts` 属性设置为 `1`，以初始化一个输入端口和一个输出端口。
   > - 调用运行时对象的 [SetPreCompInpPortInfoToDynamic] 和 [SetPreCompOutPortInfoToDynamic] 方法，表明输入和输出端口从模型继承其编译属性（尺寸、数据类型、复杂性和采样模式）。
   > - 将运行时对象的 `InputPort` 的 `DirectFeedthrough` 属性设置为 `false`，以表示输入端口没有直接馈通。保留在您复制的模板文件中设置的所有其他输入和输出端口属性的默认值。对于使用 `SetPreCompInpPortInfoToDynamic` 和 `SetPreCompOutPortInfoToDynamic` 方法继承的值，设定了 `Dimensions`、`DatatypeID` 和 `Complexity` 属性将覆盖这些值。
   > - 将运行时对象的 `NumDialogPrms` 属性设置为 `1` ，以初始化一个 S-function 对话参数。
   > - 通过将运行时对象的 `SampleTimes` 属性设定为 [-1 0] 来指定 S-function 具有继承样本时间。
   > - 调用运行时对象的 `RegBlockMethod` 方法来注册此 S-function 中使用到得以下四个回调方法。
   >

   Remove any other registered callback methods from your copy of the template file. In the calls to `RegBlockMethod`, the first input argument is the name of the S-function API method and the second input argument is the function handle to the associated local function in the MATLAB S-function.

   > 从模板文件副本中删除任何其他已注册的回调方法。在对 `RegBlockMethod` 的调用中，第一个输入参数是 S-function API 方法的名称，第二个输入参数是 MATLAB S-function 中相关联的局部函数的函数句柄。
   >

   The following `setup` method from `msfcn_unit_delay.m` performs the previous list of steps:

   ```matlab
   function setup(block)

     %% Register a single dialog parameter
     block.NumDialogPrms  = 1;

     %% Register number of input and output ports
     block.NumInputPorts  = 1;
     block.NumOutputPorts = 1;

     %% Setup functional port properties to dynamically
     %% inherited.
     block.SetPreCompInpPortInfoToDynamic;
     block.SetPreCompOutPortInfoToDynamic;

     %% Hard-code certain port properties
     block.InputPort(1).Dimensions        = 1;
     block.InputPort(1).DirectFeedthrough = false;

     block.OutputPort(1).Dimensions       = 1;

     %% Set block sample time to [0.1 0]
     block.SampleTimes = [0.1 0];

     %% Register methods
     block.RegBlockMethod('PostPropagationSetup',@DoPostPropSetup);
     block.RegBlockMethod('InitializeConditions',@InitConditions);
     block.RegBlockMethod('Outputs',             @Output);
     block.RegBlockMethod('Update',              @Update);
   end
   ```

   If your S-function needs continuous states, initialize the number of continuous states in the `setup` method using the run-time object's `NumContStates` property. Do not initialize discrete states in the `setup` method.

   > 如果 S 函数需要连续状态，请在 `setup` 方法中使用运行时对象的 `NumContStates` 属性初始化连续状态的数量。不要在 `setup` 方法中初始化离散状态。
   >
3. Initialize the discrete states in the `PostPropagationSetup` method. A Level-2 MATLAB S-function stores discrete state information in a DWork vector. The default `PostPropagationSetup` method in the template file suffices for this example.

   > 在 `PostPropagationSetup` 方法中初始化离散状态。 Level-2 MATLAB S-function 将离散状态信息存储在 DWork 向量中。 模板文件中的默认 `PostPropagationSetup` 方法对于此示例足够。
   >

   The following `PostPropagationSetup` method from `msfcn_unit_delay.m`, named `DoPostPropSetup`, initializes one DWork vector with the name `x0`.

   ```matlab
   function DoPostPropSetup(block)
     %% Setup Dwork
     block.NumDworks = 1;
     block.Dwork(1).Name = 'x0';
     block.Dwork(1).Dimensions      = 1;
     block.Dwork(1).DatatypeID      = 0;
     block.Dwork(1).Complexity      = 'Real';
     block.Dwork(1).UsedAsDiscState = true;

   ```

   If your S-function uses additional DWork vectors, initialize them in the `PostPropagationSetup` method, as well (see [Using DWork Vectors in Level-2 MATLAB S-Functions](https://ww2.mathworks.cn/help/simulink/sfg/dwork-matlab.html#brd2qpw)).
4. Initialize the values of discrete and continuous states or other DWork vectors in the `InitializeConditions` or `Start` callback methods. Use the `Start` callback method for values that are initialized once at the beginning of the simulation. Use the `InitializeConditions` method for values that need to be reinitialized whenever an enabled subsystem containing the S-function is reenabled.

   > 在 `InitializeConditions` 或 `Start` 回调方法中初始化离散和连续状态或其他 DWork 向量的值。对于在仿真开始时只初始化一次的值，请使用 `Start` 回调方法。对于每次启用包含 S-Function 的启用子系统时都需要重新初始化的值，请使用 `InitializeConditions` 方法。
   >

   For this example, use the `InitializeConditions` method to set the discrete state's initial condition to the value of the S-function's dialog parameter. For example, the `InitializeConditions` method in `msfcn_unit_delay.m` is:

   ```matlab
   function InitConditions(block)
     %% Initialize Dwork
     block.Dwork(1).Data = block.DialogPrm(1).Data;
   ```

   For S-functions with continuous states, use the `ContStates` run-time object method to initialize the continuous state data. For example:

   ```matlab
    block.ContStates.Data(1) = 1.0;
   ```
5. Calculate the S-function's outputs in the `Outputs` callback method. For this example, set the output to the current value of the discrete state stored in the DWork vector.

   > 计算 S 函数的输出在“输出”回调方法中。对于这个例子，将输出设置为存储在 DWork 向量中的离散状态的当前值。
   >

   The `Outputs` method in `msfcn_unit_delay.m` is:

   ```matlab
   function Output(block)
     block.OutputPort(1).Data = block.Dwork(1).Data;
   ```
6. For an S-function with continuous states, calculate the state derivatives in the `Derivatives` callback method. Run-time objects store derivative data in their `Derivatives` property. For example, the following line sets the first state derivative equal to the value of the first input signal.

   > 对于具有连续状态的 S 函数，请在 `Derivatives` 回调方法中计算状态导数。运行时对象将导数数据存储在其 `Derivatives` 属性中。例如，以下行将第一个状态导数设置为第一个输入信号的值。
   >

   ```matlab
   block.Derivatives.Data(1) = block.InputPort(1).Data;
   ```

   This example does not use continuous states and, therefore, does not implement the `Derivatives` callback method.
7. Update any discrete states in the `Update` callback method. For this example, set the value of the discrete state to the current value of the first input signal.

   > 在 `Update` 回调方法中更新任何离散状态。对于此示例，将离散状态的值设置为第一个输入信号的当前值。
   >

   The `Update` method in `msfcn_unit_delay.m` is:

   ```matlab
   function Update(block)
     block.Dwork(1).Data = block.InputPort(1).Data;
   ```
8. Perform any cleanup, such as clearing variables or memory, in the `Terminate` method. Unlike C MEX S-functions, Level-2 MATLAB S-function are not required to have a `Terminate` method.

> 在 `Terminate` 方法中执行任何清理，例如清除变量或内存。与 C MEX S-Function 不同，**Level-2 MATLAB S-Function 不需要有 `Terminate` 方法**。

For information on additional callback methods, see [Level-2 MATLAB S-Function Callback Methods](https://ww2.mathworks.cn/help/simulink/sfg/writing-level-2-matlab-s-functions.html#f7-68222). For a list of run-time object properties, see the reference page for [`Simulink.MSFcnRunTimeBlock`](https://ww2.mathworks.cn/help/simulink/slref/simulink.msfcnruntimeblock.html) and the parent class [`Simulink.RunTimeBlock`](https://ww2.mathworks.cn/help/simulink/slref/simulink.runtimeblock.html).

> 对于其他回调方法的信息，请参阅[Level-2 MATLAB S-Function 回调方法]。有关运行时对象属性的列表，请参阅[`Simulink.MSFcnRunTimeBlock`]的参考页面以及父类[`Simulink.RunTimeBlock`]。

### Instantiating a Level-2 MATLAB S-Function

To use a Level-2 MATLAB S-function in a model, copy an instance of the [Level-2 MATLAB S-Function](https://ww2.mathworks.cn/help/simulink/slref/level2matlabsfunction.html) block into the model. Open the Block Parameters dialog box for the block and enter the name of the MATLAB file that implements your S-function into the **S-function name** field. If your S-function uses any additional parameters, enter the parameter values as a comma-separated list in the Block Parameters dialog box **Parameters** field.

> 在模型中使用二级 MATLAB S-Function，请将[二级 MATLAB S-Function]块的一个实例复制到模型中。打开该块的块参数对话框，并在 **S-Function 名称**字段中输入实现 S-Function 的 MATLAB 文件的名称。如果您的 S-Function 使用任何其他参数，请在块参数对话框**参数**字段中以逗号分隔的列表形式输入参数值。

### Operations for Variable-Size Signals

Following are modifications to the Level-2 MATLAB S-functions template (`[msfuntmpl_basic.m](matlab:edit('msfuntmpl_basic.m'))`) and additional operations that allow you to use variable-size signals.

> 以下是对 Level-2 MATLAB S-functions 模板(`[msfuntmpl_basic.m](matlab:edit('msfuntmpl_basic.m'))`)的修改以及允许您使用可变大小信号的附加操作。

```matlab
function setup(block)
  % Register the properties of the output port
  block.OutputPort(1).DimensionsMode = 'Variable';
  block.RegBlockMethod('SetInputPortDimensionsMode',  @SetInputDimsMode);

function DoPostPropSetup(block)
  %Register dependency rules to update current output size of output port a depending on
  %input ports b and c
  block.AddOutputDimsDependencyRules(a, [b c], @setOutputVarDims);

  %Configure output port b to have the same dimensions as input port a
  block.InputPortSameDimsAsOutputPort(a,b);

  %Configure DWork a to have its size reset when input size changes.
  block.DWorkRequireResetForSignalSize(a,true);

function SetInputDimsMode(block, port, dm)
  % Set dimension mode
  block.InputPort(port).DimensionsMode = dm;
  block.OutputPort(port).DimensionsMode = dm;

function setOutputVarDims(block, opIdx, inputIdx)
  % Set current (run-time) dimensions of the output
  outDimsAfterReset = block.InputPort(inputIdx(1)).CurrentDimensions;
  block.OutputPort(opIdx).CurrentDimensions = outDimsAfterReset;
```

### Generating Code from a Level-2 MATLAB S-Function

Generating code for a model containing a Level-2 MATLAB S-function requires that you provide a corresponding Target Language Compiler (TLC) file. You do not need a TLC file to accelerate a model containing a Level-2 MATLAB S-function. The Simulink Accelerator™ software runs Level-2 MATLAB S-functions in interpreted mode. However,​ M-file S-functions do not work with accelerated mode if the M-file S-function is in a model reference. For more information on writing TLC files for MATLAB S-functions, see [Inlining S-Functions](https://ww2.mathworks.cn/help/rtw/tlc/inlining-s-functions.html) (Simulink Coder) and [Inline MATLAB File S-Functions](https://ww2.mathworks.cn/help/rtw/tlc/introduction-inline-s-functions.html#f34125) (Simulink Coder).

> 生成包含 Level-2 MATLAB S-Function 的模型的代码需要提供相应的目标语言编译器(TLC)文件。不需要 TLC 文件来加速包含 Level-2 MATLAB S-Function 的模型。 Simulink Accelerator™ 软件以解释模式运行 Level-2 MATLAB S-Function。 但是，如果 M-file S-Function 位于模型引用中，则 M-file S-Function 不适用于加速模式。 有关编写 MATLAB S-Function 的 TLC 文件的更多信息，请参阅 [Inlining S-Functions](https://ww2.mathworks.cn/help/rtw/tlc/inlining-s-functions.html)(Simulink Coder)和 [Inline MATLAB File S-Functions](https://ww2.mathworks.cn/help/rtw/tlc/introduction-inline-s-functions.html#f34125)(Simulink Coder)。

### MATLAB S-Function Examples

The Level-2 MATLAB S-function examples provide a set of self-documenting models that illustrate the use of Level-2 MATLAB S-functions. Enter `[sfundemos](matlab:open_system('sfundemos');)` at the MATLAB command prompt to view the examples.

> Level-2 MATLAB S-Function 示例提供一组自文档化的模型，用于说明 Level-2 MATLAB S-Function 的使用。在 MATLAB 命令提示符下输入 `[sfundemos](matlab:open_system('sfundemos');)` 以查看示例。

### MATLAB S-Function Limitations

- Level-2 MATLAB S-functions do not support zero-crossing detection.
- You cannot trigger a function-call subsystem from a Level-2 MATLAB S-function.

## See Also

[Level-2 MATLAB S-Function](https://ww2.mathworks.cn/help/simulink/slref/level2matlabsfunction.html) | [S-Function Builder](https://ww2.mathworks.cn/help/simulink/slref/sfunctionbuilder.html) | [S-Function](https://ww2.mathworks.cn/help/simulink/slref/sfunction.html) | [MATLAB Function](https://ww2.mathworks.cn/help/simulink/slref/matlabfunction.html)

> [二级 MATLAB S-Function] | [S-Function 构建器] | [S-Function] | [MATLAB 函数]

## Related Topics

- [S-Function Concepts](https://ww2.mathworks.cn/help/simulink/sfg/s-function-concepts.html)
- [S-Function Examples](https://ww2.mathworks.cn/help/simulink/sfg/s-function-examples.html)
- [Configure Block Features for MATLAB S-Functions](https://ww2.mathworks.cn/help/simulink/configure_matlab_block_features.html)
