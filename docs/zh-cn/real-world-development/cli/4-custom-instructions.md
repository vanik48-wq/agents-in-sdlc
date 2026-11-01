---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "查看存储库指令，添加文档规范，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，应让 Copilot 能够访问相关指导。[指令文件][instruction-files]不仅说明您需要什么代码，还说明代码应如何组织。现在您已构建筛选功能，接下来将查看 Copilot 使用的指令、添加文档规范并应用到代码中。

本课将介绍如何：

- 了解存储库指令和按路径限定的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码规范。
- 查看指令文件对代码的影响。

## 场景

Tailspin Toys 制定了一系列开发规范和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果有可注入的 `db` 参数，也要说明。
- 可复用的 Astro 组件应说明其 `Props` 接口契约；相关代码更改时，注释也应保持最新。
- 应保留现有格式和 lint 指南。

使用指令文件可确保 Copilot 获得正确的信息，并按照这些规范完成任务。

## 指令文件

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解您的编码风格和要求。您可以指定首选编码规范、库以及注释风格。指令既可以适用于整个存储库，也可以仅针对特定文件类型或任务。

指令文件分为两类：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。内容应包括适用于大多数聊天或 CLI 请求的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指南的文件。

> [!NOTE]
> 其他指令格式及其支持情况因环境而异。依赖特定格式前，请查看[自定义指令支持参考][custom-instructions-support]。

## 查看项目中的自定义指令文件

入门项目中已包含一组指令文件。先查看现有文件，再进行更改，以了解更改的影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指南。这些指令适用于本存储库中的每次 Copilot 交互。
4. 打开 `.github/instructions` 文件夹并查看其中的文件，包括针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它定义了指令适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向的其他指令文件和项目文件。这种方式可将大型指令集拆分成较小的可复用文件，并为 Copilot 提供参考示例。

## 更新指令文件以符合团队规范

现有文件是不错的起点，但仍有不足。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，该部分应位于第 35 行附近。
2. 在该部分末尾添加以下项目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指导

Copilot CLI 会在每次启动对话时加载存储库指令。保存更新后的指令后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新指令。

1. 如果筛选对话仍处于打开状态，请输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选对话，并确认检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，查看已更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认它们准确说明了代码。

## 总结和后续步骤

您已了解 Copilot CLI 如何读取指令文件，并将新规范应用到功能中。具体来说，您：

- 了解了存储库指令和按路径限定的指令文件如何传递给智能体。
- 更新了指令文件，确保遵循编码规范。
- 观察了指令文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
