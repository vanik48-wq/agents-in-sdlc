---
title: "练习 3 - 自定义 agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义 agent？

GitHub Copilot 中的[自定义 agent][custom-agents-concept]是针对开发工作流中特定任务或领域定制的专用 AI 助手。你可以在存储库的 `.github/agents` 文件夹中通过 Markdown 文件定义 agent，为 Copilot 提供专注的说明、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识整理成可复用的 agent，例如负责遵循 [WCAG][wcag] 要求的无障碍 agent、遵循安全编码实践的安全 agent，或维护一致测试模式的测试 agent。

自定义 agent 通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少包含 `name` 和 `description`，后面接一段定义 agent 行为、专业知识和说明的 Markdown 提示。

### 自定义 agent 与 agent skills 的比较

自定义 agent 和 [agent skills][agent-skills-concept]在概念上有些重叠。两者主要通过 Markdown 文件定义，都会告诉 AI 如何执行操作。最简单的区分方式是：**自定义 agent 是执行者，skills 是工具**。

自定义 agent 拥有自己的上下文窗口，能够协调 skills（甚至其他 agent）来完成工作。在本实验中，无障碍自定义 agent 会根据无障碍指南审核并更新网站；在执行过程中，它可以调用拉取请求工作流 skill 或运行和管理测试的 skill。

> [!NOTE]
> 编写自定义 agent 没有唯一的“正确”方式。与 AI 的其他应用一样，应在自己的环境和场景中测试并不断迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

你将使用自定义 agent 探索以下内容：

- 如何定义自定义 agent；
- 如何将任务分配给自定义 agent。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视力能力或偏好的影响。近期用户反馈指出，由于文本与背景颜色对比度不足，一些用户觉得当前深色主题难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍至关重要，因此你希望尽快实现此功能。接下来会使用自定义 agent 生成相关功能。

## 查看无障碍自定义 agent

我们已经为你创建了一个无障碍自定义 agent。先查看它的内容，了解它将如何指导 Copilot。

返回 codespace，然后查看无障碍自定义 agent 文件：

1. 打开 `.github/agents/accessibility.md`。
2. 留意其中包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义 agent 必须包含带有 `name` 和 `description` 的 frontmatter。

3. 接着浏览后续部分，其中包括：
   - 为无障碍网站生成代码时的核心职责；
   - 无障碍最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见陷阱和错误列表。

## 创建并分配 issue

Mission control 是你在环境中使用所有 agent 的中心位置。在这里，你可以将任务分配给 Copilot cloud agent、监控任务，还可以重定向并为正在进行的会话提供更多指导。现在，先将实现高对比度模式的任务分配给 Copilot。

1. 打开你的存储库。
2. 选择 issues 选项卡。
3. 选择 **New issue**，打开新建 issue 对话框。
4. 选择 **Blank issue**，新建 issue。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义 agent 列表中选择 **Accessibility agent**。

    ![cloud agent 分配界面中突出显示自定义 agent 和 Accessibility agent 的截图](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 就会在后台开始处理任务！

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义 agent][custom-agents]，它们是针对特定任务和领域定制的专用 AI 助手。使用自定义 agent，可以将团队的专业知识和标准整理成可复用的 agent，从而指导 Copilot 更有效地完成特定类型的工作。

你探索了以下内容：

- 如何定义自定义 agent；
- 如何将任务分配给自定义 agent。

Copilot 正在实现高对比度模式。接下来，我们将前往 mission control，[监控并引导 agent 会话][next-lesson]。

## 资源

- [关于自定义 agent][custom-agents]
- [为在组织中使用自定义 agent 做准备][org-custom-agents]
- [为在企业中使用自定义 agent 做准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理 agent →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
