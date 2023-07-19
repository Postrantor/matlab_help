> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.mathworks.com](https://www.mathworks.com/videos/reduced-bus-wiring-bus-element-ports-1487889625625.html)

> 快速将信号分组为总线，并自动创建总线元件端口，以减少......

在 Simulink® 中通过单击操作将信号快速分组为总线并自动创建总线元件端口。这些总线元件端口块提供了一种简化且灵活的方式来使用总线信号作为子系统的输入和输出。Bus Element 模块相当于 Inport 或 Outport 模块与 Bus Selector 或 Bus Creator 模块的组合。它们允许接线的极大灵活性，以及 ​​ 按名称和颜色分组的简单规范。

使用总线元件端口可以减少信号线的复杂性和框图中的混乱，允许您通过许多单独的模块访问总线端口，这些模块可以放置在更靠近使用位置的位置，而不是将它们全部绑定到单个 Outport 或 Inport 模块。它们还可以让您在设计时轻松更改界面，因为它们可以在图表中相互独立地移动。总体而言，总线元素端口使模型更易于创建，也更易于阅读和共享。
