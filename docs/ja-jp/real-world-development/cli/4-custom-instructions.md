---
title: "レッスン 4 - カスタム指示でCopilotを導く"
description: "リポジトリの指示を確認し、ドキュメントの標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成AIを使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合、その指針をCopilotが参照できるようにします。[指示ファイル][instruction-files]では、必要なコードだけでなく、コードの構成方法も説明できます。フィルタリングを作成したので、Copilotが使った指示を確認し、ドキュメントの標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパスで対象を限定した指示ファイルが、エージェントにどのように適用されるかを確認します。
- コーディング標準に従うよう、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toysには、開発プラクティスに関するガイドラインと要件があります。具体的には、次のとおりです。

- コメントでは、コードの言い換えではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明するTSDoc/JSDocを記述します。該当する場合は、注入可能な `db` 引数も含めます。
- 再利用可能なAstroコンポーネントには `Props` の契約を記述します。また、関連コードが変更されたときはコメントも最新の状態に保ちます。
- 既存のフォーマットとLintのガイダンスを維持します。

指示ファイルを使うと、Copilotに適切な情報を提供し、ここで取り上げたプラクティスに沿ってタスクを実行できるようになります。

## 指示ファイル

カスタム指示を使うと、コンテキストや好みをCopilotに伝え、コーディングスタイルや要件を理解しやすくできます。コーディング規約、ライブラリ、含めたいコメントの種類などを指定できるため、より適切な提案やコードスニペットを得られます。リポジトリ全体に適用する指示ファイルも、特定のファイルやタスクに適用する指示ファイルも作成できます。

指示ファイルには、次の2種類があります。

- `.github/copilot-instructions.md`: リポジトリに関するすべてのリクエストでCopilotに送られる、単一の指示ファイルです。多くのリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md`: 特定の言語、ファイルの種類、タスクに適用するガイドラインです。

> [!NOTE]
> ほかの形式の指示ファイルへの対応状況は、環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

変更の影響を確認するため、まずスタータープロジェクトに含まれている指示ファイルを見てみましょう。

1. Codespaceに戻ります。
2. Codespacesエディターで (ターミナルではなく)、`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングのガイダンスを確認します。この指示は、このリポジトリでCopilotを使うすべての操作に適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astroファイル、Drizzleデータレイヤー、テストなどの指示が含まれています。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、指示の適用対象を決めるglobを指定していることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存のプロジェクトファイルへのリンクを確認します。これにより、大きな指示セットを小さく再利用可能なファイルに分け、参考例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルはよい出発点ですが、まだ不足があります。新たに生成するTypeScriptにTSDocコメントを追加するよう、中心となる `copilot-instructions.md` ファイルを変更しましょう。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** という見出しのセクションを探します。およそ35行目にあります。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLIは会話の開始時にリポジトリの指示を読み込みます。指示を保存したら、フィルタリングの会話を終了して再開してください。機能のコンテキストを保ったまま、新しいガイダンスを読み込めます。

1. フィルタリングの会話を開いたままの場合は、`/exit` を入力して終了します。Copilot CLIは会話を自動的に保存します。
2. リポジトリのルートで `game-filters-cli` ブランチに切り替え、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示された場合は、フィルタリングの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilotに更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更されたTypeScriptファイルを確認します。新しく生成されたTSDocコメントが正確にコードを説明していることを確認します。

## まとめと次のステップ

Copilot CLIが指示をどのように読み込むかを確認し、新しい標準を追加して機能に適用しました。このレッスンでは、次のことを行いました。

- リポジトリの指示とパスで対象を限定した指示ファイルが、エージェントにどのように適用されるかを確認しました。
- コーディング標準に従うよう、指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、Lintとテストを一貫して実行します。

## リソース

- [Copilot CLI向けのカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
