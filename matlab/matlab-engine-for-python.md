---
url: https://ww2.mathworks.cn/help/matlab/matlab-engine-for-python.html
title: 从 Python 中调用 MATLAB
- MATLAB & Simulink
- MathWorks 中国
date: 2023-07-13 10:55:27
tag: 
summary: 编写可用于 MATLAB 的 Python 程序
---

# 从 Python 中调用 MATLAB

编写可用于 MATLAB® 的 Python® 程序

MATLAB Engine API for Python 可提供一个包，供 Python 将 MATLAB 作为计算引擎来调用。该引擎支持参考实现 (CPython)。有关支持的版本信息，请参阅 [MATLAB 产品（按版本）兼容的 Python 版本](https://www.mathworks.com/content/dam/mathworks/mathworks-dot-com/support/sysreq/files/python-compatibility.pdf)。

- 要安装和启动该引擎，请参阅[用于 Python 的 MATLAB 引擎 API 快速入门](matlab_external/get-started-with-matlab-engine-for-python.html)。
- 要从 MATLAB 调用 Python 函数，请参阅[从 MATLAB 中调用 Python](call-python-libraries.html)。

引擎应用程序需要已安装版本的 MATLAB；您无法在只有 MATLAB Runtime 的机器上运行 MATLAB Engine。

## 函数

[全部展开](<javascript:void(0);>)

### Python 函数

<table><tbody><tr><td><a href="apiref/matlab.engine.start_matlab.html"><code>matlab.engine.start_matlab</code></a></td><td>启动 <span>MATLAB</span> Engine for <span><span>Python</span></span></td></tr><tr><td><a href="apiref/matlab.engine.find_matlab.html"><code>matlab.engine.find_matlab</code></a></td><td>查找共享的 <span>MATLAB</span> 会话以连接到用于 <span><span>Python</span></span> 的 <span>MATLAB</span> 引擎</td></tr><tr><td><a href="apiref/matlab.engine.connect_matlab.html"><code>matlab.engine.connect_matlab</code></a></td><td>将共享 <span>MATLAB</span> 会话连接到用于 <span><span>Python</span></span> 的 <span>MATLAB</span> 引擎</td></tr></tbody></table>

### MATLAB 函数

<table><tbody><tr><td><a href="ref/matlab.engine.shareengine.html"><code>matlab.engine.shareEngine</code></a></td><td>将正在运行的 <span>MATLAB</span> 会话转换为共享会话</td></tr><tr><td><a href="ref/matlab.engine.enginename.html"><code>matlab.engine.engineName</code></a></td><td>返回共享 <span>MATLAB</span> 会话的名称</td></tr><tr><td><a href="ref/matlab.engine.isengineshared.html"><code>matlab.engine.isEngineShared</code></a></td><td>确定 <span>MATLAB</span> 会话是否共享</td></tr></tbody></table>

## 类

[全部折叠](<javascript:void(0);>)

### Python 类

<table><tbody><tr><td><a href="apiref/matlab.engine.matlabengine-class.html" hreflang="en"><code>matlab.engine.MatlabEngine</code></a></td><td><span><span>Python</span></span> object using <span>MATLAB</span> as computational engine within <span><span>Python</span></span> session</td></tr><tr><td><a href="apiref/matlab.engine.futureresult-class.html"><code>matlab.engine.FutureResult</code></a></td><td>存储在 <span><span>Python</span></span> 对象中的 <span>MATLAB</span> 函数异步调用的结果</td></tr></tbody></table>

## 主题

### 安装

- **[用于 Python 的 MATLAB 引擎 API 的系统要求](matlab_external/system-requirements-for-matlab-engine-for-python.html)**  
  编写和编译用于 Python 的 MATLAB 引擎应用程序的要求。
- **[安装用于 Python 的 MATLAB Engine API](matlab_external/install-the-matlab-engine-for-python.html)**

  要在 Python 会话中启动 MATLAB Engine，请将该引擎 API 安装为 Python 包。

  - **[Python Setup Script to Install MATLAB Engine API](matlab_external/python-setup-script-to-install-matlab-engine-api.html)**
  - **[在非默认位置安装用于 Python 的 MATLAB Engine API](matlab_external/install-matlab-engine-api-for-python-in-nondefault-locations.html)**

### 快速入门

- **[用于 Python 的 MATLAB 引擎 API 快速入门](matlab_external/get-started-with-matlab-engine-for-python.html)**  
  用于 Python 的 MATLAB 引擎 API 提供了名为 `matlab` 的 Python 包，使您能够通过 Python 调用 MATLAB 函数。该包仅安装一次，然后您便可在当前或未来的 Python 会话中调用引擎。有关安装或启动引擎的帮助，请参阅：
- **[启动和停止用于 Python 的 MATLAB 引擎](matlab_external/start-the-matlab-engine-for-python.html)**  
  用以启动用于 Python 的 MATLAB 引擎的选项。
- **[通过 Python 调用 MATLAB 函数](matlab_external/call-matlab-functions-from-python.html)**  
  如何从 MATLAB 函数返回输出参数。如何从函数读取多个输出。当 MATLAB 函数没有返回输出参数时应该怎么做。
- **[从 Python 获取 MATLAB 函数的帮助](matlab_external/get-help-for-matlab-functions-from-python.html)**  
  您可以从 Python 访问所有 MATLAB 函数的支持文档。此文档包括示例，并说明每个函数的输入参数、输出参数和调用语法。

### 会话管理

- **[将 Python 连接到正在运行的 MATLAB 会话](matlab_external/connect-python-to-running-matlab-session.html)**  
  如何将用于 Python 的 MATLAB 引擎连接到已在您的本地机器上运行的共享 MATLAB 会话。

### 使用 MATLAB 工作区

- **[在 Python 中使用 MATLAB 引擎工作区](matlab_external/use-the-matlab-engine-workspace-in-python.html)**  
  此示例说明如何在 Python 中将变量添加到 MATLAB 引擎工作区。

### 数据交换和映射

- **[在 Python 中使用 MATLAB 数组](matlab_external/use-matlab-arrays-in-python.html)**  
  此示例说明如何在 Python 中创建 MATLAB 数组并将其作为输入参数传递给 MATLAB `sqrt` 函数。
- **[MATLAB 数组作为 Python 变量](matlab_external/matlab-arrays-as-python-variables.html)**  
  `matlab` Python 模块提供数组类以将 MATLAB 数值类型的数组表示为 Python 变量，以便 MATLAB 数组可以在 Python 和 MATLAB 之间传递。
- **[从 Python 将数据传递到 MATLAB](matlab_external/pass-data-to-matlab-from-python.html)**  
  当您将 Python 数据作为输入参数传递到 MATLAB 函数时，MATLAB Engine for Python 会将数据转换为等效的 MATLAB 数据类型。
- **[处理从 MATLAB 返回到 Python 的数据](matlab_external/handle-data-returned-from-matlab-to-python.html)**  
  当 MATLAB 函数返回输出参数时，用于 Python 的 MATLAB 引擎 API 会将数据转换为等同的 Python 数据类型。
- **[在 Python 中使用 MATLAB 句柄对象](matlab_external/use-matlab-handle-objects-in-python.html)**  
  此示例说明如何从 MATLAB 句柄类创建对象，并在 Python 中调用其方法。
- **[MATLAB 和 Python 中的默认数值类型](matlab_external/default-numeric-types-in-matlab-and-python.html)**  
  默认情况下，MATLAB 以双精度浮点数形式存储所有数值。而 Python 默认情况下将一些数值存储为整数。由于这种差异，您可能会将整数作为输入参数传递给需要双精度数值的 MATLAB 函数。

### 调用 MATLAB 函数

- **[从 Python 中调用用户脚本和函数](matlab_external/call-user-script-and-function-from-python.html)**  
  此示例显示如何通过 Python 来调用 MATLAB 脚本，以计算三角形的面积。
- **[从 Python 对 MATLAB 数据进行分类并绘图](matlab_external/sort-and-plot-matlab-data-from-python.html)**  
  此示例说明如何在 Python 中将患者数据分类到吸烟者和非吸烟者列表中，并对 MATLAB 患者的血压读数绘图。
- **[从 Python 以异步方式调用 MATLAB 函数](matlab_external/call-matlab-functions-asynchronously-from-python.html)**  
  此示例说明如何从 Python 异步调用 MATLAB `sqrt` 函数，并稍后检索平方根。
- **[将标准输出和错误重定向到 Python](matlab_external/redirect-standard-output-and-error-to-python.html)**  
  此示例说明如何将标准输出和标准错误从 MATLAB 函数重定向到 Python `StringIO` 对象。

## 疑难解答

**[MATLAB Engine API for Python 的限制](matlab_external/limitations-to-the-matlab-engine-for-python.html)**

用于 Python 的 MATLAB 引擎 API 不支持以下功能。

**[Troubleshoot MATLAB Errors in Python](matlab_external/troubleshoot-matlab-errors-in-python.html)**

When a MATLAB function raises an error, the MATLAB Engine for Python stops the function and catches the exception raised by MATLAB.

## 相关信息

- **[MATLAB 产品（按版本）兼容的 Python 版本](https://www.mathworks.com/content/dam/mathworks/mathworks-dot-com/support/sysreq/files/python-compatibility.pdf)**
- **[从 MATLAB 中调用 Python](call-python-libraries.html)**
