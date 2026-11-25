---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに `quality-checks` スキルを使って自動チェックを実行し、Playwright MCP でブラウザーのフィルタリング動作を確認しました。今度は、明確に定義した QA プロセスを持つカスタムエージェントに、これらの機能をまとめます。

このレッスンでは、次の内容を学習します。

- カスタムエージェントが命令、スキル、MCP ツールと連携する仕組みを確認します。
- 再利用可能な quality assurance プロファイルを作成して確認します。
- QA エージェントを選択し、フィルタリングの Issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) の作成前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザーの動作を一貫してレビューしたいと考えています。カスタムエージェントを使うと、この QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する、Copilot の特化バージョンです。プロファイルには、エージェントの目的、命令、利用可能なツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使用したカスタマイズには、それぞれ異なる役割があります。リポジトリの命令はチームの標準を示します。`quality-checks` スキルは、繰り返し実行するチェックをまとめます。Playwright MCP はブラウザーツールを提供します。QA プロファイルは、要件を評価して結果を報告するために、それらの機能を Copilot がどう使うかを示します。これらのカスタマイズを置き換えるものではなく、新しい会話も必要ありません。

## QA プロファイルを作成する

機能の PR を作成する前に、再利用可能な QA プロファイルを作成するよう Copilot に依頼します。プロファイルでは、QA で実行するチェックと、その作業の境界を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、新しいカスタムエージェントの作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、意図した QA ワークフローと作業範囲がプロファイルに反映されていることを確認します。これにより、検証だけを行いたいのに、不完全または広範すぎるエージェントが機能を変更するのを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを読みます。`description` は必須です。`name` は任意ですが、含めるとエージェントの表示名が明確になります。
3. プロファイルの命令を読み、QA が要件を確認し、リポジトリの命令に従い、`quality-checks` スキルを実行して、Playwright MCP を使うことを確認します。
4. QA が証拠を示して報告し、実装コードを変更する前に確認を求め、変更をコミットしたりプルリクエストを作成したりしないことを確認します。
5. これらの役割や制約が不足している場合は、続ける前に既定のエージェントに修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は、会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後、同じフィルタリングの会話を再開し、QA を選択します。これにより、Issue と計画中に決めた内容を利用できます。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成した直後のエージェントは、利用可能なエージェントの一覧に表示されない場合があります。次のコマンドで有効になります。

2. 次のプロンプトを使って、QA エージェントに機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を始めます。
4. 完了したら、作成されたレポートを読みます。

## まとめと次のステップ

再利用可能な専門家の役割をワークフローに追加し、その成果をレビューしました。このレッスンでは、次の作業を行いました。

- カスタムエージェントが命令、スキル、MCP ツールと連携する仕組みを確認しました。
- 再利用可能な quality assurance プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルタリングの Issue に照らして結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートの準備ができました。次は、[機能の PR にまとめて Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
