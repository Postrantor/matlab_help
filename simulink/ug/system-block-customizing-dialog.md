---
url: https://ww2.mathworks.cn/help/simulink/ug/system-block-customizing-dialog.html
title: Customize MATLAB System Block Dialog
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-03 11:31:47
tag: 
summary: This example shows you how to customize the block dialog for a MATLAB System block.
---
This example shows you how to customize the block dialog for a MATLAB System block.

### System objects

System objects allow you to implement algorithms using MATLAB. System objects are a specialized kind of MATLAB object, designed specifically for implementing and simulating dynamic systems with inputs that change over time.

After you define a System object, you can include it in a Simulink model using a MATLAB System block.

### Model Description

This example contains a MATLAB System block that implements the System object `CustomDialog`. Use the MATLAB System block dialog box to modify properties of the System object. This example shows how to customize the prompt for a property and how to create check boxes, lists, groups, tabs, and buttons in the dialog box.

![](https://ww2.mathworks.cn/help/examples/simulink/win64/SystemBlockCustomizingDialogExample_01.png)

![](https://ww2.mathworks.cn/help/examples/simulink/win64/xxSystemObjectCustomDialogImage.PNG)

### System Object Class Definition

You can access MATLAB source code used by the MATLAB System block by clicking the "Source Code" hyperlink from the block dialog. The System object `CustomDialog` implements the `getPropertyGroupsImpl` and `getHeaderImpl` methods that are used to customize the appearance of the block dialog and organize the System object properties.

1.  `PropertyDefault` - Property with no customization
    
2.  `PropertyCustomPrompt` - Property with custom prompt
    
3.  `PropertyEnum` - Enumeration property with a finite list of options
    
4.  `PropertyLogical` - Property validation with `logical` to create check box
    
5.  `PropertyInDifferentTab` - Property shown on a different tab of the dialog box
    

The `getPropertyGroupsImpl` method uses the matlab.system.display.Section and matlab.system.display.SectionGroup classes to create property sections and tabs in the dialog box. `getPropertyGroupsImpl` also creates a button in the "Group 2" section which calls the `visualize` method of the System object.

```
classdef CustomDialog < matlab.System
% CustomDialog Customize System block dialog

    properties
        PropertyDefault = 10
        % For PropertyDefault, with no comment above, property name is used
        % as prompt

        % PropertyCustomPrompt Use comment above property for custom prompt
        PropertyCustomPrompt = 20
        
        % PropertyEnum Use enumeration to limit values to finite list
        PropertyEnum (1,1) ColorValues = ColorValues.blue

        % PropertyInDifferentTab Use getPropertyGroupsImpl to create tabs
        PropertyInDifferentTab = 30
    end
    
    properties(Nontunable)
        % PropertyLogical Use logical property validation to create a checkbox
        % Logical properties need to be Nontunable for use in Simulink
        PropertyLogical (1,1) logical = true
    end
    
    methods(Access = protected)
        function y = stepImpl(~, u)
            y = u;
        end
    end
    
    methods (Static, Access = protected)
        function groups = getPropertyGroupsImpl
        % Use getPropertyGroupsImpl to create property sections in the
        % dialog. Create two sections with titles "Group1" and
        % "Group2". "Group1" contains PropertyDefault and
        % PropertyCustomPrompt. "Group2" contains PropertyEnum, 
        % PropertyLogical, and a Visualize button.
            group1 = matlab.system.display.Section(...
                'Title','Group 1',...
                'PropertyList',{'PropertyDefault','PropertyCustomPrompt'});
 
            group2 = matlab.system.display.Section(...
                'Title','Group 2',...
                'PropertyList',{'PropertyEnum','PropertyLogical'});

            % Add a button that calls back into the visualize method
            group2.Actions = matlab.system.display.Action(@(actionData,obj)...
                    visualize(obj,actionData),'Label','Visualize');

            tab1 = matlab.system.display.SectionGroup(...
                    'Title', 'Tab 1', ...
                    'Sections',  [group1, group2]);
           
            tab2 = matlab.system.display.SectionGroup(...
                    'Title', 'Tab 2', ...
                    'PropertyList',  {'PropertyInDifferentTab'});

            groups = [tab1, tab2];
        end
        
        function header = getHeaderImpl
           header = matlab.system.display.Header(mfilename('class'), ...
                   'Title','AlternativeTitle',...
                   'Text','Customize dialog header using getHeaderImpl method.');
         end
    end

    methods
        function visualize(obj, actionData)
            % Use actionData to store custom data
            f = actionData.UserData;
            if isempty(f) || ~ishandle(f)
                f = figure;
                actionData.UserData = f;
            else
                figure(f); % Make figure current
            end
        
            d = 1:obj.PropertyCustomPrompt;
            plot(d);
        end
    end
end


```

## See Also

[`getPropertyGroupsImpl`](https://ww2.mathworks.cn/help/simulink/slref/matlab.system.getpropertygroupsimpl.html)

## Related Topics

*   [What Are System Objects?](https://ww2.mathworks.cn/help/matlab/matlab_prog/what-are-system-objects.html)
*   [Why Use the MATLAB System Block?](https://ww2.mathworks.cn/help/simulink/ug/what-is-matlab-system-block.html#btyq1xw)
*   [Customize System Block Appearance](https://ww2.mathworks.cn/help/simulink/ug/customize-system-block-appearance.html)

您曾对此示例进行过修改。是否要打开带有您的编辑的示例？

How useful was this information?

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