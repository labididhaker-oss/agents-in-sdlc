---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

agent mode でフィルター機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot に新しいツールを提供する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- ブラウザーで Playwright を使ってフィルター機能をテストするよう Copilot に依頼します。
- ブランチを公開し、フィルター作業のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

agent mode はエディターの外部にアクセスできると、さらに便利になります。Copilot がそのために使うのが Model Context Protocol (MCP) です。エージェントが外部のツールやサービスと通信するための標準的な方法です。

![agent mode の内部構造と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとのやりとりを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部サービスとリアルタイムにやりとりできます。リソースを使って最新情報にアクセスしたり、ツールを使って代理で操作したりできます。

これらのツールやリソースには MCP サーバーを通じてアクセスします。MCP サーバーは AI エージェントと外部のツールやサービスをつなぎます。既存の API や NPM パッケージなどのローカルツールとの通信を管理し、エージェントが利用できるツールやリソースを提供します。

よく利用される MCP サーバーには、次のようなものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API を提供します。新しいリポジトリの作成、既存リポジトリの更新、課題やプルリクエストの管理などを AI エージェントが実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。AI エージェントが Web ページに移動し、フォームに入力して、ボタンを選択できます。

ほかにもさまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub は、エコシステム内での発見や貢献を促進するために [MCP registry](https://github.com/mcp) を公開しています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクト内のほかの依存関係と同様に扱ってください。利用する前にソースコードをよく確認し、発行元を検証して、セキュリティへの影響を検討してください。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用する MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` サーバーが設定されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` の設定はプロジェクトのテンプレートに含まれているため、前の演習で Copilot はバックログの課題を読むことができました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使っており、ローカルへのインストールは不要です。VS Code は HTTP 経由で接続し、Copilot がツールを初めて使う際に GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと Copilot がブラウザーを操作できるため、機能のテストに適しています。

1. `.vscode/mcp.json` で `github` と並べて `playwright` の設定を追加し、ファイルが次のようになるようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートサーバーの `github` と異なり、`playwright` は**ローカル**サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると画面を表示せずにブラウザーを実行できます。Codespace にはブラウザーを表示するデスクトップ環境がないため、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトではエンドツーエンドテストに Playwright を使うため、ブラウザーはすでにインストールされていることが多いです。後で Copilot がブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行するよう依頼し、もう一度試してください。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて起動**します。Copilot が初めてサーバーのツールを必要としたときに、VS Code がサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` 設定の上に **Start** 操作が表示されます。選択してサーバーを起動し、設定が正しいことを確認します。
2. VS Code からサーバーを信頼するか尋ねられた場合は、設定を確認し、信頼する選択肢を選んでサーバーを起動します。

手動で起動する必要はありません。省略した場合は、次の手順で Copilot がツールを初めて必要としたときに、VS Code がサーバーを起動し、信頼を求めます。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作する代わりに、エージェントがページを開き、移動し、フィルターを適用して、その結果を確認できます。結果の要約も受け取れます。会話を続けながら機能が期待どおりに動くか確認できる、すばやい方法です。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を参照します。エージェントはボタン、リンク、リスト項目など、ラベルが付いた構造化要素を使って判断します。支援技術と同様にページを読み取るため、簡単な機能確認でアクセシビリティもある程度確認できます。

MCP サーバーを接続してアプリを実行した状態で、次のプロンプトを Copilot に送信してフィルター機能を確認します。

```text
Playwright MCP サーバーを使って、http://localhost:4321 で実行中のアプリをブラウザーで開き、新しいゲームフィルター機能を確認してください。

1. ゲームページに移動し、ゲームがいくつ表示されているか確認します。
2. カテゴリフィルターを適用し、一覧がそのカテゴリのゲームだけに更新されることを確認します。
3. フィルターを解除し、パブリッシャーフィルターを適用して、一覧がそのパブリッシャーのゲームに更新されることを確認します。
4. カテゴリとパブリッシャーの両方を指定し、両方の条件が結果に適用されることを確認します。

各手順で確認したことを報告し、期待どおりに動作しない点があれば指摘してください。
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。課題の受け入れ基準と照らし合わせて要約を確認してください。問題があれば、追加の質問をするか、プルリクエストを開く前にコードの修正を依頼します。

> [!NOTE]
> このテストの実行中は、`http://localhost:4321` でアプリが動作している必要があります。開発サーバーを停止した場合は、プロンプトを送る前にもう一度起動してください。Copilot が Playwright MCP サーバーを初めて使う際にブラウザーをダウンロードすることがあります。ブラウザーが見つからないと報告された場合は、`npx playwright install chromium` を実行するよう依頼し、再試行してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたので、チームがレビューできるようにプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージングします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![変更されたファイルが表示されている Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドラインインターフェイス (CLI) を使うなど、プルリクエストを作成する方法はいくつかあります。GitHub Copilot を使っているので、PR の作成も依頼しましょう。Copilot は関連課題を見つけ、課題に関連付けて PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. GitHub ツールを使えるよう、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を Copilot に依頼します。

   ```plaintext
   リポジトリ内でカテゴリとパブリッシャーによる絞り込みに関する課題を探してください。現在のブランチ用に新しいプルリクエストを作成し、該当する課題に関連付けてください。
   ```

4. Copilot が情報を集めたり操作を行ったりする際に、必要に応じて **Continue** を選択します。GitHub ツールを初めて使う際は、GitHub で認証を求められることがあります。プロンプトに従って許可してください。
5. Copilot が課題を検索し、該当するものを見つけて PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択してプルリクエストをレビューします。まだ**マージしないでください**。

## まとめと次のステップ

おめでとうございます。この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot に新しいツールを提供する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- 出荷前に、ブラウザーでフィルター機能を手動テストしました。
- ブランチを公開し、フィルター作業のプルリクエストを作成しました。

MCP サーバーを使って機能を*テスト*しました。MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティレビューなど、対象を絞ったタスクを効率化する[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
