---
url: https://ww2.mathworks.cn/help/simulink/slref/datastoreread.html
title: 从数据存储中读取数据 - Simulink
- MathWorks 中国
date: 2023-08-18 20:59:52
tag: 
summary: Data Store Read 模块将数据从指定数据存储或其所选部分复制到其输出。多个 Data Store Read 模块可从同一个数据存储读取数据。
---
*   ![](https://ww2.mathworks.cn/help/simulink/slref/data_store_read_block_icon.png)
    

![](https://ww2.mathworks.cn/help/simulink/slref/data_store_read_block_icon.png)

**库：**  
Simulink / Signal Routing

## 描述

Data Store Read 模块将数据从指定数据存储或其所选部分复制到其输出。多个 Data Store Read 模块可从同一个数据存储读取数据。

用来读取数据的源数据存储由 Data Store Memory 模块或定义数据存储的信号对象的位置决定。有关详细信息，请参阅[数据存储](https://ww2.mathworks.cn/help/simulink/data-stores.html)和 [Data Store Memory](https://ww2.mathworks.cn/help/simulink/slref/datastorememory.html)。

要从数据存储获取正确的结果，必须确保数据存储按照预期的顺序进行读取和写入。有关详细信息，请参阅[对数据存储访问进行排序](https://ww2.mathworks.cn/help/simulink/ug/model-global-data-using-data-stores.html#brytnz0)和 [数据存储诊断](https://ww2.mathworks.cn/help/simulink/ug/data-store-basics.html#brytn1d-1_1)。

您可以选择 Data Store Read、Data Store Write 或 Data Store Memory 模块来突出显示与其相关的模块。要在打开的图或新选项卡中显示相关模块，请在选择后出现的省略号上暂停。然后，从操作栏中选择**相关模块**

![](https://ww2.mathworks.cn/help/simulink/slref/related-blocks-button.png)

。当多个模块对应于所选模块时，将打开一个相关模块列表。您可以通过在文本框中输入搜索词来筛选相关模块列表。从列表中选择相关模块后，窗口焦点转至显示该相关模块的打开的图或新选项卡。

## 端口

### 输入

全部展开

外部端口，用于指定选择的对应数据存储子元素的索引。

#### 依存关系

要启用外部索引端口，请在**元素选择**选项卡上，选择**启用索引**。然后，在**索引选项**表的第 `N` 行中，将**索引选项**设置为 “`索引向量(端口)`” 或 “`起始索引(端口)`”。

**数据类型：** `int8` | `int16` | `int32` | `uint8` | `uint16`

### 输出

全部展开

### **Port_1** — 来自指定数据存储的值  
标量 | 向量 | 矩阵 | N 维数组

来自指定数据存储的值，使用与数据存储中相同的数据类型和维度数进行输出。该模块同时支持实信号和复信号。您可以选择是输出整个数据存储还是仅输出选定元素。

您可以将总线数组与 Data Store Read 模块结合使用。有关定义和使用总线数组的详细信息，请参阅[使用总线数组组合非虚拟总线](https://ww2.mathworks.cn/help/simulink/ug/combining-buses-into-an-array-of-buses.html)。

**数据类型：** `single` | `double` | `half` | `int8` | `int16` | `int32` | `int64` | `uint8` | `uint16` | `uint32` | `uint64` | `Boolean` | `fixed point` | `enumerated` | `bus` | `image`

## 参数

全部展开

### 参数

### **数据存储名称** — 模块从中读取数据的数据存储的名称  
`A` （默认） | 数据存储的名称

指定此模块从中读取数据的数据存储的名称。旁边的列表列出了模型中与 Data Store Read 模块在同一级别或更高级别的 Data Store Memory 模块的名称。该列表中还包括基础工作区和模型工作区中的所有 `Simulink.Signal` 对象。要更改名称，请从该列表中选择名称，或者直接在编辑字段中输入名称。

当编译包含此模块的模型时，Simulink® 从该模块级别向上搜索模型，寻找具有指定数据存储名称的 Data Store Memory 模块。如果 Simulink 找不到这样的模块，它将在模型工作区和 MATLAB® 工作区中搜索具有相同名称的 [`Simulink.Signal`](https://ww2.mathworks.cn/help/simulink/slref/simulink.signal.html) 对象。如果 Simulink 找到信号对象，它将在模型的根级创建一个隐藏的 Data Store Memory 模块，此模块的属性由信号对象指定，初始值设置为全零数组。该数组的维度从信号对象的 `Dimensions` 属性继承。

如果 Simulink 既找不到 Data Store Memory 模块，也找不到信号对象，它将停止编译并显示错误。有关搜索路径的详细信息，请参阅[符号解析](https://ww2.mathworks.cn/help/simulink/ug/resolving-symbols.html)。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><span><strong>模块参数</strong>：<code>DataStoreName</code></span></td></tr><tr><td><span><strong>类型</strong>：字符向量</span></td></tr><tr><td><span><strong>值</strong>：数据存储名称</span></td></tr><tr><td><span><strong>默认值</strong>：<code>'A'</code></span></td></tr></tbody></table>

### **Data Store Memory 模块** — Data Store Memory 模块名称  
模块路径

此 参数 为只读。

此字段列出 Data Store Memory 模块，该模块对它要从中读取数据的存储进行了初始化。

### **对应的 Data Store Write 模块** — 对应的 Data Store Write 模块列表  
模块路径

此 参数 为只读。

此字段列出符合以下条件的所有 Data Store Write 模块的路径：与此模块具有相同的数据存储名称，且在模型层次结构中位于同一（子）系统级别或其下的任何子系统中。点击此列表中的任何条目以在模型中突出显示对应的模块。

### **采样时间** — 采样时间  
-1 （默认） | 标量 | 向量

采样时间，控制模块何时从数据存储中读取数据。值为 `-1` 表示继承采样时间。有关详细信息，请参阅 [指定采样时间](https://ww2.mathworks.cn/help/simulink/ug/how-to-specify-the-sample-time.html)。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><span><strong>模块参数</strong>：<code>SampleTime</code></span></td></tr><tr><td><span><strong>类型</strong>：字符向量</span></td></tr><tr><td><span><strong>值</strong>：标量 | 向量</span></td></tr><tr><td><span><strong>默认值</strong>：<code>'-1'</code></span></td></tr></tbody></table>

### 元素选择

### **数组中的元素 (总线中的信号)** — 关联的数据存储中的元素  
字符向量（无默认值）

关联的数据存储中元素的列表。对于包含数组的数据存储，您可以读取整个数据存储，也可以指定数据存储的一个或多个元素。对于总线数据类型的数据存储，您可以展开树视图以查看并选择总线元素。此列表在括号中显示每个元素的最大维度。

如果未选择**启用索引**，请选择一个元素并使用以下方法之一：

*   点击**选择 >>** 在**所选元素**列表中显示该元素及其所有子元素。
    
*   使用**指定要选择的元素**编辑框指定您要选择进行读取的子元素。然后点击**选择 >>**。
    

要选择多个元素，请对每个元素重复上述过程。

您也可以选择**启用索引**，然后选择单个元素，并使用**索引选项**参数动态指定子元素。

要刷新显示并反映对数据存储中使用的数组或总线的修改，请点击**刷新**。

#### 依存关系

对此部分（**数组中的元素**或**总线中的信号**）的提示取决于数据存储中数据的类型。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><span><strong>模块参数</strong>：<code>DataStoreElements</code></span></td></tr><tr><td><span><strong>类型</strong>：字符向量</span></td></tr><tr><td><span><strong>值</strong>：以井号分隔的元素列表（请参阅<a href="https://ww2.mathworks.cn/help/simulink/ug/model-global-data-using-data-stores.html#bsvla4x">使用命令行指定</a>。）</span></td></tr><tr><td><span><strong>默认值</strong>：<code>''</code></span></td></tr></tbody></table>

### **指定要选择的元素** — 定义要选择的元素的 MATLAB 表达式  
字符向量（无默认值）

输入 MATLAB 表达式以定义要读取的特定元素，然后点击**选择 >>** 将该元素添加到**所选元素**表中。重复以上操作以选择其他元素。

例如，对于名为 `DSM`、最大维度为 `[3,5]` 的数据存储，您可以在编辑框中输入表达式，如 `DSM(2,4)` 或 `DSM([1 3],2)`。请参阅[访问特定的总线和矩阵元素](https://ww2.mathworks.cn/help/simulink/ug/model-global-data-using-data-stores.html#bsvjg1i)。

要应用元素选择，请点击**确定**或**应用**。

#### 依存关系

仅当未选择**启用索引**时，才会出现**指定要选择的元素**编辑框。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><span><strong>模块参数</strong>：<code>DataStoreElements</code></span></td></tr><tr><td><span><strong>类型</strong>：字符向量</span></td></tr><tr><td><span><strong>值</strong>：以井号分隔的元素列表（请参阅<a href="https://ww2.mathworks.cn/help/simulink/ug/model-global-data-using-data-stores.html#bsvla4x">使用命令行指定</a>。）</span></td></tr><tr><td><span><strong>默认值</strong>：<code>''</code></span></td></tr></tbody></table>

### **所选元素** — 选定元素的列表  
字符向量（无默认值）

您从数据存储中选择的元素。Data Store Read 模块图标为您指定的每个元素显示一个输出端口。

要更改列表中的总线或矩阵元素的顺序，请在列表中选择元素，然后点击**向上**或**向下**。更改列表中的元素顺序会同时更改端口的顺序。要移除某个元素，请点击**删除**。

#### 依存关系

仅当未选择**启用索引**时，才会出现**所选元素**表。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><span><strong>模块参数</strong>：<code>DataStoreElements</code></span></td></tr><tr><td><span><strong>类型</strong>：字符向量</span></td></tr><tr><td><span><strong>值</strong>：以井号分隔的元素列表（请参阅<a href="https://ww2.mathworks.cn/help/simulink/ug/model-global-data-using-data-stores.html#bsvla4x">使用命令行指定</a>。）</span></td></tr><tr><td><span><strong>默认值</strong>：<code>''</code></span></td></tr></tbody></table>

### **启用索引** — 启用索引以指定要读取的数据存储元素的子元素  
'off' （默认） | 'on'

选中此参数可启用类似于 [Selector](https://ww2.mathworks.cn/help/simulink/slref/selector.html) 模块所使用的索引，由此您可以使用一个或多个索引输入端口动态指定要读取的子元素的索引，以及使用模块对话框指定索引。选中此参数时，Data Store Read 模块只能从数据存储的单个元素（即总线中的单个信号）中进行读取。要使用动态索引从数据存储的多个元素中进行读取，请使用多个 Data Store Read 模块。

清除此参数可禁用 Selector 模块样式索引。您可以选择多个要读取的数据存储元素，但只能使用模块对话框来指定要读取哪些子元素。

**注意**

如果关联的数据存储只包含一个标量元素，请不要选择**启用索引**。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><span><strong>模块参数</strong>：<code>EnableIndexing</code></span></td></tr><tr><td><span><strong>类型</strong>：字符向量</span></td></tr><tr><td><span><strong>值</strong>：<code>'off'</code> | <code>'on'</code></span></td></tr><tr><td><span><strong>默认值</strong>：<code>'off'</code></span></td></tr></tbody></table>

### **维数** — 数据存储元素的维数  
1 （默认） | 正整数

所选数据存储元素的维数。您必须显式指示此数目。

#### 依存关系

仅当选中**启用索引**时，此参数才会启用。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><span><strong>模块参数</strong>：<code>NumberOfDimensions</code></span></td></tr><tr><td><span><strong>类型</strong>：字符向量</span></td></tr><tr><td><span><strong>值</strong>：<code>positive integer</code></span></td></tr><tr><td><span><strong>默认值</strong>：<code>'1'</code></span></td></tr></tbody></table>

### **索引模式** — 索引模式  
“`从 1 开始`” （默认） | “`从 0 开始`”

选择索引模式。如果选择 “`从 1 开始`”，则索引 1 指定输入向量的第一个元素。如果选择 “`从 0 开始`”，则索引 0 指定输入向量的第一个元素。

#### 依存关系

仅当选中**启用索引**时，此参数才会启用。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><span><strong>参数：</strong><code>IndexMode</code></span></td></tr><tr><td><span><strong>类型：</strong>字符向量</span></td></tr><tr><td><span><strong>值：</strong><code>'Zero-based'</code> | <code>'One-based'</code></span></td></tr><tr><td><span><strong>默认值：</strong><code>'One-based'</code></span></td></tr></tbody></table>

### **索引选项** — 子元素的索引方法  
“`索引向量(对话框)`” （默认） | “`全选`” | “`索引向量(端口)`” | “`起始索引(对话框)`” | “`起始索引(端口)`”

按维度定义所选数据存储元素的子元素的索引方式。从列表中，选择：

<table><colgroup><col width="50%"><col width="50%"></colgroup><thead><tr><th>菜单项</th><th>操作</th></tr></thead><tbody><tr><td>“<code>全选</code>”</td><td><p>读取所有子元素。</p></td></tr><tr><td>“<code>索引向量(对话框)</code>”</td><td><p>启用<strong>索引</strong>列。输入包含要读取的子元素索引的向量。</p></td></tr><tr><td>“<code>索引向量(端口)</code>”</td><td><p>相关索引端口定义要读取的子元素的索引。</p></td></tr><tr><td>“<code>起始索引(对话框)</code>”</td><td><p>启用<strong>索引</strong>和<strong>输出大小</strong>列。输入要读取的子元素范围的起始索引和大小。</p></td></tr><tr><td>“<code>起始索引(端口)</code>”</td><td><p>启用<strong>输出大小</strong>列。索引端口定义要读取的元素范围的起始索引。输入该范围的大小。</p></td></tr></tbody></table>

**索引**和**输出大小**列显示为具有相关性。

#### 依存关系

仅当选中**启用索引**时，此参数才会启用。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><span><strong>参数：</strong><code>IndexOptionArray</code></span></td></tr><tr><td><span><strong>类型：</strong>字符向量</span></td></tr><tr><td><span><strong>值：</strong> <code>'Select all'</code> | <code>'Index vector (dialog)'</code> | <code>'Index vector (port)'</code> | <code>'Starting index (dialog)'</code> | <code>'Starting index (port)'</code></span></td></tr><tr><td><span><strong>默认值：</strong><code>'Index vector (dialog)'</code></span></td></tr></tbody></table>

### **索引** — 子元素的索引或起始索引  
`1` （默认） | 整数 | 整数向量

如果**索引选项**是 “`索引向量(对话框)`”，请输入包含要读取的每个子元素的索引的向量。

如果**索引选项**设置为 “`起始索引(对话框)`”，则输入要读取的子元素范围的起始索引。

#### 依存关系

仅当选择**启用索引**并且维度的**索引选项**为 “`索引向量(对话框)`” 或 “`起始索引(对话框)`” 时，此参数才启用。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><span><strong>参数：</strong><code>IndexParamArray</code></span></td></tr><tr><td><span><strong>类型：</strong>字符向量</span></td></tr><tr><td><span><strong>值：</strong>元胞数组</span></td></tr><tr><td><span><strong>默认值：</strong><code>'{ }'</code></span></td></tr></tbody></table>

### **输出大小** — 要读取的子元素范围的大小  
`1` （默认） | 整数

如果**索引选项**是 “`起始索引(对话框)`” 或 “`起始索引(端口)`”，请输入要读取的子元素范围的大小。

#### 依存关系

仅当选择**启用索引**并且维度的**索引选项**为 “`起始索引(对话框)`” 或 “`起始索引(端口)`” 时，此参数才启用。

#### 编程用法

<table summary="Simple list"><tbody><tr><td><span><strong>模块参数</strong>：<code>OutputSizeArray</code></span></td></tr><tr><td><span><strong>类型：</strong>字符向量</span></td></tr><tr><td><span><strong>值：</strong>元胞数组</span></td></tr><tr><td><span><strong>默认值：</strong><code>'{ }'</code></span></td></tr></tbody></table>

## 模块特性

<table><colgroup><col width="28.70%"><col width="71.30%"></colgroup><tbody><tr><td><p><strong>数据类型</strong></p></td><td><p><code>Boolean</code> | <code>bus</code> | <code>double</code> | <code>enumerated</code> | <code>fixed point</code> | <code>half</code> | <code>integer</code> | <code>single</code> | <code>string</code></p></td></tr><tr><td><p><strong>直接馈通</strong></p></td><td><p><code>否</code></p></td></tr><tr><td><p><strong>多维信号</strong></p></td><td><p><code>是</code></p></td></tr><tr><td><p><strong>可变大小信号</strong></p></td><td><p><code>否</code></p></td></tr><tr><td><p><strong>过零检测</strong></p></td><td><p><code>否</code></p></td></tr></tbody></table>

## 扩展功能

### C/C++ 代码生成  
使用 Simulink® Coder™ 生成 C 代码和 C++ 代码。

### 定点转换  
使用 Fixed-Point Designer™ 设计和仿真定点系统。

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