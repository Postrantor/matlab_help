---
url: https://ww2.mathworks.cn/help/simulink/slref/simulink.mdlinfo.html
title: 在不加载文件的情况下提取 SLX、SLXP 或 MDL 文件信息 - MATLAB
- MathWorks 中国
date: 2023-07-19 10:28:16
tag: #版本管理 #simulink
summary: Simulink.MDLInfo 对象从 SLX、SLXP 或 MDL 文件中提取信息，而不将其加载到内存中。
---
# Simulink.MDLInfo

在不加载文件的情况下提取 SLX、SLXP 或 MDL 文件信息

[全页展开](javascript:void(0);)

## 说明

`Simulink.MDLInfo` 对象从 SLX、SLXP 或 MDL 文件中提取信息，而不将其加载到内存中。

要在不创建 `MDLInfo` 对象的情况下从文件中提取说明和元数据，请分别使用 [`Simulink.MDLInfo.getDescription`](simulink.mdlinfo.getdescription.html) 和 [`Simulink.MDLInfo.getMetadata`](simulink.mdlinfo.getmetadata.html) 函数。

## 创建对象

### 语法

`[info = Simulink.MDLInfo(file)](#d124e307628)`

### 描述

[示例](simulink.mdlinfo.html#mw_5c766a16-a0e9-40a8-aecb-49a2270f029e)

`` `info` = Simulink.MDLInfo([`file`](#mw_5c8c7788-49d8-4830-b14f-3f251892eabb))`` 创建名为 `info` 的 `MDLInfo` 对象，并用指定模型文件中的信息填充属性。

### 输入参数

[全部展开](javascript:void(0);)

### `file` — SLX、SLXP 或 MDL 文件的名称

字符向量 | 字符串标量

SLX、SLXP 或 MDL 文件的名称，指定为字符向量或字符串标量。

文件名可以包括部分路径、完整路径、相对路径或无路径。如果不提供路径，文件扩展名是可选的。

为避免共享同一名称的遮蔽文件导致意外结果，请指定完全限定的文件名。

**示例：** `Simulink.MDLInfo('vdp')`

**示例：** `Simulink.MDLInfo('mymodel.slx')`

**示例：** `Simulink.MDLInfo('mydir/mymodel.slx')`

**示例：** `Simulink.MDLInfo('C:/mydir/mymodel.slx')`

**数据类型：** `char` | `string`

## 属性

[全部展开](javascript:void(0);)

### 文件名和内容

### `BlockDiagramName` — 模块图的名称。

字符向量

此 属性 为只读。

模块图的名称，以字符向量形式返回。

模块图的名称与文件名匹配，但没有扩展名。

**数据类型：** `char`

### `BlockDiagramType` — 文件类型

字符向量

此 属性 为只读。

文件类型，以字符向量形式返回。

**数据类型：** `char`

### `FileName` — 完全限定的文件名

字符向量

此 属性 为只读。

完全限定的文件名，以字符向量形式返回。

**数据类型：** `char`

### `Interface` — 输入、输出和引用的说明

结构体

此 属性 为只读。

输入、输出和引用的说明，以结构体形式返回。

该结构体包括顶层端口、模型引用和子系统引用的名称和属性。

**数据类型：** `struct`

### `IsLibrary` — true 或 false 结果

`1` | `0`

此 属性 为只读。

true 或 false 结果，以数据类型 `logical` 的 `1` 或 `0` 形式返回。

- `1` (`true`) - 文件是库。
- `0` (`false`) - 文件不是库。

**数据类型：** `logical`

### 用户指定的信息

### `Description` — 用户指定的说明

字符向量

此 属性 为只读。

用户为文件指定的说明，以字符向量形式返回。

#### 提示

- 要在不加载模型或创建 `MDLInfo` 对象的情况下提取说明，请使用 [`Simulink.MDLInfo.getDescription`](simulink.mdlinfo.getdescription.html) 函数。
- 要在不加载模型或创建 `MDLInfo` 对象的情况下查看说明，请在 MATLAB® 命令行窗口中输入：

  ```
  help 'mymodelname'

  ```
- 要查看一个打开模型的说明，请在 “模型属性” 对话框中打开**说明**选项卡。

**数据类型：** `char`

### `Metadata` — 任意数据的名称和值

结构体

此 属性 为只读。

与文件相关联的任意数据的名称和值，以结构体形式返回。

结构体字段可以是字符向量、`double` 类型的数值矩阵或结构体。

#### 提示

要在不加载模型或创建 `MDLInfo` 对象的情况下提取元数据结构体，请使用 [`Simulink.MDLInfo.getMetadata`](simulink.mdlinfo.getmetadata.html) 函数。

**数据类型：** `struct`

### 保存信息

### `ReleaseUpdateLevel` — 用于保存文件的发行版更新

正整数

此 属性 为只读。

用于保存文件的发行版更新，以正整数形式返回。

- `0` - 文件保存在正式发行版中，例如，`'R2020a'`，或保存在 R2020a 之前的版本中。
- 正整数 - 文件保存在更新发行版中，例如，`2` 表示模型保存在 `'R2020a Update 2'` 中。

**数据类型：** `int32`

### `LastModifiedBy` — 上次保存文件的用户的名称

字符向量

此 属性 为只读。

上次保存文件的用户的名称，以字符向量形式返回。

**数据类型：** `char`

### `LastSavedArchitecture` — 用于保存文件的平台

字符向量

此 属性 为只读。

用于保存文件的平台，以字符向量形式返回。

**示例：** `'glnxa64'`

**数据类型：** `char`

### `ModelVersion` — 版本号

字符向量

此 属性 为只读。

文件的版本号，以字符向量形式返回。

**数据类型：** `char`

### `ReleaseName` — 用于保存文件的 MATLAB 版本

字符向量

此 属性 为只读。

用于保存文件的 MATLAB 版本，以字符向量形式返回。

**示例：** `'R2020a'`

**数据类型：** `char`

### `SavedCharacterEncoding` — 字符编码

字符向量

此 属性 为只读。

保存文件时的字符编码，以字符向量形式返回。

**示例：** `'UTF-8'`

**数据类型：** `char`

### `SimulinkVersion` — 用于保存文件的 Simulink® 版本号

字符向量

此 属性 为只读。

用于保存文件的 Simulink 版本号，以字符向量形式返回。

**示例：** `'10.1'`

**数据类型：** `char`

## 示例

[全部折叠](javascript:void(0);)

### 获取模型信息

创建一个对应于 `vdp.slx` 文件的 `Simulink.MDLInfo` 对象。

```
info = Simulink.MDLInfo('vdp.slx');

```

通过使用圆点表示法来访问属性值，获取有关文件的信息，如文件类型。

```
type = info.BlockDiagramType

```

```
type =

    'Model'

```

`vdp` 是模型文件。

### 在不加载顶层模型的情况下查找引用模型

获取有关 `sldemo_mdlref_depgraph` 模型的信息。

```
info = Simulink.MDLInfo('sldemo_mdlref_depgraph');

```

获取接口信息。

```
info.Interface

```

```
ans =

  struct with fields:

                       Inports: [0×1 struct]
                      Outports: [0×1 struct]
                     Trigports: [0×1 struct]
                   Enableports: [0×1 struct]
                  ModelVersion: '1.84'
           SubsystemReferences: {0×1 cell}
               ModelReferences: {4×1 cell}
        ParameterArgumentNames: ''
            TestPointedSignals: [0×1 struct]
             ProvidedFunctions: [0×1 struct]
         IsExportFunctionModel: 0
           IsArchitectureModel: 0
    IsAUTOSARArchitectureModel: 0
                   ResetEvents: [0×1 struct]
            HasInitializeEvent: 0
             HasTerminateEvent: 0
    PreCompExecutionDomainType: 'Unset'
            ParameterArguments: [0×1 struct]
         ExternalFileReference: [4×1 struct]

```

获取引用模型。

```
info.Interface.ModelReferences


```

```
ans =

  4×1 cell array

    {'sldemo_mdlref_depgraph/heat2cost|sldemo_mdlref_heat2cost'      }
    {'sldemo_mdlref_depgraph/house|sldemo_mdlref_house'              }
    {'sldemo_mdlref_depgraph/outdoor temp|sldemo_mdlref_outdoor_temp'}
    {'sldemo_mdlref_depgraph/thermostat|sldemo_mdlref_heater'        }

```

### 添加和检查文件元数据

创建一个包含元数据信息的结构体。

```
m.TestStatus = 'untested';
m.ExpectedCompletionDate = '01/01/2011';

```

创建一个模型，更新 `'Metadata'` 参数，并将元数据保存在模型中。

```
new_system('MDLInfoMetadataModel')
set_param('MDLInfoMetadataModel','Metadata',m)
save_system('MDLInfoMetadataModel')

```

使用 `MDLInfo` 对象检查模型的元数据。

```
info = Simulink.MDLInfo('MDLInfoMetadataModel');
info.Metadata

```

```
ans =

  struct with fields:

                TestStatus: 'untested'
    ExpectedCompletionDate: '01/01/2011'

```

## 版本历史记录

**在 R2009b 中推出**

## 另请参阅

[`Simulink.MDLInfo.getDescription`](simulink.mdlinfo.getdescription.html) | [`Simulink.MDLInfo.getMetadata`](simulink.mdlinfo.getmetadata.html)

### 主题

- [管理模型版本并指定模型属性](../ug/managing-model-versions.html)
