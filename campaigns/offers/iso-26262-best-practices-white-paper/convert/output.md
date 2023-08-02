---
tip: translate by openai@2023-07-19 11:22:38
url: [11 Best Practices for Developing ISO 26262 Applications with Simulink - MATLAB & Simulink](https://www.mathworks.com/campaigns/offers/iso-26262-functional-safety-best-practices.html?s_tid=vid_pers_ofr_recs)
> 11 最佳实践，用 Simulink 开发 ISO 26262 应用 - MATLAB 和 Simulink
...

# 11 Best Practices for Developing ISO 26262 Applications with Simulink

The use of Model-Based Design in the automotive market has expanded throughout the last few decades. One main use case has been to use MATLAB® and Simulink® to develop and deploy algorithms for automotive embedded appli- cations. Engineers have used Model-Based Design to develop applications such as engine controllers, transmission controllers, body controllers, and more recently, autonomous driving and advanced driver-assistance systems. As automotive embedded applications evolve to rely less on driver input, the embedded systems that control the vehicle will increasingly need to meet functional safety requirements.

> 在过去几十年中，**基于模型的设计在汽车市场的使用已经扩大。一个主要的用例是使用 MATLAB® 和 Simulink® 来开发和部署汽车嵌入式应用程序的算法**。工程师们使用基于模型的设计来开发诸如发动机控制器、变速箱控制器、车身控制器等应用程序，最近还有自动驾驶和先进的驾驶员辅助系统。随着汽车嵌入式应用程序越来越依赖驾驶员输入，控制车辆的嵌入式系统将越来越需要满足功能安全要求。

To increase the likelihood that functional safety requirements for these systems are met, standards have been devel- oped to guide engineers through various aspects of the development cycle. One standard that has gained traction in the automotive functional safety space is ISO 26262. ISO 26262 uses a top-down workflow and breaks down various aspects of the development cycle including system-, hardware-, and software-level guidelines to achieve functional safety objectives.

> 为了增加这些系统实现功能安全要求的可能性，已经开发了标准来指导工程师完成各个阶段的开发。在汽车功能安全领域获得广泛应用的标准之一是 ISO 26262。**ISO 26262 采用自上而下的工作流程**，将开发周期的各个方面划分为系统、硬件和软件级别的指导原则，以实现功能安全目标。

IEC Certification Kit can be used as a guide to compliance with the portions of the ISO 26262 standard that are applicable to Model-Based Design. The kit provides a high-level workflow that you can reference and extend as needed. One thing that you will need to carefully consider is the modeling style and architecture that you will use when constructing algorithms. By adopting a modeling style and architecture that are conducive to ISO 26262, you can greatly reduce the work needed to meet key aspects of the standard such as freedom from interference. This white paper details a variety of modeling best practices that you can use to form a modeling architecture that com- plements the referenced workflow in IEC Certification Kit. MathWorks Consulting Services has uncovered these best practices during consulting engagements with various automotive companies.

> IEC 认证工具包可用作参考，以符合 ISO 26262 标准中适用于基于模型设计的部分。该工具包提供了一个高级工作流，您可以根据需要参考和扩展。您需要仔细考虑的一件事是构建算法时使用的建模风格和体系结构。通过采用符合 ISO 26262 的建模风格和体系结构，可以大大减少满足标准关键方面(如免受干扰)所需的工作。本白皮书详细介绍了各种建模最佳实践，您可以使用这些最佳实践形成与 IEC 认证工具包中引用的工作流程相协调的建模体系结构。MathWorks 咨询服务在与各种汽车公司的咨询服务中发现了这些最佳实践。

# Why ISO 26262?

The ISO 26262 standard is used to guide development organizations through the functional safety aspects of electri- cal and/or electronic systems. Some of the key aspects that ISO 26262 tries to address are:

> ISO 26262 标准用于指导开发组织实现电子/电子系统的功能安全方面。ISO 26262 试图解决的一些关键方面是：

- The development process stages necessary for functional safety
- Guidance on the automotive safety life cycle
- Functional safety decomposition for systems engineering, hardware engineering, and software engineering
- Methods for using Automotive Safety Integrity Level (ASIL) standards to determine safety requirements for acceptable risk levels
- Guidance on acceptance criteria for validation activities based on ASIL

> - 功能安全所需的开发过程阶段
> - 汽车安全生命周期的指导
> - 功能安全分解，用于系统工程、硬件工程和软件工程
> - 使用汽车安全完整性级别(ASIL)标准确定可接受风险水平的安全要求的方法

Development platforms for Model-Based Design such as MATLAB and Simulink enable you to create deployable algorithms for a variety of embedded systems. Simulink also enables you to verify those algorithms early and often in the development cycle. The IEC Certification Kit reference workflow uses these capabilities to provide a compre- hensive workflow that you can use to create testable unit models, integration models, and system-level models. This high-level workflow is broken down into two sections as shown in Figures 1 and 2.

> 开发基于模型的设计平台，如 MATLAB 和 Simulink，可以为各种嵌入式系统创建可部署的算法。 Simulink 还可以在开发周期的早期和经常验证这些算法。 IEC 认证套件参考工作流程使用这些功能，提供一个全面的工作流程，可用于创建可测试的单元模型、集成模型和系统级模型。如图 1 和图 2 所示，这个高级工作流程被分解为两个部分。

> ![](./media/image1.jpeg){width="6.17371062992126in" height="2.41125in"}
> _Figure 1. Development phase 1: Model-Based Design._

> ![](./media/image2.jpeg){width="6.2283333333333335in" height="1.9116655730533683in"}
> _Figure 2. Development phase 2: Embedded software testing._

IEC Certification Kit stops short of providing suggestions on how a model architecture should be constructed. The general workflows shown above demonstrate at a high level the development and verification stages necessary for an algorithm. The amount of a work and the ease of following these steps can vary greatly depending on how an algo- rithm is broken down during the development and verification process. It is crucial to first think through these design choices for the algorithm's architecture because they can have a substantial impact on the efficiency of the development organization, reusability of the software, and testability of the software.

> IEC 认证工具包未能提供有关如何构建模型架构的建议。上面展示的一般工作流程高度概括了算法的开发和验证阶段所必需的步骤。工作量和遵循这些步骤的容易程度可能会因算法在开发和验证过程中的分解方式而有很大不同。首先要仔细思考算法架构的设计选择，因为它们可能会对开发组织的效率、软件的可重用性和软件的可测试性产生重大影响。

# Best Practices for ISO 26262 Development with Model-Based Design

This white paper provides suggestions on modeling practices that you can use to segment algorithms to reduce veri- fication and deployment efforts when adhering to ISO 26262 and using Model-Based Design. These best practices can be classified into the following categories:

> 这份白皮书提供了有关建模实践的建议，您可以使用这些实践来分割算法，以减少在遵守 ISO 26262 并使用基于模型的设计时的验证和部署工作。 这些最佳实践可分为以下几类：

- Model architecture:

1.  Use model reference for unit-level models
2.  Pick a strategy for grouping units into features
3.  Split ASIL and QM levels at the top level of the model
4.  Eliminate algorithmic content at the integration level
5.  Use model metrics to monitor unit complexity

- Signal routing and definition:

6.  Group bus signals by ASIL, feature, and rate
7.  Pass only necessary signals to units
8.  Optimize placement of signal and parameter objects
9.  Protect data exchanged between ASILs

- Code generation configuration:

10. Determine a code placement strategy
11. Use different name tokens for shared utilities

Please note that these best practices were designed to work together. Deviation from best practices while still achiev- ing the overall objectives is possible, but not recommended.

> 请注意，这些最佳实践是设计用来协同工作的。虽然仍然可以实现整体目标，但不推荐偏离最佳实践。

# Model Architecture

One of the first decisions when you are developing algorithms in Simulink involves the general model architecture. This is an important first step because the decisions made at this stage impact:

> 在 Simulink 中开发算法时，其中**一个首要决定是一般模型架构**。这是一个重要的第一步，因为此阶段做出的决定会影响：

- Software testability
- Software reusability
- Unit and integration testing methods
- Ease of software integration
- Software segmentation for freedom from interference

These best practices, created by MathWorks consultants working with engineers who are targeting ISO 26262 com- pliance, will help you optimize these traits. The best practices center around areas that make ISO 26262 compliance easier while increasing efficiency in the validation, verification, and documentation phases.

> 这些最佳实践由 MathWorks 顾问创建，他们与针对 ISO 26262 合规性的工程师合作，将有助于您优化这些特征。最佳实践集中在使 ISO 26262 合规性更容易，同时提高验证、验证和文档阶段效率的领域。

## Use Model Reference for Unit-Level Models

One of the main focal points in part 6 of ISO 26262 is the workflow for developing, validating, and verifying soft- ware units. Software units are intended to be the smallest testable parts of an application that must be individually tested for proper operation. Requirements will be mapped to and/or within these individual software units, and requirements-based test cases will be built to exhaustively test the software unit. Therefore, the modeling construct used for unit development needs to consider various aspects, including:

> 六部分的 ISO 26262 的主要焦点之一是开发、验证和验证软件单元的工作流程。软件单元旨在成为应用程序的最小可测试部分，必须单独测试才能正常运行。要求将映射到和/或这些单独的软件单元中，并且将建立基于要求的测试用例来彻底测试软件单元。因此，用于单元开发的建模构造需要考虑各种方面，包括：

- Unit testability
- Unit code generation
- Unit complexity
- Unit testing workflow
- Unit traceability
- Unit documentation

These characteristics point to relying on model references as the primary modeling pattern for unit development. Model references have multiple characteristics that are conducive to the desired outcome, including:

> 这些特征表明，将模型引用作为单元开发的主要建模模式是可行的。模型引用具有多个有利于预期结果的特征，包括：

- Code generation -- There is a one-to-one mapping from model reference to source files generated.
- Reusability -- Model references can be used in multiple places throughout an integration model.
- Testability -- Model references are ideal for test harness construction.
- Team collaboration -- Model references enable parallel development across developers and simplify the overall configuration management and version control processes.

> 团队协作 -- 模型参考可以实现开发人员之间的并行开发，简化整体配置管理和版本控制过程。

Figure 3 shows how you can use model references to split functionality into testable units.

> 图 3 显示了如何使用模型引用将功能分解为可测试的单元。

> ![](./media/image3.jpeg){width="4.053325678040245in" height="3.578333333333333in"}
> _Figure 3. Using model reference for algorithmic units._

Although this approach is not recommended as a best practice, atomic subsystems in Simulink libraries can serve as software units. For this approach, developers need to explicitly manage and control the boundaries for partitioning and testing. For example, interfaces need to be locked down, meaning the user needs to specify the data types, sample times, and related block settings including code generation options such as function prototype control and file partitioning. Additional care and user assessment is required when placing an atomic subsystem in different exe- cution contexts such as single rate, multirate, or multitasking. In Release 2019a, Simulink introduced library-based code generation for reusable subsystem libraries to facilitate such use of atomic subsystems. This feature offers new capabilities for better management, specification, generation, test, and reuse of atomic subsystems as standalone units.

> 尽管不推荐使用此方法作为最佳实践，但 Simulink 库中的原子子系统可以用作软件单元。对于此方法，开发人员需要明确管理和控制划分和测试的边界。例如，需要锁定接口，即用户需要指定数据类型、采样时间以及相关块设置，包括代码生成选项，如函数原型控制和文件划分。在将原子子系统放置在单速、多速或多任务等不同执行上下文中时，需要额外的照顾和用户评估。在 2019a 版本中，Simulink 引入了基于库的代码生成，用于可重用的子系统库，以便于这种使用原子子系统的方式。此功能提供了新的功能，可以更好地管理、指定、生成、测试和重用原子子系统作为独立的单元。

You can also use subsystems as containers to further support functional grouping of similar units. Nested model ref- erence is generally not recommended because it adds another layer of complexity in model and data management (see next best practice for more detail).

> 你也可以使用子系统作为容器来进一步支持类似单元的功能分组。一般不建议使用嵌套模型参考，因为它会增加模型和数据管理的另一层复杂性(详见下一个最佳实践)。

## Pick a Strategy for Grouping Units into Features

When building large-scale models, you can choose from various types of modeling constructs to add model hierar- chy, such as:

> 在构建大规模模型时，您可以从各种类型的建模构造中选择，以**增加模型层次结构**，例如：

- Virtual subsystems
- Atomic subsystems
- Model references
- Library blocks

For ISO 26262, there is flexibility on how these unit models can be grouped under their respective ASIL. The previ- ous best practice added one "hard" constraint, in that model reference should be used for the unit-level algorithm. However, grouping of units can be subsystems or even model reference. Some of the tradeoffs to consider are the number of model references that need to be managed and how firm a modeling boundary you want at a feature level. These units could be grouped into virtual subsystems if the architecture is indifferent on how the model is segment- ed and the user base is able to absorb working on a larger model. When you are determining a feature model seg- mentation strategy, consider:

> 对于 ISO 26262，可以灵活地将这些单元模型分组到各自的 ASIL 下。以前的最佳实践增加了一个“硬”约束，即应该在单元级算法中使用模型参考。但是，单元的分组可以是子系统甚至是模型参考。需要考虑的一些权衡因素是需要管理的模型参考数量以及在功能级别上你想要多坚实的建模边界。如果架构对模型如何分割是漠不关心的，而且用户可以接受在更大的模型上工作，这些单元可以被分组到虚拟子系统中。当你确定功能模型分割策略时，请考虑：

- Generated code file and functionality grouping
- Parallel feature development by multiple developers
- The overhead associated with model reference files
- The ease of visibility for feature grouping in the design

At this intermediate level of the model between unit and the top level, the choice of modeling constructs is up to your organization's modeling guidelines and preferences. Figure 4 shows an acceptable method of using virtual subsystems.

> 在单元和最高层之间的这个中间模型层，建模构造的选择取决于您组织的建模准则和偏好。图 4 显示了一种使用的可接受方法。

> ![](./media/image4.jpeg){width="6.204232283464567in" height="2.0699989063867017in"}
> _Figure 4. Grouping multiple units in a feature._

## Split ASIL and QM Levels at the Top Level of the Model

One key concept in ISO 26262 centers around freedom from interference. ISO 26262 has five distinct safety levels (Quality Management \[QM\], and ASIL A--D) that can be used to classify system- and software-level functionality based on the functional safety aspects of the system. Electrical and electronic systems that are following ISO 26262 will likely have components at different ASILs. For example, a portion of an algorithm that reports out non-critical diagnostic data may be classified as a QM component, whereas a section of the algorithm that could impact the vehi- cle's ability to brake may be classified to a higher-level ASIL component due to the high degree of hazard/injury risk if a failure occurs.

> 一个关键概念在 ISO 26262 中围绕着免受干扰。ISO 26262 有五个不同的安全级别(质量管理[QM]和 ASIL A-D)，可用于基于系统的功能安全方面对系统和软件功能进行分类。遵循 ISO 26262 的电子和电子系统可能具有不同 ASIL 级别的组件。例如，报告非关键诊断数据的算法部分可能被分类为 QM 组件，而可能影响车辆制动能力的算法部分由于失效时的高度危险/伤害风险可能被分类为更高级别的 ASIL 组件。

A system with multiple ASIL components will benefit from an architecture that efficiently segments these algorithms into separate containers. The benefit will be seen for two reasons:

> 一个具有多个 ASIL 组件的系统将从有效地将这些算法分割到不同容器的架构中受益。这种好处将有两个原因：

- Each ASIL can have different development, validation, and verification requirements.
- Separating and segmenting ASILs enables freedom from interference.

> - 每个 ASIL 都可以具有不同的开发，验证和验证要求。
> - 分开和分割 ASIL 可以免于干扰。

Since the various ASILs will be split, you should choose a modeling construct to aid in that segmentation. Use model references so that when the algorithm is deployed, there will be a firm boundary at each ASIL's border. Therefore, you should split the top level of the system-level model into multiple model references with each model reference rep- resenting a separate ASIL. Note that in this case, due to code generation configuration settings (see the Code Generation Configuration section for detail), the system-level model is for simulation only, and code generation can only be done separately based on ASIL. Generating code at the unit level and then integrating it is another option.

> 由于各种 ASIL 将被分割，您应该选择一个建模结构来帮助进行分割。使用模型引用，这样当算法部署时，每个 ASIL 的边界都会有一个坚定的边界。因此，您应该将系统级模型的顶级分割成多个模型引用，每个模型引用代表一个单独的 ASIL。请注意，在这种情况下，由于代码生成配置设置(有关详细信息，请参见代码生成配置部分)，系统级模型仅用于模拟，并且只能根据 ASIL 单独进行代码生成。另一种选择是在单元级别生成代码，然后进行集成。

However, generating code at as high a level as possible reduces the amount of overhead during code integration.

> 然而，尽可能高级别地生成代码可以减少代码集成期间的开销。

By using a model reference for each ASIL or even more generically, whenever freedom from interference is needed, separate functions and source files will be generated for each model reference. By following this and the code genera- tion configuration best practice, each segmented partition will have its own source files, shared utilities, and data definitions, which will make it easier to achieve freedom from interference between the different sections of the algo- rithm. Figure 5 demonstrates how a model could hierarchically be split based on ASIL.

> 通过为每个 ASIL 使用模型参考，或者更一般地说，每当需要免受干扰时，将为每个模型参考生成单独的功能和源文件。 通过遵循此操作和代码生成配置最佳实践，每个分段分区将具有自己的源文件，共享实用程序和数据定义，这将使得实现算法不同部分之间的自由免受干扰变得更加容易。 图 5 演示了如何基于 ASIL 层次分割模型。

> ![](./media/image5.jpeg){width="3.412409230096238in" height="4.301666666666667in"}
> _Figure 5. Model hierarchy based on ASIL._

## Eliminate Algorithmic Content at the Integration Level

ISO 26262 has the notion of multiple testing levels in the representative architecture, including unit level, integration level, and system level. Typically, a software unit must go through various levels of testing rigor based on the target- ed ASIL. For example, ASIL D may require full modified condition and decision coverage (MCDC), whereas for ASIL A or B, condition and decision coverage may be acceptable. Because of this, units are the only place where algo- rithmic functionality should be implemented. If algorithmic content occurs at the integration or system level of the model, it can be more difficult to achieve full coverage of the design. For this reason, it is recommended to ensure that no algorithm content occurs outside of unit-level models (Figure 6).

> ISO 26262 提出了多个测试级别的概念，包括单元级、集成级和系统级。根据目标 ASIL，软件单元通常必须经历多个级别的测试严格性。例如，ASIL D 可能需要完整的修改条件和决策覆盖(MCDC)，而 ASIL A 或 B 可能只需要条件和决策覆盖。因此，单元是唯一可以实现算法功能的地方。如果算法内容出现在模型的集成或系统级别，则可能更难实现设计的完整覆盖。因此，建议确保没有算法内容出现在单元级模型之外(图 6)。

> ![](./media/image6.jpeg){width="5.576666666666667in" height="1.7266666666666666in"}
> _Figure 6. Avoiding algorithm content outside of the unit._

This best practice can also be mapped to MathWorks Automotive Advisory Board (MAAB) rule db_0143: Similar block types on the model levels.

> 这种最佳实践也可以映射到 MathWorks 汽车顾问委员会(MAAB)规则 db_0143：模型层次上的类似块类型。

## Use Model Metrics to Monitor Unit Complexity

Many organizations realize late in the development cycle that their algorithms will be difficult to validate to the level of coverage that ISO 26262 recommends. This typically stems from the lack of architectural consideration at design time and management of unit-level size and complexity during development. One methodology that can alleviate this issue is to set unit size metric thresholds for the entire development organization. These thresholds can include:

> 许多组织在开发周期后期才意识到，他们的**算法很难达到 ISO 26262 建议的验证水平。这通常源于设计时期缺乏体系结构的考虑以及开发过程中单元级别的大小和复杂性的管理**。一种可以缓解这个问题的方法是为整个开发组织设定单元大小指标阈值。这些阈值可以包括：

- Maximum number of inputs and outputs per unit
- Reusable libraries
- Cyclomatic complexity
- Number of elements

To manage the unit's complexity, include the review of these metrics during the model review process of the develop- ment cycle. This will enable visibility of the complexity and scope of how the algorithm has been designed during the development process. The Model Metrics Dashboard in Simulink Check™ (Figure 7) can make this process easier.

> 为了管理单元的复杂性，在开发周期的模型审查过程中包括这些指标的审查。这将使开发过程中算法设计的复杂性和范围可见。Simulink Check™ 中的模型指标仪表板(图 7)可以使这一过程更加容易。

> ![](./media/image7.jpeg){width="5.520832239720035in" height="3.504166666666667in"}
> _Figure 7. The Model Metrics Dashboard in Simulink Check._

The Model Metrics Dashboard can make total block count, MATLAB lines of code (LOC), Stateflow® LOC, model complexity, number of blocks, and other metrics easily visible to the modelers and model reviewers. The dashboard supports the design workflow by continuously monitoring models so you can ensure that they are not being divided into units that are too small (increasing management complexity) or too large (unable to be easily tested and difficult to reuse).

> 模型指标仪表板可以轻松地显示总块计数、MATLAB 行代码(LOC)、Stateflow® LOC、模型复杂度、块数量等指标，以便模型建模者和模型审查者可以轻松获取。仪表板支持设计工作流，可以持续监控模型，以确保它们不会被分割成太小的单元(增加管理复杂度)或太大的单元(无法轻松测试，难以重用)。

Threshold values are often an area of debate. This is because the model complexity usually can't be measured from a single aspect of the model, and often requires you to analyze multiple metrics to make meaningful decisions. For example, one Stateflow chart can have very high cyclomatic complexity while its block count is only one.

> 阈值通常是一个争论的焦点。这是因为**模型的复杂性通常不能从模型的单一方面来衡量**，通常需要分析多个指标才能做出有意义的决定。例如，一个 Stateflow 图可以具有非常高的循环复杂度，而其块计数却只有一个。

The recently published paper [**_Model Quality Objectives for Embedded Software Development with MATLAB and Simulink_**](https://www.mathworks.com/content/dam/mathworks/mathworks-dot-com/company/events/conferences/automotive-conference-stuttgart/2018/proceedings/model-quality-objectives-for-collaboration-between-oem-and-suppliers.pdf), from MathWorks and a group of automotive companies (Delphi Technologies, Bosch, PSA, Renault, and Valeo), provides guidance on metrics and thresholds. For example, model cyclomatic complexity should be 30 or lower, and the number of model elements should be less than 500.

> 最近发表的论文[使用 MATLAB 开发嵌入式软件的模型质量目标](../model-quality-objectives-for-collaboration-between-oem-and-suppliers.pdf)
> MathWorks 和一组汽车公司(Delphi Technologies，Bosch，PSA，Renault 和 Valeo)的 Simulink 提供了关于指标和阈值的指导。例如，模型循环复杂度应该低于 30，模型元素数量应该小于 500。

Each company will have different threshold values, and MathWorks Consulting Services provides support in this area.

> 每家公司将有不同的阈值，MathWorks 咨询服务可以提供支持。

# Signal Routing and Interface Definition

Part 6 of ISO 26262 has multiple considerations that address interface complexity and data exchange between units and components. For example:

> 第 6 部分 ISO 26262 涉及多个方面，以解决单元和组件之间的接口复杂性和数据交换。例如：

- Table 3 - 1b: Restricted size and complexity of software components
- Table 3 - 1c: Restricted size of interfaces
- Table 7 - 1g: Data flow analysis
- Table 7 - 1k: Interface tests

To achieve these goals, it is important to determine architectural strategies for how data will be exchanged between units, components, and different ASILs. While working with various engineering teams, MathWorks has created multiple best practices to reduce the work necessary to analyze data interface exchanges between unit level models, components, and various ASILs. This section provides four best practices aimed at managing interface complexity and data exchange.

> 为了实现这些目标，**确定数据在单元、组件和不同的 ASIL 之间交换的架构策略是很重要的**。在与各种工程团队合作的过程中，MathWorks 已经创建了多种最佳实践，以减少分析单元级模型、组件和各种 ASIL 之间数据接口交换所需的工作量。本节提供了四种旨在管理接口复杂性和数据交换的最佳实践。

## Group Bus Signals by ASIL, Feature, and Rate

ISO 26262 Part 6 Table 7 - 1g recommends that development teams perform data flow analysis. Data flow analysis is necessary to understand how signals are passed through a software algorithm and down to the unit level. This type of analysis can spot areas where signal requirements clash or where various signals should not be directly used based on characteristics of the provider. To perform this analysis, organizations must develop a bus hierarchy strategy to make it easier for the developer to understand where the signal is coming from and what the characteristics of the signal are. If you don't specify how bus signals should be grouped hierarchically, the following issues can occur:

> ISO 26262 第 6 部分表 7-1g 建议开发团队进行数据流分析。**数据流分析是理解信号如何通过软件算法并传递到单元级别的必要步骤**。此类分析可以**发现信号要求冲突的区域**，或者基于提供者特性，不应直接使用各种信号的区域。要执行此分析，组织必须制定总线层次策略，以便使开发人员更容易理解信号来源以及信号的特性。如果不**指定总线信号应如何分层**，可能会出现以下问题：

- Inefficient bus segmentation and modeling patterns
- Inconsistent bus grouping from developer to developer
- Modeling difficulty when splitting and recreating buses
- Inefficient code generation

> - 效率低下的总线分割和建模模式
> - 从开发人员到开发人员不一致的 BUS 分组
> - 在分裂和重新创建 BUS 时建模难度
> - 效率低下的代码生成

Just as model architecture requires a top-down design approach, the same is true for bus hierarchy. To manage the ASIL for each signal, group signals based on task rate and ASIL in the bus hierarchy (Figure 8). By grouping these signals based on ASIL, it will be easier to determine the provider of a signal and determine if the signal is being used in a unit of a higher ASIL. For example, if a signal is provided from a QM component but used by an ASIL compo- nent, additional analysis will be necessary to ensure that the dependency on this signal has been analyzed and accounted for.

> 正如模型架构需要采用自上而下的设计方法一样，总线层次结构也是如此。为了管理每个信号的 ASIL，根据任务速率和 ASIL 在总线层次结构中对信号进行分组(图 8)。通过基于 ASIL 分组这些信号，将更容易确定信号的提供者，并确定信号是否在 ASIL 组件中使用。例如，如果一个信号是由 QM 组件提供的，但是被 ASIL 组件使用，则需要进行额外的分析，以确保对该信号的依赖性已经进行了分析和考虑。

> ![](./media/image8.png){width="5.924248687664042in" height="0.9187489063867017in"}
> _Figure 8. Grouping signals in a bus hierarchy._

## Pass Only Necessary Signals to Units

Table 3 and Table 7 of ISO 26262 Part 6 have important suggestions relating to unit-level interfaces and data exchange. These two tables mention that the size and complexity of software components and interfaces should be reduced where possible. Also, during the verification process, users should perform interface tests. If unnecessary variables are passed down to a unit-level model, additional testing will be necessary to ensure that these signals do not have an impact on the unit. To alleviate this concern, reducing the inputs that are passed to the unit level can help. To accomplish this, you can split bus signals prior to entering a unit-level model to only signals used in the cor- responding unit (Figure 9).

> 表 3 和表 7 的 ISO 26262 第 6 部分提出了重要的建议，关于单元级接口和数据交换。这两张表提到，应尽可能减少软件组件和接口的大小和复杂性。此外，在验证过程中，用户应执行接口测试。如果将不必要的变量传递到单元级模型，则需要进行额外的测试，以确保这些信号不会对单元产生影响。为了减轻这种担忧，可以减少传递给单元级的输入。为此，可以在进入单元级模型之前拆分总线信号，以只传递给相应单元所使用的信号(图 9)。

> ![](./media/image9.jpeg){width="6.345646325459318in" height="2.6975in"}
> _Figure 9. Splitting bus signals entering a unit-level model._

## Optimize Placement of Signal and Parameter Objects

The high-level use case for signal and parameter objects is to define the interface between the model and base soft- ware. In such a use case, parameter objects are typically used for specifying calibration values and placed inside model blocks such as Gain and Lookup Table blocks. The usage of signal objects is more complex, but it usually is associated either with internal signals to support calibration activities or with root input and output ports (Figure 10).

> **高级用例中使用信号和参数对象的目的是定义模型和基础软件之间的接口**。在这种用例中，参数对象通常用于指定校准值，并放置在诸如增益和查找表块之类的模型块中。信号对象的使用更加复杂，但通常与内部信号相关联，以支持校准活动或根输入和输出端口。

> ![](./media/image10.png){width="4.492722003499563in" height="3.518957786526684in"}
> _Figure 10. Placing signal and parameter objects._

It is important to note that the interface to the model reference block does not contain signal objects. This is because signals at the boundary of the unit level are considered internal interface signals. This also assumes that the code is being generated at the highest ASIL partition as mentioned in best practice #6 above. For the internal interface sig- nals, it is best to let Embedded Coder® define those signals based on its internal optimization algorithms. Data type information, however, does need to be explicitly defined as part of the port block configuration at model reference boundaries, as shown in Figure 11.

> 重要的是要注意，模型参考块的接口不包含信号对象。这是因为单元级别的边界信号被视为内部接口信号。这也假定代码是按照上述最佳实践#6 中提到的最高 ASIL 分区生成的。对于内部接口信号，最好让嵌入式编码器根据其内部优化算法定义这些信号。但是，数据类型信息确实需要作为模型参考边界端口块配置的一部分明确定义，如图 11 所示。

> ![](./media/image11.png){width="5.491510279965004in" height="4.734374453193351in"}
> _Figure 11. Defining data type for a port._

This best practice restricts the placement of interface signal objects to the code generation model boundaries. It also ensures that when code is generated for each ASIL, the root-level output ports will point to the corresponding inter- face function in the other ASIL section.

> 这种最佳实践限制了接口信号对象的放置范围仅限于代码生成模型的边界。它还确保当为每个 ASIL 生成代码时，根级输出端口将指向另一个 ASIL 部分中的相应接口功能。

The storage class used for the signal and parameter objects can be set based on the software architecture and coding practices. The exception will be the protection needed for data exchange between ASIL models or partitioning as required by freedom from interference.

> 存储类用于信号和参数对象可以根据软件架构和编码实践设置。除了 ASIL 模型之间数据交换所需的保护或根据免受干扰的要求进行分区之外，没有例外。

## Protect Data Exchanged Between ASILs

One consideration when code for each ASIL is generated separately is that a protection method is needed to exchange data between each ASIL. Multiple strategies exist using storage classes on the ASIL sections' root-level input and output ports. One prevalent method is to use a storage class that has get and set access functions for the data. By using get and set access functions, you can add additional protection to these interfaces so that only appro- priate software components are accessing data.

> 当为每个 ASIL 生成单独的代码时，需要考虑的一个因素是需要一种保护方法来在每个 ASIL 之间交换数据。使用 ASIL 部分根级输入和输出端口上的存储类可以使用多种策略。一种流行的方法是使用具有 get 和 set 访问功能的存储类。通过使用 get 和 set 访问功能，您可以为这些接口添加额外的保护，以便只有适当的软件组件才能访问数据。

Figure 12 depicts one example of how GetSet storage classes can be used on the root-level input ports and the corre- sponding generated code.

> 图 12 描绘了一个使用 GetSet 存储类在根级输入端口上的示例，以及相应生成的代码。

> ![](./media/image12.jpeg){width="6.3699989063867015in" height="1.8466666666666667in"}
> _Figure 12. Non AUTOSAR Interfaces Between ASIL Components_

Figure 13 shows how one of the storage classes was configured to ensure that the Get and Set API match between ASILs.

> 图 13 显示了如何配置其中一个存储类，以确保 ASIL 之间的 Get 和 Set API 匹配。

> ![](./media/image13.png){width="6.310725065616798in" height="3.3536450131233595in"}
> _Figure 13. Configuring code generation options for the root-level I/O storage class._
> 图 13. 为根级 I/O 存储类配置代码生成选项。

It is important to note that the GetSet storage class objects only provide an entry point for the interface protection. The actual implementation of the protection is typically done through low-level software layers implemented by hand coding, which can be easily integrated using the GetSet storage class.

> 重要的是要注意，GetSet 存储类对象只提供接口保护的入口点。**保护的实际实现通常是通过手工编码实现的低级软件层来完成的**，可以使用 GetSet 存储类轻松集成。

# Code Generation Configuration

Once the model has been developed according to the best practices listed above, there are still code generation con- figuration settings that need special attention to achieve objectives set forth by ISO 26262. This section provides best practices for code generation configuration setting with respect to code placement and separation of shared utility files. Again, the configuration listed below assumes the previous best practices have been followed.

> 一旦模型根据上述最佳实践进行了开发，仍然需要特别关注代码生成配置设置，以实现 ISO 26262 设定的目标。本节提供了有关代码位置和共享实用程序文件分离的最佳实践代码生成配置设置。同样，以下配置假定已经遵循了先前的最佳实践。

## Determine a Code Placement Strategy

One key design concept in ISO 26262 is freedom from interference. Freedom from interference is necessary to ensure that if there is an issue with one section of the system at a particular ASIL, it will not impact functionality at a sepa- rate ASIL. For example, if a QM or ASIL A component has an issue or a failure occurs, the design should segment this functionality away from functionality that is ASIL D to ensure that the ASIL D functionality can continue oper- ating. In embedded systems, one type of fault that is a concern is if portions of the application have access to sections of memory or functions that they should not have access to. One way to address this concern is to separate the func- tionality into separate memory sections or on separate cores of a microprocessor. This can be done by telling the compiler into which section each variable, function, or file should be placed. Figure 14 demonstrates how the appli- cation could be separated.

> ISO 26262 的**一个关键设计概念是免受干扰**。免受干扰是必要的，以确保如果特定 ASIL 的系统的**一个部分出现问题，它不会影响另一个 ASIL 的功能**。例如，如果 QM 或 ASIL A 组件出现问题或发生故障，则设计应将此功能与 ASIL D 功能分离开来，以确保 ASIL D 功能可以继续运行。在嵌入式系统中，一种关注的故障是应用程序的部分是否可以访问它们不应该访问的内存或功能部分。解决此问题的一种方法是将功能分割到单独的内存部分或微处理器的单独核心中。这可以通过告诉编译器将每个变量、函数或文件放置到哪个部分来实现。图 14 演示了如何分离应用程序。

> ![](./media/image14.jpeg){width="2.9025in" height="3.368333333333333in"}
> _Figure 14. Segmenting the system architecture to ensure freedom from interference._
> 图 14。将系统架构分割以确保免受干扰。

Another technique is to split various ASIL and QM levels into separate memory sections. Splitting the memory sec- tions alleviates some concerns with various ASILs unintendedly interacting with each other, for example, a function within the QM software component (SW-C) writing to a protected ASIL RAM location. To configure this, memory sections can be selected in the configuration options as shown in Figure 15. This method does not have to be used, but it does simplify the overall workflow.

> 另一种技术是将各种 ASIL 和 QM 级别拆分成单独的内存部分。拆分内存部分可以减轻一些关注，例如 QM 软件组件(SW-C)在受保护的 ASIL RAM 位置写入功能。为了配置此功能，可以在配置选项中选择内存部分，如图 15 所示。不必使用此方法，但它确实可以简化整个工作流程。

> ![](./media/image15.png){width="5.822397200349957in" height="3.566666666666667in"}
> _Figure 15. Configuring parameter settings for memory sections._

## Use Different Name Tokens for Shared Utilities

Embedded Coder generates common utility files knows as shared utilities. These files are basic functions that are used across the code generation model. This method presents an issue in a safety-related application because there will not be any distinctions between the sources of the shared utility files. To compile code into a signal application with the segmentation concept, shared utilities must be generated and allocated based on their ASIL. This can be done through code generation configuration, as shown in Figure 16.

> 嵌入式编码器生成称为共享实用程序的常用实用程序文件。这些文件是在代码生成模型中使用的基本功能。在安全相关应用中，这种方法会带来问题，**因为共享实用程序文件的来源没有任何区别。要使用分段概念编译代码成信号应用程序，必须根据 ASIL 生成和分配共享实用程序**。这可以通过代码生成配置来实现，如图 16 所示。

> ![](./media/image16.png){width="6.382811679790026in" height="2.8621872265966752in"}
> _Figure 16. Configuring shared utility settings._

With the above settings, you can create the generated shared utility with a unique identifier that is a function of the ASIL. For example, the shared utility for the Lookup Table block when configured based on the above QM suffix is shown in Figure 17.

> 通过上述设置，您可以创建具有唯一标识符的生成共享实用程序，该标识符是 ASIL 的函数。例如，基于上述 QM 后缀配置的查找表块的共享实用程序如图 17 所示。

> ![](./media/image17.png){width="6.369374453193351in" height="3.1354166666666665in"}
> _Figure 17. Shared utility configured for QM._

# Summary and Future Work

The findings presented in this document are best practices created through multiple MathWorks consulting engage- ments. These best practices are proven enablers to adoption of ISO 26262. However, following these best practices does not guarantee ISO 26262 compliance because they address a subset of all ISO 26262 requirements, and each application has its unique needs.

> 本文件所提出的研究结果是通过多个 MathWorks 咨询项目创建的最佳实践。这些最佳实践是实现 ISO 26262 采用的证明有效的因素。但是，**遵循这些最佳实践并不能保证 ISO 26262 的合规性，因为它们只涉及 ISO 26262 要求的一部分**，每个应用都有其独特的需求。

Future work is underway in applying the above best practices for AUTOSAR standards. **_[Request an early draft of the paper.](http://www.mathworks.com/services/consulting/contact.html)_**

> 正在进行未来的工作，以应用上述最佳实践来实现 AUTOSAR 标准。[请求一份早期的草稿论文]。

# Learn More

- **_[ISO 26262 Support in MATLAB and Simulink](https://www.mathworks.com/solutions/automotive/standards/iso-26262.html)_** - Overview
- **_[ISO 26262 Process Deployment Advisory Service](https://www.mathworks.com/services/consulting/proven-solutions/iso26262.html)_** - Consulting Services

© 2019 The MathWorks, Inc. MATLAB and Simulink are registered trademarks of The MathWorks, Inc. See mathworks.com/trademarks for a list of additional trademarks. Other product or brand names may be trademarks or registered trademarks of their respective holders.
---
