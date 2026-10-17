---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que executasse três tarefas diferentes para melhorar a experiência e adicionar funcionalidades. Embora o agente de nuvem trabalhe de forma assíncrona e autônoma, ainda é importante acompanhar as tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel, você pode ver tarefas de agentes com pull requests (PRs) abertos, explorar as operações realizadas e orientar uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para acompanhar tarefas do agente de nuvem.
- orientar uma sessão em andamento e pedir funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a criação de um modo de alto contraste, a equipe percebeu que também seria útil adicionar um modo claro. Como o agente já estava atualizando os estilos e implementando controles, é natural incluir o novo recurso. Você orientará o agente para garantir que ele adicione os dois modos.

## Revisar as tarefas do agente de nuvem do Copilot

Vamos verificar o estado das tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Ela deve incluir tarefas atribuídas ao Copilot, como:
   - atualizar a documentação da base de código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em execução e revise as operações realizadas pelo Copilot. Elas podem incluir:
   - obter o código do repositório.
   - preparar o ambiente do Copilot.
   - configurar servidores MCP.
   - executar várias etapas para concluir a tarefa.

> [!NOTE]
> As etapas exatas variam de acordo com o estado do trabalho do Copilot e a abordagem usada.

4. Observe também o painel de pull request à direita, que permite acompanhar o PR e os arquivos alterados.

## Orientar o agente de nuvem

Agora que você viu as tarefas ativas, peça ao Copilot que inclua o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada ao modo de alto contraste. O título exato varia conforme o nome usado pelo Copilot e o andamento do trabalho.

    ![Sessão de acessibilidade no painel de agentes](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão até que ela conclua a configuração e comece a trabalhar. Isso pode levar alguns minutos.
3. Na caixa de diálogo **Steer active session while Copilot is working**, envie este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de tela da tarefa do agente de nuvem na página de agentes, com a caixa de diálogo para orientar a sessão ativa em destaque](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe o Copilot confirmar o prompt e incluí-lo no fluxo de trabalho.

## Deixar o Copilot trabalhar

Como antes, o Copilot executará a tarefa atualizada. Ele incluirá o novo pedido depois de concluir a etapa em andamento quando você enviou a mensagem.

O processo levará alguns minutos. Aproveite para refletir sobre o que aprendeu e explorou até aqui.

## Resumo e próximos passos

Este exercício apresentou a página de agentes do Copilot, o ponto central para acompanhar e orientar tarefas do agente de nuvem do GitHub Copilot. Com esse painel, você acompanha tarefas ativas e concluídas, revisa o trabalho e redireciona sessões em andamento.

Você explorou estes conceitos:

- usou o painel de agentes para acompanhar as tarefas do agente de nuvem.
- redirecionou uma sessão em andamento e pediu funcionalidades adicionais.

Enquanto o Copilot conclui o trabalho de acessibilidade, vamos [iterar nos pull requests criados][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
