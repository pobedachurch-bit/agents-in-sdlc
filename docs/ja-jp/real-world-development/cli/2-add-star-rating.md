---
title: "演習 2 - 星評価を追加する: 小さく始める"
description: "Copilot CLI を使ってゲーム カードに小さな変更を加え、転送されたブラウザーで確認し、最初の pull request としてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試したので、次はプロジェクトに最初の変更を加えます。変更は小さく保ちましょう。ゲームのデータにはすでに星評価がありますが、ホーム ページのゲーム カードには表示されていません。エージェントに評価を表示するよう依頼し、変更を確認して、最初の pull request としてマージします。

この演習では、次のことを行います。

- 機能ブランチで Copilot の会話を始めます。
- エージェントにプロジェクトの小さな変更を依頼します。
- `/diff` で変更を確認します。
- アプリを実行し、転送されたブラウザーで変更を確認します。
- 最初の pull request（PR）を作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価を設定でき、ゲーム詳細ページにはすでに表示されています。一方、ホーム ページのゲーム カードに表示されるのは、タイトル、カテゴリ、販売元、説明だけです。ウォームアップとして、エージェントに既存の評価を各カードに表示させます。最初のセッションにぴったりな、小さく独立した変更です。

## 会話の構成

**会話**は、Copilot CLI と一緒にタスクに取り組む場所です。Copilot app とは異なり、通常の CLI の会話では、専用の worktree を作るのではなく、ターミナルで現在チェックアウトしているリポジトリと Git ブランチを使います。保存した会話に戻って同じやり取りを再開できますが、ファイルとブランチはディスク上の通常の Git 状態のままです。

会話では、プロンプトとエージェントの応答、ファイルの調査や編集に使ったエージェントのツール操作、そして `/diff` で確認できる変更の 3 つが表示されます。

## 会話を始めて変更を依頼する

機能の実装を始めるため、新しい会話を開始します。

1. Codespace に戻ります。
2. まだターミナルを開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）を押します。
3. まだ Copilot を起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. スラッシュ コマンド `/new` を入力して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトを使って変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲーム詳細の表示に使われるファイルを見つけて、必要なコードを作成します。これで Copilot CLI を使って新しい機能を追加できました。

## 差分を確認する

小さな変更であっても、人工知能（AI）が生成した変更はすべてマージ前に確認してください。ここでは Copilot CLI で変更を確認します。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. ゲーム カードに評価がある場合は数値が表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. テストで両方の状態が対象になっていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. 確認が終わったら、<kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> Copilot を含む生成 AI ツールは確率的に動作し、毎回同じ結果になるとは限りません。特定の実装を期待するのではなく、動作を確認してください。

## 変更を確認する

コードを読んだだけで動作すると判断するのではなく、Copilot に Web サイトを起動してもらい、Codespaces で転送されたブラウザーから更新されたユーザー インターフェイス（UI）を確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. ゲーム カードに 5 段階評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初の pull request を作成してマージする

機能を作成できました。次は、プロジェクトに変更を取り込む pull request（PR）を作成します。

1. 既定のエージェントに変更のコミットを依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR の作成時に既存のコミットを push し、PR の URL を表示します。
3. <kbd>Command</kbd>（Mac）または <kbd>Ctrl</kbd>（Windows/Linux）を押しながら、Copilot CLI に表示された URL を選択して PR を開きます。
4. 変更されたファイルとチェック結果を確認します。
5. 準備ができたら、**Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

おめでとうございます。GitHub Copilot CLI を使って最初の変更をリリースしました。具体的には、次のことを行いました。

- 機能ブランチで Copilot の会話を始めました。
- エージェントにゲーム カードの小さな変更を依頼しました。
- `/diff` で変更を確認しました。
- アプリを実行し、転送されたブラウザーで星評価を確認しました。
- 最初の pull request を作成してマージしました。

次は、フィルタリングに関する issue から始めて、Plan モードと Autopilot モードを使って大きな機能を構築します。[次の演習に進む][next-lesson]

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
