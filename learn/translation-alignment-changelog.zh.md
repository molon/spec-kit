# 中文文档对齐翻译变更汇总

> **日期**：2026-04-04
>
> **背景**：将所有 `.zh.md` 文件重新对齐翻译以匹配上游 `github/spec-kit` v0.5.0 的英文原版。
>
> **原因**：中文文档停留在 v0.0.90 之前的状态，大量新增内容和机制变更未反映。

---

## 一、命令模板（templates/commands/）

### specify.zh.md — 重大变更

| 变更项 | 旧版（中文） | 新版（对齐英文） |
|--------|-------------|-----------------|
| **Hook 系统** | 完全缺失 | 新增 `hooks.before_specify` 和 `hooks.after_specify` 扩展 Hook 检查 |
| **分支创建逻辑** | 手动扫描 `specs/` 目录确定编号 | 使用脚本 `create-new-feature.sh` + `--short-name` + `--json`，支持顺序模式和时间戳模式 |
| **规格质量验证** | 缺失 | 新增第 6 步：规格质量验证框架（最多 3 轮迭代） |
| **Success Criteria 指南** | 缺失 | 新增详细指南：可测量、技术无关、用户聚焦、可验证 |
| **Section Requirements** | 缺失 | 新增必选/可选部分说明 |
| **宪法检查** | 有第 5 步"宪法检查" | 移除（英文版 specify 不读取宪法） |
| **澄清标记** | 未提及 | 新增 `[NEEDS CLARIFICATION]` 最多 3 个标记机制 |

### plan.zh.md — 重大变更

| 变更项 | 旧版 | 新版 |
|--------|------|------|
| **Hook 系统** | 缺失 | 新增 `hooks.before_plan` 和 `hooks.after_plan` |
| **接口合约描述** | "API 端点" | "接口合约"（范围扩大：库、CLI 工具、Web 服务、解析器、UI 合约） |
| **Agent 上下文更新** | 简略提及 | 详细描述 `AGENT_SCRIPT` 执行和 Agent 特定上下文文件保留 |

### implement.zh.md — 重大变更

| 变更项 | 旧版 | 新版 |
|--------|------|------|
| **Hook 系统** | 缺失 | 新增 `hooks.before_implement` 和 `hooks.after_implement` |
| **Checklist 状态检查** | 完全缺失 | 新增第 2 步：完整的 Checklist 验证工作流（扫描、统计、决策逻辑） |
| **C 语言忽略模式** | 缺少 `autom4te.cache/` | 补充完整 |
| **步骤总数** | ~8 步 | 10 步（新增 Hook 检查和 Checklist 验证） |

### clarify.zh.md — 重大变更

| 变更项 | 旧版 | 新版 |
|--------|------|------|
| **问题上限** | "最多 10 个总问题" | "最多 5 个"（v0.1.13 修正） |
| **NFR 术语** | "非功能/质量属性部分" | "Success Criteria > Measurable Outcomes" |
| **截断内容** | 在第 151 行截断，缺少第 6-8 节和行为规则 | 完整翻译 |

### analyze.zh.md — 中等变更

| 变更项 | 旧版 | 新版 |
|--------|------|------|
| **NFR 术语** | "非功能需求" | "Success Criteria（可测量成果）" |
| **需求清单定义** | 缺少 FR-###/SC-### 标识符模式 | 补充标识符模式和 post-launch KPI 排除规则 |
| **覆盖率分析** | 缺少"buildable work"过滤条件 | 补充：仅包含需要可构建工作的 Success Criteria 项 |

### constitution.zh.md — 重大变更

| 变更项 | 旧版 | 新版 |
|--------|------|------|
| **宪法路径** | `/memory/constitution.md` | `.specify/memory/constitution.md` |
| **模板路径** | `/templates/plan-template.md` 等 | `.specify/templates/plan-template.md` 等 |
| **初始化说明** | 缺失 | 新增：从 `.specify/templates/constitution-template.md` 初始化的说明 |

### tasks.zh.md — 中等变更

| 变更项 | 旧版 | 新版 |
|--------|------|------|
| **Hook 系统** | 缺失 | 新增 `hooks.before_tasks` 和 `hooks.after_tasks` |
| **contracts 描述** | "API 端点" | "接口合约" |
| **Phase N 翻译** | "抛光和跨切面关注点" | "完善和横切关注点" |

### checklist.zh.md — 中等变更

| 变更项 | 旧版 | 新版 |
|--------|------|------|
| **文件处理行为** | 矛盾描述（创建 vs 追加不清晰） | 明确：不存在则创建（CHK001 起）；存在则追加（续接 CHK ID）；永不删除/替换 |
| **"no pre-baked catalog"** | 缺失 | 补充说明 |

### taskstoissues.zh.md — 小变更

| 变更项 | 旧版 | 新版 |
|--------|------|------|
| **CAUTION 标记** | 翻译为 `[!警告]` | 保留英文 `[!CAUTION]`（GitHub markdown 标准语法） |

---

## 二、模板文件（templates/）

### spec-template.zh.md

| 变更项 | 旧版 | 新版 |
|--------|------|------|
| **Assumptions 部分** | 完全缺失 | 新增 `## Assumptions` 部分（目标用户、范围边界、数据/环境、依赖） |

### plan-template.zh.md

| 变更项 | 旧版 | 新版 |
|--------|------|------|
| **Project Type 示例** | 只有 3 个（单个/网络/移动） | 扩充为 6 个（library/cli/web-service/mobile-app/compiler/desktop-app） |
| **路径引用** | 不一致 | 对齐为 `.specify/templates/commands/plan.md` |

### tasks-template.zh.md — 重大变更

| 变更项 | 旧版 | 新版 |
|--------|------|------|
| **截断** | 在第 150 行截断 | 补充 ~100 行缺失内容 |
| **缺失部分** | Phase N 完善内容、依赖与执行顺序、并行示例、实现策略（MVP 优先、增量交付、并行团队）、Notes | 全部补充 |

---

## 三、根目录文档

### README.zh.md — 重大变更

此前缺失约 50% 的内容，包括：
- 社区扩展（Community Extensions）整个部分
- 社区预设（Community Presets）部分
- 社区演练（Community Walkthroughs）部分
- 社区伙伴（Community Friends）部分
- "Extensions & Presets" 自定义指南
- 10+ 个新增 Agent（Kiro、Tabnine、Mistral、Kimi、iFlow、Forge、Junie、Trae 等）
- `/speckit.clarify`、`/speckit.analyze`、`/speckit.checklist` 详细说明
- `--ai-skills` 标志说明

### AGENTS.zh.md — 重大变更

此前缺失：
- "Special Processing Requirements" 部分（Copilot/Forge 集成）
- 10+ 个 Agent 条目
- 多个 Agent 目录结构有误（如 Kilo Code `.kilocode/rules/` → `.kilocode/workflows/`）
- "Future Considerations" 部分

### CHANGELOG.zh.md — 完全重写

- 旧版：183 行，覆盖 v0.0.22 ~ v0.0.4（截止 2025-11）
- 新版：1166 行，覆盖 v0.5.0 ~ v0.0.1（截止 2026-04）
- 新增 60+ 个版本条目

### CONTRIBUTING.zh.md

新增：
- "Recommended validation flow" 部分（3 步验证：自动检查 → 手动测试 → 调试）
- `TESTING.md` 引用和 `uv sync --extra test` / `uv pip install -e .` 说明

### spec-driven.zh.md

- 基本对齐（此前差异不大）

---

## 四、docs/ 目录

### docs/installation.zh.md — 重大变更

新增：
- "Enterprise / Air-Gapped Installation" 完整部分（4 步：wheel 构建、传输、离线安装、弃用说明）
- `--ai pi` 选项
- 版本固定 `@vX.Y.Z` 说明
- Windows/Python 注意事项

### docs/upgrade.zh.md

- 基本对齐（补充了版本固定细节和更明确的示例）

### docs/quickstart.zh.md

新增：
- 第 6 步后的 "Phased Implementation" 提示
- Taskify 示例中的 "Step 7: Define Tasks" 部分
- 修正 "Next Steps" URL 为绝对 GitHub URL

### docs/README.zh.md、docs/index.zh.md、docs/local-development.zh.md

- 小幅翻译质量改进（无结构性变更）

---

## 五、已确认无需更新的文件

| 文件 | 状态 |
|------|------|
| `templates/checklist-template.zh.md` | 已对齐 |
| `templates/agent-file-template.zh.md` | 已对齐 |
| `templates/constitution.zh.md`（宪法模板） | 已对齐 |
| `CODE_OF_CONDUCT.zh.md` | 小幅翻译改进 |
| `SECURITY.zh.md` | 小幅翻译改进 |
| `SUPPORT.zh.md` | 结构对齐（改为项目符号格式） |

---

## 六、关键术语变更汇总

| 旧术语 | 新术语 | 影响范围 |
|--------|--------|---------|
| NFR（非功能需求） | Success Criteria（成功标准） | analyze、clarify、spec-template |
| `/memory/constitution.md` | `.specify/memory/constitution.md` | constitution 命令 |
| `/templates/xxx` | `.specify/templates/xxx` | constitution 命令的传播路径 |
| API 端点 | 接口合约（Interface Contracts） | plan、tasks |
| 抛光（Phase N） | 完善 | tasks-template |
| 跨切面关注点 | 横切关注点 | tasks-template |
| 最多 10 个问题 | 最多 5 个问题 | clarify |
| `[!警告]` | `[!CAUTION]` | taskstoissues（保留 GitHub 标准语法） |

---

## 七、新增机制汇总

以下机制在旧版中文文档中**完全不存在**，现已通过翻译对齐补充：

1. **Extension Hook 系统**：所有命令现在都支持 `before_*/after_*` Hook，通过 `.specify/extensions.yml` 配置
2. **规格质量验证框架**（specify 命令第 6 步）：最多 3 轮迭代的质量检查
3. **Checklist 验证工作流**（implement 命令第 2 步）：实现前检查清单完成状态
4. **脚本化分支创建**：`create-new-feature.sh` + 顺序/时间戳双模式
5. **Enterprise/Air-Gapped 离线部署**：Core Pack 嵌入 wheel
6. **Assumptions 部分**（spec-template）：新增假设记录区域
7. **Success Criteria 指南**（specify 命令）：详细的成功标准编写准则
