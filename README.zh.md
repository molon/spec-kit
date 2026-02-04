<div align="center">
    <img src="./media/logo_large.webp" alt="Spec Kit Logo" width="200" height="200"/>
    <h1>🌱 Spec Kit</h1>
    <h3><em>更快地构建高质量软件。</em></h3>
</div>

<p align="center">
    <strong>一个开源工具包，让你专注于产品场景和可预测的结果，而不是从零开始编写每一段代码。</strong>
</p>

<p align="center">
    <a href="https://github.com/github/spec-kit/actions/workflows/release.yml"><img src="https://github.com/github/spec-kit/actions/workflows/release.yml/badge.svg" alt="Release"/></a>
    <a href="https://github.com/github/spec-kit/stargazers"><img src="https://img.shields.io/github/stars/github/spec-kit?style=social" alt="GitHub stars"/></a>
    <a href="https://github.com/github/spec-kit/blob/main/LICENSE"><img src="https://img.shields.io/github/license/github/spec-kit" alt="License"/></a>
    <a href="https://github.github.io/spec-kit/"><img src="https://img.shields.io/badge/docs-GitHub_Pages-blue" alt="Documentation"/></a>
</p>

---

## 目录

- [🤔 什么是规格驱动开发？](#-什么是规格驱动开发)
- [⚡ 快速开始](#-快速开始)
- [📽️ 视频概览](#️-视频概览)
- [🤖 支持的 AI 代理](#-支持的-ai-代理)
- [🔧 Specify CLI 参考](#-specify-cli-参考)
- [📚 核心哲学](#-核心哲学)
- [🌟 开发阶段](#-开发阶段)
- [🎯 实验目标](#-实验目标)
- [🔧 前置条件](#-前置条件)
- [📖 了解更多](#-了解更多)
- [📋 详细流程](#-详细流程)
- [🔍 故障排除](#-故障排除)
- [👥 维护者](#-维护者)
- [💬 支持](#-支持)
- [🙏 致谢](#-致谢)
- [📄 许可证](#-许可证)

## 🤔 什么是规格驱动开发？

规格驱动开发**颠覆了传统软件开发**。几十年来，代码一直是王道——规格说明只是我们在"真正的工作"（编码）开始后就丢弃的脚手架。规格驱动开发改变了这一点：**规格说明变成了可执行的**，直接生成工作实现，而不仅仅是指导它们。

## ⚡ 快速开始

### 1. 安装 Specify CLI

选择你喜欢的安装方法：

#### 选项 1：持久安装（推荐）

安装一次，随处使用：

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

然后直接使用该工具：

```bash
# 创建新项目
specify init <PROJECT_NAME>

# 或在现有项目中初始化
specify init . --ai claude
# 或
specify init --here --ai claude

# 检查已安装的工具
specify check
```

要升级 Specify，请参阅[升级指南](./docs/upgrade.md)了解详细说明。快速升级：

```bash
uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git
```

#### 选项 2：一次性使用

直接运行而无需安装：

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME>
```

**持久安装的优势：**

- 工具保持安装并在 PATH 中可用
- 无需创建 shell 别名
- 使用 `uv tool list`、`uv tool upgrade`、`uv tool uninstall` 更好地管理工具
- 更清洁的 shell 配置

### 2. 建立项目原则

在项目目录中启动你的 AI 助手。`/speckit.*` 命令在助手中可用。

使用 **`/speckit.constitution`** 命令创建你的项目的治理原则和开发指南，这些原则将指导所有后续开发。

```bash
/speckit.constitution 创建专注于代码质量、测试标准、用户体验一致性和性能要求的原则
```

### 3. 创建规格说明

使用 **`/speckit.specify`** 命令描述你想要构建什么。专注于**什么**和**为什么**，而不是技术栈。

```bash
/speckit.specify 构建一个应用程序，帮助我将照片组织到单独的相册中。相册按日期分组，可以在主页上通过拖放重新组织。相册永远不会在其他嵌套相册中。在每个相册中，照片以瓷砖式界面预览。
```

### 4. 创建技术实现计划

使用 **`/speckit.plan`** 命令提供你的技术栈和架构选择。

```bash
/speckit.plan 应用程序使用 Vite，库数量最少。尽可能使用原生 HTML、CSS 和 JavaScript。图像不上传到任何地方，元数据存储在本地 SQLite 数据库中。
```

### 5. 分解为任务

使用 **`/speckit.tasks`** 从你的实现计划创建可操作的任务列表。

```bash
/speckit.tasks
```

### 6. 执行实现

使用 **`/speckit.implement`** 执行所有任务并根据计划构建你的功能。

```bash
/speckit.implement
```

有关详细的分步说明，请参阅我们的[综合指南](./spec-driven.md)。

## 📽️ 视频概览

想看 Spec Kit 的实际应用？观看我们的[视频概览](https://www.youtube.com/watch?v=a9eR1xsfvHg&pp=0gcJCckJAYcqIYzv)！

[![Spec Kit 视频标题](/media/spec-kit-video-header.jpg)](https://www.youtube.com/watch?v=a9eR1xsfvHg&pp=0gcJCckJAYcqIYzv)

## 🤖 支持的 AI 代理

| 代理 | 支持 | 备注 |
| --- | --- | --- |
| [Qoder CLI](https://qoder.com/cli) | ✅ | |
| [Amazon Q Developer CLI](https://aws.amazon.com/developer/learning/q-developer-cli/) | ⚠️ | Amazon Q Developer CLI [不支持](https://github.com/aws/amazon-q-developer-cli/issues/3064)自定义斜杠命令参数。|
| [Amp](https://ampcode.com/) | ✅ | |
| [Auggie CLI](https://docs.augmentcode.com/cli/overview) | ✅ | |
| [Claude Code](https://www.anthropic.com/claude-code) | ✅ | |
| [CodeBuddy CLI](https://www.codebuddy.ai/cli) | ✅ | |
| [Codex CLI](https://github.com/openai/codex) | ✅ | |
| [Cursor](https://cursor.sh/) | ✅ | |
| [Gemini CLI](https://github.com/google-gemini/gemini-cli) | ✅ | |
| [GitHub Copilot](https://code.visualstudio.com/) | ✅ | |
| [IBM Bob](https://www.ibm.com/products/bob) | ✅ | 基于 IDE 的代理，支持斜杠命令 |
| [Jules](https://jules.google.com/) | ✅ | |
| [Kilo Code](https://github.com/Kilo-Org/kilocode) | ✅ | |
| [opencode](https://opencode.ai/) | ✅ | |
| [Qwen Code](https://github.com/QwenLM/qwen-code) | ✅ | |
| [Roo Code](https://roocode.com/) | ✅ | |
| [SHAI (OVHcloud)](https://github.com/ovh/shai) | ✅ | |
| [Windsurf](https://windsurf.com/) | ✅ | |

## 🔧 Specify CLI 参考

`specify` 命令支持以下选项：

### 命令

| 命令 | 描述 |
| --- | --- |
| `init` | 从最新模板初始化新的 Specify 项目 |
| `check` | 检查已安装的工具（`git`、`claude`、`gemini`、`code`/`code-insiders`、`cursor-agent`、`windsurf`、`qwen`、`opencode`、`codex`、`shai`、`qoder`） |

### `specify init` 参数和选项

| 参数/选项 | 类型 | 描述 |
| --- | --- | --- |
| `<project-name>` | 参数 | 新项目目录的名称（如果使用 `--here` 或使用 `.` 作为当前目录，则可选） |
| `--ai` | 选项 | 要使用的 AI 助手：`claude`、`gemini`、`copilot`、`cursor-agent`、`qwen`、`opencode`、`codex`、`windsurf`、`kilocode`、`auggie`、`roo`、`codebuddy`、`amp`、`shai`、`q`、`bob` 或 `qoder` |
| `--script` | 选项 | 要使用的脚本变体：`sh`（bash/zsh）或 `ps`（PowerShell） |
| `--ignore-agent-tools` | 标志 | 跳过对 Claude Code 等 AI 代理工具的检查 |
| `--no-git` | 标志 | 跳过 git 仓库初始化 |
| `--here` | 标志 | 在当前目录而不是创建新目录中初始化项目 |
| `--force` | 标志 | 在初始化当前目录时强制合并/覆盖（跳过确认） |
| `--skip-tls` | 标志 | 跳过 SSL/TLS 验证（不推荐） |
| `--debug` | 标志 | 启用详细调试输出以进行故障排除 |
| `--github-token` | 选项 | 用于 API 请求的 GitHub 令牌（或设置 GH_TOKEN/GITHUB_TOKEN 环境变量） |

### 示例

```bash
# 基本项目初始化
specify init my-project

# 使用特定 AI 助手初始化
specify init my-project --ai claude

# 使用 Cursor 支持初始化
specify init my-project --ai cursor-agent

# 使用 Qoder 支持初始化
specify init my-project --ai qoder

# 使用 Windsurf 支持初始化
specify init my-project --ai windsurf

# 使用 Amp 支持初始化
specify init my-project --ai amp

# 使用 SHAI 支持初始化
specify init my-project --ai shai

# 使用 IBM Bob 支持初始化
specify init my-project --ai bob

# 使用 PowerShell 脚本初始化（Windows/跨平台）
specify init my-project --ai copilot --script ps

# 在当前目录中初始化
specify init . --ai copilot
# 或使用 --here 标志
specify init --here --ai copilot

# 强制合并到当前（非空）目录而无需确认
specify init . --force --ai copilot
# 或
specify init --here --force --ai copilot

# 跳过 git 初始化
specify init my-project --ai gemini --no-git

# 启用调试输出以进行故障排除
specify init my-project --ai claude --debug

# 使用 GitHub 令牌进行 API 请求（有助于企业环境）
specify init my-project --ai claude --github-token ghp_your_token_here

# 检查系统要求
specify check
```

### 可用的斜杠命令

运行 `specify init` 后，你的 AI 编码代理将可以访问这些斜杠命令以进行结构化开发：

#### 核心命令

Spec-Driven Development 工作流的基本命令：

| 命令 | 描述 |
| --- | --- |
| `/speckit.constitution` | 创建或更新项目治理原则和开发指南 |
| `/speckit.specify` | 定义你想要构建的内容（需求和用户故事） |
| `/speckit.plan` | 使用你选择的技术栈创建技术实现计划 |
| `/speckit.tasks` | 为实现生成可操作的任务列表 |
| `/speckit.implement` | 执行所有任务以根据计划构建功能 |

#### 可选命令

用于增强质量和验证的其他命令：

| 命令 | 描述 |
| --- | --- |
| `/speckit.clarify` | 澄清不充分的区域（建议在 `/speckit.plan` 之前；以前称为 `/quizme`） |
| `/speckit.analyze` | 跨工件一致性和覆盖范围分析（在 `/speckit.tasks` 之后、`/speckit.implement` 之前运行） |
| `/speckit.checklist` | 生成验证需求完整性、清晰度和一致性的自定义质量检查清单（如"英文单元测试"） |

### 环境变量

| 变量 | 描述 |
| --- | --- |
| `SPECIFY_FEATURE` | 覆盖非 Git 仓库的功能检测。设置为功能目录名称（例如 `001-photo-albums`）以在不使用 Git 分支时处理特定功能。<br/>**必须在使用 `/speckit.plan` 或后续命令之前在你正在使用的代理上下文中设置。 |

## 📚 核心哲学

规格驱动开发是一个强调以下内容的结构化过程：

- **意图驱动开发**，其中规格说明在"如何"之前定义"什么"
- **丰富的规格说明创建**，使用护栏和组织原则
- **多步骤细化**，而不是从提示进行一次性代码生成
- **大量依赖**高级 AI 模型的能力进行规格说明解释

## 🌟 开发阶段

| 阶段 | 焦点 | 关键活动 |
| --- | --- | --- |
| **0-to-1 开发**（"绿地"） | 从零开始生成 | <ul><li>从高级需求开始</li><li>生成规格说明</li><li>规划实现步骤</li><li>构建生产就绪的应用程序</li></ul> |
| **创意探索** | 并行实现 | <ul><li>探索多样化的解决方案</li><li>支持多个技术栈和架构</li><li>尝试 UX 模式</li></ul> |
| **迭代增强**（"棕地"） | 棕地现代化 | <ul><li>迭代添加功能</li><li>现代化遗留系统</li><li>调整流程</li></ul> |

## 🎯 实验目标

我们的研究和实验重点是：

### 技术独立性

- 使用多样化的技术栈创建应用程序
- 验证规格驱动开发是一个不受特定技术、编程语言或框架束缚的过程的假设

### 企业约束

- 演示关键任务应用程序开发
- 纳入组织约束（云提供商、技术栈、工程实践）
- 支持企业设计系统和合规要求

### 以用户为中心的开发

- 为不同的用户群体和偏好构建应用程序
- 支持各种开发方法（从即兴编码到 AI 原生开发）

### 创意和迭代流程

- 验证并行实现探索的概念
- 提供强大的迭代功能开发工作流
- 扩展流程以处理升级和现代化任务

## 🔧 前置条件

- **Linux/macOS/Windows**
- [支持的](#-支持的-ai-代理) AI 编码代理。
- [uv](https://docs.astral.sh/uv/) 用于包管理
- [Python 3.11+](https://www.python.org/downloads/)
- [Git](https://git-scm.com/downloads)

如果你遇到代理问题，请开启一个 issue，以便我们可以改进集成。

## 📖 了解更多

- **[完整的规格驱动开发方法论](./spec-driven.md)** - 深入了解完整流程
- **[详细的演练](#-详细流程)** - 分步实现指南

---

## 📋 详细流程

<details>
<summary>点击展开详细的分步演练</summary>

你可以使用 Specify CLI 来引导你的项目，这将在你的环境中引入所需的工件。运行：

```bash
specify init <project_name>
```

或在当前目录中初始化：

```bash
specify init .
# 或使用 --here 标志
specify init --here
# 跳过确认，当目录已经有文件时
specify init . --force
# 或
specify init --here --force
```

![Specify CLI 在终端中引导新项目](./media/specify_cli.gif)

系统会提示你选择你正在使用的 AI 代理。你也可以直接在终端中主动指定它：

```bash
specify init <project_name> --ai claude
specify init <project_name> --ai gemini
specify init <project_name> --ai copilot

# 或在当前目录中：
specify init . --ai claude
specify init . --ai codex

# 或使用 --here 标志
specify init --here --ai claude
specify init --here --ai codex

# 强制合并到非空当前目录
specify init . --force --ai claude

# 或
specify init --here --force --ai claude
```

CLI 将检查你是否安装了 Claude Code、Gemini CLI、Cursor CLI、Qwen CLI、opencode、Codex CLI、Qoder CLI 或 Amazon Q Developer CLI。如果你没有，或者你更喜欢在不检查正确工具的情况下获取模板，请使用 `--ignore-agent-tools` 和你的命令：

```bash
specify init <project_name> --ai claude --ignore-agent-tools
```

### **步骤 1：建立项目原则**

进入项目文件夹并运行你的 AI 代理。在我们的示例中，我们使用 `claude`。

![引导 Claude Code 环境](./media/bootstrap-claude-code.gif)

如果你看到 `/speckit.constitution`、`/speckit.specify`、`/speckit.plan`、`/speckit.tasks` 和 `/speckit.implement` 命令可用，你就会知道事情配置正确。

第一步应该是使用 `/speckit.constitution` 命令建立你的项目的治理原则。这有助于确保在所有后续开发阶段中做出一致的决策：

```text
/speckit.constitution 创建专注于代码质量、测试标准、用户体验一致性和性能要求的原则。包括这些原则应如何指导技术决策和实现选择的治理。
```

此步骤使用你的项目的基础指南创建或更新 `.specify/memory/constitution.md` 文件，AI 代理将在规格说明、规划和实现阶段引用这些指南。

### **步骤 2：创建项目规格说明**

建立项目原则后，你现在可以创建功能规格说明。使用 `/speckit.specify` 命令，然后提供你想要开发的项目的具体需求。

> [!IMPORTANT]
> 尽可能明确地说明你想要构建**什么**以及**为什么**。**此时不要关注技术栈**。

一个示例提示：

```text
开发 Taskify，一个团队生产力平台。它应该允许用户创建项目、添加团队成员、
分配任务、评论和在 Kanban 风格的板之间移动任务。在此初始阶段，对于此功能，
让我们称之为"创建 Taskify"，让我们有多个用户，但用户将提前声明，预定义。
我想要两个不同类别的五个用户，一个产品经理和四个工程师。让我们创建三个
不同的示例项目。让我们为任务的状态使用标准 Kanban 列，例如"待办"、
"进行中"、"审查中"和"完成"。此应用程序没有登录，因为这只是第一个测试内容
以确保我们的基本功能已设置。对于 UI 中任务卡的每个任务，
你应该能够在 Kanban 工作板的不同列之间更改任务的当前状态。
你应该能够为特定卡留下无限数量的评论。你应该能够从该任务
卡中分配一个有效用户。当你首次启动 Taskify 时，它会给你一个五个用户的列表来选择
从。不需要密码。当你点击用户时，你进入主视图，显示项目列表。当你点击项目时，
你打开该项目的 Kanban 板。你会看到列。
你将能够在不同列之间拖放卡。你会看到分配给你的任何卡，当前登录的用户，
与所有其他卡的颜色不同，所以你可以快速看到你的。你可以编辑你做的任何评论，
但你不能编辑其他人做的评论。你可以删除你做的任何评论，但你不能删除任何人做的评论。
```

输入此提示后，你应该看到 Claude Code 启动规划和规格说明起草过程。Claude Code 还会触发一些内置脚本来设置仓库。

完成此步骤后，你应该创建了一个新分支（例如 `001-create-taskify`），以及 `specs/001-create-taskify` 目录中的新规格说明。

生成的规格说明应该包含一组用户故事和功能需求，如模板中定义的那样。

此时，你的项目文件夹内容应该类似于以下内容：

```text
└── .specify
    ├── memory
    │  └── constitution.md
    ├── scripts
    │  ├── check-prerequisites.sh
    │  ├── common.sh
    │  ├── create-new-feature.sh
    │  ├── setup-plan.sh
    │  └── update-claude-md.sh
    ├── specs
    │  └── 001-create-taskify
    │      └── spec.md
    └── templates
        ├── plan-template.md
        ├── spec-template.md
        └── tasks-template.md
```

### **步骤 3：功能规格说明澄清（规划前必需）

创建基线规格说明后，你可以继续澄清规格说明中未正确捕获的任何需求。

你应该在创建技术计划之前运行结构化澄清工作流，以减少下游返工。

首选顺序：

1. 使用 `/speckit.clarify`（结构化）– 顺序、基于覆盖范围的提问，将答案记录在澄清部分。
2. 可选地进行临时自由形式的细化，如果仍然有些模糊的话。

如果你有意想跳过澄清（例如，尖峰或探索性原型），请明确说明，以便代理不会在缺少澄清的情况下阻止。

自由形式细化提示示例（如果仍然需要，在 `/speckit.clarify` 之后）：

```text
对于你创建的每个示例项目或项目，应该有 5 到 15 个任务之间的可变数量
对于每个任务，随机分布到不同的完成状态。确保每个完成阶段至少有一个任务。
```

你还应该要求 Claude Code 验证**审查和验收检查清单**，检查验证/通过需求的内容，并留下未检查的内容。可以使用以下提示：

```text
读取审查和验收检查清单，如果功能规格说明符合标准，则检查清单中的每一项。如果不符合，则将其留空。
```

重要的是要将与 Claude Code 的互动作为澄清和提出关于规格说明的问题的机会——**不要将其第一次尝试视为最终**。

### **步骤 4：生成计划**

现在你可以具体说明技术栈和其他技术要求。你可以使用 `/speckit.plan` 命令，提示如下：

```text
我们将使用 .NET Aspire 生成这个，使用 Postgres 作为数据库。前端应该使用
Blazor 服务器，带有拖放任务板、实时更新。应该创建一个 REST API，包含项目 API、
任务 API 和通知 API。
```

此步骤的输出将包括许多实现详细文档，你的目录树将类似于：

```text
.
├── CLAUDE.md
├── memory
│  └── constitution.md
├── scripts
│  ├── check-prerequisites.sh
│  ├── common.sh
│  ├── create-new-feature.sh
│  ├── setup-plan.sh
│  └── update-claude-md.sh
├── specs
│  └── 001-create-taskify
│      ├── contracts
│      │  ├── api-spec.json
│      │  └── signalr-spec.md
│      ├── data-model.md
│      ├── plan.md
│      ├── quickstart.md
│      ├── research.md
│      └── spec.md
└── templates
    ├── CLAUDE-template.md
    ├── plan-template.md
    ├── spec-template.md
    └── tasks-template.md
```

检查 `research.md` 文档以确保根据你的说明使用了正确的技术栈。如果任何组件突出，你可以要求 Claude Code 细化它，或者甚至让它检查你想要使用的平台/框架的本地安装版本（例如 .NET）。

此外，如果技术栈快速变化（例如 .NET Aspire、JS 框架），你可能想要要求 Claude Code 研究所选技术栈的详细信息，提示如下：

```text
我想要你通过实现计划和实现细节，寻找可能受益于额外研究的区域，因为 .NET Aspire 是一个快速变化的库。对于你识别需要进一步研究的那些区域，我想要你用关于我们将在此 Taskify 应用程序中使用的特定版本的额外详细信息更新研究文档，并生成平行研究任务以澄清
任何详细信息使用网络研究。
```

在此过程中，你可能会发现 Claude Code 过于热情并添加了你没有要求的组件——你可以帮助用改进的提示将其推向正确的方向：

```text
我认为我们需要将其分解为一系列步骤。首先，识别你在实现过程中需要做的任务列表
你不确定或会受益于进一步研究。写下这些任务的列表。然后对于每一个这些任务，
我想要你生成一个单独的研究任务，以便最终结果是我们并行研究
所有那些非常具体的任务。我看到你在做的是你在一般研究 .NET Aspire，我不认为
那会为我们做很多。那太不针对性了。研究需要帮助你解决一个特定的有针对性的问题。
```

> [!NOTE]
> Claude Code 可能过于热情并添加你没有要求的组件。要求它澄清理由和变更的来源。

### **步骤 5：让 Claude Code 验证计划**

计划就位后，你应该让 Claude Code 通过它来确保没有遗漏的部分。你可以使用这样的提示：

```text
现在我想要你去审计实现计划和实现细节文件。
阅读它，眼睛盯着确定是否有一个明显的任务序列
你需要做的。因为我不知道这里是否足够。例如，
当我看核心实现时，引用实现中的适当位置会很有用
细节，它可以在走过每一步时找到信息
核心实现或细化。
```

这有助于细化实现计划并帮助你避免 Claude Code 在其规划周期中遗漏的潜在盲点。一旦初始细化通过完成，要求 Claude Code 再次通过检查清单，然后你可以进行实现。

你也可以要求 Claude Code（如果你安装了 [GitHub CLI](https://docs.github.com/en/github-cli/github-cli)）继续并从你的当前分支创建一个拉取请求到 `main`，并提供详细的描述，以确保工作得到适当的跟踪。

> [!NOTE]
> 在代理实现它之前，也值得提示 Claude Code 交叉检查细节以查看是否有任何过度工程的部分（记住——它可能过于热情）。如果存在过度工程的组件或决策，你可以要求 Claude Code 解决它们。确保 Claude Code 遵循[宪法](base/memory/constitution.md)作为它在建立计划时必须遵守的基础部分。

### **步骤 6：使用 /speckit.tasks 生成任务分解**

验证实现计划后，你现在可以将计划分解为可以按正确顺序执行的特定、可操作的任务。使用 `/speckit.tasks` 命令从你的实现计划自动生成详细的任务分解：

```text
/speckit.tasks
```

此步骤在你的功能规格说明目录中创建一个 `tasks.md` 文件，其中包含：

- **按用户故事组织的任务分解** - 每个用户故事成为一个单独的实现阶段，有自己的一组任务
- **依赖关系管理** - 任务被排序以尊重组件之间的依赖关系（例如，模型在服务之前，服务在端点之前）
- **并行执行标记** - 可以并行运行的任务用 `[P]` 标记以优化开发工作流
- **文件路径规范** - 每个任务包括实现应该发生的确切文件路径
- **测试驱动开发结构** - 如果请求了测试，测试任务包括在内并排序为在实现前编写
- **检查点验证** - 每个用户故事阶段包括检查点以验证独立功能

生成的 tasks.md 为 `/speckit.implement` 命令提供了清晰的路线图，确保系统实现维护代码质量并允许用户故事的增量交付。

### **步骤 7：实现**

准备好后，使用 `/speckit.implement` 命令执行你的实现计划：

```text
/speckit.implement
```

`/speckit.implement` 命令将：

- 验证所有前置条件都已就位（宪法、规格说明、计划和任务）
- 从 `tasks.md` 解析任务分解
- 按正确的顺序执行任务，尊重依赖关系和并行执行标记
- 遵循你的任务计划中定义的 TDD 方法
- 提供进度更新并适当处理错误

> [!IMPORTANT]
> AI 代理将执行本地 CLI 命令（例如 `dotnet`、`npm` 等）——确保你的机器上安装了所需的工具。

实现完成后，测试应用程序并解决 CLI 日志中可能不可见的任何运行时错误（例如浏览器控制台错误）。你可以将此类错误复制并粘贴回你的 AI 代理以进行解决。

</details>

---

## 🔍 故障排除

### Linux 上的 Git 凭证管理器

如果你在 Linux 上遇到 Git 身份验证问题，你可以安装 Git 凭证管理器：

```bash
#!/usr/bin/env bash
set -e
echo "下载 Git 凭证管理器 v2.6.1..."
wget https://github.com/git-ecosystem/git-credential-manager/releases/download/v2.6.1/gcm-linux_amd64.2.6.1.deb
echo "安装 Git 凭证管理器..."
sudo dpkg -i gcm-linux_amd64.2.6.1.deb
echo "配置 Git 使用 GCM..."
git config --global credential.helper manager
echo "清理..."
rm gcm-linux_amd64.2.6.1.deb
```

## 👥 维护者

- Den Delimarsky ([@localden](https://github.com/localden))
- John Lam ([@jflam](https://github.com/jflam))

## 💬 支持

如需支持，请开启 [GitHub issue](https://github.com/github/spec-kit/issues/new)。我们欢迎错误报告、功能请求和关于使用规格驱动开发的问题。

## 🙏 致谢

此项目深受 [John Lam](https://github.com/jflam) 的工作和研究的影响和基础。

## 📄 许可证

此项目根据 MIT 开源许可证的条款获得许可。请参阅 [LICENSE](./LICENSE) 文件了解完整条款。
