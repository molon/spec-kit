# 一致性传播机制

## 概述

一致性传播是 Spec Kit 中的一个**概念性机制**，描述了宪法（`/memory/constitution.md`）如何通过命令执行影响生成的工件。

**重要说明**：Spec Kit 目前**没有自动化的一致性传播系统**。宪法的一致性是通过以下方式实现的：

1. **命令执行时读取宪法**：`/speckit.plan` 和 `/speckit.analyze` 命令在执行时会读取当前的宪法
2. **手动验证**：开发人员需要手动运行 `/speckit.analyze` 来检查一致性
3. **手动更新**：宪法变化后，现有工件需要手动重新生成

## 核心概念

### 什么是一致性传播？

在 Spec Kit 中，一致性传播是指**宪法原则如何通过命令执行传递到生成的工件**。这不是一个自动化的系统，而是一个设计原则：

- **宪法是真实来源**：定义项目的核心原则和约束
- **命令读取宪法**：某些命令在执行时会读取宪法内容
- **工件反映宪法**：生成的工件应该符合宪法原则

### 哪些命令会读取宪法？

根据实际的命令文件，以下命令会读取 `/memory/constitution.md`：

1. **`/speckit.plan`**（`templates/commands/plan.md`）：
   - 在第 2 步明确指出：`Load context: Read FEATURE_SPEC and /memory/constitution.md`
   - 填充 "Constitution Check" 部分
   - 评估宪法门控

2. **`/speckit.analyze`**（`templates/commands/analyze.md`）：
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

## 宪法在命令中的实际使用

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
- 检查需求或计划元素是否与 MUST 原则冲突
- 检查是否缺少宪法要求的部分或质量门控
- 宪法冲突自动标记为 CRITICAL 级别

## 宪法更新后的工作流

由于 Spec Kit **没有自动化的一致性传播**，宪法更新后需要手动操作：

### 1. 更新宪法

手动编辑 `/memory/constitution.md` 文件。

### 2. 检查现有工件

运行 `/speckit.analyze` 命令检查现有工件是否符合新宪法：

```bash
/speckit.analyze
```

这会：

- 读取最新的宪法
- 检查 spec.md、plan.md、tasks.md 是否符合宪法
- 报告任何宪法违规（标记为 CRITICAL）

### 3. 手动更新工件

如果发现违规，需要手动更新受影响的工件：

```bash
# 重新生成计划（会读取新宪法）
/speckit.plan

# 重新生成任务
/speckit.tasks
```

### 4. 新功能自动应用新宪法

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
```

## 最佳实践

### 1. 宪法更新后运行分析

```bash
# 更新宪法后，检查现有工件
/speckit.analyze
```

### 2. 重新生成受影响的计划

```bash
# 如果分析发现宪法违规，重新生成计划
/speckit.plan
```

### 3. 在宪法中记录变化

在宪法文件中维护修订历史：

```markdown
**版本**：2.1.0 | **批准**：2025-06-13 | **最后修订**：2025-07-16
```

### 4. 在 PR 中验证一致性

```markdown
## PR 检查清单

- [ ] 运行 /speckit.analyze 检查宪法对齐
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

Spec Kit 的一致性传播机制是**基于命令执行的**，而不是自动化的：

- ✅ `/speckit.plan` 在执行时读取宪法并应用门控
- ✅ `/speckit.analyze` 在执行时读取宪法并检查一致性
- ❌ 没有自动化的宪法变化检测
- ❌ 没有自动化的工件更新
- ❌ 没有自动化的通知系统

开发人员需要：

1. 宪法更新后手动运行 `/speckit.analyze`
2. 根据分析结果手动更新受影响的工件
3. 使用 `/speckit.plan` 重新生成计划以应用新宪法
