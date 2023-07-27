---
tip: translate by openai@2023-07-20 09:40:25
url: https://ww2.mathworks.cn/help/ros/ug/exchange-data-with-ros-2-publishers-and-subscribers.html
title: Exchange Data with ROS 2 Publishers and Subscribers - MATLAB & Simulink - MathWorks 中国
date: 2023-07-20 09:05:47
tag:
summary: Publish and subscribe to topics in a ROS network.
---

This example shows how to publish and subscribe to topics in a ROS 2 network.

> 这个例子展示了如何在 ROS 2 网络中发布和订阅主题。

The primary mechanism for ROS 2 nodes to exchange data is to send and receive _messages_. Messages are transmitted on a _topic_ and each topic has a unique name in the ROS 2 network. If a node wants to share information, it must use a _publisher_ to send data to a topic. A node that wants to receive that information must use a _subscriber_ for that same topic. Besides its unique name, each topic also has a _message type_, which determines the type of messages that are allowed to be transmitted in the specific topic.

> ROS 2 节点交换数据的主要机制是发送和接收消息。消息在主题上传输，每个主题在 ROS 2 网络中都有一个唯一的名称。如果一个节点想要共享信息，它必须使用发布者将数据发送到一个主题。想要接收该信息的节点必须使用同一个主题的订阅者。除了其唯一的名称，每个主题还有一个消息类型，它决定了允许在特定主题中传输的消息类型。

This publisher-subscriber communication has the following characteristics:

> 这种发布者-订阅者通信具有以下特点：

- Topics are used for many-to-many communication. Multiple publishers can send messages to the same topic and multiple subscribers can receive them.

> 主题用于多对多通信。多个发布者可以向同一主题发送消息，多个订阅者可以接收它们。

- Publisher and subscribers are decoupled through topics and can be created and destroyed in any order. A message can be published to a topic even if there are no active subscribers.

> 发布者和订阅者通过主题解耦，可以以任何顺序创建和销毁。即使没有活动的订阅者，也可以将消息发布到主题。

![](https://ww2.mathworks.cn/help/examples/ros/win64/ExchangeDataWithROS2PublishersAndSubscribersExample_01.png)

Besides how to publish and subscribe to topics in a ROS 2 network, this example also shows how to:

> 除了如何在 ROS 2 网络中发布和订阅主题外，此示例还展示了如何：

- Wait until a new message is received, or
- Use callbacks to process new messages in the background

Prerequisites: [Get Started with ROS 2](https://ww2.mathworks.cn/help/ros/ug/get-started-with-ros-2.html), [Connect to a ROS 2 Network](https://ww2.mathworks.cn/help/ros/ug/connect-to-a-ros-2-network.html)

> 前提条件：[开始使用 ROS 2](https://ww2.mathworks.cn/help/ros/ug/get-started-with-ros-2.html)，[连接到 ROS 2 网络](https://ww2.mathworks.cn/help/ros/ug/connect-to-a-ros-2-network.html)

### Subscribe and Wait for Messages

Create a sample ROS 2 network with several publishers and subscribers.

```
exampleHelperROS2CreateSampleNetwork

```

Use `ros2 topic list` to see which topics are available.

```
/parameter_events
/pose
/rosout
/scan


```

Assume you want to subscribe to the `/scan` topic. Use `ros2subscriber` to subscribe to the `/scan` topic. Specify the name of the node with the subscriber. If the topic already exists in the ROS 2 network, `ros2subscriber` detects its message type automatically, so you do not need to specify it.

> 假设你想订阅`/scan`主题。使用`ros2subscriber`订阅`/scan`主题。指定带订阅者的节点的名称。如果该主题已经存在于 ROS 2 网络中，`ros2subscriber`会自动检测其消息类型，因此您不需要指定它。

```
detectNode = ros2node("/detection");
pause(5)
laserSub = ros2subscriber(detectNode,"/scan");
pause(5)

```

Use `receive` to wait for a new message. Specify a timeout of 10 seconds. The output `scanData` contains the received message data. `status` indicates whether a message was received successfully and `statustext` provides additional information about the `status`.

> 使用`接收`等待新消息，设置超时时间为 10 秒。输出`scanData`包含接收到的消息数据，`status`表示是否成功接收到消息，`statustext`提供有关`status`的附加信息。

```
[scanData,status,statustext] = receive(laserSub,10);

```

You can now remove the subscriber `laserSub` and the node associated to it.

> 你现在可以删除订阅者`laserSub`及其相关节点。

```
clear laserSub
clear detectNode

```

### Subscribe Using Callback Functions

Instead of using `receive` to get data, you can specify a function to be called when a new message is received. This allows other MATLAB code to execute while the subscriber is waiting for new messages. Callbacks are essential if you want to use multiple subscribers.

> 代替使用`接收`来获取数据，您可以指定一个函数，以便在收到新消息时调用该函数。这样，订阅者等待新消息时，其他 MATLAB 代码可以执行。如果要使用多个订阅者，回调是必不可少的。

Subscribe to the `/pose` topic, using the callback function `exampleHelperROS2PoseCallback`, which takes a received message as the input. One way of sharing data between your main workspace and the callback function is to use global variables. Define two global variables `pos` and `orient`.

> 订阅`/pose`主题，使用回调函数`exampleHelperROS2PoseCallback`，它将接收到的消息作为输入。在主工作空间和回调函数之间共享数据的一种方法是使用全局变量。定义两个全局变量`pos`和`orient`。

```
controlNode = ros2node("/base_station");
pause(5)
poseSub = ros2subscriber(controlNode,"/pose",@exampleHelperROS2PoseCallback);
global pos
global orient

```

The global variables `pos` and `orient` are assigned in the `exampleHelperROS2PoseCallback` function when new message data is received on the `/pose` topic.

> 全局变量`pos`和`orient`在接收到`/pose`主题的新消息数据时，会在`exampleHelperROS2PoseCallback`函数中被赋值。

```
function exampleHelperROS2PoseCallback(message)
    % Declare global variables to store position and orientation
    global pos
    global orient

    % Extract position and orientation from the ROS message and assign the
    % data to the global variables.
    pos = [message.linear.x message.linear.y message.linear.z];
    orient = [message.angular.x message.angular.y message.angular.z];
end


```

Wait a moment for the network to publish another `/pose` message. Display the updated values.

> 等待网络发布另一个'/pose'消息一会儿。 显示更新后的值。

```
       0.00235920447111606       -0.0201184589892978        0.0203969078651195


```

```
       -0.0118389124011118       0.00676849978014866        0.0387860955311228


```

If you type in `pos` and `orient` a few times in the command line you can see that the values are continuously updated.

> 如果你在命令行中多次输入`pos`和`orient`，你会发现这些值会不断更新。

Stop the pose subscriber by clearing the subscriber variable

```
clear poseSub
clear controlNode

```

_Note_: There are other ways to extract information from callback functions besides using globals. For example, you can pass a handle object as additional argument to the callback function. See the [Create Callbacks for Graphics Objects](https://ww2.mathworks.cn/help/matlab/creating_plots/create-callbacks-for-graphics-objects.html) documentation for more information about defining callback functions.

> _注意_：除了使用全局变量之外，还有其他从回调函数中提取信息的方法。例如，您可以将句柄对象作为附加参数传递给回调函数。有关定义回调函数的更多信息，请参阅[为图形对象创建回调](https://ww2.mathworks.cn/help/matlab/creating_plots/create-callbacks-for-graphics-objects.html)文档。

### Publish Messages

Create a publisher that sends ROS 2 string messages to the `/chatter` topic.

> 创建一个发布者，将 ROS 2 字符串消息发送到`/chatter`主题。

```
chatterPub = ros2publisher(node_1,"/chatter","std_msgs/String");

```

Create and populate a ROS 2 message to send to the `/chatter` topic.

```
chatterMsg = ros2message(chatterPub);
chatterMsg.data = 'hello world';

```

Use `ros2 topic list` to verify that the `/chatter` topic is available in the ROS 2 network.

> 使用`ros2 topic list`来验证 ROS 2 网络中是否有`/chatter`主题。

```
/chatter
/parameter_events
/pose
/rosout
/scan


```

Define a subscriber for the `/chatter` topic. `exampleHelperROS2ChatterCallback` is called when a new message is received, and displays the string content in the message.

> 定义一个订阅者用于`/chatter`主题。当收到新消息时，将调用`exampleHelperROS2ChatterCallback`，并显示消息中的字符串内容。

```
chatterSub = ros2subscriber(node_2,"/chatter",@exampleHelperROS2ChatterCallback)

```

```
chatterSub =
  ros2subscriber with properties:

        TopicName: '/chatter'
    LatestMessage: []
      MessageType: 'std_msgs/String'
    NewMessageFcn: @exampleHelperROS2ChatterCallback
          History: 'keeplast'
            Depth: 10
      Reliability: 'reliable'
       Durability: 'volatile'



```

Publish a message to the `/chatter` topic. Observe that the string is displayed by the subscriber callback.

> 发布一条消息到`/chatter`主题。观察订阅者回调显示的字符串。

```
send(chatterPub,chatterMsg)
pause(3)

```

The `exampleHelperROS2ChatterCallback` function was called when the subscriber received the string message.

> 当订阅者收到字符串消息时，将调用`exampleHelperROS2ChatterCallback`函数。

### Disconnect From ROS 2 Network

Remove the sample nodes, publishers and subscribers from the ROS 2 network. Also clear the global variables `pos` and `orient`

> 从 ROS 2 网络中移除样本节点、发布者和订阅者。同时清除全局变量`pos`和`orient`。

```
clear global pos orient
clear

```

### Next Steps
