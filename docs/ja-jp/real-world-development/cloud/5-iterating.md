---
title: "演習 5: GitHub Copilot の作業を反復的に改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザーエクスペリエンスの改善や機能の追加を目的に、GitHub Copilot にいくつかのタスクを依頼しました。コードへのドキュメント追加、デザインチームが改善を重ねられる関連ゲーム機能の作成、ハイコントラストモードとライトモードの切り替えなどのアクセシビリティ機能の実装を依頼しました。ここでは、コードの変更内容を確認し、必要に応じて Copilot にフィードバックを送って作業を改善してもらいます。その後、チームのほかのメンバーにもレビューを依頼します。

### シナリオ

これまでに何度も説明したように、生成 AI を利用してもソフトウェア設計と DevOps の基本は変わりません。生成されたコードを常にレビューし、通常の DevOps プロセスに沿って作業を進めます。チームにレビューを依頼する前に、ドキュメントの作成、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した内容を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期かつ監督なしでタスクを実行するため、安全を確保するための制約が設けられています。次のような制約があります。

- Copilot に付与されるのはリポジトリへの読み取りアクセスと、コード変更用のブランチへの書き込みアクセス**のみ**です。
- cloud agent は GitHub Actions 内で実行され、作業用に分離された一時的な環境を作成します。
- GitHub Actions のワークフローは、人間が承認するまで実行されません。
- [既定では外部リソースへのアクセスが制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーも対象です。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent がドキュメントを追加するために作成した最初のプルリクエスト (PR) を確認します。標準的な GitHub.com の PR インターフェイスを使って作業します。

> [!NOTE]
> PR を確認する際、GitHub Copilot がファイアウォールでブロックされているという警告が表示される場合があります。これは想定どおりです。Copilot は、外部 MCP サーバーへの呼び出しなど、既定では外部リソースへのアクセスが制限されています。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com でリポジトリを開きます。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルのプルリクエストを開きます。実際のタイトルは多少異なる場合があります。

> [!NOTE]
> Copilot が作業中の場合、プルリクエストのタイトルに **[WIP]** が付いています。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあります。その間に休憩したり、これまでに学んだことを振り返ったりしてください。

4. **Files changed** タブを選択し、変更内容を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 更新されたコードを確認します。新しい TSDoc のドキュメントコメントや、そのほかのドキュメントが含まれています。変更内容は状況によって異なります。

    変更を確認しながら、TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーに注目してください。これらは演習 2 の最初に確認したカスタム指示ファイルによるものです。

6. 更新内容に問題がなければ、**Conversation** タブに戻って下へスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄で、ワークフローがキューに追加されたことを確認します。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業するのは、一方通行ではありません。チームメンバーと同じように、プルリクエストやコードのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、別のセッションを開始して対応します。結果は予測できないため、必ずこう依頼すべきという文面はありません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、ファイルの役割を簡潔に説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. **@copilot** をメンションして、生成されたドキュメントへの変更を依頼します。上記の例を使うか、コードベースに必要なドキュメントについて別の変更を依頼します。
2. **View Session** を選択して、Copilot が作業する様子を確認します。更新に対応するため、新しいセッションが始まります。
3. **Back to pull request** を選択して、プルリクエストに戻ります。

    ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、プルリクエストに新しいコミットが追加されます。
5. **Files changed** タブを選択し、変更内容を確認します。

満足するまで、必要に応じて何度でも改善を依頼してください。納得できたら、ドラフト PR をレビュー可能な状態に変更して、`main` ブランチにマージできます。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue を解決するために Copilot が作成した PR を確認します。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。実際のタイトルは多少異なる場合があります。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容に問題がなければ、**Conversation** タブに戻って下へスクロールします。
6. ワークフローの承認待ちを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄で、ワークフローがキューに追加されたことを確認します。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストすることもできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名（例: **copilot/fix-8**）に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent に繰り返し更新を依頼できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーと TSDoc のドキュメントコメントを追加したりできます（この作業はカスタム指示を更新する前に依頼したことを思い出してください）。前の PR と同様に、**Conversation** タブでコメントすると、Copilot がその内容を確認し、別のセッションで更新を開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用のカスタムエージェントで実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 のミッションコントロールで追加を依頼したライトモードの両方が含まれているはずです。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。実際のタイトルは多少異なる場合があります。

> [!NOTE]
> Copilot が作業中の場合、プルリクエストのタイトルに **[WIP]** が付いています。その場合は、Copilot が作業を完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注目します。
   - モード切り替え用の UI コンポーネント
   - ユーザー設定を保持するためのローカルストレージの使用
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更
   - アクセシビリティ属性（ARIA ラベル、キーボード操作など）
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 更新内容に問題がなければ、**Conversation** タブに戻って下へスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄で、ワークフローがキューに追加されたことを確認します。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストすることもできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイコントラストモードとライトモードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認してください。

カスタムアクセシビリティエージェントが、Copilot にアクセシビリティのベストプラクティスに沿った機能を実装するよう導いたことに注目してください。アクセシビリティに関する懸念や改善点があれば、前の PR と同じように、コメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 委任を続ける

cloud agent に実際のバックログ項目を任せれば、自分は別の作業に集中できます。この習慣を身につけるために、リポジトリにいくつか Issue を作成して Copilot に割り当ててみましょう。たとえば、次のようなタスクがあります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent のハーネスを完了しました。これらのレッスンでは、次のことを行いました。

- **このリポジトリに用意されたカスタム指示ファイルを確認し、後から cloud agent の出力に及ぼす影響を確認しました。**
- **Copilot cloud agent に Issue を割り当て、非同期で環境を設定し、計画して、作業を進める様子を確認しました。**
- **アクセシビリティ用のカスタムエージェントを作成して使用し、ハイコントラストモードとライトモードを追加しました。**
- **Copilot エージェントページをミッションコントロールとして使用し、アクセシビリティのセッションを監視して途中で指示を送りました。**
- **cloud agent が作成したプルリクエストをレビューして改善し、`@copilot` に更新を依頼し、ワークフローを承認しました。**

## レビューと次のステップ

Cloud agent のハーネスを完了しました。さらに学びたい場合は、ほかのハーネスも役立ちます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat のエージェントモードや MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェントスキル、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent との任意の `/delegate` 連携などを使って、ターミナルからレビュー済みの変更を 2 件作成します。

自分のリポジトリでは、次のようなアイデアを試してみましょう。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てる。
- 別の分野のカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
