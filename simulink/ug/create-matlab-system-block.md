---
url: https://ww2.mathworks.cn/help/simulink/ug/create-matlab-system-block.html
title: Implement a MATLAB System Block
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-03 11:29:50
tag: 
summary: Implement a block and assign a System object to it.
---
## Implement a MATLAB System Block

Implement a block and assign a System object™ to it. You can then explore the block to see the effect.

1.  Create a new model and add the MATLAB System block from the User-Defined Functions library.
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/matlab_system_block.png)
    
2.  In the block dialog box, from the **New** list, select `Basic`, `Advanced`, or `Simulink Extension` if you want to create a new System object from a template. Modify the template according to your needs and save the System object.
    
3.  Enter the full path name for the System object in the **System object name**. Click the list arrow. If valid System objects exist in the current folder, the names appear in the list.
    
    The MATLAB System block icon and port labels update to those of the corresponding System object. For example, suppose you selected a System object named `lmsSysObj` in your current folder. The block updates as shown in the figure:
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/matlab_system_block_updatedicon.png)
    

**Note**

After you associate the block with a System object class name, you cannot assign a new System object using the same MATLAB System block dialog box. Instead, right-click the MATLAB System block, select **Block Parameters (MATLABSystem)** and enter a new class name in **System object name**.

### Understanding the MATLAB System Block

1.  Double-click the block. The MATLAB System block dialog box reflects the System object parameters. The dialog box usually includes a **Source code** link that leads to the System object class file. For example:
    
    ![](https://ww2.mathworks.cn/help/simulink/ug/matlab_system_block_updateddialog.png)
    
    The **Source code** link appears if the System object uses MATLAB® language. It does not appear if you have:
    
    *   Converted the System object to P-code
        
    *   Overridden the default behavior using the `getHeaderImpl` method
        
    
2.  Click **Source code** and observe that the public and active properties in the System object appear in the MATLAB System block dialog box as block parameters.
    
3.  Select how you want the model to simulate the block using the **Simulate using** parameter. (This parameter appears at the bottom of each MATLAB System block if there is only one tab, or the bottom of the first of multiple tabs.)
    

## Related Examples

*   [Change Blocks Implemented with System Objects](https://ww2.mathworks.cn/help/simulink/ug/change-system-object-assigned-to-block.html)

## More About

*   [MATLAB System Block](https://ww2.mathworks.cn/help/simulink/ug/what-is-matlab-system-block.html)
*   [Mapping System Object Code to MATLAB System Block Dialog Box](https://ww2.mathworks.cn/help/simulink/ug/integrate-system-object-in-model.html)
*   [Simulation Modes](https://ww2.mathworks.cn/help/simulink/ug/simulation-modes.html)

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