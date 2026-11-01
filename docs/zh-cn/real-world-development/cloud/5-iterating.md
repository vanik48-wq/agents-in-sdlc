---
title: "练习 5：迭代 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理 agent][previous-lesson] |
|:--|

## 查看工作成果

在本实验中，你与 GitHub Copilot 一起完成了多项任务，旨在改进用户体验并添加功能。你让 Copilot 为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现包括高对比度和浅色模式切换在内的无障碍功能。现在，我们来查看代码更改；如有需要，还可以向 Copilot 提供反馈，改进其工作。

### 场景

如前所述，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审核生成的代码，并遵循常规 DevOps 流程。基于这一点，在允许团队其他成员开始审核之前，先来检查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能提出的建议。

## 安全性和 GitHub Copilot cloud agent

由于 Copilot cloud agent 会在无人监督的情况下异步执行任务，因此我们设置了一些安全限制，以确保整个过程安全：

- Copilot 对存储库只有读取权限，对用于存放其代码的分支才有写入权限。
- Cloud agent 在 GitHub Actions 中运行，并会创建一个独立的临时环境来工作。
- GitHub Actions 工作流必须经人工批准后才能运行。
- [默认情况下，外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 查看生成的文档

先查看 GitHub Copilot cloud agent 生成的第一个拉取请求（PR），其中为代码添加了文档。你将使用 GitHub.com 上标准的 PR 界面完成此任务。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括调用外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot cloud agent 的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似于 **Add missing documentation** 或更完整标题的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]** 标记。此时请等待 Copilot 完成工作。可能需要几分钟，可以趁此休息一下，或回顾目前学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并查看更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新更新的代码，其中包括新生成的 TSDoc 文档注释和其他文档。具体更改会有所不同。

    浏览更改时，留意 TSDoc 文档注释、TypeScript 约定和注释头。这些内容来自你在练习 2 开始时查看的自定义说明文件。

6. 查看完更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到有工作流正在等待批准的提示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. 你应该会看到工作流在拉取请求的 checks 部分排队。如果一切正常，单个 Astro 应用的项目检查就会通过。可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 在拉取请求上协作并非单向过程。你可以像标记团队其他成员一样，在拉取请求评论或代码行内评论中标记 Copilot。Copilot 会读取这些评论，并启动新的会话来处理请求。由于结果具有非确定性，我们无法提供固定的提示文本。你可以请求 Copilot 更新以下内容：

- 在每个代码文件顶部添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用的结构。

1. 添加评论，请求修改生成的文档，并像标记任何用户一样标记 **@copilot**。可以使用上面的任一建议，也可以提出其他希望在代码库中看到的文档改进建议。
2. 选择 **View Session**，观察 Copilot 执行修改。留意 Copilot 如何启动新会话来更新内容。
3. 选择 **Back to pull request**，返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应该会出现新的提交。
5. 选择 **Files changed** 选项卡，查看更改。

你可以继续迭代，直到满意为止。满意后，可以将 PR 从草稿转换为准备审核状态，然后合并到 main 分支。

![将 PR 转为准备审核状态](../../../_images/ex7-ready-for-review.png)

## 查看相关游戏功能

接下来，查看 Copilot 为解决“在游戏详情页显示相关游戏”这一 issue 创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Show related games on the game details page** 或更完整标题的 PR。
4. 选择 **Files changed** 选项卡，查看生成的代码。
5. 查看完更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
6. 你应该会看到有工作流正在等待批准的提示。
7. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
8. 你应该会看到工作流在拉取请求的 checks 部分排队。如果一切正常，单个 Astro 应用的项目检查就会通过。可能需要几分钟。
9. **可选：**你也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```
   
Copilot 已经构建了相关游戏功能！与之前一样，你可以迭代使用 Copilot cloud agent 并请求更新。例如，你可能希望调整显示的相关游戏数量，或要求添加注释头和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义说明！）。和之前一样，你可以在 **Conversation** 选项卡添加评论来提出请求，Copilot 会读取评论并启动新会话。

## 查看无障碍功能

最后，查看使用无障碍自定义 agent 实现的无障碍功能。此 PR 应该同时包含你在练习 3 中分配的高对比度模式，以及你在练习 4 的 mission control 中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Add high contrast mode to website** 或更完整标题的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]** 标记。此时请等待 Copilot 完成工作，可能需要几分钟。

4. 选择 **Files changed** 选项卡，查看生成的代码。
5. 查看实现，重点关注以下内容：
   - 用于切换模式的 UI 组件；
   - 用于保存用户偏好的 local storage；
   - 高对比度模式和浅色模式的 CSS 或样式更改；
   - 无障碍属性（ARIA 标签、键盘导航等）；
   - 用于管理模式切换的 JavaScript/TypeScript 代码。

6. 查看完更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到有工作流正在等待批准的提示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. 你应该会看到工作流在拉取请求的 checks 部分排队。如果一切正常，单个 Astro 应用的项目检查就会通过。可能需要几分钟。
10. **可选：**你可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，并在浏览器中测试高对比度和浅色模式切换，确认功能按预期运行，并且在重新加载页面后仍会保留设置。

留意无障碍自定义 agent 如何帮助 Copilot 按照无障碍最佳实践实现这些功能。如果发现任何无障碍问题或改进点，可以像处理之前的 PR 一样，在评论中标记 **@copilot** 并请求更新。

## 可选练习 — 继续委派任务

Cloud agent 最适合处理实际待办事项，让你能转而专注其他工作。可以向存储库提交一些 issue 并分配给 Copilot，逐步养成这种习惯。以下是一些想法：

- 在游戏详情页创建支持者兴趣表单。
- 在游戏列表页实现分页。
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你已完成 Cloud agent harness。在这些课程中，你：

- **查看了此存储库自带的自定义说明文件**，以便之后了解它们对 cloud agent 输出的影响。
- **将 issue 分配给 Copilot cloud agent**，并观察它如何设置环境、制定计划并异步执行任务。
- **创建并使用了自定义 agent** 来处理无障碍问题，并添加高对比度和浅色模式切换。
- **使用 Copilot agents 页面作为 mission control**，监控并引导正在进行的无障碍会话。
- **审核并迭代了 cloud agent 创建的拉取请求**，通过标记 `@copilot` 请求修改并批准工作流。

## 回顾和后续步骤

你已完成 Cloud agent harness。其他 harness 可以补充你在这里练习的内容，继续探索：

- 🖥️ **[VS Code harness](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat agent 模式和 MCP 集成。
- 💻 **[CLI harness](../../cli/)** — 在终端中通过 Plan 和 Autopilot 模式、agent skills、自定义 QA agent、Playwright MCP、Agent Merge，以及可选的 `/delegate` cloud agent 桥接，交付两项经过审核的更改。

在自己的存储库中，可以尝试以下后续练习：

- 将重构或测试覆盖率 issue 分配给 cloud agent。
- 为其他领域创建自定义 agent。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot agents][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理 agent][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
