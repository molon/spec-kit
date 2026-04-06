# 快速入门指南

本指南将帮助你使用 Spec Kit 开始规格驱动开发。

> [!NOTE]
> 所有自动化脚本现在提供 Bash（`.sh`）和 PowerShell（`.ps1`）两种变体。`specify` CLI 根据操作系统自动选择，除非你传递 `--script sh|ps`。

## 6 步流程

> [!TIP]
> **上下文感知**：Spec Kit 命令根据你当前的 Git 分支（例如 `001-feature-name`）自动检测活跃功能。要在不同规格说明之间切换，只需切换 Git 分支。

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
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME> --script ps  # Force PowerShell
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME> --script sh  # Force POSIX shell
```

### 步骤 2：定义你的宪法

**在你的 AI 代理的聊天界面中**，使用 `/speckit.constitution` 斜杠命令来建立项目的核心规则和原则。你应该将项目的具体原则作为参数提供。

```markdown
/speckit.constitution This project follows a "Library-First" approach. All features must be implemented as standalone libraries first. We use TDD strictly. We prefer functional programming patterns.
```

### 步骤 3：创建规格说明

**在聊天中**，使用 `/speckit.specify` 斜杠命令描述你想要构建的内容。专注于**做什么**和**为什么**，而不是技术栈。

```markdown
/speckit.specify Build an application that can help me organize my photos in separate photo albums. Albums are grouped by date and can be re-organized by dragging and dropping on the main page. Albums are never in other nested albums. Within each album, photos are previewed in a tile-like interface.
```

### 步骤 4：细化规格说明

**在聊天中**，使用 `/speckit.clarify` 斜杠命令来识别并解决规格说明中的歧义。你可以将特定的关注领域作为参数提供。

```bash
/speckit.clarify Focus on security and performance requirements.
```

### 步骤 5：创建技术实现计划

**在聊天中**，使用 `/speckit.plan` 斜杠命令来提供你的技术栈和架构选择。

```markdown
/speckit.plan The application uses Vite with minimal number of libraries. Use vanilla HTML, CSS, and JavaScript as much as possible. Images are not uploaded anywhere and metadata is stored in a local SQLite database.
```

### 步骤 6：分解并实现

**在聊天中**，使用 `/speckit.tasks` 斜杠命令来创建可操作的任务列表。

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

> [!TIP]
> **分阶段实现**：对于复杂项目，分阶段实现以避免使代理的上下文过载。从核心功能开始，验证其正常工作，然后逐步添加功能。

## 详细示例：构建 Taskify

以下是构建团队生产力平台的完整示例：

### 步骤 1：定义宪法

初始化项目的宪法以设定基本规则：

```markdown
/speckit.constitution Taskify is a "Security-First" application. All user inputs must be validated. We use a microservices architecture. Code must be fully documented.
```

### 步骤 2：使用 `/speckit.specify` 定义需求

```text
Develop Taskify, a team productivity platform. It should allow users to create projects, add team members,
assign tasks, comment and move tasks between boards in Kanban style. In this initial phase for this feature,
let's call it "Create Taskify," let's have multiple users but the users will be declared ahead of time, predefined.
I want five users in two different categories, one product manager and four engineers. Let's create three
different sample projects. Let's have the standard Kanban columns for the status of each task, such as "To Do,"
"In Progress," "In Review," and "Done." There will be no login for this application as this is just the very
first testing thing to ensure that our basic features are set up.
```

### 步骤 3：细化规格说明

使用 `/speckit.clarify` 命令交互式地解决规格说明中的任何歧义。你也可以提供想要确保包含的具体细节。

```bash
/speckit.clarify I want to clarify the task card details. For each task in the UI for a task card, you should be able to change the current status of the task between the different columns in the Kanban work board. You should be able to leave an unlimited number of comments for a particular card. You should be able to, from that task card, assign one of the valid users.
```

你可以继续使用 `/speckit.clarify` 提供更多细节来细化规格说明：

```bash
/speckit.clarify When you first launch Taskify, it's going to give you a list of the five users to pick from. There will be no password required. When you click on a user, you go into the main view, which displays the list of projects. When you click on a project, you open the Kanban board for that project. You're going to see the columns. You'll be able to drag and drop cards back and forth between different columns. You will see any cards that are assigned to you, the currently logged in user, in a different color from all the other ones, so you can quickly see yours. You can edit any comments that you make, but you can't edit comments that other people made. You can delete any comments that you made, but you can't delete comments anybody else made.
```

### 步骤 4：验证规格说明

使用 `/speckit.checklist` 命令验证规格说明检查清单：

```bash
/speckit.checklist
```

### 步骤 5：使用 `/speckit.plan` 生成技术计划

具体说明你的技术栈和技术要求：

```bash
/speckit.plan We are going to generate this using .NET Aspire, using Postgres as the database. The frontend should use Blazor server with drag-and-drop task boards, real-time updates. There should be a REST API created with a projects API, tasks API, and a notifications API.
```

### 步骤 6：定义任务

使用 `/speckit.tasks` 命令生成可操作的任务列表：

```bash
/speckit.tasks
```

### 步骤 7：验证并实现

使用 `/speckit.analyze` 让你的 AI 代理审计实现计划：

```bash
/speckit.analyze
```

最后，实现解决方案：

```bash
/speckit.implement
```

> [!TIP]
> **分阶段实现**：对于像 Taskify 这样的大型项目，考虑分阶段实现（例如，第 1 阶段：基本项目/任务结构，第 2 阶段：看板功能，第 3 阶段：评论和分配）。这可以防止上下文饱和，并允许在每个阶段进行验证。

## 关键原则

- **明确**你正在构建什么以及为什么
- 在规格说明阶段**不要关注技术栈**
- 在实现之前**迭代和细化**你的规格说明
- 在编码开始之前**验证**计划
- **让 AI 代理处理**实现细节

## 后续步骤

- 阅读[完整方法论](https://github.com/github/spec-kit/blob/main/spec-driven.md)以获取深入指导
- 查看仓库中的[更多示例](https://github.com/github/spec-kit/tree/main/templates)
- 探索 [GitHub 上的源代码](https://github.com/github/spec-kit)
