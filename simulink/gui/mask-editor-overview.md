---
url: https://ww2.mathworks.cn/help/simulink/gui/mask-editor-overview.html
title: 封装编辑器概述
- MATLAB & Simulink
- MathWorks 中国
date: 2023-07-28 08:28:42
tag: 
summary: 使用封装编辑器界面创建和自定义封装。
---
## 封装编辑器概述

封装是一种自定义模块用户界面，它可隐藏模块内容，使用它自己的图标和参数对话框将内容以原子块的形式向用户显示。

**封装编辑器**对话框可帮助您创建和自定义模块封装。当您创建或编辑封装时，**封装编辑器**对话框会打开。您可以通过以下任一方式访问**封装编辑器**对话框：

要创建封装，请执行下列操作：

* 在**建模**选项卡中的**组件**下，点击**创建系统封装**。
* 选择模块，并在**模块**选项卡上，在**封装**组中，点击**创建掩膜**。封装编辑器随即打开。

要编辑封装，请执行下列操作：

* 在**模块**选项卡的**封装**组中，点击**编辑封装**。
* 右键点击模块并选择 > 。

**注意**

您也可以使用键盘快捷方式 **Ctrl + M** 打开封装编辑器。

**封装编辑器**对话框包含一系列选项卡窗格，其中每个窗格允许您定义一项封装功能。这些选项卡是：

### “参数和对话框” 窗格

* [控件](#bu085z0)
* [对话框](#bu08562)
* [属性编辑器](#bu085_d)
* [“文档” 窗格](#bu086gp)
* [类型](#mw_bd7bd3d6-188a-4253-bb52-477fd8be0ef6)
* [描述](#mw_ec4a38d2-e47c-48e6-ad70-b96a9ef5be11)
* [帮助](#mw_11638814-6b4a-4966-8b73-9ca16ff2ff64)
* [提供 URL](#mw_22df3cd1-b1c8-4c95-8c45-b44fce84a04c)
* [提供一个 `web` 命令](#mw_930359cf-a986-47a7-bdeb-19f57682660d)
* [提供一个 `eval` 命令](#mw_e072f84c-8078-46f6-be04-0b1b6c2dec0a)
* [提供文字或 HTML 文本](#mw_dbe6a6cc-4705-4aa3-a491-0d9620bc3431)

通过**参数和对话框**窗格，您可以使用**参数**、**显示**和**动作**选项板中的对话框控件设计封装对话框。

![](https://ww2.mathworks.cn/help/simulink/gui/parameters_pane_empt.png)

**参数和对话框**窗格分为以下各部分：

**“参数和对话框” 窗格**

<table summary="“参数和对话框”窗格"><colgroup><col width="25%"><col width="25%"><col width="17%"><col width="33%"></colgroup><thead><tr><th>部分</th><th>部分描述</th><th>子部分</th><th>子部分描述</th></tr></thead><tbody><tr><td rowspan="4"><a href="https://ww2.mathworks.cn/help/simulink/gui/mask-editor-overview.html#bu085z0">控件</a></td><td rowspan="4">参数是封装对话框中用户可与之交互以添加或处理数据的元素。</td><td>参数</td><td>参数是参与仿真的用户输入。<strong>参数</strong>选项板具有一组参数对话框控件，您可以将它们添加到封装对话框中。</td></tr><tr><td>容器</td><td>&nbsp;</td></tr><tr><td>显示</td><td><strong>显示</strong>选项板上的<strong>控件</strong>允许您在封装对话框中将对话框控件分组，并显示文本和图像</td></tr><tr><td>操作</td><td>“操作” 控件允许您在封装对话框中执行一些操作。例如，您可以点击封装对话框中的超链接或按钮。</td></tr><tr><td><a href="https://ww2.mathworks.cn/help/simulink/gui/mask-editor-overview.html#bu08562">对话框</a></td><td>也可以点击对话框控件或将其从选项板拖放到<strong>对话框</strong>以创建封装对话框。</td><td>不适用</td><td>不适用</td></tr><tr><td rowspan="4"><a href="https://ww2.mathworks.cn/help/simulink/gui/mask-editor-overview.html#bu085_d">属性编辑器</a></td><td rowspan="4"><strong>属性编辑器</strong>允许您查看和设置<strong>参数</strong>、<strong>显示</strong>、<strong>容器</strong>和<strong>动作</strong>控件的属性。</td><td>属性</td><td>定义所有对话框控件的基本信息，例如<strong>名称</strong>、<strong>值</strong>、<strong>提示</strong>和<strong>类型</strong>。</td></tr><tr><td>属性</td><td>定义封装对话框控件的解释方式。属性仅与参数相关。</td></tr><tr><td>对话框</td><td>定义对话框控件在封装对话框中的显示方式。</td></tr><tr><td>布局</td><td>定义对话框控件在封装对话框上的布局。</td></tr></tbody></table>

#### 控件

控件部分被进一步划分为 “参数”、“显示” 和“操作”部分。[控件表](https://ww2.mathworks.cn/help/simulink/gui/mask-editor-overview.html#mw_177993f1-32e4-4ec5-b433-583110a3de08)列出了不同控件及其描述。

**控件表**

<table summary="控件表"><colgroup><col width="33%"><col width="33%"><col width="34%"></colgroup><thead><tr><th colspan="2">控件</th><th>描述</th></tr></thead><tbody><tr><td colspan="3"><p><strong>参数</strong></p></td></tr><tr><td><p><span></span></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/edit.png"></div><p></p></td><td><p>编辑</p></td><td><p>允许您通过在字段中键入来输入参数值。</p><p>您可以将约束与 <code>Edit</code> 参数相关联。</p></td></tr><tr><td><p><span></span></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/checkbox.png"></div><p></p></td><td><p>复选框</p></td><td><p>接受布尔值。</p></td></tr><tr><td><p><span></span></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/popup1.png"></div><p></p></td><td><p>弹出</p></td><td><p>允许您从可能值列表中选择参数值。当您选中<strong>计算</strong>复选框时，关联的变量会保存所选项的索引。请注意，索引从 1 开始，而不是从 0 开始。禁用<strong>计算</strong>时，关联的变量将保存所选项的字符串。</p></td></tr><tr><td><p><span></span></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/popup.png"></div><p></p></td><td><p>组合框</p></td><td><p>允许您从可能值列表中选择参数值。您也可以键入此列表中的值或不在此列表中的值。当您选中<strong>计算</strong>复选框时，关联的变量会保存所选项的实际值。</p><p>您可以将约束与 “组合框” 参数相关联。</p><p>有关详细信息，请参阅 <a href="matlab:open_system([matlabroot '/examples/simulink_masking/main/slexMaskParameterOptionsExample'])" target="_blank">slexMaskParameterOptionsExample</a> 中的组合框示例。</p></td></tr><tr><td><p><span></span></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/listbox.png"></div><p></p></td><td><p>列表框</p></td><td><p>允许您创建参数值列表。所有可能值的选项都显示在封装对话框中。您可以从中选择多个值。</p></td></tr><tr><td><p><span></span></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/radiobtn.png"></div><p></p></td><td><p>单选按钮</p></td><td><p>允许您从可能值列表中选择参数值。单选按钮的所有选项都显示在封装对话框中。</p></td></tr><tr><td><p><span></span></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/slider.png"></div><p></p></td><td><p>滑块</p></td><td><p>允许您滑动到由最小值和最大值定义的范围内的值。<strong>滑块</strong>参数可以接受数字或变量名称形式的输入。如果指定的变量是基础工作区或模型工作区变量，则您可以通过<strong>滑块</strong>调整变量值。</p>您可以使用<strong>轴刻度</strong>下拉菜单调整线性刻度或对数刻度中的值。<p>还可以动态控制滑块范围。有关详细信息，请参阅 <a href="matlab:open_system([matlabroot '/examples/simulink_masking/main/slexMaskParameterOptionsExample'])" target="_blank">slexMaskParameterOptionsExample</a>。</p><div><p><strong>注意</strong></p><p>为 “滑块” 指定的值将自动应用。</p></div></td></tr><tr><td><p><span></span></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/dial.png"></div><p></p></td><td><p>刻度盘</p></td><td><p>允许您拨动到由最小值和最大值定义的范围内的值。<strong>对话框</strong>参数可以接受数字或变量名称形式的输入。如果指定的变量是基础工作区或模型工作区变量，则您可以通过<strong>对话框</strong>调整变量值。</p>您可以使用<strong>轴刻度</strong>下拉菜单调整线性刻度或对数刻度中的值。<p>还可以动态控制刻度盘范围。有关详细信息，请参阅 <a href="matlab:open_system([matlabroot '/examples/simulink_masking/main/slexMaskParameterOptionsExample'])" target="_blank">slexMaskParameterOptionsExample</a>。</p><div><p><strong>注意</strong></p><p>为 “刻度盘” 指定的值将自动应用。</p></div></td></tr><tr><td><p><span></span></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/spinbox.png"></div><p></p></td><td><p>微调框</p></td><td><p>允许您在由最小值和最大值定义的范围内微调值。您可以指定值的步长。</p><div><p><strong>注意</strong></p><p>为 “微调框” 指定的值将自动应用。</p></div></td></tr><tr><td><p><span></span></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/datatypestr.png"></div><p></p></td><td><p>数据类型</p></td><td><p>允许您指定封装参数的数据类型。您可以将<strong>最小值</strong>、<strong>最大值</strong>和<strong>编辑</strong>参数与数据类型参数相关联。有关详细信息，请参阅<a href="https://ww2.mathworks.cn/help/simulink/gui/datatypestr-parameter.html">使用 “数据类型字符串” 参数指定数据类型</a>。</p></td></tr><tr><td><p><span></span></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/min.png"></div><p></p></td><td><p>最小值</p></td><td><p>指定<strong>数据类型字符串</strong>参数的最小值。</p></td></tr><tr><td><p><span></span></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/max.png"></div><p></p></td><td><p>最大值</p></td><td><p>指定<strong>数据类型字符串</strong>参数的最大值。</p></td></tr><tr><td><p><span><span></span></span></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/unit_parameter_icon.png"></div><p></p></td><td>单位</td><td><p>允许您为封装模块的输出值或输入值设置测量单位。参数<strong>单位</strong>可以接受任何测量单位作为输入。例如，表示角速度的 rad/sec，表示加速度的 meters/sec<sup>2</sup>，或表示距离的 km 或 m。有关详细信息，请参阅 <a href="matlab:open_system([matlabroot '/examples/simulink_masking/main/slexMaskParameterOptionsExample'])" target="_blank">slexMaskParameterOptionsExample</a>。</p></td></tr><tr><td><p><span><span></span></span></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/table.png"></div><p></p></td><td><p>自定义表</p></td><td>允许您在封装对话框中添加表。您可以在属性编辑器的<strong>值</strong>部分将值添加为嵌套元胞数组。有关详细信息，请参阅 <a href="matlab:open_system([matlabroot '/examples/simulink_masking/main/slexMaskParameterOptionsExample'])" target="_blank">slexMaskParameterOptionsExample</a>。</td></tr><tr><td><p><span></span></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/promoteadd.png"></div><p></p></td><td><p>一到一提升</p></td><td><p>允许您有选择地将模块参数从底层模块提升到封装层。点击<strong>一到一提升</strong>，打开<strong>提升的参数选择器</strong>对话框。在此对话框中，您可以选择要提升的模块参数。点击<strong>确定</strong>将其关闭。</p></td></tr><tr><td><p><span></span></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/promoteall.png"></div><p></p></td><td><p>多到一提升</p></td><td><p>允许您将所有底层模块参数提升到封装层。当您提升所有参数时，提升操作将删除之前已提升的参数。</p></td></tr><tr><td colspan="3"><p><strong>容器</strong></p></td></tr><tr><td><p><span></span></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/groupbox.png"></div><p></p></td><td><p>组框</p></td><td><p>将封装对话框中的其他对话框控件和容器进行分组的容器。</p></td></tr><tr><td><p><span></span></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/tab.png"></div><p></p></td><td><p>选项卡</p></td><td><p>将封装对话框中的对话框控件进行分组的选项卡。选项卡包含在选项卡容器中。一个选项卡容器可以有多个选项卡。</p></td></tr><tr><td><span><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/table.png"></div></span></td><td><p>表</p></td><td><p>以表格形式对<strong>编辑</strong>、<strong>复选框</strong>和<strong>弹出</strong>参数进行分组的容器。您还可以对<strong>表</strong>容器中列出的内容进行搜索和排序。</p><p>有关详细信息，请参阅 <a href="matlab:open_system([matlabroot '/examples/simulink_masking/main/slexMaskLayoutOptionsExample'])" target="_blank">Dialog Layout Options</a> 和 <a href="https://ww2.mathworks.cn/help/simulink/ug/handling-large-number-of-mask-parameters.html" hreflang="en">Handling Large Number of Mask Parameters</a> 中的表示例。</p></td></tr><tr><td><p><span></span></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/collapsiblepanel.png"></div><p></p></td><td><p>可折叠面板</p></td><td><p>将对话框控件进行分组的容器，与<strong>面板</strong>类似。您可以选择展开或折叠<strong>可折叠面板</strong>对话框控件。</p><p>有关详细信息，请参阅 <a href="matlab:open_system([matlabroot '/examples/simulink_masking/main/slexMaskLayoutOptionsExample'])" target="_blank">Dialog Layout Options</a> 中的可折叠面板示例。</p></td></tr><tr><td><p><span></span></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/panel.png"></div><p></p></td><td><p>面板</p></td><td><p>对话框控件组的容器。可使用<strong>面板</strong>对对话框控件进行逻辑分组。</p></td></tr><tr><td colspan="3"><p><strong>显示</strong></p></td></tr><tr><td><p><span></span></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/label.png"></div><p></p></td><td><p>文本</p></td><td><p>封装对话框中显示的文本。</p></td></tr><tr><td><p><span></span></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/image.png"></div><p></p></td><td><p>图像</p></td><td><p>封装对话框中显示的图像。</p></td></tr><tr><td><p><span></span></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/editarea.png"></div><p></p></td><td><p>文本区域</p></td><td>在封装对话框中添加自定义文本或 MATLAB 代码。</td></tr><tr><td><p><span><span></span></span></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/listbox.png"></div><p></p></td><td><p>列表框控件</p></td><td><p>允许您从可能值列表中选择值。您可以选择多个值（<strong>Ctrl</strong> + 点击）。</p></td></tr><tr><td><span><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/tree_control_icon.png"></div></span></td><td><p>树控件</p></td><td><p>允许您从可能值的层次结构树中选择值。您可以选择多个值（<strong>Ctrl</strong> + 点击）。</p></td></tr><tr><td><div><p></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/lutcontrol.png"></div><p></p></div></td><td>查找表控件</td><td>允许您可视化 N 维表和断点数据</td></tr><tr><td colspan="3"><p><strong>操作</strong></p></td></tr><tr><td><p><span></span></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/hyperlink.png"></div><p></p></td><td><p>超链接</p></td><td><p>封装对话框中显示的超链接文本。</p></td></tr><tr><td><p><span></span></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/pushbutton.png"></div><p></p></td><td><p>按钮</p></td><td><p>封装对话框上的按钮控件。您可以对按钮编程使其执行特定操作。还可以在按钮控件上添加图像。有关详细信息，请参阅 <a href="matlab:open_system([matlabroot '/examples/simulink_masking/main/slexMaskParameterOptionsExample'])" target="_blank">slexMaskParameterOptionsExample</a>。</p></td></tr></tbody></table>

#### 对话框

您可以通过将对话框控件从**控件**部分拖到**参数和对话框**选项卡，来构建对话框控件的层次结构。您也可以点击**控件**部分的选项板，将所需控件添加到**参数和对话框**选项卡中。您可以在**参数和对话框**选项卡中添加最多 32 级的层次结构。

**参数和对话框**显示三个字段：**类型**、**提示**和**名称**。

* **类型**字段显示对话框控件的类型，但不能对其进行编辑。它还显示参数对话框控件的序列号。
* **提示**字段显示对话框控件的提示文本。
* **名称**字段将自动填充，用于唯一地标识对话框控件。您可以选择在**名称**字段中添加不同的值（有效的 MATLAB 名称），但不能与内置参数名称相同。

在**对话框**上，**参数**控件以浅蓝色背景显示，而**显示**和**动作**控件以白色背景显示。

您可以在层次结构中移动对话框控件，可以复制和粘贴对话框控件，也可以删除某个节点。有关详细信息，请参阅 [Dialog Control Operations](https://ww2.mathworks.cn/help/simulink/gui/dialog-control-operations.html)。

#### 属性编辑器

**属性编辑器**允许您查看和设置**参数**、**显示**、**容器**及**动作**对话框控件的属性。下面显示了**参数**的**属性编辑器**：

![](https://ww2.mathworks.cn/help/simulink/gui/property_editor.png)

您可以设置**参数**、**动作**和**显示**对话框控件的以下属性。有关详细信息，请参阅示例[属性编辑器](https://ww2.mathworks.cn/help/simulink/gui/mask-editor-overview.html#bu0_6lc)。

**属性编辑器**

<table summary="属性编辑器"><colgroup><col width="35%"><col width="66%"></colgroup><thead><tr><th>属性</th><th>描述</th></tr></thead><tbody><tr><td colspan="2"><strong>属性</strong></td></tr><tr><td><p>名称</p></td><td><p>唯一地标识封装对话框中的对话框控件。必须为所有对话框控件设置<strong>名称</strong>属性。</p></td></tr><tr><td><p>值</p></td><td><p><strong>参数</strong>的值。<strong>值</strong>属性仅适用于<strong>参数</strong>对话框控件。</p></td></tr><tr><td><p>提示</p></td><td><p>标识封装对话框中的参数的标签文本。<strong>提示</strong>属性适用于除<strong>面板</strong>和<strong>图像</strong>对话框控件之外的所有对话框控件。</p></td></tr><tr><td><p>类型</p></td><td><p>对话框控件的类型。您可以使用<strong>类型</strong>字段更改<strong>参数</strong>和<strong>容器</strong>类型。您不能将任何容器类型更改为<strong>选项卡</strong>，反之亦然。</p></td></tr><tr><td><p>展开</p></td><td><p>用于指定可折叠的面板对话框控件默认情况下是处于展开还是折叠状态。</p></td></tr><tr><td><p>类型选项</p></td><td><p><strong>类型选项</strong>属性允许您设置特定的<strong>参数</strong>属性。<strong>类型选项</strong>属性适用于<strong>弹出</strong>、<strong>单选按钮</strong>、<strong>数据类型字符串</strong>和<strong>提升</strong>参数。</p></td></tr><tr><td><p>文件路径</p></td><td><p>您可以使用<strong>图像</strong>对话框控件向封装中添加图像。还可以在<strong>按钮</strong>对话框控件上显示图像。无论是哪种情况，都应在为这两个对话框控件启用的<strong>文件路径</strong>属性中提供图像的路径。对于<strong>按钮</strong>对话框控件，为<strong>提示</strong>属性指定一个空字符向量以便能够显示图像。</p><p>请注意，当您提供文件路径时，不要使用引号 (' ')。例如，如果要添加图像，请提供如下形式的文件路径：<code>C:\Users\User1\Image_Repositort\motor.png</code></p></td></tr><tr><td><p>文字换行</p></td><td><p><strong>文字换行</strong>属性用于对长文本进行换行。<strong>文字换行</strong>属性仅适用于<strong>文本</strong>对话框控件。</p></td></tr><tr><td><p>最大值和最小值</p></td><td><p><strong>最大值</strong>和<strong>最小值</strong>属性允许您为<strong>微调框</strong>、<strong>滑块</strong>和<strong>对话框</strong>等控件指定一个范围。</p></td></tr><tr><td><p>步长</p></td><td><p>允许您指定值的步长。此属性仅适用于<strong>微调框</strong>对话框控件。</p></td></tr><tr><td><p>工具提示</p></td><td><p>允许您为选定的对话框控件类型指定工具提示。当您将光标悬停在封装对话框中的对话框控件上时，将显示工具提示。您可以为除<strong>组框</strong>、<strong>选项卡</strong>、<strong>可折叠面板</strong>和<strong>面板</strong>之外的所有对话框控件类型添加工具提示。</p></td></tr><tr><td><p>刻度</p></td><td>允许您将<strong>滑块</strong>和<strong>刻度盘</strong>对话框控件的调整刻度设置为 <code>linear</code> 或 <code>log</code>。</td></tr><tr><td><p>表参数</p></td><td>为<strong>查找表</strong>参数指定表数据。</td></tr><tr><td>表单位</td><td>指定表数据的单位。</td></tr><tr><td>表显示名称</td><td>指定<strong>查找表</strong>控件的显示名称。</td></tr><tr><td>断点参数</td><td>为<strong>查找表</strong>控件指定断点参数。例如，<code>{'torque','engine speed'}</code></td></tr><tr><td>断点单位</td><td>指定断点参数的单位。例如，<code>{'Nm','rpm'}</code></td></tr><tr><td>数据设定</td><td>通过显式指定参数中的值或通过数据对象，可以为表和断点参数指定数据</td></tr><tr><td>查找表对象</td><td>为表和断点参数值指定数据对象的名称</td></tr><tr><td>文本类型</td><td>为<strong>文本区域</strong>参数指定文本的类型。它可以接受<strong>纯文本</strong>、<strong>HTML 文本</strong>和 <strong>MATLAB 代码</strong>。<strong>文本区域</strong>参数能够处理 HTML 代码并在封装对话框中显示输出。同样，它可以处理 <strong>MATLAB 代码</strong>并显示输出。</td></tr><tr><td colspan="2"><strong>属性</strong></td></tr><tr><td><p>计算</p></td><td><p>如果您输入 MATLAB 表达式作为封装参数输入，Simulink<sup>®</sup> 以如下两种方式之一处理该输入：</p><div><ol><li><p>如果选择了<strong>计算</strong>选项，Simulink 将计算表达式并使用计算的最终结果。要成功完成计算，表达式的变量必须在模型或基础工作区中初始化。例如，如果变量 <code>a</code> 和 <code>b</code> 分别包含值 2 和 9，则 '<code>a + b</code>' 的计算结果为 11。</p></li><li><p>如果未选择<strong>计算</strong>选项，则 Simulink 会在您在 “封装参数” 对话框中键入输入内容时对其进行文字读取。例如，'<code>a + b</code>' 读取为 <code>a</code> + <code>b</code>。</p></li></ol></div><p>对于<strong>编辑</strong>、<strong>复选框</strong>和<strong>弹出</strong>封装参数，默认情况下<strong>计算</strong>选项处于选中状态。</p></td></tr><tr><td><p>可调</p></td><td><p>默认情况下，您可以在仿真期间更改封装参数值。要防止在仿真期间更改参数值，请清除<strong>可调</strong>选项。如果封装参数不支持参数调优，Simulink 将忽略封装参数的<strong>可调</strong>选项设置。仿真时，这些参数将在 “封装” 对话框中禁用。“可调”中可用的模式有：</p><div><ul><li><p><strong>off</strong> - 在此模式下，您无法在仿真期间更改封装参数值。</p></li><li><p><strong>on</strong> - 您可以在仿真期间更改封装参数值。每次进行更改时，都会编译模型。</p></li><li><p><strong>运行到运行</strong> - 您可以在仿真期间更改封装参数值，但在您更改任何封装参数值时，模型不会重新编译。当仿真复杂的模型时，此模式有助于减少在快速重启模式下仿真时的模型编译时间。</p></li></ul></div><p>您也可以在快速重启模式下仿真模型时更改封装参数值。根据为<strong>可调</strong>属性和仿真模式指定的值，封装参数可以是只读的或读写的。</p><div><table><colgroup><col width="25%"><col width="25%"><col width="25%"><col width="25%"></colgroup><tbody><tr><td>&nbsp;</td><td>off</td><td>on</td><td>run-to-run</td></tr><tr><td>普通</td><td>只读</td><td>读写</td><td>&nbsp;</td></tr><tr><td>快速重启</td><td>只读</td><td>读写</td><td>读写</td></tr></tbody></table></div><p>有关参数调优和支持参数调优的模块的信息，请参阅<a href="https://ww2.mathworks.cn/help/simulink/ug/using-tunable-parameters.html">使用模块参数值进行调优和试验</a>。</p></td></tr><tr><td><p>只读</p></td><td><p>表示参数不能进行修改。</p></td></tr><tr><td><p>隐藏</p></td><td><p>表示参数不能显示在封装对话框中。</p></td></tr><tr><td><p>从不保存</p></td><td><p>表示参数值永远不会保存在模型文件中。</p></td></tr><tr><td><p>约束</p></td><td><p>允许您向所选参数添加约束。</p></td></tr><tr><td colspan="2"><strong>对话框</strong></td></tr><tr><td><p>启用</p></td><td><p>默认情况下<strong>启用</strong>处于选中状态。如果您清除此选项，选定控件将无法进行编辑。封装模块的用户不能设置该参数的值。</p></td></tr><tr><td><p>可见</p></td><td><p>仅当选择此选项时，选定控件才会出现在封装对话框中。</p></td></tr><tr><td><p>回调</p></td><td><p>您希望 Simulink 在用户向选定控件应用更改时执行的 MATLAB 代码。Simulink 使用临时工作区执行回调代码。</p></td></tr><tr><td colspan="2"><strong>布局</strong></td></tr><tr><td><p>项目位置</p></td><td><p>用于设置对话框控件要在当前行或新行中出现的位置。</p></td></tr><tr><td><p>对齐提示</p></td><td><p>允许您对齐封装对话框中的参数。除<strong>表</strong>以外的所有 “显示” 控件类型都支持此选项。</p><p>有关详细信息，请参阅 <a href="matlab:open_system([matlabroot '/examples/simulink_masking/main/slexMaskParameterOptionsExample'])" target="_blank">“组合框” 参数</a>。</p></td></tr><tr><td><p>提示位置</p></td><td><p>用于将对话框控件的提示位置设置为对话框控件的顶部或左侧。</p><p>您不能为<strong>复选框</strong>、<strong>对话框</strong>、<strong>数据类型字符串</strong>、<strong>可折叠面板</strong>和<strong>单选按钮</strong>设置<strong>提示位置</strong>属性。</p></td></tr><tr><td><p>方向</p></td><td><p>用于指定滑块和单选按钮的水平或垂直方向。</p></td></tr><tr><td><p>水平拉伸</p></td><td><p>如果选择此选项，则当您调整封装对话框的大小时，封装对话框上的控件将水平拉伸。默认情况下，<strong>水平拉伸</strong>复选框处于选中状态。</p><p>有关详细信息，请参阅 <a href="matlab:open_system([matlabroot '/examples/simulink_masking/main/slexMaskLayoutOptionsExample'])" target="_blank">“水平拉伸” 属性</a>。</p></td></tr></tbody></table>

#### “文档” 窗格

在**文档**窗格中，您可以定义或修改封装模块的类型、描述和帮助文本。

![](https://ww2.mathworks.cn/help/simulink/gui/mask_editor_doc_pane.png)

#### 类型

封装类型是显示在封装对话框中和模块的所有**封装编辑器**窗格上的模块分类。当 Simulink 显示封装对话框时，它会为封装类型添加后缀 `(mask)`。要定义封装类型，请在**类型**字段中输入类型。文本可以包含任何有效的 MATLAB 字符，但不能包含换行符。

#### 描述

封装描述是描述模块的用途或功能的简要帮助文本。默认情况下，封装描述显示在封装对话框中的封装类型下面。要定义封装描述，请在**描述**字段中输入描述。文本可以包含任何合法的 MATLAB 字符。Simulink 自动对较长的行进行换行。您可以使用 **Enter** 键强制换行。

#### 帮助

封装模块的 Online Help（联机帮助）提供**类型**和**描述**字段所提供信息之外的其他信息。当封装模块用户点击封装对话框上的**帮助**按钮时，将会在一个单独的窗口中显示此信息。要定义封装帮助，请在**帮助**字段中键入以下某一项：

* URL 设定
* `web` 或 `eval` 命令
* 文字或 HTML 文本

#### 提供 URL

如果**帮助**字段的第一行是一个 URL，Simulink 会将该 URL 传递给您的默认 Web 浏览器。该 URL 可以以 `https:`、`www:`、`file:`、`ftp:` 或 `mailto:` 开头。示例：

```
https://www.mathworks.com
file:///c:/mydir/helpdoc.html

```

一旦浏览器处于活动状态，MATLAB 和 Simulink 将无法再控制其操作。

#### 提供一个 `web` 命令

如果**帮助**字段的第一行是一个 `web` 命令，Simulink 会将该命令传递给 MATLAB，以便在 MATLAB Online Help 浏览器中显示指定的文件。示例：

```
web([docroot '/MyBlockDoc/' get_param(gcb,'MaskType') '.html'])

```

有关详细信息，请参阅 MATLAB [`web`](https://ww2.mathworks.cn/help/matlab/ref/web.html) 命令文档。用于封装帮助的 `web` 命令不能返回值。

#### 提供一个 `eval` 命令

如果**帮助**字段的第一行是一个 `eval` 命令，Simulink 会将该命令传递给 MATLAB，以便执行指定的计算。示例：

有关详细信息，请参阅 MATLAB [`eval`](https://ww2.mathworks.cn/help/matlab/ref/eval.html) 命令文档。用于封装帮助的 `eval` 命令不能返回值。

#### 提供文字或 HTML 文本

如果**帮助**字段的第一行不是 URL，或者 `web` 或 `eval` 命令，Simulink 将在 MATLAB Online Help 浏览器中的某个标题下显示该文本，该标题是**封装类型**字段的值。文本可以包含任何合法的 MATLAB 字符、换行符和任何标准的 HTML 标记，包括像 `img` 这样显示图像的标记。

Simulink 首先将该文本复制到临时文件夹，然后使用 [`web`](https://ww2.mathworks.cn/help/matlab/ref/web.html) 命令显示该文本。如果您希望文本显示图像，则可以提供图像文件的 URL 路径，也可以将图像文件放在临时文件夹中。使用 [`tempdir`](https://ww2.mathworks.cn/help/matlab/ref/tempdir.html) 可查找 Simulink 用于您的系统的临时文件夹。

### 代码窗格

* [对话框变量](#bu086et)
* [初始化命令](#bu086fb)
* [初始化命令的规则](#bu086f_)
* [允许库模块修改其内容](#bu086fw)
* [封装参数回调](#mw_9f50a04d-90dc-4e9e-a151-b54bc3e03617)

**代码**窗格为您提供模块初始化和参数回调代码的集成视图。封装编辑器代码功能类似于 MATLAB 编辑器中的功能，但存在一些限制。例如，支持自动补全功能，但无法在代码中设置断点。

![](https://ww2.mathworks.cn/help/simulink/gui/initialization_pane.gif)

当您打开模型时，Simulink 会查找位于在模型顶层或开放式子系统中的可见封装模块。仅当这些可见的封装模块满足以下任一条件时，Simulink 才对这些模块执行初始化命令：

* 封装模块具有图标绘制命令。

  **注意**

  Simulink 不会初始化不包含图标绘制命令的封装模块，即使它们具有初始化命令也是如此。
* 封装模块属于一个库，并且已启用**允许库模块修改其内容**。

当您执行以下操作时，模型中所有封装模块的初始化命令将会运行：

当您执行以下操作时，个别封装模块的初始化命令将会运行：

* 使用封装编辑器或 `set_param` 命令更改定义封装的任何封装参数，例如 `MaskDisplay` 和 `MaskInitialization`。
* 旋转或翻转封装模块（如果图标依赖于初始化命令）。
* 致使图标被绘制或重绘，并且图标绘制依赖于初始化代码。
* 通过使用模块对话框或 `set_param` 命令更改封装参数的值。
* 在同一模型中或不同模型之间复制封装模块。

**代码**窗格包含本节中所述的控件。

#### 对话框变量

**对话框变量**列表会显示对话框控件和关联的封装参数的名称，这些参数在**参数和对话框**窗格中定义。您还可以使用该列表更改封装参数的名称。要更改名称，请双击列表中的名称。随即会出现包含现有名称的编辑字段。编辑现有名称，然后按 **Enter** 或在编辑字段外部点击以确认您所做的更改。

#### 初始化命令

在此字段中输入初始化命令。您可以输入任何有效的 MATLAB 表达式，其中包含 MATLAB 函数和脚本、运算符以及在封装工作区中定义的变量。初始化命令在封装工作区而非基础工作区中运行。

#### 初始化命令的规则

以下规则适用于封装初始化命令：

* 不使用初始化代码创建其外观或控件设置会随其他控件设置的更改而更改的封装对话框。改用专门为此用途提供的封装回调。
* 避免在初始化命令的前面加上变量名称 `MaskParam_L_` 和 `MaskParam_M_`。这些特定前缀是保留项，仅供内部变量名称使用。
* 如果模块位于一个封装子系统中，而该封装子系统又位于正在初始化的其他封装子系统中，则应避免使用 `set_param` 命令来设置这些模块的参数。有关详细信息，请参阅[设置嵌套封装模块参数](https://ww2.mathworks.cn/help/simulink/ug/create-dynamic-mask-dialog-boxes.html#bsp20cb-2)。

#### 允许库模块修改其内容

仅当封装模块位于库中时，才会启用此复选框。选择此选项可允许您修改封装模块的参数。如果封装模块是封装子系统，则此选项允许您添加或删除模块并设置该子系统内模块的参数。如果未选择此选项，则当封装库模块尝试以任何方式修改其内容时，都将会生成错误。

#### 封装参数回调

“代码” 窗格为您提供封装初始化代码和封装回调代码的集成视图。要添加参数回调代码，请点击**参数**列表中参数旁边的加号按钮，此时会显示回调代码的框架。输入用于回调的 MATLAB 命令。

![](https://ww2.mathworks.cn/help/simulink/gui/callback.png)

### 图标窗格

* [图形图标编辑器](#mw_16258ead-f46d-4fde-b114-d57bd8f374fd)
* [封装图标绘制命令](#mw_b30427fd-1987-4eb7-9764-40db84cc2621)

**图标**窗格可以帮助您创建一个包含描述性文本、状态方程、图像以及图形的模块图标。您可以使用图形编辑器或封装绘制命令创建模块图标。

#### 图形图标编辑器

**图形编辑器**：您可以通过图形环境创建和编辑模块的封装图标。图形图标编辑器中的各种功能可以帮助您轻松创建图标。从封装编辑器启动图形图标编辑器。

![](https://ww2.mathworks.cn/help/simulink/gui/graphical-icon-editor.png)

* **交互式图形环境**：使用图形工具，如钢笔、曲率、文本、剪刀、连接器和方程（支持 LaTeX），以创建丰富的图形图标。网格、智能参考线和标尺帮助您创建像素完美的图标。除了绘图工具，一些内置形状，如电阻、电感和旋转阻尼，也都是现成的
* **元素浏览器**：元素浏览器列出图标中的所有元素。

  * 隐藏或取消隐藏图标中的元素。
  * 锁定或解锁一个元素，以便在处理图标的其他元素时不会意外更改该元素的形状或位置。
  * 为图标中的每个元素命名以便于识别。
* **端口绑定 / 解除绑定**：如果您使用模块上下文创建或修改模块，则每个模块上的端口数是预定义的。例如，Simscape 模块或 Aerospace 模块的端口数是预定义的，它们显示在模块图标上。如果要在不使用模块上下文的情况下创建或修改模块，还可以在模块图标上定义端口数。
* **条件可见性**：基于模块参数或封装参数隐藏或取消隐藏模块元素。
* **预览选项**：使用水平拉伸、翻转或缩放等预览选项在 Simulink 中预览图标。您也可以使用修改后的模块参数预览图标。
* **显示适合图标大小的元素**：当您调整模块大小时，自适应功能可帮助您仅显示适合图标大小的元素。
* **相对定位元素**：自动布局约束功能可帮助您在画布上相对于其他元素定位每个元素。
* **文本参数化**：您可以在模块图标上查看模块参数或封装参数的计算值。在参数 / 值中输入模块参数名称或占位符，该占位符将在运行时返回文本或值。要在模块图标上查看模块参数的计算值，请在 Simulink 画布上预览图标。

要了解有关图形图标编辑器的详细信息，请参阅 [Create and Edit Masked Block Icon Using Graphical Icon Editor](https://ww2.mathworks.cn/help/simulink/ug/graphical-icon-editor.html)

#### 封装图标绘制命令

封装编辑器为您提供每个绘图命令的框架。您可以为封装图标设置图像。点击**添加图像**以导入一个图像。

![](https://ww2.mathworks.cn/help/simulink/gui/dboxicon.gif)

“封装图标绘制命令” 窗格分为以下几个部分：

* [属性](https://ww2.mathworks.cn/help/simulink/gui/mask-editor-overview.html#mw_8a9190f1-2bf6-4ab4-b851-372ffffabe56): 提供可以应用于封装图标的不同控制项的列表。
* [预览](https://ww2.mathworks.cn/help/simulink/gui/mask-editor-overview.html#mw_a27f777e-9fa0-415a-a208-4fbb11342113): 显示模块封装图标的预览。
* [图标绘制命令](https://ww2.mathworks.cn/help/simulink/gui/mask-editor-overview.html#mw_03e8e168-b37f-4722-a9cb-79a287f8cfe9): 使您能够通过使用 MATLAB 代码绘制封装图标。

**属性.** 右窗格中可用的**属性**是一个控制项列表，允许您指定封装图标的特性。这些选项包括：

**模块边框.** 模块边框是围绕模块的矩形。您可以通过将**模块边框**参数设置为 “`可见`” 或 “`不可见`”，来选择显示或隐藏边框。默认值是使模块边框可见。例如，下图显示 AND 门模块的可见和不可见的模块边框。

![](https://ww2.mathworks.cn/help/simulink/gui/masking9.gif)

**图标透明度.** 根据您是希望隐藏还是显示图标下的内容，可以将图标透明度设置为 “`不透明`”、“`不透明(带端口)`” 或 “`透明`”。默认选项 “`不透明`” 将隐藏诸如端口标签等信息。透明图标将显示模块边框，不透明图标将隐藏模块边框。

![](https://ww2.mathworks.cn/help/simulink/gui/masking22.gif)

对于子系统模块，如果您将图标透明度设置为 “`不透明(带端口)`”，端口标签将可见。

![](https://ww2.mathworks.cn/help/simulink/gui/opaqueports.png)

**注意**

* 对于用于隐藏端口标签的 “`不透明`” 选项，必须在封装编辑器中添加一个图标绘制命令。
* 如果您将图标透明度设置为 “`透明`”，则 Simulink 不隐藏模块边框，即使您将**模块边框**属性设置为 “`不可见`” 也是如此。

**图标单位.** 此选项控制绘制命令使用的坐标系。它仅适用于 `plot`、`text` 和 `patch` 绘制命令。您可以从以下选项中选择：“`自动缩放`”、“`归一化`” 和 “`像素`”。

![](https://ww2.mathworks.cn/help/simulink/gui/masking24.gif)

* “`自动缩放`” 缩放图标以适应模块边框。当模块调整大小时，图标也调整大小。例如，下图显示使用这些向量绘制的图标：

  ```
  X = [0 2 3 4 9]; Y = [4 6 3 5 8];


  ```

  ![](https://ww2.mathworks.cn/help/simulink/gui/blkdrawauto.gif)

  模块边框的左下角是 (0,3)，右上角是 (9,8)。_x_ 轴范围是 9（0 到 9），而 _y_ 轴范围是 5（3 到 8）。
* “`归一化`” 在模块边框内绘制图标，其左下角为 (0,0)，其右上角为 (1,1)。只显示从 0 到 1 的 X 和 Y 值。当模块调整大小时，图标也调整大小。例如，下图显示使用这些向量绘制的图标：

  ```
  X = [.0 .2 .3 .4 .9]; Y = [.4 .6 .3 .5 .8];


  ```

  ![](https://ww2.mathworks.cn/help/simulink/gui/blkdrawnorm2.gif)
* “`像素`” 使用以像素表示的 X 值和 Y 值绘制图标。在调整模块大小时，图标大小不会自动调整。要强制图标随模块一起调整大小，请基于模块大小定义绘制命令。

**图标旋转.** 在模块发生旋转或翻转时，您可以选择是旋转或翻转图标，还是让图标固定在其原始方向。默认值是不旋转图标。图标旋转方式与模块端口旋转方式一致。下图显示在旋转 AND 门模块时选择 “`固定`” 和 “`旋转`” 图标旋转的结果。

![](https://ww2.mathworks.cn/help/simulink/gui/masking23.gif)

**端口旋转.** 此选项允许您指定封装模块的端口旋转类型。相关选择包括：

* “`默认`”

  顺时针旋转后端口将重新排序，以保持从左到右的端口编号顺序（对于位于模块上下两端的端口）以及从上到下的端口编号顺序（对于位于模块左右两侧的端口）。
* “`物理`”

  端口随模块一起旋转，而不在顺时针旋转后重新排序。

Default 旋转选项适用于控制系统和其他建模应用，它们的模块图通常采用从上到下和从左到右的方向。它通过最大限度减少旋转后重新连接模块以保持标准方向的需求，简化了模块图的编辑。

类似地，physical 旋转选项适用于电子、机械、液压和其他建模应用，它们的模块表示物理组件，线条表示物理连接。physical 旋转选项可以更保真地对所代表的设备行为进行建模（即端口随模块一起旋转，就像它们在物理设备上那样）。此外，该选项还可避免因为旋转而导致线交叉，从而使模块图更容易解读。

例如，下面两个图表示相同的晶体管电路。一张图中表示晶体管的封装模块使用 default 旋转，而另一张图中使用 physical 旋转。

![](https://ww2.mathworks.cn/help/simulink/gui/port_rotation1.gif)

这两张图都避免了线交叉而使图难以解读。下图显示了发生单次顺时针旋转后的模块图。

![](https://ww2.mathworks.cn/help/simulink/gui/port_rotation2.gif)

**注意**

使用 default 旋转的图引入了线交叉，而使用 physical 旋转的图没有发生这种情况。此外，使用 default 旋转的图无法通过编辑来删除线交叉。有关详细信息，请参阅[翻转或旋转模块](https://ww2.mathworks.cn/help/simulink/ug/changing-a-blocks-appearance.html#brzm8mf)。

**运行初始化.** **运行初始化**选项用于控制封装初始化命令的执行。相关选择包括：

* **off**（默认值）：不执行封装初始化命令。当封装绘制命令与封装工作区之间不存在依存关系时，建议将**运行初始化**值指定为 **off**。将该值设置为 **off** 有助于优化 Simulink 性能，因此这样将不会执行封装初始化命令。
* **on**：如果封装工作区不是最新的，则执行封装初始化命令。如果指定此选项，不管封装工作区与封装绘制命令之间是否存在依存关系，都会在执行封装绘制命令之前执行封装初始化命令。
* **分析**：仅在存在工作区依存关系的情况下才执行封装初始化命令。如果指定此选项，Simulink 将在执行封装图标绘制命令之前执行封装初始化命令。**分析**选项是为了实现向后兼容性，其他情况下不建议使用。对于 R2016b 或以前的 Simulink 模型，建议使用升级顾问进行升级。

  有关详细信息，请参阅 [slexMaskDrawingExamples](matlab:open_system([matlabroot '/examples/simulink_masking/main/slexMaskDrawingExamples']))。

**预览.** 此部分显示模块封装图标的预览。仅当封装包含绘制的图标时，模块封装预览才可用。

当您添加图标绘制命令并点击**应用**时，预览图像将刷新并显示在**图标**窗格的**预览**部分。

**图标绘制命令.** 向编辑器中添加代码以绘制模块图标。您可以使用左窗格中的命令列表来绘制模块图标。

**封装图标绘制命令**

<table summary="封装图标绘制命令"><colgroup><col width="25%"><col width="25%"><col width="25%"><col width="25%"></colgroup><thead><tr><th>绘制命令</th><th>描述</th><th>语法示例</th><th>预览</th></tr></thead><tbody><tr><td><a href="https://ww2.mathworks.cn/help/simulink/slref/color.html"><code>color</code></a></td><td><p>更改后续封装图标绘制命令的绘图颜色</p></td><td><code>color('red'); port_label('output',1,'Text')</code></td><td><span><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/color.jpg"></div></span></td></tr><tr><td><a href="https://ww2.mathworks.cn/help/simulink/slref/disp_cmd.html"><code>disp</code></a></td><td><p>在封装图标上显示文本。</p></td><td><code>disp('Gain')</code></td><td><span><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/disp.jpg"></div></span></td></tr><tr><td><a href="https://ww2.mathworks.cn/help/simulink/slref/dpoly.html" hreflang="en"><code>dpoly</code></a></td><td><p>在封装图标上显示传递函数</p></td><td><code>dpoly([0 0 1], [1 2 1], 'z')</code></td><td><span><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/dpoly.jpg"></div></span></td></tr><tr><td><a href="https://ww2.mathworks.cn/help/simulink/slref/droots.html" hreflang="en"><code>droots</code></a></td><td><p>在封装图标上显示传递函数</p></td><td><code>droots([-1], [-2 -3], 4)</code></td><td><span><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/droot.jpg"></div></span></td></tr><tr><td><a href="https://ww2.mathworks.cn/help/simulink/slref/fprintf.html"><code>fprintf</code></a></td><td><p>在封装图标上居中显示变量文本</p></td><td><code>fprintf('Sum = %d', 7)</code></td><td><span><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/fprintf.jpg"></div></span></td></tr><tr><td><a href="https://ww2.mathworks.cn/help/simulink/slref/image.html"><code>image</code></a></td><td><p>在封装图标上显示 RGB 图像</p><div><p><strong>注意</strong></p><p>要从用户界面添加封装图标图像，请点击上下文菜单中的 &gt; 。</p><div><p></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/addmaskiconimage.png"></div><p></p></div></div></td><td><code>image('b747.jpg')</code></td><td><span><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/image.jpg"></div></span></td></tr><tr><td><a href="https://ww2.mathworks.cn/help/simulink/slref/patch.html"><code>patch</code></a></td><td><p>在封装图标上绘制指定形状的彩色补片</p></td><td><code>patch([0 10 20 30 30 0], [10 30 20 25 10 10],[1 0 0])</code></td><td><span><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/patch.jpg"></div></span></td></tr><tr><td><a href="https://ww2.mathworks.cn/help/simulink/slref/plot.html"><code>plot</code></a></td><td><p>在封装图标上绘制由一系列点连接而成的图形</p></td><td><code>plot([10 20 30 40], [10 20 10 15])</code></td><td><span><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/plot.jpg"></div></span></td></tr><tr><td><a href="https://ww2.mathworks.cn/help/simulink/slref/port_label.html"><code>port_label</code></a></td><td><p>在封装图标上绘制端口标签</p></td><td><code>port_label('output', 1, 'xy')</code></td><td><span><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/port_label.jpg"></div></span></td></tr><tr><td><a href="https://ww2.mathworks.cn/help/simulink/slref/text.html"><code>text</code></a></td><td><p>在封装图标上的特定位置显示文本。</p><p>您必须在<strong>图标单位</strong>框中选择 <code>Pixels</code>。</p></td><td><p><code>text(5,10, 'Gain')</code></p></td><td><span><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/text.jpg"></div></span></td></tr><tr><td><a href="https://ww2.mathworks.cn/help/simulink/slref/block_icon.html" hreflang="en"><code>block_icon</code></a></td><td><p>将包含在子系统中的模块的图标提升到子系统封装</p></td><td><code>block_icon(BlockName)</code><p>此处，模块的图标提升到它的 Subsystem 模块。</p><p>有关详细信息，请参阅 <a href="matlab:open_system([matlabroot '/examples/simulink_masking/main/slexMaskDrawingExamples'])" target="_blank">slexMaskDrawingExamples</a>。</p></td><td><span><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/gui/block_icon.png"></div></span></td></tr></tbody></table>

**注意**

Simulink 不支持匿名函数内的封装绘制命令。

绘制命令按照将它们添加到文本框中的相同顺序执行。绘制命令可访问封装工作区中的所有变量。如果任何绘制命令无法成功执行，模块图标会显示问号

![](https://ww2.mathworks.cn/help/simulink/gui/unresolved.png)

。

在以下情况下，绘制命令在绘制模块后执行：

* 在封装对话框进行了更改并已应用。
* 在封装编辑器中进行了更改。
* 对模块图进行了更改，并且这些更改会影响模块的外观，例如旋转该模块。

### 约束

封装参数约束帮助您创建对封装参数进行的验证，而不必编写您自己的代码。有三种类型的约束：参数约束、交叉参数约束和端口约束。

![](https://ww2.mathworks.cn/help/simulink/gui/constraints.png)

**参数约束**：封装可以包含接受用户输入值的参数。您可以使用封装对话框为封装参数提供输入值。约束确保封装参数的输入在指定的范围内。例如，假设有一个封装的 Gain 模块。您可以设置一个约束，使输入值必须介于 1 和 10 之间。如果您提供的输入超出指定的范围，将显示错误。左窗格中的约束浏览器可帮助您管理共享约束。

**交叉参数约束**：交叉参数约束应用于两个或更多的**编辑**或**组合框**类型的封装参数。当您要指定诸如 Parameter1 必须大于 Parameter2 之类的情况时，可以使用交叉参数约束。

**端口约束**：您可以对封装模块的输入端口和输出端口指定约束。编译模型时，会对照约束检查端口属性。

### 其他选项

以下按钮将出现在**封装编辑器**上：

* **保存封装**应用封装设置并将**封装编辑器**保持为打开状态。
* **预览对话框**应用您所做的更改，并打开封装对话框。
* **删除封装**删除封装并关闭**封装编辑器**。要再次创建封装，请选择模块，然后在**模块**选项卡上，在**封装**组中，点击**创建掩膜**。
* **复制封装**从 Simulink 库模块复制封装定义。搜索所需的模块，然后点击**复制封装**以从现有模块导入封装定义。
* **计算模块**计算回调和初始化代码。

## 相关主题

* [封装基础知识](https://ww2.mathworks.cn/help/simulink/ug/block-masks.html)
* [创建简单封装](https://ww2.mathworks.cn/help/simulink/ug/how-to-mask-a-block.html)
* [创建模块封装](https://ww2.mathworks.cn/help/simulink/block-masks.html)
* [创建封装：“参数”和 “对话框” 窗格](https://www.mathworks.com/videos/creating-a-mask-parameters-and-dialog-pane-120638.html)
