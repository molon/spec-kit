# 更新日志

<!-- markdownlint-disable MD024 -->

Specify CLI 和模板的所有值得注意的更改都记录在此处。

格式基于[保留更新日志](https://keepachangelog.com/en/1.0.0/)，
此项目遵循[语义版本控制](https://semver.org/spec/v2.0.0.html)。

## [0.0.22] - 2025-11-07

- 支持 VS Code/Copilot 代理，并从提示转向具有交接的适当代理。
- 移至为 Copilot 工作负载使用 `AGENTS.md`，因为它已开箱即用支持。
- 添加对版本命令的支持。([#486](https://github.com/github/spec-kit/issues/486))
- 修复 `create-new-feature.ps1` 脚本中的潜在错误，该脚本在确定下一个功能编号时忽略现有功能分支 ([#975](https://github.com/github/spec-kit/issues/975))
- 在模板获取期间为 GitHub API 速率限制添加优雅的回退和日志记录 ([#970](https://github.com/github/spec-kit/issues/970))

## [0.0.21] - 2025-10-21

- 修复 [#975](https://github.com/github/spec-kit/issues/975)（感谢 [@fgalarraga](https://github.com/fgalarraga)）。
- 添加对 Amp CLI 的支持。
- 添加对 VS Code 交接的支持，并将提示转换为完整的聊天模式。
- 添加对 `version` 命令的支持（解决 [#811](https://github.com/github/spec-kit/issues/811) 和 [#486](https://github.com/github/spec-kit/issues/486)，感谢 [@mcasalaina](https://github.com/mcasalaina) 和 [@dentity007](https://github.com/dentity007)）。
- 添加对从 CLI 遇到时呈现速率限制错误的支持 ([#970](https://github.com/github/spec-kit/issues/970)，感谢 [@psmman](https://github.com/psmman))。

## [0.0.20] - 2025-10-14

### 添加

- **智能分支命名**：`create-new-feature` 脚本现在支持 `--short-name` 参数用于自定义分支名称
  - 提供 `--short-name` 时：直接使用自定义名称（清理和格式化）
  - 省略时：使用停用词过滤和基于长度的过滤自动生成有意义的名称
  - 过滤掉常见停用词（I、want、to、the、for 等）
  - 移除短于 3 个字符的单词（除非它们是大写首字母缩略词）
  - 从描述中取 3-4 个最有意义的单词
  - **强制执行 GitHub 的 244 字节分支名称限制**，带有自动截断和警告
  - 示例：
    - "I want to create user authentication" → `001-create-user-authentication`
    - "Implement OAuth2 integration for API" → `001-implement-oauth2-integration-api`
    - "Fix payment processing bug" → `001-fix-payment-processing`
    - 非常长的描述会自动在单词边界处截断以保持在限制内
  - 为 AI 代理设计以提供语义短名称，同时保持独立可用性

### 改变

- 增强了 `create-new-feature.sh` 和 `create-new-feature.ps1` 脚本的帮助文档，包含示例
- 分支名称现在根据 GitHub 的 244 字节限制进行验证，如果需要，自动截断

## [0.0.19] - 2025-10-10

### 添加

- 支持 CodeBuddy（感谢 [@lispking](https://github.com/lispking) 的贡献）。
- 你现在可以在 Specify CLI 中看到 Git 源错误。

### 改变

- 修复了 `plan.md` 中宪法的路径（感谢 [@lyzno1](https://github.com/lyzno1) 发现）。
- 修复了为 Gemini 生成的 TOML 文件中的反斜杠转义（感谢 [@hsin19](https://github.com/hsin19) 的贡献）。
- 实现命令现在确保添加了正确的忽略文件（感谢 [@sigent-amazon](https://github.com/sigent-amazon) 的贡献）。

## [0.0.18] - 2025-10-06

### 添加

- 支持在 `specify init .` 命令中使用 `.` 作为当前目录的缩写，等同于 `--here` 标志，但对用户更直观。
- 使用 `/speckit.` 命令前缀轻松发现 Spec Kit 相关命令。
- 重构提示和模板以简化其功能以及如何跟踪它们。不再在不需要时用测试污染事物。
- 确保为每个用户故事创建任务（简化测试和验证）。
- 添加对 Visual Studio Code 提示快捷方式和自动脚本执行的支持。

### 改变

- 所有命令文件现在以 `speckit.` 为前缀（例如 `speckit.specify.md`、`speckit.plan.md`），以便在 IDE/CLI 命令面板和文件浏览器中更好地发现和区分

## [0.0.17] - 2025-09-22

### 添加

- 新的 `/clarify` 命令模板，为现有规格说明提出最多 5 个有针对性的澄清问题，并将答案持久化到规格说明中的澄清部分。
- 新的 `/analyze` 命令模板，提供非破坏性的跨工件差异和对齐报告（规格说明、澄清、计划、任务、宪法），在 `/tasks` 之后和 `/implement` 之前插入。
  - 注意：宪法规则被明确视为不可协商的；任何冲突都是需要工件补救的关键发现，而不是削弱原则。

## [0.0.16] - 2025-09-22

### 添加

- `init` 命令的 `--force` 标志，在使用 `--here` 在非空目录中时绕过确认并继续合并/覆盖文件。

## [0.0.15] - 2025-09-21

### 添加

- 支持 Roo Code。

## [0.0.14] - 2025-09-21

### 改变

- 错误消息现在一致显示。

## [0.0.13] - 2025-09-21

### 添加

- 支持 Kilo Code。感谢 [@shahrukhkhan489](https://github.com/shahrukhkhan489) 的 [#394](https://github.com/github/spec-kit/pull/394)。
- 支持 Auggie CLI。感谢 [@hungthai1401](https://github.com/hungthai1401) 的 [#137](https://github.com/github/spec-kit/pull/137)。
- 项目配置完成后显示的代理文件夹安全通知，警告用户某些代理可能在其代理文件夹中存储凭证或身份验证令牌，并建议将相关文件夹添加到 `.gitignore` 以防止意外凭证泄露。

### 改变

- 显示警告以确保人们意识到他们可能需要将其代理文件夹添加到 `.gitignore`。
- 清理了 `check` 命令输出。

## [0.0.12] - 2025-09-21

### 改变

- 为 OpenAI Codex 用户添加了额外的上下文——他们需要设置额外的环境变量，如 [#417](https://github.com/github/spec-kit/issues/417) 中所述。

## [0.0.11] - 2025-09-20

### 添加

- Codex CLI 支持（感谢 [@honjo-hiroaki-gtt](https://github.com/honjo-hiroaki-gtt) 在 [#14](https://github.com/github/spec-kit/pull/14) 中的贡献）
- Codex 感知上下文更新工具（Bash 和 PowerShell），以便功能计划在不进行手动编辑的情况下刷新 `AGENTS.md` 以及现有助手。

## [0.0.10] - 2025-09-20

### 修复

- 解决了 [#378](https://github.com/github/spec-kit/issues/378)，其中 GitHub 令牌可能在为空时附加到请求。

## [0.0.9] - 2025-09-19

### 改变

- 改进的代理选择器 UI，带有代理键的青色突出显示和全名的灰色括号

## [0.0.8] - 2025-09-19

### 添加

- Windsurf IDE 支持作为额外的 AI 助手选项（感谢 [@raedkit](https://github.com/raedkit) 在 [#151](https://github.com/github/spec-kit/pull/151) 中的工作）
- GitHub 令牌支持 API 请求以处理企业环境和速率限制（由 [@zryfish](https://github.com/@zryfish) 在 [#243](https://github.com/github/spec-kit/pull/243) 中贡献）

### 改变

- 使用 Windsurf 示例和 GitHub 令牌使用更新了 README
- 增强了发布工作流以包括 Windsurf 模板

## [0.0.7] - 2025-09-18

### 改变

- 更新了 CLI 中的命令说明。
- 清理了代码，在通用时不呈现代理特定信息。

## [0.0.6] - 2025-09-17

### 添加

- opencode 支持作为额外的 AI 助手选项

## [0.0.5] - 2025-09-17

### 添加

- Qwen Code 支持作为额外的 AI 助手选项

## [0.0.4] - 2025-09-14

### 添加

- 通过 `httpx[socks]` 依赖为企业环境提供 SOCKS 代理支持

### 修复

N/A

### 改变

N/A
