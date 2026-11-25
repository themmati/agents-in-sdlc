---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルタリングを検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、ソフトウェア開発で行う作業はあります。これまでは、コードが動作することを手動で検証し、命令ファイルを使って規約に従うようにしました。テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

このようなタスクには、**エージェントスキル**が適しています。スキルを使うと、こうした処理を適切に実行する方法を Copilot が理解できます。

このレッスンでは、次の内容を学習します。

- 既存の `quality-checks` スキルを確認します。
- 結果の書式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、ユニットテスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするため、レポートを改善したいと考えています。

## 命令、スクリプト、リソース

エージェントスキルには、再利用可能なタスクの命令、実行可能なスクリプト、補助リソースをまとめられます。基本的には、スキル名のフォルダーと、その中にある `SKILL.md` で構成されます。Markdown ファイルには、スキルの名前と説明を定義するフロントマター、機能の概要、呼び出すタイミングのガイダンスを記述します。スキルによっては、呼び出されたときに使用するスクリプトなどのリソースをサブフォルダーに含められます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトに既存のコマンドを使うため、`SKILL.md` だけで構成されています。

エージェントスキルは、チームが共有・再利用できるリポジトリアセットとしてプロジェクトの `.github/skills` フォルダーに配置するか、ユーザーのスキルフォルダー `~/.copilot/skills` に配置できます。

## スキルを確認する

Tailspin Toys のチームが作成した、ユニットテスト、lint、型チェックを実行する `quality-checks` スキルを確認します。

1. Codespace に戻ります。Codespaces のエディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。説明は、Copilot がスキルを呼び出すタイミングを判断するのに役立ちます。
3. 命令を読み、テストと lint の処理をどのようにガイドしているか確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更を加える前にスキルを実行する

Copilot にスキルの 3 つのチェックを実行してもらいます。スキルは Copilot CLI から直接呼び出すことも、自然言語で実行を依頼することもできます。

1. Interactive モードでフィルタリングの会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認します。

## レポートをカスタマイズする

実行したチェック、成功したかどうか、ツールが報告した内容を把握できる、より良いレポートが必要です。スキルを更新して、この形式のレポートを作成できるようにします。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの最後に、次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を加えたので、スキルを実行してみましょう。Copilot CLI では、会話を再起動せずに編集したスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 前とまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして実行しました。このレッスンでは、次の作業を行いました。

- ユニットテスト、lint、型チェック用の `quality-checks` スキルを確認しました。
- 結果の書式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、機能の PR に含めます。次は、[Playwright MCP サーバー][next-lesson]を介して Copilot がサイトを直接操作できるようにします。

## スキルの例

次のコミュニティの例は参考資料であり、追加の作業ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とバンドルされたエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
