---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]让你可以创建专用人工智能（AI）助手，针对开发工作流中的特定任务或领域进行定制。在存储库的 `.github/agents` 文件夹中通过 Markdown 文件定义智能体，可以为 Copilot 提供专注的说明、最佳实践、编码模式和特定领域知识，从而引导它更有效地完成特定类型的工作。团队可以将专业知识编写成可复用的智能体，例如遵循《Web 内容无障碍指南》（WCAG）的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含 YAML（YAML Ain't Markup Language）frontmatter，至少需要 `name` 和 `description`，后面是用于定义智能体行为、专业知识和说明的 Markdown 提示。

### 自定义智能体与智能体技能的比较

自定义智能体和[智能体技能][agent-skills-concept]在概念上有些重叠。两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体**是执行者，**技能**是工具。

自定义智能体有各自的上下文窗口，并且可以在工作中协调技能（甚至其他智能体）。在本实验中，无障碍自定义智能体会根据无障碍指南审查并更新网站；在此过程中，它可以调用 pull request 工作流技能或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。与 AI 的其他用法一样，需要在自己的环境和场景中测试并不断迭代，找到合适的方式。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能访问其众筹平台，不受视觉能力或偏好的影响。近期用户反馈指出，由于文字和背景颜色对比度不足，有些用户难以阅读当前的暗色主题。为解决这一无障碍问题，设计团队要求实现用户可以开关的高对比度模式。

无障碍至关重要，因此你希望尽快实现此功能。接下来将使用自定义智能体生成相应功能。

在本练习中，将：

- 检查现有的无障碍自定义智能体。
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 检查无障碍自定义智能体

已为你创建了无障碍自定义智能体。先检查其内容，了解它将如何引导 Copilot。

返回 Codespace，打开终端，并从 `main` 创建一个用于无障碍功能的新分支（这样筛选功能 PR 就能与练习 3 中的工作分开）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 注意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 接着浏览并检查以下部分：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - 超文本标记语言（HTML）、层叠样式表（CSS）和 JavaScript 代码示例。
   - 常见问题和错误列表。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉菜单中显示 `.github/agents` 定义的所有自定义智能体。你可以选择自定义智能体，让聊天会话使用该智能体的说明和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，先返回 Codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始干净的对话。不同练习所用的模式和模型可能不同；相关步骤会在适用处说明。

1. 如果 Chat 视图的智能体下拉菜单中尚未选择 **Agent**，请先选择它。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉菜单（它会显示当前活动的智能体，默认是 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat 会开始工作——它会探索代码库、提出更改建议并将编辑内容应用到项目文件。每项编辑都会以内嵌形式显示在聊天中，并包含文件路径和差异供你检查。
6. 随着编辑内容加入，**Files changed** 指示器会更新，让你查看智能体在工作期间修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 正在实际修改存储库——工作期间会编辑 Astro 组件、CSS 和相关测试等现有文件。

下一节练习中，你将检查并引导这项正在进行的工作。

## 总结和后续步骤

本练习介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，也就是针对特定任务和领域定制的专用 AI 助手。使用自定义智能体，可以将团队专业知识和标准编写成可复用的智能体，从而引导 Copilot 更有效地完成特定类型的工作。

你了解了以下概念：

- 如何定义自定义智能体。
- 如何在 Copilot Chat agent mode 中使用自定义智能体。

接下来，[监控并引导智能体的工作][next-lesson]——在更改发生时检查更改，并在同一会话中添加浅色模式切换开关。

## 资源

- [关于自定义智能体][custom-agents]
- [在集成开发环境（IDE）中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一节：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一节：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
