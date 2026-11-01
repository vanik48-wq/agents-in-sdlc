---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

您已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。您：

- 准备了 Codespace，安装 Copilot CLI，探索项目并找到预置的筛选议题。
- 添加了星级评分，在转发的浏览器中检查结果，并手动合并了第一个拉取请求 (PR)。
- 从筛选议题开始，在 Plan 模式中确定方案，在 Autopilot 模式中实现，并在 Interactive 模式中审查。
- 使用自定义指令引导智能体，随后自定义现有的 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查。
- 添加了 Playwright Model Context Protocol (MCP) 服务器，并在真实浏览器中体验筛选功能。
- 创建并选择质量保证 (QA) 自定义智能体，评估需求、覆盖率、技能检查结果和浏览器证据。
- 审查完整的筛选更改，并授权 Agent Merge 合并筛选 PR。
- 探索用于上下文、模型、共享和可选云端委派的斜杠命令。

## 已交付的内容

本研讨会包含两个 PR 里程碑，分别基于更新后的 `main` 分支创建独立分支：

1. **星级评分**：在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流**：实现筛选、更新指令并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置文件并添加相关测试。

从规划筛选功能到创建 PR，您一直使用同一对话和分支。为简化研讨会，我们将这些工作合并到一个 PR 中。

## 不同类型的验证

您通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 让 Copilot 在浏览器中体验。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 会将这些结果与需求和测试覆盖率审查结合起来，再交由 PR 审查。

新增的测试应弥补实际缺口；如果 QA 检查后认为无需新增测试，也可能是合理结果。授权合并前，请审查代码和证据；更改后应重新生成受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，您更新了指令、自定义了技能、创建了 QA 配置文件并配置了 MCP 服务器。可以在其他对话中复用这些自定义项，并根据团队需求调整。指令设定规范，技能描述可重复执行的任务，自定义智能体定义专用角色，MCP 服务器连接外部工具。应审查实际更改和工具结果，而不只看智能体摘要。

根据任务选择合适的**模式和模型**。使用 **Plan** 在构建前确定方案，使用 **Interactive** 跟进专注的更改，并将 **Autopilot** 用于范围明确的任务。常规编辑可选择更快的模型；复杂工作则可选择能力更强的模型。

上下文和基础设施同样重要。清楚说明要构建的内容、原因和方式，会显著影响输出。

## 继续探索

核心工作流已完成。还可以了解一些 Copilot CLI 功能：

- `/review`：让代码审查智能体分析更改。
- `/rubber-duck`：讨论问题并获得另一种观点。
- `/fleet`：并行协调相互独立的子任务。
- `/worktree`：隔离单独任务。
- `/delegate`：将任务发送给 Copilot 云智能体。

## 后续步骤

提升工具使用能力的最佳方法就是持续使用它！可将其用于生产代码、个人项目，或用于实现一直想做却尚未着手的小应用。与团队分享经验并相互学习，同时也要继续探索文档。

如果想从终端扩展 Tailspin Toys，请继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若要比较其他环境，请参阅 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
