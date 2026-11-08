---
title: "Lección 2 - Añadir valoraciones con estrellas: una mejora rápida"
description: "Usa Copilot CLI para hacer un pequeño cambio en las tarjetas de juegos, revísalo en un navegador con un puerto reenviado y fusiónalo como tu primera pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ahora que has instalado Copilot CLI y has probado una conversación, es el momento de hacer tu primer cambio en el proyecto. Será un cambio pequeño: los juegos ya incluyen una valoración con estrellas en sus datos, pero las tarjetas de la página de inicio todavía no la muestran. Pedirás al agente que la muestre, revisarás el cambio y lo fusionarás como tu primera pull request.

En esta lección, aprenderás a:

- iniciar una conversación específica de Copilot en una rama de funcionalidad.
- pedir al agente que haga un pequeño cambio en el proyecto.
- revisar el cambio con `/diff`.
- ejecutar la aplicación y confirmar el cambio en un navegador con un puerto reenviado.
- abrir y fusionar tu primera pull request.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración con estrellas, que ya aparece en la página de detalles del juego. Sin embargo, las tarjetas de la página de inicio solo muestran el título, la categoría, el editor y la descripción. Como primera toma de contacto, pedirás al agente que muestre en cada tarjeta la valoración existente: es un cambio pequeño e independiente, perfecto para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI usa el repositorio y la rama de Git que tienes actualmente en el terminal, en lugar de crear un worktree específico. Las conversaciones guardadas te permiten volver a la misma conversación más tarde, mientras que los archivos y la rama siguen siendo el estado habitual de Git en el disco.

En una conversación verás tres elementos: tus prompts y las respuestas del agente, la actividad de las herramientas que usa el agente para explorar y editar archivos, y los cambios que puedes inspeccionar con `/diff`.

## Iniciar una conversación y pedir el cambio

Inicia una conversación nueva para empezar a implementar la funcionalidad.

1. Vuelve a tu Codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si Copilot no está abierto, inicia Copilot con el siguiente comando:

   ```bash
   copilot --yolo
   ```

4. Asegúrate de iniciar una sesión nueva con el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Usa el siguiente prompt para pedir el cambio:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot explora el proyecto, localiza los archivos que se usan para mostrar los detalles de los juegos y crea el código necesario. ¡Ya has añadido una funcionalidad con Copilot CLI!

## Revisar el diff

Todos los cambios generados por IA deben revisarse antes de fusionarlos, aunque sean pequeños. Vamos a revisar los cambios directamente en Copilot CLI.

1. Introduce `/diff` e inspecciona todos los archivos modificados.
2. Confirma que la tarjeta del juego muestra la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones que ejecutó Copilot y pídele que corrija los errores que haya.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diff.

> [!NOTE]
> Como Copilot, al igual que todas las herramientas de inteligencia artificial (IA) generativa, funciona de forma probabilística y no determinista, el código exacto puede variar. Revisa el comportamiento en lugar de esperar una implementación concreta.

## Comprobar los cambios

No basta con leer el código y dar por hecho que funciona. Pidamos a Copilot que inicie el sitio web para poder examinar la interfaz de usuario (UI) actualizada en el navegador al que Codespaces reenvía el puerto.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas de juegos muestran las valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que ha iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abrir y fusionar tu primera pull request

Ya has creado la funcionalidad. Ahora crea una pull request (PR) para fusionar el nuevo código en el proyecto.

1. Pide al agente predeterminado que cree un commit para el cambio:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Introduce `/pr create`. Copilot CLI sube el commit existente al crear la PR y muestra la URL de la PR.
3. Abre la PR manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la fusión.
6. Vuelve a tu Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y siguientes pasos

¡Enhorabuena! Has publicado tu primer cambio con GitHub Copilot CLI. En concreto, has:

- iniciado una conversación específica de Copilot en una rama de funcionalidad.
- indicado al agente que hiciera un pequeño cambio en las tarjetas de juegos.
- revisado el cambio con `/diff`.
- ejecutado la aplicación para confirmar la valoración con estrellas en un navegador con un puerto reenviado.
- abierto y fusionado tu primera pull request.

A continuación, [partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para crear una funcionalidad más amplia.

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
