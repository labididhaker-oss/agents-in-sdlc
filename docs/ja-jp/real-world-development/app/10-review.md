---
title: "レッスン 10 - 振り返りと次のステップ"
description: "App のワークフロー、2 回の PR、キャンバス演習、再利用できる品質向上の方法を振り返り、関連リソースを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot app を使い、Tailspin Toys の一連のワークフローを進めました。具体的には、次のことを行いました。

- リポジトリを接続し、アプリのワークスペースと用意されたバックログを確認して、クイックチャットを試しました。
- 星評価に取り組むセッションを開始し、ブラウザーのキャンバスで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルターの課題から始め、**Plan** モードで方針を決め、**Autopilot** モードで実装し、**Interactive** モードでレビューしました。
- カスタム指示を追加し、既存の `quality-checks` スキルを調整して、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルターを確認しました。
- quality assurance (QA) カスタムエージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの確認内容を評価しました。
- フィルター変更全体をレビューし、2 回目の PR では **Agent Merge** を許可しました。
- 既存の Database Explorer キャンバスを使い、リポジトリに接続するトリアージ用キャンバスも作成してテストしました。

## 成果物

このワークショップには、更新済みの `main` からそれぞれ作成したブランチ上に、2 つの PR の節目があります。

1. **星評価:** ゲームカードに既存の `starRating` と未評価状態を明示して表示します。
2. **フィルターと品質ワークフロー:** フィルターを実装し、指示を更新して機能に適用し、`quality-checks` のレポートを調整し、QA プロファイルを作成して関連テストを追加します。

フィルターの計画から PR の作成までは、同じセッション、ワークツリー、ブランチを使いました。ワークショップを簡潔にするため、これらの作業を 1 つの PR にまとめました。その後、既存の Database Explorer を使い、PR ワークフローを繰り返さずにリポジトリ連携トリアージ用キャンバスを作成しました。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を使った Copilot のブラウザー操作など、いくつかの方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA は PR の前に、それらの結果と要件およびテスト範囲のレビューをまとめました。

追加するテストは、実際の不足を補うものであるべきです。QA の実行で新しいテストが不要と判断される場合もあります。ツールの不足、未実行のチェック、失敗は合格ではなく、対処が必要です。マージを許可する前にコードと証拠を確認し、変更後は影響を受ける証拠を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業の質を左右します。このワークショップでは、指示を更新し、スキルを調整し、QA プロファイルを作成し、MCP サーバーを設定して、キャンバスを作成しました。これらのカスタマイズをセッション間で再利用し、チームのニーズに合わせて調整してください。指示は標準を設定し、スキルは繰り返し実行できるタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続し、キャンバスは共有の対話型作業領域を提供します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。実装前に方針を考えるには **Plan**、焦点を絞った変更を進めながら関与するには **Interactive**、範囲が明確で独立したタスクに限って **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業には推論能力の高いモデルを選びます。

インフラと同じくらい、コンテキストも重要です。何を、なぜ、どのように作るのかを明確に説明すると、出力は大きく変わります。クイックチャットは、アイデアを本格的なセッションに進める前に範囲を決めるのに便利です。

## さらに試す

基本的なワークフローを学びました。次の機能も試してみてください。

- [**Automations**][using-automations] は、最近の作業の要約など、定期的または必要に応じて実行するタスクに使えます。採用する前にスケジュール、権限、範囲を確認してください。自動化の作成は次のステップであり、このワークショップには含まれません。
- **Rubber duck** で問題を説明し、実装前に要点を押さえたフィードバックを得ます。
- [`/chronicle`][chronicle] でセッション中の出来事を説明する文章を生成します。
- [Bring your own key (BYOK)][byok] を使うと、Ollama、Foundry Local、LM Studio のローカルモデルなど、独自のプロバイダーのモデルを利用できます。
- [Deep links][deep-links] を使うと、リポジトリ、セッション、プロンプトを指定してアプリを直接開けます。

## 次のステップ

どのツールも、使い続けることが上達への近道です。業務用のコードにも、趣味のコードにも、何年も作ろうと思っていた小さなアプリにも使ってみてください。学んだことをチームと共有し、チームの知見からも学びましょう。いつものように、ドキュメントも確認してください。

GitHub Copilot エコシステムをさらに試すには、[VS Code harness][vscode-harness]、[Copilot CLI harness][cli-harness]、[Cloud agent harness][cloud-harness]を確認してください。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の概要][getting-started]
- [GitHub Copilot app をカスタマイズする][customize]
- [Automations を使う][using-automations]
- [キャンバス拡張機能を使う][canvas-docs]

[vscode-harness]: ../../vscode/
[cli-harness]: ../../cli/
[cloud-harness]: ../../cloud/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[customize]: https://docs.github.com/copilot/how-tos/github-copilot-app/customize-github-copilot-app
[using-automations]: https://docs.github.com/copilot/how-tos/github-copilot-app/using-automations
[canvas-docs]: https://docs.github.com/copilot/how-tos/github-copilot-app/working-with-canvas-extensions
[chronicle]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/chronicle
[byok]: https://docs.github.com/copilot/how-tos/github-copilot-app/use-byok-models
[deep-links]: https://docs.github.com/copilot/how-tos/github-copilot-app/open-with-deep-links
