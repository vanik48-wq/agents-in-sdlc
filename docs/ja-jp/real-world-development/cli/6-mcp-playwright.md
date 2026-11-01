---
title: "レッスン 6 - Playwright MCPで機能を検証する"
description: "Copilot CLIでPlaywright MCPを確認または構成し、ブラウザーでフィルタリング機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

すでに説明したように、コードを書くことだけが開発ではありません。データや外部サービスを扱い、Copilotが利用できる自動化機能を追加することも必要です。そこで役立つのがMCPサーバーです。MCPサーバーを使うと、CopilotはCLIに組み込まれた機能だけでなく、さらに多くのツールやサービスを利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) とは何か、Copilot CLIでどのように使われるかを確認します。
- まだ利用できない場合は、Playwright MCPサーバーを追加します。
- エージェントにブラウザーを操作させ、フィルタリング機能を試します。

## シナリオ

ユニットテストとエンドツーエンドテストは重要ですが、UIの更新を検証するには、実際にUIを操作する必要があります。Copilotに、ユーザーと同じように作業中のWebサイトを操作させることで、変更が期待どおりに動作するかを確かめる作業をさらに自動化し、信頼性を高めたいと考えています。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post]を使うと、AIエージェントは外部のツールやサービスと通信できます。MCPを使えば、AIエージェントは外部のツールやサービスとリアルタイムにやり取りでき、最新情報へのアクセスや、ユーザーに代わった操作が可能になります。

これらのツールやリソースには、AIエージェントと外部のツールやサービスをつなぐMCPサーバーを介してアクセスします。各MCPサーバーは、AIエージェントが利用できる異なるツールとリソースを提供します。

よく使われているMCPサーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHubリポジトリ、Issue、プルリクエストを管理するAPIを利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwrightを使ったブラウザー自動化機能を利用できます。

ほかにも多くのMCPサーバーがあります。GitHubでは、エコシステム内のリソースを見つけやすくし、貢献を促進するために、[MCPレジストリ][mcp-registry]を提供しています。

> [!CAUTION]
> MCPサーバーは、ほかのプロジェクト依存関係と同様に扱ってください。使用する前にソースコードを確認し、公開元を検証して、セキュリティへの影響を検討してください。

## Playwright MCPサーバーを追加する

Playwright MCPサーバーをまだ利用できない場合は、Copilotがユーザーと同じようにWebサイトを操作できるように追加します。

> [!NOTE]
> Playwright MCPをCopilot CLIですでに構成している場合は、セットアップ手順を省略して次のセクションに進んでください。

1. Codespaceに戻ります。
2. Copilot CLIで次のコマンドを入力し、MCPサーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認し、<kbd>Enter</kbd> を押してから <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しいMCPサーバーを保存します。
7. <kbd>Esc</kbd> を押してMCPダイアログを閉じます。

## Playwrightを使って機能を試すようCopilotに依頼する

これまでは、機能が期待どおりに動作するかを手動で確認しました。次は、Copilotで利用できるPlaywright MCPサーバーを使って、同じ確認を行いましょう。

1. 次のプロンプトを使い、Playwright MCPサーバーで機能を検証するようCopilotに指示します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilotは通常、自動的にMCPサーバーを見つけて使うため、明示的に指示する必要はありません。ただし、使用するツールが分かっている場合は、方向を示しておくと役立ちます。結果の一貫性が高まり、トークンも節約できます。

2. Copilotがブラウザーで操作を行い、機能が動作するかを確認する様子を見ます。
3. Copilotのレポートを読み、すべて期待どおりに動作していることを確認します。

Copilotはサーバーを起動し、PlaywrightでWebサイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

おめでとうございます。Copilot CLIからPlaywright MCPサーバーを使い、実際のブラウザーで機能を試しました。このレッスンでは、次のことを行いました。

- Model Context Protocol (MCP) とは何か、Copilot CLIでどのように使われるかを学びました。
- まだ利用できない場合は、Playwright MCPサーバーを追加しました。
- エージェントにブラウザーを操作させ、フィルタリング機能を試しました。

次は、スキルとブラウザーツールを組み合わせた[QAカスタムエージェントを作成します][next-lesson]。

## リソース

- [MCPとは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLIにMCPサーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
