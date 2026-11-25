---
title: "演習 1 - カスタム命令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストにアクセスできるようにします。[命令ファイル][instruction-files]を使うと、コード生成についてのガイダンスを Copilot に伝えられます。

この演習では、次の内容を学習します。

- リポジトリのカスタム命令とパスに対応した命令ファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- フィルタリング用の最初のデータスライス (publishers ヘルパー) を、現在の命令のまま生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードが新しい標準を取り入れる様子を確認します。
- 命令の更新とフィルタリングのスライスを `main` にコミットし、次の演習で Copilot が更新したガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は確率的に動作します。この演習の目的は、命令を更新した後の傾向の変化を確認することであり、文字単位で同じ出力を再現することではありません。

## 命令ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。

- データレイヤーには、必ずユニットテストを用意します。
- UI はダークモードで、モダンな見た目にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

命令ファイルを使って、Copilot がこれらの慣行に沿ってタスクを実行するための情報を提供します。

### カスタム命令

カスタム命令を使うと、コンテキストや希望する動作を Copilot に伝え、コーディングスタイルや要件を理解させることができます。コーディング規約、ライブラリ、記述してほしいコメントの種類などを指定できます。リポジトリ全体や、特定の種類のファイルに対する命令を作成できます。

命令ファイルは次の 2 種類です。

- `.github/copilot-instructions.md`: リポジトリに関するすべてのリクエストで Copilot に送信される単一のファイルです。技術スタック、構築中の内容、ベストプラクティスなど、ほとんどのチャットや CLI リクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md`: 特定のタスクやファイルの種類に関する命令を作成できます。TypeScript や Astro などの言語、UI コンポーネントの作成やユニットテストの追加などのタスクに関するガイドラインを記載します。

> [!NOTE]
> IDE では、命令ファイルは Copilot Chat でのコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの命令と `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の命令ファイルにも対応しています。詳細は[カスタム命令のサポート][custom-instructions-support]を確認してください。

### 命令ファイルを管理するためのベストプラクティス

このワークショップでは、命令ファイルの作成について詳しく扱いませんが、サンプルプロジェクトに代表的な方法が含まれています。

- `copilot-instructions.md` には、構築するものの説明、プロジェクト構成、全体に適用するコーディング標準など、プロジェクト全体に関するガイダンスを記載します。
- `*.instructions.md` には、ファイルの種類 (ユニットテスト、Astro コンポーネント、データレイヤー) や特定のタスクに関する命令を記載します。
- 自然な言葉を使い、明確なガイダンスとコードの例を示します。

命令ファイルの作成方法は 1 つではありません。AI の使い方と同様に、試しながらプロジェクトに適した方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な命令ファイルを用意することをお勧めします。このプロジェクトには [UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク用の命令があります。
>
> Copilot に命令ファイルを生成してもらうこともできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、環境によって方法が異なります。各環境向けのレッスンで該当する方法を説明します。
>
> テンプレートや開始点を探している場合は、命令ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## プロジェクトのカスタム命令ファイルを確認する

このリポジトリに含まれる命令ファイルを読みます。中心となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** に関するガイダンスもあります。これらの内容は、Copilot を使ったすべての作業に適用されます。
3. `.github/instructions` フォルダーを開いて確認します。Astro ファイル、Drizzle データレイヤー、テストなどの命令があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリルートを基準とした glob を設定します。ここでは、たとえば `**/*.test.ts` に一致する TypeScript のテストファイルに命令が適用されます。
5. このプロジェクトのユニットテストを作成するための命令を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾を確認します。`unit-tests.instructions.md` などの命令ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな命令セットを再利用可能な小さなファイルに分け、コード生成時の参考例を示せます。ここにあるパスはリポジトリルートではなく、命令ファイルの場所を基準とします。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトの標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で TSDoc コメントとファイルヘッダーのルールを追加します。

## 変更用のブランチを作成する

ベストプラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 命令を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム命令の効果を確認するため、まず現在の命令ファイルを使ってプロンプトを送信します。後で命令を更新し、同じプロンプトをもう一度送信して違いを確認します。

1. 前の演習で使ったエディタータブを閉じ、Copilot に必要なコンテキストだけを渡します。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に示します。
3. Chat ビューのエージェントドロップダウンから **Agent** を選択し、ファイル変更を適用できるようにします。

   ![Chat ビューのエージェントピッカーのスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べ、ヘルパーファイルとテストを含むコードを更新します。
6. 提案されたヘルパーが、`db` クライアントを最初の引数に取り、型付きの出版社配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に基づいています。
7. 提案されたコードに TSDoc コメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、命令がなくてもドキュメントコメントが追加される場合があります。その場合も問題ありません。命令を更新した後の一貫性の向上を確認することが目的です。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を伝えるためのファイルです。コード提案を改善するために、コーディング標準を追加します。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する規則はありません。
3. 既存の標準の直後に、ファイルコメントヘッダーと TSDoc コメントを追加する次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、命令ファイルはリポジトリ全体に適用する `.github/copilot-instructions.md` と、特定の言語、ファイルの種類、タスクに適用する `*.instructions.md` として作成できます。今回追加したドキュメントコメントのルールのように、プロジェクト全体に適用する標準はリポジトリレベルのファイルに記載します。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をクリアします。
2. `src/lib/publishers.ts` を選択し、Copilot が適切なファイルに注目できるようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Tailspin Toys クラウドファンディングプラットフォームの出版社データアクセスヘルパーです。
    * データベースから出版社情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれることを確認します。

   ```typescript
   /**
    * すべての出版社の ID と名前の一覧を返します。
    *
    * @param db - Drizzle データベースクライアントです。
    * @returns 出版社オブジェクトの配列に解決される Promise です。
    */
   ```

Copilot に新しいプロジェクト標準を伝え、次の演習につながる実際のコードに適用できました。

## 命令とブランチをコミットしてプッシュする

命令ファイルもリポジトリ内のほかのファイルと同じように、ソース管理で管理します。命令とブランチをコミットしてプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. 次のコマンドを実行し、命令とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 命令の更新と、ヘルパーの基盤として Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の関連変更 (テストなど) が表示される場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の Issue に関連付けてプルリクエストを作成します。GitHub ツールを使って Copilot に Issue を探し、PR を作成してマージしてもらいます。

> [!NOTE]
> Copilot に GitHub 上の操作を依頼するのは今回が初めてです。プロジェクトテンプレートでは GitHub のツールがあらかじめ設定されています。Copilot が最初にツールを使うとき、VS Code から GitHub へのサインインを求められる場合があります。表示される手順に従って許可してください。後の演習では、この接続に使われる Model Context Protocol (MCP) を学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントピッカーで **Agent** が選択されていることを確認します。
3. 命令ファイルの更新に関する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。命令の変更と、今後のフィルタリング作業に使う publishers ヘルパーの基盤の両方を説明に含めます。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. GitHub の操作を許可するよう求められたら、コマンドを確認し、問題がなければ **Allow** を選択します。
6. PR が作成されたら、次のプロンプトを使って PR をマージし、ブランチを `main` に戻すよう Copilot に依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作やシェルコマンドを許可するよう求められたら、内容を確認して **Allow** を選択します。

これで GitHub Copilot の支援を使って、プルリクエストを作成してマージできました。

## まとめと次のステップ

Copilot がこのプロジェクトの命令からコンテキストを取得する仕組みを確認し、VS Code の Copilot Chat を使って次の作業を行いました。

- 既存の命令を使ったコード生成プロンプトを送信し、Copilot が作成するコードを確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映されることを確認しました。
- 命令の更新とヘルパーの基盤をコミットして `main` にプッシュし、次の演習で利用できるようにしました。

次は Copilot がコードベース全体に新機能を追加するときに、これらの命令を[agent mode][next-lesson]で使います。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [効果的なカスタム命令を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム命令を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム命令][personal-instructions]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
