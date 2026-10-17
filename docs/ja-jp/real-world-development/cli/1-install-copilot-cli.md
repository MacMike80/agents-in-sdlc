---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、基本操作を確認して、用意されているフィルタリングの issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] はターミナルで動作する強力なエージェント型コーディング アシスタントです。コードベースを調べ、コードを生成し、コマンドを実行し、外部ツールを利用できます。すべてコマンド ラインから実行できるため、作業に集中できます。まずツールをインストールします。使い慣れたツールを使ってインストールできます。

このレッスンでは、次のことを行います。

- npm を使って GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込み GitHub MCP サーバーを使ってフィルタリングの issue を見つけます。

## シナリオ

チームでは、増え続けるバックログに対応するため、AI エージェントを使い始めています。Copilot CLI は、多くの開発者が日常的に使うターミナルでその機能を提供します。このレッスンでは、インストールと認証を済ませ、ワークショップの残りで使えるようにします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、この演習では npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されるはずです。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されるはずです。

## GitHub で認証する

Copilot CLI を初めて起動すると、GitHub アカウントでの認証が求められます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証が求められたら、device code の手順に従って Copilot CLI を認証し、利用を許可します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されているパスが Tailspin Toys のリポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択します。

> [!NOTE]
> Codespace では、GitHub のセッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めて問題ありません。

## 基本操作を確認する

通常のシェル プロンプトでは、コマンドは Codespace で直接実行されます。Copilot CLI が起動すると、自然言語はエージェントに送られ、スラッシュ コマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストール済みバージョンで利用できるコマンドを確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. Copilot が動作することを確認するため、簡単な質問をします。

   ```plaintext
   What are the key files in this project?
   ```

4. 応答を読み、回答の前に Copilot がリポジトリを調査することを確認します。
5. `/mcp list` を入力し、組み込み GitHub MCP サーバーが利用できることを確認します。
6. Copilot にフィルタリングの issue を探すよう依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて issue を読みます。最初の変更を終えた後に使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトしているブランチを使い、自動的に worktree を作成しません。変更を始める前に機能ブランチを作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は通常、設定済みの権限を超えるツールを使う前に確認を求めます。このワークショップでは `--yolo` を付けて再起動し、Codespace 内で承認プロンプトを省略します。

> [!CAUTION]
> `--yolo` はすべての権限 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を自動的に許可します。Codespace や VM などの分離された環境でのみ使用し、日常的な開発の既定値として alias に設定しないでください。詳しくは[ツールの使用を許可または拒否する][allow-all-warning]を参照してください。

`--enable-all-github-mcp-tools` を付けると、後のレッスンで issue やプルリクエストを扱うために使う GitHub MCP の読み書きツールが有効になります。Codespace からローカル コンピューターにはアクセスできませんが、認証済みの GitHub リソースは実際のものです。公開やマージの前に変更を確認してください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて簡単な質問をし、会話が動作していることを確認してから `/exit` で終了します。

Copilot は会話を自動的に保存します。後で指示を変更したりエージェントを追加したりした後、`copilot --resume` を使って同じ機能の会話とブランチに戻ります。

## まとめと次のステップ

GitHub Copilot CLI のインストールと認証が完了しました。このレッスンでは、次のことを行いました。

- npm を使って GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、簡単な会話を試しました。
- 組み込み GitHub MCP サーバーを使ってフィルタリングの issue を見つけました。

次は、最初の小さな変更を始め、ゲーム カードに星評価を表示するために [Copilot CLI を使います][next-lesson]。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
