---
title: "练习 5：迭代优化 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作成果

在整个实验中，你与 GitHub Copilot 一起完成了多项改善用户体验和添加功能的任务。你让 Copilot 为代码添加文档、构建供设计团队迭代优化的相关游戏功能，以及实现包含高对比度和浅色模式切换开关的无障碍功能。现在来检查代码更改，并在需要时向 Copilot 提供反馈，改进它的工作。

### 场景

如前所述，引入生成式 AI 不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。基于这一点，现在先审查 GitHub Copilot 提出的文档、相关游戏功能和无障碍功能，再让团队其他成员开始审查。

## 安全和 GitHub Copilot cloud agent

Copilot cloud agent 会在异步且无人监督的情况下执行任务，因此我们设置了一些安全限制，确保一切保持安全：

- Copilot 只有存储库的读取权限，并且仅对用于编写代码的分支具有写入权限。
- cloud agent 在 GitHub Actions 中运行，并创建独立的临时环境来工作。
- 任何 GitHub Actions 工作流都必须由人工批准后才能运行。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot cloud agent 创建的第一个拉取请求 (PR)，其中包含代码文档。你将使用 GitHub.com 上的标准 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。你可以[自定义或禁用 Copilot cloud agent 的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似于 **Add missing documentation** 或更完整的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。请等待 Copilot 完成工作，这可能需要几分钟。你可以休息片刻，或回顾目前学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 浏览刚更新的代码，其中包含新生成的 TSDoc 文档注释和其他文档。具体更改会有所不同。

    查看更改时，留意 TSDoc 文档注释、TypeScript 规范和注释头。它们来自你在练习 2 开始时查看的自定义指令文件。

6. 审查更新并确认符合要求后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流指示器。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入拉取请求检查列表的队列。正常情况下，单个 Astro 应用的项目检查将全部通过。这可能需要几分钟。

## 向 GitHub Copilot 请求更改

与 Copilot 协作并非单向过程。你可以像提及团队其他成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会看到这些评论，并启动另一个会话来处理请求。由于结果具有非确定性，我们无法给出固定的提示文本。你可以要求 Copilot 更新以下内容：

- 在每个代码文件的顶部添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 添加评论，要求 Copilot 更新生成的文档，并像提及其他用户一样提及 **@copilot**。可以使用上述任一建议，也可以提出其他文档方面的要求。
2. 选择 **View Session**，观察 Copilot 如何开始新会话并进行更新。你也可以选择 **Back to pull request** 返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

3. Copilot 完成更改后，拉取请求中应会出现新提交。
4. 选择 **Files changed** 选项卡，审查更改。

可以继续迭代，直到满意为止。完成后，可以将 PR 从草稿转换为准备审查状态，再合并到主分支。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在返回 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Show related games on the game details page** 或更完整的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认符合要求后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流指示器。
7. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应会进入拉取请求检查列表的队列。正常情况下，单个 Astro 应用的项目检查将全部通过。这可能需要几分钟。
9. **可选：**你也可以在 Codespace 中切换到该分支，手动测试相关游戏功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能！和之前一样，可以迭代优化 Copilot cloud agent 的工作。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加注释头和 TSDoc 文档注释（注意：分配该任务时，你还没有更新自定义指令）。与前面的 PR 一样，可以在 **Conversation** 选项卡中添加评论，Copilot 会看到并启动新会话。

## 审查无障碍功能

最后，审查通过无障碍自定义智能体实现的功能。这个 PR 应包含练习 3 中分配的高对比度模式，以及练习 4 在 mission control 中要求添加的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Add high contrast mode to website** 或更完整的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，尤其关注：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认符合要求后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流指示器。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入拉取请求检查列表的队列。正常情况下，单个 Astro 应用的项目检查将全部通过。这可能需要几分钟。
10. **可选：**你可以在 Codespace 中切换到该分支，手动测试无障碍功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换开关，确认功能正常并且在重新加载页面后仍然保留选择。

注意，无障碍自定义智能体帮助 Copilot 根据无障碍最佳实践实现了这些功能。如果发现任何无障碍问题或改进点，可以像处理前面 PR 一样在评论中提及 **@copilot**，请求更新。

## 可选练习 — 继续委派

cloud agent 最适合处理真实待办任务，让你可以把注意力放在其他工作上。可以创建几个议题并分配给 Copilot，练习这一工作方式。以下是一些建议：

- 在游戏详情页创建支持者意向表单。
- 为游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 学习路径。在这些课程中，你：

- **检查了此存储库附带的自定义指令文件**，稍后在 cloud agent 输出中观察它们的影响。
- **将议题分配给 Copilot cloud agent**，并观察它设置环境、制定计划和异步执行任务。
- **创建并使用自定义智能体**实现无障碍功能，添加高对比度和浅色模式切换开关。
- **使用 Copilot agents 页面作为 mission control**，监视并引导无障碍会话。
- **审查并迭代优化 cloud agent 的拉取请求**，提及 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成 Cloud agent 学习路径。其他学习路径可以帮助你继续探索：

- 🖥️ **[VS Code 学习路径](../../vscode/)** — 在 IDE 中直接探索 Copilot Chat agent mode 和 MCP 集成。
- 💻 **[CLI 学习路径](../../cli/)** — 在终端交付两个经过审查的更改，使用 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 桥接以委派给 cloud agent。

你也可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给 cloud agent。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
