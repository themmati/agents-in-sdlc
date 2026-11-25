---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你要求 Copilot 云智能体分别处理三个任务，改进用户体验并添加功能。云智能体虽然可以异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制中心，可以查看所有已创建开放拉取请求 (PR) 的智能体任务，查看它们执行的操作，还可以引导正在进行的会话。

本课将：

- 探索智能体页面，监控云智能体任务。
- 引导正在进行的会话，要求添加额外功能。

## 场景

将高对比度模式任务交给智能体后，团队意识到也可以顺便添加浅色模式。由于智能体已经在更新网站样式并添加模式切换功能，因此增加浅色模式十分合理。现在需要引导智能体，确保它同时实现高对比度模式和浅色模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态：

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主区域和左侧区域中的任务列表。应能看到你分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，其中可能包括：
   - 从存储库检出代码。
   - 为 Copilot 创建环境。
   - 设置 MCP 服务器。
   - 执行完成所分配任务的各种操作。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 的工作进度和采用的方法而变化。

4. 同时留意右侧显示的拉取请求窗格。可以在其中查看 PR 和更改的文件，以便进一步监控。

## 引导云智能体

了解当前运行的任务后，要求 Copilot 在实现高对比度模式的同时加入浅色模式切换功能。

1. 选择与添加高对比度模式相关的会话。确切标题可能因 Copilot 采用的名称和当前工作状态而异。

   ![控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到 Copilot 完成准备工作并开始处理任务。看到类似的消息时，就表示已开始执行任务。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Copilot 智能体页面中突出显示了用于引导活动会话的对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 等待 Copilot 完成工作

与之前一样，Copilot 会继续处理更新后的任务。收到新消息后，它会在完成当前步骤后将该请求纳入工作流程。

这同样需要几分钟。此时可以暂停一下，回顾之前学习和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面——监控和引导 GitHub Copilot 云智能体任务的中心位置。在此控制中心，可以跟踪所有正在进行和已完成的任务、审查已执行的工作，也可以重新引导正在运行的任务，调整范围或提供更多指导。

本课探索了以下内容：

- 使用控制中心和智能体页面监控云智能体任务。
- 引导正在进行的会话，要求添加额外功能。

Copilot 正在完成无障碍功能。接下来将[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
