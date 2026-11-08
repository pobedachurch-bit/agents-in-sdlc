---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot アプリを使って Pages ワークフローを構成し、その pull request をレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages を使って Space Quiz をデプロイし、誰でもアクセスできる場所に公開します。Copilot はワークフローを準備し、デプロイを案内できますが、変更をレビューし、サイトが公開されたことを確認するのはユーザーの責任です。

このレッスンでは、次の操作を行います。

- Copilot に GitHub Pages のデプロイを構成するよう依頼します。
- マージする前にワークフローと pull request をレビューします。
- デプロイの状況を追跡し、公開されたサイトを確認します。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、手軽に試す方法が必要です。単一ファイルのサイトを GitHub Pages で公開すれば、サーバーやランタイム依存関係を追加せずに、公開 URL から利用できるようになります。

## Copilot にサイトのデプロイを依頼する

`space-quiz` の Copilot アプリ セッションで、次のプロンプトを送信します。

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![3 段階の図: マージ済みの space-quiz pull request、Build website、Upload artifact、Deploy to Pages の各ステップを含む成功した GitHub Actions の Deploy website 実行、yourname.github.io/space-quiz/ で公開された Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> このプロンプトは、エージェントに pull request の作成とマージ、およびリポジトリの Pages 設定の変更を依頼します。続行する前に、差分、デプロイ権限、GitHub の承認要求を確認してください。

## ワークフローをレビューしてデプロイを確認する

1. リポジトリに README がまだない場合に限り、README が追加されていることを確認します。
2. ワークフローのビルド手順とデプロイ手順を含め、pull request の変更をレビューします。
3. 必須の pull request チェックに合格した後にのみマージします。
4. GitHub Actions のデプロイ実行が成功するまで確認します。
5. 報告された URL を開き、Space Quiz が表示されることを確認します。

エージェントが Pages の有効化や pull request のマージを行えない場合は、提示された具体的な手順に従います。デプロイが成功し、公開サイトが読み込まれるまで、タスクを完了したと見なさないでください。

## まとめと次のステップ

Space Quiz を公開し、エージェントの報告をそのまま信頼せずにデプロイを確認しました。[レッスン 13: 振り返りと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
