---
url: https://ww2.mathworks.cn/help/matlab/ref/validateattributes.html
title: 检查数组的有效性 - MATLAB validateattributes
- MathWorks 中国
date: 2023-07-20 10:11:08
tag: 
summary: 此 MATLAB 函数 验证数组 A 是否属于至少一个指定的类（或其子类）并具有所有指定的属性。
---
## 说明

[示例](https://ww2.mathworks.cn/help/matlab/ref/validateattributes.html#btnqd1y-2)

``validateattributes([`A`](#btnqd0h-A),[`classes`](#btnqd0h-classes),[`attributes`](#btnqd0h-attributes))`` 验证数组 `A` 是否属于至少一个指定的类（或其子类）并具有所有指定的属性。如果 `A` 不符合条件，MATLAB® 将引发错误，并显示一条格式化的错误消息。否则，`validateattributes` 将完成并且不显示任何输出。

## 示例

全部折叠

```
classes = {'numeric'};
attributes = {'size',[4,6,2]};

A = rand(3,5,2);
validateattributes(A,classes,attributes)

```

```
Expected input to be of size 4x6x2 when it is actually size 3x5x2.

```

因为 `A` 与指定的属性不匹配，所以 MATLAB 将引发一条错误消息。

确定数组是递增还是非递减。

```
A = [1 5 8 2;
     9 6 9 4]
validateattributes(A, {'double'},{'nondecreasing'})
validateattributes(A, {'double'},{'increasing'})


```

由于 `A` 同时具有递增和非递减的特点，因此 `validateattributes` 在进行这两项属性检查时均不会引发错误。

将 `A(2,3)` 设置为等于 `A(1,3)` 会导致产生不再严格递增的列，因此 `validateattributes` 会引发错误。

```
A(2,3) = 8
validateattributes(A, {'double'},{'increasing'})

```

```
A =

     1     5     8     2
     9     6     8     4

Expected input to be strictly increasing.


```

但是，由于各列元素均大于等于其上一个列元素，因此，这些列仍然为非递减。下列代码不会引发错误。

```
validateattributes(A, {'double'},{'nondecreasing'})


```

假定 `a` 是某个函数的第二个输入参数，检查它是否为非负数。

```
a = complex(1,1);
validateattributes(a,{'numeric'},{'nonnegative'},2)

```

```
Expected input number 2 to be nonnegative.

```

因为复数缺少在复平面中明确定义的排序，`validateattributes` 无法将复数识别为正数或负数。

检查数组中的值是否为从 0 到 10 的 8 位整数。

假定此代码位于调用 `Rankings` 的函数中。

```
classes = {'uint8','int8'};
attributes = {'>',0,'<',10};
funcName = 'Rankings';
A = int8(magic(4));

validateattributes(A,classes,attributes,funcName)

```

```
Error using Rankings
Expected input to be an array with all of the values < 10.

```

创建一个通过 `inputParser` 检查输入参数的自定义函数，并使用 `validateattributes` 作为 `addRequired` 和 `addOptional` 方法的验证函数。

定义该函数。

```
function a = findArea(shape,dim1,varargin)
   p = inputParser;
   charchk = {'char'};
   numchk = {'numeric'};
   nempty = {'nonempty'};

   addRequired(p,'shape',@(x)validateattributes(x,charchk,nempty))
   addRequired(p,'dim1',@(x)validateattributes(x,numchk,nempty))
   addOptional(p,'dim2',1,@(x)validateattributes(x,numchk,nempty))
   parse(p,shape,dim1,varargin{:})

   switch shape
      case 'circle'
         a = pi * dim1.^2;
      case 'rectangle'
         a = dim1 .* p.Results.dim2;
   end
end

```

调用该函数并且第三个输入为非数值。

```
myarea = findArea('rectangle',3,'x')

```

```
Error using findArea (line 10)
The value of 'dim2' is invalid. Expected input to be one of these types:

double, single, uint8, uint16, uint32, uint64, int8, int16, int32, int64

```

检查函数的输入，并在生成的错误中包括有关输入名称和位置的信息。

定义该函数。

```
function v = findVolume(shape,ht,wd,ln)
   validateattributes(shape,{'char'},{'nonempty'},mfilename,'Shape',1)
   validateattributes(ht,{'numeric'},{'nonempty'},mfilename,'Height',2)
   validateattributes(wd,{'numeric'},{'nonempty'},mfilename,'Width',3)
   validateattributes(ln,{'numeric'},{'nonempty'},mfilename,'Length',4)

```

调用该函数而不带 `shape` 输入参数。

```
Error using findVolume
Expected input number 1, Shape, to be one of these types:

char

Instead its type was double.

Error in findVolume (line 2)
validateattributes(shape,{'char'},{'nonempty'},mfilename,'Shape',1)

```

函数名称作为错误标识符的一部分。

```
MException.last.identifier

```

```
ans =

MATLAB:findVolume:invalidType

```

## 输入参数

全部折叠

输入，指定为任何类型的数组。

**数据类型：** `single` | `double` | `int8` | `int16` | `int32` | `int64` | `uint8` | `uint16` | `uint32` | `uint64` | `logical` | `char` | `string` | `struct` | `cell` | `function_handle`
**复数支持：** 是

### `classes` — 有效数据类型

字符向量 | 字符向量元胞数组 | 字符串数组

有效属性类型，指定为字符向量、字符向量元胞数组或字符串数组。`classes` 的每个元素都可以是任何内置或自定义类的名称，包括：

<table><colgroup><col width="32%"><col width="69%"></colgroup><tbody><tr><td><code>'half'</code></td><td>半精度数</td></tr><tr><td><code>'single'</code></td><td>单精度数</td></tr><tr><td><code>'double'</code></td><td>双精度数</td></tr><tr><td><code>'int8'</code></td><td>有符号 8 位整数</td></tr><tr><td><code>'int16'</code></td><td>有符号 16 位整数</td></tr><tr><td><code>'int32'</code></td><td>有符号 32 位整数</td></tr><tr><td><code>'int64'</code></td><td>有符号 64 位整数</td></tr><tr><td><code>'uint8'</code></td><td>无符号 8 位整数</td></tr><tr><td><code>'uint16'</code></td><td>无符号 16 位整数</td></tr><tr><td><code>'uint32'</code></td><td>无符号 32 位整数</td></tr><tr><td><code>'uint64'</code></td><td>无符号 64 位整数</td></tr><tr><td><code>'logical'</code></td><td>逻辑值 <code>1</code> (<code>true</code>) 或 <code>0</code> (<code>false</code>)</td></tr><tr><td><code>'char'</code></td><td>字符</td></tr><tr><td><code>'string'</code></td><td>字符串数组</td></tr><tr><td><code>'struct'</code></td><td>结构体数组</td></tr><tr><td><code>'cell'</code></td><td>元胞数组</td></tr><tr><td><code>'table'</code></td><td>表</td></tr><tr><td><code>'timetable'</code></td><td>时间表</td></tr><tr><td><code>'function_handle'</code></td><td>函数句柄</td></tr></tbody></table>

<table><colgroup><col width="32%"><col width="69%"></colgroup><tbody><tr><td><code>'numeric'</code></td><td><code>isa(A,'numeric')</code> 函数为其返回 true 的任何数据类型，包括 <code>int8</code>、<code>int16</code>、<code>int32</code>、<code>int64</code>、<code>uint8</code>、<code>uint16</code>、<code>uint32</code>、<code>uint64</code>、<code>single</code> 或 <code>double</code></td></tr><tr><td><code>'&lt;<code>class_name</code>&gt;'</code></td><td>任何其他类名</td></tr></tbody></table>

**数据类型：** `cell` | `string`

### `attributes` — 有效属性

元胞数组 | 字符串数组

有效属性，指定为元胞数组或字符串数组。

某些属性还需要数值，例如，用于指定 [`A`](#btnqd0h-A) 的元素的大小或数量的属性。对于这些属性，数值或向量必须紧跟在元胞数组中的属性名称的后面。字符串数组不能用于表示 `attributes` 中的数值。

这些属性描述数组 `A` 的大小和形状。

<table><colgroup><col width="29%"><col width="71%"></colgroup><tbody><tr><td><code>'2d'</code></td><td>二维数组，包括标量、向量、矩阵和空数组</td></tr><tr><td><code>'3d'</code></td><td>维度不大于三的数组</td></tr><tr><td><code>'column'</code></td><td>列向量，<code>N</code>×1</td></tr><tr><td><code>'row'</code></td><td>行向量，1×<code>N</code></td></tr><tr><td><code>'scalar'</code></td><td>标量值，1×1</td></tr><tr><td><code>'scalartext'</code></td><td>字符串标量或字符向量，包括无任何字符的输入</td></tr><tr><td><code>'vector'</code></td><td>行或列向量，或标量值</td></tr><tr><td><code>'size', [d1,...,dN]</code></td><td>维度为 <code>d1</code>-×-...-×-<code>dN</code> 的数组。要跳过检查特定维度的步骤，请为该维度指定 <code>NaN</code>，例如 <code>[3,4,NaN,2]</code>。</td></tr><tr><td><code>'numel', N</code></td><td>具有 <code>N</code> 个元素的数组</td></tr><tr><td><code>'ncols', N</code></td><td>具有 <code>N</code> 列的数组</td></tr><tr><td><code>'nrows', N</code></td><td>具有 <code>N</code> 行的数组</td></tr><tr><td><code>'ndims', N</code></td><td><code>N</code> 维数组</td></tr><tr><td><code>'square'</code></td><td>方阵；换句话说，行数和列数相等的二维数组</td></tr><tr><td><code>'diag'</code></td><td>对角矩阵</td></tr><tr><td><code>'nonempty'</code></td><td>不存在为零的维度</td></tr><tr><td><code>'nonsparse'</code></td><td>非稀疏数组</td></tr></tbody></table>

这些属性为 `A` 中的值指定有效范围。

<table><colgroup><col width="29%"><col width="71%"></colgroup><tbody><tr><td><code>'&gt;', N</code></td><td>大于 <code>N</code> 的所有值</td></tr><tr><td><code>'&gt;=', N</code></td><td>大于或等于 <code>N</code> 的所有值</td></tr><tr><td><code>'&lt;', N</code></td><td>小于 <code>N</code> 的所有值</td></tr><tr><td><code>'&lt;=', N</code></td><td>小于或等于 <code>N</code> 的所有值</td></tr><tr><td><code>'finite'</code></td><td>所有值均为有限值</td></tr><tr><td><code>'nonnan'</code></td><td>没有 NaN 值（非数字）</td></tr></tbody></table>

这些属性检查数值数组或逻辑数组 `A` 中值的类型。

<table><colgroup><col width="29%"><col width="71%"></colgroup><tbody><tr><td><code>'binary'</code></td><td>由一和零组成的数组</td></tr><tr><td><code>'even'</code></td><td>由偶数（包括零）组成的数组</td></tr><tr><td><code>'odd'</code></td><td>由奇数组成的数组</td></tr><tr><td><code>'integer'</code></td><td>由整数值组成的数组</td></tr><tr><td><code>'real'</code></td><td>由实数值组成的数组</td></tr><tr><td><code>'nonnegative'</code></td><td>没有小于零的元素</td></tr><tr><td><code>'nonzero'</code></td><td>没有等于零的元素</td></tr><tr><td><code>'positive'</code></td><td>没有小于或等于零的元素</td></tr><tr><td><code>'decreasing'</code></td><td>一列中的每个元素均小于其前面的元素，且无 <code>NaN</code> 元素。</td></tr><tr><td><code>'increasing'</code></td><td>一列中的每个元素均大于其前面的元素，且无 <code>NaN</code> 元素。</td></tr><tr><td><code>'nondecreasing'</code></td><td>一列中的每个元素均大于或等于其前面的元素，且无 <code>NaN</code> 元素。</td></tr><tr><td><code>'nonincreasing'</code></td><td>一列中的每个元素均小于或等于其前面的元素，且无 <code>NaN</code> 元素。</td></tr></tbody></table>

**数据类型：** `cell`

### `funcName` — 用于验证的函数的名称

字符向量 | 字符串标量

用于验证的函数的名称，指定为字符向量或字符串标量。如果指定空字符向量 `''` 或 `<missing>` 字符串，则 `validateattributes` 函数将忽略 `funcName` 输入。

**数据类型：** `char` | `string`

### `varName` — 输入变量的名称

字符向量 | 字符串标量

输入变量的名称，指定为字符向量或字符串标量。如果指定空字符向量 `''` 或 `<missing>` 字符串，则 `validateattributes` 函数将忽略 `varName` 输入。

**数据类型：** `char` | `string`

输入参数的位置，指定为正整数。

**数据类型：** `double`

## 扩展功能

### C/C++ 代码生成

使用 MATLAB® Coder™ 生成 C 代码和 C++ 代码。

用法说明和限制：

- 某些错误消息是 MATLAB 错误消息的简化版本。
- `classes`、`funcName`、`varName` 和 `argIndex` 参数必须为常量。
- 属性名称必须为常量。
- 在生成的代码中，错误消息中的数字格式可能与 MATLAB 中的格式不同。例如，以下是 MATLAB 中的错误消息：

  ```
  Expected input to be an array with all of the values > 3.

  ```

  以下是生成的代码中的错误消息：

  ```
  Expected input to be an array with all of the values >   3.000000000000000e+00.


  ```
- 半精度数据类型支持 `scalar` 和 `real` 属性。

### 基于线程的环境

使用 MATLAB® `backgroundPool` 在后台运行代码或使用 Parallel Computing Toolbox™ `ThreadPool` 加快代码运行速度。

### GPU 数组

通过使用 Parallel Computing Toolbox™ 在图形处理单元 (GPU) 上运行来加快代码执行。

此函数完全支持 GPU 数组。有关详细信息，请参阅 [Run MATLAB Functions on a GPU](https://ww2.mathworks.cn/help/parallel-computing/run-matlab-functions-on-a-gpu.html) (Parallel Computing Toolbox)。

### 分布式数组

使用 Parallel Computing Toolbox™ 在集群的组合内存中对大型数组进行分区。

此函数完全支持分布式数组。有关详细信息，请参阅 [Run MATLAB Functions with Distributed Arrays](https://ww2.mathworks.cn/help/parallel-computing/run-matlab-functions-with-distributed-arrays.html) (Parallel Computing Toolbox)。

本页内容对您有帮助吗？

Unrated

1 star

2 stars

3 stars

4 stars

5 stars

Unrated 1 star 2 stars 3 stars 4 stars 5 stars

您点击的链接对应于以下 MATLAB 命令：

请在 MATLAB 命令行窗口中直接输入以执行命令。Web 浏览器不支持 MATLAB 命令。

Select a Web Site

Choose a web site to get translated content where available and see local events and offers. Based on your location, we recommend that you select: **中国**.

You can also select a web site from the following list:

## How to Get Best Site Performance

Select the China site (in Chinese or English) for best site performance. Other MathWorks country sites are not optimized for visits from your location.

[Contact your local office](https://ww2.mathworks.cn/)
