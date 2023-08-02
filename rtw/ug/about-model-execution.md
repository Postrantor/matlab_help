---
tip: translate by openai@2023-07-20 17:09:31
url: https://www.mathworks.com/help/rtw/ug/about-model-execution.html
title: Execution of Code Generated from a Model - MATLAB & Simulink
date: 2023-07-20 11:36:37
tag:
summary: Execute code generated from single-tasking and multitasking models for rapid-prototyping and embedded......
> 摘要：执行从单任务和多任务模型生成的代码，用于快速原型设计和嵌入式......
---
## Execution of Code Generated from a Model

The code generator produces algorithmic code as defined by your model. You can include external (for example, custom or legacy) code in a model by using techniques explained in [Choose an External Code Integration Workflow](https://www.mathworks.com/help/rtw/ug/choose-an-external-code-integration-approach.html).

> 代码生成器根据您的模型生成算法代码。您可以通过使用[选择外部代码集成工作流程]中解释的技术，将外部(例如自定义或遗留)代码包含在模型中。

The code generator also provides an interface that executes the generated model code. The interface and model code are compiled together to create an executable program. The next figure shows a high-level object-oriented view of the executable.

> 代码生成器还提供一个接口来执行生成的模型代码。接口和模型代码一起编译，形成一个可执行程序。下图显示了可执行程序的高级面向对象视图。

**The Object-Oriented View of a Real-Time Program**

![](https://www.mathworks.com/help/rtw/ug/object_oriented_view_real_time_program.png)

In general, the conceptual design of the model execution driver does not change between the rapid prototyping and embedded style of generated code. The following sections describe model execution for single-tasking and multitasking environments both for simulation (non-real-time) and for real time. For most model code, the multitasking environment provides the most efficient model execution (that is, fastest sample rate).

> 一般来说，快速原型和嵌入式生成代码之间，模型执行驱动器的概念设计不会发生变化。以下部分描述单任务和多任务环境下的模型执行，无论是模拟(非实时)还是实时。对于大多数模型代码，多任务环境提供了最高效的模型执行(即最快的采样率)。

The following concepts are useful in describing how model code executes.

- **Initialization**: ``_`model`__initialize`` initializes the interface code and the model code.

> - **初始化**：`_` model `__initialize` 初始化接口代码和模型代码。

- **ModelOutputs**: Calls blocks in your model that have a sample hit at the current time and has them produce their output. ``_`model`__output`` can be done in major or minor time steps. In major time steps, the output is a given simulation time step. In minor time steps, the interface integrates the derivatives to update the continuous states.

> - **模型输出**：在当前时间调用模型中具有样本命中的块，并使它们产生输出。 ``_`model`__output`` 可以在主要或次要时间步骤中完成。 在主要时间步骤中，输出是给定的仿真时间步长。 在次要时间步骤中，接口集成导数以更新连续状态。

- **ModelUpdate**: ``_`model`__update`` calls blocks that have a sample hit at the current point in time and has them update their discrete states or similar type objects.

> `- **模型更新**：`\_`模型`\_\_update` 调用在当前时间点有样本命中的块，并使它们更新其离散状态或类似类型的对象。

- **ModelDerivatives**: Calls blocks in your model that have continuous states and has them update their derivatives. ``_`model`__derivatives`` is only called in minor time steps.

> `模型导数：调用模型中具有连续状态的块，并使其更新其导数。`**model `**derivatives` 仅在小时间步中调用。

- **ModelTerminate**: ``_`model`__terminate`` terminates the program if it is designed to run for a finite time. It destroys the real-time model data structure, deallocates memory, and can write data to a file.

> - **ModelTerminate**：`model`\_\_terminate` 终止程序，如果它设计为运行有限时间。它销毁实时模型数据结构，释放内存，并可以将数据写入文件。

### Program Execution

A real-time program cannot require 100% of the CPU time. This requirement provides an opportunity to run background tasks during the free time.

Background tasks include operations such as writing data to a buffer or file, allowing access to program data by third-party data monitoring tools, or updating program parameters.

It is important, however, that the program be able to preempt the background task so the model code can execute in real time.

The way the program manages tasks depends on capabilities of the environment in which it operates.

> <font color=Red><b>**一个实时程序不能要求 100％的 CPU 时间**。这个要求提供了在空闲时间运行后台任务的机会。</b></font>
> 背景任务包括诸如将数据写入缓冲区或文件、允许第三方数据监控工具访问程序数据或更新程序参数等操作。
> 重要的是，**程序能够抢占后台任务**，以便模型代码能够实时执行。
> 程序管理任务的方式取决于它运行的环境的能力。

### Program Timing

Real-time programs require careful timing of the task invocations (either by using an interrupt or a real-time operating system tasking primitive) so that the model code executes to completion before another task invocation occurs. The timing includes time to read and write data to and from external hardware.

> 实时程序需要仔细调度任务调用(通过使用中断或实时操作系统任务原语)，以便模型代码在另一个任务调用发生之前完成执行。时间调度还包括从外部硬件读写数据的时间。

The next figure illustrates interrupt timing.

**Task Timing**

![](https://www.mathworks.com/help/rtw/ug/task_timing.png)

The sample interval must be long enough to allow model code execution between task invocations.

> 样本间隔必须足够长，以便在任务调用之间执行模型代码。

In the figure above, the time between two adjacent vertical arrows is the sample interval. The empty boxes in the upper diagram show an example of a program that can complete one step within the interval and still allow time for the background task. The gray box in the lower diagram indicates what happens if the sample interval is too short. Another task invocation occurs before the task is complete. Such timing results in an execution error.

> 在上图中，两个相邻的垂直箭头之间的时间是采样间隔。上图中的空框表示可以在间隔内完成一个步骤，并且仍然有时间执行后台任务的程序的示例。下图中的灰色框表示，如果采样间隔太短，会发生什么情况。另一个任务调用发生在任务完成之前。这种时序会导致执行错误。

If the real-time program is designed to run forever (that is, the final time is 0 or infinite so that the `while` loop never exits), then the shutdown code does not execute.

> 如果实时程序设计为永久运行(即最终时间为 0 或无限，因此 `while` 循环永不退出)，则关机代码不会执行。

For more information on how the timing engine works, see [Absolute and Elapsed Time Computation](https://www.mathworks.com/help/rtw/ug/absolute-and-elapsed-time-computation.html).

> 要了解更多关于时序引擎如何工作的信息，请参阅[绝对时间和累计时间计算]。

### External Mode Communication

External mode allows communication between the Simulink® block diagram and the standalone program that is built from the generated code. In this mode, the real-time program functions as an interprocess communication server, responding to requests from the Simulink engine.

> 外部模式允许 Simulink® 模块图与由生成的代码构建的独立程序之间进行通信。在此模式下，实时程序充当进程间通信服务器，响应 Simulink 引擎的请求。

### Data Logging in Single-Tasking and Multitasking Model Execution

[Configure Model for Debugging](https://www.mathworks.com/help/rtw/ug/instrumentation-for-debugging.html) explains how you can save system states, outputs, and time to a MAT-file at the completion of the model execution. The `LogTXY` function, which performs data logging, operates differently in single-tasking and multitasking environments.

> [配置调试模型](https://www.mathworks.com/help/rtw/ug/instrumentation-for-debugging.html)解释了如何在模型执行完成后将系统状态、输出和时间保存到 MAT 文件中。用于数据记录的 `LogTXY` 函数在单任务和多任务环境中的操作方式不同。

If you examine how `LogTXY` is called in the single-tasking and multitasking environments, notice that for single-tasking `LogTXY` is called after `ModelOutputs`. During this `ModelOutputs` call, blocks that have a hit at time _t_ execute, whereas in multitasking, `LogTXY` is called after `ModelOutputs(tid=0)`, which executes only the blocks that have a hit at time _t_ and that have a task identifier of 0. This results in differences in the logged values between single-tasking and multitasking logging. Specifically, consider a model with two sample times, the faster sample time having a period of 1.0 second and the slower sample time having a period of 10.0 seconds. At time t = k\*10, k=0,1,2... both the fast (`tid=0`) and slow (`tid=1`) blocks execute. When executing in multitasking mode, when `LogTXY` is called, the slow blocks execute, but the previous value is logged, whereas in single-tasking the current value is logged.

> 如果您检查单任务和多任务环境中 LogTXY 的调用情况，您会注意到，在单任务中，LogTXY 是在 ModelOutputs 之后调用的。在此 ModelOutputs 调用期间，在时间 t 处有命中的块将执行，而在多任务中，LogTXY 是在 ModelOutputs(tid = 0)之后调用的，它只执行具有时间 t 的命中和任务标识符 0 的块。这导致了单任务和多任务记录之间记录值的差异。具体来说，考虑一个具有两个采样时间的模型，其中较快的采样时间具有 1.0 秒的周期，较慢的采样时间具有 10.0 秒的周期。在 t = k \* 10，k = 0,1,2...时，快速(tid = 0)和慢速(tid = 1)块都会执行。在多任务模式下执行时，当调用 LogTXY 时，慢速块会执行，但会记录上一个值，而在单任务中会记录当前值。

Another difference occurs when logging data in an enabled subsystem. Consider an enabled subsystem that has a slow signal driving the enable port and fast blocks within the enabled subsystem. In this case, the evaluation of the enable signal occurs in a slow task, and the fast blocks see a delay of one sample period; thus the logged values will show these differences.

> 另一个差异发生在启用子系统中记录数据时。考虑一个启用的子系统，它具有驱动启用端口的慢信号和子系统内的快速块。在这种情况下，启用信号的评估发生在一个缓慢的任务中，快速块看到一个采样周期的延迟；因此，记录的值将显示这些差异。

To summarize differences in logged data between single-tasking and multitasking, differences will be seen when

> 总结单任务和多任务的登录数据之间的差异，当进行比较时会看到差异。

- A root outport block has a sample time that is slower than the fastest sample time
- A block with states has a sample time that is slower than the fastest sample time
- A block in an enabled subsystem where the signal driving the enable port is slower than the rate of the blocks in the enabled subsystem

> 在启用的子系统中，驱动启用端口的信号比启用子系统中的块的速率慢的块。

For the first two cases, even though the logged values are different between single-tasking and multitasking, the model results are not different. The only real difference is where (at what point in time) the logging is done. The third (enabled subsystem) case results in a delay that can be seen in a real-time environment.

> 对于前两种情况，即使单任务和多任务之间的记录值不同，模型结果也没有不同。唯一真正的不同之处在于(在何时)记录被完成。第三种(启用子系统)情况会导致在实时环境中可以看到的延迟。

### Non-Real-Time Single-Tasking Systems

This pseudocode shows the execution of a model for a non-real-time single-tasking system.

> 这段伪代码展示了一个非实时单任务系统的执行模型。

```
main()
{
  Initialization
  While (time < final time)
    ModelOutputs     -- Major time step.
    LogTXY           -- Log time, states and root outports.
    ModelUpdate      -- Major time step.
    Integrate        -- Integration in minor time step for
                     -- models with continuous states.
      ModelDerivatives
      Do 0 or more
        ModelOutputs
        ModelDerivatives
      EndDo -- Number of iterations depends upon the solver
      Integrate derivatives to update continuous states.
    EndIntegrate
  EndWhile
  Termination
}
```

The initialization phase begins first. This consists of initializing model states and setting up the execution engine. The model then executes, one step at a time. First `ModelOutputs` executes at time _t_, then the workspace I/O data is logged, and then `ModelUpdate` updates the discrete states. Next, if your model has continuous states, `ModelDerivatives` integrates the continuous states' derivatives to generate the states for time tnew=t+h, where _h_ is the step size. Time then moves forward to tnew and the process repeats.

> 初始化阶段首先开始。这包括初始化模型状态并设置执行引擎。然后模型按步骤执行，首先在时间 *t* 上执行 `ModelOutputs`，然后记录工作空间 I/O 数据，然后 `ModelUpdate` 更新离散状态。接下来，如果您的模型具有连续状态，则 `ModelDerivatives` 将连续状态的导数集成以生成时间 tnew = t + h 的状态，其中 *h* 是步长。然后时间前进到 tnew，并重复该过程。

During the `ModelOutputs` and `ModelUpdate` phases of model execution, only blocks that reach the current point in time execute.

> 在模型执行的 `ModelOutputs` 和 `ModelUpdate` 阶段，只有到达当前时间点的块才会执行。

### Non-Real-Time Multitasking Systems

This pseudocode shows the execution of a model for a non-real-time multitasking system.

> 这段伪代码展示了一个非实时多任务系统的执行模型。

```
main()
{
  Initialization
  While (time < final time)
    ModelOutputs(tid=0)   -- Major time step.
    LogTXY                -- Log time, states, and root
                          -- outports.
    ModelUpdate(tid=0)    -- Major time step.
    Integrate       -- Integration in minor time step for
                    -- models with continuous states.
      ModelDerivatives
      Do 0 or more
        ModelOutputs(tid=0)
        ModelDerivatives
      EndDo (Number of iterations depends upon the solver.)
      Integrate derivatives to update continuous states.
    EndIntegrate
    For i=1:NumTids
      ModelOutputs(tid=i) -- Major time step.
      ModelUpdate(tid=i)  -- Major time step.
    EndFor
  EndWhile
  Termination
  }
```

Multitasking operation is more complex than single-tasking execution because the output and update functions are subdivided by the _task identifier_ (`tid`) that is passed into these functions. This allows for multiple invocations of these functions with different task identifiers using overlapped interrupts, or for multiple tasks when using a real-time operating system. In simulation, multiple tasks are emulated by executing the code in the order that would occur if preemption did not exist in a real-time system.

> 多任务操作比单任务执行更复杂，因为输出和更新函数被传入这些函数的任务标识符(`tid`)划分。这允许使用重叠中断或使用实时操作系统时多个任务的多次调用这些函数。在模拟中，多个任务通过按照在实时系统中不存在抢占时的顺序执行代码来模拟。

Multitasking execution assumes that task rates are multiples of the base rate. The Simulink product enforces this when you create a fixed-step multitasking model. The multitasking execution loop is very similar to that of single-tasking, except for the use of the task identifier (`tid`) argument to `ModelOutputs` and `ModelUpdate`.

> 多任务执行假定任务速率是基本速率的倍数。当您创建固定步骤多任务模型时，Simulink 产品强制执行此操作。多任务执行循环与单任务执行循环非常相似，只是使用任务标识符(`tid`)参数调用 `ModelOutputs` 和 `ModelUpdate`。

You cannot use `tid` values from code generated by a target file and not by Simulink Coder™. Simulink Coder tracks the use of `tid` when generating code for a specific subsystem or function type. When you generate code in a target file, this argument cannot be tracked because the scope does not have subsystem or function type. Therefore, `tid` becomes an undefined variable and your target file fails to compile.

> 你不能使用由目标文件生成的代码中的 `tid` 值，而不是由 Simulink Coder™ 生成的。 Simulink Coder 跟踪在为特定子系统或函数类型生成代码时使用 `tid` 的情况。 当您在目标文件中生成代码时，此参数无法跟踪，因为范围没有子系统或函数类型。 因此，`tid` 变成一个未定义的变量，您的目标文件无法编译。

### Real-Time Single-Tasking Systems

This pseudocode shows the execution of a model in a real-time single-tasking system where the model is run at interrupt level.

> 这段伪代码展示了在实时单任务系统中模型的执行，其中模型运行在中断级别。

```
rtOneStep()
{
  Check for interrupt overflow
  Enable "rtOneStep" interrupt
  ModelOutputs    -- Major time step.
  LogTXY          -- Log time, states and root outports.
  ModelUpdate     -- Major time step.
  Integrate       -- Integration in minor time step for models
                  -- with continuous states.
     ModelDerivatives
     Do 0 or more
       ModelOutputs
       ModelDerivatives
     EndDo (Number of iterations depends upon the solver.)
     Integrate derivatives to update continuous states.
  EndIntegrate
}

main()
{
  Initialization (including installation of rtOneStep as an
  interrupt service routine, ISR, for a real-time clock).
  While(time < final time)
    Background task.
  EndWhile
  Mask interrupts (Disable rtOneStep from executing.)
  Complete any background tasks.
  Shutdown
}
```

Real-time single-tasking execution is very similar to non-real-time single-tasking execution, except that instead of free-running the code, the `rt_OneStep` function is driven by a periodic timer interrupt.

> 实时单任务执行与非实时单任务执行非常相似，只是**不是自由运行代码，而是由定期的定时器中断驱动 `rt_OneStep` 函数**。

At the interval specified by the program's base sample rate, the interrupt service routine (ISR) preempts the background task to execute the model code. The base sample rate is the fastest in the model. If the model has continuous blocks, then the integration step size determines the base sample rate.

> 在程序的基本采样率所指定的间隔，中断服务例程(ISR)优先执行模型代码，而背景任务被抢占。基本采样率是模型中最快的。如果模型有连续块，那么集成步长决定了基本采样率。

For example, if the model code is a controller operating at 100 Hz, then every 0.01 seconds the background task is interrupted. During this interrupt, the controller reads its inputs from the analog-to-digital converter (ADC), calculates its outputs, writes these outputs to the digital-to-analog converter (DAC), and updates its states. Program control then returns to the background task. These steps must occur before the next interrupt.

> 例如，如果模型代码是以 100 Hz 操作的控制器，那么每 0.01 秒背景任务就会被中断。在此中断期间，控制器从模拟数字转换器(ADC)读取其输入，计算其输出，将这些输出写入数字模拟转换器(DAC)，并更新其状态。然后程序控制返回到背景任务。在下一次中断之前，必须先执行这些步骤。

### Real-Time Multitasking Systems

This pseudocode shows how a model executes in a real-time multitasking system where the model is run at interrupt level.

> 这段伪代码展示了一个模型如何在实时多任务系统中以中断级别运行。

```
rtOneStep()
{
  Check for interrupt overflow
  Enable "rtOneStep" interrupt
  ModelOutputs(tid=0)     -- Major time step.
  LogTXY                  -- Log time, states and root outports.
  ModelUpdate(tid=0)      -- Major time step.
  Integrate               -- Integration in minor time step for
                          -- models with continuous states.
     ModelDerivatives
     Do 0 or more
       ModelOutputs(tid=0)
       ModelDerivatives
     EndDo (Number of iterations depends upon the solver.)
     Integrate derivatives and update continuous states.
  EndIntegrate
  For i=1:NumTasks
    If (hit in task i)
      ModelOutputs(tid=i)
      ModelUpdate(tid=i)
    EndIf
  EndFor
}

main()
{
  Initialization (including installation of rtOneStep as an
    interrupt service routine, ISR, for a real-time clock).
  While(time < final time)
    Background task.
  EndWhile
  Mask interrupts (Disable rtOneStep from executing.)
  Complete any background tasks.
  Shutdown
}


```

Running models at interrupt level in a real-time multitasking environment is very similar to the previous single-tasking environment, except that overlapped interrupts are employed for concurrent execution of the tasks.

> 在实时多任务环境中以中断级别运行模型与先前的单任务环境非常相似，除了采用重叠中断来同时执行任务。

The execution of a model in a single-tasking or multitasking environment when using real-time operating system tasking primitives is very similar to the interrupt-level examples discussed above. The pseudocode below is for a single-tasking model using real-time tasking primitives.

> 在使用实时操作系统任务原语的单任务或多任务环境中执行模型与上述中讨论的中断级别示例非常相似。下面的伪代码是用于使用实时任务原语的单任务模型。

```
tSingleRate()
{
  MainLoop:
    If clockSem already "given", then error out due to overflow.
    Wait on clockSem
    ModelOutputs            -- Major time step.
    LogTXY                  -- Log time, states and root
                            -- outports
    ModelUpdate             -- Major time step
    Integrate               -- Integration in minor time step
                            -- for models with continuous
                            -- states.
      ModelDeriviatives
      Do 0 or more
        ModelOutputs
        ModelDerivatives
      EndDo (Number of iterations depends upon the solver.)
      Integrate derivatives to update continuous states.
    EndIntegrate
  EndMainLoop
}

main()
{
  Initialization
  Start/spawn task "tSingleRate".
  Start clock that does a "semGive" on a clockSem semaphore.
  Wait on "model-running" semaphore.
  Shutdown
}


```

In this single-tasking environment, the model executes as real-time operating system tasking primitives. In this environment, create a single task (`tSingleRate`) to run the model code. This task is invoked when a clock tick occurs. The clock tick gives a `clockSem` (clock semaphore) to the model task (`tSingleRate`). The model task waits for the semaphore before executing. The clock ticks occur at the fundamental step size (base rate) for your model.

> 在这种单任务环境中，模型执行作为实时操作系统任务原语。在这个环境中，创建一个单任务(`tSingleRate`)来运行模型代码。当时钟滴答发生时，就会调用这个任务。时钟滴答给模型任务(`tSingleRate`)一个 `clockSem`(时钟信号量)。模型任务在执行之前等待信号量。时钟滴答以模型的基本步长(基率)发生。

### Multitasking Systems Using Real-Time Tasking Primitives

This pseudocode is for a multitasking model using real-time tasking primitives.

> 这段伪代码是用实时任务原语实现的多任务模型。

```
tSubRate(subTaskSem,i)
{
  Loop:
    Wait on semaphore subTaskSem.
    ModelOutputs(tid=i)
    ModelUpdate(tid=i)
  EndLoop
}
tBaseRate()
{
  MainLoop:
    If clockSem already "given", then error out due to overflow.
    Wait on clockSem
    For i=1:NumTasks
      If (hit in task i)
        If task i is currently executing, then error out due to
          overflow.
        Do a "semGive" on subTaskSem for task i.
      EndIf
    EndFor
    ModelOutputs(tid=0)    -- major time step.
    LogTXY                 -- Log time, states and root outports.
    ModelUpdate(tid=0)     -- major time step.
    Loop:                  -- Integration in minor time step for
                           -- models with continuous states.
      ModelDeriviatives
      Do 0 or more
        ModelOutputs(tid=0)
        ModelDerivatives
      EndDo (number of iterations depends upon the solver).
      Integrate derivatives to update continuous states.
    EndLoop
  EndMainLoop
}
main()
{
  Initialization
  Start/spawn task "tSubRate".
  Start/spawn task "tBaseRate".

  Start clock that does a "semGive" on a clockSem semaphore.
  Wait on "model-running" semaphore.
  Shutdown
}


```

In this multitasking environment, the model is executed using real-time operating system tasking primitives. Such environments require several model tasks (`tBaseRate` and several `tSubRate` tasks) to run the model code. The base rate task (`tBaseRate`) has a higher priority than the subrate tasks. The subrate task for `tid=1` has a higher priority than the subrate task for `tid=2`, and so on. The base rate task is invoked when a clock tick occurs. The clock tick gives a `clockSem` to `tBaseRate`. The first thing `tBaseRate` does is give semaphores to the subtasks that have a hit at the current point in time. Because the base rate task has a higher priority, it continues to execute. Next it executes the fastest task (`tid=0`), consisting of blocks in your model that have the fastest sample time. After this execution, it resumes waiting for the clock semaphore. The clock ticks are configured to occur at the fundamental step size for your model.

> 在这种多任务环境中，模型使用实时操作系统任务原语来执行。这种环境需要多个模型任务(`tBaseRate` 和多个 `tSubRate` 任务)来运行模型代码。基本速率任务(`tBaseRate`)的优先级高于子速率任务。`tid = 1` 的子速率任务的优先级高于 `tid = 2` 的子速率任务，依此类推。当发生时钟滴答时，会调用基本速率任务。`tBaseRate` 首先会给当前时间点有命中的子任务发出信号量。由于基本速率任务的优先级更高，它会继续执行。接下来，它会执行最快的任务(`tid = 0`)，其中包含模型中具有最快采样时间的块。在此执行之后，它会恢复等待时钟信号量。为您的模型配置的时钟滴答将发生在基本步长上。

### Rapid Prototyping and Embedded Model Execution Differences

The rapid prototyping program framework provides a common application programming interface (API) that does not change between model definitions.

> 快速原型程序框架提供了一个不随模型定义而变化的通用应用程序编程接口(API)。

The Embedded Coder® product provides a different framework called the embedded program framework. The embedded program framework provides an optimized API that is tailored to your model. When you use the embedded style of generated code, you are modeling how you would like your code to execute in your embedded system. Therefore, the definitions defined in your model should be specific to your embedded targets. Items such as the model name, parameter, and signal storage class are included as part of the API for the embedded style of code.

> 产品 Embedded Coder® 提供了一个不同的框架，称为嵌入式程序框架。嵌入式程序框架提供了一个针对您的模型优化的 API。当您使用生成的嵌入式代码样式时，您正在建模您希望在嵌入式系统中执行的代码。因此，在模型中定义的定义应该针对您的嵌入式目标。模型名称、参数和信号存储类别等项目作为嵌入式代码样式的 API 的一部分包括在内。

One major difference between the rapid prototyping and embedded style of generated code is that the latter contains fewer entry-point functions. The embedded style of code can be configured to have only one function, ``_`model`__step``.

> 一个主要的区别在于快速原型和嵌入式风格的生成代码，后者包含较少的入口函数。嵌入式风格的代码可以配置为只有一个函数，"model_step"。

Thus, model execution code eliminates `Loop...EndLoop` statements and groups `ModelOutputs`, `LogTXY`, and `ModelUpdate` into a single statement, ``_`model`__step``.

> 因此，模型执行代码消除了 `Loop...EndLoop` 语句，并将 `ModelOutputs`，`LogTXY` 和 `ModelUpdate` 组合成单个语句 `model_step`。

For more information about how generated embedded code executes, see [Configure Generated C Function Interface for Model Entry-Point Functions](https://www.mathworks.com/help/rtw/ug/configure-c-code-generation-for-model-entry-point-functions.html).

> 要了解有关生成嵌入式代码如何执行的更多信息，请参阅[配置模型入口点函数的生成的 C 函数接口](https://www.mathworks.com/help/rtw/ug/configure-c-code-generation-for-model-entry-point-functions.html)。

## Related Topics

- [Time-Based Scheduling and Code Generation](https://www.mathworks.com/help/ecoder/ug/time-based-scheduling-and-code-generation.html) (Embedded Coder)
- [Sample Times in Subsystems](https://www.mathworks.com/help/simulink/ug/managing-sample-times-in-subsystems.html)
- [Sample Times in Systems](https://www.mathworks.com/help/simulink/ug/managing-sample-times-in-systems.html)
- [Time-Based Scheduling Example Models](https://www.mathworks.com/help/ecoder/ug/time-based-scheduling-example-models-1.html) (Embedded Coder)

How useful was this information?

Unrated

1 star

2 stars

3 stars

4 stars

5 stars

Unrated 1 star 2 stars 3 stars 4 stars 5 stars

You clicked a link that corresponds to this MATLAB command:

Run the command by entering it in the MATLAB Command Window. Web browsers do not support MATLAB commands.

> 在 MATLAB 命令窗口中输入命令来运行。网络浏览器不支持 MATLAB 命令。

Select a Web Site

Choose a web site to get translated content where available and see local events and offers. Based on your location, we recommend that you select: .

> 根据您的位置，我们建议您选择：选择一个网站以获取可用的翻译内容，并查看当地活动和优惠。

You can also select a web site from the following list:

[Contact your local office](#)
