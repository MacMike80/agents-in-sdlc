---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应该了解一些背景信息，就需要确保这些上下文可供它使用。可以通过多种方式向 Copilot 提供上下文，其中重要的一种是[说明文件][instruction-files]，它们可以指导代码生成。

在本练习中，你将：

- 了解如何通过存储库自定义说明和按路径限定范围的说明文件，向 Copilot 提供项目专属上下文、编码准则和文档标准；
- 在现有说明的基础上，生成用于过滤功能的第一块数据切片（publishers helper）；
- 向 `.github/copilot-instructions.md` 添加一项新的全库标准；
- 重新运行相同的提示，观察生成的代码如何采用新标准；
- 将说明更新和过滤功能基础提交到 `main`，让 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能与某些标准不完全一致。Copilot 具有非确定性。本练习的重点是观察更新说明后行为变化的*趋势*，而不是逐字符匹配输出。

## 说明文件

### 场景

和优秀的开发团队一样，Tailspin Toys 也有一套开发实践准则和要求，包括：

- 数据层始终需要单元测试。
- UI 应使用深色模式，并具有现代感。
- 应以 TSDoc 文档注释的形式为代码添加文档。
- 每个文件顶部都应添加一段注释，说明该文件的用途。

使用说明文件可以确保 Copilot 获得正确的信息，并按照这些实践要求完成任务。

### 自定义说明

自定义说明可以向 Copilot 提供上下文和偏好，帮助它更好地理解编码风格和要求。你可以指定偏好的编码约定、库，甚至希望添加的注释类型。既可以为整个存储库创建说明，也可以针对特定文件类型创建说明，为具体任务提供上下文。

说明文件分为两类：

- `.github/copilot-instructions.md`：这是单个说明文件，会针对该存储库中的**每个**请求发送给 Copilot。此文件应包含项目级信息，即大多数 Copilot Chat 或 CLI 请求都会用到的上下文，例如技术栈、项目概览、最佳实践及其他全局指导。
- `.github/instructions/*.instructions.md`：可以为特定任务或文件类型创建这类文件。例如，为 TypeScript 或 Astro 等语言提供指导，也可以针对创建 UI 组件或新增一组单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中工作时，说明文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级说明文件和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持采用其他标准的说明文件][custom-instructions-support]，包括 `AGENTS.md` 和 `CLAUDE.md`。

### 管理说明文件的最佳实践

完整讨论如何创建说明文件超出了本研讨会的范围。不过，示例项目中的文件展示了一种常见做法。总体而言：

- 将 `copilot-instructions.md` 中的说明聚焦于项目级指导，例如项目概览、结构和全局编码标准。
- 使用 `*.instructions.md` 文件，为特定文件类型（单元测试、Astro 组件、数据层）或任务提供具体说明。
- 使用自然语言并保持指导清晰，提供代码应该如何编写以及不应该如何编写的示例。

创建说明文件没有唯一正确的方法，就像使用 AI 也没有唯一正确的方法一样。通过实验，你会逐渐找到最适合项目的方式。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的说明文件。浏览本项目中的文件时，你会看到它们涵盖了多种任务，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面对这一功能的呈现方式不同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）；相关工作流会在适当的位置介绍这些功能。
>
> 想找模板或起点？可以查看 [awesome-copilot][awesome-copilot]，这是一个汇集说明文件、自定义智能体和其他资源的存储库。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义说明文件

花点时间阅读此存储库附带的说明文件，其中包括一个核心 `copilot-instructions.md` 文件，以及一组适用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web UI 中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，注意项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下，还要注意嵌套的 **GitHub Actions Workflows** 指导。这些说明适用于与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览。注意其中包含针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它会设置一个相对于存储库根目录的 glob，用来确定说明适用的文件。此处，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都会匹配。
5. 注意其中针对为本项目创建单元测试的专门说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意其中链接到了其他说明文件（如 `unit-tests.instructions.md`）以及项目中的现有文件。这样可以将较大的说明集拆分为更小、可复用的文件，并在生成代码时为 Copilot 提供可参考的示例。（这些路径相对于说明文件本身，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但目前还没有要求在代码中添加文档。接下来的步骤会加入 TSDoc 文档注释和文件注释头规则。

## 为更改创建分支

按照最佳实践，为更改创建分支。

1. 返回上一练习中使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明*之前*先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 codespace 并打开 Copilot Chat 面板，然后选择 **New Chat** 开始新的对话。模式和模型选择因练习而异；各步骤会在需要时说明。

要了解自定义说明的影响，先在现有说明生效的情况下发送提示。稍后会更新说明文件，再次发送相同的提示，观察结果有何不同。

1. 关闭之前练习中打开的编辑器标签页，让 Copilot 只获取当前需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道 helper 应该放在哪里。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，让 Copilot 能应用文件更改。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会涉及 helper 文件及其测试。
6. 注意生成的 helper 是一个带类型的函数，第一个参数接收 `db` 客户端，并返回带类型的 publishers 数组——这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有非确定性，因此即使没有明确要求，也可能添加文档注释。如果出现这种情况也没关系；更新说明后行为的*一致性*仍然是这里要观察的重点。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加存储库编码标准，提升代码建议质量。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。注意此处已记录项目编码标准，但还没有代码文档规则，这就是生成的 helper 缺少文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别创建说明文件（`.github/copilot-instructions.md`）来提供全局指导，也可以创建 `*.instructions.md` 文件来指导特定语言、文件类型或任务。刚添加的文档注释规则属于项目级标准，适合放在存储库级文件中。

## 重新运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清空上下文。
2. 再次选择 `src/lib/publishers.ts`，让 Copilot 关注正确的文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意生成的文件现在会以类似下面的注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供从数据库检索发行商信息的函数。
    */
   ```

5. 注意生成的函数现在会包含类似下面的 TSDoc 注释：

   ```typescript
   /**
    * 返回包含所有发行商 ID 和名称的列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 一个 Promise，解析为发行商对象数组。
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用到实际代码中；下一练习会继续扩展这些代码。

## 提交说明并推送分支

说明文件和存储库中的其他资源一样，都通过源代码管理进行跟踪。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认说明更新和 helper 更改已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交说明更新以及 Copilot 为 helper 基础所更改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他支持性更改（例如测试），也请添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求并关联到积压工作中的文档标准议题。也可以手动完成，但这里会让 Copilot 使用已连接到项目的 GitHub 工具来查找议题、创建用于关闭议题的 PR，然后合并 PR。

> [!NOTE]
> 这是第一个让 Copilot 代表你操作 GitHub 的练习。项目模板已为工作区配置 GitHub 工具，因此无需额外设置；Copilot 首次使用这些工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。后续练习会介绍这种连接（模型上下文协议）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并在智能体选择器中确认已选择 **Agent**。
3. 请 Copilot 查找与更新说明文件相关的议题，并从当前分支创建 PR，说明其中既更新了说明，也添加了 publishers helper 基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. 如果 Copilot 提示 **Allow** 其代表你执行 GitHub 操作，请检查命令，并根据需要选择 **Allow**。
6. PR 创建后，使用以下提示让 Copilot 合并 PR 并将本地分支切回 main：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果 Copilot 提示 **Allow** 其代表你执行 GitHub 操作和运行 shell 命令，请检查命令，并根据需要选择 **Allow**。

现在，你已使用 GitHub Copilot 创建并合并拉取请求！

## 总结和后续步骤

你了解了 Copilot 如何从本项目中的说明文件获取上下文，然后使用 VS Code 中的 Copilot Chat：

- 发送代码生成提示，观察 Copilot 如何依据*现有*说明生成内容；
- 向 `.github/copilot-instructions.md` 添加一项新的全库标准；
- 重新运行相同的提示，观察生成的代码如何采用新标准；
- 将说明更新和 helper 基础提交到 `main`，让下一练习继续使用这些代码。

接下来，你会在[智能体模式][next-lesson]中使用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [为 Copilot 编写更好自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot——说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
