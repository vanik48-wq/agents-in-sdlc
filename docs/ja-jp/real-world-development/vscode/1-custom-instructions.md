---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使う際はコンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に背景情報を伝えたい場合は、そのコンテキストを参照できるようにします。Copilot に特定のコンテキストを共有する方法はいくつかあります。その中心となるのが[指示ファイル][instruction-files]です。指示ファイルを使うと、コード生成の指針を伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot にどのように伝わるかを確認します。
- 現在の指示を適用した状態で、フィルター機能の最初のデータスライス（出版社ヘルパー）を生成します。
- リポジトリ全体に適用する新しい標準を `.github/copilot-instructions.md` に追加します。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映されることを確認します。
- 指示の更新とフィルタリング用データスライスを `main` にコミットし、次の演習で Copilot が更新後のガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot の出力は確定的ではありません。この演習では、指示を更新した後の動作の「傾向」を確認します。生成結果が文字単位で一致することを目指すものではありません。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データレイヤーには必ずユニットテストを用意します。
- UI はダークモードにし、現代的な印象にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使うことで、Copilot に適切な情報を提供し、これらの開発慣行に沿ったタスクの実行を支援します。

### カスタム指示

カスタム指示を使うと、コンテキストや好みを Copilot に伝え、コーディングスタイルや要件を理解しやすくできます。これは、より適切な提案やコードスニペットを得るための強力な機能です。好みのコーディング規約、ライブラリ、コードに含めたいコメントなどを指定できます。リポジトリ全体に対する指示も、特定のファイル種別に対する指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送られる単一の指示ファイルです。Copilot Chat や CLI でのほとんどのリクエストに関係する、プロジェクトレベルの情報を記載します。技術スタック、開発対象の概要、ベストプラクティス、全般的なガイダンスなどが該当します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイル種別向けに作成できます。TypeScript などの言語や、UI コンポーネントや新しいユニットテストの作成といったタスクに関するガイドラインを記載できます。

> [!NOTE]
> IDE で作業する場合、指示ファイルは Copilot Chat でのコード生成にのみ使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示ファイルと、`applyTo` フロントマターで対象を指定した `*.instructions.md` ファイルの両方をコード生成時に使用します。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、ほかの標準に基づく指示ファイルにも対応しています。[対応する指示ファイルの標準][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく説明することはこのワークショップの範囲外ですが、サンプルプロジェクトでは代表的な方法を紹介しています。基本的には、次のようにします。

- `copilot-instructions.md` には、開発対象の説明、プロジェクト構成、全体に適用するコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` ファイルは、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種別や、特定のタスクに関する指示に使用します。
- 自然な言葉で、明確なガイダンスを記載します。コードをどのように書くべきか、またどのように書くべきでないかの例を含めます。

指示ファイルの作成方法は一つではありません。AI の使い方と同様に、プロジェクトで試しながら最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトに、充実した指示ファイルを用意しましょう。このプロジェクトの指示ファイルを確認すると、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルが見つかります。
>
> Copilot に指示ファイルの作成を支援してもらうこともできます。利用する環境によって方法は異なります（たとえば VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` です）。必要に応じて、利用中の環境向けのレッスンで説明します。
>
> テンプレートや出発点をお探しですか。[awesome-copilot][awesome-copilot]では、指示ファイル、カスタムエージェント、その他のリソースを確認できます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認してください。中心となる `copilot-instructions.md` が 1 つあり、さまざまなタスク向けの `*.instructions.md` ファイルもあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、指示の適用対象を決めるリポジトリルート相対の glob を指定しています。たとえば `**/*.test.ts` に一致する TypeScript テストファイルが対象です。
5. このプロジェクトでユニットテストを作成する際の指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの他の指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。これにより、大きな指示を再利用しやすい小さなファイルに分割し、コード生成時に参考にする例を Copilot に示せます。ここに記載されたパスはリポジトリルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションではプロジェクトのコーディング標準が説明されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDoc のドキュメントコメントとファイルコメントヘッダーのルールを追加します。

## 変更用のブランチを作成する

ここでは、ベストプラクティスに従って変更用ブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを適用した状態でプロンプトを送信します。後で指示を更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に示します。
3. Chat ビューのエージェントのドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べてコードを更新します。多くの場合、ヘルパーファイルとテストの両方が変更されます。
6. 提案されたヘルパーは、最初の引数として `db` クライアントを受け取り、型付きの出版社配列を返す関数です。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約によるものです。
7. 提案されたコードには、TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の出力は確率的であるため、指示がなくてもドキュメントコメントが追加される場合があります。その場合も問題ありません。指示を更新した後に一貫性が高まることを確認するのが、この演習の目的です。

## 新しいリポジトリ標準を追加する

前に説明したように、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コード提案の質を高めるため、リポジトリのコーディング標準を記載します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを見つけます。ここにはプロジェクトのコーディング標準が記載されていますが、生成されたヘルパーにドキュメントコメントがなかった理由となる、コード内ドキュメントのルールはまだありません。
3. 既存の標準の直後に、次の Markdown 行を追加します。これにより、ファイルコメントヘッダーと TSDoc のドキュメントコメントを追加するよう Copilot に指示できます。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリレベル（`.github/copilot-instructions.md`）にも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` ファイルにも作成できます。追加したドキュメントコメントのルールのように、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をリセットします。
2. `src/lib/publishers.ts` を選択し、Copilot が適切なファイルに注目するようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 今度は、提案されたファイルの先頭に次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Tailspin Toys クラウドファンディングプラットフォーム用の出版社データアクセスヘルパーです。
    * データベースから出版社情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * すべての出版社の ID と名前の一覧を返します。
    *
    * @param db - Drizzle データベースクライアントです。
    * @returns 出版社オブジェクトの配列に解決される Promise です。
    */
   ```

これで新しいプロジェクト標準に従うよう Copilot を調整し、次の演習で利用する実際のコードに適用できました。

## 指示ファイルをコミットしてブランチをプッシュする

指示ファイルも他のアセットと同様に、ソース管理で管理します。指示ファイルと変更をリポジトリにコミットしてプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押して、新しいターミナルウィンドウを開きます。
2. ターミナルで次のコマンドを実行し、指示ファイルとヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで次のコマンドを実行し、指示ファイルの更新とヘルパーの基盤に関して Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加の変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 次のコマンドでブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の issue に関連付けてプルリクエストを作成します。手動でもできますが、プロジェクトに接続済みの GitHub ツールを使って Copilot に依頼できます。issue を検索して、それをクローズするプルリクエストを作成し、マージするようにプロンプトを送ります。

> [!NOTE]
> この演習では初めて、Copilot が GitHub 上の操作を行います。プロジェクトテンプレートには GitHub ツールがすでに接続されています。そのため、設定は不要です。Copilot が初めてツールを使うときに VS Code から GitHub へのサインインを求められる場合があります。画面の指示に従って許可してください。後の演習では、この接続の仕組みである Model Context Protocol について学びます。

1. codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）を押して Copilot Chat ビューを開き、エージェントの選択メニューで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する issue を検索し、現在のブランチからプルリクエストを作成するよう Copilot に依頼します。プルリクエストには、指示の更新と、今後のフィルタリング作業に向けた新しい出版社ヘルパーの基盤の両方を記載します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue の検索とプルリクエストの作成を開始します。
5. Copilot に GitHub 操作を許可するよう求められたら、コマンドを確認し、適切な場合は **Allow** を選択します。
6. プルリクエストが作成されたら、次のプロンプトを使ってマージし、ブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot に GitHub 操作とシェルコマンドの実行を許可するよう求められたら、コマンドを確認し、適切な場合は **Allow** を選択します。

これで GitHub Copilot の支援を受けてプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- コード生成プロンプトを送信し、*既存の*指示で Copilot が生成する内容を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体へ適用する標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映されることを確認しました。
- 指示とヘルパーの基盤を `main` にコミットし、次の演習で利用できるようにしました。

次は[エージェントモード][next-lesson]で指示を活用し、Copilot にコードベース全体にわたる新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示をよりよく書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
