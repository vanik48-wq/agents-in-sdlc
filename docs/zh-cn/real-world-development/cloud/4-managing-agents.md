---
title: "练习 4 - 监控和管理 agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot cloud agent 处理了三项独立任务，用于改进用户体验和添加功能。Cloud agent 虽然可以异步自主运行，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的 [agents 页面][agents-page]。在这个 mission control 中，你可以查看所有有未关闭拉取请求（PR）的 agent 任务、探索 agent 执行的操作，甚至可以引导正在进行的会话。

在本课中，你将：

- 浏览 agents 页面，监控 cloud agent 任务；
- 引导正在进行的会话，请求添加其他功能。

## 场景

将任务分配给 agent 来创建高对比度模式后，团队意识到也可以顺便添加浅色模式。既然已经开始更新网站样式并添加切换功能，将这项功能一并纳入也很合理。现在，你希望引导 agent 的工作，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot cloud agent 任务

查看当前分配给 Copilot cloud agent 的所有任务状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 留意主窗格和左侧窗格中的任务列表。你应该能看到分配给 Copilot 的任务，包括：
    - 更新代码文档；
    - 生成用于修改游戏的 API；
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的任务，其中可能包括：
    - 从存储库检出代码；
    - 创建 Copilot 的工作环境；
    - 设置 MCP 服务器；
    - 执行完成所分配任务所需的各种步骤。

> [!NOTE]
> 具体显示哪些步骤会因 Copilot 的工作状态和采用的方法而异。

4. 另外，留意右侧出现的拉取请求（PR）窗格。你可以在这里查看 PR 和已更改的文件，以便进一步监控。

## 引导 cloud agent

现在已了解当前正在执行的任务，接下来请 Copilot 在实现高对比度模式的同时，也添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示设置已完成并开始执行任务。看到类似下方的消息时，就表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![agents 页面中的 cloud agent 任务截图，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务！发送消息时，它正在执行的步骤完成后，就会将新请求纳入工作流程。

和之前一样，这需要几分钟。你可以趁此暂停片刻，回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，它是监控和引导 GitHub Copilot cloud agent 任务的中心位置。在这个 mission control 中，你可以跟踪所有正在进行和已完成的任务、查看 agent 执行的工作，还可以重定向正在进行的任务以调整范围或提供更多指导。

你探索了以下内容：

- 使用 mission control 和 agents 页面监控 cloud agent 任务；
- 重定向正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能。接下来，我们将开始[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义 agent][custom-agents]

---

| [← 上一课：自定义 agent][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
