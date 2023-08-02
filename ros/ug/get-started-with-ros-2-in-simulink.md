---
tip: translate by openai@2023-07-10 14:26:26
url: https://ww2.mathworks.cn/help/ros/ug/get-started-with-ros-2-in-simulink.html
title: Get Started with ROS 2 in Simulink
- MATLAB & Simulink
- MathWorks 中国
date: 2023-07-10 14:24:00
tag: 
summary: Use Simulink blocks for ROS 2 to send and receive messages from a local ROS 2 network.
> 使用Simulink块为ROS 2从本地ROS 2网络发送和接收消息。
---
This example shows how to use Simulink blocks for ROS 2 to send and receive messages from a local ROS 2 network.

> 这个例子展示了如何使用 Simulink 块与本地 ROS 2 网络进行消息发送和接收。

### Introduction

Simulink support for Robot Operating System 2 (ROS 2) enables you to create Simulink models that work with a ROS 2 network. ROS 2 is a communication layer that allows different components of a robot system to exchange information in the form of _messages_. A component sends a message by _publishing_ it to a particular _topic_, such as `/odometry`. Other components receive the message by _subscribing_ to that topic.

> Simulink 支持机器人操作系统 2(ROS 2)，可以创建与 ROS 2 网络一起工作的 Simulink 模型。 ROS 2 是一个通信层，允许机器人系统的不同组件以*消息*的形式交换信息。组件通过*发布*消息到特定的*主题*(如 `/odometry`)发送消息。其他组件通过订阅该主题来接收消息。

Simulink support for ROS 2 includes a library of Simulink blocks for sending and receiving messages for a designated topic. When you simulate the model, Simulink connects to a ROS 2 network, which can be running on the same machine as Simulink or on a remote system. Once this connection is established, Simulink exchanges messages with the ROS 2 network until the simulation is terminated. If Simulink Coder™ is installed, you can also generate C++ code for a standalone ROS 2 node, from the Simulink model.

> Simulink 支持 ROS 2，包括一个用于发送和接收指定主题消息的 Simulink 块库。当您模拟模型时，Simulink 将连接到 ROS 2 网络，该网络可以在与 Simulink 相同的计算机上运行，也可以在远程系统上运行。建立连接后，Simulink 将与 ROS 2 网络交换消息，直到模拟终止。如果安装了 Simulink Coder™，您还可以从 Simulink 模型生成用于独立 ROS 2 节点的 C++ 代码。

This example shows how to:

- Create and run a Simulink model to send and receive ROS 2 messages
- Work with data in ROS 2 messages

Prerequisites: [Create a Simple Model](https://www.mathworks.com/help/simulink/gs/create-a-simple-model.html), [Get Started with ROS 2](https://ww2.mathworks.cn/help/ros/ug/get-started-with-ros-2.html)

> 前提条件：[创建简单模型]，[开始使用 ROS 2]

### Model

You will use Simulink to publish the `X` and `Y` location of a robot. You will also subscribe to the same location topic and display the received `X`,`Y` location.

> 你将使用 Simulink 发布机器人的 X 和 Y 位置。你还将订阅同一位置主题，并显示接收的 X，Y 位置。

Enter the following command to open the completed [model](matlab:ros2GetStartedExample) created in example.

> 输入以下指令以开启示例中建立的完成[模型](matlab:ros2GetStartedExample)：

```
open_system('ros2GetStartedExample');
```

### Create a Publisher

Configure a block to send a `geometry_msgs/Point` message to a topic named `/location` (the "`/`" is standard ROS syntax).

> 配置一个块来发送一个 `geometry_msgs/Point` 消息到一个名为 `/location` 的主题("`/`"是标准 ROS 语法)。

- From the MATLAB Toolstrip, select **Home** _>_ **New** _>_ **Simulink Model** to open a new Simulink model.

> 从 MATLAB 工具条中，选择**主页**_>_**新建**_>_**Simulink 模型**以打开新的 Simulink 模型。

- From the Simulink Toolstrip, select **Simulation** _>_ **Library Browser** to open the Simulink Library Browser. Click on the **ROS Toolbox** tab (you can also type `roslib` in MATLAB command window). Select the **ROS 2** Library.

> 从 Simulink 工具栏中，选择**模拟**\_> **库浏览器**打开 Simulink 库浏览器。点击 **ROS Toolbox** 选项卡(您也可以在 MATLAB 命令窗口中输入 `roslib`)。选择 **ROS 2** 库。

- Drag a **Publish** block to the model. Double-click on the block to configure the topic and message type.

> 拖拽一个**发布**块到模型上。双击块以配置主题和消息类型。

- Select **Specify your own** for the **Topic source**, and enter `/location` in **Topic**.
- Click **Select** next to **Message type**. A pop-up window will appear. Select `geometry_msgs/Point` and click **OK** to close the pop-up window.

> 点击**消息类型**旁边的**选择**。 会出现一个弹出窗口。 选择 `geometry_msgs/Point`，然后点击**确定**关闭弹出窗口。

![](https://ww2.mathworks.cn/help/examples/ros/win64/GetStartedWithROS2InSimulinkExample_01.png)

![](https://ww2.mathworks.cn/help/examples/ros/win64/GetStartedWithROS2InSimulinkExample_02.png)

### Create a ROS 2 Message

Create a blank ROS 2 message and populate it with the x and y location for the robot path. Then publish the updated ROS 2 message to the ROS 2 network.

> 创建一个空的 ROS 2 消息，并用机器人路径的 x 和 y 位置填充它。然后将更新后的 ROS 2 消息发布到 ROS 2 网络。

A ROS 2 message is represented as a _bus signal_ in Simulink. A bus signal is a bundle of Simulink signals, and can also include other bus signals (see the [Explore Simulink Bus Capabilities](https://ww2.mathworks.cn/help/simulink/slref/simulink-bus-signals.html) (Simulink) example for an overview). The ROS 2 **Blank Message** block outputs a Simulink bus signal corresponding to a ROS 2 message.

> <font color=Red><b> **一个 ROS 2 消息在 Simulink 中被表示为总线信号**。总线信号是 Simulink 信号的一个包，也可以包含其他总线信号(参见[探索 Simulink 总线功能](Simulink)示例以获取概述)。ROS 2 **空消息**块输出一个与 ROS 2 消息对应的 Simulink 总线信号。</b></font>

- Click **ROS Toolbox** tab in the Library Browser, or type `roslib` at the MATLAB command line. Select the **ROS 2** Library.

> 点击库浏览器中的 **ROS Toolbox** 选项卡，或在 MATLAB 命令行输入 `roslib`。选择 **ROS 2** 库。

- Drag a **Blank Message** block to the model. Double-click on the block to open the block mask.
- Click on **Select** next to the **Message type** box, and select `geometry_msgs/Point` from the resulting pop-up window. set **Sample time** to 0.01. Click **OK** to close the block mask.

> 点击**消息类型**框旁边的**选择**，从弹出的窗口中选择 `geometry_msgs/Point`。将**采样时间**设置为 0.01。点击**确定**关闭块掩码。

- From the **Simulink** _>_ **Signal Routing** tab in the Library Browser, drag a **Bus Assignment** block.

> 从库浏览器中的 Simulink> 信号路由选项卡，拖拽一个总线分配块。

- Connect the output port of the **Blank Message** block to the Bus input port of the **Bus Assignment** block. Connect the output port of the **Bus Assignment** block to the input port of **Publish** block.

> 将**空白消息**块的输出端口连接到**总线分配**块的总线输入端口。将**总线分配**块的输出端口连接到**发布**块的输入端口。

- Double-click on the **Bus Assignment** block. You should see x, y and z (the signals comprising a `geometry_msgs/Point` message) listed on the left. Select `??? signal1` in the right listbox and click **Remove**. Select both `X` and `Y` signals in the left listbox and click **Select**. Click **OK** to apply changes.

> 双击**公交分配**块。您应该在左侧看到 x，y 和 z(组成 `geometry_msgs/Point` 消息的信号)。在右侧列表框中选择 `??? signal1` 并单击**删除**。在左侧列表框中选择 `X` 和 `Y` 信号，然后单击**选择**。单击**确定**以应用更改。

![](https://ww2.mathworks.cn/help/examples/ros/win64/GetStartedWithROS2InSimulinkExample_03.png)

NOTE: If you do not see x, y and z listed, close the block mask for the Bus Assignment block, and under the **Modeling** tab*,* click **Update Model** to ensure that the bus information is correctly propagated. If you see the error, "Selected signal'signal1'in the Bus Assignment block cannot be found", it indicates that the bus information has not been propagated. Close the Diagnostic Viewer, and repeat the above step.

> 注意：如果您没有看到 x，y 和 z 列出，请关闭总线分配块的块掩码，并在**建模**选项卡下，点击**更新模型**以确保总线信息正确传播。如果您看到错误“总线分配块中所选信号'signal1'未找到”，则表示总线信息尚未传播。关闭诊断视图，然后重复上述步骤。

You can now populate the bus signal with the robot location.

- From the **Simulink** _>_ **Sources** tab in the Library Browser, drag two **Sine Wave** blocks into the model.

> 从库浏览器中的 **Simulink**\_> **源**选项卡，将两个**正弦波**块拖放到模型中。

- Connect the output ports of each **Sine Wave** block to the assignment input ports x and y of the **Bus Assignment** block.

> 将每个正弦波块的输出端口连接到总线分配块的 x 和 y 分配输入端口。

- Double-click on the **Sine Wave** block that is connected to input port `X`. Set the **Phase** parameter to `-pi/2` and click **OK**. Leave the **Sine Wave** block connected to input port `Y` as default.

> 双击连接到输入端口 X 的**正弦波**块。将**相位**参数设置为 `-pi/2`，然后单击**确定**。将**正弦波**块保持连接到输入端口 Y，保持默认设置。

Your publisher should look like this:

![](https://ww2.mathworks.cn/help/examples/ros/win64/GetStartedWithROS2InSimulinkExample_04.png)

At this point, the model is set up to publish messages to the ROS 2 network. You can verify this as follows:

> 在这一点上，模型已经设置好向 ROS 2 网络发布消息。您可以通过以下方式验证：

- Under the **Simulation** tab, set the simulation stop time to `inf`.
- Click **Run** to start simulation. Simulink creates a dedicated ROS 2 node for the model and a ROS 2 publisher corresponding to the **Publish** block.

> 点击**运行**开始仿真。 Simulink 为模型创建一个专用的 ROS 2 节点，并为**发布**块创建一个 ROS 2 发布者。

- While the simulation is running, type `ros2 node` `list` in the MATLAB command window. This lists all the nodes available in the ROS network, and includes a node with a name like `/untitled_90580` (the name of the model along with a random number to make it unique).

> 当模拟运行时，在 MATLAB 命令窗口中输入 `ros2 node` `list`。这将列出 ROS 网络中所有可用的节点，其中包括一个名称类似 `/untitled_90580`(模型的名称加上一个随机数以使其唯一)的节点。

- While the simulation is running, type `ros2 topic` `list` in the MATLAB command window. This lists all the topics available in the ROS 2 network, and it includes `/location`.

> 当模拟运行时，在 MATLAB 命令窗口中输入 `ros2 topic` `list`。这将列出 ROS 2 网络中所有可用的主题，其中包括 `/location`。

![](https://ww2.mathworks.cn/help/examples/ros/win64/GetStartedWithROS2InSimulinkExample_05.png)

- Click **Stop** to stop the simulation. Simulink deletes the ROS 2 node and publisher. In general, the ROS 2 node for a model and any associated publishers and subscribers are automatically deleted at the end of a simulation; no additional clean-up steps are required.

> 点击**停止**以停止模拟。Simulink 将删除 ROS 2 节点和发布者。一般来说，模型的 ROS 2 节点及其相关发布者和订阅者会在模拟结束时自动删除，无需进行额外的清理步骤。

### Create a Subscriber

Use Simulink to receive messages sent to the `/location` topic. You will extract the x and y location from the message and plot it in the xy-plane.

> 使用 Simulink 接收发送到 `/location` 主题的消息。您将从消息中提取 x 和 y 位置，并将其绘制在 xy 平面上。

- From the **ROS Toolbox** tab in the Library Browser, drag a **Subscribe** block to the model. Double-click on the block.

> 从库浏览器中的 ROS 工具箱选项卡中，将一个订阅块拖放到模型中。双击该块。

- Select **Specify your own** in the **Topic source** box, and enter `/location` in the **Topic** box.

> 选择**指定自己的**在**主题来源**框中，并在**主题**框中输入 `/location`。

- Click **Select** next to the **Message type** box, and select `geometry_msgs/Point` from the pop-up window. set **Sample time** to 0.01. Click **OK** to close the block mask.

> 点击**消息类型**框旁边的**选择**，从弹出窗口中选择 `geometry_msgs/Point`。将**采样时间**设置为 0.01。点击**确定**关闭块掩码。

The **Subscribe** block outputs a Simulink bus signal, so you need to extract the x and y signals from it.

> 订阅块输出一个 Simulink 总线信号，因此您需要从中提取 x 和 y 信号。

- From the **Simulink** _>_ **Signal Routing** tab in the Library Browser, drag a **Bus Selector** block to the model.

> 从库浏览器中的 Simulink> 信号路由选项卡，拖拽一个总线选择器块到模型中。

- Connect the **Msg** output of the **Subscribe** block to the input port of the **Bus Selector** block.

> 将订阅块的 **Msg** 输出连接到总线选择器块的输入端口。

- From the **Modeling** tab, select **Update Model** to ensure that the bus information is propagated. You may get an error, "Selected signal'signal1'in the Bus Selector block'untitled/Bus Selector'cannot be found in the input bus signal". This error is expected, and will be resolved by the next step.

> 从**建模**选项卡中，选择**更新模型**以确保传播公共汽车信息。您可能会得到一个错误，“选定的信号'signal1'在总线选择器块'untitled/Bus Selector'中找不到输入总线信号”。这个错误是正常的，下一步将解决它。

- Double-click on the **Bus Selector** block. Select `??? signal1` and `??? signal2` in the right listbox and click **Remove**. Select both x and y signals in the left listbox and click **Select**. Click **OK**.

> 双击**总线选择器**块。在右边的列表框中选择 `??? signal1` 和 `??? signal2`，然后点击**移除**。在左边的列表框中选择 x 和 y 信号，然后点击**选择**。点击**确定**。

The **Subscribe** block will output the most-recently received message for the topic on every time step. The **IsNew** output indicates whether the message has been received during the prior time step. For the current task, the **IsNew** output is not needed, so do the following:

> 订阅块每次时间步骤都会输出该主题最近接收到的消息。**IsNew** 输出表明在上一个时间步骤中是否接收到消息。对于当前任务，**IsNew** 输出是不需要的，因此请执行以下操作：

- From the **Simulink** _>_ **Sinks** tab in the Library Browser, drag a **Terminator** block to the model.

> 从库浏览器中的 Simulink>Sinks 选项卡，将一个 Terminator 块拖拽到模型中。

- Connect the **IsNew** output of the **Subscribe** block to the input of the **Terminator** block.

> 将 **Subscribe** 块的 **IsNew** 输出连接到 **Terminator** 块的输入。

The remaining steps configure the display of the extracted `X` and `Y` signals.

> 剩余步骤配置提取的 `X` 和 `Y` 信号的显示。

- From the **Simulink** _>_ **Sinks** tab in the Library Browser, drag an **XY Graph** block to the model. Connect the output ports of the **Bus Selector** block to the input ports of the **XY Graph** block.

> 从库浏览器中的 Simulink > Sinks 选项卡，拖拽一个 XY Graph 块到模型中。将 Bus Selector 块的输出端口连接到 XY Graph 块的输入端口。

- From the **Simulink** _>_ **Sinks** tab in the Library Browser, drag two **Display** blocks to the model. Connect each output of the **Bus Selector** block to each **Display** block.

> 从库浏览器中的 Simulink> Sinks 选项卡中，拖放两个 Display 块到模型中。将 Bus Selector 块的每个输出连接到每个 Display 块。

- Save your model.

Your entire model should look like this:

![](https://ww2.mathworks.cn/help/examples/ros/win64/GetStartedWithROS2InSimulinkExample_06.png)

### Configure and Run the Model

- From the **Modeling** tab, select **Model Settings**. In the **Solver** pane, set **Type** to **Fixed-step** and **Fixed-step size** to `0.01`.

> 从**建模**选项卡中，选择**模型设置**。在**求解器**窗格中，将**类型**设置为**固定步长**，将**固定步长大小**设置为 `0.01`。

- Set simulation stop time to `10.0`.
- Click **Run** to start simulation. An `XY` plot will appear.

![](https://ww2.mathworks.cn/help/examples/ros/win64/GetStartedWithROS2InSimulinkExample_07.png)

The first time you run the model in Simulink, the `XY` plot may look more jittery than the one above due to delays caused by loading ROS libraries. Once you rerun the simulation a few times, the plot should look smoother.

> 第一次在 Simulink 中运行模型时，由于加载 ROS 库所造成的延迟，`XY` 图可能会比上面的图看起来更加抖动。重新运行几次模拟后，图形应该会变得更加平滑。

Note that the simulation **does not** work in actual or "real" time. The blocks in the model are evaluated in a loop that only simulates the progression of time, and is not intended to track actual clock time (for details, see [Simulation Loop Phase](https://ww2.mathworks.cn/help/simulink/ug/simulating-dynamic-systems.html#f7-8261) (Simulink)).

> 模拟并不是以实际时间或“实时”工作。模型中的块在循环中被评估，只模拟时间的进程，而不是跟踪实际的时钟时间(有关详情，请参见 [Simulation Loop Phase](https://ww2.mathworks.cn/help/simulink/ug/simulating-dynamic-systems.html#f7-8261) (Simulink))。

### Modify the Model to React Only to New Messages

In the above model, the **Subscribe** block outputs a message (bus signal) on every time step; if no messages have been received at all, it outputs a blank message (i.e., a message with zero values). Consequently, the `XY` coordinates are initially plotted at `(0,0)`.

> 在上述模型中，“订阅”块每一时间步都会输出一条消息(总线信号)；如果一条消息都没有收到，它会输出一条空消息(即，一条值为零的消息)。因此，XY 坐标最初绘制在(0,0)。

In this task, you will modify the model to use an **Enabled Subsystem**, so that it plots the location only when a new message is received (for more information, see [Using Enabled Subsystems](https://ww2.mathworks.cn/help/simulink/ug/enabled-subsystems.html) (Simulink)). A [pre-configured model](matlab:ros2GetStartedExample) is included for your convenience.

> 在此任务中，您将修改模型以使用**启用子系统**，以便仅在收到新消息时才绘制位置(有关更多信息，请参阅[使用启用子系统](https://ww2.mathworks.cn/help/simulink/ug/enabled-subsystems.html)(Simulink))。 为方便起见，包括一个[预配置模型](matlab:ros2GetStartedExample)。

- In the model, click and drag to select the **Bus Selector** block and **XY Graph** blocks. Right-click on the selection and select **Create Subsystem from Selection**.

> 在模型中，单击并拖动以选择**公共汽车选择器**块和** XY 图**块。 右键单击选择，然后选择**从选择创建子系统**。

- From the **Simulink** _>_ **Ports & Subsystems** tab in the Library Browser, drag an **Enable** block into the newly-created subsystem.

> 从库浏览器中的 Simulink> 端口和子系统选项卡，将一个启用块拖放到新创建的子系统中。

- Connect the **IsNew** output of the **Subscribe** block to the enabled input of the subsystem as shown in the picture below. Delete the **Terminator** block. Note that the **IsNew** output is true only if a new message was received during the previous time step.

> 将 **Subscribe** 块的 **IsNew** 输出连接到子系统的启用输入，如下图所示。删除 **Terminator** 块。请注意，**IsNew** 输出仅在上一个时间步骤中收到新消息时为真。

![](https://ww2.mathworks.cn/help/examples/ros/win64/GetStartedWithROS2InSimulinkExample_08.png)

- Save your model.
- Click **Run** to start simulation. You should see the following `XY` plot.

![](https://ww2.mathworks.cn/help/examples/ros/win64/GetStartedWithROS2InSimulinkExample_09.png)

The blocks in the enabled subsystem are only executed when a new ROS 2 message is received by the **Subscribe** block. Hence, the initial `(0,0)` value will not be displayed in the `XY` plot.

> 被启用的子系统中的模块只有在接收到 ROS 2 消息时才会被执行。因此，初始值 `(0,0)` 不会显示在 `XY` 图中。
