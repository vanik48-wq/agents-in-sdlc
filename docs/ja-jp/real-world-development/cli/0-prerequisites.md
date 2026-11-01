---
title: "レッスン 0 - 前提条件"
description: "Tailspin Toys の自分用コピーを作成し、Copilot CLI ワークショップ用の GitHub Codespace を準備します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI のレッスンを始める前に、必要なものを準備します。Tailspin Toys リポジトリの自分用コピーを作成し、[Codespace][codespaces] を起動します。次のレッスンでは、その統合ターミナルを使って Copilot CLI をインストールし、実行します。

このレッスンでは、次のことを行います。

- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成します。
- Codespace を作成し、プロジェクトの準備ができていることを確認します。

## ワークショップ用リポジトリを準備する

Tailspin Toys プロジェクトの自分用コピーを使って作業します。[テンプレートリポジトリ][tailspin-template]から今すぐ作成しましょう。新しいリポジトリには、ワークショップに必要なファイルがすべて含まれています。

1. 新しいブラウザーウィンドウで、[Tailspin Toys テンプレート][tailspin-template]に移動します。
2. **Use this template** を選択し、続けて **Create a new repository** を選択して、自分用のコピーを作成します。
3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。
4. 作成したリポジトリのパス（`organization-or-user-name/repository-name`）をメモします。ワークショップの後半で使用します。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub Issues のバックログも自動的に作成されます。ワークショップではこの Issue を使って作業します。自分で Issue を登録する必要はありません。

ワークショップのテンプレートは新しく作成してください。リポジトリの指示、アプリケーションコード、テスト、`quality-checks` スキル、バックログが含まれています。以前作成したコピーを使う場合は、必要なファイルが揃っていることをファシリテーターに確認してください。

## Codespace を作成する

続いて、Codespace を使ってワークショップを進めます。

[GitHub Codespaces][codespaces] は、ブラウザー上でコードの記述、実行、デバッグができるクラウド開発環境です。さまざまなプログラミング言語、拡張機能、ツールに対応した高機能なエディターを利用できます。

1. 作成したリポジトリに移動します。
2. **Code** を選択します。
3. **Codespaces** タブを選び、**Create codespace on main** を選択します。
4. Codespace のセットアップが完了するまで待ちます。テンプレートがプロジェクトの依存関係、Playwright Chromium、ローカルデータベースをインストールします。
5. **Do you trust the authors of the files in this folder?** と表示されたら、**Trust Folder & Continue** を選択します。
6. リポジトリのルートでターミナルを開き、アプリケーションを起動します。

   ```bash
   npm run dev
   ```

7. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択し、Tailspin Toys のサイトが読み込まれることを確認します。
8. ターミナルに戻り、<kbd>Ctrl</kbd>+<kbd>C</kbd> を押して開発サーバーを停止します。

## まとめと次のステップ

準備ができました。このレッスンでは、次のことを行いました。

- Tailspin Toys プロジェクトの自分用コピーを作成しました。
- Codespace を作成し、プロジェクトの準備ができていることを確認しました。

次は [GitHub Copilot CLI のインストール][next-lesson]に進み、GitHub アカウントで認証します。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の開始方法][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
