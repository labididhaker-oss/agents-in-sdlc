---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：Agent mode →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应了解某些背景信息，就需要确保它能访问这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中一种主要方式是[指令文件][instruction-files]，它们可以为代码生成提供指导。

本练习将介绍如何：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令和按路径限定的指令文件提供给 Copilot。
- 使用当前指令生成筛选功能的第一个数据切片（发行商辅助函数）。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何遵循新标准。
- 将指令更新和筛选功能切片提交到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合你设置的部分标准。Copilot 的输出具有非确定性。本练习的重点是观察更新指令后整体行为的变化趋势，而不是逐字匹配某种输出。

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

- `.github/copilot-instructions.md`：单个指令文件，会随 Copilot 针对该存储库的**每个**请求发送。此文件应包含项目级信息，即 Copilot 收到的大多数请求都适用的上下文，例如所用技术栈、项目概述、最佳实践和全局指导。
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

## 为更改创建分支

按照最佳实践，为更改创建一个分支。

1. 返回上一练习中使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；每个步骤会在需要时说明。

为了观察自定义指令的影响，先在当前指令文件下发送提示。稍后更新指令后，再发送相同提示并比较结果。

1. 关闭之前练习中打开的编辑器选项卡，避免 Copilot 获取不需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，让 Copilot 可以应用文件更改。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 注意生成的辅助函数是类型化函数，以 `db` 客户端为第一个参数，并返回类型化的发行商数组。这些约束来自适用于 `src/lib/*.ts` 的 `.github/instructions/drizzle.instructions.md` 数据层规范。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的输出具有概率性，因此它有可能在没有相关指令时也添加文档注释。如果发生这种情况也没关系，本练习的重点仍是观察添加指令后行为的一致性变化。

## 添加新的存储库标准

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

## 重新运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意现在生成的文件开头会包含类似以下内容的注释块：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意生成的函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将它应用到后续练习会用到的实际代码中。

## 提交指令并推送分支

指令文件和存储库中的其他资源一样，也需要使用相同的源代码管理方式。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），打开新终端窗口。
2. 在终端中运行以下命令，确认指令更新和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送完成后，应创建拉取请求，并关联待办列表中关于文档标准的议题。也可以手动操作，但 Copilot 可以使用已连接到项目的 GitHub 工具代为完成。让 Copilot 找到议题、创建用于关闭该议题的 PR，然后合并 PR。

> [!NOTE]
> 这是 Copilot 首次代表你在 GitHub 上操作。项目模板已将 GitHub 工具连接到工作区，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按提示授权即可。后续练习会介绍这种连接的工作方式（模型上下文协议）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保在智能体选择器中选中 **Agent**。
3. 让 Copilot 查找与更新指令文件有关的议题，并从当前分支创建 PR，说明指令更新和新的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 将开始查找议题并创建 PR。
5. 如果 Copilot 提示 **Allow** 它代表你执行 GitHub 操作，请审查命令，并酌情选择 **Allow**。
6. PR 创建后，使用以下提示要求 Copilot 合并 PR 并让本地分支返回 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果 Copilot 提示 **Allow** 它代表你执行 GitHub 操作和运行 shell 命令，请审查命令，并酌情选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结和后续步骤

你了解了 Copilot 如何获取本项目指令文件中的上下文，并使用 VS Code 中的 Copilot Chat：

- 发送代码生成提示，并观察现有指令下 Copilot 的输出。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 再次发送相同提示，观察生成的代码如何遵循新标准。
- 将指令更新和辅助函数基础提交到 `main`，让下一练习可以继续使用。

接下来，在 [agent mode][next-lesson] 中让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 的个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
