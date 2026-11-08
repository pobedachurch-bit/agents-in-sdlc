---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es clave cuando trabajas con inteligencia artificial (IA) generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita conocer información de fondo, asegúrate de que tenga acceso a ese contexto. Hay varias formas de compartir contexto específico con Copilot. Una de las principales son los [archivos de instrucciones][instruction-files], que te permiten orientar la generación de código.

En este ejercicio, aprenderás a:

- explorar cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones aplicables a rutas concretas.
- generar la primera función auxiliar de acceso a datos para el filtrado (para obtener editores) con las instrucciones *actuales*.
- añadir un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`.
- volver a ejecutar el mismo prompt y observar cómo el código generado adopta el nuevo estándar.
- crear un commit con la actualización de las instrucciones y el primer componente del filtrado en `main` para que Copilot pueda usar las directrices actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir algunos de los estándares que hayas definido. Copilot no es determinista. El objetivo de este ejercicio es observar la *tendencia* de los cambios en el comportamiento después de actualizar las instrucciones, no obtener exactamente el mismo resultado carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo. Entre ellos se incluyen los siguientes:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- Se debe añadir documentación al código mediante comentarios TSDoc.
- Al principio de cada archivo se debe añadir un bloque de comentarios que describa su contenido.

Con los archivos de instrucciones, te asegurarás de que Copilot tenga la información necesaria para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Esta función te permite orientar las sugerencias y los fragmentos de código para que se ajusten mejor a tus necesidades. Puedes especificar las convenciones de programación y las bibliotecas que prefieres, e incluso los tipos de comentarios que quieres incluir en el código. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot con **todas** las solicitudes del repositorio. Debe contener información del proyecto pertinente para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción general del proyecto, las prácticas recomendadas y otras directrices generales.
- Puedes crear archivos `.github/instructions/*.instructions.md` para tareas o tipos de archivos concretos. Sirven para proporcionar directrices sobre lenguajes específicos, como TypeScript o Astro, o sobre tareas, como crear un componente de interfaz de usuario o un nuevo conjunto de pruebas unitarias.

> [!NOTE]
> En el entorno de desarrollo integrado (IDE), los archivos de instrucciones solo se usan para generar código con Copilot Chat, no para completar código ni para las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que incluyen el campo `applyTo` en el frontmatter.
>
> Copilot también [admite archivos de instrucciones de otros estándares][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Explicar detalladamente cómo crear archivos de instrucciones queda fuera del ámbito del taller. Sin embargo, los ejemplos del proyecto muestran un enfoque representativo. A grandes rasgos:

- Mantén las instrucciones de `copilot-instructions.md` centradas en directrices para todo el proyecto, como una descripción de lo que se está desarrollando, la estructura del proyecto y los estándares generales de programación.
- Usa archivos `*.instructions.md` para proporcionar instrucciones específicas sobre tipos de archivos (pruebas unitarias, componentes Astro o la capa de datos) o tareas concretas.
- Usa lenguaje natural y redacta directrices claras. Incluye ejemplos de cómo debería ser el código y de cómo no debería serlo.

No hay una única manera correcta de crear archivos de instrucciones, igual que no hay una única manera de usar la IA. La experimentación te ayudará a descubrir qué funciona mejor en tu proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían tener una colección completa de archivos de instrucciones. Al explorar los de este proyecto, puede que veas archivos para muchos tipos de tareas, como [actualizaciones de la interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta opción de una forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI). La lección correspondiente al entorno que estés usando explicará cómo hacerlo cuando sea pertinente.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas de este proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**. Estas instrucciones se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Observa que hay instrucciones para archivos Astro, la capa de datos de Drizzle, las pruebas y otros ámbitos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: define un patrón glob (relativo a la raíz del repositorio) que determina a qué archivos se aplican las instrucciones. En este caso, coincide con cualquier archivo de pruebas TypeScript, por ejemplo, los que sigan el patrón `**/*.test.ts`.
5. Fíjate en las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar a Copilot ejemplos que puede seguir al generar código. (Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio).

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero todavía no exige documentar el código. En los pasos siguientes añadirás reglas para los comentarios de documentación TSDoc y los encabezados de comentarios de los archivos.

## Crear una rama para los cambios

Vamos a seguir algunas prácticas recomendadas y crear una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección de modo y modelo varía en cada ejercicio; los pasos correspondientes lo indicarán cuando sea necesario.

Para observar el efecto de las instrucciones personalizadas, empieza enviando un prompt con el archivo de instrucciones actual. Más adelante, lo actualizarás y volverás a enviar el mismo prompt para comparar el resultado.

1. Cierra las pestañas abiertas del editor de los ejercicios anteriores para que Copilot use solo el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe estar la función auxiliar.
3. En el menú desplegable de agentes de la vista Chat, selecciona **Agent** para que Copilot pueda aplicar cambios a los archivos.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía el siguiente prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explora el proyecto y aplica cambios al código, a menudo tanto al archivo de la función auxiliar como a sus pruebas.
6. Fíjate en que la función auxiliar propuesta es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editores. Esto se debe a las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios de documentación TSDoc y el encabezado de comentario del archivo.

> [!CAUTION]
> Como Copilot funciona de forma probabilística, es posible que añada comentarios de documentación aunque no se lo pidas. No pasa nada: lo importante es observar la mejora de **coherencia** después de actualizar las instrucciones.

## Añadir un nuevo estándar para el repositorio

Como ya hemos indicado, `.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Vamos a documentar los estándares de programación del repositorio para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Fíjate en que documenta los estándares de programación del proyecto, pero todavía no incluye ninguna regla para documentar el código. Por eso la función auxiliar generada no tenía comentarios de documentación.
3. Añade las siguientes líneas de Markdown justo debajo de los estándares existentes para indicar a Copilot que añada encabezados de comentario a los archivos y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones en el nivel del repositorio (`.github/copilot-instructions.md`) para proporcionar directrices generales, o archivos `*.instructions.md` para lenguajes, tipos de archivos o tareas concretas. El archivo del repositorio es el lugar adecuado para incluir estándares generales, como la regla sobre los comentarios de documentación que acabas de añadir.

## Volver a ejecutar el prompt y observar el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para limpiar el búfer.
2. Vuelve a seleccionar `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
3. Envía el **mismo prompt** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Fíjate en que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Acabas de orientar a Copilot para que siga un estándar nuevo y lo aplique al código real sobre el que se trabajará en el siguiente ejercicio.

## Crear un commit con las instrucciones y subir la rama

Los archivos de instrucciones, como cualquier otro recurso del repositorio, se gestionan mediante el mismo sistema de control de versiones. Vamos a crear un commit con los cambios y subir la rama al repositorio.

1. Abre una ventana de terminal nueva pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Desde el terminal, confirma que están presentes las actualizaciones de las instrucciones y de la función auxiliar ejecutando:

   ```bash
   git status
   ```

3. Desde el terminal, prepara los cambios y crea un commit con la actualización de las instrucciones y todos los archivos que Copilot haya modificado para crear la base de la función auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Desde el terminal, sube la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y fusionar una pull request

Ya has subido la rama. Ahora conviene crear una pull request (PR) y vincularla a la incidencia del backlog sobre el estándar de documentación. Podrías hacerlo manualmente, pero Copilot puede encargarse mediante las herramientas de GitHub que ya están conectadas al proyecto. Pídele que busque la incidencia, cree una PR que la cierre y, después, la fusione.

> [!NOTE]
> En este ejercicio, Copilot realizará por primera vez acciones en GitHub en tu nombre. La plantilla del proyecto ya conecta las herramientas de GitHub a tu espacio de trabajo, así que no tienes que configurar nada. La primera vez que Copilot use una de ellas, es posible que VS Code te pida que inicies sesión en GitHub. Sigue las instrucciones para permitirlo. En un ejercicio posterior aprenderás cómo funciona esta conexión mediante Model Context Protocol (MCP).

1. Abre Copilot Chat en tu codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado en el selector de agentes.
3. Pide a Copilot que busque la incidencia relacionada con la actualización de los archivos de instrucciones y cree una PR desde la rama actual, describiendo tanto la actualización de las instrucciones como la nueva base de la función auxiliar para editores:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot empezará a buscar la incidencia y crear la PR.
5. Cuando se te pida que **Allow** a Copilot realizar acciones de GitHub en tu nombre, revisa el comando y selecciona **Allow** si corresponde.
6. Cuando se cree la PR, pide a Copilot que la fusione y que vuelva a `main` con el siguiente prompt:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando se te pida que **Allow** a Copilot realizar acciones de GitHub y ejecutar comandos de shell en tu nombre, revisa el comando y selecciona **Allow** si corresponde.

¡Ya has creado y fusionado una pull request con ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo obtiene Copilot el contexto de los archivos de instrucciones de este proyecto y después has usado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones *existentes*.
- añadir un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`.
- volver a ejecutar el mismo prompt y observar cómo el código propuesto adopta el nuevo estándar.
- crear un commit en `main` con la actualización de las instrucciones y la base de la función auxiliar para que el siguiente ejercicio pueda aprovecharlas.

A continuación, aplicarás esas instrucciones en el [modo agente][next-lesson] mientras Copilot añade una funcionalidad nueva en varios archivos del código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [5 consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
