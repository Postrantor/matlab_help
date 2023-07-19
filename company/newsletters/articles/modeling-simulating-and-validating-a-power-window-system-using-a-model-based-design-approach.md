---
tip: translate by openai@2023-07-18 14:21:44
url: https://ww2.mathworks.cn/company/newsletters/articles/modeling-simulating-and-validating-a-power-window-system-using-a-model-based-design-approach.html?s_tid=srchtitle_openExample%2528%2527driving_stateflow%252FAutomatedDriverControlInStateflowExample%2527%2529_1
title: Modeling, Simulating, and Validating a Power Window System Using a Model-Based Design Approach - MATLAB & Simulink
> 使用基于模型的设计方法建模、仿真和验证电动窗系统
date: 2023-07-18 14:20:20
tag:
summary: The need to bring innovative, high quality products to market faster is driving the increased use of ......
> 更快地将创新、高质量的产品推向市场的需求正在推动越来越多地使用......
author: By Sameer M. Prabhu, MathWorks and Pieter J. Mosterman, MathWorks
---
The need to bring innovative, high quality products to market faster is driving the increased use of models during the design and realization process in industry. Model-based design facilitates an advanced approach to product development, which aids in delivering products on time and within budget while meeting initial requirements. In addition, advances in model-based design tools, which allow automatic generation of prototype and production code from a model, significantly shorten the development path. This paper applies the model-based design process to the design of a power window control system that is typically found in modern automobiles and considers various aspects of the validation process via testing, both during simulation and physical realization.

> 需要更快地将创新的高质量产品投放市场，正在推动工业在设计和实现过程中越来越多地使用模型。**基于模型的设计提供了一种先进的产品开发方法，有助于按时、按预算完成初始要求**。此外，基于模型的设计工具的进步，**可以从模型自动生成原型和生产代码**，大大缩短了开发路径。本文将基于模型的设计过程应用于通常可以在现代汽车中找到的电动窗控制系统的设计，并通过模拟和物理实现的测试考虑验证过程的各个方面。

Given competitive temporal and cost constraints, developing a product on time and within budget requires a systematic approach to its design and realization. It not only ensures that the final product meets the initial requirements but it also allows engineering teams with different specializations to work together and to communicate between stages in the overall process. In addition, it also forces the design process as well as the final product to be documented for maintenance and future development.

> 在有竞争的时间和成本限制的情况下，按时在预算内开发产品需要对其设计和实现采取系统的方法。它不仅确保最终产品满足最初的要求，而且还使具有不同专业的工程团队能够共同工作并在整个过程的各个阶段进行沟通。此外，它还迫使设计过程以及最终产品被记录以便维护和未来的发展。

The systematic design and realization process in the aerospace and automotive industry is typically represented by a V diagram as shown in Figure 1 (for examples, see [3,4]). The two branches of the V correspond to two distinctly different activities:

> 在航空航天和汽车工业中，**系统设计和实现过程通常用 V 图表示**，如图 1 所示(例如参见[3,4])。V 图的两个分支对应两种不同的活动：

- The left branch captures the decomposition of the initial system requirements into subsystems and components that are specified and implemented at an increasingly detailed level
- The right branch represents the realization of these subsystems and components and their integration

> - 左分支捕获了最初系统要求的**分解**，这些要求以越来越详细的级别被指定和实施。
> - 右分支代表了这些子系统和组件的实现以及它们的**集成**。

In the traditional approach, strict boundaries between design activities are enforced and documents are passed back and forth between the respective teams of engineers. This has the following drawbacks:

> 在传统的方法中，严格地限定设计活动，并在相应的工程师团队之间来回传递文件。这有以下缺点：

- Though rigorous, documents can be unwieldy and in an unsuitable form to record functionality.
- It is difficult to keep the documentation synchronized with the present state of the design.
- The separation between different design stages requires coding to be a separate and manual activity.

> 分离不同设计阶段需要将编码作为一项独立的手动活动。

- Using documents as deliverables leads to a duplication of effort when information is needed in an electronic format. It is difficult to trace the source of errors along a paper trail.

> 使用文件作为交付物会在需要电子格式的信息时导致重复努力。沿着纸质路径很难追踪错误的来源。

The need to address the shortcomings listed above and to address the continuing increase in product complexity, performance requirements, and reduced product development times has driven the increased use of models as part of the design and realization process. The use of models in the early design stages allows ‘ executable specifications’, i.e., the specifications can be validated and verified against the requirements immediately. Validation focuses on ensuring that the requirements are correct and that they represent the intended behavior. Verification focuses on ensuring that the outputs of each step satisfy the step’s inputs, i.e., it ensures that the system satisfies its requirements. Less formally, verification checks whether the model is built correctly and validation whether the correct model is built. By introducing the verification and validation activities in the early design stages, model-based design allows errors to be detected earlier when the cost to fix them is less.

> 鉴于上述缺陷的需要以及产品复杂性、性能要求和产品开发时间的不断增加，模型的使用越来越多地被用于设计和实现过程中。在早期设计阶段使用模型可以实现“可执行规范”，即规范可以立即根据要求进行验证和验证。验证的重点是确保要求是正确的，并且代表预期的行为。验证的重点是确保每个步骤的输出满足步骤的输入，即确保系统满足其要求。不太正式地说，验证检查模型是否正确构建，而验证则检查是否构建了正确的模型。通过在早期设计阶段引入验证和验证活动，基于模型的设计可以更早地检测出错误，从而降低修复错误的成本。

![](https://ww2.mathworks.cn/company/newsletters/articles/modeling-simulating-and-validating-a-power-window-system-using-a-model-based-design-approach/_jcr_content/mainParsys/image_0.adapt.full.medium.jpg/1469941374413.jpg)

Figure 1: A V diagram of the system design and realization process

Further down the design process, models can be used to communicate between engineering teams with different specializations allowing them to work together during in the overall process. Moreover, initial design models can be incrementally extended to include increasing implementation detail. Thus, model-based design allows experimenting with different design alternatives, even in very early conceptual design stages, while having an executable specification and taking detailed implementation effects into account. This is, in contrast to a document-centered approach, where each of the design stages generates new models of the same system under design from the specification of the previous design stage.

> 在设计过程的更深处，模型可用于在具有不同专业的工程团队之间进行沟通，以便在整个过程中共同工作。此外，初始设计模型可以逐步扩展，以包括越来越多的实施细节。因此，基于模型的设计允许在非常早期的概念设计阶段就对不同的设计方案进行实验，同时具有可执行的规范，并考虑到详细的实施效果。这与以文档为中心的方法形成了对比，在这种方法中，每个设计阶段都会根据前一个设计阶段的规范生成同一系统的新模型。

Even more sophisticated is the use of model transformation to generate different representations of the same system, which further reduces the effort to move from one design stage to another. In particular, the use of automatic code generation technology and hardware-in-the-loop testing alleviates errors introduced during manual implementation and realization tasks and shortens the path to product delivery by generating code for testing, calibration, and the final production. An important benefit of this model-based design paradigm is the traceability of design decisions all the way down to the implementation. So, test results can be directly interpreted as high-level design decisions. Finally, even though electronic models are easier to navigate than paper documents, the formal system design process still requires detailed documentation. Advanced tools allow automatic generation of this documentation from a model while model-based design forces the design process and the final product to be documented for maintenance and future developments.

> 更复杂的是使用模型转换来生成同一系统的不同表示，进一步减少从一个设计阶段到另一个设计阶段的努力。特别是，使用自动代码生成技术和硬件在环测试减轻了手动实现和实现任务中引入的错误，并通过生成用于测试、校准和最终生产的代码来缩短产品交付的路径。这种基于模型的设计范式的一个重要优点是设计决策可以直接追溯到实施。因此，测试结果可以直接解释为高级设计决策。最后，尽管电子模型比纸质文档更容易导航，但是正式的系统设计过程仍然需要详细的文档。先进的工具允许从模型自动生成此文档，而基于模型的设计迫使设计过程和最终产品被记录以供维护和未来发展。

This paper applies the model-based design process to the design of a power window control system (shown in Figure 2) as typically found in modern automobiles, and the verification and validation of the developed models through real-time implementation. This verification and validation process covers both testing during simulation and testing on the real system to tune the model to approximate the behavior of the real system. The MATLAB®-Simulink® environment [6,7] is used throughout the design process since it provides high-level formalisms such as SimMechanics [9] and SimPowerSystems [10] to support detailed modeling of the window system, the plant. Similarly, high-level formalisms such as Stateflow® [8] allow intuitive and elegant modeling of intricate control behavior, such as fixed-point effects. This unprecedented level of detail brings the design process much closer to the realization before committing to an implementation, uncovers incompatibilities (such as a different system of units for quantities) while the system is still in its electronic form and can be modified easily. Further, experimenting with different design alternatives is made possible, even in very early conceptual design stages, while having an executable specification and taking detailed implementation effects into account. Finally, automation shortens the path to product delivery by generating code for testing and calibration as well as the final production code.

> 本文应用基于模型的设计流程，对现代汽车中常见的电动窗控制系统(见图 2)进行设计，并通过实时实现进行模型的验证和验证。此验证和验证过程涵盖了模拟测试和实际系统测试，以调整模型以接近实际系统的行为。整个设计过程使用 MATLAB®-Simulink® 环境[6,7]，因为它提供了高级形式，如 SimMechanics[9]和 SimPowerSystems[10]，以支持窗系统和工厂的详细建模。同样，高级形式，如 Stateflow®[8]，可以直观而优雅地建模复杂的控制行为，例如固定点效应。这种前所未有的细节水平使设计过程更接近实现，在提交实施之前发现不兼容性(例如不同的量的单位系统)，而系统仍处于电子形式，可以轻松修改。此外，即使在非常早期的概念设计阶段，也可以尝试不同的设计选择，同时具有可执行的规范，并考虑到详细的实施效果。最后，自动化可以通过生成测试和校准以及最终生产代码的代码来缩短产品交付的路径。

![](https://ww2.mathworks.cn/company/newsletters/articles/modeling-simulating-and-validating-a-power-window-system-using-a-model-based-design-approach/_jcr_content/mainParsys/image_1.adapt.full.medium.jpg/1469941377280.jpg)

Figure 2: A typical automobile power window

The paper is organized as follows: Section 2 discusses the behavioral modeling of a power window control system, and presents simulation results that demonstrate concept feasibility. The detailed software design aspects including model based testing, requirements management, source control management, and documentation for the power window control system are covered in Section 3. Section 4 focuses on production code generation and embedded system integration. The results of hardware validation are presented in Section 5, and the conclusions are outlined in Section 6.

> 本文的组织结构如下：第 2 节讨论了电动窗控制系统的行为建模，并呈现了模拟结果以证明概念可行性。第 3 节涵盖了电动窗控制系统的详细软件设计方面，包括基于模型的测试、需求管理、源代码控制管理和文档。第 4 节专注于生产代码生成和嵌入式系统集成。第 5 节介绍了硬件验证的结果，第 6 节总结了结论。

### 2. Behavioral Modeling of a Power Window Control System

A typical power window system is designed to meet various requirements. For the system under consideration the following requirements drive the design process:

> 典型的电动窗系统旨在满足各种要求。对于本系统考虑，以下要求驱动设计过程：

- The window has to start moving within 200 [ms] after the command is issued.
- The window has to be fully opened and fully closed within 4 [s].
- The force to detect when an object is present should be less than 100 [N].
- If the up or down command is issued for at least 200 [ms] and at most 1 [s], the window has to be fully opened or closed, respectively.

> 如果上或下指令发出时间在 200 毫秒到 1 秒之间，窗口就必须完全打开或关闭。

- When an object is present, the window should be lowered by approximately 10 [cm].
- The driver command has priority over the passenger command.

Given the initial requirements listed above, the discrete event core control algorithm can be elegantly modeled by exploiting the hierarchical state behavior of Stateflow (e.g., priority of driver commands over any of the passenger commands), as shown in Figure 3. The behavior of the discrete event control algorithm can be verified by submitting it to a variety of inputs that correspond to driver and passenger commands, in order to verify that the requirements are correctly captured in the Stateflow diagram. The animation capability of Stateflow provides visual feedback regarding the functioning of the control algorithm.

> 根据上述初始要求，可以通过利用 Stateflow 的分层状态行为(例如，司机命令优先于乘客命令)优雅地对离散事件核心控制算法进行建模，如图 3 所示。可以通过提交各种对应于司机和乘客命令的输入来验证离散事件控制算法的行为，以确认要求是否正确地捕获在 Stateflow 图中。Stateflow 的动画功能可以提供有关控制算法运行的视觉反馈。

![](https://ww2.mathworks.cn/company/newsletters/articles/modeling-simulating-and-validating-a-power-window-system-using-a-model-based-design-approach/_jcr_content/mainParsys/image_2.adapt.full.medium.jpg/1469941373329.jpg)

Figure 3: Discrete event control algorithm model

In order to study the continuous-time behavior (e.g., the 10 [cm] bounce-back in case an obstacle is detected), the Stateflow control algorithm can then be connected to a second order plant model in Simulink, as shown in Figure 4. This second order plant model allows calibrating the parameter that governs the downward movement of the power window to ensure that the 10 [cm] bounce-back requirement is met.

> 为了研究连续时间行为(例如，在检测到障碍物时 10 厘米的反弹)，可以将 Stateflow 控制算法连接到 Simulink 中的二阶工厂模型，如图 4 所示。该二阶工厂模型可以校准控制电动窗下降运动的参数，以确保满足 10 厘米的反弹要求。

![](https://ww2.mathworks.cn/company/newsletters/articles/modeling-simulating-and-validating-a-power-window-system-using-a-model-based-design-approach/_jcr_content/mainParsys/image_3.adapt.full.medium.jpg/1469941374452.jpg)

Figure 4: Simple second order power window plant model

Other requirements, such as the maximum force of 100 [N] on an obstacle, necessitate a more detailed plant model. This is facilitated by using modeling formalisms based on capturing the physics in terms of energy flow, such as SimPowerSystems, for the electrical and SimMechanics for the mechanical part These formalisms are based on modeling the energy exchange between physical components, and, therefore, the modeler does not have to perform the tedious analysis of the underlying signal flow in the physical model so it can be implemented in Simulink.

> 其他要求，例如障碍物的最大力 100 [N]，需要更详细的工厂模型。这可以通过使用基于捕获物理中能量流的建模形式来实现，例如 SimPowerSystems 用于电气部分，SimMechanics 用于机械部分。这些形式基于建模物理组件之间的能量交换，因此，建模者不必对物理模型中的基础信号流进行繁琐的分析，以便在 Simulink 中实现。

For the power window, the direct-current (DC) motor and the electrical circuit driving the power window can be modeled using the SimPowerSystems blockset. The motion of the DC motor can be connected to a model of the scissors mechanism (which moves the window glass up and down) that is built using rigid bodies, joints, and other components from the the SimMechanics blockset. Finally, the physical layout and geometry of the power window mechanism can be visualized by the Virtual Reality Toolbox [11] (e.g., to study the rotation direction of the worm gear). This integrates computer aided design (CAD) with the controller development as many CAD tools can export the virtual reality modeling language (VRML) format used by the Virtual Reality Toolbox. Figure 5 shows the various elements of the power window model.

> 对于电动窗户，可以使用 SimPowerSystems 模块集来建模直流(DC)电机和驱动电动窗户的电路。 DC 电机的运动可以连接到使用从 SimMechanics 模块集构建的剪刀机构(用于上下移动窗玻璃)的模型。最后，可以通过虚拟现实工具箱[11]可视化电动窗机构的物理布局和几何形状(例如，研究蜗轮齿轮的旋转方向)。这将计算机辅助设计(CAD)与控制器开发集成在一起，因为许多 CAD 工具可以导出虚拟现实建模语言(VRML)格式，该格式由虚拟现实工具箱使用。图 5 显示了电动窗模型的各个元素。

![](https://ww2.mathworks.cn/company/newsletters/articles/modeling-simulating-and-validating-a-power-window-system-using-a-model-based-design-approach/_jcr_content/mainParsys/image_4.adapt.full.medium.jpg/1469941376873.jpg)

Figure 5: Power window electrical and mechanical system model and 3-D visualization

> 图 5：电动车窗电气和机械系统模型及 3D 可视化

After the detailed model is constructed it can be used to run simulations of the discrete control algorithm interacting with the electro-mechanical plant model and thereby verify that the behavior of the control algorithm approximates the desired behavior as specified in the requirements. This involves subjecting the model to various test cases approximating the driver and passenger commands, and verifying that the system outputs such as window position and force exerted on the obstacle etc. are within the limits outlined in the requirements. The control commands can be observed in the same environment to ensure that control response requirements are met as well. Simulation results for a particular test case (after 1 [s], the window is commanded to go up automatically and an obstacle is present) are shown in Figure 6.

> 当详细模型构建完成后，它可以用于运行离散控制算法与电动机械装置模型的模拟，从而验证控制算法的行为是否接近要求中指定的期望行为。这涉及将模型暴露于各种模拟驱动程序和乘客命令的测试用例，并验证系统输出(如窗户位置和施加于障碍物的力等)是否在要求范围内。同样的环境中也可以观察控制命令，以确保满足控制响应要求。图 6 显示了特定测试用例(1 秒后，窗户被指令自动上升，并且存在障碍物)的模拟结果。

![](https://ww2.mathworks.cn/company/newsletters/articles/modeling-simulating-and-validating-a-power-window-system-using-a-model-based-design-approach/_jcr_content/mainParsys/image_5.adapt.full.medium.jpg/1469941374863.jpg)

Figure 6: Simulation results

During all of these design and implementation stages the controller specification is in an executable form and allows validation against the specific requirements under investigation. Modeling and simulation plays a critical role in ensuring that the requirements are valid and in determining if any of the requirements conflict. Simulation is thus a key validation step since it ensures that a system can be realized such that it satisfies the requirements. In addition, simulation can form the basis for multi-objective parameter synthesis methods for robust control design.

> 在所有这些设计和实施阶段，控制器规范以可执行形式存在，并允许针对正在研究的特定要求进行验证。建模和仿真在确保要求有效并确定是否存在任何冲突的要求方面发挥着关键作用。因此，仿真是一个关键的验证步骤，因为它确保系统可以实现，以满足要求。此外，仿真可以为健壮控制设计的多目标参数综合方法提供基础。

Because models of physical systems are approximate in nature, it is important to note that the validation steps carried out so far are limited by the fidelity of the plant model. One way this shortcoming can be mitigated is by rapid-prototyping the control system with a real physical system instead of the approximate plant model. This lets you determine whether the control algorithm can ensure that the performance of the real system meets the requirements. Thus, it provides an estimate of the accuracy of the models and also validates the requirements.

> 由于物理系统的模型是近似的，因此重要的是要注意，到目前为止所进行的验证步骤受到工厂模型的保真度的限制。缓解这一缺点的一种方法是通过快速原型化控制系统，而不是近似的工厂模型，来进行实际物理系统的控制。这样可以确定控制算法是否能确保实际系统的性能符合要求。因此，它可以估计模型的准确性，并验证要求。

A typical approach to rapid prototyping that is practiced in industry is the use of a powerful, general purpose computer with flexible input and output hardware capability as the controller. Real-Time Workshop® [12] and xPC Target [14] provides this rapid-prototyping capability by automatically generating the control model in the form of C code that runs on a real-time operating system. The real-time general purpose computer is an xPC TargetBox™ that is capable of running the real-time application generated by xPC Target. This configuration is used to interactively test and calibrate controller parameters such as armature current thresholds for obstacle detection. Because automatic code generation allows a seamless transition between model and realization, even control structure changes can be quickly experimented with at this point.

> 在工业中常用的快速原型开发方法是使用具有灵活的输入和输出硬件能力的强大通用计算机作为控制器。Real-Time Workshop® [12]和 xPC Target [14]通过自动生成以实时操作系统运行的 C 代码形式的控制模型，提供了这种快速原型开发能力。实时通用计算机是一个 xPC TargetBox™，能够运行 xPC Target 生成的实时应用程序。这种配置用于交互式测试和校准控制器参数，如电枢电流阈值用于检测障碍物。由于自动代码生成可以在模型和实现之间实现无缝过渡，因此即使是控制结构的更改也可以在此时快速尝试。

### 3. Detailed Software Design for the Power Window Control System

As mentioned before, recent advances in code generation enable very efficient code to be synthesized directly from models that were used for control system specification, development, and verification and validation. The software engineers accomplish this by adding software-specific detail to the existing model. When automatically generated code is used without modification the models serve as the final implementation.

> 随着代码生成技术的进步，可以直接从用于控制系统规范、开发和验证和验证的模型中合成非常有效的代码。软件工程师通过向现有模型添加软件特定的细节来完成这一点。当使用未经修改的自动生成的代码时，模型就成为最终实现。

A variety of tools and techniques are used throughout this key step in the design process. Requirements capture and traceability tools are used to associate the requirements with the implementation, so that when the requirements change their effect on the design can be evaluated. After software is generated, it is subjected to testing and code coverage tools are used to evaluate the completeness of the testing process. Code coverage analysis involves dynamically analyzing the way the code executes and then reporting on measures such as statement coverage, decision coverage, condition coverage, and modified condition/decision coverage. Model-based coverage involves analyzing the model execution behavior and then reporting on the decision coverage, condition coverage, and modified condition/decision coverage metrics [2]. The basic goal of model-based coverage is to provide the equivalent information of code coverage in the context of the model under simulation. The entire process of creating test vectors, generating expected outputs based on requirements, and coverage analysis in the context of a model is referred to as model-based testing.

> 各种工具和技术在设计过程中的这一关键步骤中被使用。要求捕获和可追溯性工具用于将要求与实施相关联，以便当要求发生变化时，可以评估其对设计的影响。软件生成后，将经过测试，并使用代码覆盖工具来评估测试过程的完整性。代码覆盖分析涉及动态分析代码执行的方式，然后报告语句覆盖率、决策覆盖率、条件覆盖率和修改条件/决策覆盖率等指标。基于模型的覆盖率涉及分析模型执行行为，然后报告决策覆盖率、条件覆盖率和修改条件/决策覆盖率指标[2]。基于模型的覆盖率的基本目标是在仿真的模型上下文中提供等同于代码覆盖率的信息。创建测试向量、基于要求生成预期输出以及在模型上下文中进行覆盖分析的整个过程称为基于模型的测试。

For the power window control system, the various test cases were manually generated and incorporated into the model using the SignalBuilder component of Simulink, as shown in Figure 7. This allows all the test cases to be defined and facilitates running the test cases either individually or as a test suite in order to obtain model coverage metrics.

> 对于电动窗控制系统，各种测试用例是手动生成的，并使用 Simulink 的 SignalBuilder 组件纳入模型，如图 7 所示。这样可以定义所有测试用例，并便于单独运行测试用例或作为测试套件以获得模型覆盖度度量。

![](https://ww2.mathworks.cn/company/newsletters/articles/modeling-simulating-and-validating-a-power-window-system-using-a-model-based-design-approach/_jcr_content/mainParsys/image_6.adapt.full.medium.jpg/1469941373811.jpg)

Figure 7: Test vectors for power window model

To associate the model with the system requirements, the Requirements Management Interface [16] is used to allow each requirement to be tied to the appropriate hierarchical level of the model where that requirement is realized.

> 为了将模型与系统要求联系起来，使用要求管理接口[16]允许将每个要求与模型的适当层次相关联，以实现该要求。

After the requirements have been associated with the model they can be used to derive the expected behavior of the model, which in turn can be used to create self-validating models by using the blocks from the Model Verification library in Simulink, as shown in Figure 8. When the results of the simulation do not meet the requirements, the blocks from the model verification library can be set-up to stop the simulation and report on the requirement being violated. Combining the SignalBuilder with the Model Verification blocks allows an automated approach to subjecting the model to various test conditions and ensuring that all requirements are being met.

> 当需求与模型关联后，它们可以用来推导模型的预期行为，进而可以通过在 Simulink 中使用模型验证库中的块来创建自我验证的模型，如图 8 所示。当仿真结果不符合要求时，可以设置模型验证库中的块来停止仿真并报告被违反的要求。将 SignalBuilder 与 Model Verification 块结合起来，可以实现自动化的方法，将模型提交给各种测试条件，并确保满足所有要求。

![](https://ww2.mathworks.cn/company/newsletters/articles/modeling-simulating-and-validating-a-power-window-system-using-a-model-based-design-approach/_jcr_content/mainParsys/image_7.adapt.full.medium.jpg/1469941375886.jpg)

Figure 8: Power window verification model

As mentioned before, it is necessary to assess the completeness of the testing, so as to ensure that the model meets all the requirements in various operating modes. This is accomplished by using the model coverage tools in Simulink that assess the cumulative results of a test suite to determine which blocks were not executed or which states were not reached. A coverage analysis report is generated after a simulation run, as shown in Figure 9.

> 在之前提到，有必要评估测试的完整性，以确保模型满足各种操作模式的所有要求。这可以通过使用 Simulink 中的模型覆盖工具来实现，该工具可以评估测试套件的累计结果，以确定哪些块未执行或哪些状态未达到。模拟运行后，将生成覆盖分析报告，如图 9 所示。

![](https://ww2.mathworks.cn/company/newsletters/articles/modeling-simulating-and-validating-a-power-window-system-using-a-model-based-design-approach/_jcr_content/mainParsys/image_8.adapt.full.medium.jpg/1469941376120.jpg)

Figure 9: Coverage analysis report

When the model-based testing process is complete, the model is ready for deployment. Frequently this is when the model is documented in order to capture the various design decisions, and report on simulation results that demonstrate the verification and validation work. This can be accomplished in an automated fashion by using the MATLAB and Simulink Report Generator [17]. These tools together with the DocBlock component of Simulink are used to create a self-contained report regarding the requirements, models, test cases and simulation results, as shown in Figure 10.

> 当基于模型的测试过程完成后，模型就可以部署了。通常这时候会记录模型，以捕获各种设计决策，并报告模拟结果，以证明验证和验证工作。这可以通过使用 MATLAB 和 Simulink 报告生成器[17]以自动方式完成。这些工具与 Simulink 的 DocBlock 组件一起用于创建有关要求、模型、测试用例和模拟结果的自包含报告，如图 10 所示。

![](https://ww2.mathworks.cn/company/newsletters/articles/modeling-simulating-and-validating-a-power-window-system-using-a-model-based-design-approach/_jcr_content/mainParsys/image_9.adapt.full.medium.jpg/1469941376581.jpg)

Figure 10: Model documentation report

Software processes emphasize the notion of Software Configuration Management (SCM) [3] to store, version and retrieve the various developmental stages of software, so that changes to the software are carefully controlled. Software engineers check-out software before making changes, and then check-in software after making changes so that they may be merged with other changes. The same concept can be applied to the model-based design environment by the SCM Interface that is available for Simulink as shown below in Figure 11.

> 软件过程强调软件配置管理(SCM)[3]的概念，以存储、版本和检索软件的各个开发阶段，以便对软件进行精细控制。软件工程师在进行更改前检出软件，然后在进行更改后检入软件，以便将其与其他更改合并。同样的概念可以通过 Simulink 中可用的 SCM 接口应用于基于模型的设计环境，如下图 11 所示。

![](https://ww2.mathworks.cn/company/newsletters/articles/modeling-simulating-and-validating-a-power-window-system-using-a-model-based-design-approach/_jcr_content/mainParsys/image_10.adapt.full.medium.jpg/1469941375355.jpg)

Figure 11: Model source configuration management

### 4. Production Code Generation and Embedded Systems Integration

After the model has been verified, validated, and documented, code can be generated from the model for implementation purposes. The production code is automatically generated by the Real-Time Workshop Embedded Coder [13] either in fixed-point or floating-point format. If fixed-point code is desired, the block diagram specification can be enhanced to include the fixed-point parameter settings. Thus, the same model can be refined further for software engineering purpose. Since, the Motorola MPC555® processor was selected as the processor on which to implement the power window control algorithm, The Embedded Target for Motorola MPC555® [18] is used to customize the generated code to run on an MPC555 processor and target the I/O devices on the processor to achieve real-time control of the actual window. An important consideration in generating code from a model is traceability between code and model. This is accomplished by an HTML report, created as part of the code-generation process, that hyperlinks code back to the portion of the model that corresponds to the code as shown in Figure 12.

> 在模型验证、验证和文档化之后，可以从模型生成代码以用于实施目的。实时工作室嵌入式编码器[13]可以自动生成固定点或浮点格式的生产代码。如果需要固定点代码，可以增强块图规范，以包括固定点参数设置。因此，同一个模型可以进一步细化以用于软件工程目的。由于选择摩托罗拉 MPC555® 处理器来实现电动窗控制算法，因此使用嵌入式目标摩托罗拉 MPC555®[18]来定制生成的代码，以在 MPC555 处理器上运行，并将处理器上的 I/O 设备目标化以实现实时控制实际窗口。从模型生成代码时的一个重要考虑是代码和模型之间的可追溯性。这可以通过代码生成过程中创建的 HTML 报告来实现，该报告将代码超链接回与代码对应的模型部分，如图 12 所示。

![](https://ww2.mathworks.cn/company/newsletters/articles/modeling-simulating-and-validating-a-power-window-system-using-a-model-based-design-approach/_jcr_content/mainParsys/image_11.adapt.full.medium.jpg/1469941376980.jpg)

Figure 12: Traceability between generated code and model

When the generated code is specific to a particular processor and the associated compiler-debugger toolchain, the code-generation report can be enhanced with measurements of ROM/RAM usage. These measurements can be used as a guide to further optimize the generated code using various optimization settings and user configuration options that are provided by the Real-Time Workshop Embedded Coder and the Embedded Target for Motorola MPC555. Further, the task of interactively testing and calibrating controller parameters such as armature current thresholds, using commercial controller area network (CAN) [5] Calibration Protocol (CCP) based tools is facilitated by an ASAP2 file that is created during the code-generation process.

> 当生成的代码专门针对特定处理器和相关的编译器调试工具链时，代码生成报告可以通过 ROM/RAM 使用情况的测量来增强。这些测量结果可以用作进一步优化生成的代码的指南，使用 Real-Time Workshop Embedded Coder 和 MPC555 的 Embedded Target 提供的各种优化设置和用户配置选项。此外，使用商业控制器区域网络(CAN)[5]校准协议(CCP)基于工具交互式测试和校准控制器参数(如电枢电流阈值)的任务，可以通过在代码生成过程中创建的 ASAP2 文件来实现。

The generated code is implemented on a Phytec MPC555 evaluation board, and the power window system is controlled using the digital output and PWM ports on the MPC555. An H-Bridge is used to convert the digital and PWM outputs from the MPC555 to the high voltage, current and direction reversal capabilities required for driving the power window DC motor. The switches and the current feedback for obstacle detection are read using the analog-to-digital converter blocks on the MPC555. The code for these blocks is automatically generated using the corresponding I/O blocks from the Embedded Target for Motorola MPC555 library.

> 代码在 Phytec MPC555 评估板上实现，使用 MPC555 上的数字输出和 PWM 端口控制电动窗系统。H 型桥用于将 MPC555 的数字和 PWM 输出转换为驱动电动窗直流电机所需的高电压、电流和方向反转能力。使用 MPC555 上的模拟转数字转换器块读取开关和电流反馈以检测障碍物。这些块的代码使用 Motorola MPC555 库中的相应 I/O 块自动生成。

### 5. Verification and Validation of the Power Window Control System

When the design has been physically realized on the target embedded system, the Data Acquisition Toolbox [19] is used to measure window position and force to verify that the physical system behavior meets the initial requirements. The Data Acquisition Toolbox can also be used in earlier stages of the design process to acquire data for calibrating the plant model at different levels of detail, as required by the design of the control algorithm. Figure 13 shows a plot of the position response of the power window in the presence of an obstacle, obtained from a position sensor added to the system for data-acquisition purposes. It can be seen that the power window bounces back approximately 10 [cm] in the presence of an obstacle, and thereby the design meets the bounce back requirement outlined earlier.

> 当设计已经在目标嵌入式系统上实现后，使用数据采集工具箱[19]来测量窗口位置和力，以验证物理系统行为是否符合最初的要求。数据采集工具箱也可以在设计过程的早期阶段用来获取用于根据控制算法的设计校准植物模型的不同细节水平的数据。图 13 显示了在存在障碍物的情况下，从用于数据采集目的添加到系统中的位置传感器获得的动力窗口位置响应的绘图。可以看出，动力窗口在存在障碍物的情况下反弹大约 10 厘米，从而设计满足了先前提出的反弹要求。

![](https://ww2.mathworks.cn/company/newsletters/articles/modeling-simulating-and-validating-a-power-window-system-using-a-model-based-design-approach/_jcr_content/mainParsys/image_12.adapt.full.medium.jpg/1469941375652.jpg)

Figure 13: Position response of power window

Figure 14 shows a plot of the force exerted on the obstacle, obtained via a load cell added to the system for data acquisition purposes. As can be seen, the force generated on the obstacle is well within the 100 [N] limit specified in the requirements. The other requirements can be verified to be satisfied as well and so the developed model is fully realized in real-time, and meets the requirements for the system.

> 图 14 显示了通过添加到系统中的负载单元获得的施加在障碍物上的力的图示。可以看出，在障碍物上产生的力量完全在要求中指定的 100 [N]限制内。其他要求也可以被验证满足，因此开发的模型可以实时实现，并满足系统的要求。

![](https://ww2.mathworks.cn/company/newsletters/articles/modeling-simulating-and-validating-a-power-window-system-using-a-model-based-design-approach/_jcr_content/mainParsys/image_13.adapt.full.medium.jpg/1469941375174.jpg)

Figure 14: Force response of power window

This paper illustrates how the entire embedded control systems development process from conceptualization to implementation can be realized using a model-based design approach in combination with an integrated tools-suite. Using the model-based design approach results in designs that are consistent with requirements and allows verification and validation throughout the development process. This results in early detection of errors when the cost to correct them is less than at later stages of the development process. Further, automatic generation of code avoids the errors associated with manual implementation approaches. The same model can be used and refined for a variety of different tasks carried out in a typical development process. The net result is improved efficiency of the development process, which translates into faster time-to-market, reduced development costs, and higher quality.

> 这篇论文说明了如何使用基于模型的设计方法结合一套集成的工具套件，从概念到实施实现整个嵌入式控制系统的开发过程。使用基于模型的设计方法可以产生符合要求的设计，并允许在整个开发过程中进行验证和验证。这样可以在成本更低的后期开发阶段发现错误。此外，自动生成代码可以避免与手动实施方法相关的错误。同一个模型可以用于并且可以用于典型开发过程中的各种不同任务的改进。最终的结果是提高了开发过程的效率，这可以转化为更快的上市时间、降低开发成本和更高的质量。

This paper was presented at the Society for Experimental Machines IMAC Conference on January 26-29, 2004 in Dearborn, Michigan.

> 这篇论文于 2004 年 1 月 26 日至 29 日在密歇根州迪尔伯恩举行的实验机器学会 IMAC 会议上发表。

Published 2003

### References

2. Aldrich, B., “Using model coverage analysis to improve the controls development process,” AIAA 2002.

> Aldrich B.，“利用模型覆盖分析改善控制开发过程”，AIAA 2002。

3. Dahlqvist, A, U. Asklund, I. Crnkovic, A. Hedin, M. Larsson, J. Ranby and D. Svensson, “Product Data Management and Software Configuration Management—Similarities and Differences,” The Association of Swedish Engineering Industries, September, 2001

> 黛尔奎斯特、阿斯克隆德、克尔诺维奇、海丁、拉尔森、兰比和斯文森，“产品数据管理与软件配置管理——相似点和差异”，瑞典工业协会，2001 年 9 月。

4. Mosterman, P.J., J. Sztipanovits, and S. Engell, “Computer Automated Multi-Paradigm Modeling in Control Systems Technology,” IEEE Transactions on Control System Technology, 2003.

> Mosterman, P.J., J. Sztipanovits 和 S. Engell，“控制系统技术中的计算机自动多范式建模”，IEEE 控制系统技术杂志，2003 年。

5. Müller-Glaser, K.D., G. Frick, E. Sax, and M. Kühl, “Multi-Paradigm Modeling in Embedded Systems Design,” IEEE Transactions on Control System Technology, 2003.

> 5. Müller-Glaser、K.D.、G. Frick、E. Sax 和 M. Kühl，《嵌入式系统设计中的多范式建模》，IEEE 控制系统技术杂志，2003 年。

6. Robert Bosch GmbH, “CAN Specification,” Technical Report, Robert Bosch GmbH, Postfach 30 02 40, D-70442, Stuttgart, Germany, 1991

> 罗伯特·博世有限公司，“CAN 规范”，技术报告，罗伯特·博世有限公司，邮政编码 30 02 40，D-70442，德国斯图加特，1991 年。

7. The MathWorks Inc., “Using MATLAB,” Version 6.5, The MathWorks Inc., Natick, MA, August, 2002.

> The MathWorks Inc.，《使用 MATLAB》，版本 6.5，The MathWorks Inc.，Natick，MA，2002 年 8 月。

8. The MathWorks Inc., “Using Simulink,” Version 5.0.2, The MathWorks Inc., Natick, MA, April, 2003.

> 《使用 Simulink》，The MathWorks Inc.，版本 5.0.2，The MathWorks Inc.，Natick，MA，2003 年 4 月。

9. The MathWorks Inc., “Stateflow and Stateflow Coder”, User's Guide, Version 5.0, The MathWorks Inc., Natick, MA, July, 2002.

> 《The MathWorks Inc.，“Stateflow 和 Stateflow Coder”，用户指南，版本 5.0，The MathWorks Inc.，Natick，MA，2002 年 7 月》

10. The MathWorks Inc., “SimMechanics”, User’s Guide, Version 2.0, The MathWorks Inc., Natick, MA, November, 2002.

> 10. MathWorks Inc.，《SimMechanics》用户指南，版本 2.0，MathWorks Inc.，Natick，MA，2002 年 11 月。

11. The MathWorks Inc., “SimPowerSystems”, User’s Guide, Version 2.3, The MathWorks Inc., Natick, MA, July, 2002.

> 11. The MathWorks Inc.，《SimPowerSystems》用户指南，版本 2.3，The MathWorks Inc.，Natick，MA，2002 年 7 月。

12. The MathWorks Inc., “Virtual Reality Toolbox”, User’s Guide, Version 3.1, The MathWorks Inc., Natick, MA, October, 2002.

> 12. MathWorks Inc.，《虚拟现实工具箱》，用户指南，版本 3.1，MathWorks Inc.，Natick，MA，2002 年 10 月。

13. The MathWorks Inc., “Real-Time Workshop”, User's Guide, Version 5.0, The MathWorks Inc., Natick, MA, July, 2002.

> 13. MathWorks Inc.，《实时工作室》用户指南，版本 5.0，MathWorks Inc.，Natick，MA，2002 年 7 月。

14. The MathWorks Inc., “Real-Time Workshop Embedded Coder”, User's Guide, Version 3.0, The MathWorks Inc., Natick, MA, July, 2002.

> 14. MathWorks Inc.，《实时工作室嵌入式编码器》用户指南，版本 3.0，MathWorks Inc.，Natick，MA，2002 年 7 月。

15. The MathWorks Inc., “xPC Target”, User's Guide, Version 2, The MathWorks Inc., Natick, MA, July, 2002.

> 15. MathWorks Inc.，《xPC Target》用户指南，第 2 版，MathWorks Inc.，Natick，MA，2002 年 7 月。

16. The MathWorks Inc., “Requirements Management Interface”, User's Guide, Version 1.04, The MathWorks Inc., Natick, MA, July, 2002.

> 16. The MathWorks Inc.，《需求管理界面》用户指南，版本 1.04，The MathWorks Inc.，Natick，MA，2002 年 7 月。

17. The MathWorks Inc., “Report Generator”, User's Guide, Version 1.2, The MathWorks Inc., Natick, MA, July, 2002.

> 17. The MathWorks Inc.，《报告生成器》用户指南，版本 1.2，The MathWorks Inc.，Natick，MA，2002 年 7 月。

18. The MathWorks Inc., “Embedded Target for Motorola MPC555”, User’s Guide, Version 1.0.1, The MathWorks Inc., Natick, MA, July, 2002.

> 18. MathWorks Inc.，《Motorola MPC555 嵌入式目标》用户指南，版本 1.0.1，MathWorks Inc.，Natick，MA，2002 年 7 月。

19. The MathWorks Inc., “Data Acquisition Toolbox”, User’s Guide, Version 2.2, The MathWorks Inc., Natick, MA, July, 2002.

> MathWorks Inc.，《数据采集工具箱》，用户指南，版本 2.2，MathWorks Inc.，Natick，MA，2002 年 7 月。
