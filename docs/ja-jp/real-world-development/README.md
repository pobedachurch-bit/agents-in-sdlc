---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更の計画、コードの作成、動作のテスト、pull request のレビュー、エージェントによる作業管理を繰り返しながら進めます。

## シナリオ

開発者をテーマにしたボードゲームのクラウドファンディングを提供する架空の会社、Tailspin Toys に入社したばかりの開発者として作業します。チームのバックログは GitHub Issue として登録済みで、すぐに着手できます。フィルターやページネーションなどの機能開発に加えて、アクセシビリティやコーディング標準などの品質改善も含まれています。

## 作業環境を選ぶ

GitHub Copilot は、さまざまな作業環境で利用できます。希望する開発方法に合った環境を選びます。各ワークショップは独自のセットアップから始まり、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使用します。Plan モードと Autopilot モード、指示、スキル、カスタム エージェント、Playwright MCP、Agent Merge、便利なスラッシュ コマンドを扱います。

### [GitHub Copilot app][app]

複数のエージェント セッションを並行して実行し、セッション モードを切り替え、Canvas で共同作業を行い、デスクトップ アプリで GitHub Issue と pull request を管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業をガイドし、進行状況を監視して、エージェントが作成した pull request をレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使用します。エディターを離れずに、Copilot Chat のエージェント モード、MCP サーバー、カスタム エージェントを活用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
