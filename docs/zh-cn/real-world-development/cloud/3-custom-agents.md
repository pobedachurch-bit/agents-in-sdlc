---
title: "练习 3 - 自定义智能体"
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

使用自定义智能体时，你将探索：

- 如何定义自定义智能体。
- 如何将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能访问其众筹平台，不受视觉能力或偏好的影响。近期用户反馈指出，由于文字和背景颜色对比度不足，有些用户难以阅读当前的暗色主题。为解决这一无障碍问题，设计团队要求实现用户可以开关的高对比度模式。

无障碍至关重要，因此你希望尽快实现此功能。接下来将使用自定义智能体生成相应功能。

## 检查无障碍自定义智能体

已为你创建了无障碍自定义智能体。先检查其内容，了解它将如何引导 Copilot。

返回 Codespace，然后检查无障碍自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 注意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 接着浏览并检查以下部分：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误列表。

## 创建并分配 issue

任务控制中心是管理环境中所有智能体的中心位置。你可以将任务分配给 Copilot cloud agent、监控任务，甚至重定向任务并提供额外指导。现在先将实现高对比度模式的任务分配给 Copilot。

1. 转到存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新 issue 对话框。
4. 选择 **Blank issue**，创建新 issue。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

   ![cloud agent 分配界面截图，突出显示自定义智能体和无障碍智能体](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务！

## 总结和后续步骤

本练习介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，也就是针对特定任务和领域定制的专用 AI 助手。使用自定义智能体，可以将团队专业知识和标准编写成可复用的智能体，从而引导 Copilot 更有效地完成特定类型的工作。

你了解了以下概念：

- 如何定义自定义智能体。
- 如何将任务分配给自定义智能体。

Copilot 正在实现高对比度模式。接下来，[从任务控制中心监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为组织使用自定义智能体做准备][org-custom-agents]
- [为企业使用自定义智能体做准备][enterprise-custom-agents]

---

| [← 上一节：GitHub Copilot cloud agent][previous-lesson] | [下一节：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
