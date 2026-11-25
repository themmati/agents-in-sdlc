---
title: "第 9 课 - GitHub Copilot CLI 中的斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选委派给云智能体的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。它们能访问高级功能、后台信息和其他配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令，现在来了解其他实用命令。

本课将：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 探索可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行工作、工作树以及委派任务给云智能体的可选命令。

## 场景

CLI 核心工作流已经完成，现在探索其他功能：管理上下文、切换模型、共享会话，以及选择是否将任务委派给 [Copilot 云智能体][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理大型或复杂任务时，可能会接近模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话，你也可以使用斜杠命令查看或手动压缩上下文。

1. 返回 Codespace；如果 Copilot CLI 尚未打开，则从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 留意当前模型、令牌使用量，以及系统指令、工具、消息和可用空间之间的上下文分配。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> 上下文窗口接近满载时，Copilot CLI 会自动压缩上下文。使用 `/compact` 可以自行选择压缩时机。如果要切换到无关任务并开启新对话，可以使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，不同开发人员也有不同偏好。Copilot CLI 可以列出模型并让你选择。

1. 输入：

   ```plaintext
   /model
   ```

2. 浏览可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队成员共享经验有助于提升 AI 工具的使用效果。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布为 GitHub gist。

1. 输入 `/help`，查看当前版本中的 `/share` 选项。
2. 如需共享此会话，请选择合适的目标，例如 `/share file`，将其导出为本地 Markdown 文件。
3. 发送给他人或发布之前，先审查导出内容。会话导出可能包含提示词、回答和项目详细信息。

发布链接或 gist 是可选的。不要发布团队不打算共享的存储库或会话内容。

## 可选：扩展并行工作或委派任务

核心研讨会内容已完成。Copilot CLI 还为较大型的任务提供以下命令：

- `/fleet` 可以将独立子任务分配给多个子智能体并行执行。
- `/worktree` 可以为单独任务创建隔离的 Git 工作树。
- `/delegate` 可以将任务交给 Copilot 云智能体异步处理；它可能会创建拉取请求。

这些命令是可选的，因为它们可能创建额外工作树或远程工作。试用前先创建一个范围明确的新任务，并通过常规工作流审查结果。若要深入了解异步智能体工作，可以继续学习[云智能体研讨会][cloud-workshop]。

## 总结和后续步骤

使用 Copilot CLI 斜杠命令，可以进行配置、共享会话并查看后台状态。本课完成了以下事项：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 探索可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行工作、工作树以及委派任务给云智能体的可选命令。

Copilot CLI 还有更多命令和功能可供探索。现在通过[回顾所学内容][next-lesson]并了解后续学习资源，结束这段学习旅程。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
