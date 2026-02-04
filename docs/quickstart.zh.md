# 快速入门指南

本指南将帮助你使用 Spec Kit 开始规格驱动开发。

> [!NOTE]
> 所有自动化脚本现在提供 Bash（`.sh`）和 PowerShell（`.ps1`）变体。`specify` CLI 根据操作系统自动选择，除非你传递 `--script sh|ps`。

## 6 步流程

> [!TIP]
> **上下文感知**：Spec Kit 命令根据你的当前 Git 分支（例如 `001-feature-name`）自动检测活跃功能。要在不同规格说明之间切换，只需切换 Git 分支。

### 步骤 1：安装 Specify

**在你的终端中**，运行 `specify` CLI 命令来初始化你的项目：

```bash
# 创建新项目目录
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME>

# 或在当前目录中初始化
uvx --from git+https://github.com/github/spec-kit.git specify init .
```

明确选择脚本类型（可选）：

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME> --script ps  # 强制 PowerShell
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME> --script sh  # 强制 POSIX shell
```

### 步骤 2：定义你的宪法

**在你的 AI 代理的聊天界面中**，使用 `/speckit.constitution` 斜杠命令建立你的项目的核心规则和原则。你应该提供你的项目的具体原则作为参数。

```markdown
/speckit.constitution 此项目遵循"库优先"方法。所有功能必须首先作为独立库实现。我们严格使用 TDD。我们更喜欢函数式编程模式。
```

### 步骤 3：创建规格说明

**在聊天中**，使用 `/speckit.specify` 斜杠命令描述你想要构建的内容。专注于**什么**和**为什么**，而不是技术栈。

```markdown
/speckit.specify 构建一个应用程序，帮助我将照片组织到单独的相册中。相册按日期分组，可以在主页上通过拖放重新组织。相册永远不会在其他嵌套相册中。在每个相册中，照片以瓷砖式界面预览。
```

### 步骤 4：细化规格说明

**在聊天中**，使用 `/speckit.clarify` 斜杠命令识别并解决规格说明中的歧义。你可以提供特定的焦点区域作为参数。

```bash
/speckit.clarify 专注于安全和性能要求。
```

### 步骤 5：创建技术实现计划

**在聊天中**，使用 `/speckit.plan` 斜杠命令提供你的技术栈和架构选择。

```markdown
/speckit.plan 应用程序使用 Vite，库数量最少。尽可能使用原生 HTML、CSS 和 JavaScript。图像不上传到任何地方，元数据存储在本地 SQLite 数据库中。
```

### 步骤 6：分解并实现

**在聊天中**，使用 `/speckit.tasks` 斜杠命令创建可操作的任务列表。

```markdown
/speckit.tasks
```

可选地，使用 `/speckit.analyze` 验证计划：

```markdown
/speckit.analyze
```

然后，使用 `/speckit.implement` 斜杠命令执行计划。

```markdown
/speckit.implement
```

## 详细示例：构建 Taskify

以下是构建团队生产力平台的完整示例：

### 步骤 1：定义宪法

初始化项目的宪法以设置基本规则：

```markdown
/speckit.constitution Taskify 是一个"安全优先"应用程序。所有用户输入必须被验证。我们使用微服务架构。代码必须完全记录。
```

### 步骤 2：使用 `/speckit.specify` 定义需求

```text
开发 Taskify，一个团队生产力平台。它应该允许用户创建项目、添加团队成员、
分配任务、评论和在 Kanban 风格的板之间移动任务。在此初始阶段，对于此功能，
让我们称之为"创建 Taskify"，让我们有多个用户，但用户将提前声明，预定义。
我想要两个不同类别的五个用户，一个产品经理和四个工程师。让我们创建三个
不同的示例项目。让我们为任务的状态使用标准 Kanban 列，例如"待办"、
"进行中"、"审查中"和"完成"。此应用程序没有登录，因为这只是第一个测试内容
以确保我们的基本功能已设置。
```

### 步骤 3：细化规格说明

使用 `/speckit.clarify` 命令交互式解决规格说明中的任何歧义。你也可以提供你想要确保包括的具体细节。

```bash
/speckit.clarify 我想澄清任务卡详细信息。对于 UI 中任务卡的每个任务，你应该能够在 Kanban 工作板的不同列之间更改任务的当前状态。你应该能够为特定卡留下无限数量的评论。你应该能够从该任务卡中分配一个有效用户。
```

你可以继续使用 `/speckit.clarify` 细化规格说明：

```bash
/speckit.clarify 当你首次启动 Taskify 时，它会给你一个五个用户的列表来选择。不需要密码。当你点击用户时，你进入主视图，显示项目列表。当你点击项目时，你打开该项目的 Kanban 板。你会看到列。你将能够在不同列之间拖放卡。你会看到分配给你的任何卡，当前登录的用户，与所有其他卡的颜色不同，所以你可以快速看到你的。你可以编辑你做的任何评论，但你不能编辑其他人做的评论。你可以删除你做的任何评论，但你不能删除任何人做的评论。
```

### 步骤 4：验证规格说明

使用 `/speckit.checklist` 命令验证规格说明检查清单：

```bash
/speckit.checklist
```

### 步骤 5：使用 `/speckit.plan` 生成技术计划

具体说明你的技术栈和技术要求：

```bash
/speckit.plan 我们将使用 .NET Aspire 生成这个，使用 Postgres 作为数据库。前端应该使用 Blazor 服务器，带有拖放任务板、实时更新。应该创建一个 REST API，包含项目 API、任务 API 和通知 API。
```

### 步骤 6：验证并实现

使用 `/speckit.analyze` 命令让你的 AI 代理审计实现计划：

```bash
/speckit.analyze
```

最后，实现解决方案：

```bash
/speckit.implement
```

## 关键原则

- **明确**你正在构建什么以及为什么
- **不要关注技术栈**在规格说明阶段
- **迭代和细化**你的规格说明在实现前
- **验证**计划在编码开始前
- **让 AI 代理处理**实现细节

## 后续步骤

- 阅读[完整方法论](../spec-driven.md)以获取深入指导
- 查看仓库中的[更多示例](../templates)
- 探索 [GitHub 上的源代码](https://github.com/github/spec-kit)
