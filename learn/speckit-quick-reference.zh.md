# Spec Kit 快速参考指南

## 什么是 Spec Kit？

Spec Kit 是一个**规格驱动开发（Spec-Driven Development, SDD）**工具包，帮助开发团队在编写代码之前先定义清晰的规格说明。它通过一系列 AI 辅助命令，将自然语言的功能描述转化为结构化的规格说明、实现计划和任务列表。

### 核心理念

- **先规格，后代码**：在实现之前明确"做什么"和"为什么"
- **宪法约束**：项目宪法定义核心原则，所有工件必须遵守
- **渐进式细化**：从模糊描述 → 规格说明 → 计划 → 任务 → 实现
- **AI 辅助**：每个阶段都有专门的 AI 命令支持

## 工作流程图

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         Spec Kit 开发工作流                                  │
└─────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────────────┐
│ 阶段 0: 项目初始化（首次或需要更新宪法时）                                     │
│ ┌──────────────────────────┐                                                │
│ │  /speckit.constitution   │ ──→ 创建/更新 constitution.md（项目宪法）       │
│ └──────────────────────────┘     • 定义核心原则和约束                        │
│                                  • 执行一致性传播到模板                       │
│                                  • 生成同步影响报告                           │
└──────────────────────────────────────────────────────────────────────────────┘
                                       │
                              ┌────────▼─────────┐
                              │  功能描述/需求    │
                              │  (自然语言)      │
                              └────────┬─────────┘
                                       │
                                       ▼
┌──────────────────────────────────────────────────────────────────────────────┐
│ 阶段 1: 规格定义                                                              │
│ ┌────────────────────┐                                                       │
│ │  /speckit.specify  │ ──→ 创建 spec.md + checklists/requirements.md        │
│ └────────────────────┘     • 用户场景和测试                                   │
│          │                 • 功能需求、成功标准、边缘案例                       │
│          ▼                                                                   │
│ ┌────────────────────┐                                                       │
│ │  /speckit.clarify  │ ──→ 识别歧义，提问澄清（可选）                          │
│ └────────────────────┘                                                       │
└──────────────────────────────────────────────────────────────────────────────┘
                                       │
                                       ▼
┌──────────────────────────────────────────────────────────────────────────────┐
│ 阶段 2: 技术规划                                                              │
│ ┌────────────────────┐                                                       │
│ │   /speckit.plan    │ ──→ 创建 plan.md（实现计划）                           │
│ └────────────────────┘     • 读取 constitution.md（宪法检查）                 │
│                            • 技术上下文和架构                                  │
│                            • 数据模型、API 契约、研究文档                      │
└──────────────────────────────────────────────────────────────────────────────┘
                                       │
                                       ▼
┌──────────────────────────────────────────────────────────────────────────────┐
│ 阶段 3: 任务分解                                                              │
│ ┌────────────────────┐                                                       │
│ │   /speckit.tasks   │ ──→ 创建 tasks.md（任务列表）                          │
│ └────────────────────┘     • 按用户故事组织、依赖关系排序                      │
│          │                 • 并行执行标记 [P]                                  │
│          ▼                                                                   │
│ ┌────────────────────┐                                                       │
│ │ /speckit.checklist │ ──→ 创建领域特定检查清单（可选，按需调用）              │
│ └────────────────────┘     • 如 ux.md, security.md, api.md                   │
└──────────────────────────────────────────────────────────────────────────────┘
                                       │
                                       ▼
┌──────────────────────────────────────────────────────────────────────────────┐
│ 阶段 4: 质量验证                                                              │
│ ┌────────────────────┐                                                       │
│ │  /speckit.analyze  │ ──→ 一致性分析报告                                     │
│ └────────────────────┘     • 读取 constitution.md（宪法验证）                 │
│                            • 检查 spec ↔ plan ↔ tasks 一致性                  │
│                            • 识别重复、歧义、欠规格、覆盖率差距                 │
│                            • ⚠️ 只读操作，不修改文件                           │
└──────────────────────────────────────────────────────────────────────────────┘
                                       │
                          ┌────────────┴────────────┐
                          ▼                         ▼
┌─────────────────────────────────────┐   ┌─────────────────────────────────┐
│ 阶段 5a: 创建 GitHub Issues         │   │ 阶段 5b: 直接实现                │
│ ┌─────────────────────────────┐     │   │ ┌─────────────────────────────┐ │
│ │  /speckit.taskstoissues     │     │   │ │   /speckit.implement        │ │
│ └─────────────────────────────┘     │   │ └─────────────────────────────┘ │
│ • 将任务转为 GitHub Issues          │   │ • 检查清单状态                   │
│ • 需要 GitHub 远程仓库              │   │ • 加载实现上下文                 │
│ • 使用 GitHub MCP Server            │   │ • 按阶段执行任务                 │
│                                     │   │ • 进度跟踪和错误处理             │
└─────────────────────────────────────┘   └─────────────────────────────────┘
```

## 命令详解

### `/speckit.constitution` - 创建或更新项目宪法 ⭐

**作用**：创建或更新项目宪法，并执行一致性传播到所有依赖的模板。

**创建 vs 更新**：

- 如果 `constitution.md` **不存在** → 创建新宪法
- 如果 `constitution.md` **已存在** → 更新现有宪法
- **两种情况都会**执行一致性传播

**输入**：宪法修改描述（自然语言）

**输出**：

- 创建/更新 `/memory/constitution.md`
- 更新相关模板文件（plan-template.md, spec-template.md 等）
- 生成同步影响报告

**主要步骤**：

1. 加载现有宪法模板（或创建新模板）
2. 收集/推导占位符的值
3. 起草更新后的宪法内容
4. **执行一致性传播检查清单**：
   - 更新 `/templates/plan-template.md` 确保宪法检查对齐
   - 更新 `/templates/spec-template.md` 确保 Requirements 部分与宪法约束对齐
   - 更新 `/templates/tasks-template.md` 确保任务阶段划分反映新原则
   - 更新 `/templates/commands/*.md` 验证无过时引用
   - 更新 README.md、docs/quickstart.md 等文档
5. 生成同步影响报告（版本变化、修改的原则、需要更新的模板）
6. 验证并写入宪法文件

**关键特性**：这是**一致性传播的核心命令**，修改宪法后应该运行此命令。

---

### `/speckit.specify` - 创建功能规格说明

**作用**：从自然语言功能描述创建结构化的规格说明文档。

**输入**：功能描述（自然语言）

**输出**：

- `specs/[###-feature]/spec.md` - 功能规格说明
- `specs/[###-feature]/checklists/requirements.md` - 规格质量检查清单

**主要步骤**：

1. 生成简短的分支名称（2-4 词）
2. 检查现有分支，确定下一个可用编号
3. 创建并切换到新分支
4. 使用 `spec-template.md` 生成规格说明
5. 验证规格质量
6. 如有歧义（最多 3 个），提问用户澄清

**不涉及**：宪法检查（由 `/speckit.plan` 处理）

---

### `/speckit.clarify` - 澄清规格需求

**作用**：识别规格说明中的歧义和欠规格区域，通过提问澄清。

**输入**：现有的 `spec.md`

**输出**：更新后的 `spec.md`（歧义已解决）

**主要步骤**：

1. 读取现有规格说明
2. 识别欠规格的区域
3. 生成澄清问题
4. 记录用户回答
5. 更新规格说明

**不涉及**：宪法检查

---

### `/speckit.plan` - 创建实现计划

**作用**：基于规格说明创建技术实现计划。

**输入**：自动检测当前分支对应的 feature 目录，读取其中的 `spec.md`

- **不需要**手动指定 feature 名称

**输出**：

- `specs/[###-feature]/plan.md` - 实现计划
- `specs/[###-feature]/research.md` - 研究文档
- `specs/[###-feature]/data-model.md` - 数据模型
- `specs/[###-feature]/contracts/` - API 契约
- `specs/[###-feature]/quickstart.md` - 快速开始指南

**主要步骤**：

1. 读取 `spec.md` 和 **`/memory/constitution.md`**
2. 填充技术上下文
3. **填充宪法检查部分**
4. **评估宪法门控**（违规则报错）
5. 阶段 0：生成研究文档
6. 阶段 1：生成数据模型、API 契约
7. **重新评估宪法检查**

**生成后是否需要手动修改？**

- **通常不需要**：生成的 plan.md 应该是完整的
- **例外情况**：如果有 "NEEDS CLARIFICATION" 标记，需要补充信息
- **最佳实践**：先运行 `/speckit.analyze` 检查，有问题再修改或重新生成

**生成的工件可以调整吗？**

**可以，这是预期行为。** Spec Kit 的核心原则之一是**持续细化（Continuous Refinement）**：

> "一致性验证是持续进行的，而不是一次性的门控。AI 会持续分析规格说明中的歧义、矛盾和差距。"

如果对 `research.md`、`data-model.md`、`contracts/` 等生成的文档内容不满意，开发者可以：

1. **直接与 AI 沟通调整**：描述需要修改的内容，让 AI 重新生成或修改
2. **手动编辑**：直接修改文件内容
3. **重新运行命令**：提供更详细的输入，重新生成文档

这种迭代式的细化过程是 Spec Kit 设计的一部分，确保最终的规格说明和设计文档能够准确反映项目需求。

**关键特性**：这是**生成工件时读取宪法**的命令（`/speckit.analyze` 也会读取宪法用于验证）。

---

### `/speckit.tasks` - 生成任务列表

**作用**：基于设计工件生成可执行的任务列表。

**输入**：`plan.md`, `spec.md`, 及其他设计文档

**输出**：`specs/[###-feature]/tasks.md`

**主要步骤**：

1. 加载设计文档（plan.md, spec.md, data-model.md 等）
2. 按用户故事组织任务
3. 生成依赖图
4. 标记可并行执行的任务 [P]
5. 验证任务完整性

**任务格式**：

```
- [ ] T001 [P] [US1] 描述 文件路径
```

**不涉及**：直接读取宪法

---

### `/speckit.checklist` - 创建检查清单

**作用**：为特定领域创建自定义检查清单（"需求的单元测试"）。

**输入**：领域描述（可选，如 "UX 设计"、"安全性"、"性能"）

- 如果提供输入：直接生成对应领域的清单
- 如果不提供输入：会询问 2-3 个澄清问题后生成

**输出**：`specs/[###-feature]/checklists/[domain].md`

**与 `/speckit.specify` 的区别**：

- `/speckit.specify` **自动**生成 `requirements.md`（规格质量检查）
- `/speckit.checklist` **按需**生成领域特定清单（如 ux.md, security.md）
- 两者**不重复**，职责不同

**不涉及**：宪法检查

---

### `/speckit.analyze` - 一致性分析 ⭐

**作用**：对 spec.md、plan.md、tasks.md 进行**只读**的一致性和质量分析。

**输入**：`spec.md`, `plan.md`, `tasks.md`, `constitution.md`

**输出**：分析报告（不写入文件）

**分析内容**：
| 类别 | 检查项 |
|------|--------|
| 重复检测 | 近似重复的需求 |
| 歧义检测 | 模糊形容词、未解决占位符 |
| 欠规格检测 | 缺少可测量结果的需求 |
| **宪法对齐** | 与 MUST 原则冲突、缺少必需部分 |
| 覆盖率差距 | 需求无对应任务、任务无对应需求 |
| 不一致性 | 术语漂移、数据实体引用不一致 |

**严重级别**：

- **CRITICAL**：违反宪法 MUST、缺少核心工件
- **HIGH**：重复/冲突需求、不可测试的验收标准
- **MEDIUM**：术语漂移、缺少非功能任务覆盖
- **LOW**：风格/措辞改进

**关键特性**：

- ⚠️ **只读操作**，不修改任何文件
- 宪法冲突自动标记为 CRITICAL
- 必须在 `/speckit.tasks` 之后运行

---

### `/speckit.taskstoissues` - 转换为 GitHub Issues

**作用**：将 tasks.md 中的任务转换为 GitHub Issues。

**前提条件**：

- Git remote 必须是 GitHub URL
- 需要 GitHub MCP Server

**输入**：`tasks.md`

**输出**：GitHub Issues（在对应仓库中创建）

**主要步骤**：

1. 读取 tasks.md
2. 获取 Git remote URL
3. 验证是 GitHub 仓库
4. 为每个任务创建 GitHub Issue

---

### `/speckit.implement` - 执行实现

**作用**：按照 tasks.md 执行实现计划。

**输入**：`tasks.md`, `plan.md`, 及其他设计文档

**输出**：实现的代码

**主要步骤**：

1. **检查清单状态**（扫描 checklists/ 目录，统计完成情况）
   - 如有未完成项 → 显示状态表并询问是否继续
   - 全部完成 → 自动继续
2. 加载实现上下文
3. 项目设置验证（创建 .gitignore 等）
4. 解析任务结构
5. **按阶段执行任务**
6. 进度跟踪和错误处理
7. 完成验证

**执行规则**：

- 阶段顺序执行
- 尊重依赖关系
- [P] 标记的任务可并行
- TDD 方式：测试先于实现
- 完成的任务标记为 [X]

**关键特性**：这是**唯一会检查 checklists/ 的命令**。

---

## 宪法与一致性

### 哪些命令读取/更新宪法？

| 命令                     | 宪法相关 | 用途                          |
| ------------------------ | -------- | ----------------------------- |
| `/speckit.constitution`  | ✅ 更新  | 创建/更新宪法 + 一致性传播 ⭐ |
| `/speckit.specify`       | ❌       | -                             |
| `/speckit.clarify`       | ❌       | -                             |
| `/speckit.plan`          | ✅ 读取  | 填充宪法检查、评估门控        |
| `/speckit.tasks`         | ❌       | -                             |
| `/speckit.checklist`     | ❌       | -                             |
| `/speckit.analyze`       | ✅ 读取  | 验证宪法对齐                  |
| `/speckit.taskstoissues` | ❌       | -                             |
| `/speckit.implement`     | ❌       | -                             |

### 哪些命令检查 Checklist？

| 命令                     | 检查清单 | 说明                                         |
| ------------------------ | -------- | -------------------------------------------- |
| `/speckit.analyze`       | ❌       | 只分析 spec/plan/tasks 一致性，不检查清单    |
| `/speckit.implement`     | ✅       | 执行前检查 checklists/，未完成会询问是否继续 |
| `/speckit.taskstoissues` | ❌       | 只读取 tasks.md 创建 Issues，不检查清单      |

> **注意**：目前没有专门只检查清单的命令。如需单独检查，请手动查看 `checklists/` 目录。

### Checklist 的创建与更新

| 清单类型             | 创建者               | 自动更新状态                | 用户手动标记          |
| -------------------- | -------------------- | --------------------------- | --------------------- |
| `requirements.md`    | `/speckit.specify`   | ✅ 验证时自动更新 pass/fail | 不需要                |
| 领域清单（ux.md 等） | `/speckit.checklist` | ❌ 否                       | ✅ 需要手动标记 `[X]` |

**说明**：

- `/speckit.specify` 在创建规格说明时会自动生成 `requirements.md`，并在验证过程中自动更新每个项目的 pass/fail 状态
- `/speckit.checklist` 创建的领域清单（如 ux.md, security.md）需要用户在审查后手动标记完成（`[ ]` → `[X]`）
- `/speckit.implement` 只读取清单状态统计完成情况，不会修改清单文件

### 宪法更新后的操作

**推荐流程**：使用 `/speckit.constitution` 命令修改宪法

```bash
# 1. 运行 constitution 命令，描述你要做的修改
/speckit.constitution 添加新的安全原则：所有用户输入必须验证

# 该命令会自动：
# - 更新 /memory/constitution.md
# - 执行一致性传播检查清单
# - 更新相关模板文件
# - 生成同步影响报告
# - 更新版本号
```

**如果直接编辑了宪法文件**：

```bash
# 1. 运行 constitution 命令来触发一致性传播
/speckit.constitution 确认并传播最近的宪法修改

# 2. 检查现有工件是否符合新宪法
/speckit.analyze
```

**后续步骤**：

1. 运行 `/speckit.analyze` 检查现有工件是否符合新宪法
2. 如有违规，运行 `/speckit.plan` 重新生成计划
3. 运行 `/speckit.tasks` 重新生成任务

**新功能自动应用新宪法**：后续创建的新功能会自动使用新宪法，`/speckit.plan` 会读取新宪法并应用门控。

### 计划模板中的宪法检查

计划模板（`plan-template.md`）包含一个 "Constitution Check" 部分：

```markdown
## Constitution Check

_Gating: Must pass before Phase 0 research. Re-check after Phase 1 design._

[Gates determined based on constitution file]
```

- `/speckit.plan` 命令会根据宪法文件填充这个部分
- 门控在第 0 阶段研究前必须通过
- 在第 1 阶段设计后需要重新检查

### 宪法依赖关系图

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

## 目录结构

```
project/
├── .specify/                        # Spec Kit 核心目录（由 specify init 创建）
│   ├── memory/
│   │   └── constitution.md          # 项目宪法（核心原则和约束）
│   ├── scripts/                     # 辅助脚本
│   │   └── bash/
│   │       ├── check-prerequisites.sh
│   │       ├── common.sh
│   │       ├── create-new-feature.sh
│   │       ├── setup-plan.sh
│   │       └── update-agent-context.sh
│   └── templates/                   # 模板文件
│       ├── agent-file-template.md   # AI Agent 规则模板（update-agent-context.sh 使用）
│       ├── checklist-template.md    # 检查清单模板
│       ├── plan-template.md         # 实现计划模板
│       ├── spec-template.md         # 功能规格模板
│       └── tasks-template.md        # 任务列表模板
├── .windsurf/                       # Windsurf IDE 集成（其他 AI 工具有对应目录）
│   ├── rules/                       # Windsurf 规则
│   │   └── specify-rules.md         # AI Agent 规则（update-agent-context.sh 生成/更新）
│   └── workflows/                   # Windsurf 工作流（命令定义）
│       ├── speckit.analyze.md       # 一致性分析命令
│       ├── speckit.checklist.md     # 检查清单命令
│       ├── speckit.clarify.md       # 澄清规格命令
│       ├── speckit.constitution.md  # 宪法管理命令
│       ├── speckit.implement.md     # 实现执行命令
│       ├── speckit.plan.md          # 实现计划命令
│       ├── speckit.specify.md       # 规格定义命令
│       ├── speckit.tasks.md         # 任务生成命令
│       └── speckit.taskstoissues.md # 任务转 Issues 命令
└── specs/                           # 功能规格目录（运行时生成）
    └── 001-feature-name/            # 功能目录（由 /speckit.specify 创建）
        ├── spec.md                  # 功能规格说明（/speckit.specify 生成）
        ├── plan.md                  # 实现计划（/speckit.plan 生成）
        ├── tasks.md                 # 任务列表（/speckit.tasks 生成）
        ├── research.md              # 研究文档（/speckit.plan 生成）
        ├── data-model.md            # 数据模型（/speckit.plan 生成）
        ├── quickstart.md            # 快速开始（/speckit.plan 生成）
        ├── contracts/               # API 契约（/speckit.plan 生成）
        │   └── api.md
        └── checklists/              # 检查清单
            ├── requirements.md      # 规格质量检查（/speckit.specify 自动生成）
            ├── ux.md                # UX 检查（/speckit.checklist 按需生成）
            └── security.md          # 安全检查（/speckit.checklist 按需生成）
```

## AI Agent 上下文更新

### 什么是 Agent 上下文？

Spec Kit 支持多种 AI 编程助手（Claude、Windsurf、Cursor、Copilot 等）。每个 AI 工具都有自己的规则文件，用于向 AI 提供项目上下文信息（技术栈、目录结构、最近变更等）。

### 相关文件

| 文件                      | 位置                     | 说明                     |
| ------------------------- | ------------------------ | ------------------------ |
| `agent-file-template.md`  | `.specify/templates/`    | AI Agent 规则文件的模板  |
| `specify-rules.md`        | `.windsurf/rules/` 等    | 生成的 AI Agent 规则文件 |
| `update-agent-context.sh` | `.specify/scripts/bash/` | 更新脚本                 |

### 工作流程

```
plan.md（项目元数据来源）
    │
    ▼
update-agent-context.sh（解析 plan.md）
    │
    ├─→ 读取 agent-file-template.md（模板）
    │
    └─→ 生成/更新各 AI 工具的规则文件
        ├─→ .windsurf/rules/specify-rules.md
        ├─→ .cursor/rules/specify-rules.mdc
        ├─→ CLAUDE.md
        └─→ 其他 AI 工具对应文件...
```

### 触发时机

`update-agent-context.sh` 脚本由 **`/speckit.plan`** 命令在 Phase 1 完成后**自动触发**：

```yaml
# 在 plan.md 命令模板中定义
agent_scripts:
  sh: scripts/bash/update-agent-context.sh __AGENT__
  ps: scripts/powershell/update-agent-context.ps1 -AgentType __AGENT__
```

也可以手动运行：

```bash
# 更新所有已存在的 AI Agent 规则文件
./.specify/scripts/bash/update-agent-context.sh

# 只更新特定 AI 工具的规则文件
./.specify/scripts/bash/update-agent-context.sh windsurf
./.specify/scripts/bash/update-agent-context.sh claude
```

### 手动运行场景

- 项目技术栈在 `plan.md` 之外发生变化后
- 需要单独更新某个 AI 工具的规则文件时
- 调试或测试 AI Agent 上下文时

## 最佳实践

### 1. 完整流程

```bash
# 1. 创建规格说明
/speckit.specify 我想要添加用户认证功能

# 2. 澄清歧义（可选）
/speckit.clarify

# 3. 创建实现计划
/speckit.plan

# 4. 生成任务列表
/speckit.tasks

# 5. 一致性分析（推荐）
/speckit.analyze

# 6a. 创建 GitHub Issues（团队协作）
/speckit.taskstoissues

# 6b. 或直接实现（个人开发）
/speckit.implement
```

### 2. 在实现前运行分析

始终在 `/speckit.implement` 之前运行 `/speckit.analyze`，确保：

- 所有需求都有对应任务
- 没有宪法违规
- 没有重大歧义或欠规格

### 3. 宪法是不可变的

在实现期间不应修改宪法。如需修改：

1. 完成当前功能
2. 更新宪法
3. 运行 `/speckit.analyze` 检查影响
4. 更新受影响的工件

### 4. 在 PR 中验证一致性（手动最佳实践）

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

### 5. 宪法版本控制

`/speckit.constitution` 命令会自动管理宪法版本号，遵循语义版本控制：

- **MAJOR**：向后不兼容的治理/原则删除或重新定义
- **MINOR**：新增原则/部分或实质性扩展指导
- **PATCH**：澄清、措辞、拼写修复、非语义性改进

## 限制和注意事项

### 当前限制

1. **没有自动化传播**：宪法更新后不会自动更新现有工件（spec/plan/tasks）
2. **没有自动通知**：宪法更新后不会自动通知开发人员
3. **没有 git hooks**：没有自动触发一致性检查的 git hooks
4. **部分命令不读取宪法**：`/speckit.specify`、`/speckit.clarify` 等命令不直接读取宪法

### 注意事项

1. **手动验证是必需的**：宪法更新后需要手动运行 `/speckit.analyze`
2. **现有工件需要手动更新**：宪法更新后需要手动重新生成受影响的工件
3. **宪法冲突是 CRITICAL**：`/speckit.analyze` 会将宪法冲突标记为 CRITICAL 级别
