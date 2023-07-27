---
url: https://www.mathworks.com/help/rtw/ug/use-timers-in-asynchronous-tasks.html
title: Timers in Asynchronous Tasks - MATLAB & Simulink --- 异步任务中的计时器 - MATLAB & Simulink
date: 2023-07-21 10:18:53
tag: 
summary: Maintain absolute and elapsed timing data for blocks that execute in the context of an asynchronous t......
---
## Timers in Asynchronous Tasks

An ISR can set a source for absolute time. This is done with the function `ssSetTimeSource`. The function `ssSetTimeSource` cannot be called before `ssSetOutputPortWidth` is called. If this occurs, the program will come to a halt and generate an error message. `ssSetTimeSource` has the following three options:

*   `SS_TIMESOURCE_SELF`: Each generated ISR maintains its own absolute time counter, which is distinct from a periodic base rate or subrate counters in the system. The counter value and the timer resolution value (specified with the block parameter **Timer resolution (seconds)** of the Async Interrupt block) are used by downstream blocks to determine absolute time values required by block computations.
    
*   `SS_TIMESOURCE_CALLER`: The ISR reads time from a counter maintained by its caller. Time resolution is thus the same as its caller's resolution.
    
*   `SS_TIMESOURCE_BASERATE`: The ISR can read absolute time from the model's periodic base rate. Time resolution is thus the same as its base rate resolution.
    

**Note**

The operating system integration techniques that are demonstrated in this section use blocks that are in the Interrupt Templates block library. The blocks in that library provide starting point examples to help you develop custom blocks for a target environment.

By default, the counter is implemented as a 32-bit unsigned integer member of the `Timing` substructure of the real-time model structure. For a target that supports the `rtModel` data structure, when the time data type is not set by using `ssSetAsyncTimeDataType`, the counter word size is determined by the setting for model configuration parameter [**Application lifespan (days)**](https://www.mathworks.com/help/simulink/gui/application-lifespan-days.html). As an example (from ERT target code),

```
/* Real-time Model Data Structure */
struct _RT_MODEL_elapseTime_exp_Tag {
   const char *errorStatus;
  
  /*
   * Timing:
   * The following substructure contains information regarding
   * the timing information for the model.
   */
  struct {
    uint32_T clockTick1;
    uint32_T clockTick2;
  } Timing;
};

```

The example omits unused fields in the `Timing` data structure (a feature of ERT target code not found in GRT). For a target that supports the `rtModel` data structure, the counter word size is determined by the setting of model configuration parameter [**Application lifespan (days)**](https://www.mathworks.com/help/simulink/gui/application-lifespan-days.html).

By default, the `vxlib1` library blocks for the example RTOS (VxWorks®) set the timer source to `SS_TIMESOURCE_SELF` and update their counters by using the system call `tickGet`. `tickGet` returns a timer value maintained by the RTOS kernel. The maximum word size for the timer is `UINT32`. The following example shows a generated call to `tickGet`.

```
/* VxWorks Interrupt Block: '<Root>/Async Interrupt' */
void isr_num2_vec193(void)
{

 /* Use tickGet() as a portable tick counter example. A much
    higher resolution can be achieved with a hardware counter */
 rtM->Timing.clockTick2 = tickGet();
. . .

```

The `tickGet` call is supplied only as an example. It can (and in many instances should) be replaced by a timing source that has better resolution. If you are implementing a custom asynchronous block for an RTOS other than the example RTOS (VxWorks), you should either generate an equivalent call to the target RTOS, or generate code to read a timer register on the target hardware.

Change the setting for the block parameter **Timer resolution (seconds)** for your Async Interrupt block to match the resolution of timing source for you target computer.

The counter is updated at interrupt level. Its value represents the tick value of the timing source at the most recent execution of the ISR. The rate of this timing source is unrelated to sample rates in the model. In fact, typically it is faster than the model's base rate. Select the timer source and set its rate and resolution based on the expected rate of interrupts to be serviced by the Async Interrupt block.

For an example of timer code generation, see [Async Interrupt Block Implementation](https://www.mathworks.com/help/rtw/ug/create-a-customized-asynchronous-library.html#f24730).

## Related Examples

*   [Generate Interrupt Service Routines](https://www.mathworks.com/help/rtw/ug/generate-interrupt-service-routines.html)
*   [Spawn and Synchronize Execution of RTOS Task](https://www.mathworks.com/help/rtw/ug/spawn-a-wind-river-vxworks-task.html)
*   [Create a Customized Asynchronous Library](https://www.mathworks.com/help/rtw/ug/create-a-customized-asynchronous-library.html)
*   [Import Asynchronous Event Data for Simulation](https://www.mathworks.com/help/rtw/ug/import-asynchronous-event-data-for-simulation.html)

## More About

*   [Absolute and Elapsed Time Computation](https://www.mathworks.com/help/rtw/ug/absolute-and-elapsed-time-computation.html)
*   [Time-Based Scheduling and Code Generation](https://www.mathworks.com/help/rtw/ug/time-based-scheduling-and-code-generation.html)
*   [Asynchronous Events](https://www.mathworks.com/help/rtw/ug/asynchronous-events.html)
*   [Asynchronous Support Limitations](https://www.mathworks.com/help/rtw/ug/asynchronous-support-limitations.html)

How useful was this information?

Unrated

1 star

2 stars

3 stars

4 stars

5 stars

 Unrated  1 star  2 stars  3 stars  4 stars  5 stars

You clicked a link that corresponds to this MATLAB command:

Run the command by entering it in the MATLAB Command Window. Web browsers do not support MATLAB commands.