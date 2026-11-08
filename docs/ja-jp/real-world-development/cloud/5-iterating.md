---
title: "演習 5 - GitHub Copilot の作業を改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前の演習: エージェントの管理][previous-lesson] |
|:--|

## 作業内容を確認する

このラボでは、ユーザー エクスペリエンスを改善し、機能を追加するいくつかのタスクで GitHub Copilot を使いました。Copilot にコードのドキュメント追加、デザイン チームが改善を重ねられる関連ゲーム機能の作成、ハイ コントラストとライト モードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送り、作業を改善しましょう。

### シナリオ

何度も説明してきたように、生成系人工知能（AI）を加えてもソフトウェア設計と DevOps の基本は変わりません。生成されたコードを常に確認し、通常の DevOps プロセスに沿って進めます。そこで、チームのレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は監視されずに非同期でタスクを実行するため、安全を保つためのセキュリティ制約が設けられています。次のような制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みアクセスはコードの作業に使うブランチに**限られます**。
- cloud agent は GitHub Actions 内で動作し、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- Model Context Protocol（MCP）サーバーを含む[外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントを確認する

最初に、GitHub Copilot cloud agent が生成した最初の pull request（PR）、コードへのドキュメント追加を確認します。github.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認するときに、GitHub Copilot がファイアウォールでブロックされているという警告が表示されることがあります。Copilot は、外部 MCP サーバーへの呼び出しなど、既定では外部リソースへのアクセスが制限されているため、これは**想定どおりの動作**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、pull request の一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いた pull request を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、pull request のタイトルに **[WIP]** が含まれます。その場合は、Copilot の作業が終わるまで待ってください。数分かかることがあります。休憩を取ったり、ここまでに学んだことを振り返ったりしてもよいでしょう。

4. pull request の準備ができたら、**Files changed** タブを選択して変更を確認します。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc のドキュメント コメントなど、新たに更新されたコードを確認します。実際の変更は環境によって異なります。

   変更を確認するときは、TSDoc のドキュメント コメント、TypeScript の規約、コメント ヘッダーを探します。これらは、演習 2 の最初に確認したカスタム命令ファイルによって追加されています。

6. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローが承認待ちであることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)

9. pull request のチェック セクションに、ワークフローがキューに登録されたことが表示されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

pull request で Copilot と作業するのは、一方通行ではありません。チームのほかのメンバーと同じように、pull request のコメントやコード内のインライン コメントで Copilot にメンションできます。Copilot はコメントを認識し、対応するための新しいセッションを開始します。結果は確定的ではないため、必ずこう依頼すべきという文面はありません。次のような更新を依頼できます。

- 各コード ファイルの先頭に、ファイルの内容を簡単に説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメント コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. **@copilot** をほかのユーザーと同じようにメンションして、生成されたドキュメントへの変更を依頼するコメントを追加します。上記の例、またはコードベースに追加してほしいドキュメントについて、別の案を使ってください。
2. **View Session** を選択して、Copilot の作業を確認します。Copilot が更新用の新しいセッションを始めることに注目してください。
3. **Back to pull request** を選択すると、pull request に戻れます。

   ![pull request に戻るボタン](../../../_images/ex7-back-to-pr.png)

4. Copilot の変更が完了すると、pull request に新しいコミットが表示されます。
5. **Files changed** タブを選択して、変更を確認します。

満足するまで改善を繰り返してください。準備ができたら、ドラフトからレビュー可能な状態に変更して、`main` ブランチにマージできます。

![レビュー可能な状態に変更する画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能を確認する

ゲーム詳細ページに関連ゲームを表示する issue を解決するため、Copilot が作成した PR に戻ります。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
6. ワークフローが承認待ちであることを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)

8. pull request のチェック セクションに、ワークフローがキューに登録されたことが表示されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
9. **任意:** 関連ゲーム機能を手動でテストするため、Codespace でこのブランチに切り替えることもできます。Codespace に移動し、ターミナルを開いて、次のコマンドを実行します（`<branch-name>` は Copilot が作成したブランチ名（例: **copilot/fix-8**）に置き換えてください）。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent に更新を依頼して、反復的に作業できます。たとえば、表示する関連ゲームの数を調整したり、コメント ヘッダーと TSDoc のドキュメント コメントを追加したりするよう依頼できます（この作業は、カスタム命令を更新する**前**に割り当てたことを思い出してください）。前の PR と同様に、**Conversation** タブに新しいコメントを追加すると、Copilot がそれを認識して新しいセッションを開始します。

## アクセシビリティ機能を確認する

最後に、アクセシビリティ用のカスタム エージェントで実装した機能を確認します。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 の mission control で追加を依頼したライト モードの両方が含まれるはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、pull request のタイトルに **[WIP]** が含まれます。その場合は、Copilot の作業が終わるまで待ってください。数分かかることがあります。

4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 実装を確認し、特に次の点を確認します。
   - モードを切り替えるユーザー インターフェイス（UI）コンポーネント。
   - ユーザー設定を保存する local storage の使用。
   - ハイ コントラスト モードとライト モードに関する CSS またはスタイルの変更。
   - アクセシビリティ属性（Accessible Rich Internet Applications（ARIA）ラベル、キーボード ナビゲーションなど）。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローが承認待ちであることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)

9. pull request のチェック セクションに、ワークフローがキューに登録されたことが表示されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
10. **任意:** アクセシビリティ機能を手動でテストするため、このブランチに切り替えることもできます。Codespace に移動し、ターミナルを開いて、次のコマンドを実行します（`<branch-name>` は Copilot が作成したブランチ名に置き換えてください）。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   その後、アプリケーションを起動し、ブラウザーでハイ コントラストとライト モードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ用のカスタム エージェントが、アクセシビリティのベスト プラクティスに沿って Copilot の機能実装を導いたことに注目してください。アクセシビリティに関する懸念や改善点があれば、前の PR と同じようにコメントで **@copilot** にメンションして更新を依頼できます。

## 任意の演習 — 委任を続ける

cloud agent は、実際のバックログ項目を任せて別の作業に集中できるときに効果を発揮します。習慣にするため、リポジトリにいくつか issue を作成して Copilot に割り当ててみてください。たとえば、次のような作業があります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス helper に入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。演習を通じて、次のことを行いました。

- このリポジトリに用意されたカスタム命令ファイルを確認し、後で cloud agent の出力にどう影響するかを確かめられるようにしました。
- issue を Copilot cloud agent に割り当て、環境のセットアップ、計画、非同期の実行を確認しました。
- アクセシビリティ用のカスタム エージェントを作成して使い、ハイ コントラストとライト モードの切り替えを追加しました。
- Copilot agents ページを mission control として使い、作業中のアクセシビリティ セッションを監視して指示しました。
- cloud agent の pull request を確認して改善し、変更を依頼するため **@copilot** にメンションし、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。引き続き確認したい場合は、ほかのハーネスで今回の内容を補完できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から直接 Copilot Chat の agent mode と MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、agent skill、QA カスタム エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` 連携を使い、ターミナルからレビュー済みの変更を 2 つ届けます。

自分のリポジトリでは、次のアイデアを試してください。

- リファクタリングやテスト カバレッジの issue を cloud agent に割り当てる。
- 別の分野のカスタム エージェントを作成する。
- 異なるスタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前の演習: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
