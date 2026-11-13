---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローを進めました。具体的には、次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、用意されたフィルターの課題を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルターの課題から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示を追加し、既存の `quality-checks` スキルを調整して、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルターを確認しました。
- quality assurance (QA) カスタムエージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの証拠を評価しました。
- フィルターの変更全体をレビューし、フィルターの PR で Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使うスラッシュコマンドを確認しました。

## 成果物

このワークショップでは、更新済みの `main` からそれぞれ作成したブランチ上で、2 つの PR を完成させます。

1. **星評価:** ゲームカードに既存の `starRating` と未評価状態を明示して表示します。
2. **フィルターと品質ワークフロー:** フィルターを実装し、指示を更新して機能に適用し、`quality-checks` のレポートを調整し、QA プロファイルを作成して関連テストを追加します。

フィルターの計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を使った Copilot のブラウザー操作など、いくつかの方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA は PR の前に、それらの結果と要件およびテスト範囲のレビューをまとめました。

追加するテストは、実際の不足を補うものであるべきです。QA の実行で新しいテストが不要と判断される場合もあります。ツールの不足、未実行のチェック、失敗は合格ではなく、対処が必要です。マージを許可する前にコードと証拠を確認し、変更後は影響を受ける証拠を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業の質を左右します。このワークショップでは、指示を更新し、スキルを調整し、QA プロファイルを作成し、MCP サーバーを設定しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整してください。指示は標準を設定し、スキルは繰り返し実行できるタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。実装前に方針を考えるには **Plan**、焦点を絞った変更に関わり続けるには **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラと同じくらい、コンテキストも重要です。何を、なぜ、どのように作るのかを明確に説明すると、出力は大きく変わります。

## さらに試す

ワークショップの主要な内容を学びました。Copilot CLI には、次のような機能もあります。

- `/review` でコードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を説明し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` でタスクを Copilot cloud agent に送ります。

## 次のステップ

どのツールも、使い続けることが上達への近道です。業務用のコードにも、趣味のコードにも、何年も作ろうと思っていた小さなアプリにも使ってみてください。学んだことをチームと共有し、チームの知見からも学びましょう。いつものように、ドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
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
