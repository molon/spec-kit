# 为 Spec Kit 做贡献

你好！我们很高兴你想为 Spec Kit 做贡献。对此项目的贡献将根据[项目的开源许可证](LICENSE)[发布](https://help.github.com/articles/github-terms-of-service/#6-contributions-under-repository-license)给公众。

请注意，此项目附带了[贡献者行为准则](CODE_OF_CONDUCT.md)。参与此项目即表示你同意遵守其条款。

## 运行和测试代码的前置条件

这些是能够在本地测试你的更改（作为拉取请求 (PR) 提交流程的一部分）所需的一次性安装。

1. 安装 [Python 3.11+](https://www.python.org/downloads/)
1. 安装 [uv](https://docs.astral.sh/uv/) 用于包管理
1. 安装 [Git](https://git-scm.com/downloads)
1. 有一个[可用的 AI 编码代理](README.md#-supported-ai-agents)

<details>
<summary><b>💡 如果你使用 <code>VSCode</code> 或 <code>GitHub Codespaces</code> 作为 IDE 的提示</b></summary>

<br>

假设你的机器上安装了 [Docker](https://docker.com)，你可以通过此 [VSCode 扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)利用 [Dev Containers](https://containers.dev)，借助 `.devcontainer/devcontainer.json` 文件（位于项目根目录），轻松设置开发环境，上述工具均已安装和配置。

只需：

- 检出仓库
- 用 VSCode 打开它
- 打开[命令面板](https://code.visualstudio.com/docs/getstarted/userinterface#_command-palette)并选择 "Dev Containers: Open Folder in Container..."

在 [GitHub Codespaces](https://github.com/features/codespaces) 上更简单，因为它在打开代码空间时会自动使用 `.devcontainer/devcontainer.json`。

</details>

## 提交拉取请求

> [!NOTE]
> 如果你的拉取请求引入了对 CLI 或仓库其余部分产生重大影响的大型更改（例如，你引入了新模板、参数或其他重大更改），请确保已**经项目维护者讨论并达成一致**。未经事先沟通和达成共识的大型更改拉取请求将被关闭。

1. Fork 并克隆仓库
1. 配置并安装依赖项：`uv sync --extra test`
1. 确保 CLI 在你的机器上正常工作：`uv run specify --help`
1. 创建新分支：`git checkout -b my-branch-name`
1. 进行更改、添加测试并确保一切仍然正常
1. 如果相关，使用示例项目测试 CLI 功能
1. 推送到你的 fork 并提交拉取请求
1. 等待你的拉取请求被审查和合并。

有关详细的测试工作流、命令选择提示和 PR 报告模板，请参阅 [`TESTING.md`](./TESTING.md)。
激活项目虚拟环境（参见 [`TESTING.md`](./TESTING.md) 中的"设置"部分），然后从你的工作树安装 CLI（在 `uv sync --extra test` 之后运行 `uv pip install -e .`），或者确保 shell 使用本地的 `specify` 二进制文件，然后再运行下面描述的手动斜杠命令测试。

以下是一些可以增加你的拉取请求被接受可能性的建议：

- 遵循项目的编码约定。
- 为新功能编写测试。
- 如果你的更改影响面向用户的功能，请更新文档（`README.md`、`spec-driven.md`）。
- 保持你的更改尽可能集中。如果你想做多个互不依赖的更改，请考虑将它们作为单独的拉取请求提交。
- 写一个[好的提交消息](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)。
- 使用规格驱动开发工作流测试你的更改以确保兼容性。

## 开发工作流

在处理 spec-kit 时：

1. 在你选择的编码代理中使用 `specify` CLI 命令（`/speckit.specify`、`/speckit.plan`、`/speckit.tasks`）测试更改
2. 验证 `templates/` 目录中的模板正常工作
3. 在 `scripts/` 目录中测试脚本功能
4. 如果进行了重大流程更改，请确保更新内存文件（`memory/constitution.md`）

### 推荐的验证流程

为了获得最顺畅的审查体验，请按以下顺序验证更改：

1. **先运行聚焦的自动化检查** — 使用 [`TESTING.md`](./TESTING.md) 中的快速验证命令，尽早发现打包、脚手架和配置方面的回归问题。
2. **其次运行手动工作流测试** — 如果你的更改影响斜杠命令或开发者工作流，请按照 [`TESTING.md`](./TESTING.md) 选择正确的命令，在代理中运行它们，并为你的 PR 捕获结果。
3. **调试打包输出时使用本地发布包** — 如果你需要检查 CI 风格打包产生的确切文件，请按照下面的描述生成本地发布包。

### 在本地测试模板和命令更改

运行 `uv run specify init` 会拉取已发布的包，其中不会包含你的本地更改。
要在本地测试你的模板、命令和其他更改，请按照以下步骤操作：

1. **创建发布包**

   运行以下命令生成本地包：

   ```bash
   ./.github/workflows/scripts/create-release-packages.sh v1.0.0
   ```

2. **将相关包复制到你的测试项目**

   ```bash
   cp -r .genreleases/sdd-copilot-package-sh/. <path-to-test-project>/
   ```

3. **打开并测试代理**

   导航到你的测试项目文件夹并打开代理以验证你的实现。

如果你只需要在进行手动代理测试之前验证生成的文件结构和内容，请先使用 [`TESTING.md`](./TESTING.md) 中的聚焦自动化检查。本节适用于你需要在本地检查确切打包输出的情况。

## Spec Kit 中的 AI 贡献

> [!IMPORTANT]
>
> 如果你使用**任何类型的 AI 辅助**为 Spec Kit 做贡献，
> 必须在拉取请求或 issue 中披露。

我们欢迎并鼓励使用 AI 工具来帮助改进 Spec Kit！许多有价值的贡献已通过 AI 辅助得到增强，用于代码生成、问题检测和功能定义。

话虽如此，如果你在为 Spec Kit 做贡献时使用任何类型的 AI 辅助（例如代理、ChatGPT），
**这必须在拉取请求或 issue 中披露**，以及 AI 辅助使用的程度（例如文档注释与代码生成）。

如果你的 PR 回复或评论是由 AI 生成的，也请一并披露。

作为例外，琐碎的间距或拼写错误修复不需要披露，只要更改仅限于代码的小部分或短语。

一个披露示例：

> 此 PR 主要由 GitHub Copilot 编写。

或更详细的披露：

> 我咨询了 ChatGPT 来理解代码库，但解决方案完全由我自己手动编写。

未能披露这一点首先是对拉取请求另一端的人类操作员的不尊重，同时也使得难以确定应对贡献施加多少审查力度。

在一个完美的世界中，AI 辅助会产出与任何人类同等或更高质量的工作。但那不是我们所处的世界，在大多数没有人类监督或专业知识参与的情况下，AI 生成的代码无法被合理地维护或演进。

### 我们在寻找什么

提交 AI 辅助的贡献时，请确保包括：

- **清晰的 AI 使用披露** - 你对 AI 的使用及其程度保持透明
- **人类理解和测试** - 你已亲自测试更改并理解其作用
- **清晰的理由** - 你能解释为什么需要这个更改以及它如何契合 Spec Kit 的目标
- **具体证据** - 包括展示改进效果的测试用例、场景或示例
- **你自己的分析** - 分享你对端到端开发者体验的看法

### 我们会关闭什么

我们保留关闭以下类型贡献的权利：

- 未经验证就提交的未测试更改
- 不针对 Spec Kit 具体需求的通用建议
- 没有经过人类审查或理解的批量提交

### 成功指南

关键是证明你理解并验证了你提出的更改。如果维护者能轻易看出某个贡献完全由 AI 生成而没有经过人类输入或测试，那它可能在提交前还需要更多工作。

持续提交低质量 AI 生成更改的贡献者可能会被维护者酌情限制进一步贡献。

请尊重维护者并披露 AI 辅助的使用。

## 资源

- [规格驱动开发方法论](./spec-driven.md)
- [如何为开源做贡献](https://opensource.guide/how-to-contribute/)
- [使用拉取请求](https://help.github.com/articles/about-pull-requests/)
- [GitHub 帮助](https://help.github.com)
