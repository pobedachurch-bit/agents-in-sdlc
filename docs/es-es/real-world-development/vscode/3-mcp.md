---
title: "Ejercicio 3 - Probar la funcionalidad con el servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo agente. Antes de abrir una pull request, debes confirmar que funciona en el navegador. En vez de probar la aplicación tú mismo, conectarás el **servidor Playwright MCP, basado en Model Context Protocol (MCP)**, y dejarás que Copilot controle un navegador real para probar la funcionalidad. Después, publicarás la rama y abrirás la PR.

En este ejercicio, aprenderás a:

- entender qué es Model Context Protocol (MCP) y cómo amplían los servidores MCP las herramientas de Copilot.
- añadir el servidor Playwright MCP al espacio de trabajo.
- pedir a Copilot que lo use para probar manualmente la funcionalidad de filtrado en un navegador.
- publicar la rama y abrir una pull request para el trabajo de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo agente es mucho más potente cuando puede acceder a elementos fuera del editor. Copilot lo consigue mediante Model Context Protocol (MCP), un estándar que permite al agente comunicarse con herramientas y servicios externos.

![Diagrama del funcionamiento interno del modo agente y su interacción con el contexto, los modelos de lenguaje de gran tamaño (LLM) y las herramientas, incluidas las que proporcionan los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de inteligencia artificial (IA) se comuniquen con herramientas y servicios externos. Con MCP, los agentes de IA pueden comunicarse en tiempo real con herramientas y servicios externos. Así pueden acceder a información actualizada (mediante recursos) y realizar acciones en tu nombre (mediante herramientas).

Se accede a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente de IA y las herramientas y servicios externos. El servidor MCP se encarga de gestionar la comunicación entre el agente de IA y las herramientas externas, como las API existentes o las herramientas locales, por ejemplo, los paquetes NPM. Cada servidor MCP representa un conjunto distinto de herramientas y recursos al que puede acceder el agente.

Algunos servidores MCP conocidos son:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona acceso a un conjunto de API para gestionar repositorios de GitHub. Permite al agente de IA realizar acciones como crear repositorios, actualizar los existentes y gestionar incidencias y pull requests.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: ofrece funciones de automatización del navegador con Playwright. Permite al agente de IA realizar acciones como navegar por páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP que proporcionan acceso a distintas herramientas y recursos. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar el descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Por motivos de seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa cuidadosamente su código fuente, verifica quién lo publica y ten en cuenta las implicaciones de seguridad. Usa solo servidores MCP de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisar la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu codespace.
2. Deberías ver que ya está configurado un servidor `github`:

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

La entrada `github` viene con la plantilla del proyecto. Por eso Copilot pudo leer el backlog de incidencias en el ejercicio anterior. Usa el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no hace falta instalar nada localmente: VS Code se conecta mediante HTTP y te autenticas con GitHub la primera vez que Copilot usa una de sus herramientas.

## Añadir el servidor Playwright MCP

Ahora añadirás un segundo servidor. El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar, justo lo que necesitas para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

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

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**: VS Code lo inicia en tu equipo ejecutando `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin mostrar una ventana, algo necesario en un codespace, donde no hay un escritorio que pueda mostrarla.

> [!NOTE]
> El proyecto Tailspin Toys ya usa Playwright para las pruebas de extremo a extremo, así que normalmente el navegador que necesita Playwright ya está instalado. Si más adelante Copilot informa de que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Iniciar el servidor y confiar en él

VS Code inicia los servidores MCP **a petición**: la primera vez que Copilot necesite las herramientas de un servidor, VS Code lo iniciará y te pedirá que confirmes que confías en él.

1. En `.vscode/mcp.json`, aparecerá una acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pide que confirmes que confías en el servidor, revisa la configuración y selecciona la opción para confiar en él y permitir que se inicie.

Iniciarlo manualmente es opcional. Si omites este paso, VS Code iniciará el servidor y te pedirá que confirmes que confías en él la primera vez que Copilot necesite sus herramientas en el paso siguiente.

## Probar la funcionalidad de filtrado

El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador real que puede controlar. En vez de probar tú la aplicación, el agente puede abrir una página, navegar por ella, aplicar filtros y leer los resultados para comunicártelos. Después, resumirá lo que ha observado. Es la forma más rápida de confirmar que una funcionalidad se comporta como esperas sin salir de la conversación.

Internamente, el servidor Playwright MCP se basa en el [árbol de accesibilidad][playwright-mcp-server] de la página, no en capturas de pantalla. Eso significa que el agente analiza elementos estructurados y etiquetados (botones, enlaces y elementos de lista), igual que lo haría una tecnología de asistencia. Así, una comprobación funcional rápida también sirve como una verificación básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe la funcionalidad de filtrado que acabas de crear:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot abrirá un navegador mediante el servidor Playwright MCP, seguirá los pasos y te comunicará lo que haya encontrado. Compara su resumen con los criterios de aceptación de la incidencia. Si algo no parece correcto, haz preguntas de seguimiento o pídele que corrija el código antes de abrir una pull request.

> [!NOTE]
> Para esta prueba, la aplicación debe estar en ejecución en `http://localhost:4321`. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt. La primera vez que Copilot use el servidor Playwright MCP, es posible que tenga que descargar un navegador. Si informa de que falta uno, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar la rama y crear una pull request

Ya has confirmado que la funcionalidad funciona. Ahora puedes abrir una pull request (PR) para que el equipo la revise. El primer paso es publicar la rama `filtering-vscode`.

1. Ve al panel **Source Control** del codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Captura de pantalla del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para subir la rama al repositorio.

## Crear la pull request

Hay varias formas de crear una pull request, por ejemplo, desde github.com o con la interfaz de línea de comandos (CLI) de GitHub. Pero, como ya estás trabajando con GitHub Copilot, deja que cree la PR. Puede buscar la incidencia correspondiente y vincularla a la PR.

1. Ve al panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Confirma que **Agent** está seleccionado en el menú desplegable de agentes para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Cuando sea necesario, selecciona **Continue** para permitir que Copilot realice las tareas necesarias para recopilar información y ejecutar operaciones. La primera vez que use una herramienta de GitHub, es posible que se te pida que te autentiques. Sigue las instrucciones para permitirlo.
5. Observa cómo Copilot busca entre las incidencias, encuentra la correcta y crea la PR.
6. Selecciona el enlace generado por Copilot para revisar la pull request, pero **no la fusiones todavía**.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has:

- aprendido qué es Model Context Protocol (MCP) y cómo amplían los servidores MCP las herramientas de Copilot.
- añadido el servidor Playwright MCP al espacio de trabajo.
- usado el servidor para probar manualmente la funcionalidad de filtrado en un navegador antes de publicarla.
- publicado la rama y abierto una pull request para el trabajo de filtrado.

Has usado un servidor MCP para *probar* una funcionalidad, pero MCP es solo una de las formas de ampliar Copilot. A continuación, vamos a [crear un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro de GitHub MCP][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
