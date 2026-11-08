---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前の演習: カスタム命令][previous-lesson] |
|:--|

技術的負債に悩む組織は少なくありません。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能リクエストなどがその例です。GitHub Copilot の cloud agent は、コードの更新や機能の追加といったタスクを自律的に実行できます。作業が完了すると、人間の開発者が確認できるドラフトの pull request（PR）を作成します。これにより、面倒な作業を任せて開発プロセスを加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次のことを確認します。

- コード生成環境をカスタマイズする。
- 操作が安全に行われるようにする。
- 範囲を明確に定めた issue の重要性を理解する。
- issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には対処したい技術的負債があります。最初のサイトを構築した委託業者は、望ましくない状態でドキュメントを残しました。実際には、ドキュメントがまったくありません。最初の対応として、アプリケーション内のエクスポートされたすべての関数に TSDoc のドキュメント コメントを追加したいと考えています。

さらに、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属するほかのゲームを表示し、閲覧を続けられるようにしたいと考えています。まだ完成度の高い実装は必要なく、ユーザー エクスペリエンス（UX）の受け入れテストに使える状態を希望しています。現在これはブロッカーですが、優先度がより高い issue がほかにあります。

どちらも優先順位が下がりやすいタスクの例であり、Copilot cloud agent に割り当てるのに適しています。Copilot cloud agent が非同期に作業している間、開発者は別のタスクに集中できます。その後で作業内容を確認し、すべてが期待どおりであることを確かめられます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。たとえば、[GitHub issue を Copilot に割り当てる][assign-issue]方法があります。割り当てると、Copilot は進捗を記録するドラフトの pull request を作成し、環境をセットアップしてタスクに取りかかります。作業中でも完了後でも、Copilot のセッションを詳しく確認できます。提案された解決策を確認できる状態になると、pull request で開発者に通知します。

## 範囲を明確にした命令の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく見ると複雑さが分かります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に定めてください。人工知能（AI）を使ったペア プログラマーとして扱い、段階を踏んで学び、試し、調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム命令

これまでの演習では、カスタム命令と、Copilot を導く方法を確認しました。カスタム命令の実習をすでに行っている場合は、`.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、ここで簡単に確認しましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに用意された命令ファイルを読み取り専用で確認し、後からその影響を見つけられるようにします。

リポジトリの GitHub Web UI、または起動済みの codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクションを確認します。特に、TypeScript の規約と TSDoc のドキュメント コメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` frontmatter を確認します。この命令は `**/*.test.ts` に一致するファイルに適用されます。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てたら、作成された pull request に TSDoc のドキュメント コメント、TypeScript の規約、コメント ヘッダーが反映されているか確認してください。後の演習で PR をレビューするときにも、改めて取り上げます。

## Copilot cloud agent の開発環境をセットアップする

誰が作業する場合でも、コードを作成するには通常、特定の環境と、状態を整えるために実行するセットアップ スクリプトが必要です。ソフトウェア エンジニアのように作業する Copilot にタスクを割り当てる場合も同じです。

cloud agent は作業時の環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業の開始前に実行する環境をカスタマイズできます。必要な開発ツールや依存関係を cloud agent が利用できるようになります。ラボの流れを整え、この機会に学べるよう、あらかじめ設定済みです。この設定により、単一の Astro アプリで使う Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーションとシードが済んだ SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub ワークフロー ファイルと同じように見えますが、重要な点がいくつかあります。

- `copilot-setup-steps` という 1 つのジョブが含まれています。このジョブは、Copilot が pull request の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーに注目してください。リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot が実行するのを待たずに、ワークフローが正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントが重要であることは誰もが理解していますが、多くのプロジェクトでは情報が古くなっていたり、まったくなかったりします。この種の技術的負債は対処されないままになることが多く、生産性を下げ、コードベースの保守や新しいチーム メンバーの受け入れを難しくします。Copilot はドキュメントの作成を得意としているため、これは Copilot cloud agent に割り当てるのに適した issue です。バックグラウンドで必要なドキュメントを生成してもらい、後の演習で作業内容を確認します。

1. 新しいブラウザー タブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue ダイアログを開きます。
4. **Blank issue** を選択して、新しい issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく生成された pull request（PR）を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものになります。新しい PR が一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで、数分かかることがあります。Copilot は先ほど説明したワークフローを実行して環境を作成し、プロジェクトを分析して、問題への最適な取り組み方を判断しています。

13. リストを確認し、Copilot が実行するタスクを確認します。
14. pull request のタイムラインを下にスクロールし、Copilot が issue の作業を開始したという更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッションを表示する画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するために、ウィンドウを更新する必要がある場合があります。

16. ライブ セッションをスクロールしながら、Copilot の作業を確認します。コードを調べて状態を把握する様子、計画を考えて判断するために一時停止する様子、コードを作成する様子が見られます。

作業には数分かかる場合があります。Copilot cloud agent の主な目的の 1 つは、タスクを非同期に実行して、開発者が別の作業に集中できるようにすることです。この機能を活用し、Copilot cloud agent に別のタスクも割り当ててから、アプリに機能を追加するコードの作成に取りかかります。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点の 1 つは、作業を分けられることです。開発者があるタスクに集中する一方で、agent は別のタスクを進められます。デザイン チームのために関連ゲームのセクションを追加する作業は、長時間かからないかもしれません。それでも、別の作業に使える時間を確保できます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue ダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、issue の最初のコメントに 👀 が表示され、Copilot が作業を始めたことが分かります。

![Copilot が issue の作業中に目の絵文字を表示する画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取りかかりました。Copilot cloud agent はソフトウェア エンジニアと同じように作業するため、常に監視する必要はありません。完了後に確認できます。次は、カスタム エージェントの作成と使用を見ていきましょう。

## まとめと次のステップ

この演習では、[GitHub Copilot cloud agent][copilot-agents]を確認しました。cloud agent に issue を割り当てると、非同期に作業を実行できます。技術的負債への対処、新機能の作成、あるフレームワークから別のフレームワークへのコード移行などを依頼できます。

次の概念を確認しました。

- コード生成環境をカスタマイズする。
- 操作が安全に行われるようにする。
- 範囲を明確に定めた issue の重要性を理解する。
- issue を Copilot に割り当てる。

cloud agent がバックグラウンドで作業している間に、カスタム エージェントの作成と使用に取りかかります。[Copilot cloud agent は Model Context Protocol（MCP）サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム命令も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次の演習: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
