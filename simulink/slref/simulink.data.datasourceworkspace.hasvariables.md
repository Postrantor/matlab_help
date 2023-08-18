---
url: https://ww2.mathworks.cn/help/
.html
title: Check if variables exist in data source workspace - MATLAB hasVariables
- MathWorks 中国
date: 2023-08-18 20:57:37
tag: 
summary: This MATLAB function checks if the specified variables exist in the data source workspace represented......
---
Add some variables to a data source workspace.

```
setVariable(sourceWorkspace,'b', 2);
setVariables(sourceWorkspace,["c" "d"], {3,4});
setVariables(sourceWorkspace,{'e','f'}, {5,6});

```

Check if variable `b` is in the data source workspace.

```
retVal = hasVariables(sourceWorkspace,'b');
retVal

```

```
retVal = 
    true

```

Now check if the variables `a` and `e` are in the data source workspace.

```
retVal = hasVariables(sourceWorkspace,["a" "e"]);
retVal

```

```
retVal = 
    1x2 logical array

      0  1

```