---
title: "演習 5: GitHub Copilot の作業を反復改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業を確認する

このラボでは、ユーザー エクスペリエンスの改善や機能の追加を目的とした複数のタスクを GitHub Copilot に依頼しました。コードへのドキュメントの追加、デザイン チームが改善できる関連ゲーム機能の作成、ハイ コントラスト モードとライト モードを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って作業を改善しましょう。

### シナリオ

これまでに何度も説明したように、生成 AI を導入しても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードを確認し、通常の DevOps プロセスに従う必要があります。まず、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案を確認し、その後、チームでレビューを有効にします。

## セキュリティと GitHub Copilot クラウド エージェント

Copilot クラウド エージェントは、監視されずに非同期でタスクを実行するため、安全性を保つための制約があります。

- Copilot はリポジトリに対する読み取りアクセスのみを持ち、書き込みは作業に使用するブランチに対してのみ許可されます。
- クラウド エージェントは GitHub Actions 内で実行され、作業用に一時的な環境を別途作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- [既定では外部リソースへのアクセスが制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーも対象です。

## 生成されたドキュメントを確認する

まず、GitHub Copilot クラウド エージェントが作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加を確認します。GitHub.com の通常の PR インターフェイスを使います。

> [!NOTE]
> PR の確認時に、GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。これは想定どおりです。Copilot は既定で外部リソースへのアクセスが制限されているため、外部 MCP サーバーへの呼び出しなども制限されます。必要に応じて、[Copilot クラウド エージェントのファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot が作業中の場合、プルリクエストには **[WIP]** の表示が付きます。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあるので、休憩したり、これまでに学んだことを振り返ったりしてください。

4. プルリクエストの準備ができたら、**Files changed** タブを選択して変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc のドキュメント コメントなど、新たに追加されたドキュメントを確認します。変更の内容は状況によって異なります。

    変更を確認するときは、TSDoc のドキュメント コメント、TypeScript の規約、コメント ヘッダーに注目します。これらは、演習 2 の最初に確認したカスタム指示ファイルに記載されています。

6. 更新内容を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示があることを確認します。
8. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. ワークフローがプルリクエストのチェック セクションにキューされたことを確認します。正常に進めば、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業するときは、一方向に進めるだけではありません。チームの他のメンバーと同じように、プルリクエストのコメントやコードのインライン コメントで Copilot にメンションすることができます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は予測できないため、依頼する文面を細かく指定することはできません。たとえば、次のような変更を依頼できます。

- 各コード ファイルの先頭に、そのファイルの概要を説明するコメント ヘッダーを追加します。
- TypeScript と Astro のファイルに TSDoc のドキュメント コメントを追加します。
- Astro アプリの構成を説明する README を作成します。

1. 上記の例、またはコードベースのドキュメントについて依頼したい内容を使って、**@copilot** にメンションし、生成されたドキュメントへの変更をコメントで依頼します。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot が更新のために新しいセッションを開始することに注目してください。
3. **Back to pull request** を選択してプルリクエストに戻れます。

    ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して変更を確認します。

問題がなくなるまで、作業の反復を続けてください。完了したら、ドラフトからレビュー可能な状態に変更し、`main` ブランチにマージできます。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能を確認する

ゲーム詳細ページに関連ゲームを表示する issue を解決するために、Copilot が作成した PR に戻りましょう。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** のようなタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 更新内容に問題がなければ、**Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示があることを確認します。
7. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. ワークフローが PR のチェック セクションにキューされたことを確認します。正常に進めば、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかることがあります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストすることもできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名 (たとえば **copilot/fix-8**) に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。これまでと同様に、Copilot クラウド エージェントに更新を依頼して反復改善できます。たとえば、表示する関連ゲームの数を調整したり、コメント ヘッダーと TSDoc のドキュメント コメントを追加したりできます (この issue はカスタム指示を更新する**前**に割り当てたことを思い出してください)。これまでと同じように、**Conversation** タブでコメントを追加すると Copilot が確認し、新しいセッションを開始します。

## アクセシビリティ機能を確認する

最後に、アクセシビリティ カスタム エージェントを使って実装したアクセシビリティ機能を確認します。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 でミッション コントロールから追加を依頼したライト モードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** のようなタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot が作業中の場合、プルリクエストには **[WIP]** の表示が付きます。作業が完了するまで待ってください。数分かかることがあります。

4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 実装を確認するときは、特に次の点に注目します。
   - 表示モードを切り替える UI コンポーネント
   - ユーザー設定を保存するためのローカル ストレージの利用
   - ハイ コントラスト モードとライト モードの CSS またはスタイル変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード ナビゲーションなど)
   - モードの切り替えを管理する JavaScript/TypeScript コード

6. 更新内容に問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示があることを確認します。
8. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. ワークフローが PR のチェック セクションにキューされたことを確認します。正常に進めば、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかることがあります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストすることもできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次に、アプリケーションを起動し、ブラウザーでハイ コントラスト モードとライト モードを切り替えて、ページを再読み込みしても設定が維持されることを確認します。

アクセシビリティ カスタム エージェントにより、アクセシビリティのベスト プラクティスに沿って機能が実装されることを確認できました。アクセシビリティに関する懸念や改善点があれば、前の PR と同様に、コメントで **@copilot** に変更を依頼できます。

## 任意の演習 — 委任を続ける

クラウド エージェントに実際のバックログ項目を任せて、他の作業に集中すると、その利点を活用できます。いくつか issue を作成して Copilot に割り当て、この方法を身につけましょう。たとえば、次のような項目があります。

- ゲーム詳細ページに支援者向けの関心フォームを作成します。
- ゲーム一覧ページにページネーションを実装します。
- データ アクセス ヘルパーに入力検証とエラー処理を追加します。

## まとめ

クラウド エージェントのハーネスを完了しました。各レッスンを通じて、次のことを行いました。

- このリポジトリに含まれるカスタム指示ファイルを確認し、クラウド エージェントの出力に影響することを確認しました。
- issue を Copilot クラウド エージェントに割り当て、環境のセットアップ、計画、非同期の作業を確認しました。
- アクセシビリティ用のカスタム エージェントを作成して使用し、ハイ コントラスト モードとライト モードを追加しました。
- Copilot エージェント ページをミッション コントロールとして使い、進行中のアクセシビリティ セッションを監視して誘導しました。
- Copilot のプルリクエストを確認して反復改善し、`@copilot` に変更を依頼して、ワークフローを承認しました。

## レビューと次のステップ

クラウド エージェントのハーネスを完了しました。引き続き学習する場合は、他のハーネスでも今回の内容を応用できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat のエージェント モードと MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェント スキル、カスタム QA エージェント、Playwright MCP、Agent Merge、任意の `/delegate` によるクラウド エージェントへの引き継ぎを使い、ターミナルから 2 つの変更をレビューして提供します。

自分のリポジトリでは、次のアイデアを試してみてください。

- リファクタリングやテスト カバレッジの issue をクラウド エージェントに割り当てる。
- 別の分野向けにカスタム エージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot クラウド エージェントのセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot クラウド エージェントのファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
