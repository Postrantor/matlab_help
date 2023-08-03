---
url: https://ww2.mathworks.cn/help/simulink/ug/call-simulink-functions-from-matlab-system-block.html
title: Call Simulink Functions from MATLAB System Block
- MATLAB & Simulink
- MathWorks 中国
date: 2023-08-03 11:27:08
tag: 
summary: Learn to call a Simulink function from a MATLAB System block.
---
## Call Simulink Functions from MATLAB System Block

A Simulink® function is a graphical representation of a computational unit in the Simulink environment. Once you create the Simulink function, it can be executed by any computational unit and can be called in multiple places. You can only call a Simulink function inside the `stepImpl`, `outputImpl`, or `updateImpl` method of a System object™. See [Simulink Functions Overview](https://ww2.mathworks.cn/help/simulink/ug/simulink-functions-overview.html) for more information on Simulink function.

**Note**

`Interpreted mode` is not supported for a model that contains a MATLAB System block calling a Simulink function.

### Create a Simulink Function Block

Set up a Simulink Function block that implements a simple function such as `y = 2 * u`.

1.  Open a new model from and add a Simulink Function block by typing Simulink function on Simulink canvas.
    
2.  When window opens, type `y = timestwo_func(u)` as the function definition. This indicates that you define a function called `timestwo_func` that takes `u` as the input argument and produces `y` as the output argument. Alternatively, define the function name from Simulink Function block parameters.
    
3.  Double-click the Simulink Function block and observe that a Trigger Port function block appears as well as an input and an output argument block.
    
4.  Double-click the Trigger Port block in the Simulink Function block, and observe that the **Function visibility** is set to `scoped`. To learn more about `scoped` Simulink functions, see [Scoped, Global, and Port-Scoped Simulink Function Blocks Overview](https://ww2.mathworks.cn/help/simulink/ug/scoped-and-global-simulink-functions.html).
    
5.  Add a Gain block and set its value to 2. Connect it with the input and output argument block. Click **Navigate Up to Parent** to return to the main model.
    

**Note**

To determine which Simulink function a MATLAB System block is calling, turn on the function tracing lines. In the **Debug** tab, select > .

### Create a MATLAB System Block and Define System Object

Drag a MATLAB System block into the model and implement a System object to this block.

1.  Add a MATLAB System block to your Simulink model.
    
2.  In the block dialog box, from the `New` list, select . This opens a basic System object template for you to type your code.
    
3.  To subclass an object from `matlab.System`, replace `Untitled` with the name of your System object. For this example, name it `SimulinkFcnCaller`.
    
4.  In the `stepImpl` method of your System object, declare the Simulink function using `getSimulinkFunctionNamesImpl`.
    
    See an example of the System object code below. To learn more about how to write a System object, see [Define Basic System Objects](https://ww2.mathworks.cn/help/matlab/matlab_prog/define-basic-system-objects-1.html).
    
    ```
    classdef SimulinkFcnCaller < matlab.System
        % Public, tunable properties
        
        % SimulinkFcnCaller calls a Simulink function from a  
        % MATLAB System block to multiply the signal's value by 2.
    
        methods(Access = protected)
            
            function y = stepImpl(obj,u)
                % Implement algorithm. Calculate y as a function of input u and
                % discrete states.
                y = timestwo_func(u);
            end       
            function names = getSimulinkFunctionNamesImpl(obj)
                % Use 'getSimulinkFunctionNamesImpl' method to declare  
                % the name of the Simulink function that will be called  
                % from the MATLAB System block's System object code.
                names = {'timestwo_func'};
            end
        end
    end
    
    ```
    
5.  Save the file and name it `SimulinkFcnCaller.m`.
    

### Call a Simulink Function in a Subsystem from a MATLAB System Block

The hierarchy of the Simulink Function block affects the function calls in the System object. For example, if the Simulink function is defined at a higher hierarchy in the Simulink model, the function is defined for all blocks in that hierarchy. If the Simulink function is defined at a lower hierarchy, you need to qualify the Subsystem and the function name. For example, suppose you have a Subsystem that contains a Simulink Function block at the same level as a MATLAB System block. When a Simulink Function block is placed in a subsystem, the function name is not visible to the outside the subsystem. You can call the Simulink Function block by qualifying the function name using the Subsystem name in your System object. To qualify the Subsystem and the function name follow these steps:

1.  In the `stepImpl` method of your System object code, call the Simulink function using dot notation. For example, in the code `y = Subsystem1.timestwo_func(u)`, `Subsystem1` corresponds to the Subsystem, and `timestwo_func` corresponds to the Simulink function name.
    
2.  Similarly, declare the Subsystem and the Simulink function in the `getSimulinkFunctionNamesImpl` method using the dot notation. The System object code shows the `timestwo` example written for a Simulink function defined at a lower hierarchy than the MATLAB System block.
    
    ```
    classdef SimulinkFcnCallerQualified < matlab.System 
        
        % SimulinkFcnCallerQualified calls a Simulink function embedded in a Subsystem  
        % from a MATLAB System block, and multiplies the signal's value by 2.
        
        methods(Access = protected)
    
            function y = stepImpl(obj,u)
                % Use the '.' notation to call a scoped Simulink function from
                % a Simulink Function block.
                % Subsystem1 corresponds to  the block name, where 
                % timestwo_funct is the Simulink function name.
                y = Subsystem1.timestwo_func(u);
            end
    
            function names = getSimulinkFunctionNamesImpl(obj)
                % Use the 'getSimulinkFunctionNamesImpl' method with the '.' 
                % notation to declare the name of a Simulink function in  
                % MATLAB System block's System object code.
                names = {'Subsystem1.timestwo_func'};
            end
        end
    end
    
    ```
    
3.  Save the System object file and name it `SimulinkFcnCallerQualified.m`.
    

### Call Simulink Functions from a MATLAB System Block

This example shows two Simulink® Functions conditionally called by a MATLAB System block using the nontunable properties of the System object®.

The MATLAB System block calls one of the Simulink functions inside two different subsystems, depending on the value of the signal coming from the Sine Wave block. If the value of the signal is less than 10, the MATLAB System block calls the `timestwo_func` Simulink Function inside the `SS1` subsystem. If the value is larger than 10, it calls the `timesthree_func` in the `SS2` subsystem.

Function names are defined as nontunable properties, are switched from string to functions using the `str2func` function. Then, these functions are declared as properties in the `getSimulinkFunctionNamesImpl` method.

![](https://ww2.mathworks.cn/help/examples/simulink/win64/CallSimulinkFunctionsFromMATLABSystemBlockExample_01.png)

## See Also

[`getSimulinkFunctionNamesImpl`](https://ww2.mathworks.cn/help/simulink/slref/matlab.system.getsimulinkfunctionnamesimpl.html)

## Related Topics

*   [Define Basic System Objects](https://ww2.mathworks.cn/help/matlab/matlab_prog/define-basic-system-objects-1.html)
*   [Use a MATLAB Function Block to Call a Simulink Function Block](https://ww2.mathworks.cn/help/simulink/ug/simulink-function-callers.html#mw_e7d7bf91-3df9-4cd3-8d29-365c4d19d7df)
*   [Use a Function Caller Block to Call a Simulink Function Block](https://ww2.mathworks.cn/help/simulink/ug/simulink-function-callers.html#mw_b88de221-b298-416f-91c6-aaceeddbe49d)