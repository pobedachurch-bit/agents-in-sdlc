---
title: "练习 6 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并使用它在浏览器中体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

正如前面提到的，编写代码不只是写代码。我们还需要处理数据、外部服务，并让 Copilot 能够使用更多自动化工具。这正是 MCP 服务器的用武之地。MCP 服务器可以让 Copilot 使用 CLI 内置功能之外的工具和服务。

在本练习中，将：

- 了解模型上下文协议（Model Context Protocol，MCP）以及 Copilot CLI 如何使用它。
- 如果尚未提供 Playwright MCP 服务器，则添加该服务器。
- 让智能体操作浏览器，体验筛选功能。

## 场景

单元测试和端到端测试很重要，但要验证 UI 更新，还需要实际与 UI 交互。你希望 Copilot 能像用户一样使用正在开发的网站，从而进一步自动化更改流程，并增强对更新效果的信心。

## 什么是模型上下文协议（MCP）？

[模型上下文协议（MCP）][mcp-blog-post]为 AI 智能体与外部工具和服务通信提供了一种方式。借助 MCP，AI 智能体可以实时与外部工具和服务通信，从而访问最新信息并代你执行操作。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁。每个 MCP 服务器都代表 AI 智能体可以使用的一组工具和资源。

一些常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、issue 和 pull request 的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：通过 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 提供了一个 [MCP 注册表][mcp-registry]，帮助开发者发现并为生态系统做出贡献。

> [!CAUTION]
> 对待 MCP 服务器应与对待项目中的其他依赖项一样。使用前，请检查其源代码、验证发布者，并考虑相关安全影响。

## 添加 Playwright MCP 服务器

如果尚未提供 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过这些设置步骤，继续下一部分。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 名称输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下内容：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux），保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 体验功能

之前你手动确认了功能是否符合预期。现在使用 Copilot CLI 中的 Playwright MCP 服务器完成同样的检查。

1. 使用以下提示，让 Copilot 使用 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常，Copilot 会自行判断应使用 MCP 服务器，因此不必明确要求它使用该服务器。不过，既然你知道它应该使用什么，明确指出也很有帮助！这样可以让结果更一致，还能节省一些 token。

2. 观察 Copilot 如何列出它在浏览器中执行的各项操作，以确认功能是否正常。
3. 阅读报告，确认所有功能均符合预期。

Copilot 会启动服务器，使用 Playwright 与网站交互，停止服务器，然后向你提供报告。

## 总结和后续步骤

恭喜，你已使用 Playwright MCP 服务器，通过 Copilot CLI 在真实浏览器中体验功能！回顾一下，你：

- 了解了模型上下文协议（MCP）以及 Copilot CLI 如何使用它。
- 如果 Playwright MCP 服务器尚未提供，则添加了该服务器。
- 让智能体操作浏览器，体验了筛选功能。

接下来，[创建 QA 自定义智能体][next-lesson]，将技能和浏览器工具整合到专门的角色中。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
