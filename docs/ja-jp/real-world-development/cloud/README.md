---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウドで非同期に作業できます。GitHub で作業を割り当てると、cloud agent はバックグラウンドでリポジトリを調べ、変更を行い、プルリクエストを作成します。その間、別の作業を続けられます。

演習を通じて、cloud agent が従うカスタム指示を追加し、GitHub の課題を割り当てて実装を任せます。カスタムエージェントを使って方針を定め、エージェントのダッシュボードでセッションを監視・誘導し、最後にプルリクエストをレビューして結果を反復的に改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. カスタム指示][ex1] | コンテキスト | cloud agent が従うカスタム指示を追加する |
| [2. Cloud agent][ex2] | 非同期エージェント | Copilot に課題を割り当てる |
| [3. カスタムエージェント][ex3] | 専門エージェント | カスタムエージェントを確認して使用する |
| [4. エージェントを管理する][ex4] | 監視 | エージェントのセッションを監視して誘導する |
| [5. 反復的に改善する][ex5] | レビュー | PR をレビューし、Copilot の作業を改善して、次のステップを選ぶ |

## 前提条件

このワークショップに参加する前に、次のことを確認してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] アカウントまたは組織で Copilot cloud agent が有効になっている

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、すべてのハーネスを完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot cloud agent は Copilot Free を除く **Copilot Student、Pro、Pro+、Business、Enterprise** の各プランで利用できます。Copilot Business または Enterprise の場合は、管理者が組織で有効にする必要があります。

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
