---
tip: translate by openai@2023-07-18 08:58:38
url: https://www.mathworks.com/help/stateflow/ug/sf4ml-automatic-driving-example.html
title: Automate Control of Intelligent Vehicles by Using Stateflow Charts - MATLAB & Simulink
date: 2023-07-18 08:57:08
tag:
summary: Model a highway scenario with intelligent vehicles that are controlled by the same decision logic.
> 概括：用相同的决策逻辑控制的智能车辆模拟高速公路场景。
---
This example shows how to model a highway scenario with intelligent vehicles that are controlled by the same decision logic. Each vehicle determines when to speed up, slow down, or change lanes based on the logic defined by a standalone Stateflow® chart. Because the driving conditions (including the relative position and speed of nearby vehicles) differ from vehicle to vehicle, separate chart objects in MATLAB® control the individual vehicles on the highway.

> 这个例子展示了如何使用同一决策逻辑控制智能车辆来模拟公路场景。每辆车根据独立的 Stateflow® 图定义的逻辑来决定何时加速、减速或变道。由于驾驶条件（包括附近车辆的相对位置和速度）因车辆而异，MATLAB® 中的单独图表对路上的各个车辆进行控制。

### Open Driving Scenario

To start the example, run the script `sf_driver_demo.m`. The script displays a 3-D animation of a long highway and several vehicles. The view focuses on a single vehicle and its surroundings. As this vehicle moves along the highway, the standalone Stateflow chart `sf_driver` shows the decision logic that determines its actions.

> 开始这个示例，运行脚本'sf_driver_demo.m'。该脚本将显示一个长长的公路和几辆车的 3D 动画。视图关注一辆车及其周围环境。当这辆车沿着公路行驶时，独立的 Stateflow 图'sf_driver'显示决策逻辑，以确定其行动。

![](https://www.mathworks.com/help/examples/driving_stateflow/win64/xxsf_driver_highway_scenario.png)

Starting from a random position, each vehicle attempts to travel at a target speed. Because the target speeds are chosen at random, the vehicles can obstruct one another. In this situation, a vehicle will try to change lanes and resume its target speed.

> 从一个随机位置开始，每辆车尝试以目标速度行驶。由于目标速度是随机选择的，车辆会互相阻碍。在这种情况下，车辆会尝试换道，并恢复其目标速度。

The class file `HighwayScenario` defines a ``[`drivingScenario`](https://www.mathworks.com/help/driving/ref/drivingscenario.html) (Automated Driving Toolbox)`` object that represents the 3-D environment that contains the highway and the vehicles on it. To control the motion of the vehicles, the `drivingScenario` object creates an array of Stateflow chart objects. Each chart object controls a different vehicle in the simulation.

> 这个类文件 `HighwayScenario` 定义了一个[`drivingScenario`](https://www.mathworks.com/help/driving/ref/drivingscenario.html)（自动驾驶工具箱）对象，它表示包含公路及其上的车辆的 3D 环境。为了控制车辆的运动，`drivingScenario` 对象创建了一个 Stateflow 图对象数组。每个图对象控制模拟中的不同车辆。

### Execute Decision Logic for Vehicles

The Stateflow chart `sf_driver` consists of two top-level states, `LaneKeep` and `LaneChange`.

> 图表 sf_driver 由两个顶级状态 LaneKeep 和 LaneChange 组成。

When the `LaneKeep` state is active, the corresponding vehicle stays in its lane of traffic. In this state, there are two possible substates:

> 当 `LaneKeep` 状态处于活动状态时，相应的车辆保持在其交通车道上。在此状态下，有两种可能的子状态：

- `Cruise` is active when the zone directly in front of the vehicle is empty and the vehicle can travel at its target speed.

> 当车辆前方的区域为空，且车辆可以以其目标速度行驶时，自动巡航功能就会被激活。

- `Follow` becomes active when the zone directly in front of the vehicle is occupied and its target speed is faster than the speed of the vehicle in front. In this case, the vehicle is forced to slow down and attempt to change lanes.

> 当车辆前方的区域被占据，且其目标速度比前方车辆的速度更快时，“跟随”功能会被激活。在这种情况下，车辆被迫减速并尝试换道。

When the `LaneChange` state is active, the corresponding vehicle attempts to change lanes. In this state, there are two possible substates:

> 当 `LaneChange` 状态处于活动状态时，相应的车辆尝试更换车道。在此状态下，有两种可能的子状态：

- `Continue` is active when the zone next to the vehicle is empty and the vehicle can change lanes safely.

> 继续行驶是当车辆旁边的区域为空时，车辆可以安全更换车道时才激活的。

- `Abort` becomes active when the zone next to the vehicle is occupied. In this case, the vehicle is forced to remain in its lane.

> 当车辆旁边的区域被占据时，`中止` 就会变为活动状态。在这种情况下，车辆被迫留在其车道上。

![](https://www.mathworks.com/help/examples/driving_stateflow/win64/AutomatedDriverControlInStateflowExample_01.png)

The transitions between the states `LaneKeep` and `LaneChange` are guarded by the value of `isLaneChanging`. In the `LaneKeep` state, the chart sets this local data to `true` when the substate `Follow` is active and there is enough room beside the vehicle to change lanes. In the `LaneChange` state, the chart sets this local data to `false` when the vehicle finishes changing lanes.

> 在 `LaneKeep` 和 `LaneChange` 状态之间的转换由 `isLaneChanging` 的值来守卫。在 `LaneKeep` 状态中，当子状态 `Follow` 处于活动状态且车辆旁边有足够的空间可以更换车道时，图表将此本地数据设置为 `true`。在 `LaneChange` 状态中，当车辆完成更换车道时，图表将此本地数据设置为 `false`。

## See Also

[`drivingScenario`](https://www.mathworks.com/help/driving/ref/drivingscenario.html) (Automated Driving Toolbox)

> 驾驶场景（自动驾驶工具箱）

## Related Topics

- [Create Stateflow Charts for Execution as MATLAB Objects](https://www.mathworks.com/help/stateflow/ug/create-stateflow-chart-objects.html)
- [Create Driving Scenario Programmatically](https://www.mathworks.com/help/driving/ug/create-driving-scenario-programmatically.html) (Automated Driving Toolbox)
- [Create Actor and Vehicle Trajectories Programmatically](https://www.mathworks.com/help/driving/ug/create-actor-and-vehicle-trajectories.html) (Automated Driving Toolbox)
- [Define Road Layouts Programmatically](https://www.mathworks.com/help/driving/ug/define-road-layouts.html) (Automated Driving Toolbox)
