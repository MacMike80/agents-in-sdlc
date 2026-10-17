---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使ううえで、コンテキストは重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、その情報にアクセスできるようにします。Copilot とコンテキストを共有する方法はいくつかあります。特に重要なのが[指示ファイル][instruction-files]です。指示ファイルを使って、コード生成に関するガイダンスを提供できます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス単位の指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 現在の指示を使い、フィルタリング用の最初のデータ スライス (発行元ヘルパー) を生成します。
- `.github/copilot-instructions.md` に新しいリポジトリ共通の標準を追加します。
- 同じプロンプトを再実行し、新しい標準に従ったコードが生成される様子を確認します。
- 指示とフィルタリングの変更をコミットして `main` に反映し、次の演習で Copilot が更新後のガイダンスを使えるようにします。

> [!CAUTION]
> 生成されたコードが指定した標準から外れる場合があります。Copilot は決定論的ではありません。この演習の目的は、出力を一字一句同じにすることではなく、指示の更新後に動作がどのように変わるかを確認することです。

## 指示ファイル

### シナリオ

優れた開発チームである Tailspin Toys には、開発時に守るべきガイドラインと要件があります。たとえば次のとおりです。

- データ レイヤーには必ずユニット テストが必要です。
- UI はダーク モードで、モダンな見た目にします。
- コードには TSDoc のドキュメント コメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使って Copilot に適切な情報を伝え、これらの開発慣行に沿った作業を促します。

### カスタム指示

カスタム指示を使うと、Copilot がコーディング スタイルや要件を理解しやすくするためのコンテキストや設定を伝えられます。コーディング規約、ライブラリ、含めてほしいコメントの種類などを指定して、より適切な提案やコード スニペットを得るために活用できます。リポジトリ全体、または特定の種類のファイルに対する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md`: リポジトリに関するすべてのリクエストで Copilot に送信される単一の指示ファイルです。Copilot Chat や CLI に送るほとんどのリクエストに関係する、プロジェクト レベルの情報を記載します。使用する技術スタック、開発するものの概要、ベスト プラクティス、全体的なガイダンスなどが適しています。
- `.github/instructions/*.instructions.md`: 特定のタスクやファイルの種類に使う指示ファイルです。TypeScript や Astro などの言語、UI コンポーネントやユニット テストの作成などのタスクに対するガイドラインを指定できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat でのコード生成にのみ使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot クラウド エージェントは、リポジトリ レベルの指示ファイルと、`applyTo` フロントマターがある `*.instructions.md` ファイルの両方を使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、他の標準形式の指示ファイルにも対応しています。[指示ファイルでサポートされる形式][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成について詳しく説明することはこの演習の範囲外ですが、サンプル プロジェクトでは一般的な方法を紹介しています。基本的には、次の点に留意してください。

- `copilot-instructions.md` には、開発するものやプロジェクトの構成、全体的なコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` は、ユニット テスト、Astro コンポーネント、データ レイヤーなど、ファイルの種類や特定のタスクに関する指示に使います。
- 自然な言葉で、明確なガイダンスと例を示します。コードの例には良い例と避けるべき例の両方を含めます。

AI の使い方に唯一の正解がないように、指示ファイルの作り方にも唯一の正解はありません。試行錯誤を重ねて、プロジェクトやシナリオに適した方法を見つけてください。

> [!TIP]
> GitHub Copilot を使用するすべてのプロジェクトに、充実した指示ファイルを用意することをお勧めします。このプロジェクトのファイルを確認すると、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスクに対応する指示があります。
>
> Copilot に指示ファイルを生成してもらうこともできます。各環境での方法は異なります。たとえば、VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。該当するレッスンで詳しく説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタム エージェント、その他のリソースを集めたリポジトリ [awesome-copilot][awesome-copilot]を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを読んでみましょう。共通の `copilot-instructions.md` が 1 つと、さまざまなタスクに対応する複数の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要に加え、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内にある **GitHub Actions Workflows** のガイダンスにも注目してください。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示を適用するファイルを決める glob です。この例では、`**/*.test.ts` に一致する TypeScript テスト ファイルが対象になります。
5. このプロジェクトでユニット テストを作成するときの指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` の末尾を開きます。`unit-tests.instructions.md` などの他の指示ファイルや、プロジェクト内のファイルへのリンクがあります。大きな指示を再利用可能な小さなファイルに分割し、例を参照するよう Copilot に伝えられます。ここにあるパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内ドキュメントの要件はまだありません。次の手順で、TSDoc のドキュメント コメントとファイル コメント ヘッダーに関するルールを追加します。

## 変更用のブランチを作成する

ベスト プラクティスに従って、変更用のブランチを作成しましょう。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する**前**に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルは演習ごとに異なります。該当する手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。後で指示ファイルを更新して同じプロンプトをもう一度送り、違いを確認します。

1. 前の演習で開いたエディター タブを閉じ、Copilot に必要なコンテキストだけが伝わるようにします。
2. Copilot がヘルパーの作成場所を把握できるよう、`src/lib/publishers.ts` を開きます。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント ピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べ、ヘルパー ファイルやテストなど、複数のファイルにコードの変更を加えます。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを受け取り、型付きの発行元配列を返す型付き関数であることに注目します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約に従ったものです。
7. 提案されたコードには、TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメント コメントを追加する場合があります。その場合も問題ありません。指示を更新した後に一貫性が高まることが、この演習のポイントです。

## リポジトリ共通の標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を伝えるためのファイルです。コード提案を改善するため、リポジトリのコーディング標準を追加しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクションを探します。27 行目付近にあります。プロジェクトのコーディング標準が記載されていますが、生成されたヘルパーにドキュメント コメントがなかったように、コード内ドキュメントのルールはありません。
3. 既存の標準のすぐ下に、次の Markdown 行を追加します。これにより、Copilot にファイル コメント ヘッダーと TSDoc のドキュメント コメントを追加するよう指示できます。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルは、リポジトリ レベル (`.github/copilot-instructions.md`) に作成して全体的に適用することも、特定の言語、ファイルの種類、タスク向けに `*.instructions.md` ファイルとして作成することもできます。ドキュメント コメントのルールのようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をクリアします。
2. `src/lib/publishers.ts` をクリックして、Copilot が正しいファイルに集中できるようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されていることを確認します。

   ```typescript
   /**
    * Tailspin Toys クラウドファンディング プラットフォーム用の発行元データ アクセス ヘルパーです。
    * データベースから発行元情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * すべての発行元を ID と名前とともに返します。
    *
    * @param db - Drizzle データベース クライアントです。
    * @returns 発行元オブジェクトの配列を解決する Promise です。
    */
   ```

これで新しいプロジェクト標準に従うよう Copilot を導き、次の演習で使用する実際のコードに適用できました。

## 指示をコミットしてブランチをプッシュする

指示ファイルもリポジトリ内の他のファイルと同様、ソース管理を使って管理します。指示とブランチをコミットして、リポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. ターミナルで、指示の更新と、ヘルパーの基礎作業として Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の関連する更新 (テストなど) が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルからブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたので、バックログのドキュメント標準に関する issue に関連付けたプルリクエストを作成しましょう。手動で作成することもできますが、プロジェクトに接続済みの GitHub ツールを使って Copilot に依頼できます。Copilot に issue を見つけさせ、それを解決する PR を作成してマージします。

> [!NOTE]
> この演習では初めて、Copilot に GitHub 上の操作を依頼します。プロジェクト テンプレートでは GitHub のツールがワークスペースに接続済みのため、セットアップは不要です。Copilot が初めてツールを使うとき、VS Code で GitHub へのサインインを求められる場合があります。画面の指示に従って許可してください。この接続 (Model Context Protocol) の仕組みは後の演習で学びます。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する issue を探し、現在のブランチから PR を作成して、指示の更新と発行元ヘルパーの基礎作業を説明するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を見つけて PR を作成します。
5. Copilot による GitHub 操作を **Allow** するよう求められたら、コマンドを確認し、適切な操作を許可します。
6. PR の作成後、次のプロンプトを使って PR をマージし、ブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot による GitHub 操作とシェル コマンドの実行を **Allow** するよう求められたら、コマンドを確認し、適切な操作を許可します。

これで GitHub Copilot の助けを借りてプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- コード生成のプロンプトを送り、*既存の*指示で Copilot が生成する内容を確認しました。
- `.github/copilot-instructions.md` に新しいリポジトリ共通の標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードが新しい標準に従うことを確認しました。
- 次の演習で利用できるよう、指示とヘルパーの基礎作業をコミットして `main` に反映しました。

次は、Copilot がコードベース全体に新機能を追加する際に[エージェント モード][next-lesson]で指示を活用します。

## リソース

- [GitHub Copilot のカスタマイズに使用する指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Copilot 向けのカスタム指示を作成するための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
