# 更新日志

<!-- insert new changelog below this comment -->

## [0.5.0] - 2026-04-02

### 变更

- 引入 DEVELOPMENT.md (#2069)
- 将 Community Friends 中的 cc-sdd 引用更新为 cc-spex (#2007)
- chore: 发布 0.4.5，开始 0.4.6.dev0 开发 (#2064)

## [0.4.5] - 2026-04-02

### 变更

- 阶段 6：完成迁移 — 移除旧版脚手架路径 (#1924) (#2063)
- 安装 Claude Code 作为原生技能并对齐预设/集成流程 (#2051)
- 添加 repoindex 0402 (#2062)
- 阶段 5：技能、通用和选项驱动的集成 (#1924) (#2052)
- feat(scripts): 为 create-new-feature 添加 --dry-run 标志 (#1998)
- fix: 支持 4 位及以上数字的功能分支编号 (#2040)
- 添加社区内容免责声明 (#2058)
- docs: 在 README 和扩展文档中添加社区扩展网站链接 (#2014)
- docs: 移除失效的 Cognitive Squad 和 Understanding 扩展链接及 extensions/catalog.community.json 中的条目 (#2057)
- 将 fix-findings 扩展添加到社区目录 (#2039)
- 阶段 4：TOML 集成 — gemini 和 tabnine 迁移到插件架构 (#2050)
- feat: 将 5 个生命周期扩展添加到社区目录 (#2049)
- 阶段 3：标准 Markdown 集成 — 19 个代理迁移到插件架构 (#2038)
- chore: 发布 0.4.4，开始 0.4.5.dev0 开发 (#2048)

## [0.4.4] - 2026-04-01

### 变更

- 阶段 2：Copilot 集成 — 使用共享模板原语的概念验证 (#2035)
- docs: 同步 AGENTS.md 与 AGENT_CONFIG 中缺失的代理 (#2025)
- docs: 确保手动测试使用本地 specify (#2020)
- 阶段 1：集成基础 — 基类、清单系统和注册表 (#1925)
- fix: 加固 GitHub Actions 工作流 (#2021)
- chore: 在发布后的 main 分支使用 PEP 440 .dev0 版本号 (#2032)
- feat: 将 superpowers bridge 扩展添加到社区目录 (#2023)
- feat: 将 product-forge 扩展添加到社区目录 (#2012)
- feat(scripts): 为 create-new-feature 添加 --allow-existing-branch 标志 (#1999)
- fix(scripts): 修正 copilot-instructions.md 的路径 (#1997)
- 更新 README.md (#1995)
- fix: 防止扩展命令遮蔽 (#1994)
- 修复 npm 本地安装时的 Claude Code CLI 检测 (#1978)
- fix(scripts): 支持 PowerShell 代理和脚本过滤器 (#1969)
- feat: 将 MAQA 扩展套件（7 个扩展）添加到社区目录 (#1981)
- feat: 将 spec-kit-onboard 扩展添加到社区目录 (#1991)
- 将 plan-review-gate 添加到社区目录 (#1993)
- chore(deps): 将 actions/deploy-pages 从 4 升级到 5 (#1990)
- chore(deps): 将 DavidAnson/markdownlint-cli2-action 从 19 升级到 23 (#1989)
- chore: 版本升级至 0.4.3 (#1986)

## [0.4.3] - 2026-03-26

### 变更

- 统一 Kimi/Codex 技能命名并迁移旧版点分 Kimi 目录 (#1971)
- fix(ps1): 替换空条件运算符以兼容 PowerShell 5.1 (#1975)
- chore: 版本升级至 0.4.2 (#1973)

## [0.4.2] - 2026-03-25

### 变更

- feat: 在适用时自动为扩展注册 ai-skills (#1840)
- docs: 添加斜杠命令验证的手动测试指南 (#1955)
- 将 AIDE、Extensify 和 Presetify 添加到社区扩展 (#1961)
- docs: 在主 README 中添加社区预设部分 (#1960)
- docs: 将社区扩展表格移至主 README 以提高可发现性 (#1959)
- docs(readme): 合并 Community Friends 部分并修复目录锚点 (#1958)
- fix(commands): 在 analyze 和 clarify 中将 NFR 引用重命名为成功标准 (#1935)
- 在 README 中添加 Community Friends 部分 (#1956)
- docs: 添加包含 Spec Kit Assistant VS Code 扩展的 Community Friends 部分 (#1944)

## [0.4.1] - 2026-03-24

### 变更

- 添加 checkpoint 扩展 (#1947)
- fix(scripts): 优先使用 .specify 而非 git 进行仓库根目录检测 (#1933)
- docs: 在社区项目中添加 AIDE 扩展演示 (#1943)
- fix(templates): 在规格模板中添加缺失的假设部分 (#1939)

## [0.4.0] - 2026-03-23

### 变更

- fix(cli): 在 YAML I/O 中添加 allow_unicode=True 和 encoding="utf-8" (#1936)
- fix(codex): 原生技能回退刷新 + 旧版提示抑制 (#1930)
- feat(cli): 在 wheel 中嵌入核心包以支持离线/气隙部署 (#1803)
- ci: 将 stale 工作流的 operations-per-run 增加到 250 (#1922)
- docs: 更新发布指南，添加类别和效果列 (#1913)
- fix: 对齐原生技能 frontmatter 与 install_ai_skills (#1920)
- feat: 为 `specify init` 添加基于时间戳的分支命名选项 (#1911)
- docs: 添加社区扩展的扩展对比指南 (#1897)
- docs: 更新 SUPPORT.md，修复 issue 模板，添加预设提交模板 (#1910)
- 添加 Junie 支持 (#1831)
- feat: 将 Codex/agy init 迁移到原生技能工作流 (#1906)

## [0.3.2] - 2026-03-19

### 变更

- 将 conduct 扩展添加到社区目录 (#1908)
- feat(extensions): 将 verify-tasks 扩展添加到社区目录 (#1871)
- feat(presets): 添加启用/禁用切换和更新语义 (#1891)
- feat: 添加 iFlow CLI 支持 (#1875)
- feat(commands): 将 before/after 钩子事件接入 specify 和 plan 模板 (#1886)
- docs(catalog): 将 speckit-utils 添加到社区目录 (#1896)
- docs: 在 README 中添加扩展和预设部分 (#1898)
- chore: 更新 DocGuard 扩展至 v0.9.11 (#1899)
- 更新 cognitive-squad 目录条目 — 三元模型、完整生命周期 (#1884)
- feat: 注册 spec-kit-iterate 扩展 (#1887)
- fix(scripts): 为 PowerShell create-new-feature 参数添加显式位置绑定 (#1885)
- fix(scripts): 将残余 JSON 控制字符编码为 \uXXXX 而非直接剥离 (#1872)
- chore: 更新 DocGuard 扩展至 v0.9.10 (#1890)
- Feature/spec kit add pi coding agent pullrequest (#1853)
- feat: 注册 spec-kit-learn 扩展 (#1883)

## [0.3.1] - 2026-03-17

### 变更

- docs: 在 README 中添加全新 Spring Boot 海盗风格预设演示 (#1878)
- fix(ai-skills): 从技能中排除非 speckit 的 copilot 代理 Markdown (#1867)
- feat: 添加 Trae IDE 作为新代理支持 (#1817)
- feat(cli): settings.json 的礼貌深度合并并支持 JSONC (#1874)
- feat(extensions,presets): 添加基于优先级的解析排序 (#1855)
- fix(scripts): 在 create-new-feature.sh 中抑制 git fetch 的 stdout 输出 (#1876)
- fix(scripts): 加固 bash 脚本 — 转义、兼容性和错误处理 (#1869)
- 将 cognitive-squad 添加到社区扩展目录 (#1870)
- docs: 在社区演练中添加 Go / React 棕地项目演练 (#1868)
- chore: 更新 DocGuard 扩展至 v0.9.8 (#1859)
- 功能：添加 specify status 命令 (#1837)
- fix(extensions): 在列表输出中显示扩展 ID (#1843)
- feat(extensions): 将 Archive 和 Reconcile 扩展添加到社区目录 (#1844)
- feat: 将 DocGuard CDD 执行扩展添加到社区目录 (#1838)

## [0.3.0] - 2026-03-13

### 变更

- feat(presets): 可插拔预设系统，包含目录、解析器和技能传播 (#1787)
- fix: 匹配带或不带粗体标记的 'Last updated' 时间戳 (#1836)
- 添加 specify doctor 命令用于项目健康诊断 (#1828)
- fix: 加固 bash 脚本以防止 shell 注入并提高健壮性 (#1809)
- fix: 清理命令模板（specify、analyze）(#1810)
- fix: 将 Qwen Code CLI 从 TOML 格式迁移到 Markdown 格式 (#1589) (#1730)
- fix(cli): 弃用 agy 的显式命令支持 (#1798) (#1808)
- 添加 /selftest.extension 核心扩展用于测试其他扩展 (#1758)
- feat(extensions): RFC 对齐的目录集成的生活质量改进 (#1776)
- 将 Java 棕地项目演练添加到社区演练 (#1820)

## [0.2.1] - 2026-03-11

### 变更

- 添加 2026 年 2 月简报 (#1812)
- feat: 添加 Kimi Code CLI 代理支持 (#1790)
- docs: 修复快速入门指南中的断开链接 (#1759) (#1797)
- docs: 添加目录 CLI 帮助文档 (#1793) (#1794)
- fix: 使用静默 checkout 以避免 git checkout 时的异常 (#1792)
- feat(extensions): 支持 .extensionignore 以在安装期间排除文件 (#1781)
- feat: 为扩展命令注册添加 Codex 支持 (#1767)

## [0.2.0] - 2026-03-09

### 变更

- fix: 同步代理列表注释与实际支持的代理 (#1785)
- feat(extensions): 同时支持多个活动目录 (#1720)
- Pavel/add tabnine cli support (#1503)
- 将 Understanding 扩展添加到社区目录 (#1778)
- 将 ralph 扩展添加到社区目录 (#1780)
- 更新 README 中的项目初始化说明 (#1772)
- feat: 将 review 扩展添加到社区目录 (#1775)
- 将 fleet 扩展添加到社区目录 (#1771)
- 将 Mistral vibe 支持集成到 speckit 中 (#1725)
- fix: 移除 specify.md 中的重复选项 (#1765)
- fix: 使用全局分支编号代替按短名称检测 (#1757)
- 在 README 中添加社区演练部分 (#1766)
- feat(extensions): 将 Jira 集成添加到社区目录 (#1764)
- 将 Azure DevOps 集成扩展添加到社区目录 (#1734)
- 修复文档：更新 Antigravity 链接并添加初始化示例 (#1748)
- fix: 将 after_tasks 和 after_implement 钩子事件接入命令模板 (#1702)
- 使 c 的忽略规则与 c++ 保持一致 (#1747)

## [0.1.13] - 2026-03-03

### 变更

- feat: 添加 kiro-cli 和 AGENT_CONFIG 一致性覆盖 (#1690)
- feat: 将 verify 扩展添加到社区目录 (#1726)
- 将 Retrospective 扩展添加到社区目录 README 表格 (#1741)
- fix(scripts): 添加空描述验证和分支 checkout 错误处理 (#1559)
- fix: 修正 Copilot 扩展命令注册 (#1724)
- fix(implement): 从 C 忽略模式中移除 Makefile (#1558)
- 将 sync 扩展添加到社区目录 (#1728)
- fix(checklist): 澄清追加与创建的文件处理行为 (#1556)
- fix(clarify): 修正冲突的问题限制从 10 改为 5 (#1557)

## [0.1.12] - 2026-03-02

### 变更

- fix: 使用 RELEASE_PAT 以便标签推送触发发布工作流 (#1736)

## [0.1.11] - 2026-03-02

### 变更

- fix: release-trigger 使用 release 分支 + PR 代替直接推送到 main (#1733)
- fix: 拆分发布流程以同步 pyproject.toml 版本与 git 标签 (#1732)

## [0.1.10] - 2026-02-27

### 变更

- fix: 为 Cursor .mdc 文件前置 YAML frontmatter (#1699)

## [0.1.9] - 2026-02-28

### 变更

- chore(deps): 将 astral-sh/setup-uv 从 6 升级到 7 (#1709)

## [0.1.8] - 2026-02-28

### 变更

- chore(deps): 将 actions/setup-python 从 5 升级到 6 (#1710)

## [0.1.7] - 2026-02-27

### 变更

- chore: 更新过时的 GitHub Actions 版本 (#1706)
- docs: 记录扩展的双目录系统 (#1689)
- 修复文档中的 version 命令 (#1685)
- 将 Cleanup 扩展添加到 README (#1678)
- 将 retrospective 扩展添加到社区目录 (#1681)

## [0.1.6] - 2026-02-23

### 变更

- 将 Cleanup 扩展添加到目录 (#1617)
- 修复 CLI 中的参数排序问题 (#1669)
- 将 V-Model Extension Pack 更新至 v0.4.0 (#1665)
- docs: 修复文档中缺失的步骤 (#1496)
- 将 V-Model Extension Pack 更新至 v0.3.0 (#1661)

## [0.1.5] - 2026-02-21

### 变更

- 修复 #1658：添加 commands_subdir 字段以支持非标准代理目录结构 (#1660)
- feat: 添加 GitHub issue 模板 (#1655)
- 将 V-Model Extension Pack 更新至 v0.2.0（社区目录）(#1656)
- 将 V-Model Extension Pack 添加到目录 (#1640)
- refactor: 从模板中移除 OpenAPI/GraphQL 偏向 (#1652)

## [0.1.4] - 2026-02-20

### 变更

- fix: 将 Qoder AGENT_CONFIG 键从 'qoder' 重命名为 'qodercli' 以匹配实际 CLI 可执行文件 (#1651)

## [0.1.3] - 2026-02-20

### 变更

- 添加通用代理支持，可自定义命令目录 (#1639)

## [0.1.2] - 2026-02-20

### 变更

- fix: 固定 click>=8.1 以防止 Python 3.14/Homebrew 环境隔离崩溃 (#1648)

## [0.0.102] - 2026-02-20

### 变更

- fix: 在发布工作流触发器中包含 'src/**' 路径 (#1646)

## [0.0.101] - 2026-02-19

### 变更

- chore(deps): 将 github/codeql-action 从 3 升级到 4 (#1635)

## [0.0.100] - 2026-02-19

### 变更

- 在 CI 中添加 pytest 和 Python 代码检查（ruff）(#1637)
- feat: 添加 pull request 模板以提供更好的贡献指南 (#1634)

## [0.0.99] - 2026-02-19

### 变更

- Feat/ai skills (#1632)

## [0.0.98] - 2026-02-19

### 变更

- chore(deps): 将 actions/stale 从 9 升级到 10 (#1623)
- feat: 添加 dependabot 配置用于 pip 和 GitHub Actions 更新 (#1622)

## [0.0.97] - 2026-02-18

### 变更

- 从 README.md 中移除维护者部分 (#1618)

## [0.0.96] - 2026-02-17

### 变更

- fix: plan-template.md 中的拼写错误 (#1446)

## [0.0.95] - 2026-02-12

### 变更

- Feat: 添加新代理：Google Anti Gravity (#1220)

## [0.0.94] - 2026-02-11

### 变更

- 为 180 天不活跃的 issues 和 PR 添加 stale 工作流 (#1594)

## [0.0.93] - 2026-02-10

### 变更

- 添加模块化扩展系统 (#1551)

## [0.0.92] - 2026-02-10

### 变更

- 修复 #1586 - .specify.specify 路径错误 (#1588)

## [0.0.91] - 2026-02-09

### 变更

- fix: 在重新初始化期间保留 constitution.md (#1541) (#1553)
- fix: 解决文档中的 markdownlint 错误 (#1571)

## [0.0.90] - 2025-12-04

### 变更

- 更新 Markdown 格式
- 更新 Markdown 格式
- docs: 在入门指南中添加现有项目初始化说明

## [0.0.89] - 2025-12-02

### 变更

- 更新 scripts/bash/create-new-feature.sh
- fix(scripts): 防止功能编号解析中的八进制解释
- fix: 从分支编号函数中移除未使用的 short_name 参数
- 更新 scripts/powershell/create-new-feature.ps1
- 更新 scripts/bash/create-new-feature.sh
- fix: 使用全局最大值进行分支编号以防止冲突

## [0.0.88] - 2025-12-01

### 变更

- 修复错误的 task-template 文件路径

## [0.0.87] - 2025-12-01

### 变更

- 限制宽度和高度为 200px 以匹配小图标
- docs: 将 readme 图标切换为 logo_large.webp
- fix:merge
- fix
- fix
- feat:qoder agent
- docs: 使用提示框和示例增强快速入门指南
- docs: 在快速入门指南中添加 constitution 步骤（修复 #906）
- 更新 README.md 中支持的 AI 代理
- cancel:test
- test
- fix:literal bug
- fix:test
- test
- fix:qoder url
- fix:download owner
- test
- feat:支持 Qoder CLI

## [0.0.86] - 2025-11-26

### 变更

- feat: 在新的 update-agent-context.ps1 中添加 bob + 注释一致性
- feat: 添加对 IBM Bob IDE 的支持

## [0.0.85] - 2025-11-14

### 变更

- 在获取 SCRIPT_DIR 时取消设置 CDPATH

## [0.0.84] - 2025-11-14

### 变更

- docs: 修复断开的链接并改进代理参考
- docs: 重组升级文档结构
- docs: 从升级指南中移除相关文档部分
- fix: 移除指向现有项目指南的断开链接
- docs: 添加 Spec Kit 的全面升级指南
- 重构 implement.md 中的 ESLint 配置检查以解决弃用问题

## [0.0.83] - 2025-11-14

### 变更

- feat: 添加 OVHcloud SHAI AI 代理

## [0.0.82] - 2025-11-14

### 变更

- fix: 使用 AGENTS 或 SCRIPTS 子集创建发布包的逻辑错误

## [0.0.81] - 2025-11-14

### 变更

- 修复 tasktoissues.md 以使用 'github/github-mcp-server/issue_write' 工具

## [0.0.80] - 2025-11-14

### 变更

- 重构功能脚本逻辑并更新代理上下文脚本
- 更新 templates/commands/taskstoissues.md
- 更新 CHANGELOG.md
- 更新代理配置
- 更新 scripts/powershell/create-new-feature.ps1
- 更新 src/specify_cli/__init__.py
- 创建 create-release-packages.ps1
- 脚本变更
- 更新 taskstoissues.md
- 创建 taskstoissues.md
- 更新 src/specify_cli/__init__.py
- 更新 CONTRIBUTING.md
- 修复代码扫描警告第 3 号的潜在问题：工作流不包含权限
- 更新 src/specify_cli/__init__.py
- 更新 CHANGELOG.md
- 修复 #970
- 修复 #975
- 支持 version 命令
- 排除生成的发布
- Lint 修复
- 提示更新
- 使用提示进行交接
- 聊天模式重新流行
- 切换到正式提示
- 更新提示
- 使用提示更新
- 测试交接
- 使用 VS Code 交接

## [0.0.79] - 2025-10-23

### 变更

- docs: 恢复 specify 命令中关于 JSON 输出的重要说明
- fix: 改进分支编号检测以检查所有来源
- feat: 检查远程分支以防止重复分支编号

## [0.0.78] - 2025-10-21

### 变更

- 更新 CONTRIBUTING.md
- docs: 添加本地测试模板和命令变更的步骤
- 更新 specify 以使 create-new-feature.sh 的 "short-name" 参数位于正确位置

## [0.0.77] - 2025-10-21

### 变更

- fix: 在 `GitHub Release` 的正文中包含最新的更新日志

## [0.0.76] - 2025-10-21

### 变更

- 修复 update-agent-context.sh 以处理没有 Active Technologies/Recent Changes 部分的文件

## [0.0.75] - 2025-10-21

### 变更

- 修复缩进。
- 为 Amp 代理 CLI 脚本添加正确的 `install_url`。
- 添加对 Amp 代码代理的支持。

## [0.0.74] - 2025-10-21

### 变更

- feat(ci): 添加 markdownlint-cli2 以实现一致的 Markdown 格式

## [0.0.73] - 2025-10-21

### 变更

- 还原 vscode 自动移除多余空格
- fix: 修正 implement.md 中的命令引用
- 修复 copilot 建议相关的问题
- fix: 修正 speckit.analyze.md 中的命令引用
- 支持更多语言/DevOps 的常见技术模式
- chore: 在 `devcontainer` 中用 `node/npm` 替换 `bun`（因为许多基于 CLI 的代理实际上需要 `node` 运行时）
- chore: 在 devcontainer 配置中添加 Claude Code 扩展
- chore: 在 `devcontainer` 中添加 `codebuddy` CLI 的安装
- chore: 修复 vscode settings 中的 powershell 脚本路径
- fix: 修正 `run_command` 退出行为并改进安装说明（针对 `Amazon Q`）在 `post-create.sh` 中 + 修复 `CONTRIBUTING.md` 中的拼写错误
- chore: 将 `specify` 的 github copilot 聊天设置添加到 `devcontainer`
- chore: 添加 `devcontainer` 支持以简化开发者工作站设置

## [0.0.72] - 2025-10-18

### 变更

- fix: 修正 create-new-feature.sh 脚本中的参数解析

## [0.0.71] - 2025-10-18

### 变更

- fix: 跳过基于 IDE 的代理在 check 命令中的 CLI 检查
- 修改循环条件以包含最后一个参数

## [0.0.70] - 2025-10-18

### 变更

- fix: 修复损坏的媒体文件
- 更新 README.md
- 函数参数缺少类型提示。建议添加类型注解以提高代码清晰度和 IDE 支持。
- - **VS Code 设置的智能 JSON 合并**：`.vscode/settings.json` 现在在 `specify init --here` 或 `specify init .` 期间会智能合并而非被覆盖   - 保留现有设置   - 添加新的 Spec Kit 设置   - 嵌套对象递归合并   - 防止意外丢失自定义 VS Code 工作区配置
- Fix: 修正代理上下文文件中的命令格式错误，重新修复 #895

## [0.0.69] - 2025-10-15

### 变更

- 更新 scripts/bash/create-new-feature.sh
- 更新 create-new-feature.sh
- 更新文件
- 更新文件
- 创建 .gitattributes
- 更新措辞
- 更新参数逻辑
- 更新脚本逻辑

## [0.0.68] - 2025-10-15

### 变更

- 按 copilot 建议格式化内容
- Ruby、PHP、Rust、Kotlin、C、C++

## [0.0.67] - 2025-10-15

### 变更

- 使用编号前缀查找正确的规格

## [0.0.66] - 2025-10-15

### 变更

- 将 CodeBuddy 代理名称更新为 'CodeBuddy CLI'
- 在更新脚本中将 CodeBuddy 重命名为 CodeBuddy CLI
- 更新安装指南中的 AI 编码代理引用
- 在 AGENTS.md 中将 CodeBuddy 重命名为 CodeBuddy CLI
- 更新 README.md
- 更新 README.md 中的 CodeBuddy 链接
- 更新 codebuddyCli

## [0.0.65] - 2025-10-15

### 变更

- Fix: 修正代理上下文文件中的命令格式错误
- docs: 修复标题大小写以保持一致性
- 更新 README.md

## [0.0.64] - 2025-10-14

### 变更

- 更新 tasks.md
- 更新 README.md

## [0.0.63] - 2025-10-14

### 变更

- fix: 更新代理上下文脚本中的 CODEBUDDY 文件路径
- docs(readme): 添加 /speckit.tasks 步骤并重新编号演练

## [0.0.62] - 2025-10-11

### 变更

- 代码审查中的更多待更新之处
- fix: 统一 Cursor 代理命名为 'cursor-agent'

## [0.0.61] - 2025-10-10

### 变更

- 更新 clarify.md
- 添加如何升级 specify 安装

## [0.0.60] - 2025-10-10

### 变更

- 更新 vscode-settings.json
- 更新说明和错误修复

## [0.0.59] - 2025-10-10

### 变更

- 更新 __init__.py
- 统一 Cursor 命名
- 更新 CHANGELOG.md
- Git 错误现在会高亮显示。
- 更新 __init__.py
- 重构代理配置
- 更新 src/specify_cli/__init__.py
- 更新 scripts/powershell/update-agent-context.ps1
- 更新 AGENTS.md
- 更新 templates/commands/implement.md
- 更新 templates/commands/implement.md
- 更新 CHANGELOG.md
- 更新更新日志
- 更新 plan.md
- 在 /speckit.implement 命令中添加忽略文件验证步骤
- 转义 TOML 输出中的反斜杠
- 将 CodeBuddy 更新为国际站点
- feat: 支持 codebuddy ai
- feat: 支持 codebuddy ai

## [0.0.58] - 2025-10-08

### 变更

- 在命令模板中添加转义指南
- 更新 README.md
- 更新 README.md

## [0.0.57] - 2025-10-06

### 变更

- 更新 CHANGELOG.md
- 更新命令参考
- 为 Copilot 打包 VS Code 设置
- 更新 tasks-template.md
- 更新 templates/tasks-template.md
- 清理
- 更新 CLI 变更
- 更新模板和文档
- 更新 checklist.md
- 更新模板
- 清理冗余
- 更新 checklist.md
- Codex CLI 现已完全支持
- 更新 specify.md
- 提示更新
- 更新提示前缀
- 更新 .github/workflows/scripts/create-release-packages.sh
- 命令一致性更新
- 更新命令。
- 更新日志
- 模板清理和重组
- 移除 Codex 命名参数限制警告
- 从 README.md 中移除 Codex 命名参数限制

## [0.0.56] - 2025-10-02

### 变更

- docs(readme): 链接 Amazon Q 斜杠命令限制 issue
- docs: 澄清 Amazon Q 限制并更新 init 文档字符串
- feat(agent): 添加 Amazon Q Developer CLI 集成

## [0.0.55] - 2025-09-30

### 变更

- 更新文档中贡献指南和支持指南的 URL
- fix: 在 update-agent-context.ps1 中为文件读写操作添加 UTF-8 编码
- 更新 __init__.py
- 更新 src/specify_cli/__init__.py
- docs: 修复生成文件的路径（移至 `.specify/` 文件夹下）
- 更新 src/specify_cli/__init__.py
- feat: 支持 'specify init .' 进行当前目录初始化
- feat: 添加 emacs 风格的上/下键

## [0.0.54] - 2025-09-25

### 变更

- 更新 CONTRIBUTING.md
- 改进 `plan-template.md`，优化项目类型检测、澄清结构决策流程并增强研究任务指导。
- 更新 __init__.py

## [0.0.53] - 2025-09-24

### 变更

- 更新规格文件创建的模板路径
- 更新规格文件创建的模板路径
- docs: 从 README 中移除 constitution_update_checklist

## [0.0.52] - 2025-09-22

### 变更

- 更新 analyze.md
- 更新 templates/commands/analyze.md
- 更新 templates/commands/clarify.md
- 更新 templates/commands/plan.md
- 添加额外命令更新
- 添加 --force 标志更新
- feat: 在 README 中添加 uv tool install 说明

## [0.0.51] - 2025-09-21

### 变更

- 添加 Roo Code 支持更新

## [0.0.50] - 2025-09-21

### 变更

- 更新 generate-release-notes.sh
- 更新错误消息
- Auggie 文件夹修复

## [0.0.49] - 2025-09-21

### 变更

- 更新 scripts/powershell/update-agent-context.ps1
- 更新 templates/commands/implement.md
- 清理 check 命令
- 添加 Auggie 支持
- 更新 AGENTS.md
- 添加 Kilo Code 支持更新
- 更新 README.md
- 更新 templates/commands/constitution.md
- 更新 templates/commands/implement.md
- 更新 templates/commands/plan.md
- 更新 templates/commands/specify.md
- 更新 templates/commands/tasks.md
- 更新 README.md
- 停止将警告拆分为多行
- 基于 #419 更新模板
- docs: 更新 README，在 check 命令中添加 codex

## [0.0.48] - 2025-09-21

### 变更

- 更新 scripts/powershell/check-prerequisites.ps1
- 更新 CHANGELOG.md
- 更新 CHANGELOG.md
- 更新更新日志
- 更新 scripts/bash/update-agent-context.sh
- 修复脚本配置
- 更新 scripts/bash/common.sh
- 更新 scripts/powershell/update-agent-context.ps1
- 更新 scripts/powershell/update-agent-context.ps1
- 澄清说明
- 更新提示
- 更新 update-agent-context.ps1
- 更新 CONTRIBUTING.md
- 更新 CONTRIBUTING.md
- 更新 CONTRIBUTING.md
- 更新 CONTRIBUTING.md
- 更新 CONTRIBUTING.md
- 更新贡献指南。
- 根目录检测逻辑
- 更新 templates/plan-template.md
- 更新 scripts/bash/update-agent-context.sh
- 更新 scripts/powershell/create-new-feature.ps1
- 简化
- 脚本和模板调整
- 更新配置
- 更新 scripts/powershell/check-prerequisites.ps1
- 更新 scripts/bash/check-prerequisites.sh
- 修复脚本路径
- 脚本清理
- 更新 scripts/bash/check-prerequisites.sh
- 更新 scripts/powershell/check-prerequisites.ps1
- 更新 GitHub Action 的脚本委派
- 清理生成包的设置
- 使用正确的行尾符
- 合并脚本

## [0.0.47] - 2025-09-20

### 变更

- 更新代理上下文文件

## [0.0.46] - 2025-09-20

### 变更

- 更新 update-agent-context.ps1
- 更新包发布
- 更新配置
- 更新 __init__.py
- 更新 __init__.py
- 移除初始化脚本中的 Codex 特定逻辑
- 更新版本号
- 更新 __init__.py
- 增强 Codex 支持：自动同步提示文件、允许在没有 git 的情况下生成规格，并记录更清晰的 /specify 用法。
- 一致性调整
- 一致的步骤着色
- 更新 __init__.py
- 更新 __init__.py
- 快速 UI 调整
- 更新包发布
- 将工作区命令播种限制为 Codex init 并相应更新 Codex 文档。
- 澄清 Codex 特定的 README 说明及其不同工作流的原因。
- 升级至 0.0.7 并记录 Codex 支持
- 将 Codex 命令模板标准化为基于脚本的模式并自动升级生成的命令。
- 修复 __init__.py 中剩余的合并冲突标记
- 添加 Codex CLI 支持，包含 AGENTS.md 和命令引导

## [0.0.45] - 2025-09-19

### 变更

- 添加 Windsurf 支持更新
- 通过 cli --github-token 将令牌暴露为参数
- 当设置了 GITHUB_TOKEN/GH_TOKEN 时添加 github 认证头

## [0.0.44] - 2025-09-18

### 变更

- 更新 specify.md
- 更新 __init__.py

## [0.0.43] - 2025-09-18

### 变更

- 添加 /implement 支持更新

## [0.0.42] - 2025-09-18

### 变更

- 更新 constitution.md

## [0.0.41] - 2025-09-18

### 变更

- 更新 constitution.md

## [0.0.40] - 2025-09-18

### 变更

- 更新 constitution 命令

## [0.0.39] - 2025-09-18

### 变更

- 清理
- fix: qwen 的命令格式

## [0.0.38] - 2025-09-18

### 变更

- 修复 update-agent-context.sh 中的模板路径
- docs: 修复 markdown 文件中的语法错误

## [0.0.37] - 2025-09-17

### 变更

- fix: 在发布工作流和代理脚本中添加缺失的 Qwen 支持

## [0.0.36] - 2025-09-17

### 变更

- feat: 添加 opencode ai 代理
- 修复 --no-git 参数解析。

## [0.0.35] - 2025-09-17

### 变更

- chore(release): 版本升级至 0.0.5 并更新更新日志
- chore: 处理审查反馈 - 移除注释并修复编号
- feat: 将 Qwen Code 支持添加到 Spec Kit

## [0.0.34] - 2025-09-15

### 变更

- 更新模板。

## [0.0.33] - 2025-09-15

### 变更

- 更新脚本

## [0.0.32] - 2025-09-15

### 变更

- 更新模板路径

## [0.0.31] - 2025-09-15

### 变更

- 更新 Cursor 规则和脚本路径
- 更新 Specify 定义
- 更新 README.md
- 添加视频头
- fix(docs): 移除多余的空白

## [0.0.30] - 2025-09-12

### 变更

- 更新 update-agent-context.ps1

## [0.0.29] - 2025-09-12

### 变更

- 更新 create-release-packages.sh
- 添加 check 变更更新

## [0.0.28] - 2025-09-12

### 变更

- 更新措辞
- 更新 release.yml

## [0.0.27] - 2025-09-12

### 变更

- 支持 Cursor

## [0.0.26] - 2025-09-12

### 变更

- 更合理的脚本方式

## [0.0.25] - 2025-09-12

### 变更

- 更新打包

## [0.0.24] - 2025-09-12

### 变更

- 修复打包逻辑

## [0.0.23] - 2025-09-12

### 变更

- 更新配置
- 更新 __init__.py
- 使用平台特定约束进行重构
- 更新 README.md
- 更新 CLI 参考
- 更新 __init__.py
- refactor: 将 Claude 本地路径提取为常量以提高可维护性
- fix: 支持通过 migrate-installer 安装的 Claude CLI

## [0.0.22] - 2025-09-11

### 变更

- 更新 release.yml
- 更新 create-release-packages.sh
- 更新 create-release-packages.sh
- 更新发布文件

## [0.0.21] - 2025-09-11

### 变更

- 合并脚本创建
- 更新 Copilot 提示的创建方式
- 更新 local-development.md
- 本地开发指南和脚本更新
- 更新 CONTRIBUTING.md
- 增强 HTTP 客户端初始化，添加可选 SSL 验证并将版本升级至 0.0.3
- 完善 Gemini CLI 命令说明
- 重构 HTTP 客户端以使用 truststore 获取 SSL 上下文
- docs: 更新 Commands 部分重命名以匹配实现
- docs: 修复 README.md 中的格式问题以保持一致性
- 更新文档和发布

## [0.0.20] - 2025-09-08

### 变更

- 更新 docs/quickstart.md
- 文档设置

## [0.0.19] - 2025-09-08

### 变更

- 更新 README.md

## [0.0.18] - 2025-09-08

### 变更

- 更新 README.md

## [0.0.17] - 2025-09-08

### 变更

- 移除 tasks.md 模板中的尾随空白

## [0.0.16] - 2025-09-07

### 变更

- 修复发布工作流以适应仓库规则

## [0.0.15] - 2025-09-07

### 变更

- 使用 `/usr/bin/env bash` 代替 `/bin/bash` 作为 shebang

## [0.0.14] - 2025-09-04

### 变更

- fix: 修正 spec-driven.md 中的拼写错误

## [0.0.13] - 2025-09-04

### 变更

- 修复使用说明中的格式

## [0.0.12] - 2025-09-04

### 变更

- 修复 plan 命令文档中的模板路径

## [0.0.11] - 2025-09-04

### 变更

- fix: 示例中的目录结构错误

## [0.0.10] - 2025-09-04

### 变更

- 修复第一条中的小拼写错误

## [0.0.9] - 2025-09-03

### 变更

- 将 CLI 命令从 '/spec' 更新为 '/specify'

## [0.0.8] - 2025-09-02

### 变更

- 为脚本添加可执行权限，以便编码代理启动时可以执行它们

## [0.0.7] - 2025-09-02

### 变更

- doco(spec-driven): 修复文档中的小拼写错误

## [0.0.6] - 2025-08-25

### 变更

- 更新 README.md

## [0.0.5] - 2025-08-25

### 变更

- 更新 .github/workflows/release.yml
- 修复发布工作流以适应仓库规则

## [0.0.4] - 2025-08-25

### 变更

- 添加 John Lam 为贡献者并添加发布徽章

## [0.0.3] - 2025-08-22

### 变更

- 更新依赖要求

## [0.0.2] - 2025-08-22

### 变更

- 更新 README.md

## [0.0.1] - 2025-08-22

### 变更

- 更新 release.yml
