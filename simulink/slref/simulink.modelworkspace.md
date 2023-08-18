---
url: https://ww2.mathworks.cn/help/simulink/slref/simulink.modelworkspace.html
title: 以编程方式与模型的模型工作区进行交互 - MATLAB
- MathWorks 中国
date: 2023-08-18 20:57:17
tag: 
summary: 使用 Simulink.ModelWorkspace 对象与模型工作区进行交互。例如，您可以添加和删除变量、设置工作区的数据源以及将更改保存到工作区。
---
Main Content

## 说明

使用 `Simulink.ModelWorkspace` 对象与模型工作区进行交互。例如，您可以添加和删除变量、设置工作区的数据源以及将更改保存到工作区。

有关详细信息，请参阅[模型工作区](https://ww2.mathworks.cn/help/simulink/ug/using-model-workspaces.html)。

## 创建对象

要创建 `Simulink.ModelWorkspace`，请使用 [`get_param`](https://ww2.mathworks.cn/help/simulink/slref/get_param.html) 函数查询模型参数 `ModelWorkspace` 的值。例如，要创建名为 `mdlWks` 的对象（该对象表示名为 `myModel.slx` 的模型的模型工作区），请执行以下代码：

```
mdlWks = get_param('myModel','ModelWorkspace')

```

## 属性

全部展开

### `DataSource` — 用于初始化模型工作区中变量的源  
`'Model File'` （默认） | `'MAT-File'` | `'MATLAB Code'` | `'MATLAB File'`

用于初始化模型工作区中变量的源，指定为以下字符向量之一：

*   `'Model File'` - 变量存储在模型文件中。保存模型时，会同时保存变量。
    
*   `'MATLAB Code'` - 变量由您编写并存储在模型文件中的 MATLAB 代码创建。
    
*   `'MAT-File'` - 变量存储在 MAT 文件中，您可以从模型文件中单独管理和操作这些变量。
    
*   `'MATLAB File'` - 变量由脚本文件中的 MATLAB 代码创建，您可以从模型文件中单独管理和操作这些变量。
    

**数据类型：** `char`

### `FileName` — 存储或创建变量的外部文件的名称  
`''`（空字符向量） （默认） | 字符向量

存储或创建变量的外部文件的名称，指定为字符向量。要启用此属性，请将 `DataSource` 设置为 `'MAT-File'` 或 `'MATLAB File'`。

**示例：** `'myFile.mat'`

**示例：** `'myFile.m'`

**数据类型：** `char`

### `MATLABCode` — 用于初始化变量的 MATLAB 代码  
`''`（空字符向量） （默认） | 字符向量

用于初始化变量的 MATLAB 代码，指定为字符向量。要启用此属性，请将 `DataSource` 设置为 `'MATLAB Code'`。

**示例：** `sprintf('%% Create variables that this model uses.\n\nK = 0.00983;\n\nP = Simulink.Parameter(5);')`

**数据类型：** `char`

## 对象函数

<table><tbody><tr><td><a href="https://ww2.mathworks.cn/help/simulink/slref/simulink.modelworkspace.getvariable.html"><code>getVariable</code></a></td><td>在模型的模型工作区中返回变量的值</td></tr><tr><td><a href="https://ww2.mathworks.cn/help/simulink/slref/simulink.modelworkspace.getvariablepart.html" hreflang="en"><code>getVariablePart</code></a></td><td>Get value of variable property in model workspace</td></tr><tr><td><a href="https://ww2.mathworks.cn/help/simulink/slref/simulink.modelworkspace.setvariablepart.html" hreflang="en"><code>setVariablePart</code></a></td><td>Set property of variable in model workspace</td></tr><tr><td><a href="https://ww2.mathworks.cn/help/simulink/slref/simulink.modelworkspace.hasvariable.html" hreflang="en"><code>hasVariable</code></a></td><td>Determine whether variable exists in the model workspace of a model</td></tr><tr><td><a href="https://ww2.mathworks.cn/help/simulink/slref/simulink.modelworkspace.whos.html" hreflang="en"><code>whos</code></a></td><td>Return list of variables in the model workspace of a model</td></tr><tr><td><a href="https://ww2.mathworks.cn/help/simulink/slref/simulink.modelworkspace.savetosource.html" hreflang="en"><code>saveToSource</code></a></td><td>Save model workspace changes to the external data source of the model workspace</td></tr><tr><td><a href="https://ww2.mathworks.cn/help/simulink/slref/simulink.modelworkspace.save.html"><code>save</code></a></td><td>将模型工作区的内容保存到 MAT 文件中</td></tr><tr><td><a href="https://ww2.mathworks.cn/help/simulink/slref/simulink.modelworkspace.reload.html" hreflang="en"><code>reload</code></a></td><td>Reinitialize variables from the data source of a model workspace</td></tr><tr><td><a href="https://ww2.mathworks.cn/help/simulink/slref/simulink.modelworkspace.evalin.html" hreflang="en"><code>evalin</code></a></td><td>Evaluate expression in the model workspace of a model</td></tr><tr><td><a href="https://ww2.mathworks.cn/help/simulink/slref/simulink.modelworkspace.clear.html"><code>clear</code></a></td><td>从模型的模型工作区中清除变量</td></tr><tr><td><a href="https://ww2.mathworks.cn/help/simulink/slref/simulink.modelworkspace.assignin.html"><code>assignin</code></a></td><td>为模型的模型工作区中的变量赋值</td></tr></tbody></table>

## 示例

全部折叠

在模型的模型工作区中创建一个变量。然后，修改该变量并查询变量值以确认修改。

打开示例模型 `vdp`。

创建表示 `vdp` 模型工作区的 `Simulink.ModelWorkspace` 对象 `mdlWks`。

```
mdlWks = get_param('vdp','ModelWorkspace');

```

在模型工作区中创建具有值 `5.12` 的变量 `myVar`。

```
assignin(mdlWks,'myVar',5.12)

```

应用新值 `7.22`。为此，请首先使用 `getVariable` 函数创建变量的临时副本。然后，修改副本并使用它覆盖模型工作区中的原始变量。

```
temp = getVariable(mdlWks,'myVar');
temp = 7.22;
assignin(mdlWks,'myVar',temp)

```

通过查询变量的值来确认新值。

```
getVariable(mdlWks,'myVar')

```

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

![](https://ww2.mathworks.cn/images/responsive/global/pic-header-mathworks-logo2.svg)

Select a Web Site

Choose a web site to get translated content where available and see local events and offers. Based on your location, we recommend that you select: .

You can also select a web site from the following list:

[Contact your local office](#)