---
url: https://ww2.mathworks.cn/help/coder/ug/structure-of-example-cc-main-function.html
title: 生成的示例 C/C++ 主函数的结构
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-02 10:35:14
tag: 
summary: 检查生成的示例主函数的各部分，以便针对您的应用程序修改示例主函数。
---
## 生成的示例 C/C++ 主函数的结构

当您编译使用生成的 C/C++ 代码的应用程序时，必须提供调用生成的代码的 C/C++ 主函数。

默认情况下，对于 C/C++ 源代码、静态库、动态库和可执行文件的代码生成，MATLAB® Coder™ 会生成示例 C/C++ 主函数。此函数是可以帮助您将生成的 C/C++ 代码合并到应用程序中的模板。示例主函数声明和初始化数据，包括动态分配的数据。它会调用入口函数，但不使用入口函数返回的值。要使用示例主函数，请将示例主函数源文件和头文件复制到编译文件夹以外的某个位置，然后在新位置修改文件以满足您的应用程序的要求。

MATLAB Coder 在编译文件夹的 `examples` 子文件夹中生成示例主函数的源文件和头文件。对于 C 代码生成，它会生成 `main.c` 和 `main.h` 文件。对于 C++ 代码生成，它会生成 `main.cpp` 和 `main.h` 文件。

### 文件 `main.c` 或 `main.cpp` 的内容

对于示例主函数源文件 `main.c` 或 `main.cpp`，MATLAB Coder 会生成以下各节内容：

* [Include 文件](#buqzhus)
* [函数声明](#buqzhuw)
* [参数初始化函数](#buqzhuy)
* [入口函数](#buqzhu0)
* [主函数](#buqzhu3)

默认情况下，MATLAB Coder 还会在示例主函数源文件中生成注释，这些注释可帮助您修改示例主函数以在您的应用程序中使用。

#### Include 文件

本节包含调用不在示例主函数源文件中的代码所需的头文件。如果在修改示例主函数源文件时调用外部函数，请包含任何其他必需的头文件。

#### 函数声明

本节声明在示例主函数源文件中定义的参数初始化和入口函数的函数原型。修改函数原型以匹配您在函数定义中所做的修改。为在示例主函数源文件中定义的函数声明新的函数原型。

#### 参数初始化函数

本节为入口函数用作参数的每种数据类型定义初始化函数。参数初始化函数将参数的大小初始化为默认值，将数据的值初始化为零。然后，该函数返回经过初始化的数据。请更改这些大小和数据值以满足您的应用程序的要求。

对于维度大小为 `<dimSizes>` 且使用 MATLAB C/C++ 数据类型 `<baseType>` 的参数，示例主函数源文件定义名称为 `argInit_<dimSizes>_<baseType>` 的初始化函数。例如，对于使用 MATLAB 双精度类型的数据的 5×5 数组，示例主函数源文件将定义参数初始化函数 `argInit_5x5_real_T`。

MATLAB Coder 会更改参数初始化函数的名称，如下所示：

* 如果有任何维度的大小是可变的，则 MATLAB Coder 会将这些维度的大小指定为 `d<maxSize>`，其中 `<maxSize>` 是该维度的最大大小。例如，对于具有 MATLAB 双精度类型数据的数组，如果其第一个维度的静态大小为 2，第二个维度为可变大小且最大为 10，则示例主函数源文件将定义参数初始化函数 `argInit_2xd10_real_T`。
* 如果有无界维度，则 MATLAB Coder 会将这些维度的大小指定为 `Unbounded`。
* 如果初始化函数的返回类型为 `emxArray`，则 MATLAB Coder 会将函数定义为返回指向 `emxArray` 的指针。
* 如果初始化函数名称的长度超过了在配置设置中为函数名称设置的最大字符数，则 MATLAB Coder 会在函数名称的前面添加一个标识符。然后，MATLAB Coder 会将函数名称截断为标识符长度允许的最大字符数。

  **注意**

  默认情况下，对于生成的标识符，允许的最大字符数为 31。要使用 MATLAB Coder App 将该值指定为设置的最大标识符长度，请在代码生成设置的 **Code Appearance** 选项卡上选择 **Maximum identifier length** 值。要使用命令行界面将该值指定为设置的最大标识符长度，请更改 `MaxIdLength` 配置对象设置的值。

#### 入口函数

本节为每个 MATLAB 入口函数定义函数。对于 MATLAB 函数 `foo.m`，示例主函数源文件将定义入口函数 `main_foo`。此函数会创建变量并调用 C/C++ 源函数 `foo.c` 或 `foo.cpp` 所需的数据初始化函数。它调用此 C/C++ 源函数，但不返回结果。修改 `main_foo`，使其能根据您的应用程序的需要接受输入和返回输出。

#### 主函数

本节定义执行以下操作的 `main` 函数：

* 如果您的输出语言为 C，则它将声明并命名变量 `argc` 和 `argv`，但会将其转换为 void 类型。如果您的输出语言为 C++，则生成的示例主函数将声明变量 `argc` 和 `argv`，但不会命名这些变量。
* 对每个入口函数执行一次调用。
* 调用终止函数 `foo_terminate`，该函数是针对为代码生成而声明的第一个 MATLAB 入口函数 `foo` 命名的。即使在函数 `main` 中调用多个入口函数，也只需调用一次终止函数。
* 返回零。

默认情况下，示例 `main` 函数不调用初始化函数 `foo_initialize`。代码生成器会在生成的 C/C++ 入口函数开始处包含一个对初始化函数的调用。生成的代码中还包括一些检查，以确保初始化函数只被自动调用一次，即使有多个入口函数也是如此。

您可以选择在生成的入口函数中不包含对初始化函数的调用。要实现此目的，请执行以下操作之一：

如果您照此操作，示例 `main` 函数将会包括对初始化函数 `foo_initialize` 的调用。

请参阅 [Use Generated Initialize and Terminate Functions](https://ww2.mathworks.cn/help/coder/ug/use-generated-initialize-and-terminate-functions.html)。

修改函数 `main`，包括 `main` 和入口函数的输入与输出，以满足您的应用程序的要求。

### 文件 `main.h` 的内容

对于示例主函数头文件 `main.h`，MATLAB Coder 会生成以下内容：

* [Include Guard](#buq0974)
* [Include 文件](#buq098d)
* [函数声明](#buq098g)

默认情况下，MATLAB Coder 还会在 `main.h` 中生成注释，这些注释可帮助您修改示例主函数以在您的应用程序中使用。

#### Include Guard

`main.h` 使用 include guard 来防止文件的内容被多次包含。include guard 包含 `#ifndef` 构造内的 include 文件和函数声明。

#### Include 文件

`main.h` 包含调用未在本函数内定义的代码所需的头文件。

#### 函数声明

`main.h` 声明在示例主函数源文件 `main.c` 或 `main.cpp` 中定义的主函数的函数原型。

## 相关示例

* [使用示例主函数合并生成的代码](https://ww2.mathworks.cn/help/coder/ug/generate-an-example-cc-main-function.html)
* [在应用程序中使用示例 C 主函数](https://ww2.mathworks.cn/help/coder/ug/generate-and-modify-an-example-cc-main-function.html)

## 详细信息

* [Mapping MATLAB Types to Types in Generated Code](https://ww2.mathworks.cn/help/coder/ug/mapping-matlab-types-to-cc-types.html)
* [Use Generated Initialize and Terminate Functions](https://ww2.mathworks.cn/help/coder/ug/use-generated-initialize-and-terminate-functions.html)

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
