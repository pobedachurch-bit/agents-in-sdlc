---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一节：管理智能体][previous-lesson] |
|:--|

## 检查工作成果

在本实验中，你与 GitHub Copilot 一起完成了多项任务，旨在改善用户体验并添加功能。你让 Copilot 为代码添加文档、为设计团队构建可供迭代的相关游戏功能，以及实现包括高对比度和浅色模式切换开关在内的无障碍功能。现在检查代码更改，并在必要时向 Copilot 提供反馈，改进其工作。

### 场景

正如前面多次强调的那样，引入生成式人工智能（AI）并不会改变软件设计和开发运维（DevOps）的基本原则。我们始终需要检查生成的代码，并遵循常规 DevOps 流程。因此，在开启团队其他成员的审查之前，先检查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能提供的建议。

## 安全与 GitHub Copilot cloud agent

由于 Copilot cloud agent 会在无人监督的情况下异步执行任务，因此设有一些安全限制，以确保操作安全。这些限制包括：

- Copilot 对存储库只有读取权限，对它用于编写代码的分支才有写入权限。
- Cloud agent 在 GitHub Actions 中运行，并创建独立的临时环境来工作。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- [默认情况下，外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括模型上下文协议（MCP）服务器。

## 检查生成的文档

先检查 GitHub Copilot cloud agent 创建的第一个 pull request（PR），其中包含为代码添加的文档。你将在 GitHub.com 的标准 PR 界面中完成此任务。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限地访问外部资源，包括外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot cloud agent 的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开 pull request 列表。
3. 打开标题类似于 **Add missing documentation** 或者标题更具体的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，pull request 会带有 **[WIP]** 标记。如果出现这种情况，请等待 Copilot 完成工作。这可能需要几分钟；你可以稍作休息，或回顾迄今为止学到的内容。

4. pull request 准备就绪后，选择 **Files changed** 选项卡并检查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 浏览更新后的代码，其中包含新生成的 TSDoc 文档注释和其他文档。具体更改会有所不同。

   浏览更改时，请查找 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自你在练习 2 开始时检查的自定义说明文件。

6. 检查完更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 应该会看到一些工作流正在等待批准的指示器。
8. 如果有工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 应该会看到工作流在 pull request 的检查部分中排队。如果一切顺利，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作处理 pull request 并非单向过程。你可以像在 pull request 中标记其他团队成员一样，在评论或代码行内评论中标记 Copilot。Copilot 会看到这些评论，并启动另一个会话来处理它们。由于结果具有非确定性，我们无法提供固定的提示文本。可以考虑要求 Copilot 更新以下内容：

- 在每个代码文件顶部添加注释头，简要说明文件的用途。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用的结构。

1. 添加评论，请求更改生成的文档，并像标记其他用户一样标记 **@copilot**。可以使用上面的任一建议，也可以提出其他希望在代码库中看到的文档要求。
2. 选择 **View Session**，观察 Copilot 执行工作。注意 Copilot 会启动新会话来进行更新。
3. 可以选择 **Back to pull request** 返回 pull request。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，pull request 中应该会出现新的提交。
5. 选择 **Files changed** 选项卡，检查更改。

可以继续迭代，直到满意为止。满意后，可以将 PR 从草稿转换为准备审查状态，并将其合并到 main 分支。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 检查相关游戏功能

现在回到 Copilot 为解决“在游戏详情页显示相关游戏” issue 创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Show related games on the game details page** 或者标题更具体的 PR。
4. 选择 **Files changed** 选项卡，检查 Copilot 生成的代码。
5. 检查完更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
6. 应该会看到一些工作流正在等待批准的指示器。
7. 如果有工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 应该会看到工作流在 pull request 的检查部分中排队。如果一切顺利，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。
9. **可选：**也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。转到 Codespace，打开终端，并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已构建相关游戏功能！与之前一样，你可以通过迭代方式使用 Copilot cloud agent 请求更新。例如，可以要求 Copilot 调整显示的相关游戏数量，或确保添加文件注释头和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义说明！）。与之前一样，可以在 **Conversation** 选项卡中添加评论来提出请求，Copilot 会看到评论并启动新会话。

## 检查无障碍功能

最后，检查使用无障碍自定义智能体实现的无障碍功能。此 PR 应同时包含你在练习 3 中分配的高对比度模式，以及你在练习 4 的任务控制中心中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Add high contrast mode to website** 或者标题更具体的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，pull request 会带有 **[WIP]** 标记。如果出现这种情况，请等待 Copilot 完成工作。这可能需要几分钟。

4. 选择 **Files changed** 选项卡，检查 Copilot 生成的代码。
5. 检查实现，尤其要关注：
   - 用于切换模式的用户界面（UI）组件
   - 使用本地存储保存用户偏好
   - 高对比度和浅色模式的层叠样式表（CSS）或样式更改
   - 无障碍富互联网应用（ARIA）属性、键盘导航等
   - 管理模式切换的 JavaScript/TypeScript 代码

6. 检查完更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 应该会看到一些工作流正在等待批准的指示器。
8. 如果有工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 应该会看到工作流在 pull request 的检查部分中排队。如果一切顺利，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。
10. **可选：**可以切换到 Codespace 中的此分支，手动测试无障碍功能。转到 Codespace，打开终端，并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   然后启动应用，在浏览器中测试高对比度和浅色模式切换开关，并确认它们能正常工作且在页面重新加载后仍然生效。

注意，无障碍自定义智能体如何引导 Copilot 遵循无障碍最佳实践实现这些功能。如果发现任何无障碍问题或改进点，可以像处理之前的 PR 一样，在评论中标记 **@copilot** 并请求更新。

## 可选练习 — 继续委托任务

Cloud agent 最适合处理真实的待办事项，让你可以先去做其他工作。为培养这种工作习惯，可以针对存储库创建更多 issue 并分配给 Copilot。以下是一些想法：

- 在游戏详情页创建支持者意向表单。
- 为游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 工作坊。在这些练习中，你：

- **检查了此存储库附带的自定义说明文件，**以便稍后查看它们对 cloud agent 输出的影响。
- **将 issue 分配给 Copilot cloud agent，**并观察它设置环境、制定计划并异步执行任务。
- **创建并使用自定义智能体，**为无障碍功能添加高对比度和浅色模式切换开关。
- **使用 Copilot agents 页面作为任务控制中心，**监控并引导进行中的无障碍会话。
- **检查并迭代改进 cloud agent 的 pull request，**通过标记 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成 Cloud agent 工作坊。想继续探索的话，其他工作坊路径可以补充你在这里练习的内容：

- 🖥️ **[VS Code 工作坊](../../vscode/)** — 直接在集成开发环境（IDE）中探索 Copilot Chat agent mode 和 MCP 集成。
- 💻 **[CLI 工作坊](../../cli/)** — 在终端中交付两项经过审查的更改，使用 Plan 和 Autopilot 模式、智能体技能、自定义质量保证（QA）智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云端委托功能。

在自己的存储库中，还可以尝试以下后续任务：

- 将重构或测试覆盖率 issue 分配给 cloud agent。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一节：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
