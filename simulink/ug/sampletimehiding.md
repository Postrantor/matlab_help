---
tip: translate by openai@2023-07-07 21:33:38
url: https://www.mathworks.com/help/simulink/ug/sampletimehiding.html
title: Blocks for Which Sample Time Is Not Recommended - MATLAB & Simulink --- 不建议采样时间的模块 - MATLAB & Simulink
date: 2023-07-07 21:32:27
tag:
summary: Best practices for modeling sample times.
---
## Blocks for Which Sample Time Is Not Recommended

Some blocks do not enable you to set the **Sample Time** parameter by default. However, you can see and set the **Sample Time** parameter for these blocks in an existing model if the sample time is set to a value other than the default of `-1` (inherited sample time). The **Sample Time** parameter is not available on certain blocks because specifying a sample time that is not `-1` on blocks such as the Gain, Sum, and n-D Lookup Table causes sample rate transition to be implicitly mixed with block algorithms. This mixing can often lead to ambiguity and confusion in Simulink® models.

> 有些块默认情况下不允许您设置**采样时间**参数。但是，如果采样时间设置为与默认值-1（继承采样时间）不同的值，您可以在现有模型中查看和设置这些块的**采样时间**参数。某些块不提供**采样时间**参数，因为在增益、求和和 n-D 查找表等块上指定不是-1 的采样时间会导致采样率转换隐式混合到块算法中。这种混合通常会导致 Simulink® 模型中的模糊性和混淆。

In most modeling applications, you specify rates for a model on the boundary of your system instead of on a block within the subsystem. You specify the system rate from incoming signals or the rate of sampling the output. You can also decide rates for events you are modeling that enter the subsystem as trigger, function-call, or enable/disable signals. Some global variables (such as Data Store Memory blocks) might need additional sample time specification. If you want to change rate within a system, use a Rate Transition block, which is designed specifically to model rate transitions.

> 在大多数建模应用中，您可以在系统边界而不是子系统内的块上指定模型的速率。您可以从传入信号或输出采样率指定系统速率。您还可以为您正在建模的事件确定速率，这些事件以触发器、函数调用或启用/禁用信号进入子系统。一些全局变量（例如数据存储内存块）可能需要额外的采样时间规范。如果要在系统内更改速率，请使用专门用于建模速率转换的速率转换块。

In a future release, you might not be able see or set this parameter on blocks where it is not appropriate.

> 在未来的版本中，您可能无法在不适当的块上查看或设置此参数。

### Best Practice to Model Sample Times

Use these approaches instead of setting the **Sample Time** parameter in the blocks where it is not appropriate:

> 使用这些方法而不是在不合适的块中设置**采样时间**参数：

- Adjust your model by specifying **Sample Time** only in the blocks listed in [Appropriate Blocks for the Sample Time Parameter](https://www.mathworks.com/help/simulink/ug/sampletimehiding.html#bumsmut), and set **Sample Time** to `-1` for all other blocks. To change the sample time for multiple blocks simultaneously, use Model Explorer. For more information, see **[Model Explorer](https://www.mathworks.com/help/simulink/slref/modelexplorer.html)**.

> 调整模型的方式只是在[适当的块中指定样本时间参数](https://www.mathworks.com/help/simulink/ug/sampletimehiding.html#bumsmut)，并且将所有其它块的样本时间设置为 `-1`。要同时更改多个块的样本时间，请使用模型浏览器。有关详细信息，请参阅[模型浏览器](https://www.mathworks.com/help/simulink/slref/modelexplorer.html)。

- Use the [Rate Transition](https://www.mathworks.com/help/simulink/slref/ratetransition.html) block to model rate transitions in your model.

> 使用[速率转换](https://www.mathworks.com/help/simulink/slref/ratetransition.html)块在模型中模拟速率转换。

- Use the [Signal Specification](https://www.mathworks.com/help/simulink/slref/signalspecification.html) block to specify sample time in models that don’t have source blocks, such as algebraic loops.

> 使用[信号规范](https://www.mathworks.com/help/simulink/slref/signalspecification.html)块在没有源块的模型中指定采样时间，例如代数环。

- Specify the simulation rate independently from the block sample times, using the Model Parameter dialog box.

> 在模型参数对话框中，独立于块采样时间指定模拟速率。

Once you have completed these changes, verify whether your model gives the same outputs as before.

> 完成这些更改后，请验证您的模型是否与以前的输出相同。

### Appropriate Blocks for the Sample Time Parameter

Specify sample time on the boundary of a model or subsystem, or in blocks designed to model rate transitions. Examples include:

> 在模型或子系统的边界上指定样本时间，或在为模拟速率转换而设计的块中指定样本时间。例子包括：

- Blocks in the Sources library
- Blocks in the Sinks library
- Trigger ports (if **Trigger type** is set to `function-call`) and Enable ports

> 触发端口（如果触发类型设置为“函数调用”）和启用端口

- Data Store Read and Data Store Write blocks, as the Data Store Memory block they link to might be outside the boundary of the subsystem

> 读取数据存储块和写入数据存储块，与它们相关联的数据存储存储块可能在子系统的边界之外。

- Rate Transition block
- Signal Specification block
- Blocks in the Discrete library
- Message Receive block
- Function Caller block

### Specify Sample Time in Blocks Where Hidden

You can specify sample time in the blocks that do not display the parameter on the block dialog box. If you specify value other than `-1` in these blocks, no error occurs when you simulate the model. However, a message appears on the block dialog box advising to set this parameter to `-1` (inherited sample time). If you promote the sample time block parameter to a mask, this parameter is always visible on the mask dialog box.

> 你可以在不在模块对话框中显示参数的模块中指定样本时间。 如果在这些模块中指定其他值，则模拟模型时不会出现错误。 但是，对话框中会出现一条消息，提示将此参数设置为“-1”（继承的样本时间）。 如果将样本时间模块参数提升为掩码，则此参数始终可在掩码对话框中可见。

To change the sample time in this case, use the `set_param` command. For example, select a block in the Simulink Editor and, at the command prompt, enter:

> 在此情况下改变采样时间，请使用 `set_param` 命令。例如，在 Simulink 编辑器中选择一个块，在命令提示符下输入：

```
set_param(gcb,'SampleTime','2');
```

## Related Topics

- [Resolve Rate Transitions](https://www.mathworks.com/help/simulink/ug/resolving-rate-transitions.html)
- [What Is Sample Time?](https://www.mathworks.com/help/simulink/ug/what-is-sample-time.html)
- [Sample Times in Subsystems](https://www.mathworks.com/help/simulink/ug/managing-sample-times-in-subsystems.html)
- [Sample Times in Systems](https://www.mathworks.com/help/simulink/ug/managing-sample-times-in-systems.html)

> - [解决速率转换](https://www.mathworks.com/help/simulink/ug/resolving-rate-transitions.html))
> - [什么是采样时间？](https://www.mathworks.com/help/simulink/ug/what-is-sample-time.html)
> - [子系统中的采样时间](https://www.mathworks.com/help/simulink/ug/managing-sample-times-in-subsystems.html)
> - [系统中的采样时间](https://www.mathworks.com/help/simulink/ug/managing-sample-times-in-systems.html)
