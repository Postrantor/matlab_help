---
url: https://ww2.mathworks.cn/help/simulink/sfg/s-function-builder-dialog-box.html
title: Build S-Functions Automatically Using S-Function Builder - MATLAB & Simulink - MathWorks 中国 --- 使用 S-Function Builder 自动构建 S-Function - MATLAB & Simulink - MathWorks 中国
date: 2023-07-31 16:27:01
tag: 
summary: Use S-Function Builder to automatically generate S-functions from C code.
---
## Build S-Functions Automatically Using S-Function Builder

The S-Function Builder is a Simulink® block that integrates C/C++ code to build an S-function from specifications and C code that you supply. The S-Function Builder block also serves as a wrapper for the generated S-function in models that use the S-function.

### Create an S-Function Builder Block and Specify Settings

To add the S-Function Builder block to a model, click the Simulink canvas and type `S-Function Builder` or drag a S-Function Builder block from > . To open the S-Function Builder editor, double-click the S-Function Builder block icon or select the block. Then select **Open Block** from the **Edit** menu on the model editor or the block's context menu.

![](https://ww2.mathworks.cn/help/simulink/sfg/sfb_empty_new22b.png)

S-Function Builder editor is composed of four user interface components:

* S-Function Builder toolstrip
* Settings pane
* Ports and Parameters Table
* Libraries table

To start constructing your S-function, specify a name for the S-function. When you name the S-function, all functions in the S-Function Builder editor change, and the S-function name becomes a prefix to all wrapper functions.

![](https://ww2.mathworks.cn/help/simulink/sfg/sfb_portsandparams_new22b.png)

Use the S-Function Builder toolstrip to specify a target language for the S-function:

* **C** — Generate C S-functions.
* **C++** — Generate C++ S-functions.
* **Inherit from model** — Inherit the language from the **[Language](https://ww2.mathworks.cn/help/rtw/ref/language.html) (Simulink Coder)** configuration parameter for the model.

Use the **Settings** pane on the right of the S-Function Builder editor to specify the basic features of the S-function, such as initial conditions, and number of states. The S-Function Builder block uses the settings to generate the [`mdlInitializeSizes`](https://ww2.mathworks.cn/help/simulink/sfg/mdlinitializesizes.html) callback method. The Simulink engine invokes this method during the model initialization phase of the simulation to obtain basic information about the S-function.

![](https://ww2.mathworks.cn/help/simulink/sfg/sfb_settings_new22b.png)

#### Set Number of States and Initial Conditions

Using the **Settings** pane, you can specify the number of discrete and continuous states and their initial conditions.

You can enter the values of initial conditions as a comma-separated list, (for example, `1,1,1`) or as a vector (for example, `[1 1 1]`). The number of initial conditions must be equal to the number of states indicated.

#### Set Array Layout

Set the array layout of your C/C++ code. You can select one of the options listed in the table.

<table><colgroup><col width="26%"><col width="26%"><col width="48%"></colgroup><thead><tr><th>Option</th><th>Array Layout of C/C++ Function</th><th>Action</th></tr></thead><tbody><tr><td><code>Column-major</code></td><td>Column-major</td><td><p>The <span>S-Function Builder</span> block adds the SimStruct function <a href="https://ww2.mathworks.cn/help/simulink/sfg/sssetarraylayoutforcodegen.html"><code>ssSetArrayLayoutForCodeGen</code></a> in <code>mdlInitializeSizes</code> to mark the S-function for column-major code generation.</p></td></tr><tr><td><code>Row-major</code></td><td>Row-major</td><td><p>The <span>S-Function Builder</span> block adds the SimStruct function <a href="https://ww2.mathworks.cn/help/simulink/sfg/sssetarraylayoutforcodegen.html"><code>ssSetArrayLayoutForCodeGen</code></a> in <code>mdlInitializeSizes</code> to mark the S-function for row-major code generation.</p><p>During simulation, if your C/C++ code involves matrices or multidimensional inputs, outputs, or parameters, transposes are added to these S-function callback methods:</p><div><ul><li><p><code>mdlOutputs</code></p></li><li><p><code>mdlUpdate</code></p></li><li><p><code>mdlDerivatives</code></p></li></ul></div><p>Simulink also applies the these transposes when running accelerator and rapid accelerator simulations. The S-function is not inlined by using TLC. Instead, the MEX S-function with transposes is compiled directly.</p></td></tr><tr><td><code>Any</code></td><td>C/C++ function is not affected by array layout</td><td><p>The <span>S-Function Builder</span> block adds the SimStruct function <a href="https://ww2.mathworks.cn/help/simulink/sfg/sssetarraylayoutforcodegen.html"><code>ssSetArrayLayoutForCodeGen</code></a> in <code>mdlInitializeSizes</code> to mark the S-function as accepting both row-major and column-major code generation.</p></td></tr></tbody></table>

#### Select Sample Mode

Select the sample mode of your S-function. The sample mode determines the length of the interval between the times when the S-function updates its output. You can select one of these options:

* `Inherited` — The S-function inherits the sample mode from the block connected to the input port. When inherited sample mode is selected, the **Sample time value** field of the **Settings** table is inactive.
* `Continuous` — The block updates output values at each simulation step. When continuous sample time is selected, the **Sample time value** field of the **Settings** table is inactive.
* `Discrete` — The S-function updates output values at the rate specified in the **Sample time value** field of the **Settings** table.

#### Set the Number of `PWorks`

Set `PWorks`, the number of data pointers used by the S-function. `PWorks` points to the memory over the lifecycle of the block. If you enter any other value than `0` for **Number of PWorks**, it adds a pointer, `void **PW`, to all functions in S-Function Builder. For example, you can declare and initialize a pointer to a file or memory at the `Start_wrapper`, access it in `Outputs_wrapper`, `Update_wrapper`, and `Derivatives_wrapper` functions, and deallocate it at the `Terminate_wrapper` function. The code written in these functions is called by the `mdlStart`, `mdlOutputs`, `mdlUpdate`, `mdlDerivatives`, and `mdlTerminate` methods. See the examples [Moving Average with Start and Terminate](matlab:open_system('sfbuilder_movingAverage')) and [Permutation using Cpp Classes](matlab:open_system('sfbuilder_permutation')) in [S-Function Demos](matlab:open_system('sfundemos')).

**Note**

To use `PWorks` and model operating point in the same S-function, specify the operating point compliance as `USE_CUSTOM_OPERATING_POINT`.

When you use `PWorks` without explicitly specifying the operating point compliance, the compliance becomes `DISALLOW_OPERATING_POINT` after compilation. For more information, see [S-Function Compliance with the ModelOperatingPoint](https://ww2.mathworks.cn/help/simulink/sfg/s-function-compliance-with-the-simstate.html).

#### Enable Access to SimStruct

Make the `SimStruct` (`S`) accessible to the wrapper functions that the S-Function Builder is using. This selection enables you to use the `SimStruct` macros and functions with your code in the `Outputs_wrapper`, `Derivatives_wrapper`, and the `Update_wrapper` functions. When you turn this setting on, `SimStruct *S` is added to the list of parameters.

For example, with this option enabled, you can use macros such as [`ssGetT`](https://ww2.mathworks.cn/help/simulink/sfg/ssgett.html) in code that computes the S-function outputs:

<table><colgroup><col width="100%"></colgroup><tbody><tr><td><div><pre>double t = ssGetT(S);
  if(t &lt; 2) 
  {
    y0[0] = u0[0];
  } else 
  {
    y0[0]= 0.0;
  }

</pre></div></td></tr></tbody></table>

#### Direct Feedthrough

If the values from the current time step of the S-function inputs are used to compute the outputs, then on the **Settings** table, select **Direct Feedthrough**. The Simulink engine uses this information to detect algebraic loops created by directly or indirectly connecting the S-function output to the S-function input.

#### Multi-instance Support

To declare that multiple execution instances of the generated S-function can operate with each other and that the function is configured to be used within the [For Each Subsystem](https://ww2.mathworks.cn/help/simulink/slref/foreachsubsystem.html), on the **Settings** table, select **Multi-instance Support**. For more information on configuring an S-Function for Multi-instance support, see S-Function Support under [For Each Subsystem](https://ww2.mathworks.cn/help/simulink/slref/foreachsubsystem.html).

#### Multithreaded Execution Support

To declare that the generated S-function can run multithreaded, on the **Settings** table, select **Multithreaded Execution Support**.

#### Code Reuse Support

To declare that the generated S-function meets the requirements for subsystem code reuse, on the **Settings** table, select **Code Reuse Support**. For more information on configuring an S-function for code reuse, see [S-Functions for Code Reuse](https://ww2.mathworks.cn/help/rtw/ug/s-functions-for-code-reuse.html) (Simulink Coder).

### Specify Ports and Parameters for the S-Function

Use the **Ports and Parameters** table on the bottom of the editor to specify input and output ports and parameters for the S-function. To add a port or parameter:

* Select the **Ports and Parameters** table, and from the S-Function Builder toolstrip, select your choice under `Insert Port`.
* Select the **Ports and Parameters** table and right-click one of the headers on the table.

To delete a port or parameter in the table, select the port or parameter you would like to delete, right-click to open the menu and click **Delete**.

The order of the ports and parameters in the table is the order of ports and parameters on the block. For example, the first input port on the table is the input port with the index `0`, and the first parameter is the parameter with index `0`.

You can define these settings in the **Ports and Parameters** table.

1. **Name** — Name of port or parameter. To change the name of a port or parameter, double-click the name.
2. **Scope** — Scope of port or parameter. The variable could be an input or output port or a parameter. Click the scope value to change scope of the port or parameter.
3. **Data type** — Data type of port or parameter. You can specify Simulink built-in numeric data types for ports and parameters. You can also specify `fixdt` data types and bus types for ports. The `int64` and `uint64` data types are not supported for parameters.
4. **Dimensions** — Dimension of port or parameter. To inherit dimensions for ports, specify the dimension as `-1`. For parameters, the dimension is set to `-1` and is inherited from the block parameters on the block interface.

   To specify a 1-D dimension, only enter the number of rows for the dimension, for example, `[2]`. To enter an N-D dimension, enter the dimension as a vector with elements specifying size of each dimension. Dynamically sized ports are not supported for the third or higher dimensions
5. **Complexity** — Specify the complexity of the port or parameter as real or complex.

### Use the Libraries Table to Specify External Code and Paths

The **Libraries** table allows you to specify the location of external code files referenced by custom code that you enter in other the wrapper methods of the S-Function Builder editor.

To enter a new path or an entry, select the **Libraries** table and select one of the options under `Insert Paths` on the S-Function Builder toolstrip. Alternatively, click one of the tags or values on the **Libraries** table. Once you select path or entry, you can change your selection by clicking the tag on the table. You can enter paths or entries to the external libraries, object codes and source files referenced by custom code on the S-Function Builder editor.

<table><colgroup><col width="50%"><col width="50%"></colgroup><thead><tr><th>Tag to choose</th><th>Purpose</th></tr></thead><tbody><tr><td><code>LIB_PATH</code></td><td>Specify object and library paths.</td></tr><tr><td><code>INC_PATH</code></td><td>Specify the include search paths for header files and source files.</td></tr><tr><td><code>SRC_PATH</code></td><td>Specify search paths for object files and source files.</td></tr><tr><td><code>ENV_PATH</code></td><td>Specify environment variables.</td></tr><tr><td><code>ENTRY</code></td><td><p>Specify the object, source, and library file names. You can also enter preprocessor directives in this field, for example, <code>-DDEBUG</code>, <code>-DHARDWARE_VARIANT=1</code> or <code>-UDEBUG</code>. Specify each file on a separate line.</p></td></tr></tbody></table>

Enclose the names of the header files on the top of the code editor. If you are using custom header files that are not on the same path, include the directory with the `INC_PATH` tag. Similarly, use the top of the editor to declare external functions that are not declared in the header files. Include each declaration on a separate line.

You can enter paths or entries for to the external libraries, object codes, and source files referenced by custom code on the S-Function Builder editor.

For example, consider an S-Function Builder project in `C:\Program Files\MATLAB\work`. The table shows on how to link to external files:

<table><colgroup><col width="50%"><col width="50%"></colgroup><thead><tr><th>File Locations</th><th>Entries to the Libraries Table</th></tr></thead><tbody><tr><td><p><code>c:\customfolder\customfunctions.lib</code></p></td><td><p><code>LIB_PATH c:\customfolder</code></p><p><code>ENTRY customfunctions.lib</code></p></td></tr><tr><td><code>C:\Program Files\MATLAB\work\customobjs\userfunctions.obj</code></td><td><p><code>LIB_PATH $MATLABROOT\customobjs</code></p><p><code>ENTRY userfunctions.obj</code></p></td></tr><tr><td><p><code>D:\externalsource\freesource.c</code></p></td><td><p><code>SRC_PATH D:\externalsource</code></p><p><code>ENTRY freesource.c</code></p></td></tr></tbody></table>

You can use `LIB_PATH` to specify both object and library file paths. You can include the library name in the `LIB_PATH` declaration. However, you must place the object file name on a separate line. The tag `$MATLABROOT` indicates a path relative to the MATLAB® installation. List multiple `LIB_PATH` entries on separate lines. The paths are searched in the order you specify.

Each directive must be listed on a separate line.

**Note**

Do not put quotation marks around the library path, even if the path name has spaces in it. If you add quotation marks, the compiler will not find the library.

### Develop the S-Function

The S-Function Builder block uses wrapper methods to specify the S-function code and properties that generate the corresponding S-function. Use the appropriate wrapper methods to construct the S-function body. Click **Apply** on the toolstrip to generate the S-function code.

This table provides a summary of S-Function Builder methods:

<table><colgroup><col width="33%"><col width="33%"><col width="33%"></colgroup><thead><tr><th>S-Function Method</th><th>Wrapper Method</th><th>Purpose</th></tr></thead><tbody><tr><td>Start and Terminate</td><td><p><code>&lt;system_name&gt;_Start_wrapper</code></p><p><code>&lt;system_name&gt;_Terminate_wrapper</code></p></td><td>Allocate and deallocate memory at the start and the end of simulation. Allocated memory is referenced using <code>PWorks</code> for use throughout the S-Function.</td></tr><tr><td>Outputs</td><td><p><code>&lt;system_name&gt;_Outputs_wrapper</code></p></td><td>Enter the code that computes the outputs of the S-function at each simulation time step.</td></tr><tr><td>Update</td><td><p><code>&lt;system_name&gt;_Update_wrapper</code></p></td><td>Enter the code that computes the value of discrete states at the next time step using the values at the current time step. This method only exists when you specify <strong>Number of Discrete States</strong> on the <strong>Settings</strong> table.</td></tr><tr><td>Derivatives</td><td><p><code>&lt;system_name&gt;_Derivatives_wrapper</code></p></td><td>Enter the code to compute state derivatives. This method only exists when you specify <strong>Number of Continuous States</strong> on the <strong>Settings</strong> table.</td></tr></tbody></table>

#### Compute Outputs Using Outputs Method

In the S-Function Builder, use the `Outputs_wrapper` method to enter the code that computes the outputs of the S-function at each simulation time step. When generating the S-function code, the S-Function Builder block generates a wrapper method using the name of your model and a wrapper function in the form of `<system_name>_<function_name>_wrapper`. For example, if your model is named `dsfunc_builder`, the output method appears as `dsfunc_builder_Output_wrapper` in the S-Function Builder editor. If you do not have a name for your S-Function Builder block yet, it appears as `system_Output_wrapper`.

See the following code for an example of the `Outputs` method:

<table><colgroup><col width="100%"></colgroup><tbody><tr><td><div><pre>void sfun_Outputs_wrapper(const real_T *u,
                          real_T       *y,
				const real_T *xD, /* optional */
				const real_T *xC, /* optional */
				const real_T  *param0, /* optional */
				int_T p_width0 /* optional */
				real_T  *param1 /* optional */
				int_t p_width1 /* optional */
				int_T y_width, /* optional */
				int_T u_width) /* optional */
{

/* Your code inserted here */
}

</pre></div></td></tr></tbody></table>

where `sfun` is the name of the S-function. The S-Function Builder inserts a call to this wrapper function in the [`mdlOutputs`](https://ww2.mathworks.cn/help/simulink/sfg/mdloutputs.html) callback method that it generates for your S-function. The Simulink engine invokes the `mdlOutputs` method at each simulation or sample time step to compute the S-function output. The `mdlOutputs` method in turn invokes the wrapper function containing your output code. Your output code then computes and returns the S-function output.

The `mdlOutputs` method passes some or all of the following arguments to the outputs wrapper function.

<table><colgroup><col width="28%"><col width="72%"></colgroup><thead><tr><th>Argument</th><th>Description</th></tr></thead><tbody><tr><td><code>u0, u1, ... uN</code></td><td>Pointers to arrays that contain the inputs to the S-function, where <code>N</code> is the number of input ports specified on the <strong>Input</strong> scope found on the <strong>Ports and Parameters</strong> table. The names of the arguments that appear in the outputs wrapper function are the same as the names found on the <strong>Input</strong> scope of the <strong>Ports and Parameters</strong> table. The width of each array is the same as the input width specified for each input on the <strong>Dimensions</strong> pane. If the input is a matrix, the width equals the product of the dimensions of the arrays. If you specify -1 as an input width, the width of the array is specified by the wrapper function's <code>u_width</code> argument.</td></tr><tr><td><code>y0, y1, ... yN</code></td><td>Pointer to arrays that contain the outputs of the S-function, where <code>N</code> is the number of output ports specified on the <strong>Scope</strong> pane on the <strong>Ports and Parameters</strong> table. The names of the arguments that appear in the outputs wrapper function are the same as the names found on the <strong>Output</strong> scope of <strong>Ports and Parameters</strong> table. The width of each array is the same as the output width specified for each output on the <strong>Dimensions</strong> pane. If an output is a matrix, the width equals the product of the dimensions of the arrays. If you specified -1 as the output width, the width of the array is specified by the wrapper function's <code>y_width</code> argument. Use this array to pass the outputs that your code computes back to the Simulink engine.</td></tr><tr><td><code>xD</code></td><td>Pointer to an array that contains the discrete states of the S-function. This argument appears only if you specify discrete states on the <strong>Number of discrete states</strong> in the <strong>Settings</strong> menu. At the first simulation time step, the discrete states have the initial values that you specify on the <strong>Discrete states IC</strong>. At subsequent sample-time steps, the states are obtained from the values that the S-function computes at the preceding time step.</td></tr><tr><td><code>xC</code></td><td>Pointer to an array that contains the continuous states of the S-function. This argument appears only if you specify number of continuous states on the <strong>Number of continuous states</strong> row of the <strong>Settings</strong> menu. At the first simulation time step, the continuous states have the initial values that you specify on the <strong>Continuous states IC</strong> row. At subsequent time steps, the states are obtained by numerically integrating the derivatives of the states at the preceding time step.</td></tr><tr><td><code>param0</code>, <code>p_width0</code>, <code>param1</code>, <code>p_width1</code>, ... <code>paramN</code>, <code>p_widthN</code></td><td><code>param0</code>, <code>param1</code>, ...<code>paramN</code> are pointers to arrays that contain the S-function parameters, where <code>N</code> is the number of parameters specify on the <strong>Ports and Parameters</strong> table. <code>p_width0</code>, <code>p_width1</code>, ...<code>p_widthN</code> are the widths of the parameter arrays. If a parameter is a matrix, the width equals the product of the dimensions of the arrays. For example, the width of a 3-by-2 matrix parameter is 6.</td></tr><tr><td><code>y_width</code></td><td>Width of the array that contains the S-function outputs. This argument appears in the generated code only if you specify <code>-1</code> as the width of the S-function output. If the output is a matrix, <code>y_width</code> is the product of the dimensions of the matrix.</td></tr><tr><td><code>u_width</code></td><td>Width of the array that contains the S-function inputs. This argument appears in the generated code only if you specify <code>-1</code> as the width of the S-function input. If the input is a matrix, <code>u_width</code> is the product of the dimensions of the matrix.</td></tr></tbody></table>

These arguments permit you to compute the output of the block as a function of its inputs and, optionally, its states and parameters. The code that you enter in this field can invoke external functions declared in the header files or external declarations on the **Libraries** table, which allows you to use existing code to compute the outputs of the S-function.

#### Update Discrete States Using Update Method

Use the `Update_wrapper` function to enter code that computes the values of the discrete states at the next time step according to the values at the current time step. See the following code:

```
void sfun_Update_wrapper(const real_T *u,
                         const real_T *y,
                         real_T *xD,
                         const real_T  *param0,  /* optional */
                         int_T p_width0, /* optional */
                         real_T  *param1,/* optional */
                         int_T p_width1, /* optional */
                         int_T y_width, /* optional */
                         int_T u_width) /* optional */
{

	/* Your code inserted here. */

}


```

where `sfun` is the name of the S-function. The S-Function Builder block inserts a call to this wrapper function in the [`mdlUpdate`](https://ww2.mathworks.cn/help/simulink/sfg/mdlupdate.html) callback method that it generates for the S-function. The Simulink engine calls the `mdlUpdate` method at the end of each time step to obtain the values of the discrete states at the next time step (see [Simulink Engine Interaction with C S-Functions](https://ww2.mathworks.cn/help/simulink/sfg/how-the-simulink-engine-interacts-with-c-s-functions.html)). At the next time step, the engine passes the updated states back to the `mdlOutputs` method.

The `mdlUpdate` callback method generated for the S-function passes the following arguments to the `Update_wrapper` function:

* `u`
* `y`
* `xD`
* `param0`, `p_width0`, `param1`, `p_width1`, ... `paramN`, `p_widthN`
* `y_width`
* `u_width`

See [Compute Outputs Using Outputs Method](https://ww2.mathworks.cn/help/simulink/sfg/s-function-builder-dialog-box.html#mw_23eee386-9d60-4d8e-bf28-f91c5ff7e6eb) for the meanings and usage of these arguments. Your code should use the discrete states variable, `xD`, to return the values of the discrete states that it computes. The arguments allow your code to compute the discrete states as functions of the S-function inputs, outputs, and, optionally, parameters. Your code can invoke external functions declared in the code editor.

#### Compute Continuous States Using Derivatives Method

If the S-function has continuous states, use the `Derivatives_wrapper` function to enter the code required to compute the state derivatives. Enter code for the `mdlDerivatives` function to compute the derivatives of the continuous states editor under this field. An example declaration of the function may look like the following:

<table><colgroup><col width="100%"></colgroup><tbody><tr><td><div><pre>void system_Derivatives_wrapper(const real_T *u,
				      const real_T *y, 
				      real_T *dx,
				      real_T *xC, 
				      const real_T  *param0,  /* optional */
				      int_T p_width0, /* optional */
				      real_T  *param1,/* optional */
	 			     int_T p_width1, /* optional */
				      int_T y_width, /* optional */
		 		     int_T u_width) /* optional */
{

```
/* Your code inserted here. */
```

}

</pre></div></td></tr></tbody></table>

where `system` is the name of the S-function.

The S-Function Builder block inserts a call to this wrapper function in the [`mdlDerivatives`](https://ww2.mathworks.cn/help/simulink/sfg/mdlderivatives.html) method that it generates for the S-function. The Simulink engine calls the `mdlDerivatives` method at the end of each time step to obtain the derivatives of the continuous states (see [Simulink Engine Interaction with C S-Functions](https://ww2.mathworks.cn/help/simulink/sfg/how-the-simulink-engine-interacts-with-c-s-functions.html)). The Simulink solver numerically integrates the derivatives to determine the continuous states at the next time step. At the next time step, the engine passes the updated states back to the `mdlOutputs` method. For more information on how Simulink engine interacts with S-functions, see [Simulink Engine Interaction with C S-Functions](https://ww2.mathworks.cn/help/simulink/sfg/how-the-simulink-engine-interacts-with-c-s-functions.html).

The `mdlDerivatives` method generated for the S-function passes the following arguments to the derivatives wrapper function:

* `u`
* `y`
* `dx`
* `xC`
* `param0`, `p_width0`, `param1`, `p_width1`, ... `paramN`, `p_widthN`
* `y_width`
* `u_width`

The `dx` argument is a pointer to an array whose width is the same as the number of continuous derivatives specified on the **Settings** pane. Your code should use this array to return the values of the derivatives that it computes. See the explanation under [Compute Outputs Using Outputs Method](https://ww2.mathworks.cn/help/simulink/sfg/s-function-builder-dialog-box.html#mw_23eee386-9d60-4d8e-bf28-f91c5ff7e6eb) method for the meanings and usage of the other arguments. The arguments allow your code to compute derivatives as a function of the S-function inputs, outputs, and, optionally, parameters. Your code can invoke external functions declared in the code editor.

#### Allocate and Deallocate Memory Using Start and Terminate Methods

Use the `Start_wrapper` method to write code to allocate memory at the start of simulation. The allocated is referenced by `Pworks` for use throughout the S-function. Similarly, use the `Terminate_wrapper` method to write code to free up the memory allocated at the `Start_wrapper` method. Memory referenced by `PWorks` can also be seen by **Terminate**, and should be deallocated here.

See the [Set the Number of PWorks](https://ww2.mathworks.cn/help/simulink/sfg/s-function-builder-dialog-box.html#mw_82a7451d-74b0-4d96-849f-2ffd7ccfdb77) to learn more about `PWorks`.

### Build the S-Function

After entering the code in the S-Function Builder editor, investigate the options under **Build** menu to build your S-function.

![](https://ww2.mathworks.cn/help/simulink/sfg/build_pane.png)

The build menu contains the following selections:

* — Log each build step in the **Build Log** field.
* — Include debugging information in the generated MEX file.
* — Selecting this option allows you to generate a TLC file. You need to generate a TLC file if you are running your model in rapid accelerator mode or generating Simulink Coder™ code from your model. Also, while it is not necessary for accelerator mode simulations, the TLC file generates code for the S-function and thus makes your model run faster in accelerator mode.
* — Make an S-Function compatible with model coverage. For more information, see [Coverage for Custom C/C++ Code in Simulink Models](https://ww2.mathworks.cn/help/slcoverage/ug/coverage-for-custom-code.html) (Simulink Coverage) in the Simulink Coverage™ documentation.
* — Generate an S-function for use with the Simulink Design Verifier™. For more information, see [Support Limitations for S-Functions and C/C++ Code](https://ww2.mathworks.cn/help/sldv/ug/support-limitations-for-s-functions.html#buzt_1v-4) (Simulink Design Verifier).

When you make your selection, click **Build** to build your S-function and build a MEX-file. To exclude building of a MEX-file from the generated source code, select **Generate Code Only**.

## See Also

[S-Function](https://ww2.mathworks.cn/help/simulink/slref/sfunction.html) | [S-Function Builder](https://ww2.mathworks.cn/help/simulink/slref/sfunctionbuilder.html)

## Related Topics

* [Model a State-Space System Using S-Function Builder](https://ww2.mathworks.cn/help/simulink/sfg/example-modeling-a-two-inputtwo-output-system.html)
* [Use a Bus with S-Function Builder to Create an S-Function](https://ww2.mathworks.cn/help/simulink/sfg/building-s-functions-automatically.html)

How useful was this information?

Unrated

1 star

2 stars

3 stars

4 stars

5 stars

Unrated  1 star  2 stars  3 stars  4 stars  5 stars

您点击的链接对应于以下 MATLAB 命令：

请在 MATLAB 命令行窗口中直接输入以执行命令。Web 浏览器不支持 MATLAB 命令。

Select a Web Site

Choose a web site to get translated content where available and see local events and offers. Based on your location, we recommend that you select: .

You can also select a web site from the following list:

[Contact your local office](#)
