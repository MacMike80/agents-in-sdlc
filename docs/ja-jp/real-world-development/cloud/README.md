---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot クラウド エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot クラウド エージェント](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウド上で非同期に作業できます。GitHub で作業を割り当てると、クラウド エージェントはバックグラウンドでリポジトリを調査し、変更を加えてプルリクエストを作成します。その間、他の作業を続けられます。

この一連の演習では、まずクラウド エージェントが従うカスタム指示を追加し、GitHub issue を割り当てて実装を任せます。カスタム エージェントを確認して使用し、agents ダッシュボードからセッションを監視および誘導した後、プルリクエストを確認して結果を改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. カスタム指示][ex1] | コンテキスト | クラウド エージェントが従うカスタム指示を追加する |
| [2. クラウド エージェント][ex2] | 非同期エージェント | issue を Copilot クラウド エージェントに割り当てる |
| [3. カスタム エージェント][ex3] | 特化型エージェント | カスタム エージェントを確認して使用する |
| [4. エージェントの管理][ex4] | 監視 | エージェント セッションを監視して誘導する |
| [5. 反復改善][ex5] | レビュー | Copilot の作業を確認し、反復して次のステップを選ぶ |

## 前提条件

このワークショップを始める前に、次の要件を満たしていることを確認してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、または Enterprise** プランがある GitHub アカウント
- [ ] アカウントまたは組織で Copilot クラウド エージェントが有効になっていること

> [!TIP]
> 有料プランをお持ちでない場合、条件を満たす学生は[GitHub Education][callout-student-plan-education]を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用するエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、どのハーネスでも完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> クラウド エージェントは **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Free を除くすべてのプランが対象です。Copilot Business または Enterprise では、管理者が組織で有効にする必要があります。

> [!NOTE]
> このハーネスでは MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI ハーネス](../cli/)または [VS Code ハーネス](../vscode/)を参照してください。

## はじめに

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
