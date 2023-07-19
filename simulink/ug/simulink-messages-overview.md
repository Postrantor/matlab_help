---
tip: translate by openai@2023-06-21 11:16:11
...

::: {#product_info_alert xmlns="http://www.w3.org/1999/xhtml"}
:::

::: {#doc_center_content .section xmlns="http://www.w3.org/1999/xhtml" itemprop="content" lang="en" data-language="en"}
::: {#pgtype-topic}
::: {.section itemprop="content"}

## [Simulink]{.trademark .entity} Messages Overview {#mw_b6777d63-d458-4544-9c56-e831bbca5c0f .title .r2023a itemprop="title content"}

Message-based communication is necessary in various applications, such as control system architectures in which centralized architectures are replaced with distributed architectures due to the complexity of the systems. In a distributed architecture, multiple components of the system communicate via a shared network.

> 消息通信在各种应用中是必要的，比如由于系统复杂性，集中式架构被分布式架构所取代的控制系统架构。在分布式架构中，系统的多个组件通过共享网络进行通信。

A distributed architecture has these three elements:

> 分布式架构有以下三个要素：

::: itemizedlist

- Component --- Represents partitions of a design that performs a set of functionalities or algorithms with defined I/O interfaces. Generally, components generate events and data asynchronously.

> 组件——代表设计的分区，可以执行具有定义的 I/O 接口的一组功能或算法。通常，组件会异步生成事件和数据。

- Interface --- Provides a shared boundary through which components of the system communicate. To provide asynchronous communication, messages are useful modeling artifacts that combine events with related data.

> 接口——通过它提供系统组件之间的共享边界。为了提供异步通信，消息是有用的建模工件，它将事件与相关数据结合起来。

- Middleware --- Provides the services needed by the components to support asynchronous communication across the shared network.

> 中间件---为组件提供在共享网络上支持异步通信所需的服务。
> :::

Below is an illustration that shows the composition of a distributed architecture and its elements.

> 下面是一张图表，展示了分布式架构及其元素的组成。

::: informalfigure
::: {#d124e49160 .mediaobject}
![](https://ww2.mathworks.cn/help/simulink/ug/messages_composition_figure.png)
[Message based modeling framework]{.image}
:::

When modeling such an architecture, you typically model components that are clearly identifiable, reusable, and deployable. To achieve asynchronous event-based communication between components, use message send and receive interfaces. Model the middleware to facilitate the network topology that represents the connectivity of components, such as one-to-many, many-to-one, or many-to-many based on the number of message sending and receiving components. For an example, see [Build a Shared Communication Channel with Multiple Senders and Receivers](use-messages-simevents-and-stateflow-for-message-based-communication.html){.a}.

> 当建模这种架构时，通常会建模明显可识别、可重用和可部署的组件。为了实现组件之间的异步事件驱动通信，请使用消息发送和接收接口。模型中间件以表示组件连接性的网络拓扑，例如基于消息发送和接收组件数量的一对多、多对一或多对多。有关示例，请参阅[使用消息、SimEvents 和 Stateflow 构建具有多个发送者和接收者的共享通信通道](use-messages-simevents-and-stateflow-for-message-based-communication.html){.a}。

To learn how to model a distributed architecture, using Simulink^®^, SimEvents^®^, and Stateflow^®^, see the illustration below. The illustration includes two message sending and three message receiving components that are created as referenced models. You can model components with send and receive interfaces using Simulink [Send](../slref/send.html){.a} and [Receive](../slref/receive.html){.a} blocks. If your send and receive interfaces involve states or require decision logic, use a Stateflow chart. You can also model event-driven or message triggered receive interfaces using [Message Triggered Subsystem](../slref/messagetriggeredsubsystem.html){.a}.

> 要学习如何使用 Simulink®、SimEvents® 和 Stateflow® 建模分布式架构，请参阅下图。该图包括两个发送消息和三个接收消息的组件，这些组件都是作为引用模型创建的。您可以使用 Simulink [发送](../slref/send.html){.a}和[接收](../slref/receive.html){.a}块来建模具有发送和接收接口的组件。如果您的发送和接收接口涉及状态或需要决策逻辑，请使用 Stateflow 图。您还可以使用[消息触发子系统](../slref/messagetriggeredsubsystem.html){.a}来建模基于事件或消息触发的接收接口。

::: informalfigure
::: {#d124e49194 .mediaobject}
![Message based modeling using Simulink, Stateflow, and SimEvents](https://ww2.mathworks.cn/help/simulink/ug/messages_composition_modeling_figure.png)
:::

After you model your components and interfaces:

> 在你建立你的组件和接口之后：

::: itemizedlist

- Simulate the behavior of your distributed architecture by modeling the middleware using SimEvents. Using the blocks from the SimEvents library, you can model custom routing and communication patterns, such as merging, delaying, distributing, and broadcasting messages, and investigate the effects of middleware on your communication network.

> 模拟您的分布式架构的行为，通过使用 SimEvents 模拟中间件。使用 SimEvents 库中的块，您可以模拟自定义路由和通信模式，如合并、延迟、分发和广播消息，并研究中间件对通信网络的影响。

- Generate code for your components, including the interface, and connect to your middleware or an operating system communication API.

> 生成您的组件的代码，包括接口，并连接到中间件或操作系统通信 API。
> :::

::: {.section itemprop="content"}

### Model Message Send and Receive Interfaces and Generate Code {#mw_94e2ae0b-df33-4603-901b-34d0d9f5c4ae .title}

Let us start by understanding how message blocks work. To create a model that uses messages, use [Send]{.block} blocks to convert data and send messages and [Receive]{.block} blocks to receive and convert messages to data. For a simple example that shows how [Send]{.block} and [Receive]{.block} blocks work, see [Animate and Understand Sending and Receiving Messages](visualize-animate-and-inspect-messages.html){.a}.

> 让我们从了解消息块如何工作开始。要创建一个使用消息的模型，请使用[发送]{.block}块将数据转换并发送消息，以及[接收]{.block}块以接收和转换消息为数据。要查看[发送]{.block}和[接收]{.block}块如何工作的简单示例，请参阅[动画及理解发送和接收消息](visualize-animate-and-inspect-messages.html){.a}。

Use [Send]{.block} and [Receive]{.block} blocks to model message send and receive interfaces for your components. For a simple example that shows the basics of creating send and receive interfaces, see [Establish Message Send and Receive Interfaces Between Software Components](message-based-communication-between-software-components.html){.a}. To learn how to generate code for the same model, see [Generate C++ Messages to Communicate Data Between Simulink Components](../../ecoder/ug/message-communication.html#mw_25731e9c-2717-4ea5-87da-593af7ccd16f_1){.a}[ (Embedded Coder)]{role="cross_prod"}.

> 使用[发送]{.block}和[接收]{.block}块来模拟组件之间的消息发送和接收接口。要查看有关如何创建发送和接收接口的简单示例，请参阅[在软件组件之间建立消息发送和接收接口](message-based-communication-between-software-components.html){.a}。要了解如何为同一模型生成代码，请参阅[生成 C ++消息以在 Simulink 组件之间传输数据](../../ecoder/ug/message-communication.html#mw_25731e9c-2717-4ea5-87da-593af7ccd16f_1){.a}[（嵌入式编码器）]{role="cross_prod"}。

You can further modify send and receive interfaces for custom behavior. For example, you can synchronize when a receive interface executes to when data is available. For more information, see [Connect Message Receive Interface with Simulink Functions](run-based-on-message-data-availability.html){.a}.

> 你可以进一步修改发送和接收接口以实现自定义行为。例如，你可以同步接收接口的执行时间，以便当数据可用时执行。有关更多信息，请参见[将消息接收接口与 Simulink 函数连接](run-based-on-message-data-availability.html){.a}。

After modeling, generate code for your send and receive interfaces and connect them to the middleware or an operating system communication API. For an example that generates code for a top model and allows your application to communicate in a distributed system that uses an external message protocol service (for example, DDS, ROS, SOMEIP, or POSIX messages), see [Generate C++ Messages to Communicate Data Between Simulink and an Operating System or Middleware](../../ecoder/ug/message-communication.html#mw_b3d60f69-2274-4ed5-81df-b0842cd23062){.a}[ (Embedded Coder)]{role="cross_prod"}.

> 在建模之后，为您的发送和接收接口生成代码，并将其连接到中间件或操作系统通信 API。要查看为顶级模型生成代码，以及允许您的应用程序在使用外部消息协议服务（例如 DDS、ROS、SOMEIP 或 POSIX 消息）的分布式系统中进行通信的示例，请参阅[嵌入式编码器]中的[在操作系统或中间件之间生成 C ++消息以传输数据](../../ecoder/ug/message-communication.html#mw_b3d60f69-2274-4ed5-81df-b0842cd23062)。
> :::

::: {.section itemprop="content"}

### Model Event-Driven Receive Interfaces {#mw_a9b6f8e6-00ac-4b34-84a5-3f3b8f5cbfce .title}

Use [Message Triggered Subsystem](../slref/messagetriggeredsubsystem.html){.a} block to configure your subsystem to be triggered by messages and to respond to events.

> 使用[消息触发子系统](../slref/messagetriggeredsubsystem.html){.a}块配置您的子系统，以便由消息触发并响应事件。

When you use [Message Triggered Subsystem]{.block} block in scheduled mode, the execution order of the subsystem can be scheduled as aperiodic partitions using the Schedule Editor to model an asynchronous behavior. The subsystems pull and process the data based on a schedule instead of periodic execution. For an example, see [Asynchronous Message Handling in Adaptive Cruise Control](model-asynchronous-event-handling-in-adaptive-cruise-control.html){.a}.

> 当您以计划模式使用[消息触发子系统]{.block}块时，可以使用调度编辑器将子系统的执行顺序调度为非周期分区，以模拟异步行为。子系统根据计划拉取和处理数据，而不是周期性执行。有关示例，请参见[自适应巡航控制中的异步消息处理]{.a}。
> :::

::: {.section itemprop="content"}

### Simulate Middleware Effects on a Distributed Architecture {#mw_66a12842-d367-4186-9f73-fada3d45bd61 .title}

Use [[Queue]{.block}](../slref/queue.html) blocks to store, sort and queue messages. The [Queue]{.block} block allows you to specify message storage capacity and the overwriting and sorting policies for message transitions. For a simple example that shows how a [Queue]{.block} block works, see [Use a Queue Block to Manage Messages](use-a-queue-block-to-manage-messages.html){.a}.

> 使用[[队列]{.block}](../slref/queue.html)块来存储、排序和排队消息。[队列]{.block}块允许您指定消息存储容量以及消息转换的覆盖和排序策略。要查看[队列]{.block}块如何工作的简单示例，请参阅[使用队列块管理消息](use-a-queue-block-to-manage-messages.html){.a}。

You can also use SimEvents to model and simulate middleware effects on your communication network. Use the blocks provided by the SimEvents library to model message routing, peer-to-peer communication, wireless communication, packet loss, and channel delays. For more information about SimEvents, see [Discrete-Event Simulation in Simulink Models](../../simevents/gs/discrete-event-simulation-using-simevents-in-simulink-models.html){.a}[ (SimEvents)]{role="cross_prod"}.

> 您还可以使用 SimEvents 来模拟和模拟中间件对您的通信网络的影响。使用 SimEvents 库提供的块来模拟消息路由、点对点通信、无线通信、数据包丢失和信道延迟。有关 SimEvents 的更多信息，请参见[在 Simulink 模型中使用离散事件仿真] (../../simevents/gs/discrete-event-simulation-using-simevents-in-simulink-models.html){.a}[（SimEvents）]{role="cross_prod"}。

For basic communication patterns that can be modeled by SimEvents, see [Modeling Message Communication Patterns with SimEvents](modeling-mesage-communication-patterns.html){.a}. You can use combinations of these patterns to create more complex communication behavior. For an example of a system with multiple message sending and receiving components and an ideal shared channel with delay, see [Build a Shared Communication Channel with Multiple Senders and Receivers](use-messages-simevents-and-stateflow-for-message-based-communication.html){.a}. To see a model with shared wireless channel with channel failure and packet loss, see [Model Wireless Message Communication with Packet Loss and Channel Failure](message-communication-with-packet-loss.html){.a}.

> 对于可以由 SimEvents 建模的基本通信模式，请参阅[用 SimEvents 建模消息通信模式]（modeling-mesage-communication-patterns.html）{.a}。您可以结合这些模式来创建更复杂的通信行为。要查看具有多个消息发送和接收组件以及具有延迟的理想共享通道的系统的示例，请参阅[使用消息，SimEvents 和 Stateflow 构建基于消息的通信]（use-messages-simevents-and-stateflow-for-message-based-communication.html）{.a}。要查看具有共享无线信道、信道故障和数据包丢失的模型，请参阅[使用数据包丢失和信道故障建模无线消息通信]（message-communication-with-packet-loss.html）{.a}。

To see an example that shows how to model more complex network behavior, such as an Ethernet communication network with CSMA/CD protocol, see [Model an Ethernet Communication Network with CSMA/CD Protocol](model-an-ethernet-communication-netwotk-with-csmacd-protocol.html){.a}.

> 要查看一个模拟更复杂网络行为的示例，例如使用 CSMA/CD 协议的以太网通信网络，请参阅[使用 CSMA/CD 协议模拟以太网通信网络](model-an-ethernet-communication-netwotk-with-csmacd-protocol.html){.a}。

::: {.alert .alert-info}
[]{.alert_icon .icon-alert-info-reverse}
**Note**
SimEvents blocks do not support code generation.
:::

## See Also {#d124e49315}

[[[[Sine Wave]{.block}]{itemprop="name"}](../slref/sinewave.html){itemprop="url"}]{itemscope="" itemtype="http://www.mathworks.com/help/schema/MathWorksDocPage/SeeAlso" itemprop="seealso"} \| [[[[Send]{.block}]{itemprop="name"}](../slref/send.html){itemprop="url"}]{itemscope="" itemtype="http://www.mathworks.com/help/schema/MathWorksDocPage/SeeAlso" itemprop="seealso"} \| [[[[Receive]{.block}]{itemprop="name"}](../slref/receive.html){itemprop="url"}]{itemscope="" itemtype="http://www.mathworks.com/help/schema/MathWorksDocPage/SeeAlso" itemprop="seealso"} \| [[[[Queue]{.block}]{itemprop="name"}](../slref/queue.html){itemprop="url"}]{itemscope="" itemtype="http://www.mathworks.com/help/schema/MathWorksDocPage/SeeAlso" itemprop="seealso"} \| [[[[Sequence Viewer]{.block}]{itemprop="name"}](../slref/sequenceviewer.html){itemprop="url"}]{itemscope="" itemtype="http://www.mathworks.com/help/schema/MathWorksDocPage/SeeAlso" itemprop="seealso"}

> 振荡波 | 发送 | 接收 | 队列 | 序列查看器

## Related Topics {#d124e49335}

- [Establish Message Send and Receive Interfaces Between Software Components](message-based-communication-between-software-components.html){.a}
- [Connect Message Receive Interface with Simulink Functions](run-based-on-message-data-availability.html){.a}
- [Generate C Messages to Communicate Data Between Simulink Components](../../ecoder/ug/message-communication.html#mw_25731e9c-2717-4ea5-87da-593af7ccd16f){.a}[ (Embedded Coder)]{role="cross_prod"}
- [Generate C++ Messages to Communicate Data Between Simulink Components](../../ecoder/ug/message-communication.html#mw_25731e9c-2717-4ea5-87da-593af7ccd16f_1){.a}[ (Embedded Coder)]{role="cross_prod"}
- [Generate C++ Messages to Communicate Data Between Simulink and an Operating System or Middleware](../../ecoder/ug/message-communication.html#mw_b3d60f69-2274-4ed5-81df-b0842cd23062){.a}[ (Embedded Coder)]{role="cross_prod"}
- [Model Message-Based Communication Integrated with POSIX Message Queues](../../ecoder/ug/message-communication.html#mw_1608e0b9-559e-4eae-957d-9ce72d9b9267){.a}[ (Embedded Coder)]{role="cross_prod"}
- [Discrete-Event Simulation in Simulink Models](../../simevents/gs/discrete-event-simulation-using-simevents-in-simulink-models.html){.a}[ (SimEvents)]{role="cross_prod"}
  :::
  :::

::: clearfix
:::

::: {#mw_docsurvey .feedbackblock align="center"}
:::
:::
---
