---
title: "演習 4 - エージェントを監視して管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の 2 つの演習では、ユーザー エクスペリエンスの改善と機能追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期に自律して動作しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。[GitHub.com の agents ページ][agents-page]もその 1 つです。このコントロール センターでは、オープンな pull request（PR）があるすべてのエージェント タスクを確認できます。実行された操作を調べたり、進行中のセッションに指示を追加して作業の方向を導いたりできます。

この演習では、次のことを行います。

- agents ページで cloud agent のタスクを監視します。
- 進行中のセッションに指示を追加し、機能の追加を依頼します。

## シナリオ

エージェントにハイ コントラスト モードの作成を依頼した後、チームはライト モードも追加する良い機会だと考えました。サイトのスタイルを更新して切り替え機能を追加しているため、ライト モードも含めるのが自然だと思われました。エージェントの作業に指示を追加し、ハイ コントラスト モードに加えてライト モードも追加するよう依頼します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインのタスク一覧を確認します。Copilot に割り当てた次のタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲームを変更するための API を生成する。
   - Web サイトにハイ コントラスト モードを追加する。
3. 実行中のタスクを 1 つ選択し、Copilot が実施した作業を確認します。次のような操作が含まれる場合があります。
   - リポジトリからコードをチェックアウトする。
   - Copilot が作業する環境を作成する。
   - MCP サーバーをセットアップする。
   - 割り当てられたタスクを完了するためにさまざまな手順を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示される pull request（PR）ペインも確認してください。追加の監視に使える PR と変更ファイルが表示されます。

## cloud agent に指示する

実行中のタスクを確認したので、エージェントがハイ コントラスト モードに取り組んでいる間に、ライト モードの切り替えも追加するよう依頼します。

1. ハイ コントラスト モードを追加するセッションを選択します。エージェントが付けた名前や作業状態によって、タイトルは異なります。

   ![mission control に表示されるアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業を始めたことが示されるまで、数分間セッションを確認します。次のようなメッセージが表示されたら、作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![agents ページの Copilot タスクと、選択された Steer active session while Copilot is working ダイアログ](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業の流れに追加したことを確認します。

## Copilot に作業を続けてもらう

これまでと同様に、更新されたタスクに Copilot が取り組みます。メッセージを送信した時点で実行していた手順が終わった後に、新しい依頼が作業の流れに追加されます。

ここでも完了まで数分かかります。待っている間に、これまでに学び、確認したことを振り返りましょう。

## まとめと次のステップ

この演習では、GitHub Copilot cloud agent のタスクを監視して導くための中心的な場所である、Copilot agents ページを確認しました。このコントロール センターでは、すべてのアクティブなタスクと完了したタスクを追跡し、実行中の作業を確認して、進行中のタスクの範囲を変更したり、追加のガイダンスを与えたりできます。

次の概念を確認しました。

- cloud agent のタスクを監視するため、mission control と agents ページを確認しました。
- 進行中のセッションに指示を追加し、機能の追加を依頼しました。

Copilot がアクセシビリティ機能を作成している間に、次は [Copilot が作成した pull request を改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前の演習: カスタム エージェント][previous-lesson] | [次の演習: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
