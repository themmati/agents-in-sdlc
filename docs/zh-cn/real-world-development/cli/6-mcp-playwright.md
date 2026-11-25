---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并通过浏览器探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

前面已经提到，编写代码不只是写代码。我们还要处理数据、外部服务，并允许 Copilot 使用其他自动化工具。MCP 服务器可以让 Copilot 使用 CLI 内置功能之外的工具和服务。

本课将：

- 了解模型上下文协议 (MCP) 及 Copilot CLI 如何使用它。
- 如果尚未配置，则添加 Playwright MCP 服务器。
- 请智能体驱动浏览器，探索筛选功能。

## 场景

单元测试和端到端测试都很重要，但验证 UI 更新还需要实际与界面交互。希望 Copilot 能像用户一样使用正在开发的网站，以自动化方式验证更改，并提高对功能正确性的信心。

## 什么是模型上下文协议 (MCP)？

[模型上下文协议 (MCP)][mcp-blog-post]让 AI 智能体能够与外部工具和服务通信。使用 MCP 后，AI 智能体可以实时访问外部工具和服务，从而获取最新信息并代表你执行操作。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当智能体与外部工具和服务之间的桥梁，每个服务器都会提供一组智能体可以访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化能力。

还有许多其他 MCP 服务器。GitHub 提供了一个 [MCP 注册表][mcp-registry]，帮助发现并为生态系统做出贡献。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前先检查源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请添加它，以便 Copilot 能像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已经配置了 Playwright MCP，可以跳过设置步骤并继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器的对话框：

   ```plaintext
   /mcp add
   ```

3. 将名称设置为 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 请 Copilot 使用 Playwright 探索功能

之前已手动确认功能行为符合预期，现在使用 Copilot 中的 Playwright MCP 完成同样的工作。

1. 使用以下提示，请 Copilot 通过 Playwright MCP 验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常无需明确告诉 Copilot 使用 MCP 服务器，它会自行判断。但既然知道应该使用哪个工具，也可以在提示中指出。这有助于获得更一致的结果，并减少一些令牌消耗。

2. 观察 Copilot 在浏览器中执行的各项操作，确认功能是否正常。
3. 阅读报告，确保行为符合预期。

Copilot 会启动服务器、使用 Playwright 操作网站、停止服务器，并提供报告。

## 总结和后续步骤

恭喜，你已经通过 Playwright MCP 在 Copilot CLI 中使用真实浏览器探索了功能。本课完成了以下事项：

- 了解模型上下文协议 (MCP) 及 Copilot CLI 如何使用它。
- 如果 Playwright MCP 尚未配置，则添加服务器。
- 请智能体驱动浏览器，探索筛选功能。

接下来，[创建 QA 自定义智能体][next-lesson]，将技能与浏览器工具结合起来。

## 资源

- [MCP 到底是什么，为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
