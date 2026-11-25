---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム命令][previous-lesson] |
|:--|

多くの組織では、未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残っている機能など、技術的負債に直面しています。GitHub Copilot cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行できます。作業が完了すると、人がレビューできるドラフトのプルリクエスト (PR) を作成します。時間のかかる作業を任せることで、開発者はより大きな課題に集中できます。

Copilot cloud agent で次の内容を確認します。

- コード生成環境のカスタマイズ。
- 操作を安全に行う方法。
- Issue の範囲を明確に定める重要性。
- Issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを作成した委託先がドキュメントを適切に整備しなかったため、ほとんど存在していません。まず、アプリケーションのエクスポート関数すべてに TSDoc のドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリのほかのゲームを関連ゲームとして表示し、閲覧を続けられるようにします。洗練された実装はまだ必要ありません。UX の受け入れテストに使える状態にすることが目的です。現在はブロッカーですが、優先度の高い Issue がほかにあります。

どちらも優先順位が下がりやすいタスクです。Copilot cloud agent に割り当てると、開発者が別の作業に集中している間に非同期で対応してもらい、後から成果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent][copilot-agents] は、人間の開発者と同様にバックグラウンドでタスクを実行できます。たとえば [GitHub Issue を Copilot に割り当て][assign-issue]ます。割り当て後、Copilot は進捗を追跡するドラフト PR を作成し、環境を準備してタスクに取り組みます。作業中または完了後にセッションを確認できます。解決案をレビューできる状態になると、プルリクエストで通知されます。

## 範囲を明確に定めた指示の重要性

GitHub Copilot を使っても、考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく調べると複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペアプログラマーとして扱い、段階的に作業し、学び、試し、適宜調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム命令

前の演習では、Copilot をガイドするカスタム命令を確認しました。まだ確認していない場合は、cloud agent に作業を割り当てる前に、このリポジトリに含まれる命令ファイルを読み取り専用で確認してください。

- `.github/copilot-instructions.md`: **Code standards** セクションを確認し、TypeScript の規約と TSDoc コメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` フロントマターが `**/*.test.ts` ファイルに命令を適用することを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てます。生成された PR で、これらの命令に由来する TSDoc コメント、TypeScript 規約、コメントヘッダーを確認してください。

## Copilot cloud agent の開発環境を設定する

コードの作成には、環境の準備とセットアップスクリプトが必要です。Copilot にタスクを割り当てる場合も、ソフトウェアエンジニアのように作業するため、同様の準備が必要です。

Cloud agent は [GitHub Actions][github-actions] を使って作業環境を構成します。作業開始前に実行する[セットアップワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に作成すると、必要な開発ツールと依存関係を利用できるようになります。このラボでは、Node.js、プロジェクトの依存関係、E2E テスト用のブラウザーバイナリ、単一 Astro アプリ用にマイグレーションと初期データ投入を行った SQLite データベースを利用できるよう、あらかじめ設定されています。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順* に必要な最小限の権限を設定します。Copilot は操作用の独自トークンを受け取ります。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は、`contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを使えるよう、ローカル SQLite データベースをマイグレーションして初期データを投入します。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じですが、次の点を確認してください。

- `copilot-setup-steps` という 1 つのジョブがあり、Copilot が PR に取り組む前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うため、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待たずに、正常に動作するかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、古い情報しかない、またはドキュメントが存在しないプロジェクトもあります。これは生産性を低下させ、コードベースの保守や新しい開発者の参加を難しくする技術的負債です。Copilot はドキュメントの作成に役立つため、cloud agent に依頼するのに適したタスクです。後の演習で成果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択し、**New issue**、**Blank issue** の順に選択します。
3. **Title** に `Code lacks documentation` と入力します。
4. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

5. **Create** を選択して Issue を作成します。
6. 右側で **Assign to Copilot** を選択し、割り当てのダイアログで **Assign** を選択します。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

    ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

7. **Pull Requests** タブを選択します。
8. `[WIP]: Code lacks documentation` のようなタイトルの、新しく作成されたプルリクエストを開きます。一覧に表示されない場合は、少し待ってからブラウザーを更新します。
9. 数分後、Copilot が PR に To Do リストを作成します。

> [!NOTE]
> Copilot が To Do リストを作成するまで数分かかる場合があります。前の手順で説明したワークフローを実行して環境を準備し、プロジェクトを分析して、問題への最適な対応方法を検討しています。

10. リストを確認し、実行するタスクを確認します。
11. PR のタイムラインを下にスクロールし、Copilot が Issue に取り組み始めたことを確認します。
12. **View session** を選択します。

    ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示が見えるように、ブラウザーを更新する必要がある場合があります。

13. ライブセッションをスクロールして、Copilot がコードを調査し、状態を理解し、計画を決め、コードを作成する様子を確認します。

通常は数分かかります。Copilot cloud agent の主な目的は、非同期でタスクを実行して、ほかの作業に集中できるようにすることです。別のタスクを割り当ててから、アプリケーションの機能を追加するコードを作成する間に、この機能を活用します。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分けられることです。デザインチームが求めている関連ゲームのセクションは、短時間で作成できるかもしれません。それでも、別の作業に使える時間を確保するため、cloud agent に依頼します。

1. github.com のリポジトリに戻り、**Issues**、**New issue**、**Blank issue** の順に選択します。
2. **Title** に `Show related games on the game details page` と入力します。
3. **Description** に次の要件を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

4. **Create** を選択し、Issue を作成します。
5. 右側で **Assign to Copilot** を選択し、割り当てダイアログを開いて **Assign** を選択します。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

その後まもなく、Issue の最初のコメントに 👀 が表示され、Copilot が作業中であることが分かります。

![目の絵文字で Issue に取り組んでいることを示す Copilot](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。cloud agent はソフトウェアエンジニアと同じように非同期で作業するため、完了まで常に監視する必要はありません。完了後にレビューします。次はカスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使うと、非同期で作業するよう Copilot に Issue を割り当てられます。技術的負債への対応、新機能の作成、別のフレームワークからのコード移行などに利用できます。

次の内容を確認しました。

- コード生成環境のカスタマイズ。
- 操作を安全に行う方法。
- Issue の範囲を明確に定める重要性。
- Issue を Copilot に割り当てる方法。

cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントを作成して使います。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで説明したカスタム命令にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
