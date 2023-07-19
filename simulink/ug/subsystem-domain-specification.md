---
tip: translate by openai@2023-07-07 22:27:35
url: https://www.mathworks.com/help/simulink/ug/subsystem-domain-specification.html
title: Specify Execution Domain - MATLAB & Simulink --- 指定执行域 - MATLAB & Simulink
date: 2023-07-07 22:26:36
tag:
summary: Enforce discrete dynamics for a model or subsystem.
---
## Specify Execution Domain

Execution domain specification allows you to set a model and its subsystems and referenced models to simulate as discrete-time or data-driven systems. Use this setting to separate the discrete dynamics from the rest of its dynamics, for example, in the design of a deployable controller for a plant that is modeled with continuous-time dynamics.

> 执行域规范允许您设置模型及其子系统和引用模型以离散时间或数据驱动系统的方式进行仿真。使用此设置将离散动力学与其余动力学分开，例如，在设计具有连续时间动力学的工厂的可部署控制器时。

To simulate a computationally intensive signal processing or multirate signal processing system, you can also assign a dataflow domain. Dataflow domains simulate using a model of computation synchronous dataflow, which is data-driven and statically scheduled. For more information, see [Dataflow Domain](https://www.mathworks.com/help/dsp/ug/dataflow-domains.html) (DSP System Toolbox).

> 为了模拟计算密集的信号处理或多速率信号处理系统，您还可以指定数据流域。数据流域使用同步数据流模型模拟，该模型是数据驱动的，并且是静态调度的。有关详细信息，请参阅[数据流域](https://www.mathworks.com/help/dsp/ug/dataflow-domains.html)（DSP 系统工具箱）。

You can create subsystems that maintain their discrete execution domain irrespective of their environment. By constraining a subsystem to be discrete, you can increase reusability of your subsystem as a component. To improve code generation, this specification reduces unnecessary update methods, reduces major time step checks, and increases reusability of generated code.

> 你可以创建一个子系统，它可以保持其独立的执行域，不受其环境的影响。通过限制子系统的独立性，你可以增加子系统作为组件的可重用性。为了提高代码生成的效率，本规范减少了不必要的更新方法，减少了主时间步检查，增加了生成代码的可重用性。

### Domain Specification Badge

The domain specification badge indicates the execution domain computed to a model or subsystem when you update the model diagram. You can toggle the visibility of the domain specification badge by turning on the **Sample Time Display**. For more information on visualizing sample time, see [View Sample Time Information](https://www.mathworks.com/help/simulink/ug/how-to-view-sample-time-information.html). The badge is visible at the bottom left corner of the Simulink® Editor.

> 待定规范徽章表明，当您更新模型图时，计算到模型或子系统的执行域。您可以通过打开**样本时间显示**来切换待定规范徽章的可见性。有关可视化样本时间的更多信息，请参阅[查看样本时间信息](https://www.mathworks.com/help/simulink/ug/how-to-view-sample-time-information.html)。徽章位于 Simulink® 编辑器的左下角。

The model below shows a discrete Sine Wave block whose rate is reduced by the Rate Transition block before driving the Gain block.

> 模型如下：经过率变换块减少率后，离散正弦波块驱动增益块。

![](https://www.mathworks.com/help/simulink/ug/sds_badge_top_level_discrete.png)

Observe that the model receives the _Discrete_ execution domain because its contents are all discrete.

> 观察模型因其内容全部是离散的，而接收*离散*执行域。

You can also toggle the visibility of the badge by enabling or disabling the **Set Domain Specification** parameter in the **Execution** tab of the **Property Inspector**.

> 您还可以通过在属性检查器的执行选项卡中启用或禁用“设置域规范”参数来切换徽章的可见性。

### Types of Execution Domains

You can instruct Simulink to assign the execution domain, along with the allowed sample times, via the **Property Inspector**.

> 你可以通过**属性检查器**来指示 Simulink 分配执行域以及允许的采样时间。

<table><colgroup><col width="25%"><col width="25%"><col width="25%"><col width="25%"></colgroup><thead><tr><th>Specification</th><th>Discrete</th><th>Other</th><th>Dataflow</th></tr></thead><tbody><tr><td><code>Deduce from contents</code></td><td>X</td><td>X</td><td>-</td></tr><tr><td><code>Discrete</code></td><td>X</td><td>-</td><td>-</td></tr><tr><td><code>Dataflow</code></td><td>-</td><td>-</td><td>X</td></tr><tr><td><code>Export function</code></td><td>X</td><td>-</td><td>-</td></tr></tbody></table>

- ![](https://www.mathworks.com/help/simulink/ug/deduce_domain_icon.png)

  `Deduce from contents` Let Simulink assign the execution domain based on the contents of the subsystem.

> 让 Simulink 根据子系统的内容来分配执行域。

- ![](https://www.mathworks.com/help/simulink/ug/discrete_specified.png)

  `Discrete` Constrain all blocks in a subsystem to be discrete.
- ![](https://www.mathworks.com/help/simulink/ug/dataflow_specified.png)

  `Dataflow` Simulate a computationally-intensive signal processing or multi-rate signal processing system. This setting requires the DSP System Toolbox™.

> 模拟计算密集型信号处理或多速率信号处理系统。此设置需要 DSP 系统工具箱 ™。

- ![](https://www.mathworks.com/help/simulink/ug/export-function-model-badge7510e3e9c2ee416a4884f05eaa0fcc91.png)

> 导出函数模型

`Export function` Specify that the model is to be treated as an export-function model. When you specify a model as an export-function model, Simulink automatically sets the **Solver selection** in the Model Configuration Parameters to `Fixed-step` and `auto`. See [Export-Function Models Overview](https://www.mathworks.com/help/simulink/ug/export-function-models.html) for more information.

> 指定模型被当作出口功能模型。当您指定一个模型为出口功能模型时，Simulink 会自动将模型配置参数中的**求解器选择**设置为 `Fixed-step` 和 `auto`。有关更多信息，请参阅[出口功能模型概述](https://www.mathworks.com/help/simulink/ug/export-function-models.html)。

When you update the model diagram or simulate the model, the badge displays the computed execution domain for the model component. There are three execution domains in Simulink:

> 当您更新模型图或模拟模型时，徽章将显示模型组件的计算执行域。 Simulink 中有三个执行域：

- ![](https://www.mathworks.com/help/simulink/ug/discrete_compiled.png)

  **Discrete** Blocks have discrete states and sample times. Allowed samples times include [Discrete Sample Time](https://www.mathworks.com/help/simulink/ug/types-of-sample-time.html#brrdmmw-6), [Controllable Sample Time](https://www.mathworks.com/help/simulink/ug/types-of-sample-time.html#mw_5294bc25-b324-4d52-b18c-d8ae5bf3e9d4), and [Asynchronous Sample Time](https://www.mathworks.com/help/simulink/ug/types-of-sample-time.html#brrdmmw-13).

> 块的状态和采样时间是离散的。允许的采样时间包括[离散采样时间](https://www.mathworks.com/help/simulink/ug/types-of-sample-time.html#brrdmmw-6)、[可控采样时间](https://www.mathworks.com/help/simulink/ug/types-of-sample-time.html#mw_5294bc25-b324-4d52-b18c-d8ae5bf3e9d4)和[异步采样时间](https://www.mathworks.com/help/simulink/ug/types-of-sample-time.html#brrdmmw-13)。

- ![](https://www.mathworks.com/help/simulink/ug/dataflow_compiled.png)

  **Dataflow** Dataflow domains simulate using computation synchronous dataflow, which is data-driven and statically scheduled. This execution domain requires the DSP System Toolbox. For more information, see [Specifying Dataflow Domains](https://www.mathworks.com/help/dsp/ug/dataflow-domains.html#mw_0c833cd9-d846-4409-bfc5-30795e168fea) (DSP System Toolbox).

> 数据流域模拟使用计算同步数据流，它是数据驱动的，静态调度。此执行域需要 DSP 系统工具箱。有关详细信息，请参阅[指定数据流域](https://www.mathworks.com/help/dsp/ug/dataflow-domains.html#mw_0c833cd9-d846-4409-bfc5-30795e168fea)（DSP 系统工具箱）。

- ![](https://www.mathworks.com/help/simulink/ug/other_compiled_v2.png)

  **Other** Blocks are not strictly discrete.

  Subsystems that receive the **Other** execution domain include:

If a subsystem has continuous, variable, fixed-in-minor step, [Constant Sample Time](https://www.mathworks.com/help/simulink/ug/types-of-sample-time.html#brrdmmw-10), or a mixture of sample times, you can use the badge to enable or disable domain specification. The subsystem still receives the **Other** time domain.

> 如果子系统具有连续、可变、小步长固定的[恒定采样时间](https://www.mathworks.com/help/simulink/ug/types-of-sample-time.html#brrdmmw-10)或采样时间的混合，您可以使用徽章启用或禁用域规范。子系统仍然接收**其他**时间域。

The domain specification badge is not actionable when the currently selected subsystem or model is a linked block, inside a library block, or a conditionally executed subsystem that receives the **Other** domain. To change the execution domain of a linked library block, break the link to the parent library block. See [Disable or Break Links to Library Blocks](https://www.mathworks.com/help/simulink/ug/disable-links-to-library-blocks.html).

> 待选择的子系统或模型是链接块、库块内部的链接块或接收**其他**域的条件执行子系统时，域规范徽章无法执行。要更改链接库块的执行域，请断开与父库块的链接。请参阅[禁用或断开与库块的链接](https://www.mathworks.com/help/simulink/ug/disable-links-to-library-blocks.html)。

### Set Execution Domain

You can set the domain specification per subsystem and at the root level of the model using the **Execution** tab of the Property Inspector. To enable the Property Inspector for the model, on the **Modeling** tab, under **Design**, click **Property Inspector**, or press **Ctrl+Shift+I** on your keyboard. If the domain specification badge is displayed, you can also open the **Execution** settings in the Property Inspector by clicking the badge. See [Domain Specification Badge](https://www.mathworks.com/help/simulink/ug/subsystem-domain-specification.html#mw_ac9e2503-f3d2-4b8c-9429-4a8a5512dd90).

> 您可以使用属性检查器的**执行**选项卡，在子系统和模型的根级别设置域规范。要为模型启用属性检查器，请在**建模**选项卡下的**设计**部分单击**属性检查器**，或在键盘上按 **Ctrl + Shift + I**。如果显示域规范徽章，您还可以通过单击徽章在属性检查器中打开**执行**设置。请参阅[域规范徽章](https://www.mathworks.com/help/simulink/ug/subsystem-domain-specification.html#mw_ac9e2503-f3d2-4b8c-9429-4a8a5512dd90)。

![](https://www.mathworks.com/help/simulink/ug/sds_pi_opened.png)

Select the **Set Execution Domain** check box. You can now specify the **Domain**.

> 勾选**设置执行域**复选框。您现在可以指定**域**。

**Note**

Changing the domain specification at the root level of the model does not change the setting for its child subsystems.

> 对模型的根层级更改域规范不会更改其子系统的设置。

You can also enable this setting from the command line using `set_param` to set the `SetExecutionDomain` parameter `'on'` or `'off'`.

> 你也可以通过使用 `set_param` 从命令行启用此设置，将 `SetExecutionDomain` 参数设置为 `'on'` 或 `'off'`。

Once enabled, the default setting for the **Domain** parameter is `Deduce from contents`. When you update the diagram, the execution domain is deduced from the characteristics of the blocks in the currently open subsystem. For example, a system that has only discrete blocks is in the **Discrete** execution domain. See [Types of Execution Domains](https://www.mathworks.com/help/simulink/ug/subsystem-domain-specification.html#mw_0f58e817-ca53-4334-9858-3855a280c650).

> 一旦启用，“域”参数的默认设置为“从内容推断”。当您更新图表时，执行域将根据当前打开子系统中的块的特性推断出来。例如，只有离散块的系统处于**离散**执行域。请参阅[执行域类型](https://www.mathworks.com/help/simulink/ug/subsystem-domain-specification.html#mw_0f58e817-ca53-4334-9858-3855a280c650)。

The badge shows the current specification setting. If you set the subsystem domain to `Deduce from contents`, the badge text displays **Deduce** until you update the diagram. Once you update the model diagram, the badge shows the computed execution domain, as described in [Types of Execution Domains](https://www.mathworks.com/help/simulink/ug/subsystem-domain-specification.html#mw_0f58e817-ca53-4334-9858-3855a280c650). When you enable **Set domain specification** and **Domain** is set to `Deduce from Contents`, Simulink computes the execution domain of the currently focused subsystem based on the blocks and sample times inside the subsystem.

> 徽章显示当前规范设置。如果将子系统域设置为“从内容推断”，则徽章文本将显示**推断**，直到您更新图表。一旦您更新模型图，徽章将显示计算执行域，如[执行域类型](https://www.mathworks.com/help/simulink/ug/subsystem-domain-specification.html#mw_0f58e817-ca53-4334-9858-3855a280c650)中所述。当您启用**设置域规范**并且**域**设置为“从内容推断”时，Simulink 将根据子系统中的块和采样时间计算当前聚焦子系统的执行域。

To set the **Domain** parameter from the command line, use `set_param` to change `ExecutionDomainType` to either `'Deduce'` or `'Discrete'`. You can also get the computed execution domain after you update the diagram using the `CompiledExecutionDomain` parameter of the subsystem.

> 从命令行设置**域**参数，使用 `set_param` 将 `ExecutionDomainType` 更改为 `'Deduce'` 或 `'Discrete'`。更新图后，您还可以使用子系统的 `CompiledExecutionDomain` 参数获取计算执行域。

### Enforce Discrete Execution Domain for a Subsystem

This model shows how to specify execution domains for the constituent subsystems of a model. The model has a discrete cruise controller subsystem that tracks the reference speed set in the Desired Speed block. A car dynamics subsystem models the continuous-time dynamics of the car.

> 这个模型展示了如何为模型的组成子系统指定执行域。该模型具有一个离散巡航控制子系统，用于跟踪 Desired Speed 模块中设置的参考速度。汽车动力学子系统模拟汽车的连续时间动力学。

![](https://www.mathworks.com/help/examples/simulink/win64/EnforceDiscreteExecutionDomainForASubsystemExample_01.png)

Notice that the discrete cruise controller of the model has a hybrid sample time due to the presence of a continuous-time signal from the output of the car dynamics at the input port of the controller.

> 注意，由于从汽车动力学输出端到控制器输入端存在连续时间信号，模型的离散巡航控制器具有混合采样时间。

To enforce discrete-time execution of the controller, select the subsystem and open the **Execution** tab of the **Property Inspector** by clicking on the Domain badge at the bottom-left corner of the Simulink Editor.

> 在 Simulink 编辑器左下角的域徽章上点击，选择子系统并打开属性检查器的**执行**选项卡，以强制实施离散时间控制器。

![](https://www.mathworks.com/help/examples/simulink/win64/EnforceDiscreteExecutionDomainForASubsystemExample_02.png)

Enable the **Set execution domain** parameter and set **Domain** to `Discrete`. Update the model diagram or simulate the model.

> 启用**设置执行域**参数，将**域**设置为“离散”。更新模型图或模拟模型。

![](https://www.mathworks.com/help/examples/simulink/win64/EnforceDiscreteExecutionDomainForASubsystemExample_03.png)

Note that the discrete cruise controller subsystem is now discrete.

You can also set the execution domain of the car dynamics to `Deduce from Contents`. The car dynamics subsystem receives the Hybrid sample time and the **Other** execution domain. If you wish, set the **Sample Time** parameter of the Inport block in this subsystem to 0.

> 你也可以将汽车动力学的执行域设置为“从内容推断”。汽车动力学子系统接收混合采样时间和其他执行域。如果需要，可以将此子系统中的 Inport 块的采样时间参数设置为 0。

![](https://www.mathworks.com/help/examples/simulink/win64/EnforceDiscreteExecutionDomainForASubsystemExample_04.png)

## See Also

[What Is Sample Time?](https://www.mathworks.com/help/simulink/ug/what-is-sample-time.html) | [Sample Times in Subsystems](https://www.mathworks.com/help/simulink/ug/managing-sample-times-in-subsystems.html) | [How Propagation Affects Inherited Sample Times](https://www.mathworks.com/help/simulink/ug/how-propagation-affects-inherited-sample-times.html) | [Dataflow Domain](https://www.mathworks.com/help/dsp/ug/dataflow-domains.html) (DSP System Toolbox)

> 【什么是采样时间？】（[https://www.mathworks.com/help/simulink/ug/what-is-sample-time.html](https://www.mathworks.com/help/simulink/ug/what-is-sample-time.html)）|【子系统中的采样时间】（[https://www.mathworks.com/help/simulink/ug/managing-sample-times-in-subsystems.html](https://www.mathworks.com/help/simulink/ug/managing-sample-times-in-subsystems.html)）|【传播如何影响继承的采样时间】（[https://www.mathworks.com/help/simulink/ug/how-propagation-affects-inherited-sample-times.html](https://www.mathworks.com/help/simulink/ug/how-propagation-affects-inherited-sample-times.html)）|【数据流域】（[https://www.mathworks.com/help/dsp/ug/dataflow-domains.html](https://www.mathworks.com/help/dsp/ug/dataflow-domains.html)）（DSP 系统工具箱）
