---
title: "Lição 10 - Encerramento e próximos passos"
description: "Recapitule o fluxo de trabalho do Copilot CLI, as duas pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI em um fluxo de trabalho contínuo na Tailspin Toys. Você:

- preparou um Codespace, instalou o Copilot CLI, explorou o projeto e encontrou a issue de filtragem já criada.
- adicionou avaliações por estrelas, revisou o resultado em um navegador e mesclou manualmente sua primeira pull request (PR).
- começou pela issue de filtragem, definiu a abordagem no modo Plan, implementou no modo Autopilot e revisou no modo Interactive.
- orientou o agente com instruções personalizadas, personalizou a skill existente `quality-checks` e a usou para executar testes unitários, lint e verificações de tipos.
- adicionou o servidor Model Context Protocol (MCP) do Playwright e o usou para explorar os filtros em um navegador real.
- criou e selecionou um agente personalizado de garantia de qualidade (QA) para avaliar os requisitos, a cobertura, os resultados da skill e as evidências do navegador.
- revisou a alteração completa dos filtros e autorizou o Agent Merge para a PR de filtragem.
- explorou comandos de barra para contexto, modelos, compartilhamento e delegação opcional para o agente de nuvem.

## O que você entregou

O workshop inclui duas etapas de PR, cada uma em sua própria branch criada a partir de `main` atualizada:

1. **Avaliações por estrelas:** exibir o valor existente de `starRating` e um estado explícito para jogos sem avaliação nos cards.
2. **Filtragem e fluxo de trabalho de qualidade:** implementar os filtros, atualizar as instruções e aplicá-las ao recurso, personalizar o relatório de `quality-checks`, criar um perfil de QA e incluir os testes associados.

Do planejamento da filtragem até a abertura da PR, você usou a mesma conversa e a mesma branch. Reunimos esse trabalho em uma única PR para simplificar o workshop.

## Diferentes tipos de verificação

Você verificou o código de várias maneiras: com testes automatizados, uma inspeção própria no navegador e a exploração do navegador pelo Copilot com o MCP. A skill `quality-checks` executou testes unitários, lint e verificações de tipos e apresentou os resultados no novo formato. O QA reuniu esses resultados com uma revisão dos requisitos e da cobertura dos testes antes da PR.

Os testes adicionados devem preencher lacunas reais; uma execução de QA que não precisa adicionar testes também pode estar correta. Revise o código e as evidências antes de autorizar a mesclagem e atualize as evidências afetadas depois de fazer alterações.

## Práticas recomendadas

O contexto e as ferramentas que você fornece ao Copilot influenciam o trabalho dele. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações em outras conversas e ajuste-as conforme as necessidades da equipe. As instruções definem padrões, as skills descrevem tarefas repetíveis, os agentes personalizados definem funções especializadas e os servidores MCP conectam ferramentas externas. Revise as alterações e os resultados das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** de acordo com a tarefa. Use o **Plan** para definir uma abordagem antes de começar, o **Interactive** para acompanhar de perto alterações específicas e o **Autopilot** para tarefas com escopo bem definido. Escolha um modelo mais rápido para alterações rotineiras e outro mais capaz para tarefas complexas.

O contexto continua sendo tão importante quanto a infraestrutura. Descrever com clareza *o que* você quer desenvolver, *por que* e *como* faz uma diferença significativa no resultado.

## Mais recursos para explorar

Você concluiu o fluxo de trabalho principal. Confira mais alguns recursos da CLI:

- `/review` para pedir ao agente de revisão de código que analise as alterações.
- `/rubber-duck` para conversar sobre um problema e obter outra perspectiva.
- `/fleet` para coordenar subtarefas independentes em paralelo.
- `/worktree` para isolar uma tarefa separada.
- `/delegate` para enviar uma tarefa ao agente de nuvem do Copilot.

## Próximos passos

A melhor maneira de melhorar com qualquer ferramenta é continuar usando-a! Use-a em código de produção, em projetos pessoais ou naquele pequeno aplicativo que você imagina há anos, mas ainda não teve tempo de criar. Compartilhe o que aprendeu com a equipe e aprenda com ela também. E, como sempre, explore a documentação.

Se quiser ampliar a Tailspin Toys pelo terminal, continue com a série opcional [Foundry Backer Concierge][foundry]. Para comparar outros ambientes, explore o [workshop do VS Code][vscode], o [workshop do aplicativo GitHub Copilot][app] ou o [workshop do agente de nuvem do Copilot][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalizar o Copilot CLI][customize-cli]
- [Gerenciar pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
