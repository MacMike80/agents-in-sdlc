---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，将议题要求、quality-checks 技能和 Playwright MCP 结合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并通过 Playwright MCP 在浏览器中查看筛选体验。现在将这些能力整合到一个具有明确 QA 流程的自定义智能体中。

在本课中，你将：

- 了解自定义智能体如何与说明、技能和 MCP 工具协作；
- 创建并检查可复用的质量保证配置；
- 选择 QA 智能体，并根据筛选议题审查其发现。

## 场景

创建拉取请求（PR）前，Tailspin Toys 希望一致地审查需求、代码质量、自动检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程并生成可复用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专用版本。该配置文件会描述智能体的用途、说明和可用工具。本研讨会中，你将在 `.github/agents/qa.agent.md` 定义 QA 角色，并在 Copilot CLI 中选择它。

你使用的自定义功能各有不同用途：存储库说明记录团队标准，`quality-checks` 技能打包可重复运行的检查，Playwright MCP 提供浏览器工具。QA 配置文件会指导 Copilot 如何组合这些能力来评估需求并报告发现。它不会替代这些功能，也不需要另开对话。

## 创建 QA 配置文件

创建功能 PR 前，请 Copilot 创建可复用的 QA 配置文件。它会定义 QA 要执行的检查以及需要遵循的边界。

1. 返回 Codespace 并确认筛选功能对话处于 Interactive 模式。
2. 请默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体前，先审查它的配置，确认 Copilot 准确记录了 QA 工作流和边界。这样可以避免只想验证功能时，智能体意外或过度地修改功能。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 是必需项；`name` 是可选项，但添加后会有清晰的显示名称。
3. 阅读配置说明，确认 QA 会从需求开始，遵循存储库说明，运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持性证据；修改实现代码前会先询问；不会提交更改或创建拉取请求。
5. 如果生成的配置遗漏了这些职责或边界，请在继续前让默认智能体修订。

## 根据议题运行 QA

Copilot CLI 会在对话开始时加载项目智能体。创建配置文件后，恢复同一个筛选功能对话并选择 QA，以便它使用已有的议题和计划决策。

1. 使用以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚刚创建的智能体可能不会显示在可用智能体列表中，但它已经存在，上述命令会将其激活。

2. 使用以下提示，请 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体会开始工作。
4. 完成后，阅读它提供的报告。

## 总结和后续步骤

你已为工作流添加可复用的专业角色并审查其工作。本课中你：

- 了解了自定义智能体如何与说明、技能和 MCP 工具协作；
- 创建并检查了可复用的质量保证配置；
- 选择了 QA 智能体，并根据筛选议题审查其发现。

实现、技能更新、QA 配置、测试和验证报告都已准备好供审查。接下来，[创建功能 PR 并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
