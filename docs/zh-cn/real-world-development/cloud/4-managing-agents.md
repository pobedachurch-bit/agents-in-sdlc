---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几节练习中，你让 Copilot cloud agent 承担了三项独立任务，用于改善用户体验和添加功能。Cloud agent 虽然可以自主异步工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，你可以查看所有拥有未关闭 pull request（PR）的智能体任务，查看它们执行的操作，甚至可以在会话进行中引导它。

在本练习中，将：

- 探索智能体页面，监控 cloud agent 任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将任务分配给智能体以创建高对比度模式后，团队意识到也可以顺便添加浅色模式。由于已经在更新网站样式并添加切换功能，因此加入此功能似乎很合理。你将引导智能体的工作，确保它同时添加浅色模式和高对比度模式。

## 检查 Copilot cloud agent 任务

现在查看分配给 Copilot cloud agent 的所有任务的当前状态。

1. 转到 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。应该能看到分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 生成用于修改游戏的应用程序编程接口（API）。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，检查 Copilot 已执行的工作。这些操作可能包括：
   - 从存储库检出代码。
   - 创建 Copilot 的工作环境。
   - 设置模型上下文协议（MCP）服务器。
   - 执行完成分配任务所需的各种步骤。

> [!NOTE]
> 列出的具体步骤会因 Copilot 工作的状态和采取的方法而异。

4. 同时留意右侧出现的 pull request（PR）窗格。可以在其中查看 PR 和更改的文件，以便进一步监控。

## 引导 cloud agent

现在你已查看当前活动的任务，可以请求 Copilot 在处理高对比度模式时也添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

   ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到会话显示已完成设置并开始工作。出现类似下方的消息时，就表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![智能体页面中 cloud agent 任务的截图，突出显示“Steer active session while Copilot is working”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 完成工作

与之前一样，Copilot 会继续处理更新后的任务！发送消息时，它正在执行某个步骤；完成该步骤后，它会将新的请求纳入工作流程。

与之前一样，这需要几分钟。现在正好可以暂停一下，回顾迄今为止学到和探索的内容。

## 总结和后续步骤

本练习介绍了 Copilot agents 页面，也就是监控和引导 GitHub Copilot cloud agent 任务的中心位置。你可以通过这个任务控制中心跟踪所有活动和已完成的任务、检查正在执行的工作，甚至可以重定向进行中的任务，调整范围或提供额外指导。

你了解了以下概念：

- 使用任务控制中心和智能体页面监控 cloud agent 任务。
- 重定向正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能。接下来，[迭代改进 Copilot 创建的 pull request][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一节：自定义智能体][previous-lesson] | [下一节：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
