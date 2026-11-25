---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更の計画、コードの作成、動作のテスト、プルリクエストのレビュー、エージェントを使った作業管理を繰り返し行います。

## シナリオ

あなたは、開発者をテーマにしたボードゲームのクラウドファンディングを提供する架空の会社 Tailspin Toys に入社したばかりの開発者です。チームのバックログは GitHub Issue としてすでに登録されており、取り組む作業を選べます。バックログには、フィルターやページネーションなどの機能開発に加えて、アクセシビリティやコーディング規約などの品質改善も含まれています。

## 環境を選ぶ

GitHub Copilot は、普段の作業環境で利用できます。自分に合った環境を選んでください。各ワークショップは個別のセットアップから始まり、共通の Tailspin Toys シナリオを使います。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、命令、スキル、カスタムエージェント、Playwright MCP、Agent Merge、実用的なスラッシュコマンドを活用します。

### [GitHub Copilot app][app]

デスクトップアプリでエージェントセッションを並行して実行し、セッションモードを切り替え、Canvas で共同作業し、GitHub Issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業をガイドし、進行状況を確認して、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat の agent mode、MCP サーバー、カスタムエージェントを活用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
