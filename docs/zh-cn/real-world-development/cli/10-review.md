---
title: "练习 10 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个 pull request、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的完整工作流中使用 GitHub Copilot CLI。你：

- 准备了 Codespace，安装 Copilot CLI，探索项目，并找到预先创建的筛选 issue。
- 添加了星级评分，在转发的浏览器中检查结果，并手动合并了第一个 pull request（PR）。
- 从筛选 issue 开始，在 Plan 模式下确定方案，在 Autopilot 模式下实现功能，并在 Interactive 模式下检查结果。
- 使用自定义说明引导智能体，然后自定义现有的 `quality-checks` 技能，并使用该技能运行单元测试、lint 和类型检查。
- 添加了 Playwright 模型上下文协议（MCP）服务器，并通过它在真实浏览器中体验筛选功能。
- 创建并选择质量保证（QA）自定义智能体，以评估需求、覆盖率、技能结果和浏览器证据。
- 检查了完整的筛选功能更改，并授权 Agent Merge 合并筛选功能 PR。
- 探索了用于管理上下文、模型、共享和可选云端委托的斜杠命令。

## 完成的内容

本工作坊包含两个 PR 里程碑，每个里程碑都从更新后的 `main` 分支创建独立分支：

1. **星级评分：**在游戏卡片上显示现有的 `starRating` 及明确的未评分状态。
2. **筛选和质量工作流：**实现筛选功能，更新说明并将其应用于功能，自定义 `quality-checks` 报告，创建 QA 配置文件，并添加相关测试。

从规划筛选功能到打开 PR，你始终使用同一对话和分支。为了简化工作坊流程，这些工作都合并在一个 PR 中。

## 不同的验证方式

你通过多种方式检查了代码：自动化测试、亲自在浏览器中检查，以及使用 MCP 让 Copilot 操作浏览器。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按你新设定的格式报告结果。QA 将这些结果与需求和测试覆盖率检查结果整合起来，在 PR 创建前提供审查报告。

新增测试应解决实际存在的缺口；如果 QA 检查后认为无需新增测试，也可能是正确的。授权合并前，请检查代码和证据；更改代码后，应重新生成受影响的证据。

## 最佳实践

为 Copilot 提供的上下文和工具会影响它的工作。在本工作坊中，你更新了说明、自定义了技能、创建了 QA 配置文件，并配置了 MCP 服务器。可以在不同对话中复用这些自定义项，并根据团队需求调整。说明设定标准，技能描述可重复执行的任务，自定义智能体定义专门角色，MCP 服务器连接外部工具。应检查实际更改和工具结果，而不只是智能体的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 在编码前规划方案，使用 **Interactive** 参与专注于单一任务的更改，使用 **Autopilot** 处理范围明确的任务。日常编辑可选择响应更快的模型；复杂工作则可选择能力更强的模型。

上下文与基础设施同样重要。清楚说明要构建什么、为什么构建以及如何构建，会显著影响输出结果。

## 深入探索

你已完成核心工作流。还有一些值得了解的 CLI 功能：

- 使用 `/review`，请求代码审查智能体分析更改。
- 使用 `/rubber-duck` 讨论问题并获得另一种观点。
- 使用 `/fleet` 并行协调相互独立的子任务。
- 使用 `/worktree` 隔离单独任务。
- 使用 `/delegate` 将任务发送给 Copilot cloud agent。

## 后续步骤

提升工具使用能力的最佳方法就是持续实践！可以用它处理生产代码、个人项目，或多年来一直想构建却尚未着手的小应用。与团队分享经验，也向团队成员学习。别忘了继续探索文档。

如果想通过终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。该系列包含三个模块，带你准备模型、构建并部署托管智能体，以及将其集成到网站中。要比较其他环境，还可以探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot cloud agent 工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理 pull request][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
