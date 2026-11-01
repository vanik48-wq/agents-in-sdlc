---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 VS Code 练习前，需要先做好准备。你将创建 Tailspin Toys 仓库的副本，启动一个 [codespace][codespaces] 进行练习，并确认 GitHub Copilot Chat 已在编辑器中正常运行。

## 设置实验室仓库

要创建用于存放代码的仓库副本，需要从[模板][template-repository]创建一个实例。新仓库包含实验所需的全部文件，你将在整个练习过程中使用它。

1. 在新的浏览器窗口中，导航到 [Tailspin Toys 模板][tailspin-template]。
2. 在实验仓库页面上选择 **Use this template** 按钮，创建自己的仓库副本。然后选择 **Create a new repository**。

    ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你是在由 GitHub 或 Microsoft 主办的活动中参加本工作坊，请遵循导师提供的说明。否则，可以在你有 GitHub Copilot 访问权限的组织中创建新仓库。

    ![填写仓库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下新仓库的路径（**organization-or-user-name/repository-name**），后续练习会用到它。

> [!NOTE]
> **待办事项已准备就绪**
>
> 从模板创建仓库时，系统会自动为你创建一组 GitHub issues。整个工作坊都会使用这些 issues，因此无需自行创建。

## 创建 codespace

接下来，你将使用 codespace 完成实验练习。

[GitHub Codespaces][codespaces] 是一种基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码。它提供功能齐全的 IDE，支持多种编程语言、扩展和工具。

1. 导航到刚创建的仓库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 按钮创建新的 Codespace。

    ![创建新的 codespace](../../../_images/ex0-create-codespace.png)

创建 codespace 需要几分钟，不过这仍比手动安装所有服务快得多。等待期间，可以探索 GitHub Copilot 的其他功能；接下来我们会介绍这些功能。

> [!CAUTION]
> 后续练习还会用到这个 codespace。目前请将它保留在浏览器标签页中，不要关闭。

> [!NOTE]
> 本工作坊可在 codespace 或本地 [dev container][dev-containers] 中运行。两者都包含顺利完成练习所需的全部前置条件。如果想在本地运行，请在 VS Code 中打开克隆的仓库，并在出现提示时选择 **Reopen in Container**。VS Code 将构建与 codespace 相同的 dev container。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 使用 GitHub Copilot Chat 和代理模式

要使用 GitHub Copilot Chat 的代理模式，需要在 IDE 中安装 GitHub Copilot Chat 扩展。如果使用 GitHub Codespace，通常已经安装了该扩展。

> [!TIP]
> 如果尚未安装 GitHub Copilot Chat 扩展，可以从 [Visual Studio Code Marketplace][copilot-chat-extension] 安装。也可以在 Visual Studio Code 中打开扩展视图，搜索 **GitHub Copilot Chat**，然后选择 **Install**。

安装扩展后，可能还需要使用 GitHub 账户进行身份验证，才能启用它。

1. 返回 codespace。
2. 如果编辑器右侧没有显示 Copilot Chat，请选择 codespace 顶部的 **Copilot Chat** 图标。
3. 在 Copilot Chat 窗口中输入类似 “Hello world” 的消息，然后按 Enter。这会激活 Copilot Chat。
4. 如果尚未完成身份验证，系统会提示你登录 GitHub 账户。按照说明完成身份验证。

    ![Copilot Chat 身份验证提示示例](../../../_images/ex1-copilot-authentication.png)

5. 完成身份验证后，Copilot Chat 窗口应该会出现。

## 总结

恭喜，你已经创建了实验仓库的副本！你也启动了 codespace 的创建流程，稍后将用它开始编写代码。

## 下一步

现在开始让 Copilot 工作吧。继续阅读[练习 1 - 自定义指令][next-lesson]，学习如何向 Copilot 介绍项目约定。

## 资源

- [GitHub Codespaces 概览][codespaces]
- [从模板创建仓库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
