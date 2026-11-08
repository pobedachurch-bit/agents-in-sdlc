---
title: "Exercício 3 - Testar seu recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar a funcionalidade de filtragem com o modo de agente. Antes de abrir uma pull request, confirme se ela realmente funciona no navegador. Em vez de testar o aplicativo manualmente, você conectará o **servidor MCP do Playwright** e permitirá que o Copilot controle um navegador real para testar a funcionalidade. Em seguida, publicará sua branch e abrirá a PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot.
- adicionar o servidor MCP do Playwright ao workspace.
- pedir ao Copilot que use o servidor para testar manualmente a funcionalidade de filtragem em um navegador.
- publicar sua branch e abrir uma pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente fica muito mais eficiente quando pode acessar recursos além do editor. O Model Context Protocol (MCP) permite que o Copilot faça isso: é um padrão para o agente se comunicar com ferramentas e serviços externos.

![Diagrama da estrutura interna do modo de agente e de sua interação com o contexto, o LLM e as ferramentas — incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem com ferramentas e serviços externos. Com o MCP, os agentes de IA podem se comunicar com ferramentas e serviços externos em tempo real. Assim, podem acessar informações atualizadas usando recursos e realizar ações em seu nome usando ferramentas.

Essas ferramentas e esses recursos são acessados por meio de um servidor MCP, que atua como uma ponte entre o agente de IA e as ferramentas e os serviços externos. O servidor MCP gerencia a comunicação entre o agente de IA e as ferramentas externas, como APIs existentes ou ferramentas locais, por exemplo, pacotes npm. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente de IA pode acessar.

Dois servidores MCP populares são:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: fornece acesso a um conjunto de APIs para gerenciar repositórios do GitHub. O agente de IA pode executar ações como criar repositórios, atualizar os existentes e gerenciar issues e pull requests.
- **[Servidor MCP do Playwright](https://github.com/microsoft/playwright-mcp)**: oferece recursos de automação de navegador com o Playwright. O agente de IA pode executar ações como navegar em páginas da Web, preencher formulários e selecionar botões.

Há muitos outros servidores MCP que oferecem acesso a ferramentas e recursos diferentes. O GitHub mantém um [registro de MCP](https://github.com/mcp) para facilitar a descoberta e as contribuições ao ecossistema.

> [!CAUTION]
> Em relação à segurança, trate os servidores MCP como qualquer outra dependência do seu projeto. Antes de usar um servidor MCP, analise cuidadosamente o código-fonte, verifique quem o publicou e considere as implicações de segurança. Use apenas servidores MCP em que você confia e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no seu codespace.
2. Você verá um servidor `github` já configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

Essa entrada `github` vem com o modelo do projeto, por isso o Copilot conseguiu ler o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], então não é necessário instalar nada localmente. O VS Code se conecta por HTTP e você se autentica com o GitHub na primeira vez que o Copilot usa uma de suas ferramentas.

## Adicionar o servidor MCP do Playwright

Agora você adicionará outro servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador que ele pode controlar, exatamente o que você precisa para testar seu recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github` para que o arquivo fique assim:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Salve o arquivo.

Diferentemente do servidor remoto `github`, `playwright` é um servidor **local**: o VS Code inicia o servidor na sua máquina executando `npx @playwright/mcp@latest`. O sinalizador `--headless` instrui o Playwright a executar o navegador sem uma janela visível. Isso é necessário em um codespace, que não tem um desktop para exibi-la.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright para testes de ponta a ponta, então o navegador necessário normalmente já está instalado. Se o Copilot informar mais tarde que o navegador está ausente, peça que execute `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia servidores MCP **sob demanda**: na primeira vez que o Copilot precisa das ferramentas de um servidor, o VS Code o inicia e pede que você confirme se confia nele.

1. Em `.vscode/mcp.json`, aparece uma ação **Start** acima da entrada `playwright`. Selecione-a para iniciar o servidor agora e confirmar se a configuração está correta.
2. Se o VS Code pedir que você confirme se confia no servidor, revise a configuração e escolha a opção para confiar nele e permitir que seja iniciado.

Iniciar o servidor manualmente é opcional. Se você não fizer isso, o VS Code iniciará o servidor e solicitará que você confie nele na primeira vez que o Copilot precisar das ferramentas, na próxima etapa.

## Testar a funcionalidade de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador real para controlar. Em vez de navegar pelo aplicativo para testar seu trabalho, o agente pode abrir uma página, navegar, aplicar filtros e ler o resultado para você. Em seguida, ele resume o que observou. É a maneira mais rápida de confirmar se um recurso funciona como esperado sem sair da conversa.

Por baixo dos panos, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, não capturas de tela. Isso significa que o agente interpreta elementos estruturados e rotulados — botões, links e itens de lista — da mesma forma que a tecnologia assistiva. Assim, uma verificação funcional rápida também serve como uma verificação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste a funcionalidade de filtragem que você acabou de criar:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot abrirá um navegador pelo servidor MCP do Playwright, seguirá cada etapa e informará o que encontrou. Compare o resumo com os critérios de aceitação da issue. Se algo parecer errado, faça perguntas complementares ou peça ao Copilot que corrija o código antes de abrir uma pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321` para esse teste. Se você tiver parado o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor MCP do Playwright, talvez seja necessário baixar um navegador. Se ele informar que o navegador está ausente, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar a branch e criar uma pull request

Agora que você confirmou que o recurso funciona, está tudo pronto para abrir uma pull request (PR) para que a equipe possa revisá-lo. Primeiro, publique a branch `filtering-vscode`.

1. Abra o painel **Source Control** no codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Painel Source Control com as alterações feitas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Criar a pull request

Há várias maneiras de criar uma pull request, inclusive pelo github.com e pela interface de linha de comando do GitHub (CLI). Mas, como você já está trabalhando com o GitHub Copilot, vamos pedir a ele que crie a PR. Ele pode localizar a issue relevante e criar uma PR associada a ela.

1. Abra o painel Copilot Chat e selecione **New Chat** para iniciar uma sessão nova.
2. Confirme se **Agent** está selecionado na lista suspensa de agentes para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot que crie uma PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Se necessário, selecione **Continue** para permitir que o Copilot reúna informações e execute as operações necessárias. Na primeira vez que ele usar uma ferramenta do GitHub, talvez seja solicitado que você se autentique. Siga as instruções para permitir o acesso.
5. Observe o Copilot pesquisar as issues, encontrar a correta e criar a PR.
6. Selecione o link gerado pelo Copilot para revisar sua pull request, mas **não faça o merge ainda**.

## Resumo e próximos passos

Parabéns! Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot.
- adicionou o servidor MCP do Playwright ao workspace.
- usou o servidor para testar manualmente a funcionalidade de filtragem em um navegador antes de publicá-la.
- publicou sua branch e abriu uma pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* um recurso, mas o MCP é apenas uma das maneiras de ampliar os recursos do Copilot. Em seguida, vamos [criar um agente personalizado][next-lesson] para facilitar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Playwright da Microsoft][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro de MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
