---
title: "Lección 4 - Guiar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es clave cuando trabajas con inteligencia artificial (IA) generativa. Si una tarea debe realizarse de una forma concreta, conviene que Copilot tenga esas indicaciones disponibles. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que ya has creado el filtrado, explorarás las instrucciones que ha usado Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección, aprenderás a:

- explorar cómo llegan al agente las instrucciones del repositorio y los archivos de instrucciones aplicables a rutas concretas.
- actualizar el archivo de instrucciones para garantizar que se sigan los estándares de programación.
- observar el efecto de los archivos de instrucciones en el código.

## Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo. Entre ellos se incluyen los siguientes:

- Los comentarios deben explicar la intención y las decisiones que no sean obvias, no repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, sus parámetros y sus valores de retorno con TSDoc/JSDoc, incluido el argumento `db` inyectable cuando exista.
- Los componentes reutilizables de Astro deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Se deben conservar las directrices de formato y lint existentes.

Con los archivos de instrucciones, te asegurarás de que Copilot tenga la información necesaria para realizar las tareas de acuerdo con estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Esta función te permite orientar las sugerencias y los fragmentos de código para que se ajusten mejor a tus necesidades. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot con **todas** las solicitudes del repositorio. Debe contener información del proyecto que sea pertinente para la mayoría de las solicitudes.
- Los archivos `.github/instructions/*.instructions.md`, que proporcionan directrices para determinados lenguajes, tipos de archivos o tareas.

> [!NOTE]
> La disponibilidad de otros formatos de instrucciones varía según el entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explorar los archivos de instrucciones personalizadas del proyecto

El proyecto inicial ya incluye varios archivos de instrucciones. Antes de hacer cambios, vamos a explorar los que hay para ver qué efecto tienen.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces, no en el terminal, abre `.github/copilot-instructions.md`.
3. Explora el archivo y fíjate en la breve descripción del proyecto y en las directrices de programación. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora los archivos. Observa que hay instrucciones para archivos Astro, la capa de datos de Drizzle, las pruebas y otros ámbitos.
5. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: define un patrón glob que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y fíjate en los enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar ejemplos que Copilot puede seguir.

## Actualizar los archivos de instrucciones según las directrices del equipo

Los archivos existentes son un buen punto de partida, pero falta una directriz. Modificaremos el archivo principal `copilot-instructions.md` para asegurarnos de que el nuevo código TypeScript incluya comentarios TSDoc.

1. En `.github/copilot-instructions.md`, localiza la sección **Code formatting guidance**, que debería estar alrededor de la línea 35.
2. Añade el siguiente elemento al final de la lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Aplicar las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar las instrucciones actualizadas, cierra y retoma la conversación de filtrado para que Copilot cargue las directrices nuevas sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, introduce `/exit` para cerrarla. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio, en la rama `game-filters-cli`, retoma la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está seleccionada la rama `game-filters-cli`.
3. Pide a Copilot que aplique las directrices actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Introduce `/diff` y revisa los archivos TypeScript modificados. Observa los comentarios TSDoc nuevos y confirma que explican el código correctamente.

## Resumen y siguientes pasos

Has explorado cómo Copilot CLI obtiene el contexto de los archivos de instrucciones y has aplicado un estándar nuevo a la funcionalidad. En concreto, has:

- explorado cómo llegan al agente las instrucciones del repositorio y los archivos de instrucciones aplicables a rutas concretas.
- actualizado el archivo de instrucciones para garantizar que se sigan los estándares de programación.
- observado el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable `quality-checks`][next-lesson] para asegurarte de que el lint y las pruebas se ejecutan de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
