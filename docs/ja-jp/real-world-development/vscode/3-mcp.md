---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードを使ってフィルタリング機能を作成しました。プルリクエストを作成する前に、ブラウザーで正しく動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーで機能をテストしてもらいます。その後、ブランチを公開してプルリクエストを作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーで Copilot にツールを追加する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot に Playwright MCP サーバーを使わせ、ブラウザーでフィルタリング機能を手動テストします。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成します。

## Model Context Protocol (MCP) とは何ですか？

エージェントモードは、エディターの外部にアクセスできると、さらに強力になります。Copilot が外部にアクセスする仕組みが Model Context Protocol (MCP) です。エージェントが外部のツールやサービスと通信するための標準的な方法です。

![エージェントモードの内部と、コンテキスト、LLM、ツール、および MCP サーバーや VS Code 拡張機能が提供するツールとのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスとリアルタイムで通信できます。これにより、最新情報へのアクセス（リソースを使用）や、ユーザーに代わった操作（ツールを使用）が可能になります。

これらのツールやリソースには、MCP サーバーを介してアクセスします。MCP サーバーは AI エージェントと外部のツールやサービスをつなぐ橋渡し役です。既存の API や npm パッケージなどのローカルツールと AI エージェントとの通信を管理します。各 MCP サーバーは、AI エージェントが利用できるさまざまなツールやリソースを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。AI エージェントは、新しいリポジトリの作成、既存リポジトリの更新、issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。AI エージェントは、Web ページへの移動、フォームへの入力、ボタンの選択などを実行できます。

ほかにも、さまざまなツールやリソースを提供する MCP サーバーがあります。GitHub は、エコシステム内のサーバーを見つけやすくし、貢献を促進するために [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点では、MCP サーバーを他の依存関係と同様に扱ってください。使用前にソースコードをよく確認し、発行元を検証し、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用できる MCP サーバーを構成します。

1. codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに構成されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクトテンプレートに含まれています。そのため、前の演習では Copilot が issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使用しているため、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot がサーバーのツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が制御できるブラウザーを提供します。機能のテストに最適です。

1. `.vscode/mcp.json` で `github` と並べて `playwright` エントリを追加し、ファイルが次のようになるようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル**サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、Playwright は画面を表示せずにブラウザーを実行します。codespace には表示するデスクトップ環境がないため、この指定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトではエンドツーエンドテストに Playwright をすでに使用しているため、必要なブラウザーは通常インストール済みです。後で Copilot がブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行するよう依頼してから、もう一度試してください。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot がサーバーのツールを初めて必要としたときにサーバーが起動し、信頼するかどうかを確認するメッセージが表示されます。

1. `.vscode/mcp.json` で、`playwright` エントリの上に **Start** アクションが表示されます。これを選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code からサーバーを信頼するかどうか尋ねられた場合は、構成を確認して、サーバーを起動できるよう信頼することを選択します。

手動で起動する必要はありません。起動を省略すると、次の手順で Copilot が初めてツールを必要としたときに、VS Code がサーバーを起動して信頼を求めます。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。アプリを自分で操作して確認する代わりに、エージェントがページを開き、移動し、フィルターを適用して、その結果を読み取って要約します。会話を離れずに機能が期待どおり動作することを確認する最速の方法です。

内部では、Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]を使います。つまり、エージェントはボタン、リンク、リスト項目などのラベル付き要素を構造化された形で扱います。支援技術と同じように推論するため、簡単な機能チェックがアクセシビリティの基本確認にもなります。

サーバーが接続され、アプリが実行中であることを確認したら、次のプロンプトを Copilot に送り、作成したフィルタリング機能をテストします。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。その要約を issue の受け入れ条件と照らし合わせます。気になる点があれば、プルリクエストを作成する前に追加の質問をするか、コードを修正するよう依頼してください。

> [!NOTE]
> テストするには、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動してください。Copilot が Playwright MCP サーバーを初めて使うときに、ブラウザーのダウンロードが必要になる場合があります。ブラウザーが見つからないと報告された場合は、`npx playwright install chromium` を実行するよう依頼して、もう一度試してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が正しく動作することを確認できたら、チームのレビューに向けてプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. codespace で **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![Source Control パネルに Copilot の変更が表示された画面](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## プルリクエストを作成する

プルリクエストの作成方法には、github.com や GitHub コマンドラインインターフェイス (CLI) を使う方法など、いくつかあります。ここでは GitHub Copilot に作成を依頼しましょう。Copilot は関連する issue を見つけ、それに関連付けたプルリクエストを作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. Copilot が GitHub ツールを使えるよう、エージェントのドロップダウンで **Agent** が選択されていることを確認します。
3. 次のように Copilot にプルリクエストの作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を収集して操作を実行するために求めたら、**Continue** を選択して許可します。GitHub ツールを初めて使うときは、GitHub への認証を求められる場合があります。画面の指示に従って許可してください。
5. Copilot が issue を検索して該当するものを見つけ、プルリクエストを作成する様子を確認します。
6. Copilot が生成したリンクを選択してプルリクエストを確認します。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーで Copilot にツールを追加する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- Playwright MCP サーバーを使って、リリース前にブラウザーでフィルタリング機能を手動テストしました。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成しました。

機能の**テスト**に MCP サーバーを使いましたが、Copilot を拡張する方法は MCP だけではありません。次は[カスタムエージェント][next-lesson]を作成し、アクセシビリティレビューなど、対象を絞ったタスクを効率化しましょう。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
