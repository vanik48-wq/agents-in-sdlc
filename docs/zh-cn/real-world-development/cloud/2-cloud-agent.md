---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

很少有组织完全不受技术债务困扰。技术债务可能是尚未解决的安全问题、需要更新的旧代码，也可能是因缺少时间而长期搁置的功能请求。GitHub Copilot cloud agent 可以自主执行代码更新和功能添加等任务。agent 完成工作后，会生成一份草稿 PR，供开发者审核。这样可以把繁琐任务交给 agent，加快开发进程，让开发者专注于更重要的工作。

你将使用 Copilot cloud agent 探索以下内容：

- 自定义代码生成环境；
- 确保操作安全；
- 明确限定 issue 范围的重要性；
- 将 issue 分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受聘创建网站的承包商没有留下理想的文档——事实上，你会发现项目完全缺少文档。首先，他们希望为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队已准备好改进游戏发现体验。他们希望每款游戏的详情页都显示相关游戏，也就是同一类别中的其他游戏。暂时不需要完善的实现，只需要一个可用于验收测试 UX 的版本。目前这项工作受阻，但其他 issue 的优先级更高。

这两项工作都可能很快被降低优先级，因此很适合交给 Copilot cloud agent。Copilot cloud agent 可以异步处理这些任务，让开发者先专注其他工作，之后再回来检查 Copilot 的成果并确认一切符合预期。

## 介绍 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像人工开发者一样在后台执行任务。与人工开发者协作类似，可以通过多种方式启动任务，包括[将 GitHub issue 分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求来跟踪进度、设置环境并开始处理任务。你可以在 Copilot 会话进行中或完成后查看会话。建议方案准备好供你审核时，Copilot 会在拉取请求中标记你。

## 明确限定任务范围的重要性

使用 GitHub Copilot 并没有捷径——你仍然需要思考。看似简单的操作，深入分析后也会发现其复杂性，因此[为 Copilot cloud agent 明确限定任务范围][cloud-agent-best-practices]非常重要。把它当作 AI 结对编程助手：分阶段工作、学习、试验并逐步调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义说明

之前的练习介绍了自定义说明及其如何指导 Copilot。如果你已经完成自定义说明的动手练习，就会看到 `.github/copilot-instructions.md` 的实际效果；如果还没有，现在可以快速查看一下。将工作分配给 Copilot cloud agent 之前，先只读查看此存储库中已有的说明文件，这样之后就能留意它们的作用。

在 GitHub Web UI 中打开以下文件，或者在已启动的 codespace 中打开：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将这些说明限定为适用于 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` issue 分配给 cloud agent 时，留意生成的拉取请求是否包含 TSDoc 文档注释、TypeScript 约定和文件注释头——这些内容来自上述说明文件。之后的练习会再次提醒你检查这些内容。

## 为 Copilot cloud agent 设置开发环境

无论由谁编写代码，通常都需要特定环境，并运行一些设置脚本，确保环境状态良好。将任务分配给 Copilot 时也一样，因为它会以类似软件工程师（SWE）的方式执行任务。

Cloud agent 在 GitHub Actions 中运行。你可以通过创建[专用设置工作流][setup-workflow]来自定义环境，该工作流配置在 `.github/workflows/copilot-setup-steps.yml` 文件中，并在 agent 开始工作前运行。这能让它访问所需的开发工具和依赖项。为便于 workshop 流程并提供学习机会，我们已预先配置好环境，确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及为单个 Astro 应用迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

这看起来与其他 GitHub 工作流文件类似，但有几点值得注意：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，会先在 GitHub Actions 中运行此作业。
- 留意 `workflow_dispatch` 触发器，它允许你从存储库的 **Actions** 选项卡手动运行工作流。这有助于测试工作流是否运行成功，而无需等待 Copilot 启动它。

## 添加文档

大家都知道文档很重要，但多数项目的信息要么过时，要么完全缺失。这类技术债务常常得不到处理，进而降低工作效率，也让代码库维护和团队新成员入职变得更加困难。幸运的是，Copilot 很擅长生成文档，因此这正是交给 Copilot cloud agent 的好任务。它会在后台生成所需文档。后续练习中，你会回来检查它的工作。

1. 在新的浏览器标签页中，打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新建 issue 对话框。
4. 选择 **Blank issue**，新建 issue。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求（PR），其标题类似于 `[WIP]: Code lacks documentation`。如果列表中没有出现新的 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应该会创建一个待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会显示在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看列表和即将执行的任务。
14. 向下滚动拉取请求时间线，你应该会看到 Copilot 已开始处理 issue 的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 你可能需要刷新窗口才能看到更新后的状态指示器。

16. 查看实时会话，了解 Copilot 如何解决问题，包括探索代码并了解当前状态、暂停思考并决定合适的计划，以及创建代码。

这可能需要几分钟。Copilot cloud agent 的主要目标之一是异步执行任务，让我们能专注其他工作。接下来，我们会利用这一点：先把另一项任务分配给 Copilot cloud agent，再转而编写代码为应用添加功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot cloud agent 的一大优势是可以分配不同的工作，让你专注一组任务，而 agent 同时处理另一组任务。即使为设计团队添加相关游戏部分不会花太多时间，这些时间也可以用于其他工作。现在就把它交给 Copilot cloud agent 吧！

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新建 issue 对话框。
4. 选择 **Blank issue**，使用空白模板。
5. 将 **Title** 设置为：`Show related games on the game details page`
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久之后，你应该会在 issue 的第一条评论中看到一组 👀，表示 Copilot 已开始处理！

![Copilot 使用眼睛表情表示正在处理 issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正认真处理你的第二项请求！Copilot cloud agent 的工作方式类似 SWE，因此你不需要主动监控，而是在完成后进行审核。接下来，我们将开始创建和使用自定义 agent。

## 总结和后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。使用 cloud agent，可以将 issue 分配给 Copilot，让它异步处理任务。你可以用 Copilot 解决技术债务、创建新功能，或协助将代码从一个框架迁移到另一个框架。

你探索了以下内容：

- 自定义代码生成环境；
- 确保操作安全；
- 明确限定 issue 范围的重要性；
- 将 issue 分配给 Copilot。

Cloud agent 正在后台处理任务，我们现在可以开始创建和使用自定义 agent。[Copilot cloud agent 也可以使用 MCP 服务器][cloud-agent-mcp]，并且可以读取自定义说明；我们在之前的模块中已经介绍过自定义说明。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义 agent →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
