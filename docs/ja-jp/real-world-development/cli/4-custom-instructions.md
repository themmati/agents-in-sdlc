---
title: "レッスン 4 - カスタム命令で Copilot をガイドする"
description: "リポジトリの命令を確認し、ドキュメントの規約を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合は、そのガイダンスを Copilot が利用できるようにします。[命令ファイル][instruction-files]には、求めるコードだけでなく、コードの構成方法も記述できます。フィルタリングを実装したので、Copilot が使用した命令を確認し、ドキュメントの規約を追加してコードに適用します。

このレッスンでは、次の内容を学習します。

- リポジトリの命令とパスに対応した命令ファイルがエージェントに適用される仕組みを確認します。
- コーディング規約を守るため、命令ファイルを更新します。
- 命令ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys では、開発手法に関するガイドラインと要件を定めています。たとえば、次のような内容です。

- コメントでは、コードの言い換えではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。該当する場合は、注入可能な `db` 引数も含めます。
- 再利用可能な Astro コンポーネントには `Props` の契約を記述し、関連するコードを変更した場合はコメントも最新の状態にします。
- 既存のフォーマットと lint のガイダンスを維持します。

命令ファイルを使うと、Copilot がチームの開発慣行に沿ってタスクを実行するために必要な情報を提供できます。

## 命令ファイル

カスタム命令を使うと、コンテキストや希望する動作を Copilot に伝え、コーディングスタイルや要件をより正確に理解させることができます。たとえば、コーディング規約、ライブラリ、記述してほしいコメントの種類を指定できます。リポジトリ全体に適用する命令と、特定のファイルの種類に適用する命令を作成できます。

命令ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md`: リポジトリに関するすべてのリクエストで Copilot に送信される、単一の命令ファイルです。大半のリクエストに関係するプロジェクトレベルの情報を記述します。
- `.github/instructions/*.instructions.md`: 特定の言語、ファイルの種類、タスクに関するガイドラインを記述します。

> [!NOTE]
> ほかの命令形式とサポート状況は、環境によって異なります。特定の形式を使用する前に、[カスタム命令のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## プロジェクトのカスタム命令ファイルを確認する

スタータープロジェクトには、すでに命令ファイルが含まれています。変更を加える前に、既存の内容を確認してみましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングガイダンスを確認します。この命令は、リポジトリでの Copilot とのすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開いて、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどの命令が含まれています。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドでは、命令が適用されるファイルを決める glob を指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの命令ファイルや既存のプロジェクトファイルへのリンクを確認します。大きな命令セットを再利用可能な小さなファイルに分け、Copilot が参考にできる例を示せます。

## チームのガイダンスに合わせて命令ファイルを更新する

既存のファイルは有用ですが、まだ不足があります。新しく生成する TypeScript に TSDoc コメントを追加するよう、中心となる `copilot-instructions.md` ファイルを変更します。

1. `.github/copilot-instructions.md` で、35 行目前後にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は、会話の開始時にリポジトリの命令を読み込みます。機能のコンテキストを保ったまま新しいガイダンスを適用するため、命令を保存した後にフィルタリングの会話を終了して再開します。

1. フィルタリングの会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   メッセージが表示されたら、フィルタリングの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. Copilot に次のプロンプトを送信し、更新したガイダンスを適用します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI がコンテキストを取り込む方法を確認し、新しい規約を機能に適用しました。このレッスンでは、次の作業を行いました。

- リポジトリとパスに対応した命令ファイルがエージェントに適用される仕組みを確認しました。
- コーディング規約を守るため、命令ファイルを更新しました。
- 命令ファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストを一貫して実行できるようにします。

## リソース

- [Copilot CLI のカスタム命令を追加する][instruction-files]
- [カスタム命令のサポート][custom-instructions-support]
- [効果的なカスタム命令を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
