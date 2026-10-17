---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

エージェント モードを使って、フィルタリング機能を作成しました。プルリクエストを作成する前に、ブラウザーで実際に動作することを確認しましょう。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot に Playwright MCP サーバーを使わせ、ブラウザーでフィルタリング機能を手動テストします。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェント モードは、エディターの外部にもアクセスできると、さらに強力になります。Copilot は Model Context Protocol (MCP) を使って外部にアクセスします。MCP はエージェントが外部のツールやサービスと通信するための標準的な方法です。

![エージェント モードの内部と、MCP サーバーや VS Code 拡張機能が提供するツールなど、コンテキスト、LLM、ツールとのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントが外部のツールやサービスと通信できます。MCP により、AI エージェントは外部のツールやサービスとリアルタイムに通信できます。その結果、リソースを使って最新の情報にアクセスしたり、ツールを使ってユーザーに代わって操作を実行したりできます。

これらのツールやリソースへのアクセスは、AI エージェントと外部のツールやサービスとの橋渡しをする MCP サーバーを通じて行います。MCP サーバーは、既存の API や NPM パッケージのようなローカル ツールなど、外部のツールとの通信を管理します。各 MCP サーバーは、AI エージェントが利用できる異なるツールとリソースを提供します。

よく使われる MCP サーバーの例を紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API を利用できます。AI エージェントは、新しいリポジトリの作成、既存リポジトリの更新、issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を利用できます。AI エージェントは Web ページを開き、フォームに入力し、ボタンを選択するなどの操作を実行できます。

ほかにも、さまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub は、エコシステムの発見や貢献を促すための [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティに関して、MCP サーバーはプロジェクトの他の依存関係と同様に扱ってください。MCP サーバーを使用する前に、ソース コードを慎重に確認し、公開元を検証して、セキュリティ上の影響を検討します。信頼できる MCP サーバーだけを使用し、機密リソースや操作へのアクセス許可には注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用できる MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーはすでに構成されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクト テンプレートに含まれているため、前の演習では Copilot が issue のバックログを読み取ることができました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使用するため、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと Copilot がブラウザーを操作できるため、機能をテストするのに適しています。

1. `.vscode/mcp.json` で、`github` と並べて `playwright` エントリを追加し、次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル** サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは、画面を表示せずに Playwright のブラウザーを実行するよう指定します。Codespace にはブラウザーを表示するデスクトップがないため、このフラグが必要です。

> [!NOTE]
> Tailspin Toys プロジェクトではエンドツーエンド テストに Playwright を使用しているため、通常、Playwright に必要なブラウザーはすでにインストールされています。Copilot が後でブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行して再試行するよう依頼してください。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。Copilot がサーバーのツールを初めて必要としたとき、VS Code はサーバーを起動し、信頼するかどうかを確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** アクションが表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code がサーバーを信頼するか確認した場合は、構成を確認し、サーバーを起動するために信頼することを選択します。

手動での起動は省略できます。省略した場合、次の手順で Copilot がツールを必要としたときに、VS Code がサーバーを起動し、信頼するか確認します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。アプリを自分でクリックして確認する代わりに、エージェントにページを開かせ、移動し、フィルターを適用して結果を読み取らせ、確認内容を要約してもらえます。会話を離れずに、機能が期待どおり動作するか確認できる最も速い方法です。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を基に操作します。つまり、支援技術と同じように、ボタン、リンク、リスト項目など、ラベルが付いた構造化要素をエージェントが処理します。そのため、機能の簡単な確認をアクセシビリティの基本的な確認にも活用できます。

サーバーを接続し、アプリを実行した状態で、作成したフィルタリング機能をテストするよう Copilot に依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して、確認した内容を報告します。その要約を issue の受け入れ条件と照らし合わせます。問題があれば、プルリクエストを作成する前に追加の質問をするか、コードを修正するよう依頼します。

> [!NOTE]
> このテストには、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止していた場合は、プロンプトを送る前に再起動してください。Copilot が Playwright MCP サーバーを初めて使用するときに、ブラウザーのダウンロードが必要な場合があります。ブラウザーがないと報告された場合は、`npx playwright install chromium` を実行して再試行するよう依頼してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたので、チームのレビュー用にプルリクエスト (PR) を作成しましょう。まず、`filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が加えた変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![Copilot が行った変更を表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンド ライン インターフェイス (CLI) など、プルリクエストを作成する方法はいくつかあります。今回は GitHub Copilot を使って PR を作成しましょう。Copilot は関連する issue を見つけ、issue と関連付けた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. Copilot が GitHub ツールを使えるよう、エージェント ドロップダウンで **Agent** が選択されていることを確認します。
3. Copilot に PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 情報の収集や操作の実行を許可するため、必要に応じて **Continue** を選択します。GitHub ツールを初めて使う場合は、GitHub への認証が求められることがあります。指示に従って許可します。
5. Copilot が issue を検索して、該当するものを見つけ、PR を作成することを確認します。
6. Copilot が作成したリンクを選択してプルリクエストを確認します。ただし、まだ**マージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーが Copilot にツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- 機能を公開する前に Playwright MCP サーバーを使い、ブラウザーでフィルタリング機能を手動テストしました。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成しました。

MCP サーバーを使って機能を**テスト**しましたが、Copilot を拡張する方法は MCP だけではありません。次は、アクセシビリティ レビューなどの特定の作業を効率化する[カスタム エージェントを作成し][next-lesson]ます。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
