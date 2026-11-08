---
title: "演習 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つの pull request、再利用可能なカスタマイズ、追加のリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys で一連の開発作業を行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを調べ、用意されているフィルタリングの issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初の pull request（PR）を手動でマージしました。
- フィルタリングの issue から始め、Plan モードで進め方を決め、Autopilot モードで実装し、Interactive モードで確認しました。
- カスタム命令でエージェントを導き、既存の `quality-checks` skill をカスタマイズして、ユニット テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol（MCP）サーバーを追加し、実際のブラウザーでフィルタリングを試しました。
- 要件、テスト カバレッジ、skill の結果、ブラウザーでの検証結果を評価するため、品質保証（QA）用のカスタム エージェントを作成して選択しました。
- フィルタリング機能全体を確認し、その PR に Agent Merge を使用することを承認しました。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使うスラッシュ コマンドを確認しました。

## 作成したもの

このワークショップには、更新した `main` からそれぞれ別のブランチを作成する、2 つの PR の節目があります。

1. **星評価**: 既存の `starRating` と、評価がないことを明示する表示をゲーム カードに追加します。
2. **フィルタリングと品質確認のワークフロー**: フィルタリングを実装し、命令を更新して機能に適用し、`quality-checks` レポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルタリングの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザーでの確認、MCP を使った Copilot によるブラウザー操作という、複数の方法でコードを確認しました。`quality-checks` skill はユニット テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA は、その結果に要件とテスト カバレッジのレビューを加えて、PR の前にまとめて確認できるようにしました。

追加するテストは、実際に不足しているカバレッジを補うものである必要があります。QA を実行しても新しいテストが不要と判断される場合があります。これは適切な結果です。マージを承認する前に、コードと根拠を確認してください。変更があった場合は、影響を受ける根拠を更新してから確認します。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業結果を左右します。このワークショップでは、命令を更新し、skill をカスタマイズし、QA プロファイルを作成して、MCP サーバーを構成しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整してください。命令は標準を設定し、skill は繰り返し実行できるタスクを説明し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけで判断せず、実際の変更とツールの結果を確認してください。

タスクに合った **モードとモデル** を選びます。**Plan** は実装前に進め方を考えるために、**Interactive** は重点を絞った変更に自分も関わるために、**Autopilot** は範囲を明確にしたタスクを進めるために使います。定型的な編集には高速なモデルを選び、複雑な作業にはより高性能なモデルを選びます。

ツールや環境と同じくらい、コンテキストも重要です。何を、なぜ、どのように構築するのかを明確に伝えることで、出力は大きく変わります。

## さらに試す

ワークショップの主要なワークフローを学びました。Copilot CLI には、ほかにも確認する価値のある機能があります。

- `/review` を使うと、コード レビュー エージェントに変更の分析を依頼できます。
- `/rubber-duck` を使うと、問題について対話し、別の視点を得られます。
- `/fleet` を使うと、独立したサブタスクを並行して調整できます。
- `/worktree` を使うと、別のタスクを分離できます。
- `/delegate` を使うと、Copilot cloud agent にタスクを送信できます。

## 次のステップ

どのツールも、使い続けることが上達への近道です。実際のプロダクション コード、趣味のコード、長年作ろうと思っていた小さなアプリなどで活用してください。チームで知識を共有し、互いの経験から学びましょう。そして、ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境での作業を比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI で pull request を管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
