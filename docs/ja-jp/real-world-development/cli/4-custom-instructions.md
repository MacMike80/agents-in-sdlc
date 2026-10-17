---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使ううえで、コンテキストは重要です。タスクを特定の方法で進める必要がある場合は、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]には、必要なコードだけでなく、コードの構成方法も記述します。フィルタリング機能を作成したので、Copilot が使った指示を確認し、ドキュメント標準を追加して、コードに適用しましょう。

このレッスンでは、次のことを行います。

- リポジトリの指示とパス単位の指示ファイルがエージェントに渡される仕組みを確認します。
- コーディング標準が守られるように指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

優れた開発チームである Tailspin Toys には、開発時に守るべきガイドラインと要件があります。たとえば次のとおりです。

- コメントではコードを言い換えず、意図や自明でない決定を説明します。
- `db/` と `src/lib/` のエクスポートされた関数では、`db` 引数があれば注入可能であることを含め、目的、パラメーター、戻り値を TSDoc/JSDoc で記述します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連コードを変更したときはコメントを最新に保ちます。
- 既存の書式と lint のガイダンスを維持します。

指示ファイルを使って Copilot に適切な情報を伝え、これらの開発慣行に沿った作業を促します。

## 指示ファイル

カスタム指示を使うと、Copilot がコーディング スタイルや要件を理解しやすくするためのコンテキストや設定を伝えられます。コーディング規約、ライブラリ、含めてほしいコメントの種類などを指定できます。リポジトリ全体、または特定の種類のファイルに対する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md`: リポジトリに関するすべてのリクエストで Copilot に送信される単一の指示ファイルです。多くのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md`: 特定の言語、ファイルの種類、タスクに関するガイドラインを指定します。

> [!NOTE]
> 他の指示形式とそのサポート状況は環境によって異なります。特定の形式を前提とする前に、[カスタム指示のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには指示ファイルが含まれています。変更の効果を確認する前に、既存の内容を調べましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングに関するガイダンスを確認します。この指示は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ レイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、指示を適用するファイルを決める glob を指定する `applyTo` フィールドを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルやプロジェクト ファイルへのリンクを確認します。指示を小さく再利用可能なファイルに分割し、例を参照するよう Copilot に伝えられます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルはよい出発点ですが、まだ不足があります。`copilot-instructions.md` を編集し、新しく生成される TypeScript に TSDoc コメントを追加する標準を指定します。

1. `.github/copilot-instructions.md` で **Code formatting guidance** セクションを探します。35 行目付近にあります。
2. 次の項目を最後の箇条書きとして追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は、会話の開始時にリポジトリの指示を読み込みます。指示を保存した後、フィルタリングの会話を終了して再開すると、機能のコンテキストを保持したまま、更新後のガイダンスを読み込めます。

1. フィルタリングの会話が開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチにあるリポジトリのルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合は、フィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. Copilot に更新済みのガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードの説明が正確であることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを得る方法を確認し、新しい標準を機能に適用しました。次のことを行いました。

- リポジトリの指示とパス単位の指示ファイルがエージェントに渡される仕組みを確認しました。
- コーディング標準が守られるように指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
