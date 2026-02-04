# 本地开发指南

本指南展示了如何在本地迭代 `specify` CLI，而无需发布发布或首先提交到 `main`。

> 脚本现在有 Bash（`.sh`）和 PowerShell（`.ps1`）变体。CLI 根据操作系统自动选择，除非你传递 `--script sh|ps`。

## 1. 克隆并切换分支

```bash
git clone https://github.com/github/spec-kit.git
cd spec-kit
# 在功能分支上工作
git checkout -b your-feature-branch
```

## 2. 直接运行 CLI（最快反馈）

你可以通过模块入口点执行 CLI，而无需安装任何内容：

```bash
# 从仓库根目录
python -m src.specify_cli --help
python -m src.specify_cli init demo-project --ai claude --ignore-agent-tools --script sh
```

如果你更喜欢调用脚本文件风格（使用 shebang）：

```bash
python src/specify_cli/__init__.py init demo-project --script ps
```

## 3. 使用可编辑安装（隔离环境）

使用 `uv` 创建隔离环境，以便依赖项的解析方式与最终用户完全相同：

```bash
# 创建并激活虚拟环境（uv 自动管理 .venv）
uv venv
source .venv/bin/activate  # 或在 Windows PowerShell 上：.venv\Scripts\Activate.ps1

# 在可编辑模式下安装项目
uv pip install -e .

# 现在 'specify' 入口点可用
specify --help
```

由于可编辑模式，代码编辑后重新运行不需要重新安装。

## 4. 直接从 Git 使用 uvx 调用（当前分支）

`uvx` 可以从本地路径（或 Git ref）运行以模拟用户流：

```bash
uvx --from . specify init demo-uvx --ai copilot --ignore-agent-tools --script sh
```

你也可以指向 uvx 到特定分支而无需合并：

```bash
# 首先推送你的工作分支
git push origin your-feature-branch
uvx --from git+https://github.com/github/spec-kit.git@your-feature-branch specify init demo-branch-test --script ps
```

### 4a. 绝对路径 uvx（从任何地方运行）

如果你在另一个目录中，使用绝对路径而不是 `.`：

```bash
uvx --from /mnt/c/GitHub/spec-kit specify --help
uvx --from /mnt/c/GitHub/spec-kit specify init demo-anywhere --ai copilot --ignore-agent-tools --script sh
```

为方便起见设置环境变量：

```bash
export SPEC_KIT_SRC=/mnt/c/GitHub/spec-kit
uvx --from "$SPEC_KIT_SRC" specify init demo-env --ai copilot --ignore-agent-tools --script ps
```

（可选）定义 shell 函数：

```bash
specify-dev() { uvx --from /mnt/c/GitHub/spec-kit specify "$@"; }
# 然后
specify-dev --help
```

## 5. 测试脚本权限逻辑

运行 `init` 后，检查 shell 脚本在 POSIX 系统上是否可执行：

```bash
ls -l scripts | grep .sh
# 期望所有者执行位（例如 -rwxr-xr-x）
```

在 Windows 上，你将改为使用 `.ps1` 脚本（不需要 chmod）。

## 6. 运行 Lint / 基本检查（添加你自己的）

目前没有强制的 lint 配置，但你可以快速进行理智检查导入性：

```bash
python -c "import specify_cli; print('Import OK')"
```

## 7. 在本地构建轮子（可选）

在发布前验证打包：

```bash
uv build
ls dist/
```

如果需要，将构建的工件安装到新的临时环境中。

## 8. 使用临时工作区

在脏目录中测试 `init --here` 时，创建临时工作区：

```bash
mkdir /tmp/spec-test && cd /tmp/spec-test
python -m src.specify_cli init --here --ai claude --ignore-agent-tools --script sh  # 如果仓库复制到这里
```

或仅复制修改的 CLI 部分，如果你想要更轻的沙箱。

## 9. 调试网络 / TLS 跳过

如果你需要在实验时绕过 TLS 验证：

```bash
specify check --skip-tls
specify init demo --skip-tls --ai gemini --ignore-agent-tools --script ps
```

（仅用于本地实验。）

## 10. 快速编辑循环摘要

| 操作 | 命令 |
|--------|---------|
| 直接运行 CLI | `python -m src.specify_cli --help` |
| 可编辑安装 | `uv pip install -e .` 然后 `specify ...` |
| 本地 uvx 运行（仓库根目录） | `uvx --from . specify ...` |
| 本地 uvx 运行（绝对路径） | `uvx --from /mnt/c/GitHub/spec-kit specify ...` |
| Git 分支 uvx | `uvx --from git+URL@branch specify ...` |
| 构建轮子 | `uv build` |

## 11. 清理

快速删除构建工件 / 虚拟环境：

```bash
rm -rf .venv dist build *.egg-info
```

## 12. 常见问题

| 症状 | 修复 |
|---------|-----|
| `ModuleNotFoundError: typer` | 运行 `uv pip install -e .` |
| 脚本不可执行（Linux） | 重新运行 init 或 `chmod +x scripts/*.sh` |
| Git 步骤被跳过 | 你传递了 `--no-git` 或 Git 未安装 |
| 下载了错误的脚本类型 | 明确传递 `--script sh` 或 `--script ps` |
| 企业网络上的 TLS 错误 | 尝试 `--skip-tls`（不用于生产） |

## 13. 后续步骤

- 使用你修改的 CLI 更新文档并运行快速启动
- 满意时打开 PR
- （可选）一旦更改登陆 `main` 时标记发布
