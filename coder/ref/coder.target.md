---
url: https://ww2.mathworks.cn/help/coder/ref/coder.target.html?searchHighlight=coder.target(%27MATLAB%27)&s_tid=srchtitle_support_results_1_coder.target(%2527MATLAB%2527)
title: 确定代码生成目标是否为指定的目标 - MATLAB coder.target
- MathWorks 中国
date: 2023-08-03 11:49:19
tag: 
summary: 此 MATLAB 函数 返回 true (1)。否则，它返回 false (0)。
---
参数化 MATLAB 函数，以便它在 MATLAB 或生成的代码中工作。当函数在 MATLAB 中运行时，它会调用 MATLAB 函数 `myabsval`。然而，生成的代码会调用 C 库函数 `myabsval`。

编写 MATLAB 函数 `myabsval`。

```
function y = myabsval(u)   
%#codegen
y = abs(u);

```

通过使用 `-args` 选项指定输入参数的大小、类型和复 / 实性，为 `myabsval` 生成 C 静态库。

```
codegen -config:lib myabsval -args {0.0}


```

`codegen` 函数在文件夹 `\codegen\lib\myabsval` 中创建库文件 `myabsval.lib` 和头文件 `myabsval.h`。（库文件扩展名可以根据您的平台而有所不同。）它在同一个文件夹中生成函数 `myabsval_initialize` 和 `myabsval_terminate`。

编写一个 MATLAB 函数，以使用 `coder.ceval` 调用生成的 C 库函数。

```
function y = callmyabsval(y)  
%#codegen
% Check the target. Do not use coder.ceval if callmyabsval is
% executing in MATLAB
if coder.target('MATLAB')
  % Executing in MATLAB, call function myabsval
  y = myabsval(y);
else
  % add the required include statements to generated function code
  coder.updateBuildInfo('addIncludePaths','$(START_DIR)\codegen\lib\myabsval');
  coder.cinclude('myabsval_initialize.h');
  coder.cinclude('myabsval.h');
  coder.cinclude('myabsval_terminate.h');

  % Executing in the generated code. 
  % Call the initialize function before calling the 
  % C function for the first time
  coder.ceval('myabsval_initialize');

  % Call the generated C library function myabsval
  y = coder.ceval('myabsval',y);
  
  % Call the terminate function after
  % calling the C function for the last time
  coder.ceval('myabsval_terminate');
end


```

生成 MEX 函数 `callmyabsval_mex`。在命令行中提供生成的库文件。

```
codegen -config:mex callmyabsval codegen\lib\myabsval\myabsval.lib -args {-2.75}


```

您可以使用 [`coder.updateBuildInfo`](https://ww2.mathworks.cn/help/coder/ref/coder.updatebuildinfo.html) 在函数中指定该库，而不是在命令行中提供库。使用此选项可预配置编译。将以下行添加到 `else` 模块中：

```
coder.updateBuildInfo('addLinkObjects','myabsval.lib','$(START_DIR)\codegen\lib\myabsval',100,true,true);


```

**注意**

只有使用 MATLAB Coder™ 生成代码时，才支持 `START_DIR` 宏。

运行调用库函数 `myabsval` 的 MEX 函数 `callmyabsval_mex`。

调用 MATLAB 函数 `callmyabsval`。

`callmyabsval` 函数在 MATLAB 和代码生成中的执行均展示出了预期的行为。