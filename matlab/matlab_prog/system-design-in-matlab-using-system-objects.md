---
url: https://ww2.mathworks.cn/help/matlab/matlab_prog/system-design-in-matlab-using-system-objects.html?searchHighlight=system%20object&s_tid=srchtitle_support_results_4_system%20object
title: 使用 System object 在 MATLAB 中进行系统设计
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-03 16:34:19
tag: 
summary: 使用 System object 在 MATLAB 中进行系统设计和仿真。
---
### 在 MATLAB 中进行系统设计和仿真

通过 System object，您可以在 MATLAB® 中进行系统设计和仿真。您可按下图所示在 MATLAB 中使用 System object。

![](https://ww2.mathworks.cn/help/matlab/matlab_prog/sys_obj_start_here.png)

### 创建单个组件

此部分中的示例说明如何使用软件中预定义的 System object。如果您使用某个函数来创建并使用 System object，请通过条件代码指定对象创建。条件化创建可以防止在循环中调用该函数时出现错误。您也可以创建您自己的 System object，请参阅[创建 System object](https://ww2.mathworks.cn/help/matlab/create-system-objects.html)。

此部分说明如何使用 DSP System Toolbox™ 和 Audio Toolbox™ 中的预定义组件来设置您的系统：

*   `dsp.AudioFileReader` - 读取音频数据文件
    
*   `dsp.FIRFilter` - 对音频数据进行滤波
    
*   `audioDeviceWriter` - 播放滤波后的音频数据
    

首先，使用默认的属性设置创建组件对象。

```
audioIn = dsp.AudioFileReader;
filtLP = dsp.FIRFilter;
audioOut = audioDeviceWriter;


```

### 配置组件

#### 何时配置组件

如果您在创建对象时未设置其属性并且不想使用默认值，则必须显式设置这些属性。某些属性允许您在系统正在运行时更改其值。有关信息，请参阅[重新配置对象](https://ww2.mathworks.cn/help/matlab/matlab_prog/system-design-in-matlab-using-system-objects.html#btp1tw_-1)。

大多数属性都是相互独立的。但是，某些 System object 属性会启用或禁用其他属性，或者限制其他属性的值。为避免出现错误或警告，您应先设置控制属性，然后再设置从属属性。

#### 显示组件属性值

要显示某个对象的当前属性值，请在命令行中键入该对象的句柄名称（如 `audioIn`）。要显示特定属性的值，请键入 `objecthandle.propertyname`（如 `audioIn.FileName`）。

#### 配置组件属性值

此部分说明如何通过设置组件对象的属性来为您的系统配置组件。

如果您在配置组件之前已单独创建了组件，请按照此过程操作。您也可以同时创建和配置组件，如后面的示例所述。

对于文件读取器对象，指定要读取的文件并设置输出数据类型。

对于滤波器对象，使用 fir1 函数（用于指定低通滤波器阶数和截止频率）指定滤波器分子系数。

对于音频设备写入器对象，指定采样率。在本示例中，使用相同的采样率作为输入数据。

```
audioIn.Filename = "speech_dft_8kHz.wav";
audioIn.OutputDataType = "single";
filtLP.Numerator = fir1(160,.15);
audioOut.SampleRate = audioIn.SampleRate;

```

### 同时创建和配置组件

此示例说明如何同时创建 System object 组件和配置所需属性。使用'Name', Value 对组参数指定每个属性。

创建文件读取器对象、指定要读取的文件并设置输出数据类型。

```
audioIn = dsp.AudioFileReader("speech_dft_8kHz.wav",...
                              'OutputDataType',"single");


```

创建滤波器对象，然后使用 fir1 函数指定滤波器分子。指定 fir1 函数的低通滤波器阶数和截止频率。

```
filtLP = dsp.FIRFilter('Numerator',fir1(160,.15));


```

创建音频播放器对象，然后将采样率设置为与输入数据相同的采样率。

```
audioOut = audioDeviceWriter('SampleRate',audioIn.SampleRate);


```

### 将组件组合到系统中

#### 连接 System object

当您确定了所需组件并已创建和配置 System object 之后，请组装您的系统。您可以像其他 MATLAB 变量一样使用 System object，并将它们包含在 MATLAB 代码中。您可以在 System object 中传递 MATLAB 变量。

使用 System object 和使用函数的主要区别在于 System object 使用一个两步骤的过程。首先，创建相应对象并设置其参数，然后运行该对象。运行该对象会将其初始化并控制系统的数据流和状态管理。通常在代码循环中调用 System object。

您可以使用某个对象的输出作为另一个对象的输入。对于某些 System object，您可使用这些对象的属性来更改输入或输出。要验证是否正在使用合适数目的输入和输出，可以对任何 System object 使用 `nargin` 和 `nargout`。有关所有可用的 System object 函数的信息，请参阅 [System object 函数](https://ww2.mathworks.cn/help/matlab/matlab_prog/what-are-system-objects.html#btsx3d1-1)。

#### 连接系统中的组件

此部分演示了如何将组件连接在一起以读取、滤波和播放音频数据文件。while 循环使用 `isDone` 函数来完整读取整个文件。

```
while ~isDone(audioIn)
    audio = audioIn();    % Read audio source file
    y = filtLP(audio);        % Filter the data
    audioOut(y);              % Play the filtered data
end


```

### 运行系统

通过在命令行下直接键入或运行包含程序的文件来运行代码。当您运行系统代码时，数据是通过您的对象来处理的。

#### 当系统正在运行时您无法更改的内容

首次调用 System object 时会初始化并运行该对象。当 System object 已开始处理数据时，您不能更改不可调属性。

根据 System object 的不同，其他设定也可能受到限制：

*   输入大小
    
*   输入复 / 实性
    
*   输入数据类型
    
*   可调属性数据类型
    
*   离散状态数据类型
    

如果 System object 作者对这些设定进行了限制，则您在 System object 使用期间尝试更改这些设定会出错。

### 重新配置对象

#### 更改属性

当 System object 已开始处理数据时，您不能更改_不可调_属性。您可以对任何 System object 使用 `isLocked` 以验证对象是否正在处理数据。处理完成后，您可以使用 `release` 函数释放资源并允许更改不可调属性。

某些对象属性为_可调_属性，这使您能够对它们进行更改，即使对象在使用中也是如此。大多数 System object 属性是不可调属性。请参阅对象的参考页以确定单个属性是否为可调属性。

#### 更改输入复 / 实性、维度或数据类型

在对象使用期间，在您调用算法后，某些 System object 不允许更改输入复 / 实性、大小或数据类型。如果 System object 对这些设定进行了限制，您可以调用 `release` 来更改这些设定。调用 `release` 还会重置 System object 的其他方面，例如状态和离散状态。

#### 更改系统中的可调属性

此示例说明如何在代码运行时通过修改滤波器对象的 `Numerator` 属性，将滤波器类型更改为高通滤波器。更改将在下次调用该对象时生效。

```
reset(audioIn);% Reset audio file
Wn = [0.05,0.1,0.15,0.2];
for x=1:4000
    Wn_X = ceil(x/1000);
    filtLP.Numerator = fir1(160,Wn(Wn_X),'high');
    audio = audioIn();    % Read audio source file
    y = filtLP(audio);    % Filter the data
    audioOut(y);          % Play the filtered data
end

```