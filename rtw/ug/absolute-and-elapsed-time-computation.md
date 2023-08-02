---
tip: translate by openai@2023-07-21 09:11:12
url: https://www.mathworks.com/help/rtw/ug/absolute-and-elapsed-time-computation.html
title: Absolute and Elapsed Time Computation
- MATLAB & Simulink
date: 2023-07-21 09:09:56
tag: 
summary: Handle absolute and elapsed time specified for blocks in a model with the code generator.
> 摘要：使用代码生成器处理模型中指定的绝对时间和经过的时间。
---
## Absolute and Elapsed Time Computation

### About Timers

Certain blocks require the value of either _absolute_ time (that is, the time from the start of program execution to the present time) or _elapsed_ time (for example, the time elapsed between two trigger events). Targets that support the real-time model (`rtModel`) data structure provide efficient time computation services to blocks that request absolute or elapsed time. Absolute and elapsed timer features include

> 某些块需要绝对时间的值（即从程序执行开始到当前时间的时间）或经过时间（例如，两个触发事件之间经过的时间）。支持实时模型（`rtModel`）数据结构的目标提供高效的时间计算服务，以满足块对绝对或经过时间的请求。绝对和经过时间计时器功能包括

- Timers are implemented as unsigned integers in generated code.
- In multirate models, at most one timer is allocated per rate. If no blocks executing at a given rate require a timer, a timer is not allocated to that rate. This minimizes memory allocated for timers and significantly reduces overhead involved in maintaining timers.

> 在多率模型中，每个速率最多分配一个计时器。如果没有在给定速率上执行的块需要一个计时器，则不会分配计时器给该速率。这可以最大限度地减少为计时器分配的内存，并显着减少维护计时器所涉及的开销。

- Allocation of elapsed time counters for use of blocks within triggered subsystems is minimized, further reducing memory usage and overhead.

> 分配用于触发子系统中块的经过时间计数器被最小化，进一步减少内存使用和开销。

- S-function and TLC APIs let your S-functions access timers, in simulation and code generation.

> S-函数和 TLC API 允许您的 S-函数在模拟和代码生成中访问定时器。

- The word size of the timers is determined by a user-specified maximum counter value that you set with model configuration parameter [**Application lifespan (days)**](https://www.mathworks.com/help/simulink/gui/application-lifespan-days.html). If you specify this value, timers do not overflow. For more information, see [Control Memory Allocation for Time Counters](https://www.mathworks.com/help/rtw/ug/controlling-memory-allocation-for-time-counters.html).

> 计时器的字大小由用户指定的最大计数值确定，您可以使用模型配置参数[**应用程序寿命（天）**]（[https://www.mathworks.com/help/simulink/gui/application-lifespan-days.html](https://www.mathworks.com/help/simulink/gui/application-lifespan-days.html)）设置它。如果您指定此值，计时器不会溢出。有关详细信息，请参阅[控制时间计数器的内存分配]（[https://www.mathworks.com/help/rtw/ug/controlling-memory-allocation-for-time-counters.html](https://www.mathworks.com/help/rtw/ug/controlling-memory-allocation-for-time-counters.html)）。

See [Absolute Time Limitations](https://www.mathworks.com/help/rtw/ug/absolute-time-limitations.html) for more information about absolute time and the restrictions that it imposes.

> 请参阅[绝对时间限制](https://www.mathworks.com/help/rtw/ug/absolute-time-limitations.html)以了解更多关于绝对时间及其施加的限制的信息。

### Timers for Periodic and Asynchronous Tasks

Timing services provided for blocks execute within _periodic_ tasks (that is, tasks running at the model base rate or subrates).

> 提供定时服务以在*周期性*任务（即以模型基本速率或子速率运行的任务）中执行块。

The code generator also provides timer support for blocks whose execution is _asynchronous_ with respect to the periodic timing source of the model. See the following topics:

> 代码生成器还为与模型的周期定时源异步执行的块提供定时器支持。请参阅以下主题：

### Allocation of Timers

If you create or maintain an S-Function block that requires absolute or elapsed time data, it must register the requirement (see [Access Timers Programmatically](https://www.mathworks.com/help/rtw/ug/access-timers-programmatically.html)). In multirate models, timers are allocated on a per-rate basis. For example, consider a model structured as follows:

> 如果您创建或维护需要绝对或累计时间数据的 S-Function 块，则必须注册该要求（请参阅[编程访问计时器](https://www.mathworks.com/help/rtw/ug/access-timers-programmatically.html)）。在多率模型中，计时器按比率分配。例如，考虑以下结构的模型：

- There are three rates, A, B, and C, in the model.
- No blocks running at rate B require absolute or elapsed time.
- Two blocks running at rate C register a requirement for absolute time.

> 两个以速度 C 运行的块注册了一个绝对时间的要求。

- One block running at rate A registers a requirement for absolute time.

> 一个以 A 的速率运行的块注册了对绝对时间的要求。

In this case, two timers are generated, running at rates A and C respectively. The timing engine updates the timers as the tasks associated with rates A and C execute. Blocks executing at rates A and C obtain time data from the timers associated with rates A and C.

> 在这种情况下，生成了两个计时器，分别以 A 和 C 的速率运行。定时引擎会随着与 A 和 C 速率相关的任务的执行而更新计时器。以 A 和 C 速率执行的块从与 A 和 C 速率相关的计时器获取时间数据。

### Integer Timers in Generated Code

In the generated code, timers for absolute and elapsed time are implemented as unsigned integers. The default size is 64 bits. This is the amount of memory allocated for a timer if you set model configuration parameter [**Application lifespan (days)**](https://www.mathworks.com/help/simulink/gui/application-lifespan-days.html) to `inf`. For an application with a sample rate of 1000 MHz, a 64-bit counter will not overflow for more than 500 years. See [Timers in Asynchronous Tasks](https://www.mathworks.com/help/rtw/ug/use-timers-in-asynchronous-tasks.html) and [Control Memory Allocation for Time Counters](https://www.mathworks.com/help/rtw/ug/controlling-memory-allocation-for-time-counters.html) for more information.

> 在生成的代码中，绝对时间和经过时间的计时器被实现为无符号整数。默认大小为 64 位。如果将模型配置参数[**应用程序生命周期（天）**]（[https://www.mathworks.com/help/simulink/gui/application-lifespan-days.html](https://www.mathworks.com/help/simulink/gui/application-lifespan-days.html)）设置为“无限”，则此为计时器分配的内存量。对于采样率为 1000 MHz 的应用程序，64 位计数器不会溢出超过 500 年。有关更多信息，请参阅[异步任务中的计时器]（[https://www.mathworks.com/help/rtw/ug/use-timers-in-asynchronous-tasks.html](https://www.mathworks.com/help/rtw/ug/use-timers-in-asynchronous-tasks.html)）和[控制时间计数器的内存分配]（[https://www.mathworks.com/help/rtw/ug/controlling-memory-allocation-for-time-counters.html](https://www.mathworks.com/help/rtw/ug/controlling-memory-allocation-for-time-counters.html)）。

### Elapsed Time Counters in Triggered Subsystems

Some blocks, such as the Discrete-Time Integrator block, perform computations requiring the elapsed time (delta T) since the previous block execution. Blocks requiring elapsed time data must register the requirement (see [Access Timers Programmatically](https://www.mathworks.com/help/rtw/ug/access-timers-programmatically.html)). A triggered subsystem then allocates and maintains a single elapsed time counter if required. This timer functions at the subsystem level, not at the individual block level. The timer is generated if the triggered subsystem (or a unconditionally executed subsystem within the triggered subsystem) contains one or more blocks requiring elapsed time data.

> 一些块，如离散时间积分器块，执行需要自上次块执行以来的经过时间（delta T）的计算。需要经过时间数据的块必须注册要求（参见[编程访问定时器](https://www.mathworks.com/help/rtw/ug/access-timers-programmatically.html)）。触发子系统然后分配和维护单个经过时间计数器（如果需要）。此计时器以子系统级别运行，而不是以单个块级别运行。如果触发子系统（或触发子系统中的无条件执行子系统）包含一个或多个需要经过时间数据的块，则会生成计时器。

**Note**

If you are using simplified initialization mode, elapsed time is reset on first execution after becoming enabled, whether or not the subsystem is configured to reset on enable. For more information, see [Underspecified initialization detection](https://www.mathworks.com/help/simulink/gui/underspecified-initialization-detection.html).

> 如果您使用简化的初始化模式，则在首次执行后，无论子系统是否配置为在启用时重置，均会重置已过去的时间。有关更多信息，请参阅[未指定的初始化检测](https://www.mathworks.com/help/simulink/gui/underspecified-initialization-detection.html)。

## Related Topics

- [Access Timers Programmatically](https://www.mathworks.com/help/rtw/ug/access-timers-programmatically.html)

> \*[通过编程访问定时器](https://www.mathworks.com/help/rtw/ug/access-timers-programmatically.html)

- [Generate Code for an Elapsed Time Counter](https://www.mathworks.com/help/rtw/ug/generate-code-for-an-elapsed-time-counter.html)

> 生成经过时间计数器的代码（[https://www.mathworks.com/help/rtw/ug/generate-code-for-an-elapsed-time-counter.html](https://www.mathworks.com/help/rtw/ug/generate-code-for-an-elapsed-time-counter.html)）

- [Optimize Memory Usage for Time Counters](https://www.mathworks.com/help/rtw/ug/timerscounters-for-absolute-and-elapsed-time.html)

> 优化时间计数器的内存使用情况（[https://www.mathworks.com/help/rtw/ug/timerscounters-for-absolute-and-elapsed-time.html](https://www.mathworks.com/help/rtw/ug/timerscounters-for-absolute-and-elapsed-time.html)）

- [Absolute Time Limitations](https://www.mathworks.com/help/rtw/ug/absolute-time-limitations.html)
  > \*[绝对时间限制](https://www.mathworks.com/help/rtw/ug/absolute-time-limitations.html)
  >
