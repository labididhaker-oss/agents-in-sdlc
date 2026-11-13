---
title: "演習 5: GitHub Copilot の作業を反復的に改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントを管理する][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザーエクスペリエンスの改善や機能の追加を目的に、GitHub Copilot といくつかのタスクに取り組みました。コードにドキュメントを追加し、デザインチームが改善を検討できる関連ゲーム機能を作成し、ハイコントラストとライトモードの切り替えを含むアクセシビリティ機能を実装するよう依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを伝えて改善しましょう。

### シナリオ

何度も説明したように、生成 AI を導入しても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って進める必要があります。チーム全体によるレビューを有効にする前に、GitHub Copilot が提案したドキュメント、関連ゲーム機能、アクセシビリティ機能を確認しましょう。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期に、監督を受けずにタスクを実行するため、安全性を確保するためのセキュリティ制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みアクセスはコード用ブランチに限られます。
- cloud agent は GitHub Actions 内で実行され、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot cloud agent が作成した、コードへのドキュメント追加に関する最初のプルリクエスト (PR) を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に、GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot は MCP サーバーなどの外部リソースへのアクセスが既定で制限されているため、これは**想定どおりの動作**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** の表示があります。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. プルリクエストの準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく更新されたコードと、追加された TSDoc のドキュメントコメントなどを確認します。具体的な変更は異なります。

    変更を確認する際は、TSDoc のドキュメントコメント、TypeScript の規約、ファイルヘッダーコメントに注目してください。これらは演習 2 の最初に確認したカスタム指示ファイルによるものです。

6. 更新内容のレビューが終わり、問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄に、ワークフローがキューに追加されたことが表示されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業するのは、一方通行ではありません。チームメンバーと同じように、プルリクエストのコメントやコードのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は確率的に変わるため、決まった依頼文はありません。次のような修正を依頼できます。

- 各コードファイルの先頭に、ファイルの概要を説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 生成されたドキュメントへの変更を依頼するコメントを追加し、ユーザーと同じように **@copilot** をメンションします。上記の例を使うか、コードベースに必要なドキュメントについて別の変更を提案します。
2. **View Session** を選択し、Copilot が作業する様子を確認します。更新のために新しいセッションが始まります。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択し、変更をレビューします。

満足するまで改善を繰り返してください。準備ができたら、ドラフト PR をレビュー準備完了に変更し、`main` ブランチにマージできます。

![PR をレビュー準備完了に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

次は、ゲーム詳細ページに関連ゲームを表示する課題について、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新内容のレビューが終わり、問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示を確認します。
7. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄にワークフローがキューに追加されたことが表示されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前の作業と同じように、Copilot cloud agent に追加の変更を依頼し、反復的に改善できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーや TSDoc のドキュメントコメントを追加したりできます (この課題はカスタム指示を更新する前に割り当てました)。前の作業と同様に、**Conversation** タブでコメントを追加すると、Copilot が新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティカスタムエージェントを使って実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 の Mission Control で追加を依頼したライトモードの両方が含まれるはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** の表示があります。その場合は、作業が完了するまで数分待ちます。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注目します。
   - モードを切り替える UI コンポーネント
   - ユーザー設定を維持するローカルストレージの使用方法
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 更新内容のレビューが終わり、問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローがキューに追加されたことが表示されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリを起動し、ブラウザーでハイコントラストモードとライトモードの切り替えを試します。ページを再読み込みしても設定が維持されることを確認してください。

アクセシビリティカスタムエージェントが、Copilot にアクセシビリティのベストプラクティスに沿った実装を促したことを確認します。アクセシビリティ上の問題や改善点があれば、前の PR と同様に、コメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 委任を続ける

バックログの実際の項目を cloud agent に任せ、その間に別の作業に取り組むことができます。この習慣を身につけるために、リポジトリにいくつかの課題を作成して Copilot に割り当ててみてください。たとえば、次のようなアイデアがあります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンを通じて、次のことを行いました。

- **このリポジトリに用意されたカスタム指示ファイルを確認し**、cloud agent の出力に後でどう反映されるかを確認しました。
- **Copilot cloud agent に課題を割り当て**、環境のセットアップ、計画、非同期の実行を確認しました。
- アクセシビリティ用のカスタムエージェントを**作成して使用し**、ハイコントラストとライトモードの切り替えを追加しました。
- **Copilot エージェントページを Mission Control として使い**、アクセシビリティのセッションを監視して途中で指示を加えました。
- **cloud agent のプルリクエストをレビューして改善し**、`@copilot` に変更を依頼して、ワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent ハーネスを完了しました。さらに学びたい場合は、次のハーネスで今回の内容を応用できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE 内で Copilot Chat の agent mode と MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、任意の `/delegate` による cloud agent への連携を使い、ターミナルからレビュー済みの変更を 2 つリリースします。

自分のリポジトリでは、次のアイデアを試してください。

- リファクタリングやテスト範囲の課題を cloud agent に割り当てる。
- 別の分野向けのカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub の課題を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントを管理する][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
