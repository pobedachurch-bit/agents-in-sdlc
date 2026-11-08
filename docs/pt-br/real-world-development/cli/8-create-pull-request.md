---
title: "Lição 8 - Criar e mesclar a PR do recurso"
description: "Revise a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, crie uma PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação dos filtros, as atualizações das instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Agora é hora de revisar tudo em conjunto e abrir uma pull request. Você mesmo mesclou a pull request (PR) de avaliação por estrelas; desta vez, o **Agent Merge** gerenciará o processo.

> [!NOTE]
> Normalmente, dividiríamos o recurso, as atualizações das instruções, a atualização da skill e o agente de QA em algumas PRs separadas. Para simplificar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma única conversa e branch, e todo esse trabalho será incluído nesta PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionar a alteração completa do recurso e as evidências de verificação.
- criar a PR de filtragem.
- habilitar o Agent Merge somente depois da revisão e confirmar se a PR foi mesclada.

## Cenário

Ao longo do fluxo de trabalho de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora, a Tailspin Toys quer automatizar o trabalho restante da PR, mantendo a autorização para mesclar sob o controle de quem desenvolve.

## Conhecer o Agent Merge

O **Agent Merge** automatiza o trabalho restante para concluir uma pull request. Quando você o habilita, o Copilot resolve os impedimentos da PR — corrige verificações de integração contínua (CI) com falha, responde aos comentários da revisão e faz rebase quando necessário — e, em seguida, habilita a mesclagem automática do GitHub quando o repositório permite.

Até agora, você mesmo selecionou **Merge pull request**. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme se ele está pronto antes de habilitar o Agent Merge.

## Usar o Agent Merge para gerenciar a PR

Com todo o código criado, vamos revisá-lo em conjunto, criar a PR e deixar o Agent Merge gerenciar as etapas restantes.

1. Volte ao seu Codespace.
2. Abra a caixa de diálogo do agente digitando `/agent`.
3. Selecione **Default** na lista de opções e pressione <kbd>Enter</kbd>.
4. Crie uma PR com o comando `/pr create`.
5. Ative o Agent Merge com o comando `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua da PR. Quando tudo for aprovado, ele fará a mesclagem.
7. Confirme se você recebeu uma mensagem do Copilot semelhante a "PR #14 was squash-merged successfully."

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de mesclagem, configurações do repositório ou permissões ausentes. Se o processo for bloqueado, leia o motivo informado e, se o repositório permitir, conclua manualmente a mesclagem depois de revisar o trabalho.

## Resumo e próximos passos

Você automatizou várias etapas do processo de desenvolvimento, incluindo gerar, testar e validar código e, agora, gerenciar o processo de pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionou a alteração completa do recurso e as evidências de verificação.
- criou a PR de filtragem.
- habilitou o Agent Merge somente depois da revisão e confirmou se a PR foi mesclada.

Em seguida, você vai [explorar outros comandos de barra úteis do Copilot CLI][next-lesson] para gerenciar contexto, modelos e compartilhamento, além da delegação opcional para o agente de nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
