---
title: "レッスン 8 - 機能PRを作成してマージする"
description: "フィルタリング、指示、スキルの更新、QAプロファイル、テストをまとめて確認し、PRを作成してAgent Mergeを使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、すべて1つのブランチに保存されています。まとめて確認し、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にマージの作業を任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QAエージェントをいくつかのPRに分けます。ワークショップを簡潔にするため、フィルタリングと品質確認の作業は1つの会話とブランチにまとめ、このPRに含めました。

このレッスンでは、次のことを行います。

- Agent Mergeとは何か、マージのライフサイクルをどのように自動化するかを学びます。
- 機能全体の変更と検証の根拠を確認します。
- フィルタリングのPRを作成します。
- 確認が済んでからAgent Mergeを有効にし、PRがマージされたことを確認します。

## シナリオ

フィルタリング機能の開発では、Copilotを使って計画、実装、検証を行いました。Tailspin Toysでは、マージの許可を開発者が管理しながら、PRに必要な残りの作業を自動化したいと考えています。

## Agent Mergeの概要

**Agent Merge**は、プルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、CopilotはPRを妨げている問題に対処します。たとえば、失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要な場合のリベースなどを行い、リポジトリで許可されていればGitHubの自動マージを有効にします。

これまでは、自分で **Merge pull request** を選択していました。Agent Mergeにその作業を任せることもできます。Agent Mergeを有効にする前に、作業を確認し、問題がないことを判断してください。

## Agent MergeでPRを管理する

すべてのコードができたので、まとめて確認し、PRを作成して、残りの作業をAgent Mergeに任せましょう。

1. Codespaceに戻ります。
2. `/agent` を入力して、エージェントダイアログを開きます。
3. 選択肢から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って、新しいPRを作成します。
5. `/pr agentmerge` を使って、Agent Mergeを有効にします。
6. CopilotはPRの継続的インテグレーションの進行状況を監視します。すべて成功したら、マージを実行します。
7. 「PR #14 was squash-merged successfully.」などのメッセージがCopilotから表示されたことを確認します。

> [!IMPORTANT]
> Agent Mergeは、必須の承認、ブランチ保護、マージキュー、リポジトリの設定、権限の不足を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されていれば、確認済みの手順で手動マージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストのプロセスなど、開発プロセスのいくつかを自動化しました。このレッスンでは、次のことを行いました。

- Agent Mergeとは何か、マージのライフサイクルをどのように自動化するかを学びました。
- 機能全体の変更と検証の根拠を確認しました。
- フィルタリングのPRを作成しました。
- 確認が済んでからAgent Mergeを有効にし、PRがマージされたことを確認しました。

次は、コンテキスト、モデル、共有、オプションのクラウドへの委任に使える[便利なCopilot CLIスラッシュコマンドを確認します][next-lesson]。

## リソース

- [Copilot CLIでプルリクエストを管理する][manage-prs]
- [Copilot CLIコマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
