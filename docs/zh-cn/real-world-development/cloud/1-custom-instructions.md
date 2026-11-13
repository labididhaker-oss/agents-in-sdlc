---
title: "练习 1 - 自定义指令（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应了解某些背景信息，就需要确保它能访问这些上下文。[指令文件][instruction-files]可以提供这些指导，帮助 Copilot 理解你想要它做什么，以及应该如何完成。

本练习将介绍如何：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令和按路径限定的指令文件提供给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 学习路径不同，本练习不会比较更新指令前后 Copilot 的输出。Copilot cloud agent 会在 GitHub 议题上异步工作，因此难以实时并排展示指令的影响。稍后审查 cloud agent 创建的拉取请求时，你会看到指令文件产生的影响。

## 指令文件

### 场景

Tailspin Toys 和其他优秀的开发团队一样，制定了开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 应在代码中通过 TSDoc 文档注释添加文档。
- 每个文件开头都应添加描述文件用途的注释块。

通过指令文件，可以确保 Copilot 获取正确的信息，并按照这些实践执行任务。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好，帮助它了解你的编码风格和要求。你可以通过这种强大功能引导 Copilot，获得更相关的建议和代码片段。你可以为整个存储库创建指令，也可以为特定类型的文件创建指令，以便在具体任务中使用。

指令文件有两种：

- `.github/copilot-instructions.md`：单个指令文件，会随 Copilot 针对该存储库的**每个**请求发送。此文件应包含项目级信息，即 Copilot 收到的大多数聊天或 CLI 请求都适用的上下文，例如所用技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：为特定任务或文件类型创建的文件。你可以为特定编程语言（如 TypeScript 或 Astro），或特定任务（如创建 UI 组件或一组单元测试）提供指南。

> [!NOTE]
> 在 IDE 中使用时，指令文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一个编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时都会使用存储库级指令和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

全面讨论如何创建指令文件超出了本研讨会的范围。不过，示例项目展示了有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 中的指令限定为项目级指导，例如项目内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，保持指导清晰，并提供代码应如何编写（以及不应如何编写）的示例。

创建指令文件没有唯一的方法，使用 AI 也是如此。通过试验，逐渐找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应建立完善的指令文件集合。浏览本项目中的文件时，你会看到针对多种任务的指令，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助你生成指令文件。不同界面提供的方式不同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`），当前学习路径会在相关处说明。
>
> 需要模板或起点？浏览 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

花些时间阅读此存储库附带的指令文件，其中包含一个核心 `copilot-instructions.md` 文件和多个针对不同任务的 `*.instructions.md` 文件。你可以在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，注意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。查看 **Code standards** 下的 **GitHub Actions Workflows** 指南。这些内容适用于你与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。这里有针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它定义了相对于存储库根目录的 glob，用于确定适用的文件。此处，任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会匹配。
5. 查看此项目创建单元测试时需遵循的具体指令。
6. 最后打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。注意其中指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小的可复用文件，并指向供 Copilot 参考的示例。（这些路径是相对于指令文件的位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求代码中包含文档注释。接下来会添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来只需进行一项编辑：添加一条项目级规则，要求代码中使用 TSDoc 文档注释和文件级注释头。稍后在本学习路径中为 cloud agent 分配任务时，它会读取这条指令。

编辑前先创建分支（cloud agent 会从议题目标分支读取指令，但仍建议单独提交这些更改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，该部分应位于第 27 行附近。注意其中记录了项目编码标准，但尚未规定代码文档要求，这就是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建全局指导，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。存储库级文件适合放置项目级标准，例如刚添加的文档注释规则。

## 提交、推送并合并指令更新

cloud agent 会从议题目标分支读取指令。下一练习中将议题分配给 Copilot 时，它会从 `main` 分支开始工作，因此指令更改必须先合并到 `main`，cloud agent 才能读取。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。之后 cloud agent 处理下一练习中分配的任务时，就会读取这些指令。

> [!TIP]
> 如果希望直接在 `main` 上完成本研讨会，可以跳过分支步骤并直接提交到 `main`。此处创建分支是为了让研讨会符合真实项目的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何获取本项目指令文件中的上下文，并向 `.github/copilot-instructions.md` 添加了新的存储库级标准。在本学习路径后续练习中审查 cloud agent 创建的拉取请求时，可以看到这条标准的影响。

接下来，[将第一个议题分配给 Copilot cloud agent][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
