---
url: https://ww2.mathworks.cn/help/simulink/ug/how-to-mask-a-block.html
title: 创建简单封装
- MATLAB & Simulink
- MathWorks 中国
date: 2023-07-28 08:26:34
tag: 
summary: 创建和自定义模块封装。
---
## 创建简单封装

您可以使用封装编辑器以交互方式封装模块，也可以编程方式封装模块。此示例说明如何使用**封装编辑器**来封装模块。要以编程方式封装模块，请参阅[以编程方式控制封装](https://ww2.mathworks.cn/help/simulink/ug/control-masks-programmatically.html)。

有关封装的示例，请参阅 [Simulink 封装示例](matlab:open_system([matlabroot '/examples/simulink_masking/main/slexMaskingExamples']))。

### 步骤 1：打开封装编辑器

1.  打开您要在其中封装模块的模型。例如，打开 `[subsystem_example](matlab:open_system([matlabroot '/examples/subsystem_reference/main/subsystem_example']))`。
    
    此模型包含一个 Subsystem 模块，该模块为直线方程建模：`y = mx + b`。
    
2.  选择 Subsystem 模块，在**子系统**选项卡上，在**封装**组中，点击**创建掩膜**。
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/maskexeditora3cc21232fd49ce251350a84fee9f6ad_zh_CN.png)
    

### 步骤 2：定义封装

**封装编辑器**包含四个选项卡，可让您定义模块封装和自定义封装对话框。

有关每个窗格的详细信息，请参阅 [封装编辑器概述](https://ww2.mathworks.cn/help/simulink/gui/mask-editor-overview.html)。

#### “参数和对话框” 选项卡

使用此选项卡向封装对话框添加参数、显示和操作等控制项。

向模块封装添加**编辑**框。

1.  在左窗格的**参数**下，点击**编辑**两次以向**对话框**窗格中添加两个新行。
    
2.  在**提示**列中，为两个**编辑**参数键入 `Slope` 和 `Intercept`。您在**提示**列中输入的值将出现在封装对话框中。同样，在**名称**列中键入 `m` 和 `b`。您在**名称**列中输入的值是封装参数名称。封装参数名称必须是有效的 MATLAB® 名称。
    
3.  在右窗格的**属性编辑器**下，为**属性**、**对话框**和**布局**部分中的字段提供输入值。
    
4.  点击**应用**。
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/maskexeditor9778ae7fb1274b08ff4f7f3982ff4178_zh_CN.png)
    
5.  要预览封装对话框而不退出封装编辑器，请点击**预览**。
    

有关详细信息，请参阅 [“参数和对话框” 窗格](https://ww2.mathworks.cn/help/simulink/gui/mask-editor-overview.html#bu085yl)。

**注意**

Simulink® 封装参数无法引用同一封装上的另一个参数。

#### 代码窗格

使用此窗格指定 MATLAB 代码来控制封装参数。例如，您可以为封装参数提供预定义的值。

以示例中的方程 `y = mx + b` 为例。要设置对应于'm' 的子模块的值，可以使用代码窗格中的 `set_param` 函数。

![](https://ww2.mathworks.cn/help/simulink/ug/initialization_pane.gif)

#### 图标窗格

使用此选项卡创建模块封装图标。封装编辑器为您提供两种创建模块图标的模式。

**图形图标编辑器**：您可以通过图形环境创建和编辑模块的封装图标。图形图标编辑器中的各种功能可以帮助您轻松创建图标。从封装编辑器启动图形图标编辑器。

交互式图形环境提供了一系列图形工具，如钢笔、曲率、文本、剪刀、连接器和方程（支持 LaTeX），以创建丰富的图形图标。网格、智能参考线和标尺帮助您创建像素完美的图标。除了提供绘图工具外，还有一些内置形状，如电阻、电感和旋转阻尼，也都是立即可用的。有关详细信息，请参阅 [Create and Edit Masked Block Icon Using Graphical Icon Editor](https://ww2.mathworks.cn/help/simulink/ug/graphical-icon-editor.html)

**封装绘制命令**：使用一组封装绘制命令创建封装模块图标。您可以使用左侧的**属性**窗格指定图标属性和图标可见性。有关详细信息，请参阅[图标绘制命令](https://ww2.mathworks.cn/help/simulink/gui/mask-editor-overview.html#mw_03e8e168-b37f-4722-a9cb-79a287f8cfe9)。

您可以创建静态或动态模块封装图标。有关详细信息，请参阅[绘制封装图标](https://ww2.mathworks.cn/help/simulink/ug/create-mask-icon.html)和 [slexMaskDisplayAndInitializationExample](matlab:open_system([matlabroot '/examples/simulink_masking/main/slexMaskDisplayAndInitializationExample']))。

#### 约束窗格

封装参数约束帮助您创建对封装参数进行的验证，而不必编写您自己的代码。有三种类型的约束：参数约束、交叉参数约束和端口约束。

![](https://ww2.mathworks.cn/help/simulink/ug/constraints_zh_CN.png)

**参数约束**：封装可以包含接受用户输入值的参数。您可以使用封装对话框为封装参数提供输入值。约束确保封装参数的输入在指定的范围内。例如，假设有一个封装的 Gain 模块。您可以设置一个约束，使输入值必须介于 1 和 10 之间。如果您提供的输入超出指定的范围，将显示错误。左窗格中的约束浏览器可帮助您管理共享约束。

**交叉参数约束**：交叉参数约束应用于两个或更多的**编辑**或**组合框**类型的封装参数。当您要指定诸如 Parameter1 必须大于 Parameter2 之类的情况时，可以使用交叉参数约束。

**端口约束**：您可以对封装模块的输入端口和输出端口指定约束。编译模型时，会对照约束检查端口属性。

### 步骤 3：对封装进行操作

1.  您可以预览封装，还可以选择取消模块封装或编辑模块封装。
    
2.  双击封装模块。
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/maskexdialogbox_zh_CN.png)
    
    随即显示封装对话框。
    
3.  在封装对话框的 `Slope` 和 `Intercept` 框中键入值。要查看输出，请对模型进行仿真。
    
4.  点击**确定**。
    
5.  要编辑封装定义，请选择子系统模块，然后从工具条中的 “子系统” 选项卡中点击**编辑封装**。有关详细信息，请参阅 [Manage Existing Masks](https://ww2.mathworks.cn/help/simulink/ug/operate-on-existing-masks.html)。
    
6.  选择封装模块，并在 **Subsystem 模块**选项卡上的**封装**组中，点击**查看封装内部**以查看：
    
    *   封装子系统内的模块
        
    *   封装模块的内置模块对话框
        
    *   链接的封装模块的基础封装对话框
        
    

## 相关主题

*   [创建模块封装](https://ww2.mathworks.cn/help/simulink/block-masks.html)
*   [创建封装：封装基础知识（3 分 46 秒）](https://www.mathworks.com/videos/creating-a-mask--masking-fundamentals-1480968643715.html)
*   [封装编辑器概述](https://ww2.mathworks.cn/help/simulink/gui/mask-editor-overview.html)
*   [封装基础知识](https://ww2.mathworks.cn/help/simulink/ug/block-masks.html)

本页内容对您有帮助吗？

Unrated

1 star

2 stars

3 stars

4 stars

5 stars

 Unrated  1 star  2 stars  3 stars  4 stars  5 stars

您点击的链接对应于以下 MATLAB 命令：

请在 MATLAB 命令行窗口中直接输入以执行命令。Web 浏览器不支持 MATLAB 命令。