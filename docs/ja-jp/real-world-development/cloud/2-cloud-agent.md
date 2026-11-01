---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残ったままの機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行します。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。これにより、面倒な作業を任せて開発プロセスを加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次のことを学びます。

- コード生成環境をカスタマイズする方法
- 安全に操作を実行する方法
- Issue の範囲を明確に定めることの重要性
- Copilot に Issue を割り当てる方法

## シナリオ

Tailspin Toys には、解決したい技術的負債があります。サイトの初期バージョンを作成した委託先がドキュメントを十分に整備しておらず、実質的にドキュメントがまったくありません。まず、アプリケーションのすべてのエクスポートされた関数に TSDoc のドキュメントコメントを追加したいと考えています。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属するほかのゲーム、つまり関連ゲームを表示して、ユーザーがゲームを見続けられるようにします。まだ仕上げられた実装である必要はなく、UX の受け入れテストに利用できるものを求めています。この作業は現在のブロッカーですが、ほかに優先度の高い Issue があります。

どちらも、優先度が下がりやすいタスクの例です。Copilot cloud agent に割り当てると、開発者が別の作業に集中している間に非同期で処理できます。その後、戻って Copilot の作業をレビューし、期待どおりか確認できます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。人間の開発者に作業を依頼する場合と同様に、[GitHub Issue を Copilot に割り当てる][assign-issue]など、いくつかの方法で依頼できます。割り当てると、Copilot は進行状況を追跡するドラフトプルリクエストを作成し、環境をセットアップして、タスクに取り掛かります。作業中でも完了後でも、Copilot のセッションを詳しく確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 適切な範囲の指示が重要

GitHub Copilot に魔法はありません。考える手間を省けるわけではありません。一見簡単そうな操作でも、詳しく見ると複雑さが明らかになります。そのため、[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階的に進め、学び、試し、必要に応じて調整しましょう。生成 AI を利用しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と、それが Copilot をどのように導くかを確認しました。カスタム指示の演習をすでに行った場合は、`.github/copilot-instructions.md` の効果を見たことがあるでしょう。まだの場合は、ここで簡単に確認しましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに用意された指示ファイルを読み取り専用で確認して、後でその影響を見分けられるようにします。

リポジトリの GitHub Web UI、または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc のドキュメントコメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターを確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで cloud agent に `Code lacks documentation` Issue を割り当てたら、生成されたプルリクエストで TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーを確認してください。これらは指示ファイルによるものです。後の演習で PR をレビューする際にも、改めて確認します。

## Copilot cloud agent の開発環境を設定する

誰がコードを書く場合でも、通常は環境を整え、状態を良好に保つためのセットアップスクリプトを実行する必要があります。Copilot にタスクを割り当てる場合も同様です。Copilot は SWE と同じような方法で作業します。

cloud agent は GitHub Actions を作業環境として使用します。作業を始める前に実行する[専用のセットアップワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に作成すると、環境をカスタマイズできます。これにより、必要な開発ツールや依存関係を利用できるようになります。このラボの流れを円滑にし、学習の機会を設けるために、すでに設定してあります。このワークフローは、単一の Astro アプリの Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションとシードを適用した SQLite データベースを Copilot が利用できるようにします。

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

ほかの GitHub Actions ワークフローと同じように見えますが、いくつか重要な点があります。

- `copilot-setup-steps` という単一のジョブが含まれています。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーに注目してください。リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot の実行を待たずに、ワークフローが正常に動くかテストするのに便利です。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、多くのプロジェクトでは情報が古かったり、ドキュメントがなかったりします。この種の技術的負債は放置されがちで、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメントの作成を得意とするため、Copilot cloud agent に割り当てるのに適した Issue です。Copilot はバックグラウンドで必要なドキュメントを作成します。後の演習で作業をレビューします。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てのダイアログを開きます。

  ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。新しい PR が一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が Todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の Todo リストが PR に表示されるまで数分かかる場合があります。Copilot は環境を作成し（前述のワークフローを実行します）、プロジェクトを分析して、問題に取り組むための最適な方法を検討しています。

13. Todo リストと、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下へスクロールし、Copilot が Issue の作業を始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されない場合は、ウィンドウを更新してください。

16. ライブセッションをスクロールして、Copilot がどのように問題を解決しているかを確認します。コードを調べて状態を理解する様子、計画を考えて決定するために一時停止する様子、コードを作成する様子などを確認できます。

作業には数分かかるでしょう。Copilot cloud agent の主な目的のひとつは、タスクを非同期で実行し、私たちがほかの作業に集中できるようにすることです。この機能を活用するため、もうひとつタスクを Copilot cloud agent に割り当てた後、アプリケーションへの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームのセクションを追加する

前に説明したように、GitHub Copilot cloud agent の大きな利点のひとつは作業を分担できることです。デザインチーム向けに関連ゲームのセクションを追加する作業は長時間かからないかもしれませんが、その時間をほかのタスクに使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使用します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次のテキストを入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てのダイアログを開きます。

  ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業中であることを示す 👀 が Issue の最初のコメントに表示されます。

![Copilot が Issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent は SWE と同じような方法で作業するため、積極的に監視する必要はありません。完了後にレビューすればよいのです。次は、カスタムエージェントの作成と使用に取り組みます。

## まとめと次のステップ

このレッスンでは、[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に Issue を割り当てると、非同期で作業を依頼できます。技術的負債の解消、新しい機能の作成、あるフレームワークから別のフレームワークへのコード移行などに Copilot を活用できます。

次のことを学びました。

- コード生成環境をカスタマイズする方法
- 安全に操作を実行する方法
- Issue の範囲を明確に定めることの重要性
- Copilot に Issue を割り当てる方法

cloud agent がバックグラウンドで作業を進める間に、カスタムエージェントの作成と使用に取り組みます。[Copilot cloud agent は MCP サーバー][cloud-agent-mcp]と、前のモジュールで学んだカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
