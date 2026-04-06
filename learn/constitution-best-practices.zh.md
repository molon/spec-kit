# Constitution 最佳实践指南

## 一、核心定位

Constitution（项目宪法）只放**非协商性原则**（non-negotiable）—— 违反即为 CRITICAL 问题，而不是"最好这样做"的建议性指导。

官方最小示例：

> "Security-First 应用。所有用户输入必须验证。微服务架构。代码必须完全文档化。"

---

## 二、什么该放，什么不该放

| 适合放 Constitution | 不适合放（应放其他文件） |
|---|---|
| 架构边界（禁止 ORM、分层规则） | 功能需求 → `spec.md` |
| 测试强制要求（TDD、覆盖率门槛） | 实现细节（怎么做）→ `plan.md` |
| 技术栈约束（语言版本、框架选型） | AI 操作指令 → `AGENTS.md` / `CLAUDE.md` |
| 代码质量底线（lint、命名约定） | 大量安全合规细节 → 单独文件，constitution 中引用 |
| 安全要求（OWASP Top 10 遵从） | 运行时命令参考 → `CLAUDE.md` |
| Git/commit 规范 | 代码示例 → 子目录文件 |
| 治理规则（谁能修改宪法） | |

**关键边界（社区共识）**：`spec.md` 中禁止出现框架名、库名、架构模式等技术词汇，这些全部属于 `plan.md` 的范畴。如果 `spec.md` 里出现了技术词汇，属于 blocker 级别问题。

---

## 三、语言差异：核心无关，技术约束节必须特化

核心原则（测试纪律、架构边界、安全要求）是语言无关的，可跨项目复用。但**技术约束节必须写明语言和框架的具体要求**，不同语言项目使用独立的 constitution。

### Go 项目

- 错误处理：explicit error returns，禁止 panic
- 并发：goroutines + channels，避免 mutex（Actor 模式）
- 工具链：golangci-lint + gofmt + gosec + govulncheck
- 测试：go test -race，table-driven tests，testcontainers-go
- 数据库：禁止 ORM，直接使用 pgx，context-aware queries

### Python FastAPI 项目

- 所有公开函数必须有 type hints，请求/响应使用 Pydantic 模型
- 包管理：uv
- 代码质量：ruff（统一替代 flake8 + isort + black）+ mypy strict

### TypeScript Next.js 项目

- strict mode，禁止 any 类型
- 优先使用 Server Components
- 性能基准：Lighthouse ≥ 90，LCP < 2.5s，CLS < 0.1

---

## 四、长度建议

| 项目规模 | 推荐长度 | 结构 |
|---|---|---|
| 个人 / 小项目 | 50 ~ 150 行 | 3 ~ 5 条核心原则 |
| 中型项目 | 150 ~ 300 行 | 6 ~ 10 条原则 + 技术约束节 |
| 企业 / 大项目 | constitution 保持精简，详细规则放子文件并引用 | constitution 作为索引入口 |

**大项目推荐结构**（官方维护者建议：内容小就放 constitution 里，内容大就放单独文件并引用）：

```
.specify/memory/
  constitution.md          ← 精简入口，内含 "Constitution Index" 表
  security-standards.md    ← 详细安全规范
  api-guidelines.md        ← API 设计规范
  testing-policy.md        ← 测试策略细节
```

---

## 五、章节结构模板

以下为官方与社区综合推荐的章节顺序：

1. **元数据头部** —— 版本号、批准人、日期
2. **Project Context** —— 项目背景、目标、范围
3. **Core Principles** —— 核心原则，每条附带 MUST/SHOULD 措辞和 Rationale
4. **Technical Constraints** —— 技术栈、工具链、语言特定规范
5. **Code Quality Standards** —— 测试、lint、命名、文档要求
6. **Security Requirements** —— 安全合规要求
7. **Development Workflow** —— Git 规范、PR 流程、CI/CD
8. **Governance** —— 修订程序、版本策略

---

## 六、真实项目示例

以下为 Go 项目 `mcpproxy-go`（Version 1.1.0）的 constitution 结构示例：

```markdown
<!-- Sync Impact Report: v1.0.0 → v1.1.0 ... -->

**Version**: 1.1.0 | **Ratified**: 2026-01-15 | **Last Amended**: 2026-02-20

## Core Principles

### Article I: Library-First
MUST 先做成库。

**Rationale**：提升可复用性，避免业务逻辑耦合到 CLI 层。

### Article II: Actor-Based Concurrency
MUST 用 goroutines + channels，禁止 mutex（除非 benchmark 证明必要）。

**Rationale**：避免死锁，提升可测试性。

### Article III: Test-First
MUST 先写测试，再写实现。

## Technical Constraints

- Go 最低版本：1.22+
- 禁止 ORM，直接使用 pgx
- 错误处理：显式 return，不 panic
- 工具：golangci-lint + testcontainers-go

## Development Workflow

- Conventional Commits 格式
- 不要在 commit 中加 Co-Authored-By AI 字样
- PR 必须通过 CI 才能 merge
```

---

## 七、实用写法建议

1. **用 RFC 2119 措辞**：`MUST`（强制）、`SHOULD`（建议）、`MAY`（可选）—— 让 AI 能精确判断违规级别，避免歧义。

2. **每条原则加 Rationale**：解释"为什么"，AI 才能在边界情况下做出正确判断，而不只是机械遵守规则。

3. **明确写"违反则 CRITICAL"**：让 `/speckit.analyze` 能正确升级问题的严重级别，而不是当作普通建议处理。

4. **版本控制语义化**：
   - MAJOR —— 删除或重定义原则
   - MINOR —— 新增原则
   - PATCH —— 措辞修正

5. **内容大了就拆文件**：constitution 作为引用索引，详细规范放子文件，保持主文件精简可读。

---

## 八、与其他文件的边界

| 文件 | 职责 |
|---|---|
| `constitution.md` | 架构原则、非协商性约束（WHAT is unacceptable） |
| `CLAUDE.md` | AI 操作指令、工作流说明（HOW the AI should work） |
| `spec.md` | 功能需求（纯产品视角，无技术词汇） |
| `plan.md` | 技术实现细节 |

**社区建议**：只维护 `constitution.md`，在 `CLAUDE.md` 里放一行链接指向它。两者互补，不重复内容。
