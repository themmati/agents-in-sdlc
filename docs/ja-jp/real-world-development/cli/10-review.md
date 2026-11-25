---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、フィルタリングの Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリングの Issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム命令でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- quality assurance (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーの証拠を評価しました。
- フィルタリングの変更全体をレビューし、フィルタリングの PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュコマンドを確認しました。

## リリースした内容

このワークショップでは、更新した `main` からそれぞれ別のブランチを作り、2 つの PR を作成しました。

1. **星評価**: 既存の `starRating` と未評価の場合の明示的な表示をゲームカードに追加しました。
2. **フィルタリングと品質確認のワークフロー**: フィルタリングを実装し、命令を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加しました。

フィルタリングの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザーの確認、MCP を介した Copilot によるブラウザー操作でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA は PR の前に、これらの結果と要件およびテストカバレッジのレビューをまとめました。

追加するテストは、実際に不足している部分を補うものである必要があります。QA の実行で追加テストが不要と判断される場合もあります。マージを許可する前にコードと証拠をレビューし、変更後は影響を受ける証拠を再確認してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業の結果を左右します。このワークショップでは、命令を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整してください。命令は標準を設定し、スキルは繰り返し実行するタスクを説明し、カスタムエージェントは専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。構築前に方針を検討するときは **Plan**、範囲の絞られた変更を進める間も確認を続けるときは **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業には高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を、なぜ、どのように作りたいかを明確に説明すると、結果が大きく変わります。

## さらに試す

主要なワークフローは完了しました。Copilot CLI には、ほかにも確認する価値のある機能があります。

- `/review` でコードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を整理し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールも、使い続けることが上達への近道です。実際のコード、個人のコード、長年作りたいと思っていた小さなアプリで試してみましょう。チームと学んだことを共有し、お互いの経験から学んでください。そして、ドキュメントも引き続き確認しましょう。

ターミナルから Tailspin Toys を拡張する場合は、オプションの[Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
