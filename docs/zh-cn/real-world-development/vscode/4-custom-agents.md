---
title: "练习 4 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中的特定任务或领域定制的 AI 助手。你可以通过仓库 `.github/agents` 文件夹中的 Markdown 文件定义代理，为 Copilot 提供专门的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定工作。团队可以把专业知识整理成可复用的代理，例如确保符合 [WCAG][wcag] 的无障碍代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

自定义代理由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在全局位置 `~/.copilot/agents` 定义。每个文件都包含 YAML frontmatter，其中至少有 `name` 和 `description` 字段；之后是定义代理行为、专业能力和指令的 Markdown 提示词。

### 自定义代理与代理技能的区别

自定义代理和[代理技能][agent-skills-concept]有一些相似之处。两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义代理**是工作者，**技能**是工具。

自定义代理拥有独立的上下文窗口，能够协调技能（甚至其他代理）来完成工作。在本实验中，无障碍自定义代理会根据无障碍指南审查并更新网站；执行过程中，它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 编写自定义代理没有唯一的“正确”方式。和 AI 的其他用法一样，需要不断测试和迭代，找到适合你的环境和场景的方法。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不论用户的视觉能力或偏好如何。近期用户反馈指出，当前深色主题中的文本与背景对比度不足，导致部分用户难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍功能至关重要，因此你希望尽快实现它。接下来将使用自定义代理生成此功能。

在本练习中，你将：

- 查看已有的无障碍自定义代理；
- 在 Copilot Chat 中使用无障碍代理实现高对比度模式。

## 查看无障碍自定义代理

项目已经为你创建了一个无障碍自定义代理。现在查看其内容，了解它如何指导 Copilot。

返回 codespace，打开终端，并从 `main` 创建一个新分支，用于无障碍功能开发（Exercise 3 中的筛选 PR 将保留在单独的分支上）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 留意 YAML frontmatter 中的 `name` 和 `description` 字段。

> [!CAUTION]
> 自定义代理必须在 frontmatter 中包含 `name` 和 `description`。

3. 接着浏览后续部分，留意以下内容：
   - 为无障碍网站生成代码时的核心职责；
   - 无障碍最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见陷阱和错误。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Copilot Chat 视图底部的代理下拉菜单中显示 `.github/agents` 下定义的所有自定义代理。选择自定义代理后，聊天会话将使用该代理的指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始干净的对话。模式和模型会因练习而异；每一步会在适用时说明。

1. 如果代理下拉菜单中尚未选中 **Agent**，请先选择它。

   ![显示 Chat 视图中代理选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 选择聊天视图底部的代理下拉菜单（它会显示当前代理；默认是 **default**）。
3. 从可用代理列表中选择 **Accessibility agent**。
4. 向无障碍代理发送以下提示词：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作：探索代码库、提出更改并编辑项目文件。每项编辑都会以内联 diff 的形式显示在聊天中，并标明文件路径，供你审阅。
6. 随着编辑逐步完成，**Files changed** 指示器会更新，显示代理在本次会话中修改的文件。

> [!NOTE]
> 这个过程可能需要几分钟。Copilot 会实际修改仓库中的文件，例如 Astro 组件、CSS 和相关测试。

下一练习将介绍如何审阅并引导这项进行中的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，即针对特定任务和领域定制的 AI 助手。使用自定义代理可以将团队的专业知识和标准整理成可复用的代理，指导 Copilot 更有效地完成特定类型的工作。

你了解了：

- 如何定义自定义代理；
- 如何在 Copilot Chat 的代理模式中使用自定义代理。

接下来，[监控并引导代理的工作][next-lesson]：在工作进行时审阅更改，并在同一会话中添加浅色模式切换。

## 资源

- [About custom agents][custom-agents]
- [Creating custom agents in your IDE][creating-custom-agents-ide]
- [Custom agents in VS Code][custom-agents-vscode]
- [Custom agents configuration][custom-agents-config]
- [Custom agents on awesome-copilot][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
