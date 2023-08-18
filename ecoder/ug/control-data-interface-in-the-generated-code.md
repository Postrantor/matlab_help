---
url: https://ww2.mathworks.cn/help/ecoder/ug/control-data-interface-in-the-generated-code.html
title: Control Data and Function Interface in Generated Code
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-18 21:00:23
tag: 
summary: Control how generated code exchanges data with a calling environment.
---
## Control Data and Function Interface in Generated Code

To use the code that you generate from a model, you call generated entry-point functions such as `step` and `initialize`. The calling environment and the generated functions exchange input and output data through global variables or through formal parameters (arguments). This data and the exchange mechanisms constitute the interfaces of the entry-point functions. For information about the default interfaces for reentrant and nonreentrant models in the generated code, see [How Generated Code Exchanges Data with an Environment](https://ww2.mathworks.cn/help/ecoder/ug/how-generated-code-exchanges-data-with-an-environment.html).

By controlling the interfaces that appear in the generated code, you can:

*   Minimize the modifications that you must make to existing code.
    
*   Generate stable interfaces that do not change or minimally change when you make changes to the model.
    
*   Generate code that exchanges data more efficiently (for example, by using pointers and pass-by-reference arguments for nonscalar data).
    

### Control Type Names, Field Names, and Variable Names of Standard I/O Structures (Embedded Coder)

By default, for nonreentrant code, Inport blocks at the root level of the model appear in the generated code as fields of a global structure variable. Similarly, Outport blocks appear in a different structure. For reentrant code, depending on your setting for model configuration parameter , the code generator can also package input and output data into standard structures.

With Embedded Coder®, you can control these names. See [Manage Replacement of Simulink Data Types in Generated Code](https://ww2.mathworks.cn/help/ecoder/ug/control-data-type-names-in-generated-code.html).

### Control Names of Generated Entry-Point Functions (Embedded Coder)

To use the generated code, you write code that calls generated entry-point functions. For example, entry-point functions include ``_`model`__step``, ``_`model`__initialize``, and top-level functions generated from an export-function model. To control the names of the model entry-point functions, use the Code Mappings editor (requires Embedded Coder) to apply a combination of these techniques:

*   On the **Function Defaults** tab, specify default naming rules for categories of entry-point functions by applying function customization templates in the Code Mappings editor. With this technique, a naming rule applies to functions in a category. For more information, see [Configure Default Code Generation for Functions](https://ww2.mathworks.cn/help/ecoder/ug/configure-default-code-generation-for-categories-of-model-data-and-functions.html#mw_d8769be6-5dd2-412c-a917-6b836bc7e860).
    
*   On the **Functions** tab, specify names for individual entry-point functions by either directly editing the **Function Name** column or through a configuration dialog box opened from the **Function Preview** column. Names that you specify override default naming rules specified by function customization templates. For more information, see [Configure Generated C Function Interface for Model Entry-Point Functions](https://ww2.mathworks.cn/help/ecoder/ug/configure-c-code-generation-for-model-entry-point-functions.html)
    

### Control Data Interface for Nonreentrant Code

When you set the model configuration parameter to `Nonreusable function` (the default), generated entry-point functions are not reentrant. Typically, the functions exchange data with the calling environment through direct access to global variables.

#### Configure Inport or Outport Block as Separate Global Variable

To remove the block from the standard I/O structures by creating a separate global variable, apply a storage class, such as `ExportedGlobal` or `ExportToFile`, to the signal that the block represents. You can configure a default storage class for categories of data elements, such as Inport blocks. As you add such blocks to the model, they acquire the storage class that you specify. You can use the Code Mappings editor also to configure individual blocks. You might do this if a model has just a few data elements of a specific category or override the default configuration settings.

For an example, see [Design Data Interface by Configuring Inport and Outport Blocks](https://ww2.mathworks.cn/help/rtw/ug/configure-data-interface-by-applying-storage-classes-to-inport-and-outport-blocks.html). For general information about configuring data for code generation, see [C Code Generation Configuration for Model Interface Elements](https://ww2.mathworks.cn/help/ecoder/ug/c-code-generation-configuration-for-model-interface-elements.html).

#### Configure Generated Code to Read or Write to Global Variables Defined by External Code

If your calling code already defines a global variable that you want the generated code to use as input data or use to store output data, you can reuse the variable by preventing the code generator from duplicating the definition. Apply a storage class to the corresponding Inport or Outport block in the model. Choose a storage class that specifies an imported data scope, such as `ImportedExtern` or `ImportFromFile`. For information about applying storage classes, see [C Code Generation Configuration for Model Interface Elements](https://ww2.mathworks.cn/help/ecoder/ug/c-code-generation-configuration-for-model-interface-elements.html) and [Organize Parameter Data into a Structure by Using Struct Storage Class](https://ww2.mathworks.cn/help/ecoder/ug/configure-parameters-for-code-generation.html#br0debj).

#### Package Multiple Inputs or Outputs into Custom Structure

You can configure a single Inport or Outport block to appear in the generated code as a custom structure that contains multiple input or output signals. You can also configure the block to appear as a substructure of the default I/O structures or as a separate structure variable.

Configure the block as a nonvirtual bus by using a `Simulink.Bus` object as the data type of the block. If your external code defines the structure type, consider using the `Simulink.importExternalCTypes` function to generate the bus object.

*   To generate the bus signal as a substructure in the standard I/O structures, leave the block storage class at the default setting, `Auto`. If you have Embedded Coder, in the Code Mappings editor, on the **Data Defaults** tab, set the storage class for categories **Inports** and **Outports** to `Default`.
    
*   To generate the bus signal as a separate global structure variable, apply a storage class such as `ExportedGlobal` or `ExportToFile`.
    

For more information about grouping signals into custom structures in the generated code, see [Organize Data into Structures in Generated Code](https://ww2.mathworks.cn/help/ecoder/ug/organize-data-into-structures-in-generated-code.html).

#### Configure Inport or Outport Block as Function Call (Embedded Coder)

If your external code defines a function that returns input data for the generated code or accepts output data that the generated code produces, you can configure an Inport or Outport block so that the generated code calls the function instead of accessing a global variable. Apply the Embedded Coder storage class `GetSet`. For more information, see [Access Data Through Functions with Storage Class GetSet](https://ww2.mathworks.cn/help/ecoder/ug/getset-custom-storage-classes.html).

#### Pass Inputs and Outputs Through Function Arguments (Embedded Coder)

With Embedded Coder, you can optionally configure the model step (execution) function to access root-level input and output through arguments instead of directly reading and writing to global variables. Completely control the argument characteristics such as name, order, and passing mechanism (by reference or by value). This level of configuration can help to integrate generated code with your external code.

To pass inputs and outputs through arguments, in the Configure C Step Function interface dialog box, select **Configure arguments for Step function prototype**. Each Inport and Outport block at the root level of the model appears in the code as an argument of the execution function. For more information, see [Configure Generated C Function Interface for Model Entry-Point Functions](https://ww2.mathworks.cn/help/ecoder/ug/configure-c-code-generation-for-model-entry-point-functions.html).

#### Configure Referenced Model Inputs and Outputs as Global Variables (`void-void`)

By default, for a nonreentrant referenced model, the generated code passes root-level input and output through function arguments. A nonreentrant referenced model is one in which you set model configuration parameter **Total number of instances allowed per top model** to `One`.

To pass this data through global variables instead (for a `void-void` interface), in the referenced model, apply storage classes such as `ExportedGlobal` and `ExportToFile` to root-level Inport and Outport blocks.

### Control Data Interface for Reentrant Code

When you set **Code interface packaging** to `Reusable function`, generated entry-point functions are reentrant. The functions exchange data with the calling environment through formal parameters (arguments). By default, each root-level Inport and Outport block appears in the generated code as a separate argument instead of a field of the standard I/O structures.

### Prevent Unintended Changes to the Interface

Some changes that you make to a model change the entry-point function interfaces in the generated code. For example, if you change the name of the model, the names of the functions can change. If you configure the model code to exchange data through arguments, when you add or remove Inport or Outport blocks or change the names of the blocks, the corresponding arguments can change.

For easier maintenance of your calling code, prevent changes to the interfaces of the entry-point functions.

*   If you exchange input and output data through arguments, configure the generated code to package Inport and Outport blocks into structures instead of allowing each block to appear as a separate argument (the default). Then, when you add or remove Inport or Outport blocks, or change their properties such as name and data type, the fields of the structures change, but the function interfaces do not change. See [Reduce Number of Arguments by Using Structures](https://ww2.mathworks.cn/help/ecoder/ug/control-data-interface-in-the-generated-code.html#mw_a5413158-54a7-4207-8257-129b905e7c6e).
    
*   Set the data types of Inport and Outport blocks explicitly instead of using an inherited data type setting (which these blocks use by default). Inherited data type settings can cause the blocks to use different data types depending on the data types of upstream and downstream signals. For more information about configuring data types, see [Control Data Types of Signals](https://ww2.mathworks.cn/help/simulink/ug/control-signal-data-types.html).
    
*   With Embedded Coder, specify function names that do not depend on the model name. If you specify a naming rule with a function customization template, do not use the token `$R` in the rule. See [Control Names of Generated Entry-Point Functions (Embedded Coder)](https://ww2.mathworks.cn/help/rtw/ug/control-data-interface-in-the-generated-code.html#mw_7cd0e66f-cfa8-4c0c-bd56-8f69b836cb74).
    

### Reduce Number of Arguments by Using Structures

Reducing the number of arguments of a function can improve the readability of the code and reduce consumption of stack memory. To create a structure argument that can pass multiple pieces of data at one time, use these techniques:

*   Manually combine multiple Inport or Outport blocks so that they appear in the generated code as fields of a structure or substructure of a standard data structure. The generated entry-point function or functions accept the address of the structure as a separate argument or as a substructure (field) of the standard I/O structures.
    
    Replace the Inport or Outport blocks with a single block, and configure the new block as a nonvirtual bus by using a `Simulink.Bus` object as the data type of the block. If your external code defines the structure type, consider using the `Simulink.importExternalCTypes` function to generate the bus object. See [Organize Data into Structures in Generated Code](https://ww2.mathworks.cn/help/ecoder/ug/organize-data-into-structures-in-generated-code.html) and [`Simulink.importExternalCTypes`](https://ww2.mathworks.cn/help/simulink/slref/simulink.importexternalctypes.html).
    
*   When you generate reentrant code with Embedded Coder, configure Inport and Outport blocks to appear in aggregated structures by default. Set model configuration parameter **Pass root-level I/O as** to a setting other than `Individual arguments`.
    
    *   To package Inport and Outport blocks into the real-time model data structure, select `Part of model data structure`. The code generator aggregates the blocks into the default I/O structures to which the real-time model data structure points. The generated entry-point function or functions accept the real-time model data structure as a single argument. If you choose this setting, the function has the smallest number of arguments.
        
    *   To aggregate Inport blocks into a structure and Outport blocks into a different structure, select `Structure reference`. The generated entry-point function or functions accept the address of each structure as an argument. Another argument accepts the real-time model data structure. If you choose this setting, the inputs and outputs are more identifiable in the function interfaces.
        
        With this setting, if you remove the root-level Inport blocks or the root-level Outport blocks, the function signature can change. Similarly, the signature can change if you add a root-level Inport or Outport block to a model that does not include such blocks.
        
    *   For more control over the characteristics of the structures, set **Pass root-level I/O as** to `Part of model data structure`. Then, set the default storage class for Inport and Outport blocks to a structured storage class that you create. With this technique:
        
        *   You can create a single structure for the blocks or create two separate structures.
            
        *   You can control the names of the structure types.
            
        *   The structures appear as substructures of the real-time model data structure. You must set **Pass root-level I/O as** to `Part of model data structure`.
            
        
        Create the storage class by using the Embedded Coder Dictionary (see [Define Service Interfaces, Storage Classes, Memory Sections, and Function Templates for Software Architecture](https://ww2.mathworks.cn/help/ecoder/ug/define-storage-class-memory-section-and-function-template-settings-for-a-software-architecture.html)). Apply the storage class by using the Code Mappings editor (see [Configure Default Code Generation for Data](https://ww2.mathworks.cn/help/ecoder/ug/configure-default-code-generation-for-categories-of-model-data-and-functions.html#mw_abf23689-bf8b-4b4c-854e-b072022c3b12)).
        
    
    For more information, see [Pass root-level I/O as](https://ww2.mathworks.cn/help/ecoder/ref/passrootlevelioas.html).
    

### Control Data Types of Arguments

You can configure the generated entry-point functions to exchange data through arguments. For a scalar or array argument, to control the name of the primitive data type, use a `Simulink.AliasType` object to either set the data type of the corresponding block or to configure data type replacements for the entire model. These techniques require Embedded Coder. For more information, see [Manage Replacement of Simulink Data Types in Generated Code](https://ww2.mathworks.cn/help/ecoder/ug/control-data-type-names-in-generated-code.html).

### Promote Data Item to the Interface

By default, the code generator assumes that Inport and Outport blocks at the root level of the model constitute the data interface of the model. You can promote an arbitrary signal, block parameter, or block state to the interface so that other systems and components can access it. See [Promote Internal Data to the Interface](https://ww2.mathworks.cn/help/ecoder/ug/how-generated-code-stores-internal-signal-state-and-parameter-data.html#mw_3e0f39a7-17ca-4ab4-bc1e-ea342d2120e9).

## Related Topics

*   [Design Data Interface by Configuring Inport and Outport Blocks](https://ww2.mathworks.cn/help/ecoder/ug/configure-data-interface-by-applying-storage-classes-to-inport-and-outport-blocks.html)
*   [Define Interfaces of Model Components](https://ww2.mathworks.cn/help/simulink/ug/interface-design.html)
*   [Analyze Generated Data Code Interface](https://ww2.mathworks.cn/help/ecoder/ug/analyze-the-generated-code-interface.html)
*   [Configure Generated C Function Interface for Model Entry-Point Functions](https://ww2.mathworks.cn/help/ecoder/ug/configure-c-code-generation-for-model-entry-point-functions.html)
*   [How Generated Code Exchanges Data with an Environment](https://ww2.mathworks.cn/help/ecoder/ug/how-generated-code-exchanges-data-with-an-environment.html)
*   [Exchange Data Between External C/C++ Code and Simulink Model or Generated Code](https://ww2.mathworks.cn/help/ecoder/ug/generate-code-for-interfacing-data-with-external-code.html)
*   [Configure Generated Code According to Interface Control Document Specifications](https://ww2.mathworks.cn/help/ecoder/ug/configure-generated-code-according-to-ICD-interactively.html)
*   [C Code Generation Configuration for Model Interface Elements](https://ww2.mathworks.cn/help/ecoder/ug/c-code-generation-configuration-for-model-interface-elements.html)

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