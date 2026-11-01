---
title: "练习 1 - 自定义说明（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就应确保它能够获取这些上下文。[说明文件][instruction-files]可以提供这些指导，让 Copilot 不仅了解你希望它做什么，也了解应该如何完成。

在本练习中，你将：

- 了解如何通过存储库自定义说明和按路径限定范围的说明文件，将项目专属上下文、编码指南和文档标准传达给 Copilot；
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI harness 不同，本练习不会使用“前后对比”提示。Copilot cloud agent 会在 GitHub issue 上异步工作，因此难以实时并排展示效果。之后查看 cloud agent 在本 harness 中生成的拉取请求时，你会看到说明文件的影响。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践指南和要求。这些要求包括：

- 数据层始终需要单元测试。
- UI 应使用深色模式，并具有现代感。
- 应以 TSDoc 文档注释的形式在代码中添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过使用说明文件，你可以确保 Copilot 获得正确的信息，并按照这些实践开展工作。

### 自定义说明

自定义说明可以为 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求。你可以借此引导 Copilot 提供更相关的建议和代码片段。例如，你可以指定偏好的编码约定、库，以及希望添加的注释类型。你可以为整个存储库创建说明，也可以为特定文件类型或任务创建说明。

说明文件有两种类型：

- `.github/copilot-instructions.md`：每次针对该存储库提出请求时，都会将这个唯一的说明文件发送给 Copilot。此文件应包含项目级信息，即 Copilot 收到的大多数聊天或 CLI 请求都适用的上下文，例如所用技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建这些文件。你可以用它们为特定语言（例如 TypeScript 或 Astro）提供指南，也可以指导创建 UI 组件或一组新的单元测试等任务。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时，都会使用存储库级说明文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [支持使用其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理说明文件的最佳实践

完整讨论如何创建说明文件超出了本 workshop 的范围。不过，示例项目展示了一种有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 中的内容限定为项目级指导，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供专门说明。
- 使用自然语言，确保指导清晰，并提供代码应如何编写（以及不应如何编写）的示例。

创建说明文件并没有唯一的方法，使用 AI 也是如此。通过试验，你会逐渐找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的说明文件。浏览此项目中的文件时，你可能会发现其中包含多种任务的说明，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮你生成说明文件。不同产品界面的操作方式各不相同（例如，VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）；当前产品界面的课程会在相关处说明。
>
> 想找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中收录了许多说明文件、自定义 agent 和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览此项目中的自定义说明文件

花些时间阅读此存储库附带的说明文件：其中有一个核心文件 `copilot-instructions.md`，还有一组用于各种任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web UI 中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简要说明以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下，留意嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中内容。注意这里有关于 Astro 文件、Drizzle 数据层、测试等方面的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于确定说明适用的文件。这里，任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会符合条件。
5. 留意此项目中有关创建单元测试的专门说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。留意其中指向其他说明文件（例如 `unit-tests.instructions.md`）以及项目现有文件的链接。这样可以将较大的说明集拆分成更小、可复用的文件，并为 Copilot 提供生成代码时可参考的示例。（这些路径相对于说明文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但尚未要求在代码中添加文档。接下来的步骤会添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来，你要做唯一一项编辑：添加一条项目级规则，要求以 TSDoc 文档注释和文件级注释头的形式在代码中添加文档。之后在本 harness 中分配 issue 时，cloud agent 会应用这条规则。

编辑之前，先创建一个分支（cloud agent 会读取 issue 目标分支中的说明，不过仍应单独提交这些编辑）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在来记录存储库的编码标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，它应该位于第 27 行附近。注意该部分记录了项目的编码标准，但尚未规定代码文档要求，这就是生成的 helper 没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建说明文件，以提供全局指导；也可以使用 `*.instructions.md` 文件针对特定语言、文件类型或任务提供说明。刚才添加的文档注释规则属于项目级标准，因此适合放在存储库级文件中。

## 提交、推送并合并说明更新

Copilot cloud agent 会读取 issue 目标分支中的说明文件。下一练习将 issue 分配给 Copilot 时，它会从 `main` 分支创建分支，因此必须先将说明更改合并到 `main`，cloud agent 才能使用这些说明。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并将其合并。下一练习中 cloud agent 处理分配给它的 issue 时，就会读取这些说明。

> [!TIP]
> 如果希望在本 workshop 中直接使用 `main` 分支，可以跳过创建分支的步骤，并直接提交到 `main`。此处使用分支是为了让 workshop 流程贴近实际项目中的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何读取此项目中的说明文件，并在 `.github/copilot-instructions.md` 中添加了一条新的存储库级标准。在本 harness 后续练习中，你会看到 Copilot cloud agent 生成的拉取请求如何应用这条标准。

接下来，[将第一个 issue 分配给 Copilot cloud agent][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 说明文件和其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
