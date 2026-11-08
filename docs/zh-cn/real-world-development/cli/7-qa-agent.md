---
title: "练习 7 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，整合 issue 要求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动化检查，并使用 Playwright MCP 在浏览器中观察筛选功能。现在将这些能力整合到自定义智能体中，为其定义清晰的 QA 流程。

在本练习中，将：

- 了解自定义智能体如何与说明、技能和 MCP 工具协作。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 智能体，并根据筛选 issue 检查其发现。

## 场景

Tailspin Toys 希望在打开 pull request（PR）前，一致地检查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义智能体可以协调这一 QA 流程，并提供可复用的报告。

## 什么是自定义智能体？

自定义智能体是在 Markdown 配置文件中定义的 Copilot 专用版本。配置文件描述智能体的用途、说明和可用工具。在本工作坊中，你将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

之前使用的自定义项各有用途：存储库说明描述团队标准；`quality-checks` 技能封装可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置文件则告诉 Copilot 如何利用这些能力评估需求并报告发现。它不会取代这些功能，也不需要另开对话。

## 创建 QA 配置文件

打开功能 PR 前，让 Copilot 创建可复用的 QA 配置文件。配置文件会定义 QA 执行的检查以及必须遵守的边界。

1. 返回 Codespace，并确认筛选功能对话处于 Interactive 模式。
2. 请求默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体前，先检查其配置文件，确认 Copilot 捕获了预期的 QA 流程和边界。这样可以避免智能体在你只想验证功能时，因配置不完整或范围过宽而修改功能。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 是必需项；`name` 是可选项，但添加它能为智能体提供明确的显示名称。
3. 阅读配置文件中的说明，确认 QA 会从需求出发、遵循存储库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会先询问，并且不会提交更改或打开 pull request。
5. 如果生成的配置文件遗漏了上述职责或边界，请在继续之前要求默认智能体修改。

## 根据 issue 运行 QA

Copilot CLI 会在对话启动时加载项目智能体。创建配置文件后，恢复同一筛选功能对话并选择 QA，这样它就能使用现有的 issue 和规划决策。

1. 使用以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能不会显示在可用智能体列表中。它仍然可用，上面的命令会启用它。

2. 使用以下提示，请 QA 智能体检查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. 等待 QA 智能体开始工作。
4. 完成后，阅读它提供的报告。

## 总结和后续步骤

你已为工作流添加可复用的专业角色，并检查了它的工作。本练习中，你：

- 了解了自定义智能体如何与说明、技能和 MCP 工具协作。
- 创建并检查了可复用的质量保证配置文件。
- 选择了 QA 智能体，并根据筛选 issue 检查了其发现。

现在，实现、技能更新、QA 配置文件、测试和验证报告均已准备好等待检查。接下来，将它们[整合到功能 PR 中，并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
