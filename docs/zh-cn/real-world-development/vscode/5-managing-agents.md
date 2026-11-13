---
title: "练习 5 - 监视和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

使用 GitHub Copilot agent mode 时，它会自主探索代码库、提出更改、编辑文件并运行命令。由于 Copilot 在你的计算机上实时工作，VS Code 中的 Copilot Chat 会显示每次工具调用和文件编辑。你可以内联审查每个差异、接受或拒绝单项更改，也可以通过后续提示引导会话，在编辑器中优化或扩展工作。

本练习将介绍如何：

- 实时监视无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话中途引导智能体，在高对比度模式之外添加浅色模式切换开关。

## 场景

[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作进行期间，设计团队提出了额外需求：还需要一个浅色模式切换开关，供觉得默认主题太暗的用户使用。你将引导现有会话扩展工作，而不是从头开始新会话。

## 监视 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会以内联差异的形式显示，并提供 **Keep** 和 **Undo** 控件。

1. 返回练习 4 中打开的 Copilot Chat 视图。
2. 随着智能体工作，滚动聊天内容并查看每次工具调用。你会看到文件读取、文件编辑和它请求运行的终端命令。
3. 智能体提出文件编辑时，差异会内联显示：
    - 选择 **Keep** 接受更改。
    - 选择 **Undo** 还原更改，让智能体重新尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看该会话中智能体修改的所有文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你确认安全的命令。

5. 等待智能体完成高对比度模式的实现。最后应会看到总结更改内容的消息。

## 引导智能体添加浅色模式

高对比度模式已实现，现在扩展同一对话，添加浅色模式切换开关。由于继续使用同一聊天会话，智能体会保留先前工作的上下文，包括涉及的组件、样式方法和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会继续之前的工作，探索已经做出的更改，并提出添加浅色模式的方案。
3. 按照高对比度模式的步骤审查每项新编辑，并使用 **Keep** 或 **Undo**。
4. 如果智能体的方案与你的预期不同（例如它尝试重构切换 UI），可以使用聊天输入框旁的 **Stop** 按钮停止任务，然后发送说明进一步澄清。

> [!TIP]
> 在工作进行中引导会话比从头开始更快。可以用后续提示要求调整（“简化 CSS，改用 CSS 变量而不是重复选择器”）、纠正方向（“不要添加第三个切换开关，将浅色模式集成到现有设置面板”）或扩展范围（“为两个切换开关都添加键盘快捷方式”）。

## 审查工作集

提交工作前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查所有已更改文件。你应能看到 Astro 组件、样式以及相关测试的更新。
3. 打开几个已更改文件并查看差异。确认其中采用了自定义智能体所要求的无障碍模式，包括 ARIA 属性、键盘导航、语义化 HTML 和本地存储。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课将发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat agent mode 监视并引导了正在进行的会话：逐项审查文件编辑、接受或拒绝更改，并发送后续提示扩展工作，同时保留上下文。

本课中，你了解了：

- 实时监视智能体的工作。
- 接受或拒绝单项文件编辑。
- 使用后续提示在会话进行中引导智能体。

下一课将[发布无障碍更改并创建拉取请求][next-lesson]，同时审查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用 agent mode][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代优化 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
