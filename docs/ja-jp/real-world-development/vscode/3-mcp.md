---
title: "演習 3 - Playwright の Model Context Protocol（MCP）サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前の演習: Agent mode][previous-lesson] · [次の演習: カスタム エージェント →][next-lesson]

agent mode でフィルタリング機能を作成しました。pull request（PR）を開く前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright の Model Context Protocol（MCP）サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を開きます。

この演習では、次のことを行います。

- Model Context Protocol（MCP）とは何か、MCP サーバーが Copilot に新しいツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot に Playwright MCP サーバーを使わせ、ブラウザーでフィルタリング機能を手動テストします。
- ブランチを公開し、フィルタリングの pull request を開きます。

## Model Context Protocol（MCP）とは

agent mode は、エディターの外部にある機能を使えると、さらに強力になります。Copilot がそれを実現するのが Model Context Protocol（MCP）です。MCP は、エージェントが外部のツールやサービスと通信するための標準的な方法です。

![agent mode の内部の仕組みと、コンテキスト、大規模言語モデル（LLM）、ツール（MCP サーバーや VS Code 拡張機能が提供するツールを含む）とのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP によって、AI エージェントはリソースを使って最新情報にアクセスしたり、ツールを使って依頼に応じた操作を実行したりできます。

こうしたツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを通じてアクセスします。MCP サーバーは、AI エージェントと外部ツール（既存の API や npm パッケージなどのローカル ツール）との通信を管理します。各 MCP サーバーが、AI エージェントから利用できるツールとリソースを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する一連のアプリケーション プログラミング インターフェイス（API）にアクセスできます。AI エージェントは、新しいリポジトリの作成、既存リポジトリの更新、issue や pull request の管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。AI エージェントは、Web ページへの移動、フォームへの入力、ボタンの選択などを実行できます。

さまざまなツールやリソースを利用できる MCP サーバーはほかにもあります。GitHub は、エコシステム内で見つけやすくし、貢献を促すための [MCP registry](https://github.com/mcp) をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクト内のほかの依存関係と同様に扱ってください。使用する前に、ソース コードを慎重に確認し、公開元を検証して、セキュリティへの影響を検討してください。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用できる MCP サーバーを構成します。

1. codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに構成されていることを確認します。

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

この `github` エントリはプロジェクト テンプレートに含まれているため、前の演習で Copilot がバックログの issue を読み取れました。これは[リモート GitHub MCP サーバー][remote-github-mcp-server]を使います。ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot がサーバーのツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が操作できるブラウザーを利用できます。これは、機能をテストするために必要なものです。

1. `.vscode/mcp.json` の `github` と並べて `playwright` エントリを追加し、ファイルが次のようになるようにします。

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

リモートの `github` サーバーとは異なり、`playwright` は**ローカル** サーバーです。VS Code は `npx @playwright/mcp@latest` を実行してサーバーを起動します。`--headless` フラグを指定すると、Playwright は表示ウィンドウなしでブラウザーを実行します。デスクトップ画面がない codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンド テストで Playwright をすでに使っているため、通常は Playwright 用ブラウザーもインストール済みです。後で Copilot からブラウザーが見つからないと報告された場合は、`npx playwright install chromium` を実行するよう依頼し、もう一度試してください。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot がそのサーバーのツールを初めて使うときに VS Code が起動し、サーバーを信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** 操作が表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認された場合は、構成を確認して、サーバーが起動できるよう信頼することを選択します。

手動で起動する必要はありません。省略した場合は、次の手順で Copilot が初めてツールを使うときに VS Code がサーバーを起動し、信頼するか確認します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。開発者がアプリを操作して確認する代わりに、エージェントがページを開き、移動し、フィルターを適用し、結果を読み取って要約します。会話を中断せずに、機能が期待どおりに動作することをすばやく確認できます。

内部では、Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。つまり、エージェントは支援技術と同じように、ラベルが付いた構造化要素（ボタン、リンク、リスト項目など）をもとに判断します。そのため、機能をすばやく確認すると同時に、アクセシビリティも簡易チェックできます。

サーバーを接続し、アプリを起動した状態で、作成したフィルタリング機能を試すよう Copilot に依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して、結果を報告します。issue の受け入れ基準と照らし合わせて報告を確認してください。問題がありそうな場合は、pull request を開く前に追加の質問をするか、コードを修正するよう依頼します。

> [!NOTE]
> このテストでは、アプリを `http://localhost:4321` で実行しておく必要があります。開発サーバーを停止した場合は、プロンプトを送る前にもう一度起動してください。Copilot が初めて Playwright MCP サーバーを使うときは、ブラウザーのダウンロードが必要な場合があります。ブラウザーが見つからないと報告された場合は、`npx playwright install chromium` を実行するよう依頼し、もう一度試してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開して pull request を作成する

機能が動作することを確認できたら、チームでレビューするための pull request（PR）を開きます。最初に `filtering-vscode` ブランチを公開します。

1. codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![変更内容を表示する Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリに push します。

## pull request を作成する

github.com や GitHub コマンド ライン インターフェイス（CLI）など、pull request を作成する方法はいくつかあります。GitHub Copilot をすでに使っているので、PR の作成も任せましょう。Copilot は関連する issue を見つけ、issue に関連付けた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを始めます。
2. エージェント ドロップダウンで **Agent** が選択されていることを確認します。Copilot が GitHub ツールを使うために必要です。
3. 次のプロンプトを使い、Copilot に PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、情報の取得と操作に必要な作業を Copilot に許可します。GitHub ツールを初めて使うときに、GitHub での認証を求められることがあります。表示された指示に従って許可してください。
5. Copilot が issue を検索し、該当するものを見つけて PR を作成することを確認します。
6. Copilot が作成したリンクを選択して pull request を確認します。ただし、**まだマージしないでください**。

## まとめと次のステップ

おめでとうございます。この演習では、次のことを行いました。

- Model Context Protocol（MCP）とは何か、MCP サーバーが Copilot に新しいツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- Playwright MCP サーバーを使い、リリース前にブラウザーでフィルタリング機能を手動テストしました。
- ブランチを公開し、フィルタリングの pull request を開きました。

MCP サーバーを使って機能を**テスト**しましたが、MCP は Copilot を拡張する方法の 1 つにすぎません。次は、アクセシビリティ レビューのような範囲を絞ったタスクを効率化するため、[カスタム エージェントを作成します][next-lesson]。

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
