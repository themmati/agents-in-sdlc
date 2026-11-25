---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、複数ファイルの変更やテスト実行などが必要になる場合があります。開発者は通常、必要なファイルを探し、変更し、テストを実行し、問題をデバッグして、変更漏れのファイルを見つけて再度更新します。Copilot Agent Mode は、こうした作業に役立ちます。

Copilot Agent Mode は IDE 内で、より自律的に作業するよう設計されています。まず既存のプロジェクト構成を調べ、必要な更新を行い、テストなどのタスクを実行し、問題があれば自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる使い方を想定しています。Agent mode は IDE 内で作業し、すばやいフィードバックとやりとりを可能にします。Cloud agent はチームメンバーのように非同期で作業し、Issue やプルリクエストを通じて連携します。

この演習では、次の内容を学習します。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定し、複数の変更を調整する方法。
- UI とデータレイヤーにまたがる新機能を GitHub Copilot Agent Mode で実装する方法。
- Copilot Agent Mode が生成した変更とテストを、コードベースにマージする前にレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリと発行元で絞り込めるようにします。前の演習では publishers ヘルパーを追加しました。次は Copilot Agent Mode を使い、データレイヤー、UI、テストの残りの作業を行います。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の現在の機能を確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームを一覧表示し、詳細を表示できます。単一の Astro アプリであり、ビルド時に静的 HTML としてページを生成します。ページは frontmatter 内で Drizzle ORM を使ってローカル SQLite データベースを直接検索します。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、インタラクティブな機能は標準 DOM API を使う小規模な Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習が終わり、`main` にいることを確認します。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的にリダイレクトされます。これは Web サーバーをホストしている Codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認できます。

- **ホームページ**: ボードゲームの一覧と、タイトル、画像、説明を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、さらに機能を追加したいと考えています。まずバックログを確認します。テンプレートからプロジェクトを設定すると GitHub Issue のバックログが自動的に作成されるため、GitHub Copilot に一覧を表示してもらいます。

1. Chat ビューのエージェントドロップダウンから **Agent** を選択します。Agent は自律的に計画し、複数のファイルを更新し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェントピッカーのスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案の一貫性を高めることが目的ですが、LLM は確率的に動作するため、記載された内容と異なる提案が表示されることがあります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が異なる MCP コマンドを使う場合がありますが、タスク自体は完了できるはずです。

3. 次のプロンプトを Copilot に送信して、バックログについて質問します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot がユーザーに最も役立つと判断した項目に基づいて、優先順位も付けていることを確認します。

## 命令ファイルを確認する

コードの生成を始める前に、Copilot にコンテキストを提供する命令ファイルを確認します。Web サイトに機能を追加する方法を説明する[ユーザーインターフェイス (UI) 命令ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 次のような、機能を追加するときの全体的なガイダンスを確認します。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle などのファイル形式に応じた命令へのリンク。

> [!TIP]
> 命令ファイルから、ほかの命令ファイルやプロジェクト内のファイルを参照できます。パスは命令ファイルの場所からの相対パスです。命令を再利用し、複雑な内容を管理しやすい単位に分け、例やテンプレートを示せます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも次の 3 つの変更が必要です。

- データレイヤー (`src/lib/`) の残りのフィルタリングロジックを追加または調整します。
- フィルタリングの動作を確認するテストを追加または更新します。
- ゲーム一覧ページを更新し、フィルタリング UI を追加します。

さらに、コードベースにマージする前にテストが成功する必要があります。Copilot Agent Mode にこれらの作業を依頼します。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、命令ファイルを追加します。

   ![UI 命令ファイルの選択例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェントピッカーのスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルに **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、目的の機能に関連するファイルを探します。データレイヤーのヘルパー、UI、テストを見つけてから、ファイルの変更とテストの実行を始める様子を確認します。

   ![Copilot がプロジェクトファイルを調べるスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイル変更、テスト実行などを行うため、タスクやコードベースの複雑さによっては数分かかります。処理中にコードエディターに **Keep** と **Undo** が表示されることがあります。作業中に選択する必要はありません。Copilot が完了すると、変更全体に対して **Keep** または **Undo** を選べるようになります。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![テストの実行を確認する Copilot Chat のダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。Copilot はタスクを完了し、エラーがないと判断するまで、コード生成とテストを繰り返します。

   ![Copilot Agent Mode の完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を試します。
11. すべて動作し、コードのレビューも終わったら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加する方法を確認しました。次の内容を学びました。

- GitHub Copilot Agent Mode で UI とデータレイヤーを横断する新機能を実装する方法。
- プロジェクトを調べ、関連ファイルを特定し、複数の変更を調整する方法。
- コードベースにマージする前に、生成された変更とテストをレビューする方法。

次は[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### ボーナス演習 - ページネーションを実装する

ゲームの一覧が増えるとページネーションが必要です。この演習で学んだことを使って、Copilot にページネーションの実装を依頼してみましょう。

- 既存の命令ファイルを使うなど、現在のベストプラクティスに従います。
- ページサイズをユーザーが選択できるようにするか、固定にするかなど、実装方法を検討します。
- 希望どおりの実装を作成できるよう、プロンプトに必要なガイダンスを含めます。
- Copilot に変更を依頼したりコンテキストを追加したりして、繰り返し改善する場合があります。これは Copilot を使う通常のワークフローです。

## リソース

- [Copilot の ask、edit、agent mode: それぞれの機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
