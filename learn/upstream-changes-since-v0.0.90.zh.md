# Spec Kit 上游变更总结：v0.0.90 → v0.5.0

> **时间跨度**：2025-12-04（v0.0.90）→ 2026-04-02（v0.5.0）
>
> **上游仓库**：[github/spec-kit](https://github.com/github/spec-kit)
>
> **版本跨度**：v0.0.90 → v0.0.102 → v0.1.x → v0.2.x → v0.3.x → v0.4.x → v0.5.0

---

## 一、版本演进概览

| 版本范围 | 时间 | 主题 |
|----------|------|------|
| v0.0.90–v0.0.102 | 2025-12 ~ 2026-02 | Bug 修复、新 Agent 支持、脚本增强 |
| v0.1.x | 2026-02 | 通用 Agent 支持、Extension 系统早期、CI/发布流程重构 |
| v0.2.x | 2026-03 | 多目录（Catalog）支持、Hook 事件、更多 Agent |
| **v0.3.x** | **2026-03** | **Preset 系统（重大架构变更）、`specify doctor` 命令** |
| **v0.4.x** | **2026-03~04** | **Plugin 架构迁移、Copilot 集成、离线部署、Legacy 清理** |
| v0.5.0 | 2026-04 | 开发文档、社区引用更新 |

---

## 二、重大架构变更

### 1. Extension 扩展系统（v0.0.93 引入，v0.1.x~v0.4.x 成熟）

这是 v0.0.90 之后**最大的新增机制**。Extension 允许在不修改核心工具的情况下添加新功能。

**核心概念**：

- **Category 分类**：`docs`（文档验证）、`code`（代码审查）、`process`（流程编排）、`integration`（外部平台集成）、`visibility`（项目健康报告）
- **Effect 效果**：`Read-only`（只读报告）或 `Read+Write`（修改文件）
- **Catalog 目录**：分 `catalog.json`（组织自用）和 `catalog.community.json`（社区贡献）

**关键文件结构**：

```
extensions/
├── selftest/                          # 自测试扩展
├── template/                          # 扩展模板
├── EXTENSION-API-REFERENCE.md         # API 参考
├── EXTENSION-DEVELOPMENT-GUIDE.md     # 开发指南
├── EXTENSION-PUBLISHING-GUIDE.md      # 发布指南
├── EXTENSION-USER-GUIDE.md            # 用户指南
├── RFC-EXTENSION-SYSTEM.md            # 技术设计文档
├── catalog.json                       # 组织目录
└── catalog.community.json             # 社区目录
```

**重要特性演进**：

| 版本 | 特性 |
|------|------|
| v0.0.93 | 引入模块化扩展系统 |
| v0.1.5 | `commands_subdir` 支持非标准 Agent 目录 |
| v0.1.7 | 双 Catalog 系统文档化 |
| v0.2.0 | 支持多个活跃 Catalog 同时使用 |
| v0.2.1 | `.extensionignore` 排除文件支持 |
| v0.3.0 | RFC 对齐的 Catalog 集成改进、`/selftest.extension` 核心扩展 |
| v0.3.2 | 扩展的 enable/disable 开关 |
| v0.4.2 | 扩展的 AI Skills 自动注册 |
| v0.4.4 | 防止扩展命令名称冲突（shadowing） |
| v0.4.5 | 19 个 Agent 迁移至 Plugin 架构 |

**社区扩展示例**：

- **Checkpoint**：实现过程中自动创建 Git 提交
- **Cleanup**：实现后的质量门控
- **MAQA**：多 Agent 工作流 + CI/CD 门控（7 个扩展组成）
- **DocGuard**：CDD 执行检查
- **Jira/Azure DevOps Integration**：外部平台集成
- **V-Model Extension Pack**：V 模型开发支持
- **Iterate**：允许实现中更新 Spec 而不丢失上下文

### 2. Preset 预设系统（v0.3.0 引入）

Preset 是另一个**重大架构变更**，允许自定义 Spec Kit 的行为——覆盖模板、命令和术语——无需修改核心工具。

**核心机制**：

- **Pluggable（可插拔）**：通过 Catalog + Resolver 实现
- **Skills Propagation（技能传播）**：Preset 可以带有自己的 AI Skills
- **Priority-based Resolution（优先级解析）**：多个 Extension/Preset 冲突时按优先级解决（v0.3.1）

**示例**：Pirate Speak Preset 将所有输出转换为航海语言（Spec 变 "Voyage Manifests"，Tasks 变 "Crew Assignments"）。

**相关版本**：

| 版本 | 特性 |
|------|------|
| v0.3.0 | 引入 Pluggable Preset 系统 |
| v0.3.1 | 优先级解析、Preset Demo |
| v0.3.2 | enable/disable 开关 |
| v0.4.2 | AI Skills 自动注册也支持 Preset |

### 3. Plugin 架构迁移（v0.4.x）

v0.4.x 是一次重大的内部重构，将 Agent 集成从硬编码迁移到 Plugin 架构。

**6 个阶段的迁移**：

1. **Stage 1**（v0.4.4）：Integration Foundation — 基础类、Manifest 系统、Registry
2. **Stage 2**（v0.4.4）：Copilot Integration PoC — 共享模板原语
3. **Stage 3**（v0.4.5）：Standard Markdown Integrations — 19 个 Agent 迁移
4. **Stage 4**（v0.4.5）：TOML Integrations — Gemini/Tabnine 迁移
5. **Stage 5**（v0.4.5）：Skills, Generic & Option-Driven Integrations
6. **Stage 6**（v0.4.5）：Complete Migration — 移除 Legacy Scaffold 路径

**影响**：

- 新增 Agent 更加容易（只需定义 Manifest）
- 旧的 dotted 目录结构已废弃并迁移
- Claude Code 现在作为 Native Skills 安装

### 4. AI Skills 系统（v0.0.99 引入）

`AI Skills` 是一个新概念，让 AI Agent 能直接感知和使用 Spec Kit 的能力。

- v0.0.99：初始实现
- v0.4.0：对齐 Native Skills frontmatter
- v0.4.2：扩展的 AI Skills 自动注册
- v0.4.5：Claude Code 安装为 Native Skills

**对于 Claude Code**：Skills 安装到 `.claude/skills/` 目录。

---

## 三、新增 CLI 命令与功能

### `specify doctor`（v0.3.0）

项目健康诊断命令，检查项目配置是否正确。

### `specify status`（v0.3.1）

显示项目当前状态。

### `specify check`

版本检查和环境验证（早期就有，后续持续改进）。

### `--dry-run` 创建功能分支（v0.4.5）

```bash
# 模拟创建，不实际执行
create-new-feature --dry-run
```

### `--allow-existing-branch`（v0.4.4）

```bash
# 允许使用已存在的分支
create-new-feature --allow-existing-branch
```

### Timestamp-based Branch Naming（v0.4.0）

```bash
# 基于时间戳的分支命名选项
specify init --timestamp-branch
```

### 离线/Air-gapped 部署（v0.4.0）

Core Pack 嵌入 wheel 包，支持无网络环境部署。

---

## 四、Agent 支持变化

v0.0.90 时支持的 Agent 较少，之后大量新增：

### 新增的 CLI-Based Agent

| Agent | 版本 |
|-------|------|
| Qoder CLI | v0.0.87 |
| IBM Bob IDE | v0.0.86 |
| OVHcloud SHAI | v0.0.83 |
| CodeBuddy CLI | v0.0.59 (之前有，后改名) |
| Google Antigravity | v0.0.95 |
| Kiro CLI | v0.1.13 |
| Generic Agent（通用） | v0.1.3 |
| Tabnine CLI | v0.2.0 |
| Mistral Vibe | v0.2.0 |
| Kimi Code CLI | v0.2.1 |
| Trae IDE | v0.3.1 |
| iFlow CLI | v0.3.2 |
| Pi Coding Agent | v0.3.2 |
| Junie | v0.4.0 |

### Agent 架构变化

- **AGENT_CONFIG** 成为所有 Agent 元数据的 Single Source of Truth（在 `src/specify_cli/__init__.py` 中）
- Key 必须匹配真实的 CLI 可执行文件名（如 `cursor-agent` 而非 `cursor`）
- 支持 `commands_subdir` 配置非标准目录结构
- 命令文件格式多样：Markdown（多数）、TOML（Gemini/Tabnine）、自定义

### 总计支持的 Agent

- **CLI-Based**：约 22 个
- **IDE-Based**：约 8 个（Copilot, Cursor, Windsurf, Kilo Code, Roo Code, IBM Bob, Trae, Antigravity）

---

## 五、命令模板与工作流变化

### Hook 事件系统（v0.2.0, v0.3.2）

命令模板现在支持 `before/after` Hook 事件：

- `after_tasks`：tasks 生成后触发
- `after_implement`：implement 完成后触发
- `before/after` hook events 在 specify 和 plan 模板中也已支持

**用途**：扩展可以注册 Hook，在特定阶段自动执行。

### 模板变化

- `NFR`（非功能性需求）已重命名为 `Success Criteria`（成功标准）（v0.4.2）
- Spec 模板新增 `Assumptions` 部分（v0.4.1）
- 模板移除了 OpenAPI/GraphQL 偏见，更加中立（v0.1.5）
- Clarify 命令的问题上限从 10 修正为 5（v0.1.13）

### Settings.json 处理改进（v0.3.1）

- 支持 JSONC（带注释的 JSON）
- "Polite deep merge"：合并而非覆盖已有配置

---

## 六、脚本和工具链变化

### 分支管理改进

| 版本 | 变化 |
|------|------|
| v0.0.89 | 全局分支编号最大值，防止冲突 |
| v0.0.79 | 改进分支号检测，检查远程分支 |
| v0.2.0 | 全局分支编号替代 per-short-name 检测 |
| v0.4.5 | 支持 4+ 位数的分支号 |

### PowerShell 兼容性

- 多次修复 PowerShell 5.1 兼容性问题（null-conditional 操作符等）
- 参数绑定修复
- UTF-8 编码支持

### 安全加固

- Bash 脚本防注入加固（v0.3.0）
- GitHub Actions 工作流安全加固（v0.4.4）
- `dependabot` 配置用于自动更新依赖

---

## 七、发布与 CI 流程变化

- 从直接 push main 改为 Release Branch + PR 流程（v0.1.11~v0.1.12）
- `pyproject.toml` 版本与 Git Tag 同步
- 使用 PEP 440 `.dev0` 版本号标记开发中版本（v0.4.4 后）
- CodeQL 从 v3 升级到 v4
- 新增 `pytest` 和 `ruff`（Python linting）到 CI

---

## 八、升级指南

### CLI 升级

```bash
uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git@v0.5.0
```

### 项目文件更新

```bash
specify init --here --force --ai <your-agent>
```

### 升级注意事项

1. **`constitution.md` 会被覆盖**：升级前备份，或升级后用 `git restore` 恢复
2. **自定义模板修改会丢失**：需手动合并
3. **IDE 命令可能重复**：旧命令文件需手动删除
4. **`specs/` 目录安全**：升级不会修改已有的规格文件

### 从 v0.0.90 升级的特别注意

- `.specify/` 目录结构可能有变化
- Agent 命令文件前缀统一为 `speckit.`（如果之前不是）
- 旧的 dotted 目录结构（如 `.kimi.code/`）已迁移为新格式
- Extension 和 Preset 系统是全新的，需要重新学习
- `specify doctor` 命令可以帮助检查项目健康状态

---

## 九、核心机制对比（v0.0.90 vs v0.5.0）

| 方面 | v0.0.90 | v0.5.0 |
|------|---------|--------|
| **Agent 集成** | 硬编码在核心中 | Plugin 架构，Manifest 驱动 |
| **扩展性** | 无 | Extension 系统 + Catalog |
| **自定义** | 手动修改模板 | Preset 系统（可插拔覆盖） |
| **Agent 数量** | ~15 | 30+ |
| **命令** | 基础 SDD 工作流 | + doctor, status, hook events |
| **AI Skills** | 无 | Native Skills 集成 |
| **离线支持** | 无 | Core Pack 嵌入 wheel |
| **分支管理** | 基础 | dry-run, timestamp, allow-existing |
| **模板** | 有 OpenAPI/GraphQL 偏见 | 中立，NFR→Success Criteria |
| **发布流程** | 直接 push | Release Branch + PR |
| **配置合并** | 覆盖 | Polite deep merge + JSONC |

---

## 十、值得深入学习的新概念

### 1. Extension 开发

如果你想为 Spec Kit 开发扩展，建议阅读：
- `extensions/EXTENSION-DEVELOPMENT-GUIDE.md`
- `extensions/RFC-EXTENSION-SYSTEM.md`
- `extensions/template/` 模板目录

### 2. Preset 自定义

如果你想自定义工作流术语和模板：
- 了解 Catalog + Resolver 机制
- 查看 Pirate Speak 示例了解预设如何覆盖

### 3. Plugin 架构

如果你想了解 Agent 如何集成：
- 查看 `AGENT_CONFIG` 结构
- 了解 Manifest 系统和 Registry
- 了解 Native Skills 的工作方式

### 4. Hook Events

如果你想在工作流的特定阶段插入自定义逻辑：
- 了解 `before_*/after_*` 事件
- 了解扩展如何注册 Hook

---

## 十一、完整版本变更明细

<details>
<summary>点击展开 v0.0.91 ~ v0.0.102 明细</summary>

- **v0.0.91**（2026-02-09）：重新初始化时保留 constitution.md；修复 markdownlint 错误
- **v0.0.92**（2026-02-10）：修复 `.specify.specify` 路径错误
- **v0.0.93**（2026-02-10）：**引入模块化扩展系统**
- **v0.0.94**（2026-02-11）：180 天不活跃 Issue/PR 自动标记 stale
- **v0.0.95**（2026-02-12）：添加 Google Antigravity Agent
- **v0.0.96**（2026-02-17）：修复 plan-template.md 拼写错误
- **v0.0.97**（2026-02-18）：从 README 移除 Maintainers 部分
- **v0.0.98**（2026-02-19）：添加 dependabot 配置
- **v0.0.99**（2026-02-19）：**引入 AI Skills 功能**
- **v0.0.100**（2026-02-19）：CI 添加 pytest 和 ruff
- **v0.0.101**（2026-02-19）：CodeQL action 从 v3 升级到 v4
- **v0.0.102**（2026-02-20）：发布工作流路径触发修复

</details>

<details>
<summary>点击展开 v0.1.x 明细</summary>

- **v0.1.3**（2026-02-20）：**通用 Agent 支持**，可自定义命令目录
- **v0.1.4**（2026-02-20）：修复 Qoder CLI 配置 key
- **v0.1.5**（2026-02-21）：`commands_subdir` 字段；移除模板的 OpenAPI/GraphQL 偏见
- **v0.1.6**（2026-02-23）：Cleanup Extension；修复 CLI 参数排序
- **v0.1.7**（2026-02-26）：文档化双 Catalog 系统
- **v0.1.8~v0.1.10**（2026-02-27~28）：依赖更新；Cursor .mdc 文件 YAML frontmatter
- **v0.1.12~v0.1.13**（2026-03-02~03）：发布流程重构；新 Agent + Extension

</details>

<details>
<summary>点击展开 v0.2.x 明细</summary>

- **v0.2.0**（2026-03-09）：**多活跃 Catalog**；Tabnine/Mistral 支持；全局分支编号；`after_tasks/after_implement` Hook
- **v0.2.1**（2026-03-11）：Kimi Code CLI；`.extensionignore`；Codex 扩展命令注册

</details>

<details>
<summary>点击展开 v0.3.x 明细</summary>

- **v0.3.0**（2026-03-13）：**Pluggable Preset 系统**；`specify doctor`；Bash 脚本安全加固；`/selftest.extension`
- **v0.3.1**（2026-03-17）：Preset Demo；Trae IDE；**JSONC 支持 + polite deep merge**；`specify status`
- **v0.3.2**（2026-03-19）：Extension enable/disable；iFlow CLI；Hook 事件集成到 specify/plan 模板

</details>

<details>
<summary>点击展开 v0.4.x 明细</summary>

- **v0.4.0**（2026-03-23）：YAML Unicode；离线部署；Junie 支持；Codex/agy 迁移到 Native Skills
- **v0.4.1**（2026-03-24）：Checkpoint 扩展；`.specify` 优先于 git 检测项目根目录
- **v0.4.2**（2026-03-25）：AI Skills 自动注册；NFR → Success Criteria 重命名
- **v0.4.3**（2026-03-26）：Kimi/Codex Skill 命名统一
- **v0.4.4**（2026-04-01）：**Stage 1-2 Plugin 迁移**；GitHub Actions 安全加固；MAQA 扩展套件
- **v0.4.5**（2026-04-02）：**Stage 3-6 Plugin 迁移完成**；Legacy 路径移除；Claude Code Native Skills

</details>

<details>
<summary>点击展开 v0.5.0 明细</summary>

- **v0.5.0**（2026-04-02）：引入 DEVELOPMENT.md 文档；社区引用从 cc-sdd 更新为 cc-spex

</details>
