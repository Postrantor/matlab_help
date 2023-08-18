---
url: https://ww2.mathworks.cn/help/simulink/slref/simulink.modelworkspace.hasvariable.html
title: Determine whether variable exists in the model workspace of a model - MATLAB hasVariable
- MathWorks 中国
date: 2023-08-18 21:00:59
tag: 
summary: This MATLAB function returns 1 if a variable whose name is varName exists in the model workspace repr......
---
Determine whether variable exists in the model workspace of a model

## Description

[example](https://ww2.mathworks.cn/help/simulink/slref/simulink.modelworkspace.hasvariable.html#mw_9607fa97-5a90-419a-aa33-024a18d89ba5)

``[`varExists`](#d124e608666) = hasVariable([`mdlWks`](#d124e608615),[`varName`](#d124e608637))`` returns `1` if a variable whose name is `varName` exists in the model workspace represented by the `Simulink.ModelWorkspace` object `mdlWks`.

## Examples

collapse all

### Determine Existence of Variable in Model Workspace

Open the example model `vdp`.

Create a `Simulink.ModelWorkspace` object that represents the model workspace of `vdp`.

```
mdlWks = get_param('vdp','ModelWorkspace');

```

Create a variable named `myVar` in the model workspace.

```
assignin(mdlWks,'myVar',5.12)


```

Determine whether a variable named `myVar` exists in the model workspace.

```
exists = hasVariable(mdlWks,'myVar')


```

## Input Arguments

collapse all

### `mdlWks` — Target model workspace  
`Simulink.ModelWorkspace` object

Target model workspace, specified as a `Simulink.ModelWorkspace` object.

### `varName` — Name of target variable  
character vector

Name of the target variable, specified as a character vector.

**Example:** `'myVariable'`

**Data Types:** `char`

## Output Arguments

collapse all

### `varExists` — Indication of existence  
`1` | `0`

Indication of variable existence, returned as `1` (true) or `0`.

## Version History

**Introduced in R2012a**