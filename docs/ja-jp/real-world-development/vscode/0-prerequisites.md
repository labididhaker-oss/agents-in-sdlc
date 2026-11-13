---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、必要なものを準備します。Tailspin Toys リポジトリのコピーを作成し、作業用の [codespace][codespaces] を起動します。また、エディターで GitHub Copilot Chat が使えることを確認します。

## ラボ用リポジトリをセットアップする

作成するコードのためのリポジトリを用意します。[テンプレート][template-repository]からインスタンスを作成すると、ラボに必要なファイルがすべて含まれます。演習中は、そのリポジトリを使います。

1. 新しいブラウザーウィンドウで [Tailspin Toys テンプレート][tailspin-template]に移動します。
2. ラボリポジトリのページで **Use this template** ボタンを選択し、**Create a new repository** を選択して、自分用のコピーを作成します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

    ![リポジトリテンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) をメモします。ラボの後半で使います。

> [!NOTE]
> **バックログの準備は完了しています**
>
> テンプレートからリポジトリを作成すると、GitHub の課題のバックログも自動的に作成されます。ワークショップではこの課題を使います。自分で課題を作成する必要はありません。

## Codespace を作成する

次に、ラボの演習を行う Codespace を作成します。

[GitHub Codespaces][codespaces] は、ブラウザーから直接コードを作成、実行、デバッグできるクラウドベースの開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする、すべての機能を備えた IDE を利用できます。

1. 新しく作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい Codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、必要なサービスを手作業でインストールするよりもずっと早く済みます。作成中に GitHub Copilot のほかの機能を試してみてください。次にその機能を使います。

> [!CAUTION]
> 今後の演習で Codespace に戻ります。しばらくは、ブラウザーのタブで開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で動作するように構成されています。どちらも、スムーズに進めるために必要なものがすべてインストールされた環境です。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、表示されたら **Reopen in Container** を選択します。Codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat と agent mode を使う

GitHub Copilot Chat の agent mode を利用するには、IDE に GitHub Copilot Chat 拡張機能がインストールされている必要があります。GitHub Codespace を使用している場合は、すでにインストールされています。

> [!TIP]
> GitHub Copilot Chat 拡張機能がインストールされていない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]できます。Visual Studio Code の Extensions ビューを開き、**GitHub Copilot Chat** を検索して **Install** を選択する方法もあります。

拡張機能をインストールした後、GitHub アカウントで認証して有効にする必要がある場合があります。

1. Codespace に戻ります。
2. エディターの右側に Copilot Chat が表示されていない場合は、Codespace の上部にある **Copilot Chat** アイコンを選択します。
3. Copilot Chat ウィンドウに `Hello world` のようなメッセージを入力し、Enter キーを押します。Copilot Chat が有効になります。
4. 認証されていない場合は、GitHub アカウントへのサインインが求められます。手順に従って認証します。

    ![Copilot Chat の認証プロンプトの例](../../../_images/ex1-copilot-authentication.png)

5. 認証後、Copilot Chat ウィンドウが表示されます。

## まとめ

おめでとうございます。ラボリポジトリのコピーを作成し、コードを書き始めるときに使う Codespace の作成を開始しました。

## 次のステップ

Copilot に作業を始めてもらいましょう。次は[演習 1 - カスタム指示][next-lesson]に進み、Copilot にプロジェクトの規約を伝えます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
