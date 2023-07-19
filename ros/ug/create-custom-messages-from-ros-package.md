---
tip: translate by openai@2023-07-07 13:39:56
url: https://www.mathworks.com/help/ros/ug/create-custom-messages-from-ros-package.html
title: Create Custom Messages from ROS Package - MATLAB & Simulink --- 从 ROS 包创建自定义消息 - MATLAB & Simulink
date: 2023-07-07 13:35:57
tag:
summary: Procedure for creating a ROS custom message in MATLAB.
---
## Create Custom Messages from ROS Package

In this example, you go through the procedure for creating ROS custom messages in MATLAB®. You must have a ROS package that contains the required `msg`, `srv`, and `action` files. The correct file contents and folder structure are described in [Custom Message Contents](https://www.mathworks.com/help/ros/ug/ros-custom-message-support.html#butuk98). This folder structure follows the standard [ROS package](https://wiki.ros.org/Packages) conventions. Therefore, if you have any existing packages, they should match this structure.

> 在这个例子中，您将学习如何在 MATLAB® 中创建 ROS 自定义消息。您必须具备包含必要的 `msg`、`srv` 和 `action` 文件的 ROS 包。正确的文件内容和文件夹结构描述在[自定义消息内容](https://www.mathworks.com/help/ros/ug/ros-custom-message-support.html#butuk98)中。此文件夹结构遵循标准的 [ROS 包](https://wiki.ros.org/Packages)约定。因此，如果您有任何现有的包，它们应该与此结构匹配。

To ensure you have the proper third-party software, see [ROS Toolbox System Requirements](https://www.mathworks.com/help/ros/gs/ros-system-requirements.html).

> 确保您具备正确的第三方软件，请参阅 [ROS Toolbox 系统要求](https://www.mathworks.com/help/ros/gs/ros-system-requirements.html)。

After ensuring that your custom message package is correct, note the folder path location. Then, call [`rosgenmsg`](https://www.mathworks.com/help/ros/ref/rosgenmsg.html) with the specified path and follow the steps output in the command window. The following example has three messages, `A`, `B`, and `C`, that have dependencies on each other. This example also illustrates that you can use a folder containing multiple messages and generate them all at the same time.

> 确保您的自定义消息包正确无误后，注意文件夹路径位置。然后，使用指定的路径调用[`rosgenmsg`]，并按照命令窗口中的步骤操作。以下示例具有三个消息 `A`，`B` 和 `C`，它们相互依赖。此示例还说明，您可以使用包含多个消息的文件夹，并同时生成它们。

To set up custom messages in MATLAB:

- Open MATLAB in a new session
- Place your custom messages in a location and note the folder path. We recommend you put all your custom message definitions in a single packages folder.

> 请将您的自定义消息放置在某个位置，并记录下文件夹路径。我们建议您将所有自定义消息定义放在一个单独的包文件夹中。

```
folderpath = 'c:\MATLAB\custom_msgs\packages';
```

- **(Optional)** If you have an existing catkin workspace (`catkin_ws`), you can specify the path to its `src` folder instead. However, this workspace might contain a large number of packages and message generation will be run for all of them.

> - **（可选）** 如果您有现有的 catkin 工作区（`catkin_ws`），您可以指定其 `src` 文件夹的路径。 但是，此工作区可能包含大量软件包，并且将为所有软件包运行消息生成。

```
folderpath = fullfile('catkin_ws','src');
```

- Specify the folder path that contains the custom message packages and call the `rosgenmsg` function to create custom messages for MATLAB.

> 指定包含自定义消息包的文件夹路径，并调用 `rosgenmsg` 函数为 MATLAB 创建自定义消息。

```
rosgenmsg('c:\MATLAB\custom_msgs')
```

- Then, follow steps from the output of `rosgenmsg`.

1. Add the given files to the MATLAB path by running `addpath` and `savepath` in the command window.

> 在命令行中运行 `addpath` 和 `savepath`，将给定的文件添加到 MATLAB 路径中。

```
addpath('C:\MATLAB\custom_msgs\packages\matlab_msg_gen_ros1\msggen')
savepath
```

2. Refresh all message class definitions, which requires clearing the workspace:

> 刷新所有消息类定义，需要清除工作区：

3. You can then use the custom messages like any other ROS messages supported in ROS Toolbox. Verify these changes by either calling `rosmsg list` and search for your message types, or use `rosmessage` to create a new message.

> 您然后可以像使用 ROS Toolbox 支持的其他 ROS 消息一样使用自定义消息。 通过调用 `rosmsg list` 并搜索您的消息类型或使用 `rosmessage` 来创建新消息来验证这些更改。

```
custommsg = rosmessage('B/Standalone')
```

```
custommsg =
ROS Standalone message with properties:
MessageType: 'B/Standalone'
IntProperty: 0
StringPropert: ''
Use showdetails to show the contents of the message
```

This final verification shows that you have performed the custom message generation process correctly. You can now send and receive these messages over a ROS network using MATLAB and Simulink®. The new custom messages can be used like normal message types. You should see them create objects specific to their message type and be displayed in your workspace.

> 最终验证表明您已正确执行自定义消息生成过程。您现在可以使用 MATLAB 和 Simulink® 在 ROS 网络上发送和接收这些消息。新的自定义消息可以像正常消息类型一样使用。您应该看到它们创建特定于其消息类型的对象，并在您的工作区中显示出来。

```
custommsg = rosmessage('B/Standalone');
custommsg2 = rosmessage('A/DependsOnB');
```

![](https://www.mathworks.com/help/ros/ug/custom_msg_success.png)

Custom messages can also be used with the ROS Simulink blocks.

![](https://www.mathworks.com/help/ros/ug/custom_msg_block.png)

## See Also

[`rosgenmsg`](https://www.mathworks.com/help/ros/ref/rosgenmsg.html) | [`ros2genmsg`](https://www.mathworks.com/help/ros/ref/ros2genmsg.html)

## Related Topics

- [Generate ROS Custom Messages in MATLAB](https://www.mathworks.com/help/ros/ref/rosgenmsg.html#mw_d6e56669-8476-4a87-8a72-576dd3b3e81e)
- [Generate ROS 2 Custom Messages in MATLAB](https://www.mathworks.com/help/ros/ref/ros2genmsg.html#mw_b5ca9791-a32b-46ae-b7b2-1da57fd4b88e)
- [ROS Toolbox System Requirements](https://www.mathworks.com/help/ros/gs/ros-system-requirements.html)
