---
url: https://ww2.mathworks.cn/help/ecoder/ug/custom-file-processing-user-interface-options.html
title: Specify Templates For Code Generation
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-18 21:00:37
tag: 
summary: Create CGT files and CFP templates to use custom file processing features.
---
To use custom file processing features, create CGT files and CFP templates. These files are based on default templates provided by the code generation software. Once you have created your templates, you must integrate them into the code generation process.

Select and edit CGT files and CFP templates, and specify their use in the code generation process in the > pane of a model configuration set. The following figure shows options configured for their defaults.

The options related to custom file processing are:

*   The **Source file (.c) template** field in the **Code templates** and **Data templates** sections. This field specifies the name of a CGT file to use when generating source (`.c` or `.cpp`) files. You must place this file on the MATLAB® path.
    
*   The **Header file (.h) template** field in the **Code templates** and **Data templates** sections. This field specifies the name of a CGT file to use when generating header (`.h`) files. You must place this file on the MATLAB path.
    
    By default, the template for both source and header files is [``_`matlabroot`_/toolbox/rtw/targets/ecoder/ert_code_template.cgt``](matlab:edit([matlabroot '/toolbox/rtw/targets/ecoder/ert_code_template.cgt']);).
    
*   The **File customization template** edit field in the **Custom templates** section. This field specifies the name of a CFP template file to use when generating code files. You must place this file on the MATLAB path. The default CFP template is [``_`matlabroot`_/toolbox/rtw/targets/ecoder/example_file_process.tlc``](matlab:edit([matlabroot '/toolbox/rtw/targets/ecoder/example_file_process.tlc']);).
    

In each of these fields, click **Browse** to navigate to and select an existing CFP template or CGT file. Click **Edit** to open the specified file into the MATLAB editor where you can customize it.

How useful was this information?

Unrated

1 star

2 stars

3 stars

4 stars

5 stars

 Unrated  1 star  2 stars  3 stars  4 stars  5 stars

您点击的链接对应于以下 MATLAB 命令：

请在 MATLAB 命令行窗口中直接输入以执行命令。Web 浏览器不支持 MATLAB 命令。