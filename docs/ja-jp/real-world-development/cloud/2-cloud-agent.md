---
title: "演習 2 - GitHub Copilot クラウド エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織はほとんどありません。解決されていないセキュリティ上の問題、更新が必要なレガシー コード、実装する時間がなくバックログに残り続ける機能のリクエストなど、さまざまな例があります。GitHub Copilot のクラウド エージェントは、コードの更新や機能の追加などを自律的に実行します。エージェントは作業を完了すると、人間の開発者が確認できるドラフト PR を作成します。これにより、退屈な作業を任せて開発プロセスを加速し、開発者はより大きな課題に集中できます。

Copilot クラウド エージェントを使って、次のことを確認します。

- コード生成の環境をカスタマイズします。
- 安全に作業が行われるようにします。
- 範囲を明確に定めた issue の重要性を理解します。
- Copilot に issue を割り当てます。

## シナリオ

Tailspin Toys には、解決したい技術的負債があります。サイトの初期バージョンを作成した委託業者は、ドキュメントを十分に整備していません。実際、ドキュメントがまったくありません。まず、アプリケーション内のすべてのエクスポートされた関数に TSDoc のドキュメント コメントを追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のタイトル、つまり関連ゲームを表示し、ユーザーが続けてゲームを探せるようにします。現時点では完成度の高い実装は不要で、UX の受け入れテストに使用できるものがあれば十分です。このタスクは現在ブロッカーですが、優先度の高い他の issue もあります。

このようなタスクは優先順位が下がりがちですが、Copilot クラウド エージェントに割り当てるのに適しています。クラウド エージェントは非同期で作業できるため、開発者は他の作業に集中し、後から Copilot の成果を確認できます。

## GitHub Copilot クラウド エージェントについて

[GitHub Copilot クラウド エージェント](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)は、人間の開発者と同様にバックグラウンドでタスクを実行できます。[GitHub issue を Copilot に割り当てる][assign-issue]など、いくつかの方法で作業を依頼できます。割り当てると、Copilot は進捗を追跡するドラフト プルリクエストを作成し、環境をセットアップして作業を開始します。進行中でも完了後でも Copilot のセッションを確認できます。提案された解決策を確認できる状態になると、プルリクエストで通知されます。

## 適切な範囲の指示が重要な理由

GitHub Copilot に魔法のような力があるわけではなく、考える必要がなくなるわけでもありません。一見単純な操作にも、詳しく調べると複雑さがあります。[クラウド エージェントに依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペア プログラマーとして扱い、段階を踏み、学び、試して、進め方を調整します。生成 AI を取り入れても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と、それが Copilot を導く方法を確認しました。カスタム指示の演習をすでに行っていれば、`.github/copilot-instructions.md` がどのように機能するかを確認できています。まだの場合は、ここで簡単に確認しましょう。Copilot クラウド エージェントに作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認し、後からその影響を見られるようにします。

リポジトリの GitHub Web UI、またはすでに起動している Codespace で、次のファイルを開いてください。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc のドキュメント コメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターを確認します。これは `**/*.test.ts` ファイルに指示を適用します。

次のセクションで `Code lacks documentation` issue をクラウド エージェントに割り当てると、指示ファイルに記載された TSDoc コメント、TypeScript の規約、コメント ヘッダーが、プルリクエストに反映されているか確認できます。後の演習で PR をレビューするときにも、この点に注目します。

## Copilot クラウド エージェントの開発環境を設定する

誰がコードを作成する場合でも、通常は特定の環境やセットアップ手順が必要です。Copilot にタスクを割り当てる場合も同様です。クラウド エージェントは SWE と同じような方法で作業します。

クラウド エージェントは、作業環境として [GitHub Actions][github-actions] を使用します。作業を開始する前に実行する[セットアップ ワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` ファイルに作成して、環境をカスタマイズできます。これにより、必要な開発ツールや依存関係を利用できるようになります。このラボでは学習をスムーズに進められるよう、あらかじめ構成されています。Copilot が Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、単一の Astro アプリ用にマイグレーションとシードを済ませた SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *各手順*に必要な最小限のアクセス許可を設定します。Copilot には操作用の独自トークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は、`contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体が Astro + Drizzle/libSQL になりました)
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

      # ビルドとテストに必要なデータを用意するため、ローカルの SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフロー ファイルと同じ形式ですが、次の点に注目してください。

- `copilot-setup-steps` という単一のジョブが含まれています。このジョブは、Copilot がプルリクエストに取り組む前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを指定しているため、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot が実行するまで待たずに、ワークフローが正常に実行されるかテストするのに便利です。

## ドキュメントを追加する

誰もがドキュメントの重要性を理解していますが、古い情報が残っていたり、ドキュメントがなかったりするプロジェクトは少なくありません。このような技術的負債は放置され、生産性が低下したり、コードの保守や新しい開発者の参加が難しくなったりします。Copilot はドキュメント作成を得意とするため、クラウド エージェントに割り当てるのに適した issue です。バックグラウンドで必要なドキュメントを生成するので、後の演習で確認できます。

1. 新しいブラウザー タブで、github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して新しい issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てのダイアログを開きます。

  ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しいプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようになります。一覧に表示されない場合は、しばらく待ってからブラウザーを更新してください。
12. 数分後、Copilot が TODO リストを作成したことを確認できます。

> [!NOTE]
> Copilot の TODO リストが PR に表示されるまで、数分かかる場合があります。Copilot は環境を作成し (前述のワークフローを実行します)、プロジェクトを分析し、問題に取り組むための適切な方法を検討しています。

13. リストを確認し、Copilot が実行しようとしているタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新後の表示を確認するため、ウィンドウの更新が必要になる場合があります。

16. ライブ セッションをスクロールして、Copilot が問題を解決する過程を確認します。コードの調査、状態の把握、計画を立てるための思考、コードの作成などが行われます。

完了まで数分かかる場合があります。Copilot クラウド エージェントの主な利点は、非同期で作業して他のタスクに集中できることです。この利点を活用して、別のタスクをクラウド エージェントに割り当ててから、アプリケーションに機能を追加する作業に取り組みましょう。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot クラウド エージェントの大きな利点の 1 つは、作業を分担できることです。デザイン チーム向けの関連ゲームの追加に時間がかからない場合でも、別の作業に時間を使えます。クラウド エージェントに割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てのダイアログを開きます。

  ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、issue の最初のコメントに 👀 が表示されます。Copilot が作業を始めたことを示しています。

![Copilot が issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot クラウド エージェントは SWE と同様に動作するため、常に監視する必要はありません。作業の完了後に確認すればよいので、カスタム エージェントの作成と使用に進みましょう。

## まとめと次のステップ

このレッスンでは、[GitHub Copilot クラウド エージェント][copilot-agents]について学びました。クラウド エージェントに issue を割り当てると、非同期で作業を任せることができます。技術的負債の解消、新機能の作成、あるフレームワークから別のフレームワークへのコード移行などに活用できます。

このレッスンでは、次のことを行いました。

- コード生成の環境をカスタマイズしました。
- 安全に作業が行われるようにしました。
- 範囲を明確に定めた issue の重要性を確認しました。
- Copilot に issue を割り当てました。

クラウド エージェントがバックグラウンドで作業している間に、カスタム エージェントの作成と使用に進みます。[Copilot クラウド エージェントは MCP サーバーも使用できます][cloud-agent-mcp]。また、前のモジュールで学んだカスタム指示も利用できます。

## リソース

- [Copilot クラウド エージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot クラウド エージェントのセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
