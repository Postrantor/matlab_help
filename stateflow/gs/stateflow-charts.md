---
> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [ww2.mathworks.cn](https://ww2.mathworks.cn/help/stateflow/gs/stateflow-charts.html)
> 在 Stateflow 图形编程编辑器中构建状态转移图。
---
## 构造并运行 Stateflow 图

Stateflow® 图是有限状态机的图形表示，由状态、转移和数据组成。您可以创建 Stateflow 图来定义 MATLAB® 算法或 Simulink® 模型如何响应外部输入信号、事件和基于时间的条件。

例如，下面的 Stateflow 图展示半波整流器的基础逻辑。该图包含两个标签为 `On` 和 `Off` 的状态。在 `On` 状态下，图输出信号 `y` 等于输入 `x`。在 `Off` 状态下，输出信号设置为零。当输入信号跨越某个阈值 `t0` 时，图在这些状态之间转移。各个状态下的动作在仿真的每一时间步都会更新 `y` 的值。

![](https://ww2.mathworks.cn/help/stateflow/gs/rectify-step4.png)

此示例说明如何创建这样的 Stateflow 图，以在 Simulink 中进行仿真和在 MATLAB 中执行。

### 构造 Stateflow 图

#### 打开 Stateflow 编辑器

Stateflow 编辑器是一个图形环境，用于设计状态转移图、流程图、状态转移表和真值表。在打开 Stateflow 编辑器之前，需要先确定最能满足您需求的图执行模式。

- 要建立周期性或连续时间 Simulink 算法的条件、基于事件和基于时间的逻辑模型，请使用 [`sfnew`](https://ww2.mathworks.cn/help/stateflow/ref/sfnew.html) 函数创建一个可在 Simulink 模型中作为模块进行仿真的 Stateflow 图。在 MATLAB 命令提示符处，输入：

  ```
  sfnew rectify     % create chart for simulation in a Simulink model
  ```

  Simulink 创建一个名为 `rectify` 的模型，其中包含一个空的 Stateflow [Chart](https://ww2.mathworks.cn/help/stateflow/ref/chart.html) 模块。要打开 Stateflow 编辑器，请双击图模块。
- 要为 MATLAB 应用程序设计可重用的状态机和时序逻辑，请使用 [`edit`](https://ww2.mathworks.cn/help/matlab/ref/edit.html) 函数创建可作为 MATLAB 对象执行的独立 Stateflow 图。在 MATLAB 命令提示符处，输入：

  ```
  edit rectify.sfx  % create chart for execution as a MATLAB object
  ```

  如果文件 `rectify.sfx` 不存在，Stateflow 编辑器将创建名为 `rectify` 的空图。

Stateflow 编辑器的主要组件是图画布、对象选项板和**符号**窗格。

- 图画布是一个绘图区域，您可以在其中通过组合状态、转移和其他图形元素来创建图。
- 在画布的左侧有一个对象选项板，其中显示了一组可向图中添加图形元素的工具。
- 在画布的右侧有一个**符合**窗格，您可以用它向图添加新的数据、事件和消息并解析任何未定义或未使用的符号。

![](https://ww2.mathworks.cn/help/stateflow/gs/stateflow-editor.png)

**提示**

在构造 Stateflow 图后，您可以将其内容复制到另一个具有不同执行模式的图中。例如，您可以构造在 MATLAB 中执行的图，并将其内容复制到在 Simulink 中进行仿真的图中。

#### 添加状态和转移

1. 在对象选项板中，点击**状态**图标 ![](https://ww2.mathworks.cn/help/stateflow/gs/palette-state.png) 并将指针移至图画布。将出现具有默认转移的状态。要放置该状态，请点击画布上的某个位置。在文本提示中，输入状态名称 `On` 和状态动作 `y = x`。

   ![](https://ww2.mathworks.cn/help/stateflow/gs/rectify-step1.png)
2. 添加另一个状态。右键点击并拖动 `On` 状态。蓝色图形提示可以帮助您水平或垂直对齐状态。新状态的名称变为 `Off`。双击该状态并将状态动作修改为 `y = 0`。

   ![](https://ww2.mathworks.cn/help/stateflow/gs/rectify-step2.png)
3. 重新对齐两个状态并在两个状态之间的空白处停留片刻。蓝色转移提示指示您可以连接状态的几种方式。要添加转移，请点击适当的提示。

   或者，要绘制转移，请点击并从一个状态的边拖动到另一个状态的边。

   ![](https://ww2.mathworks.cn/help/stateflow/gs/rectify-step3.png)
4. 双击每个转移并输入适当的转移条件 `x<t0` 或 `x>=t0`。条件出现在方括号内。

   ![](https://ww2.mathworks.cn/help/stateflow/gs/rectify-step4.png)
5. 清理图：

   - 为使图更加清晰，将每个转移标签移到其对应转移上方或下方的方便位置。
   - 要对齐图的图形元素并调整其大小，请在**格式**选项卡中，点击**自动排列**或按 **Ctrl+Shift+A**。
   - 要调整图的大小以适合画布，请按空格键或点击**适应视图大小**图标 ![](https://ww2.mathworks.cn/help/stateflow/gs/fit-to-view.png)。

#### 解析未定义的符号

在执行图之前，必须定义图中使用的每个符号并指定其作用域（例如，输入数据、输出数据或局部数据）。在**符号**窗格中，未定义的符号用红色错误标记 ![](https://ww2.mathworks.cn/help/stateflow/gs/symbols-error.png) 进行标记。**类型**列根据每个未定义符号在图中的使用情况显示其建议作用域。

1. 打开**符号**窗格。

   - 如果您构建的是 Simulink 模型中的图，请在**建模**选项卡中，在**设计数据**下，选择**符号窗格**。
   - 如果您构建的是要在 MATLAB 中执行的独立图，请在**状态图**选项卡中选择 > 。
2. 在**符号**窗格中，点击**解析未定义的符号** ![](https://ww2.mathworks.cn/help/stateflow/gs/symbols-resolve-undefined.png)。

   - 如果构建的是在 Simulink 模型中的图，Stateflow 编辑器会将符号 `x` 和 `t0` 解析为输入数据 ![](https://ww2.mathworks.cn/help/stateflow/gs/symbols-input-data.png)，将 `y` 解析为输出数据 ![](https://ww2.mathworks.cn/help/stateflow/gs/symbols-output-data.png)。
   - 如果您构建的是要在 MATLAB 中执行的独立图，Stateflow 编辑器则将 `t0`、`x` 和 `y` 解析为局部数据 ![](https://ww2.mathworks.cn/help/stateflow/gs/symbols-local-data.png)。

   ![](https://ww2.mathworks.cn/help/stateflow/gs/rectify-resolve-symbols.png)
3. 由于阈值 `t0` 在仿真过程中不会更改，因此将其作用域更改为常量数据。在**类型**列中，点击 `t0` 旁边的数据类型图标，然后选择![](https://ww2.mathworks.cn/help/stateflow/gs/symbols-constant-data.png) “`常量数据`”。
4. 设置阈值 `t0` 的值。在**值**列中，点击 `t0` 旁边的空白输入框，并输入值 0。
5. 保存您的 Stateflow 图。

您的图现在即可在 Simulink 中进行仿真，或者在 MATLAB 中执行。

### 将图作为 Simulink 模块进行仿真

要在 Simulink 模型中对图进行仿真，请通过输入和输出端口将图模块连接到模型中的其他模块。如果您要从 MATLAB 命令行窗口中执行图，请参阅[将图作为 MATLAB 对象执行](https://ww2.mathworks.cn/help/stateflow/gs/stateflow-charts.html#mw_210af1c7-36a5-4635-823a-dcfc5b2c1729)。

1. 要返回到 Simulink 编辑器，请在画布顶部的浏览器栏中点击 Simulink 模型的名称：![](https://ww2.mathworks.cn/help/stateflow/gs/simulink-model.png)“`rectify`”。如果浏览器栏不可见，请点击对象选项板顶部的**隐藏 / 显示资源管理器栏**图标 ![](https://ww2.mathworks.cn/help/stateflow/gs/palette-explorer-bar.png)。
2. 执行以下操作以将信源添加到模型中：

   - 从 Simulink Sources 库中，添加一个 [Sine Wave](https://ww2.mathworks.cn/help/simulink/slref/sinewave.html) (Simulink) 模块。
   - 双击 Sine Wave 模块并将**采样时间**设置为 0.2。
   - 将 Sine Wave 模块的输出连接到 Stateflow 图的输入。
   - 将信号标记为 `x`。
3. 向模型中添加一个信宿：

   - 从 Simulink Sinks 库中，添加一个具有两个输入端口的 [Scope](https://ww2.mathworks.cn/help/simulink/slref/scope.html) (Simulink) 模块。
   - 将 Sine Wave 模块的输出连接到 Scope 模块的第一个输入。
   - 将 Stateflow 图的输出连接到 Scope 模块的第二个输入。
   - 将信号标记为 `y`。
4. 保存 Simulink 模型。

   ![](https://ww2.mathworks.cn/help/stateflow/gs/rectify-model.png)
5. 要仿真模型，请点击**运行** ![](https://ww2.mathworks.cn/help/stateflow/gs/toolstrip-run-simulation.png)。在仿真过程中，Stateflow 编辑器通过图动画突出显示激活状态和转移。
6. 对模型进行仿真后，双击 Scope 模块。示波器显示 Stateflow 图的输入信号和输出信号图。

   ![](https://ww2.mathworks.cn/help/stateflow/gs/rectify-scope.png)

   仿真结果显示整流器滤除了负输入值。

### 将图作为 MATLAB 对象执行

要在 MATLAB 命令行窗口中执行图，请创建一个图对象，并调用其 `step` 函数。如果您要在 Simulink 模型中对图进行仿真，请参阅[将图作为 Simulink 模块进行仿真](https://ww2.mathworks.cn/help/stateflow/gs/stateflow-charts.html#mw_83cbfede-9ce2-4ad8-99df-68a135a08694)。

1. 通过使用包含图定义作为函数的 `sfx` 文件的名称，创建一个图对象 `r`。将图数据 `x` 的初始值指定为名称 - 值对组。
2. 初始化图执行的输入和输出数据。向量 `X` 包含来自正弦波的输入值。向量 `Y` 是一个空的累加器。

   ```
   T = 0:0.2:10;
   X = sin(T);
   Y = [];
   ```
3. 通过多次调用 `step` 函数来执行图对象。将来自向量 `X` 的单个值作为图数据 `x` 传递。在向量 `Y` 中收集 `y` 的结果值。在执行过程中，Stateflow 编辑器通过图动画突出显示激活状态和转移。

   ```
   for i = 1:51
       step(r,x=X(i));
       Y(i) = r.y;
   end
   ```
4. 从 MATLAB 工作区中删除图对象 `r`。
5. 检查图执行的结果。例如，您可以调用 [`stairs`](https://ww2.mathworks.cn/help/matlab/ref/stairs.html) 函数来创建一个阶梯图，用于比较 `X` 和 `Y` 的值。

   ```
   ax1 = subplot(2,1,1);
   stairs(ax1,T,X,color="#0072BD")
   title(ax1,"x")

   ax2 = subplot(2,1,2);
   stairs(ax2,T,Y,color="#D95319")
   title(ax2,"y")
   ```

   ![](https://ww2.mathworks.cn/help/stateflow/gs/rectify-matlab-plot.png)

   执行结果显示整流器滤除了负输入值。

## 相关主题

- [Stateflow 编辑器操作](https://ww2.mathworks.cn/help/stateflow/ug/editor-operations.html)
- [Manage Symbols in the Stateflow Editor](https://ww2.mathworks.cn/help/stateflow/ug/use-the-symbols-window-with-stateflow-data-events-and-messages.html)
- [Create Stateflow Charts for Execution as MATLAB Objects](https://ww2.mathworks.cn/help/stateflow/ug/create-stateflow-chart-objects.html)

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
