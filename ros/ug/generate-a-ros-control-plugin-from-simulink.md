---
url: https://www.mathworks.com/help/ros/ug/generate-a-ros-control-plugin-from-simulink.html
title: Generate a ROS Control Plugin from Simulink - MATLAB & Simulink --- 从 Simulink 生成 ROS 控制插件 - MATLAB & Simulink
date: 2023-07-07 15:03:18
tag:
summary: Generate and build a ros_control plugin from a Simulink model.
---
> [!NOTE]
> .[pdf: ros_control](./pdf/10.21105.joss.00456.pdf)

This example shows how to generate and build a ros_control plugin from a Simulink model. In this example, you first configure a model to generate C++ code for a ros_control package. You then deploy the plugin on a virtual machine running Gazebo® to control a Pioneer 3DX 2-wheeled differential drive robot.

> 此示例演示如何从 Simulink 模型生成和构建 ros_control 插件。在此示例中，您**首先配置一个模型来为 ros_control 包生成 C++ 代码**。然后，您可以在运行 Gazebo® 的虚拟机上部署该插件，以控制 Pioneer 3DX 2 轮差动驱动机器人。

### Prerequisites 先决条件

### Overview 概述

The [ros_control](http://wiki.ros.org/ros_control) framework provides a generic set of tools to create controllers for various types of robots.

> ros_control 框架提供了一组通用工具来为各种类型的机器人创建控制器。

This example focuses on auto-generating C++ code for a controller designed in Simulink® and packaging it and deploying that controller as a ros_control plugin as shown in the figure below. In this example, you generate code for a controller designed in Simulink, and deploy it as a ros_control plugin. The data flow of the controller is shown in the following diagram.

> 此示例重点关注为 Simulink® 中设计的控制器**自动生成 C++ 代码**，并将其打包并将该控制器部署为 ros_control 插件，如下图所示。在此示例中，您为在 Simulink 中设计的控制器生成代码，并将其部署为 ros_control 插件。控制器的数据流如下图所示。

![](https://www.mathworks.com/help/examples/ros/win64/GenerateAROSControlPluginFromSimulinkExample_01.png)

The Simulink model [drivecontroller](matlab:open('./drivecontroller.slx')) implements a simple closed-loop proportional controller. It receives the goal location from `/command` topic of type, `geometry_msgs/Pose2D`, and the current position of the robot from `/p3dx/ground_truth/state` topic of type, `nav_msgs/Odometry`. The controller sets the left and right wheel speed commands using `VelocityJointInterface`. This topology is shown in the diagram below.

> Simulink 模型驱动控制器实现了一个简单的闭环比例控制器。它从 `/command` 类型为 `geometry_msgs/Pose2D` 的主题接收目标位置，并从 `/p3dx/ground_truth/state` 类型为 `nav_msgs/Odometry` 的主题接收机器人的当前位置 > . 控制器使用 `VelocityJointInterface` 设置左右轮速度命令。该拓扑如下图所示。

![](https://www.mathworks.com/help/examples/ros/win64/GenerateAROSControlPluginFromSimulinkExample_02.png)

### Open Controller Simulink Model

After connecting to the ROS network, open the [example controller model](matlab:drivecontroller).

> 连接到 ROS 网络后，打开示例控制器模型。

```
open_system('drivecontroller.slx');
```

![](https://www.mathworks.com/help/examples/ros/win64/GenerateAROSControlPluginFromSimulinkExample_03.png)

The model implements a proportional controller. At each step, the algorithm orients the robot toward the desired location and drives it forward. Once the desired location is reached, the algorithm stops the robot.

> 该模型实现了比例控制器。在每一步中，算法都会将机器人定向到所需位置并驱动其前进。一旦到达所需位置，算法就会停止机器人。

### Start ROS Core 启动 ROS 核心

- In MATLAB®, change the current folder to a temporary location where you have write permission.
  > 在 MATLAB® 中，将当前文件夹更改为您具有写入权限的临时位置。
  >
- ros_control interface setting prepares the model for simulation to ensure that all blocks are properly initialized. This requires a valid connection to a ROS core.
  > ros_control 接口设置准备模型进行仿真，以确保所有模块都正确初始化。这需要与 ROS 核心的有效连接。
  >
- Start the ROS core on the virtual machine using the `rosdevice` object
  > 使用 `rosdevice` 对象在虚拟机上启动 ROS 核心
  >

```
d = rosdevice;
runCore(d);
```

Another roscore / ROS master is already running on the ROS device. Use the 'stopCore' function to stop it.

> [!NOTE]
> 怎么还有 master？
> 还是 ros1 中的概念

- Use [`rosinit`](https://www.mathworks.com/help/ros/ref/rosinit.html) to connect MATLAB to the ROS master running on the ROS device:
  > 使用 `rosinit` 将 MATLAB 连接到 ROS 设备上运行的 ROS master：
  >

```
rosinit(d.DeviceAddress,11311)
```

Initializing global node /matlab_global_node_69673 with NodeURI [http://192.168.93.1/](http://192.168.93.1:49973/) and MasterURI [http://192.168.93.144](http://192.168.93.144:11311.)

### Configure ROS Control Interfaces

The [example controller model](matlab:drivecontroller) is configured for Robot Operating System (ROS). Use the ros_control Settings button to map the output ports of the model to correct resource type.

> 示例控制器模型是针对机器人操作系统 (ROS) 配置的。使用 ros_control 设置按钮将模型的输出端口映射到正确的资源类型。

In **ROS** tab, under **PREPARE** section, click on **ROS Control Settings** button from the **ROS Toolbox** category.

> 在 ROS 选项卡的 PREPARE 部分下，单击 ROS Toolbox 类别中的 ROS Control Settings 按钮。

In the **Configure ROS Control** dialog:

> 在配置 ROS 控制对话框中：

1. Set the name of the **Controller C++ class** to `ControllerHost`
   > 将控制器 C++ 类的名称设置为 `ControllerHost`
   >
2. In the **Outports** tab, set the **Resource name** of the output port 1, to `base_right_wheel_joint` and set the name of the output port 2, to `base_left_wheel_joint`.
   > 在 “输出端口” 选项卡中，将输出端口 1 的资源名称设置为 `base_right_wheel_joint` ，并将输出端口 2 的名称设置为 `base_left_wheel_joint` 。
   >
3. Set the **Resource type** for both output ports to `VelocityJointInterface`.
   > 将两个输出端口的资源类型设置为 `VelocityJointInterface` 。
   >
4. From the **Parameters** tab in the dialog, enable **`Gain (Slider)`**, **`Linear Velocity (Slider)`** and **`Distance Threshold`** for dynamic reconfigure.
   > 从对话框的 “参数” 选项卡中，启用 `Gain (Slider)` 、 `Linear Velocity (Slider)` 和 `Distance Threshold` 进行动态重新配置。
   >
5. Select **Generate ros_control controller package** checkbox.
   > 选择生成 ros_control 控制器包复选框。
   >
6. Click **OK** to save the settings and close the dialog.
   > 单击 “确定” 保存设置并关闭对话框。
   >

The dialog settings are shown below for reference.

> 对话框设置如下所示，以供参考。

![](https://www.mathworks.com/help/examples/ros/win64/GenerateAROSControlPluginFromSimulinkExample_04.png)
![](https://www.mathworks.com/help/examples/ros/win64/GenerateAROSControlPluginFromSimulinkExample_05.png)

### Configure Connection to the ROS Device

1. Under the **ROS** tab, from the **Deploy to** drop-down, click **Manage Remote Device**.
   > 在 ROS 选项卡下，从部署到下拉列表中，单击管理远程设备。
   >
2. In the **Connect to a ROS device** dialog that opens, enter all the information that Simulink needs to deploy the ROS node.
   > 在打开的 “连接到 ROS 设备” 对话框中，输入 Simulink 部署 ROS 节点所需的所有信息。
   >
3. Specify IP address or host name of your ROS device, login credentials, and the Catkin workspace.
   > 指定 ROS 设备的 IP 地址或主机名、登录凭据和 Catkin 工作区。
   >
4. Change **Catkin workspace** to `~/Documents/mw_catkin_ws`. This workspace has a Pioneer 3DX Gazebo® world and related files to run a simulated differential drive robot.
   > 将 Catkin 工作区更改为 `~/Documents/mw_catkin_ws` 。该工作区有一个 Pioneer 3DX Gazebo® 世界和相关文件，用于运行模拟差动驱动机器人。
   >

![](https://www.mathworks.com/help/examples/ros/win64/GenerateAROSControlPluginFromSimulinkExample_06.png)

### Deploy the ROS Control Plugin(部署 ROS 控制插件)

Generate C++ code for the proportional controller and create a ros_control plugin package, automatically transfer, and build it on the ROS device.

> **生成比例控制器的 C++ 代码并创建 ros_control 插件包**，自动传输并在 ROS 设备上构建。

#### Deploy the ROS Control Plugin

- Configure Simulink to use the same ROS connection settings as MATLAB. Under the **Simulation** tab, in **Prepare** section, select **ROS Network**. Set the ROS Master (ROS 1) and Node Host network addresses to **Default**.
  > 配置 Simulink 以使用与 MATLAB 相同的 ROS 连接设置。在 “模拟” 选项卡下的 “准备” 部分中，选择“ROS 网络”。将 ROS Master (ROS 1) 和节点主机网络地址设置为默认值。
  >
- Under the **ROS** tab, click **Deploy > Build & Load**. If you get any errors about bus type mismatch, close the model, clear all variables from the base MATLAB workspace, and re-open the model.
  > 在 ROS 选项卡下，单击部署 > 构建和加载。如果您收到有关总线类型不匹配的任何错误，请关闭模型，清除基础 MATLAB 工作区中的所有变量，然后重新打开模型。
  >
- Click on the **View Diagnostics** link at the bottom of the model toolbar to see the output of the build process.
  > 单击模型工具栏底部的 “查看诊断” 链接可查看构建过程的输出。
  >
- Once the code generation completes, the ROS package is transferred to the Catkin workspace on the ROS device and built there.
  > 代码生成完成后，ROS 包将传输到 ROS 设备上的 Catkin 工作区并在那里构建。
  >

### Integrate Simulink ROS Control Plugin with Gazebo

#### Register Controller and Build Workspace

Modify the Pioneer 3DX controller configuration YAML file to register the generated controller, and build the workspace.

- Replace the controller configuration YAML file using the `rosdevice` object. To use the controller plugin generated from the model, copy the pre-configured [`pioneer3dx.yaml`](matlab:open('./pioneer3dx.yaml')) file to the catkin workspace.

```
system(d,['cd ',d.CatkinWorkspace,' && cp src/pioneer_p3dx_model/p3dx_control/config/pioneer3dx.yaml src/pioneer_p3dx_model/p3dx_control/config/backup.pioneer3dx.yaml'])

```

```
ans =

  0×0 empty char array


```

```
putFile(d,fullfile(pwd,'pioneer3dx.yaml'),[d.CatkinWorkspace,'/src/pioneer_p3dx_model/p3dx_control/config/pioneer3dx.yaml'])

```

```
system(d,['cd ',d.CatkinWorkspace,' && source ./devel/setup.bash && catkin_make'])

```

```
ans =
    '[  0%] Built target sensor_msgs_generate_messages_eus
     [  1%] Generating dynamic reconfigure files from cfg/controller_params.cfg: /home/user/Documents/mw_catkin_ws/devel/include/drivecontroller/controller_paramsConfig.h /home/user/Documents/mw_catkin_ws/devel/lib/python3/dist-packages/drivecontroller/cfg/controller_paramsConfig.py
     [  1%] Built target std_msgs_generate_messages_nodejs
     [  1%] Built target std_msgs_generate_messages_py
     [  1%] Built target roscpp_generate_messages_cpp
     [  1%] Built target rosgraph_msgs_generate_messages_nodejs
     [  1%] Built target roscpp_generate_messages_eus
     Generating reconfiguration files for ControllerHost in drivecontroller
     Wrote header file in /home/user/Documents/mw_catkin_ws/devel/include/drivecontroller/ControllerHostConfig.h
     [  1%] Built target std_msgs_generate_messages_lisp
     [  1%] Built target roscpp_generate_messages_py
     [  1%] Built target std_msgs_generate_messages_eus
     [  1%] Built target roscpp_generate_messages_nodejs
     [  1%] Built target drivecontroller_gencfg
     [  1%] Built target std_msgs_generate_messages_cpp
     [  1%] Built target rosgraph_msgs_generate_messages_eus
     [  1%] Built target rosgraph_msgs_generate_messages_lisp
     [  1%] Built target roscpp_generate_messages_lisp
     [  1%] Built target rosgraph_msgs_generate_messages_py
     [  1%] Built target tf2_msgs_generate_messages_nodejs
     [  1%] Built target tf2_msgs_generate_messages_lisp
     [  1%] Built target tf2_msgs_generate_messages_eus
     [  1%] Built target actionlib_generate_messages_nodejs
     [  1%] Built target actionlib_generate_messages_lisp
     [  1%] Built target sensor_msgs_generate_messages_nodejs
     [  1%] Built target geometry_msgs_generate_messages_nodejs
     [  1%] Built target tf2_msgs_generate_messages_py
     [  1%] Built target geometry_msgs_generate_messages_cpp
     [  1%] Built target dynamic_reconfigure_generate_messages_cpp
     [  1%] Built target tf_generate_messages_lisp
     [  1%] Built target nav_msgs_generate_messages_cpp
     [  1%] Built target sensor_msgs_generate_messages_lisp
     [  1%] Built target nav_msgs_generate_messages_lisp
     [  1%] Built target geometry_msgs_generate_messages_eus
     [  1%] Built target dynamic_reconfigure_generate_messages_eus
     [  1%] Built target dynamic_reconfigure_gencfg
     [  1%] Built target dynamic_reconfigure_generate_messages_lisp
     [  1%] Built target actionlib_msgs_generate_messages_lisp
     [  1%] Built target dynamic_reconfigure_generate_messages_py
     [  1%] Built target nav_msgs_generate_messages_eus
     [  1%] Built target nav_msgs_generate_messages_nodejs
     [  1%] Built target actionlib_msgs_generate_messages_eus
     [  1%] Built target sensor_msgs_generate_messages_py
     [  1%] Built target geometry_msgs_generate_messages_py
     [  1%] Built target nav_msgs_generate_messages_py
     [  1%] Built target actionlib_generate_messages_eus
     [  1%] Built target actionlib_msgs_generate_messages_cpp
     [  1%] Built target rosgraph_msgs_generate_messages_cpp
     [  1%] Built target actionlib_generate_messages_py
     [  1%] Built target actionlib_msgs_generate_messages_nodejs
     [  1%] Built target geometry_msgs_generate_messages_lisp
     [  1%] Built target actionlib_generate_messages_cpp
     [  1%] Built target actionlib_msgs_generate_messages_py
     [  1%] Built target tf2_msgs_generate_messages_cpp
     [  1%] Built target tf_generate_messages_eus
     [  1%] Built target tf_generate_messages_py
     [  1%] Built target tf_generate_messages_nodejs
     [  1%] Built target dynamic_reconfigure_generate_messages_nodejs
     [  1%] Built target sensor_msgs_generate_messages_cpp
     [  2%] Generating dynamic reconfigure files from cfg/controller_params.cfg: /home/user/Documents/mw_catkin_ws/devel/include/drvctrl_pub/controller_paramsConfig.h /home/user/Documents/mw_catkin_ws/devel/lib/python3/dist-packages/drvctrl_pub/cfg/controller_paramsConfig.py
     [  3%] Generating dynamic reconfigure files from cfg/controller_params.cfg: /home/user/Documents/mw_catkin_ws/devel/include/shape_tracer/controller_paramsConfig.h /home/user/Documents/mw_catkin_ws/devel/lib/python3/dist-packages/shape_tracer/cfg/controller_paramsConfig.py
     Generating reconfiguration files for ControllerHost in drvctrl_pub
     Wrote header file in /home/user/Documents/mw_catkin_ws/devel/include/drvctrl_pub/ControllerHostConfig.h
     [  3%] Built target drvctrl_pub_gencfg
     Generating reconfiguration files for ControllerHost in shape_tracer
     Wrote header file in /home/user/Documents/mw_catkin_ws/devel/include/shape_tracer/ControllerHostConfig.h
     [  3%] Built target _turtlebot_actions_generate_messages_check_deps_TurtlebotMoveActionFeedback
     [  3%] Built target shape_tracer_gencfg
     [  3%] Built target tf_generate_messages_cpp
     [  3%] Built target _turtlebot_actions_generate_messages_check_deps_TurtlebotMoveGoal
     [  3%] Built target _turtlebot_actions_generate_messages_check_deps_FindFiducialFeedback
     [  3%] Built target _turtlebot_actions_generate_messages_check_deps_FindFiducialActionResult
     [  3%] Built target _turtlebot_actions_generate_messages_check_deps_FindFiducialGoal
     [  3%] Built target _turtlebot_actions_generate_messages_check_deps_TurtlebotMoveAction
     [  3%] Built target _turtlebot_actions_generate_messages_check_deps_FindFiducialAction
     [  3%] Built target _turtlebot_actions_generate_messages_check_deps_TurtlebotMoveActionResult
     [  3%] Built target _turtlebot_actions_generate_messages_check_deps_TurtlebotMoveActionGoal
     [  3%] Built target _turtlebot_actions_generate_messages_check_deps_FindFiducialActionGoal
     [  3%] Built target _turtlebot_actions_generate_messages_check_deps_TurtlebotMoveResult
     [  3%] Built target _turtlebot_actions_generate_messages_check_deps_TurtlebotMoveFeedback
     [  3%] Built target _turtlebot_actions_generate_messages_check_deps_FindFiducialActionFeedback
     Scanning dependencies of target drvctrl_pub
     Scanning dependencies of target drivecontroller
     [  4%] Building CXX object drvctrl_pub/CMakeFiles/drvctrl_pub.dir/src/drvctrl_pub_ctrlr_host.cpp.o
     Scanning dependencies of target shape_tracer
     [  5%] Building CXX object drivecontroller/CMakeFiles/drivecontroller.dir/src/drivecontroller_ctrlr_host.cpp.o
     [  5%] Built target _turtlebot_actions_generate_messages_check_deps_FindFiducialResult
     [  6%] Building CXX object shape_tracer/CMakeFiles/shape_tracer.dir/src/shape_tracer_ctrlr_host.cpp.o
     [ 19%] Built target turtlebot_actions_generate_messages_lisp
     [ 31%] Built target turtlebot_actions_generate_messages_nodejs
     [ 45%] Built target turtlebot_actions_generate_messages_py
     [ 58%] Built target turtlebot_actions_generate_messages_cpp
     [ 71%] Built target turtlebot_actions_generate_messages_eus
     [ 71%] Built target turtlebot_actions_gencpp
     [ 71%] Built target turtlebot_actions_generate_messages
     [ 73%] Built target turtlebot_move_action_server
     [ 76%] Built target find_fiducial_pose
     [ 77%] Linking CXX shared library /home/user/Documents/mw_catkin_ws/devel/lib/libdrvctrl_pub.so
     /usr/bin/c++ -fPIC -O3 -DNDEBUG  -shared -Wl,-soname,libdrvctrl_pub.so -o /home/user/Documents/mw_catkin_ws/devel/lib/libdrvctrl_pub.so CMakeFiles/drvctrl_pub.dir/src/drvctrl_pub.cpp.o CMakeFiles/drvctrl_pub.dir/src/drvctrl_pub_ctrlr_host.cpp.o CMakeFiles/drvctrl_pub.dir/src/drvctrl_pub_data.cpp.o CMakeFiles/drvctrl_pub.dir/src/rtGetInf.cpp.o CMakeFiles/drvctrl_pub.dir/src/rtGetNaN.cpp.o CMakeFiles/drvctrl_pub.dir/src/rt_nonfinite.cpp.o CMakeFiles/drvctrl_pub.dir/src/slros_busmsg_conversion.cpp.o CMakeFiles/drvctrl_pub.dir/src/slros_initialize.cpp.o CMakeFiles/drvctrl_pub.dir/src/slros_generic_param.cpp.o  -Wl,-rpath,/home/user/Documents/mw_catkin_ws/install/lib
     [ 78%] Linking CXX shared library /home/user/Documents/mw_catkin_ws/devel/lib/libdrivecontroller.so
     /usr/bin/c++ -fPIC -O3 -DNDEBUG  -shared -Wl,-soname,libdrivecontroller.so -o /home/user/Documents/mw_catkin_ws/devel/lib/libdrivecontroller.so CMakeFiles/drivecontroller.dir/src/drivecontroller.cpp.o CMakeFiles/drivecontroller.dir/src/drivecontroller_ctrlr_host.cpp.o CMakeFiles/drivecontroller.dir/src/drivecontroller_data.cpp.o CMakeFiles/drivecontroller.dir/src/rtGetInf.cpp.o CMakeFiles/drivecontroller.dir/src/rtGetNaN.cpp.o CMakeFiles/drivecontroller.dir/src/rt_nonfinite.cpp.o CMakeFiles/drivecontroller.dir/src/slros_busmsg_conversion.cpp.o CMakeFiles/drivecontroller.dir/src/slros_initialize.cpp.o CMakeFiles/drivecontroller.dir/src/slros_generic_param.cpp.o  -Wl,-rpath,/home/user/Documents/mw_catkin_ws/install/lib
     [ 85%] Built target drvctrl_pub
     [ 91%] Built target drivecontroller
     [ 92%] Linking CXX shared library /home/user/Documents/mw_catkin_ws/devel/lib/libshape_tracer.so
     /usr/bin/c++ -fPIC -O3 -DNDEBUG  -shared -Wl,-soname,libshape_tracer.so -o /home/user/Documents/mw_catkin_ws/devel/lib/libshape_tracer.so CMakeFiles/shape_tracer.dir/src/rtGetInf.cpp.o CMakeFiles/shape_tracer.dir/src/rtGetNaN.cpp.o CMakeFiles/shape_tracer.dir/src/rt_nonfinite.cpp.o CMakeFiles/shape_tracer.dir/src/shape_tracer.cpp.o CMakeFiles/shape_tracer.dir/src/shape_tracer_ctrlr_host.cpp.o CMakeFiles/shape_tracer.dir/src/shape_tracer_data.cpp.o CMakeFiles/shape_tracer.dir/src/slros_busmsg_conversion.cpp.o CMakeFiles/shape_tracer.dir/src/slros_initialize.cpp.o CMakeFiles/shape_tracer.dir/src/slros_generic_param.cpp.o  -Wl,-rpath,/home/user/Documents/mw_catkin_ws/install/lib
     [100%] Built target shape_tracer
     Base path: /home/user/Documents/mw_catkin_ws
     Source space: /home/user/Documents/mw_catkin_ws/src
     Build space: /home/user/Documents/mw_catkin_ws/build
     Devel space: /home/user/Documents/mw_catkin_ws/devel
     Install space: /home/user/Documents/mw_catkin_ws/install
     ####
     #### Running command: "make cmake_check_build_system" in "/home/user/Documents/mw_catkin_ws/build"
     ####
     ####
     #### Running command: "make -j4 -l4" in "/home/user/Documents/mw_catkin_ws/build"
     ####
     '



```

#### Start Pioneer 3DX Gazebo World

- In the Ubuntu VM, click on **Gazebo Pioneer 3DX** desktop icon to launch the Gazebo world.
- Use `rossvcclient` to load the controller, `sl_drivecontroller`

```
allServices = rosservice('list');
controllerName = 'sl_drivecontroller';
loadSvcName = allServices(endsWith(allServices,'/load_controller'));
switchCtrlSvcName = allServices(endsWith(allServices,'/switch_controller'));
% Load controller
loadCtrlSvc = rossvcclient(loadSvcName{1},'DataFormat','struct','Timeout',10);
loadCtrlMsg = rosmessage(loadCtrlSvc);
loadCtrlMsg.Name = controllerName;
call(loadCtrlSvc,loadCtrlMsg)

```

```
ans = struct with fields:
    MessageType: 'controller_manager_msgs/LoadControllerResponse'
             Ok: 1



```

```
% Start controller
switchCtrlSvc = rossvcclient(switchCtrlSvcName{1},'DataFormat','struct','Timeout',10);
swtCtrlMsg = rosmessage(switchCtrlSvc);
swtCtrlMsg.StartControllers = {controllerName};
swtCtrlMsg.Strictness = swtCtrlMsg.BESTEFFORT;
call(switchCtrlSvc,swtCtrlMsg)

```

```
ans = struct with fields:
    MessageType: 'controller_manager_msgs/SwitchControllerResponse'
             Ok: 1



```

- Alternatively, you can also use the `rqt_controller_manager` application as shown below:

![](https://www.mathworks.com/help/examples/ros/win64/GenerateAROSControlPluginFromSimulinkExample_07.png)

To change the display name of the controller, `sl_drivecontroller`, in `rqt_controller_manager` UI,

### **Send Commands to Move the Robot**

To move the Pioneer 3DX robot, open the [set_command](matlab:open('./set_command.slx')) model and click **RUN** from **SIMULATE** section in the **SIMULATION** tab. Observe the robot move in the Gazebo world.

![](https://www.mathworks.com/help/examples/ros/win64/GenerateAROSControlPluginFromSimulinkExample_08.png)

### Tune Controller Parameters with Dynamic Reconfigure

While the above model sends different commands, you can tune the controller parameters `Gain (Slider)`, `Linear Velocity (Slider)` and `Distance Threshold`, using dynamic reconfigure framework. Use the following MATLAB code to send updated values for these parameters:

#### Display Parameter

```
dynParamSvc = rossvcclient('/p3dx/sl_drivecontroller/set_parameters',DataFormat='struct',Timeout=10);
msg = rosmessage(dynParamSvc);
resp = call(dynParamSvc,msg);
% List the Names and Values
for k=1:numel(resp.Config.Doubles)
    disp(resp.Config.Doubles(k));
end

```

```
    MessageType: 'dynamic_reconfigure/DoubleParameter'
           Name: 'GainSlider_gain'
          Value: 5

    MessageType: 'dynamic_reconfigure/DoubleParameter'
           Name: 'LinearVelocitySlider_gain'
          Value: 1

    MessageType: 'dynamic_reconfigure/DoubleParameter'
           Name: 'DistanceThreshold_const'
          Value: 0.5000


```

```
for k=1:numel(resp.Config.Ints)
    disp(resp.Config.Doubles(k));
end
for k=1:numel(resp.Config.Strs)
    disp(resp.Config.Doubles(k));
end
for k=1:numel(resp.Config.Bools)
    disp(resp.Config.Doubles(k));
end

```

#### Change Linear Velocity

```
% Set Linear Velocity (Slider) gain to 2
msg.Config.Doubles(1).Name = 'LinearVelocitySlider_gain';
msg.Config.Doubles(1).Value = 2;
call(dynParamSvc,msg)

```

```
ans = struct with fields:
    MessageType: 'dynamic_reconfigure/ReconfigureResponse'
         Config: [1×1 struct]



```

Observe that the linear velocity of the mobile robot has increased.

#### Change Distance Threshold

```
% Set Distance Threshold to 0.1 and Linear Velocity to 0.5
msg.Config.Doubles(1).Name = 'LinearVelocitySlider_gain';
msg.Config.Doubles(1).Value = 0.5;
msg.Config.Doubles(2).Name = 'DistanceThreshold_const';
msg.Config.Doubles(2).Value = 0.1;
call(dynParamSvc,msg)

```

```
ans = struct with fields:
    MessageType: 'dynamic_reconfigure/ReconfigureResponse'
         Config: [1×1 struct]



```

With smaller velocity and distance threshold, the robot should be able to go very close to the desired position.

#### Use rqt_reconfigure Application

Alternatively, you can also change these values from the `rqt_reconfigure` application on the Virtual Machine as shown in the snapshot below:

![](https://www.mathworks.com/help/examples/ros/win64/GenerateAROSControlPluginFromSimulinkExample_09.png)

Shutdown the global MATLAB node

```
Shutting down global node /matlab_global_node_69673 with NodeURI http://192.168.93.1:49973/ and MasterURI http://192.168.93.144:11311.


```

### References

S. Chitta, E. Marder-Eppstein, W. Meeussen, V. Pradeep, A. Rodríguez Tsouroukdissian, J. Bohren, D. Coleman, B. Magyar, G. Raiola, M. Lüdtke and E. Fernandez Perdomo

- "ros_control: A generic and simple control framework for ROS"
- The Journal of Open Source Software, 2017. [PDF](http://www.theoj.org/joss-papers/joss.00456/10.21105.joss.00456.pdf)

You have a modified version of this example. Do you want to open this example with your edits?
