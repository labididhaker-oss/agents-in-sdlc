---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルター機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

すでに説明したように、コードを書くことだけが開発ではありません。データや外部サービスを使ったり、Copilot で追加の自動化を利用できるようにしたりする必要があります。そこで役立つのが MCP サーバーです。MCP サーバーを使うと、Copilot CLI に組み込まれた機能だけでなく、さらに多くのツールやサービスを利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と、Copilot CLI での利用方法を理解します。
- まだ利用できない場合は Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルター機能を確認します。

## シナリオ

ユニットテストやエンドツーエンドテストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。Copilot がユーザーと同じように開発中の Web サイトを使えるようにすると、変更の自動化が進み、期待どおりに動作する確信を高められます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部のツールやサービスとリアルタイムにやりとりできます。最新情報にアクセスしたり、代理で操作したりできます。

これらのツールやリソースには MCP サーバーを通じてアクセスします。MCP サーバーは AI エージェントと外部のツールやサービスをつなぎます。それぞれの MCP サーバーは、エージェントが利用できる異なるツールやリソースのセットを提供します。

よく利用される MCP サーバーには、次のようなものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、課題、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内での発見や貢献を促進するために [MCP registry][mcp-registry] を公開しています。

> [!CAUTION]
> MCP サーバーは、プロジェクト内のほかの依存関係と同様に扱ってください。利用する前にソースコードを確認し、発行元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーと同じように Web サイトを操作できるように追加します。

> [!NOTE]
> Copilot CLI ですでに Playwright MCP が設定されている場合は、セットアップ手順を飛ばして次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力して MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類が **STDIO** であることを確認し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を確認するよう Copilot に依頼する

これまでは、機能が期待どおりに動作することを手動で確認しました。次は、Copilot で利用できる Playwright MCP サーバーを使って、同じ確認を行いましょう。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   アプリを起動し、Playwright MCP で課題と計画に照らし合わせてフィルターを確認してください。変更は加えず、うまく動作したことと問題があったことを報告してください。完了したら、起動したサーバーを停止してください。
   ```

> [!NOTE]
> 通常は、MCP サーバーを使うよう指定しなくても Copilot が判断します。ただし、使用するツールが分かっている場合は、明示しても問題ありません。より一貫した結果につながり、トークンの節約にもなります。

2. Copilot がブラウザーで実行する手順を確認し、機能が動作するか調べる様子を見ます。
3. レポートを読み、すべてが期待どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作し、サーバーを停止したうえで結果を報告します。

## まとめと次のステップ

おめでとうございます。Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を確認しました。このレッスンでは、次のことを行いました。

- Model Context Protocol (MCP) と、Copilot CLI での利用方法を学びました。
- まだ利用できない場合は Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を確認しました。

次は、スキルとブラウザーツールを組み合わせる[QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
