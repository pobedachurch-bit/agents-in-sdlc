---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** 可让 GitHub Copilot 在云端异步工作。你可以在 GitHub 上分配任务，cloud agent 会在后台接手工作——探索存储库、进行更改并打开 pull request——而你可以继续处理其他事情。

在这些练习中，你将添加 cloud agent 会遵循的自定义说明，然后分配一个 GitHub issue，让它实现相应工作。你还将检查并使用自定义智能体来引导它的工作方式，在智能体仪表板中监控并引导会话，最后检查它创建的 pull request，并迭代改进结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 Codespace |
| [1. 自定义说明][ex1] | 上下文 | 添加 cloud agent 会遵循的自定义说明 |
| [2. Cloud Agent][ex2] | 异步智能体 | 将 issue 分配给 GitHub Copilot cloud agent |
| [3. 自定义智能体][ex3] | 专用智能体 | 检查并使用自定义智能体 |
| [4. 管理智能体][ex4] | 监控 | 监控并引导智能体会话 |
| [5. 迭代改进][ex5] | 审查 | 检查 pull request（PR）、迭代改进 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本工作坊前，请确保具备以下条件：

- [ ] 拥有 GitHub 账户，并已启用 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划
- [ ] 已为你的账户或组织启用 Copilot cloud agent

> [!TIP]
> 没有付费计划？已验证的学生可以通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊所用的智能体、模型上下文协议（MCP）、代码审查和 Copilot CLI 功能，因此可以用它完成本工作坊的所有实践路径。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot cloud agent 适用于 **Copilot Student、Pro、Pro+、Business 和 Enterprise**，所有计划均可使用，只有 Copilot Free 除外。使用 Copilot Business 或 Enterprise 时，管理员必须为组织启用该功能。

> [!NOTE]
> 本 Cloud agent 工作坊不涉及 MCP。要了解如何将 MCP 服务器与 Copilot 搭配使用，请参阅 [CLI 工作坊](../cli/) 或 [VS Code 工作坊](../vscode/)。

## 开始

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
