---
url: https://ww2.mathworks.cn/help/simulink/ug/simulink-data-class-extension-using-matlab-class-syntax.html
title: 定义数据类
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-18 20:59:23
tag: 
summary: 通过创建您自己的数据对象类，自定义您的模型与数据（信号、参数和状态）之间的交互方式。
---
## 定义数据类

此示例说明如何创建 Simulink® 数据类的子类。

使用 MATLAB® 类语法在一个包中创建一个数据类。或者，为数据类指定属性并定义存储类。

*   [使用示例来定义数据类](#mw_86d12ce7-6ac7-441e-81ea-f2b5c48df69a)
    
*   [手动定义数据类](#mw_3974b49a-7b2c-4816-a0c5-efd769af17ee)
    
*   [可选：将属性添加到数据类](#mw_75718e12-12cd-454f-9761-2ec73edeb78d)
    
*   [可选：将初始化代码添加到数据类](#mw_244b5906-68a1-4358-a4f5-f97ad0885f36)
    
*   [可选：定义存储类](#mw_24834be1-80b9-4a91-8fd8-e9f87e9157aa)
    
*   [可选：定义存储类的自定义属性](#mw_703cbf79-aa01-471a-a81a-269d2f9a847d)
    

### 使用示例来定义数据类

1.  查看 `` `matlabroot`/toolbox/simulink/simdemos/dataclasses`` 文件夹中的数据类包 `+SimulinkDemos`（[打开](matlab:cd(fullfile(matlabroot,'/toolbox/simulink/simdemos/dataclasses')))）。
    
    该包包含预定义的数据类。
    
2.  将该文件夹复制到您要用于定义您的数据类的位置。
    
3.  重命名文件夹 `+mypkg` 并将其父文件夹添加到 MATLAB 路径中。
    
4.  修改数据类定义。
    

### 手动定义数据类

1.  创建包文件夹 `+mypkg` 并将其父文件夹添加到 MATLAB 路径中。
    
2.  在 `+mypkg` 内创建类文件夹 `@Parameter` 和 `@Signal`。
    
    **注意**
    
    Simulink 要求在 `+Package/@Class` 文件夹内定义数据类。
    
3.  在 `@Parameter` 文件夹中，创建 MATLAB 文件 `Parameter.m` 并将其打开进行编辑。
    
4.  使用 MATLAB 类语法定义作为 `Simulink.Parameter` 子类的数据类。
    
    ```
    classdef Parameter < Simulink.Parameter
       
    end % classdef
    
    
    ```
    

要使用自定义类名称而不是 `Parameter` 或 `Signal`，请使用自定义名称命名类文件夹。例如，要定义一个类 `mypkg.myParameter`：

*   将数据类定义为 `Simulink.Parameter` 或 `Simulink.Signal` 的子类。
    
    ```
    classdef myParameter < Simulink.Parameter
       
    end % classdef
    
    
    ```
    
*   在类定义中，将构造函数方法命名为 `myParameter` 或 `mySignal`。
    
*   将类文件夹（其中包含类定义）命名为 `@myParameter` 或 `@mySignal`。
    

### 可选：将属性添加到数据类

属性定义模块的前后分别用 `properties` 和 `end` 关键字括起来。

```
classdef Parameter < Simulink.Parameter
	properties % Unconstrained property type
		Prop1 = [];
	end

	properties(PropertyType = 'logical scalar')
		Prop2 = false;
	end

	properties(PropertyType = 'char')
		Prop3 = '';
	end

	properties(PropertyType = 'char',...
      AllowedValues = {'red'; 'green'; 'blue'})
		Prop4 = 'red';
	end
end % classdef


```

如果要将属性添加到 `Simulink.Parameter`、`Simulink.Signal` 或 `Simulink.CustomStorageClassAttributes` 的子类，则可以指定以下属性类型。

<table><colgroup><col width="50%"><col width="50%"></colgroup><thead><tr><th>属性类型</th><th>语法</th></tr></thead><tbody><tr><td>双精度值</td><td><code>properties(PropertyType = 'double scalar')</code></td></tr><tr><td>int32 值</td><td><code>properties(PropertyType = 'int32 scalar')</code></td></tr><tr><td>逻辑值</td><td><code>properties(PropertyType = 'logical scalar')</code></td></tr><tr><td>字符向量 (char)</td><td><code>properties(PropertyType = 'char')</code></td></tr><tr><td>具有有限个允许值的字符向量</td><td><code>properties(PropertyType = 'char', AllowedValues = {'a', 'b', 'c'})</code></td></tr></tbody></table>

如果使用 MATLAB 属性验证（请参阅[验证属性值](https://ww2.mathworks.cn/help/matlab/matlab_oop/validate-property-values.html)），而不是 `PropertyType`，则这些属性将在该类的属性对话框中显示为编辑字段。如果您使用 `PropertyType` 和 `AllowedValues`，则会显示属性对话框：

*   逻辑标量属性的复选框。
    
*   字符向量和 `AllowedValues` 的下拉菜单。
    

### 可选：将初始化代码添加到数据类

您可以将构造函数添加您的数据类中以在实例化该类时执行初始化活动。添加的构造函数不能要求输入参数。

在此示例中，构造函数基于可选的输入参数初始化对象 `obj` 的值。

```
classdef Parameter < Simulink.Parameter
	methods
		function obj = Parameter(optionalValue)
			if (nargin == 1)
				obj.Value = optionalValue;
			end
		end
	end % methods
end % classdef


```

### 可选：定义存储类

使用 `setupCoderInfo` 方法来配置您的类的 `CoderInfo` 对象。然后，创建对 `useLocalCustomStorageClasses` 方法的调用并打开自定义存储类设计器。

1.  在您的数据类内的构造函数中，调用 `useLocalCustomStorageClasses` 方法。
    
    ```
    classdef Parameter < Simulink.Parameter
    	methods
    		function setupCoderInfo(obj)
    			useLocalCustomStorageClasses(obj, 'mypkg');
    			
    			obj.CoderInfo.StorageClass = 'Custom';
    		end
    	end % methods
    end % classdef
    
    
    ```
    
2.  为您的包打开自定义存储类设计器。
    
3.  定义存储类。
    

### 可选：定义存储类的自定义属性

1.  创建 MATLAB 文件 `myCustomAttribs.m` 并将其打开进行编辑。将此文件保存在 `+mypkg/@myCustomAttribs` 文件夹中，其中 `+mypkg` 是包含 `@Parameter` 和 `@Signal` 文件夹的文件夹。
    
2.  使用 MATLAB 类语法定义 `Simulink.CustomStorageClassAttributes` 的子类。例如，考虑一个存储类，它不仅使用原始标识符定义数据，还在生成的代码中提供数据的替代名称。
    
    ```
    classdef myCustomAttribs < Simulink.CustomStorageClassAttributes
    	properties(PropertyType = 'char')
    		AlternateName = '';
    	end
    end % classdef
    
    
    ```
    
3.  覆盖 `isAddressable` 方法的默认实现以确定存储类是否可写。
    
    ```
    classdef myCustomAttribs < Simulink.CustomStorageClassAttributes
    		properties(PropertyType = 'logical scalar')
    			IsAlternateNameInstanceSpecific = true;
    		end
    		
    		methods
    			function retVal = isAddressable(hObj, hCSCDefn, hData)
    				retVal = false;
    			end
    		end % methods
    end % classdef
    
    
    ```
    
4.  覆盖 `getInstanceSpecificProps` 方法的默认实现。
    
    有关示例，请参阅以下覆盖函数：
    
    ```
    function props = getInstanceSpecificProps(hObj)
        % GETINSTANCESPECIFICPROPERTIES  Return instance-specific properties
        %   (custom attributes that can be modified on each data object).
            
          if hObj.IsStructNameInstanceSpecific
            props = findprop(hObj, 'StructName');
          else
            props = [];
          end
        end
    
    ```
    
    ```
    function props = getInstanceSpecificProps(hObj)
        % GETINSTANCESPECIFICPROPERTIES  Return instance-specific properties
        %   (custom attributes that can be modified on each data object).
          props = [];
          
          if hObj.IsOwnerInstanceSpecific
              ptmp = findprop(hObj, 'Owner');
              props = [props; ptmp];
          end
          
          if hObj.IsDefinitionFileInstanceSpecific
              ptmp = findprop(hObj, 'DefinitionFile');
              props = [props; ptmp];
          end
          
          if hObj.IsPersistenceLevelInstanceSpecific
              ptmp = findprop(hObj, 'PersistenceLevel');
              props = [props; ptmp];
          end
        end
    
    ```
    
    **注意**
    
    这是可选步骤。默认情况下，所有自定义属性都特定于实例且对于每个数据对象来说都是可修改的。但是，您可以限制哪些属性可以是特定于实例的。
    
5.  覆盖 `getIdentifiersForInstance` 方法的默认实现以定义数据类的对象的标识符。
    
    **注意**
    
    在其默认实现中，此方法查询数据对象的名称或标识符，并在生成的代码中使用该标识符。通过覆盖此方法，您可以控制生成的代码中您的数据对象的标识符。
    
    ```
    classdef myCustomAttribs < Simulink.CustomStorageClassAttributes
    	properties(PropertyType = 'char')
    		GetFunction = '';
    		SetFunction = '';
    	end
     
    	methods
    		function retVal = getIdentifiersForInstance(hCSCAttrib,...
     hCSCDefn, hData, identifier)
    			retVal = struct('GetFunction',...
     hData.CoderInfo.CustomAttributes.GetFunction, ...
    			'SetFunction', hData.CoderInfo.CustomAttributes.SetFunction);
    		end%
    	end % methods
    end % classdef
    
    ```
    
6.  如果您正在使用已分组的存储类，请覆盖 `getIdentifiersForGroup` 方法的默认实现以指定在生成的代码中该组的标识符。
    
    有关示例，请参阅文件夹 `` `matlabroot`\toolbox\simulink\simulink\dataclasses\+Simulink\@CSCTypeAttributes_FlatStructure`` 中的 `CSCTypeAttributes_FlatStructure.m`（[打开](matlab:cd(fullfile(matlabroot,'toolbox','simulink','simulink','dataclasses','+Simulink','@CSCTypeAttributes_FlatStructure')))）。
    

## 相关主题

*   [数据对象](https://ww2.mathworks.cn/help/simulink/ug/working-with-data-objects.html)

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

Select a Web Site

Choose a web site to get translated content where available and see local events and offers. Based on your location, we recommend that you select: .

You can also select a web site from the following list:

[Contact your local office](#)