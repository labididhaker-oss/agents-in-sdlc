---
title: "练习 4 - 监视和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot cloud agent 处理了三个改善用户体验和添加功能的独立任务。cloud agent 虽然可以异步自主工作，但监视这些任务仍然很重要。

GitHub 提供了多种工具来管理分配给 cloud agent 的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制中心，你可以查看所有有未关闭拉取请求 (PR) 的智能体任务、了解它们执行的操作，甚至引导正在进行的会话。

本课将介绍如何：

- 使用智能体页面监视 cloud agent 任务。
- 引导正在进行的会话，要求添加其他功能。

## 场景

让智能体创建高对比度模式后，团队意识到也可以同时添加浅色模式。既然正在更新网站样式并添加切换功能，将这项功能也纳入其中就很合理。现在你要引导智能体的工作，让它同时添加浅色模式和高对比度模式。

## 审查 Copilot cloud agent 任务

查看当前分配给 Copilot cloud agent 的所有任务的状态。

1. 转到 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。你应能看到分配给 Copilot 的任务，包括：
    - 更新代码文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从存储库检出代码。
    - 为 Copilot 创建环境。
    - 设置 MCP 服务器。
    - 执行完成分配任务所需的其他步骤。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 的工作进度和采用的方案而有所不同。

4. 还可以查看右侧显示的拉取请求 (PR) 窗格，以便监视 PR 和已更改的文件。

## 引导 cloud agent

现在你已了解活动任务的状态。接下来要求 Copilot 在创建高对比度模式的同时添加浅色模式。

1. 选择有关添加高对比度模式的会话。具体标题会因 Copilot 采用的名称和当前工作状态而异。

    ![mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示设置已完成并开始工作。此时会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中突出显示 steer active session while copilot is working 对话框的屏幕截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示并将它纳入工作流程。

## 让 Copilot 完成工作

和之前一样，Copilot 会继续处理更新后的任务！在你发送消息时，它会先完成当前步骤，然后将新请求纳入工作流程。

与之前一样，这需要几分钟时间。你可以趁此暂停片刻，回顾至今学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，它是监视和引导 GitHub Copilot cloud agent 任务的中心位置。通过这个控制中心，你可以跟踪所有活动和已完成的任务、审查正在进行的工作，甚至重定向执行中的任务。

你了解了：

- 使用 mission control 和智能体页面监视 cloud agent 任务。
- 重定向正在进行的会话，以请求添加其他功能。

Copilot 正在完成无障碍功能。接下来[迭代优化 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代优化 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
