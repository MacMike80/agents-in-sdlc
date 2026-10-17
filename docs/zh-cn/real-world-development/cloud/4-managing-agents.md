---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot 云智能体分别完成了三项任务，以改善用户体验并添加功能。云智能体虽然可以异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制中心，你可以查看所有带有未关闭拉取请求（PR）的智能体任务，了解智能体执行的操作，甚至引导正在运行的会话。

在本课中，你将：

- 浏览智能体页面，监控云智能体任务；
- 引导正在运行的会话，请求添加其他功能。

## 场景

将任务分配给智能体以创建高对比度模式后，团队意识到同时添加浅色模式也很合适。既然已经开始更新网站样式并加入切换功能，就可以顺势纳入该功能。你需要引导智能体，让它在添加高对比度模式的同时，也添加浅色模式。

## 查看 Copilot 云智能体任务

查看分配给 Copilot 云智能体的所有任务当前状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。你应该能看到已分配给 Copilot 的任务，例如：
    - 更新代码文档；
    - 生成用于修改游戏的 API；
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的步骤，例如：
    - 从存储库检出代码；
    - 为 Copilot 创建环境；
    - 设置 MCP 服务器；
    - 执行完成分配任务所需的其他操作。

> [!NOTE]
> 显示的具体步骤会因 Copilot 的工作状态和执行方式而异。

4. 同时注意右侧显示的拉取请求（PR）面板。你可以在此查看 PR 和已更改的文件，以便进一步监控。

## 引导云智能体

现在你已了解活动任务，可以请求 Copilot 在处理高对比度模式时也加入浅色模式切换开关。

1. 选择创建高对比度模式的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它完成环境设置并开始工作。此时会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务屏幕截图，其中突出显示了引导活动会话的对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示，并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会着手处理更新后的任务！收到消息时，它会先完成正在执行的步骤，然后再将新请求纳入工作流程。

这也需要几分钟，正好可以停下来回顾迄今为止学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，它是监控和引导 GitHub Copilot 云智能体任务的中心。通过此控制中心，可以跟踪所有活动和已完成的任务、审查智能体正在执行的工作，也可以重定向正在运行的任务来调整范围或提供进一步指导。

你了解了以下内容：

- 使用控制中心和智能体页面监控云智能体任务；
- 重定向正在运行的会话，请求添加其他功能。

Copilot 正在完成功能开发，接下来可以[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
