---
title: "演習 2 - Copilot エージェント モードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションへの簡単な変更でも、通常は複数のファイルを更新し、テストを実行するなどの作業が必要です。開発者は、必要なファイルを探し、変更を加え、テストを実行し、問題をデバッグし、変更し忘れたファイルを特定して、さらに更新を加えるといった作業を行います。こうした作業は際限なく続くことがあります。

そこで役立つのが Copilot エージェント モードです。

Copilot エージェント モードは、IDE 内でより自律的に動作するように設計されています。開発者と同じように、まず既存のプロジェクト構成を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動で修正します。エージェント モードを使って、サイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェント モードとクラウド エージェントは異なる利用体験を目的としています。エージェント モードは IDE 内でタスクを実行し、短いフィードバック サイクルと対話を可能にします。クラウド エージェントはチームの一員のように非同期で作業するペア プログラマーとして設計されており、issue とプルリクエストを通じてやり取りします。

この演習では、次の方法を学びます。

- Copilot エージェント モードでプロジェクトを調査し、関連ファイルを特定して、複数のファイルを連携して変更します。
- GitHub Copilot エージェント モードで UI とデータ レイヤーにまたがる新機能を実装します。
- コードベースにマージする前に、Copilot エージェント モードが生成した変更とテストを確認します。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリや発行元でゲームを絞り込めるようにします。前の演習で発行元を取得するヘルパーを追加しました。次は、Copilot エージェント モードを使って、データ レイヤー、UI、テストの残りの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認して、現在の機能を把握しましょう。

この Web サイトは、開発者向けテーマのボード ゲーム クラウドファンディング プラットフォームです。ユーザーはゲームを一覧表示し、詳細を確認できます。Web サイトは単一の Astro アプリで、ビルド時にページを静的 HTML としてレンダリングします。ページは Drizzle ORM を使い、フロントマターからローカルの SQLite データベースに直接クエリします。別個のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` にあり、対話機能は標準 DOM API を使用する小規模でスコープを限定した Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習の後、`main` にいるはずです。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、Codespace 内に新しいターミナルを開きます。
3. フィルタリング作業用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を示すバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう求めるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナル内の Astro サーバー アドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択し、Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost URL のリンクを選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは Codespace をホストするプライベート トンネルです。

### Web サイトを確認する

Web サイトを起動したら、その機能を確認できます。主な機能は次のとおりです。

- **ホーム ページ**: タイトル、画像、説明付きのボード ゲーム一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの詳細ページが表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルは演習ごとに異なります。該当する手順で説明します。

Web サイトの最初の実装は機能しますが、新しい機能を追加して改善します。まずバックログを確認しましょう。テンプレートからプロジェクトを設定すると、自動的に issue のバックログが作成されます。GitHub Copilot に issue の一覧を表示するよう依頼します。

1. Chat ビューのエージェント ドロップダウンで **Agent** を選択します。**Agent** エージェントは自律的に計画を立て、複数のファイルを変更し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント ピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用できるモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨しているわけではありません。ワークショップの作成時には Claude Sonnet 4.5 を使用したため、ここでも同じモデルを指定しています。安定した学習体験を提供するためですが、LLM は確率的に動作するため、ワークショップに記載された内容と異なる提案が表示されることがあります。これは正常であり、想定されています。

> [!NOTE]
> LLM の確率的な性質上、Copilot が異なる MCP コマンドを使うことがありますが、タスクは完了できるはずです。

3. 次のプロンプトを送信して、バックログの issue について Copilot に尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot が、ユーザーにとって最も役立つと思われる項目を基に優先順位まで付けていることを確認しましょう。

## 指示ファイルを確認する

コードの生成をエージェントに依頼する前に、Copilot にコンテキストを伝えるための指示ファイルを確認しましょう。Web サイトへの機能追加の方法を説明する [ユーザー インターフェイス (UI)][ui-instructions] ファイルを使います。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加のための全般的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要
   - コンポーネント設計、テスト容易性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzle など、ファイルの種類ごとの指示ファイルへのリンク

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所からの相対パスです。これにより、指示を再利用し、複雑な内容を小さなファイルに分割して、例やテンプレートを提示できます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくともアプリケーションに次の 3 つの変更が必要です。

- データ レイヤー (`src/lib/`) に残りのフィルタリング ロジックを追加または調整します。
- フィルタリングの動作をテストするためのテストを追加または更新します。
- ゲーム一覧ページにフィルタリング UI を追加します。

さらに、コードベースにマージする前にテストを実行し、成功することを確認します。Copilot エージェント モードにこれらの作業を任せましょう。

1. 現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント ピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルとして **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログの関連 issue に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関係するファイルを探す様子を確認します。データ レイヤーのヘルパー、UI、テストを見つけてからファイルを変更し、テストを実行します。

   ![Copilot がプロジェクト ファイルを調べる様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースの複雑さによって、完了まで数分かかることがあります。作業中、コード エディターに **Keep** と **Undo** ボタンが表示されることがあります。作業が完了するとすべての変更に対して **Keep** または **Undo** を選択できます。作業中に選択する必要はありません。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![Copilot Chat ペインに表示される、テスト実行の確認ダイアログを示すスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストに失敗したりする場合があります。問題ありません。Copilot はタスクを完了してエラーが検出されなくなるまで、コード生成とテストを繰り返します。

   ![Copilot エージェント モードの完了したチャット セッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードを確認することは、常に重要です。

10. Web サイトを開いているブラウザーに戻り、新機能を確認します。
11. 問題なく動作することを確認し、コードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

おめでとうございます。この演習では、GitHub Copilot エージェント モードを使って Tailspin Toys の Web サイトに機能を追加する方法を確認しました。

- GitHub Copilot エージェント モードで UI とデータ レイヤーにまたがる新機能を実装できることを学びました。
- Copilot エージェント モードでプロジェクトを調べ、関連ファイルを特定し、連携して変更できることを学びました。
- コードベースにマージする前に、Copilot エージェント モードが生成した変更とテストを確認できることを学びました。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### ボーナス演習 — ページ分割を実装する

ゲームの一覧が増えると、ページ分割が必要になります。この演習で学んだスキルを使って、Copilot にページ分割を実装するよう依頼します。次の点を検討してください。

- 指示ファイルを使用するなど、既存のベスト プラクティスに従います。
- ユーザーがページ サイズを選択できるようにするか、固定にするかなど、ページ分割の方法を検討します。
- 希望する実装に必要なガイダンスを、プロンプトで Copilot に伝えます。
- 変更を依頼したりコンテキストを提供したりして、GitHub Copilot と反復改善します。Copilot と作業する際の一般的な流れです。

## リソース

- [Copilot の ask、edit、agent モード — それぞれの機能と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
