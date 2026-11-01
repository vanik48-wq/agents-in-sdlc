---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能访问这些上下文。与 Copilot 共享特定上下文的方法有很多，其中一种重要方式是使用 [instruction files][instruction-files]，为代码生成提供指导。

在本练习中，你将：

- 了解如何通过仓库自定义指令和按路径匹配的指令文件，向 Copilot 提供项目上下文、编码规范和文档标准；
- 在现有指令生效时生成第一个筛选数据切片（publisher 辅助函数）；
- 向 `.github/copilot-instructions.md` 添加一项仓库级标准；
- 重新运行相同的提示词，观察生成的代码如何采用新标准；
- 将指令更新和筛选数据切片提交到 `main`，让 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不完全符合你设定的标准。Copilot 的输出具有随机性。本练习旨在观察更新指令前后的行为趋势，而不是要求输出逐字一致。

## 指令文件

### 场景

和优秀的开发团队一样，Tailspin Toys 也有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式，并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加注释块，说明文件的用途。

使用指令文件可以确保 Copilot 获得正确的信息，并按照这些实践执行任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和要求。你可以用它引导 Copilot 提供更相关的建议和代码片段，并指定编码约定、库，甚至希望代码包含的注释类型。你可以为整个仓库创建指令，也可以为特定文件类型提供任务级上下文。

指令文件有两种：

- `.github/copilot-instructions.md` 是一个指令文件，会随仓库中的**每个请求**发送给 Copilot。它应包含项目级信息，即大多数 Copilot Chat 或 CLI 请求都适用的上下文，例如技术栈、项目概述、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md` 可用于特定任务或文件类型，例如为 TypeScript 或 Astro 提供语言指导，或者为创建 UI 组件或单元测试提供任务指导。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时都会使用仓库级指令和带有 `applyTo` 前置元数据的 `*.instructions.md` 文件。
>
> 此外，Copilot [支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本工作坊的范围。不过，示例项目展示了一种有代表性的方法。总体来说：

- 让 `copilot-instructions.md` 专注于项目级指导，例如项目内容、结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，清晰表达指导，并提供代码应当如何编写或不应如何编写的示例。

创建指令文件没有唯一的方法，使用 AI 也是如此。通过实践，你会找到最适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的指令文件。浏览本项目时，你会发现其中包含多种任务的指令，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮你生成指令文件。不同界面提供此功能的方式各不相同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）。当前界面的课程会在相关位置介绍具体方式。
>
> 如果需要模板或起点，可以浏览 [awesome-copilot][awesome-copilot]；该仓库收录了指令文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义指令文件

花些时间阅读本仓库附带的指令文件：一个核心文件 `copilot-instructions.md`，以及一组适用于各种任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下，留意嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中内容。这里包含 Astro 文件、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它设置了相对于仓库根目录的 glob，用于确定指令适用的文件。这里，任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会命中。
5. 留意该文件中针对本项目单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。留意指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可复用的文件，并提供示例供 Copilot 参考。这里的路径相对于指令文件，而不是仓库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求添加代码文档。接下来，你会添加 TSDoc 文档注释和文件注释头的规则。

## 为本次修改创建分支

先创建一个分支来保存本次修改，这是一种良好的实践。

1. 返回上一练习中的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始干净的对话。模式和模型会因练习而异；每一步会在适用时说明。

要观察自定义指令的效果，先在当前指令文件生效时发送一条提示词。稍后更新指令后，再发送相同的提示词，比较结果。

1. 关闭之前练习中打开的编辑器标签页，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的代理下拉菜单中选择 **Agent**，让 Copilot 能够修改文件。

   ![显示 Chat 视图中代理选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示词：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会涉及辅助函数文件及其测试。
6. 留意建议的辅助函数是一个带类型的函数：第一个参数是 `db` 客户端，返回值是带类型的 publishers 数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 留意建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的输出具有随机性，因此它有时可能会在没有明确要求的情况下添加文档注释。这也没关系；本练习要观察的是更新指令后行为是否更加**一致**。

## 添加新的仓库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在，为仓库记录编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，约在第 27 行。留意这里记录了项目编码标准，但尚未规定代码文档要求，这也是生成的辅助函数没有文档注释的原因。
3. 在现有标准后添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，指令文件可以放在仓库级文件（`.github/copilot-instructions.md`）中提供全局指导，也可以放在 `*.instructions.md` 文件中为特定语言、文件类型或任务提供指导。像刚添加的文档注释规则这样的项目级标准，适合放在仓库级文件中。

## 重新运行提示词并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清空对话。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确的文件。
3. 发送与之前相同的提示词：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意建议的文件现在以类似下面的注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 留意建议的函数现在也包含类似下面的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将它应用到下一练习将继续使用的真实代码中。

## 提交指令并推送分支

指令文件和其他资源一样，也需要通过源代码管理进行维护。现在将指令更新提交并推送分支到仓库。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令和辅助函数更改都已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到仓库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送完成后，创建拉取请求，并将它关联到 backlog 中的文档标准 issue。可以手动完成，也可以让 Copilot 代劳：提示 Copilot 查找 issue、创建可关闭该 issue 的 PR，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上操作。项目模板已经将 GitHub 工具接入工作区，无需额外设置。Copilot 首次使用其中某个工具时，VS Code 可能会提示你登录 GitHub；按照提示完成操作即可。稍后的练习会介绍这种连接所用的 Model Context Protocol。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保在代理选择器中选中 **Agent**。
3. 让 Copilot 查找与更新指令文件相关的 issue，并从当前分支创建 PR。说明 PR 会关闭该 issue，同时也包含为后续筛选工作添加的 publishers 辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找 issue 并创建 PR。
5. Copilot 提示需要执行 GitHub 操作时，检查操作内容，并在适当时选择 **Allow**。
6. PR 创建后，要求 Copilot 合并 PR 并在本地切回 `main`，然后拉取最新代码。使用以下提示词：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 提示需要执行 GitHub 操作或 shell 命令时，检查命令并在适当时选择 **Allow**。

现在，你已经在 GitHub Copilot 的帮助下创建并合并了拉取请求！

## 总结和后续步骤

你探索了 Copilot 如何通过项目指令文件获取上下文，并在 VS Code 中使用 Copilot Chat 完成了以下工作：

- 发送代码生成提示词，观察 Copilot 在*现有*指令下生成的内容；
- 向 `.github/copilot-instructions.md` 添加仓库级标准；
- 重新运行相同的提示词，观察建议的代码如何采用新标准；
- 将指令更新和辅助函数基础提交到 `main`，供下一练习继续使用。

接下来，在[代理模式][next-lesson]中运用这些指令，让 Copilot 在整个代码库中实现新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 的个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
