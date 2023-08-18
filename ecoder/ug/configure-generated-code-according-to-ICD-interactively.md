---
url: https://ww2.mathworks.cn/help/ecoder/ug/configure-generated-code-according-to-ICD-interactively.html
title: Configure Generated Code According to Interface Control Document Specifications
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-18 21:00:18
tag: 
summary: Configure code generation settings for a model according to specifications from an interface control ......
---
This example shows how to configure code generation settings for a model according to specifications in an interface control document (ICD). The example also shows how to store shared Simulink® variables and data objects in a data dictionary.

An ICD describes the data interface between software components. To exchange and share data, the components declare and define global variables that store signal and parameter values according to the ICD. The ICD names the variables and lists characteristics such as data type, physical units, and parameter values. When you create component models in Simulink, you can configure the models such that the generated code conforms to the ICD. For example, you can use the ICD as a reference source while configuring models interactively or you can automate configuraiton by using a script to import data from an ICD.

### Explore Interface Control Document

In Microsoft® Excel® or another compatible program, open the `ex_ICD_PCG.xlsx` workbook and review the contents of the worksheets.

*   **`Signals`** **worksheet.** Each row represents a signal that crosses the interface boundary. Inspect the cell values in the worksheet. The `Owner` column identifies the name of the component that allocates memory for a signal. The `DataType` column names the signal data type in memory. For example, the worksheet uses the expression `Bus: EngSensors` to name the structure type `EngSensors`.
    
*   **`Parameters`** **worksheet.** The `Value` column lists the value of each parameter. If the value of a parameter is nonscalar, the value is stored in its own separate worksheet, which has the same name as the parameter.
    
*   **`Numeric Types`** **worksheet.** Each row represents a named numeric data type. In this ICD, the data uses fixed-point data types (Fixed-Point Designer). The `IsAlias` column indicates whether the C code uses the name of the data type (for example, `s16En3`) or uses the name of the primitive integer data type that corresponds to the word length (such as `short`). The `DataScope` column indicates whether the generated code exports or imports the data type definition.
    
*   **`Structure Types`** **worksheet.** Each row represents a structure type or a field of a structure type. For structure types, the value in the `DataType` column is `struct`. Subsequent rows that do not use `struct` represent fields of the preceding structure type. This ICD defines structure type, `EngSensors`, with four fields: `throttle`, `speed`, `ego`, and `map`.
    
*   **`Enumerated Types`** **worksheet.** Similar to the `Structure Types` worksheet, each row represents an enumerated type or an enumeration member. This ICD defines enumerated type `sld_FuelModes`.
    

### Explore External Code

Some data items in the ICD belong to `other_component`, which is a component that exists outside of MATLAB®. Open and explore the content of these example files, which define and declare the external data:

```
edit ex_inter_types.h
edit ex_inter_sigs.c
edit ex_inter_sigs.h

```

### Explore Example Model

In this example, you generate code from the controller model `rtwdemo_fuelsys_dd_controller`. Open your copy of the `mode`l.

```
open_system('rtwdemo_fuelsys_dd_controller')

```

Some signals in the controller model have names, for example, the input signal `sensors`. Some block parameters in the model refer to `Simulink.Parameter` objects in a data dictionary. For example, in the `airflow_calc` subsystem, the `Pumping Constant` block uses the parameter objects `PumpCon`, `SpeedVect`, and `PressVect`. These parameter objects set the values of the corresponding block parameters. You can apply code generation settings to the signals and parameter objects.

The controller model is linked to data dictionary rtw`demo_fuelsys_dd_controller.sldd`. Explore the data dictionary.

1.  In the lower-left corner of the controller model, click the model data badge.
    
2.  Click the **External Data** link.
    
3.  In the Model Explorer **Model Hierarchy** pane, expand the nodes **rtwdemo_fuelsys_dd_controller**, **External data**, and **rtwdemo_fuelsys_dd_controller**.
    
4.  Select **Design Data**.
    

The dictionary stores:

*   Parameter objects
    
*   `Simulink.NumericType` objects, such as `u8En7`
    
*   `Simulink.Bus` object, `EngSensors`
    

### Configure Model Design Data According to ICD

1.  Open the ICD spreadsheet, if not open already.
    
2.  In the Simulink Editor window for the controller model, navigate to the root level of the model.
    
3.  On the **Modeling** tab, click **Model Data Editor**.
    
4.  In the Model Data Editor, click the **Change scope** button. The Model Data Editor shows information about data items in the model subsystems.
    
5.  Click the **Show/refresh additional information** button. The Model Data Editor shows information about data objects (the `Simulink.Parameter` objects in the data dictionary) that the model uses.
    

#### Configure Design Settings for Signals

1.  In the ICD, click the **Signals** tab.
    
2.  In the Model Data Editor, click the **Inports/Outports** tab.
    
3.  Make sure that the configuration settings for Inport block `sensors` match the ICD specification. **Data Type** must be set to Bus: E`ngSensors`.
    
4.  Make sure that the configuration settings for Outport block `fuel_rate` match the ICD specification. **Data Type** must be set to `s16En7`. **Min** must be set to `0.8000`. **Max** must be set to `1.7000`. **Unit** must be set to `g/s`.
    
5.  Make sure that the configuration settings for signal `fuel_mode` match the ICD specification. Because `fuel_mode` is an output signal of a Stateflow® chart, you must use Model Explorer to configure the design settings. In the model, navigate into the Stateflow chart. Open Model Explorer. In the Model Explorer **Contents** pane (the middle pane), select `fuel_mode`. **Data Type** must be set to `Enum:sld_FuelModes`.
    

#### Configure Design Settings for Parameters

1.  In the ICD, click the **Parameters** tab.
    
2.  Navigate to the root level of the control model.
    
3.  In the Model Data Editor, click the **Parameters** tab.
    
4.  Use the **Filter contents** box to search for parameter `PressEst`. The Model Data Editor shows two rows: one row that corresponds to parameter object `PressEst` and one row that corresponds to the block parameter that uses `PressEst`.
    
5.  Make sure that the configuration settings for parameter `PressEst` match the ICD specification. **Data Type** must be `u8En7`. **Value** must be a 19x45 matrix of numeric values.
    
6.  Make sure that the **Data Type** and **Value** settings for parameters `PumpCon`, `SpeedEst`, `ThrotEst`, and `RampRateKiz` match the ICD specification.
    

#### Configure Design Settings for Numeric Types

1.  In the ICD, click the **Numeric Types** tab.
    
2.  If the Model Explorer is not open, open it.
    
3.  In the **Model Hierarchy** pane, navigate to the design data in the data dictionary for the controller model (**rtwdemo_fuelsys_dd_controller**> **External Data**>**rtwdemo_fuelsys_dd_controller**>**Design Data**).
    
4.  In the **Contents** (middle) pane, select the `Simulink.NumericType` object `u8En7`. This object represents one of the `typedef` statements in `ex_inter_types.h`.
    
5.  In the right pane, on the **Design** tab, confirm that the setting for **Data type mode** aligns with the ICD specification.
    
6.  Make sure that the design configuration for numeric types `s16En3`, `s16En7`, and `s16En15` matches the ICD specification.
    

#### Configure Design Settings for Structures

1.  In the ICD, click the **Structure Types** tab.
    
2.  In the Model Explorer **Contents** pane, select `EngSensors`. This `Simulink.Bus` object represents the structure type defined in `ex_inter_types.h`.
    
3.  Confirm that the data type configuration aligns with the ICD specification. In the right pane, on the **Design** tab, or in the Type Editor, adjust the bus signal configurations (for example, settings for **Min** and **Max**) to align with the ICD specification.
    

#### Configure Design Settings for Enumerated Types

1.  In the ICD, click the **Enumerated Types** tab.
    
2.  Open the file `sld_FuelModes.m`, which defines the class for the enumerated type `sld_FuelModes`.
    
3.  Confirm that the class definitnion aligns with the ICD specification. In the file sld_FuelModes.m, adjust member names and underlying integer values to align with the ICD specification.
    

### Configure Model for Code Generation According to ICD

1.  Open the ICD spreadsheet, if not open already.
    
2.  Open the Embedded Coder app.
    

#### Configure Code Generation Settings for Inport and Outport Blocks

1.  In the ICD, click the **Signals** tab. The Owner column in the ICD implies that a different component (`other_component`), not `rtwdemo_fuelsys_dd_controller`, provides the code definition of the `sensors` variable.
    
2.  Include the C source code file that defines the variable `sensors`. In the Simulink Editor, on the C Code tab, select **Settings**> **C/C++ Code generation settings**. Make sure that the code generation custom code parameter **Source files** is set to `ex_inter_sigs.c`.
    
3.  In the Simulink Editor, on the **C Code** tab, select **Code Interface**> **Individual Element Code Mappings**.
    
4.  In the Code Mappings editor, click the **Inports** tab.
    
5.  Select the row for `sensors`. Because the source code definition is provided in the external file `ex_inter_sigs.c`, which you configured in step 2, **Storage class** must be set to `ImportFromFile`.
    
6.  Click the
    
    ![](https://ww2.mathworks.cn/help/examples/ecoder/win64/ConfigGenCodeAccdingICDInterExample_01.png)
    
    icon. Check that the **HeaderFile** property setting matches the ICD specification, `ex_inter_sigs.h`. Make sure that a value is specified for the **Identifier** property. This property specifes the variable name for the data element in the generated code.
    
7.  In the Code Mappings editor, click the **Outports** tab.
    
8.  Select the row for `fuel_rate`. Because the ICD specifies `rtwdemo_fuelsys_dd_controller` as the owner of `fuel_rate`, **Storage Class** must be set to `ExportToFile`.
    
9.  Click the
    
    ![](https://ww2.mathworks.cn/help/examples/ecoder/win64/ConfigGenCodeAccdingICDInterExample_02.png)
    
    icon. Make sure that the settings for properties **HeaderFile, DefinitionFile**, and **Owner** match the ICD specifications `global_data.h`, `signals.c`, and `rtwdemo_fuelsys_dd_controller`, respectively. Make sure that a value is specified for the **Identifier** property.
    

#### Configure Code Generation Settings for Signals

1.  In the Code Mappings editor, click the **Signals/States** tab. The third signal listed on the **Signals** tab of the ICD is `fuel_mode`.
    
2.  Add the `fuel_mode` signal to the model code mappings. In the model diagram, select signal `fuel_mode`. Place your cursor on the ellipsis that appears above or below the signal line to open the action bar. Click the **Add selected signals to code mappings** button. In the Code Mappings editor, the **Signals** node expands and lists the signal that you added. The Storage Class setting must indicate that the signal resolves to a signal object and that the storage class for the object is set to `ExportToFile`. Because the signal is associated with a signal object, you must use the Model Explorer to confirm that the code generation configuration aligns with the ICD specification.
    
3.  Open the Model Explorer. In the Model Hierarchy pane, navigate to the design data in the data dictionary for the controller model (**rtwdemo_fuelsys_dd_controller**> **External Data**> **rtwdemo_fuelsys_dd_controller**> **Design Data**). In the **Contents** pane, select `fuel_mode`.
    
4.  In the right pane, click the **Code Generation** tab. **Storage class** must be set to `ExportToFile`. Confirm that properties **HeaderFile**, **DefintionFile**, and **Owner** match the ICD specifications `global_data.h`, `signals.c`, and `rtwdemo_fuelsys_dd_controller`, respectively.
    

#### Configure Code Generation for Parameters

1.  In the ICD, click the **Parameters** tab.
    
2.  In the Code Mappings editor, click the **Paramters** tab.
    
3.  Use the **Filter contents** box to search for parameter `PressEst`. The Code Mappings editor shows a row that corresponds to parameter object `PressEst`. Because the ICD specifies `rtwdemo_fuelsys_dd_controller` as the owner of `PressEst`, **Storage Class** should be set to `ExportToFile`.
    
4.  Click the
    
    ![](https://ww2.mathworks.cn/help/examples/ecoder/win64/ConfigGenCodeAccdingICDInterExample_03.png)
    
    icon. Check that the settings for properties **HeaderFile, DefinitionFile**, and **Owner** match the ICD specifications `global_data.h`, param`s.c`, and `rtwdemo_fuelsys_dd_controller`, respectively. Also, make sure that a value is specified for the **Identifier** property.
    
5.  Check the **Storage Class**, **HeaderFile**, **DefinitionFile**, **Identifier**, and **Owner** settings for parameters `PumpCon`, `SpeedEst`, `ThrotEst`, and `RampRateKiz`.
    

#### Configure Code Generation for Numeric Types

1.  In the ICD, click the **Numeric Types** tab.
    
2.  If the Model Explorer is not open, open it.
    
3.  In the **Model Hierarchy** pane, navigate to the design data in the data dictionary for the controller model (**rtwdemo_fuelsys_dd_controller**> **External Data**> **rtwdemo_fuelsys_dd_controller**> **Design Data**).
    
4.  In the **Contents** pane, select the `Simulink.NumericType` object `u8En7`. This object represents one of the `typedef` statements in `ex_inter_types.h`.
    
5.  In the right pane, click the **Code Generation** tab. Confirm that the settings for **Data scope** and **Header file** align with the ICD specifications, `Imported` and `ex_inter_types.h`, respectively.
    
6.  Check and adjust the code generation configuration for numeric types `s16En3`, `s16En7`, and `s16En15`.
    

#### Configure Code Generation for Structures

1.  In the ICD, click the **Structure Types** tab.
    
2.  In the Model Explorer **Contents** pane, select `EngSensors`. This `Simulink.Bus` object represents the structure type defined in `ex_inter_types.h`.
    
3.  In the right pane, click the **Code Generation** tab. Confirm that the settings for **Data scope** and **Header file** align with the ICD specifications, `Imported` and `ex_inter_types.h`, respectively.
    

### Generate and Inspect Code

1. Generate code for the controller model.

```
slbuild('rtwdemo_fuelsys_dd_controller')

```

```
### Starting build procedure for: rtwdemo_fuelsys_dd_controller
### Successful completion of build procedure for: rtwdemo_fuelsys_dd_controller

Build Summary

Top model targets built:

Model                          Action                       Rebuild Reason                                    
==============================================================================================================
rtwdemo_fuelsys_dd_controller  Code generated and compiled  Code generation information file does not exist.  

1 of 1 models built (0 models already up to date)
Build duration: 0h 3m 1.371s


```

2. Inspect the generated code. The generated header file rtw`demo_fuelsys_dd_controller_types.h:`

*   Includes (`#include`) the external header file `ex_inter_types.h`, which defines numeric data types `u8En7, s16En3`, and `s16En7` and the structure type `EngSensors`
    
*   Defines the enumeration `sld_FuelModes`
    

```
file = fullfile('rtwdemo_fuelsys_dd_controller_ert_rtw',...
    'rtwdemo_fuelsys_dd_controller_types.h');
rtwdemodbtype(file,'#include "ex_inter_types.h"','} sld_FuelModes;',1,1)

```

```
#include "ex_inter_types.h"

/* Model Code Variants */
#ifndef DEFINED_TYPEDEF_FOR_sld_FuelModes_
#define DEFINED_TYPEDEF_FOR_sld_FuelModes_

typedef enum {
  LOW = 1,                             /* Default value */
  RICH,
  DISABLED
} sld_FuelModes;


```

The file `rtwdemo_fuelsys_dd_controller_private.h` includes the header file `ex_inter_sigs.h`. This external header file contains the `extern` declaration of the signal `sensors`, which a different software component owns.

The data header file `global_data.h` declares the exported parameters and signals that the ICD specifies. To share this data, other components can include this header file.

```
file = fullfile('rtwdemo_fuelsys_dd_controller_ert_rtw','global_data.h');
rtwdemodbtype(file,'/* Exported data declaration */',...
    'fuel_rate',1,1)

```

```
/* Exported data declaration */

/* Declaration for custom storage class: ExportToFile */
extern real32_T PressEst[855];   /* Referenced by: '<S6>/Pressure Estimation' */
extern real32_T PumpCon[551];       /* Referenced by: '<S1>/Pumping Constant' */
extern real32_T RampRateKiz[25];       /* Referenced by: '<S1>/Ramp Rate Ki' */
extern real32_T SpeedEst[1305];     /* Referenced by: '<S7>/Speed Estimation' */
extern real32_T ThrotEst[551];   /* Referenced by: '<S8>/Throttle Estimation' */
extern sld_FuelModes fuel_mode;        /* '<Root>/control_logic' */
extern real32_T fuel_rate;             /* '<Root>/fuel_rate' */


```

The algorithm code is in the model `step` function in the file rtw`demo_fuelsys_dd_controller.c`. The algorithm uses the global data that the ICD identifies. For example, the algorithm uses the value of the signal `fuel_mode` in a `switch` block to control the flow of execution.

```
file = fullfile('rtwdemo_fuelsys_dd_controller_ert_rtw',...
    'rtwdemo_fuelsys_dd_controller.c');
rtwdemodbtype(file,'/* End of MultiPortSwitch: ''<S9>/Multiport Switch'' */',...
    '/* Outport: ''<Root>/fuel_rate'' incorporates:',0,0)

```

```
  /* SwitchCase: '<S10>/Switch Case' */
  switch (fuel_mode) {
   case LOW:
    /* Outputs for IfAction SubSystem: '<S10>/low_mode' incorporates:
     *  ActionPort: '<S12>/Action Port'
     */
    /* DiscreteFilter: '<S12>/Discrete Filter' incorporates:
     *  DiscreteIntegrator: '<S1>/Discrete Integrator'
     */
    denAccum = rtDWork.DiscreteIntegrator_DSTATE - -0.7408F *
      rtDWork.DiscreteFilter_states_g;


```

```
bdclose('rtwdemo_fuelsys_dd_controller')
Simulink.data.dictionary.closeAll('rtwdemo_fuelsys_dd_controller.sldd','-discard')

```

### Import ICD Specifications into Simulink

You can use a MATLAB script to import data settings from the ICD into variables in the Simulink base workspace.

1. Open and inspect the example script `ex_importICD_PCG.m`.

The script`:`

*   Imports the data from each worksheet of the ICD into variables in the base workspace.
    
*   Uses the imported data to configure design and code generation properties of `Simulink.Signal` and `Simulink.Parameter` objects in the base workspace.
    

If the base workspace already contains a data object that corresponds to a data element in the ICD, the script configures properties of the existing object. If the object does not exist, the script creates the object.

2. Load the model and run the script.

```
load_system('rtwdemo_fuelsys_dd_controller')
run('ex_importICD_PCG')

```

3. Regenerate and inspect the code.

```
slbuild('rtwdemo_fuelsys_dd_controller')

```

```
### Starting build procedure for: rtwdemo_fuelsys_dd_controller
### Successful completion of build procedure for: rtwdemo_fuelsys_dd_controller

Build Summary

Top model targets built:

Model                          Action                       Rebuild Reason                   
=============================================================================================
rtwdemo_fuelsys_dd_controller  Code generated and compiled  Generated code was out of date.  

1 of 1 models built (0 models already up to date)
Build duration: 0h 1m 36.732s


```

### Considerations

*   When you make changes to an ICD, you can reuse your import script to reconfigure the model.
    
*   If you use a script to import data from an ICD to the base workspace, consider migrating the data objects and variables to a data dictionary. A data dictionary permanently stores and tracks changes to data objects and variables. For example, you can import the definition of enumerated type `sld_FuelModes` into the controller model dictionary. See [Import and Export Dictionary Data](https://ww2.mathworks.cn/help/simulink/ug/import-and-export-dictionary-data.html) and [Enumerations in Data Dictionary](https://ww2.mathworks.cn/help/simulink/ug/migrate-enumerated-types-into-data-dictionary.html).
    
*   You can store configuration settings for signal and state data elements, such as data types, minimum and maximum values, and physical units inside or outside of model files. `Simulink.Signal` objects store the configuration settings outside of a model file. Alternatively, you can store the configuration settings in the model file by using block and port parameters that you can access through the Model Data Editor, Property Inspector, and other dialog boxes. See [Store Design Attributes of Signals and States](https://ww2.mathworks.cn/help/simulink/ug/signal-basics.html#bvcdbmy).
    

## Related Topics

*   [Exchange Data Between External C/C++ Code and Simulink Model or Generated Code](https://ww2.mathworks.cn/help/ecoder/ug/generate-code-for-interfacing-data-with-external-code.html)
*   [Replace and Rename Simulink Coder Data Types to Conform to Coding Standards](https://ww2.mathworks.cn/help/ecoder/ug/conform-to-coding-standards-by-renaming-data-types.html)
*   [Control Data and Function Interface in Generated Code](https://ww2.mathworks.cn/help/ecoder/ug/control-data-interface-in-the-generated-code.html)
*   [Data Import and Export](https://ww2.mathworks.cn/help/matlab/data-import-and-export.html)
*   [What Is a Data Dictionary?](https://ww2.mathworks.cn/help/simulink/ug/what-is-a-data-dictionary.html)
*   [Integrate External Application Code with Code Generated from PID Controller](https://ww2.mathworks.cn/help/ecoder/ug/replace-external-code-with-generated-code.html)

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