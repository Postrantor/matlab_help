---
url: https://ww2.mathworks.cn/help/ecoder/ug/how-generated-code-stores-internal-signal-state-and-parameter-data.html
title: 生成的代码如何存储内部信号、状态和参数数据
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-18 21:00:33
tag: 
summary: 为了根据输入数据计算输出数据，生成的代码必须在内存中存储一些内部数据，例如模块状态数据和非标量参数数据。
---
## 生成的代码如何存储内部信号、状态和参数数据

为了根据输入计算输出，生成的代码会在全局内存中存储一些_内部数据_。未连接到根级别输入或输出（Inport 或 Outport 模块）的信号是内部数据。

内部数据还可以包括：

*   模块状态，例如 Unit Delay 模块的状态。算法必须保留执行周期之间的状态值，因此生成的代码通常将状态存储在全局内存中（例如，作为全局变量或全局结构体变量的一个字段）。
    
*   模块参数，例如 Gain 模块的**增益**参数，代码生成器不能在代码中内联该参数的值。例如，代码生成器不能内联非标量参数的值。
    
*   条件执行子系统（例如使能子系统）的状态指示符。
    

要获得更高效的代码，您可以配置优化，例如 > 和 > ，这些优化会尝试消除内部数据的存储。但是，优化不能消除某些数据的存储，这会占用生成的代码的内存使用量。

当您了解生成的代码存储内部数据的默认格式时，您可以：

*   默认情况下，使信号可访问且参数可调。然后，您可以在执行期间与代码进行交互并监视代码。
    
*   消除内部数据存储，并根据您的硬件和编译工具链来控制优化不能消除的数据在内存中的位置，从而生成高效的生产代码。
    
*   将内部数据段提升到模型接口，以便其他组件和系统可以访问这些数据。
    

有关生成的代码如何通过接口与调用方环境交换数据的信息，请参阅 [How Generated Code Exchanges Data with an Environment](https://ww2.mathworks.cn/help/ecoder/ug/how-generated-code-exchanges-data-with-an-environment.html)。

### 生成的代码中的内部数据

此示例说明生成的代码如何存储模块状态等内部数据。

**浏览模型示例**

打开示例模型 `RollAxisAutopilot`。

```
open_system('RollAxisAutopilot')


```

![](https://ww2.mathworks.cn/help/ecoder/ug/internaldatainthegeneratedcodeexample_01_zh_CN.png)

该模型包含不连接到根级别 Inport 或 Outport 模块的内部信号。某些信号具有名称，例如 `phiCmd` 信号。

该模型还包含一些维护状态数据的模块。例如，在 `BasicRollMode` 子系统中，标记为 `Integrator` 的 Discrete-Time Integrator 模块用于维护状态。

在模型中，将**配置参数 > 代码生成 > 系统目标文件**设置为 `grt.tlc`。

```
set_param('RollAxisAutopilot','SystemTargetFile','grt.tlc')


```

检查**配置参数 > 代码生成 > 接口 > 代码接口打包**的设置。设置 `Nonreusable function` 表示生成的代码不可重用（可重入）。

对于此示例，通过清除**配置参数 > 代码生成 > 接口 > 高级参数 > Mat 文件记录**生成更简单的代码。

```
set_param('RollAxisAutopilot','MatFileLogging','off')


```

**生成不可重用的代码**

设置以下配置参数：

*   将**默认参数行为**设置为 `Tunable`。
    
*   清除**信号存储重用**。
    

```
set_param('RollAxisAutopilot','DefaultParameterBehavior','Tunable',...
    'OptimizeBlockIOStorage','off')


```

从模型中生成代码。

```
slbuild('RollAxisAutopilot')


```

```
### Starting build procedure for: RollAxisAutopilot
### Successful completion of build procedure for: RollAxisAutopilot

Build Summary

Top model targets built:

Model              Action                        Rebuild Reason                                    
===================================================================================================
RollAxisAutopilot  Code generated and compiled.  Code generation information file does not exist.  

1 of 1 models built (0 models already up to date)
Build duration: 0h 0m 33.39s


```

文件 `RollAxisAutopilot.h` 定义了几种表示内部数据的结构体类型。例如，模块输入和输出结构体为模型中的每个内部信号定义一个字段。每个字段名称都派生自生成该信号的模块的名称，或者派生自该信号的名称（如果您指定了信号名称）。

```
file = fullfile('RollAxisAutopilot_grt_rtw','RollAxisAutopilot.h');
rtwdemodbtype(file,...
    '/* Block signals (default storage) */','} B_RollAxisAutopilot_T;',1,1)


```

```
/* Block signals (default storage) */
typedef struct {
  real32_T phiCmd;                     /* '<Root>/ModeSwitch' */
  real32_T hdgError;                   /* '<S2>/Sum' */
  real32_T DispGain;                   /* '<S2>/DispGain' */
  real32_T Product;                    /* '<S2>/Product' */
  real32_T Abs;                        /* '<S3>/Abs' */
  real32_T FixPtUnitDelay1;            /* '<S4>/FixPt Unit Delay1' */
  real32_T Xnew;                       /* '<S4>/Enable' */
  real32_T TKSwitch;                   /* '<S3>/TKSwitch' */
  real32_T RefSwitch;                  /* '<S3>/RefSwitch' */
  real32_T Integrator;                 /* '<S1>/Integrator' */
  real32_T DispLimit;                  /* '<S1>/DispLimit' */
  real32_T Sum;                        /* '<S1>/Sum' */
  real32_T DispGain_f;                 /* '<S1>/DispGain' */
  real32_T RateLimit;                  /* '<S1>/RateLimit' */
  real32_T Sum1;                       /* '<S1>/Sum1' */
  real32_T RateGain;                   /* '<S1>/RateGain' */
  real32_T Sum2;                       /* '<S1>/Sum2' */
  real32_T CmdLimit;                   /* '<S1>/CmdLimit' */
  real32_T IntGain;                    /* '<S1>/IntGain' */
  boolean_T NotEngaged;                /* '<S3>/NotEngaged' */
  boolean_T TKThreshold;               /* '<S3>/TKThreshold' */
  boolean_T RefThreshold2;             /* '<S3>/RefThreshold2' */
  boolean_T RefThreshold1;             /* '<S3>/RefThreshold1' */
  boolean_T Or;                        /* '<S3>/Or' */
  boolean_T NotEngaged_e;              /* '<S1>/NotEngaged' */
} B_RollAxisAutopilot_T;


```

该文件定义一种 DWork 结构体类型，用于表示模块状态，例如 Discrete-Time Integrator 模块的状态。

```
rtwdemodbtype(file,...
    '/* Block states (default storage) for system','} DW_RollAxisAutopilot_T;',1,1)


```

```
/* Block states (default storage) for system '<Root>' */
typedef struct {
  real32_T FixPtUnitDelay1_DSTATE;     /* '<S4>/FixPt Unit Delay1' */
  real32_T Integrator_DSTATE;          /* '<S1>/Integrator' */
  int8_T Integrator_PrevResetState;    /* '<S1>/Integrator' */
} DW_RollAxisAutopilot_T;


```

该文件定义一种表示参数数据的结构体类型。模型中的每个可调模块参数（例如 Gain 模块的**增益**参数）显示为此结构体的一个字段。如果模块参数从 MATLAB® 变量或 `Simulink.Parameter` 对象获取其参数值，则该变量或对象显示为字段，而不是模块参数。

该文件还定义一种结构体类型，即_实时模型数据结构体_，其单个字段表示一种运行时指示，用于指示生成的代码在执行期间是否遇到错误。

```
rtwdemodbtype(file,'/* Real-time Model Data Structure */',...
    '/* Block parameters (default storage) */',1,0)


```

```
/* Real-time Model Data Structure */
struct tag_RTM_RollAxisAutopilot_T {
  const char_T *errorStatus;
};


```

对于表示实时模型数据结构体的结构体类型，文件 `RollAxisAutopilot_types.h` 会创建一个别名，生成的代码稍后将使用该别名为结构体分配内存。

```
file = fullfile('RollAxisAutopilot_grt_rtw','RollAxisAutopilot_types.h');
rtwdemodbtype(file,'/* Forward declaration for rtModel */',...
    'RT_MODEL_RollAxisAutopilot_T;',1,1)


```

```
/* Forward declaration for rtModel */
typedef struct tag_RTM_RollAxisAutopilot_T RT_MODEL_RollAxisAutopilot_T;


```

文件 `RollAxisAutopilot.c` 使用这些结构体类型来定义用于为生成的算法存储内部数据的全局结构体变量（为其分配内存）。该文件还定义表示实时模型数据结构体的变量和指向该结构体的指针。

```
file = fullfile('RollAxisAutopilot_grt_rtw','RollAxisAutopilot.c');
rtwdemodbtype(file,'/* Block signals (default storage) */',...
    '= &RollAxisAutopilot_M_;',1,1)


```

```
/* Block signals (default storage) */
B_RollAxisAutopilot_T RollAxisAutopilot_B;

/* Block states (default storage) */
DW_RollAxisAutopilot_T RollAxisAutopilot_DW;

/* External inputs (root inport signals with default storage) */
ExtU_RollAxisAutopilot_T RollAxisAutopilot_U;

/* External outputs (root outports fed by signals with default storage) */
ExtY_RollAxisAutopilot_T RollAxisAutopilot_Y;

/* Real-time model */
static RT_MODEL_RollAxisAutopilot_T RollAxisAutopilot_M_;
RT_MODEL_RollAxisAutopilot_T *const RollAxisAutopilot_M = &RollAxisAutopilot_M_;


```

模型 `step` 函数（表示主模型算法）使用 `void void` 接口（不带参数）。

```
rtwdemodbtype(file,...
    '/* Model step function */','void RollAxisAutopilot_step(void)',1,1)


```

```
/* Model step function */
void RollAxisAutopilot_step(void)


```

在函数定义中，算法通过直接访问全局变量来执行计算并将中间结果存储在信号和状态结构体中。该算法还从对应的全局变量中读取参数数据。例如，在 `BasicRollMode` 子系统中，为 `Integrator` 模块生成的代码在结构体中读取和写入信号、状态和参数数据。

```
rtwdemodbtype(file,'/* DiscreteIntegrator: ''<S1>/Integrator'' *',...
    '/* End of DiscreteIntegrator: ''<S1>/Integrator'' */',1,1)


```

```
  /* DiscreteIntegrator: '<S1>/Integrator' */
  if (RollAxisAutopilot_B.NotEngaged_e ||
      (RollAxisAutopilot_DW.Integrator_PrevResetState != 0)) {
    RollAxisAutopilot_DW.Integrator_DSTATE = RollAxisAutopilot_P.Integrator_IC;
  }

  if (RollAxisAutopilot_DW.Integrator_DSTATE >= RollAxisAutopilot_P.intLim) {
    RollAxisAutopilot_DW.Integrator_DSTATE = RollAxisAutopilot_P.intLim;
  } else if (RollAxisAutopilot_DW.Integrator_DSTATE <=
             RollAxisAutopilot_P.Integrator_LowerSat) {
    RollAxisAutopilot_DW.Integrator_DSTATE =
      RollAxisAutopilot_P.Integrator_LowerSat;
  }

  /* DiscreteIntegrator: '<S1>/Integrator' */
  RollAxisAutopilot_B.Integrator = RollAxisAutopilot_DW.Integrator_DSTATE;

  /* Saturate: '<S1>/DispLimit' */
  u0 = RollAxisAutopilot_B.phiCmd;
  u1 = RollAxisAutopilot_P.DispLimit_LowerSat;
  u2 = RollAxisAutopilot_P.dispLim;
  if (u0 > u2) {
    /* Saturate: '<S1>/DispLimit' */
    RollAxisAutopilot_B.DispLimit = u2;
  } else if (u0 < u1) {
    /* Saturate: '<S1>/DispLimit' */
    RollAxisAutopilot_B.DispLimit = u1;
  } else {
    /* Saturate: '<S1>/DispLimit' */
    RollAxisAutopilot_B.DispLimit = u0;
  }

  /* End of Saturate: '<S1>/DispLimit' */

  /* Sum: '<S1>/Sum' incorporates:
   *  Inport: '<Root>/Phi'
   */
  RollAxisAutopilot_B.Sum = RollAxisAutopilot_B.DispLimit -
    RollAxisAutopilot_U.Phi;

  /* Gain: '<S1>/DispGain' */
  RollAxisAutopilot_B.DispGain_f = RollAxisAutopilot_P.dispGain *
    RollAxisAutopilot_B.Sum;

  /* Saturate: '<S1>/RateLimit' */
  u0 = RollAxisAutopilot_B.DispGain_f;
  u1 = RollAxisAutopilot_P.RateLimit_LowerSat;
  u2 = RollAxisAutopilot_P.rateLim;
  if (u0 > u2) {
    /* Saturate: '<S1>/RateLimit' */
    RollAxisAutopilot_B.RateLimit = u2;
  } else if (u0 < u1) {
    /* Saturate: '<S1>/RateLimit' */
    RollAxisAutopilot_B.RateLimit = u1;
  } else {
    /* Saturate: '<S1>/RateLimit' */
    RollAxisAutopilot_B.RateLimit = u0;
  }

  /* End of Saturate: '<S1>/RateLimit' */

  /* Sum: '<S1>/Sum1' incorporates:
   *  Inport: '<Root>/Rate_FB'
   */
  RollAxisAutopilot_B.Sum1 = RollAxisAutopilot_B.RateLimit -
    RollAxisAutopilot_U.Rate_FB;

  /* Gain: '<S1>/RateGain' */
  RollAxisAutopilot_B.RateGain = RollAxisAutopilot_P.rateGain *
    RollAxisAutopilot_B.Sum1;

  /* Sum: '<S1>/Sum2' */
  RollAxisAutopilot_B.Sum2 = RollAxisAutopilot_B.Integrator +
    RollAxisAutopilot_B.RateGain;

  /* Saturate: '<S1>/CmdLimit' */
  u0 = RollAxisAutopilot_B.Sum2;
  u1 = RollAxisAutopilot_P.CmdLimit_LowerSat;
  u2 = RollAxisAutopilot_P.cmdLim;
  if (u0 > u2) {
    /* Saturate: '<S1>/CmdLimit' */
    RollAxisAutopilot_B.CmdLimit = u2;
  } else if (u0 < u1) {
    /* Saturate: '<S1>/CmdLimit' */
    RollAxisAutopilot_B.CmdLimit = u1;
  } else {
    /* Saturate: '<S1>/CmdLimit' */
    RollAxisAutopilot_B.CmdLimit = u0;
  }

  /* End of Saturate: '<S1>/CmdLimit' */

  /* Gain: '<S1>/IntGain' */
  RollAxisAutopilot_B.IntGain = RollAxisAutopilot_P.intGain *
    RollAxisAutopilot_B.Sum1;

  /* Update for DiscreteIntegrator: '<S1>/Integrator' */
  RollAxisAutopilot_DW.Integrator_DSTATE +=
    RollAxisAutopilot_P.Integrator_gainval * RollAxisAutopilot_B.IntGain;
  if (RollAxisAutopilot_DW.Integrator_DSTATE >= RollAxisAutopilot_P.intLim) {
    RollAxisAutopilot_DW.Integrator_DSTATE = RollAxisAutopilot_P.intLim;
  } else if (RollAxisAutopilot_DW.Integrator_DSTATE <=
             RollAxisAutopilot_P.Integrator_LowerSat) {
    RollAxisAutopilot_DW.Integrator_DSTATE =
      RollAxisAutopilot_P.Integrator_LowerSat;
  }

  RollAxisAutopilot_DW.Integrator_PrevResetState = (int8_T)
    RollAxisAutopilot_B.NotEngaged_e;

  /* End of Update for DiscreteIntegrator: '<S1>/Integrator' */
  /* End of Outputs for SubSystem: '<Root>/BasicRollMode' */

  /* Switch: '<Root>/EngSwitch' incorporates:
   *  Inport: '<Root>/AP_Eng'
   */
  if (RollAxisAutopilot_U.AP_Eng) {
    /* Outport: '<Root>/Ail_Cmd' */
    RollAxisAutopilot_Y.Ail_Cmd = RollAxisAutopilot_B.CmdLimit;
  } else {
    /* Outport: '<Root>/Ail_Cmd' incorporates:
     *  Constant: '<Root>/Zero'
     */
    RollAxisAutopilot_Y.Ail_Cmd = RollAxisAutopilot_P.Zero_Value_c;
  }

  /* End of Switch: '<Root>/EngSwitch' */
}

/* Model initialize function */
void RollAxisAutopilot_initialize(void)
{
  /* Registration code */

  /* initialize error status */
  rtmSetErrorStatus(RollAxisAutopilot_M, (NULL));

  /* block I/O */
  (void) memset(((void *) &RollAxisAutopilot_B), 0,
                sizeof(B_RollAxisAutopilot_T));

  /* states (dwork) */
  (void) memset((void *)&RollAxisAutopilot_DW, 0,
                sizeof(DW_RollAxisAutopilot_T));

  /* external inputs */
  (void)memset(&RollAxisAutopilot_U, 0, sizeof(ExtU_RollAxisAutopilot_T));

  /* external outputs */
  RollAxisAutopilot_Y.Ail_Cmd = 0.0F;

  /* SystemInitialize for Atomic SubSystem: '<Root>/RollAngleReference' */
  /* InitializeConditions for UnitDelay: '<S4>/FixPt Unit Delay1' */
  RollAxisAutopilot_DW.FixPtUnitDelay1_DSTATE =
    RollAxisAutopilot_P.LatchPhi_vinit;

  /* End of SystemInitialize for SubSystem: '<Root>/RollAngleReference' */

  /* SystemInitialize for Atomic SubSystem: '<Root>/BasicRollMode' */
  /* InitializeConditions for DiscreteIntegrator: '<S1>/Integrator' */
  RollAxisAutopilot_DW.Integrator_DSTATE = RollAxisAutopilot_P.Integrator_IC;
  RollAxisAutopilot_DW.Integrator_PrevResetState = 0;

  /* End of SystemInitialize for SubSystem: '<Root>/BasicRollMode' */
}

/* Model terminate function */
void RollAxisAutopilot_terminate(void)
{
  /* (no terminate code required) */
}


```

由于存在 `void void` 接口和直接数据访问，该函数不可重入。如果在一个应用程序中多次调用该函数，则每次调用都会将数据写入全局结构体变量，后续调用可以读取该数据，从而导致各次调用之间出现意外的干扰。

模型初始化函数 `RollAxisAutopilot_initialize` 将所有内部数据初始化为零。该函数还通过调用专用宏函数对错误状态进行初始化。初始化函数直接访问全局变量，这意味着该函数不可重入。

```
rtwdemodbtype(file,'/* Model initialize function */',...
    'sizeof(DW_RollAxisAutopilot_T));',1,1)


```

```
/* Model initialize function */
void RollAxisAutopilot_initialize(void)
{
  /* Registration code */

  /* initialize error status */
  rtmSetErrorStatus(RollAxisAutopilot_M, (NULL));

  /* block I/O */
  (void) memset(((void *) &RollAxisAutopilot_B), 0,
                sizeof(B_RollAxisAutopilot_T));

  /* states (dwork) */
  (void) memset((void *)&RollAxisAutopilot_DW, 0,
                sizeof(DW_RollAxisAutopilot_T));


```

然后，该函数将 DWork 结构体中的模块状态初始化为模型中的模块参数指定的初始值。模型的三个状态中的两个具有可调初始值，因此代码通过从参数结构体中读取数据来初始化它们。

```
rtwdemodbtype(file,...
    '/* SystemInitialize for Atomic SubSystem: ''<Root>/RollAngleReference'' */',...
    '/* Model terminate function */',1,0)


```

```
  /* SystemInitialize for Atomic SubSystem: '<Root>/RollAngleReference' */
  /* InitializeConditions for UnitDelay: '<S4>/FixPt Unit Delay1' */
  RollAxisAutopilot_DW.FixPtUnitDelay1_DSTATE =
    RollAxisAutopilot_P.LatchPhi_vinit;

  /* End of SystemInitialize for SubSystem: '<Root>/RollAngleReference' */

  /* SystemInitialize for Atomic SubSystem: '<Root>/BasicRollMode' */
  /* InitializeConditions for DiscreteIntegrator: '<S1>/Integrator' */
  RollAxisAutopilot_DW.Integrator_DSTATE = RollAxisAutopilot_P.Integrator_IC;
  RollAxisAutopilot_DW.Integrator_PrevResetState = 0;

  /* End of SystemInitialize for SubSystem: '<Root>/BasicRollMode' */
}


```

**生成可重用的代码**

您可以将生成的代码配置为可重入代码，这意味着您可以在一个应用程序中多次调用入口函数。使用此配置时，入口函数并不直接访问全局变量，而是通过形参（指针参数）接受内部数据。通过使用这些指针参数，每次调用都可以在一组单独的全局变量中维护内部数据，从而防止各次调用之间出现意外的交互。

在模型中，将**配置参数 > 代码生成 > 接口 > 代码接口打包**设置为 `Reusable function`。

```
set_param('RollAxisAutopilot','CodeInterfacePackaging','Reusable function')


```

从模型中生成代码。

```
slbuild('RollAxisAutopilot')


```

```
### Starting build procedure for: RollAxisAutopilot
### Successful completion of build procedure for: RollAxisAutopilot

Build Summary

Top model targets built:

Model              Action                        Rebuild Reason                   
==================================================================================
RollAxisAutopilot  Code generated and compiled.  Generated code was out of date.  

1 of 1 models built (0 models already up to date)
Build duration: 0h 0m 22.83s


```

现在，在 `RollAxisAutopilot.h` 中，实时模型数据结构体包含指向错误指示的指针、内部数据以及 `ExtU` 和 `ExtY` 子结构体形式的主输入和输出数据（其字段表示模型根级别的 Inport 和 Outport 模块）。

```
file = fullfile('RollAxisAutopilot_grt_rtw','RollAxisAutopilot.h');
rtwdemodbtype(file,'/* Real-time Model Data Structure */',...
    '/* External data declarations for dependent source files */',1,0)


```

```
/* Real-time Model Data Structure */
struct tag_RTM_RollAxisAutopilot_T {
  const char_T *errorStatus;
  B_RollAxisAutopilot_T *blockIO;
  ExtU_RollAxisAutopilot_T *inputs;
  ExtY_RollAxisAutopilot_T *outputs;
  DW_RollAxisAutopilot_T *dwork;
};

/* Block parameters (default storage) */
extern P_RollAxisAutopilot_T RollAxisAutopilot_P;


```

要在一个应用程序中多次调用生成的代码，您的代码必须在每次调用时为实时模型数据结构体分配内存。文件 `RollAxisAutopilot.c` 定义一个专用函数，它为新实时模型数据结构体分配内存并返回指向该结构体的指针。该函数还为模型数据结构体中的字段指向的子结构体（例如 DWork 结构体）分配内存。

```
file = fullfile('RollAxisAutopilot_grt_rtw','RollAxisAutopilot.c');
rtwdemodbtype(file,'/* Model data allocation function */',...
    'RT_MODEL_RollAxisAutopilot_T *RollAxisAutopilot(void)',1,1)


```

```
/* Model data allocation function */
RT_MODEL_RollAxisAutopilot_T *RollAxisAutopilot(void)


```

模型 `step` 函数接受表示实时模型数据结构体的参数。

```
rtwdemodbtype(file,'/* Model step function */','void RollAxisAutopilot_step',1,1)


```

```
/* Model step function */
void RollAxisAutopilot_step(RT_MODEL_RollAxisAutopilot_T *const


```

在函数定义中，算法首先将每个指针从实时模型数据结构体中提取到局部变量中。

```
rtwdemodbtype(file,'*RollAxisAutopilot_B =','RollAxisAutopilot_M->outputs;',1,1)


```

```
  B_RollAxisAutopilot_T *RollAxisAutopilot_B = RollAxisAutopilot_M->blockIO;
  DW_RollAxisAutopilot_T *RollAxisAutopilot_DW = RollAxisAutopilot_M->dwork;
  ExtU_RollAxisAutopilot_T *RollAxisAutopilot_U = (ExtU_RollAxisAutopilot_T *)
    RollAxisAutopilot_M->inputs;
  ExtY_RollAxisAutopilot_T *RollAxisAutopilot_Y = (ExtY_RollAxisAutopilot_T *)
    RollAxisAutopilot_M->outputs;


```

然后，为了访问存储在全局内存中的内部数据，该算法与这些局部变量交互。

```
rtwdemodbtype(file,'/* DiscreteIntegrator: ''<S1>/Integrator'' */',...
    '/* End of DiscreteIntegrator: ''<S1>/Integrator'' */',1,1)


```

```
  /* DiscreteIntegrator: '<S1>/Integrator' */
  if (RollAxisAutopilot_B->NotEngaged_e ||
      (RollAxisAutopilot_DW->Integrator_PrevResetState != 0)) {
    RollAxisAutopilot_DW->Integrator_DSTATE = RollAxisAutopilot_P.Integrator_IC;
  }

  if (RollAxisAutopilot_DW->Integrator_DSTATE >= RollAxisAutopilot_P.intLim) {
    RollAxisAutopilot_DW->Integrator_DSTATE = RollAxisAutopilot_P.intLim;
  } else if (RollAxisAutopilot_DW->Integrator_DSTATE <=
             RollAxisAutopilot_P.Integrator_LowerSat) {
    RollAxisAutopilot_DW->Integrator_DSTATE =
      RollAxisAutopilot_P.Integrator_LowerSat;
  }

  /* DiscreteIntegrator: '<S1>/Integrator' */
  RollAxisAutopilot_B->Integrator = RollAxisAutopilot_DW->Integrator_DSTATE;

  /* Saturate: '<S1>/DispLimit' */
  u0 = RollAxisAutopilot_B->phiCmd;
  u1 = RollAxisAutopilot_P.DispLimit_LowerSat;
  u2 = RollAxisAutopilot_P.dispLim;
  if (u0 > u2) {
    /* Saturate: '<S1>/DispLimit' */
    RollAxisAutopilot_B->DispLimit = u2;
  } else if (u0 < u1) {
    /* Saturate: '<S1>/DispLimit' */
    RollAxisAutopilot_B->DispLimit = u1;
  } else {
    /* Saturate: '<S1>/DispLimit' */
    RollAxisAutopilot_B->DispLimit = u0;
  }

  /* End of Saturate: '<S1>/DispLimit' */

  /* Sum: '<S1>/Sum' */
  RollAxisAutopilot_B->Sum = RollAxisAutopilot_B->DispLimit -
    RollAxisAutopilot_U->Phi;

  /* Gain: '<S1>/DispGain' */
  RollAxisAutopilot_B->DispGain_f = RollAxisAutopilot_P.dispGain *
    RollAxisAutopilot_B->Sum;

  /* Saturate: '<S1>/RateLimit' */
  u0 = RollAxisAutopilot_B->DispGain_f;
  u1 = RollAxisAutopilot_P.RateLimit_LowerSat;
  u2 = RollAxisAutopilot_P.rateLim;
  if (u0 > u2) {
    /* Saturate: '<S1>/RateLimit' */
    RollAxisAutopilot_B->RateLimit = u2;
  } else if (u0 < u1) {
    /* Saturate: '<S1>/RateLimit' */
    RollAxisAutopilot_B->RateLimit = u1;
  } else {
    /* Saturate: '<S1>/RateLimit' */
    RollAxisAutopilot_B->RateLimit = u0;
  }

  /* End of Saturate: '<S1>/RateLimit' */

  /* Sum: '<S1>/Sum1' */
  RollAxisAutopilot_B->Sum1 = RollAxisAutopilot_B->RateLimit -
    RollAxisAutopilot_U->Rate_FB;

  /* Gain: '<S1>/RateGain' */
  RollAxisAutopilot_B->RateGain = RollAxisAutopilot_P.rateGain *
    RollAxisAutopilot_B->Sum1;

  /* Sum: '<S1>/Sum2' */
  RollAxisAutopilot_B->Sum2 = RollAxisAutopilot_B->Integrator +
    RollAxisAutopilot_B->RateGain;

  /* Saturate: '<S1>/CmdLimit' */
  u0 = RollAxisAutopilot_B->Sum2;
  u1 = RollAxisAutopilot_P.CmdLimit_LowerSat;
  u2 = RollAxisAutopilot_P.cmdLim;
  if (u0 > u2) {
    /* Saturate: '<S1>/CmdLimit' */
    RollAxisAutopilot_B->CmdLimit = u2;
  } else if (u0 < u1) {
    /* Saturate: '<S1>/CmdLimit' */
    RollAxisAutopilot_B->CmdLimit = u1;
  } else {
    /* Saturate: '<S1>/CmdLimit' */
    RollAxisAutopilot_B->CmdLimit = u0;
  }

  /* End of Saturate: '<S1>/CmdLimit' */

  /* Gain: '<S1>/IntGain' */
  RollAxisAutopilot_B->IntGain = RollAxisAutopilot_P.intGain *
    RollAxisAutopilot_B->Sum1;

  /* Update for DiscreteIntegrator: '<S1>/Integrator' */
  RollAxisAutopilot_DW->Integrator_DSTATE +=
    RollAxisAutopilot_P.Integrator_gainval * RollAxisAutopilot_B->IntGain;
  if (RollAxisAutopilot_DW->Integrator_DSTATE >= RollAxisAutopilot_P.intLim) {
    RollAxisAutopilot_DW->Integrator_DSTATE = RollAxisAutopilot_P.intLim;
  } else if (RollAxisAutopilot_DW->Integrator_DSTATE <=
             RollAxisAutopilot_P.Integrator_LowerSat) {
    RollAxisAutopilot_DW->Integrator_DSTATE =
      RollAxisAutopilot_P.Integrator_LowerSat;
  }

  RollAxisAutopilot_DW->Integrator_PrevResetState = (int8_T)
    RollAxisAutopilot_B->NotEngaged_e;

  /* End of Update for DiscreteIntegrator: '<S1>/Integrator' */
  /* End of Outputs for SubSystem: '<Root>/BasicRollMode' */

  /* Switch: '<Root>/EngSwitch' */
  if (RollAxisAutopilot_U->AP_Eng) {
    /* Outport: '<Root>/Ail_Cmd' */
    RollAxisAutopilot_Y->Ail_Cmd = RollAxisAutopilot_B->CmdLimit;
  } else {
    /* Outport: '<Root>/Ail_Cmd' incorporates:
     *  Constant: '<Root>/Zero'
     */
    RollAxisAutopilot_Y->Ail_Cmd = RollAxisAutopilot_P.Zero_Value_c;
  }

  /* End of Switch: '<Root>/EngSwitch' */
}

/* Model initialize function */
void RollAxisAutopilot_initialize(RT_MODEL_RollAxisAutopilot_T *const
  RollAxisAutopilot_M)
{
  DW_RollAxisAutopilot_T *RollAxisAutopilot_DW = RollAxisAutopilot_M->dwork;

  /* SystemInitialize for Atomic SubSystem: '<Root>/RollAngleReference' */
  /* InitializeConditions for UnitDelay: '<S4>/FixPt Unit Delay1' */
  RollAxisAutopilot_DW->FixPtUnitDelay1_DSTATE =
    RollAxisAutopilot_P.LatchPhi_vinit;

  /* End of SystemInitialize for SubSystem: '<Root>/RollAngleReference' */

  /* SystemInitialize for Atomic SubSystem: '<Root>/BasicRollMode' */
  /* InitializeConditions for DiscreteIntegrator: '<S1>/Integrator' */
  RollAxisAutopilot_DW->Integrator_DSTATE = RollAxisAutopilot_P.Integrator_IC;
  RollAxisAutopilot_DW->Integrator_PrevResetState = 0;

  /* End of SystemInitialize for SubSystem: '<Root>/BasicRollMode' */
}

/* Model terminate function */
void RollAxisAutopilot_terminate(RT_MODEL_RollAxisAutopilot_T
  * RollAxisAutopilot_M)
{
  /* model code */
  rt_FREE(RollAxisAutopilot_M->blockIO);
  rt_FREE(RollAxisAutopilot_M->inputs);
  rt_FREE(RollAxisAutopilot_M->outputs);
  rt_FREE(RollAxisAutopilot_M->dwork);
  rt_FREE(RollAxisAutopilot_M);
}

/* Model data allocation function */
RT_MODEL_RollAxisAutopilot_T *RollAxisAutopilot(void)
{
  RT_MODEL_RollAxisAutopilot_T *RollAxisAutopilot_M;
  RollAxisAutopilot_M = (RT_MODEL_RollAxisAutopilot_T *) malloc(sizeof
    (RT_MODEL_RollAxisAutopilot_T));
  if (RollAxisAutopilot_M == (NULL)) {
    return (NULL);
  }

  (void) memset((char *)RollAxisAutopilot_M, 0,
                sizeof(RT_MODEL_RollAxisAutopilot_T));

  /* block I/O */
  {
    B_RollAxisAutopilot_T *b = (B_RollAxisAutopilot_T *) malloc(sizeof
      (B_RollAxisAutopilot_T));
    rt_VALIDATE_MEMORY(RollAxisAutopilot_M,b);
    RollAxisAutopilot_M->blockIO = (b);
  }

  /* states (dwork) */
  {
    DW_RollAxisAutopilot_T *dwork = (DW_RollAxisAutopilot_T *) malloc(sizeof
      (DW_RollAxisAutopilot_T));
    rt_VALIDATE_MEMORY(RollAxisAutopilot_M,dwork);
    RollAxisAutopilot_M->dwork = (dwork);
  }

  /* external inputs */
  {
    ExtU_RollAxisAutopilot_T *RollAxisAutopilot_U = (ExtU_RollAxisAutopilot_T *)
      malloc(sizeof(ExtU_RollAxisAutopilot_T));
    rt_VALIDATE_MEMORY(RollAxisAutopilot_M,RollAxisAutopilot_U);
    RollAxisAutopilot_M->inputs = (((ExtU_RollAxisAutopilot_T *)
      RollAxisAutopilot_U));
  }

  /* external outputs */
  {
    ExtY_RollAxisAutopilot_T *RollAxisAutopilot_Y = (ExtY_RollAxisAutopilot_T *)
      malloc(sizeof(ExtY_RollAxisAutopilot_T));
    rt_VALIDATE_MEMORY(RollAxisAutopilot_M,RollAxisAutopilot_Y);
    RollAxisAutopilot_M->outputs = (RollAxisAutopilot_Y);
  }

  {
    B_RollAxisAutopilot_T *RollAxisAutopilot_B = RollAxisAutopilot_M->blockIO;
    DW_RollAxisAutopilot_T *RollAxisAutopilot_DW = RollAxisAutopilot_M->dwork;
    ExtU_RollAxisAutopilot_T *RollAxisAutopilot_U = (ExtU_RollAxisAutopilot_T *)
      RollAxisAutopilot_M->inputs;
    ExtY_RollAxisAutopilot_T *RollAxisAutopilot_Y = (ExtY_RollAxisAutopilot_T *)
      RollAxisAutopilot_M->outputs;

    /* block I/O */
    (void) memset(((void *) RollAxisAutopilot_B), 0,
                  sizeof(B_RollAxisAutopilot_T));

    /* states (dwork) */
    (void) memset((void *)RollAxisAutopilot_DW, 0,
                  sizeof(DW_RollAxisAutopilot_T));

    /* external inputs */
    (void)memset(RollAxisAutopilot_U, 0, sizeof(ExtU_RollAxisAutopilot_T));

    /* external outputs */
    RollAxisAutopilot_Y->Ail_Cmd = 0.0F;
  }

  return RollAxisAutopilot_M;
}


```

同样，模型初始化函数接受实时模型数据结构体作为参数。

```
rtwdemodbtype(file,...
    '/* Model initialize function */','void RollAxisAutopilot_initialize',1,1)


```

```
/* Model initialize function */
void RollAxisAutopilot_initialize(RT_MODEL_RollAxisAutopilot_T *const


```

由于您对入口函数的每次调用都与一个单独的实时模型数据结构体交互，因此可以避免各次调用之间发生意外交互。

**使用代码生成优化消除内部数据**

为了获得消耗更少内存的更高效代码，请选择优化，例如您在前面清除了的**默认参数行为**。

```
set_param('RollAxisAutopilot','DefaultParameterBehavior','Inlined',...
    'OptimizeBlockIOStorage','on',...
    'LocalBlockOutputs','on')


```

在此示例中，为了获得更简单的代码，请将**代码接口打包**设置为 `Nonreusable function`。

```
set_param('RollAxisAutopilot','CodeInterfacePackaging','Nonreusable function')


```

从模型中生成代码。

```
slbuild('RollAxisAutopilot')


```

```
### Starting build procedure for: RollAxisAutopilot
### Successful completion of build procedure for: RollAxisAutopilot

Build Summary

Top model targets built:

Model              Action                        Rebuild Reason                   
==================================================================================
RollAxisAutopilot  Code generated and compiled.  Generated code was out of date.  

1 of 1 models built (0 models already up to date)
Build duration: 0h 0m 18.309s


```

现在，`RollAxisAutopilot.h` 没有定义用于模块输入和输出的结构体。对于模型中的所有内部信号，优化要么已消除了存储，要么创建了局部函数变量而不是全局结构体字段。

优化未能消除三种模块状态的存储，因此文件继续定义 DWork 结构体类型。

```
file = fullfile('RollAxisAutopilot_grt_rtw','RollAxisAutopilot.h');
rtwdemodbtype(file,...
    '/* Block states (default storage) for system','} DW_RollAxisAutopilot_T;',1,1)


```

```
/* Block states (default storage) for system '<Root>' */
typedef struct {
  real32_T FixPtUnitDelay1_DSTATE;     /* '<S4>/FixPt Unit Delay1' */
  real32_T Integrator_DSTATE;          /* '<S1>/Integrator' */
  int8_T Integrator_PrevResetState;    /* '<S1>/Integrator' */
} DW_RollAxisAutopilot_T;


```

现在，为 Discrete-Time Integrator 模块生成的代码仅在 DWork 结构体中存储状态和输出数据。

```
file = fullfile('RollAxisAutopilot_grt_rtw','RollAxisAutopilot.c');
rtwdemodbtype(file,'/* Update for DiscreteIntegrator: ''<S1>/Integrator''',...
    '/* End of Update for DiscreteIntegrator: ''<S1>/Integrator'' */',1,1)


```

```
  /* Update for DiscreteIntegrator: '<S1>/Integrator' incorporates:
   *  Gain: '<S1>/IntGain'
   */
  RollAxisAutopilot_DW.Integrator_DSTATE += 0.5F * rtb_TKSwitch * 0.025F;
  if (RollAxisAutopilot_DW.Integrator_DSTATE >= 5.0F) {
    RollAxisAutopilot_DW.Integrator_DSTATE = 5.0F;
  } else if (RollAxisAutopilot_DW.Integrator_DSTATE <= -5.0F) {
    RollAxisAutopilot_DW.Integrator_DSTATE = -5.0F;
  }

  RollAxisAutopilot_DW.Integrator_PrevResetState = (int8_T)tmp;



```

优化还消除了模型中模块参数的存储。例如，在 Discrete-Time Integrator 模块中，**饱和上限**和**饱和下限**参数设置为 `intLim` 和 `-intLim`。`intLim` 是 `Simulink.Parameter` 对象，用于存储值 `5`。在为 Discrete-Time Integrator 生成的代码中，这些模块参数和 `intLim` 显示为内联字面数字 `5.0F` 和 `-5.0F`。

如果模型包含代码生成器不能直接内联的参数（例如数组参数），则代码会定义表示该数据的结构体类型。此_常量参数_结构体使用 `const` 存储类型限定符，因此某些编译工具链可以进一步优化程序集代码。

结构体 `ConstParam__model_` 的声明位于 `_model_.h` 中。

```
/* Constant parameters (auto storage) */
typedef struct {
   /* Expression: [1 2 3 4 5 6 7]
    * Referenced by: '<Root>/Constant'
    */
   real_T Constant_Value[7];
   
   /* Expression: [7 6 5 4 3 2 1]
    * Referenced by: '<Root>/Gain'
    */
   real_T Gain_Gain[7];
 } ConstParam_model;


```

默认情况下，常量参数 `_model__ConstP` 的声明位于 `_model__data.c` 中。

```
/* Constant parameters (auto storage) */
const ConstParam_model model_ConstP = {
   /* Expression: [1 2 3 4 5 6 7]
    * Referenced by: '<Root>/Constant'
    */
   { 1.0, 2.0, 3.0, 4.0, 5.0, 6.0, 7.0 },

   /* Expression: [7 6 5 4 3 2 1]
    * Referenced by: '<Root>/Gain'
    */
   { 7.0, 6.0, 5.0, 4.0, 3.0, 2.0, 1.0 }
};


```

`_model__ConstP` 作为参数传递给引用模型。

### 生成的代码中的局部变量

当您选择 > 优化时，代码生成器会尝试通过将内部信号表示为局部变量（而不是全局结构体的字段）来生成更高效的代码。如果局部变量消耗的内存有超过目标硬件上的可用堆栈空间的风险，请考虑通过设置 > 来指示最大堆栈大小。有关详细信息，请参阅 [Maximum stack size (bytes)](https://ww2.mathworks.cn/help/rtw/ref/maximumstacksizebytes.html)。

### 生成的代码中测试点的外观

_测试点_是存储在唯一内存位置的信号。有关在模型中包含测试点的信息，请参阅[将信号配置为测试点](https://ww2.mathworks.cn/help/simulink/ug/working-with-test-points.html)。

为包含测试点的模型生成代码时，编译过程会为每个测试点分配单独的内存缓冲区。默认情况下，测试点存储为标准数据结构体的成员，例如 `` `model`_B``。

如果您有 Embedded Coder®：

虚拟总线不显示在生成的代码中，即使与测试点相关联时也是如此。要在生成的代码中显示总线，请使用非虚拟总线或使用通过 [Signal Conversion](https://ww2.mathworks.cn/help/simulink/slref/signalconversion.html) 模块转换为非虚拟总线的虚拟总线。

### 生成的代码中工作区变量的外观

您可以使用_工作区变量_来指定模型中的模块参数值。工作区变量包括您存储在工作区（例如基础工作区）或数据字典中的 MATLAB® 数值变量和 `Simulink.Parameter` 对象。

将**默认参数行为**设置为 “`可调`” 时，默认情况下，工作区变量在生成的代码中显示为全局参数结构体的可调字段。如果使用一个这样的变量指定多个模块参数值，则该变量显示为全局参数结构体的单个字段。代码不会创建多个字段来表示模块参数。因此，在代码执行期间调整字段值会更改模型的数学行为，就像在仿真期间调整 MATLAB 变量或参数对象的值一样。

如果您有 Embedded Coder，则可以通过在代码映射编辑器中指定参数数据类别的代码生成设置来控制工作区变量的默认表示形式（请参阅 [Configure Default Code Generation for Data](https://ww2.mathworks.cn/help/ecoder/ug/configure-default-code-generation-for-categories-of-model-data-and-functions.html#mw_abf23689-bf8b-4b4c-854e-b072022c3b12)）。

*   **模型参数**类别适用于存储在模型工作区中的变量。
    
*   **外部参数**类别适用于存储在基础工作区或数据字典中的变量。
    

### 将内部数据提升到接口

默认情况下，代码生成器假定应用程序中的其他系统和组件不需要访问内部数据。例如，代码生成器会对内部数据进行优化，以从生成的代码中消除它们。对于原型构建和测试目的，您可以通过清除优化或配置测试点并应用存储类来访问内部数据（请参阅 [Preserve Variables in Generated Code](https://ww2.mathworks.cn/help/ecoder/ug/configure-data-accessibility-for-rapid-prototyping.html)）。对于优化的生产代码，将单个数据项配置为作为模型接口的一部分显示在生成的代码中。

#### 您可以提升的数据

根据生成的代码的重入性，即您为**代码接口打包**选择的设置，您可以通过将模型中的每个数据项配置为在代码中显示为下列实体之一来参与接口：

*   全局符号，例如全局变量或对专用函数的调用
    
*   生成的入口函数的形参（参数）
    

下表显示了每个数据类别参与接口时可使用的机制。

<table><colgroup><col width="33%"><col width="33%"><col width="33%"></colgroup><thead><tr><th>数据类别</th><th>显示为全局符号</th><th>显示为入口函数的参数</th></tr></thead><tbody><tr><td>根级别 <span>Inport</span> 或 <span>Outport</span> 模块</td><td>仅适用于非重入模型。</td><td>是。</td></tr><tr><td>连接两个模块的信号</td><td>仅适用于非重入模型。</td><td><p>仅适用于可重入模型，且仅作为结构体字段。</p><p>或者，将信号连接到根级别 <span>Outport</span> 模块。</p></td></tr><tr><td>模块状态</td><td>仅适用于非重入模型。</td><td>仅适用于可重入模型，且仅作为结构体字段。</td></tr><tr><td>数据存储，例如 <span>Data Store Memory</span> 模块</td><td>是。</td><td>仅适用于可重入模型，且仅作为结构体字段。</td></tr><tr><td>模块参数或参数对象，例如 <code>Simulink.Parameter</code></td><td>是。</td><td>仅作为结构体的字段。</td></tr></tbody></table>

#### 单实例算法

对于单实例算法（将**代码接口打包**设置为 “`不可重用函数`”），使用模型数据编辑器或属性检查器将存储类直接应用于单个数据项。使用直接应用的存储类时，数据项在代码中显示为全局符号，例如全局变量。存储类还可以防止优化消除数据项的存储。

您可以将存储类应用于信号、模块状态和模块参数。（对于模块参数，可以通过 `Simulink.Parameter` 等参数对象间接应用存储类）。但是，对于信号，请考虑将信号连接到模型根级别的 Outport 模块。然后，您可以选择将存储类应用于模块。在模块图中，Outport 模块显示信号表示系统输出。

有关存储类的详细信息，请参阅[模型接口元素的 C 代码生成配置](https://ww2.mathworks.cn/help/ecoder/ug/c-code-generation-configuration-for-model-interface-elements.html)。

#### 可重入算法

对于可重入算法（将**代码接口打包**设置为 “`可重用函数`”），可使用不同方法将数据项配置为在代码中体现为生成的入口函数的形参。

### 控制内部数据的默认表示形式 (Embedded Coder)

默认情况下，代码生成器会将优化无法消除的内部数据（如大多数状态数据）聚合到 DWork 结构体等标准结构体中。使用 Embedded Coder，您可以控制生成的代码存储这些数据的方式。

#### 通过插入 pragma 指令控制数据在内存中的位置

使用代码映射编辑器为每个类别的数据（例如属于**内部数据**的状态和信号数据）指定默认内存段。在生成的代码中，您的自定义 pragma 指令或其他装饰元素位于数据定义和声明的前后位置。

您还可以根据模型中的原子子系统对结构体进行分区，以便为子例程和其他算法子组件的数据指定不同的默认内存段。

有关详细信息，请参阅 [Control Data and Function Placement in Memory by Inserting Pragmas](https://ww2.mathworks.cn/help/ecoder/ug/control-data-and-function-placement-in-memory-by-inserting-pragmas.html)。

#### 控制标准数据结构体的类型、字段和全局变量的名称

您可以控制标准数据结构体的某些特性。有关详细信息，请参阅 [Manage Replacement of Simulink Data Types in Generated Code](https://ww2.mathworks.cn/help/ecoder/ug/control-data-type-names-in-generated-code.html)。

要对结构体特性施加更多控制，例如在生成的代码文件中的位置，请使用 Embedded Coder 字典创建自己的结构化存储类。然后，使用代码映射编辑器将存储类应用于数据类别。存储类会删除标准结构体中的数据，从而创建您可以施加更多控制的其他结构体。有关将默认存储类应用于数据类别的详细信息，请参阅 [Configure Default Code Generation for Data](https://ww2.mathworks.cn/help/ecoder/ug/configure-default-code-generation-for-categories-of-model-data-and-functions.html#mw_abf23689-bf8b-4b4c-854e-b072022c3b12)。有关创建存储类的详细信息，请参阅 [Define Service Interfaces, Storage Classes, Memory Sections, and Function Templates for Software Architecture](https://ww2.mathworks.cn/help/ecoder/ug/define-storage-class-memory-section-and-function-template-settings-for-a-software-architecture.html)。

#### 根据子组件将数据组织到结构体中

#### 将信号和参数数据组织成有意义的自定义结构体和子结构体

要将任意信号和参数组织成自定义结构体和子结构体，请创建非虚拟总线信号和参数结构体。（可选）要防止优化消除代码中的数据，请将总线信号或参数结构体的存储类设置为 “`自动`”（默认设置）以外的值。

在向模型添加模块时，必须将每个新信号和参数显式放入总线或结构体中。

有关详细信息，请参阅 [Organize Data into Structures in Generated Code](https://ww2.mathworks.cn/help/ecoder/ug/organize-data-into-structures-in-generated-code.html)。

#### 创建单独的全局变量而不是结构体字段

要使内部数据的类别在生成的代码中显示为单独的非结构化全局变量而不是标准数据结构体的字段，请使用代码映射编辑器将非结构化存储类应用于数据类别。例如，应用存储类 `ExportedGlobal`。但是，如果通过将配置参数**代码接口打包**设置为 “`不可重用函数`” 以外的值来生成多实例可重入代码，则不能将此方法用于某些数据类别（请参阅 [Storage Classes and Reentrant, Multi-Instance Models and Components](https://ww2.mathworks.cn/help/rtw/ug/code-definition-and-mapping-limitations-and-considerations.html#mw_45f83615-8d4e-4dc6-b55c-90bb430c2307)）。

要使用代码映射编辑器将默认存储类应用于数据类别，请参阅 [Configure Default Code Generation for Data](https://ww2.mathworks.cn/help/ecoder/ug/configure-default-code-generation-for-categories-of-model-data-and-functions.html#mw_abf23689-bf8b-4b4c-854e-b072022c3b12)。要选择存储类，请参阅 [Choose Storage Class for Controlling Data Representation in Generated Code](https://ww2.mathworks.cn/help/rtw/ug/choose-a-built-in-storage-class-for-controlling-data-representation-in-the-generated-code.html)。

## 相关主题

*   [Data Structures in the Generated Code](https://ww2.mathworks.cn/help/ecoder/ug/data-structures-in-the-generated-code.html)
*   [How Generated Code Exchanges Data with an Environment](https://ww2.mathworks.cn/help/ecoder/ug/how-generated-code-exchanges-data-with-an-environment.html)
*   [模型接口元素的 C 代码生成配置](https://ww2.mathworks.cn/help/ecoder/ug/c-code-generation-configuration-for-model-interface-elements.html)

您曾对此示例进行过修改。是否要打开带有您的编辑的示例？

本页内容对您有帮助吗？

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