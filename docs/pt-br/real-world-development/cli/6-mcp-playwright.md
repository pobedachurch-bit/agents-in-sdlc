---
title: "Lição 6 - Validar a funcionalidade com o MCP do Playwright"
description: "Inspecione ou configure o MCP do Playwright no Copilot CLI e use-o para explorar o recurso de filtragem em um navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como já destacamos, escrever código é apenas parte do trabalho. Também precisamos trabalhar com dados e serviços externos e disponibilizar automações adicionais para o Copilot. É aí que entram os servidores MCP. Eles permitem que o Copilot vá além dos recursos integrados à CLI, oferecendo mais ferramentas e serviços.

Nesta lição, você vai:

- entender o que é o Model Context Protocol (MCP) e como o Copilot CLI o utiliza.
- adicionar o servidor MCP do Playwright, caso ele ainda não esteja disponível.
- pedir ao agente que controle um navegador e explore o recurso de filtragem.

## Cenário

Testes unitários e de ponta a ponta são importantes, mas validar alterações na interface do usuário exige interagir com ela. Você quer permitir que o Copilot use o site em que está trabalhando como um usuário faria, para automatizar ainda mais as alterações e aumentar sua confiança de que elas funcionam como esperado.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de IA se comuniquem com ferramentas e serviços externos. Com o MCP, os agentes de IA podem se comunicar com ferramentas e serviços externos em tempo real. Assim, podem acessar informações atualizadas e realizar ações em seu nome.

Essas ferramentas e esses recursos são acessados por meio de um servidor MCP, que atua como uma ponte entre o agente de IA e as ferramentas e os serviços externos. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente de IA pode acessar.

Dois servidores MCP populares são:

- **[Servidor MCP do GitHub][github-mcp]**: fornece acesso a APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Servidor MCP do Playwright][playwright-mcp-server]**: oferece recursos de automação de navegador com o Playwright.

Há muitos outros servidores MCP disponíveis. O GitHub mantém um [registro de MCP][mcp-registry] para facilitar a descoberta e as contribuições ao ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do seu projeto. Antes de usar um servidor, analise o código-fonte, verifique quem o publicou e considere as implicações de segurança.

## Adicionar o servidor MCP do Playwright

Se o servidor MCP do Playwright ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site como um usuário faria.

> [!NOTE]
> Se o MCP do Playwright já estiver configurado no Copilot CLI, ignore estas etapas de configuração e continue na próxima seção.

1. Volte ao seu Codespace.
2. Abra a caixa de diálogo para adicionar um servidor MCP digitando o seguinte comando no Copilot CLI:

   ```plaintext
   /mcp add
   ```

3. Digite `playwright` como nome e pressione <kbd>Tab</kbd>.
4. Confirme o tipo de servidor **STDIO** pressionando <kbd>Enter</kbd> e, em seguida, pressione <kbd>Tab</kbd>.
5. Cole o seguinte na caixa de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o novo servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo do MCP.

## Pedir ao Copilot que explore o recurso com o Playwright

Antes, você confirmou manualmente se a funcionalidade estava funcionando como esperado. Agora, vamos usar o servidor MCP do Playwright disponível no Copilot para fazer o mesmo!

1. Use o prompt a seguir para pedir ao Copilot que valide a funcionalidade com o servidor MCP do Playwright:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Não é necessário dizer ao Copilot que use o servidor MCP, pois normalmente ele identifica essa necessidade. Mas, como você sabe qual ferramenta deve ser usada, vale a pena indicar a direção. Isso ajuda a obter resultados mais consistentes e economiza alguns tokens!

2. Acompanhe o Copilot enquanto ele lista as etapas executadas no navegador para confirmar se a funcionalidade está correta.
3. Leia o relatório e confirme se tudo funciona como esperado.

O Copilot inicia o servidor, usa o Playwright para interagir com o site, para o servidor e apresenta um relatório.

## Resumo e próximos passos

Parabéns! Você usou o servidor MCP do Playwright para explorar seu recurso em um navegador real a partir do Copilot CLI! Em resumo, você:

- aprendeu o que é o Model Context Protocol (MCP) e como o Copilot CLI o utiliza.
- adicionou o servidor MCP do Playwright, caso ele ainda não estivesse disponível.
- pediu ao agente que controlasse um navegador e explorasse o recurso de filtragem.

Em seguida, você vai [criar um agente personalizado de QA][next-lesson] que reúne a skill e as ferramentas de navegador em uma função especializada.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Playwright da Microsoft][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
