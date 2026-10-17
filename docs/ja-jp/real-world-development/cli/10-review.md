---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、その他のリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、用意されているフィルタリングの issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリングの issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニット テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) カスタム エージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの確認内容を評価しました。
- フィルタリングの変更全体をレビューし、フィルタリングの PR で Agent Merge を許可しました。
- コンテキスト、モデル、共有、クラウド エージェントへの任意の委任に使うスラッシュ コマンドを確認しました。

## 作成したもの

このワークショップでは、更新済みの `main` から別々のブランチを作成して、2 つの PR を作ります。

1. **星評価**: 既存の `starRating` と評価がない状態を明示して、ゲーム カードに表示します。
2. **フィルタリングと品質ワークフロー**: フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して関連テストを追加します。

フィルタリングの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡略化するため、作業を 1 つの PR にまとめています。

## 異なる種類の検証

自動テスト、自分でのブラウザー確認、MCP を使った Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルはユニット テスト、lint、型チェックを実行し、カスタマイズした形式で結果を報告しました。QA は PR の作成前に、それらの結果と要件およびテスト カバレッジの確認内容をまとめました。

追加するテストは、実際の不足を補う必要があります。QA の実行で新しいテストが不要になる場合もあります。マージを許可する前に、コードと検証の根拠を確認し、変更後は影響を受ける検証結果を更新してください。

## ベスト プラクティス

Copilot に提供するコンテキストやツールが、作業内容に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して MCP サーバーを構成しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整してください。指示は標準を設定し、スキルは繰り返し可能なタスクを説明し、カスタム エージェントは専門家の役割を定義し、MCP サーバーは外部ツールと接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。作成前に方針を検討する場合は **Plan**、範囲を定めた変更に対話しながら取り組む場合は **Interactive**、明確に定義されたタスクを任せる場合は **Autopilot** を使います。定型的な変更には高速なモデルを、複雑な作業にはより高性能なモデルを選択できます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を作るのか、なぜ作るのか、どのように作るのかを明確に伝えると、出力が大きく変わります。

## さらに試す

中心的なワークフローを学びました。次の CLI 機能も確認してみましょう。

- `/review` を使って、コードレビュー エージェントに変更の分析を依頼します。
- `/rubber-duck` を使って、問題について会話し、別の視点を得る。
- `/fleet` を使って、独立したサブタスクを並行して調整します。
- `/worktree` を使って、別のタスクを分離します。
- `/delegate` を使って、Copilot クラウド エージェントにタスクを送信します。

## 次のステップ

ツールを上達させる最善の方法は、使い続けることです。業務のコード、趣味のコード、何年も作りたいと思いながら手をつけていなかったアプリなどで試してください。学んだことをチームで共有し、お互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。他の環境も試したい場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot アプリのワークショップ][app]、[Copilot クラウド エージェントのワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
