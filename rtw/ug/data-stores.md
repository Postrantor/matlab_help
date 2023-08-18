---
url: https://ww2.mathworks.cn/help/rtw/ug/data-stores.html
title: Data Stores in Generated Code
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-18 20:59:57
tag: 
summary: Use a data store to explicitly model a piece of shared global data in the generated code.
---
## Data Stores in Generated Code

### About Data Stores

A data store contains data that is accessible in a model hierarchy at or below the level in which the data store is defined. Data stores can allow subsystems and referenced models to share data without having to use I/O ports to pass the data from level to level. See [Data Stores with Data Store Memory Blocks](https://ww2.mathworks.cn/help/simulink/ug/model-global-data-using-data-stores.html#bryzv43-1) for information about data stores in Simulink®. This section provides additional information about data store code generation.

### Generate Code for Data Store Memory Blocks

To control the code generated for a Data Store Memory block, apply a storage class to the data store. You can associate a Data Store Memory block with a signal object that you store in a workspace or data dictionary, and control code generation for the block by applying the storage class to the object:

1.  On the **Modeling** tab, click **Model Data Editor**.
    
2.  In the Model Data Editor, select the **Data Stores** tab.
    
3.  Begin editing the name of the target Data Store Memory block by clicking the corresponding row in the **Name** column.
    
4.  Next to the name, click the button
    
    ![](https://ww2.mathworks.cn/help/rtw/ug/prop_action_ctrl.png)
    
    and select **Create and Resolve**.
    
5.  In the Create New Data dialog box, set **Value** to `Simulink.Signal`. Optionally, use the **Location** drop-down list to choose a workspace for storing the resulting `Simulink.Signal` object.
    
6.  Click **Create**. The `Simulink.Signal` object, which has the same name as the data store, appears in the target workspace. Simulink selects the block parameter **Data store name must resolve to Simulink signal object**.
    
    When the property dialog box for the object opens, click **OK**.
    
7.  In the **Simulink Coder** or **Embedded Coder** app, open the Code Mappings editor. In the **C Code** tab, select > .
    
8.  On the **Data Stores** tab, apply the target storage class.
    

**Note**

When a Data Store Memory block is associated with a signal object, the mapping between the **Data store name** and the signal object name must be one-to-one. If two or more identically named entities map to the same signal object, the name conflict is flagged as an error at code generation time. See [Resolve Conflicts in Configuration of Signal Objects for Code Generation](https://ww2.mathworks.cn/help/simulink/ug/working-with-data-objects.html#f1126515) for more information.

### Storage Classes for Data Store Memory Blocks

You can control how [Data Store Memory](https://ww2.mathworks.cn/help/simulink/slref/datastorememory.html) blocks in your model are stored and represented in the generated code by assigning storage classes and type qualifiers. You do this in almost exactly the same way you assign storage classes and type qualifiers for block states.

Data Store Memory blocks, like block states, have `Auto` storage class by default, and their memory is stored within the `DWork` vector. The symbolic name of the storage location is based on the data store name.

You can generate code from multiple Data Store Memory blocks that have the same data store name, subject to the following restriction: _at most one_ of the identically named blocks can have a storage class other than `Auto`. An error is reported if this condition is not met.

For blocks with `Auto` storage class, the code generator produces a unique symbolic name for each block to avoid name clashes. For Data Store Memory blocks with storage classes other than `Auto`, the generated code uses the data store name as the symbol.

In the following model, a Data Store Write block writes to memory declared by the Data Store Memory block `myData`:

![](https://ww2.mathworks.cn/help/rtw/ug/structs8.png)

To control the storage declaration for a Data Store Memory block, in the coder app, use the Code Mappings editor. On the **Data Stores** tab, select a **Storage Class** for the block.

Data Store Memory blocks are nonvirtual because code is generated for their initialization in `.c` and `.cpp` files and their declarations in header files. The following table shows how the code generated for the Data Store Memory block in the preceding model differs for different storage classes. The table gives the variable declarations and `MdlOutputs` code generated for the `myData` block.

<table><colgroup><col width="32%"><col width="35%"><col width="33%"></colgroup><thead><tr><th><p><strong>Storage Class</strong></p></th><th><p><strong>Declaration</strong></p></th><th><p><strong>Code</strong></p></th></tr></thead><tbody><tr><td><p><code>Auto</code> or <code>Model default</code> (when Code Mapping Editor specifies <code>Default</code> storage class)</p></td><td><p>In <em><code>model</code></em><code>.h</code></p><div><pre>typedef struct 
D_Work_tag 
{
  real_T myData;
} 
D_Work;
</pre></div><p>In <em><code>model</code></em><code>.c</code> or <em><code>model</code></em><code>.cpp</code></p><div><pre>/* Block states (auto storage) */
D_Work model_DWork;
</pre></div></td><td><div><pre>model_DWork.myData = 
  rtb_SineWave;
</pre></div></td></tr><tr><td><p><code>ExportedGlobal</code></p></td><td><p>In <em><code>model</code></em><code>.c</code> or <em><code>model</code></em><code>.cpp</code></p><div><pre>/* Exported block states */
real_T myData; 
</pre></div><p>In <em><code>model</code></em><code>.h</code></p><div><pre>extern real_T myData;
</pre></div></td><td><div><pre>myData = rtb_SineWave;
</pre></div></td></tr><tr><td><p><code>ImportedExtern</code></p></td><td><p>In <em><code>model</code></em><code>_private.h</code></p><div><pre>extern real_T myData;
</pre></div></td><td><div><pre>myData = rtb_SineWave;
</pre></div></td></tr><tr><td><p><code>ImportedExternPointer</code></p></td><td><p>In <em><code>model</code></em><code>_private.h</code></p><div><pre>extern real_T *myData;
</pre></div></td><td><div><pre>(*myData) = rtb_SineWave;
</pre></div></td></tr></tbody></table>

For information about applying storage classes, see [C Code Generation Configuration for Model Interface Elements](https://ww2.mathworks.cn/help/rtw/ug/c-code-generation-configuration-for-model-interface-elements.html).

For ERT models, you can preserve dimensions of multidimensional arrays in data stores. For more information, see [Preserve Dimensions of Multidimensional Arrays in Generated Code](https://ww2.mathworks.cn/help/ecoder/ug/preserve-dimensions-of-multidimensional-arrays-in-generated-code.html) (Embedded Coder).

### Data Store Buffering in Generated Code

A [Data Store Read](https://ww2.mathworks.cn/help/simulink/slref/datastoreread.html) block is a nonvirtual block that copies the value of the data store to its output buffer when it executes. Since the value is buffered, downstream blocks connected to the output of the data store read utilize the same value, even if a Data Store Write block updates the data store in between execution of two of the downstream blocks.

The next figure shows a model that uses blocks whose priorities have been modified to achieve a particular order of execution:

![](https://ww2.mathworks.cn/help/rtw/ug/image027model.png)

![](https://ww2.mathworks.cn/help/rtw/ug/image027.jpg)

The following execution order applies:

1.  The block Data Store Read buffers the current value of the data store `A` at its output.
    
2.  The block Abs1 uses the buffered output of Data Store Read.
    
3.  The block Data Store Write updates the data store.
    
4.  The block Abs uses the buffered output of Data Store Read.
    

Because the output of Data Store Read is a buffer, both Abs and Abs1 use the same value: the value of the data store at the time that Data Store Read executes.

The next figure shows another example:

![](https://ww2.mathworks.cn/help/rtw/ug/image029model.png)

![](https://ww2.mathworks.cn/help/rtw/ug/image029.jpg)

In this example, the following execution order applies:

1.  The block Data Store Read buffers the current value of the data store `A` at its output.
    
2.  Atomic Subsystem executes.
    
3.  The Sum block adds the output of Atomic Subsystem to the output of Data Store Read.
    

Simulink assumes that Atomic Subsystem might update the data store, so Simulink buffers the data store. Atomic Subsystem executes after Data Store Read buffers its output, and the buffer provides a way for the Sum block to use the value of the data store as it was when Data Store Read executed.

In some cases, the code generator determines that it can optimize away the output buffer for a Data Store Read block, and the generated code refers to the data store directly, rather than a buffered value of it. The next figure shows an example:

![](https://ww2.mathworks.cn/help/rtw/ug/image031model.png)

![](https://ww2.mathworks.cn/help/rtw/ug/image031.jpg)

In the generated code, the argument of the `fabs()` function is the data store `A` rather than a buffered value.

### Data Stores Shared by Instances of a Reusable Model

You can use a data store to share a piece of data between the instances of a reusable referenced model (see [Share Data Among Referenced Model Instances](https://ww2.mathworks.cn/help/simulink/ug/define-referenced-model-inputs-and-outputs-1.html#mw_cf67482d-bf21-4aec-8eda-e8d60bd5d580)) or a model that you configure to generate reentrant code (by setting the configuration parameter **Code interface packaging** to `Reusable function`). If you implement the data store as a Data Store Memory block and select the **Share across model instances** parameter:

*   By default, the data store appears in the generated code as a separate global symbol.
    
*   If you have Embedded Coder®, to restrict access such that only the code generated from the model can use the data store, configure the data store to appear as `static` by applying the storage class `FileScope`. For more information about `FileScope` and other storage classes, see [Choose Storage Class for Controlling Data Representation in Generated Code](https://ww2.mathworks.cn/help/ecoder/ug/choose-a-built-in-storage-class-for-controlling-data-representation-in-the-generated-code.html) (Embedded Coder).
    

### Structures in Generated Code Using Data Stores

If you use more than one data store to provide global access to multiple signals in generated code, you can combine the signals into a single structure variable by using one data store. This combination of signal data can help you integrate the code generated from a model with other existing code that requires the data in a structure format.

This example shows how to store several model signals in a structure in generated code using a single data store. To store multiple signals in a data store, you configure the data store to accept a composite signal, such as a nonvirtual bus signal or an array of nonvirtual bus signals.

#### Explore Example Model

Open the example model `BusStructInCode`.

```
open_system('BusStructInCode')


```

![](https://ww2.mathworks.cn/help/examples/simulinkcoder/win64/DataStoresInCodeExploreModelExample_01.png)

The model contains three subsystems that perform calculations on the inputs from the top level of the model. In each subsystem, a Data Store Memory block stores an intermediate calculated signal.

Generate code with the model. In the code generation report, view the file `BusStructInCode.c`. The code defines a global variable for each data store.

```
real_T BioBTURate;
real_T CoalBTURate;
real_T GasBTURate;


```

Suppose that you want to integrate code generated from the example model with other existing code. Suppose also that the existing code requires access to data from the three data stores in a single structure variable. You can use a data store to assemble the target data in a structure in generated code.

#### Configure Data Store

Configure a data store to contain multiple signals by creating a bus type to use as the data type of the data store. Define the bus type using the same hierarchy of elements as the structure that you want to appear in generated code.

1.  Open the Type Editor tool.
    
2.  Define a new bus type `Raw_BTU_Rate` with one element for each of the three target signals. Name the elements `BioBTU`, `GasBTU`, and `CoalBTU`.
    
    ![](https://ww2.mathworks.cn/help/rtw/ug/struct_code_buseditor.png)
    
3.  At the top level of the example model, add a Data Store Memory block.
    
4.  On the **Modeling** tab, click **Model Data Editor**.
    
5.  In the Model Data Editor, inspect the **Data Stores** tab.
    
6.  For the new Data Store Memory block, use the **Name** column to set the data store name to `Raw_BTU_Data`.
    
7.  Use the **Data Type** column to set the data type of the data store to `Bus: Raw_BTU_Rate`.
    
8.  In the Code Mappings editor, on the **Data Stores** tab, apply the storage class `ExportedGlobal` to `Raw_BTU_Data`.
    

#### Write to Data Store Elements

To write to a specific element of a data store, use a Data Store Write block. On the **Element Assignment** tab in the dialog box, you can specify to write to a single element, a collection of elements, or the entire contents of a data store.

1.  Open the **Biomass Calc** subsystem.
    
2.  Delete the Data Store Memory block `BioBTURate`.
    
3.  In the block dialog box for the Data Store Write block, set **Data store name** to `Raw_BTU_Data`.
    
4.  On the **Element Assignment** tab, under **Signals in the bus**, expand the contents of the data store `Raw_BTU_Data`. Click the element `BioBTU`, and then click **Select**. Click **OK**.
    
    ![](https://ww2.mathworks.cn/help/rtw/ug/struct_code_dsw.png)
    
5.  Modify the **Gas Calc** and **Coal Calc** subsystems similarly.
    
    *   Delete the Data Store Memory block in each subsystem.
        
    *   In each Data Store Write block dialog box, set **Data store name** to `Raw_BTU_Data`.
        
    *   In the **Gas Calc** subsystem, use the Data Store Write block to write to the data store element `GasBTU`. In the **Coal Calc** subsystem, write to the element `CoalBTU`.
        
    

#### Generate Code with Data Store Structure

1.  Generate code for the example model.
    
2.  In the code generation report, view the file `BusStructInCode.h`. The code defines a structure that corresponds to the bus type `Raw_BTU_Rate`.
    
    ```
    typedef struct {
      real_T BioBTU;
      real_T GasBTU;
      real_T CoalBTU;
    } Raw_BTU_Rate;
    
    
    ```
    
3.  View the file `BusStructInCode.c`. The code represents the data store with a global variable `Raw_BTU_Data` of the structure type `Raw_BTU_Rate`. In the model step function, the code assigns the data from the calculated signals to the fields of the global variable `Raw_BTU_Data`.
    

## Related Topics

*   [C Code Generation Configuration for Model Interface Elements](https://ww2.mathworks.cn/help/rtw/ug/c-code-generation-configuration-for-model-interface-elements.html)
*   [Structures in Generated Code Using Data Stores](https://ww2.mathworks.cn/help/rtw/ug/data-stores.html#buqx1s3-1)
*   [When to Use a Data Store](https://ww2.mathworks.cn/help/simulink/ug/data-store-basics.html#br2h8hn)
*   [Generate Code That Dereferences Data from a Literal Memory Address](https://ww2.mathworks.cn/help/ecoder/ug/standalone-programs-no-operating-system.html#bvm9vry) (Embedded Coder)

您曾对此示例进行过修改。是否要打开带有您的编辑的示例？

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