---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是面向开发工作流中特定任务或领域的专用 AI 助手。你可以在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，为 Copilot 提供聚焦的说明、最佳实践、编码模式和领域知识，引导它更有效地完成特定类型的工作。团队可以将专业知识整理为可复用的智能体，例如确保符合 [WCAG][wcag] 的无障碍智能体、遵循安全编码实践的安全智能体，或保持测试模式一致的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件至少包含 `name` 和 `description` 两个 YAML frontmatter 字段，后接用于定义智能体行为、专业知识和说明的 Markdown 提示词。

### 自定义智能体与智能体技能的比较

自定义智能体和[智能体技能][agent-skills-concept]有一些相似之处。两者主要都通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体**是执行任务的工作者，**技能**则是工具。

自定义智能体拥有自己的上下文窗口，旨在通过编排技能（甚至其他智能体）来完成工作。在本实验中，无障碍自定义智能体会依据无障碍指南审查并更新网站；在此过程中，它可以调用拉取请求工作流技能，或用于运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一的正确方法。与 AI 的其他用法一样，应针对自己的环境和场景测试并迭代改进。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论他们的视力状况或偏好如何。近期的用户反馈指出，部分用户觉得当前深色主题难以阅读，因为文本与背景颜色之间的对比度不足。为解决此无障碍问题，设计团队要求实现一种可由用户切换的高对比度模式。

无障碍至关重要，因此需要尽快实现这一功能。你将使用自定义智能体生成相关功能。

在本练习中，你将：

- 审查现有的无障碍自定义智能体；
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 审查无障碍自定义智能体

项目已为无障碍任务创建了一个自定义智能体。先查看其内容，了解它会如何指导 Copilot。

返回 codespace，打开终端并从 `main` 创建新分支，为无障碍相关工作单独分支（练习 3 中的筛选 PR 会单独保留）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 注意其中包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带有 `name` 和 `description` 的 frontmatter。

3. 浏览后续各部分，了解其中列出的内容：
   - 生成无障碍网站代码时的核心职责；
   - 无障碍最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见问题和错误。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉菜单中显示 `.github/agents` 中定义的所有自定义智能体。选择自定义智能体后，聊天会话就会遵循该智能体的说明和工具配置。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 codespace 并打开 Copilot Chat 面板，然后选择 **New Chat** 开始新的对话。模式和模型选择因练习而异；各步骤会在需要时说明。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**（如果尚未选中）。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉菜单（其中显示当前使用的智能体，默认值为 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作：探索代码库、提出更改方案并编辑项目文件。每项编辑都会在聊天中显示文件路径和差异，供你审查。
6. 随着编辑完成，**Files changed** 指示器会更新，显示智能体修改的工作集。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际修改存储库中的文件，包括 Astro 组件、CSS 和相关测试。

下一课会审查并引导这项正在进行的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，这类专用 AI 助手面向特定任务和领域。使用自定义智能体，可以将团队的专业知识和标准整理为可复用的智能体，引导 Copilot 更有效地完成特定类型的工作。

你了解了以下内容：

- 如何定义自定义智能体；
- 如何在 Copilot Chat 智能体模式中使用自定义智能体。

接下来，你将[监控并引导智能体的工作][next-lesson]，审查实时生成的更改，并在同一会话中添加浅色模式切换开关。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
