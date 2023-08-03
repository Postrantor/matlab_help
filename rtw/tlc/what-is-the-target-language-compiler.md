---
tip: translate by openai@2023-08-02 13:58:34
url: https://www.mathworks.com/help/rtw/tlc/what-is-the-target-language-compiler.html
title: Target Language Compiler Basics
- MATLAB & Simulink
date: 2023-08-02 13:49:18
tag: 
summary: Use the Target Language Compiler to produce platform-specific code and incorporate your own algorithm......
> 使用目标语言编译器来生成平台特定的代码，并将自己的算法纳入其中......
---

## Target Language Compiler Basics

### Target Language Compiler Overview

Target Language Compiler (TLC) is an integral part of the code generator. It enables you to customize generated code. Through customization, you can produce platform-specific code, or you can incorporate your own algorithmic changes for performance, code size, or compatibility with existing methods.

> 目标语言编译器（TLC）是代码生成器的重要组成部分。它可以让您自定义生成的代码。通过定制，您可以生成特定平台的代码，或者可以结合自己的算法变化来提高性能、代码大小或与现有方法的兼容性。

The TLC includes:

- Files corresponding to a subset of the provided Simulink® blocks.
- Files for model-wide information that specify header and parameter information.

> 文件用于模型范围内的信息，指定标题和参数信息。

The TLC files are ASCII files that explicitly control the way that code is generated. By editing a TLC file, you can alter the way that the code is generated.

> TLC 文件是 ASCII 文件，可以明确控制代码生成的方式。通过编辑 TLC 文件，您可以改变代码生成的方式。

The Target Language Compiler provides a complete set of ready-to-use TLC files for generating ANSI® C or C++ code. You can view the TLC files and make minor or extensive changes to them. This open environment provides tremendous flexibility for customizing the generated code.

> **编译器目标语言提供了一套完整的可直接使用的 TLC 文件，用于生成 ANSI® C 或 C++代码**。您可以查看 TLC 文件，并对其进行小幅度或大幅度的更改。这种开放的环境提供了定制生成代码的巨大灵活性。

For more information, see [Implement C/C++ S-Functions](https://www.mathworks.com/help/simulink/c-c-s-functions.html), which describes how to write wrapped and fully inlined S-functions, with emphasis on the `mdlRTW()` function.

> 更多信息，请参阅[实现 C/C++ S-函数]，其中描述了如何编写封装和完全内联的 S-函数，重点介绍`mdlRTW()`函数。

**Note**

Do not customize TLC files in the folder `` _`matlabroot`_/rtw/c/tlc ``, even though the capability exists. Such TLC customizations might not be applied during the code generation process and can lead to unpredictable results.

> 不要在文件夹`` _`matlabroot`_/rtw/c/tlc ``中自定义 TLC 文件，即使有这种能力。这些 TLC 自定义可能不会在代码生成过程中应用，可能会导致不可预测的结果。

### Overview of the TLC Process

This top-level diagram shows how the Target Language Compiler fits in with the code generation process.

> 这个顶级图表展示了目标语言编译器如何与代码生成过程相结合。

![](https://www.mathworks.com/help/rtw/tlc/tlc_process_overview.png)

The Target Language Compiler (TLC) is designed to convert the model description file `` _`model`_.rtw `` (or similar files) into target-specific code or text.

> 目标语言编译器（TLC）旨在将模型描述文件`` _`model`_.rtw ``（或类似文件）转换为特定于目标的代码或文本。

The Target Language Compiler transforms a representation of a Simulink block diagram, called `` _`model`_.rtw ``, into C or C++ code. The `` _`model`_.rtw `` file contains a partial representation of the model. The representation describes the execution semantics of the block diagram in a high-level language. For more information, see [model.rtw File and Scopes](https://www.mathworks.com/help/rtw/tlc/introduction-to-the-model-rtw-file.html).

> 编译器会将 Simulink 块图的表示，称为`` _`model`_.rtw ``，转换成 C 或 C++代码。 `` _`model`_.rtw ``文件包含模型的部分表示。该表示以高级语言描述块图的执行语义。有关更多信息，请参阅[model.rtw 文件和范围](https://www.mathworks.com/help/rtw/tlc/introduction-to-the-model-rtw-file.html)。

After reading the `` _`model`_.rtw `` file, the Target Language Compiler generates its code based on _target files_, which specify particular code for each block, and _model-wide files_, which specify the overall code style. The TLC uses the target files and the `` _`model`_.rtw `` file to generate ANSI C or C++ code.

> 在阅读`` _`model`_.rtw ``文件之后，目标语言编译器根据*target files*生成其代码，这些文件指定每个块的特定代码，以及*model-wide files*指定整体代码风格。 TLC 使用目标文件和`` _`model`_.rtw ``文件生成 ANSI C 或 C++代码。

To create a target-specific application, the code generator requires a template makefile that specifies a C or C++ compiler and compiler options for the build process. The code generator transforms the template makefile into a target makefile (`` _`model`_.mk ``) by performing token expansion specific to a given model. The target makefile is a modified version of the generic `rt_main` file (or `grt_main`). You must modify `grt_main` to conform to the target’s specific requirements, such as interrupt service routines. See [Template Makefiles and Make Options](https://www.mathworks.com/help/rtw/ug/template-makefiles-and-make-options.html) and [Customize Template Makefiles](https://www.mathworks.com/help/rtw/ug/customizing-template-makefiles.html).

> 要创建特定目标的应用程序，代码生成器需要一个模板 makefile，指定用于构建过程的 C 或 C++编译器和编译器选项。代码生成器通过执行特定于给定模型的标记扩展来将模板 makefile 转换为目标 makefile（`` _`model`_.mk ``）。目标 makefile 是通用`rt_main`文件（或`grt_main`）的修改版本。您必须修改`grt_main`以符合目标的特定要求，例如中断服务例程。请参阅[模板 Makefiles 和 Make 选项](https://www.mathworks.com/help/rtw/ug/template-makefiles-and-make-options.html)和[自定义模板 Makefiles](https://www.mathworks.com/help/rtw/ug/customizing-template-makefiles.html)。

The Target Language Compiler has similarities with HTML, Perl, and MATLAB®. It has markup syntax similar to HTML, the power and flexibility of Perl and other scripting languages, and the data handling power of MATLAB (TLC can invoke MATLAB functions). The code that TLC generated is highly optimized and fully commented. With TLC, you can generate code from linear, nonlinear, continuous, discrete, or hybrid Simulink models. The models can include Simulink blocks that are automatically converted to code. Exceptions are MATLAB function blocks and S-function blocks that invoke MATLAB files. The Target Language Compiler uses _block target files_ to transform each block in the `` _`model`_.rtw `` file and a _model-wide target file_ for global customization of the code.

> 目标语言编译器与 HTML、Perl 和 MATLAB® 有相似之处。它具有类似 HTML 的标记语法、Perl 及其他脚本语言的功能和灵活性以及 MATLAB 的数据处理能力（TLC 可以调用 MATLAB 函数）。TLC 生成的代码高度优化且完全注释。使用 TLC，您可以从线性、非线性、连续、离散或混合 Simulink 模型生成代码。模型可以包括自动转换为代码的 Simulink 块，例外情况是调用 MATLAB 文件的 MATLAB 函数块和 S 函数块。目标语言编译器使用 _块目标文件_ 来转换`` _`model`_.rtw ``文件中的每个块，以及 _模型范围的目标文件_ 来全局自定义代码。

You can incorporate C MEX S-functions, with the generated code into the program executable. You can write a target file for your C MEX S-function to _inline_ the S-function (see [Inline C MEX S-Functions](https://www.mathworks.com/help/rtw/tlc/inlining-c-mex-s-functions.html)), to improve performance by eliminating function calls to the S-function itself and the memory overhead of the `SimStruct` of the S-function. Inlining an S-function incorporates the S-function block code into the generated code for the model. When a TLC target file is not present for the S-function, its C or C++ code file is invoked through a function call. See [Inline S-Functions](https://www.mathworks.com/help/rtw/tlc/introduction-inline-s-functions.html). You can also write target files for MATLAB language files or Fortran S-functions.

> 您可以将生成的代码与 C MEX S 函数相结合，将其纳入程序可执行文件中。您可以为 C MEX S 函数编写目标文件以 _inline_ 该 S 函数（参见[Inline C MEX S 函数](https://www.mathworks.com/help/rtw/tlc/inlining-c-mex-s-functions.html)），以通过消除对 S 函数本身以及 S 函数的`SimStruct`的内存开销的函数调用来提高性能。将 S 函数块代码内联到模型的生成代码中。当 S 函数没有 TLC 目标文件时，其 C 或 C++代码文件将通过函数调用调用。参见[内联 S 函数](https://www.mathworks.com/help/rtw/tlc/introduction-inline-s-functions.html)。您还可以为 MATLAB 语言文件或 Fortran S 函数编写目标文件。

### Overview of the Code Generation Process

The Target Language Compiler works with its target files and the code generator output to produce code.

> 目标语言编译器通过其目标文件和代码生成器输出来生成代码。

![](https://www.mathworks.com/help/rtw/tlc/tlc_code_gen_process_flow.png)

When generating code from a Simulink model, the first step in the automated process is to generate a `` _`model`_.rtw `` file. The `` _`model`_.rtw `` file includes the model-specific information required for generating code from the Simulink model. `` _`model`_.rtw `` is passed to the Target Language Compiler, which uses it in combination with a set of included system target files and block target files to generate the code.

> 当从 Simulink 模型生成代码时，自动化过程的第一步是生成一个`` _`model`_.rtw ``文件。 `` _`model`_.rtw ``文件包含生成代码所需的模型特定信息。 `` _`model`_.rtw ``被传递给目标语言编译器，它使用它与一组包含的系统目标文件和块目标文件结合来生成代码。

Only the final executable file is written directly to the current folder. For other files created during code generation, including the `` _`model`_.rtw `` file, a build folder is used. This folder is created in the current folder and is named `` _`model`___`target`__rtw ``. `` _`target`_ `` is the abbreviation for the target environment `grt` that is a generic real-time target.

> 只有最终可执行文件才会直接写入当前文件夹。对于代码生成过程中创建的其他文件，包括`` _`model`_.rtw ``文件，都使用构建文件夹。该文件夹将在当前文件夹中创建，并命名为`` _`model`___`target`__rtw ``。`` _`target`_ ``是目标环境`grt`的缩写，它是一个通用实时目标。

Files placed in the build folder include:

- The body for the generated C or C++ source code (`` _`model`_.c `` or `` _`model`_.cpp ``)

> 生成的 C 或 C++源代码（`` _`model`_.c `` 或 `` _`model`_.cpp ``）的主体

- Header files (`` _`model`_.h ``)

- Header file `` _`model`__private.h `` defining parameters and data structures private to the generated code

> 头文件"\_model_private.h"定义了生成代码私有的参数和数据结构。

- A makefile, `` _`model`_.mk ``, for building the application

- Additional files, described in [Manage Build Process Files](https://www.mathworks.com/help/rtw/ug/build-process-files.html)

> 附加文件，参见[管理构建过程文件](https://www.mathworks.com/help/rtw/ug/build-process-files.html)

## Related Topics

- [Why Use the Target Language Compiler?](https://www.mathworks.com/help/rtw/tlc/target-language-compiler-capabilities.html)
- [Advice About TLC Tutorials](https://www.mathworks.com/help/rtw/tlc/introduction-tlc-tutorials.html)
- [TLC Files](https://www.mathworks.com/help/rtw/tlc/tlc-files.html)
- [Inline S-Functions with TLC](https://www.mathworks.com/help/rtw/tlc/inlining-s-functions-with-tlc.html)
