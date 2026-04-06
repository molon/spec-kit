# 本地开发指南

本指南介绍如何在本地迭代 `specify` CLI，而无需先发布版本或提交到 `main`。

> 脚本现在同时提供 Bash（`.sh`）和 PowerShell（`.ps1`）两种变体。CLI 会根据操作系统自动选择，除非你传递 `--script sh|ps`。

## 1. 克隆并切换分支

```bash
git clone https://github.com/github/spec-kit.git
cd spec-kit
# 在功能分支上工作
git checkout -b your-feature-branch
```

## 2. 直接运行 CLI（最快反馈）

你可以通过模块入口点执行 CLI，无需安装任何东西：

```bash
# 从仓库根目录
python -m src.specify_cli --help
python -m src.specify_cli init demo-project --ai claude --ignore-agent-tools --script sh
```

如果你更喜欢脚本文件调用方式（使用 shebang）：

```bash
python src/specify_cli/__init__.py init demo-project --script ps
```

## 3. 使用可编辑安装（隔离环境）

使用 `uv` 创建隔离环境，使依赖解析与最终用户完全一致：

```bash
# 创建并激活虚拟环境（uv 自动管理 .venv）
uv venv
source .venv/bin/activate  # 或在 Windows PowerShell 上：.venv\Scripts\Activate.ps1

# 以可编辑模式安装项目
uv pip install -e .

# 现在 'specify' 入口点可用
specify --help
```

由于使用了可编辑模式，代码修改后重新运行无需重新安装。

## 4. 直接从 Git 使用 uvx 调用（当前分支）

`uvx` 可以从本地路径（或 Git ref）运行，以模拟用户流程：

```bash
uvx --from . specify init demo-uvx --ai copilot --ignore-agent-tools --script sh
```

你也可以将 uvx 指向特定分支而无需合并：

```bash
# 首先推送你的工作分支
git push origin your-feature-branch
uvx --from git+https://github.com/github/spec-kit.git@your-feature-branch specify init demo-branch-test --script ps
```

### 4a. 绝对路径 uvx（从任意位置运行）

如果你在其他目录中，使用绝对路径代替 `.`：

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
# 期望有所有者执行位（例如 -rwxr-xr-x）
```

在 Windows 上则使用 `.ps1` 脚本（无需 chmod）。

## 6. 运行 Lint / 基本检查（可自行添加）

目前没有内置强制的 lint 配置，但你可以快速检查模块是否可导入：

```bash
python -c "import specify_cli; print('Import OK')"
```

## 7. 本地构建 Wheel（可选）

在发布前验证打包：

```bash
uv build
ls dist/
```

如有需要，可将构建产物安装到全新的临时环境中。

## 8. 使用临时工作区

在脏目录中测试 `init --here` 时，创建临时工作区：

```bash
mkdir /tmp/spec-test && cd /tmp/spec-test
python -m src.specify_cli init --here --ai claude --ignore-agent-tools --script sh  # 如果仓库已复制到此处
```

或者仅复制修改过的 CLI 部分，以获得更轻量的沙箱环境。

## 9. 调试网络 / TLS 跳过

如果你需要在实验时绕过 TLS 验证：

```bash
specify check --skip-tls
specify init demo --skip-tls --ai gemini --ignore-agent-tools --script ps
```

（仅用于本地实验。）

## 10. 快速编辑循环总结

| 操作 | 命令 |
|--------|---------|
| 直接运行 CLI | `python -m src.specify_cli --help` |
| 可编辑安装 | `uv pip install -e .` 然后 `specify ...` |
| 本地 uvx 运行（仓库根目录） | `uvx --from . specify ...` |
| 本地 uvx 运行（绝对路径） | `uvx --from /mnt/c/GitHub/spec-kit specify ...` |
| Git 分支 uvx | `uvx --from git+URL@branch specify ...` |
| 构建 Wheel | `uv build` |

## 11. 清理

快速删除构建产物 / 虚拟环境：

```bash
rm -rf .venv dist build *.egg-info
```

## 12. 常见问题

| 症状 | 修复方法 |
|---------|-----|
| `ModuleNotFoundError: typer` | 运行 `uv pip install -e .` |
| 脚本不可执行（Linux） | 重新运行 init 或 `chmod +x scripts/*.sh` |
| Git 步骤被跳过 | 你传递了 `--no-git` 或未安装 Git |
| 下载了错误的脚本类型 | 显式传递 `--script sh` 或 `--script ps` |
| 企业网络上的 TLS 错误 | 尝试 `--skip-tls`（不用于生产环境） |

## 13. 后续步骤

- 使用你修改后的 CLI 更新文档并完成快速入门流程
- 满意后打开 PR
- （可选）更改合并到 `main` 后标记发布版本
