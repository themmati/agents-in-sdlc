---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルタリング機能を調べます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに説明したように、コードを書くこと以外にも作業はあります。データや外部サービスを扱い、Copilot で追加の自動化を利用できるようにする必要もあります。そこで役立つのが MCP サーバーです。MCP サーバーを使うと、Copilot CLI に組み込まれている機能に加えて、さらに多くのツールやサービスを利用できます。

このレッスンでは、次の内容を学習します。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように利用するかを理解します。
- まだ利用できない場合は Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルタリング機能を調べます。

## シナリオ

ユニットテストや E2E テストは重要ですが、UI の更新を検証するには、実際に UI を操作する必要があります。ユーザーと同じように Copilot が Web サイトを使えるようにすれば、変更の自動確認をさらに進め、期待どおりに動作するという確信を高められます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部のツールやサービスとリアルタイムに通信できます。最新の情報にアクセスしたり、ユーザーに代わって操作したりできます。

これらのツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを介してアクセスします。各 MCP サーバーは、AI エージェントが利用できる異なるツールとリソースのセットを提供します。

よく使われている MCP サーバーの例を紹介します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内での発見や貢献を促進するために [MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクト内のほかの依存関係と同様に扱ってください。利用する前にソースコードを確認し、発行元を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーと同じように Web サイトを操作できるよう追加します。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに設定されている場合は、このセットアップ手順を省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認し、<kbd>Enter</kbd>、続けて <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを終了します。

## Playwright で機能を調べるよう Copilot に依頼する

これまでは、機能が期待どおりに動作することを手動で確認しました。今度は、Copilot で利用できる Playwright MCP サーバーを使って、同じ確認を行います。

1. Copilot に Playwright MCP サーバーを使って機能を検証するよう、次のプロンプトを送信します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は MCP サーバーを指定しなくても、Copilot が自動的に判断します。使うツールが分かっている場合は、明示しても問題ありません。より一貫した結果が得られ、トークンも節約できます。

2. Copilot がブラウザーで機能を確認するために実行している手順を確認します。
3. レポートを読み、すべて期待どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作して、サーバーを停止した後、レポートを作成します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を調べました。このレッスンでは、次の作業を行いました。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように利用するかを学びました。
- まだ利用できない場合は Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、フィルタリング機能を調べました。

次は、スキルとブラウザーツールを組み合わせた[QA カスタムエージェント][next-lesson]を作成します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
