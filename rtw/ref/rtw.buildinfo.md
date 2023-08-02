---
url: https://ww2.mathworks.cn/help/rtw/ref/rtw.buildinfo.html
title: 提供用于编译和链接生成的代码的信息 - MATLAB
- MathWorks 中国
date: 2023-07-20 17:25:33
tag: 
summary: RTW.BuildInfo 对象包含用于编译和链接生成的代码的信息。
---
# RTW.BuildInfo

提供用于编译和链接生成的代码的信息

[全页展开](javascript:void(0);)

## 说明

`RTW.BuildInfo` 对象包含用于编译和链接生成的代码的信息。

## 创建对象

### 语法

`[buildInformation = RTW.BuildInfo](#d124e2820)`

### 描述

[示例](rtw.buildinfo.html#mw_916d08a7-cbdb-4c58-94b8-90608f51d795)

`` `buildInformation` = RTW.BuildInfo`` 返回一个编译信息对象。您可以使用该对象来指定用于编译和链接生成的代码的信息。例如：

* 编译器选项
* 预处理器标识符定义
* 链接器选项
* 源文件和路径
* 包含文件和路径
* 预编译的外部库

## 属性

[全部展开](javascript:void(0);)

### `ComponentName`  — 组件名称

字符向量 | 字符串

生成的代码组件的名称。

## 对象函数

<table><tbody><tr><td><a href="rtw.buildinfo.addcompileflags.html" hreflang="en"><code>addCompileFlags</code></a></td><td>Add compiler options to build information</td></tr><tr><td><a href="rtw.buildinfo.adddefines.html" hreflang="en"><code>addDefines</code></a></td><td>Add preprocessor macro definitions to build information</td></tr><tr><td><a href="rtw.buildinfo.addincludefiles.html" hreflang="en"><code>addIncludeFiles</code></a></td><td>Add include files to build information</td></tr><tr><td><a href="rtw.buildinfo.addincludepaths.html" hreflang="en"><code>addIncludePaths</code></a></td><td>Add include paths to build information</td></tr><tr><td><a href="rtw.buildinfo.addlinkflags.html" hreflang="en"><code>addLinkFlags</code></a></td><td>Add link options to build information</td></tr><tr><td><a href="rtw.buildinfo.addlinkobjects.html" hreflang="en"><code>addLinkObjects</code></a></td><td>Add link objects to build information</td></tr><tr><td><a href="rtw.buildinfo.addnonbuildfiles.html" hreflang="en"><code>addNonBuildFiles</code></a></td><td>Add nonbuild-related files to build information</td></tr><tr><td><a href="rtw.buildinfo.addsourcefiles.html" hreflang="en"><code>addSourceFiles</code></a></td><td>Add source files to build information</td></tr><tr><td><a href="rtw.buildinfo.addsourcepaths.html" hreflang="en"><code>addSourcePaths</code></a></td><td>Add source paths to build information</td></tr><tr><td><a href="rtw.buildinfo.addtmftokens.html" hreflang="en"><code>addTMFTokens</code></a></td><td>Add template makefile (TMF) tokens to build information</td></tr><tr><td><a href="rtw.buildinfo.findbuildarg.html" hreflang="en"><code>findBuildArg</code></a></td><td>Find a specific build argument in build information</td></tr><tr><td><a href="rtw.buildinfo.findincludefiles.html" hreflang="en"><code>findIncludeFiles</code></a></td><td>Find and add include (header) files to build information</td></tr><tr><td><a href="rtw.buildinfo.getbuildargs.html" hreflang="en"><code>getBuildArgs</code></a></td><td>Get build arguments from build information</td></tr><tr><td><a href="rtw.buildinfo.getcompileflags.html" hreflang="en"><code>getCompileFlags</code></a></td><td>Get compiler options from build information</td></tr><tr><td><a href="rtw.buildinfo.getdefines.html" hreflang="en"><code>getDefines</code></a></td><td>Get preprocessor macro definitions from build information</td></tr><tr><td><a href="rtw.buildinfo.getfullfilelist.html" hreflang="en"><code>getFullFileList</code></a></td><td>Get list of files from build information</td></tr><tr><td><a href="rtw.buildinfo.getincludefiles.html" hreflang="en"><code>getIncludeFiles</code></a></td><td>Get include files from build information</td></tr><tr><td><a href="rtw.buildinfo.getincludepaths.html" hreflang="en"><code>getIncludePaths</code></a></td><td>Get include paths from build information</td></tr><tr><td><a href="rtw.buildinfo.getlinkflags.html" hreflang="en"><code>getLinkFlags</code></a></td><td>Get link options from build information</td></tr><tr><td><a href="rtw.buildinfo.getnonbuildfiles.html" hreflang="en"><code>getNonBuildFiles</code></a></td><td>Get nonbuild-related files from build information</td></tr><tr><td><a href="rtw.buildinfo.getsourcefiles.html" hreflang="en"><code>getSourceFiles</code></a></td><td>Get source files from build information</td></tr><tr><td><a href="rtw.buildinfo.getsourcepaths.html" hreflang="en"><code>getSourcePaths</code></a></td><td>Get source paths from build information</td></tr><tr><td><a href="removesourcefiles.html" hreflang="en"><code>removeSourceFiles</code></a></td><td>Remove source files from build information object</td></tr><tr><td><a href="settargetprovidesmain.html" hreflang="en"><code>setTargetProvidesMain</code></a></td><td>Disable inclusion of code generator provided (generated or static) <code>main.c</code> source file during build</td></tr><tr><td><a href="updatefilepathsandextensions.html" hreflang="en"><code>updateFilePathsAndExtensions</code></a></td><td>Update files in build information with missing paths and file extensions</td></tr><tr><td><a href="updatefileseparator.html" hreflang="en"><code>updateFileSeparator</code></a></td><td>Update file separator character for file lists in build information</td></tr></tbody></table>

## 示例

[全部折叠](javascript:void(0);)

### 检索编译信息对象

当您编译生成的代码时，编译过程会在 `buildInfo.mat` 文件中存储 `RTW.BuildInfo` 对象。要从包含 `buildInfo.mat` 文件的代码生成文件夹中检索该对象，请运行：

```
bi=load('buildInfo.mat');
bi.buildInfo

```

```
ans = 

  BuildInfo with properties:

          ComponentName: 'slexAircraftExample'
                 Viewer: []
                 Tokens: [27×1 RTW.BuildInfoKeyValuePair]
              BuildArgs: [13×1 RTW.BuildInfoKeyValuePair]
               MakeVars: []
               MakeArgs: ''
    TargetPreCompLibLoc: ''
        TargetLibSuffix: ''
              ModelRefs: []
                 SysLib: [1×1 RTW.BuildInfoModules]
                   Maps: [1×1 struct]
                LinkObj: []
                Options: [1×1 RTW.BuildInfoOptions]
                    Inc: [1×1 RTW.BuildInfoModules]
                    Src: [1×1 RTW.BuildInfoModules]
                  Other: [1×1 RTW.BuildInfoModules]
                   Path: []
               Settings: [1×1 RTW.BuildInfoSettings]
           DisplayLabel: 'Build Info'
                  Group: ''

```

该对象包含编译信息。

### 配置 `RTW.BuildInfo` 以指定用于编译的代码

此示例说明如何创建 `RTW.BuildInfo` 对象并注册源文件。

创建一个 `RTW.BuildInfo` 对象。

```
buildInfo = RTW.BuildInfo;

```

注册源文件。

```
buildInfo.ComponentName = 'foo1';
addSourceFiles(buildInfo, 'foo1.c');

```

指定编译方法并创建静态库。

```
tmf = fullfile(tmffolder, 'ert_vcx64.tmf');
buildResult1 = codebuild(pwd, buildInfo, tmf)

```

## 版本历史记录

**在 R2006a 中推出**

## 另请参阅

### 主题

* [Customize Post-Code-Generation Build Processing](../ug/customizing-post-code-generation-build-processing.html)
