---
tip: translate by openai@2023-06-21 14:04:32
...
---
> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [ww2.mathworks.cn](https://ww2.mathworks.cn/help/stateflow/ug/model-battery-management-with-custom-code.html)
> Use custom code to design a system for battery management.

---

This example shows how to use custom C code with Stateflow® to model a system that manages battery percentage, also known as the state of charge (SOC).

> 这个例子展示了如何使用 Stateflow® 中的自定义 C 代码来模拟一个管理电池百分比（也称为充电状态（SOC））的系统。

With Stateflow you can integrate your custom C code into charts. Using custom C code in a Stateflow chart allows you to:

> 使用 Stateflow，您可以将自定义 C 代码集成到图表中。在 Stateflow 图表中使用自定义 C 代码可以：

- Reuse existing algorithms that you have already coded.
- Use C code for low-level hardware operations, which may be difficult to implement with Stateflow.

> 使用 C 代码进行低级硬件操作，可能很难用 Stateflow 实现。

### Battery Management

This model represents several components of a battery management system. This system is designed to be implemented on a controller for battery powered devices, such as battery powered vehicle or a cell phone. The purpose of the battery management system is to limit the power demands on the battery and to ensure that the SOC does not get too high or too low. An SOC that is too high or too low would be detrimental to the health of the battery. Additionally, the model is designed to limit the discharge of the battery when the charge is low in a trade-off of performance for battery lifetime.

> 这个模型代表了电池管理系统的几个组成部分。该系统旨在在受电池驱动的设备（如电动车或手机）的控制器上实施。电池管理系统的目的是限制电池的功率需求，并确保 SOC 不会太高或太低。SOC 过高或过低都会对电池的健康造成不利影响。此外，该模型旨在在性能和电池寿命之间进行权衡，以限制电池在电量低时的放电量。

The battery management model achieves these goals with three different charts.

> 模型的电池管理可以通过三张不同的图表来实现这些目标。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/BatteryManagementExample_01.png)

Chart `Sensor Readings with Fault Detection` reads the sensor values from the battery pack and reports out when the sensors is in a faulted state. Chart `Battery State Estimation` uses the sensor reading to estimate the SOC of the battery. Chart `Battery Power Limit Control` conserves the battery, protects the battery health, and keeps the SOC away from either extreme. The chart accomplishes these tasks by setting power limits for the controller.

> 传感器读数与故障检测图表读取电池组的传感器值，并在传感器处于故障状态时报告。电池状态估计图表使用传感器读数来估计电池的 SOC。电池功率限制控制图表通过设定控制器的功率限制来节约电池，保护电池健康，并使 SOC 远离极端值。

With this model you can generate code and deploy that code to an embedded controller along with other control code that your system may need.

> 随着这个模型，你可以生成代码，并将代码部署到嵌入式控制器上，以及其他你的系统可能需要的控制代码。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/xxbattery-mgmt-design.png)

### Simulate Communication with Hardware

The chart `Sensor Readings with Fault Detection` consists of three parallel states (`VoltageSensor`, `CurrentSensor`, and `TemperatureSensor`) that model the readings of the battery voltage, current, and temperature sensors. The three parallel states contain similar decision logic for choosing between simulation and code generation behavior. For example, when the parameter `CODEGEN_FLAG` is `false`, the `VoltageSensor` contains this logic for simulating the voltage readings.

> 图表“带有故障检测的传感器读数”由三个并行状态（“VoltageSensor”、“CurrentSensor”和“TemperatureSensor”）组成，模拟电池电压、电流和温度传感器的读数。这三个并行状态包含相似的决策逻辑，用于选择仿真和代码生成行为。例如，当参数“CODEGEN_FLAG”为“false”时，“VoltageSensor”包含以下逻辑来模拟电压读数。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/xxbattery-mgmt-variant.png)

When using the model for simulation, the Dashboard panel allows you to control the sensor readings for the system inputs. If the calls to the battery monitor timeout, an error code of `-9999` is returned from the function.

> 当使用模型进行模拟时，仪表板面板允许您控制系统输入的传感器读数。如果调用电池监视器超时，函数将返回错误代码 `-9999`。

In each parallel state, the substate `SensorFaultDetection` handles the error signals returned by the sensors. In the event of a sensor error, `SensorFaultDetection` holds the last known valid sensor reading until the error code has been received for a certain amount of time. After this threshold is met, `SensorFaultDetection` sends a fault message and assumes it will be handled by the other control components of the controller.

> 在每个并行状态中，子状态“SensorFaultDetection”处理传感器返回的错误信号。如果发生传感器错误，“SensorFaultDetection”将保持最后一次已知的有效传感器读数，直到收到错误代码持续一段时间。超过这个阈值后，“SensorFaultDetection”发送故障消息，并假定其将由控制器的其他控制组件处理。

The example includes two custom C code files: `batteryMonitorDriver.h` and `batteryMonitorDriver.c`. These files represent the device driver code that would be used to get sensor data from the system, including battery voltage, current, and temperature and are used for code generation. For more information, see [Code Generation](#BatteryManagementExample-8).

> 示例包括两个自定义的 C 代码文件：`batteryMonitorDriver.h` 和 `batteryMonitorDriver.c`。这些文件代表设备驱动程序代码，用于从系统获取传感器数据，包括电池电压、电流和温度，并用于代码生成。有关更多信息，请参见[代码生成](#BatteryManagementExample-8)。

To simulate the model with the driver code:

> 模擬模型的驅動代碼：

1. Open the Configuration Parameters dialog box.

> 打开配置参数对话框。

2. In the **Simulation Target** pane, specify the header file and source file.

> 在**模拟目标**面板中，指定头文件和源文件。

3. Under **Advanced parameters**, select **Import custom code**.

> 在**高级参数**下，选择**导入自定义代码**。

For more information, see [Configure Custom Code for Your Model](https://ww2.mathworks.cn/help/stateflow/ug/share-data-using-custom-c-code.html#brj0vwv).

> 更多信息，请参阅[为模型配置自定义代码](https://ww2.mathworks.cn/help/stateflow/ug/share-data-using-custom-c-code.html#brj0vwv)。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/xxbattery-mgmt-sim-target.png)

### Estimate Battery State of Charge by Reusing Custom Code

To estimate the battery state of charge, the model utilizes a custom C code algorithm. The included file `estimateSOC.c` contains this code:

> 为了估计电池状态充电，模型采用了定制的 C 语言算法。包含的文件“estimateSOC.c”包含了这段代码：

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/xxbattery-mgmt-est-SOC-code.png)

With this algorithm, you can easily call the C code function, rather than reimplementing it with Stateflow charts.

> 通过这个算法，您可以轻松地调用 C 代码函数，而不用用 Stateflow 图重新实现它。

In order to account for the sensitivity of noise and change of current in the `estimateSOC` algorithm, Stateflow logic is used to implement a debouncing algorithm. This logic simplifies the SOC percentage into 5 ranges: `MAX`, `HIGH`, `NORMAL`, `LOW`, and `MIN`. These ranges prevent rapid fluctuation between different control states. The exit transitions from the child states go to the edge of the parent state. When these transitions are taken, Stateflow returns to the default transition of the parent state.

> 为了考虑噪声的敏感性和 `estimateSOC` 算法中电流的变化，使用 Stateflow 逻辑来实现去抖动算法。该逻辑将 SOC 百分比简化为 5 个范围：`MAX`，`HIGH`，`NORMAL`，`LOW` 和 `MIN`。这些范围可以防止不同控制状态之间的快速波动。从子状态出口的转换转到父状态的边缘。当这些转换被采取时，Stateflow 会返回父状态的默认转换。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/BatteryManagementExample_02.png)

### Logic to Control Device State of Charge

It is easier to design this control logic with a Stateflow chart, rather than implementing the logic control through custom code. This chart implements power limit on the battery based on the estimated battery state.

> 用状态流图设计这个控制逻辑比通过自定义代码实现控制逻辑更容易。此图表根据估计的电池状态实现电池的功率限制。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/BatteryManagementExample_03.png)

The chart represents five possible modes for power limits on the battery.

> 图表表示电池上的五种可能的功率限制。

1. Performance Mode: Allow high power draw when battery charge is high.

> 允许在电池充电高时使用高功率模式。

2. Battery Saver Mode: Limit power draw on the battery for efficiency when charge is low.

> 电池节能模式：在电量低时限制功率消耗以提高效率。

3. Off: Do not allow Power Draw when battery is at state of charge limits.

> 不允许电池处于充电限制状态时进行功率抽取。

4. Fast Charge: Quickly charge the battery when charge is low.

> 快速充电：电量低时快速充电电池。

5. Slow Charge: Slowly charge the battery when charge is high for battery health benefits.

> 慢充：充电电压高时，为了保护电池健康，应慢慢充电。

### Simulate Using the Dashboard Panel

To test that the model behaves as expected, you can use the dashboard panel to simulate the voltage, current, and temperature readings. The switches allow you to simulate a sensor error to test the fault detection logic. The gauge and plot dashboard blocks are bound to the activity of stateflow charts to visualize internal states and data. You can move and minimize the dashboard panel while navigating the model. For more information on dashboard blocks, see [Control Simulations with Interactive Displays](https://ww2.mathworks.cn/help/simulink/control-and-visualize-simulations-with-interactive-displays.html) (Simulink).

> 为了测试模型的预期行为，您可以使用仪表板面板模拟电压、电流和温度读数。开关允许您模拟传感器错误以测试故障检测逻辑。仪表和绘图仪表板块绑定到 Stateflow 图的活动，可视化内部状态和数据。在导航模型时，您可以移动和最小化仪表板面板。有关仪表板块的更多信息，请参阅[使用交互式显示控制仿真](https://ww2.mathworks.cn/help/simulink/control-and-visualize-simulations-with-interactive-displays.html)（Simulink）。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/xxbattery-mgmt-dashboard-panel.png)

### Code Generation

Inputs into the chart `Sensor Readings with Fault Detection` are provided with two C code files: `batteryMonitorDriver.h` and `batteryMonitorDriver.c`. These two files represent the device driver code that would be used to get sensor data from the system, including battery voltage, current, and temperature.

> 输入到图表“带故障检测的传感器读数”的是由两个 C 代码文件提供的：`batteryMonitorDriver.h` 和 `batteryMonitorDriver.c`。这两个文件代表了从系统中获取传感器数据的设备驱动程序，包括电池电压、电流和温度。

To use this model for code generation, the driver code must communicate with the external hardware. To enable this functionality, a variant transition using the control variable `CODEGEN_FLAG` allows the Stateflow chart to call the C code directly when generating code and simulate the sensor value with noise. In the Model Explorer, open the Base Workspace and set the value of `CODEGEN_FLAG` to `true`. For more information on Stateflow Variants and variant transitions, see [Control Indicator Lamp Dimmer Using Variant Conditions](https://ww2.mathworks.cn/help/stateflow/ug/variant-transitions.html).

> 要使用此模型进行代码生成，驱动程序代码必须与外部硬件进行通信。为了启用此功能，使用控制变量 `CODEGEN_FLAG` 的变量转换允许 Stateflow 图表在生成代码时直接调用 C 代码，并使用噪声模拟传感器值。在模型浏览器中，打开基础工作区，将 `CODEGEN_FLAG` 的值设置为 `true`。有关 Stateflow 变量和变量转换的更多信息，请参阅[使用变量条件控制指示灯调光器](https://ww2.mathworks.cn/help/stateflow/ug/variant-transitions.html)。

To compile the generated code with the driver code, open the Configuration Parameters dialog box and, in the **Code Generation** > **Custom Code** pane, specify the header file and source file. For more information, see [Configure Custom Code for Your Model](https://ww2.mathworks.cn/help/stateflow/ug/share-data-using-custom-c-code.html#brj0vwv).

> 要用驱动程序代码编译生成的代码，请打开“配置参数”对话框，然后在**代码生成** > **自定义代码**面板中指定头文件和源文件。有关详细信息，请参阅[为模型配置自定义代码](https://ww2.mathworks.cn/help/stateflow/ug/share-data-using-custom-c-code.html#brj0vwv)。

![](https://ww2.mathworks.cn/help/examples/stateflow/win64/xxbattery-mgmt-custom-code.png)

## References

[1] Ramadass, P., B. Haran, R. E. White, and B. N. Popov. “Mathematical modeling of the capacity fade of Li-ion cells.” _Journal of Power Sources_. 123 (2003), pp. 230–240.

> [1] Ramadass, P., B. Haran, R. E. White 和 B. N. Popov。《锂离子电池容量损耗的数学建模》。《动力源期刊》123 (2003)，页 230-240。

[2] Ning, G., B. Haran, and B. N. Popov. “Capacity fade study of lithium-ion batteries cycled at high discharge rates.” _Journal of Power Sources_. 117 (2003), pp. 160–169.

> [2] 宁、G.、B. Haran 和 B. N. Popov。“高放电速率循环的锂离子电池容量衰减研究”。《动力源杂志》117 (2003)，页 160-169。

## Related Topics

- [Reuse Custom Code in Stateflow Charts](https://ww2.mathworks.cn/help/stateflow/ug/share-data-using-custom-c-code.html)
- [Control Indicator Lamp Dimmer Using Variant Conditions](https://ww2.mathworks.cn/help/stateflow/ug/variant-transitions.html)
- [Code Generation Using Simulink Coder](https://ww2.mathworks.cn/help/rtw/gs/algorithm-development-workflows.html) (Simulink Coder)
- [Code Generation Workflows with Embedded Coder](https://ww2.mathworks.cn/help/ecoder/gs/code-generation-workflows-with-embedded-coder.html#mw_c3d8e5de-80fe-41b1-b964-364d9c5b561d) (Embedded Coder)
- [Tune and Experiment with Block Parameter Values](https://ww2.mathworks.cn/help/simulink/ug/using-tunable-parameters.html) (Simulink)
