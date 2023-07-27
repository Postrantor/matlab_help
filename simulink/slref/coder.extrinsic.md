---
url: https://ww2.mathworks.cn/help/simulink/slref/coder.extrinsic.html
title: untitled
date: 2023-07-26 14:15:39
tag: 
summary: 此 MATLAB 函数 将 function 声明为外部函数。
---
将函数声明为外部函数，并在 MATLAB 中执行它

## 说明

[示例](https://ww2.mathworks.cn/help/simulink/slref/coder.extrinsic.html#mw_9ee8a1d8-897d-4f95-8bf9-7fa71d45fcc1)

``coder.extrinsic([`function`](#mw_7fd3bed0-bfde-42c2-9075-74d0b2196e20))`` 将 `function` 声明为外部函数。代码生成器不为外部函数体生成代码，而是使用 MATLAB® 引擎来执行调用。在执行期间，仅当 MATLAB 引擎可用时，此功能才可用。MATLAB 引擎可用的情形示例包括 MEX 函数的执行、Simulink® 仿真或代码生成时的函数调用（也称为编译时）。

在独立代码生成过程中，代码生成器会尝试确定外部函数是否只具有副作用（例如，通过显示绘图），或它是否会影响调用时所在函数的输出（例如，通过将值返回到输出变量）。如果输出没有改变，代码生成器会继续执行代码生成，但会从生成的代码中排除外部函数。否则，代码生成器会产生编译错误。

对于您使用 `coder.extrinsic` 声明为外部函数的函数，不能使用 `coder.ceval`。此外，在代码生成期间外，系统会忽略 `coder.extrinsic` 指令。

请参阅[使用 MATLAB 引擎在生成的代码中执行函数调用](https://ww2.mathworks.cn/help/simulink/ug/use-matlab-engine-to-execute-a-function-call-in-generated-code.html)。

**注意**

代码生成器自动将许多常见的 MATLAB 可视化函数（例如 `plot`、`disp` 和 `figure`）视为外部函数。您不必使用 `coder.extrinsic` 将它们显式声明为外部函数。

`coder.extrinsic(function1, ... ,functionN)` 通过 `functionN` 将 `function1` 声明为外部函数。

``coder.extrinsic(`'-sync:on'`, function1, ... ,functionN)`` 允许在调用外部函数 `function1` 一直到 `functionN` 之前和之后，在 MATLAB 执行和生成的代码执行或 Simulink 仿真之间同步全局数据。如果只有几个外部调用使用或修改全局数据，可通过将全局同步模式设置为 `At MEX-function entry and exit` 来关闭所有外部函数调用前后的同步过程。然后再使用 `'-sync:on'` 选项只为确实修改全局数据的外部调用打开同步功能。

如果使用 MATLAB Coder™ 生成 MEX 函数，则 `'-sync:on'` 选项允许在调用外部函数后验证 MATLAB 和 MEX 函数之间的常量全局数据的一致性。

请参阅 [Generate Code for Global Data](https://ww2.mathworks.cn/help/coder/ug/code-generation-for-global-data.html) (MATLAB Coder)。

``coder.extrinsic(`'-sync:off'`, function1, ... ,functionN)`` 禁止在调用外部函数 `function1` 一直到 `functionN` 之前和之后，在 MATLAB 执行和生成的代码执行之间同步全局数据。如果大多数外部调用都使用或修改全局数据，但有几个不修改，则可以使用 `'-sync:off'` 选项关闭不修改全局数据的外部调用的同步。

如果使用 MATLAB Coder 生成 MEX 函数，则 `'-sync:off'` 选项禁止在调用外部函数后验证 MATLAB 和 MEX 函数之间的常量全局数据的一致性。

请参阅 [Generate Code for Global Data](https://ww2.mathworks.cn/help/coder/ug/code-generation-for-global-data.html) (MATLAB Coder)。

## 示例

全部折叠

代码生成不支持 MATLAB 函数 [`patch`](https://ww2.mathworks.cn/help/matlab/ref/patch.html)。此示例说明如何通过将 `patch` 声明为 MATLAB 函数的外部函数，仍可以在生成的 MEX 函数中使用 `patch` 的功能。

此 MATLAB 代码在局部函数 `create_plot` 中将 `patch` 声明为外部函数。通过将 `patch` 声明为外部函数，您指示代码生成器不要为 `patch` 生成代码。在这种情况下，代码生成器将 `patch` 调度给 MATLAB 来执行。

代码生成器自动将许多常见的 MATLAB 可视化函数（例如此代码使用的函数 [`axis`](https://ww2.mathworks.cn/help/matlab/ref/axis.html)）视为外部函数。

```
function c = pythagoras(a,b,color) %#codegen
% Calculate the hypotenuse of a right triangle
% and display the triangle as a patch object. 
c = sqrt(a^2 + b^2);
create_plot(a, b, color);
end

function create_plot(a, b, color)
%Declare patch as extrinsic
coder.extrinsic('patch'); 
x = [0;a;a];
y = [0;0;b];
patch(x,y,color);
axis('equal');
end

```

**注意**

以下代码调用 `patch` 而不请求任何输出参数。生成独立代码时，代码生成器会忽略此类调用。

为 `pythagoras` 生成一个 MEX 函数。此外，还生成代码生成报告。

```
codegen pythagoras -args {1, 1, [.3 .3 .3]} -report

```

在报告中，查看 `create_plot` 的 MATLAB 代码。

![](https://ww2.mathworks.cn/help/simulink/slref/extrinsic_function2_zh_CN.png)

报告突出显示了 `patch` 和 `axis` 函数，以表明它们被视为外部函数。

运行 MEX 函数。

```
pythagoras_mex(3, 4, [1.0 0.0 0.0]);

```

MATLAB 将直角三角形的绘图显示为红色补片对象。

![](https://ww2.mathworks.cn/help/simulink/slref/ex_extrinsic_patch_zh_CN.png)

**注意**

您也可以将函数 `pythagoras` 放在 Simulink 模型的 MATLAB Function 模块中，而不是使用 `codegen` 命令生成 MEX 文件。仿真模型时，MATLAB Function 模块的行为与 `pythagoras_mex` 相似。

外部函数在运行时返回的输出是 `mxArray`，也称为 MATLAB 数组。对 `mxArray` 有效的操作只有下列几个：将其存储在变量中，将其传递给另一个外部函数，或将其返回到 MATLAB。要对 `mxArray` 值执行任何其他操作，例如在代码的表达式中使用它，您必须在运行时将 `mxArray` 转换为已知类型。要执行此操作，请将 `mxArray` 赋给变量，此变量的类型已由以前的赋值定义。

此示例说明如何将外部函数的 `mxArray` 输出直接返回到 MATLAB。下一个示例说明如何将相同的 `mxArray` 输出转换为已知类型，然后在 MATLAB 函数内部的表达式中使用它。

**定义入口函数**

定义 MATLAB 函数 `return_extrinsic_output`，该函数接受有向图的源节点和目标节点索引作为输入，并使用 `hascycles` 函数确定该图是否为无环图。代码生成不支持 `hascycles` 函数，该函数声明为外部函数。

```
type return_extrinsic_output.m

```

```
function hasCycles = return_extrinsic_output(source,target)
coder.extrinsic('hascycles');
assert(numel(source) == numel(target))
G = digraph(source,target);
hasCycles = hascycles(G);
end


```

**生成并调用 MEX 函数**

生成 `return_extrinsic_output` 的 MEX 代码。将输入指定为 `double` 类型的无界向量。

```
codegen return_extrinsic_output -args {coder.typeof(0,[1 Inf]),coder.typeof(0,[1 Inf])} -report

```

```
Code generation successful: To view the report, open('codegen/mex/return_extrinsic_output/html/report.mldatx')


```

用合适的输入调用生成的 MEX 函数 `return_extrinsic_output_mex`：

```
return_extrinsic_output([1 2 4 4],[2 3 3 1])

```

要直观地检查有向图是否有循环，请在 MATLAB 中绘制有向图。

```
plot(digraph([1 2 4 4],[2 3 3 1]))

```

![](https://ww2.mathworks.cn/help/simulink/slref/returnextrinsicoutputruntimeexample_01_zh_CN.png)

外部函数返回的输出是 `mxArray`，也称为 MATLAB 数组。对 `mxArray` 有效的操作只有下列几个：将其存储在变量中，将其传递给另一个外部函数，或将其返回到 MATLAB。要对 `mxArray` 值执行任何其他操作，例如在代码的表达式中使用它，请在运行时将 `mxArray` 转换为已知类型。要执行此操作，请将 `mxArray` 赋给变量，此变量的类型已由以前的赋值定义。

此示例说明如何将外部函数的 `mxArray` 输出转换为已知类型，然后在 MATLAB 函数内部的表达式中使用此输出。

**定义入口函数**

定义 MATLAB 函数 `use_extrinsic_output`，该函数接受有向图的源节点和目标节点索引作为输入，并使用 `hascycles` 函数确定该图是否为无环图。代码生成不支持 `hascycles` 函数，该函数声明为外部函数。入口函数根据 `hascycles` 函数的输出显示一条消息。

```
type use_extrinsic_output

```

```
function use_extrinsic_output(source,target) %#codegen
assert(numel(source) == numel(target))
G = digraph(source,target);

coder.extrinsic('hascycles');
hasCycles = true;

hasCycles = hascycles(G);
if hasCycles == true
    disp('The graph has cycles')
else
    disp('The graph does not have cycles')
end
end


```

在进行 `hasCycles = hascycles(G)` 赋值之前，先给局部变量 `hasCycles` 预赋布尔值 `true`。这种预赋值使代码生成器能够将外部函数 `hascycles` 返回的 `mxArray` 转换为布尔值，然后将其赋给 `hasCycles` 变量。此转换又使您能够在 `if` 语句的条件下将 `hasCycles` 与布尔值 `true` 进行比较。

**生成并调用 MEX 函数**

生成 `use_extrinsic_output` 的 MEX 代码。将输入指定为双精度类型的无界向量。

```
codegen use_extrinsic_output -args {coder.typeof(0,[1 Inf]),coder.typeof(0,[1 Inf])} -report

```

```
Code generation successful: To view the report, open('codegen/mex/use_extrinsic_output/html/report.mldatx')


```

用合适的输入调用生成的 MEX 函数 `use_extrinsic_output_mex`：

```
use_extrinsic_output_mex([1 2 4 4],[2 3 3 1])

```

```
The graph does not have cycles


```

要查看有向图是否包含循环，请在 MATLAB 中绘制该图。

```
plot(digraph([1 2 4 4],[2 3 3 1]))

```

![](https://ww2.mathworks.cn/help/simulink/slref/useextrinsicoutputinexpressionexample_01_zh_CN.png)

### 使用 `coder.const` 在编译时计算外部函数调用

此示例说明如何使用 [`coder.const`](https://ww2.mathworks.cn/help/simulink/slref/coder.const.html) 在代码生成时（也称为编译时）调用外部函数。由于 MATLAB 引擎在计算 `coder.const` 内表达式的期间始终可用，因此在生成 MEX 或独立代码时可以使用这种编码模式。与展示运行时执行的前两个示例不同，如果外部函数的计算在编译时发生，则不需要将外部函数的输出显式转换为已知类型。

在此示例中，入口函数 `rotate_complex` 调用另一个使用 MATLAB API 进行 XML 处理的函数 `xml2struct`。由于代码生成不支持使用 MATLAB API 进行 XML 处理，因此 `xml2struct` 函数在入口函数的主体中声明为外部函数。此外，入口函数中对 `xml2struct` 的调用返回编译时常量。因此，通过将函数调用放在 `coder.const` 指令中，实现了对此输出的常量折叠。

**检查包含参数的 XML 文件**

支持文件 `complex.xml` 包含复数的实部和虚部的值。

```
<params>
    <param name="real" value="3"/>
    <param name="imaginary" value="4"/>
</params>


```

**定义 `xml2struct` 函数**

MATLAB 函数 `xml2struct` 读取一个 XML 文件，该文件使用 `complex.xml` 的格式存储参数名称和值，将这些信息存储为结构体字段，并返回该结构体。

```
function s = xml2struct(file)
s = struct();
import matlab.io.xml.dom.*
doc = parseFile(Parser,file);
els = doc.getElementsByTagName("params");
for i = 0:els.getLength-1
    it = els.item(i);
    ps = it.getElementsByTagName("param");
    for j = 0:ps.getLength-1
        param = ps.item(j);
        paramName = char(param.getAttribute("name"));
        paramValue = char(param.getAttribute("value"));
        paramValue = evalin("base", paramValue);
        s.(paramName) = paramValue;
    end
end


```

**定义入口函数**

您的 MATLAB 入口函数 `rotate_complex` 首先调用 `xml2struct` 来读取文件 `complex.xml`。然后，它将复数旋转一定角度，该角度等于输入参数 `theta` 的度数，并返回得到的复数。

```
function y = rotate_complex(theta) %#codegen
coder.extrinsic("xml2struct");
s = coder.const(xml2struct("complex.xml"));

comp = s.real + 1i * s.imaginary;
magnitude = abs(comp);
phase = angle(comp) + deg2rad(theta);
y = magnitude * cos(phase) + 1i * sin(phase);

end


```

`xml2struct` 函数声明为外部函数，通过将该函数放在 `coder.const` 指令内，实现对输出的常量折叠。

**生成和检查静态库**

使用 [`codegen`](https://ww2.mathworks.cn/help/coder/ref/codegen.html) (MATLAB Coder) 命令为 `read_complex` 生成静态库。将输入类型指定为双精度标量。

```
codegen -config:lib rotate_complex -args {0} -report 

```

```
Code generation successful: To view the report, open('codegen/lib/rotate_complex/html/report.mldatx')


```

检查生成的 C++ 文件 `rotate_complex.c`。注意 `xml2struct` 函数的输出硬编码在生成的代码中。

```
type codegen/lib/rotate_complex/rotate_complex.c

```

```
/*
 * rotate_complex.c
 *
 * Code generation for function 'rotate_complex'
 *
 */

/* Include files */
#include "rotate_complex.h"
#include <math.h>

/* Function Definitions */
creal_T rotate_complex(double theta)
{
  creal_T y;
  double y_tmp;
  y_tmp = 0.017453292519943295 * theta + 0.92729521800161219;
  y.re = 5.0 * cos(y_tmp);
  y.im = sin(y_tmp);
  return y;
}

/* End of code generation (rotate_complex.c) */


```

## 输入参数

全部折叠

### `function` — MATLAB 函数名称  
字符向量

声明为外部函数的 MATLAB 函数的名称。

**示例：** `coder.extrinsic('patch')`

**数据类型：** `char`

## 限制

*   外部函数调用有一些开销可能会影响性能。在外部函数调用中传递的输入数据必须提供给 MATLAB，它需要制作数据副本。如果该函数具有任何输出数据，这些数据必须传输回 MEX 函数环境，它也需要副本。
    
*   代码生成器不支持使用 `coder.extrinsic` 来调用位于私有文件夹中的函数。
    
*   代码生成器不支持使用 `coder.extrinsic` 来调用局部函数。
    

## 提示

*   代码生成器自动将许多常见的 MATLAB 可视化函数（例如 `plot`、`disp` 和 `figure`）视为外部函数。您不必使用 `coder.extrinsic` 将它们显式声明为外部函数。
    
*   使用 `coder.screener` 函数可以检测哪些函数必须声明为外部函数。此函数运行代码生成就绪工具，它会筛查 MATLAB 代码中是否存在代码生成不支持的功能和函数。
    

## 扩展功能

### C/C++ 代码生成  
使用 MATLAB® Coder™ 生成 C 代码和 C++ 代码。

### GPU 代码生成  
使用 GPU Coder™ 为 NVIDIA® GPU 生成 CUDA® 代码。

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

![](https://ww2.mathworks.cn/images/responsive/global/pic-header-mathworks-logo2.svg)

Select a Web Site

Choose a web site to get translated content where available and see local events and offers. Based on your location, we recommend that you select: .

You can also select a web site from the following list:

[Contact your local office](#)