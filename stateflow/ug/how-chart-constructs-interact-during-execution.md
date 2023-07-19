在执行过程中，Stateflow® 对象彼此交互以仿真现实世界的行为。此示例使用酒店入住流程来说明 Stateflow 图中常见的图形对象和非图形对象在执行过程中如何交互。

### 酒店入住流程的模型

此模型包含一个名为 `Hotel` 的 Stateflow 图。该图从四个 [Manual Switch](https://ww2.mathworks.cn/help/simulink/slref/manualswitch.html) (Simulink) 模块接收输入事件，您将这些模块切换到：

- 入住酒店
- 呼叫客房服务
- 发出火警
- 火警后发出警报解除信号

[Mux](https://ww2.mathworks.cn/help/simulink/slref/mux.html) (Simulink) 模块将这些输入事件组合为一个输入向量，该向量连接到图顶部的触发端口。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/SemanticsHotelCheckinExample_01.png)

该图还接收一个来自 [Multiport Switch](https://ww2.mathworks.cn/help/simulink/slref/multiportswitch.html) (Simulink) 模块的名为 `room_type` 的输入信号。此信号的值对应于您要入住的客房的类型。可供选择的选项有 `"Executive"`（行政套房）、`"Family"`（家庭套房）和 `"Single"`（单间）。

在仿真期间，包括客房服务费用在内的应付总额显示在 [Display](https://ww2.mathworks.cn/help/simulink/slref/display.html) (Simulink) 模块中。

`Hotel` 图包含图形对象（如状态和历史结点）以及非图形对象（如数据和事件）。要查看标记此图中对象的图像，请参阅 [Stateflow Objects](https://ww2.mathworks.cn/help/stateflow/ug/what-do-semantics-mean-for-stateflow-charts.html#mw_7396c3fd-d93d-4a52-ab82-09a9d242a075)。

![](https://ww2.mathworks.cn/help/stateflow/ug/semanticshotelcheckinexample_02_zh_CN.png)

当您开始仿真时，图不会唤醒，直到它检测到输入事件之一中存在上升沿或下降沿时才会唤醒。

当您切换 Manual Switch 模块时，您会触发唤醒该图的一个输入事件。当图唤醒时，它从 Multiport Switch 模块读取图输入 `room_type` 的值，执行任何有效的状态或转移动作，并将 `fee` 的新值输出到 Display 模块。

在完成所有可能的执行阶段后，图返回休眠状态，等待下一个输入事件。

### 图初始化

开始仿真并触发输入事件之一。此动作对应的是进入酒店并在前台处停留。

由于图属性**初始化时执行 (进入) 图**被禁用，图会保持休眠状态，直到它检测到其输入事件之一中存在上升沿或下降沿。然后，图会唤醒并执行其默认转移。发生到状态 `Check_in` 的默认转移，使该状态激活。然后，发生到子状态 `Front_desk` 的默认转移，使该状态激活。然后图进入休眠状态。有关详细信息，请参阅[初始化时的图执行](https://ww2.mathworks.cn/help/stateflow/ug/types-of-chart-execution.html#bqkr2e1)和[进入图或状态](https://ww2.mathworks.cn/help/stateflow/ug/chart-initialization-and-entry-actions.html)。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/xxsemantics-hotel-chart-initialization.png)

### 状态之间的转移

当子状态 `Front_desk` 激活时，触发输入事件 `check_in`。此动作对应于入住酒店。您拿起包，从前台走到您的客房，然后放下包。

在图中，`check_in` 事件保护从子状态 `Front_desk` 到子状态 `Checked_in` 的出向转移。当您触发该事件时，转移即生效。`Front_desk` 的 exit 动作将局部数据对象 `move_bags` 的值设置为 1，并且子状态变为非激活。然后，`Checked_in` 被激活，entry 动作将 `move_bags` 设置为 0。有关详细信息，请参阅 [How Stateflow Charts Respond to Events](https://ww2.mathworks.cn/help/stateflow/ug/how-events-drive-chart-execution.html#f26-1022386)、[退出状态](https://ww2.mathworks.cn/help/stateflow/ug/chart-exit-actions.html)和[进入图或状态](https://ww2.mathworks.cn/help/stateflow/ug/chart-initialization-and-entry-actions.html)。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/xxsemantics-hotel-checkin-transition.png)

### 默认转移路径的计算

在图于 `Checked_in` 状态下执行 `entry` 动作后，会计算到子状态之一的默认转移路径。被激活的子状态对应于客房类型。如果选择行政套房，基础费用为 1500 美元。如果选择家庭套房，基础费用为 1000 美元。如果选择单间，基础费用为 500 美元。

图按以下顺序测试默认转移路径的分支：

- 如果图输入 `room_type` 等于 `"Executive"`，则顶部转移有效。条件动作将图输出 `fee` 设置为 1500，子状态 `Executive_suite` 被激活。
- 如果图输入 `room_type` 等于 `"Family"`，则中间转移有效。条件动作将费用设置为 1000，子状态 `Family_suite` 被激活。
- 否则，图输入 `room_type` 等于 `"Single"`，底部转移有效。条件动作将费用设置为 500，子状态 `Single_room` 被激活。

有关详细信息，请参阅 [Order of Execution for a Set of Flow Charts](https://ww2.mathworks.cn/help/stateflow/ug/process-for-grouping-and-executing-transitions.html#f26-1020691)。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/xxsemantics-hotel-default-transition.png)

### 具有互斥子状态的状态的执行

如果在图输入 `room_type` 的值为 `"Executive"` 时触发输入事件 `check_in`，子状态 `Executive_suite` 被激活。此子状态对应于待在行政套房中。行政套房有单独的卧室和用餐区，您任一时刻都只能待在套房的一个区域内。抵达行政套房，您首先进入卧室。当您呼叫客房服务时，您会进入用餐区就餐。当您要将食物从用餐区拿走时，您再次呼叫客房服务，然后回到卧室。

状态 `Executive_suite` 有互斥 (OR) 分解。该状态有两个子状态，即 `Bedroom` 和 `Dining_area`。当 `Executive_suite` 第一次被激活时，会发生到 `Bedroom` 的默认转移，从而使该子状态被激活。输入事件 `room_service` 的广播触发从 `Bedroom` 到 `Dining_area` 的转移，使得 `Bedroom` 变为非激活而 `Dining_area` 被激活。`room_service` 的后续广播触发从 `Dining_area` 到 `Bedroom` 的转移，使得 `Bedroom` 被激活而 `Dining_area` 变为非激活。有关详细信息，请参阅[进入图或状态](https://ww2.mathworks.cn/help/stateflow/ug/chart-initialization-and-entry-actions.html)。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/xxsemantics-hotel-executive-suite.png)

### 具有并行子状态的状态的执行

如果在图输入 `room_type` 的值为 `"Family"` 时触发输入事件 `check_in`，子状态 `Family_suite` 被激活。此子状态对应于待在家庭套房中。当您的家人到达家庭套房时，家庭成员可以待在两个卧室中。例如，父母可以在第一间卧室看电影，而孩子在第二间卧室睡觉。

状态 `Family_suite` 有并行 (AND) 分解。该状态有两个子状态，即 `First_bedroom` 和 `Second_bedroom`。当 `Family_suite` 被激活时，并行状态根据其执行顺序唤醒，如每个状态右上角的数字所示。两个子状态同时保持激活状态。有关详细信息，请参阅[并行状态的执行顺序](https://ww2.mathworks.cn/help/stateflow/ug/execution-order-for-parallel-states.html)和[进入图或状态](https://ww2.mathworks.cn/help/stateflow/ug/chart-initialization-and-entry-actions.html)。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/xxsemantics-hotel-family-suite.png)

### 基于状态动作的函数调用

当子状态 `Checked_in` 激活时，触发输入事件 `room_service`。此动作对应于呼叫客房服务。您的酒店账单取决于您的客房类型和请求客房服务的次数。

当图在输入事件 `room_service` 中检测到上升沿或下降沿时，`Checked_In` 状态对此事件执行 `on` 动作。该状态使局部数据对象 `service` 递增并调用 MATLAB® 函数 `expenses`。此函数将客房服务请求的总次数作为输入，并将当前酒店账单以输出形式返回。有关详细信息，请参阅[通过在父状态中使用事件动作来控制图执行](https://ww2.mathworks.cn/help/stateflow/ug/event-actions-in-a-superstate-example.html)。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/xxsemantics-hotel-function-call.png)

### 具有历史结点的状态的执行

当子状态 `Checked_in` 被激活时，触发输入事件 `fire_alarm`，这对应于发出火警。您离开大楼，在大楼外的指定等候区等候。然后触发输入事件 `all_clear`，这对应于发送警报解除信号并允许您返回酒店内原先所在的位置。

当图接收到 `fire_alarm` 的事件广播时，会发生从 `Check_in` 到 `Waiting_area` 的转移。`Check_in`、`Checked_in` 和 `Executive_suite` 中的历史结点记录这些状态中每个状态的最后一个激活子状态。从最里层子状态开始，各激活状态按层次结构的升序变为非激活状态。在 `Check_in` 变为非激活后，`Waiting_area` 被激活。

当图接收到 `all_clear` 的事件广播时，会发生从 `Waiting_area` 到先前激活子状态 `Check_in` 的转移。按层次结构（从 `Check_in` 开始）的降序，`Waiting_area` 先变为非激活状态，随后 `Check_in` 的子状态被激活。

有关详细信息，请参阅 [How Stateflow Charts Respond to Events](https://ww2.mathworks.cn/help/stateflow/ug/how-events-drive-chart-execution.html#f26-1022386)、[退出状态](https://ww2.mathworks.cn/help/stateflow/ug/chart-exit-actions.html)和[进入图或状态](https://ww2.mathworks.cn/help/stateflow/ug/chart-initialization-and-entry-actions.html)。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/xxsemantics-hotel-history-junctions.png)
