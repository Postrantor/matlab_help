---
url: https://ww2.mathworks.cn/help/matlab/matlab_prog/createmovingaveragesystemobject.html
title: 创建移动平均值 System object
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-03 16:38:15
tag: 
summary: 此示例说明如何创建用于计算移动平均值的 System object。
---
此示例说明如何创建实现移动平均滤波器的 System object™。

### 简介

System object 是派生自 `matlab.System` 的 MATLAB® 类。因此，System object 都继承一个通用公共接口，其中包括标准方法：

*   `setup` - 初始化对象，通常在仿真开始时
    
*   `reset` - 清除对象的内部状态，将其还原到默认的初始化后的状态
    
*   `release` - 释放对象内部使用的任何资源（内存、硬件或特定于操作系统的资源）
    

当您创建新种类的 System object 时，您需要为所有前面的方法提供特定实现来确定其行为。

在此示例中，您将创建并使用 `movingAverageFilter` System object。`movingAverageFilter` 是计算先前 `WindowLength` 输入采样的未加权均值的 System object。`WindowLength` 是移动平均值窗口的长度。`movingAverageFilter` 接受单精度和双精度二维输入矩阵。输入矩阵的每列被视为一个独立的（一维）通道。输入的第一个维度定义通道的长度（或输入帧的大小）。`movingAverageFilter` 随时间变化独立计算每个输入通道的移动平均值。

### 创建 System object 文件

在 MATLAB 的**主页**选项卡中，通过选择**新建 > System object > 基本**创建一个新 System object 类。System object 的基本模板在 MATLAB 编辑器中打开，以指导您创建 `movingAverageFilter` System object。

重命名类 `movingAverageFilter`，并将文件保存为 `movingAverageFilter.m`。为了确保可以使用 System object，请确保将该 System object 保存在位于 MATLAB 路径上的一个文件夹中。

为方便起见，此示例提供了完整的 `movingAverageFilter` System object 文件。要打开已完成的类，请输入：

```
edit movingAverageFilter.m

```

### 添加属性

此 System object 需要四个属性。首先，添加公共属性 `WindowLength` 来控制移动平均值窗口的长度。由于算法在数据处理开始后依赖于该值保持不变，因此该属性定义为不可调。此外，该属性只接受正实整数。为了确保输入满足这些条件，添加属性验证函数（请参阅[验证属性值](https://ww2.mathworks.cn/help/matlab/matlab_oop/validate-property-values.html)）。将此属性的默认值设置为 5。

```
properties(Nontunable)
    WindowLength (1,1){mustBeInteger,mustBePositive} = 5
end


```

接下来，添加两个属性，分别名为 `State` 和 `pNumChannels`。用户不应访问其中任一属性，因此请使用 `Access = private` 特性。`State` 保存移动平均滤波器的状态。`pNumChannels` 存储您的输入中的通道数量。此属性的值由输入中的列数决定。

```
properties(Access = private)
    State;
    pNumChannels = -1;
end


```

最后，您需要一个属性来存储 FIR 分子系数。添加一个名为 `pCoefficients` 的属性。由于系数在数据处理过程中不会更改，并且您不希望 System object 的用户访问系数，请将属性特性设置为 `Nontunable, Access = private`。

```
properties(Access = private, Nontunable)
    pCoefficients
end


```

### 添加构造函数以方便创建 System object

System object 构造函数是一个与类（此示例中为 `movingAverageFilter`）同名的方法。您需要使用 `setProperties` 方法来实现一个 System object 构造函数，以支持对 System object 使用名称 - 值对组输入。例如，利用此构造函数，用户可以使用以下语法创建 System object 的实例：`filter = movingAverageFilter('WindowLength',10)`。不要将此构造函数用于任何其他目的。所有其他设置任务都应在 `setupImpl` 方法中编写。

```
methods
    function obj = movingAverageFilter(varargin)
        % Support name-value pair arguments when constructing object
        setProperties(obj,nargin,varargin{:})
    end
end


```

### 在 `setupImpl` 中设置和初始化

`setupImpl` 方法设置对象并实现一次性初始化任务。对于此 System object，需要修改默认的 `setupImpl` 方法来计算滤波器系数、状态和通道数。滤波器系数是根据指定的 `WindowLength` 计算的。滤波器状态初始化为零。（请注意，每个输入通道有 `WindowLength-1` 个状态。）最后，通道数由输入中的列数决定。

对于 `setupImpl` 和所有 `Impl` 方法，您必须设置方法属性 `Access = protected`，因为 System object 的用户不直接调用这些方法。System object 的后端通过其他面向用户的函数调用这些方法。

```
function setupImpl(obj,x)
    % Perform one-time calculations, such as computing constants
    obj.pNumChannels = size(x,2);
    obj.pCoefficients = ones(1,obj.WindowLength)/obj.WindowLength;
    obj.State = zeros(obj.WindowLength-1,obj.pNumChannels,'like',x);
end


```

### 在 `stepImpl` 中定义算法

对象的算法在 `stepImpl` 方法中定义。`stepImpl` 中的算法是在 System object 的用户在命令行中调用该对象时执行的。在此示例中，需要修改 `stepImpl` 以计算输出，并使用 `filter` 函数更新对象的状态值。

```
function y = stepImpl(obj,u)
    [y,obj.State] = filter(obj.pCoefficients,1,u,obj.State);
end


```

### 重置和释放

状态重置方程在 `resetImpl` 方法中定义。在此示例中，需要将状态重置为零。此外，您需要添加 `releaseImpl` 方法。从**编辑器**工具条中，选择**插入方法 > 释放资源**。`releaseImpl` 方法将添加到您的 System object 中。修改 `releaseImpl` 以将通道数量设置为 `-1`，这将允许对滤波器使用新的输入。

```
function resetImpl(obj)
    % Initialize / reset discrete-state properties
    obj.State(:) = 0;
end
function releaseImpl(obj)
    obj.pNumChannels = -1;
end


```

### 验证输入

要验证 System object 的输入，您必须实现 `validateInputsImpl` 方法。此方法在初始化时和输入属性（如维度、数据类型或复 / 实性）发生变化后的每次调用时验证输入。从工具条中，选择**插入方法 > 验证输入**。在新插入的 `validateInputsImpl` 方法中，调用 `validateattributes` 以确保输入是包含浮点数据的二维矩阵。

```
function validateInputsImpl(~, u)
    validateattributes(u,{'double','single'}, {'2d',...
        'nonsparse'},'','input');
end


```

### 对象保存和加载

当您保存 System object 的实例时，`saveObjectImpl` 定义在 MAT 文件中保存的属性和状态值。如果没有为 System object 类定义 `saveObjectImpl` 方法，则只会保存公共属性和具有 `DiscreteState` 特性的属性。选择**插入方法 > 保存在 MAT 文件中**。修改 `saveObjectImpl` 以便在对象锁定时保存系数和通道数。

```
function s = saveObjectImpl(obj)
    s = saveObjectImpl@matlab.System(obj);
    if isLocked(obj)
        s.pCoefficients = obj.pCoefficients;
        s.pNumChannels = obj.pNumChannels;
    end
end


```

`loadObjectImpl` 是伴随 `saveObjectImpl` 一起的，因为它定义保存的对象如何加载。在加载保存的对象时，该对象以相同的锁定状态加载。选择**插入方法 > 从 MAT 文件加载**。修改 `loadObjectImpl`，以在对象为锁定状态时加载系数和通道数。

```
function loadObjectImpl(obj,s,wasLocked)
    if wasLocked
        obj.pCoefficients = s.pCoefficients;
        obj.pNumChannels = s.pNumChannels;
    end
    loadObjectImpl@matlab.System(obj,s,wasLocked);
end


```

### 在 MATLAB 中使用 `movingAverageFilter`

现在您已定义 System object，可以在 MATLAB 中使用该对象了。例如，使用 `movingAverageFilter` 从含噪脉冲序列中去除噪声。

```
movingAverageFilter = movingAverageFilter('WindowLength',10);

t = (1:250)';
signal = randn(250,1);
smoothedSignal = movingAverageFilter(signal);

plot(t,signal,t,smoothedSignal);
legend(["Input","Satlaboothed input"])

```

![](https://ww2.mathworks.cn/help/matlab/matlab_prog/createmovingaveragesystemobjectexample_01_zh_CN.png)

### 扩展 System object 以用于 Simulink

要在 Simulink® 中使用您的 System object，请参阅 [Create Moving Average Filter Block with System Object](https://ww2.mathworks.cn/help/simulink/ug/createmovingaveragefilterblocksystemobject.html) (Simulink)。