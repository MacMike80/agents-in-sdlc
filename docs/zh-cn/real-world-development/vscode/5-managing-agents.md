---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在 Copilot 智能体模式下，它会自主探索代码库、提出更改、编辑文件并运行命令。由于这些工作会在本地实时进行，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以直接在聊天中审查每项差异、接受或拒绝单个更改，也可以通过后续提示引导对话，调整或扩展工作，而无需离开编辑器。

在本练习中，你将：

- 实时监控无障碍智能体的工作；
- 审查并接受建议的文件编辑；
- 在会话进行中引导智能体，同时添加浅色模式切换开关。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作尚未完成时，设计团队又提出了一项需求：他们希望为觉得默认主题太暗的用户添加浅色模式切换开关。无需重新开始对话，你可以引导现有会话扩展工作范围。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括它探索的文件、运行的命令和建议的编辑。每项文件编辑都会以内嵌差异的形式显示，并提供 **Keep** / **Undo** 控件。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 智能体工作时，滚动聊天记录查看每次工具调用。你会看到文件读取、文件编辑，以及智能体请求运行的终端命令。
3. 智能体提出文件编辑后，差异会以内嵌形式显示：
    - 选择 **Keep** 接受更改；
    - 选择 **Undo** 撤销更改，让智能体再次尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看智能体在本次会话中修改的全部文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你信任的命令。

5. 等待智能体完成高对比度模式。完成后会显示总结更改的消息。

## 引导智能体添加浅色模式

高对比度模式完成后，在同一对话中继续添加浅色模式切换开关。由于沿用当前聊天会话，智能体会保留之前工作的上下文，包括它修改的组件、采用的样式方案和设置的本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会查看已完成的更改，然后提出浅色模式的补充更改。
3. 像审查高对比度更改时一样，在每项新编辑出现时进行审查。使用 **Keep** 或 **Undo** 处理每个差异。
4. 如果智能体的做法不符合预期（例如尝试重构你不喜欢的切换 UI），可以选择聊天输入框旁边的 **Stop** 按钮停止它，然后发送澄清提示。

> [!TIP]
> 引导正在运行的会话比从头开始更快。可以用后续提示要求调整（“简化 CSS——使用 CSS 变量，而不是重复选择器”）、纠正方向（“不要添加第三个切换开关，将浅色模式集成到现有设置面板中”），或扩展范围（“同时为两个切换开关添加键盘快捷键”）。

## 审查工作集

提交工作前，快速检查智能体修改的全部内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查已更改文件的完整列表。你应该会看到 Astro 组件、样式和相关测试中的更新。
3. 打开几个已更改的文件并查看差异，确认自定义智能体中的无障碍模式已应用，包括 ARIA 属性、键盘导航、语义化 HTML 和本地存储。
4. 确认无误后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat 智能体模式监控并引导正在运行的会话：审查每项文件编辑、接受或拒绝更改，并通过后续提示扩展工作，同时保留上下文。

在本课中，你了解了：

- 实时监控智能体工作；
- 接受或拒绝单个文件编辑；
- 使用后续提示引导正在进行的会话。

在[下一课][next-lesson]中，你将发布无障碍功能并创建拉取请求，审查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
