---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使う際はコンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot が背景情報を知る必要がある場合、その情報にアクセスできるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。中でも重要なのは、コード生成のガイダンスを提供する[指示ファイル][instruction-files]です。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わるしくみを確認します。
- 現在の指示のまま、フィルター機能の最初のデータ部分 (パブリッシャーヘルパー) を作成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。
- 同じプロンプトをもう一度実行し、生成されたコードが新しい標準に従う様子を確認します。
- 指示の更新とフィルター機能の作業をコミットし、次の演習で Copilot が更新済みのガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot の出力は確率的に変わるためです。この演習では、一字一句同じ結果ではなく、指示の更新後に動作がどう変化するかを確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のようなものです。

- データレイヤーには、必ずユニットテストを追加します。
- UI はダークモードにし、現代的な印象にします。
- コードには TSDoc のドキュメントコメントを追加します。
- 各ファイルの先頭に、ファイルの機能を説明するコメントを追加します。

指示ファイルを使い、Copilot が適切な情報を参照し、こうしたプラクティスに沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コンテキストや好みを Copilot に伝え、コーディングスタイルや要件を理解しやすくできます。コーディング規約、ライブラリ、コードに含めたいコメントの種類などを指定でき、より適切な提案やコードスニペットを得られます。リポジトリ全体、または特定の種類のファイルに対する指示ファイルを作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発中のものの概要、ベストプラクティス、全般的なガイダンスなど、ほとんどのチャットや CLI リクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル形式に対する指示ファイルです。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの追加などに関するガイドラインを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には適用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示と `applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の指示ファイルにも対応しています。[カスタム指示のサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成方法を詳しく説明することは、このワークショップの範囲外です。サンプルプロジェクトには、代表的な方法を示す例があります。基本的には、次のようにします。

- `copilot-instructions.md` には、開発中のもの、プロジェクトの構成、全体的なコーディング標準など、プロジェクト全体に関係するガイダンスを記載します。
- `*.instructions.md` には、ファイル形式 (ユニットテスト、Astro コンポーネント、データレイヤーなど) や特定のタスクに関する指示を記載します。
- 自然な言葉で、明確なガイダンスを書きます。コードの良い例と避けるべき例も示します。

指示ファイルの作成方法は 1 つではありません。AI の使い方と同じように、実際に試しながらプロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルのコレクションを用意してください。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスクに向けたファイルがあります。
>
> Copilot に指示ファイルを作成してもらうこともできます。利用する環境によって操作は異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。詳細が関係する場合は、該当する環境のレッスンで説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェント、その他のリソースをまとめたリポジトリ [awesome-copilot][awesome-copilot]を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

リポジトリに含まれる指示ファイルを読みましょう。コアの `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内にある **GitHub Actions Workflows** の指示にも注目してください。これらは、Copilot を使うすべての作業に適用されます。
3. `.github/instructions` フォルダーを開いて、内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示が含まれています。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリのルートからの相対 glob を設定していることを確認します。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルに適用されます。
5. このプロジェクトでユニットテストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて末尾までスクロールします。他の指示ファイル (`unit-tests.instructions.md` など) やプロジェクト内の既存ファイルへのリンクがあります。大きな指示を小さく再利用可能なファイルに分け、Copilot に参考例を示せます (パスはリポジトリのルートではなく、指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションではプロジェクトのコーディング標準を説明していますが、コード内のドキュメント化はまだ求めていません。次の手順で TSDoc コメントとファイルヘッダーコメントの規則を追加します。

## 変更用のブランチを作成する

ここでは、ベストプラクティスに従って変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。その後、指示を更新し、同じプロンプトでもう一度実行して違いを見ます。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に知らせます。
3. Chat ビューのエージェント選択メニューから **Agent** を選択し、ファイルの変更を適用できるようにします。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   src/lib/publishers.ts に、各パブリッシャーの名前と ID を含む一覧を返すデータアクセスヘルパーを作成または更新してください。ファイルの変更を適用してください。
   ```

5. Copilot はプロジェクトを調べて、ヘルパーファイルとテストを含むコードの変更を適用します。
6. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取る型付き関数であり、パブリッシャーの型付き配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約によるものです。
7. 提案されたコードに TSDoc のドキュメントコメントとファイル単位のコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。指示を更新した後に動作が一貫して改善することが、この演習のポイントです。

## リポジトリ標準を追加する

前の説明のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を提供します。コードの提案を改善するため、リポジトリのコーディング標準を明記しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. **Code formatting requirements** セクション (27 行目付近) を探します。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメント化の規則はありません。そのため、生成されたヘルパーにはドキュメントコメントがありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加し、ファイルヘッダーコメントと TSDoc コメントの追加を Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体の指示 (`.github/copilot-instructions.md`) と、言語、ファイル形式、タスクに応じた指示 (`*.instructions.md`) を作成できます。追加したドキュメントコメントの規則のようなプロジェクト全体の標準は、リポジトリレベルのファイルに記載します。

## 同じプロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をクリアします。
2. `src/lib/publishers.ts` をクリックして、Copilot に対象ファイルを認識させます。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   src/lib/publishers.ts に、各パブリッシャーの名前と ID を含む一覧を返すデータアクセスヘルパーを作成または更新してください。ファイルの変更を適用してください。
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが追加されていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準を Copilot に伝え、次の演習で使う実際のコードに適用できました。

## 指示をコミットしてブランチをプッシュする

指示ファイルもリポジトリ内のほかのファイルと同様に、ソース管理で管理します。変更をコミットしてブランチをプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで、指示の更新とヘルパーの変更が存在することを次のコマンドで確認します。

   ```bash
   git status
   ```

3. ターミナルで指示の更新とヘルパーの初期実装に対するすべての変更をステージングし、コミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の関連変更 (テストなど) が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルからブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の課題と関連付けてプルリクエストを作成します。手動で行う代わりに、すでにプロジェクトに接続されている GitHub ツールを使って Copilot に依頼します。課題を見つけて、課題を閉じる PR を作成し、マージするよう指示します。

> [!NOTE]
> この演習では、初めて Copilot に GitHub への操作を依頼します。プロジェクトのテンプレートには GitHub ツールがすでに設定されています。Copilot が初めてツールを使う際、VS Code で GitHub へのサインインを求められることがあります。プロンプトに従って許可してください。この接続のしくみ (Model Context Protocol) は、後の演習で学びます。

1. Codespace 内の Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する課題を探し、現在のブランチから PR を作成するよう Copilot に依頼します。PR では、指示の更新と、今後のフィルター機能に向けてパブリッシャーヘルパーの基盤を追加したことの両方を説明します。

   ```
   指示ファイルの更新に関する課題を探してください。現在のブランチから新しい PR を作成し、その課題を閉じることを明記してください。また、今後のフィルター作業に向けてパブリッシャーヘルパーの基盤も追加したことを記載してください。
   ```

4. Copilot が課題を探し、PR を作成します。
5. Copilot が GitHub 操作の許可を求めた場合は、コマンドを確認して適切に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージしてブランチを `main` に戻すよう依頼します。

   ```
   PR を main にマージしてください。その後、ローカルで main に戻り、最新のコードを pull して最新の状態にしてください。
   ```

7. Copilot が GitHub の操作やシェルコマンドの実行許可を求めた場合は、コマンドを確認して適切に **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルをどのように使うかを確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示でコード生成のプロンプトを送信し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、新しい標準が提案されたコードに反映される様子を確認しました。
- 次の演習で使えるよう、指示とヘルパーの基盤をコミットして `main` に反映しました。

次は、Copilot がコードベース全体に新機能を追加する際に[agent mode で指示を活用します][next-lesson]。

## リソース

- [GitHub Copilot をカスタマイズするための指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
