---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認してレポート形式をカスタマイズし、フィルタリング機能の検証に使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことが開発のすべてではありません。手動でコードを検証し、指示ファイルを使って標準に従うようにしました。では、テストや lint など、継続的インテグレーション (CI) の他の作業はどうでしょうか。

このような作業には、**エージェント スキル**が適しています。スキルを使うと、Copilot がテストなどの操作を適切に実行できるようガイダンスを提供できます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、ユニット テスト、lint、型チェックに `quality-checks` スキルを使っています。チームは結果を読みやすくするためにレポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェント スキルは、再利用可能なタスクの指示、実行可能なスクリプト、エージェントが必要に応じて読み込む補助リソースをまとめたものです。基本的には、スキル名のフォルダーと、その中にある `SKILL.md` Markdown ファイルで構成されます。Markdown ファイルには、スキル名と説明を定義するフロントマター、スキルの概要、呼び出すタイミングに関するガイダンスがあります。呼び出し時に使うスクリプトやその他のリソースをサブフォルダーに含めることもできます。

> [!NOTE]
> スキルに追加ファイルやフォルダーは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトにあるコマンドを使うため、`SKILL.md` のみで構成されています。

スキルはチームが共有、再利用するリポジトリ内の `.github/skills` フォルダー、またはユーザーのスキル フォルダー `~/.copilot/skills` に配置できます。

## スキルを確認する

ユニット テスト、lint、型チェックを実行するために Tailspin Toys チームが作成した `quality-checks` スキルを確認します。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。説明は Copilot がスキルをいつ呼び出すか判断するために使います。
3. 指示を読んで、テストと lint の処理がどのように案内されているか確認します。
4. **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

Copilot CLI から直接、または自然言語を使ってスキルを呼び出せます。3 つのチェックを実行するよう Copilot に依頼します。

1. Interactive モードのフィルタリング会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

何を実行したか、成功したか、ツールがどのような結果を報告したかが分かる、よりよいレポートが必要です。スキルを更新してレポートを改善しましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

Copilot CLI は会話を再起動せずに、編集したスキルを再読み込みできます。変更を適用して動作を確認しましょう。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 前と**同じプロンプト**を使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後のレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして実行しました。次のことを行いました。

- ユニット テスト、lint、型チェック用の `quality-checks` スキルを確認しました。
- 結果の形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、機能の PR にフィルタリングと一緒に含まれます。次は [Playwright MCP サーバー][next-lesson]を使って、Copilot がサイトを直接操作できるようにします。

## スキルの例

コミュニティの次の例は参考情報です。追加の作業ではありません。

- [Agent Skills の仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
