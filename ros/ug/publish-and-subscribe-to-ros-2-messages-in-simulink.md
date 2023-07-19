---
tip: translate by openai@2023-07-10 14:57:09
url: https://ww2.mathworks.cn/help/ros/ug/publish-and-subscribe-to-ros-2-messages-in-simulink.html?searchHighlight=bus%20assignment&s_tid=srchtitle_bus%20assignment_8
title: Publish and Subscribe to ROS 2 Messages in Simulink
  - MATLAB & Simulink
  - MathWorks 中国
date: 2023-07-10 14:26:33
tag:
summary: Publish and subscribe to a ROS 2 topic using Simulink.
---
This model shows how to publish and subscribe to a ROS 2 topic using Simulink®.

> 这个模型展示了如何使用 Simulink® 发布和订阅 ROS 2 主题。

Prerequisites: [Get Started with ROS 2 in Simulink®](https://ww2.mathworks.cn/help/ros/ug/get-started-with-ros-2-in-simulink.html)

> 前提：[使用 Simulink® 中的 ROS 2 开始](https://ww2.mathworks.cn/help/ros/ug/get-started-with-ros-2-in-simulink.html)

```
open_system('simulinkPubSubROS2Example');
```

![](https://ww2.mathworks.cn/help/examples/ros/win64/PublishAndSubscribeToROS2MessagesInSimulinkExample_01.png)

Use the Blank Message and Bus Assignment blocks to specify the x and y values of a `'geometry_msgs/Point'` message type. Open the Blank Message block mask to specify the message type. set Sample time to 0.01. Open the Bus Assignment block mask to select the signals you want to assign. Remove any values with `'???'` from the right column. Supply the Bus Assignment block with relevant values for x and y.

> 使用空白消息和总线分配块来指定 `'geometry_msgs/Point'` 消息类型的 x 和 y 值。 打开空白消息块掩码以指定消息类型。将采样时间设置为 0.01。打开总线分配块掩码以选择要分配的信号。从右侧列中删除任何值为 `'???'` 的值。为 x 和 y 提供相关值以供总线分配块使用。

Feed the `Bus` output to the Publish block. Open the block mask and choose `Specify your own` as the topic source. Specify the topic, `'/location'`, and message type, `'geoemetry_msgs/Point'`. Set Sample time to 0.01.

> 将 `Bus` 的输出馈送到发布块。打开块掩码并将“指定自己的”作为主题源。指定主题 `'/location'` 和消息类型 `'geometry_msgs/Point'`。将采样时间设置为 0.01。

Add a Subscribe block and specify the topic and message type. Feed the output `Msg` to a Bus Selector and specify the selected signals in the block mask. Display the x and y values.

> 添加一个订阅块，并指定主题和消息类型。将输出 `Msg` 馈送到总线选择器，并在块掩码中指定所选信号。显示 x 和 y 值。

Set the simulation stop time to `Inf` and run the model. You should see the `xPosition_Out` and `yPosition_Out` displays show the corresponding values published to the ROS 2 network.

> 将模拟停止时间设置为 `Inf`，然后运行模型。您应该看到 `xPosition_Out` 和 `yPosition_Out` 显示器显示发布到 ROS 2 网络的相应值。

![](https://ww2.mathworks.cn/help/examples/ros/win64/PublishAndSubscribeToROS2MessagesInSimulinkExample_02.png)
