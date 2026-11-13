---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせて、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義すると、特定の作業を効果的に進めるための指示、ベストプラクティス、コーディングパターン、分野固有の知識を Copilot に提供できます。チームの専門知識を再利用可能なエージェントにできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディングプラクティスに従うセキュリティエージェント、一貫したテストパターンを適用するテストエージェントなどです。

カスタムエージェントは、プロジェクト内の `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter を記述し、その後にエージェントの動作、専門知識、指示を定義する Markdown プロンプトを続けます。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、役割が一部重複しています。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。分かりやすく区別するには、**カスタムエージェントは作業者、スキルはツール**と考えます。

カスタムエージェントは独自のコンテキストウィンドウを持ち、作業の一部としてスキルや他のエージェントを調整するために作られています。このラボのアクセシビリティカスタムエージェントは、アクセシビリティのガイドラインに照らし合わせてサイトを確認し、更新します。その作業の一環として、プルリクエストワークフロー用のスキルやテストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI を使う他の作業と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタムエージェントを使って、次のことを学びます。

- カスタムエージェントの定義方法。
- カスタムエージェントへのタスクの割り当て方。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるようにしたいと考えています。最近のユーザーフィードバックでは、現在のダークテーマはテキストと背景色のコントラストが不十分で、読みにくいと感じる人がいることが分かりました。このアクセシビリティの問題に対処するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なので、できるだけ早く実装したいと考えています。機能の作成にはカスタムエージェントを使います。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントが用意されています。内容を確認し、Copilot を導く方法を理解しましょう。

Codespace に戻り、アクセシビリティカスタムエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドが含まれる YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには `name` と `description` の frontmatter が必要です。

3. 続くセクションを確認します。次の内容が記載されています。
   - アクセシブルな Web サイト向けのコードを生成する際の主な責務。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## 課題を作成して割り当てる

Mission Control は、環境内のすべてのエージェントを操作する場所です。Copilot cloud agent にタスクを割り当て、進行状況を監視し、実行中のセッションに追加のガイダンスを伝えられます。まず、ハイコントラストモードの作成を Copilot に割り当てます。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい課題ダイアログを開きます。
4. **Blank issue** を選択して、課題を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    Web サイトにハイコントラストモードを追加してください。ユーザーが設定できるハイコントラストの切り替えを用意し、設定をブラウザーのローカルストレージに保存してください。
    ```

7. **Create** を選択して課題を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. 利用可能なカスタムエージェントの一覧から **Accessibility agent** を選択します。

    ![cloud agent の割り当て画面で、カスタムエージェントと Accessibility が選択されている](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を始めます。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]を確認しました。カスタムエージェントは、特定のタスクや分野に合わせた専門的な AI アシスタントです。チームの専門知識と標準を再利用可能なエージェントにして、Copilot が作業をより効果的に進められるようにできます。

次の内容を学びました。

- カスタムエージェントの定義方法。
- カスタムエージェントへのタスクの割り当て方。

Copilot がハイコントラストモードを実装している間に、[Mission Control からエージェントのセッションを監視して誘導します][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使う準備をする][org-custom-agents]
- [Enterprise でカスタムエージェントを使う準備をする][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントを監視して管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
