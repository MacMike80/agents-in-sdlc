---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应该了解一些背景信息，就需要确保这些上下文可供它使用。[说明文件][instruction-files]可以提供这些指导，让 Copilot 不仅理解要做*什么*，也理解要*如何*完成。

在本练习中，你将：

- 了解如何通过存储库自定义说明和按路径限定范围的说明文件，向 Copilot 提供项目专属上下文、编码准则和文档标准；
- 向 `.github/copilot-instructions.md` 添加一项新的全库标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作流不同，这里不会运行前后对比提示词。Copilot 云智能体会在 GitHub 议题上异步工作，因此难以实时并排展示说明文件的影响。稍后在本工作流中审查云智能体创建的拉取请求时，就能看到这些说明文件的作用。

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

## 添加新的存储库标准

接下来只需进行一项编辑：添加一条项目级规则，要求在代码中使用 TSDoc 文档注释并添加文件级注释头。之后云智能体处理此工作流中的议题时，就会应用这项规则。

编辑前，先创建一个分支（云智能体会从议题目标分支读取说明，但仍建议单独提交这些更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 提交、推送并合并说明更新

云智能体会从议题目标分支读取说明。下一练习将把议题分配给 Copilot，而它会从 `main` 分支创建分支，因此必须先将说明更改合并到 `main`，云智能体才能使用。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。这样云智能体处理下一练习中的议题时，就能读取这些说明。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。这里保留分支步骤，是为了演示真实项目中的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何从本项目中的说明文件获取上下文，并向 `.github/copilot-instructions.md` 添加了一项存储库级标准。在接下来的课程中审查云智能体生成的拉取请求时，就会看到该标准的应用效果。

接下来，将[首个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [为 Copilot 编写更好自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot——说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
