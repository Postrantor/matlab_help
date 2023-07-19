---
tip: translate by openai@2023-07-07 17:09:33
...
---
url: [https://ww2.mathworks.cn/help/matlab/matlab_prog/run-functions-on-threads.html](https://ww2.mathworks.cn/help/matlab/matlab_prog/run-functions-on-threads.html)

> URL：[https://ww2.mathworks.cn/help/matlab/matlab_prog/](https://ww2.mathworks.cn/help/matlab/matlab_prog/)在线程上运行函数.html
> title: Run MATLAB Functions in Thread-Based Environment - MATLAB & Simulink - MathWorks 中国 --- 在基于线程的环境中运行 MATLAB 函数 - MATLAB & Simulink - MathWorks 中国
> date: 2023-07-07 17:05:28
> tag:

summary: Check support for MATLAB functions that you want to run in the background.

> 摘要：检查您要在后台运行的 MATLAB 函数的支持情况。

---

## Run MATLAB Functions in Thread-Based Environment

Hundreds of functions in MATLAB® and other toolboxes can run in a thread-based environment. You can use `backgroundPool` or `parpool("threads")` to run code in a thread-based environment.

> MATLAB® 和其他工具箱中的数百个函数可以在基于线程的环境中运行。您可以使用 `backgroundPool` 或 `parpool("threads")` 在基于线程的环境中运行代码。

### Run Functions in the Background

If a function is supported in a thread-based environment, you can use `parfeval` and `backgroundPool` to run it in the background.

> 如果一个函数支持在基于线程的环境中，你可以使用 `parfeval` 和 `backgroundPool` 来在后台运行它。

Use the `rand` function to generate a `100`-by-`100` matrix of random numbers in the background.

> 使用 `rand` 函数在后台生成一个大小为 `100` 乘 `100` 的随机数矩阵。

```
f = parfeval(backgroundPool,@rand,1,100);

```

For more information about running code in the background, see [`backgroundPool`](https://ww2.mathworks.cn/help/matlab/ref/parallel.backgroundpool.html).

> 如果要了解有关在后台运行代码的更多信息，请参阅[`backgroundPool`](https://ww2.mathworks.cn/help/matlab/ref/parallel.backgroundpool.html)。

### Run Functions on a Thread Pool

If a function is supported in a thread-based environment, you can run it on a thread pool if you have Parallel Computing Toolbox™.

> 如果一个函数在基于线程的环境中支持，如果您拥有 Parallel Computing Toolbox™，您可以在线程池上运行它。

```
parpool("threads");
parfor i = 1:100
    A{i} = rand(100);
end

```

For more information about thread pools, see [`ThreadPool`](https://ww2.mathworks.cn/help/parallel-computing/parallel.threadpool.html) (Parallel Computing Toolbox).

> 有关线程池的更多信息，请参阅[`ThreadPool`](https://ww2.mathworks.cn/help/parallel-computing/parallel.threadpool.html)（Parallel Computing Toolbox）。

### Automatically Scale Up

If you have Parallel Computing Toolbox, your code that uses `backgroundPool` automatically scales up to use more available cores.

> 如果您有并行计算工具箱，则使用 `backgroundPool` 的代码会自动扩展到使用更多可用的核心。

For information about the number of cores that you can use, see the `NumWorkers` property of [`BackgroundPool`](https://ww2.mathworks.cn/help/matlab/ref/parallel.backgroundpool.html).

> 有关可以使用的内核数量的信息，请参阅[`BackgroundPool`](https://ww2.mathworks.cn/help/matlab/ref/parallel.backgroundpool.html)的 `NumWorkers` 属性。

By running multiple functions in the background at the same time when you use Parallel Computing Toolbox, you can speed up the following code.

> 使用并行计算工具箱时，在后台同时运行多个函数，可以加快以下代码的执行速度。

```
for i = 1:100
    f(i) = parfeval(backgroundPool,@rand,1,100);
end

```

### Check Thread Supported Functions

If a MATLAB function has thread support, you can consult additional thread usage information on its function page. See "Thread-Based Environment" in the Extended Capabilities section at the end of the function page.

> 如果 MATLAB 函数具有线程支持，您可以在其函数页面上查阅附加的线程使用信息。请参阅函数页面末尾的“基于线程的环境”部分中的扩展功能。

In general, functionality in [Graphics](https://ww2.mathworks.cn/help/matlab/graphics.html), [App Building](https://ww2.mathworks.cn/help/matlab/gui-development.html), [External Language Interfaces](https://ww2.mathworks.cn/help/matlab/external-language-interfaces.html), [Files and Folders](https://ww2.mathworks.cn/help/matlab/files-and-folders.html), and [Environment and Settings](https://ww2.mathworks.cn/help/matlab/desktop-tools-and-development-environment.html) is not supported.

> 一般来说，[图形](https://ww2.mathworks.cn/help/matlab/graphics.html)、[应用程序构建](https://ww2.mathworks.cn/help/matlab/gui-development.html)、[外部语言接口](https://ww2.mathworks.cn/help/matlab/external-language-interfaces.html)、[文件和文件夹](https://ww2.mathworks.cn/help/matlab/files-and-folders.html)以及[环境和设置](https://ww2.mathworks.cn/help/matlab/desktop-tools-and-development-environment.html)的功能不受支持。

MATLAB and several toolboxes include functions with built-in thread support. To view lists of all functions in MATLAB and these toolboxes that have thread support, use the links in the following table. Functions in the lists with warning indicators have limitations or usage notes specific to running the function on threads. You can check the usage notes and limitations in the Extended Capabilities section of the function reference page. For information about updates to individual thread-supported functions, see the release notes.

> MATLAB 和几个工具箱包含带有内置线程支持的函数。要查看 MATLAB 和这些工具箱中所有具有线程支持的函数列表，请使用以下表中的链接。具有警告指示符的列表中的函数具有特定于在线程上运行函数的限制或使用说明。您可以在函数参考页面的扩展功能部分检查使用说明和限制。有关各个线程支持函数的更新信息，请参阅发行说明。

## See Also

[`parfeval`](https://ww2.mathworks.cn/help/matlab/ref/parfeval.html) | [`backgroundPool`](https://ww2.mathworks.cn/help/matlab/ref/parallel.backgroundpool.html)

> `parfeval` | `backgroundPool`：使用 `parfeval` 和 `backgroundPool` 进行并行计算。

## Related Topics

- [Run MATLAB Functions on a GPU](https://ww2.mathworks.cn/help/parallel-computing/run-matlab-functions-on-a-gpu.html) (Parallel Computing Toolbox)
