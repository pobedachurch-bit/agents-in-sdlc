---
title: "Ejercicio 2 - Añadir funcionalidades con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. Como desarrollador, normalmente tienes que localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo te has dejado y volver a modificarlo. Y así sucesivamente.

Para eso sirve el modo agente de Copilot.

El modo agente de Copilot está diseñado para trabajar de forma más autónoma en el entorno de desarrollo integrado (IDE). Se comporta de manera parecida a un desarrollador: primero explora la estructura del proyecto, después realiza los cambios necesarios y ejecuta tareas como las pruebas, y corrige automáticamente los errores que encuentra. Vamos a ver cómo puedes usar el modo agente para añadir funcionalidades al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente realiza las tareas en el IDE, por lo que permite obtener comentarios e interactuar con rapidez. El agente en la nube trabaja de forma asíncrona como un compañero del equipo e interactúa contigo mediante incidencias y pull requests.

En este ejercicio, aprenderás a:

- usar el modo agente de Copilot para explorar el proyecto, identificar los archivos pertinentes y hacer cambios coordinados.
- usar el modo agente de GitHub Copilot para implementar funcionalidades en la interfaz de usuario y la capa de datos.
- revisar los cambios y las pruebas generados por el modo agente de Copilot antes de fusionarlos en el código.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editor. En el ejercicio anterior ya has creado una función auxiliar para obtener editores; ahora completarás con el modo agente de Copilot el resto del trabajo de la capa de datos, la interfaz de usuario y las pruebas.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, exploremos el sitio web de Tailspin Toys para entender sus funciones actuales.

El sitio web es una plataforma de micromecenazgo para juegos de mesa con una temática de desarrollo. Permite mostrar juegos y sus detalles. El sitio es una única aplicación Astro que genera páginas en lenguaje de marcado de hipertexto (HTML) estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local desde su frontmatter mediante Drizzle, un mapeador objeto-relacional (ORM). No hay una interfaz de programación de aplicaciones (API) de backend independiente ni un framework de interfaz de usuario del lado del cliente. Las funciones auxiliares reutilizables para acceder a los datos están en `src/lib/`, y la interactividad se añade mediante un pequeño `<script>` de Astro con ámbito limitado que usa las API estándar del modelo de objetos del documento (DOM).

### Iniciar el sitio web

Para facilitar la ejecución del sitio, se ha incluido la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Puedes ejecutarla en tu GitHub Codespace siguiendo estos pasos:

1. Vuelve a tu codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre una ventana de terminal nueva en tu Codespace pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama nueva para este trabajo de filtrado y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta el siguiente comando para iniciar el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, debería aparecer un mensaje que indique la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pide abrir una ventana del navegador para `http://localhost:4321`, ciérralo seleccionando la **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando en el terminal la dirección del servidor Astro `http://localhost:4321`.

> [!NOTE]
> Si usas un codespace y seleccionas un enlace a la URL localhost en el terminal de Codespaces, se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado a tu codespace, donde ahora se ejecuta el servidor web.

### Explorar el sitio web

Con el sitio web en ejecución, puedes explorar sus funciones. Entre las principales se encuentran:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, el editor y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección de modo y modelo varía en cada ejercicio; los pasos correspondientes lo indicarán cuando sea necesario.

La implementación inicial del sitio web funciona, pero queremos mejorarla con nuevas funciones. Empecemos por revisar el backlog. Al configurar el proyecto a partir de la plantilla, se creó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te muestre esos elementos.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent**. El agente **Agent** planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos de terminal e invoca herramientas.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no muestran preferencia por un modelo concreto. Durante su desarrollo usamos Claude Sonnet 4.5, por lo que lo hemos incluido en las instrucciones. Esperamos que las sugerencias de código sean relativamente coherentes y ofrezcan una buena experiencia. Sin embargo, como los modelos de lenguaje de gran tamaño (LLM) son probabilísticos, es posible que las sugerencias que recibas difieran de las que se describen en el taller. Es normal y esperable.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, es posible que Copilot use otro comando MCP, pero debería poder completar la tarea igualmente.

3. Pide a Copilot que muestre las incidencias del backlog con el siguiente prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```
4. Selecciona **Continue** para ejecutar el comando que muestra todas las incidencias.
5. Revisa la lista generada.

Fíjate en que Copilot también ha priorizado los elementos según los que considera más útiles para el usuario.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere el código, conviene revisar el archivo de instrucciones que proporcionará contexto a Copilot. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que contiene información sobre cómo añadir funcionalidades al sitio web.

1. En tu codespace, ve a `.github/instructions/ui.instructions.md`.
2. Fíjate en las directrices generales para añadir funcionalidades, que incluyen:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a archivos de instrucciones específicos para varios tipos de archivos, entre ellos:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Los archivos de instrucciones permiten enlazar a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así puedes reutilizar instrucciones, dividir conjuntos complejos en partes más manejables y proporcionar ejemplos y plantillas.

## Implementar la funcionalidad de filtrado

Para completar el filtrado, habrá que realizar al menos tres cambios distintos en la aplicación:

- Añadir o mejorar la lógica de filtrado restante en la capa de datos (`src/lib/`).
- Añadir o actualizar las pruebas del comportamiento del filtrado.
- Actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de fusionar los cambios en el código. El modo agente de Copilot puede encargarse de estas tareas. Vamos a añadir la funcionalidad.

1. Puedes continuar la conversación actual con Copilot o iniciar otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de pantalla de un ejemplo de selección del archivo de instrucciones de interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog con el siguiente prompt:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot empieza por explorar el proyecto y localizar los archivos relacionados con la funcionalidad. Deberías ver que encuentra los asistentes de la capa de datos, la interfaz de usuario y las pruebas. Después, empieza a modificar los archivos y a ejecutar las pruebas.

   ![Captura de pantalla de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos, según la complejidad de la tarea y del código. Mientras trabaja, puede que aparezcan los botones **Keep** y **Undo** en el editor. Cuando termine, tendrás una opción **Keep** o **Undo** para cada cambio; no hace falta que selecciones ninguna mientras el trabajo siga en curso.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla de un cuadro de diálogo en el panel Copilot Chat que pide confirmación para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya algunas pausas y que incluso fallen algunas pruebas durante el proceso. No pasa nada. Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de pantalla de una sesión completa de Copilot Chat en modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado y busca posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código que genera Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio web. Explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has explorado cómo usar el modo agente de GitHub Copilot para añadir nuevas funciones al sitio web de Tailspin Toys. Has aprendido a:

- usar el modo agente de GitHub Copilot para implementar nuevas funcionalidades en la interfaz de usuario y la capa de datos.
- usar el modo agente de Copilot para explorar el proyecto, identificar los archivos pertinentes y hacer cambios coordinados.
- revisar los cambios y las pruebas generados por el modo agente de Copilot antes de fusionarlos en el código.

Ahora vamos a [probar la funcionalidad con el servidor Playwright MCP][next-lesson] y a abrir una pull request.

### Ejercicio de exploración adicional: implementar la paginación

A medida que crece la lista de juegos, será necesario implementar la paginación. Aplica lo que has aprendido en este ejercicio y pide a Copilot que actualice el sitio. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones actuales.
- decide cómo quieres implementar la paginación: por ejemplo, si el usuario podrá seleccionar el tamaño de página o si este será fijo.
- al crear el prompt, proporciona a Copilot las directrices necesarias para implementar la solución que quieres.
- puede que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Este es el flujo habitual de trabajo con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo se usan][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
