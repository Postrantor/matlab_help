---
tip: translate by openai@2023-08-01 10:23:07
url: https://www.mathworks.com/help/simulink/ug/tutorial-creating-a-custom-block.html#bq60d2q
简体中文：https://www.mathworks.com/help/simulink/ug/教程-创建自定义块.html#bq60d2q
title: Design and Create a Custom Block - MATLAB & Simulink --- 设计和创建自定义模块 - MATLAB & Simulink
date: 2023-08-01 10:21:38
tag:
summary: Build a custom block using a Level-2 MATLAB S-function.
---
## Design and Create a Custom Block

### Setup Working Environment to Design and Create a Custom Block

This example open a directory containing the following files required for this topic.

> 这个例子打开了一个目录，其中包含了本主题所需的文件。

1. `ex_customsat_lib.slx`
2. `sldemo_customsat.slx`
3. `msfuntmpl.m`
4. `custom_sat.m`
5. `custom_sat_final.m`
6. `customsat_callback.m`
7. `customsat_plotcallback.m`
8. `custom_sat_plot.m`
9. `plotsat.m`

### How to Design a Custom Block

In general, use the following process to design a custom block:

Suppose you want to create a customized saturation block that limits the upper and lower bounds of a signal based on either a block parameter or the value of an input signal. In a second version of the block, you want the option to plot the saturation limits after the simulation is finished. The following tutorial steps you through designing these blocks. The library `[ex_customsat_lib](matlab:open_system('ex_customsat_lib');)` contains the two versions of the customized saturation block.

> 假设您想创建一个定制的饱和块，根据块参数或输入信号的值限制信号的上下界。在第二个版本的块中，您希望在模拟完成后选择绘制饱和限制。以下教程将指导您设计这些块。库 `[ex_customsat_lib](matlab:open_system('ex_customsat_lib');)` 包含两个版本的定制饱和块。

![](https://www.mathworks.com/help/simulink/ug/customlib.png)

The example model `[sldemo_customsat](matlab:open_system('sldemo_customsat');)` uses the basic version of the block.

> 例如模型 `[sldemo_customsat](matlab:open_system('sldemo_customsat');)` 使用基本版本的块。

![](https://www.mathworks.com/help/simulink/ug/customsat_model.png)

### Defining Custom Block Behavior

Begin by defining the features and limitations of your custom block. In this example, the block supports the following features:

> 开始定义自定义块的特性和限制。在这个例子中，该块支持以下特性：

- Turning on and off the upper or lower saturation limit.
- Setting the upper and/or lower limits via a block parameters.
- Setting the upper and/or lower limits using an input signal.

It also has the following restrictions:

- The input signal under saturation must be a scalar.
- The input signal and saturation limits must all have a data type of double.
- Code generation is not required.

### Deciding on a Custom Block Type

Based on the custom block features, the implementation needs to support the following:

> 根据自定义块功能，实施需要支持以下内容：

- Multiple input ports
- A relatively simple algorithm
- No continuous or discrete system states

Therefore, this tutorial implements the custom block using a Level-2 MATLAB® S-function. MATLAB S-functions support multiple inputs and, because the algorithm is simple, do not have significant overhead when updating the diagram or simulating the model. See [Comparison of Custom Block Functionality](https://www.mathworks.com/help/simulink/ug/comparison-of-custom-block-functionality.html) for a description of the different functionality provided by MATLAB S-functions as compared to other types of custom blocks.

> 因此，本教程使用第 2 级 MATLAB® S 函数实现自定义块。MATLAB S 函数支持多个输入，并且由于算法简单，因此在更新图表或模拟模型时不会产生重大开销。有关 MATLAB S 函数与其他类型自定义块提供的不同功能的描述，请参阅[自定义块功能比较](https://www.mathworks.com/help/simulink/ug/comparison-of-custom-block-functionality.html)。

#### Parameterizing the MATLAB S-Function

Begin by defining the S-function parameters. This example requires four parameters:

> 开始定义 S-函数参数。此示例需要四个参数：

- The first parameter indicates how the upper saturation limit is set. The limit can be off, set via a block parameter, or set via an input signal.

> 第一个参数表明如何设置上饱和限制。该限制可以关闭，通过块参数设置，或通过输入信号设置。

- The second parameter is the value of the upper saturation limit. This value is used only if the upper saturation limit is set via a block parameter. In the event this parameter is used, you should be able to change the parameter value during the simulation, i.e., the parameter is tunable.

> 第二个参数是上饱和限制的值。只有在通过块参数设置上饱和限制时才使用此值。如果使用此参数，您应该能够在模拟期间更改参数值，即参数是可调的。

- The third parameter indicates how the lower saturation limit is set. The limit can be off, set via a block parameter, or set via an input signal.

> 第三个参数表明如何设置较低的饱和度限制。该限制可以关闭，通过块参数设置，或通过输入信号设置。

- The fourth parameter is the value of the lower saturation limit. This value is used only if the lower saturation limit is set via a block parameter. As with the upper saturation limit, this parameter is tunable when in use.

> 第四个参数是下饱和限制的值。只有在通过块参数设置下饱和限制时才使用此值。与上饱和限制一样，此参数在使用时可调节。

The first and third S-function parameters represent modes that must be translated into values the S-function can recognize. Therefore, define the following values for the upper and lower saturation limit modes:

> 第一个和第三个 S-函数参数代表模式，必须转换为 S-函数可以识别的值。因此，为上限和下限饱和限制模式定义以下值：

- `1` indicates that the saturation limit is off.
- `2` indicates that the saturation limit is set via a block parameter.
- `3` indicates that the saturation limit is set via an input signal.

#### Writing the MATLAB S-Function

After you define the S-function parameters and functionality, write the S-function. The template [`msfuntmpl.m`](matlab:edit('msfuntmpl.m');) provides a starting point for writing a Level-2 MATLAB S-function. You can find a completed version of the custom saturation block in the file [`custom_sat.m`](matlab:edit('custom_sat.m');). Open this file before continuing with this tutorial.

> 在定义 S 函数参数和功能后，编写 S 函数。模板[`msfuntmpl.m`](matlab:edit('msfuntmpl.m');)提供了编写 Level-2 MATLAB S 函数的起点。您可以在文件[`custom_sat.m`](matlab:edit('custom_sat.m');)中找到自定义饱和块的完整版本。在继续本教程之前，请打开此文件。

This S-function modifies the S-function template as follows:

- The `setup` function initializes the number of input ports based on the values entered for the upper and lower saturation limit modes. If the limits are set via input signals, the method adds input ports to the block. The `setup` method then indicates there are four S-function parameters and sets the parameter tunability. Finally, the method registers the S-function methods used during simulation.

> `setup` 函数根据输入的上下饱和限制模式的值初始化输入端口的数量。如果限制是通过输入信号设置的，该方法会向块中添加输入端口。然后，`setup` 方法指示有四个 S-函数参数，并设置参数可调性。最后，该方法注册模拟期间使用的 S-函数方法。

```
function setup(block)

% The Simulink engine passes an instance of the Simulink.MSFcnRunTimeBlock
% class to the setup method in the input argument "block". This is known as
% the S-function block's run-time object.

% Register original number of input ports based on the S-function
% parameter values

try % Wrap in a try/catch, in case no S-function parameters are entered
    lowMode    = block.DialogPrm(1).Data;
    upMode     = block.DialogPrm(3).Data;
    numInPorts = 1 + isequal(lowMode,3) + isequal(upMode,3);
catch
    numInPorts=1;
end % try/catch
block.NumInputPorts = numInPorts;
block.NumOutputPorts = 1;

% Setup port properties to be inherited or dynamic
block.SetPreCompInpPortInfoToDynamic;
block.SetPreCompOutPortInfoToDynamic;

% Override input port properties
block.InputPort(1).DatatypeID  = 0;  % double
block.InputPort(1).Complexity  = 'Real';

% Override output port properties
block.OutputPort(1).DatatypeID  = 0; % double
block.OutputPort(1).Complexity  = 'Real';

% Register parameters. In order:
% -- If the upper bound is off (1) or on and set via a block parameter (2)
%    or input signal (3)
% -- The upper limit value. Should be empty if the upper limit is off or
%    set via an input signal
% -- If the lower bound is off (1) or on and set via a block parameter (2)
%    or input signal (3)
% -- The lower limit value. Should be empty if the lower limit is off or
%    set via an input signal
block.NumDialogPrms     = 4;
block.DialogPrmsTunable = {'Nontunable','Tunable','Nontunable', ...
    'Tunable'};

% Register continuous sample times [0 offset]
block.SampleTimes = [0 0];

%% -----------------------------------------------------------------
%% Options
%% -----------------------------------------------------------------
% Specify if Accelerator should use TLC or call back into
% MATLAB script
block.SetAccelRunOnTLC(false);

%% -----------------------------------------------------------------
%% Register methods called during update diagram/compilation
%% -----------------------------------------------------------------

block.RegBlockMethod('CheckParameters',      @CheckPrms);
block.RegBlockMethod('ProcessParameters',    @ProcessPrms);
block.RegBlockMethod('PostPropagationSetup', @DoPostPropSetup);
block.RegBlockMethod('Outputs',              @Outputs);
block.RegBlockMethod('Terminate',            @Terminate);
%end setup function


```

- The `CheckParameters` method verifies the values entered into the Level-2 MATLAB S-Function block.

> 方法 `CheckParameters` 验证输入到 Level-2 MATLAB S-Function 块中的值。

```
function CheckPrms(block)

lowMode = block.DialogPrm(1).Data;
lowVal  = block.DialogPrm(2).Data;
upMode  = block.DialogPrm(3).Data;
upVal   = block.DialogPrm(4).Data;

% The first and third dialog parameters must have values of 1-3
if ~any(upMode == [1 2 3]);
    error('The first dialog parameter must be a value of 1, 2, or 3');
end

if ~any(lowMode == [1 2 3]);
    error('The first dialog parameter must be a value of 1, 2, or 3');
end

upper or lower bound is specified via a dialog, make sure there
% is a specified bound. Also, check that the value is of type double
if isequal(upMode,2),
    if isempty(upVal),
        error('Enter a value for the upper saturation limit.');
    end
    if ~strcmp(class(upVal), 'double')
        error('The upper saturation limit must be of type double.');
    end
end

if isequal(lowMode,2),
    if isempty(lowVal),
        error('Enter a value for the lower saturation limit.');
    end
    if ~strcmp(class(lowVal), 'double')
        error('The lower saturation limit must be of type double.');
    end
end

% If a lower and upper limit are specified, make sure the specified
% limits are compatible.
if isequal(upMode,2) && isequal(lowMode,2),
    if lowVal >= upVal,
        error('The lower bound must be less than the upper bound.');
    end
end

%end CheckPrms function


```

- The `ProcessParameters` and `PostPropagationSetup` methods handle the S-function parameter tuning.

> 这两个方法 `ProcessParameters` 和 `PostPropagationSetup` 处理 S 函数参数调节。

```
function ProcessPrms(block)

%% Update run time parameters
block.AutoUpdateRuntimePrms;

%end ProcessPrms function


function DoPostPropSetup(block)

%% Register all tunable parameters as runtime parameters.
block.AutoRegRuntimePrms;

%end DoPostPropSetup function

```

- The `Outputs` method calculates the block's output based on the S-function parameter settings and any input signals.

> 输出方法根据 S-函数参数设置和任何输入信号计算块的输出。

```
function Outputs(block)

lowMode    = block.DialogPrm(1).Data;
upMode     = block.DialogPrm(3).Data;
sigVal     = block.InputPort(1).Data;
lowPortNum = 2; % Initialize potential input number for lower saturation limit

% Check upper saturation limit
if isequal(upMode,2), % Set via a block parameter
    upVal = block.RuntimePrm(2).Data;
elseif isequal(upMode,3), % Set via an input port
    upVal = block.InputPort(2).Data;
    lowPortNum = 3; % Move lower boundary down one port number
else
    upVal = inf;
end

% Check lower saturation limit
if isequal(lowMode,2), % Set via a block parameter
    lowVal = block.RuntimePrm(1).Data;
elseif isequal(lowMode,3), % Set via an input port
    lowVal = block.InputPort(lowPortNum).Data;
else
    lowVal = -inf;
end

% Assign new value to signal
if sigVal > upVal,
    sigVal = upVal;
elseif sigVal < lowVal,
    sigVal=lowVal;
end

block.OutputPort(1).Data = sigVal;

%end Outputs function

```

### Placing Custom Blocks in a Library

Libraries allow you to share your custom blocks with other users, easily update the functionality of copies of the custom block, and collect blocks for a particular project into a single location. If your custom block requires a bus as an interface, you can share the bus object with library users by creating the bus object in a data dictionary and attaching the dictionary to the library (See [Attach Data Dictionary to Custom Libraries](https://www.mathworks.com/help/simulink/ug/attach-data-dictionary-to-custom-libraries.html)).

> 图书馆允许您与其他用户共享自定义块，轻松更新自定义块的功能，并将特定项目的块收集到单个位置。如果您的自定义块需要总线作为接口，您可以通过在数据字典中创建总线对象并将字典附加到图书馆（参见[将数据字典附加到自定义图书馆](https://www.mathworks.com/help/simulink/ug/attach-data-dictionary-to-custom-libraries.html)）来与图书馆用户共享总线对象。

This example places the custom saturation block into a library.

1. In the Simulink® Editor, in the **Simulation** tab, select > .
2. From the User-Defined Functions library, drag a Level-2 MATLAB S-Function block into your new library.

> 从用户定义函数库中，将一个第二级 MATLAB S-Function 块拖放到您的新库中。

```
![](https://www.mathworks.com/help/simulink/ug/saturation_lib.png)
```

3. Save your library with the filename `saturation_lib`.
4. Double-click the block to open its Function Block Parameters dialog box.

> 双击块以打开其功能块参数对话框。

5. In the **S-function name** field, enter the name of the S-function. For example, enter `custom_sat`. In the **Parameters** field enter `2,-1,2,1`.

> 在 **S 函数名称**字段中，输入 S 函数的名称。例如，输入 `custom_sat`。在**参数**字段中输入 `2,-1,2,1`。

```
![](https://www.mathworks.com/help/simulink/ug/saturation_lib1_dialog.png)
```

6. Click **OK**.

   You have created a custom saturation block that you can share with other users.

   ![](https://www.mathworks.com/help/simulink/ug/saturation_lib_with_s_function.png)

You can make the block easier to use by adding a customized user interface.

> 你可以通过添加定制的用户界面来使块更容易使用。

### Adding a User Interface to a Custom Block

You can create a block dialog box for a custom block using the masking features of Simulink. Masking the block also allows you to add port labels to indicate which ports corresponds to the input signal and the saturation limits.

> 您可以使用 Simulink 的遮罩功能为自定义块创建块对话框。遮罩块还允许您添加端口标签以指示哪些端口对应于输入信号和饱和限制。

1. Open the library `saturation_lib` that contains the custom block you created,

> 打开包含自定义块的库 `saturation_lib`。 2. Right-click the Level-2 MATLAB S-Function block and select > .

3. On the **Icon & Ports** pane in the **Icons drawing commands** box, enter `port_label('input',1,'uSig')`, and then click .

> 在**图标和端口**面板上的**图标绘制指令**框中输入 `port_label('input',1,'uSig')`，然后单击。

```
This command labels the default port as the input signal under saturation.

![](https://www.mathworks.com/help/simulink/ug/saturation_lib2.png)
```

4. In the **Parameters & Dialog** pane, add four parameters corresponding to the four S-Function parameters. For each new parameter, drag a popup or edit control to the **Dialog box** section, as shown in the table. Drag each parameter into the Parameters group.

> 在**参数和对话框**面板中，添加四个与四个 S-Function 参数对应的参数。对于每个新参数，将弹出窗口或编辑控件拖到**对话框**部分，如表所示。将每个参数拖入参数组。

```
<table><colgroup><col width="7%"><col width="10%"><col width="9%"><col width="8%"><col width="8%"><col width="22%"><col width="36%"></colgroup><thead><tr><th>Type</th><th>Prompt</th><th>Name</th><th>Evaluate</th><th>Tunable</th><th>Popup options</th><th>Callback</th></tr></thead><tbody><tr><td><code>popup</code></td><td>Upper boundary:</td><td><code>upMode</code></td><td>✓</td><td>&nbsp;</td><td><p>No limit</p><p>Enter limit as parameter</p><p>Limit using input signal</p></td><td><code>customsat_callback('upperbound_callback', gcb)</code></td></tr><tr><td><code>edit</code></td><td>Upper limit:</td><td><code>upVal</code></td><td>✓</td><td>✓</td><td>N/A</td><td><code>customsat_callback('upperparam_callback', gcb)</code></td></tr></tbody></table>

<table><colgroup><col width="7%"><col width="10%"><col width="9%"><col width="8%"><col width="8%"><col width="22%"><col width="36%"></colgroup><thead><tr><th>Type</th><th>Prompt</th><th>Name</th><th>Evaluate</th><th>Tunable</th><th>Popup options</th><th>Callback</th></tr></thead><tbody><tr><td><code>popup</code></td><td>Lower boundary:</td><td><code>lowMode</code></td><td>✓</td><td>&nbsp;</td><td><p>No limit</p><p>Enter limit as parameter</p><p>Limit using input signal</p></td><td><code>customsat_callback('lowerbound_callback', gcb)</code></td></tr><tr><td><code>edit</code></td><td>Lower limit:</td><td><code>lowVal</code></td><td>✓</td><td>✓</td><td>N/A</td><td><code>customsat_callback('lowerparam_callback', gcb)</code></td></tr></tbody></table>

The MATLAB S-Function script [`custom_sat_final.m`](<matlab:edit('custom_sat_final.m');>) contains the mask parameter callbacks. Find `custom_sat_final.m` in your working folder to define the callbacks in this example. This MATLAB script has two input arguments. The first input argument is a character vector indicating which mask parameter invoked the callback. The second input argument is the handle to the associated Level-2 MATLAB S-Function block.

The figure shows the completed **Parameters & Dialog** pane in the Mask Editor.

![](https://www.mathworks.com/help/simulink/ug/saturation_lib2_maskeditor.png)
```

5. In the **Initialization** pane, select the **Allow library block to modify its contents** check box. This setting allows the S-function to change the number of ports on the block.

> 在**初始化**面板中，选中**允许库块修改其内容**复选框。此设置允许 S-函数更改块的端口数。 6. In the **Documentation** pane:

```
- In the **Mask type** field, enter
- In the **Mask description** field, enter

  ```
  Limit the input signal to an upper and lower saturation value
  set either through a block parameter or input signal.

  ```
```

7. Click **OK**.
8. To map the S-function parameters to the mask parameters, right-click the Level-2 MATLAB S-Function block and select > .

> 右键单击 Level-2 MATLAB S-Function 块，然后选择 >，以将 S-function 参数映射到掩码参数。

9. Change the **S-function name** field to `custom_sat_final` and the **Parameters** field to `lowMode,lowVal,upMode,upVal`.

> 将 **S-函数名称**字段更改为 `custom_sat_final`，将**参数**字段更改为 `lowMode,lowVal,upMode,upVal`。

```
The figure shows the Function Block Parameters dialog box after the changes.

![](https://www.mathworks.com/help/simulink/ug/saturation_lib2_undermask.png)
```

10. Click **OK**. Save and close the library to exit the edit mode.
11. Reopen the library and double-click the customized saturation block to open the masked parameter dialog box.

> 重新打开图书馆，双击自定义饱和度块以打开被遮罩的参数对话框。

```
![](https://www.mathworks.com/help/simulink/ug/saturation_lib2_dialog.png)
```

To create a more complicated user interface, place a MATLAB graphics user interface on top of the masked block. The block `OpenFcn` invokes the MATLAB graphics user interface, which uses calls to `set_param` to modify the S-function block parameters based on settings in the user interface.

> 在屏蔽块上面放置一个 MATLAB 图形用户界面，以创建更复杂的用户界面。块 `OpenFcn` 调用 MATLAB 图形用户界面，该界面使用对 `set_param` 的调用来根据用户界面中的设置修改 S-函数块参数。

#### Writing the Mask Callback

The function [`customsat_callback.m`](matlab:edit('customsat_callback.m');) contains the mask callback code for the custom saturation block mask parameter dialog box. This function invokes local functions corresponding to each mask parameter through a call to `feval`.

> `customsat_callback.m` 文件包含自定义饱和度模块蒙版参数对话框的蒙版回调代码。该函数通过调用 `feval` 来调用与每个蒙版参数对应的局部函数。

The following local function controls the visibility of the upper saturation limit's field based on the selection for the upper saturation limit's mode. The callback begins by obtaining values for all mask parameters using a call to `get_param` with the property name `MaskValues`. If the callback needed the value of only one mask parameter, it could call `get_param` with the specific mask parameter name, for example, `get_param(block,'upMode')`. Because this example needs two of the mask parameter values, it uses the `MaskValues` property to reduce the calls to `get_param`.

> 以下本地函数控制上饱和限制字段的可见性，根据上饱和限制模式的选择。回调首先使用调用 `get_param` 获取所有掩码参数的值，其属性名称为 `MaskValues`。如果回调只需要一个掩码参数的值，它可以使用特定的掩码参数名称调用 `get_param`，例如 `get_param（block，'upMode'）`。由于此示例需要两个掩码参数值，因此使用 `MaskValues` 属性来减少对 `get_param` 的调用。

The callback then obtains the visibilities of the mask parameters using a call to `get_param` with the property name `MaskVisbilities`. This call returns a cell array of character vectors indicating the visibility of each mask parameter. The callback alters the values for the mask visibilities based on the selection for the upper saturation limit's mode and then updates the port label text.

> 回调函数然后使用名为 `MaskVisbilities` 的属性调用 `get_param` 来获取掩码参数的可见性。此调用返回一个字符向量单元数组，指示每个掩码参数的可见性。回调函数根据上限饱和限制模式的选择更改掩码可见性的值，然后更新端口标签文本。

The callback finally uses the `set_param` command to update the block's `MaskDisplay` property to label the block's input ports.

> 回调最终使用 `set_param` 命令更新块的 `MaskDisplay` 属性来标记块的输入端口。

```
function customsat_callback(action,block)
% CUSTOMSAT_CALLBACK contains callbacks for custom saturation block

%   Copyright 2003-2007 The MathWorks, Inc.

%% Use function handle to call appropriate callback
feval(action,block)

%% Upper bound callback
function upperbound_callback(block)

vals = get_param(block,'MaskValues');
vis = get_param(block,'MaskVisibilities');
portStr = {'port_label(''input'',1,''uSig'')'};
switch vals{1}
    case 'No limit'
        set_param(block,'MaskVisibilities',[vis(1);{'off'};vis(3:4)]);
    case 'Enter limit as parameter'
        set_param(block,'MaskVisibilities',[vis(1);{'on'};vis(3:4)]);
    case 'Limit using input signal'
        set_param(block,'MaskVisibilities',[vis(1);{'off'};vis(3:4)]);
        portStr = [portStr;{'port_label(''input'',2,''up'')'}];
end
if strcmp(vals{3},'Limit using input signal'),
    portStr = [portStr;{['port_label(''input'',',num2str(length(portStr)+1), ...
        ',''low'')']}];
end
set_param(block,'MaskDisplay',char(portStr));


```

The final call to `set_param` invokes the `setup` function in the MATLAB S-function `custom_sat.m`. Therefore, the `setup` function can be modified to set the number of input ports based on the mask parameter values instead of on the S-function parameter values. This change to the `setup` function keeps the number of ports on the Level-2 MATLAB S-Function block consistent with the values shown in the mask parameter dialog box.

> 最后一次调用 `set_param` 函数调用 MATLAB S-function `custom_sat.m` 中的 `setup` 函数。因此，可以修改 `setup` 函数，根据掩码参数值而不是 S-function 参数值来设置输入端口的数量。这种对 `setup` 函数的更改使得 Level-2 MATLAB S-Function 块中的端口数量与掩码参数对话框中显示的值保持一致。

The modified MATLAB S-function [`custom_sat_final.m`](matlab:edit('custom_sat_final.m');) contains the following new `setup` function. If you are stepping through this tutorial, open the file.

> 修改后的 MATLAB S-函数[`custom_sat_final.m`](matlab:edit('custom_sat_final.m');)包含以下新的 `setup` 函数。如果您正在按照本教程进行，请打开该文件。

```
%% Function: setup ===================================================
function setup(block)

% Register original number of ports based on settings in Mask Dialog
ud = getPortVisibility(block);
numInPorts = 1 + isequal(ud(1),3) + isequal(ud(2),3);

block.NumInputPorts = numInPorts;
block.NumOutputPorts = 1;

% Setup port properties to be inherited or dynamic
block.SetPreCompInpPortInfoToDynamic;
block.SetPreCompOutPortInfoToDynamic;

% Override input port properties
block.InputPort(1).DatatypeID  = 0;  % double
block.InputPort(1).Complexity  = 'Real';

% Override output port properties
block.OutputPort(1).DatatypeID  = 0; % double
block.OutputPort(1).Complexity  = 'Real';

% Register parameters. In order:
% -- If the upper bound is off (1) or on and set via a block parameter (2)
%    or input signal (3)
% -- The upper limit value. Should be empty if the upper limit is off or
%    set via an input signal
% -- If the lower bound is off (1) or on and set via a block parameter (2)
%    or input signal (3)
% -- The lower limit value. Should be empty if the lower limit is off or
%    set via an input signal
block.NumDialogPrms     = 4;
block.DialogPrmsTunable = {'Nontunable','Tunable','Nontunable','Tunable'};

% Register continuous sample times [0 offset]
block.SampleTimes = [0 0];

%% -----------------------------------------------------------------
%% Options
%% -----------------------------------------------------------------
% Specify if Accelerator should use TLC or call back into
% MATLAB script
block.SetAccelRunOnTLC(false);

%% -----------------------------------------------------------------
%% Register methods called during update diagram/compilation
%% -----------------------------------------------------------------

block.RegBlockMethod('CheckParameters',      @CheckPrms);
block.RegBlockMethod('ProcessParameters',    @ProcessPrms);
block.RegBlockMethod('PostPropagationSetup', @DoPostPropSetup);
block.RegBlockMethod('Outputs',              @Outputs);
block.RegBlockMethod('Terminate',            @Terminate);
%endfunction

```

The `getPortVisibility` local function in `custom_sat_final.m` uses the saturation limit modes to construct a flag that is passed back to the `setup` function. The `setup` function uses this flag to determine the necessary number of input ports.

> `custom_sat_final.m` 中的本地函数 `getPortVisibility` 使用饱和限制模式构建一个标志，该标志被传回 `setup` 函数。 `setup` 函数使用此标志来确定所需的输入端口数。

```
%% Function: Get Port Visibilities =======================================
function ud = getPortVisibility(block)

ud = [0 0];

vals = get_param(block.BlockHandle,'MaskValues');
switch vals{1}
    case 'No limit'
        ud(2) = 1;
    case 'Enter limit as parameter'
        ud(2) = 2;
    case 'Limit using input signal'
        ud(2) = 3;
end

switch vals{3}
    case 'No limit'
        ud(1) = 1;
    case 'Enter limit as parameter'
        ud(1) = 2;
    case 'Limit using input signal'
        ud(1) = 3;
end

```

### Adding Block Functionality Using Block Callbacks

The User-Defined Saturation with Plotting block in `ex_customsat_lib` uses block callbacks to add functionality to the original custom saturation block. This block provides an option to plot the saturation limits when the simulation ends. The following steps show how to modify the original custom saturation block to create this new block.

> 该 `ex_customsat_lib` 中的用户定义饱和度绘图块使用块回调来为原始自定义饱和度块添加功能。该块提供了一个选项，可在仿真结束时绘制饱和度限制。以下步骤演示了如何修改原始自定义饱和度块以创建此新块。

1. Add a check box to the mask parameter dialog box to toggle the plotting option on and off.

> 在掩码参数对话框中添加一个复选框，以开启和关闭绘图选项。

```
1.  Right-click the Level-2 MATLAB S-Function block in `saturation_lib` and select + **Create Mask**.
2.  On the Mask Editor **Parameters** pane, add a fifth mask parameter with the following properties.

    <table><colgroup><col width="12%"><col width="12%"><col width="11%"><col width="10%"><col width="15%"><col width="40%"></colgroup><thead><tr><th>Prompt</th><th>Name</th><th>Type</th><th>Tunable</th><th>Type options</th><th>Callback</th></tr></thead><tbody><tr><td>Plot saturation limits</td><td><code>plotcheck</code></td><td><code>checkbox</code></td><td>No</td><td>NA</td><td><code>customsat_callback('plotsaturation',gcb)</code></td></tr></tbody></table>

3.  Click **OK**.

![](https://www.mathworks.com/help/simulink/ug/saturation_lib3_blockparameters.png)
```

2. Write a callback for the new check box. The callback initializes a structure to store the saturation limit values during simulation in the Level-2 MATLAB S-Function block `UserData`. The MATLAB script `[customsat_plotcallback.m](matlab:edit('customsat_plotcallback.m');)` contains this new callback, as well as modified versions of the previous callbacks to handle the new mask parameter. If you are following through this example, open `[customsat_plotcallback.m](matlab:edit('customsat_plotcallback.m');)` and copy its local functions over the previous local functions in `customsat_callback.m`.

> 写一个新复选框的回调函数。回调函数在 Level-2 MATLAB S-Function 块 UserData 中初始化一个结构来存储模拟期间的饱和限制值。MATLAB 脚本 customsat_plotcallback.m 包含这个新回调函数，以及处理新掩码参数的先前回调函数的修改版本。如果你正在按照这个例子进行，打开 customsat_plotcallback.m 并将其本地函数复制到 customsat_callback.m 中的先前本地函数上。

```
```
%% Plotting checkbox callback
function plotsaturation(block)

% Reinitialize the block's userdata
vals = get_param(block,'MaskValues');
ud = struct('time',[],'upBound',[],'upVal',[],'lowBound',[],'lowVal',[]);

if strcmp(vals{1},'No limit'),
    ud.upBound = 'off';
else
    ud.upBound = 'on';
end

if strcmp(vals{3},'No limit'),
    ud.lowBound = 'off';
else
    ud.lowBound = 'on';
end

set_param(gcb,'UserData',ud);

```
```

3. Update the MATLAB S-function `Outputs` method to store the saturation limits, if applicable, as done in the new MATLAB S-function `[custom_sat_plot.m](matlab:edit('custom_sat_plot.m');)`. If you are following through this example, copy the `Outputs` method in `custom_sat_plot.m` over the original `Outputs` method in `custom_sat_final.m`

> 更新 MATLAB S-函数的“输出”方法，以存储适用的饱和限制，就像在新的 MATLAB S-函数[custom_sat_plot.m]（matlab：edit（'custom_sat_plot.m'）中所做的那样。如果您正在按照本示例操作，请将“输出”方法中的“custom_sat_plot.m”复制到“custom_sat_final.m”中的原始“输出”方法中。

```
```
%% Function: Outputs ===================================================
function Outputs(block)

lowMode    = block.DialogPrm(1).Data;
upMode     = block.DialogPrm(3).Data;
sigVal     = block.InputPort(1).Data;
vals = get_param(block.BlockHandle,'MaskValues');
plotFlag = vals{5};
lowPortNum = 2;

% Check upper saturation limit
if isequal(upMode,2)
    upVal = block.RuntimePrm(2).Data;
elseif isequal(upMode,3)
    upVal = block.InputPort(2).Data;
    lowPortNum = 3; % Move lower boundary down one port number
else
    upVal = inf;
end

% Check lower saturation limit
if isequal(lowMode,2),
    lowVal = block.RuntimePrm(1).Data;
elseif isequal(lowMode,3)
    lowVal = block.InputPort(lowPortNum).Data;
else
    lowVal = -inf;
end

% Use userdata to store limits, if plotFlag is on
if strcmp(plotFlag,'on');
    ud = get_param(block.BlockHandle,'UserData');
    ud.lowVal = [ud.lowVal;lowVal];
    ud.upVal = [ud.upVal;upVal];
    ud.time = [ud.time;block.CurrentTime];
    set_param(block.BlockHandle,'UserData',ud)
end

% Assign new value to signal
if sigVal > upVal,
    sigVal = upVal;
elseif sigVal < lowVal,
    sigVal=lowVal;
end

block.OutputPort(1).Data = sigVal;

%endfunction

```
```

4. Write the function `[plotsat.m](matlab:edit('plotsat.m');)` to plot the saturation limits. This function takes the handle to the Level-2 MATLAB S-Function block and uses this handle to retrieve the block's `UserData`. If you are following through this tutorial, save `plotsat.m` to your working folder.

> 请编写函数 `[plotsat.m](matlab:edit('plotsat.m');)` 来绘制饱和限制。 该函数使用此句柄检索块的 `UserData`。 如果您正在按照本教程进行，请将 `plotsat.m` 保存到您的工作文件夹中。

```
```
function plotSat(block)

% PLOTSAT contains the plotting routine for custom_sat_plot
%   This routine is called by the S-function block's StopFcn.

ud = get_param(block,'UserData');
fig=[];
if ~isempty(ud.time)
    if strcmp(ud.upBound,'on')
        fig = figure;
        plot(ud.time,ud.upVal,'r');
        hold on
    end
    if strcmp(ud.lowBound,'on')
        if isempty(fig),
            fig = figure;
        end
        plot(ud.time,ud.lowVal,'b');
    end
    if ~isempty(fig)
        title('Upper bound in red. Lower bound in blue.')
    end

    % Reinitialize userdata
    ud.upVal=[];
    ud.lowVal=[];
    ud.time = [];
    set_param(block,'UserData',ud);
end

```
```

5. Right-click the Level-2 MATLAB S-Function block and select . The Block Properties dialog box opens. On the **Callbacks** pane, modify the `StopFcn` to call the plotting callback as shown in the following figure, then click **OK**.

> 右键单击 Level-2 MATLAB S-Function 块，然后选择。Block Properties 对话框打开。在**回调**窗格中，将 `StopFcn` 修改为如下图所示调用绘图回调，然后单击**确定**。

```
![](https://www.mathworks.com/help/simulink/ug/saturation_lib5_blockproperties.png)
```

## Related Topics

- [Types of Custom Blocks](https://www.mathworks.com/help/simulink/ug/create-your-own-simulink-block.html)
- [Comparison of Custom Block Functionality](https://www.mathworks.com/help/simulink/ug/comparison-of-custom-block-functionality.html)
