---
title: "演習 4 - カスタム命令で Copilot を導く"
description: "リポジトリの命令を確認し、ドキュメントの標準を追加して、フィルタリング機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成系人工知能（AI）を使ううえで、コンテキストは重要です。タスクを特定の方法で進めたい場合は、そのガイダンスを Copilot が参照できるようにします。[命令ファイル][instruction-files]には、求めるコードだけでなく、コードの構成方法も記述できます。フィルタリング機能を作成したので、Copilot が使った命令を確認し、ドキュメントの標準を追加して、コードに適用します。

この演習では、次のことを行います。

- リポジトリの命令とパスに応じた命令ファイルがエージェントにどのように渡されるかを確認します。
- コーディング標準に従うよう、命令ファイルを更新します。
- 命令ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。内容は次のとおりです。

- コメントでは、コードの内容を言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポートされた関数では、目的、パラメーター、戻り値を TSDoc/JSDoc で文書化します。`db` 引数を注入できる場合は、その引数も含めます。
- 再利用可能な Astro コンポーネントでは、`Props` の契約を文書化します。関連するコードを変更した場合は、コメントも最新の状態に保ちます。
- 既存の書式設定と lint のガイダンスを維持します。

命令ファイルを使うことで、Copilot がこれらのプラクティスに沿って作業できるようにします。

## 命令ファイル

カスタム命令を使うと、Copilot にコンテキストや好みを伝え、コーディング スタイルや要件を理解しやすくできます。ライブラリや規約などに加え、コメントの書き方も指定できます。リポジトリ全体に適用する命令ファイルも、特定の種類のファイルやタスクに適用する命令ファイルも作成できます。

命令ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md`: リポジトリに関するすべての依頼で Copilot に渡される、単一の命令ファイルです。多くの依頼に関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md`: 特定の言語、ファイルの種類、タスクに関するガイドラインを記載します。

> [!NOTE]
> ほかの形式の命令とそのサポート状況は、環境によって異なります。特定の形式を利用する前に、[カスタム命令のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## プロジェクトのカスタム命令ファイルを確認する

スターター プロジェクトには、すでにいくつかの命令ファイルが用意されています。変更を加える前に、既存の内容を確認して、その影響を確かめましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング ガイダンスを確認します。この命令は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ レイヤー、テストなどに関する命令が含まれています。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、命令を適用するファイルを決める glob を指定していることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの命令ファイルやプロジェクト内のファイルへのリンクを確認します。命令を小さく再利用しやすい単位に分け、参考例へのリンクを付けられます。

## チームのガイダンスに合わせて命令ファイルを更新する

現在のファイルは良い出発点ですが、追加のガイダンスが必要です。新しく生成する TypeScript に TSDoc コメントを追加するよう、コアの `copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** という見出しのセクションを探します。およそ 35 行目にあります。
2. 次の内容を最後の箇条書きとして追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの命令を読み込みます。機能に関するコンテキストを保ったまま新しいガイダンスを読み込めるよう、保存後に会話を終了して再開します。

1. フィルタリングの会話がまだ開いている場合は、`/exit` を入力して終了します。Copilot CLI が会話を自動的に保存します。
2. リポジトリのルートから `game-filters-cli` ブランチに切り替えた状態で、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合は、フィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 次のプロンプトを使い、更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が命令ファイルのコンテキストをどのように取得するかを確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの命令とパスに応じた命令ファイルがエージェントにどのように渡されるかを確認しました。
- コーディング標準に従うよう、命令ファイルを更新しました。
- 命令ファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` skill をカスタマイズして実行し、lint とテストが一貫して実行されるようにします。[次の演習に進む][next-lesson]

## リソース

- [Copilot CLI にカスタム命令を追加する][instruction-files]
- [カスタム命令のサポート][custom-instructions-support]
- [効果的なカスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
