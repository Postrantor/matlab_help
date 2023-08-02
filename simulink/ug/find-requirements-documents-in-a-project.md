---
url: https://ww2.mathworks.cn/help/simulink/ug/find-requirements-documents-in-a-project.html
title: Find Requirements Documents in a Project - MATLAB & Simulink - MathWorks 中国
date: 2023-07-19 10:34:20
tag: #需求管理
summary: View and open requirements documents in a project, linked using the Requirements Management Interface......
---
## Find Requirements Documents in a Project

In a project, a dependency analysis finds requirements documents linked using the Requirements Management Interface.

- You can view and navigate to and from the linked requirements documents.
- You can create or edit Requirements Management links only if you have Requirements Toolbox™.

1. On the **Project** tab, click the down arrow to expand the **Tools** gallery. Under **Apps**, click **Dependency Analyzer**.

   Alternatively, in the project **Views** pane, select **Dependency Analyzer** and click the **Analyze** button.
2. The dependency graph displays the structure of all analyzed dependencies in the project. Project files that are not detectable dependencies of the analyzed files are not visible in the graph.
3. Use the dependency graph legend to locate the requirements documents in the graph. Arrows connect requirements documents to the files with the requirement links.
4. To find the specific block containing a requirement link, select the arrow connecting requirements documents to the files. In the **Properties** pane, in the **Impacted** column of the table, click the file to open it and highlight the block containing the dependency.
5. To open a requirements document, double-click the document in the graph.

## Related Topics

- [Run a Dependency Analysis](https://ww2.mathworks.cn/help/simulink/ug/run-dependency-analysis.html)
- [Check Dependency Results and Resolve Problems](https://ww2.mathworks.cn/help/simulink/ug/check-dependency-results-and-resolve-problems.html)
- [Perform an Impact Analysis](https://ww2.mathworks.cn/help/simulink/ug/perform-impact-analysis.html)
- [Export Dependency Analysis Results](https://ww2.mathworks.cn/help/simulink/ug/save-and-reload-dependency-analysis-results.html)
- [View Requirements Toolbox Links Associated with Model Elements](https://ww2.mathworks.cn/help/simulink/ug/viewing-linked-requirements-in-models-and-blocks.html)
