---
title: "演習 1 - カスタム命令 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前の演習: 前提条件][previous-lesson] · [次の演習: Copilot cloud agent →][next-lesson]

生成系人工知能（AI）を使ううえで、コンテキストは重要です。タスクを特定の方法で進めたい場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[命令ファイル][instruction-files]を使うと、Copilot に何をしてほしいかだけでなく、どのように実行してほしいかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム命令とパスに応じた命令ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどのように伝わるかを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code ハーネスや CLI ハーネスとは異なり、この演習では変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub issue に対して非同期に作業するため、リアルタイムで並べて効果を示すのが難しいためです。命令ファイルの影響は、このハーネスの後半で cloud agent が作成した pull request を確認するときに見られます。

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

- `.github/copilot-instructions.md`: リポジトリに関するすべての依頼で Copilot に渡される、単一の命令ファイルです。Copilot に送る多くのチャットや CLI の依頼に関係する、プロジェクト レベルの情報を記載します。たとえば、技術スタック、構築するものの概要、ベスト プラクティス、全般的なガイダンスなどです。
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
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、命令を適用するファイルを決める glob を指定しています。ここでは、たとえば `**/*.test.ts` に一致する TypeScript のテスト ファイルが対象になります。
5. このプロジェクトでユニット テストを作成するときに適用される命令を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの命令ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。これにより、大きな命令セットを小さく再利用しやすいファイルに分け、コード生成時に Copilot が参考にできる例を示せます（ここに記載されたパスはリポジトリ ルートではなく、命令ファイルからの相対パスです）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順では、TSDoc のドキュメント コメントとファイル コメント ヘッダーのルールを追加します。

## リポジトリ全体に適用する標準を追加する

この演習で編集するのは、プロジェクト全体に適用するルールの追加だけです。コード内のドキュメントを TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーで記述するようにします。cloud agent は、このハーネスの後半で割り当てられた issue に取り組むときに、このルールを読み込みます。

編集の前に、作業用のブランチを作成します。cloud agent は issue の対象ブランチから命令を読み込みますが、変更を整理してコミットするため、ここではブランチを使います。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 命令の変更をコミット、push、マージする

cloud agent は、issue が対象とするブランチの命令ファイルを読み込みます。次の演習で Copilot に issue を割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が命令の変更を読み込めるように、変更を `main` に取り込む必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けた pull request を開き、マージします。次の演習で割り当てる issue に取り組むとき、cloud agent はこの命令を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチ作成を省略して `main` に直接コミットしてもかまいません。実際のプロジェクトでの進め方に近づけるため、ここではブランチを作成しています。

## まとめと次のステップ

このプロジェクトの命令ファイルから Copilot がコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。このハーネスの後半で、cloud agent が作成する pull request を確認するときに、この標準が適用されていることを確認できます。

次は、最初の issue を Copilot cloud agent に[割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [効果的なカスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム命令を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
