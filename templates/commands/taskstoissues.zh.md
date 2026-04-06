---
description: 根据可用的设计工件将现有任务转换为可操作的、依赖关系排序的 GitHub 问题。
tools: ['github/github-mcp-server/issue_write']
scripts:
  sh: scripts/bash/check-prerequisites.sh --json --require-tasks --include-tasks
  ps: scripts/powershell/check-prerequisites.ps1 -Json -RequireTasks -IncludeTasks
---

## 用户输入

```text
$ARGUMENTS
```

你**必须**在继续之前考虑用户输入（如果不为空）。

## 概述

1. 从仓库根目录运行 `{SCRIPT}` 并解析 FEATURE_DIR 和 AVAILABLE_DOCS 列表。所有路径必须是绝对路径。对于像"I'm Groot"这样的单引号参数，使用转义语法：例如 'I'\''m Groot'（或如果可能的话使用双引号："I'm Groot"）。
1. 从执行的脚本中，提取**任务**的路径。
1. 通过运行以下命令获取 Git 远程地址：

```bash
git config --get remote.origin.url
```

> [!CAUTION]
> 仅在远程地址是 GITHUB URL 时才继续后续步骤

1. 对于列表中的每个任务，使用 GitHub MCP 服务器在与 Git 远程地址匹配的仓库中创建一个新问题。

> [!CAUTION]
> 在任何情况下都绝不在与远程 URL 不匹配的仓库中创建问题
