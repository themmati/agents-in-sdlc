---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，将议题需求、quality-checks 技能和 Playwright MCP 结合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并通过 Playwright MCP 在浏览器中观察筛选体验。现在将这些能力整合到一个自定义智能体中，定义清晰的 QA 流程。

本课将：

- 了解自定义智能体如何与指令、技能和 MCP 工具协作。
- 创建并检查可重复使用的质量保证配置文件。
- 选择 QA 智能体，根据筛选议题审查其发现。

## 场景

创建拉取请求 (PR) 前，Tailspin Toys 希望一致地检查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程，并提供可重复使用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件说明智能体的用途、指令和可用工具。本练习中，你将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

前面使用的自定义方式各有不同用途：存储库指令说明团队标准；`quality-checks` 技能封装可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置文件则指导 Copilot 使用这些能力检查需求并报告发现。它们无需互相替代，也不需要另开一个对话。

## 创建 QA 配置文件

创建功能 PR 前，请 Copilot 创建一个可重复使用的 QA 配置文件。配置文件应明确 QA 执行的检查及其工作边界。

1. 返回 Codespace，确认筛选功能对话处于 Interactive 模式。
2. 请默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体前，先审查其配置文件，确认 Copilot 记录了预期的 QA 流程和边界。这样可避免在只需要验证功能时，智能体意外修改功能实现或执行范围过大的操作。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必需字段；`name` 为可选字段，但有助于显示清晰的名称。
3. 阅读配置文件中的指令，确认 QA 会从需求开始，遵循存储库指令，运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会先询问，而且不会提交更改或创建拉取请求。
5. 如果生成的配置文件缺少任何职责或边界，请默认智能体修改后再继续。

## 根据议题运行 QA

Copilot CLI 会在对话启动时加载项目智能体。创建配置文件后，恢复同一个筛选功能对话并选择 QA，这样它就能使用已有的议题和计划决策。

1. 使用以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能不会显示在可用智能体列表中，但仍然可以使用上面的命令启用。

2. 使用以下提示请求 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体会开始工作。
4. 完成后阅读它提供的报告。

## 总结和后续步骤

你已添加可重复使用的专业角色，并审查了它的工作。本课完成了以下事项：

- 了解自定义智能体如何与指令、技能和 MCP 工具协作。
- 创建并检查可重复使用的质量保证配置文件。
- 选择 QA 智能体，根据筛选议题审查其发现。

现在，功能实现、技能更新、QA 配置文件、测试和验证报告均已准备好接受审查。接下来，[汇总这些更改并创建功能 PR，然后使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
