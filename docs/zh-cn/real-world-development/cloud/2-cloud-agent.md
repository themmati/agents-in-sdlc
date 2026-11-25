---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都面临技术债务问题，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期积压的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。完成后，它会创建草稿 PR，供开发人员审查。这样可以将繁琐任务交给智能体，加快开发进度，让开发人员专注于更重要的工作。

本练习将探索如何使用 Copilot 云智能体：

- 自定义代码生成环境。
- 确保操作安全。
- 了解明确界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。负责初始版本网站的承包商没有留下完善的文档，事实上，代码完全缺少文档注释。团队首先希望为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现功能。他们希望在每款游戏的详情页显示相关游戏，也就是同类别的其他游戏。目前不需要完善的实现，只需要能用于 UX 验收测试的版本。此需求目前受阻，不过还有其他优先级更高的议题。

这两项都是容易被推迟的工作，适合交给 Copilot 云智能体异步处理。开发人员可以继续处理其他任务，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以在后台执行任务，方式与开发人员相似。你可以通过多种方式向它分配工作，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求来跟踪进度、准备环境并开始处理任务。你可以在会话进行期间或结束后查看它的工作。解决方案准备好供审查时，Copilot 会在拉取请求中提及你。

## 明确界定指令范围的重要性

GitHub Copilot 并非魔法工具，不能替代思考。即使是看似简单的操作，也可能包含复杂细节，因此[需要认真界定交给 Copilot 云智能体的任务范围][cloud-agent-best-practices]。应将它视为 AI 结对编程助手：分阶段工作、学习、试验并不断调整。采用生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义指令

前面的练习介绍了自定义指令及其对 Copilot 的引导作用。如果已经完成自定义指令练习，就看过 `.github/copilot-instructions.md` 的实际效果；如果还没看过，现在可以快速浏览。向 Copilot 云智能体分配工作前，先只读查看此存储库附带的指令文件，稍后便能观察它们的影响。

在存储库的 GitHub 网页界面中打开以下文件；如果已启动 codespace，也可以在那里查看：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：查看 `applyTo` frontmatter，它会将这些指令应用于 `**/*.test.ts` 文件。

下一部分将 `Code lacks documentation` 议题分配给云智能体时，留意它生成的 PR 是否遵循这些指令，包括 TSDoc 文档注释、TypeScript 规范和文件注释标头。之后的练习会在审查 PR 时再次说明。

## 为 Copilot 云智能体准备开发环境

编写代码通常需要特定环境和一些设置脚本，以确保项目状态良好。分配任务给 Copilot 时也一样，因为它会以类似软件工程师的方式工作。

云智能体使用 [GitHub Actions][github-actions]提供工作环境。你可以通过创建专门的设置工作流来自定义此环境，并将其配置在 `.github/workflows/copilot-setup-steps.yml` 文件中。工作流会在智能体开始工作前运行，使其能够使用所需的开发工具和依赖项。此练习已预先配置工作流，以便顺利完成课程。工作流会确保 Copilot 能够使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及为单体 Astro 应用迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限限制为步骤所需的最低级别。Copilot 会获得自己的令牌来执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果设置步骤不克隆存储库，步骤完成后 Copilot 会自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在是 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流文件类似，但有几点需要注意：

- 它只包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 PR 之前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器使你能够从存储库的 **Actions** 选项卡手动运行工作流。这有助于验证工作流能否成功运行，而不必等待 Copilot 启动它。

## 添加文档

大家都知道文档很重要，但多数项目的文档不是过时，就是根本不存在。这类技术债务常常得不到处理，降低了工作效率，也增加了维护代码库或引入新开发人员的难度。Copilot 擅长生成文档，因此很适合将这类议题交给云智能体。它会在后台生成所需文档，之后的练习中再回来审查成果。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新创建的拉取请求 (PR)，其标题类似 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才能在 PR 中显示待办事项列表。在此期间，它会准备环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看 Copilot 即将完成的任务列表。
14. 向下滚动拉取请求时间线，应能看到 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并确定合适方案，以及编写代码。

这可能需要几分钟。Copilot 云智能体的主要优势之一，就是能够异步完成任务，让我们腾出时间处理其他工作。接下来会利用这一点：再分配一个任务给云智能体，同时开始为应用添加功能。

## 在游戏详情页添加相关游戏

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：你处理一组任务时，它可以处理另一组。为设计团队添加相关游戏区域可能不会花太久，但这段时间仍可用于其他工作。现在将此任务分配给 Copilot 云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

随后，你应该会在议题的首条评论中看到一组 👀，表示 Copilot 已开始处理。

![Copilot 用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正在处理第二个请求。Copilot 云智能体的工作方式类似软件工程师，因此无需一直监控它；等任务完成后再审查即可。接下来开始了解如何创建和使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步处理任务。Copilot 可用于处理技术债务、创建新功能，或协助将代码从一种框架迁移到另一种框架。

本课探索了以下内容：

- 自定义代码生成环境。
- 确保操作安全。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体正在后台处理任务，现在可以开始了解如何创建和使用自定义智能体。[Copilot 云智能体也能使用 MCP 服务器][cloud-agent-mcp]，并可使用之前课程中介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
