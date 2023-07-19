---
tip: translate by openai@2023-07-06 21:57:18
...
---
url: [https://www.mathworks.com/help/rtw/ug/enable-build-process-for-folder-names-with-spaces.html](https://www.mathworks.com/help/rtw/ug/enable-build-process-for-folder-names-with-spaces.html)

> URL：[https://www.mathworks.com/help/rtw/ug/](https://www.mathworks.com/help/rtw/ug/)启用带有空格的文件夹名称的构建过程。html
> title: Build Process Support for File and Folder Names - MATLAB & Simulink --- 构建对文件和文件夹名称的进程支持 - MATLAB & Simulink
> date: 2023-07-06 21:54:41
> tag:

summary: Troubleshoot build process errors that occur when file system issues prevent file processing.

> 摘要：当文件系统问题阻止文件处理时，解决构建过程中出现的错误。

---

## Build Process Support for File and Folder Names

### Filenames with Spaces

For the build process that uses `ToolchainInfo` objects, only these toolchains support the use of filenames containing spaces:

> 对于使用 `ToolchainInfo` 对象的构建过程，只有这些工具链支持使用包含空格的文件名：

- `GNU gcc/g++ | gmake (64-bit Linux)` on Linux®
- `MinGW64 | gmake (64-bit Windows)` on Windows®
- `Xcode with Clang | gmake (64-bit Mac)` on Mac

The build process that uses template makefiles does not support the use of filenames containing spaces.

> 构建过程使用模板 makefiles 不支持使用包含空格的文件名。

### Folder Names with Spaces

On a Windows system, the code generator maps a drive corresponding to the MATLAB® installation folder for either of these conditions:

> 在 Windows 系统上，代码生成器会根据以下任一条件映射到 MATLAB® 安装文件夹的驱动器：

- The `matlabroot` folder is a UNC location.
- The path the `matlabroot` folder contains spaces, and the system has no alternative name support.

> 路径 `matlabroot` 文件夹中包含空格，系统没有支持其他名称的选择。

These folder paths can contain spaces:

- The path to your MATLAB installation folder (`matlabroot`). For example, `C:\Program Files\MATLAB\R2015b`

> 路径到您的 MATLAB 安装文件夹（`matlabroot`）。例如，`C:\Program Files\MATLAB\R2015b`

- The path to the current working folder where you start the build (`pwd`). For example, `C:\Users\username\Documents\My Work`.

> 当前工作文件夹的路径，你开始构建（`pwd`）。 例如，`C：\ Users \ username \ Documents \ My Work`。

- The path to the installation folder for a compiler that the build process uses.

If your work environment includes one or more of the preceding scenarios, use the following support mechanisms for the build process:

> 如果您的工作環境包括以上一個或多個情況，請使用以下支持機制來進行構建過程：

- If you are using the toolchain approach to build generated code, the system support for spaces in folder names influences toolchain operation:

> 如果您使用工具链方法来构建生成的代码，系统对文件夹名中的空格的支持会影响工具链的操作。

- For Linux systems and Windows systems with 8.3 name creation enabled, the toolchain manages spaces in folder names by using alternative names from the operating system. The toolchain uses the `TransformPathsWithSpaces` attribute to manage these names.

> 对于启用了 8.3 文件名创建的 Linux 系统和 Windows 系统，工具链通过使用操作系统的替代名称来管理文件夹中的空格。工具链使用 `TransformPathsWithSpaces` 属性来管理这些名称。

```
```

addAttribute(toolchainObject, 'TransformPathsWithSpaces', true);

```
```

The security permissions of drives and folders can determine whether the toolchain transforms the path. For example, if the path contains a folder with a security configuration that forbids 8.3 path transformations, the toolchain does not transform the path and the build process produces a warning.

> 驱动器和文件夹的安全权限可以决定工具链是否会转换路径。例如，如果路径包含一个具有禁止 8.3 路径转换的安全配置的文件夹，工具链不会转换该路径，构建过程会产生警告。

- For Windows systems with 8.3 name creation disabled, the toolchain manages spaces in folder names by mapping a network drive using a batch file (.bat). This operation requires adding the `RequiresBatchFile` attribute to the toolchain definition.

> 对于禁用 8.3 文件名创建的 Windows 系统，工具链通过使用批处理文件（.bat）映射网络驱动器来管理文件夹名称中的空格。此操作需要将 `RequiresBatchFile` 属性添加到工具链定义中。

```
```

addAttribute(toolchainObject, 'RequiresBatchFile', true);

```
```

When developing a toolchain for a Windows system, set both attributes. For more information about the toolchain attributes, see [`addAttribute`](https://www.mathworks.com/help/coder/ref/coder.make.toolchaininfo.addattribute.html).

> 在为 Windows 系统开发工具链时，请设置两个属性。有关工具链属性的更多信息，请参阅[`addAttribute`](https://www.mathworks.com/help/coder/ref/coder.make.toolchaininfo.addattribute.html)。

- If you are using the template makefile approach to build generated code, the template makefile (`.tmf`) requires code to manage spaces in folder names. When the alternative folder names (Windows short names) differ from the file system folder names (Windows long names), add this code to the makefile.

> 如果您使用模板 Makefile 来构建生成的代码，模板 Makefile（`.tmf`）需要代码来管理文件夹名中的空格。当替代文件夹名（Windows 短名）与文件系统文件夹名（Windows 长名）不同时，请将此代码添加到 Makefile 中。

```
ALT_MATLAB_ROOT         = |>ALT_MATLAB_ROOT<|
ALT_MATLAB_BIN          = |>ALT_MATLAB_BIN<|
!if "$(MATLAB_ROOT)" != "$(ALT_MATLAB_ROOT)"
MATLAB_ROOT = $(ALT_MATLAB_ROOT)
!endif
!if "$(MATLAB_BIN)" != "$(ALT_MATLAB_BIN)"
MATLAB_BIN = $(ALT_MATLAB_BIN)
!endif

```

When the values of the location tokens are not equal, this code replaces `MATLAB_ROOT` with `ALT_MATLAB_ROOT`. The replacement indicates that the path to your MATLAB installation folder includes spaces. This code applies the same type of replacement for `MATLAB_BIN` with `ALT_MATLAB_BIN`. The preceding code is specific to `nmake`. For platform-specific examples, see the supplied template makefiles.

> 当位置令牌的值不相等时，此代码将 `MATLAB_ROOT` 替换为 `ALT_MATLAB_ROOT`。替换表明您的 MATLAB 安装文件夹的路径包含空格。此代码对 `MATLAB_BIN` 也应用相同类型的替换为 `ALT_MATLAB_BIN`。前面的代码是特定于 `nmake` 的。有关特定平台的示例，请参阅提供的模板 makefiles。

With either build approach, when there is an issue with support for creation of alternate names (short names), build errors can occur on Windows. If a build generates an error message similar to the following message, see [Troubleshooting Errors When Folder Names Have Spaces](https://www.mathworks.com/help/rtw/ug/enable-build-process-for-folder-names-with-spaces.html#bu6lx34).

> 如果支持创建替代名称（简称）存在问题，无论使用哪种构建方法，Windows 上都可能会发生构建错误。如果构建生成类似以下消息的错误消息，请参阅[当文件夹名称具有空格时的故障排除](https://www.mathworks.com/help/rtw/ug/enable-build-process-for-folder-names-with-spaces.html#bu6lx34)。

```
NMAKE : fatal error U1073: don't know how to make ' ...

```

When using operating system commands, such as `system` or `dos`, enclose paths that specify executable files or command parameters in double quotes (`" "`). For example:

> 使用操作系统命令，如 `system` 或 `dos` 时，将指定可执行文件或命令参数的路径用双引号（`" "`）括起来。例如：

```
system('dir "D:\Applications\Common Files"')

```

This table provides a summary of build folder support and limitations for Windows.

> 这张表提供了 Windows 构建文件夹支持和限制的摘要。

<table><colgroup><col width="20%"><col width="40%"><col width="40%"></colgroup><thead><tr><th>Build Process Folders</th><th>Approach for Paths with UNC or Spaces</th><th>Support for Windows</th></tr></thead><tbody><tr><td><p><code>matlabroot</code> folder</p><p>The <code>matlabroot</code> value is derived from the MATLAB installation location.</p></td><td><p>During a build, a UNC location such as:</p><div><pre>\\networkdrive
\matlab\R20xxb
</pre></div><p>could be remapped as:</p><div><pre>T:\
</pre></div><p>During a build on a Windows system with short filename (8.3) support (default for Windows using NTFS), the build process uses the Windows API <code>getShortPathName()</code> for the folder location.</p><p>During a build on a Windows system without short filename (8.3) support (systems using ReFS or using NTFS with 8.3 support disabled), a location with spaces in the path such as:</p><div><pre>C:\Program Files
\MATLAB
\R20xxb
</pre></div><p>could be remapped as:</p><div><pre>T:\R20xxb
</pre></div></td><td><p>Build process folder support available independent of file system (NTFS or ReFS) or file system configuration for short filename support.</p><p><strong>Limitations:</strong></p><p>On systems that require drive mapping for the installation location, the build process requires that a drive letter is available for mapping.</p><p>On systems without short filename (8.3) support (using ReFS or using NTFS with 8.3 support disabled), the final folder in the installation location cannot contain spaces. For example, a final folder name:</p><div><pre>C:\Program Files
\MATLAB
\R20xxb sp1
</pre></div><p>is not supported.</p></td></tr><tr><td rowspan="2"><p>Code generation folder</p><p>Simulation cache folder</p><p>Custom code source file locations—among others, these locations include folders specified by:</p><div><ul><li><p><code>rtwmakecfg.m</code></p></li><li><p>Model configuration parameter <strong>Additional build information</strong></p></li><li><p>Code replacement library</p></li></ul></div></td><td><p>For UNC locations, build process temporarily maps a drive by using the shell commands <code>pushd</code> and <code>popd</code>.</p></td><td><p>Build process folder support is available independent of file system (NTFS or ReFS) or file system configuration for short path name support.</p></td></tr><tr><td><p>For paths with spaces, build process uses the Windows short path name (8.3) by using the Windows API:</p><div><pre>getShortPathName()
</pre></div></td><td><p>Build process folder support depends on NTFS file system and requires Windows default support. Registry sets value of 2 or 0 for:</p><div><pre>NtfsDisable8dot3NameCreation
</pre></div><p><strong>Limitations:</strong> Build process does not support spaces in the path to these folders for:</p><div><ul><li><p>NTFS file system with short path name support disabled</p></li><li><p>ReFS file system (this file system does not support short path names)</p></li></ul></div></td></tr></tbody></table>

### Troubleshooting Errors When Folder Names Have Spaces

On Windows, when there is an issue with support for creation of short filenames, build process errors can occur. When this issue affects a build, you see an error message similar to:

> 在 Windows 上，如果存在支持创建短文件名的问题，构建过程可能会出错。当这个问题影响构建时，您会看到类似的错误消息：

```
NMAKE : fatal error U1073: don't know how to make 'C:\Work\My'

```

This message can occur if a space in the folder name (`C:\Work\My Models`) prevents the build process from finding the model or a file to build. For descriptions of the build-related folders that are sensitive to a space in the folder name or path, see [Folder Names with Spaces](https://www.mathworks.com/help/rtw/ug/enable-build-process-for-folder-names-with-spaces.html#bviznfr).

> 这个消息可能发生在文件夹名称（`C:\Work\My Models`）中的空格阻止构建过程找到模型或要构建的文件时。有关构建相关文件夹的描述，敏感于文件夹名称或路径中的空格，请参见[带空格的文件夹名称](https://www.mathworks.com/help/rtw/ug/enable-build-process-for-folder-names-with-spaces.html#bviznfr)。

To avoid issues from folder names with spaces when Windows short filename support for filenames is disabled, do not use paths with spaces. For example, install third-party software to paths without spaces. Do not use paths with spaces for folders containing your models, source files, or libraries.

> 为了避免在禁用 Windows 文件名简称支持时遇到文件夹名称带有空格的问题，请不要使用带有空格的路径。例如，将第三方软件安装到没有空格的路径。不要在包含模型，源文件或库的文件夹中使用带有空格的路径。

An issue can occur with builds that use folder names with spaces, because it is possible to disable Windows alternate name support. The build process uses this alternate name support on Windows systems. There are many terms for this file, folder, and path alternate name support:

> 可能会出现问题的是使用带有空格的文件夹名称的构建，因为可以禁用 Windows 的替代名称支持。构建过程在 Windows 系统上使用这种替代名称支持。这个文件、文件夹和路径的替代名称支持有很多术语：

- 8.3 name
- DOS path
- short filename (SFN, ShortFileName)
- long name alias
- Windows path alias

Verify the type of file system that the drive uses. In Windows Explorer, right-click the drive icon and select properties.

> 在 Windows 资源管理器中，右键单击驱动器图标，然后选择属性，以验证驱动器使用的文件系统类型。

- If the file system is [ReFS](https://en.wikipedia.org/wiki/ReFS) (Resilient File System), it is an issue. The ReFS does not provide short filename support. Except for the MATLAB installation folder, the build process does not support folder names with spaces for the ReFS file system. If your work environment requires short filename support for the build folder or for additional external code folders, do not use ReFS.

> 如果文件系统是 ReFS（弹性文件系统），这是一个问题。ReFS 不提供短文件名支持。除了 MATLAB 安装文件夹，ReFS 文件系统的构建过程不支持带有空格的文件夹名称。如果您的工作环境需要为构建文件夹或其他外部代码文件夹提供短文件名支持，请不要使用 ReFS。

- If the file system is [NTFS](https://en.wikipedia.org/wiki/NTFS) (New Technology File System), it is possible that the build error is related to a registry setting incompatibility. Continue with troubleshooting steps.

> 如果文件系统是 NTFS（新技术文件系统），可能构建错误与注册表设置不兼容有关。继续进行故障排除步骤。

The error could stem from an issue with short filename support on a system using NTFS. Check the Windows registry setting that enables the creation of short names for files, folders, and paths.

> 可能是系统使用 NTFS 时短文件名支持出现问题造成的错误。检查 Windows 注册表设置，以便为文件、文件夹和路径启用短名称的创建。

1. Open the Windows command prompt, running as administrator. For example, from the Windows Start menu, type `cmd`, right-click the `cmd.exe` icon, and select `Run as administrator`.

> 打开 Windows 命令提示符，以管理员身份运行。例如，从 Windows 开始菜单中输入“cmd”，右键单击“cmd.exe”图标，然后选择“以管理员身份运行”。

2. Change to the `windows\system32` folder and query the `NtfsDisable8dot3NameCreation` status by typing:

> 2. 切换到 `windows\system32` 文件夹，然后通过输入查询 `NtfsDisable8dot3NameCreation` 状态：

3. If the registry state of `NtfsDisable8dot3NameCreation` is not 0 (enable `8dot3` name creation for all volumes on the system), change the value to 0 by typing:

> 如果 `NtfsDisable8dot3NameCreation` 的注册表状态不是 0（启用系统上所有卷的 `8dot3` 名称创建），通过键入来将值更改为 0：

```
For more information about enabling creation of short names. See [https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff621566(v=ws.11)](<https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff621566(v=ws.11)>).

Changing the registry setting enables creation of short names only for files and folders that are created after the change.
```

4. To create short names for files created while short name creation was disabled, at the Windows command line, use the `fsutil` utility.

> 在 Windows 命令行中，使用 `fsutil` 实用程序来为在禁用短名创建时创建的文件创建短名。

```
To set the short name, the syntax is:

```

> fsutil file setshortname <FileName> <ShortName>

```

For example, to create the short name `PROGRA~1` for the long name `C:\Program Files`, type:

```

> fsutil file setshortname "C:\Program Files" PROGRA~1

```

The `C:\Program Files` folder name is in quotations because it has spaces.
```

5. To verify that the short name was created, use the `dir` command with `/x` option to show short names.

> 使用带有/x 选项的 `dir` 命令来验证短名称是否已创建。

### Folder Names with Special Characters

The build process might produce an error if a build-related folder path contains:

> 构建过程可能会出现错误，如果构建相关文件夹路径包含：

- Unicode® characters that do not belong to the system locale. This limitation does not apply if the build process uses a Microsoft® Visual C++® compiler.

> Unicode® 字符不属于系统语言环境。如果构建过程使用 Microsoft® Visual C++® 编译器，则不受此限制。

- A Japanese (multibyte) character where the final byte is equal to the `5C` hexadecimal character. The make and compiler tools might incorrectly interpret the final byte as the `'\'` (backslash) character.

> 一个日文（多字节）字符，最后一个字节等于十六进制字符“5C”。制作和编译工具可能会错误地将最后一个字节解释为“\”（反斜杠）字符。

### Very Long Folder Paths

For the MinGW® compiler, the build process produces an error when the command line length exceeds the Windows limit of 32,767 characters. If this error occurs, check the length of include paths. You can reduce the command line length by building the generated code in a code generation folder that has a shorter name

> 对于 MinGW® 编译器，当命令行长度超过 Windows 的 32,767 个字符限制时，构建过程会产生错误。如果发生此错误，请检查包含路径的长度。您可以通过在具有较短名称的代码生成文件夹中构建生成的代码来减少命令行长度。

## See Also

[`addAttribute`](https://www.mathworks.com/help/coder/ref/coder.make.toolchaininfo.addattribute.html)

> [添加属性](https://www.mathworks.com/help/coder/ref/coder.make.toolchaininfo.addattribute.html)

## Related Topics

- [Manage Build Process Folders](https://www.mathworks.com/help/rtw/ug/build-process-folders.html)
- [Manage Build Process Files](https://www.mathworks.com/help/rtw/ug/build-process-files.html)
- [Manage Build Process File Dependencies](https://www.mathworks.com/help/rtw/ug/build-process-file-dependencies.html)
- [Add Build Process Dependencies](https://www.mathworks.com/help/rtw/ug/add-build-process-dependencies.html)
- [Build Process Workflow for Real-Time Systems](https://www.mathworks.com/help/rtw/ug/build-process-workflow-for-real-time-systems.html)

## External Websites

- [https://www.mathworks.com/matlabcentral/answers/95399-why-is-the-build-process-failing-with-error-code-nmake-fatal-error-u1073-don-t-know-how-to-make](https://www.mathworks.com/matlabcentral/answers/95399-why-is-the-build-process-failing-with-error-code-nmake-fatal-error-u1073-don-t-know-how-to-make)
- [https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc959352(v=technet.10)](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc959352(v=technet.10))
- [https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff621566(v=ws.11)](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff621566(v=ws.11))
