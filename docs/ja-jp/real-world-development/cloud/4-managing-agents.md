---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

この数回の演習では、ユーザーエクスペリエンスの改善や機能の追加を目的に、Copilot cloud agent に 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理するツールはいくつかあります。そのひとつが、GitHub.com の[エージェントページ][agents-page]です。このミッションコントロールでは、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。エージェントが行った操作を確認したり、進行中のセッションに追加の指示を送ったりできます。

このレッスンでは、次のことを行います。

- エージェントページで cloud agent のタスクを監視します。
- 進行中のセッションに指示を送り、機能の追加を依頼します。

## シナリオ

ハイコントラストモードを作成するようエージェントに依頼した後、チームはライトモードも追加するよい機会だと考えました。サイトのスタイル更新と切り替え機能の追加にすでに取り組んでいるため、同時に実装するのが合理的です。ハイコントラストモードだけでなく、ライトモードも追加するようエージェントに指示します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状況を確認しましょう。

1. [Copilot エージェントページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインのタスクリストを確認します。Copilot に割り当てたタスクとして、次の項目が表示されます。
    - コードのドキュメントを更新する
    - ゲームを変更する API を生成する
    - Web サイトにハイコントラストモードを追加する
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。次のような操作が含まれる場合があります。
    - リポジトリのチェックアウト
    - Copilot が作業する環境の作成
    - MCP サーバーの設定
    - 割り当てられたタスクを完了するための各種手順

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。ここでは PR と変更されたファイルを確認して、作業をさらに監視できます。

## cloud agent に追加の指示を送る

現在実行中のタスクを確認したので、ハイコントラストモードを作業中の Copilot にライトモードの切り替え機能も追加するよう依頼します。

1. ハイコントラストモードの追加に関するセッションを選択します。正確なタイトルは Copilot が付けた名前や作業状況によって異なります。

    ![ミッションコントロールのアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業を開始したことが表示されるまで、数分間セッションを確認します。作業開始後は、次のようなメッセージが表示されます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot が作業中のエージェントタスクに指示を送るダイアログを表示した画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを認識し、作業に取り込むことを確認します。

## Copilot に作業を続けてもらう

これまでと同様に、Copilot が更新されたタスクに取り組みます。メッセージを送信した時点で作業中の手順を終えると、新しい依頼を作業に取り込みます。

今回も数分かかります。その間に休憩して、ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視し、指示を送るための中心的な場所である Copilot エージェントページを確認しました。このミッションコントロールでは、実行中や完了済みのタスクを追跡し、作業内容を確認できます。また、進行中のタスクの範囲を調整したり、追加のガイダンスを与えたりできます。

次のことを学びました。

- ミッションコントロールとエージェントページで cloud agent のタスクを監視する方法
- 進行中のセッションに指示を送り、機能の追加を依頼する方法

Copilot がアクセシビリティ機能を実装している間に、Copilot が作成したプルリクエストを[反復的に改善する][next-lesson]方法を学びます。

## リソース

- [Copilot エージェントページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復的に改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
