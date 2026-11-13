---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメントの標準を追加して、フィルターのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際はコンテキストが重要です。特定の方法でタスクを実行する必要がある場合、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]では、求めるコードだけでなく、コードの構成方法も説明できます。フィルターを作成したので、Copilot が使った指示を確認し、ドキュメントの標準を追加して、コードに適用してみましょう。

このレッスンでは、次のことを行います。

- リポジトリとパスに応じた指示ファイルがエージェントに適用されるしくみを確認します。
- コーディング標準が守られるように指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のようなものです。

- コメントでは、コードを言い換えるのではなく、目的や自明ではない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数では、目的、パラメーター、戻り値、および存在する場合は注入可能な `db` 引数を TSDoc/JSDoc で説明します。
- 再利用可能な Astro コンポーネントでは `Props` の仕様を説明し、関連コードの変更後もコメントを最新の状態に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使えば、Copilot に適切な情報を提供し、こうしたプラクティスに沿ってタスクを実行できます。

## 指示ファイル

カスタム指示を使うと、コンテキストや好みを Copilot に伝え、コーディングスタイルや要件を理解しやすくできます。これにより、より適切な提案やコードスニペットを得られます。好みのコーディング規約、ライブラリ、含めたいコメントの種類などを指定できます。リポジトリ全体、または特定の種類のファイルに対する指示ファイルを作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送信される単一の指示ファイルです。多くのリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル形式、タスクに関するガイドラインを記載します。

> [!NOTE]
> その他の指示形式やサポート状況は、環境によって異なります。特定の形式を利用する前に、[カスタム指示のサポートリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

作業を始める前に、スタータープロジェクトに含まれる指示ファイルを確認します。既定のプロジェクト指示ファイルと、パスに応じた指示ファイルが用意されています。

1. Codespace に戻ります。
2. Codespaces のエディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。これらの指示は、このリポジトリでの Copilot とのすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開き、各ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示が含まれています。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象のファイルを決める glob を設定していることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存プロジェクトファイルへのリンクを確認します。大きな指示を小さく再利用可能なファイルに分け、Copilot が参考にできる例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルは良い出発点ですが、追加すべき内容があります。新たに生成する TypeScript に TSDoc コメントを追加するよう、コアの `copilot-instructions.md` ファイルを変更します。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクション (35 行目付近) を探します。
2. 最後の箇条書きに次の項目を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。更新した指示を反映させつつ、機能のコンテキストを保つため、CLI を終了してフィルターの会話を再開します。

1. フィルターの会話がまだ開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートで `game-filters-cli` ブランチにいることを確認し、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示された場合は、フィルターの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新後のガイダンスを適用するよう依頼します。

   ```plaintext
   指示とコードガイダンスを更新しました。このガイダンスに合うよう、生成したコードを更新してください。
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルをどのように使うかを確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリとパスに応じた指示ファイルがエージェントに適用されるしくみを確認しました。
- コーディング標準が守られるように指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
