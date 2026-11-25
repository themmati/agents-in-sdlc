---
title: "演習 1 - カスタム命令 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。[命令ファイル][instruction-files]を使うと、何を実行するかだけでなく、どのように実行するかも Copilot に伝えられます。

この演習では、次の内容を学習します。

- リポジトリのカスタム命令とパスに対応した命令ファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue に対して非同期で動作するため、リアルタイムで並べて効果を示すのが難しいためです。後の演習で cloud agent が作成したプルリクエストをレビューするときに、命令ファイルの影響を確認します。

## 命令ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。

- データレイヤーには、必ずユニットテストを用意します。
- UI はダークモードで、モダンな見た目にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

命令ファイルを使って、Copilot がこれらの慣行に沿ってタスクを実行するための情報を提供します。

### カスタム命令

カスタム命令を使うと、コンテキストや希望する動作を Copilot に伝え、コーディングスタイルや要件を理解させることができます。コーディング規約、ライブラリ、記述してほしいコメントの種類などを指定できます。リポジトリ全体や、特定のファイルの種類に対する命令を作成できます。

命令ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md`: リポジトリに関するすべてのリクエストで Copilot に送信される単一のファイルです。技術スタック、構築中の内容、ベストプラクティスなど、ほとんどのチャットや CLI リクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md`: 特定のタスクやファイルの種類に関する命令を作成できます。TypeScript や Astro などの言語、UI コンポーネントの作成やユニットテストの追加などのタスクに関するガイドラインを記載します。

> [!NOTE]
> IDE では、命令ファイルは Copilot Chat でのコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの命令と `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の命令ファイルにも対応しています。詳細は[カスタム命令のサポート][custom-instructions-support]を確認してください。

### 命令ファイルを管理するためのベストプラクティス

命令ファイルの作成について詳しく扱うことはこのワークショップの範囲外ですが、サンプルプロジェクトに代表的な方法が含まれています。

- `copilot-instructions.md` には、構築するものの説明、プロジェクト構成、全体に適用するコーディング標準など、プロジェクト全体に関するガイダンスを記載します。
- `*.instructions.md` には、ファイルの種類 (ユニットテスト、Astro コンポーネント、データレイヤー) や特定のタスクに関する命令を記載します。
- 自然な言葉を使い、明確なガイダンスと、コードの望ましい例や望ましくない例を示します。

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
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドは、命令が適用されるファイルを決める glob をリポジトリルートを基準として設定します。ここでは、たとえば `**/*.test.ts` に一致する TypeScript のテストファイルに命令が適用されます。
5. このプロジェクトのユニットテスト作成に関する命令を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて末尾まで読みます。`unit-tests.instructions.md` などの命令ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな命令セットを再利用可能な小さなファイルに分け、Copilot が参考にできる例を示せます。ここにあるパスはリポジトリルートではなく、命令ファイルの場所を基準とします。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトの標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で TSDoc コメントとファイルヘッダーのルールを追加します。

## リポジトリ標準を追加する

この演習で編集するのは、プロジェクト全体に適用するルールの追加だけです。cloud agent がこのハーネスの後半で Issue に取り組むときに、ファイルのコメントヘッダーと TSDoc ドキュメントコメントを追加するよう指示します。

cloud agent は Issue の対象ブランチから命令ファイルを読み込みます。次の演習で Issue を割り当てると、cloud agent は `main` からブランチを作るため、命令の変更を `main` にマージする必要があります。最初に作業用のブランチを作成します。

1. Codespace のターミナルで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を伝えるためのファイルです。コーディング標準を追加して、コード提案を改善します。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。現在のコーディング標準を確認します。コード内のドキュメントに関するルールがないため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準の直後に、ファイルコメントヘッダーと TSDoc コメントを追加する次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 命令ファイルは、リポジトリ全体に適用する `.github/copilot-instructions.md` と、特定の言語、ファイルの種類、タスクに適用する `*.instructions.md` として作成できます。今回追加したドキュメントコメントのルールのように、プロジェクト全体に適用する標準はリポジトリレベルのファイルに記載します。

## 命令の更新をコミット、プッシュ、マージする

cloud agent は Issue の対象ブランチにある命令ファイルを読み込みます。次の演習では `main` からブランチが作成されるため、命令の変更を `main` に反映しておきます。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを作成してマージします。次の演習で割り当てる Issue に取り組むときに、cloud agent がこの命令を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチの作成を省略して `main` に直接コミットできます。ここでは、実際のプロジェクトで行う方法に沿ってブランチを使っています。

## まとめと次のステップ

このプロジェクトの命令ファイルから Copilot がコンテキストを取得する仕組みを確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。このハーネスの後半で cloud agent が作成するプルリクエストに、追加した標準が反映されます。

次は、最初の Issue を [Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [効果的なカスタム命令を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム命令を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
