---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Quase todas as organizações enfrentam dívida técnica: problemas de segurança não resolvidos, código legado que precisa de atualizações ou funcionalidades que permanecem no backlog por falta de tempo. O agente de nuvem do GitHub Copilot pode atualizar código e adicionar funcionalidades de forma autônoma. Ao concluir, ele cria um pull request em rascunho para revisão humana. Assim, você pode delegar tarefas repetitivas e se concentrar em questões mais importantes.

Neste exercício, você explorará:

- como personalizar o ambiente de geração de código.
- como garantir que as operações sejam feitas com segurança.
- a importância de issues com escopo claro.
- como atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem dívida técnica a resolver. As pessoas contratadas para criar a primeira versão do site não deixaram documentação adequada. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas no aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. Ela gostaria que a página de detalhes de cada jogo exibisse jogos relacionados — outros títulos da mesma categoria — para que as pessoas continuem navegando. A equipe quer apenas uma implementação inicial para testar a experiência. Embora essa tarefa esteja bloqueada, há outras issues de prioridade mais alta no momento.

Esses são exemplos de tarefas que podem ser adiadas e que são apropriadas para delegar ao agente de nuvem do Copilot. Ele trabalha de forma assíncrona enquanto o desenvolvedor se dedica a outras tarefas e, depois, revisa a solução proposta.

## Apresentação do agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano de forma semelhante a um desenvolvedor. Por exemplo, você pode [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um pull request em rascunho para acompanhar o progresso, prepara um ambiente e começa a trabalhar. Você pode acompanhar a sessão durante a execução ou depois que ela terminar. Quando a solução estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções com escopo bem definido

Não há mágica no GitHub Copilot: você ainda precisa pensar. Até tarefas que parecem simples têm complexidades que só ficam evidentes quando examinadas de perto. Por isso, [defina cuidadosamente o escopo das tarefas para o agente de nuvem][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe em etapas, aprenda, experimente e adapte. Os fundamentos do desenvolvimento de software não mudam com a IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram as instruções personalizadas e como elas orientam o Copilot. Antes de atribuir tarefas ao agente de nuvem, consulte os arquivos de instruções incluídos no repositório. Assim, você poderá observar o efeito deles posteriormente.

Abra os arquivos a seguir na interface web do GitHub ou em um codespace:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, especialmente as convenções de TypeScript e os comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que aplica as instruções aos arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente na próxima seção, observe no pull request os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentário. Essas orientações vêm dos arquivos de instruções. Voltaremos a esse ponto ao revisar os PRs em um exercício posterior.

## Configurar o ambiente de desenvolvimento do agente de nuvem

Criar código geralmente exige um ambiente específico e scripts de configuração. O mesmo vale para o Copilot, que trabalha de maneira semelhante a um engenheiro de software.

O agente de nuvem usa [GitHub Actions][github-actions] como ambiente de execução. Você pode personalizar esse ambiente com um [fluxo de configuração especial][setup-workflow] no arquivo `.github/workflows/copilot-setup-steps.yml`, executado antes do início do trabalho. Isso disponibiliza as ferramentas e dependências necessárias. O fluxo já está configurado para este laboratório: ele instala o Node.js e as dependências do projeto, os navegadores do Playwright e configura o banco de dados SQLite migrado e preenchido com dados para o único aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para as etapas. O Copilot recebe um token próprio para suas operações.
    permissions:
      # Se as etapas de configuração clonarem o repositório (por exemplo, para instalar dependências), será necessária a permissão `contents: read`. Caso contrário, o Copilot clonará o repositório automaticamente quando as etapas terminarem.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do aplicativo: Node.js (o aplicativo agora é todo Astro + Drizzle/libSQL)
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

      # Migra e preenche o banco de dados SQLite local para builds e testes.
      - name: Set up the database
        run: npm run db:setup
```

Esse arquivo se parece com outros fluxos do GitHub, mas tem algumas características importantes:

- contém um único trabalho chamado `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- usa o gatilho `workflow_dispatch`, que permite executar o fluxo manualmente na guia **Actions** do repositório. Isso é útil para testar se a configuração funciona sem esperar o Copilot executá-la.

## Adicionar documentação

Embora todos reconheçam a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação alguma. Essa dívida técnica reduz a produtividade e dificulta a manutenção da base de código e a integração de novos desenvolvedores. O Copilot pode ajudar a criar documentação, portanto esta é uma boa tarefa para delegar ao agente de nuvem.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue**.
4. Selecione **Blank issue**.
5. Defina **Title** como `Code lacks documentation`.
6. Defina **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create**.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o novo pull request, que terá um título semelhante a `[WIP]: Code lacks documentation`. Se ainda não aparecer, aguarde alguns instantes e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer. O Copilot está preparando o ambiente (executando o fluxo descrito anteriormente), analisando o projeto e determinando a melhor abordagem.

13. Revise a lista e as tarefas planejadas.
14. Role a linha do tempo do pull request até encontrar uma atualização indicando que o Copilot começou a trabalhar na issue.
15. Selecione **View session**.

  ![Exibição da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe a sessão em tempo real para ver como o Copilot explora o código, entende o estado do projeto, decide a abordagem e cria código.

Isso pode levar alguns minutos. Uma das principais vantagens do agente de nuvem é executar tarefas de forma assíncrona, deixando você livre para se dedicar a outras atividades. Vamos aproveitar esse recurso atribuindo mais uma tarefa ao agente e, em seguida, trabalhando em outra funcionalidade do aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes

Uma das vantagens do agente de nuvem é dividir o trabalho: enquanto ele executa uma tarefa, você pode se dedicar a outra. Mesmo que adicionar uma seção de jogos relacionados para a equipe de design não demore muito, esse tempo pode ser usado para outras tarefas. Vamos delegá-la ao agente de nuvem.

1. Volte ao repositório no github.com.
2. Selecione **Issues**.
3. Selecione **New issue**.
4. Selecione **Blank issue**.
5. Defina **Title** como `Show related games on the game details page`.
6. Defina **Description** como:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecione **Create**.
8. À direita, selecione **Assign to Copilot**.

  ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Logo depois, você deverá ver um emoji 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot está trabalhando na segunda solicitação. Como ele age de forma semelhante a um engenheiro de software, não é necessário acompanhá-lo continuamente; você poderá revisar o trabalho quando estiver concluído. Vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Este exercício apresentou o [agente de nuvem do GitHub Copilot][copilot-agents]. Você pode atribuir issues ao Copilot para que ele trabalhe de forma assíncrona. Ele pode ajudar a resolver dívida técnica, criar funcionalidades ou migrar código entre frameworks.

Você explorou:

- como personalizar o ambiente de geração de código.
- como garantir que as operações sejam feitas com segurança.
- a importância de issues com escopo claro.
- como atribuir issues ao Copilot.

Enquanto o agente trabalha, vamos criar e usar agentes personalizados. O [agente de nuvem também pode usar servidores MCP][cloud-agent-mcp] e arquivos de instruções personalizadas, apresentados nos módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Boas práticas do fluxo de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
