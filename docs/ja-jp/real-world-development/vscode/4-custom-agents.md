---
title: "演習 4 - カスタムエージェント"
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

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるようにしたいと考えています。最近のユーザーフィードバックでは、現在のダークテーマはテキストと背景色のコントラストが不十分で、読みにくいと感じる人がいることが分かりました。このアクセシビリティの問題に対処するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なので、できるだけ早く実装したいと考えています。機能の作成にはカスタムエージェントを使います。

この演習では、次のことを行います。

- 既存のアクセシビリティカスタムエージェントを確認する。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装する。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントが用意されています。内容を確認し、Copilot を導く方法を理解しましょう。

Codespace に戻り、アクセシビリティカスタムエージェントのファイルを確認します。ターミナルを開き、アクセシビリティ作業用に `main` から新しいブランチを作成します (演習 3 のフィルター PR とは分けておきます)。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドが含まれる YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには `name` と `description` の frontmatter が必要です。

3. 続くセクションを確認します。次の内容が記載されています。
   - アクセシブルな Web サイト向けのコードを生成する際の主な責務。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント選択メニューには、`.github/agents` で定義されたすべてのカスタムエージェントが表示されます。カスタムエージェントを選択すると、そのエージェントの指示とツールに応じたチャットセッションを開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

1. Chat ビューのエージェント選択メニューで、まだ **Agent** が選択されていない場合は選択します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント選択メニューを選択します。現在のエージェントが表示されます。既定では **default** です。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. アクセシビリティエージェントに次のプロンプトを送信します。

    ```plaintext
    サイトにハイコントラストモードを追加してください。ユーザーが設定できるハイコントラストの切り替えを用意し、設定をブラウザーのローカルストレージに保存してページの再読み込み後も維持してください。
    ```

5. Copilot Chat がコードベースを調べ、変更を提案して、プロジェクトファイルに編集を適用します。編集は、ファイルパスと差分を含めてチャット内に表示されます。
6. 編集が適用されると、セッション中にエージェントが変更したファイルの一覧を示す **Files changed** の表示が更新されます。

> [!NOTE]
> この作業には数分かかることがあります。Copilot は Astro コンポーネント、CSS、関連テストなどの既存ファイルを編集します。

次の演習では、作業中の内容をレビューして追加の指示を伝えます。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]を確認しました。カスタムエージェントは、特定のタスクや分野に合わせた専門的な AI アシスタントです。チームの専門知識と標準を再利用可能なエージェントにして、Copilot が作業をより効果的に進められるようにできます。

次の内容を学びました。

- カスタムエージェントの定義方法。
- Copilot Chat の agent mode でカスタムエージェントを使う方法。

次は[エージェントの作業を監視して誘導します][next-lesson]。変更を確認し、同じセッションでライトモードの切り替えを追加するよう依頼します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの設定][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントを監視して管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
