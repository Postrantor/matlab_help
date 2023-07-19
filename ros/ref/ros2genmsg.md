---
tip: translate by openai@2023-07-07 14:06:21
url: https://www.mathworks.com/help/ros/ref/ros2genmsg.html#mw_b5ca9791-a32b-46ae-b7b2-1da57fd4b88e
title: Generate custom messages from ROS 2 definitions - MATLAB ros2genmsg --- 从 ROS 2 定义生成自定义消息 - MATLAB ros2genmsg
date: 2023-07-07 13:51:28
tag:
summary: This MATLAB function generates ROS 2 custom messages by reading ROS 2 custom messages and service def......
> 这个MATLAB函数通过读取ROS 2自定义消息和服务定义来生成ROS 2自定义消息。
---
Use custom messages to extend the set of message types currently supported in ROS 2. Custom messages are messages that you define. If you are sending and receiving supported message types, you do not need to use custom messages. To see a list of supported message types, enter `ros2 msg list` in the MATLAB® Command Window. For more information about supported ROS 2 messages, see [Work with Basic ROS 2 Messages](https://www.mathworks.com/help/ros/ug/work-with-basic-ros-2-messages.html).

> 使用自定义消息来扩展 ROS 2 中当前支持的消息类型。自定义消息是您定义的消息。如果您正在发送和接收受支持的消息类型，则不需要使用自定义消息。要查看受支持的消息类型列表，请在 MATLAB® 命令窗口中输入 `ros2 msg list`。有关受支持的 ROS 2 消息的更多信息，请参阅[使用基本 ROS 2 消息](https://www.mathworks.com/help/ros/ug/work-with-basic-ros-2-messages.html)。

If this if your first time working with ROS 2 custom messages, see [ROS Toolbox System Requirements](https://www.mathworks.com/help/ros/gs/ros-system-requirements.html).

> 如果这是您第一次使用 ROS 2 自定义消息，请参阅 [ROS Toolbox 系统要求](https://www.mathworks.com/help/ros/gs/ros-system-requirements.html)。

ROS 2 custom messages are specified in ROS 2 package folders that contain a folder named `msg`. The `msg` folder contains all your custom message type definitions. For example, the `example_b_msgs` package in the `custom` folder, has this folder and file structure.

> ROS 2 自定义消息位于包含名为“msg”的文件夹的 ROS 2 包文件夹中。 “msg”文件夹包含所有自定义消息类型定义。例如，“custom”文件夹中的“example_b_msgs”包具有以下文件夹和文件结构。

![](https://www.mathworks.com/help/examples/ros/win64/ROS2CustomMessagesExample_01.png)

The package contains the custom message type `Standalone.msg`. MATLAB uses these files to generate the necessary files for using the custom messages contained in the package.

> 这个包包含自定义消息类型“Standalone.msg”。MATLAB 使用这些文件来生成使用包中包含的自定义消息所需的文件。

In this example, you create ROS 2 custom messages in MATLAB. You must have a ROS 2 package that contains the required `msg` file.

> 在这个例子中，您可以在 MATLAB 中创建 ROS 2 自定义消息。您必须拥有包含所需 `msg` 文件的 ROS 2 包。

After ensuring that your custom message package is correct, you specify the path to the parent folder and call `ros2genmsg` with the specified path. The following example provided three messages `example_package_a,` `example_package_b`, and `example_package_c` that have dependencies. This example also illustrates that you can use a folder containing multiple messages and generate them all at the same time.

> 在确保您的自定义消息包正确之后，您指定父文件夹的路径并使用指定的路径调用 `ros2genmsg`。以下示例提供了三个具有依赖关系的消息 `example_package_a`,`example_package_b` 和 `example_package_c`。此示例还说明，您可以使用包含多个消息的文件夹，并同时生成它们。

Open a new MATLAB session and create a custom message folder in a local folder.

> 在新的 MATLAB 会话中打开一个本地文件夹，并在其中创建一个自定义消息文件夹。

```
folderPath = fullfile(pwd,"custom");
copyfile("example_*_msgs",folderPath);

```

Specify the folder path for custom message files and use `ros2genmsg` to create custom messages.

> 指定自定义消息文件的文件夹路径，并使用 `ros2genmsg` 来创建自定义消息。

```
Identifying message files in folder 'C:/Work/custom'.Done.
Removing previous version of Python virtual environment.Done.
Creating a Python virtual environment.Done.
Adding required Python packages to virtual environment.Done.
Copying include folders.Done.
Copying libraries.Done.
Validating message files in folder 'C:/Work/custom'.Done.
[3/3] Generating MATLAB interfaces for custom message packages... Done.
Running colcon build in folder 'C:/Work/custom/matlab_msg_gen/win64'.
Build in progress. This may take several minutes...
Build succeeded.build log


```

Call `ros2 msg list` to verify creation of new custom messages.

```
action_msgs/CancelGoalRequest
action_msgs/CancelGoalResponse
action_msgs/GoalInfo
action_msgs/GoalStatus
action_msgs/GoalStatusArray
actionlib_msgs/GoalID
actionlib_msgs/GoalStatus
actionlib_msgs/GoalStatusArray
builtin_interfaces/Duration
builtin_interfaces/Time
composition_interfaces/ListNodesRequest
composition_interfaces/ListNodesResponse
composition_interfaces/LoadNodeRequest
composition_interfaces/LoadNodeResponse
composition_interfaces/UnloadNodeRequest
composition_interfaces/UnloadNodeResponse
diagnostic_msgs/AddDiagnosticsRequest
diagnostic_msgs/AddDiagnosticsResponse
diagnostic_msgs/DiagnosticArray
diagnostic_msgs/DiagnosticStatus
diagnostic_msgs/KeyValue
diagnostic_msgs/SelfTestRequest
diagnostic_msgs/SelfTestResponse
example_a_msgs/DependsOnB
example_b_msgs/Standalone
example_c_msgs/DependsOnB
example_interfaces/AddTwoIntsRequest
example_interfaces/AddTwoIntsResponse
example_interfaces/Bool
example_interfaces/Byte
example_interfaces/ByteMultiArray
example_interfaces/Char
example_interfaces/Empty
example_interfaces/Float32
example_interfaces/Float32MultiArray
example_interfaces/Float64
example_interfaces/Float64MultiArray
example_interfaces/Int16
example_interfaces/Int16MultiArray
example_interfaces/Int32
example_interfaces/Int32MultiArray
example_interfaces/Int64
example_interfaces/Int64MultiArray
example_interfaces/Int8
example_interfaces/Int8MultiArray
example_interfaces/MultiArrayDimension
example_interfaces/MultiArrayLayout
example_interfaces/SetBoolRequest
example_interfaces/SetBoolResponse
example_interfaces/String
example_interfaces/TriggerRequest
example_interfaces/TriggerResponse
example_interfaces/UInt16
example_interfaces/UInt16MultiArray
example_interfaces/UInt32
example_interfaces/UInt32MultiArray
example_interfaces/UInt64
example_interfaces/UInt64MultiArray
example_interfaces/UInt8
example_interfaces/UInt8MultiArray
example_interfaces/WString
geometry_msgs/Accel
geometry_msgs/AccelStamped
geometry_msgs/AccelWithCovariance
geometry_msgs/AccelWithCovarianceStamped
geometry_msgs/Inertia
geometry_msgs/InertiaStamped
geometry_msgs/Point
geometry_msgs/Point32
geometry_msgs/PointStamped
geometry_msgs/Polygon
geometry_msgs/PolygonStamped
geometry_msgs/Pose
geometry_msgs/Pose2D
geometry_msgs/PoseArray
geometry_msgs/PoseStamped
geometry_msgs/PoseWithCovariance
geometry_msgs/PoseWithCovarianceStamped
geometry_msgs/Quaternion
geometry_msgs/QuaternionStamped
geometry_msgs/Transform
geometry_msgs/TransformStamped
geometry_msgs/Twist
geometry_msgs/TwistStamped
geometry_msgs/TwistWithCovariance
geometry_msgs/TwistWithCovarianceStamped
geometry_msgs/Vector3
geometry_msgs/Vector3Stamped
geometry_msgs/Wrench
geometry_msgs/WrenchStamped
lifecycle_msgs/ChangeStateRequest
lifecycle_msgs/ChangeStateResponse
lifecycle_msgs/GetAvailableStatesRequest
lifecycle_msgs/GetAvailableStatesResponse
lifecycle_msgs/GetAvailableTransitionsRequest
lifecycle_msgs/GetAvailableTransitionsResponse
lifecycle_msgs/GetStateRequest
lifecycle_msgs/GetStateResponse
lifecycle_msgs/State
lifecycle_msgs/Transition
lifecycle_msgs/TransitionDescription
lifecycle_msgs/TransitionEvent
logging_demo/ConfigLoggerRequest
logging_demo/ConfigLoggerResponse
map_msgs/GetMapROIRequest
map_msgs/GetMapROIResponse
map_msgs/GetPointMapROIRequest
map_msgs/GetPointMapROIResponse
map_msgs/GetPointMapRequest
map_msgs/GetPointMapResponse
map_msgs/OccupancyGridUpdate
map_msgs/PointCloud2Update
map_msgs/ProjectedMap
map_msgs/ProjectedMapInfo
map_msgs/ProjectedMapsInfoRequest
map_msgs/ProjectedMapsInfoResponse
map_msgs/SaveMapRequest
map_msgs/SaveMapResponse
map_msgs/SetMapProjectionsRequest
map_msgs/SetMapProjectionsResponse
nav_msgs/GetMapRequest
nav_msgs/GetMapResponse
nav_msgs/GetPlanRequest
nav_msgs/GetPlanResponse
nav_msgs/GridCells
nav_msgs/MapMetaData
nav_msgs/OccupancyGrid
nav_msgs/Odometry
nav_msgs/Path
nav_msgs/SetMapRequest
nav_msgs/SetMapResponse
pendulum_msgs/JointCommand
pendulum_msgs/JointState
pendulum_msgs/RttestResults
rcl_interfaces/DescribeParametersRequest
rcl_interfaces/DescribeParametersResponse
rcl_interfaces/FloatingPointRange
rcl_interfaces/GetParameterTypesRequest
rcl_interfaces/GetParameterTypesResponse
rcl_interfaces/GetParametersRequest
rcl_interfaces/GetParametersResponse
rcl_interfaces/IntegerRange
rcl_interfaces/ListParametersRequest
rcl_interfaces/ListParametersResponse
rcl_interfaces/ListParametersResult
rcl_interfaces/Log
rcl_interfaces/Parameter
rcl_interfaces/ParameterDescriptor
rcl_interfaces/ParameterEvent
rcl_interfaces/ParameterEventDescriptors
rcl_interfaces/ParameterType
rcl_interfaces/ParameterValue
rcl_interfaces/SetParametersAtomicallyRequest
rcl_interfaces/SetParametersAtomicallyResponse
rcl_interfaces/SetParametersRequest
rcl_interfaces/SetParametersResponse
rcl_interfaces/SetParametersResult
rosgraph_msgs/Clock
sensor_msgs/BatteryState
sensor_msgs/CameraInfo
sensor_msgs/ChannelFloat32
sensor_msgs/CompressedImage
sensor_msgs/FluidPressure
sensor_msgs/Illuminance
sensor_msgs/Image
sensor_msgs/Imu
sensor_msgs/JointState
sensor_msgs/Joy
sensor_msgs/JoyFeedback
sensor_msgs/JoyFeedbackArray
sensor_msgs/LaserEcho
sensor_msgs/LaserScan
sensor_msgs/MagneticField
sensor_msgs/MultiDOFJointState
sensor_msgs/MultiEchoLaserScan
sensor_msgs/NavSatFix
sensor_msgs/NavSatStatus
sensor_msgs/PointCloud
sensor_msgs/PointCloud2
sensor_msgs/PointField
sensor_msgs/Range
sensor_msgs/RegionOfInterest
sensor_msgs/RelativeHumidity
sensor_msgs/SetCameraInfoRequest
sensor_msgs/SetCameraInfoResponse
sensor_msgs/Temperature
sensor_msgs/TimeReference
shape_msgs/Mesh
shape_msgs/MeshTriangle
shape_msgs/Plane
shape_msgs/SolidPrimitive
simple_msgs/AddTwoIntsRequest
simple_msgs/AddTwoIntsResponse
simple_msgs/Num
statistics_msgs/MetricsMessage
statistics_msgs/StatisticDataPoint
statistics_msgs/StatisticDataType
std_msgs/Bool
std_msgs/Byte
std_msgs/ByteMultiArray
std_msgs/Char
std_msgs/ColorRGBA
std_msgs/Empty
std_msgs/Float32
std_msgs/Float32MultiArray
std_msgs/Float64
std_msgs/Float64MultiArray
std_msgs/Header
std_msgs/Int16
std_msgs/Int16MultiArray
std_msgs/Int32
std_msgs/Int32MultiArray
std_msgs/Int64
std_msgs/Int64MultiArray
std_msgs/Int8
std_msgs/Int8MultiArray
std_msgs/MultiArrayDimension
std_msgs/MultiArrayLayout
std_msgs/String
std_msgs/UInt16
std_msgs/UInt16MultiArray
std_msgs/UInt32
std_msgs/UInt32MultiArray
std_msgs/UInt64
std_msgs/UInt64MultiArray
std_msgs/UInt8
std_msgs/UInt8MultiArray
std_srvs/EmptyRequest
std_srvs/EmptyResponse
std_srvs/SetBoolRequest
std_srvs/SetBoolResponse
std_srvs/TriggerRequest
std_srvs/TriggerResponse
stereo_msgs/DisparityImage
test_msgs/Arrays
test_msgs/ArraysRequest
test_msgs/ArraysResponse
test_msgs/BasicTypes
test_msgs/BasicTypesRequest
test_msgs/BasicTypesResponse
test_msgs/BoundedSequences
test_msgs/Builtins
test_msgs/Constants
test_msgs/Defaults
test_msgs/Empty
test_msgs/EmptyRequest
test_msgs/EmptyResponse
test_msgs/MultiNested
test_msgs/Nested
test_msgs/Strings
test_msgs/UnboundedSequences
test_msgs/WStrings
trajectory_msgs/JointTrajectory
trajectory_msgs/JointTrajectoryPoint
trajectory_msgs/MultiDOFJointTrajectory
trajectory_msgs/MultiDOFJointTrajectoryPoint
unique_identifier_msgs/UUID
visualization_msgs/GetInteractiveMarkersRequest
visualization_msgs/GetInteractiveMarkersResponse
visualization_msgs/ImageMarker
visualization_msgs/InteractiveMarker
visualization_msgs/InteractiveMarkerControl
visualization_msgs/InteractiveMarkerFeedback
visualization_msgs/InteractiveMarkerInit
visualization_msgs/InteractiveMarkerPose
visualization_msgs/InteractiveMarkerUpdate
visualization_msgs/Marker
visualization_msgs/MarkerArray
visualization_msgs/MenuEntry
```

You can now use the above created custom message as the standard messages. For more information on sending and receiving messages, see [Exchange Data with ROS 2 Publishers and Subscribers](https://www.mathworks.com/help/ros/ug/exchange-data-with-ros-2-publishers-and-subscribers.html).

> 您现在可以使用上面创建的自定义消息作为标准消息。有关发送和接收消息的更多信息，请参阅[使用 ROS 2 发布者和订阅者交换数据](https://www.mathworks.com/help/ros/ug/exchange-data-with-ros-2-publishers-and-subscribers.html)。

Create a publisher to use `example_b_msgs/Standalone` message.

```
node = ros2node("/node_1");
pub = ros2publisher(node,"/example_topic","example_b_msgs/Standalone");
```

Create a subscriber on the same topic.

```
sub = ros2subscriber(node,"/example_topic");
```

Create a message and send the message.

```
custom_msg = ros2message("example_b_msgs/Standalone");
custom_msg.int_property = uint32(12);
custom_msg.string_property='This is ROS 2 custom message example';
send(pub,custom_msg);
pause(3) % Allow a few seconds for the message to arrive
```

Use `LatestMessage` field to know the recent message received by the subscriber.

> 使用 `LatestMessage` 字段来了解订阅者收到的最新消息。

```
ans = struct with fields:
        MessageType: 'example_b_msgs/Standalone'
       int_property: 12
    string_property: 'This is ROS 2 custom message example'
```

Remove the created ROS objects.

**Replacing Definitions of Built-In Messages with Custom Definitions**

MATLAB provides a lot of built-in ROS 2 message types. You can replace the definitions of those message types with new definitions using the same custom message creation workflow detailed above. When you are replacing the definitions of a built-in message package, you must ensure that the custom message package folder contains new definitions (`.msg` files) for all the message types in the corresponding built-in message package.

> MATLAB 提供了许多内置的 ROS 2 消息类型。您可以使用上面详细描述的相同的自定义消息创建工作流，用新的定义替换这些消息类型的定义。当您替换内置消息包的定义时，您必须确保自定义消息包文件夹包含相应内置消息包中的所有消息类型的新定义（`.msg` 文件）。
