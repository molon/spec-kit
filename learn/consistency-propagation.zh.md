# 一致性传播机制

## 概述

一致性传播是 Spec Kit 中的核心机制，确保宪法（`/memory/constitution.md`）的变化能够传播到所有依赖的模板和工件。

**关键命令**：`/speckit.constitution` 是执行一致性传播的**主要命令**。当你修改宪法后，应该运行这个命令来触发一致性传播。

宪法的一致性通过以下方式实现：

1. **`/speckit.constitution`**：更新宪法时执行一致性传播检查清单，更新相关模板
2. **`/speckit.plan`**：生成计划时读取宪法并应用门控
3. **`/speckit.analyze`**：分析时验证工件与宪法的一致性

## 核心概念

### 什么是一致性传播？

在 Spec Kit 中，一致性传播是指**宪法原则如何通过命令执行传递到生成的工件**。这不是一个自动化的系统，而是一个设计原则：

- **宪法是真实来源**：定义项目的核心原则和约束
- **命令读取宪法**：某些命令在执行时会读取宪法内容
- **工件反映宪法**：生成的工件应该符合宪法原则

### 哪些命令会读取/更新宪法？

根据实际的命令文件，以下命令与宪法相关：

1. **`/speckit.constitution`**（`templates/commands/constitution.md`）⭐ **一致性传播的核心命令**：
   - 创建或更新宪法文件
   - **执行一致性传播检查清单**：
     - 读取 `/templates/plan-template.md` 确保 "Constitution Check" 与更新的原则对齐
     - 读取 `/templates/spec-template.md` 检查范围/需求对齐
     - 读取 `/templates/tasks-template.md` 确保任务分类反映新原则
     - 读取 `/templates/commands/*.md` 验证无过时引用
     - 读取 README.md、docs/quickstart.md 等文档，更新原则引用
   - **生成同步影响报告**：版本变化、修改的原则、需要更新的模板
   - 更新版本号（遵循语义版本控制）

2. **`/speckit.plan`**（`templates/commands/plan.md`）：
   - 在第 2 步明确指出：`Load context: Read FEATURE_SPEC and /memory/constitution.md`
   - 填充 "Constitution Check" 部分
   - 评估宪法门控

3. **`/speckit.analyze`**（`templates/commands/analyze.md`）：
   - 在第 2 步明确指出：`Load /memory/constitution.md for principle validation`
   - 检查宪法对齐问题
   - 宪法冲突被标记为 CRITICAL

### 哪些命令不读取宪法？

以下命令**不直接读取**宪法：

- **`/speckit.specify`**：专注于从用户描述创建规格说明，不涉及宪法
- **`/speckit.clarify`**：专注于识别规格说明中的歧义，不涉及宪法
- **`/speckit.tasks`**：从计划生成任务，不直接读取宪法
- **`/speckit.checklist`**：生成检查清单，不涉及宪法
- **`/speckit.implement`**：执行任务，不直接读取宪法
- **`/speckit.taskstoissues`**：转换任务为 GitHub Issues，不涉及宪法

## `/speckit.constitution` 命令详解

这是执行一致性传播的**核心命令**。根据 `templates/commands/constitution.md` 的实际内容：

### 主要功能

```markdown
You are updating the project constitution at `/memory/constitution.md`. This file is a TEMPLATE
containing placeholder tokens in square brackets (e.g. `[PROJECT_NAME]`, `[PRINCIPLE_1_NAME]`).
Your job is to (a) collect/derive concrete values, (b) fill the template precisely, and
(c) propagate any amendments across dependent artifacts.
```

### 一致性传播检查清单（第 4 步）

```markdown
4. Consistency propagation checklist (convert prior checklist into active validations):
   - Read `/templates/plan-template.md` and ensure any "Constitution Check" or rules align
     with updated principles.
   - Read `/templates/spec-template.md` for scope/requirements alignment—update if constitution
     adds/removes mandatory sections or constraints.
   - Read `/templates/tasks-template.md` and ensure task categorization reflects new or removed
     principle-driven task types (e.g., observability, versioning, testing discipline).
   - Read each command file in `/templates/commands/*.md` (including this one) to verify no
     outdated references remain when generic guidance is required.
   - Read any runtime guidance docs (e.g., `README.md`, `docs/quickstart.md`, or agent-specific
     guidance files if present). Update references to principles changed.
```

### 同步影响报告（第 5 步）

```markdown
5. Produce a Sync Impact Report (prepend as an HTML comment at top of the constitution file):
   - Version change: old → new
   - List of modified principles (old title → new title if renamed)
   - Added sections
   - Removed sections
   - Templates requiring updates (✅ updated / ⚠ pending) with file paths
   - Follow-up TODOs if any placeholders intentionally deferred.
```

### 版本控制

```markdown
`CONSTITUTION_VERSION` must increment according to semantic versioning rules:

- MAJOR: Backward incompatible governance/principle removals or redefinitions.
- MINOR: New principle/section added or materially expanded guidance.
- PATCH: Clarifications, wording, typo fixes, non-semantic refinements.
```

---

## 其他宪法相关命令

### `/speckit.plan` 命令

根据 `templates/commands/plan.md` 的实际内容：

```markdown
2. **Load context**: Read FEATURE_SPEC and `/memory/constitution.md`. Load IMPL_PLAN template (already copied).

3. **Execute plan workflow**: Follow the structure in IMPL_PLAN template to:
   - Fill Technical Context (mark unknowns as "NEEDS CLARIFICATION")
   - Fill Constitution Check section from constitution
   - Evaluate gates (ERROR if violations unjustified)
     ...
   - Re-evaluate Constitution Check post-design
```

**实际行为**：

- 读取宪法文件
- 填充计划模板中的 "Constitution Check" 部分
- 评估宪法门控，如果有未证明的违规则报错
- 在设计完成后重新评估宪法检查

### `/speckit.analyze` 命令

根据 `templates/commands/analyze.md` 的实际内容：

```markdown
**From constitution:**

- Load `/memory/constitution.md` for principle validation

...

#### D. Constitution Alignment

- Any requirement or plan element conflicting with a MUST principle
- Missing mandated sections or quality gates from constitution

...

**Constitution Authority**: The project constitution (`/memory/constitution.md`) is **non-negotiable** within this analysis scope. Constitution conflicts are automatically CRITICAL...
```

**实际行为**：

- 读取宪法文件进行原则验证

## 宪法更新后的工作流

### 推荐流程：使用 `/speckit.constitution`

当你需要修改宪法时，**推荐使用 `/speckit.constitution` 命令**，而不是直接编辑文件：

```bash
# 1. 运行 constitution 命令，描述你要做的修改
/speckit.constitution 添加新的安全原则：所有用户输入必须验证

# 该命令会：
# - 更新 /memory/constitution.md
# - 执行一致性传播检查清单
# - 更新相关模板文件
# - 生成同步影响报告
# - 更新版本号
```

### 如果直接编辑了宪法文件

如果你直接编辑了 `/memory/constitution.md` 文件，之后应该运行：

```bash
# 1. 运行 constitution 命令来触发一致性传播
/speckit.constitution 确认并传播最近的宪法修改

# 2. 检查现有工件是否符合新宪法
/speckit.analyze
```

### 验证现有工件

运行 `/speckit.analyze` 命令检查现有工件是否符合新宪法：

```bash
/speckit.analyze
```

这会：

- 读取最新的宪法
- 检查 spec.md、plan.md、tasks.md 是否符合宪法
- 报告任何宪法违规（标记为 CRITICAL）

### 更新受影响的工件

如果发现违规，需要重新生成受影响的工件：

```bash
# 重新生成计划（会读取新宪法）
/speckit.plan

# 重新生成任务
/speckit.tasks
```

### 新功能自动应用新宪法

后续创建的新功能会自动使用新宪法：

- `/speckit.plan` 会读取新宪法并应用门控
- `/speckit.analyze` 会使用新宪法进行验证

## 计划模板中的宪法检查

根据 `templates/plan-template.md` 的实际内容：

```markdown
## Constitution Check

_Gating: Must pass before Phase 0 research. Re-check after Phase 1 design._

[Gates determined based on constitution file]
```

**实际行为**：

- 计划模板包含一个 "Constitution Check" 部分
- `/speckit.plan` 命令会根据宪法文件填充这个部分
- 门控在第 0 阶段研究前必须通过
- 在第 1 阶段设计后需要重新检查

## 依赖关系图

以下是宪法与命令之间的**实际**依赖关系：

```
constitution.md（真实来源）
    │
    ├─→ /speckit.constitution 命令（读取 + 更新 + 一致性传播）⭐
    │   ├─→ 更新 constitution.md
    │   ├─→ 更新 templates/plan-template.md
    │   ├─→ 更新 templates/spec-template.md
    │   ├─→ 更新 templates/tasks-template.md
    │   ├─→ 更新 templates/commands/*.md
    │   └─→ 生成同步影响报告
    │
    ├─→ /speckit.plan 命令（直接读取）
    │   └─→ specs/*/plan.md（包含 Constitution Check 部分）
    │
    └─→ /speckit.analyze 命令（直接读取）
        └─→ 一致性报告（包含宪法对齐问题）

注意：以下命令不直接读取宪法：
- /speckit.specify
- /speckit.clarify
- /speckit.tasks
- /speckit.checklist
- /speckit.implement
- /speckit.taskstoissues
```

## 最佳实践

### 1. 使用 `/speckit.constitution` 修改宪法

```bash
# 推荐：使用命令修改宪法，自动触发一致性传播
/speckit.constitution 添加新的安全原则
```

### 2. 宪法更新后运行分析

```bash
# 更新宪法后，检查现有工件
/speckit.analyze
```

### 3. 重新生成受影响的计划

```bash
# 如果分析发现宪法违规，重新生成计划
/speckit.plan
```

### 4. 在宪法中记录变化

在宪法文件中维护修订历史：

```markdown
**版本**：2.1.0 | **批准**：2025-06-13 | **最后修订**：2025-07-16
```

### 5. 在 PR 中验证一致性（手动最佳实践）

> **注意**：这是一个**手动最佳实践建议**，Spec Kit 目前没有 CI 集成或自动化 PR 检查。

在提交 PR 前，建议：

1. 手动运行 `/speckit.analyze` 检查一致性
2. 在 PR 描述中手动添加以下检查清单：

```markdown
## PR 检查清单（手动填写）

- [ ] 已运行 /speckit.analyze 检查宪法对齐
- [ ] 计划通过所有宪法门控
- [ ] 无 CRITICAL 级别的宪法违规
```

## 限制和注意事项

### 当前限制

1. **没有自动化传播**：宪法更新后不会自动更新现有工件
2. **没有自动通知**：宪法更新后不会自动通知开发人员
3. **没有 git hooks**：没有自动触发一致性检查的 git hooks
4. **部分命令不读取宪法**：`/speckit.specify`、`/speckit.clarify` 等命令不直接读取宪法

### 注意事项

1. **手动验证是必需的**：宪法更新后需要手动运行 `/speckit.analyze`
2. **现有工件需要手动更新**：宪法更新后需要手动重新生成受影响的工件
3. **宪法冲突是 CRITICAL**：`/speckit.analyze` 会将宪法冲突标记为 CRITICAL 级别

## 总结

Spec Kit 的一致性传播机制是**基于命令执行的**：

- ✅ **`/speckit.constitution`** 是一致性传播的核心命令，更新宪法时会自动传播到模板
- ✅ `/speckit.plan` 在执行时读取宪法并应用门控
- ✅ `/speckit.analyze` 在执行时读取宪法并检查一致性
- ❌ 没有自动化的宪法变化检测（需要手动运行命令）
- ❌ 现有的 spec/plan/tasks 工件不会自动更新

**推荐工作流**：

1. 使用 `/speckit.constitution` 修改宪法（自动触发一致性传播到模板）
2. 运行 `/speckit.analyze` 检查现有工件是否符合新宪法
3. 根据分析结果，使用 `/speckit.plan` 和 `/speckit.tasks` 重新生成受影响的工件
