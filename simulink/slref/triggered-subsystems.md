---
url: https://www.mathworks.com/help/simulink/slref/triggered-subsystems.html
title: Triggered Subsystems - MATLAB & Simulink --- 触发子系统 - MATLAB & Simulink
date: 2023-07-10 09:29:46
tag: 
summary: Model showing triggered subsystems and different types of triggers.
---
This model shows triggered subsystems and describes the different trigger types. A triggered subsystem is executed for a single time step whenever the trigger port transitions in a specified direction. The transition of the trigger port may occur when the direction of the transition is rising, falling, or either rising or falling. A rising transition occurs when the trigger signal transitions from zero or below to a positive value. A falling transition occurs when the trigger signal transitions from zero or above to a negative value.

After running the simulation, look closely at the top plot in the scope. This shows a sine wave, a waveform, and the grounded value of zero. As the waveform transitions through zero, each of the subsystems is triggered appropriately. The output of each subsystem is equal to the current value of the sine wave at that time.

![](https://www.mathworks.com/help/examples/simulink_features/win64/TriggeredSubsystemsExample_01.png)

![](https://www.mathworks.com/help/examples/simulink_features/win64/TriggeredSubsystemsExample_02.png)

You have a modified version of this example. Do you want to open this example with your edits?

How useful was this information?

Unrated

1 star

2 stars

3 stars

4 stars

5 stars

Unrated  1 star  2 stars  3 stars  4 stars  5 stars

You clicked a link that corresponds to this MATLAB command:

Run the command by entering it in the MATLAB Command Window. Web browsers do not support MATLAB commands.
