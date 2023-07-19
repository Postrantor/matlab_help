---
> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [ww2.mathworks.cn](https://ww2.mathworks.cn/help/stateflow/ug/sf4ml-lamp-example.html)
> 在独立 Stateflow 图中对信号灯 App 的逻辑进行建模。
---

此示例说明如何在独立 Stateflow® 图中对图形用户界面的逻辑建模。独立图使用 MATLAB® 作为动作语言来实现经典图语义。您可以使用 MATLAB 的全部功能对图进行编程，包括那些仅限于在 Simulink® 中进行代码生成的函数。有关详细信息，请参阅 [Create Stateflow Charts for Execution as MATLAB Objects](https://ww2.mathworks.cn/help/stateflow/ug/create-stateflow-chart-objects.html)。

您可以通过调用其输入事件并使用时序运算符来执行独立 Stateflow 图。事件驱动和计时器驱动的执行工作流适用于设计人机界面 (HMI) 和图形用户界面 (UI) 的基础逻辑。

- 当您使用 MATLAB App 设计工具时，来自界面小组件的回调函数会调用图中的事件。
- 在 Stateflow 图中，时序运算符和局部数据控制用户界面的属性。

有关如何使用 MATLAB 创建图形用户界面的详细信息，请参阅[使用 App 设计工具开发 App](https://ww2.mathworks.cn/help/matlab/app-designer.html)。

### 控制 App 设计工具用户界面

此用户界面包含控制信号灯的开关。当开关处于 On 位置时，根据 Mode（模式）选项按钮的位置，信号灯以 solid（常亮）或 blinking（闪烁）两种模式之一亮起。您可以通过移动 Blink Rate（闪烁速率）滑块来控制闪烁的速率。要启动 App，请在 App 设计工具的工具条中，点击**运行**。

![](https://ww2.mathworks.cn/help/stateflow/ug/xxsf_lamp_ui_zh_CN.png)

文件 `sf_lamp_logic.sfx` 定义实现用户界面逻辑的独立 Stateflow 图。该图包含输入事件（`ON`、`OFF`、`BLINKING` 和 `SOLID`）与局部数据（`delay` 和 `app`）。图中的动作控制在每个状态下可以访问哪些小组件。例如，在 `Off` 状态下的动作会导致用户界面中的信号灯小组件、Mode 选项按钮和 Blink Rate 滑块灰显。

![](https://ww2.mathworks.cn/help/stateflow/ug/controlanappdesigneruserinterfaceexample_01_zh_CN.png)

在 `On` 状态下，子状态 `Solid` 和 `Blinking` 表示两种工作模式。为了实现闪烁的信号灯，图依赖时序逻辑运算符 `[after](https://ww2.mathworks.cn/help/stateflow/ref/after.html)`。当图进入状态 `Blinking.Off` 时，出向转移上的表达式 `after(delay,sec)` 创建一个 MATLAB 计时器对象，该对象在几秒后执行图。然后，图转移到状态 `Blinking.On`，并创建另一个计时器对象来触发返回到 `Blinking.Off` 的转移。当图在这两种状态之间不断转移时，您可以通过更改局部数据延迟的值来调整闪烁的速率，或通过调用输入事件 `SOLID` 或 `OFF` 来退出闪烁模式。

处于 `On` 状态的历史结点保留最近的激活子状态的信息，以便当您打开信号灯时用户界面返回到先前的工作模式。

### 使用事件执行独立图

您可以通过在 MATLAB 命令行窗口中调用其输入事件函数来执行独立图。Stateflow 编辑器通过图动画突出显示激活状态和转移来显示每个命令的效果。

1. 创建图对象 `L` 并将 `delay` 的值初始化为 0.5。此值对应于每秒闪烁一次的闪烁速率（保持 on 状态 0.5 秒，保持 off 状态 0.5 秒）。

```
L = sf_lamp_logic(delay=0.5);
```

2. 打开信号灯。
3. 切换到闪烁模式。
4. 将 `delay` 的值设置为 0.25。此值对应于每秒闪烁两次的闪烁速率（保持 on 状态 0.25 秒，保持 off 状态 0.25 秒）。
5. 切换到常亮模式。
6. 关闭信号灯。
7. 从 MATLAB 工作区中删除图对象 `L`。

### 将独立图连接到用户界面

要在用户界面和独立 Stateflow 图之间建立双向连接，请打开 App 设计工具窗口并选择**代码视图**。

1. 在 App 设计工具窗口中，创建一个私有属性 `lampLogic` 来存储 Stateflow 图对象的句柄。

```
properties (Access = private)
    lampLogic
end
```

2. 创建一个 `StartupFcn` 回调函数，该函数创建图对象并将其局部数据 `app` 设置为用户界面句柄。将该图对象句柄赋给 `lampLogic` 私有属性。

```
function StartupFcn(app)
    app.lampLogic = sf_lamp_logic(delay=0.5,app=app);
end
```

3. 创建一个 `CloseRequestFcn` 回调函数，它在您关闭用户界面时删除图对象。

```
function UIFigureCloseRequest(app, event)
    delete(app.lampLogic);
    delete(app);
end
```

4. 对于每个用户界面小组件，添加一个回调函数来对独立图调用适当事件。

- 开关小组件的 `ValueChangedFcn` 回调函数：

```
function SwitchValueChanged(app, event)
    value = app.Switch.Value;
    switch lower(value)
        case "off"
            OFF(app.lampLogic);
        case "on"
            ON(app.lampLogic);
    end
end
```

- Mode 按钮小组件的 `SelectionChangedFcn` 回调函数：

```
function ModeButtonGroupSelectionChanged(app, event)
    selectedButton = app.ModeButtonGroup.SelectedObject;
    if selectedButton == app.SolidButton
        SOLID(app.lampLogic);
    else
        BLINKING(app.lampLogic);
    end
end
```

- Blink Rate 滑块小组件的 `ValueChangedFcn` 回调函数：

```
function BlinkRateSliderValueChanged(app, event)
    app.lampLogic.delay = round(0.5/app.BlinkRateSlider.Value,2);
end
```

当您运行用户界面时，您可以观察在图画布和信号灯小组件上调整控件小组件的效果。

## 另请参阅

[`after`](https://ww2.mathworks.cn/help/stateflow/ref/after.html)

## 相关主题

- [Create Stateflow Charts for Execution as MATLAB Objects](https://ww2.mathworks.cn/help/stateflow/ug/create-stateflow-chart-objects.html)
- [通过发送输入事件激活 Stateflow 图](https://ww2.mathworks.cn/help/stateflow/ug/using-input-events-to-activate-a-stateflow-chart.html)
- [使用时序逻辑控制图的执行](https://ww2.mathworks.cn/help/stateflow/ug/using-temporal-logic-in-state-actions-and-transitions.html)
- [Resume Prior Substate Activity by Using History Junctions](https://ww2.mathworks.cn/help/stateflow/ug/recording-state-activity-with-history-junctions.html)
- [使用 App 设计工具开发 App](https://ww2.mathworks.cn/help/matlab/app-designer.html)
