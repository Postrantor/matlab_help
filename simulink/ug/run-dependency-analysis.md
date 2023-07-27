---
url: https://ww2.mathworks.cn/help/simulink/ug/run-dependency-analysis.html
title: Run a Dependency Analysis - MATLAB & Simulink - MathWorks 中国 --- 运行依赖性分析 - MATLAB & Simulink - MathWorks 中国
date: 2023-07-19 10:36:01
tag: #需求管理
summary: Find required files for a whole project or for specified files, and control options for analysis.
---

## Run a Dependency Analysis

**Note**

You can analyze only files that are in your project. If your project is new, add files to the project before running a dependency analysis. See [Add Files to the Project](https://ww2.mathworks.cn/help/simulink/ug/add-files-to-the-project.html).

To investigate dependencies, run a dependency analysis on your project. On the **Project** tab, click the down arrow to expand the **Tools** gallery. Under **Apps**, click **Dependency Analyzer**. Alternatively, in the project **Views** pane, select **Dependency Analyzer** and click **Analyze**.

To analyze the dependencies of specific files, in the dependency graph, select the files. In the **Impact Analysis** section, click **All Dependencies** or use the context menu and select **Find All Dependencies**.

To analyze the dependencies inside add-ons, select > . For more details about available options, see [Analysis Scope](https://ww2.mathworks.cn/help/simulink/ug/scope-and-limitations.html#mw_911bd5b5-bbc0-4851-9c7f-a237a2913670).

You can also check dependencies directly in Project. In the Project **Files** view, right-click the project files you want to analyze and select .

After you run the first dependency analysis of your project, subsequent analyses incrementally update the results. The Dependency Analyzer determines which files changed since the last analysis and updates the dependency data for those files. However, if you update add-ons or installed products and want to discover dependency changes in them, you must perform a complete analysis. To perform a complete analysis, in the Dependency Analyzer, click > .

**Note**

In the Simulink® Editor, if an open model, library, or chart belongs to a project, you can find file dependencies. On the **Simulation** tab, select > . Simulink analyzes the whole project and shows all dependencies for the file.

![](https://ww2.mathworks.cn/help/simulink/ug/dependency_graph.png)

For next steps, see:

## Related Topics

- [Check Dependency Results and Resolve Problems](https://ww2.mathworks.cn/help/simulink/ug/check-dependency-results-and-resolve-problems.html)
- [Perform an Impact Analysis](https://ww2.mathworks.cn/help/simulink/ug/perform-impact-analysis.html)
- [Export Dependency Analysis Results](https://ww2.mathworks.cn/help/simulink/ug/save-and-reload-dependency-analysis-results.html)
- [Find Requirements Documents in a Project](https://ww2.mathworks.cn/help/simulink/ug/find-requirements-documents-in-a-project.html)
