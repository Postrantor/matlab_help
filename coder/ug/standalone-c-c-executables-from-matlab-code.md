---
url: https://ww2.mathworks.cn/help/coder/ug/standalone-c-c-executables-from-matlab-code.html#bsx_bv5
title: 从 MATLAB 代码生成独立的 C/C++ 可执行文件
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-02 10:34:36
tag: 
summary: 在命令行或 MATLAB Coder App 中生成 C/C++ 可执行文件。
---
### 使用 MATLAB Coder App 生成 C 可执行文件

此示例说明如何使用 MATLAB® Coder™ App 从 MATLAB 代码生成 C 可执行文件。在此示例中，您为 MATLAB 函数生成一个可执行文件，该函数用于生成随机标量值。使用该 App 可以执行以下操作：

1. 生成示例 C `main` 函数，该函数调用生成的库函数。
2. 复制并修改生成的 `main.c` 和 `main.h`。
3. 修改工程设置，以便 App 可以找到修改后的 `main.c` 和 `main.h`。
4. 生成可执行文件。

**创建入口函数**

在本地可写文件夹中，创建 MATLAB 函数 `coderand`，该函数在开区间 (0,1) 上基于标准均匀分布生成一个随机标量值：

```
function r = coderand() %#codegen
r = rand();

```

**创建测试文件**

在同一本地可写文件夹中，创建 MATLAB 文件 `coderand_test.m`，它调用 `coderand`。

```
function y = coderand_test()
y = coderand();


```

**打开 MATLAB Coder App**

在 MATLAB 工具条的 **App** 选项卡上，点击**代码生成**下的 MATLAB Coder App 图标。

该 App 会打开**选择源文件**页面。

**指定源文件**

1. 在**选择源文件**页面中，键入或选择入口函数 `coderand` 的名称。

   该 App 将使用默认名称 `coderand.prj` 在当前文件夹中创建一个工程。
2. 点击**下一步**以转到**定义输入类型**步骤。该 App 将分析函数以查找编码问题并确定代码生成就绪情况。如果 App 发现问题，它将打开**检查代码就绪性**页面，您可以在其中查看和解决问题。在此示例中，由于 App 没有检测到问题，因此将打开**定义输入类型**页面。

**定义输入类型**

由于 C 使用静态定型，MATLAB Coder 必须在编译时确定 MATLAB 文件中所有变量的属性。您必须指定所有入口函数输入的属性。根据入口函数输入的属性，MATLAB Coder 可以推断 MATLAB 文件中所有变量的属性。

在此示例中，函数 `coderand` 没有输入。

点击**下一步**以转到**检查运行时问题**步骤。

**检查运行时问题**

**检查运行时问题**步骤从您的入口函数生成 MEX 文件，然后运行 MEX 函数并报告问题。此步骤是可选的。不过，建议最好执行此步骤。您可以检测并解决在生成的 C 代码中更难诊断出来的运行时错误。

1. 要打开**检查运行时问题**对话框，请点击**检查问题**箭头

   ![](https://ww2.mathworks.cn/help/coder/ug/ui_button_arrow.png)

   。

   选择或输入测试文件 `coderand_test`。
2. 点击**检查问题**。

   App 将为 `coderand` 生成一个 MEX 函数。它运行测试文件，将对 `coderand` 的调用替换为对 MEX 函数的调用。如果 App 在 MEX 函数生成或执行过程中检测到问题，它将提供警告和错误消息。您可以点击这些消息，导航到有问题的代码并修复问题。在本示例中，App 未检测到问题。
3. 点击**下一步**以转到**生成代码**步骤。

**生成 C `main` 函数**

在生成可执行文件时，您必须提供一个 C/C++ 主函数。默认情况下，当您生成 C/C++ 源代码、静态库、动态链接库或可执行文件时，MATLAB Coder 会生成 `main` 函数。这一生成的主函数是一个模板，您可以针对您的应用程序修改该模板。请参阅[使用示例主函数合并生成的代码](https://ww2.mathworks.cn/help/coder/ug/generate-an-example-cc-main-function.html)。在复制和修改生成的主函数后，可以使用它来生成 C/C++ 可执行文件。您也可以编写自己的主函数。

在为 `coderand` 生成可执行文件之前，请生成 `main` 函数，该函数调用 `coderand`。

1. 要打开**生成**对话框，请点击**生成** 箭头

   ![](https://ww2.mathworks.cn/help/coder/ug/ui_button_arrow.png)

   。
2. 在**生成**对话框中，将**编译类型**设置为 “`源代码`”，将**语言**设置为 C。对于其他工程编译配置设置，请使用默认值。
3. 点击**更多设置**。
4. 在**所有设置**选项卡的**高级**下，确认**生成示例主函数**设置为 “`生成但不编译示例主函数`”。点击**关闭**。
5. 点击**生成**。

   MATLAB Coder 生成一个 `main.c` 文件和一个 `main.h` 文件。App 指示代码生成成功。
6. 点击**下一步**打开**完成工作流**页面。

   在**完成工作流**页面的**生成的输出**下，您会看到 `main.c` 位于子文件夹 `coderand\codegen\lib\coderand\examples` 中。

**复制生成的示例主文件**

由于后续代码生成可能覆盖生成的示例文件，因此在修改这些文件之前，请将它们复制到 `codegen` 文件夹之外的一个可写文件夹中。对于此示例，请将 `main.c` 和 `main.h` 从子文件夹 `coderand\codegen\lib\coderand\examples` 复制到一个可写文件夹中，例如 `c:\myfiles`。

**修改生成的示例主文件**

1. 在包含示例主文件副本的文件夹中，打开 `main.c`。

   [![](https://ww2.mathworks.cn/help/includes/product/images/doc_center/arrow_right.gif)](#)

    [生成的 main.c](#)
2. 修改 `main.c`，使其打印 `coderand` 调用的结果：

   * 在 `main_coderand` 中，删除以下行
   * 在 `main_coderand` 中，将

     替换为：

     ```
     printf("coderand=%g\n", coderand());

     ```
   * 对于此示例，`main` 没有参数。在 `main` 中，删除以下行：

     将 `main` 的定义更改为

   [![](https://ww2.mathworks.cn/help/includes/product/images/doc_center/arrow_right.gif)](#)

    [修改后的 main.c](#)
3. 打开 `main.h`。

   [![](https://ww2.mathworks.cn/help/includes/product/images/doc_center/arrow_right.gif)](#)

    [生成的 main.h](#)
4. 修改 `main.h`：

   * 将 `stdio` 添加到包含文件中：
   * 将 main 的声明更改为

[![](https://ww2.mathworks.cn/help/includes/product/images/doc_center/arrow_right.gif)](#)

 [修改后的 main.h](#)

**生成可执行文件**

在修改生成的示例主文件后，打开之前创建的工程文件或从 App 中选择 `coderand.m`。您可以选择覆盖该工程，或以不同名称命名工程以同时保存这两个工程文件。

1. 要打开**生成代码**页面，请展开工作流步骤

   ![](https://ww2.mathworks.cn/help/coder/ug/app_workflow_steps_icon.png)

   ，然后点击**生成**
2. 要打开**生成**对话框，请点击**生成** 箭头

   ![](https://ww2.mathworks.cn/help/coder/ug/ui_button_arrow.png)

   。
3. 将**编译类型**设置为 “`可执行文件(.exe)`”。
4. 点击**更多设置**。
5. 在**自定义代码**选项卡的**其他源文件**中，输入 `main.c`
6. 在**自定义代码**选项卡的**其他包括目录**中，输入修改后的 `main.c` 和 `main.h` 文件的位置。例如，`c:\myfiles`。点击**关闭**。
7. 要生成可执行文件，请点击**生成**。

   App 指示代码生成成功。
8. 点击**下一步**以转至**完成工作流**步骤。
9. 在**生成的输出**下，您可以看到生成的可执行文件 `coderand.exe` 的位置。

**运行可执行文件**

要在 Windows® 平台上的 MATLAB 中运行可执行文件，请执行以下命令：

**注意**

此示例中可执行的 `coderand` 函数返回介于 0 到 1 之间的固定值。要生成每次执行都会产生新值的代码，请在 MATLAB 代码中使用 [`RandStream`](https://ww2.mathworks.cn/help/matlab/ref/randstream.html) 函数。

### 在命令行中生成 C 可执行文件

在此示例中，您将创建一个生成随机标量值的 MATLAB 函数和一个调用此 MATLAB 函数的 C 主函数。然后指定函数输入参数的类型，指定主函数，并为 MATLAB 代码生成 C 可执行文件。

1. 编写一个 MATLAB 函数 `coderand`，该函数在开区间 (0,1) 上基于标准均匀分布生成一个随机标量值：

   ```
   function r = coderand() %#codegen
   r = rand();

   ```
2. 编写一个 C 主函数 `c:\myfiles\main.c`，它调用 `coderand`。例如：

   ```
   /*
   ** main.c
   */
   #include <stdio.h>
   #include <stdlib.h>
   #include "coderand.h"
   #include "coderand_terminate.h"

   int main()
   {
       /* The initialize function is called automatically from the generated entry-point function. 
          So, a call to initialize is not included here. */ 

       printf("coderand=%g\n", coderand());

       coderand_terminate();

       return 0;
   }

   ```

   **注意**

   在此示例中，由于默认文件分区方法是为每个 MATLAB 文件生成一个文件，因此您要包含 `"coderand_terminate.h"`。如果您的文件分区方法设置为对所有函数只生成一个文件，请**不要**包含 `"coderand_terminate.h"`。
3. 将代码生成参数配置为包含 C 主函数，然后生成 C 可执行文件：

   ```
   cfg = coder.config('exe');
   cfg.CustomSource = 'main.c';
   cfg.CustomInclude = 'c:\myfiles';
   codegen -config cfg coderand 

   ```

   `codegen` 在当前文件夹中生成 C 可执行文件 `coderand.exe`。它在默认文件夹 `codegen/exe/coderand` 中生成支持文件。`codegen` 为所选代码替换库所需的头文件生成最小的 `#include` 语句集。

### 指定 C/C++ 可执行文件的主函数

生成可执行文件时，您必须提供 `main` 函数。对于 C 可执行文件，请提供 C 文件 `main.c`。对于 C++ 可执行文件，请提供 C++ 文件 `main.cpp`。确认包含主函数的文件夹中只有一个主文件。否则，`main.c` 将优先于 `main.cpp`，这会在生成 C++ 代码时导致错误。您可以从工程设置对话框、命令行或 Code Generation 对话框中指定主文件。

默认情况下，当您生成 C/C++ 源代码、静态库、动态链接库或可执行文件时，MATLAB Coder 会生成 `main` 函数。这一生成的主函数是一个模板，您可以针对您的应用程序修改该模板。请参阅[使用示例主函数合并生成的代码](https://ww2.mathworks.cn/help/coder/ug/generate-an-example-cc-main-function.html)。在复制和修改生成的主函数后，可以使用它来生成 C/C++ 可执行文件。您也可以编写自己的主函数。

将 MATLAB 函数转换为 C/C++ 库函数或 C/C++ 可执行文件时，MATLAB Coder 会生成一个初始化函数和一个终止函数。

### 指定主函数

#### 使用 MATLAB Coder App 指定主函数

1. 要打开**生成**对话框，请在**生成代码**页上点击**生成**箭头

   ![](https://ww2.mathworks.cn/help/coder/ug/ui_button_arrow.png)

   。
2. 点击**更多设置**。
3. 在**自定义代码**选项卡上，进行如下设置：

   1. 将**其他源文件**设置为包含 `main` 函数的 C/C++ 源文件的名称。例如，`main.c`。有关详细信息，请参阅[指定 C/C++ 可执行文件的主函数](https://ww2.mathworks.cn/help/coder/ug/standalone-c-c-executables-from-matlab-code.html#bsx_bv5)。
   2. 将**其他包括目录**设置为 `main.c` 的位置。例如，`c:\myfiles`。

#### 在命令行中指定主函数

设置代码生成配置对象的 `CustomSource` 和 `CustomInclude` 属性（请参阅[使用配置对象](https://ww2.mathworks.cn/help/coder/ug/build-setting-configuration.html#braq9w_-1)）。`CustomInclude` 属性指示由 `CustomSource` 指定的 C/C++ 文件的位置。

1. 为可执行文件创建配置对象：

   ```
   cfg = coder.config('exe');


   ```
2. 将 `CustomSource` 属性设置为包含 `main` 函数的 C/C++ 源文件的名称。（有关详细信息，请参阅[指定 C/C++ 可执行文件的主函数](https://ww2.mathworks.cn/help/coder/ug/standalone-c-c-executables-from-matlab-code.html#bsx_bv5)。）例如：

   ```
   cfg.CustomSource = 'main.c';

   ```
3. 将 `CustomInclude` 属性设置为 `main.c` 的位置。例如：

   ```
   cfg.CustomInclude = 'c:\myfiles';

   ```
4. 使用命令行选项生成 C/C++ 可执行文件。例如，如果 `myFunction` 接受 `double` 类型的一个输入参数：

   ```
   codegen -config cfg  myMFunction -args {0}

   ```

   MATLAB Coder 将对主函数进行编译并将它与从 `myMFunction.m` 生成的 C/C++ 代码进行链接。

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
