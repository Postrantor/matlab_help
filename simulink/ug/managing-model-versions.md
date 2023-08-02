---
url: https://ww2.mathworks.cn/help/simulink/ug/managing-model-versions.html
title: 管理模型版本并指定模型属性
- MATLAB & Simulink
- MathWorks 中国
date: 2023-07-19 10:14:15
tag: #版本管理
summary: 查看和编辑模型版本信息、历史记录、回调函数和模型说明。
---
## 管理模型版本并指定模型属性

在 Simulink® 中，您可以使用这些方法管理模型的多个版本：

- 使用 Projects 管理您的工程文件、连接到源代码管理、查看已修改的文件和比较修订版本。请参阅[工程管理](https://ww2.mathworks.cn/help/simulink/project-management.html)。
- 使用模型文件更改通知可处理如何进行源代码管理操作和管理多个用户。请参阅[模型文件更改通知](https://ww2.mathworks.cn/help/simulink/ug/managing-model-versions.html#bq4d22m-1)。
- 请使用 [`Simulink.MDLInfo`](https://ww2.mathworks.cn/help/simulink/slref/simulink.mdlinfo.html) 以从模型文件中提取信息，而不必将模块图加载到内存中。您可以使用 `MDLInfo` 查询模型版本和 Simulink 版本，查找引用模型的名称而不必将该模型加载到内存中，并将任意元数据附加到您的模型文件。

### 模型文件更改通知

您可以使用 Simulink 预设指定磁盘上的模型发生更改时是否向您发送通知。您可以在对模型进行更新或仿真、首次编辑模型或保存模型时接收此通知。例如，执行源代码管理操作和存在多个用户时，磁盘上的模型便可能发生更改。

在 Simulink 编辑器中，在**建模**选项卡上，选择 > 。在**模型文件**窗格中的**更改通知**下，您可以选择以下选项：

- 如果您选择**首次编辑模型时**，且磁盘上的文件已更改，而模块图在 Simulink 中未修改，则：

  - 修改模块图的任何交互操作（例如，添加模块）都会导致出现警告。
  - 修改模块图的任何命令行操作（例如对 `set_param` 的调用）都会导致出现警告。
- 如果您选择**保存模型时**，且磁盘上的文件已更改，则：

  - 在 Simulink 编辑器中保存模型会导致出现消息。
  - `save_system` 函数会报告错误，除非您使用 `OverwriteIfChangedOnDisk` 选项。

要以编程方式检查模型自加载以来是否在磁盘上发生了更改，请使用函数 [`slIsFileChangedOnDisk`](https://ww2.mathworks.cn/help/simulink/slref/slisfilechangedondisk.html)。

有关可帮助您处理源代码管理和多个用户的更多选项，请参阅[工程管理](https://ww2.mathworks.cn/help/simulink/project-management.html)。

### 管理模型属性

您可以使用属性检查器来查看和编辑模型版本属性、描述和回调函数。要打开属性检查器，请在**建模**选项卡中，在**设计**下，点击**属性检查器**。如果在模型顶层未选择任何内容，则模型属性（或者，如果在库模型中，则为库属性）会出现在属性检查器中。

#### 指定当前用户

当您创建或更新模型时，您的名称会记录在模型中。Simulink 假定您的名称由至少一个 `USER`、`USERNAME`、`LOGIN` 或 `LOGNAME` 环境变量指定。如果您的系统未定义上述任何变量，则 Simulink 不会在模型中更新用户名。

UNIX® 系统定义 `USER` 环境变量并将其值设置为您用于登录到系统的名称。因此，如果您使用 UNIX 系统，无需执行任何操作，Simulink 即会将您识别为当前用户。

Windows® 系统可为用户名定义 Simulink 所需的环境变量，具体取决于您的系统上安装的 Windows 版本以及它是否连接到网络。使用 MATLAB® 函数 `getenv` 确定定义了哪个环境变量。例如，在 MATLAB 命令行窗口中，输入：

此函数可确定 `USER` 环境变量是否存在于您的 Windows 系统中。如果不存在，请设置该变量。

#### 模型信息

**信息**选项卡汇总了有关模型当前版本的信息，例如所做修改、版本和上次保存日期。您可以查看和编辑模型信息，以及启用、查看和编辑模型的更改历史记录。

使用**说明**部分输入模型的说明。然后，您可以通过在 MATLAB 命令行窗口下输入 ``[`help`](https://ww2.mathworks.cn/help/matlab/ref/help.html)`` 后跟模型名称来查看模型说明。

- **模型版本**

  此模型的版本号。自上次保存模型以来，主模型版本按发布次数递增。对于 Simulink 的每个新版本，模型次要版本都会重置为零，并且每次当您在同一版本中保存模型时，模型次要版本都会递增 1。
- **创建者**

  创建此模型的人员的名称，基于创建模型时 `USER` 环境变量的值。
- **创建日期**

  此模型的创建日期和时间。不要更改此值。
- **上次保存者**

  上次保存此模型的人员的名称，基于保存模型时 `USER` 环境变量的值。
- **上次保存日期**

  上次保存此模型的日期，基于系统日期和时间。

#### 属性

您可以查看源文件位置，设置模型的压缩级别，指定保存模型设计数据的位置，并在模型属性的**属性**选项卡上定义回调。

**设置 SLX 压缩级别.** 在**属性检查器**的**属性**选项卡中，您可以选择三个 **SLX 压缩**选项之一：

- “`无`” 在保存操作期间不应用压缩。
- 默认情况下，“`普通`” 创建最小的文件大小。
- “`最快`” 创建的文件大小比选择 “`无`” 时要小，但比 “`普通`” 所需的保存时间更短。

要以编程方式设置压缩级别，请使用 `SLXCompressionType`。

**提示**

您可以通过在不压缩的情况下保存 Simulink 模型来减小 Git™ 存储库的大小。关闭压缩会导致磁盘上的 SLX 文件变大，但会减小存储库的大小。

要对新 SLX 文件使用此设置，请在 **SLX 压缩**设置为 “`无`” 的状态下使用模型模板创建模型。请参阅 [Create Template from Model](https://ww2.mathworks.cn/help/simulink/ug/create-a-template-from-a-model.html)。对于现有 SLX 文件，请设置压缩，然后保存模型。

**定义设计数据的位置.** 使用**外部数据**部分指定您的模型使用的设计数据的位置。您可以在基础工作区或数据字典中定义设计数据。请参阅[迁移单个模型以使用字典](https://ww2.mathworks.cn/help/simulink/ug/migrate-models-to-use-dictionary.html#btn2bvh)。

**回调.** 使用**回调**部分可指定要在模型仿真过程中的特定点处调用的函数。从列表中选择该回调。在框中，输入要为选定的回调调用的函数。有关这些回调的信息，请参阅[创建模型回调](https://ww2.mathworks.cn/help/simulink/ug/model-callbacks.html#f4-56417)。

### 以编程方式访问模型信息

某些版本信息存储为模型中的模型参数。您可以使用 Simulink `get_param` 函数通过编程方式访问此信息。

下表介绍了 Simulink 用于存储版本信息的模型参数。

<table><colgroup><col width="43%"><col width="57%"></colgroup><thead><tr><th>属性</th><th>描述</th></tr></thead><tbody><tr><td><p><code>BlockDiagramType</code></p></td><td><p>对于打开的 Simulink 模块图，返回 <code>model</code>。对于 Simulink 模块库，返回 <code>library</code>。</p></td></tr><tr><td><p><code>Created</code></p></td><td><p>创建日期。</p></td></tr><tr><td><p><code>Creator</code></p></td><td><p>创建此模型的人员的名称。</p></td></tr><tr><td><p><code>Description</code></p></td><td><p>用户输入的此模型的说明。在属性检查器的<strong>信息</strong>选项卡上，在<strong>说明</strong>框中输入或编辑模型说明。要在 MATLAB 命令行窗口中查看模型说明，请输入：</p><div><div><div><pre>help 'mymodelname'
</pre></div></div></div></td></tr><tr><td><p><code>Dirty</code></p></td><td><p>如果此参数的值为 <code>on</code>，则模型有未保存的更改。</p></td></tr><tr><td><p><code>FileName</code></p></td><td><p>保存模型的绝对路径。</p></td></tr><tr><td><code>LastModifiedBy</code></td><td><p>上次保存模型的用户的名称。</p></td></tr><tr><td><p><code>LastModifiedDate</code></p></td><td><p>上次保存模型时的日期。</p></td></tr><tr><td><p><code>MetaData</code></p></td><td><p>与模型有关的任意数据的名称和属性。有关详细信息，请参阅 <a href="https://ww2.mathworks.cn/help/simulink/slref/simulink.mdlinfo.getmetadata.html" hreflang="en"><code>Simulink.MDLInfo.getMetadata</code></a>。</p></td></tr><tr><td><p><code>ModifiedByFormat</code></p></td><td><p><code>ModifiedBy</code> 参数的格式。该值可以包含 <code>%&lt;Auto&gt;</code> 标记。Simulink 软件会将该标记替换为 <code>USER</code> 环境变量的当前值。</p></td></tr><tr><td><p><code>ModifiedDateFormat</code></p></td><td><p>用来生成 <code>LastModifiedDate</code> 参数值的格式。该值可以包含 <code>%&lt;Auto&gt;</code> 标记。Simulink 在保存模型时会将该标记替换为当前日期和时间。</p></td></tr><tr><td><p><code>ModelVersion</code></p></td><td><p>自上次保存模型以来，主模型版本按发布次数递增。对于 Simulink 的每个新版本，模型次要版本都会重置为零，并且每次当您在同一版本中保存模型时，模型次要版本都会递增 1。</p></td></tr><tr><td><p><code>ModelVersionFormat</code></p></td><td><p>该值包含模型格式版本 <code>%&lt;AutoIncrement:#.#&gt;</code>，其中 <code>#</code> 是整数。保存模型时，Simulink 递增模型版本号 <code>#</code>。</p></td></tr><tr><td><p><code>PreviousFileName</code></p></td><td><p>当 <code>PreSaveFcn</code> 或 <code>PostSaveFcn</code> 回调正在运行时，<code>PreviousFileName</code> 指示在保存操作开始前模型的绝对路径。</p><p>要查找模型的当前绝对路径，请改用 <code>FileName</code>。</p></td></tr><tr><td><p><code>SavedSinceLoaded</code></p></td><td><p>指示模型加载后是否保存过。<code>'on'</code> 表示模型已保存。</p></td></tr><tr><td><p><code>VersionLoaded</code></p></td><td><p>上次保存模型使用的 Simulink 版本，例如 <code>'7.6'</code>。</p></td></tr><tr><td><p><code>EnableAccessToBaseWorkspace</code></p></td><td><p>模型是否可以访问基础工作区中的设计数据和配置集，指定为 <code>'true'</code> 或 <code>'false'</code>。</p></td></tr></tbody></table>

`LibraryVersion` 是链接模块的模块参数。`LibraryVersion` 是创建链接时库的 `ModelVersion`。

有关源代码管理版本信息，请参阅[工程管理](https://ww2.mathworks.cn/help/simulink/project-management.html)。

## 另请参阅

[Model Info](https://ww2.mathworks.cn/help/simulink/slref/modelinfo.html)

## 相关主题

- [工程管理](https://ww2.mathworks.cn/help/simulink/project-management.html)
- [模型文件更改通知](https://ww2.mathworks.cn/help/simulink/ug/managing-model-versions.html#bq4d22m-1)
- [`Simulink.MDLInfo`](https://ww2.mathworks.cn/help/simulink/slref/simulink.mdlinfo.html)
