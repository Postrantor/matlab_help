---
> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [ww2.mathworks.cn](https://ww2.mathworks.cn/help/stateflow/gs/hierarchy.html)

> 通过使用层次结构在复杂系统中设计多层次的逻辑。
---
（步骤 3/7） 对有限状态机建模

构建模型时，以一次添加一个子组件的方式创建一个包含嵌套状态的层次结构。这样，您便可以控制 Stateflow® 图中复杂系统的多个层级。

要创建状态层次结构，请将一个或多个状态置于另一个状态的边界内。内部状态是外部状态的子级（或*子状态*）。外部状态是内部状态的父级（或*父状态*）。

![](https://ww2.mathworks.cn/help/stateflow/gs/hierarchygetstartedexample_01_zh_CN.png)

父状态的内容在行为上与较小的图类似。当父状态被激活时，其子状态之一也被激活。当父状态变为非激活时，其所有子状态都变为非激活。

### 媒体播放器建模

此示例对一个由 AM 无线电、FM 无线电和 CD 播放器组成的媒体系统进行建模。在仿真过程中，您可以通过点击 Media Player Helper 用户界面上的按钮来控制媒体播放器系统。

![](https://ww2.mathworks.cn/help/stateflow/gs/xxsfmediaplayer_ui_zh_CN.png)

媒体播放器最初处于关闭状态。当您选择一个 **Radio Request** 按钮时，媒体播放器将打开对应的子组件。如果您选择 CD 播放器，则可以点击其中一个 **CD Request** 按钮以选择 Play、Rewind、Fast-Forward 或 Stop。您可以在仿真过程中的任何时刻插入或弹出光盘。

### 通过使用状态层次结构实现行为

此示例通过一次只关注一个层面的活动来逐步实现媒体播放器。例如，下面列出了 CD 播放器进入快进播放模式的必要条件：

1. 您打开媒体播放器。
2. 打开了 CD 播放器。
3. 您开始播放一张光盘。
4. 您点击 FF 按钮。

该模型使用状态层次结构来单独考虑这些条件中的每个条件。例如，该层次结构的顶层定义当媒体播放器打开和关闭时的工作模式。中间各层定义不同媒体播放器子组件之间的转移，以及 CD 播放器的停止和播放模式之间的转移。层次结构的最底层定义当满足播放光盘的所有其他条件时对 **CD Request** 按钮的响应。

在模型资源管理器中 `Mode Manager` 图的下方，您可以看到实现媒体播放器行为的嵌套状态的层次结构。要打开模型资源管理器，请在**建模**选项卡中，选择**模型资源管理器**。

![](https://ww2.mathworks.cn/help/stateflow/gs/xxsf_mediaplayer-model-explorer_zh_CN.png)

在层次结构的顶层，`Mode Manager` 图有两种状态：

- `Eject`：对应于光盘弹出模式，该模式会中断所有其他媒体播放器功能。
- `NormalOperation`：对应于媒体播放器的正常工作模式。

`NormalOperation` 的子状态控制媒体播放器的活动：

- `Standby`：当媒体播放器关闭时，该状态被激活。
- `ON`：当媒体播放器打开时，该状态被激活。

`On` 的子状态控制媒体播放器子组件：

- `CDMode`：当 CD 播放器子组件打开时，该状态被激活。
- `AMMode`：当 AM 无线电子组件打开时，该状态被激活。
- `FMMode`：当 FM 无线电子组件打开时， 该状态被激活。

`CDMode` 的子状态控制 CD 播放器的活动：

- `Stop`：当 CD 播放器停止时，该状态被激活。
- `Play`：当 CD 播放器播放光盘时，该状态被激活。

`Play` 的子状态控制 CD 播放器的播放模式：

- `Normal`：在正常播放模式下，该状态被激活。
- `Rewind`：在反向播放模式下，该状态被激活。
- `FastForward`：在快进播放模式下，该状态被激活。

下图显示了图中状态的完整布局。

![](https://ww2.mathworks.cn/help/stateflow/gs/hierarchygetstartedexample_02_zh_CN.png)

### 探索示例

此示例中的模型包含另外两个 Stateflow 图：

- `App Interface` 管理与 UI 的接口，并将输入传递给 `Mode Manager` 和 `CD Player` 图。
- `CD Player` 接收 `App Interface` 和 `Mode Manager` 图的输出，并模拟 CD 播放器的机械行为。

![](https://ww2.mathworks.cn/help/stateflow/gs/hierarchygetstartedexample_03_zh_CN.png)

在仿真过程中，您可以研究每个图如何响应与 Media Player Helper 的交互。要在各图之间快速切换，请使用 Stateflow 编辑器顶部的选项卡。

## 相关主题

- [使用状态层次结构设计多级状态复杂性](https://ww2.mathworks.cn/help/stateflow/ug/state-hierarchy.html)

您曾对此示例进行过修改。是否要打开带有您的编辑的示例？

本页内容对您有帮助吗？

Unrated

1 star

2 stars

3 stars

4 stars

5 stars

Unrated 1 star 2 stars 3 stars 4 stars 5 stars

您点击的链接对应于以下 MATLAB 命令：

请在 MATLAB 命令行窗口中直接输入以执行命令。Web 浏览器不支持 MATLAB 命令。
