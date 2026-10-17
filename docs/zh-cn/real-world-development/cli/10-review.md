---
title: "第 10 课 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义设置和更多学习资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你在 Tailspin Toys 的连续工作流中使用了 GitHub Copilot CLI。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目，并找到已创建的筛选议题；
- 添加星级评分，在转发的浏览器中审查结果，并手动合并了第一个拉取请求（PR）；
- 从筛选议题开始，在 Plan 模式下确定方案，在 Autopilot 模式下构建功能，并在 Interactive 模式下审查；
- 使用自定义说明引导智能体，然后自定义现有的 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查；
- 添加 Playwright 模型上下文协议（MCP）服务器，并在真实浏览器中探索筛选功能；
- 创建并选择质量保证（QA）自定义智能体，以评估需求、覆盖率、技能结果和浏览器证据；
- 审查完整的筛选功能更改，并授权 Agent Merge 合并筛选 PR；
- 探索用于上下文、模型、共享和可选云端委派的斜杠命令。

## 交付的内容

本研讨会包含两个 PR 里程碑，各自从更新后的 `main` 创建分支：

1. **星级评分：**在游戏卡片上显示已有的 `starRating`，并明确标记尚未评分的游戏。
2. **筛选和质量工作流：**实现筛选功能、更新并应用说明、自定义 `quality-checks` 报告、创建 QA 配置，并加入相关测试。

从规划筛选功能到创建 PR，你都使用同一对话和分支。为简化研讨会，将这些工作合并到一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、自己在浏览器中的检查，以及 Copilot 通过 MCP 在浏览器中的探索。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新格式报告结果。QA 将这些结果与需求和测试覆盖率审查结合起来，再提交 PR。

新增的测试应解决真实的覆盖缺口；如果 QA 检查后认为无需添加测试，也可能是正确的。合并前请审查代码和证据，代码更改后也要重新验证受影响的证据。

## 最佳实践

为 Copilot 提供的上下文和工具会影响其工作。在本研讨会中，你更新了说明、自定义了技能、创建了 QA 配置并配置了 MCP 服务器。可以在多个对话中复用这些自定义内容，并根据团队需要调整。说明用于设定标准，技能用于描述可重复的任务，自定义智能体用于定义专用角色，MCP 服务器用于连接外部工具。审查实际更改和工具结果，而不只是智能体总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 在构建前思考方案，使用 **Interactive** 跟进重点更改，使用 **Autopilot** 执行范围明确的任务。日常编辑可选择较快的模型，复杂工作则可选择能力更强的模型。

上下文仍然和基础架构同样重要。清楚说明想构建*什么*、*为什么*构建，以及*如何*构建，会显著影响输出。

## 继续探索

核心工作流已完成。还可以了解一些其他 CLI 功能：

- `/review`：请代码审查智能体分析更改；
- `/rubber-duck`：讨论问题并获取另一个视角；
- `/fleet`：协调并行处理相互独立的子任务；
- `/worktree`：隔离单独任务；
- `/delegate`：将任务发送给 Copilot 云智能体。

## 下一步

提升任何工具使用能力的最佳方式，就是持续练习！可以将其用于生产代码、兴趣项目，或一直想构建却没时间开始的小应用。与团队分享经验，也向团队学习。一如既往，请探索相关文档。

如果要从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。如果要比较其他环境，请查看 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
