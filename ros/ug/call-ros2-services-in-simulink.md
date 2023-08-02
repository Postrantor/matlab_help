---
tip: translate by openai@2023-07-19 10:43:03
url: https://ww2.mathworks.cn/help/ros/ug/call-ros2-services-in-simulink.html
title: Call ROS 2 Service in Simulink - MATLAB & Simulink - MathWorks 中国 --- 在 Simulink 中调用 ROS 2 服务 - MATLAB & Simulink - MathWorks 中国
date: 2023-07-19 10:39:21
tag: #ros
summary: Call a service on the ROS 2 network in Simulink using the Call Service block and receive a response.
> 在Simulink中使用Call Service块调用ROS 2网络上的服务，并接收响应。
---
## 

This example shows how to call a service on the ROS 2 network in Simulink® using the [Call Service](https://ww2.mathworks.cn/help/ros/ref/callserviceros2.html) block and receive a response.

> 这个例子展示了如何使用 [Call Service](https://ww2.mathworks.cn/help/ros/ref/callserviceros2.html) 块在 Simulink® 中调用 ROS 2 网络上的服务，并接收响应。

### Set Up ROS 2 Network and Service Server

Create a sample ROS 2 network with one node.

```matlab
node_1 = ros2node(exampleHelperCreateRandomNodeName);
```

Create a service that adds two integers using the exiisting service type `example_interfaces/AddTwoInts`. Specify the callback function to be `exampleHelperROS2SumCallback` which performs the addition of numbers in the `a` and `b` fields of the service request message.

> 创建一个使用现有服务类型 `example_interfaces/AddTwoInts` 来添加两个整数的服务。指定回调函数为 `exampleHelperROS2SumCallback`，该函数执行服务请求消息中 `a` 和 `b` 字段的加法运算。

```matlab
sumserver = ros2svcserver(node_1,"/sum","example_interfaces/AddTwoInts",@exampleHelperROS2SumCallback);
```

> [!NOTE]
> 在这里通过编写 m 文件作为 callback，提供给 ros2 进行调用。

### Call Service Server from Simulink

Open the Simulink model with the [Call Service](https://ww2.mathworks.cn/help/ros/ref/callserviceros2.html) block. Use the [Blank Message](https://ww2.mathworks.cn/help/ros/ref/blankmessageros2.html) block to output a request message with the `example_interfaces/AddTwoIntsRequest` message type. Populate the bus with two values to sum together. You can ignore warnings about converting data types.

> 打开带有[调用服务]块的 Simulink 模型。使用[空消息]块输出一个使用 `example_interfaces/AddTwoIntsRequest` 消息类型的请求消息。用两个值填充总线以进行求和。您可以忽略有关转换数据类型的警告。

![](https://ww2.mathworks.cn/help/examples/ros/win64/CallROS2ServiceInSimulinkExample_01.png)

```matlab
open_system("AddTwoIntsROS2ServiceExampleModel.slx")
```

Run the model. The service call should return `-2` from the **Resp** output port, as part of the `sum` field in the response message. An error code of `0` indicates that the service call was successful.

> 运行模型。服务调用应该从 **Resp** 输出端口返回 `-2`，作为响应消息中的 `sum` 字段的一部分。错误代码 `0` 表示服务调用成功。

```matlab
sim("AddTwoIntsROS2ServiceExampleModel.slx");
```

## 

### 

Call a service in a ROS 2 network.

The Req block input accepts a ROS 2 service request message (bus signal). The Resp block output returns the service response message.

To select from a list of services available in an active ROS 2 network, set the Source parameter to "Select from ROS network" and use the "Select..." button. You must be connected to a ROS 2 network to get a list of active services. The type of the selected service is set automatically.

To enter a custom service or specify the service name without an active ROS connection, set Source to "Specify your own". Use the Name parameter to specify the service name, and the "Select..." button to select the service type.

调用 ROS 2 网络中的服务。**Req 块输入接受一个 ROS 2 服务请求消息（总线信号）。Resp 块输出返回服务响应消息。** 要从活动的 ROS 2 网络中的服务列表中选择，将 Source 参数设置为"Select from ROS network"，并使用"Select..."按钮。您必须连接到一个 ROS 2 网络才能获取活动服务的列表。所选服务的类型会自动设置。

要输入自定义服务或在没有活动的 ROS 连接的情况下指定服务名称，将 Source 设置为"Specify your own"。使用 Name 参数来指定服务名称，使用"Select..."按钮来选 择服务类型。
