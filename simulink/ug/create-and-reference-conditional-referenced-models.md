---
url: https://ww2.mathworks.cn/help/simulink/ug/create-and-reference-conditional-referenced-models.html
title: Conditionally Execute Referenced Models
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-18 20:59:37
tag: 
summary: Execute referenced models conditionally, similar to conditionally executed subsystems.
---
## Conditionally Execute Referenced Models

A _conditionally executed referenced model_, or _conditional model_, allows you to control its execution with an external signal. The external signal, called the _control signal_, is attached to the _control input port_. Conditional models are useful when you create complex model hierarchies that contain components whose execution depends on other components.

### Conditional Models

You can set up referenced models to execute conditionally, similar to conditional subsystems. For information about conditional subsystems, see [Conditionally Executed Subsystems Overview](https://ww2.mathworks.cn/help/simulink/ug/about-conditional-subsystems.html).

Simulink® software supports these conditional model types:

<table><colgroup><col width="24%"><col width="76%"></colgroup><thead><tr><th>Conditional Model</th><th>Description</th></tr></thead><tbody><tr><td>Enabled</td><td><p>An enable port executes a referenced model at each simulation step for which the control signal has a positive value. To add an enable port to a <span>Model</span> block, insert an <span>Enable</span> block in the referenced model.</p><div><p></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/ug/ex_enabled_model.png"></div><p></p></div><p>For an example of an enabled <span><em>subsystem</em></span>, see <a href="https://ww2.mathworks.cn/help/simulink/slref/enabled-subsystems.html">Enabled Subsystems</a>. A corresponding enabled referenced model uses the same blocks as are in the enabled subsystem.</p></td></tr><tr><td>Triggered</td><td><p>A trigger port executes a referenced model each time a trigger event occurs. To add a trigger port to a <span>Model</span> block, insert a <span>Trigger</span> block in the referenced model.</p><div><p></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/ug/triggered_model_rising.png"></div><p></p></div></td></tr><tr><td>Triggered and Enabled</td><td><p>A <span>Model</span> block can have both trigger and enable ports. If the enable control signal has a positive value at the time step for which a trigger event occurs, a triggered and enabled model executes once.</p></td></tr><tr><td>Function-Call</td><td><p>A function-call port executes a referenced model each time a function-call event occurs. To add a function-call port to a <span>Model</span> block, insert a <span>Trigger</span> block in the referenced model. Then, open the Block Parameters dialog box and set the <strong>Trigger type</strong> to <code>function-call</code>.</p><p>A Stateflow<sup>®</sup> chart, a <span>Function-Call Generator</span> block, a <span>Hit Crossing</span> block, or an appropriately configured custom S-function can provide function-call events. See <a href="https://ww2.mathworks.cn/help/simulink/ug/using-function-call-subsystems.html">Using Function-Call Subsystems</a>.</p><div><p></p><div class="sr-rd-content-center-small"><img class="" src="https://ww2.mathworks.cn/help/simulink/ug/function_call_model.png"></div><p></p></div><p>For an example of a function-call model, see <a href="https://ww2.mathworks.cn/help/simulink/slref/model-reference-function-call.html">Model Reference Function-Call</a>.</p></td></tr></tbody></table>

### Requirements for Conditional Models

Conditional models must meet the requirements for:

Conditional models must also meet the requirements specific to each type of conditional model.

<table><colgroup><col width="24%"><col width="76%"></colgroup><thead><tr><th>Conditional Model</th><th>Requirements</th></tr></thead><tbody><tr><td>Enabled</td><td><div><ul><li><p>Multi-rate enabled models cannot use multi-tasking solvers. Use single-tasking.</p></li><li><p>For models with enable ports at the root, if the model uses a fixed-step solver, the fixed-step size of the model must not exceed the rate for any block in the model.</p></li><li><p>Signal attributes of the enable port in the referenced model must be consistent with the input that the <span>Model</span> block provides to that enable port.</p></li></ul></div></td></tr><tr><td>Triggered</td><td><p>Signal attributes of the trigger port in the referenced model must be consistent with the input that the <span>Model</span> block provides to that trigger port.</p></td></tr><tr><td>Triggered and Enabled</td><td>See requirements for triggered models and enabled models.</td></tr><tr><td>Function-Call</td><td><div><ul><li><p>A function-call model cannot have an output port driven only by <span>Ground</span> blocks, including hidden <span>Ground</span> blocks inserted by Simulink. To meet this requirement, do the following:</p><div><ol><li><p>Insert a <span>Signal Conversion</span> block into the signal connected to the output port.</p></li><li><p>Enable the <strong>Exclude this block from 'Block reduction' optimization</strong> option of the inserted block.</p></li></ol></div></li><li><p>The parent model must trigger the function-call model at the rate specified by the &gt; <code>'Fixed-step size'</code> option if the function-call model meets both these conditions:</p><div><ul><li><p>It specifies a fixed-step solver.</p></li><li><p>It contains one or more blocks that use absolute or elapsed time.</p></li></ul></div><p>Otherwise, the parent model can trigger the function-call model at any rate.</p></li><li><p>A function-call model must not have direct internal connections between its root-level input and output ports. Simulink does not honor the <code>None</code> and <code>Warning</code> settings for the <strong>Invalid root Inport/Outport block connection</strong> diagnostic for a referenced function-call model. It reports all invalid root port connections as errors.</p></li><li><p>If the <strong>Sample time type</strong> is <code>periodic</code>, the sample-time period must not contain an offset.</p></li><li><p>The signal connected to a function-call port of a <span>Model</span> block must be scalar.</p></li></ul></div></td></tr></tbody></table>

### Modify a Referenced Model for Conditional Execution

1.  At the root level of the referenced model, insert one of the following blocks:
    
    <table><colgroup><col width="50%"><col width="50%"></colgroup><thead><tr><th>Type of Model</th><th>Blocks to Insert</th></tr></thead><tbody><tr><td><p>Enabled</p></td><td><p><a href="https://ww2.mathworks.cn/help/simulink/slref/enable.html"><span>Enable</span></a></p></td></tr><tr><td><p>Triggered</p></td><td><p><a href="https://ww2.mathworks.cn/help/simulink/slref/trigger.html"><span>Trigger</span></a></p></td></tr><tr><td><p>Triggered and Enabled</p></td><td><p>Trigger and Enable</p></td></tr><tr><td><p>Function-Call</p></td><td><p>Trigger</p></td></tr></tbody></table>
    
    For an enabled model, go to Step 3.
    
2.  For the Trigger block, set the **Trigger type** parameter:
    
    <table><colgroup><col width="50%"><col width="50%"></colgroup><thead><tr><th>Type of Model</th><th>Trigger Type Parameter Setting</th></tr></thead><tbody><tr><td><p>Triggered</p><p>Triggered and enabled</p></td><td><p>One of the following:</p><div><ul><li><p><code>rising</code></p></li><li><p><code>falling</code></p></li><li><p><code>either</code></p></li></ul></div></td></tr><tr><td><p>Function-Call</p></td><td><p><code>function-call</code></p></td></tr></tbody></table>
    
3.  Use the Model block ports to connect the referenced model to other ports in the parent model.
    
    *   The top of the Model block displays an icon that corresponds to the control signal type expected by the referenced model. For a triggered model, the top of the Model block displays this icon.
        
        ![](https://ww2.mathworks.cn/help/simulink/ug/triggered_model_model_block.png)
        
    

## See Also

[Enable](https://ww2.mathworks.cn/help/simulink/slref/enable.html) | [Trigger](https://ww2.mathworks.cn/help/simulink/slref/trigger.html) | [Function-Call Subsystem](https://ww2.mathworks.cn/help/simulink/slref/functioncallsubsystem.html)

## Related Topics

*   [Simulate Conditionally Executed Referenced Models](https://ww2.mathworks.cn/help/simulink/ug/simulate-conditionally-executed-referenced-models.html)
*   [Conditionally Executed Subsystems Overview](https://ww2.mathworks.cn/help/simulink/ug/about-conditional-subsystems.html)
*   [Export-Function Models Overview](https://ww2.mathworks.cn/help/simulink/ug/export-function-models.html)

How useful was this information?

Unrated

1 star

2 stars

3 stars

4 stars

5 stars

 Unrated  1 star  2 stars  3 stars  4 stars  5 stars

您点击的链接对应于以下 MATLAB 命令：

请在 MATLAB 命令行窗口中直接输入以执行命令。Web 浏览器不支持 MATLAB 命令。

Select a Web Site

Choose a web site to get translated content where available and see local events and offers. Based on your location, we recommend that you select: .

You can also select a web site from the following list:

[Contact your local office](#)