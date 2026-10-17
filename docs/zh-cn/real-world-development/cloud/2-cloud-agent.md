---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

许多组织都面临技术债务问题，例如尚未解决的安全问题、需要更新的旧版代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。智能体完成工作后，会创建一个草稿 PR，供开发人员审查。这样既能将繁琐任务委托出去、加快开发进程，也能让开发人员专注于更重要的工作。

你将了解 Copilot 云智能体的以下用法：

- 自定义生成代码的环境；
- 确保操作安全地执行；
- 明确界定议题范围的重要性；
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初承包网站开发的团队没有妥善编写文档，实际上完全缺少文档。首先，他们希望为应用程序中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队已准备好改善游戏发现体验。他们希望在每个游戏详情页上显示相关游戏，即同一类别中的其他游戏。目前不需要完善的实现，只需要一个可用于验收测试用户体验的版本。这项工作目前是阻碍因素，但还有优先级更高的问题。

这类任务很容易被延后，非常适合分配给 Copilot 云智能体。云智能体可以在后台异步处理这些任务，让开发人员专注于其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 介绍 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)能够像开发人员一样在后台执行任务。和与开发人员协作一样，你可以通过多种方式分配工作，包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求来跟踪进度、设置环境并开始处理任务。会话进行中或完成后，你都可以查看详情。方案准备好供你审查时，Copilot 会在拉取请求中提及你。

## 明确界定任务范围的重要性

GitHub Copilot 并没有什么魔法——使用它并不意味着可以跳过思考。即使看似简单的操作，深入分析后也可能很复杂，因此[需要谨慎界定分配给 Copilot 云智能体的任务范围][cloud-agent-best-practices]。把它当作 AI 结对程序员：分阶段工作、学习、试验并根据情况调整。生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明及其如何指导 Copilot。如果你已完成自定义说明练习，就会看到 `.github/copilot-instructions.md` 的作用；如果还没有，现在可以快速了解一下。在将工作分配给 Copilot 云智能体之前，先只读查看此存储库中已有的说明文件，以便之后识别它们的影响。

在 GitHub Web UI 中打开以下文件；如果已有正在运行的 codespace，也可以在其中打开：

- `.github/copilot-instructions.md` - 查看 **Code standards** 部分，特别是 TypeScript 约定和 TSDoc 文档注释的要求。
- `.github/instructions/unit-tests.instructions.md` - 注意 `applyTo` frontmatter，该字段会将这些说明限定到 `**/*.test.ts` 文件。

在下一节将 `Code lacks documentation` 议题分配给云智能体时，留意生成的拉取请求是否包含 TSDoc 文档注释、TypeScript 约定和文件注释头——这些要求来自说明文件。稍后的课程会在审查 PR 时再次介绍。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境和一些设置脚本，以确保一切就绪。将任务分配给 Copilot 时也一样，它会以类似软件工程师的方式工作。

云智能体在执行任务时使用 [GitHub Actions][github-actions]。你可以通过创建[专用设置工作流][setup-workflow]自定义它的环境，并将其配置在 `.github/workflows/copilot-setup-steps.yml` 文件中，让工作流在云智能体开始工作前运行，为它准备所需的开发工具和依赖项。本实验已预先配置此项，以便顺利完成课程并提供学习机会。它会为 Copilot 安装 Node.js、项目依赖项、端到端测试所需的浏览器，以及供单一 Astro 应用使用的迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可在存储库的“Actions”选项卡中测试这些设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限限制为*你的步骤*所需的最低权限。Copilot 会获得用于自身操作的独立令牌。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果设置步骤没有克隆存储库，完成后 Copilot 会自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现已使用 Astro + Drizzle/libSQL）
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

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有数据可用。
      - name: Set up the database
        run: npm run db:setup
```

这与其他 GitHub 工作流文件类似，但有几个要点：

- 其中包含名为 `copilot-setup-steps` 的单个任务。Copilot 开始处理拉取请求前，GitHub Actions 会先执行此任务。
- 注意 `workflow_dispatch` 触发器，它允许你在存储库的 **Actions** 选项卡中手动运行此工作流。这样可以在 Copilot 运行前检查工作流是否成功，而不必等待。

## 添加文档

每个人都知道文档的重要性，但大多数项目的信息不是过时，就是完全缺失。这类技术债务往往一直无人处理，会降低工作效率，并增加维护代码库或帮助新开发人员熟悉团队的难度。幸运的是，Copilot 很擅长生成文档，因此这是分配给 Copilot 云智能体的理想议题。它会在后台生成所需文档，你会在后续练习中回来审查成果。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设为 `Code lacks documentation`。
6. 将 **Description** 设为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求（PR），其标题类似 `[WIP]: Code lacks documentation`。如果列表中尚未显示新的 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，你应该能看到 Copilot 创建的待办事项列表。

> [!NOTE]
> Copilot 生成待办事项列表可能需要几分钟。Copilot 正在设置环境（运行前面介绍的工作流）、分析项目并确定最佳处理方案。

13. 查看它将完成的任务及列表内容。
14. 向下滚动拉取请求时间线，应该能看到 Copilot 已开始处理议题。
15. 选择 **View session** 按钮。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 查看实时会话，了解 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并制定适当计划，以及编写代码。

这可能需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让你可以专注于其他工作。我们会利用这一点，再分配一个任务给 Copilot 云智能体，同时编写代码为应用添加功能。

## 在游戏详情页添加相关游戏

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作，让你专注于一组任务的同时，它处理另一组任务。即使设计团队要求的相关游戏区域实现起来不需要太久，这段时间仍可用于其他工作。现在就把这项任务分配给 Copilot 云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设为：`Show related games on the game details page`
6. 将 **Description** 设为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后，你应该会在议题的第一条评论中看到 👀，表示 Copilot 已开始处理任务。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在认真处理你的第二项请求！Copilot 云智能体的工作方式类似软件工程师，因此你不必一直监控它，而是可以等工作完成后再审查。现在可以开始创建和使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。使用云智能体，可以将议题分配给 Copilot 并让它异步处理。你可以用 Copilot 处理技术债务、创建新功能，或协助将代码从一个框架迁移到另一个框架。

你了解了以下内容：

- 自定义生成代码的环境；
- 确保操作安全地执行；
- 明确界定议题范围的重要性；
- 将议题分配给 Copilot。

云智能体正在后台处理任务，现在可以转而创建和使用自定义智能体。[Copilot 云智能体也支持 MCP 服务器][cloud-agent-mcp]，并且可以使用自定义说明；前面已介绍过这些内容。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
