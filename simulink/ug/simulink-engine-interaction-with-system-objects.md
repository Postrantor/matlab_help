---
tip: translate by openai@2023-08-03 10:12:00
url: https://ww2.mathworks.cn/help/simulink/ug/simulink-engine-interaction-with-system-objects.html
title: Simulink Engine Interaction with System Object Methods
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-03 10:09:19
tag: 
summary: Follow a process view of the order in which the MATLAB System block invokes System object methods wit......
> 总结：遵循MATLAB系统块调用系统对象方法的顺序的过程视图。。。。。。
---

## Simulink Engine Interaction with System Object Methods

### Simulink Engine Phases Mapped to System Object Methods

This diagram shows a process view of the order in which the MATLAB System block invokes System object™ methods within the context of the Simulink® engine.

> 此图显示了 MATLAB 系统块调用系统对象的顺序的过程视图 ™ 方法。

![](https://ww2.mathworks.cn/help/simulink/ug/matlab_system_slengine_objs.png)

Note the following:

- Simulink calls the `stepImpl`, `outputImpl`, and `updateImpl` methods multiple times during simulation at each time step. Simulink typically calls other methods once per simulation.
- The Simulink engine calls the `isOutputFixedSizeImpl`, `getDiscreteStateSpecificationImpl`, `isOutputComplexImpl`, `getOutputDataTypeImpl`, `getOutputSizeImpl` when using propagation methods.
- Simulink calls `saveObjectImpl` and `loadObjectImpl` for saving and restoring the model operating point, the Simulation Stepper, and Fast Restart.
- Default implementations save and restore all properties with public access, including `DiscreteState`.

> - Simulink 在每个时间步长的模拟过程中多次调用`stepImpl`、`outputImpl`和`updateImpl`方法。Simulink 通常在每次模拟中调用其他方法一次。
> - Simulink 引擎在使用传播方法时调用`isOutputFixedSizeImpl`、`getDiscreteStateSpecificationImpl`、`isOutputComplexImpl`、`getOutputDataTypeImpl`和`getOututSizeImpl`。
> - Simulink 调用 `saveObjectImpl` 和 `loadObjectImpl` 来保存和恢复模型操作点、Simulation Stepper 和 Fast Restart。
> - 默认实现使用公共访问保存和恢复所有属性，包括`DiscreteState`。

## See Also

[MATLAB System](https://ww2.mathworks.cn/help/simulink/slref/matlabsystem.html)

## Related Examples

- [Implement a MATLAB System Block](https://ww2.mathworks.cn/help/simulink/ug/create-matlab-system-block.html)
- [Change Blocks Implemented with System Objects](https://ww2.mathworks.cn/help/simulink/ug/change-system-object-assigned-to-block.html)
- [Change Block Icon and Port Labels](https://ww2.mathworks.cn/help/simulink/ug/change-block-icon-and-port-labels.html)
- [Add and Implement Propagation Methods](https://ww2.mathworks.cn/help/simulink/ug/propagate-mixins.html)
- [Use System Objects in Feedback Loops](https://ww2.mathworks.cn/help/simulink/ug/nondirect-feedthrough.html)
- [Troubleshoot System Objects in Simulink](https://ww2.mathworks.cn/help/simulink/ug/troubleshoot-system-objects-in-simulink.html)

## More About

- [Customize System Objects for Simulink](https://ww2.mathworks.cn/help/simulink/define-system-object-for-simulink.html)
- [Mapping System Object Code to MATLAB System Block Dialog Box](https://ww2.mathworks.cn/help/simulink/ug/integrate-system-object-in-model.html)
- [Simulation Modes](https://ww2.mathworks.cn/help/simulink/ug/simulation-modes.html)
- [Nonvirtual Buses and MATLAB System Block](https://ww2.mathworks.cn/help/simulink/ug/nonvirtual-buses-and-matlab-system-block.html)
- [Considerations for Using System Objects in Simulink](https://ww2.mathworks.cn/help/simulink/ug/system-objects-in-matlab-system-block.html)
- [Comparison of Custom Block Functionality](https://ww2.mathworks.cn/help/simulink/ug/comparison-of-custom-block-functionality.html)
