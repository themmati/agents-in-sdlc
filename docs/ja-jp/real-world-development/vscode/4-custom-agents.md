---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせて、特化型の AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成してエージェントを定義し、目的に応じた命令、ベストプラクティス、コーディングパターン、専門知識を Copilot に提供できます。チームの専門知識を再利用可能なエージェントとしてまとめられます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディング慣行に従うセキュリティエージェント、一貫したテストパターンを維持するテストエージェントを作成できます。

カスタムエージェントは、プロジェクト内の `.github/agents` フォルダーか、グローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、命令を定義する Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、役割が重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。分かりやすく言うと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントには独自のコンテキストウィンドウがあり、作業の一部としてスキルやほかのエージェントを調整するよう設計されています。このラボでは、アクセシビリティのカスタムエージェントがサイトをアクセシビリティガイドラインに照らしてレビュー・更新します。その作業の一環として、プルリクエストのワークフロー用スキルや、テストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI のほかの使い方と同様に、環境やシナリオに合わせてテストと調整を行ってください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるよう取り組んでいます。現在のダークテーマではテキストと背景色のコントラストが不十分で、読みにくいというフィードバックがありました。このアクセシビリティ上の課題に対処するため、ユーザーが切り替えられるハイコントラストモードを実装します。

アクセシビリティは重要なので、カスタムエージェントを使って機能をすばやく作成します。

この演習では、次の内容を学習します。

- アクセシビリティ用のカスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに作成されています。内容を確認して、Copilot をどのようにガイドするかを理解します。

Codespace に戻り、ターミナルを開いて、フィルタリング PR とアクセシビリティの作業を分けるために `main` から新しいブランチを作成します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシビリティに配慮した Web サイトのコード生成で担う主な責任。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェントドロップダウンには、`.github/agents` で定義されたすべてのカスタムエージェントが表示されます。カスタムエージェントを選択すると、その命令とツールに合わせたチャットセッションになります。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

1. Chat ビューのエージェントドロップダウンから、まだ選択されていない場合は **Agent** を選びます。

   ![Chat ビューのエージェントピッカーのスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部にあるエージェントドロップダウンを選択します。既定ではアクティブなエージェントとして **default** が表示されています。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. アクセシビリティエージェントに次のプロンプトを送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べ、変更を提案して、プロジェクトファイルを編集します。編集内容は、ファイルパスとレビュー可能な差分とともにチャットに表示されます。
6. エージェントが変更した作業セットに合わせて、**Files changed** の表示が更新されます。

> [!NOTE]
> 実際のリポジトリを変更するため、数分かかる場合があります。作業中に Astro コンポーネント、CSS、関連テストなどの既存ファイルが編集されます。

次の演習で、進行中の作業をレビューして調整します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた AI アシスタントである、GitHub Copilot の[カスタムエージェント][custom-agents]について学びました。カスタムエージェントを使うと、チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot が作業を効率よく行えるようにガイドできます。

次の内容を確認しました。

- カスタムエージェントの定義方法。
- Copilot Chat の agent mode でカスタムエージェントを使う方法。

次は、エージェントの作業を[監視して調整し][next-lesson]、変更を確認しながら同じセッションにライトモードの切り替えを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
