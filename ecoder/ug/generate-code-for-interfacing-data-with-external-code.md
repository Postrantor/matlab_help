---
url: https://ww2.mathworks.cn/help/ecoder/ug/generate-code-for-interfacing-data-with-external-code.html
title: Exchange Data Between External C/C++ Code and Simulink Model or Generated Code
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-18 21:00:20
tag: 
summary: Configure the signals, states, and parameters in a Simulink model to match the data interface of your......
---
## Exchange Data Between External C/C++ Code and Simulink Model or Generated Code

Whether you import your external code into a Simulink® model (for example, by using the Legacy Code Tool) or export the generated code to an external environment, the model or the generated code typically exchange data (signals, states, and parameters) with your code.

Functions in C or C++ code, including your external functions, can exchange data with a caller or a called function through:

*   Arguments (formal parameters) of functions. When a function exchanges data through arguments, an application can call the function multiple times. Each instance of the called function can manipulate its own independent set of data so that the instances do not interfere with each other.
    
*   Direct access to global variables. Global variables can:
    
    *   Enable different algorithms (functions) and instances of the same algorithm to share data such as calibration parameters and error status.
        
    *   Enable the different rates (functions) of a multitasking system to exchange data.
        
    *   Enable different algorithms to exchange data asynchronously.
        
    

In Simulink, you can organize and configure data so that a model uses these exchange mechanisms to provide, extract, and share data with your code.

Before you attempt to match data interfaces, to choose an overall integration approach, see [Choose an External Code Integration Workflow](https://ww2.mathworks.cn/help/ecoder/ug/choose-an-external-code-integration-approach.html).

### Import External Code into Model

To exchange data between your model and your external function, choose an exchange mechanism based on the technique that you chose to integrate the function.

*   To exchange data through the arguments of your external function, construct and configure your model to create and package the data according to the data types of the arguments. Then, you connect and configure the block that calls or represents your function to accept, produce, or refer to the data from the model.
    
    For example, if you use the Legacy Code Tool to generate an S-Function block that calls your function, the ports and parameters of the block correspond to the arguments of the function. You connect the output signals of upstream blocks to the input ports and set parameter values in the block mask. Then, you can create signal lines from the output ports of the block and connect those signals to downstream blocks.
    
*   To exchange data through global variables that your external code already defines, a best practice is to use a Stateflow® chart to call your function and to access the variables. You write algorithmic C code in the chart so that during simulation or execution of the generated code, the model reads and writes to the variables.
    
    To use such a global variable as an item of parameter data (not signal or state data) elsewhere in a model, you can create a numeric MATLAB® variable or `Simulink.Parameter` object that represents the variable. If you change the value of the C-code variable in between simulation runs, you must manually synchronize the value of the Simulink variable or object. If your algorithmic code (function) changes the value of the C-code variable during simulation, the corresponding Simulink variable or object does not change.
    
    If you choose to create a Simulink representation of the C-code variable, you can configure the Simulink representation so that the generated code reads and writes to the variable but does not duplicate the variable definition. Apply a storage class to the Simulink representation.
    

### Export Generated Code to External Environment

To export the generated code into your external code, see [Exchange Data Between External Calling Code and Generated Code](https://ww2.mathworks.cn/help/ecoder/ug/exchange-data-between-external-calling-code-and-generated-code.html).

### Simulink Representations of C Data Types and Constructs

To model and reuse your custom C data such as structures, enumerations, and `typedef` aliases, use the information in these tables.

**Modeling Patterns for Matching Data in External C Code**

<table summary="Modeling Patterns for Matching Data in External C Code"><colgroup><col width="15%"><col width="35%"><col width="28%"><col width="23%"></colgroup><thead><tr><th>C Data Type or Construct</th><th>Example C Code</th><th>Simulink Equivalent</th><th>More Information</th></tr></thead><tbody><tr><td><p>Primitive type alias (<code>typedef</code>)</p></td><td><div><pre>typedef float mySinglePrec_T;
</pre></div></td><td><p>Create a <code>Simulink.AliasType</code> object. Use the object to:</p><div><ul><li><p>Set the data types of signals and block parameters in a model.</p></li><li><p>Configure data type replacements for code generation.</p></li></ul></div><p>Generating code that uses an alias data type requires Embedded Coder<sup>®</sup>.</p></td><td><p>For information about defining custom data types for your model, see <a href="https://ww2.mathworks.cn/help/simulink/slref/simulink.aliastype.html"><code>Simulink.AliasType</code></a> and <a href="https://ww2.mathworks.cn/help/ecoder/ug/control-data-type-names-in-generated-code.html">Manage Replacement of Simulink Data Types in Generated Code</a>.</p><p>For an example that shows how to export the generated code into your external code, see <a href="https://ww2.mathworks.cn/help/ecoder/ug/conform-to-coding-standards-by-renaming-data-types.html">Replace and Rename Simulink Coder Data Types to Conform to Coding Standards</a>.</p></td></tr><tr><td><p>Array</p></td><td><div><pre>int myArray[6];
</pre></div></td><td><p>Specify signal and parameter dimensions as described in <a href="https://ww2.mathworks.cn/help/simulink/ug/determining-output-signal-dimensions.html">Determine Signal Dimensions</a>.</p><p>The generated code defines and accesses multidimensional data, including matrices, as column-major serialized vectors. If your external code uses a different format, consider using alternative techniques to integrate the generated code. See <a href="https://ww2.mathworks.cn/help/ecoder/ug/code-generation-of-matrix-data-and-arrays.html">Code Generation of Matrices and Arrays</a>.</p></td><td><p>For information about how the generated code stores nonscalar data (including limitations), see <a href="https://ww2.mathworks.cn/help/ecoder/ug/code-generation-of-matrix-data-and-arrays.html">Code Generation of Matrices and Arrays</a>.</p><p>For an example that shows how to export the generated code into your external code, see <a href="https://ww2.mathworks.cn/help/rtw/ug/use-global-parameter-object-in-different-data-type-contexts.html">Reuse Parameter Data in Different Data Type Contexts</a>.</p><p>To model lookup tables, see <a href="https://ww2.mathworks.cn/help/simulink/slref/simulink.lookuptable-class.html"><code>Simulink.LookupTable</code></a>.</p></td></tr><tr><td><p>Enumeration</p></td><td><div><pre>typedef enum myColorsType {
  Red = 0,
  Yellow,
  Blue
} myColorsType;
</pre></div></td><td><p>Define a Simulink enumeration that corresponds to your enumeration definition. Use the Simulink enumeration to set data types in a model.</p></td><td><p>To use enumerated data in a Simulink model, see <a href="https://ww2.mathworks.cn/help/rtw/ug/enumerations.html">Use Enumerated Data in Generated Code</a>.</p><p>For an example that shows how to generate enumerated parameter data, see <a href="https://ww2.mathworks.cn/help/ecoder/ug/enumeration.html">Enumeration</a>.</p><p>For an example that shows how to export the generated code into your external code by exchanging enumerated data, see <a href="https://ww2.mathworks.cn/help/ecoder/ug/exchange-structured-and-enumerated-data-between-generated-and-external-code.html">Exchange Structured and Enumerated Data Between Generated and External Code</a>.</p></td></tr><tr><td><p>Structure</p></td><td><div><pre>typedef struct myStructType {
    int count;
    double coeff;
} myStructType;
</pre></div></td><td><p>Create a <code>Simulink.Bus</code> object that corresponds to your structure type.</p><p>To create structured signal or state data, package multiple signal lines in a model into a single nonvirtual bus signal.</p><p>To create structured parameter data, create a parameter object (such as <code>Simulink.Parameter</code>) that stores a MATLAB structure. Use the bus object as the data type of the parameter object.</p><p>To package lookup table data into a structure, use <code>Simulink.LookupTable</code> and, optionally, <code>Simulink.Breakpoint</code> objects.</p></td><td><p>For information about bus signals, see <a href="https://ww2.mathworks.cn/help/simulink/ug/getting-started-with-buses.html">Group Signals or Messages into Virtual Buses</a>. For information about structures of parameters, see <a href="https://ww2.mathworks.cn/help/simulink/ug/using-structure-parameters.html">Organize Related Block Parameter Definitions in Structures</a>.</p><p>For an example that shows how to import your external code into a model by using the Legacy Code Tool, see <a href="https://ww2.mathworks.cn/help/simulink/sfg/integrating-existing-c-functions-into-simulink-models-with-the-legacy-code-tool.html#bu4agtq-1">Integrate C Function Whose Arguments Are Pointers to Structures</a>.</p><p>For examples that show how to export the generated code into your external code, see <a href="https://ww2.mathworks.cn/help/ecoder/ug/exchange-structured-and-enumerated-data-between-generated-and-external-code.html">Exchange Structured and Enumerated Data Between Generated and External Code</a> and <a href="https://ww2.mathworks.cn/help/ecoder/ug/access-structured-data-through-a-pointer-that-custom-code-defines.html">Access Structured Data Through a Pointer That External Code Defines</a>.</p><p>To package lookup table data into a structure, <a href="https://ww2.mathworks.cn/help/simulink/slref/simulink.lookuptable-class.html"><code>Simulink.LookupTable</code></a>.</p><p>For general information about creating structures in the generated code, see <a href="https://ww2.mathworks.cn/help/ecoder/ug/organize-data-into-structures-in-generated-code.html">Organize Data into Structures in Generated Code</a>.</p></td></tr></tbody></table>

**Additional Modeling Patterns for Code Generation (Embedded Coder)**

<table summary="Additional Modeling Patterns for Code Generation (Embedded Coder)"><colgroup><col width="15%"><col width="38%"><col width="24%"><col width="24%"></colgroup><thead><tr><th>C Data Type or Construct</th><th>Example C Code</th><th>Simulink Equivalent</th><th>More Information</th></tr></thead><tbody><tr><td><p>Macro</p></td><td><div><pre>#define myParam 9.8
</pre></div></td><td><p>Apply the storage classes <code>Define</code> and <code>ImportedDefine</code> to parameters. With macros, you can reuse a parameter value in multiple locations in an algorithm and change the parameter value between code compilations without consuming memory to store the value. Typically, macros represent engineering constants that you do not expect to change during code execution.</p><p>This technique requires Embedded Coder.</p></td><td><p><a href="https://ww2.mathworks.cn/help/ecoder/ug/macro-definitions-define.html">Macro Definitions (#define)</a></p></td></tr><tr><td>Storage type qualifiers such as <code>const</code> and <code>volatile</code></td><td><div><pre>const myParam = 9.8;
</pre></div></td><td><p>Apply the storage classes <code>Const</code>, <code>Volatile</code>, and <code>ConstVolatile</code> to data items.</p></td><td><a href="https://ww2.mathworks.cn/help/ecoder/ug/exchange-data-between-external-calling-code-and-generated-code.html#mw_005394bf-137f-4105-8cc0-7c9f54a420b0">Protect Global Data with Keywords const and volatile</a></td></tr><tr><td><p>Bit field</p></td><td><div><pre>typedef struct myBitField {
    unsigned short int MODE : 1;
    unsigned short int FAIL : 1;
    unsigned short int OK : 1;
} myBitField
</pre></div></td><td><div><ul><li><p>Apply the storage class <code>BitField</code> to signals, states, and parameters whose data type is <code>boolean</code>.</p></li><li><p>Use a model configuration parameter to aggregate Boolean data into bit fields.</p></li></ul></div><p>These techniques require Embedded Coder.</p></td><td><p><a href="https://ww2.mathworks.cn/help/ecoder/ug/bitfields.html">Bitfields</a></p><p><a href="https://ww2.mathworks.cn/help/ecoder/ug/optimize-generated-code-by-packing-boolean-data-into-bitfields.html">Optimize Generated Code by Packing Boolean Data into Bitfields</a></p></td></tr><tr><td><p>Call to custom external function that reads or writes to data</p></td><td><p>External code:</p><div><pre>/* Call this function
to acquire the value of
the signal. */
double get_inSig(void)
{
    return myBigGlobalStruct.inSig;
}

</pre></div><p>Generated algorithmic code:</p><div><pre>algorithmInput = get_inSig();
</pre></div></td><td><p>Apply the storage class <code>GetSet</code> to signals, states, and parameters. Each data item appears in the generated code as a call to your custom functions that read and write to the target data.</p><p>This technique requires Embedded Coder.</p></td><td><p><a href="https://ww2.mathworks.cn/help/ecoder/ug/getset-custom-storage-classes.html">Access Data Through Functions with Storage Class GetSet</a></p></td></tr></tbody></table>

### Considerations for Other Modeling Goals

<table><colgroup><col width="27%"><col width="74%"></colgroup><thead><tr><th>Goal</th><th>Considerations and More Information</th></tr></thead><tbody><tr><td>Use Simulink to run and interact with the generated code</td><td><p>You can use SIL, PIL, and external mode simulations to connect your model to the corresponding generated application for simulation. When you import parameter data from your external code:</p><div><ul><li><p>At the time that you begin an external mode simulation, the external executable uses the value that your code uses to initialize the parameter data. However, when you change the corresponding value in Simulink during the simulation (for example by modifying the <code>Value</code> property of the corresponding parameter object), Simulink downloads the new value to the executable.</p></li><li><p>SIL and PIL simulations do not import the parameter value from your code. Instead, the simulations use the parameter value from Simulink. If you include your external code in the <span>Simulink Coder™</span> build process, duplicate data definitions can prevent the generated code from compiling.</p></li></ul></div><p>For information about SIL and PIL, see <a href="https://ww2.mathworks.cn/help/ecoder/ug/choosing-a-sil-or-pil-approach.html">Choose a SIL or PIL Approach</a>. For information about external mode simulation, see <a href="https://ww2.mathworks.cn/help/ecoder/ug/external-mode-simulations-for-parameter-tuning-and-signal-monitoring.html">External Mode Simulations for Parameter Tuning, Signal Monitoring, and Code Execution Profiling</a>.</p></td></tr><tr><td>Generate code comments that describe attributes of exported data including physical units, real-world initial value, and data type</td><td><p>Generating these comments can help you match data interfaces while handwriting integration code. See <a href="https://ww2.mathworks.cn/help/ecoder/ug/add-custom-comments-for-signal-or-parameter-identifier.html">Add Custom Comments for Variables in the Generated Code</a>.</p></td></tr></tbody></table>

## Related Topics

*   [How Generated Code Exchanges Data with an Environment](https://ww2.mathworks.cn/help/ecoder/ug/how-generated-code-exchanges-data-with-an-environment.html)
*   [Generate Code That Matches Appearance of External Code](https://ww2.mathworks.cn/help/ecoder/ug/generate-code-that-matches-the-appearance-of-external-code.html)
*   [Design Data Interface by Configuring Inport and Outport Blocks](https://ww2.mathworks.cn/help/ecoder/ug/configure-data-interface-by-applying-storage-classes-to-inport-and-outport-blocks.html)
*   [Configure Generated Code According to Interface Control Document Specifications](https://ww2.mathworks.cn/help/ecoder/ug/configure-generated-code-according-to-ICD-interactively.html)
*   [Generate Code That Dereferences Data from a Literal Memory Address](https://ww2.mathworks.cn/help/ecoder/ug/standalone-programs-no-operating-system.html#bvm9vry)

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