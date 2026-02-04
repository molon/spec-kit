# 升级指南

> 你已安装 Spec Kit 并想升级到最新版本以获取新功能、错误修复或更新的斜杠命令。本指南涵盖升级 CLI 工具和更新项目文件。

---

## 快速参考

| 要升级什么 | 命令 | 何时使用 |
|----------------|---------|-------------|
| **仅 CLI 工具** | `uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git` | 获取最新 CLI 功能而不触及项目文件 |
| **项目文件** | `specify init --here --force --ai <your-agent>` | 更新项目中的斜杠命令、模板和脚本 |
| **两者** | 运行 CLI 升级，然后项目更新 | 建议用于主要版本更新 |

---

## 第 1 部分：升级 CLI 工具

CLI 工具（`specify`）与你的项目文件分开。升级它以获取最新功能和错误修复。

### 如果你使用 `uv tool install` 安装

```bash
uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git
```

### 如果你使用一次性 `uvx` 命令

不需要升级——`uvx` 总是获取最新版本。只需像往常一样运行你的命令：

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init --here --ai copilot
```

### 验证升级

```bash
specify check
```

这显示已安装的工具并确认 CLI 正在工作。

---

## 第 2 部分：更新项目文件

当 Spec Kit 发布新功能（如新斜杠命令或更新的模板）时，你需要刷新项目的 Spec Kit 文件。

### 什么会被更新？

运行 `specify init --here --force` 将更新：

- ✅ **斜杠命令文件**（`.claude/commands/`、`.github/prompts/` 等）
- ✅ **脚本文件**（`.specify/scripts/`）
- ✅ **模板文件**（`.specify/templates/`）
- ✅ **共享内存文件**（`.specify/memory/`）- **⚠️ 见下面的警告**

### 什么保持安全？

这些文件**永远不会被升级触及**——模板包甚至不包含它们：

- ✅ **你的规格说明**（`specs/001-my-feature/spec.md` 等）- **确认安全**
- ✅ **你的实现计划**（`specs/001-my-feature/plan.md`、`tasks.md` 等）- **确认安全**
- ✅ **你的源代码** - **确认安全**
- ✅ **你的 git 历史** - **确认安全**

`specs/` 目录完全从模板包中排除，在升级期间永远不会被修改。

### 更新命令

在你的项目目录中运行：

```bash
specify init --here --force --ai <your-agent>
```

将 `<your-agent>` 替换为你的 AI 助手。参考此[支持的 AI 代理](../README.md#-supported-ai-agents)列表

**示例：**

```bash
specify init --here --force --ai copilot
```

### 理解 `--force` 标志

没有 `--force`，CLI 会警告你并要求确认：

```text
警告：当前目录不为空（25 项）
模板文件将与现有内容合并，可能覆盖现有文件
继续？[y/N]
```

使用 `--force`，它跳过确认并立即继续。

**重要：你的 `specs/` 目录总是安全的。** `--force` 标志仅影响模板文件（命令、脚本、模板、内存）。你在 `specs/` 中的功能规格说明、计划和任务永远不会包含在升级包中，无法被覆盖。

---

## ⚠️ 重要警告

### 1. 宪法文件将被覆盖

**已知问题：** `specify init --here --force` 当前用默认模板覆盖 `.specify/memory/constitution.md`，删除你所做的任何自定义。

**解决方法：**

```bash
# 1. 升级前备份你的宪法
cp .specify/memory/constitution.md .specify/memory/constitution-backup.md

# 2. 运行升级
specify init --here --force --ai copilot

# 3. 恢复你的自定义宪法
mv .specify/memory/constitution-backup.md .specify/memory/constitution.md
```

或使用 git 恢复它：

```bash
# 升级后，从 git 历史恢复
git restore .specify/memory/constitution.md
```

### 2. 自定义模板修改

如果你自定义了 `.specify/templates/` 中的任何模板，升级将覆盖它们。首先备份它们：

```bash
# 备份自定义模板
cp -r .specify/templates .specify/templates-backup

# 升级后，手动合并你的更改
```

### 3. 重复的斜杠命令（基于 IDE 的代理）

一些基于 IDE 的代理（如 Kilo Code、Windsurf）在升级后可能显示**重复的斜杠命令**——旧版本和新版本都出现。

**解决方案：**从你的代理文件夹手动删除旧命令文件。

**Kilo Code 示例：**

```bash
# 导航到代理的命令文件夹
cd .kilocode/rules/

# 列出文件并识别重复项
ls -la

# 删除旧版本（示例文件名——你的可能不同）
rm speckit.specify-old.md
rm speckit.plan-v1.md
```

重启你的 IDE 以刷新命令列表。

---

## 常见场景

### 场景 1："我只想要新的斜杠命令"

```bash
# 升级 CLI（如果使用持久安装）
uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git

# 更新项目文件以获取新命令
specify init --here --force --ai copilot

# 如果自定义，恢复你的宪法
git restore .specify/memory/constitution.md
```

### 场景 2："我自定义了模板和宪法"

```bash
# 1. 备份自定义
cp .specify/memory/constitution.md /tmp/constitution-backup.md
cp -r .specify/templates /tmp/templates-backup

# 2. 升级 CLI
uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git

# 3. 更新项目
specify init --here --force --ai copilot

# 4. 恢复自定义
mv /tmp/constitution-backup.md .specify/memory/constitution.md
# 如果需要，手动合并模板更改
```

### 场景 3："我在我的 IDE 中看到重复的斜杠命令"

这发生在基于 IDE 的代理（Kilo Code、Windsurf、Roo Code 等）中。

```bash
# 找到代理文件夹（示例：.kilocode/rules/）
cd .kilocode/rules/

# 列出所有文件
ls -la

# 删除旧命令文件
rm speckit.old-command-name.md

# 重启你的 IDE
```

### 场景 4："我在没有 Git 的项目上工作"

如果你使用 `--no-git` 初始化了你的项目，你仍然可以升级：

```bash
# 手动备份你自定义的文件
cp .specify/memory/constitution.md /tmp/constitution-backup.md

# 运行升级
specify init --here --force --ai copilot --no-git

# 恢复自定义
mv /tmp/constitution-backup.md .specify/memory/constitution.md
```

`--no-git` 标志跳过 git 初始化但不影响文件更新。

---

## 使用 `--no-git` 标志

`--no-git` 标志告诉 Spec Kit **跳过 git 仓库初始化**。这在以下情况下很有用：

- 你以不同方式管理版本控制（Mercurial、SVN 等）
- 你的项目是具有现有 git 设置的更大 monorepo 的一部分
- 你在实验，还不想要版本控制

**在初始设置期间：**

```bash
specify init my-project --ai copilot --no-git
```

**在升级期间：**

```bash
specify init --here --force --ai copilot --no-git
```

### `--no-git` 不做什么

❌ 不防止文件更新
❌ 不跳过斜杠命令安装
❌ 不影响模板合并

它**仅**跳过运行 `git init` 和创建初始提交。

### 在没有 Git 的情况下工作

如果你使用 `--no-git`，你需要手动管理功能目录：

**在使用规划命令之前设置 `SPECIFY_FEATURE` 环境变量：**

```bash
# Bash/Zsh
export SPECIFY_FEATURE="001-my-feature"

# PowerShell
$env:SPECIFY_FEATURE = "001-my-feature"
```

这告诉 Spec Kit 在不使用 Git 分支时使用哪个功能目录。

**为什么这很重要：**没有 git，Spec Kit 不能检测你的当前分支名称来确定活跃功能。环境变量手动提供该上下文。

---

## 故障排除

### "升级后斜杠命令没有显示"

**原因：**代理没有重新加载命令文件。

**修复：**

1. **完全重启你的 IDE/编辑器**（不仅仅是重新加载窗口）
2. **对于基于 CLI 的代理**，验证文件存在：

   ```bash
   ls -la .claude/commands/      # Claude Code
   ls -la .gemini/commands/       # Gemini
   ls -la .cursor/commands/       # Cursor
   ```

3. **检查代理特定设置：**
   - Codex 需要 `CODEX_HOME` 环境变量
   - 某些代理需要工作区重启或缓存清除

### "我失去了我的宪法自定义"

**修复：**从 git 或备份恢复：

```bash
# 如果你在升级前提交了
git restore .specify/memory/constitution.md

# 如果你手动备份了
cp /tmp/constitution-backup.md .specify/memory/constitution.md
```

**预防：**在升级前始终提交或备份 `constitution.md`。

### "警告：当前目录不为空"

**完整警告消息：**

```text
警告：当前目录不为空（25 项）
模板文件将与现有内容合并，可能覆盖现有文件
你想继续吗？[y/N]
```

**这意味着什么：**

此警告在你在已有文件的目录中运行 `specify init --here`（或 `specify init .`）时出现。它告诉你：

1. **目录有现有内容** - 在示例中，25 个文件/文件夹
2. **文件将被合并** - 新模板文件将添加到你的现有文件中
3. **某些文件可能被覆盖** - 如果你已有 Spec Kit 文件（`.claude/`、`.specify/` 等），它们将被替换为新版本

**什么会被覆盖：**

仅 Spec Kit 基础设施文件：

- 代理命令文件（`.claude/commands/`、`.github/prompts/` 等）
- `.specify/scripts/` 中的脚本
- `.specify/templates/` 中的模板
- `.specify/memory/` 中的内存文件（包括宪法）

**什么保持不变：**

- 你的 `specs/` 目录（规格说明、计划、任务）
- 你的源代码文件
- 你的 `.git/` 目录和 git 历史
- 任何其他不是 Spec Kit 模板的文件

**如何回应：**

- **输入 `y` 并按 Enter** - 继续合并（升级时推荐）
- **输入 `n` 并按 Enter** - 取消操作
- **使用 `--force` 标志** - 完全跳过此确认：

  ```bash
  specify init --here --force --ai copilot
  ```

**何时看到此警告：**

- ✅ **预期**升级现有 Spec Kit 项目时
- ✅ **预期**将 Spec Kit 添加到现有代码库时
- ⚠️ **意外**如果你认为你在空目录中创建新项目

**预防提示：**升级前提交或备份你的 `.specify/memory/constitution.md`，如果你自定义了它。

### "CLI 升级似乎不起作用"

验证安装：

```bash
# 检查已安装的工具
uv tool list

# 应该显示 specify-cli

# 验证路径
which specify

# 应该指向 uv 工具安装目录
```

如果未找到，重新安装：

```bash
uv tool uninstall specify-cli
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

### "我每次打开项目时都需要运行 specify 吗？"

**简短回答：**不，你只在每个项目运行一次 `specify init`（或升级时）。

**解释：**

`specify` CLI 工具用于：

- **初始设置：** `specify init` 在你的项目中引导 Spec Kit
- **升级：** `specify init --here --force` 更新模板和命令
- **诊断：** `specify check` 验证工具安装

一旦你运行了 `specify init`，斜杠命令（如 `/speckit.specify`、`/speckit.plan` 等）**永久安装**在你的项目的代理文件夹（`.claude/`、`.github/prompts/` 等）中。你的 AI 助手直接读取这些命令文件——不需要再次运行 `specify`。

**如果你的代理无法识别斜杠命令：**

1. **验证命令文件存在：**

   ```bash
   # 对于 GitHub Copilot
   ls -la .github/prompts/

   # 对于 Claude
   ls -la .claude/commands/
   ```

2. **完全重启你的 IDE/编辑器**（不仅仅是重新加载窗口）

3. **检查你在正确的目录中** 你运行 `specify init` 的位置

4. **对于某些代理**，你可能需要重新加载工作区或清除缓存

**相关问题：**如果 Copilot 无法打开本地文件或意外使用 PowerShell 命令，这通常是 IDE 上下文问题，与 `specify` 无关。尝试：

- 重启 VS Code
- 检查文件权限
- 确保工作区文件夹已正确打开

---

## 版本兼容性

Spec Kit 遵循主要版本的语义版本控制。CLI 和项目文件设计为在同一主要版本内兼容。

**最佳实践：**在主要版本更改期间通过同时升级两者来保持 CLI 和项目文件同步。

---

## 后续步骤

升级后：

- **测试新斜杠命令：**运行 `/speckit.constitution` 或另一个命令以验证一切有效
- **查看发布说明：**检查 [GitHub Releases](https://github.com/github/spec-kit/releases) 了解新功能和破坏性更改
- **更新工作流：**如果添加了新命令，更新你的团队的开发工作流
- **检查文档：**访问 [github.io/spec-kit](https://github.github.io/spec-kit/) 了解更新的指南
