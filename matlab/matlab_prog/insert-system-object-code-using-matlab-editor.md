---
url: https://ww2.mathworks.cn/help/matlab/matlab_prog/insert-system-object-code-using-matlab-editor.html
title: 使用 MATLAB 编辑器插入 System object 代码
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-03 11:52:51
tag: 
summary: 使用 MATLAB 编辑器中的 System object 编辑选项插入属性、方法、输入和输出以及状态的代码的说明。
---
## 使用 MATLAB 编辑器插入 System object 代码

### 通过代码插入定义 System object

您可以在 MATLAB® 编辑器中使用代码插入选项来定义 System object。当您选择这些选项时，MATLAB 编辑器可为 System object™ 添加预定义的属性、方法、状态、输入或输出。使用这些工具可更快地创建并修改 System object，以及通过减少键入错误来提高准确性。GUI 在 MATLAB Online 上略有不同，但功能相同。

要访问 System object 编辑选项，请创建一个新的或打开一个现有的 System object。

![](https://ww2.mathworks.cn/help/matlab/matlab_prog/system_object_insert_options.png)

要为 System object 添加预定义的代码，请从相应的菜单中选择该代码。例如，当您点击 > 时，MATLAB 编辑器将添加以下代码：

```
    properties(Nontunable)
        Property
    end

```

MATLAB 编辑器会插入具有默认名称 `Property` 的新属性，您可以对该名称进行重命名。如果您的现有属性组带有 `Nontunable` 特性，则 MATLAB 编辑器会将新属性插入到该组中。如果您没有属性组，MATLAB 编辑器将创建一个具有正确特性的属性组。

**插入选项**

```
function y = stepImpl(obj,u,u2)
          % Implement algorithm. Calculate y as a function of
          % input u and discrete states.
          y = u;
      end
```

### 创建温度枚举属性

1.  打开一个新的或现有的 System object。
    
2.  在 MATLAB 编辑器中，选择 > 。
    
3.  在**枚举**对话框中，输入：
    
    1.  **属性名称**为 `TemperatureUnit`。
        
    2.  **枚举名称**为 `TemperatureUnitValues`。
        
    
4.  选中**创建新枚举**复选框。
    
5.  使用 **-**（减号）按钮删除现有枚举值。
    
6.  使用 **+**（加号）按钮和以下值添加三个枚举值：
    
7.  点击**默认**选择 `Fahrenheit` 作为默认值。
    
    该对话框现在看上去如下所示：
    
    ![](https://ww2.mathworks.cn/help/matlab/matlab_prog/sysobj_enumerations.png)
    
8.  要创建此枚举和关联的类，请点击**插入**。
    
9.  在 MATLAB 编辑器中，将创建具有该枚举定义的附加类文件。将该枚举类定义文件另存为 `TemperatureUnitValues.m`。
    
    ```
    classdef TemperatureUnitValues < int32
        enumeration
            Fahrenheit (0)
            Celsius (1)
            Kelvin (2)
        end
    end
    
    ```
    
    在 System object 类定义中，添加了以下代码：
    
    ```
      properties(Nontunable)
        TemperatureUnit (1, 1) TemperatureUnitValues = TemperatureUnitValues.Fahrenheit
      end
    
    ```
    

有关枚举的详细信息，请参阅[将属性值限制为有限列表](https://ww2.mathworks.cn/help/matlab/matlab_prog/limit-property-values-to-finite-list.html)。

### 为冻结点创建自定义属性

1.  打开一个新的或现有的 System object。
    
2.  在 MATLAB 编辑器中，选择 > 。
    
3.  在 “自定义属性” 对话框的 **System object 属性**下，选择 **Nontunable**。在 **MATLAB 属性特性**下，选择 **Constant**。将 **GetAccess** 保留设置为 “`public`”。**SetAccess** 将被灰显，因为无法使用 System object 方法来设置类型为常量的属性。
    
    ![](https://ww2.mathworks.cn/help/matlab/matlab_prog/sysobj_custom_property_ex.png)
    
4.  点击**插入**，并将以下代码插入到 System object 定义中：
    
    ```
        properties(Nontunable, Constant)
            Property
        end
    
    ```
    
5.  将 `Property` 替换为您的属性。
    
    ```
        properties(Nontunable, Constant)
            FreezingPointFahrenheit = 32;
        end
    
    ```
    

### 添加方法来验证输入

1.  打开一个新的或现有的 System object。
    
2.  在 MATLAB 编辑器中，选择 > 。
    
    MATLAB 编辑器会将以下代码插入到 System object 中：
    
    ```
        function validateInputsImpl(obj,u)
           % Validate inputs to the step method at initialization
        end
    
    ```
    

## 相关主题

*   [分析 System object 代码](https://ww2.mathworks.cn/help/matlab/matlab_prog/analyze-system-object-development.html)

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