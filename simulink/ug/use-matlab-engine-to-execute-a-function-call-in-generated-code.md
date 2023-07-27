---
url: https://ww2.mathworks.cn/help/simulink/ug/use-matlab-engine-to-execute-a-function-call-in-generated-code.html
title: 使用 MATLAB 引擎在生成的代码中执行函数调用
- MATLAB & Simulink
- MathWorks 中国
date: 2023-07-26 14:17:29
tag: 
summary: 如果代码生成不支持某个函数，请将其声明为外部函数以在 MATLAB 中执行。
---

## 使用 MATLAB 引擎在生成的代码中执行函数调用

在 MATLAB® 代码中处理对函数 `foo` 的调用时，代码生成器会找到 `foo` 的定义并为其函数体生成代码。**在某些情况下，您可能希望绕过代码生成，而是使用 MATLAB 引擎来执行调用。** 使用 `coder.extrinsic('foo')` 声明对 `foo` 的调用不生成代码，而是使用 MATLAB 引擎执行。在此上下文中，`foo` 称为外部函数。在执行期间，仅当 MATLAB 引擎可用时，此功能才可用。这种情况的示例包括 MEX 函数的执行、Simulink® 仿真或代码生成时的函数调用（也称为*编译时*）。

如果为调用 `foo` 并包含 `coder.extrinsic('foo')` 的函数生成独立代码，代码生成器将尝试确定 `foo` 是否影响输出。如果 `foo` 不影响输出，则代码生成器继续生成代码，但会从生成的代码中排除 `foo`。否则，代码生成器会产生编译错误。

将 `coder.extrinsic('foo')` 指令包含在某特定 MATLAB 函数中会将该 MATLAB 函数中对 `foo` 的所有调用都声明为外部调用。或者，您可能希望将外部声明的范围缩小到仅对 `foo` 的一次调用。请参阅[使用 feval 调用 MATLAB 函数](https://ww2.mathworks.cn/help/coder/ug/use-matlab-engine-to-execute-a-function-call-in-generated-code.html#bq1h2z9-44) (MATLAB Coder)。

![](https://ww2.mathworks.cn/help/simulink/ug/intrinsic_rules_matlab_function_calls.gif)

### 将函数声明为外部函数的情形

以下是您可能会考虑将 MATLAB 函数声明为外部函数的一些常见情况：

- 函数执行显示或记录操作。此类函数主要在仿真期间有用，在嵌入式系统中不使用。
- 在您的 MEX 执行或 Simulink 仿真中，您要使用代码生成不支持的 MATLAB 函数。此工作流不适用于非仿真目标。
- 您使用 [`coder.const`](https://ww2.mathworks.cn/help/simulink/slref/coder.const.html) 指示代码生成器对函数调用进行常量折叠。在这种情况下，仅当 MATLAB 引擎可用于执行调用时，才会在代码生成期间调用该函数。

### 使用 `coder.extrinsic` 构造

要将函数 `foo` 声明为外部函数，请在您的 MATLAB 代码中包含以下语句。

当将函数声明为代码生成的外部函数时，请遵守以下规则：

- 在调用函数之前将其声明为外部函数。
- 不要在条件语句中使用外部声明。
- 将外部函数的返回值赋给已知类型。请参阅[使用 mxArray](https://ww2.mathworks.cn/help/coder/ug/use-matlab-engine-to-execute-a-function-call-in-generated-code.html#bq1h2z9-46) (MATLAB Coder)。

有关其他信息和示例，请参阅 [`coder.extrinsic`](https://ww2.mathworks.cn/help/simulink/slref/coder.extrinsic.html)。

代码生成器自动将许多常见的 MATLAB 可视化函数（例如 `plot`、`disp` 和 `figure`）视为外部函数。您不必使用 `coder.extrinsic` 将它们显式声明为外部函数。例如，您可能希望通过调用 [`plot`](https://ww2.mathworks.cn/help/matlab/ref/plot.html) 来可视化 MATLAB 环境中的结果。如果您从调用 `plot` 的函数中生成一个 MEX 函数并运行该 MEX 函数，代码生成器会将对 `plot` 函数的调用调度给 MATLAB 引擎。如果您生成一个库或可执行文件，生成的代码中将不包含对 `plot` 函数的调用。

如果您使用 MATLAB Coder™ 生成 MEX 或独立 C/C++ 代码，代码生成报告将突出显示您的 MATLAB 代码对外部函数的调用。通过检查报告，您可以确定哪些函数仅在 MATLAB 环境中受支持。

![](https://ww2.mathworks.cn/help/simulink/ug/extrinsic_function.png)

#### 外部函数声明的作用域

`coder.extrinsic` 构造具有函数作用域。以如下代码为例：

```
function y = foo %#codegen
coder.extrinsic('rat','min');
[N D] = rat(pi);
y = 0;
y = min(N, D);


```

在此示例中，`rat` 和 `min` 每次在主函数 `foo` 中调用时都被视为外部函数。有两种方式可以缩小外部函数声明在主函数内的作用域：

- 在局部函数中将 MATLAB 函数声明为外部函数，如以下示例中所示：

  ```
  function y = foo %#codegen
  coder.extrinsic('rat');
  [N D] = rat(pi);
  y = 0;
  y = mymin(N, D);

  function y = mymin(a,b)
  coder.extrinsic('min');
  y = min(a,b);


  ```

  这里，函数 `rat` 每次在主函数 `foo` 内调用时都是外部函数，但函数 `min` 只有在局部函数 `mymin` 内调用时才是外部函数。

- 请使用 [`feval`](https://ww2.mathworks.cn/help/matlab/ref/feval.html) 调用 MATLAB 函数，而不是使用 `coder.extrinsic` 构造。下一节将介绍这种方法。

#### 对非静态方法进行外部声明

假设您定义一个具有非静态方法 `foo` 的类 `myClass`，然后创建此类的实例 `obj`。如果要在用于代码生成的 MATLAB 代码中将方法 `obj.foo` 声明为外部方法，请遵循以下规则：

- 将对 `foo` 的调用编写为函数调用。不要使用圆点表示法来编写调用。
- 使用语法 `coder.extrinsic('foo')` 将 `foo` 声明为外部函数。

例如，将 `myClass` 定义为：

```
classdef myClass
    properties
        prop = 1
    end
    methods
        function y = foo(obj,x)
            y = obj.prop + x;
        end
    end
end


```

下面是将 `foo` 声明为外部函数的 MATLAB 函数示例。

```
function y = myFunction(x) %#codegen
coder.extrinsic('foo');
obj = myClass;
y = foo(obj,x);
end


```

非静态方法也称为普通方法。请参阅[方法语法](https://ww2.mathworks.cn/help/matlab/matlab_oop/specifying-methods-and-functions.html)。

#### 其他用途

使用 `coder.extrinsic` 构造可以：

- 调用在仿真过程中不会生成输出，从而不会生成不必要代码的 MATLAB 函数。
- 使您的代码具有自说明，从而更容易调试。您可以扫描 `coder.extrinsic` 语句的源代码以隔离对 MATLAB 函数的调用，它们可能会创建并传播 `mxArrays`。请参阅[使用 mxArray](https://ww2.mathworks.cn/help/coder/ug/use-matlab-engine-to-execute-a-function-call-in-generated-code.html#bq1h2z9-46) (MATLAB Coder)。

### 使用 `feval` 调用 MATLAB 函数

要将外部声明的作用域缩小到近一个函数调用，请使用函数 [`feval`](https://ww2.mathworks.cn/help/matlab/ref/feval.html)。`feval` 在代码生成过程中自动解释为外部函数。因此，您可以使用 `feval` 调用要在 MATLAB 环境中执行的函数，而不用编译为生成的代码。

请参考如下示例：

```
function y = foo
coder.extrinsic('rat');
[N D] = rat(pi);
y = 0;
y = feval('min',N,D);


```

因为 `feval` 是外部函数，所以语句 `feval('min',N,D)` 由 MATLAB 计算（而不进行编译），这样产生的结果与专门为此调用将 `min` 声明为外部函数相同。相比之下，函数 `rat` 在整个函数 `foo` 中都是外部函数。

代码生成器不支持使用 `feval` 来调用局部函数或位于私有文件夹中的函数。

### 使用 mxArray

外部函数的运行时输出是 `mxArray`，也称为 MATLAB 数组。对 `mxArrays` 有效的操作只有下列几个：

- 在变量中存储 `mxArray`。
- 将 `mxArray` 传递给外部函数。
- 将函数中的 `mxArray` 返回到 MATLAB。
- 运行时将 `mxArray` 转换为已知类型。请将 `mxArray` 赋给变量，此变量的类型已由以前的赋值定义。请参阅以下示例。

要在其他操作中使用外部函数返回的 `mxArray`（例如，从 MATLAB Function 模块返回到 Simulink 执行），必须首先将其转换为已知类型。

如果函数的输入参数是 `mxArrays`，代码生成器会自动将该函数视为外部函数。

#### 将 mxArray 转换为已知类型

要将 `mxArray` 转换为已知类型，请将 `mxArray` 指定给已定义类型的变量。在运行时，`mxArray` 将被转换为其指定给的变量的类型。如果 `mxArray` 中的数据与该变量的类型不一致，将会发生运行时错误。

以如下代码为例：

```
function y = foo %#codegen
coder.extrinsic('rat');
[N D] = rat(pi);
y = min(N,D);


```

这里，顶层函数 `foo` 调用外部 MATLAB 函数 `rat`，后者返回两个 `mxArrays`，分别代表 `pi` 的有理分式逼近式的分子 `N` 和分母 `D`。您可以将这些 `mxArrays` 传递给另一个 MATLAB 函数，在本例中为 `min`。由于传递给 `min` 的输入是 `mxArrays`，代码生成器会自动将 `min` 视为外部函数。因此，`min` 返回 `mxArray`。

使用 MATLAB Coder 生成 MEX 函数时，您可以将 `min` 返回的此 `mxArray` 直接赋给输出 `y`，因为 MEX 函数将其输出返回到 MATLAB。

```
Code generation successful.

```

但是，如果您将 `foo` 放在 Simulink 模型的 MATLAB Function 模块中，然后更新或运行该模型，则会出现以下错误：

```
Function output 'y' cannot be an mxArray in this context.
Consider preinitializing the output variable with a known type.


```

发生此错误是因为不支持将 `mxArray` 返回到 Simulink。要解决此问题，请将 `y` 定义为您希望 `min` 返回的值的类型和大小，本例中为双精度标量值：

```
function y = foo %#codegen
coder.extrinsic('rat');
[N D] = rat(pi);
y = 0; % Define y as a scalar of type double
y = min(N,D);

```

在此示例中，外部函数 `min` 的输出会影响您为其生成代码的入口函数 `foo` 的输出 `y`。如果您尝试为 `foo` 生成独立代码（例如静态库），代码生成器将无法忽略外部函数调用，并产生代码生成错误。

```
??? The extrinsic function 'min' is not available for
standalone code generation. It must be eliminated for
stand-alone code to be generated. It could not be
eliminated because its outputs appear to influence the
calling function. Fix this error by not using 'min'
or by ensuring that its outputs are unused.

Error in ==> foo Line: 4 Column: 5
Code generation failed: View Error Report

Error using codegen

```

### 使用外部函数的限制

代码生成过程中不支持完整的 MATLAB 运行时环境。因此，从外部调用 MATLAB 函数时存在以下限制：

- 用来检查调用方或读取 / 写入调用方工作区的 MATLAB 函数在代码生成过程中不起作用。此类函数包括：
- 如果您的外部函数在运行时执行以下操作，则生成的代码中的函数可能会产生不可预知的结果：

  - 更改文件夹
  - 更改 MATLAB 路径
  - 删除或添加 MATLAB 文件
  - 更改警告状态
  - 更改 MATLAB 预设
  - 更改 Simulink 参数

- 代码生成器不支持使用 `coder.extrinsic` 来调用位于私有文件夹中的函数。
- 代码生成器不支持使用 `coder.extrinsic` 来调用局部函数。
- 您可以调用最多具有 64 个输入和 64 个输出的外部函数。

## 另请参阅

[`coder.extrinsic`](https://ww2.mathworks.cn/help/simulink/slref/coder.extrinsic.html) | [`coder.const`](https://ww2.mathworks.cn/help/simulink/slref/coder.const.html)
