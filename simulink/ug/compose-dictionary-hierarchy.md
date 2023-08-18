---
url: https://ww2.mathworks.cn/help/simulink/ug/compose-dictionary-hierarchy.html
title: Partition Data for Model Reference Hierarchy Using Data Dictionaries
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-18 20:59:09
tag: 
summary: Compose a dictionary hierarchy based on a model reference hierarchy.
---
## Partition Data for Model Reference Hierarchy Using Data Dictionaries

When you use model referencing to break a large system of models into smaller components and subcomponents, you can create data dictionaries to segregate the _design data_. Design data is the set of workspace variables that the models use to specify block parameters and signal characteristics. For basic information about data dictionaries, see [What Is a Data Dictionary?](https://ww2.mathworks.cn/help/simulink/ug/what-is-a-data-dictionary.html).

To take this component-based approach to data management, create a shared dictionary that contains common data and a separate dictionary for each component that contains the data needed by that component.

### Open the Example Model and Load Design Data

Open the example model `ex_SystemModel`. This model is at the top of a reference hierarchy that includes the other example models.

Load the MAT-files contained in the working directory to create design data in the base workspace.

```
load ProjectData_Contr.mat
load ProjectData_ContrSub1.mat
load ProjectData_ContrSub2.mat
load ProjectData_ContrSubs.mat
load ProjectData_Plant.mat
load ProjectData_System.mat


```

### Create a Dictionary for Each Component

This example shows how to partition design data into dictionaries. When you finish, each component in the system has a dictionary, and dictionary references allow the components to share data.

#### Explore Example Model Hierarchy

1.  In the model, update the diagram. Each bus signal in the model uses a `Simulink.Bus` object as a data type. The objects, `SensorBus` and `CtrlBus`, are in the base workspace.
    
    The referenced models `ex_PlantComp_Lvl1` and `ex_ContrComp` use the bus objects for root-level inputs and outputs, which means the plant and controller components share the objects.
    
2.  In the base workspace, double-click the `Simulink.NumericType` object named `FloatType`. Signals, parameters, and other data items in the controller component use this shared data type.
    
3.  In the Model Explorer **Model Hierarchy** pane, expand the node ex_SystemModel.
    
    Click the `Configurations` node. In the **Contents** pane, the node `Reference to SimConfigSet` appears. `SimConfigSet` is a `Simulink.ConfigSet` object in the base workspace. To maintain configuration parameter uniformity for simulation, all of the models in the hierarchy refer to `SimConfigSet`.
    
4.  Right-click the node `Controller (ex_ContrComp)` and select .
    
5.  In the Model Explorer **Model Hierarchy** pane, expand the new node `ex_ContrComp`. Click the `Configurations` node.
    
    In the **Contents** pane, the node `Reference to CodeGenConfigSet` appears. `CodeGenConfigSet` is a `Simulink.ConfigSet` object in the base workspace. To maintain configuration parameter uniformity for code generation, the models in the controller component refer to `CodeGenConfigSet`. The models in the plant component do not use `CodeGenConfigSet`.
    
6.  In the **Model Hierarchy** pane, select **Base Workspace**. In the **Contents** pane, right-click the variable `diff` and select . In the **Select a system** dialog box, select `ex_SystemModel` and click **OK**. If you see a message about updating the diagram, click **OK**.
    
    In the **Contents** pane, the variable `diff` is used by Constant blocks in the models `ex_ContrComp_Sub1_Lvl1` and `ex_ContrComp_Sub1_Lvl2`, which make up the first controller subcomponent. Similarly, other models in the hierarchy share the base workspace variables `coeff`, `init`, `mu`, and `rho`.
    

The table shows the models that share each variable in the base workspace.

<table><colgroup><col width="33%"><col width="33%"><col width="33%"></colgroup><thead><tr><th>Variable Name</th><th>Models Using the Variable</th><th>Scope of Sharing</th></tr></thead><tbody><tr><td><code>CtrlBus</code></td><td>Top-level models in the plant and controller components</td><td>Shared globally by entire system</td></tr><tr><td><code>SensorBus</code></td><td>Top-level models in the plant and controller components</td><td>Shared globally by entire system</td></tr><tr><td><code>SimConfigSet</code></td><td>All models in the hierarchy</td><td>Shared globally by entire system</td></tr><tr><td><code>rho</code></td><td><code>ex_PlantComp_Lvl2</code> ,<code>ex_ContrComp_Sub1_Lvl2</code>, and <code>ex_ContrComp_Sub2_Lvl2</code></td><td>Shared globally by entire system</td></tr><tr><td><code>mu</code></td><td><code>ex_PlantComp_Lvl1</code> and <code>ex_PlantComp_Lvl2</code></td><td>Shared by models in the plant component</td></tr><tr><td><code>FloatType</code></td><td>All models in the controller component</td><td>Shared by controller component and subcomponents</td></tr><tr><td><code>CodeGenConfigSet</code></td><td>All models in the controller component</td><td>Shared by controller component and subcomponents</td></tr><tr><td><code>init</code></td><td><code>ex_ContrComp_Sub1_Lvl2</code> and <code>ex_ContrComp_Sub2_Lvl1</code></td><td>Shared by controller subcomponents</td></tr><tr><td><code>diff</code></td><td><code>ex_ContrComp_Sub1_Lvl1</code> and <code>ex_ContrComp_Sub1_Lvl2</code></td><td>Shared by models in the first controller subcomponent</td></tr><tr><td><code>coeff</code></td><td><code>ex_ContrComp_Sub2_Lvl1</code> and <code>ex_ContrComp_Sub2_Lvl2</code></td><td>Shared by models in the second controller subcomponent</td></tr></tbody></table>

Suppose that separate teams of developers maintain the plant component and the controller components. You can use data dictionaries to store and scope the shared design data.

#### Create Shared Global Dictionary

Create a shared global data dictionary that contains the data shared globally by the entire system.

1.  In the Model Explorer, select > > .
    
2.  Set the new dictionary name to `GlobalShare` and click **Save**.
    
3.  In the **Model Hierarchy** pane, right-click the `GlobalShare`node and select .
    
4.  In the **Model Hierarchy** pane, select **Base Workspace**. In the **Contents** pane, select the design data that are shared globally by the entire system: `CtrlBus`, `SensorBus`, and `rho`.
    
5.  Right-click and select .
    
6.  In the **Model Hierarchy** pane, right-click the `Design Data` node under `GlobalShare` and select .
    
7.  Similarly, copy `SimConfigSet` from the **Base Workspace** and copy to the `Configurations` node under `GlobalShare`.
    

#### Create Dictionary for Plant Component

Create a data dictionary for data shared by models in the Plant component. Add a reference from this dictionary to the shared global dictionary.

1.  In the Model Explorer, select > > .
    
2.  Set the new dictionary name to `Plant` and click **Save**.
    
3.  In the **Model Hierarchy** pane, select the node `Plant`. In the Dialog pane, under **Referenced Dictionaries**, click **Add**.
    
4.  Double-click `GlobalShare.sldd`.
    
5.  In the **Model Hierarchy** pane, right-click the node `Plant` and select .
    

#### Link Plant Component to Dictionary and Migrate Data

Link the Plant component to its component dictionary then migrate data shared by models in the Plant component from the base workspace to the dictionary.

1.  Open the model `ex_PlantComp_Lvl1`.
    
2.  In the model, update the diagram.
    
3.  If the Diagnostic View displays an error for multiple inconsistent definitions of `SimConfigSet`, select **Delete others** next to the `GlobalShare` instance. This fix keeps the definition in the `GlobalShare` dictionary and removes other definitions that can be seen by the model.
    
4.  In the **Modeling** tab, under **Design**, click **Link to Data Dictionary**.
    
5.  In the dialog box, click **Browse**.
    
6.  Double-click `Plant.sldd`.
    
7.  In the **Model Properties** dialog box, click **Apply**. Click **Change all models** in response to the message about linking referenced models.
    
8.  In the **Model Properties** dialog box, click **Migrate data**.
    
9.  In the Migrate Data dialog box, select **Include data from referenced models** and then click **Migrate**.
    
10.  (Optional) In the **Model Properties** dialog box, clear **Enable model access to base workspace**.
    
11.  Remove the previous method for loading model data. In the **Model Properties** dialog box, on the **Callbacks** tab, clear the `PreLoadFcn` for the model.
    
12.  Click **OK**.
    

#### Create Dictionary for Controller Component

Create a data dictionary that contains the data shared by models in the controller component. This dictionary can also reference the shared global dictionary.

1.  In the Model Explorer, select > > .
    
2.  Set the new dictionary name to `Controller` and click **Save**.
    
3.  In the **Model Hierarchy** pane, select the node `Controller`. In the Dialog pane, under **Referenced Dictionaries**, click **Add**.
    
4.  Double-click `GlobalShare.sldd`.
    
5.  In the **Model Hierarchy** pane, right-click the node `Controller` and select .
    

#### Link Controller Component to Dictionary and Migrate Data

Link the Controller component to its component dictionary then migrate data shared by models in the Controller component from the base workspace to the dictionary.

1.  Open the model `ex_ContrComp`.
    
2.  If the Diagnostic View displays an error for multiple inconsistent definitions of `SimConfigSet`, select **Delete others** next to the `GlobalShare` instance. This fix keeps the definition in the `GlobalShare` dictionary and removes other definitions that can be seen by the model.
    
3.  In the **Modeling** tab, under **Design**, click **Link to Data Dictionary**.
    
4.  In the dialog box, click **Browse**.
    
5.  Double-click `Controller.sldd`.
    
6.  In the **Model Properties** dialog box, click **Apply**. Click **Change all models** in response to the message about linking referenced models.
    
7.  In the **Model Properties** dialog box, click **Migrate data**.
    
8.  In the Migrate Data dialog box, select **Include data from referenced models** and then click **Migrate**.
    
9.  (Optional) In the **Model Properties** dialog box, clear **Enable model access to base workspace**.
    
10.  Remove the previous method for loading model data. In the **Model Properties** dialog box, on the **Callbacks** tab, clear the `PreLoadFcn` for the model.
    
11.  Click **OK**.
    

#### Link System to Global Dictionary

Finally, link the top model to the global dictionary.

1.  Open the model `ex_SystemModel`.
    
2.  In the **Modeling** tab, under **Design**, click **Link to Data Dictionary**.
    
3.  In the dialog box, click **Browse**.
    
4.  Double-click `GlobalShare.sldd`.
    
5.  In the **Model Properties** dialog box, click **OK**. Click **Change this model only** in response to the message about linking referenced models.
    

#### Inspect Data Storage

In the Model Explorer **Model Hierarchy** pane, select the dictionary node Plant. In the **Contents** pane, to view the contents of `Plant.sldd`, click **Show Current System and Below**

![](https://ww2.mathworks.cn/help/simulink/ug/me_current_system_button.png)

. The contents of the Design Data and Configurations sections appear.

![](https://ww2.mathworks.cn/help/simulink/ug/me_plant_sldd.png)

Similarly, view the contents of `Controller.sldd`.

![](https://ww2.mathworks.cn/help/simulink/ug/me_contr_sldd.png)

The **DataSource** column shows the variables and objects that each dictionary stores.

All of the globally shared variables, such as `CtrlBus` and `SensorBus`, reside in `GlobalShare.sldd`. The variable `init`, which both of the controller subcomponents share, resides in `Controller.sldd`.

If the development team assigned to the controller component must make changes to the globally shared variables, they access the `GlobalShare` dictionary file. Similarly, if the team must make changes to the variable `init`, they must access the `Controller` dictionary file.

#### Inspect Dictionary Hierarchy

To view the entire dictionary and model hierarchy, perform a dependency analysis.

1.  Open your saved model `ex_SystemModel`.
    
2.  On the **Modeling** tab, in the **Design** section, click **Dependency Analyzer**.
    

![](https://ww2.mathworks.cn/help/simulink/ug/systemhierarchy.png)

The system model, `ex_SystemModel`, is linked to the dictionary `GlobalShare.sldd`. The plant component and the controller component are each linked to a separate dictionary. To access the shared data, the component dictionaries reference the dictionary `GlobalShare.sldd`. These dictionaries form a reference hierarchy.

### Strategies to Discover Shared Data

To learn how the models in a model reference hierarchy share data, use these techniques:

*   In an open model, on the Modeling tab, select > . The Model Explorer displays the variables that the model uses, as well as the variables that referenced models use. You can then right-click a variable and select to display all of the models that use the variable. For more information, see [Edit and Manage Workspace Variables by Using Model Explorer](https://ww2.mathworks.cn/help/simulink/ug/workspace-variables-in-model-explorer.html).
    
*   At the command prompt, use the function [`Simulink.findVars`](https://ww2.mathworks.cn/help/simulink/slref/simulink.findvars.html) to determine the variables a model uses. You can then use the function [`intersect`](https://ww2.mathworks.cn/help/simulink/slref/simulink.variableusage.intersect.html) to determine the variables two models, components, or subcomponents share.
    

## Related Topics

*   [Determine Where to Store Variables and Objects for Simulink Models](https://ww2.mathworks.cn/help/simulink/ug/determine-where-to-store-data-for-simulink-models.html)
*   [Using a Data Dictionary to Manage the Data for a Fuel Control System](https://ww2.mathworks.cn/help/simulink/slref/using-a-data-dictionary-to-manage-the-data-for-a-fuel-control-system.html)
*   [Store Data in Dictionary Programmatically](https://ww2.mathworks.cn/help/simulink/ug/store-data-in-dictionary-programmatically.html)
*   [Compare Capabilities of Model Components](https://ww2.mathworks.cn/help/simulink/ug/model-architecture-guidelines.html)
*   [What Are Projects?](https://ww2.mathworks.cn/help/simulink/ug/what-are-simulink-projects.html)

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