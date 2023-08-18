---
url: https://ww2.mathworks.cn/help/simulink/ug/working-with-data-objects.html
title: 数据对象
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-18 20:59:17
tag: 
summary: 通过使用外部数据对象，在模块图外部指定参数、信号和状态的属性，包括参数值。
---
## 数据对象

您可以创建_数据对象_，以便为信号、状态和模块参数指定值、值范围、数据类型、可调性以及其他特征。您可以在 Simulink® 对话框中使用对象名称指定信号、状态和参数特征。对象存在于工作区中，例如，在基础工作区、模型工作区或 Simulink 数据字典中。利用数据对象，您只需更改工作区对象的值，即可对信号、状态和参数特征进行模型范围的更改。

您可以创建数据对象作为数据类的实例。被称为_数据类包_的内存结构体包含数据类定义。内置包 `Simulink` 定义了两个数据类，即 `Simulink.Signal` 和 `Simulink.Parameter`，您可以用它们来创建数据对象。要存储查找表数据以便在查找表模块（例如 n-D Lookup Table）之间共享，您可以使用 `Simulink.LookupTable` 和 `Simulink.Breakpoint` 类。

要决定是否使用数据对象来配置信号，包括 Inport 和 Outport 模块，请参阅[存储信号和状态的设计属性](https://ww2.mathworks.cn/help/simulink/ug/signal-basics.html#bvcdbmy)。

通过定义内置数据类的子类，您可以自定义数据对象属性和方法。有关创建数据类包的详细信息，请参阅[定义数据类](https://ww2.mathworks.cn/help/simulink/ug/simulink-data-class-extension-using-matlab-class-syntax.html)。

### 数据类命名约定

Simulink 使用圆点表示法为数据类命名：

*   _package_ 是包含类定义的包的名称。
    
*   _class_ 是类的名称。
    

这种表示法允许您创建和引用具有相同名称但属于不同包的类。在这种表示法中，包的名称_限定_类的名称。

类和包的名称区分大小写。例如，不能将 `MYPACKAGE.MYCLASS` 和 `mypackage.myclass` 互换来表示同一个类。

### 在 Simulink 模型中使用数据对象

要通过修改工作区或数据字典中的变量为信号、模块参数和状态指定仿真和代码生成选项，请使用数据对象。将这些对象与模型图中的信号、参数和状态相关联。

#### 使用参数对象

您可以使用参数对象（而不是 MATLAB® 数值变量）为模块参数指定值。例如，要创建并使用名为 `myParam` 的 `Simulink.Parameter` 对象来指定 Gain 模块的**增益**参数，请执行以下操作：

1.  在模型中，在**建模**选项卡上，在**设计**下，点击**属性检查器**。
    
2.  在模型中，点击目标 Gain 模块。属性检查器显示模块的属性和参数。
    
3.  将**增益**参数的值设置为 `myParam`。
    
4.  在参数值旁边，点击操作按钮
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/prop_action_ctrl.png)
    
    ，然后选择。
    
5.  在**创建新数据**对话框中，将**值**设置为 `Simulink.Parameter(15.23)`，然后点击**创建**。
    
    `Simulink.Parameter` 对象 `myParam` 出现在基础工作区中。属性对话框显示对象将参数值 `15.23` 存储在**值**属性中。
    
6.  使用属性对话框，通过调整对象属性指定模块参数的其他特征。例如，要指定参数可采用的最小值和最大值，请使用**最小值**和**最大值**属性。
    

现在，**增益**参数在仿真期间使用值 `15.23`。

要通过使用 `Simulink.LookupTable` 和 `Simulink.Breakpoint` 对象共享查找表数据，请参阅[为查找表打包共享断点和表数据](https://ww2.mathworks.cn/help/simulink/ug/using-structure-parameters.html#bvcx49y)。

#### 使用信号对象

您可以将信号线或模块状态（例如 Unit Delay 模块的状态）与信号对象相关联。

**用于信号.** 要使用信号对象控制模型中某个信号的特征，请使用与该信号相同的名称在工作区中创建对象。

1.  在模型中，在**建模**选项卡上，点击**模型数据编辑器**。
    
2.  在模型数据编辑器中，选择**信号**选项卡。
    
3.  在模型中，选择目标信号。模型数据编辑器突出显示与信号对应的行。
    
4.  在模型数据编辑器中的**名称**列中，给定信号名称，如 `mySig`。
    
5.  点击信号名称旁边的
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/prop_action_ctrl.png)
    
    按钮。选择**创建和解析**。
    
6.  在 “创建新数据” 对话框中，将**值**设置为 `Simulink.Signal`。使用**位置**下拉列表选择要存储对象的工作区（默认值为 “`基础工作区`”）。点击**创建**。
    
    目标工作区中出现 `Simulink.Signal` 对象 `mySig`。Simulink 选择信号属性**信号名称必须解析为 Simulink 信号对象**，从而强制模型中的信号使用信号对象存储的属性。要了解如何控制信号名称解析为信号对象的方式，请参阅[符号解析](https://ww2.mathworks.cn/help/simulink/ug/resolving-symbols.html)。
    
    将打开新对象的属性对话框。
    
7.  使用属性对话框指定信号特征。点击**确定**。
    

要以编程方式配置信号：

```
% Create the signal object.
mySig = Simulink.Signal;
mySig.DataType = 'boolean';

% Get a handle to the block port that creates the
% target signal.
portHandles = get_param('myModel/myBlock','portHandles');
outportHandle = portHandles.Outport;

% Specify the programmatic port parameter 'Name'.
set_param(outportHandle,'Name','mySig')

% Set the port parameter 'MustResolveToSignalObject'.
set_param(outportHandle,'MustResolveToSignalObject','on')

```

要以编程方式配置根级别 Outport 模块，您必须使用稍微不同的方法：

```
% Create the signal object.
mySig = Simulink.Signal;
mySig.DataType = 'boolean';

% Specify the programmatic block parameter 'SignalName'.
set_param('myModel/myOutport','SignalName','mySig')

% Set the block parameter 'MustResolveToSignalObject'.
set_param('myModel/myOutport','MustResolveToSignalObject','on')

```

**用于状态.** 您可以使用信号对象来控制模块（例如 Discrete-Time Integrator 模块）的状态特征。

1.  在模型中，在**建模**选项卡上，点击**模型数据编辑器**。
    
2.  在模型数据编辑器中，选择**状态**选项卡。
    
3.  在模型中，选择拥有目标状态的模块。模型数据编辑器突出显示与该状态对应的行。
    
4.  在模型数据编辑器中的**名称**列中，给定状态名称，例如 `myState`。
    
5.  点击状态名称旁边的
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/prop_action_ctrl.png)
    
    按钮。选择**创建和解析**。
    
6.  在 “创建新数据” 对话框中，将**值**设置为 `Simulink.Signal`。使用**位置**下拉列表选择要存储对象的工作区（默认值为 “`基础工作区`”）。点击**创建**。
    
    `Simulink.Signal` 对象 `myState` 出现在目标工作区中。Simulink 选择模块参数**状态名称必须解析为 Simulink 信号对象**，从而强制模型中的状态使用信号对象存储的属性。要了解如何控制状态名称解析为信号对象的方式，请参阅[符号解析](https://ww2.mathworks.cn/help/simulink/ug/resolving-symbols.html)。
    
    将打开新对象的属性对话框。
    
7.  使用属性对话框指定状态特征。点击**确定**。
    

要以编程方式配置状态：

```
% Create the signal object.
myState = Simulink.Signal;
myState.DataType = 'int16';

% Set the state name in the block.
set_param('myModel/myBlock','StateName','myState')

% Set the port parameter 'StateMustResolveToSignalObject'.
set_param('myModel/myBlock','StateMustResolveToSignalObject','on')

```

### 数据对象属性

要使用数据对象控制参数和信号特征，您可以为数据对象属性指定值。例如，参数和信号数据对象有一个 `DataType` 属性，它决定目标模块参数或信号的数据类型。数据类定义决定数据对象属性的名称、值类型、默认值和有效值范围。

您可以使用模型资源管理器或 MATLAB 命令更改数据对象的属性。

有关信号对象属性的列表，请参阅 [`Simulink.Signal`](https://ww2.mathworks.cn/help/simulink/slref/simulink.signal.html)。有关参数对象属性的列表，请参阅 [`Simulink.Parameter`](https://ww2.mathworks.cn/help/simulink/slref/simulink.parameter.html)。

#### 使用模型资源管理器更改对象的属性

要使用模型资源管理器更改某个对象的属性，请在模型资源管理器的**模型层次结构**窗格中选择包含该对象的工作区。然后在模型资源管理器的**目录**窗格中选择该对象。

模型资源管理器在其**对话框**窗格（如果此窗格可见）中显示对象的属性对话框。

![](https://ww2.mathworks.cn/help/simulink/ug/modexp_edit_props_2010a.png)

您可以配置模型资源管理器，使其在**目录**窗格中显示一个对象的某些属性或所有属性（请参阅**[模型资源管理器](https://ww2.mathworks.cn/help/simulink/slref/modelexplorer.html)**）。要编辑某个属性，请在**目录**或**对话框**窗格中点击该属性的值。该值会替换为一个控制项，允许您更改其中的值。

#### 使用 MATLAB 命令更改对象的属性

您也可以使用 MATLAB 命令获取和设置数据对象属性。请在 MATLAB 命令和程序中使用以下圆点表示法获取和设置数据对象属性：

```
value = obj.property;
obj.property = value;

```

其中 `` `obj` `` 是一个变量，如果该对象是值类的实例，此变量则引用该对象；如果该对象是句柄类的实例，此变量则引用该对象的句柄（请参阅[句柄类与值类](https://ww2.mathworks.cn/help/simulink/ug/working-with-data-objects.html#f14-95232)）。`PROPERTY` 是属性的名称，`VALUE` 是属性的值。例如，以下 MATLAB 代码将创建一个数据类型别名对象（即 [`Simulink.AliasType`](https://ww2.mathworks.cn/help/simulink/slref/simulink.aliastype.html) 的实例），并将其基类型设置为 `uint8`：

```
gain = Simulink.AliasType;
gain.BaseType = 'uint8';

```

您可以使用圆点表示法以递归方式获取和设置对象的属性，这些对象又是其他对象的属性值，例如：

```
gain.CoderInfo.StorageClass = 'ExportedGlobal';

```

### 从内置数据类包 `Simulink` 创建数据对象

内置包 `Simulink` 定义了两个数据对象类，即 `Simulink.Parameter` 和 `Simulink.Signal`。您可以通过用户界面或以编程方式创建这些数据对象。

#### 创建数据对象

1.  在模型资源管理器的**模型层次结构**窗格中，选择要包含数据对象的工作区。例如，点击 `Base Workspace`。
    
2.  在工具栏上，点击**添加参数**
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/add_sim_param.gif)
    
    或**添加信号**
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/add_sim_signal.gif)
    
    旁边的箭头。从下拉列表中选择 **Simulink 参数**或 **Simulink 信号**。
    
    基础工作区中将出现一个参数对象或信号对象。新参数对象的默认名称为 `Param`。新信号对象的默认名称为 `Sig`。
    
3.  要创建更多对象，请点击**添加参数**或**添加信号**。
    

要在模型资源管理器工具栏上创建 `Simulink.LookupTable` 和 `Simulink.Breakpoint` 对象，请使用

![](https://ww2.mathworks.cn/help/simulink/ug/lut_button_me.png)

按钮。

#### 以编程方式创建数据对象

```
% Create a Simulink.Parameter object named myParam whose value is 15.23.
myParam = Simulink.Parameter(15.23);

% Create a Simulink.Signal object named mySig.
mySig = Simulink.Signal;

```

#### 将数值变量转换为参数对象

可按如下所示将数值变量转换为 `Simulink.Parameter` 对象。

```
/* Define numeric variable in base workspace
myVar = 5; 
/* Create data object and assign variable value
myObject = Simulink.Parameter(myVar); 


```

### 基于另一个数据类包创建数据对象

您可以创建自己的包，以定义作为 `Simulink.Parameter` 和 `Simulink.Signal` 的子类的自定义数据对象类。通过这种方式，您可以为数据对象添加您自己的属性和方法。如果您拥有 Embedded Coder® 许可证，则可以在包中定义存储类和内存段。有关创建数据类包的详细信息，请参阅[定义数据类](https://ww2.mathworks.cn/help/simulink/ug/simulink-data-class-extension-using-matlab-class-syntax.html)。

#### 基于另一个包创建数据对象

假设您定义了一个数据类包，称为 `myPackage`。您必须在 MATLAB 路径中包含该包所在的文件夹，才能基于该包创建数据对象。

1.  在模型资源管理器的**模型层次结构**窗格中，选择要包含数据对象的工作区。例如，点击 `Base Workspace`。
    
2.  点击**添加参数**
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/add_sim_param.gif)
    
    或**添加信号**
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/add_sim_signal.gif)
    
    旁边的箭头，然后选择**自定义类列表**。
    
3.  在对话框中，选中您需要的类旁边的复选框。例如，选中 `myPackage.Parameter` 和 `myPackage.Signal` 旁边的复选框。点击**确定**。
    
4.  点击**添加参数**或**添加信号**旁边的箭头。选择您要创建的数据对象的类。例如，选择 **myPackage 参数**或 **myPackage 信号**。
    
    基础工作区中将出现一个参数对象或信号对象。新参数对象的默认名称为 `Param`。新信号对象的默认名称为 `Sig`。
    
5.  要从包 `myPackage` 中创建更多数据对象，请再次点击**添加参数**或**添加信号**。
    

#### 以编程方式从另一个包中创建数据对象

假设您定义了一个数据类包，称为 `myPackage`。您必须在 MATLAB 路径中包含该包所在的文件夹，才能基于该包创建数据对象。

```
% Create a myPackage.Parameter object named 
% myParam whose value is 15.23.
myParam = myPackage.Parameter(15.23);

% Create a myPackage.Signal object named mySig.
mySig = myPackage.Signal;

```

### 直接从对话框中创建数据对象

当您打开 “信号属性” 对话框、模块对话框或属性检查器（在**建模**选项卡上，在**设计**下点击**属性检查器**）时，您可以在工作区或数据字典中高效地创建信号或参数数据对象。

#### 基于模块对话框创建参数对象

1.  在对话框中的数值模块参数中，指定所需的数据对象名称。例如，指定名称 `myParam`。
    
2.  点击模块参数值旁边的按钮
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/prop_action_ctrl.png)
    
    。选择**创建**。
    
3.  在**创建新数据**对话框中，将**值**指定为 `Simulink.Parameter`。
    
    也可以指定您创建的数据类的名称，例如 `myPackage.Parameter`。您也可以使用下拉列表，从可用的数据对象类列表中进行选择。
    
4.  将**位置**指定为 “`基础工作区`”，然后点击**创建**。
    
    您可以使用**位置**选项选择要包含新数据对象的工作区。如果一个模型链接了数据字典，您可以选择在字典中创建数据对象。
    
5.  在打开的对话框中，配置数据对象属性。在**值**框中为参数指定一个数值。点击**确定**。
    
    参数对象 `myParam` 出现在基础工作区中。
    
6.  在模块参数对话框中，点击**确定**。
    

#### 基于 “信号属性” 对话框创建信号对象

1.  在**信号名称**框中，指定一个信号名称，例如 `mySig`。点击**应用**。
    
2.  点击**信号名称**值旁边的按钮
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/prop_action_ctrl.png)
    
    。选择**创建和解析**。
    
3.  在**创建新数据**对话框中，将**值**指定为 `Simulink.Signal`。
    
    也可以指定您创建的数据类的名称，例如 `myPackage.Signal`。同样，您可以从下拉列表中选择 MATLAB 路径中存在的数据对象类。
    
4.  将**位置**指定为 “`基础工作区`”，然后点击**创建**。
    
    您可以使用**位置**选项选择要包含新数据对象的工作区。如果一个模型链接了数据字典，您可以选择在字典中创建数据对象。
    
5.  在打开的对话框中，配置数据对象属性，然后点击**确定**。
    
    信号对象 `mySig` 出现在基础工作区中。在 “信号属性” 对话框中，**信号名称必须解析为 Simulink 信号对象**属性处于选中状态。
    

### 使用数据对象向导为模型创建数据对象

要在模型中创建表示信号、参数和状态的数据对象，可以使用数据对象向导。向导将在模型中查找没有对应数据对象的数据。

根据模型中的设定，向导将创建对象并指定以下特征：

*   信号、参数或状态名称。
    
*   参数对象的数值。
    
*   数据类型。对于信号对象，包括别名类型，如 `Sumlink.AliasType` 和 `Simulink.NumericType`。
    

1.  在 Simulink 编辑器中，在**建模**选项卡上，在**设计**下，点击**数据对象向导**。
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/dow_dialog.png)
    
2.  在**模型名称**框中，输入要搜索的模型的名称。
    
    默认情况下，此框包含您打开向导时所在模型的名称。
    
3.  在**查找选项**下，选中要创建的数据对象类型旁边的复选框。下表介绍了各种选项。
    
4.  点击**查找**。
    
    数据对象表中显示建议的对象。
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/dow_results.png)
    
5.  （可选）要基于非默认类的数据类创建对象，请选中您要更改其类的对象旁边的复选框。要选择所有对象，请点击**全选**。点击**更改类**。在打开的对话框中，使用**参数**和**信号**旁边的下拉列表选择类。
    
    如果您需要的类没有出现在下拉列表中，请选择 “`自定义类列表`”。在打开的对话框中，选中您需要的类旁边的复选框，然后点击**确定**。
    
    要更改向导用来建议对象的默认参数和信号类，请执行以下操作：
    
    *   在模型资源管理器的**模型层次结构**窗格中，选择一个工作区。例如，选择**基础工作区**。
        
    *   在工具栏上，点击**添加参数**
        
        ![](https://ww2.mathworks.cn/help/simulink/ug/add_sim_param.gif)
        
        或**添加信号**
        
        ![](https://ww2.mathworks.cn/help/simulink/ug/add_sim_signal.gif)
        
        旁边的箭头。
        
    *   从下拉列表中，选择您希望向导使用的类。例如，选择 **myPackage 参数**或 **myPackage 信号**。
        
        选定工作区中将出现一个参数对象或信号对象。新参数对象的默认名称为 `Param`。新信号对象的默认名称为 `Sig`。
        
        下次您使用数据对象向导时，向导将使用您在模型资源管理器中选择的参数或信号类来建议对象。
        
    
6.  选中您要创建的建议对象旁边的复选框。要选择所有建议的对象，请点击**全选**。
    
7.  点击**创建**。
    
    数据对象出现在基础工作区中。如果目标模型链接到一个数据字典，对象将出现在该字典中。
    
    向导根据配置参数 > > > 更改您模型中的设置。
    
    *   如果您将该参数设置为 “`仅显式`”，向导将强制模型中对应的信号和状态解析为新的信号对象。向导在每个 “信号属性” 对话框中选择**信号名称必须解析为 Simulink 信号对象**选项，在每个模块对话框中选择**状态名称必须解析为 Simulink 信号对象**选项。
        
    *   如果您将该参数设置为 “`显式和隐式`” 或 “`显式和隐式(警告)`”，向导不会为任何信号或状态更改**信号名称必须解析为 Simulink 信号对象**或**状态名称必须解析为 Simulink 信号对象**的设置。
        
    
    可以考虑使用函数 [`disableimplicitsignalresolution`](https://ww2.mathworks.cn/help/simulink/slref/disableimplicitsignalresolution.html) 为您的模型关闭隐式信号对象解析。有关详细信息，请参阅 [显式和隐式符号解析](https://ww2.mathworks.cn/help/simulink/ug/resolving-symbols.html#brjnb77-1)。
    

#### 数据对象向导故障排除

*   数据对象向导为代码生成编译模型，以便建议创建信号对象。因此，该向导不能用于对代码生成无效的模型。
    
*   数据对象向导不会建议为模型中的以下实体创建数据对象：
    
    *   多个同名的单独信号。
        
    *   与模块参数中使用的变量同名的信号。
        
    *   缺少单个连续源模块的信号。
        
    *   源模块被注释掉或被注释禁止的信号。
        
    *   Variant Source 和 Variant Sink 模块中显示为非活动的数据项。向导仅为与活动模块关联的数据项建议对象。
        
    *   当您将模型配置参数**信号解析**设置为 “`无`” 时的信号和状态。
        
    

### 以编程方式从外部数据源创建数据对象

此示例说明如何使用脚本基于外部数据源（例如 Microsoft® Excel® 文件）创建数据对象。

1.  创建一个新的 MATLAB 脚本文件。
    
2.  将相关信息放置在该文件中，以描述外部文件中您希望转换为数据对象的数据。例如，以下信息将创建两个具有指定属性的 `Simulink` 数据对象。第一个创建参数对象，第二个创建信号对象：
    
    ```
    % Parameters
    ParCon = Simulink.Parameter;
    ParCon.CoderInfo.StorageClass = 'Custom'
    ParCon.CoderInfo.CustomStorageClass ='Const';
    ParCon.Value = 3;
    % Signals
    SigGlb = Simulink.Signal;
    SigGlb.DataType = 'int8';
    
    ```
    
3.  运行脚本文件。数据对象出现在 MATLAB 工作区中。
    

如果要从外部源导入目标数据，您可以编写 MATLAB 函数和脚本，以读取信息、将信息转换为数据对象，然后将对象加载到基础工作区中。

您可以使用以下函数与 MATLAB 的外部文件进行交互：

*   `xmlread`
    
*   `xmlwrite`
    
*   `xlsread`
    
*   `xlswrite`
    
*   `csvread`
    
*   `csvwrite`
    
*   `dlmread`
    
*   `dlmwrite`
    

### 数据对象方法

数据类可以定义函数（称为方法），用于创建和操作所定义的对象。类可以定义以下任何类型的方法。

#### 动态方法

所谓动态方法，是指方法的恒等式取决于其名称，还取决于被隐式或显式指定为第一个参数的对象的类。您可以使用函数或圆点表示法来指定此对象，它必须是定义该方法的类的实例，或者定义该方法的类的子类的实例。例如，假设类 `A` 定义了名为 `setName` 的方法，它为 `A` 的实例指定名称。再假设 MATLAB 工作区中包含指定给变量 `obj` 的 A 的实例。那么，您可以使用以下任一语句将名称 `'foo'` 指定给 `obj`：

```
obj.setName('foo');
setName(obj, 'foo');

```

类可以定义一组方法，这组方法可以与该类的某个超类定义的方法同名。在这种情况下，子类定义的方法将覆盖父类定义的方法的行为。Simulink 将根据您指定作为第一个参数或隐式参数的对象的类来决定要在运行时调用哪个方法。这就是动态方法的由来。

**注意**

大多数 Simulink 数据对象方法都是动态方法。除非一个方法的说明文档中另有指定，否则，您可以认为它就是动态方法。

#### 静态方法

所谓静态方法，是指方法的身份只取决于其名称，因此不能在运行时更改。要调用静态方法，请使用其完全限定名称，包括定义该方法的类的名称，后跟该方法自身的名称。例如，[`Simulink.ModelAdvisor`](https://ww2.mathworks.cn/help/simulink/slref/simulink.modeladvisor.html) 类定义了一个名为 `getModelAdvisor` 的静态方法。此静态方法的完全限定名称为 `Simulink.ModelAdvisor.getModelAdvisor`。以下示例说明如何调用静态方法。

```
ma = Simulink.ModelAdvisor.getModelAdvisor('vdp');

```

#### 构造函数

每个数据类都会定义一个方法，用于创建该类的实例。该方法的名称与类的名称相同。例如，`Simulink.Parameter` 类的构造函数的名称为 `Simulink.Parameter`。Simulink 数据类定义的构造函数不接受任何参数。

构造函数返回的值取决于其类是句柄类还是值类。对于句柄类的构造函数，如果它创建的实例的类为句柄类，则返回该实例的句柄，否则，将返回该实例本身（请参阅[句柄类与值类](https://ww2.mathworks.cn/help/simulink/ug/working-with-data-objects.html#f14-95232)）。

### 句柄类与值类

Simulink 类（包括数据对象类）分为两种：值类和句柄类。

#### 关于值类

_值_类的构造函数（请参阅[构造函数](https://ww2.mathworks.cn/help/simulink/ug/working-with-data-objects.html#f14-93153)）返回类的实例，该实例与其最初指定给的 MATLAB 变量永久关联。将变量重新指定给函数将导致 MATLAB 创建一个原始对象的副本并将其指定给函数，而将变量传递给函数则会令 MATLAB 传递原始对象的副本。

例如，[`Simulink.NumericType`](https://ww2.mathworks.cn/help/simulink/slref/simulink.numerictype.html) 就是一个值类。执行以下语句

```
x = Simulink.NumericType;
y = x;

```

将在工作区中创建类 `Simulink.NumericType` 的两个实例，一个指定给变量 `x`，另一个指定给 `y`。

#### 关于句柄类

_句柄_类的构造函数返回句柄对象。句柄可以指定给多个变量，也可以传递给函数，而不会导致创建原始对象的副本。例如，[`Simulink.Parameter`](https://ww2.mathworks.cn/help/simulink/slref/simulink.parameter.html) 类就是一个句柄类。执行

```
x = Simulink.Parameter;
y = x;

```

将只在 MATLAB 工作区中创建 `Simulink.Parameter` 类的一个实例。变量 x 和 y 都通过句柄引用该实例。

程序可以通过修改引用实例的任何变量来修改句柄类的实例，例如，继续之前的示例：

```
x.Description = 'input gain';
y.Description


```

大多数 Simulink 数据对象类都是值类。例外情况包括 [`Simulink.Signal`](https://ww2.mathworks.cn/help/simulink/slref/simulink.signal.html) 和 [`Simulink.Parameter`](https://ww2.mathworks.cn/help/simulink/slref/simulink.parameter.html) 类。

要确定变量的值是对象（值类）还是对象的句柄，请参阅[确定对象是否为句柄](https://ww2.mathworks.cn/help/matlab/matlab_oop/handle-objects.html#btm3mqa)。

#### 复制句柄对象

可以使用句柄对象的 copy 方法创建对象实例的副本。例如，[`ConfigSet`](https://ww2.mathworks.cn/help/simulink/slref/simulink.configset.html) 是代表模型配置集的句柄对象。以下代码将为当前模型的活动配置集创建副本，并将其作为备用配置附加到模型中，以满足模型开发的需要。

```
activeConfig = getActiveConfigSet(gcs);
develConfig = copy(activeConfig);
develConfig.Name = 'develConfig';
attachConfigSet(gcs, develConfig);

```

### 比较数据对象

Simulink 数据对象提供了一个名为 `isequal` 的方法，它可以确定对象属性值是否相等。此方法对两个对象的属性值进行比较，如果所有值都相等，则返回 true (`1`)，否则，将返回 false (`0`)。例如，以下代码实例化两个信号对象（A 和 B），并为特定属性指定值。

```
A = Simulink.Signal;
B = Simulink.Signal;
A.DataType = 'int8';
B.DataType = 'int8';
A.InitialValue = '1.5';
B.InitialValue = '1.5';

```

然后使用 `isequal` 方法验证 A 和 B 的对象属性是否相等。

### 解决代码生成的信号对象配置中的冲突

如果在 “信号属性” 对话框中定义了信号，并使用命令行或模型资源管理器定义了同名的信号对象，则当 Simulink 引擎尝试解析表示信号名称的符号时，可能会出现多义性。解决多义性的方法之一是指定将信号解析为 `Simulink.Signal` 数据对象。选中 “信号属性” 对话框中的**信号名称必须解析为 Simulink 信号对象**选项。

要配置信号数据，请使用 Code Mappings 编辑器或代码映射 API 将信号添加到模型代码映射，并设置存储类和存储类属性。对于 Simulink Coder™，请参阅 [Configure Signal Data for C Code Generation](https://ww2.mathworks.cn/help/rtw/ug/configure-signals-for-code-generation.html) (Simulink Coder)。对于 Embedded Coder，请参阅 [Configure Signal Data for C Code Generation](https://ww2.mathworks.cn/help/ecoder/ug/configure-signals-for-code-generation.html) (Embedded Coder)。

### 创建持久数据对象

要保留数据对象，使它们在您关闭 MATLAB 后依然存在，您可以：

*   将对象存储在数据字典或模型工作区中。要决定在何处永久存储模型数据，请参阅[确定在何处存储 Simulink 模型的变量和对象](https://ww2.mathworks.cn/help/simulink/ug/determine-where-to-store-data-for-simulink-models.html)。
    
*   使用 `save` 命令将数据对象保存到 MAT 文件中，并在同一个会话或以后的会话中使用 `load` 命令将它们还原到 MATLAB 基础工作区中。配置模型，以便在加载模型时从 MAT 文件或脚本文件中加载对象。
    

要在加载模型时从文件中加载数据对象，可以编写一个脚本，以创建对象并配置对象属性。也可以将对象保存到 MAT 文件中。然后使用脚本或加载命令作为使用这些对象的模型的 `PreLoadFcn` 回调例程。假设您将数据对象保存在名为 `data_objects.mat` 的文件中，而使用这些对象的模型已打开并处于活动状态。则在命令提示符下输入：

```
set_param(gcs, 'PreLoadFcn', 'load data_objects');

```

会将 `load data_objects` 设置为模型的预加载函数。每次您打开模型时，数据对象都会出现在基础工作区中。

对于保存的对象，它们的类定义必须存在于 MATLAB 路径中才能还原这些对象。如果保存对象之后对象的类又获得新属性，则 Simulink 会将新属性添加到还原的对象版本中。如果保存对象之后类的属性丢失，则只还原依然存在的属性。

## 另请参阅

[`Simulink.Signal`](https://ww2.mathworks.cn/help/simulink/slref/simulink.signal.html) | [`Simulink.Parameter`](https://ww2.mathworks.cn/help/simulink/slref/simulink.parameter.html) | [`disableimplicitsignalresolution`](https://ww2.mathworks.cn/help/simulink/slref/disableimplicitsignalresolution.html) | [`Simulink.LookupTable`](https://ww2.mathworks.cn/help/simulink/slref/simulink.lookuptable-class.html) | [`Simulink.Breakpoint`](https://ww2.mathworks.cn/help/simulink/slref/simulink.breakpoint-class.html)

## 相关主题

*   [确定在何处存储 Simulink 模型的变量和对象](https://ww2.mathworks.cn/help/simulink/ug/determine-where-to-store-data-for-simulink-models.html)
*   [创建、编辑和管理工作区变量](https://ww2.mathworks.cn/help/simulink/ug/create-edit-and-manage-workspace-variables.html)
*   [定义数据类](https://ww2.mathworks.cn/help/simulink/ug/simulink-data-class-extension-using-matlab-class-syntax.html)
*   [使用 Simulink.Signal 对象指定和控制信号属性](https://ww2.mathworks.cn/help/simulink/ug/sim-signal-property-dialog-box.html)
*   [什么是数据字典？](https://ww2.mathworks.cn/help/simulink/ug/what-is-a-data-dictionary.html)
*   [Configure Generated Code According to Interface Control Document Specifications](https://ww2.mathworks.cn/help/ecoder/ug/configure-generated-code-according-to-ICD-interactively.html) (Embedded Coder)
*   [符号解析](https://ww2.mathworks.cn/help/simulink/ug/resolving-symbols.html)