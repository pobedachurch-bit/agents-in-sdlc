---
title: "演習 2 - Copilot Agent Mode で新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの最も簡単な更新でも、通常は複数のファイルを変更し、テストを実行するなどの作業が必要です。開発者は、必要なファイルを見つけ、変更を加え、テストを実行し、デバッグし、変更し忘れたファイルを特定し、さらに更新するという流れで作業します。作業は次々と続きます。

そこで役立つのが Copilot Agent Mode です。

Copilot Agent Mode は、統合開発環境（IDE）内でより自律的に作業するように設計されています。開発者と同じように既存のプロジェクト構造を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使って、サイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる種類の体験を提供します。agent mode は IDE 内で作業するため、すぐに結果を確認してやり取りできます。cloud agent はチームの一員のように非同期で作業するペア プログラマーとして設計されており、issue と pull request を通じて開発者とやり取りします。

この演習では、次の方法を学びます。

- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定し、複数のファイルにまたがる変更を行う。
- GitHub Copilot Agent Mode でユーザー インターフェイス（UI）とデータ レイヤーに新機能を実装する。
- Copilot Agent Mode が生成した変更とテストを、コードベースにマージする前に確認する。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリや販売元で絞り込めるようにします。前の演習で販売元 helper を追加しました。次は、残りのデータ レイヤー、UI、テストを Copilot Agent Mode で仕上げます。

## Tailspin Toys の Web サイトを実行する

変更を始める前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握しましょう。

この Web サイトは、開発者をテーマにしたボード ゲームのクラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示できます。この Web サイトは単一の Astro アプリであり、ビルド時に静的 HTML としてページを生成します。ページは frontmatter 内で Drizzle ORM を通じてローカルの SQLite データベースを直接クエリします。別のバックエンド アプリケーション プログラミング インターフェイス（API）やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス helper は `src/lib/` に置き、操作性は標準の DOM API を使った小規模で範囲の限定された Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるように、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. codespace に戻ります。前の演習の後なので、`main` にいるはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）を押し、新しいターミナル ウィンドウを開きます。
3. フィルタリング作業用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を示すバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くブラウザー ウィンドウを表示するダイアログが開いた場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd>（Mac）または <kbd>Ctrl</kbd>（Windows/Linux）を押しながら選択して、Web サイトを開きます。

> [!NOTE]
> codespace を使っている場合、Codespace ターミナルで localhost の URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは、Web サーバーをホストしている codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認します。主な機能は次のとおりです。

- **ホーム ページ**: タイトル、画像、説明とともにボード ゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、販売元、カテゴリなどの詳しい情報が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加して改善したいと考えています。まずバックログを確認します。テンプレートからプロジェクトをセットアップすると、バックログの issue も自動的に作成されています。GitHub Copilot に項目を表示してもらいましょう。

1. Chat ビューのエージェント ドロップダウンで **Agent** を選択します。**Agent** はファイルにまたがる変更を自律的に計画して実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューでエージェント ピッカーを表示する画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案されるコードに一貫性を持たせ、スムーズに進められることを期待しています。ただし、大規模言語モデル（LLM）の出力は確率的であるため、ワークショップに記載されたものと異なる提案が表示されることがあります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が異なる Model Context Protocol（MCP）コマンドを使うことがありますが、タスクは同様に完了できるはずです。

3. 次のプロンプトを Copilot に送信して、issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot がユーザーに最も役立つと考える項目を優先順位付けしたことに注目してください。

## 命令ファイルを確認する

エージェントにコードを生成させる前に、作業のコンテキストを Copilot に伝える命令ファイルを確認します。Web サイトへの機能追加に関するガイダンスを含む [ユーザー インターフェイス（UI）](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) ファイルを使います。

1. codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加の進め方に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティに関する原則。
   - ファイルの種類ごとの命令ファイルへのリンク。対象には次のものがあります。
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 命令ファイルから、ほかの命令ファイルやプロジェクト内のファイルを参照できます。パスは命令ファイルの場所からの相対パスです。命令を再利用しやすくし、複雑な内容を小さく分割して、Copilot が参考にできる例やテンプレートを示せます。

## フィルタリング機能を実装する

フィルタリング機能を完成させるには、少なくとも次の 3 つを更新する必要があります。

- データ レイヤー（`src/lib/`）の残りのフィルタリング ロジックを追加または調整する。
- フィルタリング動作のテストを追加または更新する。
- フィルタリング UI を追加するために、ゲーム一覧ページを更新する。

さらに、コードベースにマージする前にテストを実行して成功させる必要があります。Copilot Agent Mode にこれらのタスクを任せましょう。

1. 現在の Copilot の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、命令ファイルを追加します。

   ![UI 命令ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェント ピッカーを表示する画面](../../../_images/shared-chat-mode-selector.png)

4. モデルとして引き続き **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログ内の関連 issue に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、必要な機能に関係するファイルを見つけてから作業を始める様子を確認します。データ レイヤーの helper、UI、テストが見つかります。その後、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクト ファイルを調べる画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースの複雑さによっては、数分かかることがあります。作業中にコード エディターに **Keep** と **Undo** が表示される場合があります。Copilot の作業が終わると、すべての変更に **Keep** または **Undo** が表示されます。作業中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat ペインに表示された、テストの実行確認ダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はタスクを完了し、エラーがないと判断するまでコード生成とテストを繰り返します。

   ![Copilot Agent Mode との会話が完了した画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードは、必ず確認してください。

10. Web サイトを起動したブラウザーに戻り、新しい機能を試します。
11. すべてが動作し、コードの確認も終わったら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

おめでとうございます。この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加する方法を確認しました。次のことを学びました。

- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定し、複数のファイルにまたがる変更を行う。
- GitHub Copilot Agent Mode で UI とデータ レイヤーに新機能を実装する。
- Copilot Agent Mode が生成した変更とテストを、コードベースにマージする前に確認する。

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、pull request を開きます。

### おまけの演習 — ページ分割を実装する

ゲームの一覧が増えると、ページ分割が必要になります。この演習で学んだことを使い、Copilot にページ分割を実装するよう依頼してください。コードを作成する際は、次の点を検討します。

- 既存の命令ファイルを使うなど、既存のベスト プラクティスに従う。
- ページ分割の方法を検討する。ページ サイズをユーザーが選べるようにするか、固定にするかを決める。
- 希望する実装に必要なガイダンスを Copilot に提供するプロンプトを作成する。
- GitHub Copilot に変更とコンテキストを伝えながら、何度か改善する必要がある場合があります。これは Copilot と作業するときの通常の流れです。

## リソース

- [Copilot の ask、edit、agent mode: 機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
