---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一节：先决条件][previous-lesson] · [下一节：Agent mode →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务有特定的完成方式，或者 Copilot 应该了解背景信息，就需要确保它能获取这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中一种重要方式是使用[说明文件][instruction-files]，为代码生成提供指导。

在本练习中，将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明和按路径指定的说明文件提供给 Copilot。
- 在当前说明文件的指导下，为筛选功能生成第一个数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加一项新的全局存储库标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 将说明更新和筛选功能切片提交到 `main`，供 Copilot 在下一节练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不完全遵循你设置的标准。Copilot 的输出具有概率性。本练习旨在让你观察更新说明后行为的整体变化趋势，而不是要求输出逐字一致。

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
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建的说明文件。你可以为 TypeScript 或 Astro 等特定语言提供指南，也可以为创建 UI 组件或一组新单元测试等任务提供指南。

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

花些时间阅读此存储库附带的说明文件：一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，注意其中的项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。还要查看 **Code standards** 中的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到 Astro 文件、Drizzle 数据层、测试等方面的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于确定说明适用的文件。这里，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都符合条件。
5. 查看此项目中创建单元测试的专用说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。注意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分成更小、可复用的文件，并引用示例供 Copilot 遵循。（这些路径是相对于说明文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但目前尚未要求在代码中添加文档注释。接下来的步骤会添加 TSDoc 文档注释规则和文件注释头。

## 为更改创建分支

遵循最佳实践，为更改创建一个分支。

1. 返回上一节练习中使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，先返回 Codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始干净的对话。不同练习所用的模式和模型可能不同；相关步骤会在适用处说明。

要了解自定义说明的影响，先在当前说明文件不变的情况下发送提示。稍后更新说明后，再次发送相同提示并比较结果。

1. 关闭之前练习中打开的编辑器选项卡，让 Copilot 只获取你需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 注意，建议的辅助函数是一个经过类型化的函数，它将 `db` 客户端作为第一个参数，并返回经过类型化的发行商数组——这是来自 `.github/instructions/drizzle.instructions.md`（适用于 `src/lib/*.ts`）中的数据层约定。
7. 注意，建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的输出具有概率性，因此它有可能在没有相关说明时也添加文档注释。如果出现这种情况也没关系；本练习的重点仍是观察更新说明后行为一致性的改进。

## 添加新的存储库标准

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

## 重新运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除缓冲区。
2. 再次将焦点放在 `src/lib/publishers.ts` 文件中，让 Copilot 聚焦于正确的文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意，建议的文件现在会以类似以下内容的注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意，建议的函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用于真实代码，下一节练习将基于这些代码继续进行。

## 提交说明并推送分支

说明文件和存储库中的其他资源一样，使用相同的源代码管理方式。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端窗口。
2. 在终端中运行以下命令，确认说明更新和辅助函数更改均已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交说明更新，以及 Copilot 为辅助功能基础部分更改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并 pull request

分支推送完成后，应创建 pull request，并将其关联到待办列表中的文档标准 issue。可以手动完成，也可以让 Copilot 使用已连接到项目的 GitHub 工具代为处理。接下来让 Copilot 查找 issue，创建用于关闭该 issue 的 pull request（PR），然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已经将 GitHub 工具连接到工作区，因此无需设置任何内容。Copilot 首次使用其中某个工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。稍后的练习会介绍这种连接方式（模型上下文协议）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保在智能体选择器中选择了 **Agent**。
3. 请求 Copilot 查找与更新说明文件相关的 issue，并从当前分支创建 PR，说明其中既有说明更新，也有新的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找 issue 并创建 PR。
5. 如果系统提示你 **Allow** Copilot 代表你执行 GitHub 操作，请检查命令并酌情选择 **Allow**。
6. 创建 PR 后，使用以下提示请求 Copilot 合并 PR 并将分支切回 main：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果系统提示你 **Allow** Copilot 代表你执行 GitHub 操作和运行 shell 命令，请检查命令并酌情选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并 pull request！

## 总结和后续步骤

你了解了 Copilot 如何从项目说明文件获取上下文，并在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，并观察 Copilot 如何根据**现有**说明生成代码。
- 在 `.github/copilot-instructions.md` 中添加了一项新的全局存储库标准。
- 再次发送相同提示，观察建议的代码如何采用新标准。
- 将说明更新和辅助函数基础代码提交到 `main`，以便下一节练习基于这些内容继续进行。

接下来，使用 [agent mode][next-lesson] 将这些说明应用到 Copilot 跨代码库添加新功能的工作中。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot — 说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
