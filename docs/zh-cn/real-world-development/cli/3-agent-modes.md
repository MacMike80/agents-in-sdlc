---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定实现方案，使用 Autopilot 根据议题实现筛选功能，并在 Interactive 模式下审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚刚为项目添加了一项小功能。但较大的更改需要更完善的流程。幸运的是，GitHub Copilot CLI 可以配合组织现有的工作流，确保以正确的方式构建正确的功能。本课是多节课程中的第一课，你将遵循典型的智能体驱动开发流程：从议题开始实现新功能，确保代码有效、功能符合预期，最终成功合并到项目中。

> [!NOTE]
> 继续完成功能工作流时，你会使用同一个对话和分支。通常，针对不同文件类型会使用不同分支或 PR，但本课程会采用捷径，帮助你专注于核心概念。

首先，在本课中你将：

- 从 GitHub 议题开始新的 Copilot 对话；
- 在 Plan 模式下确定需求；
- 使用 Autopilot 模式实现新功能；
- 审查代码；
- 在转发的浏览器中手动验证功能。

继续完成此功能时，你还会更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并创建功能 PR。

## 场景

Tailspin Toys 的游戏目录越来越大，访问者需要按类别和发行商筛选游戏。积压工作中的议题描述了此功能，但组合类别筛选等细节需要在编写代码前达成一致。你将使用 Plan 模式确定这些决策，再授权 Autopilot 在限定范围内实现。

## 背景

在开发流程中引入 AI 编码智能体，并不会改变基本原则；这些原则甚至变得更加重要！大多数开发人员的工作流程包括：

1. 从描述需求的已创建议题开始。
2. 制定实现计划。
3. 编写并审查代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码审查和持续集成成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述步骤类似。

遵循这种标准方法，可以确保 AI 生成的代码符合既定要求，并像手写代码一样经过审查。

## 对话模式

**对话模式**决定智能体可以自主执行多少工作。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 在不同模式之间切换：

- **Interactive**：你和智能体协作。智能体提出更改，并在继续前等待你的输入。
- **Plan**：智能体先创建计划，在此模式下无法编辑项目文件。
- **Autopilot**：智能体自主工作，编写代码、运行测试并迭代，直到任务完成。

先使用 Plan 模式，审查计划后再使用 Autopilot 实现。

## 从议题开始

开始筛选功能前，返回 Codespace 并确保存储库和终端已准备就绪。

1. 返回 Codespace。如果已停止，请先重启。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
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

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用箭头键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示词，并返回 **Session** 选项卡。

注意提示词现在以 `#7`（或类似编号）开头。`#` 用于将 GitHub 上的议题或拉取请求（PR）添加到上下文。

## 规划筛选功能

在将实现工作交给 Copilot 前，通过计划确定如何实现功能或执行任务。对于复杂工作，花些时间规划总是有帮助的。切换到计划模式并请 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示词下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始制定计划！它会先探索项目，再确定最佳实现方案。

3. Copilot 可能会询问筛选功能应如何工作。根据你的偏好回答即可，没有错误答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 滚动查看并审查计划。
6. 如果计划中的任何部分与已确定的方案不符，请让 Copilot 修改。

## 批准 Autopilot

计划编写并审查完成后，就可以开始实现了。使用 Autopilot 让 Copilot 开始工作。

Autopilot 会让 Copilot 持续迭代问题，直到它认为任务已完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前版本中名称类似的选项。
2. 确认提示词下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何根据已确定的计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后，Copilot 可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再判断检查是否完成。

## 审查并验证实现

与其他代码一样，生成的代码也必须在合并前审查。现在一起审查代码并运行网站，确认功能正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff` 并检查筛选实现及测试。
3. 对照议题和规划时做出的决定检查结果。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并请 Copilot 修复任何失败。

## 探索新功能

代码看起来不错，但能否运行？像之前一样启动应用，并通过 Codespaces 转发端口访问网站。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 显示端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中确定的组合方式。
4. 确认重置和无结果状态符合议题要求和已作出的决定。
5. 返回 Codespace，请 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并审查功能。本课中你：

- 从 GitHub 议题开始新的 Copilot 对话；
- 在 Plan 模式下确定需求；
- 使用 Autopilot 模式实现新功能；
- 审查代码；
- 在转发的浏览器中手动验证功能。

接下来，[使用自定义说明][next-lesson]了解更多代码生成方式，确保代码遵循已记录的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
