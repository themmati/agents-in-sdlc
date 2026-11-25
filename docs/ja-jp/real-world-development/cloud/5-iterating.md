---
title: "演習 5 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善と機能追加を目的に、GitHub Copilot と複数のタスクに取り組みました。コードにドキュメントを追加し、デザインチーム向けに関連ゲーム機能を構築し、ハイコントラストとライトモードの切り替えを含むアクセシビリティ機能を実装するよう依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って改善します。

### シナリオ

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チーム全体にレビュー権限を付与する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した内容を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期で、監視を受けずにタスクを実行するため、安全性を維持するための制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みができるのは作業に使うブランチ**のみ**です。
- Cloud agent は GitHub Actions 内で実行され、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人による承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは既定で制限][agent-firewall]されています。

## 生成されたドキュメントをレビューする

GitHub Copilot cloud agent が最初に作成した PR である、コードへのドキュメント追加を確認します。github.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を開くと、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。MCP サーバーなどの外部リソースへのアクセスは既定で制限されているため、これは**想定される動作**です。必要であれば、Copilot cloud agent の[ファイアウォールを構成または無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。その場合は作業が完了するまで待ちます。数分かかる場合があります。その間に休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc のドキュメントコメントなど、更新されたコードとドキュメントを確認します。具体的な変更内容は異なる場合があります。演習 2 で確認したカスタム命令に由来する TSDoc コメント、TypeScript の規約、コメントヘッダーに注目してください。
6. 更新内容のレビューが終わったら **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す表示を確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する操作](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄でワークフローが実行されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チームメンバーと同様に、PR やコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、追加セッションを開始して対応します。結果は確率的に変わるため、具体的な依頼文は指定できません。次のような変更を依頼できます。

- 各コードファイルの先頭に、役割を短く説明するコメントヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例などを使い、生成されたドキュメントの変更を依頼するコメントを追加して、**@copilot** にメンションします。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot は更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると PR に戻れます。

    ![プルリクエストに戻る操作](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、PR に新しいコミットが追加されます。
5. **Files changed** タブで変更をレビューします。

納得できるまで繰り返し改善できます。完了したら、ドラフト PR をレビュー可能な状態にして `main` ブランチにマージします。

![PR をレビュー可能な状態にする操作](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新内容に問題がなければ、**Conversation** タブに戻り、下にスクロールします。
6. ワークフローが承認待ちになっていることを示す表示を確認します。
7. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する操作](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄でワークフローが実行されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace のターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を実装しました。前と同じように、cloud agent にコメントで更新を依頼できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーと TSDoc コメントを追加したりできます。後者はカスタム命令を更新する前に割り当てたタスクであることに注意してください。**Conversation** タブにコメントを追加すると、Copilot が新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティカスタムエージェントが実装した機能を確認します。この PR には演習 3 で依頼したハイコントラストモードと、演習 4 で mission control から追加を依頼したライトモードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。その場合は作業が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 特に次の点に注意して、実装を確認します。
   - モードを切り替える UI コンポーネント。
   - ユーザー設定を保存する local storage の使い方。
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更。
   - ARIA ラベルやキーボード操作などのアクセシビリティ属性。
   - モード切り替えを管理する JavaScript/TypeScript コード。
6. 更新内容に問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す表示を確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する操作](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄でワークフローが実行されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace で Copilot が作成したブランチに切り替えて、アクセシビリティ機能を手動でテストできます。ターミナルを開き、ブランチ名に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリケーションを起動し、ブラウザーでハイコントラストとライトモードの切り替えが動作し、ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティカスタムエージェントが、アクセシビリティのベストプラクティスに従って Copilot をガイドしたことを確認します。アクセシビリティ上の問題や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 - さらに作業を委任する

cloud agent は、実際のバックログ項目を任せて別の作業に取り組むときに役立ちます。リポジトリに Issue を追加し、Copilot に割り当ててみましょう。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。これらのレッスンを通して、次の作業を行いました。

- **このリポジトリに含まれるカスタム命令ファイルを確認**し、後で cloud agent の出力にどう影響するかを把握しました。
- **Issue を Copilot cloud agent に割り当て**、環境の準備、計画、非同期の実行を確認しました。
- **アクセシビリティ用のカスタムエージェントを作成して使用**し、ハイコントラストとライトモードの切り替えを追加しました。
- **Copilot agents ページを mission control として使い**、進行中のアクセシビリティセッションを監視して調整しました。
- **cloud agent のプルリクエストをレビューして改善**し、`@copilot` に変更を依頼して、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。引き続き学習する場合は、次のハーネスも役立ちます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat の agent mode と MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、任意の `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つリリースします。

自分のリポジトリでは、次のアイデアを試してください。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てる。
- 別の分野のカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
