---
title: "练习 2 - 使用 Copilot Agent Mode 添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。开发者的典型工作流程包括查找所有相关文件、进行更改、运行测试、调试、找出遗漏的文件并再次更新……这样的工作还会持续下去。

Copilot Agent Mode 正是为此而设计的。

Copilot Agent Mode 可以在 IDE 中更自主地工作。它的行为类似开发者：先探索现有项目结构，进行必要更新，运行测试等任务，并自动修复发现的问题。现在来了解如何使用 Agent Mode 为网站添加新功能。

> [!NOTE]
> Agent mode 和 cloud agent 的名称相似，但它们针对不同的使用场景。Agent mode 在 IDE 中执行任务，支持快速反馈和交互。Cloud agent 则像团队成员一样异步工作，通过议题和拉取请求与你交流。

本练习将介绍如何：

- 让 Copilot Agent Mode 探索项目、识别相关文件并协调多文件更改。
- 使用 GitHub Copilot Agent Mode 跨 UI 和数据层实现新功能。
- 在将代码合并到代码库之前，审查 Copilot Agent Mode 生成的更改和测试。

## 场景

游戏列表不断变长，你希望用户能够按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot Agent Mode 完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

开始更改前，先探索 Tailspin Toys 网站，了解它目前的功能。

该网站是一个采用开发者主题的桌游众筹平台，可列出游戏并显示其详情。它是一个单独的 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数放在 `src/lib/` 中，交互功能使用标准 DOM API 编写的小型作用域 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习结束后，你应已回到 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新终端窗口。
3. 创建并切换到筛选功能的新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下内容的横幅，其中包含 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，提示打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择终端中的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 Codespace 时，从终端选择 localhost URL 会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace 的专用隧道，此时网站服务器运行在 Codespace 中。

### 探索网站

网站运行后，可以探索其功能。主要功能包括：

- **主页**：显示桌游列表，包括标题、图像和描述。
- **游戏详情页**：选择游戏后会打开详情页，显示标题、描述、发行商和类别等详细信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；每个步骤会在需要时说明。

网站的初始实现可以正常工作，但我们希望添加新功能。先查看待办列表。根据模板设置项目时，系统已自动创建议题；现在让 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。**Agent** 智能体可以自主规划并跨文件实现更改、运行终端命令并调用工具。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某一种模型。构建本研讨会时使用的是 Claude Sonnet 4.5，因此将它列入说明。我们希望代码建议相对一致，以提供良好的体验。但由于 LLM 具有概率性，你可能会收到与说明不同的建议。这完全正常，也是预期情况。

> [!NOTE]
> 由于 LLM 的输出具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，让它查看议题待办列表：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 审查生成的议题列表。

注意 Copilot 还根据对用户的帮助程度为项目确定了优先级。

## 审查指令文件

让智能体生成代码前，先查看用于向 Copilot 提供上下文的指令文件。我们将使用[用户界面 (UI) 指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中介绍了如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 注意其中的总体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向不同文件类型专用指令文件的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件和项目中的文件。路径相对于指令文件所在位置，因此可以复用指令，将较大的指令拆分成更小、更易管理的部分，并提供示例和模板。

## 实现筛选功能

要完成筛选功能，至少需要对应用进行以下三项更新：

- 在数据层 (`src/lib/`) 添加或优化剩余的筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，添加筛选 UI。

此外，合并前需要运行并通过测试。Copilot Agent Mode 可以执行这些任务！现在添加功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![显示如何选择 UI 指令文件的屏幕截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单中仍选中了 **Agent**。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用与待办列表中相关议题对应的提示，让 Copilot 实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并查找与功能相关文件的过程。你应能看到它找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![显示 Copilot 正在探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会显示相应的 **Keep** 或 **Undo** 按钮，因此无需在工作进行期间选择它们。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 面板中的对话框，要求用户确认是否运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会出现暂停，也可能会有测试失败。这没有关系！Copilot 会在生成代码和运行测试之间反复迭代，直到任务完成且未检测到错误。

   ![完整的 Copilot Agent Mode 聊天会话屏幕截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 请始终审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，试用新功能！
11. 确认所有内容都能正常工作并且已审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot Agent Mode 为 Tailspin Toys 网站添加新功能。你了解了：

- 使用 GitHub Copilot Agent Mode 跨 UI 和数据层实现新功能。
- 让 Copilot Agent Mode 探索项目、识别相关文件并协调多文件更改。
- 在将代码合并到代码库之前，审查 Copilot Agent Mode 生成的更改和测试。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 额外探索练习——实现分页

随着游戏列表不断增长，需要添加分页。运用本练习学到的技能，让 Copilot 更新网站并实现分页。考虑以下事项：

- 遵循现有最佳实践，包括使用已有的指令文件。
- 规划分页方式：允许用户选择每页大小，还是使用固定值。
- 创建提示时提供必要的指导，使 Copilot 按照你的预期实现功能。
- 可能需要与 GitHub Copilot 迭代交流、请求更改并提供上下文。这是使用 Copilot 的正常工作流程。

## 资源

- [Copilot ask、edit 和 agent 模式：功能和使用时机][choose-mode]
- [VS Code 中的 agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
