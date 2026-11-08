---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一节：Agent mode][previous-lesson] · [下一节：自定义智能体 →][next-lesson]

你刚才使用 agent mode 构建了筛选功能。打开 pull request 前，应确认它在浏览器中确实有效。你无需亲自逐项操作应用，而是可以连接 **Playwright 模型上下文协议（MCP）服务器**，让 Copilot 操作真实浏览器为你测试功能，然后发布分支并打开 pull request（PR）。

在本练习中，将：

- 了解模型上下文协议（MCP）是什么，以及 MCP 服务器如何通过新工具扩展 Copilot。
- 将 Playwright MCP 服务器添加到工作区。
- 请求 Copilot 使用该服务器在浏览器中手动测试筛选功能。
- 发布分支，并为筛选工作打开 pull request。

## 什么是 MCP？

当 agent mode 可以访问编辑器之外的资源时，它的功能会强大得多。模型上下文协议（MCP）让 Copilot 可以做到这一点——它是一种让智能体与外部工具和服务通信的标准方式。

![agent mode 内部工作方式及其与上下文、大型语言模型（LLM）和工具的交互示意图，其中包括模型上下文协议（MCP）服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)为人工智能（AI）智能体与外部工具和服务通信提供了一种方式。借助 MCP，AI 智能体可以与外部工具和服务实时通信。这让它们能够使用资源访问最新信息，并使用工具代你执行操作。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，负责管理智能体与外部工具（例如现有应用程序编程接口（API）或 npm 包等本地工具）之间的通信。每个 MCP 服务器都代表 AI 智能体可以使用的一组工具和资源。

一些常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API。AI 智能体可以使用它执行创建新存储库、更新现有存储库以及管理 issue 和 pull request 等操作。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：通过 Playwright 提供浏览器自动化功能。AI 智能体可以使用它执行访问网页、填写表单和选择按钮等操作。

还有许多其他 MCP 服务器可以访问不同工具和资源。GitHub 提供了一个 [MCP 注册表](https://github.com/mcp)，帮助开发者发现并为生态系统做出贡献。

> [!CAUTION]
> 出于安全考虑，对待 MCP 服务器应与对待项目中的其他依赖项一样。使用前，请仔细检查其源代码、验证发布者，并考虑相关安全影响。只使用信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 检查 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区中可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 你应该会看到已配置好的 `github` 服务器：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

此 `github` 条目随项目模板提供，因此上一节练习中 Copilot 才能读取待办列表中的 issue。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容——VS Code 会通过 HTTP 连接到该服务器，并在 Copilot 首次使用其工具时要求你使用 GitHub 完成身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，正好可以用于测试功能。

1. 在 `.vscode/mcp.json` 中，在 `github` 旁边添加 `playwright` 条目，使文件内容如下：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标记指示 Playwright 在后台运行浏览器，不显示可见窗口。Codespace 中没有桌面环境，因此必须使用此标记。

> [!NOTE]
> Tailspin Toys 项目已在端到端测试中使用 Playwright，因此通常已经安装了 Playwright 所需的浏览器。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器——Copilot 首次需要使用服务器工具时，VS Code 会启动服务器，并要求你确认是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它，立即启动服务器并确认配置正确。
2. 如果 VS Code 要求确认是否信任服务器，请检查配置并选择信任，以便服务器启动。

手动启动是可选的。如果跳过此步骤，VS Code 会在下一步中 Copilot 首次需要使用服务器工具时启动服务器（并提示你信任它）。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可操作的真实浏览器。你无需亲自操作应用来检查工作，智能体可以打开页面、导航、应用筛选条件、读取结果并向你总结观察到的内容。无需离开对话即可快速确认功能是否符合预期。

Playwright MCP 服务器在底层使用页面的[无障碍树][playwright-mcp-server]，而不是截图。这意味着智能体会像辅助技术一样，根据带有标签的结构化元素（按钮、链接、列表项）进行推理，因此快速功能检查也可以作为简单的无障碍检查。

连接服务器并启动应用后，请 Copilot 测试刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐步执行这些操作，然后报告结果。根据 issue 中的验收标准检查它的总结。如果发现问题，可以提出后续问题，或让它在创建 pull request 前修复代码。

> [!NOTE]
> 此测试要求应用在 `http://localhost:4321` 上运行。如果已停止开发服务器，请在发送提示前重新启动。如果 Copilot 首次使用 Playwright MCP 服务器时需要下载浏览器，且报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建 pull request

确认功能正常后，就可以打开 PR，让团队审查。首先发布 `filtering-vscode` 分支。

1. 转到 Codespace 中的 **Source Control** 面板，检查 Copilot 所做的更改。
2. 选择 **+** 图标，暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![Source Control 面板中 Copilot 更改的截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建 pull request

可以通过多种方式创建 pull request，包括使用 github.com 和 GitHub 命令行界面（CLI）。既然你已在使用 GitHub Copilot，不妨让它代为创建 PR！它可以找到相关 issue 并创建与之关联的 PR。

1. 转到 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认在智能体下拉菜单中选择了 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 请求 Copilot 为你创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行收集信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会要求你使用 GitHub 完成身份验证；按照提示授权即可。
5. 注意 Copilot 如何搜索 issue、找到相关 issue 并创建 PR。
6. 选择 Copilot 生成的链接检查 pull request，但**请不要**立即合并。

## 总结和后续步骤

恭喜！本练习中，你：

- 了解了模型上下文协议（MCP）是什么，以及 MCP 服务器如何通过新工具扩展 Copilot。
- 将 Playwright MCP 服务器添加到了工作区。
- 使用该服务器在发布筛选功能前，通过浏览器手动测试了功能。
- 发布了分支，并为筛选工作打开了 pull request。

你已使用 MCP 服务器**测试**功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，以便简化无障碍审查等专注任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
