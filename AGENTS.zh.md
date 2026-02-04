# AGENTS.md

## 关于 Spec Kit 和 Specify

**GitHub Spec Kit** 是一个全面的工具包，用于实现规格驱动开发（SDD）——一种强调在实现前创建清晰规格说明的方法论。该工具包包括模板、脚本和工作流，指导开发团队通过结构化方法构建软件。

**Specify CLI** 是命令行界面，使用 Spec Kit 框架引导项目。它设置必要的目录结构、模板和 AI 代理集成，以支持规格驱动开发工作流。

该工具包支持多个 AI 编码助手，允许团队使用他们喜欢的工具，同时保持一致的项目结构和开发实践。

---

## 一般实践

- 对 Specify CLI 的 `__init__.py` 的任何更改都需要在 `pyproject.toml` 中进行版本修订，并在 `CHANGELOG.md` 中添加条目。

## 添加新代理支持

本部分说明如何向 Specify CLI 添加新 AI 代理/助手的支持。在将新 AI 工具集成到规格驱动开发工作流中时，将本指南用作参考。

### 概述

Specify 通过在初始化项目时生成代理特定的命令文件和目录结构来支持多个 AI 代理。每个代理都有自己的约定：

- **命令文件格式**（Markdown、TOML 等）
- **目录结构**（`.claude/commands/`、`.windsurf/workflows/` 等）
- **命令调用模式**（斜杠命令、CLI 工具等）
- **参数传递约定**（`$ARGUMENTS`、`{{args}}` 等）

### 当前支持的代理

| 代理 | 目录 | 格式 | CLI 工具 | 描述 |
| --- | --- | --- | --- | --- |
| **Claude Code** | `.claude/commands/` | Markdown | `claude` | Anthropic 的 Claude Code CLI |
| **Gemini CLI** | `.gemini/commands/` | TOML | `gemini` | Google 的 Gemini CLI |
| **GitHub Copilot** | `.github/agents/` | Markdown | N/A（基于 IDE） | VS Code 中的 GitHub Copilot |
| **Cursor** | `.cursor/commands/` | Markdown | `cursor-agent` | Cursor CLI |
| **Qwen Code** | `.qwen/commands/` | TOML | `qwen` | 阿里巴巴的 Qwen Code CLI |
| **opencode** | `.opencode/command/` | Markdown | `opencode` | opencode CLI |
| **Codex CLI** | `.codex/commands/` | Markdown | `codex` | Codex CLI |
| **Windsurf** | `.windsurf/workflows/` | Markdown | N/A（基于 IDE） | Windsurf IDE 工作流 |
| **Kilo Code** | `.kilocode/rules/` | Markdown | N/A（基于 IDE） | Kilo Code IDE |
| **Auggie CLI** | `.augment/rules/` | Markdown | `auggie` | Auggie CLI |
| **Roo Code** | `.roo/rules/` | Markdown | N/A（基于 IDE） | Roo Code IDE |
| **CodeBuddy CLI** | `.codebuddy/commands/` | Markdown | `codebuddy` | CodeBuddy CLI |
| **Qoder CLI** | `.qoder/commands/` | Markdown | `qoder` | Qoder CLI |
| **Amazon Q Developer CLI** | `.amazonq/prompts/` | Markdown | `q` | Amazon Q Developer CLI |
| **Amp** | `.agents/commands/` | Markdown | `amp` | Amp CLI |
| **SHAI** | `.shai/commands/` | Markdown | `shai` | SHAI CLI |
| **IBM Bob** | `.bob/commands/` | Markdown | N/A（基于 IDE） | IBM Bob IDE |

### 分步集成指南

按照这些步骤添加新代理（使用假设的新代理作为示例）：

#### 1. 添加到 AGENT_CONFIG

**重要**：使用实际的 CLI 工具名称作为键，而不是缩写版本。

将新代理添加到 `src/specify_cli/__init__.py` 中的 `AGENT_CONFIG` 字典。这是所有代理元数据的**单一真实来源**：

```python
AGENT_CONFIG = {
    # ... existing agents ...
    "new-agent-cli": {  # Use the ACTUAL CLI tool name (what users type in terminal)
        "name": "New Agent Display Name",
        "folder": ".newagent/",  # Directory for agent files
        "install_url": "https://example.com/install",  # URL for installation docs (or None if IDE-based)
        "requires_cli": True,  # True if CLI tool required, False for IDE-based agents
    },
}
```

**关键设计原则**：字典键应与用户安装的实际可执行文件名称匹配。例如：

- ✅ 使用 `"cursor-agent"`，因为 CLI 工具实际上被称为 `cursor-agent`
- ❌ 如果工具是 `cursor-agent`，不要使用 `"cursor"` 作为快捷方式

这消除了整个代码库中对特殊情况映射的需要。

**字段说明**：

- `name`：显示给用户的人类可读的显示名称
- `folder`：存储代理特定文件的目录（相对于项目根目录）
- `install_url`：安装文档 URL（对于基于 IDE 的代理，设置为 `None`）
- `requires_cli`：代理在初始化期间是否需要 CLI 工具检查

#### 2. 更新 CLI 帮助文本

更新 `init()` 命令中的 `--ai` 参数帮助文本以包括新代理：

```python
ai_assistant: str = typer.Option(None, "--ai", help="AI assistant to use: claude, gemini, copilot, cursor-agent, qwen, opencode, codex, windsurf, kilocode, auggie, codebuddy, new-agent-cli, or q"),
```

还要更新任何函数文档字符串、示例和列出可用代理的错误消息。

#### 3. 更新 README 文档

更新 `README.md` 中的**支持的 AI 代理**部分以包括新代理：

- 将新代理添加到表格中，并附上适当的支持级别（完整/部分）
- 包括代理的官方网站链接
- 添加有关代理实现的任何相关注释
- 确保表格格式保持对齐和一致

#### 4. 更新发布包脚本

修改 `.github/workflows/scripts/create-release-packages.sh`：

##### 添加到 ALL_AGENTS 数组

```bash
ALL_AGENTS=(claude gemini copilot cursor-agent qwen opencode windsurf q)
```

##### 为目录结构添加 case 语句

```bash
case $agent in
  # ... existing cases ...
  windsurf)
    mkdir -p "$base_dir/.windsurf/workflows"
    generate_commands windsurf md "\$ARGUMENTS" "$base_dir/.windsurf/workflows" "$script" ;;
esac
```

#### 5. 更新代理上下文脚本

##### Bash 脚本（`scripts/bash/update-agent-context.sh`）

添加文件变量：

```bash
WINDSURF_FILE="$REPO_ROOT/.windsurf/rules/specify-rules.md"
```

添加到 case 语句：

```bash
case "$AGENT_TYPE" in
  # ... existing cases ...
  windsurf) update_agent_file "$WINDSURF_FILE" "Windsurf" ;;
  "")
    # ... existing checks ...
    [ -f "$WINDSURF_FILE" ] && update_agent_file "$WINDSURF_FILE" "Windsurf";
    # Update default creation condition
    ;;
esac
```

##### PowerShell 脚本（`scripts/powershell/update-agent-context.ps1`）

添加文件变量：

```powershell
$windsurfFile = Join-Path $repoRoot '.windsurf/rules/specify-rules.md'
```

添加到 switch 语句：

```powershell
switch ($AgentType) {
    # ... existing cases ...
    'windsurf' { Update-AgentFile $windsurfFile 'Windsurf' }
    '' {
        foreach ($pair in @(
            # ... existing pairs ...
            @{file=$windsurfFile; name='Windsurf'}
        )) {
            if (Test-Path $pair.file) { Update-AgentFile $pair.file $pair.name }
        }
        # Update default creation condition
    }
}
```

#### 6. 更新 CLI 工具检查（可选）

对于需要 CLI 工具的代理，在 `check()` 命令和代理验证中添加检查：

```python
# In check() command
tracker.add("windsurf", "Windsurf IDE (optional)")
windsurf_ok = check_tool_for_tracker("windsurf", "https://windsurf.com/", tracker)

# In init validation (only if CLI tool required)
elif selected_ai == "windsurf":
    if not check_tool("windsurf", "Install from: https://windsurf.com/"):
        console.print("[red]Error:[/red] Windsurf CLI is required for Windsurf projects")
        agent_tool_missing = True
```

**注意**：CLI 工具检查现在根据 AGENT_CONFIG 中的 `requires_cli` 字段自动处理。`check()` 和 `init()` 命令中不需要额外的代码更改——它们自动循环遍历 AGENT_CONFIG 并根据需要检查工具。

## 重要设计决策

### 使用实际的 CLI 工具名称作为键

**关键**：向 AGENT_CONFIG 添加新代理时，始终使用**实际的可执行文件名称**作为字典键，而不是缩写或方便的版本。

**为什么这很重要：**

- `check_tool()` 函数使用 `shutil.which(tool)` 在系统 PATH 中查找可执行文件
- 如果键与实际的 CLI 工具名称不匹配，你需要在整个代码库中进行特殊情况映射
- 这会造成不必要的复杂性和维护负担

**示例 - Cursor 的教训：**

❌ **错误的方法**（需要特殊情况映射）：

```python
AGENT_CONFIG = {
    "cursor": {  # Shorthand that doesn't match the actual tool
        "name": "Cursor",
        # ...
    }
}

# Then you need special cases everywhere:
cli_tool = agent_key
if agent_key == "cursor":
    cli_tool = "cursor-agent"  # Map to the real tool name
```

✅ **正确的方法**（无需映射）：

```python
AGENT_CONFIG = {
    "cursor-agent": {  # Matches the actual executable name
        "name": "Cursor",
        # ...
    }
}

# No special cases needed - just use agent_key directly!
```

**此方法的优势：**

- 消除了整个代码库中分散的特殊情况逻辑
- 使代码更易于维护和理解
- 减少添加新代理时出现错误的机会
- 工具检查"直接有效"，无需额外映射

#### 7. 更新 Devcontainer 文件（可选）

对于具有 VS Code 扩展或需要 CLI 安装的代理，更新 devcontainer 配置文件：

##### 基于 VS Code 扩展的代理

对于作为 VS Code 扩展提供的代理，将它们添加到 `.devcontainer/devcontainer.json`：

```json
{
  "customizations": {
    "vscode": {
      "extensions": [
        // ... existing extensions ...
        // [New Agent Name]
        "[New Agent Extension ID]"
      ]
    }
  }
}
```

##### 基于 CLI 的代理

对于需要 CLI 工具的代理，将安装命令添加到 `.devcontainer/post-create.sh`：

```bash
#!/bin/bash

# Existing installations...

echo -e "\n🤖 Installing [New Agent Name] CLI..."
# run_command "npm install -g [agent-cli-package]@latest" # Example for node-based CLI
# or other installation instructions (must be non-interactive and compatible with Linux Debian "Trixie" or later)...
echo "✅ Done"

```

**快速提示：**

- **基于扩展的代理**：添加到 `devcontainer.json` 中的 `extensions` 数组
- **基于 CLI 的代理**：将安装脚本添加到 `post-create.sh`
- **混合代理**：可能需要扩展和 CLI 安装
- **彻底测试**：确保安装在 devcontainer 环境中有效

## 代理类别

### 基于 CLI 的代理

需要安装命令行工具：

- **Claude Code**：`claude` CLI
- **Gemini CLI**：`gemini` CLI
- **Cursor**：`cursor-agent` CLI
- **Qwen Code**：`qwen` CLI
- **opencode**：`opencode` CLI
- **Amazon Q Developer CLI**：`q` CLI
- **CodeBuddy CLI**：`codebuddy` CLI
- **Qoder CLI**：`qoder` CLI
- **Amp**：`amp` CLI
- **SHAI**：`shai` CLI

### 基于 IDE 的代理

在集成开发环境中工作：

- **GitHub Copilot**：内置于 VS Code/兼容编辑器
- **Windsurf**：内置于 Windsurf IDE
- **IBM Bob**：内置于 IBM Bob IDE

## 命令文件格式

### Markdown 格式

使用者：Claude、Cursor、opencode、Windsurf、Amazon Q Developer、Amp、SHAI、IBM Bob

**标准格式：**

```markdown
---
description: "Command description"
---

Command content with {SCRIPT} and $ARGUMENTS placeholders.
```

**GitHub Copilot Chat 模式格式：**

```markdown
---
description: "Command description"
mode: speckit.command-name
---

Command content with {SCRIPT} and $ARGUMENTS placeholders.
```

### TOML 格式

使用者：Gemini、Qwen

```toml
description = "Command description"

prompt = """
Command content with {SCRIPT} and {{args}} placeholders.
"""
```

## 目录约定

- **CLI 代理**：通常 `.<agent-name>/commands/`
- **IDE 代理**：遵循 IDE 特定的模式：
  - Copilot：`.github/agents/`
  - Cursor：`.cursor/commands/`
  - Windsurf：`.windsurf/workflows/`

## 参数模式

不同的代理使用不同的参数占位符：

- **Markdown/提示基础**：`$ARGUMENTS`
- **基于 TOML**：`{{args}}`
- **脚本占位符**：`{SCRIPT}`（替换为实际脚本路径）
- **代理占位符**：`__AGENT__`（替换为代理名称）

## 测试新代理集成

1. **构建测试**：在本地运行包创建脚本
2. **CLI 测试**：测试 `specify init --ai <agent>` 命令
3. **文件生成**：验证正确的目录结构和文件
4. **命令验证**：确保生成的命令与代理一起工作
5. **上下文更新**：测试代理上下文更新脚本

## 常见陷阱

1. **使用缩写键而不是实际的 CLI 工具名称**：始终使用实际的可执行文件名称作为 AGENT_CONFIG 键（例如 `"cursor-agent"` 而不是 `"cursor"`）。这可以防止在整个代码库中需要特殊情况映射。
2. **忘记更新脚本**：添加新代理时，必须更新 bash 和 PowerShell 脚本。
3. **不正确的 `requires_cli` 值**：仅对实际具有 CLI 工具的代理设置为 `True`；对于基于 IDE 的代理，设置为 `False`。
4. **错误的参数格式**：为每个代理类型使用正确的占位符格式（Markdown 使用 `$ARGUMENTS`，TOML 使用 `{{args}}`）。
5. **目录命名**：完全遵循代理特定的约定（检查现有代理的模式）。
6. **帮助文本不一致**：一致地更新所有面向用户的文本（帮助字符串、文档字符串、README、错误消息）。

## 未来考虑

添加新代理时：

- 考虑代理的原生命令/工作流模式
- 确保与规格驱动开发流程的兼容性
- 记录任何特殊要求或限制
- 用学到的经验更新本指南
- 在添加到 AGENT_CONFIG 之前验证实际的 CLI 工具名称

---

*每当添加新代理时，应更新此文档以保持准确性和完整性。*
