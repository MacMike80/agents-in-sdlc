---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使只是对应用进行简单更新，通常也要修改多个文件并执行一些操作，例如运行测试。开发人员的工作流程通常包括查找所有相关文件、进行更改、运行测试、调试、找出遗漏的文件、再次更新……类似步骤不胜枚举。

Copilot 智能体模式可以帮助处理这些工作。

智能体模式在 IDE 中自主执行任务。它会像开发人员一样，先探索现有项目结构，再进行必要更新、运行测试等任务，并自动修复发现的问题。现在来了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体虽然名称相似，但面向不同的使用体验。智能体模式在 IDE 中执行任务，方便快速反馈和交互；云智能体则像团队成员一样异步工作，通过议题和拉取请求与你互动。

在本练习中，你将了解：

- Copilot 智能体模式如何探索项目、确定相关文件并协调更改；
- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能；
- 如何在将更改合并到代码库前，审查 Copilot 智能体模式生成的更改和测试。

## 场景

游戏数量逐渐增加，你希望用户可以按类别和发行商筛选游戏。上一练习已经添加了 publishers helper，现在会使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行更改前，先浏览 Tailspin Toys 网站，了解当前功能。

该网站是一个具有开发者主题的棋盘游戏众筹平台。用户可以查看游戏列表及其详情。它是一个 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有独立的后端 API 或客户端 UI 框架。可复用的数据访问 helper 位于 `src/lib/`，交互功能则通过使用标准 DOM API 的 Astro `<script>` 添加。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按照以下步骤，在 GitHub Codespace 中运行网站：

1. 返回 codespace。上一练习完成后，你应该已经切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 创建并切换到用于筛选功能的新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示一个包含网址的横幅，类似如下内容：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果出现提示框，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端里的 localhost 链接时，系统会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 codespace 的私有通道，你的网站现已在其中运行！

### 浏览网站

网站运行后，可以浏览其功能。主要功能包括：

- **主页**：显示棋盘游戏列表、标题、图像和说明。
- **游戏详情页**：选择某个游戏后，会打开包含更多信息的详情页，例如游戏标题、说明、发行商和类别。

## 使用 Copilot 查看积压工作

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 codespace 并打开 Copilot Chat 面板，然后选择 **New Chat** 开始新的对话。模式和模型选择因练习而异；各步骤会在需要时说明。

网站的初始实现已可正常使用，但我们还想添加功能。先查看积压工作。使用模板设置项目时，系统会自动创建一批议题；请 GitHub Copilot 查看这些议题。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。**Agent** 智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并不偏好某一个模型。本研讨会使用了 Claude Sonnet 4.5，因此将它列在说明中，希望让代码建议尽可能一致。但由于大型语言模型具有非确定性，收到的建议可能与说明中列出的结果不同。这完全正常，也在预期之内。

> [!NOTE]
> 由于大型语言模型具有非确定性，Copilot 可能会使用不同的 MCP 命令，但仍应能够完成任务。

3. 向 Copilot 发送以下提示，查看议题积压工作：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 运行列出所有议题的命令。
5. 查看生成的议题列表。

注意 Copilot 甚至会按照它认为对用户最有帮助的程度为议题排序。

## 查看说明文件

让智能体生成代码前，先查看用于向 Copilot 提供工作上下文的说明文件。这里将使用[用户界面（UI）](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)文件，其中介绍了如何为网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中的总体指导，包括：
   - 架构概览；
   - 组件设计、可测试性和无障碍原则；
   - 指向特定文件类型说明的链接，包括：
     - Astro；
     - Tailwind CSS；
     - Drizzle。

> [!TIP]
> 说明文件可以引用其他说明文件和项目中的文件。路径相对于说明文件所在位置。这有助于重复使用说明、拆分复杂指导以便管理，并提供示例和模板。

## 实现筛选功能

要完成筛选功能，至少需要对应用进行三项更新：

- 在数据层（`src/lib/`）中添加或完善其余筛选逻辑；
- 添加或更新筛选行为测试；
- 更新游戏列表页，加入筛选 UI。

此外，合并到代码库前需要运行并通过测试。Copilot 智能体模式可以完成这些任务。

1. 可以继续当前 Copilot 会话，也可以选择 **New Chat** 开始新会话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 说明文件。

   ![显示如何选择 UI 说明文件的屏幕截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单中仍选择了 **Agent**。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认模型仍为 **Claude Sonnet 4.5**。
5. 使用以下提示，请 Copilot 根据积压工作中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何探索项目并找到相关文件。它应该会找到数据层 helper、UI 和测试，然后修改文件并运行测试。

   ![显示 Copilot 正在探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。具体时间取决于任务和代码库的复杂程度，可能需要几分钟。在此过程中，编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会显示 **Keep** 或 **Undo**，因此不必在工作仍在进行时选择这些按钮。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 窗格提示用户确认是否运行测试的屏幕截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至出现测试失败。这没有问题！Copilot 会在生成代码和运行测试之间反复调整，直到完成任务且不再发现错误。

   ![显示完整 Copilot 智能体模式聊天会话的屏幕截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 请记住，始终需要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回运行着网站的浏览器，探索新功能。
11. 确认一切正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加功能。你了解了：

- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能；
- Copilot 智能体模式如何探索项目、确定相关文件并协调更改；
- 如何在将更改合并到代码库前，审查 Copilot 智能体模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]并创建拉取请求。

### 额外探索练习 - 实现分页

游戏列表增加后，就需要启用分页。运用本练习学到的技能，请 Copilot 更新网站并实现分页。编写代码时可以考虑：

- 遵循现有最佳实践，包括使用已有说明文件；
- 考虑分页方式，例如是否允许用户选择每页大小，或将其固定；
- 编写提示时，为 Copilot 提供实现所需的指导；
- 可能需要通过反馈和补充上下文与 GitHub Copilot 迭代。这是使用 Copilot 的正常流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能与适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
