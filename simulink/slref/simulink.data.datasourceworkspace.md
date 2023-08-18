---
url: https://ww2.mathworks.cn/help/simulink/slref/simulink.data.datasourceworkspace.html
title: Contains data for external data source - MATLAB
- MathWorks 中国
date: 2023-08-18 20:58:21
tag: 
summary: A Simulink.data.DataSourceWorkspace object is a handle object that can contain data from an external ......
---
Main Content

Contains data for external data source

_Since R2022b_

## Description

A `Simulink.data.DataSourceWorkspace` object is a handle object that can contain data from an external data source.

When you load data from an external source, for example by using the [`loadVariablesFromExternalSource`](https://ww2.mathworks.cn/help/simulink/slref/simulink.simulationinput.loadvariablesfromexternalsource.html) function, Simulink® passes a reference to a `Simulink.data.DataSourceWorkspace` object to the custom file adapter that can read that external file format. The file adapter, derived from the [`Simulink.data.adapters.BaseMatlabFileAdapter`](https://ww2.mathworks.cn/help/simulink/slref/simulink.data.adapters.basematlabfileadapter-class.html) class, loads the data into the workspace in the form of MATLAB® variables and data objects. The workspace object acts as a cache for Simulink and can run a MATLAB script, similar to a base workspace. Symbol resolution includes symbols in the workspace. See [`run`](https://ww2.mathworks.cn/help/simulink/slref/simulink.data.datasourceworkspace.run.html).

## Creation

Only Simulink can create and manage a `Simulink.data.DataSourceworkspace` object, but you can create another handle for the workspace object by assignment.

```
copyWks = 
DataSourceWorkspace with no properties

```

To test a custom file adapter without creating a data source workspace, use the [`Simulink.data.adapters.AdapterDataTester`](https://ww2.mathworks.cn/help/simulink/slref/simulink.data.adapters.adapterdatatester.html) object.

## Examples

collapse all

### Load Data from External Source File into Workspace

Define a [`getData`](https://ww2.mathworks.cn/help/simulink/slref/simulink.data.adapters.basematlabfileadapter.getdata.html) method for an XML file adapter to populate the data from an XML file into the data source workspace that Simulink passes to the method.

This [`getData`](https://ww2.mathworks.cn/help/simulink/slref/simulink.data.adapters.basematlabfileadapter.getdata.html) method reads an XML file in the following format:

```
<customerData>
<x>10</x>
<y>15</y>
</customerData>

```

```
function diagnostic = getData(adapterObj, sourceWorkspace, previousChecksum, diagnostic)​
    % Each time getData is called on the same source, sourceWorkspace is the same as
    % the last time it was called. Clear it to make sure no old variables exist.
    clearAllVariables(sourceWorkspace);
    dom = xmlread(adapterObj.source);
    tree = dom.getFirstChild;
    if tree.hasChildNodes
        item = tree.getFirstChild;
        while ~isempty(item)
            name = item.getNodeName.toCharArray';
            if isvarname(name)
                value = item.getTextContent;
                setVariable(sourceWorkspace, name, str2num(value));
            end
            item = item.getNextSibling;
        end
    end
end

```

## Version History

**Introduced in R2022b**

What is one thing we can do to improve this information or the software described on this page?

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

![](https://ww2.mathworks.cn/images/responsive/global/pic-header-mathworks-logo2.svg)

Select a Web Site

Choose a web site to get translated content where available and see local events and offers. Based on your location, we recommend that you select: .

You can also select a web site from the following list:

[Contact your local office](#)