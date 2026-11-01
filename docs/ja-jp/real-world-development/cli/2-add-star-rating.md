---
title: "レッスン 2 - 星評価を追加する: 手軽な改善"
description: "Copilot CLIでゲームカードに小さな変更を加え、転送されたブラウザーで確認し、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLIをインストールして会話を試したので、次はプロジェクトに初めて変更を加えます。変更は小さくしましょう。ゲームのデータにはすでに星評価がありますが、ホームページのゲームカードには表示されていません。エージェントに評価を表示させ、変更を確認して、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで、目的を絞ったCopilotの会話を開始します。
- エージェントにプロジェクトの小さな変更を依頼します。
- `/diff` で変更を確認します。
- アプリを実行し、転送されたブラウザーで変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toysの各ゲームには星評価を設定でき、ゲーム詳細ページにはすでに表示されています。一方、ホームページのゲームカードに表示されるのは、タイトル、カテゴリ、販売元、説明だけです。ウォームアップとして、エージェントに既存の評価を各カードへ表示させます。初めてのセッションにぴったりの、小さく独立した変更です。

## 会話の構成

**会話**とは、Copilot CLIを使ってタスクに取り組む場所です。Copilotアプリとは異なり、通常のCLI会話では、専用のワークツリーを作成するのではなく、ターミナルで現在チェックアウトしているリポジトリとGitブランチを使用します。保存した会話に戻って同じやり取りを続けられますが、ファイルとブランチはディスク上の通常のGit状態のままです。

会話では、プロンプトとエージェントの応答、ファイルを調査・編集する際のエージェントのツール操作、そして `/diff` で確認できる変更の3つを確認できます。

## 会話を開始して変更を依頼する

新しい会話を開始して、機能の実装に取りかかりましょう。

1. Codespaceに戻ります。
2. まだターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. まだCopilotを起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. スラッシュコマンド `/new` を実行し、<kbd>Enter</kbd> を押して新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilotはプロジェクトを調査し、ゲーム詳細の表示に使われているファイルを見つけて、必要なコードを作成します。これでCopilot CLIを使った新機能の追加は完了です。

## 差分を確認する

AIが生成した変更は、たとえ小さな変更でも、マージ前にすべて確認する必要があります。Copilot CLI上で変更を確認しましょう。

1. `/diff` を入力し、変更されたファイルをすべて確認します。
2. ゲームカードに、評価がある場合は数値が、`starRating` が `null` の場合は `No rating yet` が表示されることを確認します。
3. テストで両方の状態がカバーされていることを確認します。
4. Copilotが実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. 確認が終わったら、<kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> Copilotを含む生成AIツールは確率的に動作するため、生成されるコードは毎回異なる場合があります。特定の実装を期待するのではなく、動作を確認してください。

## 変更を確認する

もちろん、コードを読んだだけで動くと決めつけるわけにはいきません。CopilotにWebサイトを起動してもらい、Codespacesから転送されたブラウザーで更新後のユーザーインターフェイス (UI) を確認しましょう。

1. Copilotにアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespacesでポート `4321` が利用可能になったことが通知されたら、**Open in Browser** を選択します。
3. ゲームカードに5段階の評価が表示されることを確認します。
4. Copilotに戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

これで機能が完成しました。新しいコードをプロジェクトにマージするため、プルリクエスト (PR) を作成しましょう。

1. デフォルトエージェントに変更のコミットを依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLIはPR作成時に既存のコミットをプッシュし、PRのURLを表示します。
3. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、Copilot CLIに表示されたURLを選択してPRを開きます。
4. 変更されたファイルとチェック結果を確認します。
5. 問題がなければ、**Merge pull request** を選択し、マージを確定します。
6. Codespaceに戻り、`/exit` でCopilot CLIを終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

おめでとうございます。GitHub Copilot CLIを使って、初めての変更をリリースしました。このレッスンでは、次のことを行いました。

- 機能ブランチで、目的を絞ったCopilotの会話を開始しました。
- エージェントにゲームカードの小さな変更を依頼しました。
- `/diff` で変更を確認しました。
- アプリを実行し、転送されたブラウザーで星評価を確認しました。
- 最初のプルリクエストを作成してマージしました。

次は、[フィルタリングのIssueから始めて、PlanモードとAutopilotモードを使い][next-lesson]、より大きな機能を作成します。

## リソース

- [GitHub Copilot CLIについて][about-copilot-cli]
- [Copilot CLIコマンドリファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
