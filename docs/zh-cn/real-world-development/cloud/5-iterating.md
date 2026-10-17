---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

本实验中，你让 GitHub Copilot 处理了多项任务，以改善用户体验并添加功能。你要求 Copilot 为代码添加文档、实现供设计团队迭代探索的相关游戏功能，以及添加高对比度和浅色模式切换等无障碍功能。现在来查看代码更改，必要时向 Copilot 提供反馈以改进成果，然后再为团队其他成员启用代码审查。

## 审查工作

如前所述，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码并遵循正常的 DevOps 流程。因此，在为团队其他成员启用审查前，先查看 GitHub Copilot 为文档、相关游戏功能和无障碍功能创建的建议。

#### 场景

在 Copilot 云智能体异步且自主地执行任务时，为确保操作安全，我们加入了一些安全限制：

- Copilot 对存储库只有读取权限，对自身代码分支才有写入权限。
- 云智能体在 GitHub Actions 中运行，并在单独的临时环境中工作。
- GitHub Actions 工作流必须经人工批准后才能运行。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先查看 GitHub Copilot 云智能体生成的第一个拉取请求（PR），了解它为代码添加了哪些文档。此处使用 GitHub.com 上的标准 PR 界面。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为默认情况下 Copilot 对外部资源（包括外部 MCP 服务器）的访问受限。你也可以[自定义或禁用 Copilot 云智能体的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 或内容更详细的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]**。此时请等待 Copilot 完成工作。可能需要几分钟，可以趁机休息一下，或回顾迄今为止学到的内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看更新后的代码，其中包含新建的 TSDoc 文档注释和其他文档。具体更改会因结果而异。

    查看更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头。这些要求来自练习 2 开始时查看的自定义说明文件。

6. 确认更新无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果工作流正在等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应该会在拉取请求的检查区域中排队运行。如果一切正常，单一 Astro 应用的项目检查就会通过。可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 协作并非单向操作。你可以像在 PR 中提及其他团队成员一样，在评论或代码行内评论中提及 Copilot。Copilot 会读取评论，并启动新会话处理要求。由于结果具有非确定性，我们无法给出固定提示词；可以考虑要求 Copilot：

- 在每个代码文件顶部添加简短说明文件用途的注释头；
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释；
- 创建一个介绍 Astro 应用结构的 README。

1. 添加评论，要求修改生成的文档，并像提及其他用户一样标记 **@copilot**。可以使用上面的建议，也可以提出其他希望 Copilot 为代码库添加的文档。
2. 选择 **View Session**，观察 Copilot 执行任务。注意它会启动新会话来进行更新。
3. 选择 **Back to pull request** 返回 PR。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中应该会出现新的提交。
5. 选择 **Files changed** 选项卡，审查更改。

你可以继续迭代，直到满意为止。完成后，可将 PR 从草稿转换为可审查状态，再合并到 `main`。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来查看 Copilot 为解决“在游戏详情页显示相关游戏”议题创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 或内容更详细的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 确认更新无误后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流正在等待批准。
7. 如果工作流正在等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应该会在 PR 的检查区域中排队运行。如果一切正常，单一 Astro 应用的项目检查就会通过。可能需要几分钟。
9. **可选：**也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已经实现相关游戏功能！与之前一样，你可以通过迭代让 Copilot 云智能体继续修改，例如调整显示的相关游戏数量，或要求它添加文件注释头和 TSDoc 文档注释（请记住：分配这项任务时，你还没有更新自定义说明）。和之前一样，可以在 **Conversation** 选项卡中添加评论，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义智能体实现的无障碍功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 中在控制中心请求添加的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 或内容更详细的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题中会包含 **[WIP]**。此时请等待 Copilot 完成，可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 查看实现，特别留意：
   - 用于切换模式的 UI 组件；
   - 用于保存用户偏好的本地存储；
   - 高对比度和浅色模式对应的 CSS 或样式更改；
   - 无障碍属性（ARIA 标签、键盘导航等）；
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 确认更新无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果工作流正在等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应该会在 PR 的检查区域中排队运行。如果一切正常，单一 Astro 应用的项目检查就会通过。可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，并在浏览器中测试高对比度和浅色模式切换开关，确认功能正常且重新加载页面后设置仍然有效。

注意，自定义无障碍智能体如何引导 Copilot 遵循无障碍最佳实践实现这些功能。如果发现无障碍问题或有改进建议，可以像之前那样在评论中标记 **@copilot**，请求更新。

## 可选练习 — 继续委派任务

云智能体很适合处理积压工作中的实际任务，让你能同时专注于其他工作。可以尝试在存储库中再创建几个议题并分配给 Copilot，逐渐养成这种工作方式。例如：

- 在游戏详情页创建支持者兴趣表单；
- 在游戏列表页实现分页；
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你已完成云智能体工作流。在这些课程中，你：

- **检查了此存储库中的自定义说明文件**，了解它们对云智能体输出的影响；
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步完成任务；
- **创建并使用自定义智能体**，实现高对比度和浅色模式切换；
- **使用 Copilot 智能体页面作为控制中心**，监控并引导正在运行的无障碍会话；
- **审查并迭代改进云智能体的拉取请求**，通过标记 `@copilot` 请求更改，并批准工作流。

## 回顾和后续步骤

你已完成云智能体工作流。如果希望继续探索，其他工作流可以补充本次练习的内容：

- 🖥️ **[VS Code 工作流](../../vscode/)** — 在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 工作流](../../cli/)** — 在终端中通过计划和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接，交付两项经过审查的更改。

也可以在自己的存储库中尝试以下后续练习：

- 将重构或测试覆盖率议题分配给云智能体；
- 为其他领域创建自定义智能体；
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
