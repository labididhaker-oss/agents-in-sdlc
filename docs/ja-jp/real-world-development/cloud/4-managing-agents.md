---
title: "演習 4 - エージェントを監視して管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの数回の演習では、Copilot cloud agent にユーザーエクスペリエンスの改善や機能の追加に関する 3 つの別々のタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理するツールはいくつかあります。GitHub.com の[エージェントページ][agents-page]もその 1 つです。この Mission Control では、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。操作内容を調べたり、実行中のセッションに指示を加えたりできます。

このレッスンでは、次のことを行います。

- cloud agent のタスクを監視するために、エージェントページを確認します。
- 実行中のセッションに指示を追加し、機能を拡張します。

## シナリオ

ハイコントラストモードを作成するようエージェントに割り当てた後、チームはライトモードも追加するよい機会だと気付きました。サイトのスタイル更新と切り替え機能をすでに進めているため、ライトモードも同時に追加するのが合理的です。ハイコントラストに加えてライトモードも実装するよう、エージェントの作業を誘導します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状況を確認します。

1. [Copilot エージェントページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインの両方にタスクの一覧が表示されます。Copilot に割り当てた次のようなタスクを確認します。
    - コードベースのドキュメントを更新する。
    - ゲーム変更用の API を生成する。
    - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを選択します。Copilot が実行した次のようなタスクを確認できます。
    - リポジトリからコードをチェックアウトする。
    - Copilot が作業する環境を作成する。
    - MCP サーバーを設定する。
    - 割り当てられたタスクを完了するための各種作業を行う。

> [!NOTE]
> 表示される具体的な手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認してください。PR と変更されたファイルを表示し、追加で監視できます。

## cloud agent に追加の指示を伝える

実行中のタスクを確認できたので、ハイコントラストモードの作業にライトモードの切り替えも含めるよう Copilot に依頼します。

1. ハイコントラストモードに関するセッションを選択します。Copilot が付けた名前や作業状況によって、タイトルは異なる場合があります。

    ![Mission Control のアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業を始めたことが表示されるまで、数分間セッションを確認します。次のようなメッセージが表示されたら作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    ハイコントラストモードに加えて、ライトモードも追加してください。このモードにも切り替えを用意し、ユーザーが表示モードを選べるようにしてください。
    ```

    ![エージェントページで、Steer active session while Copilot is working ダイアログが強調表示された状態](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを認識し、作業に追加したことを確認します。

## Copilot に作業を任せる

前回と同様に、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中だった手順を終えた後、追加の依頼を反映します。

今回も数分かかります。これまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視し、誘導するための中心的な場所である Copilot エージェントページを確認しました。Mission Control では、実行中および完了したタスクを追跡し、作業内容をレビューして、進行中のタスクに追加の指示を送れます。

次の内容を学びました。

- Mission Control とエージェントページで cloud agent のタスクを監視する。
- 実行中のセッションに指示を追加し、機能を拡張する。

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成したプルリクエストを改善します][next-lesson]。

## リソース

- [Copilot エージェントページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復的に改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
