---
title: "演習 6 - Playwright の Model Context Protocol（MCP）で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または構成し、ブラウザーでフィルタリング機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

すでに説明したように、コードを書くことだけが開発ではありません。データや外部サービスを扱い、Copilot が追加の自動化機能を使えるようにする必要もあります。そこで役立つのが Model Context Protocol（MCP）サーバーです。MCP サーバーを使うと、Copilot は CLI に組み込まれた機能だけでなく、さらに多くのツールやサービスを利用できます。

この演習では、次のことを行います。

- Model Context Protocol（MCP）とは何か、Copilot CLI がどのように利用するかを理解します。
- まだ利用できない場合は、Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルタリング機能を試します。

## シナリオ

ユニット テストとエンドツーエンド テストは重要ですが、ユーザー インターフェイス（UI）の変更を検証するには、実際に操作する必要があります。Copilot が開発中の Web サイトをユーザーと同じように使えるようにすると、変更の作成方法をさらに自動化し、期待どおりに動作するという確信を高められます。

## Model Context Protocol（MCP）とは

[Model Context Protocol（MCP）][mcp-blog-post]は、人工知能（AI）エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部のツールやサービスとリアルタイムに通信できます。これにより、最新の情報にアクセスしたり、依頼に応じて操作を実行したりできます。

こうしたツールやリソースには、人工知能（AI）エージェントと外部のツールやサービスをつなぐ MCP サーバーを通じてアクセスします。各 MCP サーバーが、AI エージェントから利用できるツールとリソースを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、pull request を管理するアプリケーション プログラミング インターフェイス（API）にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内で見つけやすくし、貢献を促すための [MCP registry][mcp-registry] をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクト内のほかの依存関係と同様に扱ってください。使用する前に、ソース コードを確認し、公開元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーと同じように Web サイトを操作できるよう、サーバーを追加します。

> [!NOTE]
> Copilot CLI ですでに Playwright MCP を構成済みの場合は、このセットアップ手順を省略して次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力して、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押して、サーバーの種類が **STDIO** であることを確認し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を試すよう Copilot に依頼する

これまでは、機能が期待どおりに動作することを手動で確認しました。今度は Copilot で利用できる Playwright MCP サーバーを使って、同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に伝えます。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は Playwright MCP サーバーを使うよう Copilot に伝える必要はありません。Copilot が自動的に判断することが多いためです。ただし、使うべきサーバーが分かっている場合は、指定して問題ありません。より一貫した結果が得られ、トークンも節約できます。

2. Copilot がブラウザーで機能を確認するために実行する手順を確認します。
3. 報告を読み、すべてが期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作して、サーバーを停止した後に結果を報告します。

## まとめと次のステップ

おめでとうございます。Copilot CLI から Playwright MCP サーバーを使って、実際のブラウザーで機能を確認しました。今回行ったことを振り返ります。

- Model Context Protocol（MCP）とは何か、Copilot CLI がどのように利用するかを学びました。
- まだ利用できない場合に、Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、フィルタリング機能を試しました。

次は、skill とブラウザー ツールを組み合わせた [QA カスタム エージェントを作成します][next-lesson]。

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
