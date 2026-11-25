---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或 Copilot 需要了解一些背景信息，就要确保它能访问这些上下文。[指令文件][instruction-files]可提供相关指导，帮助 Copilot 理解你不仅希望它做什么，也希望它如何完成。

本练习将：

- 探索如何通过存储库自定义指令和按路径限定的指令文件，向 Copilot 提供项目上下文、编码指南和文档标准。
- 在 `.github/copilot-instructions.md` 中添加一条适用于整个存储库的新标准。

> [!NOTE]
> 与 VS Code 和 CLI 练习路径不同，本练习不会运行“前后对比”提示。Copilot 云智能体会在 GitHub 议题上异步工作，因此难以实时并排展示指令的影响。稍后审查云智能体生成的拉取请求时，可以看到这些指令的作用。

## 指令文件

### 场景

和其他优秀的开发团队一样，Tailspin Toys 也有一套开发实践指南和要求，包括：

- 数据层必须始终包含单元测试。
- UI 应使用深色模式并具有现代风格。
- 应通过 TSDoc 文档注释为代码补充说明。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过指令文件，可以确保 Copilot 获得正确的信息，并按照这些实践要求完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。你可以通过自定义指令引导 Copilot，获得更符合需求的建议和代码片段。你可以为整个存储库创建指令，也可以为特定文件类型创建指令。

指令文件分为两种：

- `.github/copilot-instructions.md`：一个适用于存储库中**每个**请求的指令文件。它应包含与大多数 Copilot 聊天或 CLI 请求相关的项目级信息，例如所用技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建的指令文件。你可以为 TypeScript 或 Astro 等特定语言提供指南，也可以为创建 UI 组件或一组单元测试等任务提供指南。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [也支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

完整介绍如何创建指令文件超出了本研讨会的范围。不过，示例项目展示了一种有代表性的做法。总体而言：

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
2. 查看文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。**Code standards** 下还有 **GitHub Actions Workflows** 指引。这些指令适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意其中有 Astro 文件、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看文件开头的 `applyTo` 字段；它会设置一个相对于存储库根目录的 glob，用来确定适用的文件。此处会匹配任何 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看此项目中有关创建单元测试的专门指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件末尾。查看其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可重复使用的文件，并为 Copilot 提供可参考的示例。（这些路径是相对于指令文件的，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但目前还没有要求为代码添加文档注释。接下来的步骤会添加 TSDoc 文档注释和文件级注释标头的规则。

## 添加存储库级标准

接下来只需在此处编辑一次：添加一条适用于整个项目的规则，要求使用 TSDoc 文档注释记录代码，并添加文件级注释标头。之后的练习中，云智能体处理你分配的议题时会采用这条规则。

编辑前，先创建一个分支（云智能体会读取议题目标分支中的指令，但仍建议将更改单独提交）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，改进 Copilot 给出的代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。此部分记录了项目的编码标准，但尚未包含代码文档规则，因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释标头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级文件（`.github/copilot-instructions.md`）中编写全局指引，也可以使用 `*.instructions.md` 为特定语言、文件类型或任务提供指引。刚添加的文档注释规则属于项目级标准，因此存储库级文件是合适的位置。

## 提交、推送并合并指令更新

云智能体会读取议题目标分支中的指令。下一练习中将议题分配给 Copilot 时，它会从 `main` 分支创建新分支。因此，必须先将指令更改合并到 `main`，云智能体才能使用。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并将其合并。之后练习中，云智能体处理已分配的议题时就会读取这些指令。

> [!TIP]
> 如果想在本研讨会中直接使用 `main`，可以跳过分支步骤，直接提交到 `main`。此处采用分支，是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你已了解 Copilot 如何获取本项目指令文件中的上下文，并在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。后续练习中，云智能体生成的拉取请求将展示这条标准的作用。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更有效 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
