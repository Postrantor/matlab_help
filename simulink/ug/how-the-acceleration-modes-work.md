---
url: https://ww2.mathworks.cn/help/simulink/ug/how-the-acceleration-modes-work.html
title: 加速模式的工作原理
- MATLAB & Simulink
- MathWorks 中国
date: 2023-07-31 11:20:04
tag: 
summary: 比较和对比普通、加速和快速加速模式。
---
## 加速模式的工作原理

### 概述

加速和快速加速模式使用 Simulink® Coder™ 产品的部分内容创建可执行文件。

加速和快速加速模式会替换 Simulink 仿真中常用的解释代码，从而缩短模型的运行时间。

虽然加速模式会使用一些 Simulink Coder 代码生成技术，但您不需要安装 Simulink Coder 软件即可为您的模型加速。

**注意**

加速和快速加速模式生成的代码仅适用于加快模型仿真。对于其他目的，请使用 Simulink Coder 生成代码。

### 普通模式

在普通模式下，MATLAB® 技术计算环境是 Simulink 软件的基础环境。Simulink 控制仿真过程中使用的求解器和模型方法。模型方法包括模型输出的计算等内容。普通模式在一个进程中运行。

![](https://ww2.mathworks.cn/help/simulink/ug/rapid_accel_1a.png)

### 加速模式

默认情况下，加速模式采用即时 (JIT) 加速方式在内存中生成执行引擎，而不是生成 C 代码或 MEX 文件。您还可以将模型回退到经典加速模式，在这种模式下，Simulink 将生成代码并将代码链接到 C-MEX S-Function。

在加速模式下，模型方法与 Simulink 软件相分离，它们将作为之后进行仿真时使用的*加速目标代码*的一部分。

Simulink 会在重用加速目标代码之前检查代码是否为最新版本。有关详细信息，请参阅 [Code Regeneration in Accelerated Models](https://ww2.mathworks.cn/help/simulink/ug/code-regeneration-in-accelerated-models.html)。

在加速模式下，有两种操作模式。

#### 即时加速模式

在此默认模式下，Simulink 在内存中只为顶级模型（而不为引用模型）生成执行引擎。因此，仿真过程中不需要使用 C 编译器。

![](https://ww2.mathworks.cn/help/simulink/ug/rapid_accel_2b.png)

由于加速目标代码在内存中，因此只要模型处于打开状态，就可以重用这些代码。Simulink 还会序列化加速目标代码，因此当模型处于打开状态时，不需要重新构建模型。

#### 经典加速模式

要使用生成 C 代码的经典加速模式对您的模型进行仿真，请运行以下命令：

```
set_param(0,'GlobalUseClassicAccelMode','on');

```

在此模式下，Simulink 会生成代码并将代码链接到与 Simulink 软件进行通信的共享库。MATLAB 与 Simulink 的目标代码执行过程相同。

![](https://ww2.mathworks.cn/help/simulink/ug/rapid_accel_2a.png)

### 快速加速模式

快速加速模式从您的模型中创建一个*快速加速独立可执行文件*。这个可执行文件包含求解器和模型方法，但位于 MATLAB 和 Simulink 的外部。它使用外部模式（请参阅 [External Mode Communication](https://ww2.mathworks.cn/help/rtw/ug/about-model-execution.html#f17854) (Simulink Coder)）与 Simulink 通信。

![](https://ww2.mathworks.cn/help/simulink/ug/rapid_accel_3a.png)

MATLAB 和 Simulink 在一个进程中运行，如果有第二个处理内核可用，独立可执行文件将在该内核中运行。

## 相关主题

- [设计模型以实现有效加速](https://ww2.mathworks.cn/help/simulink/ug/designing-your-model-for-effective-acceleration.html)
- [执行加速](https://ww2.mathworks.cn/help/simulink/ug/performing-acceleration.html)
- [选择仿真模式](https://ww2.mathworks.cn/help/simulink/ug/choosing-a-simulation-mode.html)
- [Code Regeneration in Accelerated Models](https://ww2.mathworks.cn/help/simulink/ug/code-regeneration-in-accelerated-models.html)
- [比较性能](https://ww2.mathworks.cn/help/simulink/ug/comparing-performance.html)

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
