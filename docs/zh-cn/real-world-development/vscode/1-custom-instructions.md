---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或 Copilot 需要了解一些背景信息，就要确保它能访问这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中最重要的是[指令文件][instruction-files]，它们可以指导代码生成。

本练习将：

- 探索如何通过存储库自定义指令和按路径限定的指令文件，向 Copilot 提供项目上下文、编码指南和文档标准。
- 在当前指令下生成筛选功能的第一个数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加一条适用于整个存储库的新标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 将指令更新和筛选功能切片提交到 `main`，供下一练习使用。

> [!CAUTION]
> 生成的代码可能不符合所有编码标准。Copilot 的行为具有概率性，因此本练习关注的是更新指令后行为的*整体变化趋势*，而不是要求输出逐字一致。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层必须始终包含单元测试。
- UI 应使用深色模式并具有现代风格。
- 应通过 TSDoc 文档注释为代码补充说明。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过指令文件，可以确保 Copilot 获得正确的信息，并按照这些实践要求完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。你可以为整个存储库创建指令，也可以为特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：一个适用于存储库中**每个**请求的指令文件，包含与大多数 Copilot 聊天或 CLI 请求相关的项目级信息，例如所用技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：针对特定任务或文件类型创建的文件，可以为 TypeScript 或 Astro 等特定语言，以及创建 UI 组件或一组单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [也支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

完整介绍如何创建指令文件超出了本研讨会的范围，不过示例项目展示了一种有代表性的做法。总体而言：

- 让 `copilot-instructions.md` 专注于项目级指导，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，确保指导清晰，并提供代码示例，说明应该和不应该采用的写法。

创建指令文件没有唯一正确的方法，使用 AI 也是如此。通过试验找到最适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的指令文件。查看本项目中的文件时，你可能会发现其中包含多种任务的指令，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮你生成指令文件。不同产品界面的操作方式不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相应练习会在适用时介绍这些操作。
>
> 想查找模板或入门示例？可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索本项目的自定义指令文件

花些时间阅读此存储库附带的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。**Code standards** 下还有 **GitHub Actions Workflows** 指引，这些指令适用于与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意其中有 Astro、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它会设置相对于存储库根目录的 glob，确定哪些文件适用。此处会匹配任何 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看此项目中有关创建单元测试的专门指令。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件末尾。查看其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可重复使用的文件，并提供可参考的示例。（这些路径是相对于指令文件的，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但目前还没有要求为代码添加文档。接下来会添加 TSDoc 文档注释和文件注释标头的规则。

## 为更改创建分支

按照最佳实践，为更改创建分支：

1. 返回上一练习中使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新的对话。模式和模型选择因练习而异；每个步骤会在适用时说明。

先在保留当前指令文件的情况下发送提示，然后更新指令并再次发送相同提示，以查看差异。

1. 关闭之前练习中打开的编辑器标签页，让 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，告诉 Copilot 辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中显示智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 留意生成的辅助函数是一个带类型的函数，以 `db` 客户端作为第一个参数，并返回带类型的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层规范。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释标头。

> [!CAUTION]
> Copilot 的行为具有概率性，即使没有指令也可能添加文档注释。如果发生这种情况也没关系，本练习的重点仍是观察添加指令后行为是否更加一致。

## 添加存储库级标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加一项编码标准，改进 Copilot 给出的代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到通常位于第 27 行附近的 **Code formatting requirements** 部分。查看其中记录的项目编码标准；目前没有代码文档规则，因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求添加文件注释标头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级文件（`.github/copilot-instructions.md`）中编写全局指引，也可以使用 `*.instructions.md` 为特定语言、文件类型或任务提供指引。刚添加的文档注释规则属于项目级标准，因此存储库级文件是合适的位置。

## 再次运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清空当前对话。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 关注正确的文件。
3. 发送与之前**完全相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意生成文件现在以类似以下注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供从数据库检索发行商信息的函数。
    */
   ```

5. 注意生成的函数现在也包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * 返回所有发行商及其 ID 和名称的列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 解析为发行商对象数组的 Promise。
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用于真实代码。下一课会继续在此基础上开展工作。

## 提交指令并推送分支

指令文件与存储库中的其他资源一样，也通过源代码管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令和辅助函数更改已保存：

   ```bash
   git status
   ```

3. 暂存并提交指令更新，以及 Copilot 为辅助函数基础工作更改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他相关更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求，并将其与待办事项中的文档标准议题关联起来。可以手动操作，不过 Copilot 也可以利用已连接到项目的 GitHub 工具代为完成。请 Copilot 查找议题、创建与议题关联的 PR，然后合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已预先将 GitHub 工具连接到工作区，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。后续练习会介绍此连接的工作原理——模型上下文协议 (MCP)。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中选中 **Agent**。
3. 请 Copilot 查找与更新指令文件有关的议题，并从当前分支创建 PR，说明指令更新和新添加的发行商辅助函数：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. Copilot 请求你**允许**它代表你执行 GitHub 操作时，先审查命令，再酌情选择 **Allow**。
6. PR 创建后，使用以下提示请求 Copilot 将 PR 合并到 `main`，并更新本地分支：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 请求你允许它代表你执行 GitHub 操作和 shell 命令时，先审查命令，再酌情选择 **Allow**。

现在已经在 GitHub Copilot 的帮助下创建并合并了拉取请求。

## 总结和后续步骤

你已了解 Copilot 如何获取本项目指令文件中的上下文，并使用 VS Code 中的 Copilot Chat 完成以下任务：

- 发送代码生成提示，观察 Copilot 在*现有*指令下生成的内容。
- 在 `.github/copilot-instructions.md` 中添加存储库级标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 将指令更新和辅助函数基础工作提交到 `main`，供下一课继续使用。

接下来，在[智能体模式][next-lesson]中应用这些指令，让 Copilot 跨多个文件实现新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更有效 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
