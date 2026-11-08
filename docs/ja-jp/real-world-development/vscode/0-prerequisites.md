---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、必要なものを準備します。Tailspin Toys リポジトリのコピーを作成し、作業用の [codespace][codespaces] を起動します。また、エディターで GitHub Copilot Chat が動作することを確認します。

## ラボ用リポジトリをセットアップする

コードを作成するためのリポジトリのコピーとして、[テンプレート][template-repository]からインスタンスを作成します。新しいインスタンスにはラボに必要なファイルがすべて含まれます。各演習でこのリポジトリを使います。

1. 新しいブラウザー ウィンドウで、[Tailspin Toys のテンプレート][tailspin-template]に移動します。
2. ラボ リポジトリのページで **Use this template** ボタンを選択し、**Create a new repository** を選択して、リポジトリのコピーを作成します。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップを受講している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成できます。

   ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス（**organization-or-user-name/repository-name**）を控えてください。ラボの後半で使用します。

> [!NOTE]
> **バックログの準備は完了しています**
>
> テンプレートからリポジトリを作成すると、GitHub issue のバックログが自動的に作成されます。ワークショップ全体でこれらの issue を使います。自分で issue を作成する必要はありません。

## codespace を作成する

次に、ラボの演習を完了するための codespace を使います。

[GitHub Codespaces][codespaces] はクラウドベースの開発環境です。ブラウザー上で直接コードの作成、実行、デバッグができます。複数のプログラミング言語、拡張機能、ツールに対応した、機能豊富な統合開発環境（IDE）が提供されます。

1. 新しく作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

   ![Code ボタンを選択する画面](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して、新しい Codespace を作成します。

   ![新しい codespace を作成する画面](../../../_images/ex0-create-codespace.png)

codespace の作成には数分かかりますが、すべてのサービスを手作業でインストールするよりもはるかに短時間で済みます。待っている間に、次に取り上げる GitHub Copilot の機能を確認してもよいでしょう。

> [!CAUTION]
> 今後の演習で codespace に戻ります。ここでは、ブラウザーのタブを閉じずに開いたままにしてください。

> [!NOTE]
> このワークショップは、codespace またはローカルの [dev container][dev-containers] で実行できるように構成されています。どちらの環境にも、スムーズに進めるために必要な前提条件がインストールされています。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、確認メッセージが表示されたら **Reopen in Container** を選択してください。codespace と同じ dev container が VS Code によって構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat と agent mode を使う

GitHub Copilot Chat の agent mode を使うには、IDE に GitHub Copilot Chat 拡張機能をインストールする必要があります。GitHub Codespace を使っている場合は、すでにインストールされているはずです。

> [!TIP]
> GitHub Copilot Chat 拡張機能がインストールされていない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]できます。または、Visual Studio Code の Extensions ビューを開き、**GitHub Copilot Chat** を検索して **Install** を選択します。

拡張機能をインストールした後、GitHub アカウントで認証して有効にする必要がある場合があります。

1. codespace に戻ります。
2. エディターの右側に Copilot Chat が表示されていない場合は、codespace の上部にある **Copilot Chat** アイコンを選択します。
3. Copilot Chat ウィンドウに「Hello world」のようなメッセージを入力し、Enter を押します。これで Copilot Chat が起動します。
4. 認証されていない場合は、GitHub アカウントへのサインインを求められます。指示に従って認証します。

   ![Copilot Chat の認証プロンプトの例](../../../_images/ex1-copilot-authentication.png)

5. 認証が完了すると、Copilot Chat ウィンドウが表示されます。

## まとめ

おめでとうございます。ラボ用リポジトリのコピーを作成し、コードを書くときに使う codespace の作成を始めました。

## 次のステップ

Copilot に作業を任せましょう。[演習 1 - カスタム命令][next-lesson]に進み、Copilot にプロジェクトの規約を伝えます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
