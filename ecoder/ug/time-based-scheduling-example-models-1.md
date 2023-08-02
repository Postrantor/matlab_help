---
url: https://www.mathworks.com/help/ecoder/ug/time-based-scheduling-example-models-1.html
title: Time-Based Scheduling Example Models - MATLAB & Simulink --- 基于时间的调度示例模型 - MATLAB & Simulink
date: 2023-07-21 10:14:41
tag: 
summary: Models that show time-based scheduling scenarios.
---
## Time-Based Scheduling Example Models

### Optimize Memory Usage for Time Counters

This example shows how to optimize the amount of memory that the code generator allocates for time counters. The example optimizes the memory that stores elapsed time, the interval of time between two events.

The code generator represents time counters as unsigned integers. The word size of time counters is based on the setting of the model configuration parameter **Application lifespan (days)**, which specifies the expected maximum duration of time the application runs. You can use this parameter to prevent time counter overflows. The default size is 64 bits.

The number of bits that a time counter uses depends on the setting of the **Application lifespan (days)** parameter. For example, if a time counter increments at a rate of 1 kHz, to avoid an overflow, the counter has the following number of bits:

* Lifespan < 0.25 sec: 8 bits
* Lifespan < 1 min: 16 bits
* Lifespan < 49 days: 32 bits
* Lifespan > 50 days: 64 bits

A 64-bit time counter does not overflow for 590 million years.

**Open Example Model**

Open the example model `rtwdemo_abstime`.

![](https://www.mathworks.com/help/examples/simulinkcoder/win64/TimersCountersAbsAndElapsedTimeExample_01.png)

The model consists of three subsystems `SS1`, `SS2`, and `SS3`. Open the Model Configuration Parameters dialog box. On the **Math and Data Types** pane, the **Application lifespan (days)** parameter is set to the default, which is `inf`.

The three subsystems contain a discrete-time integrator that requires elapsed time as input to compute its output value. The subsystems vary as follows:

* SS1 - Clocked at 1 kHz. Does not require a time counter. **Sample time type** parameter for trigger port is set to `periodic`. Elapsed time is inlined as 0.001.
* SS2 - Clocked at 100 Hz. Requires a time counter. Based on a lifespan of 1 day, a 32-bit counter stores the elapsed time.
* SS3 - Clocked at 0.5 Hz. Requires a time counter. Based on a lifespan of 1 day, a 16-bit counter stores the elapsed time.

**Simulate the Model**

Simulate the model. By default, the model is configured to show sample times in different colors. Discrete sample times for the three subsystems appear red, green, and blue. Triggered subsystems are blue-green.

**Generate Code and Report**

1. Create a temporary folder for the build and inspection process.
2. Configure the model for the code generator to use the GRT system target file and a lifespan of `inf` days.
3. Build the model.

```
### Starting build procedure for: rtwdemo_abstime
### Successful completion of build procedure for: rtwdemo_abstime

Build Summary

Top model targets built:

Model            Action                        Rebuild Reason                                    
=================================================================================================
rtwdemo_abstime  Code generated and compiled.  Code generation information file does not exist.  

1 of 1 models built (0 models already up to date)
Build duration: 0h 0m 18.508s


```

**Review Generated Code**

Open the generated source file `rtwdemo_abstime.h`.

```
struct tag_RTM_rtwdemo_abstime_T {
  const char_T *errorStatus;

  /*
   * Timing:
   * The following substructure contains information regarding
   * the timing information for the model.
   */
  struct {
    uint32_T clockTick1;
    uint32_T clockTickH1;
    uint32_T clockTick2;
    uint32_T clockTickH2;
    struct {
      uint16_T TID[3];
      uint16_T cLimit[3];
    } TaskCounters;
  } Timing;
};

/* Block states (default storage) */
extern DW_rtwdemo_abstime_T rtwdemo_abstime_DW;

/* Zero-crossing (trigger) state */
extern PrevZCX_rtwdemo_abstime_T rtwdemo_abstime_PrevZCX;

/* External inputs (root inport signals with default storage) */
extern ExtU_rtwdemo_abstime_T rtwdemo_abstime_U;

/* External outputs (root outports fed by signals with default storage) */
extern ExtY_rtwdemo_abstime_T rtwdemo_abstime_Y;

/* Model entry point functions */
extern void rtwdemo_abstime_initialize(void);
extern void rtwdemo_abstime_step0(void);
extern void rtwdemo_abstime_step1(void);
extern void rtwdemo_abstime_step2(void);
extern void rtwdemo_abstime_terminate(void);

/* Real-time Model object */
extern RT_MODEL_rtwdemo_abstime_T *const rtwdemo_abstime_M;

/*-
 * The generated code includes comments that allow you to trace directly
 * back to the appropriate location in the model.  The basic format
 * is <system>/block_name, where system is the system number (uniquely
 * assigned by Simulink) and block_name is the name of the block.
 *
 * Use the MATLAB hilite_system command to trace the generated code back
 * to the model.  For example,
 *
 * hilite_system('<S3>')    - opens system 3
 * hilite_system('<S3>/Kp') - opens and selects block Kp which resides in S3
 *
 * Here is the system hierarchy for this model
 *
 * '<Root>' : 'rtwdemo_abstime'
 * '<S1>'   : 'rtwdemo_abstime/SS1'
 * '<S2>'   : 'rtwdemo_abstime/SS2'
 * '<S3>'   : 'rtwdemo_abstime/SS3'
 */
#endif                                 /* RTW_HEADER_rtwdemo_abstime_h_ */


```

Four 32-bit unsigned integers, `clockTick1` , `clockTickH1` , `clockTick2` , and `clockTickH2` are counters for storing the elapsed time of subsystems `SS2` and `SS3`.

**Enable Optimization and Regenerate Code**

1. Reconfigure the model to set the lifespan to 1 day.
2. Build the model.

```
### Starting build procedure for: rtwdemo_abstime
### Successful completion of build procedure for: rtwdemo_abstime

Build Summary

Top model targets built:

Model            Action                        Rebuild Reason                 
==============================================================================
rtwdemo_abstime  Code generated and compiled.  Incremental checksum changed.  

1 of 1 models built (0 models already up to date)
Build duration: 0h 0m 11.99s


```

**Review the Regenerated Code**

```
struct tag_RTM_rtwdemo_abstime_T {
  const char_T *errorStatus;

  /*
   * Timing:
   * The following substructure contains information regarding
   * the timing information for the model.
   */
  struct {
    uint32_T clockTick1;
    uint16_T clockTick2;
    struct {
      uint16_T TID[3];
      uint16_T cLimit[3];
    } TaskCounters;
  } Timing;
};

/* Block states (default storage) */
extern DW_rtwdemo_abstime_T rtwdemo_abstime_DW;

/* Zero-crossing (trigger) state */
extern PrevZCX_rtwdemo_abstime_T rtwdemo_abstime_PrevZCX;

/* External inputs (root inport signals with default storage) */
extern ExtU_rtwdemo_abstime_T rtwdemo_abstime_U;

/* External outputs (root outports fed by signals with default storage) */
extern ExtY_rtwdemo_abstime_T rtwdemo_abstime_Y;

/* Model entry point functions */
extern void rtwdemo_abstime_initialize(void);
extern void rtwdemo_abstime_step0(void);
extern void rtwdemo_abstime_step1(void);
extern void rtwdemo_abstime_step2(void);
extern void rtwdemo_abstime_terminate(void);

/* Real-time Model object */
extern RT_MODEL_rtwdemo_abstime_T *const rtwdemo_abstime_M;

/*-
 * The generated code includes comments that allow you to trace directly
 * back to the appropriate location in the model.  The basic format
 * is <system>/block_name, where system is the system number (uniquely
 * assigned by Simulink) and block_name is the name of the block.
 *
 * Use the MATLAB hilite_system command to trace the generated code back
 * to the model.  For example,
 *
 * hilite_system('<S3>')    - opens system 3
 * hilite_system('<S3>/Kp') - opens and selects block Kp which resides in S3
 *
 * Here is the system hierarchy for this model
 *
 * '<Root>' : 'rtwdemo_abstime'
 * '<S1>'   : 'rtwdemo_abstime/SS1'
 * '<S2>'   : 'rtwdemo_abstime/SS2'
 * '<S3>'   : 'rtwdemo_abstime/SS3'
 */
#endif                                 /* RTW_HEADER_rtwdemo_abstime_h_ */


```

The new setting for the **Application lifespan (days)** parameter instructs the code generator to set aside less memory for the time counters. The regenerated code includes:

* 32-bit unsigned integer, `clockTick1`, for storing the elapsed time of the task for `SS2`
* 16-bit unsigned integer, `clockTick2`, for storing the elapsed time of the task for `SS3`

**Related Information**

### Single-Rate Modeling (Bare Board, No OS)

This model shows code generation for a single-rate discrete-time model configured for a bare-board target (one with no operating system).

**Open Example Model**

Open the example model `rtwdemo_srbb`.

```
open_system('rtwdemo_srbb')


```

![](https://www.mathworks.com/help/examples/simulinkcoder/win64/SingleRateModelingBareBoardNoOSExample_01.png)

The model uses one sample time and is configured to display sample-time colors when an update diagram occurs. Inport blocks In1_1s and In2_1s specify a 1-second sample time, which is enforced by the setting of model configuration parameter **Periodic sample time constraint**.

The model is configured to display color-coded sample times with annotations. To see them, after opening the model, update the diagram by pressing **Ctrl+D**. To display the legend, press **Ctrl+J**. Because the model is configured with one sample time, the model appears red, which is the color that represents the fastest discrete sample time in the model.

### Multirate Modeling in Single-Tasking Mode (Bare Board, No OS)

This model shows the code generated for a multirate discrete-time model configured for single-tasking on a bare-board target (one with no operating system).

**Open Example Model**

Open the example model `rtwdemo_mrstbb`.

```
open_system('rtwdemo_mrstbb')


```

![](https://www.mathworks.com/help/examples/simulinkcoder/win64/MultirMdlngInSingleTskngModeBareBoardNoOSExample_01.png)

The model contains two sample times. Inport block 1 and Inport block 2 specify 1-second and 2-second sample times, respectively, which are enforced by the setting of model configuration parameter **Periodic sample time constraint**. The solver is set for single-tasking operation. Rate transition blocks are, therefore, not included between blocks executing at different sample times because preemption will not occur.

The model is configured to display color-coded sample times with annotations. To see them, after opening the model, update the diagram by pressing **Ctrl+D**. To display the legend, press **Ctrl+J**. Red represents the fastest discrete sample time in the model, green represents the second fastest, and yellow represents mixed sample times.

### Multirate Modeling in Multitasking Mode (Bare Board, No OS)

This model shows the code generated for a multirate discrete-time model configured for a multitasking bare-board target (one with no operating system).

**Open Example Model**

Open the example model `rtwdemo_mrmtbb`.

```
open_system('rtwdemo_mrmtbb')


```

![](https://www.mathworks.com/help/examples/simulinkcoder/win64/MultirtMdlngInMultitskngModeBareBoardNoOSExample_01.png)

**Explore Example Model**

The model contains two sample times. Inport block 1 and Inport block 2 specify 1-second and 2-second sample times, respectively, which are enforced by the setting of model configuration parameter **Periodic sample time constraint**. The solver is set for multitasking operation, which means a rate transition block is required to ensure that data integrity is enforced when the 1-second task preempts the 2-second task. Proper rate transitions are always enforced by Simulink® and Simulink Coder™. This model specifies an explicit rate transition block. Alternatively, this block could be automatically inserted by Simulink setting model configuration parameter **Automatically handle rate transition for data transfer**.

The model is configured to display sample-time colors upon diagram update. To see them, after opening the model, update the diagram by pressing **Ctrl+D**. To display the legend, press **Ctrl+J**. Red represents the fastest discrete sample time in the model, green represents the second fastest, and yellow represents mixed sample times.

**Data Transfer Assumptions**

Basis of operation for data transfers between tasks:

* Data transitions occur between a single reading task and a single writing task.
* A read or write of a byte sized variable is atomic.
* When two tasks interact through a data transition, only one of them can preempt the other.
* For periodic tasks, the faster rate task has higher priority than the slower rate task; the faster rate task always preempts the slower rate task.
* Tasks run on a single processor. Time slicing is not allowed.
* Processes do not crash and restart (especially while data is being transferred between tasks)

### Multirate Modeling in Multitasking Mode (VxWorks® OS)

This example generates code for a multirate discrete-time model configured for a multitasking operating system target (VxWorks®). The model contains two sample times. Inport block 1 and Inport block 2 specify 1-second and 2-second sample times, respectively, which are enforced by the setting of model configuration parameter **Periodic sample time constraint**. The solver is set for multitasking operation, which means a Rate Transition block is required to ensure that data integrity is enforced when the 1-second task preempts the 2-second task. Simulink® and the code generator enforce proper rate transitions. This model specifies an explicit Rate Transition block. Alternatively, you can instruct Simulink® to insert this block for you by setting model configuration parameter **Automatically handle rate transition for data transfer**.

The model is configured to display color-coded sample times with annotations. To see them, after opening the model, update the diagram by pressing Ctrl+D. To display the legend, press Ctrl+J. Red represents the fastest discrete sample time in the model, green represents the second fastest, and yellow represents mixed sample times.

**Example Model**

```
model = 'rtwdemo_mrmtos';
open_system(model);


```

![](https://www.mathworks.com/help/examples/ecoder/win64/MultirateModelingInMultitaskingModeVxWorksOSExample_01.png)

### Trade Determinism and Data Integrity to Improve System Performance

This model shows the differences in the operation modes of the Rate Transition block when used in a multirate, multitasking model. The flexible options for the Rate Transition block allow you to select the mode that is best suited for your application. You can trade levels of determinism and data integrity to improve system performance.

**Rate Transition Block Modes of Operation**

**Ensure data integrity and determinism (DetAndInteg)**: Data is transferred such that all data bytes for the signal (including all elements of a wide signal) are from the same time step. Additionally, it is ensured that the relative sample time (delay) from which the data is transferred from one rate to another is always the same. Only ANSI® C code is used, no target specific 'critical section' protection is needed.

**Ensure integrity (IntegOnly)**: Data is transferred such that all data bytes for the signal (including all elements of a wide signal) are from the same time step. However, from one transfer of data to the next, the relative sample time (delay) for which the data is transferred can vary. In this mode, the code to read/write the data is run more often than in the DetandInt mode. In the worst case, the delay is equivalent to the DetandInt mode, but the delay can be less which is important is some applications. Also, this mode support data transfers to/from asynchronous rates which the DetandInt mode cannot support. Only ANSI-C code is used, no target specific 'critical section' protection is needed.

**No data consistency operations are performed (None)**: For this case, the Rate Transition block does not generated code. This mode is acceptable in some application where atomic access of scalar data types is guaranteed and when the relative temporal values of the data is not important. This mode does not introduce any delay.

**Data Transfer Assumptions**

Basis of operation for data transfers between tasks:

* Data transitions occur between a single reading task and a single writing task.
* A read or write of a byte sized variable is atomic.
* When two tasks interact through a data transition, only one of them can preempt the other.
* For periodic tasks, the faster rate task has higher priority than the slower rate task; the faster rate task always preempts the slower rate task.
* All tasks run on a single processor. Time slicing is not allowed.
* Processes do not crash/restart (especially while data is being transferred between tasks).

**Model `rtwdemo_ratetrans`**

```
open_system('rtwdemo_ratetrans')


```

![](https://www.mathworks.com/help/examples/simulinkcoder/win64/TradingDeterminismAndDataIntegrityExample_01.png)

Model `rtwdemo_ratetrans` shows the differences in the operation modes of the following Rate Transition blocks.

**Rate Transition Block `DetAndIntegF2S`**

Determinism and data integrity (fast to slow transition):

* The block output is used as a persistent data buffer.
* Data is written to output at slower rate but done during the faster rate context.
* Data as seen by the slower rate is always the value when both the faster and slower rate last executed. Any subsequent steps by the faster rate (and associated data updates) while the slower rate is running are not seen by the slower rate.

**Rate Transition Block `DetAndIntegS2F`**

Determinism and data integrity (slow to fast transition):

* Uses two persistent data buffers, an internal buffer and the blocks output.
* The internal buffer is copied to the output at the slower rate but done during the faster rate context.
* The internal buffer is written at the slower rate and during the slower rate context.
* The data that Fast rate sees is always delayed, i.e. data is from the previous step of the slow rate code.

**Rate Transition Block `IntegOnlyF2S`**

Data integrity only (fast to slow transition):

* The block output is used as a persistent data buffer.
* Data is written to buffer during the faster rate context if a flag indicates it not in the process of being read.
* The flag is set and data is copied from the buffer to output at the slow rate, the flag is then cleared. This is an additional copy as compared to the deterministic case.
* Data as seen by the slower rate can be from a more recent step of the faster rate than from when the slower rate and faster rate both executed.

**Rate Transition Block `IntegOnlyS2F`**

Data integrity only (slow to fast transition):

* Uses two persistent data buffers, both are internal buffers.
* One of the 2 buffers is always copied to the output at faster rate.
* One of the 2 buffers is written at the slower rate and during the slower rate context, then the active buffer is switched.
* The data as seen by the faster rate can be more recent than for the deterministic case. Specifically, when both the slower and faster rate have their hits, the faster rate will see a previous value from the slower rate. But, subsequent steps for the faster rate may see an updated value (when the slower rate updates the non-active buffer and switches the active buffer flag.

**Rate Transition Block `NoneF2S`**

No code is generated for the Rate Transition block when determinism and data integrity is waived.

**Rate Transition Block `NoneS2F`**

No code is generated for the Rate Transition block when determinism and data integrity is waived.

```
bdclose('rtwdemo_ratetrans');


```

## Related Topics

* [Time-Based Scheduling and Code Generation](https://www.mathworks.com/help/rtw/ug/time-based-scheduling-and-code-generation.html)
* [Modeling for Single-Tasking Execution](https://www.mathworks.com/help/rtw/ug/modeling-for-single-tasking-execution.html)
* [Modeling for Multitasking Execution](https://www.mathworks.com/help/rtw/ug/modeling-for-multitasking-execution.html)

You have a modified version of this example. Do you want to open this example with your edits?

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

Select a Web Site

Choose a web site to get translated content where available and see local events and offers. Based on your location, we recommend that you select: .

You can also select a web site from the following list:

[Contact your local office](#)
