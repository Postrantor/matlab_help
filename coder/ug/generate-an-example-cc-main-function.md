---
url: https://ww2.mathworks.cn/help/coder/ug/generate-an-example-cc-main-function.html
title: 使用示例主函数合并生成的代码
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-02 10:37:59
tag: 
summary: MATLAB Coder 会生成一个示例主函数，帮助您将生成的 C 代码合并到您的应用程序中。
---
## 使用示例主函数合并生成的代码

当您编译使用生成的 C/C++ 代码的应用程序时，必须提供调用生成的代码的 C/C++ 主函数。

默认情况下，对于 C/C++ 源代码、静态库、动态库和可执行文件的代码生成，MATLAB® Coder™ 会生成示例 C/C++ 主函数。此函数是可以帮助您将生成的 C/C++ 代码合并到应用程序中的模板。示例主函数声明和初始化数据，包括动态分配的数据。它会调用入口函数，但不使用入口函数返回的值。

MATLAB Coder 在编译文件夹的 `examples` 子文件夹中生成示例主函数的源文件和头文件。对于 C 代码生成，它会生成 `main.c` 和 `main.h` 文件。对于 C++ 代码生成，它会生成 `main.cpp` 和 `main.h` 文件。

不要修改 `examples` 子文件夹中的 `main.c` 和 `main.h` 文件。如果修改这些文件，则当您重新生成代码时，MATLAB Coder 不会重新生成示例主文件。它会发出警告，提示检测到生成文件发生了更改。在使用示例主函数之前，将示例主函数源文件和头文件复制到编译文件夹以外的某个位置。修改新位置的文件以满足您的应用程序的要求。

当您使用默认配置设置生成文件时，MATLAB Coder App 的 `packNGo` 函数和 **Package** 选项不会打包示例主函数源文件和头文件。要打包示例主文件，请配置代码生成以生成和编译示例主函数，生成代码，然后打包编译文件。

### 使用示例主函数的工作流

1. 准备您的 MATLAB 代码以进行代码生成。
2. 检查是否存在运行时问题。
3. 确保启用了生成示例主函数的功能。
4. 生成入口函数的 C/C++ 代码。
5. 将示例主文件从 `examples` 子文件夹复制到另一个文件夹。
6. 修改新文件夹中的示例主文件以满足应用程序的要求。
7. 针对所需的平台部署示例主函数和生成的代码。
8. 编译应用程序。

有关如何生成示例主函数并使用它编译可执行文件的示例，请参阅[在应用程序中使用示例 C 主函数](https://ww2.mathworks.cn/help/coder/ug/generate-and-modify-an-example-cc-main-function.html)。

### 使用 MATLAB Coder App 控制示例主函数生成

1. 在 **Generate Code** 页上，要打开 **Generate** 对话框，请点击 **Generate** 箭头

   ![](https://ww2.mathworks.cn/help/coder/ug/ui_button_arrow.png)

   。
2. 在 **Generate** 对话框中，将 **Build Type** 设置为以下项之一：

   * “`Source Code`”
   * “`Static Library`”
   * “`Dynamic Library`”
   * “`Executable`”
3. 点击 **More Settings**。
4. 在 **All Settings** 选项卡上的 **Advanced** 下，将 **Generate example main** 设置为以下项之一：

   <table><colgroup><col width="50%"><col width="50%"></colgroup><thead><tr><th>设置为</th><th>目的</th></tr></thead><tbody><tr><td>“<code>Do not generate an example main function</code>”</td><td>不生成示例 C/C++ 主函数</td></tr><tr><td>“<code>Generate, but do not compile, an example main function</code>”（默认值）</td><td>生成示例 C/C++ 主函数但不进行编译</td></tr><tr><td>“<code>Generate and compile an example main function</code>”</td><td>生成示例 C/C++ 主函数并进行编译</td></tr></tbody></table>

### 使用命令行界面控制示例主函数生成

1. 为 `'lib'`、`'dll'` 或 `'exe'` 创建代码配置对象。例如：

   ```
   cfg = coder.config('lib'); % or dll or exe


   ```
2. 设置 `GenerateExampleMain` 属性。

   <table><colgroup><col width="50%"><col width="50%"></colgroup><thead><tr><th>设置为</th><th>目的</th></tr></thead><tbody><tr><td><code>'DoNotGenerate'</code></td><td>不生成示例 C/C++ 主函数</td></tr><tr><td><code>'GenerateCodeOnly'</code>（默认值）</td><td>生成示例 C/C++ 主函数但不进行编译</td></tr><tr><td><code>'GenerateCodeAndCompile'</code></td><td>生成示例 C/C++ 主函数并进行编译</td></tr></tbody></table>

   例如：

   ```
   cfg.GenerateExampleMain = 'GenerateCodeOnly';


   ```

## 相关主题

* [生成的示例 C/C++ 主函数的结构](https://ww2.mathworks.cn/help/coder/ug/structure-of-example-cc-main-function.html)
* [指定 C/C++ 可执行文件的主函数](https://ww2.mathworks.cn/help/coder/ug/standalone-c-c-executables-from-matlab-code.html#bsx_bv5)

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
