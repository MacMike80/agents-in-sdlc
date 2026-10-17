---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, desenvolvesse uma funcionalidade de jogos relacionados para a equipe de design avaliar e implementasse recursos de acessibilidade, incluindo controles para os modos de alto contraste e claro. Vamos revisar as alterações e, se necessário, pedir que o Copilot melhore o resultado.

### Cenário

Como destacamos várias vezes, os fundamentos de design de software e DevOps não mudam com a IA generativa. Sempre devemos revisar o código gerado e seguir o processo de DevOps habitual. Vamos revisar as sugestões do GitHub Copilot para documentação, jogos relacionados e acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e o agente de nuvem do GitHub Copilot

Como o agente de nuvem executa tarefas de forma assíncrona e sem supervisão, há restrições de segurança para proteger o ambiente:

- O Copilot tem acesso somente de leitura ao repositório e acesso de gravação **apenas** à branch que usará para o código.
- O agente de nuvem é executado no GitHub Actions, em um ambiente separado e temporário.
- Uma pessoa precisa aprovar todos os fluxos do GitHub Actions antes da execução.
- [Por padrão, o acesso a recursos externos é limitado](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), inclusive a servidores MCP.

## Revisar a documentação gerada

Vamos começar pelo primeiro pull request (PR) criado pelo agente de nuvem: o que adiciona documentação ao código. Faça essa revisão pela interface padrão de PR do GitHub.com.

> [!NOTE]
> Talvez o PR mostre um aviso de que o GitHub Copilot foi bloqueado por um firewall. Isso é **esperado**: o acesso do Copilot a recursos externos é limitado por padrão, incluindo chamadas a servidores MCP externos. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o PR intitulado **Add missing documentation** ou com um título semelhante.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]**. Aguarde a conclusão. Isso pode levar alguns minutos.

4. Quando o PR estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui comentários TSDoc e outra documentação. As alterações exatas podem variar.

    Ao revisar, procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário. Eles vêm dos arquivos de instruções personalizadas examinados no início do Exercício 2.

6. Quando terminar, volte à guia **Conversation** e role para baixo.
7. Procure um indicador de que há fluxos aguardando aprovação.
8. Se houver fluxos pendentes, selecione **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Os fluxos deverão aparecer na seção de verificações do pull request. Se tudo estiver correto, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.

## Pedir alterações ao GitHub Copilot

O trabalho com o Copilot em um pull request não é unilateral. Você pode mencioná-lo em comentários do PR ou comentários em linha do código, assim como faria com outra pessoa da equipe. O Copilot verá os comentários e iniciará outra sessão para atendê-los. Como os resultados são não determinísticos, não há um prompt único obrigatório. Você pode pedir, por exemplo:

- adicionar cabeçalhos de comentário ao início de cada arquivo de código com uma breve descrição.
- adicionar comentários TSDoc a arquivos TypeScript e Astro.
- criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário pedindo uma alteração na documentação gerada e mencione **@copilot**. Use uma das ideias anteriores ou peça outra melhoria de documentação.
2. Selecione **View Session** para acompanhar o trabalho. Observe que o Copilot inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar ao PR.

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, um novo commit deverá aparecer no pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Quando estiver pronto, marque o PR como pronto para revisão e integre-o à branch principal.

![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar a funcionalidade de jogos relacionados

Agora, volte ao PR que o Copilot criou para a issue de exibição de jogos relacionados na página de detalhes.

1. Acesse o repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione a guia **Files changed** e revise o código gerado.
5. Quando terminar, volte à guia **Conversation** e role para baixo.
6. Procure um indicador de que há fluxos aguardando aprovação.
7. Se houver fluxos pendentes, selecione **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Os fluxos deverão aparecer na seção de verificações do pull request. Se tudo estiver correto, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** troque para essa branch no Codespace para testar manualmente a funcionalidade. Abra um terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot desenvolveu a funcionalidade de jogos relacionados. Você pode iterar e pedir alterações, por exemplo, ajustar a quantidade de jogos exibidos ou incluir cabeçalhos de comentário e comentários TSDoc. Lembre-se de que essa tarefa foi atribuída **antes** da atualização das instruções personalizadas. Para solicitar alterações, adicione um comentário na guia **Conversation**; o Copilot iniciará outra sessão.

## Revisar os recursos de acessibilidade

Por fim, revise os recursos de acessibilidade implementados com o agente personalizado. Este PR deve incluir tanto o modo de alto contraste atribuído no Exercício 3 quanto o modo claro solicitado no painel de agentes do Exercício 4.

1. Acesse o repositório no GitHub.com.
2. Selecione **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o pull request terá a indicação **[WIP]**. Aguarde a conclusão, que pode levar alguns minutos.

4. Selecione a guia **Files changed** e revise o código gerado.
5. Examine a implementação, especialmente:
   - os controles da interface para alternar entre modos.
   - o uso do armazenamento local para persistir as preferências.
   - as alterações de CSS ou estilo dos modos de alto contraste e claro.
   - os atributos de acessibilidade, como rótulos ARIA e navegação por teclado.
   - o código JavaScript/TypeScript que controla a alternância de modos.

6. Quando terminar, volte à guia **Conversation** e role para baixo.
7. Procure um indicador de que há fluxos aguardando aprovação.
8. Se houver fluxos pendentes, selecione **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Os fluxos deverão aparecer na seção de verificações do pull request. Se tudo estiver correto, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** troque para essa branch no Codespace para testar manualmente os recursos de acessibilidade. Abra o terminal e execute:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os controles dos modos de alto contraste e claro no navegador. Confirme que funcionam e que as preferências continuam ativas depois de recarregar a página.

Observe como o agente personalizado de acessibilidade orientou o Copilot a implementar os recursos seguindo boas práticas. Se encontrar problemas ou oportunidades de melhoria, mencione **@copilot** em um comentário e peça uma atualização, como nos PRs anteriores.

## Exercício opcional — continuar delegando

O agente de nuvem é mais útil quando você pode delegar itens reais do backlog e se concentrar em outras tarefas. Para desenvolver esse hábito, crie mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse na página de detalhes do jogo.
- implementar paginação na lista de jogos.
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Ao longo destas lições, você:

- **inspecionou os arquivos de instruções personalizados do repositório** e observou posteriormente seu efeito no trabalho do agente.
- **atribuiu issues ao agente de nuvem do Copilot** e acompanhou a preparação do ambiente, o planejamento e a execução assíncrona.
- **criou e usou um agente personalizado** para acessibilidade, adicionando controles para os modos de alto contraste e claro.
- **usou a página de agentes do Copilot como painel de controle** para acompanhar e orientar a sessão de acessibilidade.
- **revisou e iterou nos pull requests do agente de nuvem**, mencionando `@copilot` para pedir alterações e aprovando fluxos.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Para continuar explorando, veja os outros percursos:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração MCP diretamente no IDE.
- 💻 **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills, um agente personalizado de QA, MCP do Playwright, Agent Merge e a delegação opcional ao agente de nuvem com `/delegate`.

No seu próprio repositório, experimente estas ideias:

- atribuir ao agente de nuvem uma issue de refatoração ou cobertura de testes.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Boas práticas do fluxo de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
