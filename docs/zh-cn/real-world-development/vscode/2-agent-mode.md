---
title: "练习 2 - 使用 Copilot Agent Mode 添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。作为开发者，你通常需要找到所有相关文件、进行修改、运行测试、调试、找出遗漏的文件、再做一次更新……诸如此类。

Copilot Agent Mode 正是为此而生。

Copilot Agent Mode 可以在集成开发环境（IDE）中更自主地工作。它的工作方式类似开发者：先探索现有项目结构，再执行必要的更新、运行测试等任务，并自动修复发现的问题。现在来了解如何使用 Agent Mode 为网站添加新功能。

> [!NOTE]
> 虽然名称相似，但 agent mode 和 cloud agent 面向两种不同的使用体验。Agent mode 在 IDE 中执行任务，支持快速反馈和交互。Cloud agent 则像团队成员一样异步工作，通过 issue 和 pull request 与你交互。

在本练习中，将学习：

- Copilot Agent Mode 如何探索项目、识别相关文件并协调执行更改。
- GitHub Copilot Agent Mode 如何跨用户界面（UI）和数据层实现新功能。
- 如何在将 Copilot Agent Mode 生成的更改和测试合并到代码库之前进行检查。

## 场景

随着游戏列表不断增加，你希望允许用户按类别和发行商筛选。上一节练习中，你已添加发行商辅助函数。现在将使用 Copilot Agent Mode 完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行任何更改前，先探索 Tailspin Toys 网站，了解其当前功能。

该网站是一个带有开发者主题的棋盘游戏众筹平台。它允许用户列出游戏并显示游戏详情。整个网站是一个 Astro 应用，会在构建时将页面渲染为静态超文本标记语言（HTML）。页面会在 frontmatter 中通过 Drizzle 对象关系映射（ORM）直接查询本地 SQLite 数据库——没有单独的后端应用程序编程接口（API）或客户端用户界面（UI）框架。可复用的数据访问辅助函数位于 `src/lib/` 中，交互功能则通过使用标准文档对象模型（DOM）API 的小型 Astro `<script>` 实现，并限定在特定范围内。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器，让网站更容易运行。按照以下步骤在 GitHub Codespace 中启动：

1. 返回 Codespace。上一节练习结束后，当前分支应为 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端窗口。
3. 创建并切换到用于筛选功能的新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，应该会看到类似以下内容的横幅，其中显示服务器 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要在浏览器窗口中打开 `http://localhost:4321`，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中，从终端选择 localhost 统一资源定位符（URL）链接时，会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是一个指向 Codespace 的专用隧道，网站服务器现在就在此处运行！

### 探索网站

网站运行后，可以探索其功能。网站的主要功能包括：

- **主页**：显示棋盘游戏列表，包括标题、图像和描述。
- **游戏详情页**：选择游戏后，会打开详情页，其中包含更多信息，例如游戏标题、描述、发行商和类别。

## 使用 Copilot 探索待办列表

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，先返回 Codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始干净的对话。不同练习所用的模式和模型可能不同；相关步骤会在适用处说明。

网站的初始实现已可以正常使用，但我们想为它添加新功能。先查看待办列表。根据模板设置项目时，系统已自动创建一组 issue；现在让 GitHub Copilot 列出这些 issue。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。**Agent** 智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并未偏好某个模型。编写本工作坊时，我们使用了 Claude Sonnet 4.5，因此将它列入说明。这样做是希望代码建议相对一致，确保开发体验顺畅。不过，由于大型语言模型（LLM）的输出具有概率性，你可能会发现建议与本工作坊描述的内容不同。这完全正常，也在预期之中。

> [!NOTE]
> 由于 LLM 的输出具有概率性，Copilot 可能会使用不同的模型上下文协议（MCP）命令，但仍应能完成任务。

3. 发送以下提示，让 Copilot 查看待办列表中的 issue：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有 issue 的命令。
5. 查看生成的 issue 列表。

注意，Copilot 还根据它判断对用户最有用的程度，为这些项目排定了优先级。

## 检查说明文件

启动智能体生成代码前，先检查将用于向 Copilot 提供工作上下文的说明文件。你将使用[用户界面（UI）](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)文件，其中包含有关如何为网站添加功能的上下文。

1. 在 Codespace 中转到 `.github/instructions/ui.instructions.md`。
2. 查看整体指导，了解如何添加功能，其中包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向多种文件类型的专用说明文件的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 说明文件可以引用其他说明文件和项目中的文件。路径是相对于说明文件所在位置的。这样可以复用说明，将复杂说明拆分成更小、更易管理的部分，并提供示例和模板。

## 实现筛选功能

要完成筛选功能，至少需要对应用进行以下三项独立更新：

- 在数据层（`src/lib/`）中添加或完善剩余的筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选用户界面（UI）。

此外，将更改合并到代码库前，还需要运行测试并确保测试通过。Copilot Agent Mode 可以为你完成这些任务！现在添加此功能。

1. 可以继续使用当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单中仍选择了 **Agent**。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍为模型选择了 **Claude Sonnet 4.5**。
5. 使用以下提示，请 Copilot 根据待办列表中的相关 issue 实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目，找到与目标功能相关的文件。你应该会看到它查找数据层辅助函数、UI 和测试。然后，它会开始修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多项任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示 **Keep** 或 **Undo** 选项，因此无需在它工作期间选择这些选项。

7. 按照 Copilot 的提示，选择 **Continue** 运行测试。

   ![Copilot Chat 窗格中询问用户是否愿意运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. Copilot 处理过程中可能会暂停，甚至出现测试失败。这没有关系！Copilot 会在生成代码和运行测试之间反复迭代，直至完成任务并且不再发现错误。

   ![Copilot Agent Mode 完整 Chat 会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 请记住，始终需要检查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能！
11. 确认一切正常并检查完代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot Agent Mode 为 Tailspin Toys 网站添加新功能。你了解了：

- Copilot Agent Mode 如何探索项目、识别相关文件并协调执行更改。
- GitHub Copilot Agent Mode 如何跨用户界面（UI）和数据层实现新功能。
- 如何在将 Copilot Agent Mode 生成的更改和测试合并到代码库之前进行检查。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建 pull request。

### 加分探索练习 - 实现分页

随着游戏列表不断增加，需要启用分页。运用本练习中学到的技能，请 Copilot 更新网站以实现分页。编写代码时可考虑：

- 遵循现有最佳实践，包括使用已有的说明文件。
- 考虑分页的实现方式，例如是否允许用户选择每页大小，还是使用固定值。
- 编写提示时，确保为 Copilot 提供所需指导，以便按预期实现功能。
- 你可能需要与 GitHub Copilot 迭代，要求它做出更改并提供上下文。这是与 Copilot 协作时的正常流程！

## 资源

- [Copilot ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的 agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
