---
url: https://ww2.mathworks.cn/help/simulink/model-reference.html
title: 模型引用
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-18 21:00:02
tag: 
summary: 将模型作为模块重用于其他模型
---
将模型作为模块重用于其他模型

_模型引用_是对使用 Model 模块的另一个模型的引用。这些引用会创建模型层次结构。每个引用模型都有一个定义的接口，该接口指定其输入和输出的属性。定义的接口使得引用模型的行为独立于它在模型层次结构中的上下文。对于仿真和代码生成，当父模型执行时，引用模型像单个模块（即原子单元）一样执行。模型引用非常适合代码重用、单元测试、并行编译和大型组件。它们还可以减少文件争用和合并问题。

要确定引用模型是否满足您的建模需求，请参阅[基于组件的建模规范](https://ww2.mathworks.cn/help/simulink/ug/component-based-modeling-guidelines.html)。

要了解模型引用层次结构的代码生成，请参阅[引用模型](https://ww2.mathworks.cn/help/rtw/referenced-models.html) (Simulink Coder)。

要创建受保护模型，请参阅[模型保护](https://ww2.mathworks.cn/help/rtw/model-protection.html) (Simulink Coder)。

## 模块

<table><tbody><tr><td><a href="https://ww2.mathworks.cn/help/simulink/slref/model.html"><span>Model</span></a></td><td>引用另一个模型来创建模型层次结构</td></tr><tr><td><a href="https://ww2.mathworks.cn/help/simulink/slref/variantsubsystemvariantmodelvariantassemblysubsystem.html" hreflang="en"><span>Variant Model</span></a></td><td>Template subsystem containing Subsystem, Model, or Subsystem Reference blocks as variant choices</td></tr></tbody></table>

## 工具

<table><tbody><tr><td><a href="https://ww2.mathworks.cn/help/simulink/slref/modelreferenceconversionadvisor.html" hreflang="en">模型引用转换顾问</a></td><td>Convert subsystems to referenced models</td></tr><tr><td><a href="https://ww2.mathworks.cn/help/simulink/slref/referencedfilespane.html" hreflang="en">引用的文件窗格</a></td><td>View, save, and close referenced subsystems and models</td></tr></tbody></table>

## 主题

### 确定何时引用模型

*   **[基于组件的建模规范](https://ww2.mathworks.cn/help/simulink/ug/component-based-modeling-guidelines.html)**  
    考虑大型模型和多用户开发团队的组件化。
*   **[模型引用基础知识](https://ww2.mathworks.cn/help/simulink/ug/overview-of-model-referencing-1.html)**  
    通过在一个模型中引用另一个模型可以创建模型层次结构。引用模型中包含多个模块，这些模块作为一个单元一起执行。
*   **[模型引用的要求和限制](https://ww2.mathworks.cn/help/simulink/ug/model-referencing-limitations.html)**  
    模型引用在可重用性、仿真模式、封装和调试等功能方面有一定的要求和限制。

## 精选示例

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