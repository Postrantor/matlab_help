---
tip: translate by openai@2023-06-21 14:25:06
...
## Stateflow Semantics


In Stateflow®, semantics describe the execution behavior of your Stateflow chart. Various factors can affect how your chart executes, including:

> 在Stateflow®中，语义描述了您的Stateflow图的执行行为。各种因素可能会影响您的图表的执行，包括：

- Explicit or implicit ordering of states
- Transition ordering between states
- Events sent by parallel or superstates


As you build your chart, you expect it to behave in a certain way. By knowing how these factors affect your chart, you can create a chart that behaves with intentional interaction of the graphical and nongraphical objects. Graphical and nongraphical objects are the building blocks for all Stateflow charts.

> 随着你构建图表，你希望它按照一定的方式表现。通过了解这些因素如何影响你的图表，你可以创建一个有意识的交互图形和非图形对象的图表。图形和非图形对象是所有Stateflow图表的基础构建块。

### Stateflow Objects


Stateflow objects are the building blocks of Stateflow charts. These objects can be categorized as either graphical or nongraphical. Graphical objects consist of objects that appear graphically in a chart. Nongraphical objects appear textually in a chart and often refer to data, events, and messages. This chart shows a variety of both graphical and nongraphical objects.

> 状态流对象是状态流图表的构建模块。这些对象可以分为图形对象和非图形对象。图形对象由图表中显示的对象组成。非图形对象以文字形式出现在图表中，通常指数据、事件和消息。本图表显示了各种图形和非图形对象。

![](https://ww2.mathworks.cn/help/stateflow/ug/semantics-hotel-objects.png)


For more information on this example, see [How Stateflow Objects Interact During Execution](https://ww2.mathworks.cn/help/stateflow/ug/how-chart-constructs-interact-during-execution.html).

> 详细了解有关此示例的信息，请参阅[Stateflow 对象在执行期间的交互](https://ww2.mathworks.cn/help/stateflow/ug/how-chart-constructs-interact-during-execution.html)。

### Graphical Objects


To build graphical objects, use the object palette in the Stateflow Editor (see [Stateflow Editor Operations](https://ww2.mathworks.cn/help/stateflow/ug/editor-operations.html)).

> 使用Stateflow编辑器中的对象面板来构建图形对象（参见[Stateflow编辑器操作](https://ww2.mathworks.cn/help/stateflow/ug/editor-operations.html)）。

### Nongraphical Objects


You create nongraphical objects textually in your chart. See [Add Stateflow Data](https://ww2.mathworks.cn/help/stateflow/ug/adding-data.html), [Define Events in a Chart](https://ww2.mathworks.cn/help/stateflow/ug/how-events-work-in-stateflow-charts.html#broq2f9), and [Define Messages in a Chart](https://ww2.mathworks.cn/help/stateflow/ug/message-syntax-in-charts.html#bux6ug1) for details. Examples of nongraphical objects include:

> 您可以文本方式在图表中创建非图形对象。有关详细信息，请参阅[添加Stateflow数据](https://ww2.mathworks.cn/help/stateflow/ug/adding-data.html)，[在图表中定义事件](https://ww2.mathworks.cn/help/stateflow/ug/how-events-work-in-stateflow-charts.html#broq2f9)和[在图表中定义消息](https://ww2.mathworks.cn/help/stateflow/ug/message-syntax-in-charts.html#bux6ug1)。非图形对象的示例包括：

### Enter a Chart


The set of default flow paths execute. If this action does not cause a state entry and the chart has parallel decomposition, then each parallel state becomes active.

> 默认流程路径执行。如果此操作不导致状态转入，并且图表具有并行分解，则每个并行状态都会处于活动状态。


If executing the default flow paths does not cause state entry, a state inconsistency error occurs.

> 如果执行默认流程路径不会导致状态进入，则会发生状态不一致错误。

### Execute an Active Chart


If the chart has no states, each execution is equivalent to initializing a chart. Otherwise, the active children execute. Parallel states execute in the same order that they become active.

> 如果图表没有状态，每次执行相当于初始化图表。否则，活动子状态执行。并行状态按照它们变为活动的顺序执行。

### Enter a State


1.  If the parent of the state is not active, perform steps 1 through 4 for the parent.

> 如果状态的父级不处于活动状态，请对父级执行1至4步操作。

2.  If this state is a parallel state, check that all siblings with a higher (that is, earlier) entry order are active. If not, perform steps 1 through 5 for these states first.

> 如果这个状态是并行状态，请检查具有较高（即较早）输入顺序的所有兄弟状态是否处于活动状态。 如果不是，请先为这些状态执行1至5步。

    Parallel (AND) states are ordered for entry based on whether you use explicit ordering (default) or implicit ordering.


3.  Mark the state active.

> 标记该状态为活动状态。

4.  Perform any entry actions.

> 执行任何入口操作。

5.  Enter children, if needed:

> 输入孩子，如果需要的话。

    1.  If the state contains a history junction and there was an active child of this state at some point after the most recent chart initialization, perform the entry actions for that child. Otherwise, execute the default flow paths for the state.
    2.  If this state has children that are parallel states (parallel decomposition), perform entry steps 1 through 5 for each state according to its entry order.
    3.  If this state has only one child substate, the substate becomes active when the parent becomes active, regardless of whether a default transition is present. Entering the parent state automatically makes the substate active. The presence of any inner transition has no effect on determining the active substate.


6.  If this state is a parallel state, perform all entry steps for the sibling state next in entry order if one exists.

> 如果此状态是并行状态，则如果存在下一个按顺序进入的兄弟状态，则执行所有进入步骤。

7.  If the transition path parent is not the same as the parent of the current state, perform entry steps 6 and 7 for the immediate parent of this state.

> 如果转换路径的父状态不同于当前状态的父状态，则对该状态的直接父状态执行步骤6和7。

### Execute an Active State


1.  The set of outer flow charts execute. If this action causes a state transition, execution stops. This step never occurs for parallel states.

> 集合外流程图执行。如果此操作导致状态转换，则执行停止。 对于并行状态，此步骤永不发生。

2.  During actions and valid on-event actions are performed.

> 在行动期间和有效的事件行动被执行。

3.  The set of inner flow charts execute. If this action does not cause a state transition, the active children execute, starting at step 1. Parallel states execute in the same order that they become active.

> 3. 内部流程图执行。如果此操作不导致状态转换，则活动子状态从步骤1开始执行。并行状态按照它们变为活动状态的顺序执行。

### Exit an Active State


1.  If this is a parallel state, make sure that all sibling states that became active after this state have already become inactive. Otherwise, perform all exiting steps on those sibling states.

> 如果这是一个平行状态，请确保在此状态之后激活的所有兄弟状态已经处于非活动状态。否则，对这些兄弟状态执行所有退出步骤。

2.  If there are any active children, perform the exit steps on these states in the reverse order that they became active.

> 如果有任何活跃的孩子，请按照它们活跃的反序执行退出步骤。

3.  Perform any exit actions.

> 执行任何退出操作。

4.  Mark the state as inactive.

> 标记该状态为非活动状态。

### Execute a Set of Flow Charts


Flow charts execute by starting at step 1 below with a set of starting transitions. The starting transitions for inner flow charts are all transition segments that originate on the respective state and reside entirely within that state. The starting transitions for outer flow charts are all transition segments that originate on the respective state but reside at least partially outside that state. The starting transitions for default flow charts are all default transition segments that have starting points with the same parent:

> 流程图从下面的步骤1开始执行，带有一组起始转换。内部流程图的起始转换是所有起始于该状态且完全位于该状态内的转换段。外部流程图的起始转换是所有起始于该状态但至少部分位于该状态外的转换段。默认流程图的起始转换是所有具有相同父级的起始点的默认转换段。


1.  Ordering of a set of transition segments occurs.

> 排序一组过渡段。

2.  While there are remaining segments to test, testing a segment for validity occurs. If the segment is invalid, testing of the next segment occurs. If the segment is valid, execution depends on the destination:

> 只要还有剩余的段可以测试，就会测试段的有效性。如果段无效，就会测试下一段。如果段有效，执行操作取决于目的地。

    **States**

    1.  Testing of transition segments stops and a transition path forms by backing up and including the transition segment from each preceding junction until the respective starting transition.
    2.  The states that are the immediate children of the parent of the transition path exit.
    3.  The transition action from the final transition path executes.
    4.  The destination state becomes active.

    **Junctions with no outgoing transition segments**

    Testing stops without any state exits or entries.

    **Junctions with outgoing transition segments**

    Step 1 is repeated with the set of outgoing segments from the junction.


3.  After testing all outgoing transition segments at a junction, backtrack the incoming transition segment that brought you to the junction and continue at step 2, starting with the next transition segment after the backtrack segment. The set of flow charts finishes execution when testing of all starting transitions is complete.

> 在一个路口测试完所有出发的过渡段后，回溯进入该路口的过渡段，从回溯段之后的下一个过渡段开始，重新开始步骤2。当所有起始过渡段的测试完成时，流程图集合完成执行。

### Execute an Event Broadcast


Output edge-trigger event execution is equivalent to changing the value of an output data value. All other events have the following execution:

> 输出边缘触发事件的执行等同于改变输出数据值。其他所有事件的执行如下：


1.  If the _receiver_ of the event is active, then it executes. The event _receiver_ is the parent of the event unless a direct event broadcast occurs using the `send()` function.

> 如果事件的接收者是活动的，那么它就会执行。除非使用`send()`函数进行直接事件广播，否则事件接收者就是事件的父节点。

    If the receiver of the event is not active, nothing happens.


2.  After broadcasting the event, the broadcaster performs early return logic based on the type of action statement that caused the event.

> 在广播事件后，广播者根据引发事件的行动声明的类型执行早期返回逻辑。

    <table><colgroup><col width="24%"><col width="76%"></colgroup><thead><tr><th><p>Action Type</p></th><th><p>Early Return Logic</p></th></tr></thead><tbody><tr><td><p>State Entry</p></td><td><p>If the state is no longer active at the end of the event broadcast, any remaining steps in entering a state do not occur.</p></td></tr><tr><td><p>State Exit</p></td><td><p>If the state is no longer active at the end of the event broadcast, any remaining exit actions and steps in state transitioning do not occur.</p></td></tr><tr><td><p>State During</p></td><td><p>If the state is no longer active at the end of the event broadcast, any remaining steps in executing an active state do not occur.</p></td></tr><tr><td><p>Condition</p></td><td><p>If the origin state of the inner or outer flow chart or parent state of the default flow chart is no longer active at the end of the event broadcast, the remaining steps in the execution of the set of flow charts do not occur.</p></td></tr><tr><td><p>Transition</p></td><td><p>If the parent of the transition path is not active or if that parent has an active child, the remaining transition actions and state entry do not occur.</p></td></tr></tbody></table>

## Related Topics

- [Graphical Objects](https://ww2.mathworks.cn/help/stateflow/ug/overview-of-stateflow-objects.html#f18-60789)
- [Nongraphical Objects](https://ww2.mathworks.cn/help/stateflow/ug/overview-of-stateflow-objects.html#f18-8997)
- [Types of Chart Execution](https://ww2.mathworks.cn/help/stateflow/ug/types-of-chart-execution.html)
---
