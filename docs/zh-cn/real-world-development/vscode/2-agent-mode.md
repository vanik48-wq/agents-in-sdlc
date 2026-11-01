---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行操作，例如运行测试。作为开发者，你通常需要定位所有相关文件、进行修改、运行测试、调试、发现遗漏的文件，再次修改……这个过程会不断重复。

Copilot 代理模式正是为此而设计的。

Copilot 代理模式可以在 IDE 中更自主地工作。它的工作方式类似开发者：先探索现有项目结构，再执行必要的更新、运行测试等任务，并自动修复发现的问题。下面来看看如何使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和云端代理虽然名称相似，但适用于不同的工作方式。代理模式在 IDE 中执行任务，便于快速反馈和交互。云端代理则像团队中的协作开发者一样异步工作，并通过 issues 和拉取请求与你交流。

在本练习中，你将了解：

- Copilot 代理模式如何探索项目、识别相关文件并协调多处修改；
- GitHub Copilot 代理模式如何跨 UI 和数据层实现新功能；
- 将更改合并到代码库之前，如何审阅 Copilot 代理模式生成的更改和测试。

## 场景

随着游戏列表增长，你希望用户能够按类别和发布者筛选游戏。上一练习中已经添加了 publishers 辅助函数；现在将使用 Copilot 代理模式完成其余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

开始修改之前，先浏览 Tailspin Toys 网站，了解它目前的功能。

这是一个带有开发者主题的桌游众筹平台。用户可以列出游戏并查看游戏详情。该网站是一个单独的 Astro 应用，会在构建时将页面渲染为静态 HTML。页面在 frontmatter 中使用 Drizzle ORM 直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数放在 `src/lib/` 中；需要交互时，则通过使用标准 DOM API 的 Astro `<script>` 实现小范围交互。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一练习结束后，你应该已经回到 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），打开新终端。
3. 为筛选工作创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，提示你打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），再选择终端中的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端里的 localhost 链接时，会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 codespace 的私有隧道；你的 Web 服务器现在由 codespace 托管。

### 浏览网站

网站运行后，可以浏览它的功能。主要功能包括：

- **主页**：显示桌游列表，包括游戏标题、图片和描述。
- **游戏详情页**：选择游戏后会打开详情页，显示标题、描述、发布者和类别等信息。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始干净的对话。模式和模型会因练习而异；每一步会在适用时说明。

网站的初始实现已经可用，但我们想增加新功能。先查看待办事项 backlog。你从模板创建项目时，系统已自动创建了一组 issues；现在可以让 GitHub Copilot 列出这些事项。

1. 在 Chat 视图的代理下拉菜单中选择 **Agent**。**Agent** 会自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![显示 Chat 视图中代理选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并不偏好某个模型。编写本工作坊时使用的是 Claude Sonnet 4.5，因此在说明中列出了该模型，希望由此获得相对一致的代码建议和良好的体验。不过，LLM 的输出具有随机性，你收到的建议可能与工作坊中的说明不同。这完全正常，也在预期之内。

> [!NOTE]
> Copilot 可能会因 LLM 输出的随机性而使用不同的 MCP 命令，但仍应能够完成任务。

3. 向 Copilot 发送以下提示词，询问仓库中的 backlog：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行命令以列出所有 issues。
5. 查看生成的 issues 列表。

留意 Copilot 也按照它认为对用户最有用的程度，为这些事项确定了优先级。

## 审阅指令文件

在让代理生成代码之前，先查看将用于提供上下文的指令文件。接下来会使用[用户界面 (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) 文件，其中包含如何为网站添加功能的相关指导。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中关于添加功能的总体指导，包括：
   - 架构概览；
   - 组件设计、可测试性和无障碍原则；
   - 指向各类文件专用指令的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件和项目中的文件。路径相对于指令文件所在目录，因此可以复用内容、将大型指令集拆分成更小且更易管理的文件，并提供示例和模板。

## 实现筛选功能

完成筛选功能至少需要对应用做三项独立更新：

- 在数据层（`src/lib/`）添加或完善其余筛选逻辑；
- 添加或更新筛选行为的测试；
- 更新游戏列表页，加入筛选 UI。

此外，在合并到代码库之前，测试必须运行并通过。Copilot 代理模式可以代为完成这些任务。现在开始添加功能。

1. 可以继续当前的 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 **ui** 指令文件。

   ![选择 UI 指令文件的示例屏幕截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的代理下拉菜单中仍然选中 **Agent**。

   ![显示 Chat 视图中代理选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍然选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示词，让 Copilot 根据 backlog 中相关 issue 的要求实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并查找相关文件。它应该会找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂度，这可能需要几分钟。过程中，代码编辑器可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会有对应的 **Keep** 或 **Undo** 控件，因此无需在工作进行时逐项选择。

7. 按 Copilot 的提示选择 **Continue**，运行测试。

   ![Copilot Chat 面板中询问用户是否允许运行测试的对话框屏幕截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会出现短暂停顿，也可能看到测试暂时失败。这没有关系！Copilot 会在生成代码和运行测试之间反复处理，直到完成任务并且不再发现错误。

   ![Copilot 代理模式完整 Chat 会话的屏幕截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，留意潜在问题。

> [!CAUTION]
> 请记住，始终需要审阅 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能！
11. 确认一切正常并完成代码审阅后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加功能。你了解到：

- GitHub Copilot 代理模式如何跨 UI 和数据层实现新功能；
- Copilot 代理模式如何探索项目、识别相关文件并协调多处修改；
- 在将 Copilot 代理模式生成的更改合并到代码库之前，如何审阅更改和测试。

接下来，使用 [Playwright MCP 服务器][next-lesson] 测试功能，并为其创建拉取请求。

### 额外探索练习——实现分页

随着游戏列表增长，需要启用分页。运用本练习学到的技能，提示 Copilot 更新网站以实现分页。编写提示词时可以考虑：

- 遵循现有最佳实践，包括使用已有的指令文件；
- 决定分页的实现方式，例如是否允许用户选择页面大小，还是使用固定大小；
- 提供足够的指导，让 Copilot 按照你期望的方式实现；
- 可能需要与 GitHub Copilot 多次交互，要求修改并补充上下文。这是使用 Copilot 的正常流程。

## 资源

- [Copilot ask、edit 和代理模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
