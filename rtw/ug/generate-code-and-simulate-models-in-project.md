---
url: https://www.mathworks.com/help/rtw/ug/generate-code-and-simulate-models-in-project.html
title: Generate Code and Simulate Models in a Project - MATLAB & Simulink --- 在项目中生成代码并仿真模型 - MATLAB & Simulink
date: 2023-08-02 15:38:48
tag: 
summary: This example shows how to use the code generation template for a new Project.
---
This example shows how to use the code generation template for a new Project. The code generation project template for Project includes multiple models. The project template also provides utilities (.m scripts) that help you generate controller code and run simulations of the harness model.

### Create a Project

Create a new Project from the code generation project template.

To create this project from the Simulink® start page, in the Command Window, type:

`simulink`

Select the Code Generation template from the start page, and create the `exampleCodeGen` project.

### Generate Code

Generate controller code for the `feedback_control.slx` model.

To generate controller code, select the **Project Shortcuts** tab and select the **Generate Controller Code** shortcut.

This shortcut runs the `generate_controller_code.m` script in the `utilities` folder of the project. The script builds the `feedback_control.slx` model in the `controller` folder of the project.

![](https://www.mathworks.com/help/examples/simulinkcoder/win64/GenerateCodeUsingSimulinkProjectExample_01.png)

### Simulate Models

Simulate the top-level harness `feedback_harness.slx` model.

To open the harness model for simulation, select the **Project Shortcuts** tab and select the **Feedback Harness** shortcut. This shortcut opens the `feedback_harness.slx` model in the `harnesses` folder of the project.

To simulate the model, click **Run**.

![](https://www.mathworks.com/help/examples/simulinkcoder/win64/GenerateCodeUsingSimulinkProjectExample_02.png)

### Observe Simulation Output

Open the `Scope` block in the model and observe the simulation output.

To open the scope and observe simulation, in the Simulink window, double-click the `Scope` block.

![](https://www.mathworks.com/help/examples/simulinkcoder/win64/GenerateCodeUsingSimulinkProjectExample_03.png)

### Use the Dependency Analyzer View

To view file dependencies, use the **Dependency Analyzer** view of the project.

From the **Dependency Analyzer** view, you can:

*   Observe file dependencies.
    
*   Hover over the dependency arrows to examine the dependency type.
    
*   Double-click on a file or a model to open it.
    

![](https://www.mathworks.com/help/examples/simulinkcoder/win64/xxDependencyAnalysisView_templateCodeGenProject.png)

### More Information

### Related Examples

You have a modified version of this example. Do you want to open this example with your edits?

How useful was this information?

Unrated

1 star

2 stars

3 stars

4 stars

5 stars

 Unrated  1 star  2 stars  3 stars  4 stars  5 stars

You clicked a link that corresponds to this MATLAB command:

Run the command by entering it in the MATLAB Command Window. Web browsers do not support MATLAB commands.