---
title: "练习 5 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义代理][previous-lesson] |
|:--|

启用 GitHub Copilot 的代理模式后，它会自主探索代码库、提出更改、编辑文件并运行命令。由于这些工作会在你的计算机上实时进行，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以直接审阅每个 diff，接受或拒绝单项更改，也可以通过后续提示词引导对话，在不离开编辑器的情况下完善或扩展工作。

在本练习中，你将：

- 实时监控无障碍代理的工作；
- 审阅并接受建议的文件编辑；
- 在会话进行时引导代理添加浅色模式切换。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义代理正在实现高对比度模式。工作进行时，设计团队又提出一项需求：他们还希望为觉得默认主题太暗的用户添加浅色模式切换。与其从头开始新对话，不如引导现有会话扩展工作。

## 监控 Copilot 的进度

Copilot Chat 视图会显示代理执行的每个步骤，包括它探索的文件、运行的命令和提出的编辑。每项文件编辑都会以内联 diff 形式显示在聊天中，并提供 **Keep** / **Undo** 控件。

1. 返回练习 4 中的 Copilot Chat 视图。
2. 代理工作时，滚动聊天记录，查看每次工具调用。你会看到文件读取、文件编辑，以及它请求运行的终端命令。
3. 代理提出文件编辑时，聊天中会显示 diff：
    - 选择 **Keep** 接受更改；
    - 选择 **Undo** 撤销更改，让代理重新尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看代理在本次会话中修改的所有文件。

> [!NOTE]
> 如果工具调用要求授权（例如运行终端命令），代理会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待代理完成高对比度模式。完成后应会看到一条总结所做更改的消息。

## 引导代理添加浅色模式

高对比度模式完成后，继续同一对话，添加浅色模式切换。由于仍在同一个聊天会话中，代理会保留之前工作的上下文，包括修改过的组件、使用的样式方案和设置的本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示词：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 代理会接着之前的工作继续探索，并提出添加浅色模式的修改。
3. 像之前一样，逐项审阅新编辑，并对每个 diff 选择 **Keep** 或 **Undo**。
4. 如果代理的方向不符合预期，例如试图以你不喜欢的方式重构切换 UI，可以选择聊天输入框旁的 **Stop** 按钮停止任务，然后发送澄清性的后续消息。

> [!TIP]
> 引导正在进行的会话比从头开始更快。可以用后续提示词要求调整（“简化 CSS——使用 CSS 变量，而不是重复选择器”）、纠正方向（“不要添加第三个切换按钮；将浅色模式整合到现有设置面板中”）或扩展范围（“也为两个切换项添加键盘快捷键”）。

## 审阅工作集

提交之前，快速检查代理修改的全部内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审阅所有已更改文件的列表。你应该会看到 Astro 组件、样式和相关测试的更新。
3. 打开几个已更改的文件并查看 diff。确认代理采用了无障碍模式，包括 ARIA 属性、键盘导航、语义化 HTML 和通过本地存储持久保存设置。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课会介绍如何[发布这些更改][next-lesson]。

## 总结和后续步骤

你使用 Copilot Chat 的代理模式监控并引导了一个正在进行的会话：逐项审阅文件编辑、接受或拒绝更改，并通过后续提示词扩展工作，同时保留原有上下文。

本练习介绍了：

- 实时监控代理的工作；
- 接受或拒绝单项文件编辑；
- 在会话进行时通过后续提示词引导代理。

在[下一课][next-lesson]中，你将把无障碍功能作为拉取请求发布，并审阅本实验中本地自定义代理生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用代理模式][agent-mode]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
