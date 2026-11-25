---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

この数回の演習では、ユーザー体験の改善と機能追加を目的に、GitHub Copilot cloud agent に 3 つの別々のタスクを依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。[GitHub.com のエージェントページ][agents-page]もその 1 つです。この mission control では、オープンなプルリクエスト (PR) があるエージェントタスクをすべて確認できます。実行された操作を調べたり、進行中のセッションに追加のガイダンスを送ったりできます。

このレッスンでは、次の内容を学習します。

- エージェントページで cloud agent のタスクを監視します。
- 進行中のセッションに指示を追加し、機能を依頼します。

## シナリオ

ハイコントラストモードの作成をエージェントに依頼した後、チームはライトモードも追加するとよいと考えました。サイトのスタイル更新と切り替え機能をすでに実装しているため、同時に追加するのが合理的です。エージェントの作業範囲を調整し、ハイコントラストモードとライトモードの両方を追加するよう依頼します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインの両方に表示されるタスクの一覧を確認します。次のようなタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲーム変更用の API を生成する。
   - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを選択し、Copilot が実行した作業を確認します。次のような手順が含まれる場合があります。
   - リポジトリからコードをチェックアウトする。
   - Copilot が作業する環境を作成する。
   - MCP サーバーを設定する。
   - 割り当てられたタスクを完了するための各種処理を行う。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示される PR ペインも確認します。PR と変更されたファイルを確認でき、追加で進行状況を監視できます。

## cloud agent の作業を調整する

現在実行中のタスクを確認できたので、Copilot がハイコントラストモードを作成している間に、ライトモードの切り替えも追加するよう依頼します。

1. ハイコントラストモードを追加しているセッションを選択します。正確なタイトルは、Copilot が付けた名前や作業状況によって異なります。

    ![mission control の Accessibility セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェントページのクラウドエージェントタスク。Copilot の作業中にアクティブなセッションを調整するダイアログが強調表示されています。](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に取り込むことを確認します。

## Copilot の作業を待つ

前回と同じように、Copilot が更新されたタスクに取り組みます。メッセージを送信した時点で実行中の手順が完了すると、新しい依頼が作業に反映されます。

今回も数分かかります。その間に、ここまでに学んだ内容を振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視・誘導するための中心的な場所である、Copilot agents ページを確認しました。この mission control では、進行中と完了済みのタスクを追跡し、実行中の作業をレビューし、進行中のタスクの範囲を変更したり追加のガイダンスを提供したりできます。

次の内容を確認しました。

- mission control とエージェントページで、cloud agent のタスクを監視しました。
- 進行中のセッションに指示を追加し、機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、Copilot が作成した[プルリクエストを改善する][next-lesson]方法を確認します。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
