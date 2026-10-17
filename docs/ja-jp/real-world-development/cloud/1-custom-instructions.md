---
title: "演習 1 - カスタム指示 (クラウド エージェント)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot クラウド エージェント →][next-lesson]

生成 AI を使ううえで、コンテキストは重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、その情報にアクセスできるようにします。[指示ファイル][instruction-files]を使うと、Copilot に「何を」してほしいかだけでなく、「どのように」進めてほしいかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス単位の指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` に新しいリポジトリ共通の標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習ではプロンプトの「変更前と変更後」の比較は行いません。Copilot クラウド エージェントは GitHub issue に対して非同期で動作するため、リアルタイムで並べて効果を示すのが難しいためです。このハーネスの後半で、クラウド エージェントが作成したプルリクエストを確認するときに、指示ファイルの影響を確認できます。

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

## リポジトリ共通の標準を追加する

ここで行う編集は 1 つです。コード内のドキュメントを TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーで記述するという、プロジェクト全体のルールを追加します。後続の演習で issue を割り当てると、クラウド エージェントがこのルールを使います。

編集する前に、作業用のブランチを作成します。クラウド エージェントは issue の対象ブランチにある指示を読み取りますが、ここでは変更をきちんとコミットします。

1. Codespace のターミナルで、ブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を伝えるためのファイルです。コード提案を改善するため、ドキュメントの標準を追加しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクションを探します。27 行目付近にあります。プロジェクトのコーディング標準が記載されていますが、生成されるヘルパーにドキュメント コメントがなかったように、コード内ドキュメントのルールはありません。
3. 既存の標準のすぐ下に、次の Markdown 行を追加します。これにより、Copilot にファイル コメント ヘッダーと TSDoc のドキュメント コメントを追加するよう指示できます。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルは、リポジトリ レベル (`.github/copilot-instructions.md`) に作成して全体的に適用することも、特定の言語、ファイルの種類、タスク向けに `*.instructions.md` ファイルとして作成することもできます。ドキュメント コメントのルールのようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## 指示の更新をコミット、プッシュ、マージする

クラウド エージェントは、issue が対象とするブランチから指示ファイルを読み取ります。次の演習で Copilot に issue を割り当てるときは、Copilot は `main` からブランチを作成します。したがって、クラウド エージェントが指示を取得できるように、変更を `main` に反映しておく必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に対するプルリクエストを作成し、マージします。次の演習で割り当てる issue にクラウド エージェントが取り組むときに、これらの指示が読み込まれます。

> [!TIP]
> このワークショップで `main` ブランチを直接使う場合は、ブランチの作成を省略して `main` に直接コミットできます。ここでは実際のプロジェクトと同様の流れを体験するため、ブランチを使っています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る方法を確認し、リポジトリ共通の標準を `.github/copilot-instructions.md` に追加しました。このハーネスの後半では、クラウド エージェントが作成するプルリクエストで、追加した標準が適用されていることを確認できます。

次は、[Copilot クラウド エージェントに最初の issue を割り当て][next-lesson]ます。

## リソース

- [GitHub Copilot のカスタマイズに使用する指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を作成するための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
