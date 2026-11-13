---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ課題、更新が必要なレガシーコード、実装する時間がなくバックログに残っている機能リクエストなど、さまざまな問題があります。GitHub Copilot cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフト PR を作成します。退屈な作業を任せて開発を加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次の内容を学びます。

- コード生成環境をカスタマイズする。
- 操作を安全に実行する。
- 範囲を明確に定めた課題が重要であることを理解する。
- Copilot に課題を割り当てる。

## シナリオ

Tailspin Toys には対処したい技術的負債があります。最初の Web サイトを作成した委託先がドキュメントを適切に整備しなかったため、ドキュメントがほとんどありません。まず、アプリケーションのすべてのエクスポート関数に TSDoc のドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のゲームを表示し、閲覧を続けられるようにします。まだ完成度の高い実装は必要なく、UX の受け入れテストに使える状態になれば十分です。この作業は現在ブロックされていますが、さらに優先度の高い課題があります。

どちらも、優先度が下がりやすいタスクの例です。Copilot cloud agent に割り当てれば、非同期で作業を進めてもらい、開発者は別のタスクに集中できます。その後で Copilot の作業をレビューし、期待どおりの結果になったことを確認できます。

## GitHub Copilot cloud agent とは

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。[GitHub の課題を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)方法などがあります。割り当てると、Copilot は進行状況を追跡するドラフトプルリクエストを作成し、環境をセットアップして、タスクに取り組み始めます。作業中でも完了後でも、Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確に定めた指示の重要性

GitHub Copilot に魔法はありません。考える手間を省けるわけではありません。一見簡単な操作でも、詳しく調べると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲設定に注意してください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階的に進め、学び、試し、必要に応じて調整してください。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

これまでの演習では、カスタム指示と Copilot を導く方法を学びました。すでにカスタム指示の演習を進めた場合は `.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、この機会に簡単に確認しましょう。Copilot cloud agent に作業を割り当てる前に、リポジトリに用意された指示ファイルを読み取り専用で確認すると、後でその影響を見つけやすくなります。

リポジトリの GitHub Web UI、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクション、特に TypeScript の規約と TSDoc のドキュメントコメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` frontmatter を確認します。この指示は `**/*.test.ts` に適用されます。

次のセクションで `Code lacks documentation` の課題を cloud agent に割り当てると、作成されたプルリクエストで、TSDoc のドキュメントコメント、TypeScript の規約、ファイルヘッダーコメントを確認できます。後の演習で PR をレビューする際にも取り上げます。

## Copilot cloud agent の開発環境をセットアップする

誰がコードを書く場合でも、一般的には特定の環境とセットアップスクリプトが必要です。タスクを Copilot に割り当てる場合も同様です。Copilot はソフトウェアエンジニアのように作業します。

cloud agent は GitHub Actions を使って作業環境を用意します。`.github/workflows/copilot-setup-steps.yml` に設定した[専用のセットアップワークフロー][setup-workflow]を作成すると、作業開始前に実行される環境をカスタマイズし、必要な開発ツールや依存関係を使用できるようにできます。このラボの流れを円滑にして学習機会を提供するため、あらかじめ設定されています。単一の Astro アプリで、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションとシードが済んだ SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *この手順*に必要な最小限の権限を設定します。Copilot は操作用に独自のトークンを受け取ります。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は、`contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動でクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (現在、アプリ全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを利用できるよう、ローカルの SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じですが、次の点に注目してください。

- `copilot-setup-steps` という 1 つのジョブがあります。このジョブは、Copilot がプルリクエストに取り組む前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot に実行されるまで待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は誰もが理解していますが、多くのプロジェクトでは情報が古かったり、まったく存在しなかったりします。こうした技術的負債は対処されないままになり、開発効率を下げ、コードベースの保守や新しいメンバーの受け入れを難しくします。Copilot はドキュメント作成を得意としているため、cloud agent に割り当てるのに適した課題です。バックグラウンドで必要なドキュメントを作成してもらい、後の演習でレビューします。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい課題ダイアログを開きます。
4. **Blank issue** を選択して、課題を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    関数とメソッドには、必要に応じて TSDoc のドキュメントコメントを含めることが、組織の要件です。しかし、最近の更新ではこの標準に従っていません。動作を明確にするドキュメントコメントが既存のコードに含まれるよう、更新する必要があります。
    ```

7. **Create** を選択して課題を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![課題に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前になります。PR が一覧に表示されない場合は、しばらく待ってからブラウザーを更新します。
12. 数分後、Copilot がタスクリストを作成していることを確認します。

> [!NOTE]
> Copilot のタスクリストが PR に表示されるまで、数分かかることがあります。Copilot は前の手順で説明したワークフローを実行して環境を用意し、プロジェクトを分析して、問題への最適な取り組み方を検討しています。

13. タスクリストを確認し、Copilot が実行する作業を確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が課題に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された状態が表示されない場合は、ウィンドウを更新してください。

16. ライブセッションをスクロールし、Copilot が問題に取り組む様子を確認します。コードを調査して状態を把握し、計画を考えて決定した後、コードを作成するところまで確認できます。

作業には数分かかることがあります。Copilot cloud agent の主な目的は、作業を非同期に行い、ほかのタスクに集中できるようにすることです。この機能を活用するため、もう 1 つのタスクを cloud agent に割り当ててから、アプリに機能を追加する作業を始めましょう。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分けられることです。デザインチームが求めている関連ゲームのセクションは、時間がかからないかもしれません。それでも、別の作業に使える時間を確保できます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい課題ダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

    ```markdown
    ゲーム詳細ページに関連ゲームを表示して、ゲームをさらに見つけやすくしたいと考えています。デザインチームは UX を確認し、受け入れテストを行う予定です。要件は次のとおりです。

   - `src/lib/` に、指定されたゲームと同じカテゴリに属する他のゲームを返すデータアクセスヘルパーを追加する。指定されたゲーム自体は結果から除外する
   - ゲーム詳細ページにこのヘルパーを使う「Related games」セクションを表示する
   - 関連ゲームがない場合に対応する
   - 新しいヘルパーのユニットテストを作成する
   - PR の作成前に、すべてのテストが成功することを確認する
    ```

7. **Create** を選択して課題を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![課題に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

その後まもなく、Copilot が課題に取り組んでいることを示す 👀 が課題の最初のコメントに表示されます。

![Copilot が課題に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組み始めました。Copilot cloud agent はソフトウェアエンジニアと同じように作業するため、常に監視する必要はありません。完了後にレビューすればよいのです。次は、カスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に課題を割り当てると、非同期で作業を進められます。技術的負債への対処、新機能の作成、あるフレームワークから別のフレームワークへのコード移行などに使えます。

次の内容を学びました。

- コード生成環境をカスタマイズする。
- 操作を安全に実行する。
- 範囲を明確に定めた課題が重要であることを理解する。
- Copilot に課題を割り当てる。

cloud agent がバックグラウンドで作業している間に、カスタムエージェントの作成と使用に取り組みましょう。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub の課題を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
