---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP ツール、プルリクエストの自動化を学びながら、Tailspin Toys の 2 つの変更をビルド、検証、納品します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** を使うと、ターミナルで GitHub Copilot をエージェント型のコーディングアシスタントとして利用できます。コードベースを調べ、コードを生成し、コマンドを実行し、外部ツールに接続できます。すべてコマンドラインから行えるため、グラフィカルエディターに切り替えずに作業を続けられます。

このワークショップでは、Tailspin Toys を使った一連のワークフローに沿って進めます。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして、基本を確認します。
2. 星評価の小さな変更を行い、ブラウザーでレビューして、最初のプルリクエスト (PR) を手動でマージします。
3. フィルタリングの Issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューします。
4. リポジトリの命令を更新し、フィルタリングの作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズして、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルタリングを確認します。
7. quality assurance (QA) カスタムエージェントを作成し、要件、カバレッジ、検証の証拠をレビューします。
8. フィルタリングの変更全体をレビューし、Agent Merge を使ってフィルタリングの PR をマージします。
9. コンテキスト、モデル、共有、任意のクラウド委任に使えるスラッシュコマンドを確認します。

ワークショップをシンプルにするため、星評価とフィルタリングの 2 つの PR を作成します。フィルタリングの PR には、命令の更新、スキルの更新、QA プロファイル、テストが含まれます。各ツールを試しながら作業を積み重ねられるよう、フィルタリングと品質確認のワークフローでは、同じ会話とブランチを使います。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証する |
| [2. 星評価を追加する: 手軽な改善][ex2] | 最初の変更 | 既存の評価と null の場合の代替表示を行い、最初の PR をマージする |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | Issue から機能を計画し、Autopilot で構築してから、Interactive モードでレビューする |
| [4. カスタム命令で Copilot をガイドする][ex4] | コンテキスト | 命令を確認・更新し、フィルタリングに適用する |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し可能なチェック | 既存のスキルを確認し、レポート形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | CLI で MCP を設定し、フィルタリングの動作を調べる |
| [7. QA エージェントを作成して使う][ex7] | 要件とカバレッジ | 専門家プロファイルを作成・選択し、最終検証の証拠を集める |
| [8. 機能の PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューし、PR を作成して Agent Merge を使う |
| [9. GitHub Copilot CLI のスラッシュコマンド][ex9] | CLI の機能 | コンテキスト、モデル、共有、クラウドエージェントへの任意の委任を確認する |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、再利用可能なカスタマイズ、追加リソースを確認する |
| [オプション: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づいた Backer Concierge を構築してデプロイし、Web サイトに接続する |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] ターミナル / コマンド ライン操作の基本的な知識
- [ ] インストール済みで設定済みの Git

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用するエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれています。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめる

**[前提条件から始める →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[foundry]: 8-foundry-agent/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
