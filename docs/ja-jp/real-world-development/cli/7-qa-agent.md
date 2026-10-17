---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上のフィルタリングを確認しました。次は、それらの機能を明確な QA プロセスを持つカスタム エージェントにまとめます。

このレッスンでは、次のことを行います。

- カスタム エージェントが指示、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成して調べます。
- QA エージェントを選び、フィルタリングの issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys は、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザーでの動作を一貫した方法で確認したいと考えています。カスタム エージェントを使って QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する Copilot の特化版です。プロファイルでは、エージェントの目的、指示、利用可能なツールを指定します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズにはそれぞれ異なる役割があります。リポジトリの指示はチームの標準を説明し、`quality-checks` スキルは繰り返し可能なチェックをまとめ、Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、Copilot がそれらをどのように使って要件を評価し、結果を報告するかを指定します。それらの代わりになるものではなく、新たな会話も必要ありません。

## QA プロファイルを作成する

機能の PR を作成する前に、再利用可能な QA プロファイルを作成するよう Copilot に依頼します。プロファイルには QA が行うチェックと、守るべき制約を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、新しいカスタム エージェントの作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA ワークフローと制約を正しく記述したことを確認します。これにより、検証のみを依頼したのに機能が変更されるといった事態を防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを読みます。`description` は必須です。`name` は省略可能ですが、指定するとエージェントの表示名が明確になります。
3. プロファイルの指示を読み、要件の確認、リポジトリ指示への準拠、`quality-checks` スキルの実行、Playwright MCP の利用が含まれることを確認します。
4. QA が根拠を示して報告し、実装コードの変更前に確認を求め、変更をコミットしたり PR を作成したりしないことを確認します。
5. 必要な責務や制約が不足している場合は、続行する前に既定のエージェントに修正を依頼します。

## issue を基に QA を実行する

Copilot CLI は、会話の開始時にプロジェクト エージェントを読み込みます。プロファイルを作成した後で同じフィルタリングの会話を再開し、QA を選択すると、すでに共有されている issue と計画の決定事項を利用できます。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成直後は、利用可能なエージェントの一覧に表示されない場合があります。エージェントは存在するため、上記のコマンドで有効にできます。

2. 次のプロンプトを使って QA エージェントに機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 完了後にレポートを確認します。

## まとめと次のステップ

ワークフローに再利用可能な専門家の役割を追加し、その結果をレビューしました。次のことを行いました。

- カスタム エージェントが指示、スキル、MCP ツールと連携する方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して調べました。
- QA エージェントを選び、フィルタリングの issue に照らして結果をレビューしました。

これで、実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次は、[これらを機能の PR にまとめて Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
