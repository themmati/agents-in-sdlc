---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace で Copilot CLI をインストールして認証し、基本を確認して、登録済みのフィルタリング Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作するエージェント型のコーディングアシスタントです。コードベースを調べ、コードを生成し、コマンドを実行し、外部ツールと連携できます。作業を任せたり、変更を依頼したりしながら、集中して作業できます。最初の手順として、使い慣れたツールを使って Copilot CLI をインストールします。

このレッスンでは、次の内容を学習します。

- npm を使って GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub MCP サーバーを使って、フィルタリングの Issue を見つけます。

## シナリオ

チームでは、増え続けるバックログに対応するため、AI エージェントを使い始めています。Copilot CLI を使うと、多くの開発者が日常的に作業するターミナルでその機能を利用できます。このレッスンでは、Copilot CLI をインストールして認証し、ワークショップの残りで利用できるようにします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew からインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、この演習では npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、バージョン要件を満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証を求められた場合は、device code の手順に従って認証し、Copilot CLI にアクセスを許可します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して信頼します。

> [!NOTE]
> Codespace では、GitHub セッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま続行できます。

## 基本を確認する

通常のシェルプロンプトでは、コマンドは Codespace で直接実行されます。Copilot CLI の起動後は、自然言語をエージェントに送信し、スラッシュコマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選んで <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストール済みのバージョンで利用できるコマンドを確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. Copilot に簡単な質問をして、正しく動作していることを確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 応答を読み、Copilot が回答前にリポジトリを調べることを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用可能であることを確認します。
6. 次のプロンプトでフィルタリングの Issue を探します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. 表示された URL を開き、Issue を読みます。最初の変更に取り組んだ後、この Issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトしているブランチを使います。自動的にワークツリーが作成されることはありません。各変更の前に機能ブランチを作成します。

## ワークショップ用のショートカットを使う

通常、Copilot CLI は許可された範囲外のツールを使う前に確認します。このワークショップでは、Codespace 内で承認プロンプトを省略して演習に集中できるよう、`--yolo` を付けて再起動します。

> [!CAUTION]
> `--yolo` は、`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls` を含む完全な自動権限を有効にします。Codespace や VM などの分離された環境でのみ使用し、日常の開発で既定値としてエイリアスに設定しないでください。詳細は[ツールの使用を許可または拒否する][allow-all-warning]を参照してください。

後のレッスンで Issue やプルリクエストを操作するため、`--enable-all-github-mcp-tools` を付けると読み取りと書き込みの GitHub MCP ツールが有効になります。Codespace はローカルコンピューターへのアクセスを制限しますが、認証された GitHub リソースは実際のものです。公開やマージの前に変更をレビューしてください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて簡単な質問をして会話が動作することを確認し、`/exit` で終了します。

Copilot は会話を自動的に保存します。後で命令を変更したりエージェントを追加したりした後に、`copilot --resume` を使って、同じ会話とブランチに戻ります。

## まとめと次のステップ

GitHub Copilot CLI をインストールして認証しました。このレッスンでは、次の作業を行いました。

- npm を使って Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼しました。
- 組み込みの GitHub MCP サーバーでフィルタリングの Issue を見つけました。

Copilot CLI をインストールできました。次は、ゲームカードに星評価を表示する[最初の変更][next-lesson]に取り組みます。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
