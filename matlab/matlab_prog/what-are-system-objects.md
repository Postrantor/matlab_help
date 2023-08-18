---
url: https://ww2.mathworks.cn/help/matlab/matlab_prog/what-are-system-objects.html?searchHighlight=system%20object&s_tid=srchtitle_support_results_1_system%20object
title: 何谓 System object？
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-03 16:39:47
tag: 
summary: System object 是一种专用的 MATLAB 对象。System object 专为实现和仿真输入随时间变化的动态系统而设计。
---
## 何谓 System object？

System object™ 是一种专用的 MATLAB® 对象。许多工具箱中都包含 System object。System object 专为实现和仿真输入随时间变化的动态系统而设计。许多信号处理、通信和控制系统都是动态的。在动态系统中，输出信号的值同时取决于输入信号的瞬时值以及系统的过往行为。System object 使用内部状态来存储下一个计算步骤中使用的系统过往行为。因此，System object 非常适用于分段处理大型数据流的迭代计算，例如视频和音频处理系统。这种处理流化数据的功能具有不必在内存中保存大量数据的优点。采用流化数据，您还可以使用可高效利用循环的简化程序。

例如，您可以在系统中使用 System object，以便从某个文件中读取数据、对该数据进行滤波，然后将滤波后的输出写入其他文件。通常，每次循环迭代中都会将指定数量的数据传递给滤波器。文件读取器对象使用状态来跟踪在文件中开始下一次数据读取的位置。同样，文件写入器对象会跟踪其最后将数据写入输出文件的位置，以使数据不会被覆盖。滤波器对象保留其自身的内部状态，以确保滤波正常执行。下图表示系统的单个循环。

![](https://ww2.mathworks.cn/help/matlab/matlab_prog/sys_obj_filter_flow.png)

这些优点使得 System object 适用于处理流化数据。

许多 System object 支持：

*   定点算术运算（需要 Fixed-Point Designer™ 许可证）
    
*   C 代码生成（需要 MATLAB Coder™ 或 Simulink® Coder 许可证）
    
*   HDL 代码生成（需要 HDL Coder™ 许可证）
    
*   可执行文件或共享库生成（需要 MATLAB Compiler™ 许可证）
    

**注意**

查看产品文档以确认要使用的特定 System object 对定点、代码生成和 MATLAB Compiler 的支持。

System object 至少使用两个命令来处理数据：

*   创建对象（如 `fft256 = dsp.FFT`）
    
*   通过对象运行数据（如 `fft256(x)`）
    

通过将创建与执行分离，您可以创建多个持久的可重用对象，并且每个对象具有不同的设置。使用此方法可避免重复进行输入确认和验证、便于在编程循环中使用并提高整体性能。相比较而言，MATLAB 函数必须在您每次调用时对参数进行验证。

除了系统工具箱提供的 System object 以外，您还可以创建自己的 System object。请参阅[创建 System object](https://ww2.mathworks.cn/help/matlab/create-system-objects.html)。

### 运行 System object

要运行某个 System object 并执行其算法定义的操作，您可以调用该对象，就好像它是一个函数一样。例如，要创建一个 FFT 对象（该对象使用 `dsp.FFT` System object、将长度指定为 1024 并命名为 `dft`），请使用：

```
dft = dsp.FFT('FFTLengthSource','Property','FFTLength',1024);


```

要使用输入 `x` 运行此对象，请使用：如果您在不带任何输入参数的情况下运行 System object，则必须包含空的圆括号。例如，`asysobj()`。

当您运行某个 System object 时，该对象还会执行与数据处理有关的其他重要任务，例如初始化和处理对象状态。

**注意**

运行 System object 的替代方法是使用 `step` 函数。例如，对于使用 `dft = dsp.FFT` 创建的对象，您可以通过 `step(dft,x)` 来运行该对象。

### System object 函数

在创建 System object 之后，可以使用各种对象函数来处理该对象的数据，或获取有关该对象的信息。使用函数的语法为 `<object function name>(<system object name>)`，加上可能的额外输入参数。例如，对于 `txfourier = dsp.FFT`（其中 `txfourier` 为所分配的名称），可使用 `reset(txfourier)` 来调用 `reset` 函数。

#### 常见对象函数

所有 System object 都支持以下对象函数。如果某个函数不适用于特定对象，调用该函数不会对此对象有任何作用。

<table><colgroup><col width="28%"><col width="72%"></colgroup><thead><tr><th>函数</th><th>描述</th></tr></thead><tbody><tr><td>运行对象函数，或<br><a href="https://ww2.mathworks.cn/help/matlab/ref/step.html"><code>step</code></a></td><td><p>运行相应对象以使用该对象定义的算法来处理数据。</p><p><span><em>示例</em></span>：对于 <code>dft = dsp.FFT;</code> 对象，通过以下方式运行对象：</p><div><ul><li><p><code>y = dft(x)</code></p></li><li><p><code>y = step(dft,x)</code></p></li></ul></div><p>在此处理过程中，该对象会根据需要初始化资源、返回输出并更新对象状态。在执行过程中，您只能更改可调属性。运行 <span>System</span> <span>object</span> 的两种方法都会返回常规的 MATLAB 变量。</p></td></tr><tr><td><a href="https://ww2.mathworks.cn/help/matlab/ref/matlab.system.releasesystemobject.html"><code>release</code></a></td><td><p>释放资源并允许更改 <span>System</span> <span>object</span> 属性值以及 <span>System</span> <span>object</span> 正在使用时受限的附加特性。</p></td></tr><tr><td><a href="https://ww2.mathworks.cn/help/matlab/ref/resetsystemobject.html"><code>reset</code></a></td><td>将 <span>System</span> <span>object</span> 重置为该对象的初始值。</td></tr><tr><td><a href="https://ww2.mathworks.cn/help/matlab/ref/matlab.system.nargin.html"><code>nargin</code></a></td><td>返回 <span>System</span> <span>object</span> 算法定义接受的输入数目。如果算法定义中包含 <code>varargin</code>，则 <code>nargin</code> 输出为负数。</td></tr><tr><td><a href="https://ww2.mathworks.cn/help/matlab/ref/matlab.system.nargout.html"><code>nargout</code></a></td><td>返回 <span>System</span> <span>object</span> 算法定义接受的输出数目。如果算法定义中包含 <code>varargout</code>，则 <code>nargout</code> 输出为负数。</td></tr><tr><td><a href="https://ww2.mathworks.cn/help/matlab/ref/matlab.system.clone.html"><code>clone</code></a></td><td>使用相同的属性值创建另一个具有同一类型的对象</td></tr><tr><td><a href="https://ww2.mathworks.cn/help/matlab/ref/matlab.system.islocked.html"><code>isLocked</code></a></td><td>返回一个逻辑值，指示对象是否已被调用并且尚未对该对象调用 <code>release</code>。</td></tr><tr><td><a href="https://ww2.mathworks.cn/help/matlab/ref/isdone.html"><code>isDone</code></a></td><td>仅适用于从 <a href="https://ww2.mathworks.cn/help/matlab/ref/matlab.system.mixin.finitesource-class.html"><code>matlab.<span>system</span>.mixin.FiniteSource</code></a> 继承的源对象。返回一个指示是否已到达数据文件的结尾的逻辑值。如果特定对象没有数据结尾功能，此函数值始终返回 <code>false</code>。</td></tr></tbody></table>

## 另请参阅

[`matlab.System`](https://ww2.mathworks.cn/help/matlab/ref/matlab.system-class.html)

## 相关主题

*   [System object 与 MATLAB 函数](https://ww2.mathworks.cn/help/matlab/matlab_prog/system-objects-vs-matlab-functions.html)
*   [使用 System object 在 MATLAB 中进行系统设计](https://ww2.mathworks.cn/help/matlab/matlab_prog/system-design-in-matlab-using-system-objects.html)
*   [System Design in Simulink Using System Objects](https://ww2.mathworks.cn/help/simulink/ug/system-design-in-simulink-using-system-objects.html) (Simulink)

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