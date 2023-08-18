---
url: https://ww2.mathworks.cn/help/simulink/slref/using-a-data-dictionary-to-manage-the-data-for-a-fuel-control-system.html
title: Using a Data Dictionary to Manage the Data for a Fuel Control System
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-18 20:59:11
tag: 
summary: This example shows how to use data dictionaries to manage the data for a fuel rate control system des......
---
This example shows how to use data dictionaries to manage the data for a fuel rate control system designed using Simulink® and Stateflow®.

### Familiarize Yourself with the Model

The `sldemo_fuelsys_dd` model is a closed-loop system containing a "plant" and "controller". The plant is used to validate the design of the controller. In this example, the plant and controller are represented by separate models that are referenced from the test harness model. Let's take a look at these models.

### Open and Compile the Test Harness Model

![](https://ww2.mathworks.cn/help/examples/simulink_automotive/win64/UseDDForFuelContSysExample_01.png)

### View the Engine Gas Dynamics System (Plant)

Double-click on the Engine Gas Dynamics block to open the plant model.

![](https://ww2.mathworks.cn/help/examples/simulink_automotive/win64/UseDDForFuelContSysExample_02.png)

### View the Fuel Rate Control System (Controller)

Double-click on the Fuel Rate Controller block to open the controller model.

![](https://ww2.mathworks.cn/help/examples/simulink_automotive/win64/UseDDForFuelContSysExample_03.png)

### Investigate the Data Used by the Controller

The global design data for the controller model is defined in a data dictionary. Using data dictionaries has many advantages over defining data in the base workspace.

The controller model is explicitly linked to a data dictionary. This link is set up on the **External Data** tab of the Model Properties dialog box. To open this dialog box, click the icon in the lower-left corner of the model window and select the gear icon on the right side.

To open the data dictionary in the Model Explorer, in the model window, click the same icon and select **External Data**. In Model Explorer, select `sldemo_fuelsys_dd_controller` under the **External Data** node.

In the **Contents** pane, you can see the parameter and signal objects that are used to configure the controller algorithm for simulation and code generation. In the **Dialog** pane on the right, you can see that the dictionary also contains a reference to another data dictionary, `sldemo_fuelsys_dd_types.sldd`, that defines the data type objects used by this model.

This data dictionary is configured for a float-point controller, as is seen by the data type display on the signal lines in the controller model. The data types dictionary, `sldemo_fuelsys_dd_types.sldd`, sources data type objects from the referenced dictionary `sldemo_fuelsys_dd_types_float.sldd`. To configure for a fixed-point controller, you could create a new dictionary that contains fixed-point data type objects and change the types dictionary (`sldemo_fuelsys_dd_types.sldd`) to reference that dictionary instead.

### Investigate the Units Used by the Components

Notice that units are visible on the model and subsystem icons and signal lines. Units are specified on the ports and on the bus, signal and parameter objects in the data dictionary.

### Simulate the Test Harness Model

The test harness model is also linked to a data dictionary (`sldemo_fuelsys_dd.sldd`). This data dictionary contains references to the data dictionaries for the plant and controller models but it does not contain any additional data.

Simulate the test harness model to validate the behavior of the controller in the floating-point configuration.

### Close the Example

Close the models and data dictionaries from this example.