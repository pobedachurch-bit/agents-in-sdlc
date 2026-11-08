---
title: "Exercício 5: Iterar no trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que adicionasse documentação ao código, criasse um recurso de jogos relacionados para a equipe de design testar e implementasse recursos de acessibilidade, incluindo opções de alternância para os modos de alto contraste e claro. Vamos explorar as alterações no código e, se necessário, enviar comentários ao Copilot para melhorar o trabalho.

### Cenário

Como já destacamos várias vezes, os fundamentos de design de software e DevOps não mudam com a inclusão da IA generativa. Sempre precisamos revisar o código gerado e seguir nosso processo normal de DevOps. Com isso em mente, vamos revisar as sugestões do GitHub Copilot para criar a documentação, o recurso de jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, foram estabelecidas algumas restrições de segurança para manter tudo protegido:

- O Copilot tem acesso de leitura ao repositório e acesso de escrita **somente** à branch que usará para o código.
- O agente de nuvem é executado no GitHub Actions, onde cria um ambiente separado e temporário para trabalhar.
- Todos os workflows do GitHub Actions precisam de aprovação humana antes de serem executados.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo o acesso a servidores MCP.

## Revisar a documentação gerada

Vamos começar explorando a primeira pull request (PR) criada pelo agente de nuvem do GitHub Copilot: a que adiciona documentação ao código. Você fará isso na interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao explorar a PR, talvez você veja um aviso informando que o firewall bloqueou o GitHub Copilot. Isso é **esperado**, pois o acesso do Copilot a recursos externos é limitado por padrão, incluindo chamadas a servidores MCP externos. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem do Copilot][agent-firewall].

1. Volte ao seu repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra a pull request intitulada **Add missing documentation** ou com um título mais descritivo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, a pull request terá a indicação **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos, então você pode fazer uma pausa ou refletir sobre o que aprendeu até agora.

4. Quando a pull request estiver pronta, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui os novos comentários de documentação TSDoc e outros elementos de documentação. As alterações exatas variam.

    Ao analisar as alterações, procure comentários de documentação TSDoc, convenções de TypeScript e cabeçalhos de comentários. Eles foram incluídos pelos arquivos de instruções personalizadas que você revisou no início do Exercício 2.

6. Depois de revisar as atualizações e confirmar que está tudo certo, volte à guia **Conversation** e role para baixo.
7. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Opção Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. Os workflows deverão aparecer na fila da seção de verificações da pull request. Se tudo der certo, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.

## Solicitar alterações ao GitHub Copilot

O trabalho com o Copilot em uma pull request não é uma via de mão única. Você também pode mencionar o Copilot em comentários da PR ou em comentários associados a trechos do código, como faria com qualquer pessoa da equipe. O Copilot verá esses comentários e iniciará outra sessão para atendê-los. Como os resultados não são determinísticos, não podemos fornecer um texto exato para solicitar alterações. Veja algumas ideias:

- Adicionar cabeçalhos de comentários no início de cada arquivo de código com uma breve descrição do conteúdo.
- Adicionar comentários de documentação TSDoc aos arquivos TypeScript e Astro.
- Criar um README com uma descrição da estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração à documentação gerada e mencione **@copilot**, como faria com qualquer pessoa. Use uma das ideias acima ou outra sugestão de documentação que gostaria de ver na base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que o Copilot inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar à pull request.

    ![Opção Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, deverá aparecer um novo commit na pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Você pode continuar iterando até ficar satisfeito. Quando estiver tudo certo, transforme a PR de rascunho em uma PR pronta para revisão e faça o merge na branch principal.

![Opção para marcar a PR como pronta para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar o recurso de jogos relacionados

Vamos voltar à PR criada pelo Copilot para resolver a issue sobre exibir jogos relacionados na página de detalhes de um jogo.

1. Volte ao seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione a PR com um título semelhante a **Show related games on the game details page** ou com um título mais descritivo.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as atualizações e confirmar que está tudo certo, volte à guia **Conversation** e role para baixo.
6. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Opção Approve and run workflows](../../../_images/shared-approve-workflows.png)
8. Os workflows deverão aparecer na fila da seção de verificações da pull request. Se tudo der certo, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** você pode selecionar essa branch no seu Codespace para testar manualmente o recurso de jogos relacionados. Acesse o Codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou o recurso de jogos relacionados! Como antes, você pode trabalhar de forma iterativa com o agente de nuvem do Copilot para solicitar atualizações. Por exemplo, você pode pedir ao Copilot que ajuste a quantidade de jogos relacionados exibidos ou que adicione cabeçalhos de comentários e comentários de documentação TSDoc. Lembre-se: essa tarefa foi atribuída **antes** de você atualizar as instruções personalizadas! Como antes, basta adicionar um novo comentário na guia **Conversation**. O Copilot verá o comentário e iniciará uma nova sessão.

## Revisar os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados com o agente personalizado de acessibilidade. Esta PR deve incluir tanto o modo de alto contraste, que você atribuiu no Exercício 3, quanto o modo claro solicitado no painel de controle do Exercício 4.

1. Volte ao seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione a PR com um título semelhante a **Add high contrast mode to website** ou com um título mais descritivo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, a pull request terá a indicação **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, prestando atenção especial a:
   - componentes da interface para alternar entre os modos.
   - uso do armazenamento local do navegador para persistir as preferências.
   - alterações de CSS ou estilo para os modos de alto contraste e claro.
   - atributos de acessibilidade, como rótulos ARIA e navegação por teclado.
   - código JavaScript/TypeScript que gerencia a alternância entre os modos.
6. Depois de revisar as atualizações e confirmar que está tudo certo, volte à guia **Conversation** e role para baixo.
7. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Opção Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. Os workflows deverão aparecer na fila da seção de verificações da pull request. Se tudo der certo, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** você pode selecionar essa branch no seu Codespace para testar manualmente os recursos de acessibilidade. Acesse o Codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da branch criada pelo Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste as opções de alternância dos modos de alto contraste e claro no navegador. Confirme se elas funcionam como esperado e se as preferências continuam salvas depois de recarregar a página.

Observe como o agente personalizado de acessibilidade ajudou a orientar o Copilot para implementar esses recursos seguindo as práticas recomendadas de acessibilidade. Se notar problemas ou oportunidades de melhoria, você pode mencionar **@copilot** em um comentário e solicitar atualizações, como fez nas PRs anteriores.

## Exercício opcional — continuar delegando

O agente de nuvem funciona melhor quando você pode atribuir tarefas reais do backlog e se dedicar a outras atividades. Para desenvolver esse hábito, crie mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- Criar um formulário para registrar o interesse de apoiadores na página de detalhes do jogo.
- Implementar paginação na página de lista de jogos.
- Adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Ao longo destas lições, você:

- **Inspecionou os arquivos de instruções personalizadas incluídos no repositório** para observar mais tarde o efeito deles nos resultados do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, adicionando opções de alternância para os modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como painel de controle** para monitorar e orientar a sessão de acessibilidade enquanto ela estava em andamento.
- **Revisou e ajustou as pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Se quiser continuar explorando, os outros percursos complementam o que você praticou aqui:

- **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, um agente personalizado de QA, MCP do Playwright, Agent Merge e uma ponte opcional `/delegate` para o agente de nuvem.

No seu próprio repositório, experimente estas ideias:

- Atribua ao agente de nuvem uma issue de refatoração ou de cobertura de testes.
- Crie um agente personalizado para outro domínio.
- Configure `copilot-setup-steps.yml` para outra pilha de tecnologia.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]
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
