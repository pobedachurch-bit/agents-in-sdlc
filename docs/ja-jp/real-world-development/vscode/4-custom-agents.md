---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせて、専門的な人工知能（AI）アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置いてエージェントを定義すると、Copilot に対象を絞った命令、ベスト プラクティス、コーディング パターン、分野固有の知識を与えられます。これにより、特定の種類の作業をより効果的に行えるようになります。チームの専門知識を再利用可能なエージェントとしてまとめることもできます。たとえば、[Web Content Accessibility Guidelines（WCAG）][wcag] への準拠を確認するアクセシビリティ エージェント、安全なコーディング プラクティスに従うセキュリティ エージェント、一貫したテスト パターンを保つテスト エージェントなどです。

カスタム エージェントは、プロジェクト内の `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter を記述し、その後にエージェントの動作、専門知識、命令を定義する Markdown のプロンプトを記述します。

### カスタム エージェントと agent skill の違い

カスタム エージェントと [agent skill][agent-skills-concept] には、機能上の重なりがあります。どちらも主に Markdown ファイルで定義され、AI に作業の方法を指示します。最も分かりやすい違いは、**カスタム エージェントは作業者、skill はツール**だという点です。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、作業の一部として skill（ほかのエージェントも含む）を調整するように設計されています。このラボでは、アクセシビリティのカスタム エージェントがアクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の一環として、pull request のワークフローを扱う skill や、テストを実行・管理する skill などを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI に関するほかの作業と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚的な能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにすることに取り組んでいます。最近のユーザーからのフィードバックによると、テキストと背景色のコントラストが不十分なため、現在のダーク テーマでは読みにくいと感じる人がいます。このアクセシビリティの懸念に対処するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装する必要があります。機能の生成には、カスタム エージェントを使います。

この演習では、次のことを行います。

- 既存のアクセシビリティ用カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントは、すでに作成されています。Copilot をどのように導くか理解するため、内容を確認しましょう。

codespace に戻り、ターミナルを開いて、アクセシビリティ作業用の新しいブランチを `main` から作成して切り替えます。演習 3 のフィルタリング pull request（PR）は別のブランチに保ちます。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` フィールドと `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシビリティに配慮した Web サイトのコード生成に関する主要な責務。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## Copilot Chat でカスタム エージェントを使う

VS Code では、`.github/agents` で定義されたすべてのカスタム エージェントが、Copilot Chat ビュー下部のエージェント ドロップダウンに表示されます。カスタム エージェントを選択すると、そのエージェントの命令とツールにチャット セッションの範囲を限定できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

1. Chat ビューのエージェント ドロップダウンで、まだ選択されていない場合は **Agent** を選択します。

   ![Chat ビューでエージェント ピッカーを表示する画面](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント ドロップダウンを選択します。現在アクティブなエージェントが表示されます。既定では **default** です。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. アクセシビリティ エージェントに次のプロンプトを送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat が作業を始めます。コードベースを調べて変更を提案し、プロジェクト ファイルを編集します。編集内容は、ファイル パスと差分とともにチャット内に表示されます。
6. 編集が追加されると、**Files changed** の表示が更新され、このエージェントが変更した作業ファイルを確認できます。

> [!NOTE]
> この作業には数分かかることがあります。Copilot はリポジトリに実際の変更を加えます。作業中に Astro コンポーネント、CSS、関連するテストなどの既存ファイルを編集します。

次の演習では、進行中の作業を確認し、指示を追加します。

## まとめと次のステップ

この演習では、特定のタスクや分野に合わせた専門的な AI アシスタントである、GitHub Copilot の[カスタム エージェント][custom-agents]を確認しました。カスタム エージェントを使うと、チームの専門知識と標準を再利用可能なエージェントとしてまとめ、Copilot が特定の種類の作業をより効果的に行えるようにできます。

次の概念を確認しました。

- カスタム エージェントの定義方法。
- Copilot Chat の agent mode でカスタム エージェントを使う方法。

次は、同じセッションに light mode の切り替えを追加しながら、[エージェントの作業を監視して指示します][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使う準備をする][org-custom-agents]
- [Enterprise でカスタム エージェントを使う準備をする][enterprise-custom-agents]

---

| [← 前の演習: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次の演習: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
