---
tip: translate by openai@2023-07-20 11:34:14
url: https://ww2.mathworks.cn/help/ros/ug/feedback-control-of-a-ros2-robot.html
title: Feedback Control of a ROS-Enabled Robot Over ROS 2
- MATLAB & Simulink
- MathWorks 中国
date: 2023-07-20 11:32:45
tag: 
summary: Use Simulink to control a simulated robot running in a Gazebo robot simulator over ROS 2 network.
> 使用Simulink控制在ROS 2网络上运行的Gazebo机器人模拟器中的模拟机器人。
---
This example shows you how to use Simulink® to control a simulated robot running in a Gazebo® robot simulator over ROS 2 network.

> 这个例子向您展示了如何使用 Simulink® 通过 ROS 2 网络控制在 Gazebo® 机器人模拟器中运行的模拟机器人。

### **Introduction**

In this example, you will run a model that implements a simple closed-loop proportional controller. The controller receives location information from a simulated robot and sends velocity commands to drive the robot to a specified location. You will adjust some parameters while the model is running and observe the effect on the simulated robot.

> 在这个例子中，你将运行一个实现简单闭环比例控制器的模型。控制器从模拟机器人接收位置信息，并发送速度指令来驱动机器人到指定位置。在模型运行时，你将调整一些参数，观察模拟机器人的反应。

The following diagram summarizes the interaction between Simulink and the robot simulator (the arrows in the diagram indicate ROS 2 message transmission). The `/odom` topic conveys location information, and the `/cmd_vel` topic conveys velocity commands.

> 以下图表总结了 Simulink 与机器人模拟器之间的交互（图中的箭头表示 ROS 2 消息传输）。 `/odom` 主题传输位置信息，而 `/cmd_vel` 主题传输速度指令。

![](https://ww2.mathworks.cn/help/examples/ros/win64/FeedbackControlOfAROS2enabledRobotExample_01.png)

### **Start a Robot Simulator and Configure Simulink**

In this task, you will start a ROS-based simulator for a differential-drive robot, start the ROS bridge configure MATLAB® connection with the robot simulator.

> 在这个任务中，您将启动基于 ROS 的差动驱动机器人模拟器，启动 ROS 桥接器，配置 MATLAB® 与机器人模拟器的连接。

![](https://ww2.mathworks.cn/help/examples/ros/win64/FeedbackControlOfAROS2enabledRobotExample_02.png)

- In the Ubuntu desktop, click the **Gazebo Empty** icon to start the empty Gazebo world.

![](https://ww2.mathworks.cn/help/examples/ros/win64/FeedbackControlOfAROS2enabledRobotExample_03.png)

![](https://ww2.mathworks.cn/help/examples/ros/win64/FeedbackControlOfAROS2enabledRobotExample_04.png)

- Click the **ROS Bridge** icon to start the ROS bridge to relay messages between Simulink ROS 2 node and Turtlebot3 ROS-enabled robot.

> 点击 **ROS Bridge** 图标启动 ROS 桥接，以在 Simulink ROS 2 节点和 Turtlebot3 ROS 启用机器人之间传递消息。

- In MATLAB on your host machine, set the proper domain ID for the ROS 2 network using the `'ROS_DOMAIN_ID'` environment variable to `25` to match the robot simulator ROS bridge settings and run `ros2 topic list` to verify that the topics from the robot simulator are visible in MATLAB.

> 在您的主机机器上的 MATLAB 中，使用 `'ROS_DOMAIN_ID'` 环境变量将 ROS 2 网络的正确域 ID 设置为 `25`，以匹配机器人模拟器 ROS 桥接设置，然后运行 `ros2 topic list` 以验证机器人模拟器中的主题是否在 MATLAB 中可见。

```
setenv('ROS_DOMAIN_ID','25')
ros2 topic list

```

```
/clock
/gazebo/link_states
/gazebo/model_states
/gazebo/performance_metrics
/imu
/joint_states
/odom
/parameter_events
/rosout
/rosout_agg
/scan
/tf


```

### **Open Existing Model**

After connecting to the ROS 2 network, open the example model.

```
open_system('robotROS2FeedbackControlExample.slx');

```

![](https://ww2.mathworks.cn/help/examples/ros/win64/FeedbackControlOfAROS2enabledRobotExample_05.png)

The model implements a proportional controller for a differential-drive mobile robot. On each time step, the algorithm orients the robot toward the desired location and drives it forward. Once the desired location is reached, the algorithm stops the robot.

> 模型为差动驱动移动机器人实现比例控制器。在每一个时间步骤中，算法会使机器人朝向期望位置并向前驱动。一旦到达期望位置，算法会停止机器人。

```
open_system('robotROS2FeedbackControlExample/Proportional Controller');

```

![](https://ww2.mathworks.cn/help/examples/ros/win64/FeedbackControlOfAROS2enabledRobotExample_06.png)

Note that there are four tunable parameters in the model (indicated by colored blocks).

> 注意，模型中有四个可调参数（由彩色块标明）。

- **Desired Position** (at top level of model): The desired location in (X,Y) coordinates
- **Distance Threshold**: The robot is stopped if it is closer than this distance from the desired location

> 距离阈值：如果机器人离所需位置更近，则停止运行。

- **Linear Velocity**: The forward linear velocity of the robot
- **Gain**: The proportional gain when correcting the robot orientation

The model also has a **Simulation Rate Control** block (at top level of model). This block ensures that the simulation update intervals follow wall-clock elapsed time.

> 模型还具有一个**模拟速率控制**块（模型的顶层）。该块确保模拟更新间隔遵循墙壁上的实际时间。

### **Configure Simulink and Run the Model**

In this task, you will configure Simulink to communicate with ROS-enabled robot simulator over ROS 2, run the model and observe the behavior of the robot in the robot simulator.

> 在这个任务中，您将配置 Simulink 以通过 ROS 2 与启用 ROS 的机器人模拟器通信，运行模型并观察机器人模拟器中机器人的行为。

To configure the network settings for ROS 2.

- Under the **Simulation** tab, in **PREPARE**, select **ROS Toolbox > ROS Network**.
- In **Configure ROS Network Addresses**, set the ROS 2 Domain ID value to _25_ and set the ROS 2 RMW Implementation as `rmw_fastrtps_cpp`.

> 在**配置 ROS 网络地址**中，将 ROS 2 域 ID 值设置为 *25*，并将 ROS 2 RMW 实现设置为 `rmw_fastrtps_cpp`。

- Click _OK_ to apply changes and close the dialog.

![](https://ww2.mathworks.cn/help/examples/ros/win64/FeedbackControlOfAROS2enabledRobotExample_07.png)

To run the model.

- Position windows on your screen so that you can observe both the Simulink model and the robot simulator.

> 将窗口位置调整到您的屏幕上，以便您可以同时观察 Simulink 模型和机器人模拟器。

- Click the Play button in Simulink to start simulation.
- While the simulation is running, double-click on the **Desired Position** block and change the Constant value to `[2 3]`. Observe that the robot changes its heading.

> 当模拟运行时，双击**期望位置**块，将常数值改为 `[2 3]`。观察机器人改变其航向。

- While the simulation is running, open the **Proportional Controller** subsystem and double-click on the **Linear Velocity (slider)** block. Move the slider to 2. Observe the increase in robot velocity.

> 当模拟运行时，打开比例控制器子系统，双击线速度（滑块）块。将滑块移动到 2。观察机器人速度的增加。

- Click the Stop button in Simulink to stop the simulation.

### **Observe Rate of Incoming Messages**

In this task, you will observe the timing and rate of incoming messages.

- Click the Play button in Simulink to start simulation.
- Open the Scope block. Observe that the **IsNew** output of the **Subscribe** block is always zero, indicating that no messages are being received for the /odom topic. The horizontal axis of the plot indicates simulation time.

> 打开 Scope 块。观察 **Subscribe** 块的 **IsNew** 输出总是为零，表明没有收到/odom 主题的消息。绘图的水平轴表示模拟时间。

- Start Gazebo Simulator in ROS network and start ROS Bridge in ROS 2, so that ROS 2 network able receive messages published by Gazebo Simulator.

> 开始 ROS 网络中的 Gazebo 模拟器，并在 ROS 2 网络中启动 ROS 桥接，以便 ROS 2 网络能够接收 Gazebo 模拟器发布的消息。

- In the Scope display, observe that the **IsNew** output has the value 1 at an approximate rate of 20 times per second, in elapsed wall-clock time.

> 在范围显示中，观察到 **IsNew** 输出在大约每秒 20 次的时间间隔内的值为 1。

![](https://ww2.mathworks.cn/help/examples/ros/win64/FeedbackControlOfAROS2enabledRobotExample_08.png)

The synchronization with wall-clock time is due to the **Simulation Rate Control** block. Typically, a Simulink simulation executes in a free-running loop whose speed depends on complexity of the model and computer speed (see [Simulation Loop Phase](https://www.mathworks.com/help/simulink/ug/simulating-dynamic-systems.html#f7-8261) (Simulink)). The **Simulation Rate Control** block attempts to regulate Simulink execution so that each update takes 0.02 seconds in wall-clock time when possible (this is equal to the fundamental sample time of the model). See the comments inside the block for more information.

> 由于模拟率控制块，与墙上时钟的同步是必要的。通常，Simulink 模拟以自由运行的循环执行，其速度取决于模型的复杂性和计算机速度（参见 [Simulation Loop Phase](https://www.mathworks.com/help/simulink/ug/simulating-dynamic-systems.html#f7-8261)（Simulink））。模拟率控制块试图调节 Simulink 执行，使每次更新在可能的情况下都要花 0.02 秒的墙上时间（这等于模型的基本采样时间）。有关更多信息，请参阅块内的注释。

In addition, the Enabled subsystems for the Proportional Controller and the Command Velocity Publisher ensure that the model only reacts to genuinely new messages. If enabled subsystems were not used, the model would repeatedly process the same (most-recently received) message over and over, leading to wasteful processing and redundant publishing of command messages.

> 此外，启用的比例控制器和命令速度发布者子系统确保模型只对真正的新消息做出反应。如果不使用启用的子系统，模型将反复处理相同的（最近收到的）消息，导致浪费处理和冗余发布命令消息。

### **Summary**

This example showed you how to use Simulink for simple closed-loop control of a simulated robot. It also showed how to use Enabled subsystems to reduce overhead in the ROS 2 network.

> 这个例子向您展示了如何使用 Simulink 来对模拟机器人进行简单的闭环控制。它还展示了如何使用启用子系统来减少 ROS 2 网络中的开销。
