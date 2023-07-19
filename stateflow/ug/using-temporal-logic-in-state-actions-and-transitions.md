---
url: https://ww2.mathworks.cn/help/stateflow/ug/using-temporal-logic-in-state-actions-and-transitions.html
title: 使用时序逻辑控制图的执行
- MATLAB & Simulink
- MathWorks 中国
date: 2023-07-18 10:12:51
tag: 
summary: 使用基于事件的和绝对时间的时序逻辑运算符。
---

## 使用时序逻辑控制图的执行

时序逻辑通过时间控制 Stateflow 图的执行。在状态动作和转移中，可以使用两种类型的时序逻辑：

- 基于事件的时序逻辑会跟踪重复发生的事件。可以将任何显式或隐式事件用作基础事件。
- 绝对时间时序逻辑会跟踪从状态激活以来经过的时间。绝对时间时序逻辑运算符的计时取决于 Stateflow® 图的类型：

  - Simulink® 模型中的图根据仿真时间定义绝对时间时序逻辑。
  - MATLAB® 中的独立图根据挂钟时间定义绝对时间时序逻辑，其精度限制在 1 毫秒。

### 时序逻辑运算符

要基于时序逻辑定义 Stateflow 图的行为，请使用下表中列出的运算符。这些运算符可以出现在以下项中：

每个时序逻辑运算符都有一个关联状态，它是动作所在的状态或转移路径的来源状态。Stateflow 图会在每次关联状态重新激活时重置每个运算符使用的计数器。

<table><colgroup><col width="11%"><col width="22%"><col width="33%"><col width="33%"></colgroup><thead><tr><th>运算符</th><th>语法</th><th>描述</th><th>示例</th></tr></thead><tbody><tr><td rowspan="4"><a href="https://ww2.mathworks.cn/help/stateflow/ref/after.html"><code>after</code></a></td><td rowspan="2"><p><code>after(n,E)</code></p><p><code>n</code> 是正整数或计算结果为正整数值的表达式。</p><p><code>E</code> 是运算符的基础事件。</p></td><td rowspan="2">如果自关联状态激活以来 <code>E</code> 事件至少已发生 <code>n</code> 次，则返回 <code>true</code>。否则，运算符返回 <code>false</code>。</td><td><p>当图处理事件 <code>E</code> 的广播时，从状态激活后 <code>E</code> 的第三次广播开始显示状态消息。</p><div><div><div><pre>on after(3,E): disp("ON");
</pre></div></div></div></td></tr><tr><td><p>当图处理事件 <code>E</code> 的广播时，从关联状态激活后 <code>E</code> 的第五次广播开始，发生转出关联状态的转移。</p><div><div><div><pre>after(5,E)
</pre></div></div></div></td></tr><tr><td><p><code>after(n,tick)</code></p><p><code>n</code> 是正整数或计算结果为正整数值的表达式。</p></td><td><p>如果图自关联状态激活以来至少唤醒了 <code>n</code> 次，则返回 <code>true</code>。否则，运算符返回 <code>false</code>。</p><p>当 Simulink 模型中的 Stateflow 图有输入事件时，不支持隐式事件 <code>tick</code>。有关详细信息，请参阅 <a href="https://ww2.mathworks.cn/help/stateflow/ug/using-implicit-events.html" hreflang="en">Control Chart Behavior by Using Implicit Events</a>。</p></td><td><p>当图自关联状态激活以来至少唤醒了七次，且变量 <code>temp</code> 大于 98.6 时，发生转出关联状态的转移。</p><div><div><div><pre>after(7,tick)[temp &gt; 98.6]
</pre></div></div></div></td></tr><tr><td><p><code>after(n,sec)</code></p><p><code>after(n,msec)</code></p><p><code>after(n,usec)</code></p><p><code>n</code> 是正实数或计算结果为正实数的表达式。</p></td><td><p>如果自关联状态激活以来至少已经过 <code>n</code> 个时间单位，则返回 <code>true</code>。否则，运算符返回 <code>false</code>。</p><p>在 Simulink 模型的图中，以秒 (<code>sec</code>)、毫秒 (<code>msec</code>) 或微秒 (<code>usec</code>) 作为时间单位。</p><p>在 MATLAB 的独立图中，以秒 (<code>sec</code>) 作为时间单位。运算符创建一个 MATLAB <a href="https://ww2.mathworks.cn/help/matlab/ref/timer.html"><code>timer</code></a> 对象，该对象生成隐式事件来唤醒图。MATLAB <code>timer</code> 对象的精度限制在 1 毫秒。有关详细信息，请参阅 <a href="https://ww2.mathworks.cn/help/stateflow/ug/how-events-drive-chart-execution.html#mw_5eb311dc-c2d5-43c3-a199-9a5174bded16" hreflang="en">Events in Standalone Charts</a>。</p></td><td><p>从关联状态激活后的 12.3 秒开始，每次图被唤醒时，都将 <code>temp</code> 变量设置为 <code>LOW</code>。</p><div><div><div><pre>on after(12.3,sec): temp = LOW;
</pre></div></div></div></td></tr><tr><td rowspan="4"><a href="https://ww2.mathworks.cn/help/stateflow/ref/at.html"><code>at</code></a></td><td rowspan="2"><p><code>at(n,E)</code></p><p><code>n</code> 是正整数或计算结果为正整数值的表达式。</p><p><code>E</code> 是运算符的基础事件。</p></td><td rowspan="2">如果自关联状态激活以来 <code>E</code> 事件恰好发生了 <code>n</code> 次，则返回 <code>true</code>。否则，运算符返回 <code>false</code>。</td><td><p>状态激活后，在图处理 <code>E</code> 事件的第三次广播时，显示状态消息。</p><div><div><div><pre>on at(3,E): disp("ON");
</pre></div></div></div></td></tr><tr><td><p>状态激活后，在图处理 <code>E</code> 事件的第五次广播时，发生转出关联状态的转移。</p><div><div><div><pre>at(5,E)
</pre></div></div></div></td></tr><tr><td><p><code>at(n,tick)</code></p><p><code>n</code> 是正整数或计算结果为正整数值的表达式。</p></td><td><p>如果图自关联状态激活以来恰好唤醒了 <code>n</code> 次，则返回 <code>true</code>。否则，运算符返回 <code>false</code>。</p><p>当 Simulink 模型中的 Stateflow 图有输入事件时，不支持隐式事件 <code>tick</code>。有关详细信息，请参阅 <a href="https://ww2.mathworks.cn/help/stateflow/ug/using-implicit-events.html" hreflang="en">Control Chart Behavior by Using Implicit Events</a>。</p></td><td><p>当图自关联状态激活以来第七次被唤醒，且变量 <code>temp</code> 大于 98.6 时，发生转出关联状态的转移。</p><div><div><div><pre>at(7,tick)[temp &gt; 98.6]
</pre></div></div></div></td></tr><tr><td><p><code>at(n,sec)</code></p><p><code>n</code> 是正实数或计算结果为正实数的表达式。</p></td><td><p>如果自关联状态激活以来恰好经过了 <code>n</code> 秒，则返回 <code>true</code>。否则，运算符返回 <code>false</code>。</p><p>仅 MATLAB 中的独立图支持使用 <code>at</code> 作为绝对时间时序逻辑运算符。运算符创建一个 MATLAB <a href="https://ww2.mathworks.cn/help/matlab/ref/timer.html"><code>timer</code></a> 对象，该对象生成隐式事件来唤醒图。MATLAB <code>timer</code> 对象的精度限制在 1 毫秒。有关详细信息，请参阅 <a href="https://ww2.mathworks.cn/help/stateflow/ug/how-events-drive-chart-execution.html#mw_5eb311dc-c2d5-43c3-a199-9a5174bded16" hreflang="en">Events in Standalone Charts</a>。</p></td><td><p>如果状态已激活恰好 12.3 秒，则将 <code>temp</code> 变量设置为 <code>HIGH</code>。</p><div><div><div><pre>on at(12.3,sec): temp = HIGH;
</pre></div></div></div></td></tr><tr><td rowspan="4"><a href="https://ww2.mathworks.cn/help/stateflow/ref/before.html"><code>before</code></a></td><td rowspan="2"><p><code>before(n,E)</code></p><p><code>n</code> 是正整数或计算结果为正整数值的表达式。</p><p><code>E</code> 是运算符的基础事件。</p></td><td rowspan="2"><p>如果自关联状态激活以来 <code>E</code> 事件的发生次数少于 <code>n</code> 次，则返回 <code>true</code>。否则，运算符返回 <code>false</code>。</p><p>时序逻辑运算符 <code>before</code> 仅在 Simulink 模型的 Stateflow 图中受支持。</p></td><td><p>状态激活后，在图处理 <code>E</code> 事件的第一次和第二次广播时，显示状态消息。</p><div><div><div><pre>on before(3,E): disp("ON");
</pre></div></div></div></td></tr><tr><td><p>当图处理事件 <code>E</code> 的广播时，仅在关联状态激活后 <code>E</code> 的广播次数少于五次时，发生转出关联状态的转移。</p><div><div><div><pre>before(5,E)
</pre></div></div></div></td></tr><tr><td><p><code>before(n,tick)</code></p><p><code>n</code> 是正整数或计算结果为正整数值的表达式。</p></td><td><p>如果图自关联状态激活以来的唤醒次数少于 <code>n</code> 次，则返回 <code>true</code>。否则，运算符返回 <code>false</code>。</p><p>当 Simulink 模型中的 Stateflow 图有输入事件时，不支持隐式事件 <code>tick</code>。有关详细信息，请参阅 <a href="https://ww2.mathworks.cn/help/stateflow/ug/using-implicit-events.html" hreflang="en">Control Chart Behavior by Using Implicit Events</a>。</p><p>时序逻辑运算符 <code>before</code> 仅在 Simulink 模型的 Stateflow 图中受支持。</p></td><td><p>当图被唤醒时，仅在自关联状态激活以来的唤醒次数少于七次，且变量 <code>temp</code> 大于 98.6 时，发生转出关联状态的转移。</p><div><div><div><pre>before(7,tick)[temp &gt; 98.6]
</pre></div></div></div></td></tr><tr><td><p><code>before(n,sec)</code></p><p><code>before(n,msec)</code></p><p><code>before(n,usec)</code></p><p><code>n</code> 是正实数或计算结果为正实数的表达式。</p></td><td><p>如果自关联状态激活以来经过的时间少于 <code>n</code> 个单位，则返回 <code>true</code>。否则，运算符返回 <code>false</code>。</p><p>指定时间，单位为秒 (<code>sec</code>)、毫秒 (<code>msec</code>) 或微秒 (<code>usec</code>)。</p><p>时序逻辑运算符 <code>before</code> 仅在 Simulink 模型的 Stateflow 图中受支持。</p></td><td><p>在关联状态激活后的 12.3 秒之内，每次图被唤醒时，都将 <code>temp</code> 变量设置为 <code>MED</code>。</p><div><div><div><pre>on before(12.3,sec): temp = MED;
</pre></div></div></div></td></tr><tr><td rowspan="4"><a href="https://ww2.mathworks.cn/help/stateflow/ref/every.html"><code>every</code></a></td><td rowspan="2"><p><code>every(n,E)</code></p><p><code>n</code> 是正整数或计算结果为正整数值的表达式。</p><p><code>E</code> 是运算符的基础事件。</p></td><td rowspan="2"><p>自关联状态激活以来，<code>E</code> 事件每发生 <code>n</code> 次<sup></sup>时，返回 <code>true</code>。否则，运算符返回 <code>false</code>。</p></td><td><p>状态激活后，在图每处理三次 <code>E</code> 事件的广播时，显示状态消息。</p><div><div><div><pre>on every(3,E): disp("ON");
</pre></div></div></div></td></tr><tr><td><p>状态激活后，在图每处理五次 <code>E</code> 事件的广播时，发生转出关联状态的转移。</p><div><div><div><pre>every(5,E)
</pre></div></div></div></td></tr><tr><td><p><code>every(n,tick)</code></p><p><code>n</code> 是正整数或计算结果为正整数值的表达式。</p></td><td><p>自关联状态激活以来，图每唤醒 <code>n</code> 次<sup></sup>时，返回 <code>true</code>。否则，运算符返回 <code>false</code>。</p><p>当 Simulink 模型中的 Stateflow 图有输入事件时，不支持隐式事件 <code>tick</code>。有关详细信息，请参阅 <a href="https://ww2.mathworks.cn/help/stateflow/ug/using-implicit-events.html" hreflang="en">Control Chart Behavior by Using Implicit Events</a>。</p></td><td><p>自关联状态激活以来，每发生七次 <code>tick</code> 事件且变量 <code>temp</code> 大于 98.6 时，发生转出关联状态的转移。</p><div><div><div><pre>every(7,tick)[temp &gt; 98.6]
</pre></div></div></div></td></tr><tr><td><p><code>every(n,sec)</code></p><p><code>n</code> 是正实数或计算结果为正实数的表达式。</p></td><td><p>自关联状态激活以来，每经过 <code>n</code> 秒时，返回 <code>true</code>。否则，运算符返回 <code>false</code>。</p><p>仅 MATLAB 中的独立图支持使用 <code>every</code> 作为绝对时间时序逻辑运算符。运算符创建一个 MATLAB <a href="https://ww2.mathworks.cn/help/matlab/ref/timer.html"><code>timer</code></a> 对象，该对象生成隐式事件来唤醒图。MATLAB <code>timer</code> 对象的精度限制在 1 毫秒。有关详细信息，请参阅 <a href="https://ww2.mathworks.cn/help/stateflow/ug/how-events-drive-chart-execution.html#mw_5eb311dc-c2d5-43c3-a199-9a5174bded16" hreflang="en">Events in Standalone Charts</a>。</p></td><td><p>状态激活后，每过 12.3 秒 <code>temp</code> 变量增加 5。</p><div><div><div><pre>on every(12.3,sec): temp = temp+5;
</pre></div></div></div></td></tr><tr><td rowspan="3"><a href="https://ww2.mathworks.cn/help/stateflow/ref/temporalcount.html"><code>temporalCount</code></a></td><td><p><code>temporalCount(E)</code></p><p><code>E</code> 是运算符的基础事件。</p></td><td><p>返回自关联状态激活以来事件 <code>E</code> 的发生次数。</p><p>仅在 Simulink 模型的 Stateflow 图中支持使用 <code>temporalCount</code> 作为基于事件的时序逻辑运算符。</p></td><td><p>每次图处理 <code>E</code> 事件的广播时，访问数组 <code>M</code> 的连续元素。</p><div><div><div><pre>on E: y = M(temporalCount(E));
</pre></div></div></div></td></tr><tr><td><code>temporalCount(tick)</code></td><td><p>返回自关联状态激活以来图唤醒的次数。</p><p>当 Simulink 模型中的 Stateflow 图有输入事件时，不支持隐式事件 <code>tick</code>。</p><p>仅在 Simulink 模型的 Stateflow 图中支持使用 <code>temporalCount</code> 作为基于事件的时序逻辑运算符。</p></td><td><p>将输入数据 <code>u</code> 的值存储在数组 <code>M</code> 的连续元素中。</p><div><div><div><pre>en,du:
   M(temporalCount(tick)+1) = u;
</pre></div></div></div></td></tr><tr><td><p><code>temporalCount(sec)</code></p><p><code>temporalCount(msec)</code></p><p><code>temporalCount(usec)</code></p></td><td><p>返回自关联状态激活以来经过的时间长度。</p><p>指定时间，单位为秒 (<code>sec</code>)、毫秒 (<code>msec</code>) 或微秒 (<code>usec</code>)。</p></td><td><p>存储自状态激活以来的毫秒数。</p><div><div><div><pre>en,du:
   y = temporalCount(msec);
</pre></div></div></div></td></tr><tr><td rowspan="2"><a href="https://ww2.mathworks.cn/help/stateflow/ref/elapsed.html"><code>elapsed</code></a></td><td><p><code>elapsed(sec)</code></p></td><td><p>返回自关联状态激活以来经过的时间长度。</p><p>等效于 <code>temporalCount(sec)</code>。</p></td><td><p>存储自状态激活以来的秒数。</p><div><div><div><pre>en,du:
   y = elapsed(sec);
</pre></div></div></div></td></tr><tr><td><code>et</code></td><td>执行 <code>elapsed(sec)</code> 的另一种方式。</td><td><p>当图处理事件 <code>E</code> 的广播时，发生转出关联状态的转移，并显示自状态激活以来经过的时间。</p><div><div><div><pre>E{disp(et);}
</pre></div></div></div></td></tr><tr><td rowspan="2"><a href="https://ww2.mathworks.cn/help/stateflow/ref/count.html"><code>count</code></a></td><td rowspan="2"><p><code>count(C)</code></p><p><code>C</code> 是计算结果为 <code>true</code> 或 <code>false</code> 的表达式。</p></td><td rowspan="2"><p>返回自条件表达式 <code>C</code> 变为 <code>true</code> 以及关联状态激活以来，图唤醒的次数。</p><p>如果条件表达式 <code>C</code> 变为 <code>false</code> 或关联状态变为非激活，Stateflow 图会重置 <code>count</code> 运算符的值。</p><p>在 Simulink 模型的图中，<code>count</code> 的值可能取决于步长。更改模型的求解器或步长会影响 <code>count</code> 运算符生成的结果。</p></td><td><p>当变量 <code>x</code> 大于或等于 2 的时间超过五次图执行时间时，发生转出关联状态的转移。</p><div><div><div><pre>[count(x&gt;=2) &gt; 5]
</pre></div></div></div></td></tr><tr><td><p>存储自变量 <code>x</code> 变为大于 5 以来图执行的次数。</p><div><div><div><pre>en,du:
   y = count(x&gt;5);
</pre></div></div></div></td></tr><tr><td rowspan="2"><a href="https://ww2.mathworks.cn/help/stateflow/ref/duration.html"><code>duration</code></a></td><td rowspan="2"><p><code>duration(C)</code></p><p><code>duration(C,sec)</code></p><p><code>duration(C,msec)</code></p><p><code>duration(C,usec)</code></p><div><ul><li><p><code>C</code> 是计算结果为 <code>true</code> 或 <code>false</code> 的表达式。</p></li></ul></div></td><td rowspan="2"><p>返回自条件表达式 <code>C</code> 变为 <code>true</code> 且关联状态激活以来经过的时间长度。</p><p>指定时间，单位为秒 (<code>sec</code>)、毫秒 (<code>msec</code>) 或微秒 (<code>usec</code>)。默认单位为秒。</p><p>如果条件表达式 <code>C</code> 变为 <code>false</code> 或关联状态变为非激活，Stateflow 图会重置 <code>duration</code> 运算符的值。</p><p>MATLAB 中的独立图不支持时序逻辑运算符 <code>duration</code>。</p></td><td><p>当变量 <code>x</code> 大于或等于 0 的时间超过 0.1 秒时，发生转出状态的转移。</p><div><div><div><pre>[duration(x&gt;=0) &gt; 0.1]
</pre></div></div></div></td></tr><tr><td><p>存储自变量 <code>x</code> 大于 5 且状态激活以来经过的毫秒数。</p><div><div><div><pre>en,du:
   y = duration(x&gt;5,msec);
</pre></div></div></div></td></tr></tbody></table>

您可以使用引号将关键字 `'tick'`、`'sec'`、`'msec'` 和 `'usec'` 括起来。例如，`after(5,'tick')` 等效于 `after(5,tick)`。

### 时序逻辑示例

#### 定义时滞

此示例说明如何在连续时间图中定义两个绝对时滞。

![](https://ww2.mathworks.cn/help/stateflow/ug/definingtimedelaysexample_01_zh_CN.png)

图的执行遵循以下步骤：

1.  当图唤醒时，状态 `Input` 先激活。
2.  仿真执行 5.33 毫秒后，会发生从 `Input` 到 `Output` 的转移。
3.  状态 `Input` 变为非激活，状态 `Output` 被激活。
4.  仿真执行 10.5 秒后，会发生从 `Output` 到 `Input` 的转移。
5.  状态 `Output` 变为非激活，状态 `Input` 被激活。

在仿真结束之前，会重复步骤 2 到 5。

如果 Stateflow 图具有离散采样时间，则该图中的任何动作都会在该采样时间的整数倍时发生。例如，如果 Simulink® 求解器使用大小为 0.1 秒的定步长，则从状态 `Input` 到状态 `Output` 的第一个转移发生在 t = 0.1 秒处。发生此行为的原因在于求解器不会在正好 t = 5.33 毫秒时唤醒图。在这种情况下，求解器会在 0.1 秒的整数倍时唤醒图，例如 t = 0.0 秒和 0.1 秒时。

#### 检测已用时间

在此示例中，[Step](https://ww2.mathworks.cn/help/simulink/slref/step.html) (Simulink) 模块向 Stateflow 图提供一个单位阶跃输入。

![](https://ww2.mathworks.cn/help/stateflow/ug/detectingelapsedtimeexample_01_zh_CN.png)

图决定输入 `u` 何时等于 1：

- 如果输入在 t = 2 秒之前等于 1，则发生从 `Start` 到 `Fast` 的转移。
- 如果输入在 t = 2 秒和 t = 5 秒之间等于 1，则发生从 `Start` 到 `Medium` 的转移。
- 如果输入在 t = 5 秒后等于 1，则发生从 `Start` 到 `Slow` 的转移。

#### 在使能子系统中使用绝对时间时序逻辑

您可以在位于条件执行子系统的 Stateflow 图中使用绝对时间时序逻辑。当该子系统被禁用时，该 Stateflow 图变为非活动图，当该 Stateflow 图处于休眠状态时，时序逻辑运算符会暂停。在重新启用该子系统并且该图唤醒前，该运算符不会继续对仿真时间进行计数。

此模型具有一个使能子系统，其**启用时的状态**参数设置为 `held`。

![](https://ww2.mathworks.cn/help/stateflow/ug/enabledsubsystemtemplogicexample_01_zh_CN.png)

该子系统包含一个使用 `after` 运算符触发转移的 Stateflow 图。

![](https://ww2.mathworks.cn/help/stateflow/ug/enabledsubsystemtemplogicexample_02_zh_CN.png)

[信号编辑器](https://ww2.mathworks.cn/help/simulink/slref/signaleditorblock.html) (Simulink) 模块提供具有以下特征的输入信号：

- 信号在 t = 0 时激活子系统。
- 信号在 t = 2 时禁用子系统。
- 信号在 t = 6 时重新激活子系统。

![](https://ww2.mathworks.cn/help/stateflow/ug/enabledsubsystemtemplogicexample_03_zh_CN.png)

下图显示图中经过的总时间。当输入信号在时间 t = 0 激活子系统时，状态 `A` 被激活。当系统被激活时，经过时间会增加。当子系统在 t = 2 禁用时，图进入休眠状态，经过时间停止增加。当 2 < t < 6 时，经过时间因为系统被禁用而冻结在 2 秒。当图在 t = 6 唤醒时，经过时间再次开始增加。

![](https://ww2.mathworks.cn/help/stateflow/ug/xxenabled_subsystem_with_after_elapsed_time_zh_CN.png)

从状态 `A` 到状态 `B` 的转移取决于状态 `A` 处于激活状态时所经过的时间，而不是取决于仿真时间。因此，当状态 `A` 中的经过时间等于 5 秒时，也就是 t = 9 时发生转移。当转移发生时，输出值 `y` 从 0 变为 1。

![](https://ww2.mathworks.cn/help/stateflow/ug/enabledsubsystemtemplogicexample_04_zh_CN.png)

此模型行为只适用于 Enable 模块参数**启用时的状态**设为 `held` 的子系统。如果该参数设为 `reset`，则当重新激活子系统时，Stateflow 图会完全重新初始化。执行默认转移，所有时序逻辑计数器都重置为 0。

### 转移中基于事件的时序逻辑的表示法

在 Simulink 模型的 Stateflow 图中，运算符 `after`、`at` 和 `before` 支持两种不同表示法来表示转移中基于事件的时序逻辑。

- *触发器表示法*定义仅依赖时序逻辑运算符的基础事件的转移。触发器表示法遵循以下语法：

  ```
  temporalLogicOperator(n,E)[C]

  ```

  其中：

  - `temporalLogicOperator` 是一个布尔时序逻辑运算符。
  - `n` 是运算符的出现次数。
  - `E` 是运算符的基础事件。
  - `C` 是一个可选条件表达式。

  当您使用触发器表示法时，仅当图处理基础事件 `E` 的广播时才会发生转移。

- *条件表示法*定义依赖基础事件和非基础事件的转移。条件表示法遵循以下语法：

  ```
  F[temporalLogicOperator(n,E) && C]

  ```

  其中：

  - `temporalLogicOperator` 是一个布尔时序逻辑运算符。
  - `n` 是运算符的出现次数。
  - `E` 是运算符的基础事件。
  - `F` 是可选的非基础事件。
  - `C` 是一个可选条件表达式。

  当您对非基础事件 `F` 使用条件表示法时，仅当图处理 `F` 的广播时才会发生转移。如果忽略非基础事件，则当图处理任何显式或隐式事件时都可以发生转移。

  MATLAB 中的独立图不支持时序逻辑运算符的条件表示法。

例如，以下转移标签使用触发表示法来指示当图处理基础事件 `E` 的广播时，从关联状态激活后 `E` 的第五次广播开始，发生转出关联状态的转移。

而下面的转移标签则使用条件表示法来指示当状态激活后基础事件 `E` 的广播次数至少为五次时发生转出关联状态的转移，即使图没有处理 `E` 的广播也是如此。

**注意**

运算符 `every` 支持触发器表示法和条件表示法。不过，这两种表示法对于此运算符是等效的。转移标签 `every(5,E)` 和 `[every(5,E)]` 均指示当图在状态激活后处理基础事件 `E` 的第 k 次广播时（其中 k 是 5 的倍数），发生转出关联状态的转移。

### 时序逻辑的最佳做法

#### 不要在没有源状态的转移路径中使用时序逻辑

时序逻辑运算符的值取决于其关联状态何时激活。为了确保每个时序逻辑运算符都有唯一关联状态，请仅在以下项中使用这些运算符：

不要对默认转移或图形函数中的转移使用时序逻辑运算符，因为这些转移并非从某一状态发出的转移。

#### 在 Simulink 模型的图中使用绝对时间时序逻辑，而不要使用 `tick`

在 Simulink 模型的图中，使用绝对时间时序逻辑表示的时滞值在语义上独立于模型的采样时间。与之相反，使用基于隐式事件 `tick` 的时序逻辑表示的时滞则取决于 Simulink 求解器使用的步长。

此外，具有输入事件的图支持绝对时间时序逻辑。当 Simulink 模型中的 Stateflow 图有输入事件时，不支持隐式事件 `tick`。

#### 不要在 Simulink 模型的图中用 `at` 来表示绝对时间时序逻辑

在 Simulink 模型的图中，不支持使用 `at` 作为绝对时间时序逻辑运算符。请改用 `after` 运算符。例如，假设您要使用表达式 `at(5.33,sec)` 定义时滞。

![](https://ww2.mathworks.cn/help/stateflow/ug/temp_logic_absolute_time_at_error.png)

为了防止运行时错误，请将转移标签更改为 `after(5.33,sec)`。

![](https://ww2.mathworks.cn/help/stateflow/ug/temp_logic_absolute_time_at_fixed.png)

#### 大参数值的意外结果

在进入具有以下条件的状态后，诸如 `after(x,sec)` 的 Stateflow 绝对时间时序逻辑条件在预期时间的计算结果可能不为 `true`：

- 图具有周期性的离散采样时间。
- 图逻辑使状态保持激活状态超过 `2147418` 个时间单位。时间单位是该状态使用的任何时序逻辑表达式中的最小时间单位。例如，如果状态有两个出向转移，其中一个使用 `after(x,sec)`，另一个使用 `after(x,msec)`，则时间单位为 `msec (milliseconds)`。

通常，当状态中的时间长度大于 `2147418` 个时间单位时，会出现意外结果。但具体情况取决于图的采样时间。

#### 不要在 Simulink 模型的图中用 `every` 来表示绝对时间时序逻辑

在 Simulink 模型的图中，不支持使用 `every` 作为绝对时间时序逻辑运算符。请改为使用通过 `after` 运算符实现的外部自环转移。例如，假设您想要在 Stateflow 图执行期间每 2.5 秒为某个激活状态打印一条状态消息。

![](https://ww2.mathworks.cn/help/stateflow/ug/temp_logic_absolute_time_every_error.png)

为了防止运行时错误，请用外部自环转移替换状态动作。

![](https://ww2.mathworks.cn/help/stateflow/ug/temp_logic_absolute_time_every_fixed.png)

在状态中添加一个历史结点，以使 Stateflow 图在每次自环转移之前记住状态设置。请参阅 [Resume Prior Substate Activity by Using History Junctions](https://ww2.mathworks.cn/help/stateflow/ug/recording-state-activity-with-history-junctions.html)。

#### 在 MATLAB 的独立图中，不要在具有多个源的转移路径中使用时序逻辑

MATLAB 的独立图不支持在具有多个源状态的转移路径中使用时序逻辑运算符。例如，以下独立图会产生运行时错误，因为时序逻辑表达式 `after(10,sec)` 会触发具有多个源状态的转移路径。

![](https://ww2.mathworks.cn/help/stateflow/ug/temp_logic_multiple_source_path_error.png)

要解决此问题，请改为对单独的转移路径（每个转移路径都有单一源状态）使用时序逻辑表达式。

![](https://ww2.mathworks.cn/help/stateflow/ug/temp_logic_multiple_source_path_fixed.png)

#### 避免在 MATLAB 的独立图的转移路径中混合使用绝对时间时序逻辑和条件

在 MATLAB 的独立图中，运算符 `after`、`at` 和 `every` 都会创建 MATLAB `timer` 对象，这种对象可生成隐式事件来唤醒图。将这些运算符与同一个转移路径中的条件结合使用会导致意外行为：

- 如果当 `timer` 唤醒图时转移路径上的条件为 false，则图执行激活状态的 `during` 和 `on` 动作。
- 图不会重置与运算符 `after` 和 `at` 关联的 `timer` 对象。即使转移路径上的条件随后变为 true，如果没有另一个显式或隐式事件来唤醒图，转移也不会发生。

例如，在下图中，从状态 `A` 到状态 `B` 的转移路径上同时使用了绝对时间时序逻辑触发器 `after(1,sec)` 和条件 `[guard]`。从状态 `A` 到状态 `C` 的转移具有绝对时间时序逻辑触发器 `after(5,sec)`。每个转移都与一个生成隐式事件的 `timer` 对象相关联。最初，局部变量 `guard` 为 `false`。

![](https://ww2.mathworks.cn/help/stateflow/ug/temp_logic_unconditional_path.png)

当您执行图时，状态 `A` 被激活。图执行 `entry` 动作，并显示消息 `Hello!`。在 1 秒后，与从 `A` 到 `B` 的转移相关联的 `timer` 唤醒图。由于转移无效，图在 `A` 状态下执行 `during` 动作，并再次显示消息 `Hello!`。

假设在 2 秒后，图接收到输入事件 `E`。图在 `A` 状态下执行 `on` 动作，并将 `guard` 的值更改为 `true`。由于图不会重置与运算符 `after` 相关联的 `timer`，因而在另一个事件唤醒图之前，不会发生从 `A` 到 `B` 的转移。

在 5 秒后，与从 `A` 到 `C` 的转移相关联的 `timer` 唤醒图。由于从 `A` 到 `B` 的转移有效并且具有更高的执行顺序，因而图不会发生到状态 `C` 的转移，也不会显示消息 `Farewell!`。取而代之的是，状态 `B` 被激活，图显示消息 `Good bye!`。

#### 使用带离散采样时间的 Stateflow 图实现更加高效的代码生成

若离散图不在触发子系统或使能子系统内，则生成代码时使用整数计数器来跟踪时间，而不是使用 Simulink 提供的时间。从管理开销和内存方面来看，此行为可以实现更加高效的代码生成，且可以让此代码用在软件在环 (SIL) 和处理器在环 (PIL) 两种仿真模式中。有关详细信息，请参阅 [SIL 和 PIL 仿真](https://ww2.mathworks.cn/help/ecoder/ug/about-sil-and-pil-simulations.html) (Embedded Coder)。

## 另请参阅

[`after`](https://ww2.mathworks.cn/help/stateflow/ref/after.html) | [`at`](https://ww2.mathworks.cn/help/stateflow/ref/at.html) | [`before`](https://ww2.mathworks.cn/help/stateflow/ref/before.html) | [`every`](https://ww2.mathworks.cn/help/stateflow/ref/every.html) | [`temporalCount`](https://ww2.mathworks.cn/help/stateflow/ref/temporalcount.html) | [`elapsed`](https://ww2.mathworks.cn/help/stateflow/ref/elapsed.html) | [`count`](https://ww2.mathworks.cn/help/stateflow/ref/count.html) | [`duration`](https://ww2.mathworks.cn/help/stateflow/ref/duration.html) | [`timer`](https://ww2.mathworks.cn/help/matlab/ref/timer.html) | [Signal Editor](https://ww2.mathworks.cn/help/simulink/slref/signaleditorblock.html) (Simulink) | [Step](https://ww2.mathworks.cn/help/simulink/slref/step.html) (Simulink)

## 相关主题

- [Control Oscillations by Using the duration Operator](https://ww2.mathworks.cn/help/stateflow/ug/simplify-stateflow-chart-using-the-duration-operator.html)
- [Implement an Automatic Transmission Gear System by Using the duration Operator](https://ww2.mathworks.cn/help/stateflow/ug/implement-an-automatic-transmission-gear-system-by-using-the-duration-operator.html)
- [使用 temporalCount 运算符对事件进行计数](https://ww2.mathworks.cn/help/stateflow/ug/counting-events.html)
- [Events in Standalone Charts](https://ww2.mathworks.cn/help/stateflow/ug/how-events-drive-chart-execution.html#mw_5eb311dc-c2d5-43c3-a199-9a5174bded16)
- [Resume Prior Substate Activity by Using History Junctions](https://ww2.mathworks.cn/help/stateflow/ug/recording-state-activity-with-history-junctions.html)

您曾对此示例进行过修改。是否要打开带有您的编辑的示例？

本页内容对您有帮助吗？

Unrated

1 star

2 stars

3 stars

4 stars

5 stars

Unrated 1 star 2 stars 3 stars 4 stars 5 stars

您点击的链接对应于以下 MATLAB 命令：

请在 MATLAB 命令行窗口中直接输入以执行命令。Web 浏览器不支持 MATLAB 命令。

Select a Web Site

Choose a web site to get translated content where available and see local events and offers. Based on your location, we recommend that you select: .

You can also select a web site from the following list:

[Contact your local office](#)
