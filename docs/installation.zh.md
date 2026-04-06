# 安装指南

## 前置条件

- **Linux/macOS**（或 Windows；现在支持 PowerShell 脚本，无需 WSL）
- AI 编码代理：[Claude Code](https://www.anthropic.com/claude-code)、[GitHub Copilot](https://code.visualstudio.com/)、[Codebuddy CLI](https://www.codebuddy.ai/cli)、[Gemini CLI](https://github.com/google-gemini/gemini-cli) 或 [Pi Coding Agent](https://pi.dev)
- [uv](https://docs.astral.sh/uv/) 用于包管理
- [Python 3.11+](https://www.python.org/downloads/)
- [Git](https://git-scm.com/downloads)

## 安装

### 初始化新项目

开始的最简单方法是初始化一个新项目。建议固定一个特定的发布标签以确保稳定性（查看 [Releases](https://github.com/github/spec-kit/releases) 获取最新版本）：

```bash
# 从特定的稳定版本安装（推荐 — 将 vX.Y.Z 替换为最新标签）
uvx --from git+https://github.com/github/spec-kit.git@vX.Y.Z specify init <PROJECT_NAME>

# 或从 main 安装最新版本（可能包含未发布的更改）
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME>
```

或在当前目录中初始化：

```bash
uvx --from git+https://github.com/github/spec-kit.git@vX.Y.Z specify init .
# 或使用 --here 标志
uvx --from git+https://github.com/github/spec-kit.git@vX.Y.Z specify init --here
```

### 指定 AI 代理

你可以在初始化时主动指定你的 AI 代理：

```bash
uvx --from git+https://github.com/github/spec-kit.git@vX.Y.Z specify init <project_name> --ai claude
uvx --from git+https://github.com/github/spec-kit.git@vX.Y.Z specify init <project_name> --ai gemini
uvx --from git+https://github.com/github/spec-kit.git@vX.Y.Z specify init <project_name> --ai copilot
uvx --from git+https://github.com/github/spec-kit.git@vX.Y.Z specify init <project_name> --ai codebuddy
uvx --from git+https://github.com/github/spec-kit.git@vX.Y.Z specify init <project_name> --ai pi
```

### 指定脚本类型（Shell vs PowerShell）

所有自动化脚本现在都有 Bash（`.sh`）和 PowerShell（`.ps1`）两种变体。

自动行为：

- Windows 默认：`ps`
- 其他操作系统默认：`sh`
- 交互模式：除非你传递 `--script`，否则系统会提示你选择

强制特定脚本类型：

```bash
uvx --from git+https://github.com/github/spec-kit.git@vX.Y.Z specify init <project_name> --script sh
uvx --from git+https://github.com/github/spec-kit.git@vX.Y.Z specify init <project_name> --script ps
```

### 忽略代理工具检查

如果你想在不检查工具是否正确的情况下直接获取模板：

```bash
uvx --from git+https://github.com/github/spec-kit.git@vX.Y.Z specify init <project_name> --ai claude --ignore-agent-tools
```

## 验证

初始化后，你应该在 AI 代理中看到以下可用命令：

- `/speckit.specify` - 创建规格说明
- `/speckit.plan` - 生成实现计划  
- `/speckit.tasks` - 分解为可操作的任务

`.specify/scripts` 目录将包含 `.sh` 和 `.ps1` 脚本。

## 故障排除

### 企业 / 隔离网络安装

如果你的环境阻止访问 PyPI（运行 `uv tool install` 或 `pip install` 时出现 403 错误），你可以在联网机器上创建便携式 wheel 包，然后将其传输到隔离网络目标机器。

**步骤 1：在联网机器上构建 wheel（与目标机器相同的操作系统和 Python 版本）**

```bash
# 克隆仓库
git clone https://github.com/github/spec-kit.git
cd spec-kit

# 构建 wheel
pip install build
python -m build --wheel --outdir dist/

# 下载 wheel 及其所有运行时依赖
pip download -d dist/ dist/specify_cli-*.whl
```

> **重要提示：** `pip download` 会解析平台特定的 wheel（例如 PyYAML 包含原生扩展）。你必须在与隔离网络目标机器**相同操作系统和 Python 版本**的机器上运行此步骤。如果你需要支持多个平台，请在每个目标操作系统（Linux、macOS、Windows）和 Python 版本上重复此步骤。

**步骤 2：将 `dist/` 目录传输到隔离网络机器**

将整个 `dist/` 目录（包含 `specify-cli` wheel 及所有依赖 wheel）通过 USB、网络共享或其他批准的传输方式复制到目标机器。

**步骤 3：在隔离网络机器上安装**

```bash
pip install --no-index --find-links=./dist specify-cli
```

**步骤 4：初始化项目（无需网络）**

```bash
# 初始化项目 — 无需访问 GitHub
specify init my-project --ai claude --offline
```

`--offline` 标志告诉 CLI 使用 wheel 中捆绑的模板、命令和脚本，而不是从 GitHub 下载。

> **弃用通知：** 从 v0.6.0 开始，`specify init` 将默认使用捆绑资源，`--offline` 标志将被移除。GitHub 下载路径将被淘汰，因为捆绑资源消除了网络访问的需求、避免了代理/防火墙问题，并保证模板始终与已安装的 CLI 版本匹配。无需任何操作 — `specify init` 将直接在无网络访问的情况下正常工作。

> **注意：** 需要 Python 3.11+。

> **Windows 注意：** 离线脚手架需要 PowerShell 7+（`pwsh`），而非 Windows PowerShell 5.x（`powershell.exe`）。请从 https://aka.ms/powershell 安装。

### Linux 上的 Git 凭证管理器

如果你在 Linux 上遇到 Git 身份验证问题，可以安装 Git 凭证管理器：

```bash
#!/usr/bin/env bash
set -e
echo "Downloading Git Credential Manager v2.6.1..."
wget https://github.com/git-ecosystem/git-credential-manager/releases/download/v2.6.1/gcm-linux_amd64.2.6.1.deb
echo "Installing Git Credential Manager..."
sudo dpkg -i gcm-linux_amd64.2.6.1.deb
echo "Configuring Git to use GCM..."
git config --global credential.helper manager
echo "Cleaning up..."
rm gcm-linux_amd64.2.6.1.deb
```
