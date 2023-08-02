---
tip: translate by openai@2023-07-19 10:38:31
...
---
url: [https://ww2.mathworks.cn/help/simulink/ug/viewing-linked-requirements-in-models-and-blocks.html](https://ww2.mathworks.cn/help/simulink/ug/viewing-linked-requirements-in-models-and-blocks.html)

> 查看模型和块中的链接要求：[https://ww2.mathworks.cn/help/simulink/ug/viewing-linked-requirements-in-models-and-blocks.html](https://ww2.mathworks.cn/help/simulink/ug/viewing-linked-requirements-in-models-and-blocks.html)
> title: View Requirements Toolbox Links Associated with Model Elements

- MATLAB & Simulink
- MathWorks 中国
  date: 2023-07-19 10:37:37
  tag: #需求管理

summary: Work with models with associated requirements without Requirements Toolbox installed.

> 摘要：在没有安装需求工具箱的情况下，与关联需求的模型一起工作。

---

## View Requirements Toolbox Links Associated with Model Elements

You can work with links that were added to the model with Requirements Toolbox™ when you use the **Requirements Viewer** app in Simulink®. For all links, you can:

> 使用 Simulink® 中的**要求查看器**应用程序时，您可以使用要求工具箱 ™ 添加到模型中的链接。对于所有链接，您可以：

- Highlight model objects with links
- View information about a link
- Filter link highlighting based on specified keywords

For links to requirements that are stored in external documents, you can also navigate to the linked requirements.

> 对于存储在外部文档中的需求链接，您也可以导航到链接的需求。

### Highlight, Filter, and View Information for Links in a Model

This example shows how to work with a Simulink model that has links using the Requirements Viewer app. You can highlight links, view information about a link, and filter link highlighting based on specified keywords.

> 这个例子展示了如何使用需求查看器应用程序来处理具有链接的 Simulink 模型。您可以突出显示链接，查看有关链接的信息，并根据指定的关键字过滤链接突出显示。

Open the `slvnvdemo_fuelsys_officereq` model.

```
open_system('slvnvdemo_fuelsys_officereq')

```

In the **Apps** tab, click **Requirements Viewer**.

**Highlight Links in a Model**

To highlight model objects that have associated outgoing links, in the **Requirements Viewer** tab, click **Highlight Links**.

> 在**需求查看器**选项卡中，点击**高亮链接**以突出具有关联外部链接的模型对象。

Model objects that have associated outgoing links are highlighted in yellow, such as the block called `MAP sensor`.

> 拥有关联出站链接的模型对象会以黄色突出显示，比如叫做“MAP 传感器”的块。

![](https://ww2.mathworks.cn/help/examples/simulink/win64/RequirementTraceabilityInSimulinkExample_01.png)

Objects that contain child objects with associated outgoing links are highlighted with an orange outline, such as the `fuel rate controller` subsystem.

> 物件包含具有相關外向連結的子物件，會以橘色輪廓加以突顯，例如 `燃料率控制` 子系統。

![](https://ww2.mathworks.cn/help/examples/simulink/win64/RequirementTraceabilityInSimulinkExample_02.png)

Model objects that do not have associated links are dimmed.

![](https://ww2.mathworks.cn/help/examples/simulink/win64/RequirementTraceabilityInSimulinkExample_03.png)

To remove highlighting, click the **Highlight Links** button again. When highlighting links, ensure that you don't have any keyword filters applied because this can prevent some objects from becoming highlighted. For more information, see [Filter Highlighted Links](https://ww2.mathworks.cn/help/simulink/ug/viewing-linked-requirements-in-models-and-blocks.html#RequirementTraceabilityInSimulinkExample-3).

> 要取消突出显示，请再次单击**突出显示链接**按钮。在突出显示链接时，确保您没有应用任何关键字过滤器，因为这可能会阻止某些对象被突出显示。有关更多信息，请参阅[过滤突出显示的链接](https://ww2.mathworks.cn/help/simulink/ug/viewing-linked-requirements-in-models-and-blocks.html#RequirementTraceabilityInSimulinkExample-3)。

**View Information About a Link**

After you've identified the model objects that have associated outgoing links, you can view information about those links. Right-click the `MAP sensor` block and select **Requirements** > **Open Outgoing Links dialog**.

> 在确定有关联的出站链接的模型对象后，您可以查看有关这些链接的信息。右键单击 `MAP传感器` 块，然后选择**要求** > **打开出站链接对话框**。

![](https://ww2.mathworks.cn/help/examples/simulink/win64/RequirementTraceabilityInSimulinkExample_04.png)

The Outgoing Links dialog displays information about the link and its destination. In this case, the link destination is a requirement stored in an external document. The dialog displays the:

> 对话框中显示出外部链接及其目标的信息。在这种情况下，链接目标是存储在外部文档中的要求。对话框显示：

- Link description
- Document type
- Document name and relative location
- Location identifier for link destination
- Link keywords

**Filter Highlighted Links**

Links in your model can have associated keywords. You can categorize a link with a keyword. For example, the `slvnvdemo_fuelsys_officereq` model uses the keywords `design`, `requirement`, and `test`.

> 模型中的链接可以关联关键字。您可以使用关键字对链接进行分类。例如，`slvnvdemo_fuelsys_officereq` 模型使用关键字 `design`，`requirement` 和 `test`。

In the `slvnvdemo_fuelsys_officereq` model, the `MAP sensor` block has the specified keyword `test`, which was found in [View Information about a Link](https://ww2.mathworks.cn/help/simulink/ug/viewing-linked-requirements-in-models-and-blocks.html#RequirementTraceabilityInSimulinkExample-2). Filter the highlighted links so that only blocks that have associated links with the keyword `test` are highlighted:

> 在 `slvnvdemo_fuelsys_officereq` 模型中，`MAP传感器` 块具有指定的关键字 `test`，该关键字可在[查看链接信息]（[https://ww2.mathworks.cn/help/simulink/ug/viewing-linked-requirements-in-models-and-blocks.html#RequirementTraceabilityInSimulinkExample-2](https://ww2.mathworks.cn/help/simulink/ug/viewing-linked-requirements-in-models-and-blocks.html#RequirementTraceabilityInSimulinkExample-2)）中找到。过滤突出显示的链接，以便只突出显示具有关键字 `test` 的相关链接的块。

1. In the **Requirements Viewer** tab, click **Link Settings**.
2. In the Requirement Settings dialog, in the **Filters** tab, select **Filter links by keywords when highlighting and reporting requirements**.

> 在需求设置对话框中，在**过滤器**选项卡中，选择**在突出显示和报告需求时使用关键字过滤链接**。

3. In the **Include links with any of these keywords** field, enter `test`.

> 在**包含具有以下关键字的链接**字段中，输入'test'。

4. Click **Close**.
5. In the **Requirements Viewer** tab, click **Highlight Links**.

Note that the `fuel rate controller` subsystem is not highlighted when the filter is applied.

> 注意，当应用过滤器时，`燃料率控制器` 子系统不会被突出显示。

![](https://ww2.mathworks.cn/help/examples/simulink/win64/RequirementTraceabilityInSimulinkExample_05.png)

You can also use the **Exclude links with any of these keywords** field to omit blocks from highlighting that have associated links with the specified keyword. You can enter multiple keywords in the **Include links with any of these keywords** and **Exclude links with any of these keywords** fields. If you select **Apply same filters to link labels**, when you right-click blocks that have links but are not highlighted, the link is greyed out and you cannot navigate to the destination.

> 您还可以使用**排除具有以下关键字的链接**字段来省略具有与指定关键字相关联的链接的高亮块。您可以在**包含具有以下关键字的链接**和**排除具有以下关键字的链接**字段中输入多个关键字。如果您选择**将相同的过滤器应用于链接标签**，则当您右键单击具有链接但未高亮显示的块时，该链接将被灰色显示，您无法导航到目标。

To clear the filter, clear **Filter links by keywords when highlighting and reporting requirements**, or remove all keywords from the **Include links with any of these keywords** and **Exclude links with any of these keywords** fields.

> 清除过滤器，清除**突出显示和报告要求时使用关键字过滤链接**，或者从**包含以下关键字的链接**和**排除以下关键字的链接**字段中移除所有关键字。

### Navigate to Externally Stored Requirements from a Model

This example shows how to navigate to linked requirements that are stored in an external document from a model using the **Requirements Viewer** app in Simulink®. If a model object is linked to a requirement that is stored in an external document, such as a Microsoft® Word or Microsoft Excel® file, or an IBM® Rational® DOORS® or IBM DOORS Next project, then you can navigate to the requirement in the external document from the associated block in Simulink.

> 这个例子展示了如何使用 Simulink® 中的 **Requirements Viewer** 应用程序从模型中导航到存储在外部文档中的链接要求。如果模型对象链接到存储在外部文档（如 Microsoft® Word 或 Microsoft Excel® 文件，或 IBM® Rational® DOORS® 或 IBM DOORS Next 项目）中的要求，则可以从 Simulink 中的相关块导航到外部文档中的要求。

Open the `slvnvdemo_fuelsys_officereq` model.

```
open_system('slvnvdemo_fuelsys_officereq')

```

The block called `MAP sensor` is linked to a requirement in a Microsoft Excel file. To navigate to the requirement in the Excel file, right-click the `MAP sensor` block, select **Requirements,** and click the link at the top of the context menu.

> 称为“MAP 传感器”的块与 Microsoft Excel 文件中的一个要求相关联。要在 Excel 文件中导航到要求，请右键单击“MAP 传感器”块，选择**要求**，然后单击上下文菜单顶部的链接。

![](https://ww2.mathworks.cn/help/examples/simulink/win64/NavigateToExternallyStoredRequirementFromAModelExample_01.png)

The associated requirement opens in Excel.

![](https://ww2.mathworks.cn/help/examples/simulink/win64/NavigateToExternallyStoredRequirementFromAModelExample_02.png)

If you don't have Requirements Toolbox™ installed, then you cannot navigate to requirements that are stored in a Requirements Toolbox requirement set.

> 如果您没有安装 Requirements Toolbox™，那么您无法导航到存储在 Requirements Toolbox 要求集中的要求。

**Cleanup**

Close the open models without saving changes.

## See Also

[Find Requirements Documents in a Project](https://ww2.mathworks.cn/help/simulink/ug/find-requirements-documents-in-a-project.html)

> 找到项目中的需求文档
