---
url: https://ww2.mathworks.cn/help/matlab/matlab_prog/define-basic-system-objects-1.html
title: 定义基本 System object
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-03 16:37:20
tag: 
summary: 使用 stepImpl 方法创建一个基本 System object。
---
## 定义基本 System object

此示例说明如何创建一个将数值以 1 为单位递增的基本 System object™。该示例中使用的类定义文件包含定义 System object 所需的最少元素。

### 创建 System object 类

您可以创建并编辑 MAT 文件，或使用 MATLAB® 编辑器创建 System object。以下示例介绍了如何在 MATLAB 编辑器中使用菜单。

1.  在 MATLAB 的 “编辑器” 选项卡中，选择 > > 。此时将打开一个简单的 System object 模板。
    
2.  基于 `matlab.System` 子类化您的对象。在您的文件的第一行中将 `Untitled` 替换为 `AddOne`。
    
    ```
    classdef AddOne < matlab.System
    
    
    ```
    
    System object 由基类 `matlab.System` 构成，并且可能包括一个或多个 mixin 类。您可以在类定义文件的第一行中指定基类和 mixin 类。
    
3.  保存该文件并将其命名为 `AddOne.m`。
    

### 定义算法

`stepImpl` 方法包含在您运行对象时要执行的算法。定义此方法以使其包含您希望 System object 执行的操作。

1.  在所创建的基本 System object 中，检查 `stepImpl` 方法模板。
    
    ```
    methods (Access = protected)
       function y = stepImpl(obj,u)
          % Implement algorithm. Calculate y as a function of input u and
          % discrete states.
          y = u;
       end
    end
    
    ```
    
    `stepImpl` 方法的访问权限始终设置为 `protected`，因为该方法为用户无法直接调用或运行的内部方法。
    
    所有方法（静态方法除外）都要求将 System object 句柄作为第一个输入参数。MATLAB 编辑器插入的默认值为 `obj`。您可以对 System object 句柄使用任何名称。
    
    默认情况下，输入数目和输出数目都为 1。可以使用来添加输入和输出。您也可以使用不同数目的输入或输出，请参阅[更改输入数目](https://ww2.mathworks.cn/help/matlab/matlab_prog/change-number-of-step-method-inputs-or-outputs-1.html)。
    
    或者，如果您通过编辑 MAT 文件创建 System object，则可以使用 > 来添加 `stepImpl` 方法。
    
2.  在 `stepImpl` 方法中更改计算以将 `1` 添加到 `u` 的值中。
    
    ```
    methods (Access = protected)
        
        function y = stepImpl(~,u)
          y = u + 1;
        end
    
    
    ```
    
    **提示**
    
    您可以使用波浪号 (`~`) 来指示函数中不使用对象句柄，而不必传入对象句柄。使用波浪号而不是对象句柄可防止出现有关未使用的变量的警告。
    
3.  删除默认情况下包含在基本模板中的未使用的方法。
    
    您可以修改这些方法以添加更多 System object 操作和属性。您也可以不进行任何更改，System object 仍将按预期运行。
    

现在，类定义文件包含此 System object 所需的全部代码。

```
classdef AddOne < matlab.System
% ADDONE Compute an output value one greater than the input value
  
  % All methods occur inside a methods declaration.
  % The stepImpl method has protected access
  methods (Access = protected)
    
    function y = stepImpl(~,u)
      y = u + 1;
    end
  end
end

```

## 另请参阅

[`stepImpl`](https://ww2.mathworks.cn/help/matlab/ref/matlab.system.stepimpl.html) | [`getNumInputsImpl`](https://ww2.mathworks.cn/help/matlab/ref/matlab.system.getnuminputsimpl.html) | [`getNumOutputsImpl`](https://ww2.mathworks.cn/help/matlab/ref/matlab.system.getnumoutputsimpl.html) | [`matlab.System`](https://ww2.mathworks.cn/help/matlab/ref/matlab.system-class.html)

## 相关主题

*   [更改输入数目](https://ww2.mathworks.cn/help/matlab/matlab_prog/change-number-of-step-method-inputs-or-outputs-1.html)
*   [在 MATLAB 中进行系统设计和仿真](https://ww2.mathworks.cn/help/matlab/matlab_prog/system-design-in-matlab-using-system-objects.html#btp1tt2-1)

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