---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot cloud agent 可以自主执行更新代码和添加功能等任务。完成工作后，它会创建待审查的草稿 PR，供开发者检查。这样可以将繁琐任务交给智能体，加快开发流程，让开发者专注于更宏观的工作。

你将探索 Copilot cloud agent 的以下功能：

- 自定义代码生成环境。
- 确保操作以安全的方式执行。
- 了解明确界定任务范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受聘开发网站的承包商没有留下完善的文档，甚至完全没有文档。首先，他们希望为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队准备改善游戏发现体验。他们希望每款游戏的详情页显示相关游戏，也就是同一类别中的其他游戏，让用户可以继续浏览。他们暂时不需要完善的实现，只需要能用于验收测试用户体验 (UX) 的版本。目前这项工作被其他更高优先级的议题挡住了。

这两类任务都很容易被降级到较低优先级，非常适合分配给 Copilot cloud agent。它可以异步处理这些任务，让开发者专注于其他工作，之后再回来审查 Copilot 的结果并确认符合预期。

## 认识 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像人类开发者一样在后台执行任务。你可以通过多种方式向它分配工作，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求来跟踪进度、设置环境并开始工作。你可以在 Copilot 会话运行期间或完成后查看它的工作。准备好让你审查建议的解决方案时，它会在拉取请求中提及你。

## 明确界定指令范围的重要性

GitHub Copilot 并非魔法，不能让你跳过思考。即使看似简单的操作，深入分析后也会发现复杂性，因此请谨慎界定 Copilot cloud agent 的任务范围，并参阅[最佳实践][cloud-agent-best-practices]。将它视为 AI 结对编程助手：分阶段工作、学习、试验并调整。引入生成式 AI 不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令以及它们如何引导 Copilot。如果你已经完成了自定义指令练习，就已了解 `.github/copilot-instructions.md` 的作用；如果还没有，现在可以快速查看。分配任务给 Copilot cloud agent 前，先以只读方式查看存储库中已有的指令文件，以便稍后观察它们的影响。

在 GitHub Web 界面中打开以下文件，或者使用已启动的 Codespace 查看：

- `.github/copilot-instructions.md`：检查 **Code standards** 部分，尤其是有关 TypeScript 规范和 TSDoc 文档注释的要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它会将指令限定在 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给 cloud agent 时，注意查看生成的拉取请求中是否包含 TSDoc 文档注释、TypeScript 规范和文件注释头。这些内容来自指令文件，我们会在后续练习审查 PR 时再次提到。

## 为 Copilot cloud agent 设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，以确保项目状态良好。分配任务给 Copilot 也是如此，因为它的工作方式类似于软件工程师 (SWE)。

cloud agent 使用 [GitHub Actions][github-actions] 提供执行任务所需的环境。你可以创建一个[专用设置工作流][setup-workflow]，并将它配置在 `.github/workflows/copilot-setup-steps.yml` 文件中，让它在 Copilot 开始工作前运行，以便访问所需的开发工具和依赖项。此工作流已提前配置，以便实验流程顺利进行并提供学习机会。对于这个单一 Astro 应用，它会确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许从存储库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限限制为*你的步骤*所需的最低级别。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在使用 Astro + Drizzle/libSQL）
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

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

这看起来和其他 GitHub 工作流文件类似，但有几个关键点：

- 它只包含名为 `copilot-setup-steps` 的单个作业。该作业会在 Copilot 开始处理拉取请求前通过 GitHub Actions 执行。
- 注意 `workflow_dispatch` 触发器。它允许你从存储库的 **Actions** 选项卡手动运行工作流，方便测试工作流是否成功，而不必等 Copilot 启动它。

## 添加文档

虽然人人都知道文档的重要性，但大多数项目的文档不是过时就是缺失。这类技术债务常常得不到处理，降低了工作效率，也增加了维护代码库或帮助新开发者加入团队的难度。Copilot 很擅长生成文档，因此这项工作非常适合分配给 Copilot cloud agent。它会在后台生成所需文档，后续练习再审查结果。

1. 在新的浏览器选项卡中转到 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建新议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开新创建的拉取请求 (PR)，其标题类似于 `[WIP]: Code lacks documentation`。如果列表中没有出现新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才能在 PR 中创建待办事项列表。它正在创建环境（运行前面介绍的工作流）、分析项目并确定最佳解决方案。

13. 查看列表和即将执行的任务。
14. 向下滚动拉取请求时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码并了解当前状态、暂停思考并决定合适的方案，以及创建代码。

整个过程可能需要几分钟。Copilot cloud agent 的主要目标之一是异步执行任务，让你专注于其他工作。接下来同时利用这一点：再分配一个任务给 Copilot cloud agent，然后转而编写应用程序代码并添加功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot cloud agent 的一大优势是可以拆分工作：你专注于一组任务，它则处理另一组。为设计团队添加相关游戏部分可能不会花很长时间，但这段时间仍可用于其他工作。现在就将此任务分配给 Copilot cloud agent！

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为：`Show related games on the game details page`
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

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题中的第一条评论旁边会出现 👀，表示 Copilot 已开始处理任务！

![Copilot 使用 eyes 表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二个请求！Copilot cloud agent 的工作方式类似于软件工程师，因此你不需要主动监视它；等它完成后再审查即可。现在可以转而创建并使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。使用 cloud agent，可以分配议题并让 Copilot 异步处理。可以让 Copilot 处理技术债务、创建新功能，或协助将代码迁移到其他框架。

你探索了以下内容：

- 自定义代码生成环境。
- 确保操作以安全的方式执行。
- 了解明确界定任务范围的重要性。
- 将议题分配给 Copilot。

Copilot cloud agent 正在后台工作。接下来转而创建并使用自定义智能体。[Copilot cloud agent 也可以使用 MCP 服务器][cloud-agent-mcp]，并且可以使用之前模块中介绍过的自定义指令。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

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
