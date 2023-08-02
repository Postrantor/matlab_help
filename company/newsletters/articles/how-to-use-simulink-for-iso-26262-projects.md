---
tip: translate by openai@2023-07-19 18:02:28
url: https://www.mathworks.com/company/newsletters/articles/how-to-use-simulink-for-iso-26262-projects.html
title: How to Use Simulink for ISO 26262 Projects - MATLAB & Simulink --- 如何使用 Simulink 进行 ISO 26262 项目 - MATLAB & Simulink
date: 2023-07-19 18:00:47
tag:
summary: This article presents the TÜV SÜD approved workflow for using Simulink for ISO 26262. Topics covered ......
> 本文介绍了TÜV SÜD认可的使用Simulink进行ISO 26262的工作流程。涵盖的主题包括......
author: By Tom Erkkinen, MathWorks
---
Automotive engineers working on safety-related, embedded systems for traditional and autonomous vehicles are looking for efficient ways to achieve the process rigor imposed by ISO® 26262 [1], a functional safety standard for road vehicle development.

> 汽车工程师正在为传统和自动驾驶汽车的安全相关嵌入式系统寻找有效的方法来实现 ISO® 26262[1]规定的过程严格性，这是一个道路车辆开发的功能安全标准。

With all the media attention paid to autonomous vehicles, there is no lack of advice. Too often, though, this advice focuses on the latest coding method or bug-zapping tool. As industry experts have long recognized, safety is more about getting the system and its requirements right than about the software and how it was coded [2].

> 随着自动驾驶汽车受到媒体的关注，建议不缺乏。然而，这些建议往往集中在最新的编码方法或错误消除工具上。正如业内专家早已认识到的那样，安全更多的是关乎系统及其要求的正确性，而不是软件及其如何编码[2]。

With continuous- and discrete-time simulation as its backbone, Model-Based Design with Simulink® lets you design and test your full system under a wide range of driving conditions and fault scenarios long before you go to the proving grounds or perform fleet tests. It also supports process activities specified by ISO 26262, including tool qualification. IEC Certification Kit details this support and provides tool certificates and reports from the international certification authority TÜV SÜD.

> 使用 Simulink® 的基于模型的设计，其背后是连续和离散时间仿真，可以让您在各种驾驶条件和故障情况下设计和测试完整系统，而无需离开试验场或进行车队测试。它还支持 ISO 26262 规定的流程活动，包括工具资格认证。IEC 认证套件详细介绍了这种支持，并提供来自国际认证机构 TÜV SÜD 的工具证书和报告。

This article presents the TÜV SÜD–approved workflow for using Simulink for ISO 26262 projects. It introduces ISO 26262 and Model-Based Design and then covers the following activities:

> 这篇文章介绍了 TÜV SÜD 认可的使用 Simulink 进行 ISO 26262 项目的工作流程。它介绍了 ISO 26262 和基于模型的设计，然后涵盖以下活动：

- Requirements development
- Design modeling
- Design verification
- Code generation
- Code verification
- Tool qualification

## ISO 26262 and Model-Based Design

ISO 26262 includes guidance for manual design and code, as well as for Model-Based Design. In particular, it lists 5 different use cases of Model-Based Design [3]:

> ISO 26262 包括手动设计和代码的指导，以及基于模型的设计的指导。特别是，它列出了 5 种不同的基于模型的设计用例[3]：

- Specification of software safety requirements
- Developing of the software architecture design
- Design and implementation of software units, with or without code generation
- Design, implementation and integration of software components, including code generation from software component models
- Static and/or dynamic verification

It also acknowledges some of the benefits of using Model-Based Design [3]:

> 它也承认使用基于模型的设计[3]的一些好处：

_Beside the specific reasons for using MBD (e.g. simulation or code generation), an adequately defined syntax and semantics is a basis for the achievement of criteria such as comprehensibility, unambiguity, correctness, consistency and verifiability of information or work products described by models especially when different parties are collaborating._

> 除了使用 MBD 的特定原因（例如模拟或代码生成）外，适当定义的语法和语义是实现模型描述的可理解性、清晰性、正确性、一致性和可验证性等标准的基础，尤其是当不同的各方合作时。

The standard mentions graphical modeling and notes that modeling tools employ semi-formal methods for software development. It states that modeling not only captures the functionality to be realized (the embedded software), but also enables simulation of the real physical system (vehicle model and environment model) to produce a full system model:

> 标准提到图形建模，并指出建模工具采用半正式方法进行软件开发。它指出，建模不仅捕获要实现的功能（嵌入式软件），还能够模拟真实物理系统（车辆模型和环境模型）以产生完整的系统模型：

_Models, together with the modelling and coding guidelines, can be used to design, implement and integrate software at a higher level of abstraction._

> 模型可以与建模和编码准则一起使用，以更高的抽象层次来设计、实现和集成软件。

and,

_Models can also serve as a means to enable or support verification activities (e.g. plant model needed for the hardware-in-the-loop testing of closed-loop controls by simulating the environment of the device under test). The use of models for verification purposes may enable the early and efficient detection of faults/failures in work products (including software), more efficient test case generation, highly automated testing, or even formal verification techniques_

> 模型也可以用作启用或支持验证活动的手段（例如，硬件在环测试所需的电厂模型可以通过模拟测试设备的环境来模拟）。将模型用于验证目的可以使早期和有效地检测工作产品（包括软件）中的故障/失效、更有效地生成测试用例、高度自动化的测试或甚至形式化验证技术。

Figure 1 shows a typical Simulink closed-loop system model. It consists of a controller and plant, plus signal processors. In ISO 26262, the system requirements and architectural design specification constitutes an input to software development, but it is much more than that, as safety is fundamentally a systems issue [2].

> 图 1 显示了一个典型的 Simulink 闭环系统模型。它由控制器和工厂以及信号处理器组成。在 ISO 26262 中，系统要求和架构设计规范构成了软件开发的输入，但它比这更多，因为安全从根本上来说是一个系统问题[2]。

> [](https://www.mathworks.com/company/newsletters/articles/how-to-use-simulink-for-iso-26262-projects/_jcr_content/mainParsys/image.adapt.full.medium.jpg/1687758040940.jpg)
> Figure 1. A Simulink system design model.

Your system architectural elements allocated to software are typically elaborated until they become a software blueprint of sufficient detail for you to generate production code. ISO 26262 describes this model elaboration process as “model refinement” [2]:

> 你的系统架构元素分配给软件通常会被详细阐述，直到它们变成足够详细的软件蓝图，以便你生成生产代码。ISO 26262 将这一模型深化过程称为“模型细化”[2]。

_Models themselves may consist of various hierarchical refinement levels or references to refined models at a lower hierarchical level (e.g. hierarchical structure with black box and white box views for each model element.)_

> 模型本身可以由各种分层细化级别或对较低层次细化模型的引用组成（例如，具有每个模型元素的黑箱和白箱视图的分层结构）。

ISO 26262 recommends methods for various activities based on Automotive Safety Integrity Levels (ASILs). You use this guidance to establish an appropriate workflow based on your use cases. Figure 2 gives an overview of the ISO 26262 process. Solid arrows show development activities, while dashed arrows indicate verification and validation activities.

> ISO 26262 推荐基于汽车安全完整性级别（ASIL）的各种活动的方法。您可以使用此指南来根据您的用例建立适当的工作流程。图 2 概述了 ISO 26262 流程。实线箭头表示开发活动，而虚线箭头表示验证和验证活动。

> [](https://www.mathworks.com/company/newsletters/articles/how-to-use-simulink-for-iso-26262-projects/_jcr_content/mainParsys/image_1913024548.adapt.full.medium.svg/1687758040953.svg)
> Figure 2. ISO 26262 system and software development and verification processes using Simulink. While the article focuses on software development, you can use Model-Based Design with System Composer and Simulink for system level activities as shown at the left side of the workflow diagram.
> 图 2. 使用 Simulink 的 ISO 26262 系统和软件开发和验证流程。虽然本文重点讨论软件开发，但您也可以使用基于模型的设计与系统组件和 Simulink 进行系统级活动，如工作流程图左侧所示。

You start the safety-related development process by authoring functional and safety requirements. ISO 26262 recommends that you verify the software architecture design using “bi-directional traceability between the software architectural design and the software safety requirements.” To achieve this, you can use Requirements Toolbox™ to author and trace requirements to models, tests, and code. Requirements Toolbox supports bidirectional tracing for other tools, including Microsoft® Word®, Microsoft Excel®, and IBM® Rational® DOORS®. Further, you can [use Simulink and Requirements Toolbox to create formal and unambiguous requirement specifications.](https://www.mathworks.com/help/slrequirements/ug/use-requirements-table-block.html) The implementation and verification status of the requirements is monitored and managed within Requirements Toolbox. Requirement links can appear in the generated code (Figure 3).

> 您可以通过编写功能和安全要求来启动安全相关的开发流程。ISO 26262 建议您使用“双向追踪软件架构设计与软件安全要求之间的联系”来验证软件架构设计。为此，您可以使用 Requirements Toolbox™ 来编写和追踪要求到模型、测试和代码。Requirements Toolbox 支持其他工具的双向跟踪，包括 Microsoft® Word®、Microsoft Excel® 和 IBM® Rational® DOORS®。此外，您可以[使用 Simulink 和 Requirements Toolbox 来创建正式而清晰的要求规范]。([https://www.mathworks.com/help/slrequirements/ug/use-requirements-table-block.html](https://www.mathworks.com/help/slrequirements/ug/use-requirements-table-block.html))要求的实施和验证状态在 Requirements Toolbox 中进行监控和管理。要求链接可以出现在生成的代码中（图 3）。

> [](https://www.mathworks.com/company/newsletters/articles/how-to-use-simulink-for-iso-26262-projects/_jcr_content/mainParsys/image_1351358178.adapt.full.medium.jpg/1687758040972.jpg)
> Figure 3. Requirements specification in Simulink.

## Design Modeling

You can use Model-Based Design with Simulink and System Composer to specify your software architectural design. This includes the static (i.e., structure with block hierarchies and interface information) and dynamic aspects (i.e., behavior with functions, logic, data and control flow).

> 你可以使用基于模型的设计和 Simulink 系统组件来指定你的软件架构设计，这包括静态（即结构块层次结构和接口信息）和动态方面（即行为功能，逻辑，数据和控制流）。

With Simulink, you can also refine functional models of your software units and components from high-level executable specification to a detailed design ready for production code generation. Typical refinements include:

> 使用 Simulink，您还可以从高级可执行规范将软件单元和组件的功能模型细化为准备生产代码生成的详细设计。典型的细化包括：

- Converting blocks from continuous-time (S domain) to discrete-time (Z domain) using the Simulink Control Design™ discretization tool

> 使用 Simulink Control Design™ 离散化工具将连续时间（S 域）块转换为离散时间（Z 域）块

- Converting data from double precision to single precision or fixed point using Fixed-Point Designer™

> 使用 Fixed-Point Designer™ 将双精度数据转换为单精度或定点数据

- Adding diagnostics, mode logic, state machines, and scheduling using Stateflow®

For ASIL A through D, ISO 26262 highly recommends using modeling guidelines, and for this, you can use MAB Style Guidelines [4] and the high-integrity guidelines for ISO 26262 provided in Simulink. Simulink Check™ automates checking for both guidelines. It can flag issues, such as the insertion of noncompliant blocks, during edit-time. You can also include your own guidelines and checks that can also be configured as edit-time checks.

> 对于 ASIL A 至 D，ISO 26262 强烈建议使用建模指南，为此，您可以使用 MAB 样式指南[4]和 Simulink 中提供的 ISO 26262 的高完整性指南。Simulink Check™ 可以自动检查这两种指南。它可以在编辑时期标记问题，例如插入不符合要求的块。您还可以包括自己的指南和检查，这些检查也可以配置为编辑时检查。

## Design Verification

ISO 26262 recommends a number of static and dynamic methods for verifying software designs and implementations, including unit- and integration-level activities. For Model-Based Design, it states that “depending on the software development process the test objects can be the code derived from this model or the model itself, or both”

> ISO 26262 建议多种静态和动态方法来验证软件设计和实施，包括单元和集成级别的活动。对于基于模型的设计，它指出“根据软件开发过程，测试对象可以是从该模型派生的代码，也可以是该模型本身，或者两者兼而有之”。

Simulink Test™ provides a framework for ISO 26262 verification and validation activities within Simulink. You can use it for authoring, managing, and executing systematic, simulation-based tests for models and for code generated from models. Figure 4 shows an example test sequence and assessment blocks.

> Simulink Test™ 提供了一个框架，用于在 Simulink 中进行 ISO 26262 验证和验证活动。您可以使用它来为模型和从模型生成的代码编写、管理和执行系统性的、基于仿真的测试。图 4 显示了一个示例测试序列和评估块。

> [](https://www.mathworks.com/company/newsletters/articles/how-to-use-simulink-for-iso-26262-projects/_jcr_content/mainParsys/image_1086821787.adapt.full.medium.jpg/1687758040990.jpg)
> Figure 4. Simulink Test sequence and assessment blocks for modeling and authoring complex test scenarios.
> 图 4。 Simulink 测试序列和评估块，用于建模和编写复杂的测试场景。

The TÜV SÜD report in IEC Certification Kit (for ISO 26262) clarifies the role of Simulink Test in automating verification and validation:

> TÜV SÜD 在 IEC 认证工具（针对 ISO 26262）中的报告澄清了 Simulink Test 在自动化验证和验证中的作用：

_[Simulink Test] allows the automation of core verification and validation activities for Simulink models and generated code. The following use cases reflect activities that are required in a software development process according to the Functional Safety Standards ISO 26262:_

> Simulink 测试可以自动化 Simulink 模型和生成代码的核心验证和验证活动。根据功能安全标准 ISO 26262，以下用例反映了软件开发过程中所需的活动。

- _Development and execution of tests for Simulink models_
- _Development and execution of tests for back-to-back testing between model and code_
- _Assessment of test results_
- _Generation of test reports_
- _Identification of traceability between requirements and tests cases_

ISO 26262 recommends structural coverage analysis to determine test completeness and identify unintended functionality. It lists three methods for increasing rigor, with the last two being highly recommended for ASIL-D.

> ISO 26262 建议结构覆盖分析来确定测试的完整性并识别意外的功能。它列出了三种增加严谨性的方法，最后两种方法非常适合 ASIL-D。

- Statement coverage
- Branch coverage
- MC/DC (Modified Condition/Decision Coverage)

The standard notes that for Model-Based Design, “_the analysis of structural coverage can be performed at the model level using analogous structural coverage metrics for models.”_ It goes on to state that if the structural coverage is insufficient, “_additional test cases shall be specified or a rationale shall be provided.”_

> 标准指出，对于基于模型的设计，“可以使用类似的模型结构覆盖度指标在模型级别上执行结构覆盖度分析。”它进一步指出，如果结构覆盖度不足，“应指定附加的测试用例或提供合理的解释。”

Simulink Coverage™ provides structural coverage analysis for models and generated code, and it is easily enabled for test execution with Simulink Test. If your model coverage is insufficient, you can use Simulink Design Verifier™ to automatically generate additional test cases to achieve the required coverage, including MC/DC. Simulink Check includes the Model Testing Dashboard for assessing requirements-based test cases and results for completeness and quality metrics in accordance with ISO 26262-6:2018, Clause 9.4.3 and Clause 9.4.5 (Figure 5).

> Simulink Coverage™ 提供模型和生成代码的结构覆盖分析，并且可以很容易地与 Simulink Test 一起启用。如果您的模型覆盖范围不足，您可以使用 Simulink Design Verifier™ 自动生成额外的测试用例以实现所需的覆盖范围，包括 MC/DC。Simulink Check 包括 Model Testing Dashboard，用于根据 ISO 26262-6:2018，第 9.4.3 条和第 9.4.5 条（图 5）评估基于要求的测试用例和结果的完整性和质量指标。

> [](https://www.mathworks.com/company/newsletters/articles/how-to-use-simulink-for-iso-26262-projects/_jcr_content/mainParsys/image_1467346705.adapt.full.medium.jpg/1687758041005.jpg)
> Figure 5. Use the Model Testing Dashboard to assess the quality and completeness of your requirements-based testing activities in accordance with ISO 26262.
> 图 5。使用模型测试仪表板，根据 ISO 26262 评估您的基于要求的测试活动的质量和完整性。

IEC Certification Kit provides tool qualification support, with TÜV SÜD certificates and reports, for Simulink Check, Simulink Coverage (including model and code coverage), Simulink Design Verifier, and Simulink Test.

> IEC 认证套件提供工具资格认证支持，包括 TÜV SÜD 证书和报告，用于 Simulink Check，Simulink Coverage（包括模型和代码覆盖），Simulink Design Verifier 和 Simulink Test。

## Code Generation

ISO 26262 states that “the implementation of the software units includes the generation of source code and the translation into object code.” To achieve this, you can use Embedded Coder® to generate C, C++, and AUTOSAR code from your Simulink and System Composer models. The code can comply with MISRA C®:2012 automatic code guidelines [5]. ISO 26262 notes that code guidelines for Model-Based Design and manual code can differ and lists MISRA® as an example.

> ISO 26262 规定“软件单元的实现包括生成源代码和转换为目标代码。”为此，您可以使用嵌入式编码器 ® 从您的 Simulink 和 System Composer 模型生成 C、C++ 和 AUTOSAR 代码。该代码可以符合 MISRA C®：2012 自动代码指南[5]。ISO 26262 指出，基于模型的设计和手动代码的代码指南可能不同，并列出 MISRA® 作为示例。

IEC Certification Kit provides tool qualification support for Embedded Coder (including ASIL A through D) for C, C++, and AUTOSAR. Its TÜV SÜD certification report states:

> IEC 认证工具包为嵌入式编码器（包括 ASIL A 至 D）提供 C、C++ 和 AUTOSAR 的工具资格支持。其 TÜV SÜD 认证报告称：

_Embedded Coder fulfills the requirements of ISO 26262 regarding tool support and automation._

> _嵌入式编码器满足 ISO 26262 关于工具支持和自动化的要求。_

_The Embedded Coder is usually applied in one of the three following use cases:_

> 嵌入式编码器通常应用于以下三种用例之一：

1. _Generating C Code for the Model Used for Production Code Generation_
2. _Generating C++ Code for the Model Used for Production Code Generation_
3. _Generating C/C++ Code and Files for AUTOSAR Application Software Components from the Model Used for Production Code Generation_

Embedded Coder provides options for optimizing code for memory and speed. In addition, you can generate processor-specific optimizations that leverage hardware accelerators such as SIMD® for ARM® and Intel®. You can verify that the optimized code matches the simulation results within prescribed tolerances, using model-to-code, processor-in-the-loop (PIL) testing as described in ISO 26262.

> 嵌入式编码器提供了优化代码的内存和速度的选项。此外，您可以生成利用 ARM® 和 Intel® 等硬件加速器的特定处理器优化。您可以使用 ISO 26262 中描述的模型到代码、处理器在环路（PIL）测试来验证优化后的代码与模拟结果是否在规定的容差范围内匹配。

Executable object code is produced from the generated source code using your compiler and linker. The workflow in IEC Certification Kit allows coder, compiler, and processor optimizations, which are critical for mass production ECUs, as long as PIL testing is used to verify the executable object code.

> 可执行对象代码是使用编译器和链接器从生成的源代码生成的。IEC 认证工具中的工作流程允许编码器、编译器和处理器优化，这对于大规模生产的 ECU 来说至关重要，只要使用 PIL 测试来验证可执行对象代码即可。

## Code Verification

ISO 26262 provides several options for verifying software designs and implementations. With Model-Based Design, you can compare model coverage to code coverage using Simulink Coverage during software-in-the-loop (SIL) testing, or you can use Simulink Code Inspector™.

> ISO 26262 提供了几种验证软件设计和实现的选项。使用基于模型的设计，您可以在软件在环路（SIL）测试期间使用 Simulink Coverage 比较模型覆盖率和代码覆盖率，或者您可以使用 Simulink Code Inspector™。

Lastly, you can check MISRA compliance with Polyspace Bug Finder™. Use of MISRA checking and code coverage analysis is especially helpful if your project includes a mixture of generated and manually coded software. For added rigor, you can use Polyspace Code Prover™ to prove the absence of run-time errors such as divide-by-zero.

> 最后，您可以使用 Polyspace Bug Finder™ 来检查 MISRA 合规性。如果您的项目包括生成的和手动编码的软件的混合，则 MISRA 检查和代码覆盖分析尤其有用。为了增加严格性，您可以使用 Polyspace Code Prover™ 来证明没有运行时错误，例如除以零。

IEC Certification Kit provides tool qualification support, with TÜV SÜD certificates and reports, for Polyspace® products.

> IEC 认证工具包提供工具资格认证支持，并提供 TÜV SÜD 证书和报告，用于 Polyspace® 产品。

After compiling and generating the executable code, you can reuse model tests on the code executing on the target processor using PIL testing (Figure 6).

> 编译和生成可执行代码之后，您可以使用 PIL 测试（图 6）在目标处理器上执行代码时重复使用模型测试。

> [](https://www.mathworks.com/company/newsletters/articles/how-to-use-simulink-for-iso-26262-projects/_jcr_content/mainParsys/image_1751885421.adapt.full.medium.jpg/1687758041024.jpg)
> Figure 6. Example PIL for embedded processor.

ISO 26262 highly recommends back-to-back testing for ASILs C and D. It notes the importance of testing in a representative target hardware environment and stresses the need to be aware of differences between the test and hardware environments:

> ISO 26262 强烈建议对 ASILs C 和 D 进行双向测试。它指出在代表性目标硬件环境中进行测试的重要性，并强调需要注意测试和硬件环境之间的差异。

_Differences between the test environment and the target environment can arise in the source code or object code, for example, due to different bit widths of data words and address words of the processors._

> 测试环境和目标环境之间的差异可能会出现在源代码或目标代码中，例如，由于处理器的数据字和地址字的位宽不同。

But as every computer scientist should know [6], there are many sources for potential numeric differences across platforms, especially for floating-point data. Some begin as minor but accumulate and grow, especially in feedback control systems. Thus, ISO 26262 lists various in-the-loop methods for back-to-back testing:

> 但是，正如每个计算机科学家都应该知道的[6]，跨平台的潜在数值差异有很多来源，尤其是浮点数据。有些开始是微小的，但积累和增长，尤其是在反馈控制系统中。因此，ISO 26262 列出了各种反向测试的方法：

_Software unit testing can be executed in different environments, for example:_

> 软件单元测试可以在不同的环境中执行，例如：

_—model-in-the-loop tests;_
_—software-in-the-loop tests;_
_—processor-in-the-loop tests; and_
_—hardware-in-the-loop tests._

Simulink Test automates in-the-loop testing, including SIL and PIL with Embedded Coder and HIL with Simulink Real-Time™, and provides pass/fail reports with coverage metrics from Simulink Coverage.

> Simulink Test 自动化了在环测试，包括使用 Embedded Coder 的 SIL 和 PIL 以及使用 Simulink Real-Time™ 的 HIL，并提供来自 Simulink Coverage 的通过/失败报告和覆盖度指标。

## Tool Qualification

ISO 26262-8 describes additional supporting processes, including change control, configuration management, and documentation. These processes are supported by Simulink Projects, Simulink model differencing and merge, and Simulink Report Generator™, respectively.

> ISO 26262-8 描述了额外的支持流程，包括变更控制、配置管理和文档管理。这些流程由 Simulink Projects、Simulink 模型差异和合并、以及 Simulink 报告生成器 ™ 分别支持。

The standard also provides tool classification and qualification guidance. It requires users to qualify the tools for their specific projects. IEC Certification Kit effectively prequalifies tools by providing typical use cases, reference workflows, tool classification analysis, software tool documentation, tool qualification plans, and validation tests. If your project involves the supported use cases and follows the reference workflow, you can produce the required tool qualification artifacts by adapting the certification kit’s template documents and executing the validation tests in your development environment.

> 标准还提供工具分类和资格指导。它要求用户为其特定项目资格化工具。IEC 认证套件通过提供典型的使用场景、参考工作流程、工具分类分析、软件工具文档、工具资格计划和验证测试，有效地预先资格化工具。如果您的项目涉及支持的使用场景并遵循参考工作流程，您可以通过调整认证套件的模板文档并在开发环境中执行验证测试，生成所需的工具资格文件。

TÜV SÜD reviews and audits MathWorks® tool development and quality processes, as well as bug reporting capabilities and tool pre-qualification artifacts, and certifies the results at each product release. IEC Certification Kit includes these TÜV SÜD certificates and reports, which are predicated on the need to follow an appropriate verification and validation workflow. The kit provides reference workflows based on typical tool use cases, such as those highlighted in this article.

> TÜV SÜD 审查和审计 MathWorks® 工具开发和质量流程，以及错误报告功能和工具预资格资料，并在每次产品发布时对结果进行认证。IEC 认证套件包括这些 TÜV SÜD 证书和报告，这些证书和报告都是基于遵循适当的验证和验证工作流程的需要。该套件提供基于典型工具使用案例的参考工作流程，例如本文中所示的案例。

The kit provides more detailed information, including a mapping of ISO 26262 objectives to Simulink supporting capabilities (Figure 7).

> 该套件提供更详细的信息，包括将 ISO 26262 目标映射到 Simulink 支持功能的映射（图 7）。

> [](https://www.mathworks.com/company/newsletters/articles/how-to-use-simulink-for-iso-26262-projects/_jcr_content/mainParsys/image_1502599058.adapt.full.medium.jpg/1687758041042.jpg)
> Figure 7. Excerpt from ISO 26262–to-Simulink mapping in IEC Certification Kit.
> 图 7。从 ISO 26262 到 Simulink 映射的摘录，在 IEC 认证套件中。

ISO 26262:2018 acknowledges that Simulink and Stateflow are suitable for Software Architecture and Software Unit Design Notation and as a basis for automatic code generation (Figure 8).

> ISO 26262:2018 承认 Simulink 和 Stateflow 适用于软件架构和软件单元设计符号，并作为自动代码生成的基础（图 8）。

> [](https://www.mathworks.com/company/newsletters/articles/how-to-use-simulink-for-iso-26262-projects/_jcr_content/mainParsys/image_1502599058_cop.adapt.full.medium.jpg/1687758041057.jpg)
> Figure 8. Excerpt from ISO 26262-6:2018 showing suitable software design notations.
> 图 8。 ISO 26262-6:2018 的摘录，显示合适的软件设计符号。

Note that the use of qualified tools does not ensure the safety of the software or system under consideration.

> 注意，使用合格的工具并不能确保考虑中软件或系统的安全性。

### ISO 26262

ISO 26262 is an international functional safety standard for road vehicles [1]. It addresses possible hazards caused by malfunctioning, safety-related electronic/electrical (E/E) systems. It uses an Automotive Safety Integrity Level (ASIL) as a risk classification designation ranging from A to D, with ASIL D indicating the highest integrity level. The standard has nine normative parts, with guidelines in parts 10 and 11. Part 12 is an adaptation of parts 1-11 for motorcycles. Each part of the standard is provided as a separate document. ISO 26262 is goal-based and not prescriptive in nature, but it includes several hundred pages of guidance. Parts 4, 6, and 8 address systems [ISO 26262-4], software [ISO 26262-6], and tool qualification [ISO 26262-8], respectively.

> ISO 26262 是一项国际道路车辆安全标准[1]。它解决了由故障引起的可能危险，与安全相关的电子/电气（E/E）系统。它使用汽车安全完整性级别（ASIL）作为风险分类标记，从 A 到 D，其中 ASIL D 表示最高的完整性级别。该标准共有九个规范部分，其中第 10 和 11 部分提供指导。第 12 部分是第 1-11 部分的摩托车适配。标准的每一部分都提供为单独的文件。ISO 26262 是基于目标的，而不是规定性的，但它包含几百页的指导。部分 4、6 和 8 分别涉及系统[ISO 26262-4]、软件[ISO 26262-6]和工具资格[ISO 26262-8]。

Published 2022

### References

1. ISO 26262:2018 Road vehicles — Functional safety
2. ISO 26262-6:2018 Road vehicles — Functional safety —Part 6: Product development at the software level

### View Articles for Related Capabilities

### View Articles for Related Industries
