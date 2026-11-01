---
title: "演習 2 - Copilot エージェントモードで新しい機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストを実行するなどの作業が必要です。開発者は通常、必要なファイルを探し、変更を加え、テストを実行し、問題をデバッグし、変更漏れのあったファイルを見つけて、さらに更新するという作業を行います。こうした作業は次々に発生します。

ここで役立つのが Copilot のエージェントモードです。

Copilot のエージェントモードは、IDE 内でより自律的に作業できるように設計されています。開発者と同様に、まず既存のプロジェクト構成を調べ、必要な更新を行い、テストなどのタスクを実行して、見つかった問題を自動的に修正します。エージェントモードを使ってサイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる利用体験を提供します。エージェントモードは IDE 内で動作するため、すばやいフィードバックと対話が可能です。cloud agent は、issue やプルリクエストを通じてやり取りするチームメンバーのように、非同期で作業するピアプログラマーとして設計されています。

この演習では、次の方法を学びます。

- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを特定し、複数ファイルにわたる変更を行う方法。
- GitHub Copilot エージェントモードで UI とデータレイヤーにわたる新機能を実装する方法。
- コードベースにマージする前に、GitHub Copilot エージェントモードが生成した変更とテストを確認する方法。

## シナリオ

ゲームの一覧が増えるにつれて、ユーザーがカテゴリや出版社で絞り込めるようにしたいと考えています。前の演習では出版社ヘルパーを追加しました。次に Copilot エージェントモードを使い、残りのデータレイヤー、UI、テストを仕上げます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握しましょう。

この Web サイトは、開発者向けのテーマを持つボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示できます。このサイトは単一の Astro アプリで、ビルド時にページを静的 HTML としてレンダリングします。ページはフロントマターで Drizzle ORM を使ってローカルの SQLite データベースを直接クエリします。独立したバックエンド API やクライアントサイド UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、インタラクティブな機能は標準の DOM API を使う小さなスコープ付き Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. codespace に戻ります。前の演習の後、`main` に戻っているはずです。
2. <kbd>Control</kbd>+<kbd>`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押して、Codespace 内に新しいターミナルウィンドウを開きます。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を示すバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログボックスが表示された場合は、**x** を選択して閉じます。

5. ターミナルで Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd>（Mac）または <kbd>Ctrl</kbd>（Windows/Linux）を押しながら選択して Web サイトを開きます。

> [!NOTE]
> codespace で Codespace ターミナルの localhost URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは codespace で Web サーバーをホストするためのプライベートトンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認できます。主な機能は次のとおりです。

- **ホームページ**: ボードゲームのタイトル、画像、説明を一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると詳細ページが開き、タイトル、説明、出版社、カテゴリなどの情報が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

現在の Web サイトは機能しますが、新しい機能を追加して改善したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトをセットアップしたときに issue のバックログが自動的に作成されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェントのドロップダウンから **Agent** を選択します。**Agent** は、ファイルにわたる変更の計画と実装、ターミナルコマンドの実行、ツールの呼び出しを自律的に行います。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨しているわけではありません。このワークショップを作成した際に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案されるコードにある程度一貫性を持たせ、快適に学習できるようにするためです。ただし、LLM の出力は確率的であるため、ワークショップに記載された内容とは異なる提案が表示される場合があります。これは通常のことであり、想定されています。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使う場合がありますが、タスク自体は完了できるはずです。

3. 次のプロンプトを Copilot に送信して、issue のバックログについて尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```
4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot が、ユーザーにとって最も役立つと思われる項目を優先順位付けしたことも確認してください。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、作業のコンテキストを Copilot に伝える指示ファイルを確認しましょう。Web サイトに機能を追加する方法が記載された [ユーザーインターフェイス (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) ファイルを使用します。

1. codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の進め方に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティに関する原則。
   - ファイル種別ごとの指示ファイルへのリンク。対象には次のものがあります。
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指示ファイルから他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所からの相対パスです。この機能を使うと、指示を再利用しやすく、小さく管理しやすい単位に分割したり、参考になる例やテンプレートを示したりできます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも次の 3 つを別々に更新する必要があります。

- データレイヤーのフィルタリングロジックを追加または調整する（`src/lib/`）。
- フィルタリングの動作を確認するテストを追加または更新する。
- ゲーム一覧ページを更新してフィルタリング UI を導入する。

さらに、コードベースにマージする前にテストを実行し、成功することを確認する必要があります。Copilot エージェントモードを使えば、これらの作業を実行できます。機能を追加してみましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントのドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き **Claude Sonnet 4.5** に選択されていることを確認します。
5. バックログの関連 issue に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、必要な機能に関連するファイルを探す様子を確認します。データレイヤーのヘルパー、UI、テストを見つけた後、ファイルの変更とテストの実行を開始します。

   ![Copilot がプロジェクトファイルを調べている画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行など、複数のタスクを実行します。タスクやコードベースの複雑さによっては、数分かかる場合があります。作業中は、エディターに **Keep** や **Undo** ボタンが表示されることがあります。Copilot が完了すると、すべての変更に対して **Keep** または **Undo** が表示されます。作業中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat ペインにテスト実行の確認ダイアログが表示された画面](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。問題ありません。Copilot はタスクを完了し、エラーがないと判断するまでコード生成とテストを繰り返します。

   ![Copilot エージェントモードとのチャットが完了した画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ず確認することが重要です。

10. Web サイトを開いているブラウザーに戻り、新しい機能を試します。
11. すべてが正しく動作し、コードの確認が完了したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot エージェントモードを使って Tailspin Toys の Web サイトに新機能を追加する方法を確認しました。次の方法を学びました。

- GitHub Copilot エージェントモードで UI とデータレイヤーにわたる新機能を実装する方法。
- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを特定し、複数ファイルにわたる変更を行う方法。
- コードベースにマージする前に、GitHub Copilot エージェントモードが生成した変更とテストを確認する方法。

次は[Playwright MCP サーバー][next-lesson]で機能をテストし、プルリクエストを作成しましょう。

### ボーナス演習 – ページネーションを実装する

ゲームの一覧が増えると、ページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションを実装するよう依頼してみましょう。次の点を検討してください。

- 既存の指示ファイルを使うなど、既存のベストプラクティスに従います。
- ページサイズをユーザーが選択できるようにするか、固定にするかなど、ページネーションの実装方法を検討します。
- 希望する実装に必要なガイダンスを含めてプロンプトを作成します。
- Copilot に変更を依頼し、コンテキストを追加しながら何度かやり取りする必要がある場合があります。これは Copilot を使う際の通常の進め方です。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
