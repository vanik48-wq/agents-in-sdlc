---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要なものを準備します。Tailspin Toys リポジトリの自分用コピーを作成し、[Codespace][codespaces] を起動します。ここでは指示ファイルを編集し、cloud agent が作成した作業をレビューします。

## ワークショップ用リポジトリを準備する

演習で使うコードのコピーを作成するため、[テンプレートリポジトリ][template-repository]から新しいリポジトリを作成します。新しいリポジトリには、演習に必要なファイルがすべて含まれています。

1. 新しいブラウザーウィンドウで、[Tailspin Toys テンプレート][tailspin-template]に移動します。
2. ラボ用リポジトリのページで **Use this template** を選択し、続けて **Create a new repository** を選択して、自分用のコピーを作成します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

    ![リポジトリテンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス（**organization-or-user-name/repository-name**）をメモします。ラボの後半で使用します。

## Codespace を作成する

次に、Codespace を使ってラボの演習を進めます。

[GitHub Codespaces][codespaces] は、ブラウザー上でコードの記述、実行、デバッグができるクラウド開発環境です。さまざまなプログラミング言語、拡張機能、ツールに対応した高機能な IDE を利用できます。

1. 作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する画面](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい Codespace を作成する画面](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、サービスを手作業でインストールするよりもはるかに短時間で済みます。作成中に GitHub Copilot の機能を確認してみましょう。次の演習から使い始めます。

> [!CAUTION]
> 今後の演習で Codespace に戻ります。今はブラウザーのタブを閉じずに、そのまま開いておいてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で実行するように構成されています。どちらも、演習をスムーズに進めるために必要な前提条件を満たします。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択してください。Codespace と同じ dev container がビルドされます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ラボ用リポジトリのコピーを作成し、Copilot cloud agent と一緒に作業する Codespace の作成を開始しました。

## 次のステップ

次に、cloud agent が従うカスタム指示を追加します。[演習 1 - カスタム指示][next-lesson]に進みましょう。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の開始方法][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
