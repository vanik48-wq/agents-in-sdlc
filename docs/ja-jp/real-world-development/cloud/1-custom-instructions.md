---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を利用する際は、コンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにする必要があります。[指示ファイル][instruction-files]を使うと、Copilot に作業内容だけでなく方法も伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスで対象を指定した指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここではプロンプトの実行前後を比較しません。Copilot cloud agent は GitHub の Issue に対して非同期で動作するため、リアルタイムで並べて効果を示すのが難しいためです。このハーネスの後半で cloud agent が作成したプルリクエストを確認する際に、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データレイヤーには必ずユニットテストを用意します。
- UI はダークモードで、現代的な見た目にします。
- コードのドキュメントには TSDoc のドキュメントコメントを使用します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って、強調したプラクティスに沿って Copilot が作業できるようにします。

### カスタム指示

カスタム指示を使うと、Copilot がコーディングスタイルや要件を理解しやすくなるよう、コンテキストや好みを指定できます。コーディング規約やライブラリ、コードに含めたいコメントの種類などを指定できるため、より適切な提案やコードスニペットを得るのに役立ちます。リポジトリ全体、または特定のファイル種別に対して指示を作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される単一の指示ファイルです。技術スタック、構築するものの概要、ベストプラクティス、チャットや CLI のほとんどのリクエストに関わる全般的なガイダンスなど、プロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に作成できる指示ファイルです。TypeScript や Astro などの言語や、UI コンポーネントやユニットテストの作成などのタスクに関するガイドラインを記載できます。

> [!NOTE]
> IDE で作業する場合、指示ファイルは Copilot Chat でのコード生成にのみ使用され、コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示ファイルと、`applyTo` フロントマターで対象を指定した `*.instructions.md` ファイルの両方を使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準に基づく指示ファイルにも対応しています。[指示ファイルのサポート][custom-instructions-support]をご覧ください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成方法を詳しく説明することは、このワークショップの範囲外です。ただし、サンプルプロジェクトでは代表的な方法を紹介しています。大まかな指針は次のとおりです。

- `copilot-instructions.md` には、構築するものの説明、プロジェクトの構造、全般的なコーディング標準など、プロジェクトレベルのガイダンスに絞って記載します。
- `*.instructions.md` ファイルには、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種別や、特定のタスクに関する指示を記載します。
- 自然な言葉で、明確なガイダンスとコードの良い例・悪い例を示します。

カスタム指示の作成方法はひとつではありません。AI のほかの使い方と同様に、プロジェクトやシナリオに合わせて試しながら調整してください。

> [!TIP]
> GitHub Copilot を使用するすべてのプロジェクトに、充実した指示ファイルを用意することをお勧めします。このプロジェクトの指示ファイルを確認すると、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあることに気付くでしょう。
>
> Copilot に指示ファイルの生成を依頼することもできます。各サーフェスで方法が異なります（たとえば VS Code の **Configure Chat → Generate Agent Instructions** や、Copilot CLI の `/init` など）。必要な場合は、各サーフェスのレッスンで説明します。
>
> テンプレートや出発点をお探しですか。[awesome-copilot][awesome-copilot]では、指示ファイル、カスタムエージェントなどを集めています。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに用意されている指示ファイルを確認してください。中心となる `copilot-instructions.md` が 1 つと、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスも確認してください。これらは、Copilot とやり取りする際に適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示ファイルがあります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を指定する glob であることを確認します。ここでは、たとえば `**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. このプロジェクトのユニットテストを作成する際の指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` の末尾までスクロールします。`unit-tests.instructions.md` などの別の指示ファイルや既存ファイルへのリンクがあります。大きな指示セットを小さく再利用可能なファイルに分け、Copilot が参考にできる例を示せます。ここに記載されたパスは、リポジトリルートではなく指示ファイルを基準とする相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションでは、このプロジェクトのコーディング標準を説明していますが、コード内のドキュメント要件はまだありません。次の手順で、TSDoc のドキュメントコメントとファイルヘッダーに関するルールを追加します。

## リポジトリ標準を追加する

ここで行う唯一の編集は、TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーをコードに追加するという、プロジェクト全体のルールです。後続のハーネスで Issue を割り当てると、cloud agent はこのルールを参照します。

編集する前に、作業用のブランチを作成して切り替えます。cloud agent は Issue の対象となるブランチから指示を読み取りますが、変更をきちんとコミットするためです。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前に説明したように、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コードの提案を改善するために、コーディング標準を記述しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクションを探します。通常は 27 行目付近にあります。プロジェクトのコーディング標準が記載されていますが、生成されたヘルパーにドキュメントコメントがなかった理由と同様、コード内のドキュメントに関するルールはまだありません。
3. 既存の標準のすぐ下に次の Markdown を追加し、ファイルコメントヘッダーと TSDoc のドキュメントコメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、指示はリポジトリレベルの `.github/copilot-instructions.md` に記載して全体に適用することも、言語やファイル種別、タスクに合わせて `*.instructions.md` ファイルに記載することもできます。ドキュメントコメントのルールのようにプロジェクト全体に適用する標準は、リポジトリレベルのファイルに記載するのが適切です。

## 指示の変更をコミット、プッシュ、マージする

Copilot cloud agent は、Issue の対象ブランチから指示ファイルを読み取ります。次の演習で Copilot に Issue を割り当てるとき、cloud agent は `main` からブランチを作成します。指示を適用するには、変更を `main` に取り込む必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを作成し、マージします。次の演習で Issue に割り当てたとき、cloud agent はこの指示を読み取ります。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチの手順を省略して `main` に直接コミットできます。ここでは、実際のプロジェクトでの作業方法を再現するためにブランチを使用しています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。このハーネスの後半で cloud agent が作成するプルリクエストを確認すると、追加した標準が適用されていることがわかります。

次は、[Copilot cloud agent に最初の Issue を割り当てる][next-lesson]方法を学びます。

## リソース

- [GitHub Copilot のカスタマイズに使用する指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示をよりよく記述するための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
