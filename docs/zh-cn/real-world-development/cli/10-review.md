---
title: "第 10 课 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和后续学习资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你通过 GitHub Copilot CLI 完成了 Tailspin Toys 的连续工作流。本次研讨会中，你：

- 准备 Codespace、安装 Copilot CLI、探索项目并找到预填充的筛选议题。
- 添加星级评分，在转发的浏览器中检查结果，并手动合并第一个拉取请求 (PR)。
- 从筛选议题开始，在 Plan 模式中确定方案，在 Autopilot 模式中实现，并在 Interactive 模式中审查。
- 使用自定义指令引导智能体，自定义现有 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查。
- 添加 Playwright 模型上下文协议 (MCP) 服务器，并在真实浏览器中探索筛选功能。
- 创建并选择质量保证 (QA) 自定义智能体，以评估需求、覆盖范围、技能结果和浏览器证据。
- 审查完整的筛选功能更改，并授权 Agent Merge 处理筛选功能 PR。
- 探索用于管理上下文、模型、共享和可选委派给云智能体的斜杠命令。

## 交付的更改

本研讨会包含两个 PR 里程碑，每个 PR 都从更新后的 `main` 分支创建自己的分支：

1. **星级评分：**在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选功能和质量工作流：**实现筛选功能，更新指令并应用于功能实现，自定义 `quality-checks` 报告，创建 QA 配置文件并加入相关测试。

从规划筛选功能到创建 PR，你一直使用同一个对话和分支。为了简化研讨会流程，这些更改合并在一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 使用 Copilot 探索浏览器。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 智能体在 PR 创建前将这些结果与需求和测试覆盖审查结合起来。

新添加的测试应弥补真实缺口；QA 运行不需要添加新测试也可能是正确的。授权合并前，应审查代码和证据；更改代码后，重新检查受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了指令、自定义了技能、创建了 QA 配置文件并配置了 MCP 服务器。可以在多个对话中复用这些自定义项，并根据团队需求调整。指令定义标准，技能描述可重复执行的任务，自定义智能体定义专用角色，MCP 服务器连接外部工具。应审查实际更改和工具结果，而不只是智能体的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 在实现前分析方案，使用 **Interactive** 跟进范围明确的更改，使用 **Autopilot** 处理边界清晰的任务。日常修改可选择速度较快的模型；复杂工作则选择能力更强的模型。

上下文仍与基础设施同样重要。清晰说明要构建什么、为什么要构建，以及如何实现，会显著影响输出结果。

## 继续探索

核心工作流已完成。还可以了解一些 CLI 功能：

- 使用 `/review` 请代码审查智能体分析更改。
- 使用 `/rubber-duck` 讨论问题并获得另一种观点。
- 使用 `/fleet` 并行协调彼此独立的子任务。
- 使用 `/worktree` 隔离单独任务。
- 使用 `/delegate` 将任务交给 Copilot 云智能体。

## 后续步骤

熟练使用工具的最佳方式就是持续实践！将它用于生产代码、个人项目，或用于构建一个想了很久却一直没开始的小应用。与团队分享经验，也向团队成员学习。和往常一样，继续探索文档。

如果想在终端中扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他工作环境，可以探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

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
