---
url: https://ww2.mathworks.cn/help/simulink/ug/operate-on-existing-masks.html
title: Manage Existing Masks
- MATLAB & Simulink
- MathWorks 中国
date: 2023-07-28 08:27:12
tag: 
summary: View or edit an existing block mask.
---
## Manage Existing Masks

### Change a Block Mask

You can change an existing mask by reopening the Mask Editor and using the same techniques that you used to create the mask:

1. Select the masked block.
2. On the **Subsystem Block** tab, in the **Mask** group, click **Edit Mask**.

The Mask Editor reopens, showing the existing mask definition. Change the mask as needed. After you change a mask, click **Apply** to preserve the changes.

### View Mask Parameters

To display a mask dialog box, double-click the block. Alternatively, select the block and on the **Block** tab, in the **Mask** group, click **Mask Parameters**.

**Tip**

Each block has a block parameter dialog box. To view the block parameters dialog box of a masked block, right-click the masked block and select .

### Look Under Block Mask

To see the block diagram under a masked Subsystem block, built-in block, or the model referenced by a masked model block, select the block and on the **Subsystem** tab, click **Look Under Mask**.

### Remove Mask

To remove a mask from a block,

1. Select the block.
2. On the **Block** tab, in the **Mask** group,click **Edit Mask**.

   The Mask Editor opens and displays the existing mask, for example:

   ![](https://ww2.mathworks.cn/help/simulink/ug/blank_mask_editor5e80ef7c203bad5543526d8428b8232c.png)
3. Click **Delete Mask** in the of the Mask Editor.

   The Mask Editor removes the mask from the block.

## Related Topics

* [Create a Simple Mask](https://ww2.mathworks.cn/help/simulink/ug/how-to-mask-a-block.html)

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
