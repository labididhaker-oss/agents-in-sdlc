---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent) 可让 GitHub Copilot 在云端异步工作。你在 GitHub 上分配任务后，cloud agent 会在后台处理：探索存储库、进行更改并创建拉取请求，让你可以继续处理其他事务。

在这些练习中，你将添加 cloud agent 会遵循的自定义指令，然后分配 GitHub 议题并让它实现工作。你将审查并使用自定义智能体来引导它的工作方式，从智能体仪表板监视并引导会话，最后审查它创建的拉取请求并迭代优化结果。

## 练习

| 练习 | 主题 | 描述 |
|----------|-------|-------------|
| [0. 准备工作][ex0] | 设置 | 创建存储库和 Codespace |
| [1. 自定义指令][ex1] | 上下文 | 添加 cloud agent 会遵循的自定义指令 |
| [2. Cloud agent][ex2] | 异步智能体 | 将议题分配给 Copilot cloud agent |
| [3. 自定义智能体][ex3] | 专业智能体 | 审查并使用自定义智能体 |
| [4. 管理智能体][ex4] | 监视 | 监视并引导智能体会话 |
| [5. 迭代][ex5] | 审查 | 审查 PR、迭代优化 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本研讨会前，请确保具备以下条件：

- [ ] 有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 帐户
- [ ] 已为你的帐户或组织启用 Copilot cloud agent

> [!TIP]
> 没有付费计划？通过 [GitHub Education][callout-student-plan-education] 验证身份的学生可免费使用 GitHub Copilot。**Copilot Student** 计划包含本研讨会所需的智能体、MCP、代码审查和 Copilot CLI 功能，因此你可以完成所有学习路径。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> 除 Copilot Free 外的所有计划（Copilot Student、Pro、Pro+、Business 和 Enterprise）都支持 cloud agent。对于 Copilot Business 或 Enterprise，管理员必须为组织启用它。

> [!NOTE]
> 本学习路径不介绍 MCP。要了解如何将 MCP 服务器与 Copilot 配合使用，请参阅 [CLI 学习路径](../cli/)或 [VS Code 学习路径](../vscode/)。

## 开始使用

**[从练习 0：准备工作开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
