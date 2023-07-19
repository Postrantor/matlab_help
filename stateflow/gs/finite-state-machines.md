## 对有限状态机建模

Stateflow® 是基于*有限状态机*的图形化编程环境。使用 Stateflow，您可以测试和调试您的设计，考虑不同仿真场景，并从状态机生成代码。

有限状态机是从一种工作模式（状态）转移到另一种工作模式的动态系统的表示。状态机可以：

- 作为复杂软件设计过程的高级起点。
- 让您能够专注于工作模式和从一种模式转至下一种模式所需的条件。
- 帮助您设计即使模型复杂度增加也能保持简洁清晰的模型。

控制系统设计在很大程度上依赖于状态机来管理复杂的逻辑。应用包括设计飞机、汽车和机器人控制系统。

### Stateflow 图的示例

Stateflow 图将状态、转移和数据组合起来实现有限状态机。以下 Stateflow 图提供了汽车四速自动传动系统换挡逻辑的简化模型。该图通过状态表示每个挡位，分别显示为一个带有 `first`、`second`、`third` 或 `fourth` 标签的矩形。像它们代表的挡位一样，这些状态是互斥的，因此在某个时刻只有一个状态被激活。

![](https://ww2.mathworks.cn/help/stateflow/gs/gearlogic.png)

图左侧的箭头表示默认转移，并指示第一个状态被激活。当您执行该图时，此状态在画布上突出显示。其他箭头表示各状态之间可能的转移。要定义状态机的动态，请将每个转移与一个布尔条件或触发事件相关联。例如，下图监控车速，并在速度超过固定阈值时切换到另一个挡位。在仿真过程中，图中的突出显示随着不同状态被激活而变化。

![](https://ww2.mathworks.cn/help/stateflow/gs/gearlogic-animation.gif)

此图仅提供了简单设计，未考虑发动机转速和扭矩等重要因素。您可以通过将此 Stateflow 图与 MATLAB® 或 Simulink® 中的其他组件链接起来，构建一个更全面和更真实的模型。以下示例说明三种可能的方法。

### 将图作为 MATLAB 对象执行

此示例展示了自动传输系统的一个修改版本，其中包含了状态层次结构、时序逻辑和输入事件。

- **层次结构：**该图包含一个父状态 `gear_logic`，其中包含了上例中的四速自动变速器图。此父状态控制车辆的速度和加速度。在执行期间，`gear_logic` 始终处于激活状态。
- **时序逻辑：**在 `gear_logic` 状态中，动作 `on every(0.25,sec)` 决定车速。运算符 `[every](https://ww2.mathworks.cn/help/stateflow/ref/every.html)` 创建一个 MATLAB 计时器，它执行图并每隔 0.25 秒更新一次图数据 `speed`。
- **输入事件：**输入事件 `SpeedUp`、`Cruise` 和 `SlowDown` 重置图数据 `delta` 的值。此数据决定汽车在每个执行步中是加速还是保持其速度。

![](https://ww2.mathworks.cn/help/stateflow/gs/executestateflowchartobjectgettingstartedexample_01_zh_CN.png)

您可以通过命令行窗口或使用脚本直接在 MATLAB 中将该图作为对象来执行。您还可以编写一个 MATLAB App 以通过图形用户界面控制图状态。例如，当您点击某按钮时，该用户界面向图发送输入事件。在图中，MATLAB 函数 `widgets` 控制界面上仪表和信号灯的值。要启动示例，请在 App 设计工具的工具条中，点击**运行**。示例将继续运行，直到您关闭用户界面窗口。

![](https://ww2.mathworks.cn/help/stateflow/gs/xxsf_car_dash_screenshot_zh_CN.png)

或者，在 Stateflow 编辑器的**状态图**选项卡中，点击**运行**。要控制车速，请在**符号**窗格中使用**加速**、**减速**和**巡航**按钮。要停止示例，请点击**停止**。

有关将 Stateflow 图作为 MATLAB 对象执行的详细信息，请参阅[在 MATLAB 中执行](https://ww2.mathworks.cn/help/stateflow/execution-in-matlab.html)。

### 将图作为具有局部事件的 Simulink 模块进行仿真

此示例提供了一种更复杂的自动变速器系统设计。Stateflow 图在 Simulink 模型中显示为模块。模型中的其他模块代表相关汽车组件。该图通过输入和输出连接来共享数据，进而与其他模块进行对接。要打开该图，请点击 `shift_logic` 模块左下角的箭头。

![](https://ww2.mathworks.cn/help/stateflow/gs/automatictransmissionwithactivestatedataexample_01_zh_CN.png)

该图合并了状态层次结构、并行机制、激活状态数据、局部事件和时序逻辑。

- **层次结构：**状态 `gear_state` 包含四速自动变速器图的一个修改版本。状态 `selection_state` 包含代表稳定状态、升挡和降挡工作模式的子状态。当需要升挡或降挡时，这些子状态将被激活。
- **并行机制：**并行状态 `gear_state` 和 `selection_state` 显示为带有虚线边框的矩形。这些状态同时工作，即使其内部的子状态存在互斥也是如此。
- **激活状态数据：**输出值 `gear` 反映仿真过程中挡位的选择。图会根据 `gear_state` 中的激活子状态生成此值。
- **局部事件：**此图不使用布尔条件，而是使用局部事件 `UP` 和 `DOWN` 触发挡位之间的转移。这些事件由 `selection_state` 中的 `[send](https://ww2.mathworks.cn/help/stateflow/ref/send.html)` 命令触发，当车速超出所选挡位的工作范围时会发出这些命令。Simulink 函数 `calc_th` 根据选择的挡位和发动机转速确定工作范围的边界值。
- **时序逻辑：**为了防止连续快速换挡，`selection_state` 使用时序逻辑运算符 `[after](https://ww2.mathworks.cn/help/stateflow/ref/after.html)` 来延迟 `UP` 和 `DOWN` 事件的广播。仅当所需的换挡时间超过某个预定时间 `TWAIT` 时，状态才会广播这些事件之一。

![](https://ww2.mathworks.cn/help/stateflow/gs/automatictransmissionwithactivestatedataexample_02_zh_CN.png)

要运行模型仿真，请执行以下操作：

1. 双击 **User Inputs** 模块。在 “信号编辑器” 对话框中，从**激活场景**列表中选择一个预定义的刹车到油门的配置文件。默认配置文件是 `Passing Maneuver`。
2. 点击**运行**。在 Stateflow 编辑器中，图动画会突出显示仿真过程中的激活状态。要减慢动画速度，请在**调试**选项卡中，从**动画速度**下拉列表中选择 `Slow`。
3. 在 Scope 模块中，检查仿真结果。每个示波器都会在仿真过程中显示其输入信号的图形。

![](https://ww2.mathworks.cn/help/stateflow/gs/automatictransmissionwithactivestatedataexample_03_zh_CN.png)

![](https://ww2.mathworks.cn/help/stateflow/gs/automatictransmissionwithactivestatedataexample_04_zh_CN.png)

### 将图作为带时序条件的 Simulink 模块进行仿真

此示例提供了汽车传动系统建模的另一种方法。Stateflow 图在 Simulink 模型中显示为模块。模型中的其他模块代表相关汽车组件。该图通过输入和输出连接来共享数据，进而与其他模块进行对接。要打开该图，请点击 `Gear_logic` 模块左下角的箭头。

![](https://ww2.mathworks.cn/help/stateflow/gs/carusingdurationgettingstartedexample_01_zh_CN.png)

该图合并了状态层次结构、激活状态数据和时序逻辑。

- **层次结构：**此模型将四速自动变速器图置于父状态 `gear` 中。该父状态监控车辆速度和发动机转速，并触发换挡。状态 gear 左上角列出的动作确定了所选挡位的运行阈值以及布尔条件 `up` 和 `down` 的值。标签 `en,du` 指示在状态第一次被激活 (`en` = `entry`) 和在状态已激活时的每个后续时间步 (`du` = `during`) 执行状态动作。
- **激活状态数据：**输出值 `gear` 反映仿真过程中挡位的选择。图会根据 `gear` 中的激活子状态生成此值。
- **时序逻辑：**为了防止连续快速换挡，布尔条件 `up` 和 `down` 使用时序逻辑运算符 `[duration](https://ww2.mathworks.cn/help/stateflow/ref/duration.html)` 来控制挡位之间的转移。当车速保持在所选挡位工作范围之外超过某个预定时间 `TWAIT`（以秒为单位测量）时，条件有效。

![](https://ww2.mathworks.cn/help/stateflow/gs/carusingdurationgettingstartedexample_02_zh_CN.png)

要运行模型仿真，请执行以下操作：

1. 双击 **User Inputs** 模块。在 “信号编辑器” 对话框中，从**激活场景**列表中选择一个预定义的刹车到油门的配置文件。默认配置文件是 `Passing Maneuver`。
2. 点击**运行**。在 Stateflow 编辑器中，图动画会突出显示仿真过程中的激活状态。要减慢动画速度，请在**调试**选项卡中，从**动画速度**下拉列表中选择 `Slow`。
3. 在 Scope 模块中，检查仿真结果。示波器显示仿真期间的挡位选择变化图。

![](https://ww2.mathworks.cn/help/stateflow/gs/carusingdurationgettingstartedexample_03_zh_CN.png)

### 后续步骤

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
