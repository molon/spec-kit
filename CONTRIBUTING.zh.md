# 为 Spec Kit 做贡献

你好！我们很高兴你想为 Spec Kit 做贡献。对此项目的贡献是[发布](https://help.github.com/articles/github-terms-of-service/#6-contributions-under-repository-license)
根据项目的[开源许可证](LICENSE)向公众提供。

请注意，此项目是通过[贡献者行为准则](CODE_OF_CONDUCT.md)发布的。通过参与此项目，你同意遵守其条款。

## 运行和测试代码的前置条件

这些是能够在本地测试你的更改作为拉取请求（PR）提交流程的一部分所需的一次性安装。

1. 安装 [Python 3.11+](https://www.python.org/downloads/)
1. 安装 [uv](https://docs.astral.sh/uv/) 用于包管理
1. 安装 [Git](https://git-scm.com/downloads)
1. 有一个[可用的 AI 编码代理](README.md#-supported-ai-agents)

<details>
<summary><b>💡 如果你使用 <code>VSCode</code> 或 <code>GitHub Codespaces</code> 作为你的 IDE 的提示</b></summary>

<br>

假设你在你的机器上安装了 [Docker](https://docker.com)，你可以通过此 [VSCode 扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) 利用 [Dev Containers](https://containers.dev)，轻松设置你的开发环境，上述工具已安装和配置，感谢 `.devcontainer/devcontainer.json` 文件（位于项目的根目录）。

要这样做，只需：

- 检出仓库
- 使用 VSCode 打开它
- 打开[命令面板](https://code.visualstudio.com/docs/getstarted/userinterface#_command-palette)并选择"Dev Containers: Open Folder in Container..."

在 [GitHub Codespaces](https://github.com/features/codespaces) 上，它甚至更简单，因为它在打开代码空间时自动利用 `.devcontainer/devcontainer.json`。

</details>

## 提交拉取请求

> [!NOTE]
> 如果你的拉取请求引入了对 CLI 或仓库其余部分的工作产生重大影响的大型更改（例如，你引入新模板、参数或其他主要更改），请确保它**被讨论和同意**由项目维护者。没有进行先前对话和协议的大型更改的拉取请求将被关闭。

1. Fork 并克隆仓库
1. 配置并安装依赖项：`uv sync`
1. 确保 CLI 在你的机器上工作：`uv run specify --help`
1. 创建新分支：`git checkout -b my-branch-name`
1. 进行更改、添加测试并确保一切仍然有效
1. 如果相关，使用示例项目测试 CLI 功能
1. 推送到你的 fork 并提交拉取请求
1. 等待你的拉取请求被审查和合并。

以下是一些可以增加拉取请求被接受的可能性的事情：

- 遵循项目的编码约定。
- 为新功能编写测试。
- 更新文档（`README.md`、`spec-driven.md`），如果你的更改影响面向用户的功能。
- 保持你的更改尽可能集中。如果有多个你想做的不相互依赖的更改，请考虑将它们作为单独的拉取请求提交。
- 写一个[好的提交消息](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)。
- 使用规格驱动开发工作流测试你的更改，以确保兼容性。

## 开发工作流

在处理 spec-kit 时：

1. 使用 `specify` CLI 命令（`/speckit.specify`、`/speckit.plan`、`/speckit.tasks`）在你选择的编码代理中测试更改
2. 验证 `templates/` 目录中的模板工作正确
3. 在 `scripts/` 目录中测试脚本功能
4. 如果进行了主要流程更改，请确保更新内存文件（`memory/constitution.md`）

### 在本地测试模板和命令更改

运行 `uv run specify init` 会拉取已发布的包，这不会包括你的本地更改。
要在本地测试你的模板、命令和其他更改，请按照以下步骤操作：

1. **创建发布包**

   运行以下命令以生成本地包：

   ```bash
   ./.github/workflows/scripts/create-release-packages.sh v1.0.0
   ```

2. **将相关包复制到你的测试项目**

   ```bash
   cp -r .genreleases/sdd-copilot-package-sh/. <path-to-test-project>/
   ```

3. **打开并测试代理**

   导航到你的测试项目文件夹并打开代理以验证你的实现。

## Spec Kit 中的 AI 贡献

> [!IMPORTANT]
>
> 如果你使用**任何类型的 AI 协助**为 Spec Kit 做贡献，
> 必须在拉取请求或问题中披露。

我们欢迎并鼓励使用 AI 工具来帮助改进 Spec Kit！许多有价值的贡献已通过 AI 协助增强，用于代码生成、问题检测和功能定义。

话虽如此，如果你在为 Spec Kit 做贡献时使用任何类型的 AI 协助（例如，代理、ChatGPT），
**这必须在拉取请求或问题中披露**，以及 AI 协助的范围（例如，文档注释与代码生成）。

如果你的 PR 响应或评论由 AI 生成，也请披露。

作为例外，琐碎的间距或拼写错误修复不需要披露，只要更改仅限于代码的小部分或短语。

一个披露示例：

> 此 PR 主要由 GitHub Copilot 编写。

或更详细的披露：

> 我咨询了 ChatGPT 来理解代码库，但解决方案完全由我自己手动编写。

未能披露这首先是对拉取请求另一端的人类操作员的不尊重，但它也使得难以确定对贡献应用多少审查。

在一个完美的世界中，AI 协助会产生与任何人类相等或更高质量的工作。那不是我们生活的世界，在大多数情况下
其中人类监督或专业知识不在循环中，它生成无法合理维护或演变的代码。

### 我们在寻找什么

提交 AI 协助的贡献时，请确保它们包括：

- **清晰的 AI 使用披露** - 你对 AI 使用和程度透明
- **人类理解和测试** - 你已个人测试更改并理解它们的作用
- **清晰的理由** - 你可以解释为什么需要更改以及它如何适应 Spec Kit 的目标
- **具体证据** - 包括演示改进的测试用例、场景或示例
- **你自己的分析** - 分享你对端到端开发者体验的想法

### 我们将关闭什么

我们保留关闭似乎是以下内容的贡献的权利：

- 未经验证就提交的未测试更改
- 不解决特定 Spec Kit 需求的通用建议
- 显示没有人类审查或理解的批量提交

### 成功指南

关键是证明你理解并验证了你提议的更改。如果维护者可以轻松判断贡献完全由 AI 生成而没有人类输入或测试，它可能需要在提交前进行更多工作。

一致提交低努力 AI 生成更改的贡献者可能会被维护者自行限制进一步贡献。

请尊重维护者并披露 AI 协助。

## 资源

- [规格驱动开发方法论](./spec-driven.md)
- [如何为开源做贡献](https://opensource.guide/how-to-contribute/)
- [使用拉取请求](https://help.github.com/articles/about-pull-requests/)
- [GitHub 帮助](https://help.github.com)
