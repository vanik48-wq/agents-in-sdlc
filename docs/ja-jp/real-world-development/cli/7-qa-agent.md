---
title: "レッスン 7 - QAエージェントを作成して使う"
description: "Issueの要件、quality-checksスキル、Playwright MCPを組み合わせたQAカスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに `quality-checks` スキルで自動チェックを実行し、Playwright MCPでブラウザー上のフィルタリング操作を確認しました。次は、明確なQAプロセスを持つカスタムエージェントに、これらの機能をまとめます。

このレッスンでは、次のことを行います。

- カスタムエージェントが指示、スキル、MCPツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QAエージェントを選択し、フィルタリングのIssueに基づく結果を確認します。

## シナリオ

Tailspin Toysでは、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザー上の動作を一貫して確認したいと考えています。カスタムエージェントを使うと、そのQAプロセスを調整し、再利用可能なレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントとは、Markdownプロファイルで定義する、Copilotの特定用途向けの設定です。プロファイルには、エージェントの目的、指示、利用できるツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` にQAの役割を定義し、Copilot CLIで選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を示し、`quality-checks` スキルは繰り返し実行できるチェックをまとめます。Playwright MCPはブラウザーツールを提供します。QAプロファイルは、これらの機能を使って要件を評価し、結果を報告する方法をCopilotに指示します。QAプロファイルはそれらを置き換えるものではなく、別の会話も必要ありません。

## QAプロファイルを作成する

機能のPRを開く前に、再利用可能なQAプロファイルを作成するようCopilotに依頼します。このプロファイルには、QAで行うチェックと守るべき境界の両方を定義します。

1. Codespaceに戻り、フィルタリングの会話がInteractiveモードであることを確認します。
2. デフォルトエージェントに、新しいカスタムエージェントの作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilotが意図したQAの流れと制約をプロファイルに反映したことを確認します。これにより、確認だけが目的なのに、プロファイルが不十分だったり、エージェントの動作範囲が広すぎたりすることを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを確認します。`description` は必須です。`name` は任意ですが、含めるとエージェントの表示名が分かりやすくなります。
3. プロファイルの指示を読み、QAが要件の確認から始め、リポジトリの指示に従い、`quality-checks` スキルを実行し、Playwright MCPを使うことを確認します。
4. QAが根拠を示して報告し、実装コードを変更する前に許可を求め、変更をコミットしたりプルリクエストを開いたりしないことを確認します。
5. これらの役割や制約が不足していた場合は、先に進む前にデフォルトエージェントに修正を依頼します。

## Issueに基づいてQAを実行する

Copilot CLIは会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成したら、同じフィルタリングの会話を再開し、QAを選択します。そうすることで、Issueと計画時の決定を引き継いで確認できます。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成したばかりの場合、利用可能なエージェントの一覧に表示されないことがあります。それでも、上記のコマンドで有効にできます。

2. 次のプロンプトを使って、QAエージェントに機能を確認してもらいます。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QAエージェントが作業を開始します。
4. 作業が完了したら、レポートを確認します。

## まとめと次のステップ

再利用可能な専門エージェントを追加し、その作業を確認しました。このレッスンでは、次のことを行いました。

- カスタムエージェントが指示、スキル、MCPツールと連携する方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QAエージェントを選択し、フィルタリングのIssueに基づく結果を確認しました。

これで、実装、スキルの更新、QAプロファイル、テスト、検証レポートの準備ができました。次は、[これらを機能PRにまとめ、Agent Mergeを使います][next-lesson]。

## リソース

- [Copilot CLI用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
