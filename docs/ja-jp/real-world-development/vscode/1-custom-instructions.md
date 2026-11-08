---
title: "演習 1 - カスタム命令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前の演習: 前提条件][previous-lesson] · [次の演習: Agent mode →][next-lesson]

生成系人工知能（AI）を使ううえで、コンテキストは重要です。タスクを特定の方法で進めたい場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。中でも重要なのは、コード生成に関するガイダンスを提供する[命令ファイル][instruction-files]です。

この演習では、次のことを行います。

- リポジトリのカスタム命令とパスに応じた命令ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどのように伝わるかを確認します。
- 現在の命令を使って、フィルタリングに必要な最初のデータ部分（販売元 helper）を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成されたコードが新しい標準に従うことを確認します。
- 命令の更新とフィルタリングの最初の部分を `main` にコミットし、次の演習で Copilot が更新したガイダンスを使えるようにします。

> [!CAUTION]
> 生成されたコードが設定した標準に従わないことがあります。Copilot の動作は確定的ではありません。この演習の目的は、命令を更新した後の動作の**傾向**を確認することであり、出力を一字一句同じにすることではありません。

## 命令ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。内容は次のとおりです。

- データ レイヤーには、必ずユニット テストを用意します。
- ユーザー インターフェイス（UI）はダーク モードにし、現代的な印象にします。
- TSDoc のドキュメント コメントをコードに追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

命令ファイルを使い、Copilot がこれらのプラクティスに沿ってタスクを実行できるよう、必要な情報を提供します。

### カスタム命令

カスタム命令を使うと、Copilot にコンテキストや好みを伝え、コーディング スタイルや要件を理解しやすくできます。ライブラリや規約などに加え、コメントの書き方も指定できます。リポジトリ全体に適用する命令ファイルも、特定の種類のファイルやタスクに適用する命令ファイルも作成できます。

命令ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md`: リポジトリに関するすべての依頼で Copilot に渡される、単一の命令ファイルです。Copilot に送る多くの依頼に関係する、プロジェクト レベルの情報を記載します。たとえば、技術スタック、構築するものの概要、ベスト プラクティス、全般的なガイダンスなどです。
- `.github/instructions/*.instructions.md`: 特定のタスクやファイルの種類に関する命令を記載します。TypeScript や Astro などの言語、UI コンポーネントの作成やユニット テストの追加などのタスクに関するガイドラインを指定できます。

> [!NOTE]
> 統合開発環境（IDE）では、命令ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの命令ファイルと、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は、AGENTS.md や CLAUDE.md など、ほかの標準に基づく命令ファイルにも対応しています。[カスタム命令のサポート][custom-instructions-support]を参照してください。

### 命令ファイルを管理するためのベスト プラクティス

命令ファイルの作成について詳しく説明することは、このワークショップの範囲外です。ただし、サンプル プロジェクトには代表的な方法が示されています。概要は次のとおりです。

- `copilot-instructions.md` には、構築するものの説明、プロジェクト構造、全体に適用するコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、ファイルの種類（ユニット テスト、Astro コンポーネント、データ レイヤーなど）や特定のタスクに関する命令を記載します。
- 自然な言葉を使い、明確なガイダンスを書きます。コードの良い例と避けるべき例を示します。

命令ファイルの作り方に決まった形式はありません。AI の使い方に唯一の正解がないのと同じです。実際に試しながら、プロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な命令ファイルを用意することをお勧めします。このプロジェクトのファイルを確認すると、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、多くのタスク向けの命令が見つかります。
>
> Copilot に命令ファイルを生成してもらうこともできます。操作方法は利用する環境によって異なります（たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など）。必要な場合は、利用中の環境に応じた演習で説明します。
>
> テンプレートや出発点を探している場合は、命令ファイル、カスタム エージェントなどのリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令ファイルを確認する

このリポジトリに用意されている命令ファイルを確認します。全体に適用する `copilot-instructions.md` が 1 つと、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には、**GitHub Actions Workflows** に関するガイダンスもあります。これらの内容は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどに関する命令があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリ ルートからの相対パスで命令を適用するファイルを決める glob を指定しています。ここでは、たとえば `**/*.test.ts` に一致する TypeScript のテスト ファイルが対象になります。
5. このプロジェクトでユニット テストを作成するときに適用される命令を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの命令ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。これにより、大きな命令セットを小さく再利用しやすいファイルに分け、コード生成時に Copilot が参考にできる例を示せます（ここに記載されたパスはリポジトリ ルートではなく、命令ファイルからの相対パスです）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順では、TSDoc のドキュメント コメントとファイル コメント ヘッダーのルールを追加します。

## 変更用のブランチを作成する

ベスト プラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 命令を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

カスタム命令の影響を確認するため、まず現在の命令ファイルを使ってプロンプトを送信します。その後、命令ファイルを更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習で使ったエディター タブを閉じ、Copilot に必要なコンテキストだけを渡します。
2. `src/lib/publishers.ts` を開き、helper を配置する場所を Copilot に示します。
3. Chat ビューのエージェント ドロップダウンで **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューでエージェント ピッカーを表示する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べてコードを更新します。helper ファイルとテストを含む複数のファイルを変更することがあります。
6. 提案された helper が型付き関数であり、最初の引数に `db` クライアントを受け取り、販売元の型付き配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約に基づいています。
7. 提案されたコードに TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の動作は確率的であり、指示がなくてもドキュメント コメントを追加する可能性があります。その場合も問題ありません。命令を更新した後に動作が一貫することが、この演習の目的です。

## リポジトリ全体に適用する標準を追加する

前に説明したように、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。コードの提案を改善するために、リポジトリのコーディング標準を文書化しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. およそ 27 行目にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準は記載されていますが、コード内のドキュメントに関するルールはありません。そのため、生成された helper にドキュメント コメントがありませんでした。
3. 既存の標準の直後に、次の Markdown を追加します。これにより、Copilot にファイル コメント ヘッダーと TSDoc のドキュメント コメントを追加するよう指示できます。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前の演習で確認したように、命令ファイルはリポジトリ レベル（`.github/copilot-instructions.md`）に作成して全体に適用することも、特定の言語、ファイルの種類、タスク向けに `*.instructions.md` ファイルとして作成することもできます。今回追加したドキュメント コメントのルールのように、プロジェクト全体の標準にはリポジトリ レベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択してコンテキストをクリアします。
2. `src/lib/publishers.ts` を再度選択し、Copilot が正しいファイルに集中できるようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しいプロジェクト標準に従うよう指示し、次の演習で使う実際のコードに適用しました。

## 命令をコミットしてブランチを push する

命令ファイルもリポジトリ内のほかのファイルと同様に、ソース管理で管理します。命令ファイルの変更をコミットして、ブランチをリポジトリに push しましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押して、新しいターミナル ウィンドウを開きます。
2. ターミナルで次のコマンドを実行し、命令の更新と helper の変更が含まれていることを確認します。

   ```bash
   git status
   ```

3. ターミナルで命令の更新と Copilot が helper の基盤として変更したファイルをすべてステージし、コミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルでブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## pull request を作成してマージする

ブランチを push したので、pull request を作成し、バックログにあるドキュメント標準の issue に関連付けます。手動で行うこともできますが、プロジェクトに接続済みの GitHub ツールを使って Copilot に作業を任せます。Copilot に issue を見つけてもらい、issue をクローズする PR を作成してから、マージを依頼します。

> [!NOTE]
> この演習では初めて Copilot に GitHub の操作を依頼します。プロジェクト テンプレートでは、GitHub のツールがあらかじめワークスペースに接続されています。そのため、セットアップは不要です。Copilot が初めてツールを使うときに、VS Code で GitHub へのサインインを求められることがあります。表示された指示に従って許可してください。後の演習では、この接続に使われる Model Context Protocol（MCP）について学びます。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選択されていることを確認します。
3. 命令ファイルの更新に関する issue を探し、現在のブランチから pull request（PR）を作成するよう Copilot に依頼します。PR には命令の更新と販売元 helper の基盤の両方を記載します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探して PR を作成します。
5. Copilot が GitHub 操作の実行許可を求めたら、コマンドを確認し、適切な場合は **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージと、ローカルのブランチを `main` に戻すことを Copilot に依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 操作とシェル コマンドの実行許可を求めたら、コマンドを確認し、適切な場合は **Allow** を選択します。

GitHub Copilot の助けを借りて pull request を作成し、マージできました。

## まとめと次のステップ

VS Code の Copilot Chat を使って、次のことを行いました。

- このプロジェクトの命令ファイルから Copilot がコンテキストを取得する方法を確認しました。
- コード生成のプロンプトを送り、**既存の**命令を使った Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードが新しい標準に従うことを確認しました。
- 命令の更新と helper の基盤を `main` にコミットし、次の演習で使えるようにしました。

次は、Copilot がコードベース全体に新機能を追加するときに[agent mode で命令を活用します][next-lesson]。

### おまけの演習 — ページ分割を実装する

ゲームの一覧が増えると、ページ分割が必要になります。この演習で学んだことを使い、Copilot にページ分割を実装するよう依頼してください。次の点を検討します。

- 既存の命令ファイルを使うなど、既存のベスト プラクティスに従う。
- ページ分割の方法を検討する。ページ サイズをユーザーが選べるようにするか、固定にするかを決める。
- 希望する実装に必要なガイダンスを Copilot に提供するプロンプトを作成する。
- GitHub Copilot に変更とコンテキストを伝えながら、何度か改善する必要がある場合があります。これは Copilot と作業するときの通常の流れです。

## リソース

- [Copilot の ask、edit、agent mode: 機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
