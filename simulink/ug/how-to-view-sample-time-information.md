---
tip: translate by openai@2023-07-07 21:09:42
url: [https://www.mathworks.com/help/simulink/ug/how-to-view-sample-time-information.html](https://www.mathworks.com/help/simulink/ug/how-to-view-sample-time-information.html)
title: View Sample Time Information - MATLAB & Simulink --- 查看采样时间信息 - MATLAB & Simulink
date: 2023-07-07 21:08:23
tag:
summary: How to access sample time information interactively.
---
## View Sample Time Information

Simulink® models can display color coding and annotations that represent specific sample times. Each sample time type has one or more colors associated with it. You can display the blocks and signal lines in color, the annotations in black, or both. To select one of these options:

> Simulink® 模型可以显示颜色编码和注释，代表特定的采样时间。每种采样时间类型都有一个或多个与之相关的颜色。您可以将块和信号线以颜色显示，注释以黑色显示，或两者皆显示。要选择其中一个选项：

1. To enable colors, in the Simulink model window, on the **Debug** tab, select > .

> 在 Simulink 模型窗口中，在**调试**选项卡上，选择 > 来启用颜色。

2. To enable annotations, on the **Debug** tab, select > Selecting both and displays both the colors and the annotations. Regardless of your choice, Simulink performs an automatically.

> 选择两者并同时显示颜色和注释。不管您的选择如何，Simulink 都会自动执行。

To turn off the colors and annotations:

1. On the **Debug** tab, select > to disable colors.
2. On the **Debug** tab, select > to disable annotations.

Simulink performs another automatically.

Your Sample Time Display choices directly control the information that the Timing Legend displays.

> 你的样例时间显示选择直接控制定时图例显示的信息。

**Note**

The discrete sample times in the table [Designations of Sample Time Information](https://www.mathworks.com/help/simulink/ug/how-to-specify-the-sample-time.html#br000z0) represent a special case. Five colors indicate the speed through the fifth fastest discrete rate. A sixth color, orange, represents all rates that are slower than the fifth discrete rate. You can distinguish between these slower rates by looking at the annotations on their respective signal lines.

> 表中的离散采样时间[采样时间信息的指定]代表一种特殊情况。五种颜色表示第五个最快离散速率的速度。第六种颜色，橙色，表示比第五个离散速率慢的所有速率。您可以通过查看它们各自的信号线上的注释来区分这些较慢的速率。

### Inspect Sample Time Using Timing Legend

You can view the Timing Legend for an individual model or for multiple models. Additionally, you can prevent the legend from automatically opening when you select options on the menu.

> 您可以查看单个模型或多个模型的时序图例。此外，您可以防止当您在菜单上选择选项时图例自动打开。

To assist you with interpreting a block diagram, the Timing Legend contains the sample time color, annotation, and value for each sample time in the model. To view the legend:

> 要协助您解释块图，定时图例包含模型中每个采样时间的采样时间颜色、注释和值。要查看图例：

1. In the Simulink model window, on the **Modeling** tab, click **Update Model**.

> 在 Simulink 模型窗口中，在“建模”选项卡上，单击“更新模型”。

2. On the **Debug** tab, select > or press **Ctrl + J**.

In addition, when you select or , Simulink updates the model diagram and opens the legend by default. The legend contents reflect your choices. If you turn colors on, the legend displays the color and the value of the sample time. Similarly, if you turn annotations on, the annotations appear in the legend.

> 此外，当您选择或更新模型时，Simulink 会默认更新模型图并打开图例。图例内容反映您的选择。如果您打开颜色，图例将显示颜色和采样时间的值。同样，如果您打开注释，注释将出现在图例中。

The legend displays sample times present in the model, classified by the type of the sample time.

> 图例显示模型中的样本时间，按样本时间类型分类。

![](https://www.mathworks.com/help/simulink/ug/timing_lengen_docked.png)

The legend provides two types of highlighting options:

- Highlighting the blocks and signals that the sample time originates from.
- Highlighting all the blocks and signals that contain the selected sample time.

To enable highlighting of the origin of the sample times, click the `Origin` option from the **Highlight** menu. You can also click the type of the sample time to highlight all sources of a particular type of sample time.

> 要启用样本时间的来源突出显示，请从**突出显示**菜单中单击 `Origin` 选项。您还可以单击样本时间的类型，以突出显示特定类型的样本时间的所有来源。

![](https://www.mathworks.com/help/simulink/ug/timing_leg_origin.png)

To enable highlighting of all the blocks that contain a selected sample time, click the `All` option from the **Highlight** menu. You can also click the type of the sample time to highlight all the blocks and signals that contain the select type of sample time.

> 要启用高亮显示包含所选择的采样时间的所有块，请从 **Highlight** 菜单中单击“ All”选项。 您还可以单击采样时间的类型以高亮显示所有包含所选类型的采样时间的块和信号。

The `None` option from the **Highlight** menu clears current highlighting.

> 从**高亮**菜单中的“无”选项可清除当前的高亮显示。

The button

![](https://www.mathworks.com/help/simulink/ug/timing_legend_1p_icon.png)

shows discrete value as 1/period when the discrete sample time is present. When clicked, the discrete period is displayed as 1/period; for a nonzero offset, it displays as offset/period. The image shows 1/period values and the corresponding highlighted block in the model.

> 当有离散采样时间时，显示为 1/周期的离散值。当点击时，离散周期显示为 1/周期；对于非零偏移量，它显示为偏移量/周期。图片显示 1/周期的值以及模型中相应的高亮块。

![](https://www.mathworks.com/help/simulink/ug/timing_leg_1p.png)

**Note**

The Timing Legend displays all of the sample times in the model, including those that are not associated with any block. For example, if the fixed step size is 0.1 and all of the blocks have a sample time of 0.2, then both rates (0.1 and 0.2) appear in the legend.

> 图例显示模型中所有的采样时间，包括那些不与任何块相关联的采样时间。例如，如果固定步长为 0.1，而所有块的采样时间为 0.2，那么两个速率(0.1 和 0.2)都会出现在图例中。

For subsequent viewings of the legend, update the diagram to access the latest known information.

> 为了查看传说的后续部分，请更新图表以获取最新已知信息。

If you do not want to view the legend upon selecting :

1. In the Simulink Editor, on the **Modeling** tab, select >
2. In the **General** pane, clear and click **Apply**.

### Inspect Sample Times Throughout a Model

The Model Data Editor (on the **Modeling** tab, click **Model Data Editor**) shows information about model data (signals, parameters, and states) in a sortable, searchable table. The **Sample Time** column shows the sample time specified for each signal in a model. After you update the block diagram, the column also shows the specific sample that each signal uses (for example, for signals for which you specify inherited sample time, `-1`). You can also use this column to specify sample times.

> 模型数据编辑器(在**建模**选项卡上，单击**模型数据编辑器**)显示模型数据(信号、参数和状态)的可排序、可搜索的表格信息。**采样时间**列显示模型中每个信号的指定采样时间。更新模块图后，该列还显示每个信号使用的具体采样(例如，对于指定继承采样时间的信号，`-1`)。您也可以使用此列来指定采样时间。

For more information, see **[Model Data Editor](https://www.mathworks.com/help/simulink/slref/modeldataeditor.html)**.

> 更多信息，请参阅 **[模型数据编辑器](https://www.mathworks.com/help/simulink/slref/modeldataeditor.html)**.

## Related Topics

- [What Is Sample Time?](https://www.mathworks.com/help/simulink/ug/what-is-sample-time.html)
- [Specify the Sample Time](https://www.mathworks.com/help/simulink/ug/model-discretizer.html#f4-141458)
- [Types of Sample Time](https://www.mathworks.com/help/simulink/ug/types-of-sample-time.html)
