---
url: https://ww2.mathworks.cn/help/simulink/ug/createmovingaveragefilterblocksystemobject.html
title: Create Moving Average Filter Block with System Object - MATLAB & Simulink - MathWorks 中国 --- 使用 System Object 创建移动平均滤波器模块 - MATLAB & Simulink - MathWorks 中国
date: 2023-08-03 16:38:57
tag: 
summary: This example shows how to modify a System object to use it in Simulink.
---
This example shows how to extend the `movingAverageFilter` System object™ for use in Simulink™. To use a System object in Simulink, include the System object in a MATLAB System block.

### `movingAverageFilter` System Object

This example extends the `movingAverageFilter` System object built in [Create Moving Average System Object](https://ww2.mathworks.cn/help/matlab/matlab_prog/createmovingaveragesystemobject.html). The `movingAverageFilter` System object computes the unweighted mean of a specified number of previous inputs. Use the `WindowLength` property to specify how many previous samples to use.

### Use in Simulink

The object is already ready to use in Simulink. Create a Simulink model and add a MATLAB System block. Specify `movingAverageFilter` as the System object name. For example, this model uses the moving average filter to eliminate noise from a signal.

```
model = 'movingaveragefilter_sl';
open_system(model);

```

![](https://ww2.mathworks.cn/help/examples/simulink/win64/CreateAMovingAverageFilterBlockWithASystemObjectExample_01.png)

![](https://ww2.mathworks.cn/help/examples/simulink/win64/CreateAMovingAverageFilterBlockWithASystemObjectExample_02.png)

The block dialog window shows the public, tunable parameters:

![](https://ww2.mathworks.cn/help/examples/simulink/win64/CreateAMovingAverageFilterBlockWithASystemObjectExample_03.png)

### Customize MATLAB System Block

Optionally, you can customize the block appearance and block dialog for a MATLAB System block by adding methods to the System object.

#### Add Simulink Block Icon Customization Method

By default the block icon shows the name of the System object, in this case `movingAverageFilter`. Customize the Moving Average Filter block icon with a cleaner name. In the **Editor** toolstrip, select the **System Block** dropdown button, then select **Add Text Icon**. The `getIconImpl` method is added to `movingAverageFilter`. Inside `getIconImpl`, set `icon` equal to the string array `["Moving","Average","Filter"];`

```
function icon = getIconImpl(~)
    % Define icon for the System block.
    icon = ["Moving","Average","Filter"];
end


```

#### Customize Block Dialog

You can also customize the block dialog by adding methods and comments to the System object. For details about block dialog customization, see [Customize System Block Appearance](https://ww2.mathworks.cn/help/simulink/ug/customize-system-block-appearance.html). In this example, rename the `WindowLength` property in the dialog box and add a method to customize the description.

By default, all public properties appear as parameters in the block dialog with their property names. In this example, add comments before the `WindowLength` property so that it appears as **Moving window length** in the dialog. Add a comments above the property in the form: `PropertyName Name in dialog`

```
 % WindowLength Moving window length
 WindowLength (1,1){mustBeInteger,mustBePositive} = 5


```

To specify the header and description in the block dialog, in the toolstrip select **System Block > Specify Dialog Header**. This option adds the `getHeaderImpl` method to `movingAverageFilter`. Modify the call to `matlab.system.display.Header` to this:

```
methods(Access = protected, Static)
    function header = getHeaderImpl
        % Define header panel for System block dialog
        header = matlab.system.display.Header('movingAverageFilter',...
            'Title','Moving Average Filter',...
            'Text', 'Unweighted moving average filter of 1- or 2D input.');
    end
end


```

You can see a preview of the block dialog by clicking the button in the toolstrip above **System Block**.

![](https://ww2.mathworks.cn/help/examples/simulink/win64/CreateAMovingAverageFilterBlockWithASystemObjectExample_04.png)

![](https://ww2.mathworks.cn/help/examples/simulink/win64/CreateAMovingAverageFilterBlockWithASystemObjectExample_05.png)

#### Customized Block in Simulink

This is the block with the added customizations:

```
model = 'movingaveragefilter_sl_extended';
open_system(model);

```

![](https://ww2.mathworks.cn/help/examples/simulink/win64/CreateAMovingAverageFilterBlockWithASystemObjectExample_06.png)

To see the completed System object with the Simulink customization methods, type:

```
edit movingAverageFilter_extended.m


```