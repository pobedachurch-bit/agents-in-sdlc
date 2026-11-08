---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウド上で非同期に作業できます。GitHub で作業を割り当てると、cloud agent はバックグラウンドでリポジトリを調べ、変更を加え、pull request を開きます。その間、開発者は別の作業を続けられます。

各演習では、まず cloud agent が従うカスタム命令を追加し、GitHub issue を割り当てて実装を依頼します。カスタム エージェントを確認して利用し、その進め方を調整する方法、agents ダッシュボードからセッションを監視して指示する方法を学びます。最後に、cloud agent が作成した pull request を確認し、結果を改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと codespace を作成する |
| [1. カスタム命令][ex1] | コンテキスト | cloud agent が従うカスタム命令を追加する |
| [2. Cloud Agent][ex2] | 非同期エージェント | GitHub issue を Copilot cloud agent に割り当てる |
| [3. カスタム エージェント][ex3] | 専門エージェント | カスタム エージェントを確認して使う |
| [4. エージェントの管理][ex4] | 監視 | エージェントのセッションを監視して指示する |
| [5. 反復改善][ex5] | レビュー | pull request（PR）を確認し、Copilot の作業を改善して、次の手順を選ぶ |

## 前提条件

このワークショップに参加する前に、次の準備をしてください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランを利用できる GitHub アカウント
- [ ] アカウントまたは組織で有効になっている Copilot cloud agent

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使う agent、Model Context Protocol（MCP）、コード レビュー、Copilot CLI の機能が含まれているため、すべてのハーネスを完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent は **Copilot Student、Pro、Pro+、Business、Enterprise** で利用でき、Copilot Free を除くすべてのプランが対象です。Copilot Business または Enterprise を利用している場合は、管理者が組織で有効にする必要があります。

> [!NOTE]
> このハーネスでは Model Context Protocol（MCP）を扱いません。Copilot で MCP サーバーを使う方法については、[CLI ハーネス](../cli/)または [VS Code ハーネス](../vscode/)を参照してください。

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
