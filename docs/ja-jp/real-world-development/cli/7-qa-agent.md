---
title: "演習 7 - 品質保証（QA）agent を作成して使う"
description: "issue の要件、quality-checks skill、Playwright MCP を組み合わせた QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` skill を使って自動チェックを実行し、Playwright の Model Context Protocol（MCP）サーバーでブラウザー上のフィルタリング操作を確認しました。次は、これらの機能を、明確な品質保証（QA）プロセスを持つカスタム エージェントにまとめます。

この演習では、次のことを行います。

- カスタム エージェントが命令、skill、MCP ツールとどのように連携するかを確認します。
- 再利用可能な品質保証プロファイルを作成し、内容を確認します。
- QA agent を選択し、フィルタリングの issue に照らして報告内容を確認します。

## シナリオ

Tailspin Toys では、pull request（PR）を開く前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザー上の動作を一貫した方法で確認したいと考えています。カスタム エージェントを使うと、この QA プロセスを進め、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する Copilot の特化バージョンです。プロファイルには、エージェントの目的、命令、利用可能なツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの命令はチームの標準を示し、`quality-checks` skill は繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、要件を評価して結果を報告するために、Copilot がこれらの機能をどう使うかを示します。QA プロファイルはそれらを置き換えるものではなく、新しい会話を必要とするものでもありません。

## QA プロファイルを作成する

機能の PR を開く前に、Copilot に再利用可能な QA プロファイルを作成してもらいます。プロファイルでは、QA が実施するチェックと、その作業の境界の両方を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のカスタム エージェントの作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が想定した QA ワークフローと作業の境界を反映できているか、プロファイルを確認します。これにより、検証だけを行いたいときに、エージェントが不完全な指示で機能を変更したり、範囲を超えた作業を行ったりすることを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は任意ですが、含めるとエージェントの表示名が明確になります。
3. プロファイルの命令を読み、QA が要件を確認し、リポジトリの命令に従い、`quality-checks` skill を実行して、Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に許可を求め、変更をコミットしたり pull request を開いたりしないことを確認します。
5. これらの責務や境界がプロファイルに含まれていない場合は、先に進む前に既定のエージェントに修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成したら同じフィルタリングの会話を再開し、QA を選択します。そうすることで、issue と計画中の判断を引き続き利用できます。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりのエージェントは、利用可能なエージェントの一覧に表示されないことがあります。その場合でも、上記のコマンドで有効にできます。

2. 次のプロンプトを使い、QA agent に機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA agent が作業を進める様子を確認します。
4. 完了したら、レポートを読みます。

## まとめと次のステップ

再利用可能な専門的役割をワークフローに加え、その結果を確認しました。この演習では、次のことを行いました。

- カスタム エージェントが命令、skill、MCP ツールとどのように連携するかを確認しました。
- 再利用可能な品質保証プロファイルを作成し、内容を確認しました。
- QA agent を選択し、フィルタリングの issue に照らして報告内容を確認しました。

実装、skill の更新、QA プロファイル、テスト、検証レポートの準備ができました。次は、これらを機能の PR にまとめ、Agent Merge を使います。[次の演習に進む][next-lesson]

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
