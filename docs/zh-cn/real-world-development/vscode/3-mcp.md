---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义代理 →][next-lesson]

你刚刚使用代理模式构建了筛选功能。打开拉取请求之前，应先确认该功能在浏览器中确实有效。与其亲自逐步操作应用，不如连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能，然后发布分支并打开 PR。

在本练习中，你将：

- 了解什么是 Model Context Protocol (MCP)，以及 MCP 服务器如何为 Copilot 扩展工具；
- 将 Playwright MCP 服务器添加到工作区；
- 让 Copilot 使用 Playwright MCP 服务器在浏览器中手动测试筛选功能；
- 发布分支，并为筛选功能创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

当代理模式能够访问编辑器以外的工具时，它会更加强大。Copilot 通过 Model Context Protocol (MCP) 实现这一点：MCP 是一种让代理与外部工具和服务通信的标准方式。

![展示代理模式如何与上下文、LLM 和工具交互的示意图，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 代理提供了与外部工具和服务通信的方式。通过 MCP，AI 代理可以实时与外部工具和服务交互，访问最新信息（使用 resources），并代表你执行操作（使用 tools）。

AI 代理通过 MCP 服务器访问这些工具和资源。MCP 服务器在 AI 代理与外部工具和服务（例如现有 API 或 NPM 包等本地工具）之间充当桥梁，并负责管理二者之间的通信。每个 MCP 服务器都提供一组不同的工具和资源，供 AI 代理访问。

一些常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 仓库的 API。AI 代理可以借此创建仓库、更新现有仓库，以及管理 issues 和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力。AI 代理可以借此访问网页、填写表单和选择按钮。

还有许多其他 MCP 服务器提供不同的工具和资源。GitHub 托管了 [MCP 注册表](https://github.com/mcp)，便于发现生态系统中的项目并参与贡献。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前应仔细审查其源代码、验证发布者并考虑安全影响。只使用信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 检查 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区中可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你应该会看到已经配置好的 `github` 服务器：

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

项目模板中包含这个 `github` 条目，因此 Copilot 能在上一练习中读取 backlog issues。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装。VS Code 通过 HTTP 连接到它，并在 Copilot 首次使用其工具时要求你登录 GitHub。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，正好可以用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列。文件应如下所示：

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

与远程 `github` 服务器不同，`playwright` 是一个**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标志指示 Playwright 在没有可见窗口的情况下运行浏览器。codespace 中没有可显示浏览器的桌面环境，因此需要使用此标志。

> [!NOTE]
> Tailspin Toys 项目已经使用 Playwright 进行端到端测试，因此所需的浏览器通常已经安装。如果 Copilot 报告缺少浏览器，让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会按需启动 MCP 服务器：Copilot 首次需要某个服务器的工具时，VS Code 会启动它，并要求你确认是否信任。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它以启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任该服务器，请检查配置并选择信任，以便启动服务器。

手动启动是可选的。如果跳过这一步，VS Code 会在下一步 Copilot 首次需要服务器工具时启动它，并提示你确认是否信任。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供了一个可操作的真实浏览器。你无需亲自操作应用进行检查；代理可以打开网页、导航、应用筛选、读取结果，并向你总结观察结果。这样无需离开对话，就能快速确认功能是否符合预期。

Playwright MCP 服务器使用页面的[无障碍树][playwright-mcp-server]，而不是截图。这意味着代理根据结构化且带标签的元素（如按钮、链接和列表项）进行推理，与辅助技术读取页面的方式类似。因此，快速功能检查也能顺便进行简单的无障碍检查。

连接服务器并启动应用后，要求 Copilot 测试刚完成的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，逐步执行这些操作并汇报结果。对照 issue 中的验收标准检查它的总结。如果发现异常，可以继续提问，或要求它修复代码，然后再创建拉取请求。

> [!NOTE]
> 测试时应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请先重新启动，再发送提示词。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器；如果它报告缺少浏览器，让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR），让团队审阅。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板，审阅 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示所做更改的 Source Control 面板屏幕截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到仓库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面（CLI）等多种方式创建拉取请求。既然正在使用 GitHub Copilot，不妨让它来创建 PR。它可以找到相关 issue，并创建与之关联的 PR。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 在代理下拉列表中确认选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 让 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行获取信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会要求你登录 GitHub；按照提示完成操作。
5. 留意 Copilot 如何搜索 issues、找到相关 issue 并创建 PR。
6. 选择 Copilot 生成的链接，查看拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜！本练习中你：

- 了解了 Model Context Protocol (MCP)，以及 MCP 服务器如何为 Copilot 扩展工具；
- 将 Playwright MCP 服务器添加到工作区；
- 在发布前使用它通过浏览器手动测试筛选功能；
- 发布分支并为筛选工作创建拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义代理][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [What the heck is MCP and why is everyone talking about it?][mcp-blog-post]
- [Microsoft Playwright MCP 服务器][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
