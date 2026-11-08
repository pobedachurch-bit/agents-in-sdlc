---
title: "练习 9 - GitHub Copilot CLI 斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选云端委托的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 也包含许多用于交互的斜杠命令。这些命令提供高级功能、幕后信息和额外配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在来探索其他一些实用命令。

在本练习中，将：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 了解可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行工作、工作树以及委托任务给 cloud agent 的可选命令。

## 场景

核心 CLI 工作流已完成。现在来看看其他一些功能：管理上下文、切换模型、共享会话，以及选择是否将任务委托给 [Copilot cloud agent][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理较大或较复杂的任务时，可能会达到模型上下文窗口的上限。Copilot CLI 会在需要时自动压缩对话；你也可以使用斜杠命令自行查看或压缩上下文。

1. 返回 Codespace；如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 使用量，以及上下文在系统说明、工具、消息和可用空间之间的分配情况。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能并不明显。

> [!NOTE]
> 当上下文窗口接近上限时，Copilot CLI 会自动压缩上下文。想自行决定压缩时机时，可以使用 `/compact`。如果要切换到不相关的任务并开始全新对话，可以使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，不同开发者也有各自偏好。Copilot CLI 支持列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型及其使用信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队成员之间协作并分享经验，有助于大家更好地使用 AI 工具。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看已安装版本中的 `/share` 选项。
2. 如果要共享此会话，请选择适合的目标，例如使用 `/share file` 导出本地 Markdown 文件。
3. 将导出的内容发送给他人或发布之前，先检查内容。会话导出内容可能包含提示、回复和项目详情。

发布链接或 gist 是可选操作。不要发布团队无意共享的存储库内容或对话内容。

## 可选：扩展工作或委托任务

核心工作坊内容已完成。Copilot CLI 还提供了适用于较大任务的命令：

- `/fleet` 可以将相互独立的子任务分配给子智能体并行执行。
- `/worktree` 可以为单独任务创建隔离的 Git 工作树。
- `/delegate` 可以将任务发送给 Copilot cloud agent，由它异步工作并可能打开 pull request。

这些命令是可选的，因为它们可能创建额外工作树或触发远程工作。尝试前，请先开始一个范围明确的新任务，并按照常规工作流检查结果。如果想深入了解异步智能体工作，可以继续学习[云端智能体工作坊][cloud-workshop]。

## 总结和后续步骤

通过 Copilot CLI 斜杠命令，你可以进行配置、共享会话，并查看幕后运行情况。本练习中，你：

- 使用 `/context` 和 `/compact` 了解了 Copilot 如何管理对话上下文。
- 使用 `/model` 了解了可用模型。
- 了解了 `/share` 如何导出或共享会话。
- 探索了用于并行工作、工作树以及委托任务给 cloud agent 的可选命令。

Copilot CLI 还有更多斜杠命令值得探索！接下来，[回顾已学内容][next-lesson]，并了解后续学习方式。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
