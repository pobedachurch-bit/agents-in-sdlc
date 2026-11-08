---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que muy pocas organizaciones, si es que hay alguna, no tengan problemas con la deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan mucho tiempo en el backlog porque no ha habido tiempo para implementarlas. El agente en la nube de GitHub Copilot está diseñado para realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera una pull request en borrador para que la revise un desarrollador. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en cuestiones de mayor alcance.

Con el agente en la nube de Copilot, explorarás lo siguiente:

- personalizar el entorno en el que se genera el código.
- asegurarte de que las operaciones se realicen de forma segura.
- comprender la importancia de delimitar claramente las incidencias.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en un estado poco satisfactorio: verás que no hay ninguna. Como primer paso, el equipo quiere añadir comentarios de documentación TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la forma de descubrir juegos. Quieren que la página de detalles de cada juego muestre juegos relacionados, es decir, otros títulos de la misma categoría, para que los usuarios puedan seguir explorando. Todavía no necesitan una implementación pulida; solo algo que les permita hacer pruebas de aceptación de la experiencia de usuario (UX). Esta tarea bloquea el trabajo, pero ahora mismo hay otras incidencias de mayor prioridad.

Son ejemplos de tareas que pueden perder prioridad rápidamente y que conviene asignar al agente en la nube de Copilot. El agente puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas, y este puede volver después para revisar el trabajo de Copilot y confirmar que todo está como se espera.

## Presentación del agente en la nube de GitHub Copilot

[El agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Al igual que cuando trabajas con otro desarrollador, puedes colaborar con el agente de varias maneras, por ejemplo, [asignándole una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando le asignas una incidencia, Copilot crea una pull request en borrador para seguir el progreso, configura un entorno y empieza a trabajar en la tarea. Puedes consultar la sesión de Copilot mientras está en curso o cuando haya terminado. Cuando la solución propuesta esté lista para que la revises, Copilot te mencionará en la pull request.

## La importancia de delimitar bien las instrucciones

GitHub Copilot no hace magia: no puedes saltarte el trabajo de análisis. Incluso las tareas que parecen sencillas pueden tener cierta complejidad cuando se examinan de cerca. Por eso, [delimita con cuidado las tareas que asignas al agente en la nube de Copilot][cloud-agent-best-practices]. Trátalo como a un programador en pareja con inteligencia artificial (IA): trabaja por etapas, aprende, experimenta y adapta el enfoque a medida que avanzas. La incorporación de IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has conocido las instrucciones personalizadas y cómo orientan a Copilot. Si ya has completado un ejercicio práctico sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente en la nube de Copilot, examina en modo de solo lectura los archivos de instrucciones que ya incluye este repositorio para que después puedas identificar su efecto.

Abre los siguientes archivos en la interfaz web de GitHub de tu repositorio o en un codespace si ya tienes uno en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las expectativas sobre las convenciones de TypeScript y los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la sección siguiente, presta atención a los comentarios de documentación TSDoc, las convenciones de TypeScript y los encabezados de comentarios que aparezcan en la pull request resultante. Estos elementos proceden de los archivos de instrucciones. Volveremos a señalarlos al revisar las PR en un ejercicio posterior.

## Configurar el entorno de desarrollo del agente en la nube de Copilot

Para crear código, independientemente de quién lo haga, suele ser necesario un entorno específico y algunos scripts de configuración que garanticen que todo está en buen estado. Lo mismo se aplica al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software (SWE).

El agente en la nube usa [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo creando un [flujo de trabajo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece. Así tendrá acceso a las herramientas y dependencias de desarrollo necesarias. Este flujo se ha configurado de antemano para facilitar el taller y permitirte explorar esta opción. Garantiza que Copilot tenga acceso a Node.js, a las dependencias del proyecto, a los binarios del navegador para las pruebas de extremo a extremo y a la base de datos SQLite migrada y precargada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
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

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro archivo de flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. Este trabajo se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la pull request.
- Fíjate en el desencadenador `workflow_dispatch`, que permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Te resultará útil para comprobar que se ejecuta correctamente, sin esperar a que lo inicie Copilot.

## Añadir documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Este tipo de deuda técnica suele quedar sin resolver, lo que reduce la productividad y dificulta mantener el código o incorporar a nuevos desarrolladores al equipo. Por suerte, Copilot destaca a la hora de crear documentación, así que esta es una tarea ideal para asignar al agente en la nube. El agente trabajará en segundo plano para generar la documentación necesaria. En un ejercicio posterior volverás para revisar el resultado.

1. En una nueva pestaña del navegador, ve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue** para crear una incidencia nueva.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

   ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

   ![Detalles de la asignación a Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la pull request (PR) que se acaba de generar. Su título será parecido a `[WIP]: Code lacks documentation`. Si no aparece ninguna PR nueva en la lista, espera un momento y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas pendientes.

> [!NOTE]
> Es posible que la lista de tareas de Copilot tarde varios minutos en aparecer en la PR. Copilot está creando su entorno (ejecutando el flujo de trabajo que se ha descrito antes), analizando el proyecto y determinando cómo abordar el problema.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate hacia abajo por la cronología de la pull request. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la ventana para ver el indicador actualizado.

16. Observa que puedes desplazarte por la sesión en directo y ver cómo Copilot resuelve el problema. Esto incluye explorar el código y comprender su estado, detenerse a analizar y decidir cuál es el plan adecuado, y crear código.

Este proceso probablemente tardará varios minutos. Uno de los objetivos principales del agente en la nube de Copilot es que pueda realizar tareas de forma asíncrona, de modo que puedas centrarte en otras tareas. Aprovecharemos esa misma función: asignaremos otra tarea al agente en la nube de Copilot y, después, nos pondremos a escribir código para añadir funcionalidades a la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles del juego

Como ya hemos indicado, una de las grandes ventajas del agente en la nube de GitHub Copilot es que permite dividir el trabajo: tú puedes centrarte en unas tareas mientras el agente se ocupa de otras. Aunque añadir una sección de juegos relacionados para el equipo de diseño no necesariamente llevaría mucho tiempo, podrías dedicar ese tiempo a otras tareas. Vamos a asignársela al agente en la nube de Copilot.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

   ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, deberías ver una serie de 👀 en el primer comentario de la incidencia. Esto indica que Copilot está trabajando en ella.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando con diligencia en tu segunda solicitud. El agente en la nube de Copilot trabaja de forma parecida a un ingeniero de software (SWE), así que no hace falta que lo supervises activamente: revisa el resultado cuando haya terminado. Ahora vamos a crear y usar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignar incidencias a este agente para que las resuelva de forma asíncrona. Copilot puede ayudarte a resolver deuda técnica, crear funcionalidades o migrar código de un framework a otro.

Has explorado estos conceptos:

- personalizar el entorno en el que se genera el código.
- asegurarte de que las operaciones se realicen de forma segura.
- comprender la importancia de delimitar claramente las incidencias.
- asignar incidencias a Copilot.

Mientras el agente en la nube de Copilot trabaja en segundo plano, puedes empezar a crear y usar agentes personalizados. [El agente en la nube de Copilot también puede usar servidores MCP][cloud-agent-mcp] y tiene acceso a instrucciones personalizadas, que hemos explorado en módulos anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
