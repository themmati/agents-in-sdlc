---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent][about-cloud-agent]** を使うと、GitHub Copilot がクラウドで非同期に作業できます。GitHub で作業を割り当てると、cloud agent がバックグラウンドでリポジトリを調べ、変更を行い、プルリクエストを作成します。その間、別の作業を進められます。

これらの演習では、まず cloud agent が従うカスタム命令を追加し、GitHub Issue を割り当てて実装を依頼します。カスタムエージェントをレビューして使用し、エージェントのダッシュボードからセッションの進捗を確認・調整した後、作成されたプルリクエストをレビューして結果を改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. カスタム命令][ex1] | コンテキスト | cloud agent が従うカスタム命令を追加する |
| [2. Cloud agent][ex2] | 非同期エージェント | GitHub Issue を Copilot cloud agent に割り当てる |
| [3. カスタムエージェント][ex3] | 専門エージェント | カスタムエージェントをレビューして使う |
| [4. エージェントを管理する][ex4] | 監視 | エージェントのセッションを監視して調整する |
| [5. 反復して改善する][ex5] | レビュー | PR をレビューし、Copilot の作業を改善して、次のステップを選ぶ |

## 前提条件

このワークショップに参加する前に、次の準備をしてください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] アカウントまたは Organization で Copilot cloud agent が有効になっていること

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、すべてのハーネスを完了できます。

[student-plan]: https://github.com/education/students

> [!NOTE]
> Cloud agent は Copilot Free を除く **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Business または Enterprise では、管理者が Organization で有効にする必要があります。

> [!NOTE]
> このハーネスでは MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI ハーネス][cli]または [VS Code ハーネス][vscode]を確認してください。

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cli]: ../cli/
[vscode]: ../vscode/
[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
