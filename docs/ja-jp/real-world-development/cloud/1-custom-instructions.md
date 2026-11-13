---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使う際はコンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot が背景情報を知る必要がある場合、その情報にアクセスできるようにします。[指示ファイル][instruction-files]を使うと、Copilot に何を実行するかだけでなく、どのように実行するかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わるしくみを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここではプロンプトの実行前後を比較しません。Copilot cloud agent は GitHub の課題に対して非同期で作業するため、リアルタイムで並べて効果を示すのは困難です。このハーネスの後半で、cloud agent が作成したプルリクエストをレビューする際に、指示ファイルの影響を確認します。

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

## リポジトリ標準を追加する

ここで行う編集は 1 つです。コード内のドキュメントを TSDoc コメントとファイル単位のコメントヘッダーで記述する、プロジェクト全体の規則を追加します。cloud agent は、このハーネスの後半で割り当てられた課題に取り組む際にこの規則を読み取ります。

編集の前に、作業用のブランチを作成します。cloud agent は課題が対象とするブランチから指示を読み取りますが、変更を分けてコミットしておきます。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 指示の変更をコミット、プッシュ、マージする

cloud agent は、課題の対象となるブランチから指示ファイルを読み取ります。次の演習で Copilot に課題を割り当てると、cloud agent は `main` からブランチを作成します。そのため、指示を読み取らせるには `main` に変更をマージする必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で Copilot に割り当てた課題に取り組む際、cloud agent はこの指示を読み取ります。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチを省略して `main` に直接コミットできます。ここでは実際のプロジェクトに近い流れにするため、ブランチを使っています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルをどのように使うかを確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。このハーネスの後半で cloud agent が作成するプルリクエストに、その標準が反映されます。

次は[最初の課題を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot をカスタマイズするための指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
