---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "了解存储库指令，添加文档标准，并将其应用到筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，你需要让 Copilot 能够获取相应指导。[指令文件][instruction-files]不仅描述想要什么代码，也说明代码应如何组织。现在你已经构建了筛选功能，接下来将了解 Copilot 使用的指令、添加文档标准，并将其应用到代码中。

本课将介绍如何：

- 了解存储库指令和按路径限定的指令文件如何提供给智能体。
- 更新指令文件，确保遵循编码标准。
- 了解指令文件对代码的影响。

## 场景

Tailspin Toys 和其他优秀的开发团队一样，制定了开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明其用途、参数和返回值（如有可注入的 `db` 参数，也要说明）。
- 可复用的 Astro 组件应说明其 `Props` 契约；相关代码更改时，注释也应保持更新。
- 应保留现有格式设置和 lint 指南。

通过指令文件，可以确保 Copilot 获取正确的信息，并按照这些实践执行任务。

## 指令文件

自定义指令可以向 Copilot 提供上下文和偏好，帮助它了解你的编码风格和要求。你可以通过这种强大功能引导 Copilot，获得更相关的建议和代码片段。你可以为整个存储库创建指令，也可以为特定类型的文件创建指令，以便在具体任务中使用。

指令文件有两种：

- `.github/copilot-instructions.md`：单个指令文件，会随该存储库的**每个**请求发送给 Copilot。此文件应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导的文件。

> [!NOTE]
> 其他指令格式及其支持情况会因环境而异。依赖特定格式前，请查阅[自定义指令支持参考][custom-instructions-support]。

## 探索项目中的自定义指令文件

入门项目已包含一组指令文件。先查看已有内容，再进行修改，了解指令产生的影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，注意其中简要的项目说明和编码指导。这些指令适用于 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。这里有针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意文件顶部的 `applyTo` 字段，它设置了决定适用文件范围的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它对其他指令文件和项目现有文件的链接。这样可以将较大的指令集拆分成较小的可复用文件，并指向供 Copilot 参考的示例。

## 更新指令文件以符合团队规范

现有文件已经提供了良好开端，但还缺少一项要求。现在修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，该部分应位于第 35 行附近。
2. 在该部分末尾添加以下项目符号：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库指令。保存更新后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，请输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认当前检出的分支为 `game-filters-cli`。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看已更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认它们准确说明代码。

## 总结和后续步骤

你了解了 Copilot CLI 如何获取指令文件中的上下文，并将新标准应用到功能中。具体来说，你：

- 了解了存储库指令和按路径限定的指令文件如何提供给智能体。
- 更新了指令文件，确保遵循编码标准。
- 了解了指令文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保以一致的方式运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建有效自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
