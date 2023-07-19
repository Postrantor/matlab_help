---
tip: translate by openai@2023-07-06 21:46:28
url: https://www.mathworks.com/help/rtw/ug/build-process-workflow-for-real-time-systems.html
title: Build Process Workflow for Real-Time Systems - MATLAB & Simulink --- 构建实时系统的流程工作流 - MATLAB & Simulink
date: 2023-07-06 21:45:16
tag:
summary: Generate C code from a model and build an executable program.
---
## Build Process Workflow for Real-Time Systems

The building process includes generating code in C or C++ from a model and building an executable program from the generated code. This example can use a generic real-time (GRT) or an embedded real-time (ERT) system target file (STF) for code generation. The resulting standalone program runs on your development computer, independent of external timing and events.

> 建筑过程包括从模型生成 C 或 C++ 代码，并从生成的代码构建可执行程序。此示例可以使用通用实时（GRT）或嵌入式实时（ERT）系统目标文件（STF）进行代码生成。由此产生的独立程序在您的开发计算机上运行，独立于外部时序和事件。

### Working Folder

This example uses a local copy of the `slexAircraftExample` model, stored in its own folder, `aircraftexample`. Set up your _working folder_ as follows:

> 这个示例使用一个本地的 `slexAircraftExample` 模型副本，存储在自己的文件夹 `aircraftexample` 中。设置你的*工作文件夹*如下：

1. In the MATLAB® Current Folder browser, navigate to a folder to which you have write access.
2. To create the working folder, enter the following MATLAB command:
3. Make `aircraftexample` your working folder:
4. Open the `slexAircraftExample` model:

   ```
   openExample('slexAircraftExample')

   ```

   The model appears in the Simulink® Editor model window.
5. In the model window, choose > . Navigate to your working folder, `aircraftexample`. Save a copy of the `slexAircraftExample` model as `myAircraftExample`.

> 在模型窗口中，选择 >。导航到您的工作文件夹 `aircraftexample`。将 `slexAircraftExample` 模型另存为 `myAircraftExample`。

### Build Folder and Code Generation Folders

While producing code, the code generator creates a _build folder_ within your working folder. The build folder name is ``_`model`___`target`__rtw``, derived from the name of the source model and the chosen system target file. The build folder stores generated source code and other files created during the build process. Examine the build folder contents at the end of this example.

> 在生成代码时，代码生成器会在您的工作文件夹中创建 *build 文件夹*。构建文件夹的名称是 ``_`model`___`target`__rtw``，由源模型的名称和所选择的系统目标文件名称派生而来。构建文件夹存储生成的源代码和构建过程中创建的其他文件。在本例结束时，请检查构建文件夹的内容。

When a model contains Model blocks (references to other models), the model build creates special subfolders in your [Code generation folder](https://www.mathworks.com/help/simulink/slref/simulinkpreferences.html#mw_52e50f72-177c-4bde-b12f-d86e3acd6a58) to organize code for the referenced models. These code generation folders exist alongside product build folders and are named `slprj`. For more information, see [Generate Code for Model Reference Hierarchy](https://www.mathworks.com/help/rtw/ug/generate-code-for-model-reference-hierarchy.html).

> 当模型包含模型块（对其他模型的引用）时，模型构建会在您的[代码生成文件夹](https://www.mathworks.com/help/simulink/slref/simulinkpreferences.html#mw_52e50f72-177c-4bde-b12f-d86e3acd6a58)中创建特殊的子文件夹，以组织引用模型的代码。这些代码生成文件夹与产品构建文件夹并存，名为 `slprj`。有关更多信息，请参阅[为模型引用层次结构生成代码](https://www.mathworks.com/help/rtw/ug/generate-code-for-model-reference-hierarchy.html)。

Under the `slprj` folder, a subfolder named `_sharedutils` contains generated code that can be shared between models.

> 在 `slprj` 文件夹下，一个名为 `_sharedutils` 的子文件夹包含可以在模型之间共享的生成代码。

### Set Model Parameters for Code Generation

To generate code from your model, you must change some of the model configuration parameters. In particular, the generic real-time (GRT) system target file and most other system target files require that the model specifies a fixed-step solver.

> 要从模型生成代码，您必须更改模型配置参数的一些内容。特别是，通用实时（GRT）系统目标文件和大多数其他系统目标文件要求模型指定固定步骤求解器。

**Note**

For models that specify variable-step solvers, the code generator produces code only if the models also specify rapid simulation (`rsim`) or S-function system target files.

> 对于指定可变步长求解器的模型，如果模型还指定快速仿真（`rsim`）或 S 函数系统目标文件，则代码生成器才会生成代码。

1. Open the `myAircraftExample` model if it is not already open.
2. In the Configuration Parameters dialog box, specify configuration parameter values for the solver:

> 在配置参数对话框中，为求解器指定配置参数值：

```
- **Start time**: `0.0`
- **Stop time**: `60`
- **Type**: `Fixed-step`
- **Solver**: `ode5 (Dormand-Prince)`
- **Fixed step size (fundamental sample time)**: `0.1`
- **Treat each discrete rate as a separate task**: `Off`
```

3. Click **Apply**.
4. Save the model.

### Configure Build Process

To configure the build process for your model, choose a system target file, a toolchain or template makefile, and a `make` command.

> 为您的模型配置构建过程，请选择一个系统目标文件、一个工具链或模板 makefile，以及一个 `make` 命令。

In these examples and in most applications, you do not need to specify these parameters individually. The examples use the ready-to-run generic real-time target (GRT) configuration. The GRT system target file builds a standalone executable program that runs on your desktop computer.

> 在这些例子和大多数应用中，您不需要单独指定这些参数。示例使用即可运行的通用实时目标（GRT）配置。GRT 系统目标文件构建一个独立的可执行程序，可在您的台式计算机上运行。

To select the GRT system target file:

1. Open the `myAircraftExample` model if it is not already open.
2. In the Configuration Parameters dialog box, in the **System target file** field, enter `grt.tlc`. Then click **Apply**.

> 在配置参数对话框中，在**系统目标文件**字段中输入 `grt.tlc`。然后单击**应用**。

```
You see selections for **Toolchain** (`Automatically locate an installed toolchain`), and **Build Configuration** (`Faster Builds`).

![](https://www.mathworks.com/help/rtw/ug/me_cp_rtw_general_f14rtw.png)
```

3. Save the model.

**Note**

If you click **Browse**, a System Target File Browser opens and displays the system target files on the MATLAB path. Some system target files require additional products. For example, `ert.tlc` requires Embedded Coder®.

> 如果您单击**浏览**，则会打开系统目标文件浏览器，并显示 MATLAB 路径上的系统目标文件。某些系统目标文件需要额外的产品。例如，`ert.tlc` 需要嵌入式编码器 ®。

### Set Code Generation Parameters

1. Open the `myAircraftExample` model if it is not already open.
2. In the Configuration Parameters dialog box, specify settings:

   1. Use the default settings for the advanced parameters, which control build verbosity and debugging:

      - **Verbose build** (`RTWVerbose`)
      - **Retain .rtw file** (`RetainRTWFile`)
      - **Profile TLC** (`ProfileTLC`)
      - **Start TLC debugger when generating code** (`TLCDebug`)
      - **Start TLC coverage when generating code** (`TLCCoverage`)
      - **Enable TLC assertion** (`TLCAssert`)
   2. Use the default > settings.
   3. The > options control the look and feel of generated code. Use the default settings.
   4. Select > .

      1. From the **Shared code placement** list, select `Shared location`. The build process places generated code for utilities in a subfolder within your [Code generation folder](https://www.mathworks.com/help/simulink/slref/simulinkpreferences.html#mw_52e50f72-177c-4bde-b12f-d86e3acd6a58).
      2. Under the **Advanced parameters**, clear the check box.
      3. Under the **Advanced parameters**, select the check box.
   5. In > , select **Create code generation report** and **Open report automatically**. This action enables the software to create and display a code generation report for the `myAircraftExample` model.
3. Click **Apply** and save the model.

### Build and Run a Program

The build process generates C code from the model. It then compiles and links the generated program to create an executable image. To build and run the program:

> 编译过程将模型生成 C 代码。然后编译和链接生成的程序，以创建可执行映像。要构建和运行程序：

1. With the `myAircraftExample` model open, perform one of these actions:

> 1. 使用 `myAircraftExample` 模型打开，执行以下操作之一：

```
- On the **Apps** tab, open the **Simulink Coder** app. In the **C Code** tab, click **Build**.
- Press **Ctrl+B**.
- Run the [`slbuild`](https://www.mathworks.com/help/simulink/slref/slbuild.html) command from the MATLAB command line.

You see code generation and compilation messages in the Command Window. The initial message is:

```

### Starting build procedure for model: myAircraftExample

```

The contents of many of the succeeding messages depends on your compiler and operating system. The final messages include:

```

### Created executable myAircraftExample.exe

### Successful completion of build procedure for model: myAircraftExample

### Creating HTML report file index.html

```

The code generation folder now contains an executable, `myAircraftExample.exe` (Microsoft® Windows® platforms) or `myAircraftExample` (UNIX® platforms). In addition, the build process has created an `slprj` folder and a `myAircraftExample_grt_rtw` folder in your [Code generation folder](https://www.mathworks.com/help/simulink/slref/simulinkpreferences.html#mw_52e50f72-177c-4bde-b12f-d86e3acd6a58).

**Note**

After generating the code for the `myAircraftExample` model, the build process displays a code generation report. See [Report Generation](https://www.mathworks.com/help/rtw/report-generation.html) for more information about how to create and use a code generation report.
```

2. To see the contents of the working folder after the build, enter the `dir` or `ls` command:

> 输入 `dir` 或 `ls` 命令，查看构建后工作文件夹的内容：

```
```

>> dir
>>

.                               myAircraftExample.slx           slprj
..                              myAircraftExample.slx.autosave
myAircraftExample.exe           myAircraftExample_grt_rtw

```
```

3. To run the executable from the Command Window, type `!myAircraftExample`. The `!` character passes the command that follows it to the operating system, which runs the standalone `myAircraftExample` program.

> 在命令窗口中运行可执行文件，请输入'！myAircraftExample'。'！'字符会将其后的命令传递给操作系统，该系统将运行独立的'myAircraftExample'程序。

```
```

>> !myAircraftExample
>>

** starting the model **
** created myAircraftExample.mat **

```
```

4. To see the files created in the build folder, use the `dir` or `ls` command again. The exact list of files produced varies among MATLAB platforms and versions. Here is a sample list from a Windows platform:

> 4. 要查看 build 文件夹中创建的文件，再次使用 `dir` 或 `ls` 命令。生成的文件列表因 MATLAB 平台和版本而异。以下是 Windows 平台的示例列表：

```
```

>> dir myAircraftExample_grt_rtw
>>

.                     rt_main.obj                  myAircraftExample_data.c
..                    rtmodel.h                    myAircraftExample_data.obj
buildInfo.mat         rtw_proj.tmw                 myAircraftExample_private.h
codeInfo.mat          myAircraftExample.bat        myAircraftExample_ref.rsp
myAircraftExample.c   myAircraftExample_types.h    html
myAircraftExample.h   myAircraftExample.mk
rt_logging.obj        myAircraftExample.obj

```
```

### Contents of the Build Folder

The build process creates a build folder and names it ``_`model`___`target`__rtw``, where ``_`model`_`` is the name of the source model and ``_`target`_`` is the system target file selected for the model. In this example, the build folder is named `myAircraftExample_grt_rtw`.

> 编译过程会创建一个名为"\_`model`**\_`target`**rtw"的构建文件夹，其中"_`model`_"是源模型的名称，"_`target`_"是为该模型选择的系统目标文件。在本例中，构建文件夹的名称为 `myAircraftExample_grt_rtw`。

The build folder includes the following generated files.

<table><colgroup><col width="46%"><col width="55%"></colgroup><thead><tr><th>File</th><th>Description</th></tr></thead><tbody><tr><td><p><code>myAircraftExample.c</code></p></td><td><p>Standalone C code that implements the model</p></td></tr><tr><td><p><code>myAircraftExample.h</code></p></td><td><p>An include header file containing definitions of parameters and state variables</p></td></tr><tr><td><p><code>myAircraftExample_private.h</code></p></td><td><p>Header file containing common include definitions</p></td></tr><tr><td><p><code>myAircraftExample_types.h</code></p></td><td><p>Forward declarations of data types used in the code</p></td></tr><tr><td><p><code>rtmodel.h</code></p></td><td><p>Header file for including generated code in the static main program (its name does not change, and it simply includes <code>myAircraftExample.h</code>)</p></td></tr></tbody></table>

The code generation report that you created for the `myAircraftExample` model displays a link for each of these files. You can click the link explore the file contents.

> 您为 `myAircraftExample` 模型创建的代码生成报告显示了每个文件的链接。您可以单击该链接查看文件内容。

The build folder contains other files used in the build process. They include:

> 构建文件夹包含构建过程中使用的其他文件。它们包括：

- `myAircraftExample.mk` — Makefile for building executable using the specified Toolchain.
- Object (`.obj`) files
- `myAircraftExample.bat` — Batch control file
- `rtw_proj.tmw` — Marker file
- `buildInfo.mat` — Build information for relocating generated code to another development environment

> - `buildInfo.mat` — 用于将生成的代码重新定位到另一个开发环境的构建信息

- `myAircraftExample_ref.rsp` — Data to include as command-line arguments to [`mex`](https://www.mathworks.com/help/matlab/ref/mex.html) (Windows systems only)

> `- ` myAircraftExample_ref.rsp ` — 将数据包括在[` mex`]([https://www.mathworks.com/help/matlab/ref/mex.html](https://www.mathworks.com/help/matlab/ref/mex.html))的命令行参数中（仅限 Windows 系统）

The build folder also contains a subfolder, `html`, which contains the files that make up the code generation report. For more information, see [Reports for Code Generation](https://www.mathworks.com/help/rtw/ug/reports-for-code-generation.html).

> 构建文件夹中还包含一个子文件夹“html”，其中包含构成代码生成报告的文件。有关更多信息，请参阅[代码生成的报告](https://www.mathworks.com/help/rtw/ug/reports-for-code-generation.html)。

### Customized Makefile Generation

After producing code, the code generator produces a customized makefile, _`model`_`.mk`. The generated makefile instructs the `make` system utility to compile and link source code generated from the model, any required harness program, libraries, or user-provided modules. The code generator produces the file _`model`_`.mk` regardless of the approach that you use for build process control:

> 代码生成后，代码生成器会生成一个定制的 makefile，_`model`_`.mk`。生成的 makefile 指示 `make` 系统实用程序编译和链接从模型生成的源代码，任何所需的外壳程序，库或用户提供的模块。无论您使用何种方法来控制构建过程，代码生成器都会生成文件*`model`*`.mk`。

- If you use the toolchain approach, the code generator creates _`model`_`.mk` based on the model **Toolchain settings**. You can modify generation of the makefile through the `rtwmakecfg.m` API.

> 如果您使用工具链方法，则代码生成器将根据模型**工具链设置**创建 `model`.mk 文件。您可以通过 `rtwmakecfg.m` API 修改 makefile 的生成。

- If you use the template makefile approach, the code generator creates _`model`_`.mk` from a system template file, _`system`_`.tmf` (where _`system`_ stands for the selected system target file name). The system template makefile is designed for your system target file. You can modify the template makefile to specify compilers, compiler options, and additional information for the creation of the executable.

> 如果您使用模板 Makefile 方法，则代码生成器会根据系统模板文件（_`system`_`.tmf`，其中*`system` *代表所选择的系统目标文件名）生成* `model`*`.mk`。系统模板 Makefile 是为您的系统目标文件设计的。您可以修改模板 Makefile 以指定编译器、编译器选项和用于生成可执行文件的其他信息。

For more information, see [Configure Toolchain (ToolchainInfo) or Template Makefile Build Process](https://www.mathworks.com/help/rtw/ug/program-builds.html).

> 若要了解更多信息，请参阅[配置工具链（ToolchainInfo）或模板 Makefile 构建过程](https://www.mathworks.com/help/rtw/ug/program-builds.html)。

## Related Topics

- [Configure Toolchain (ToolchainInfo) or Template Makefile Build Process](https://www.mathworks.com/help/rtw/ug/program-builds.html)
- [Manage Build Process Folders](https://www.mathworks.com/help/rtw/ug/build-process-folders.html)
- [Reports for Code Generation](https://www.mathworks.com/help/rtw/ug/reports-for-code-generation.html)
- [Select and Configure C or C++ Compiler](https://www.mathworks.com/help/rtw/ug/compiler-or-ide-selection-and-configuration.html)
