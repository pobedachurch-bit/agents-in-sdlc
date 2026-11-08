---
title: "练习 3 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确认方案，通过 Autopilot 根据 issue 实现筛选功能，并在 Interactive 模式下检查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先为项目添加了一个小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 可以配合组织现有的工作流，帮助我们以正确的方式构建正确的功能。接下来的几节将带你完成典型的智能体驱动开发流程：从 issue 开始实现新功能，确保代码有效、功能符合预期，最终成功合并到项目中。

> [!NOTE]
> 继续实现这个功能时，请使用同一对话和分支。通常，不同类型的文件会分别放在不同分支或 PR 中，但这里会简化流程，以便专注于核心概念。

本练习将：

- 根据 GitHub issue 启动新的 Copilot 对话。
- 在 Plan 模式下明确需求。
- 使用 Autopilot 模式实现新功能。
- 检查代码。
- 在转发的浏览器中手动验证功能。

继续实现此功能时，你还将更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体，并打开功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访问者需要按类别和发行商筛选游戏。待办列表中的 issue 描述了这个功能，但像如何组合类别这样的细节需要在编码前达成一致。你将使用 Plan 模式确定这些决策，然后授权 Autopilot 在明确范围内实现功能。

## 背景

在开发流程中引入 AI 编码智能体，并不会改变基本原则。事实上，这些原则变得更加重要！大多数开发者遵循的流程大致如下：

1. 从已提交的 issue 开始，其中描述了需要完成的工作。
2. 制定实现方案。
3. 编写并检查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建 pull request（PR）。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述主题相似。

遵循这种标准流程，可以确保 AI 生成的代码符合既定要求，并像手写代码一样经过审核。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在不同模式间切换：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你的输入后再继续。
- **Plan**：智能体先制定计划，但不能编辑项目文件。
- **Autopilot**：智能体自主工作，包括编写代码、运行测试并不断迭代，直至任务完成。

先使用 Plan 模式，检查计划，然后切换到 Autopilot 实现计划。

## 从 issue 开始

开始筛选功能的工作前，先返回 Codespace，并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请先重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd>，打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有 issue。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的 issue。
9. 按 <kbd>C</kbd> 将 issue 添加到提示中，并返回 **Session** 选项卡。

注意，提示现在以 `#7`（或类似编号）开头。`#` 可将 GitHub 上的 issue 或 pull request（PR）加入上下文。

## 规划筛选功能

规划可以让你在交给 Copilot 实现功能或执行任务前，先确定实施方案。遇到复杂任务时，花些时间规划总是有益的。现在切换到 Plan 模式，请 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的 issue 引用之后，输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会先探索项目，再确定最佳方案并制定计划。

3. Copilot 可能会询问筛选功能应如何工作。根据你的偏好作答，没有错误答案！
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动以检查计划。
6. 如果计划中的任何部分不符合你的决定，请要求 Copilot 修改。

## 批准 Autopilot

计划编写并检查完毕后，就可以开始实现了！使用 Autopilot，让 Copilot 执行计划。

Autopilot 会让 Copilot 持续迭代，直到它认为任务已经完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或选择你安装的版本中名称相近的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何根据已确认的计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后，实现可能会立即开始，因此请先检查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 检查并验证实现

与其他代码一样，生成的代码也需要在合并前检查。接下来检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 将结果与 issue 以及规划时做出的决定进行比较。
4. 检查完代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查的输出，并要求 Copilot 解决任何失败项。

## 体验新功能

代码看起来没问题，但能否运行？像之前一样启动应用，然后通过 Codespaces 转发的端口打开网站。

1. 请求 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及按计划达成一致的组合方式。
4. 确认重置和无结果时的行为符合 issue 和你的决定。
5. 返回 Codespace，并要求 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你已使用不同的对话模式构建并检查功能。本练习中，你：

- 根据 GitHub issue 启动了新的 Copilot 对话。
- 在 Plan 模式下明确了需求。
- 使用 Autopilot 模式实现了新功能。
- 检查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，通过[添加自定义说明][next-lesson]，深入了解代码生成方式，确保代码遵循文档化的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
