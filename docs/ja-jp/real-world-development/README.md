---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更の計画、コードの作成、動作のテスト、プルリクエストのレビュー、エージェントを使った作業の管理を反復して行います。

## シナリオ

あなたは、開発者向けテーマのボード ゲーム クラウドファンディングを提供する架空の会社 Tailspin Toys に、新しい開発者として参加しました。チームのバックログは GitHub issue としてすでに登録されているため、すぐに取り組めます。アクセシビリティやコーディング標準などの品質改善に加えて、フィルタリングやページネーションなどの機能開発が含まれています。

## 作業環境を選ぶ

GitHub Copilot は、どこで作業していても利用できます。自分の開発方法に合った環境を選んでください。各ワークショップには個別のセットアップ手順があり、共通の Tailspin Toys シナリオを使います。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、指示、スキル、カスタム エージェント、Playwright MCP、Agent Merge、実用的なスラッシュ コマンドを扱います。

### [GitHub Copilot アプリ][app]

デスクトップ アプリでエージェント セッションを並行して実行し、セッション モードを切り替え、Canvas で共同作業し、GitHub issue とプルリクエストを管理します。

### [GitHub Copilot クラウド エージェント][cloud]

GitHub issue を非同期エージェントに割り当て、作業を誘導し、進捗を監視して、エージェントが作成したプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat のエージェント モード、MCP サーバー、カスタム エージェントを利用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
