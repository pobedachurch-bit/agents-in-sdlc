---
title: "演習 8 - 機能の pull request を作成してマージする"
description: "フィルタリング機能、命令、skill の更新、品質保証（QA）プロファイル、テストをまとめて確認し、pull request（PR）を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリング機能の実装、命令の更新、skill の更新、品質保証（QA）プロファイル、テストは、すべて 1 つのブランチに保存されています。これらをまとめて確認し、pull request を開く準備ができました。星評価の pull request（PR）は自分でマージしましたが、今回は **Agent Merge** にマージのプロセスを任せます。

> [!NOTE]
> 通常は、機能、命令の更新、skill の更新、QA agent をいくつかの PR に分けます。ワークショップを簡潔にするため、ここではフィルタリングと品質確認の作業全体を 1 つの会話とブランチにまとめ、この PR に含めています。

この演習では、次のことを行います。

- Agent Merge とは何か、マージまでのプロセスをどう自動化するかを学びます。
- 機能全体の変更と検証の根拠を確認します。
- フィルタリングの PR を作成します。
- 確認を終えてから Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルタリング機能の開発では、Copilot を使って計画、実装、検証を行いました。Tailspin Toys では、マージの承認を開発者が管理しながら、PR に関する残りの作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、pull request をマージするために必要な残りの作業を自動化します。有効にすると、Copilot は継続的インテグレーション（CI）チェックの失敗を修正し、レビュー コメントに対応し、必要に応じて rebase を行うなど、PR を妨げている問題に取り組みます。その後、リポジトリで許可されていれば、GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せられます。Agent Merge を有効にする前に、作業を確認し、マージの準備ができていると判断してください。

## Agent Merge で PR を管理する

コードの作成が終わったら、全体を確認して PR を作成し、残りのプロセスを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 選択肢から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って、新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションの処理を確認します。すべて成功すると、マージが実行されます。
7. Copilot から `"PR #14 was squash-merged successfully."` のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリの設定、不足している権限を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されている場合は、レビュー済みのマージを手動で完了してください。

## まとめと次のステップ

コードの生成、テストと検証、pull request の処理など、開発プロセスのいくつかの部分を自動化しました。この演習では、次のことを行いました。

- Agent Merge とは何か、マージまでのプロセスをどう自動化するかを学びました。
- 機能全体の変更と検証の根拠を確認しました。
- フィルタリングの PR を作成しました。
- 確認後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に使える、[便利な Copilot CLI のスラッシュ コマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI で pull request を管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
