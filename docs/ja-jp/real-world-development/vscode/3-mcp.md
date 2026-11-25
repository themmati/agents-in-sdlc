---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

agent mode でフィルタリング機能を構築しました。プルリクエストを作成する前に、ブラウザーで動作を確認します。アプリを手動で操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次の内容を学習します。

- Model Context Protocol (MCP) とは何か、MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- ブラウザーでフィルタリング機能を手動テストするよう Copilot に依頼します。
- ブランチを公開して、フィルタリングのプルリクエストを作成します。

## Model Context Protocol (MCP) とは

Agent mode は、エディターの外にアクセスできると、さらに便利になります。Copilot が外部ツールやサービスと通信する標準的な方法が Model Context Protocol (MCP) です。

![コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールと agent mode の連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)][mcp-blog-post] を使うと、AI エージェントは外部のツールやサービスと通信できます。リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わる操作を実行したりできます。

これらのツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを介してアクセスします。MCP サーバーは、既存の API や NPM パッケージなどのローカルツールとの通信を管理します。各 MCP サーバーは、エージェントが利用できる異なるツールとリソースのセットを提供します。

- **[GitHub MCP Server][github-mcp-server]**: GitHub リポジトリを管理する API にアクセスします。リポジトリの作成や更新、Issue やプルリクエストの管理などができます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright によるブラウザー自動化機能を提供します。Web ページを開いたり、フォームに入力したり、ボタンを選択したりできます。

ほかにもさまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub は、エコシステム内での発見や貢献を促進するために [MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクト内のほかの依存関係と同様に扱ってください。利用前にソースコードをよく確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用する MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. 次のような `github` サーバーがすでに設定されています。

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

この `github` エントリはプロジェクトテンプレートに含まれているため、前の演習で Copilot が Issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルへのインストールは不要です。VS Code は HTTP で接続し、Copilot がツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が操作できるブラウザーを利用できます。これを使って機能をテストします。

1. `.vscode/mcp.json` で `github` の隣に `playwright` エントリを追加し、ファイルが次のようになるようにします。

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

リモートの `github` サーバーとは異なり、`playwright` は**ローカル**サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` は、Codespace のようにブラウザーウィンドウを表示するデスクトップ環境がない場合でも Playwright が動作できるようにします。

> [!NOTE]
> Tailspin Toys では E2E テストに Playwright を使っているため、通常はブラウザーもインストール済みです。後で Copilot がブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行して再試行するよう依頼します。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。Copilot がサーバーのツールを初めて使うときに、VS Code が起動し、サーバーを信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** が表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. サーバーを信頼するか尋ねられた場合は、構成を確認して、起動を許可します。

手動での起動は省略できます。その場合、次の手順で Copilot がツールを初めて使うときに、VS Code がサーバーを起動し、信頼の確認を求めます。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作する代わりに、エージェントがページを開き、移動し、フィルターを適用し、結果を読み取って要約します。会話を中断せずに、機能が期待どおりに動作するか確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を使います。エージェントはボタン、リンク、リスト項目など、ラベル付きの構造化された要素を調べます。これは支援技術と同様の方法で、簡単な機能確認とアクセシビリティの基本確認を兼ねられます。

サーバーを接続してアプリを起動したら、次の内容を確認するよう Copilot に依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。Issue の受け入れ条件に照らして要約を読みます。問題がある場合は、追加の質問をするか、PR を作成する前にコードの修正を依頼します。

> [!NOTE]
> テスト時にはアプリを `http://localhost:4321` で実行しておく必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Copilot が Playwright MCP サーバーを初めて使うときは、ブラウザーのダウンロードが必要な場合があります。ブラウザーが見つからない場合は `npx playwright install chromium` の実行を依頼し、再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたら、チームでレビューするためにプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![変更内容を表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドラインインターフェイス (CLI) など、複数の方法で PR を作成できます。GitHub Copilot を使っているので、PR の作成も依頼しましょう。Copilot が関連 Issue を見つけ、PR と関連付けます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェントドロップダウンで **Agent** が選択されていることを確認し、Copilot が GitHub ツールを使えるようにします。
3. 次のプロンプトを使って PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を収集して操作を行う際に、必要に応じて **Continue** を選択します。GitHub ツールを初めて使うときは、GitHub への認証を求められる場合があります。表示される手順に従ってください。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成することを確認します。
6. Copilot が作成したリンクを選択して PR をレビューします。**まだマージしないでください**。

## まとめと次のステップ

この演習では、次の内容を行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- リリース前に、ブラウザーでフィルタリング機能を手動テストしました。
- ブランチを公開し、フィルタリングのプルリクエストを作成しました。

機能をテストするために MCP サーバーを使いましたが、MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティレビューなど、対象を絞ったタスクを効率化する[カスタムエージェント][next-lesson]を作成します。

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
