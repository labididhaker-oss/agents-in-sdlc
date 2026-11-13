---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：Agent mode][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用 agent mode 构建了筛选功能。创建拉取请求前，应确认它在浏览器中正常工作。你将连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能，而不是自己逐一操作应用，然后发布分支并创建 PR。

本练习将介绍如何：

- 了解模型上下文协议 (MCP) 以及 MCP 服务器如何使用新工具扩展 Copilot。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 使用它在浏览器中手动测试筛选功能。
- 发布分支并创建拉取请求。

## 什么是模型上下文协议 (MCP)？

当 agent mode 可以访问编辑器以外的资源时，它的功能会强大得多。Copilot 使用模型上下文协议 (MCP) 完成这一点：这是智能体与外部工具和服务通信的标准方式。

![agent mode 内部工作原理示意图，展示它如何与上下文、LLM 和工具交互，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议 (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体提供了与外部工具和服务通信的方式。通过 MCP，AI 智能体可以实时访问最新信息（使用资源），并代表你执行操作（使用工具）。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当智能体与外部工具和服务之间的桥梁，并负责管理智能体与外部工具（例如现有 API 或 NPM 包等本地工具）之间的通信。每个 MCP 服务器代表一组智能体可以访问的工具和资源。

一些常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API，让 AI 智能体可以创建新存储库、更新现有存储库以及管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，让 AI 智能体可以浏览网页、填写表单和选择按钮。

还有许多其他 MCP 服务器提供不同的工具和资源。GitHub 提供了 [MCP 注册表](https://github.com/mcp)，方便发现和参与生态系统。

> [!CAUTION]
> 出于安全考虑，请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前应仔细审查源代码、验证发布者并考虑安全影响。只使用可信的 MCP 服务器，并谨慎授予其访问敏感资源或执行操作的权限。

## 检查 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区中可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 你会看到已配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

项目模板已包含此 `github` 条目，因此 Copilot 可以在上一练习中读取你的议题待办列表。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装。VS Code 会通过 HTTP 连接到它，并在 Copilot 首次使用其工具时使用 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在再添加一个服务器。[Playwright MCP 服务器][playwright-mcp-server]会为 Copilot 提供可控的浏览器，正好可以用于测试功能。

1. 在 `.vscode/mcp.json` 中，在 `github` 旁边添加 `playwright` 条目，使文件内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标记让 Playwright 在不显示窗口的情况下运行浏览器。这是 Codespace 中的必要设置，因为 Codespace 没有可显示浏览器的桌面。

> [!NOTE]
> Tailspin Toys 项目已在端到端测试中使用 Playwright，因此通常已经安装 Playwright 所需的浏览器。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器。Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它立即启动服务器，确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，以便启动服务器。

手动启动是可选步骤。如果跳过，VS Code 会在下一步 Copilot 首次需要服务器工具时启动服务器，并提示你信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]会为 Copilot 提供可驱动的真实浏览器。无需亲自操作应用来检查更改，智能体可以打开页面、导航、应用筛选条件并读取结果，然后向你总结观察结果。这是在不离开对话的情况下快速确认功能是否符合预期的方式。

Playwright MCP 服务器会基于页面的[无障碍树][playwright-mcp-server]工作，而不是使用屏幕截图。这样智能体可以像辅助技术一样，根据带标签的结构化元素（按钮、链接、列表项）进行推理，因此简单的功能检查也可以顺便验证基本无障碍情况。

连接服务器并启动应用后，要求 Copilot 运行刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐步执行操作，并报告结果。根据议题中的验收标准审查其总结。如果发现异常，可以提出后续问题，或要求它修复代码后再创建拉取请求。

> [!NOTE]
> 测试时，应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请先重新启动再发送提示。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求 (PR)，供团队审查了。首先发布 `filtering-vscode` 分支。

1. 在 Codespace 中打开 **Source Control** 面板并审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 更改的 Source Control 面板屏幕截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过多种方式创建拉取请求，例如在 github.com 上创建或使用 GitHub 命令行界面 (CLI)。既然你正在使用 GitHub Copilot，不妨让它代为创建 PR！它可以找到相关议题并将 PR 与议题关联起来。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确认智能体下拉菜单中选中了 **Agent**，这样 Copilot 才能使用 GitHub 工具。
3. 让 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行收集信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会提示你进行 GitHub 身份验证；按提示授权即可。
5. 观察 Copilot 搜索议题、找到正确议题并创建 PR 的过程。
6. 选择 Copilot 生成的链接以审查拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜！本练习中你：

- 了解了模型上下文协议 (MCP) 以及 MCP 服务器如何使用新工具扩展 Copilot。
- 将 Playwright MCP 服务器添加到工作区。
- 使用它在浏览器中手动测试筛选功能，然后再发布。
- 发布分支并为筛选功能创建拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。下一步，[创建自定义智能体][next-lesson]，简化无障碍审查等专注任务。

## 资源

- [MCP 究竟是什么？为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
