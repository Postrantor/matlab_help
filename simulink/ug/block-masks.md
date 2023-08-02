---
url: https://ww2.mathworks.cn/help/simulink/ug/block-masks.html
title: 封装基础知识
- MATLAB & Simulink
- MathWorks 中国
date: 2023-07-28 08:25:16
tag: 
summary: 了解有关封装和何时封装模块的基础知识。
---
## 封装基础知识

封装是一种自定义模块界面，它可隐藏模块内容，使用它自己的图标和参数对话框将内容以原子块的形式显示。它可以封装模块逻辑，提供对模块数据的受控访问，并简化模型的图形外观。

当您封装模块时，将创建封装定义并随模块一同保存。封装只改变模块接口，而不改变底层模块特征。您可以通过在封装上定义对应的封装参数，提供对一个或多个底层模块参数的访问。

封装 Simulink® 模块可以：

* 在模块上显示有意义的图标
* 为模块提供自定义对话框
* 提供一个对话框，只允许您访问底层模块的所选参数
* 提供特定于封装模块的用户自定义说明
* 使用 MATLAB® 代码初始化参数

请考虑代表直线方程 `y = mx + b` 的模型 [masking_example](matlab:open_system([matlabroot '/examples/simulink_masking/main/slexMaskingExamples']))。

![](https://ww2.mathworks.cn/help/simulink/ug/masking_example.png)

每个模块都有它自己的对话框，这让指定直线方程变量的值变得复杂。为了简化用户界面，可在顶层子系统模块上应用封装。

![](https://ww2.mathworks.cn/help/simulink/ug/maskexeditor9778ae7fb1274b08ff4f7f3982ff4178_zh_CN.png)

此处变量 `m` 表示斜率，变量 `b` 表示直线方程 `y = mx + b` 的截距。

封装对话框中显示了**斜率**和**截距**字段，分别对应于变量 `m` 和 `b`。

![](https://ww2.mathworks.cn/help/simulink/ug/maskexdialog.png)

封装模块不支持内容预览。要预览子系统的内容，请参阅[预览模型组件的内容](https://ww2.mathworks.cn/help/simulink/ug/preview-content-of-hierarchical-items.html)。

**提示**

有关封装的示例，请参阅 [Simulink 封装示例](matlab:openExample([matlabroot '/examples/simulink_masking/main/SimulinkMaskingDemoExample']))。这些示例按类型组合。在示例模型中：

* 要查看封装定义，请双击 View Mask 模块。
* 要查看封装对话框，请双击该模块。

极少数模块不能封装，示例如下：

* Scope 模块
* Simulink Function 模块
* Initialize Function、Terminate Function 和 Reset Function 模块
* Gauge 模块

### 封装术语

<table><colgroup><col width="50%"><col width="50%"></colgroup><thead><tr><th>术语</th><th>描述</th></tr></thead><tbody><tr><td><p>封装图标</p></td><td><p>使用绘图命令生成的封装模块图标。封装图标可以是静态的，也可以随底层模块参数值动态变化。</p></td></tr><tr><td><p>封装参数</p></td><td><p>在封装编辑器中定义并显示在封装对话框中的参数。在封装对话框中设置封装参数值将会设置对应的模块参数值。</p></td></tr><tr><td><p>封装初始化代码</p></td><td><p>用于初始化封装模块或反映当前参数值的 MATLAB 代码。在 “封装编辑器” 对话框的<strong>初始化</strong>窗格中添加封装初始化代码。例如，添加初始化代码以便自动设置参数值。</p></td></tr><tr><td><p>封装对话框回调代码</p></td><td><p>当封装参数的值更改时在基础工作区中运行的 MATLAB 代码。使用回调代码动态更改封装对话框的外观和反映当前参数值。例如，在对话框上启用可见参数。</p></td></tr><tr><td><p>封装文档</p></td><td><p>封装编辑器中定义的封装模块的描述和用法信息。</p></td></tr><tr><td><p>封装对话框</p></td><td><p>包含用于设置封装参数值的字段并提供封装描述的对话框。</p></td></tr><tr><td><p>封装工作区</p></td><td><p>定义了封装参数的封装或包含初始化代码的封装都会有一个封装工作区。此工作区用来存储封装参数的计算值和封装使用的临时值。</p></td></tr></tbody></table>

## 相关主题

* [创建模块封装](https://ww2.mathworks.cn/help/simulink/block-masks.html)
* [创建简单封装](https://ww2.mathworks.cn/help/simulink/ug/how-to-mask-a-block.html)
* [封装编辑器概述](https://ww2.mathworks.cn/help/simulink/gui/mask-editor-overview.html)
* [设置封装参数](matlab:open_system([matlabroot '/examples/simulink_masking/main/slexMaskParameterOptionsExample']))
* [创建封装：封装基础知识（3 分 46 秒）](https://www.mathworks.com/videos/creating-a-mask--masking-fundamentals-1480968643715.html)

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
