---
title: "Lección 9 - Comandos de barra en GitHub Copilot CLI"
description: "Explora los comandos de barra para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar tareas opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de CLI, GitHub Copilot CLI incluye muchos comandos de barra con los que puedes interactuar. Estos comandos ofrecen funciones avanzadas, información interna y opciones de configuración adicionales. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Vamos a explorar otros comandos útiles.

En esta lección, aprenderás a:

- usar `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- usar `/model` para explorar los modelos disponibles.
- descubrir cómo `/share` puede exportar o compartir una sesión.
- explorar comandos opcionales para trabajar en paralelo, usar worktrees y delegar tareas al agente en la nube.

## Escenario

Ya has terminado el flujo principal de CLI. Ahora veremos algunas funciones adicionales: gestionar el contexto, cambiar de modelo, compartir sesiones y delegar tareas opcionalmente a [Copilot cloud agent][about-cloud-agent].

## Explorar el contexto de Copilot CLI

Al trabajar en tareas grandes o complejas, puede que alcances la ventana de contexto máxima del modelo. Copilot CLI compacta automáticamente la conversación cuando hace falta, y puedes inspeccionar o compactar el contexto tú mismo con los comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si todavía no está abierto.
2. Introduce:

   ```plaintext
   /context
   ```

3. Fíjate en el modelo, el uso actual de tokens y cómo se distribuye el contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a introducir `/context` y compara el resultado. Si la conversación ya es corta, puede que apenas haya cambiado.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto a medida que se llena la ventana. Usa `/compact` si quieres decidir cuándo hacerlo. Usa `/clear` o `/new` para empezar una conversación nueva cuando cambies a una tarea sin relación con la anterior.

## Elegir un modelo

Cada modelo tiene puntos fuertes distintos y cada desarrollador tiene sus propias preferencias. Copilot CLI permite mostrar los modelos disponibles y seleccionar el que quieras usar.

1. Introduce:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información sobre su uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Compartir una sesión

Trabajar en equipo y compartir lo aprendido ayuda a todo el mundo a usar mejor las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un GitHub gist.

1. Introduce `/help` y revisa las opciones de `/share` de la versión instalada.
2. Si quieres compartir esta sesión, elige el destino que mejor se adapte a tus necesidades, por ejemplo, `/share file` para exportar un archivo Markdown local.
3. Revisa el contenido exportado antes de enviárselo a alguien o publicarlo. Las exportaciones de sesiones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de la conversación que tu equipo no tenga previsto compartir.

## Opcional: ampliar el trabajo o delegarlo

Has terminado el taller principal. Copilot CLI también ofrece comandos para tareas más grandes:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un worktree de Git aislado para una tarea distinta.
- `/delegate` puede enviar una tarea a Copilot cloud agent, que trabaja de forma asíncrona y puede abrir una pull request.

Estos comandos son opcionales porque pueden crear worktrees adicionales o iniciar trabajo remoto. Antes de probarlos, empieza una tarea nueva y bien delimitada, y revisa el resultado con tu flujo habitual. Si quieres profundizar en el trabajo asíncrono con agentes, continúa con el [taller de cloud agent][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra de Copilot CLI te permiten configurarlo, compartir sesiones y ver lo que ocurre entre bastidores. En esta lección, has:

- usado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- usado `/model` para explorar los modelos disponibles.
- descubierto cómo `/share` puede exportar o compartir una sesión.
- explorado comandos opcionales para trabajar en paralelo, usar worktrees y delegar tareas al agente en la nube.

Copilot CLI incluye más comandos de barra y ofrece mucho más por explorar. Para terminar el recorrido, [repasarás lo que has aprendido][next-lesson] y verás los siguientes pasos para seguir aprendiendo.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca de Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
