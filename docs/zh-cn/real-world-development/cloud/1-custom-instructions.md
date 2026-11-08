---
title: "练习 1 - 自定义说明（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一节：先决条件][previous-lesson] · [下一节：Copilot cloud agent →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务有特定的完成方式，或者 Copilot 应该了解背景信息，就需要确保它能获取这些上下文。[说明文件][instruction-files]可以提供这些指导，让 Copilot 不仅了解你希望它做什么，也了解你希望它如何完成。

在本练习中，将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明和按路径指定的说明文件提供给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加一项新的全局存储库标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作坊不同，本练习不会运行“更新前后”提示。Copilot cloud agent 会在 GitHub issue 上异步工作，因此较难实时并排展示其影响。稍后在本工作坊中检查 cloud agent 创建的 pull request 时，你会看到说明文件的影响。

## 说明文件

### 场景

与其他优秀的开发团队一样，Tailspin Toys 也有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- 用户界面（UI）应采用暗色模式，并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过说明文件，可以确保 Copilot 获取正确的信息，并按照上述实践完成任务。

### 自定义说明

自定义说明可以向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。你可以通过它引导 Copilot，生成更符合需求的建议和代码片段。你可以指定首选的编码约定、库，甚至注释风格。可以为整个存储库创建说明，也可以为特定类型的文件或任务创建说明。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。该文件应包含项目级信息，即与发送给 Copilot 的大多数聊天或命令行界面（CLI）请求相关的上下文，例如所用技术栈、项目概述、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建的说明文件。你可以为 TypeScript 或 Astro 等特定语言提供指南，也可以为创建用户界面（UI）组件或一组新单元测试等任务提供指南。

> [!NOTE]
> 在集成开发环境（IDE）中工作时，说明文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot 命令行界面（CLI）和 Copilot cloud agent 在生成代码时，都会使用存储库级别的说明以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot [还支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理说明文件的最佳实践

全面讨论如何创建说明文件超出了本工作坊的范围。不过，示例项目展示了一种有代表性的做法，主要原则如下：

- 将 `copilot-instructions.md` 中的说明聚焦于项目级指导，例如项目描述、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（单元测试、Astro 组件、数据层）或特定任务提供说明。
- 使用自然语言，保持指导清晰，并提供代码应如何编写或不应如何编写的示例。

创建说明文件没有唯一正确的方法，使用 AI 也是如此。通过实践，你会找到最适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的说明文件。查看此项目中的文件时，你可能会注意到，其中包含适用于多种任务的说明，例如[用户界面更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮你生成说明文件。不同产品界面的操作方式各不相同（例如，VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）。当前产品界面的课程会在相关处说明。
>
> 想寻找模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中包含大量说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义说明文件

花些时间阅读此存储库附带的说明文件：一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，注意其中的项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。还要查看 **Code standards** 中的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到 Astro 文件、Drizzle 数据层、测试等方面的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` frontmatter，它设置了一个相对于存储库根目录的 glob，用于确定说明适用的文件。这里，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都符合条件。
5. 查看此项目中创建单元测试的专用说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。注意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分成更小、可复用的文件，并引用示例供 Copilot 遵循。（这些路径是相对于说明文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但目前尚未要求在代码中添加文档注释。接下来的步骤会添加 TSDoc 文档注释规则和文件注释头。

## 添加新的存储库标准

接下来只需编辑一处：添加一条项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。稍后在本工作坊中分配 issue 时，cloud agent 会采用这项规则。

编辑前，先创建一个工作分支（cloud agent 会从 issue 所指向的分支读取说明，但仍应以规范方式提交自己的更改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加一项存储库编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，该部分应位于第 27 行附近。注意其中介绍了项目编码标准，但尚未规定代码文档要求，这也是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一节所见，可以在存储库级别（`.github/copilot-instructions.md`）创建全局说明，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供说明。存储库级别的文件适合放置项目级标准，例如刚刚添加的文档注释规则。

## 提交、推送并合并说明更新

Cloud agent 会从 issue 所指向的分支读取说明。下一节将 issue 分配给 Copilot 时，Copilot 会从 `main` 分支创建分支，因此说明更改必须先合并到 `main`，cloud agent 才能使用它们。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的 pull request（PR），并合并该 PR。这样，cloud agent 在下一节处理分配给它的 issue 时就会读取这些说明。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main` 分支，可以跳过创建分支的步骤，并直接提交到 `main`。此处创建分支是为了让工作坊模拟真实项目中的处理方式。

## 总结和后续步骤

你了解了 Copilot 如何从项目说明文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了一项新的存储库级标准。在本工作坊的后续练习中，你会看到 cloud agent 创建的 pull request 如何遵循这项标准。

接下来，[将第一个 issue 分配给 Copilot cloud agent][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
