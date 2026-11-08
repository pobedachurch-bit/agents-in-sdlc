---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一节：自定义说明][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务问题。这些问题可能是尚未解决的安全问题、需要更新的旧代码，或因时间不足而长期滞留在待办列表中的功能请求。GitHub Copilot cloud agent 可以自主执行更新代码、添加功能等任务。智能体完成工作后，会创建一个草稿 pull request（PR），供开发者检查。这样可以将繁琐任务交给智能体，加快开发流程，让开发者专注于更重要的工作。

你将探索如何使用 Copilot cloud agent：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 了解明确界定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有留下完善的文档——事实上，你会发现项目完全缺少文档。首先，他们希望为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队已经准备好改进游戏发现体验。他们希望每款游戏的详情页都显示相关游戏，也就是同一类别中的其他游戏，让用户可以继续浏览。他们暂时不需要完善的实现，只需要一个可用于用户体验验收测试的版本。目前这项工作受阻，但其他 issue 的优先级更高。

这两项任务都可能很快被降低优先级，因此很适合交给 Copilot cloud agent。Copilot cloud agent 可以异步处理这些任务，让开发者先处理其他工作，再回来检查 Copilot 的成果并确认一切符合预期。

## 了解 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像人类开发者一样在后台执行任务。就像与人类开发者协作一样，你可以通过多种方式完成此操作，包括[将 GitHub issue 分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿 pull request 来跟踪进度、设置环境并开始处理任务。无论 Copilot 仍在执行任务还是已经完成，你都可以查看其会话。解决方案准备好供你检查时，Copilot 会在 pull request 中标记你！

## 明确界定说明范围的重要性

GitHub Copilot 没有魔法——你仍然需要思考。即使看似简单的操作，深入分析后也会发现其复杂性，因此[请谨慎界定分配给 Copilot cloud agent 的任务范围][cloud-agent-best-practices]。把它当作人工智能（AI）结对程序员：分阶段工作、不断学习、尝试和调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明以及它们如何引导 Copilot。如果你已经完成自定义说明的实践练习，就已经看到 `.github/copilot-instructions.md` 的作用；如果还没完成，现在正好快速查看一下。将工作分配给 Copilot cloud agent 前，先只读查看此存储库中已有的说明文件，以便稍后观察它们的影响。

在存储库的 GitHub 网页界面或已启动的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：检查 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它会将这些说明限定在 `**/*.test.ts` 文件中。

下一部分会将 `Code lacks documentation` issue 分配给 cloud agent。查看生成的 pull request 时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头——它们来自这些说明文件。后续练习检查 pull request（PR）时还会再次提到这些内容。

## 为 Copilot cloud agent 设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，以确保环境状态良好。将任务分配给 Copilot 时也是如此，因为它执行任务的方式与软件工程师（SWE）类似。

Cloud agent 在工作时使用 [GitHub Actions][github-actions] 环境。你可以通过创建专用设置工作流来自定义此环境，该工作流配置在 `.github/workflows/copilot-setup-steps.yml` 文件中，并会在 cloud agent 开始工作前运行。这样，它就能使用所需的开发工具和依赖项。为简化实验流程并提供学习机会，项目已预先配置好此工作流。它会确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单个 Astro 应用已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流文件类似，但有几点值得注意：

- 文件中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 pull request 前，GitHub Actions 会先运行此作业。
- 注意 `workflow_dispatch` 触发器。它允许你从存储库的 **Actions** 选项卡手动运行工作流。这有助于验证工作流是否成功运行，而不必等待 Copilot 启动它。

## 添加文档

大家都知道文档的重要性，但大多数项目的信息都已过时或完全缺失。这类技术债务经常得不到处理，降低工作效率，也让维护代码库或帮助新开发者加入团队变得更加困难。幸运的是，Copilot 很擅长创建文档，因此这是分配给 Copilot cloud agent 的理想任务。它会在后台生成所需文档。后续练习中，你会回来检查它的工作。

1. 在新的浏览器选项卡中，转到 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新 issue 对话框。
4. 选择 **Blank issue**，创建新 issue。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 issue 分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

   ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的 pull request（PR），其标题类似于 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻，然后刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会显示在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方案。

13. 查看列表和它计划完成的任务。
14. 向下滚动 pull request 时间线，应该会看到 Copilot 已开始处理 issue 的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码并了解当前状态、暂停思考并确定合适的计划，以及编写代码。

这可能需要几分钟。Copilot cloud agent 的主要目标之一是异步执行任务，让我们可以专注于其他工作。现在就利用这一功能：先将另一项任务分配给 Copilot cloud agent，然后着手编写代码，为应用添加功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot cloud agent 的一大优势是可以分工处理任务，让你在它处理一组任务时专注于其他事情。为设计团队添加相关游戏部分可能不会花很长时间，但你仍可以利用这段时间处理其他工作。现在将这项任务分配给 Copilot cloud agent！

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新 issue 对话框。
4. 选择 **Blank issue**，使用空白模板。
5. 将 **Title** 设置为：`Show related games on the game details page`
6. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 issue 分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后，你应该会在 issue 的第一条评论中看到一组 👀，表示 Copilot 已开始处理！

![Copilot 使用眼睛表情符号表示正在处理 issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正认真处理你的第二项请求！Copilot cloud agent 的工作方式类似软件工程师（SWE），因此你不必一直监控它；等任务完成后再检查即可。接下来，我们来创建并使用自定义智能体。

## 总结和后续步骤

本练习介绍了 [GitHub Copilot cloud agent][copilot-agents]。使用 cloud agent，可以将 issue 分配给 Copilot，让它异步处理任务。你可以用 Copilot 解决技术债务、创建新功能，或协助将代码从一个框架迁移到另一个框架。

你了解了以下概念：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确界定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

Copilot cloud agent 正在后台处理任务，现在可以转而创建和使用自定义智能体。[Copilot cloud agent 也可以使用模型上下文协议（MCP）服务器][cloud-agent-mcp]，并且可以读取自定义说明；我们已在前面的模块中介绍了自定义说明。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一节：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
