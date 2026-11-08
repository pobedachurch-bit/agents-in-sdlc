---
title: "Lección 8 - Crear y fusionar la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de las instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Es el momento de revisarlo todo junto y abrir una pull request. Tú mismo fusionaste la pull request (PR) de la valoración con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente, dividiríamos la funcionalidad, las actualizaciones de las instrucciones, la actualización de la habilidad y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el trabajo de filtrado y del flujo de calidad en una conversación y una rama, y lo incluirás en esta PR.

En esta lección, aprenderás a:

- entender qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- inspeccionar el cambio completo de la funcionalidad y las pruebas de verificación.
- crear la PR del filtrado.
- activar Agent Merge solo después de la revisión y confirmar que la PR se ha fusionado.

## Escenario

Durante el trabajo en la funcionalidad de filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener bajo el control del desarrollador la autorización para fusionarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una pull request. Cuando lo activas, Copilot resuelve los bloqueos de la PR: corrige las comprobaciones de integración continua (CI) que fallan, responde a los comentarios de revisión y reaplica los cambios sobre la rama base cuando hace falta. Después, activa la fusión automática de GitHub si el repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esa tarea. Revisa el trabajo y decide si está listo antes de activar Agent Merge.

## Usar Agent Merge para gestionar la PR

Ya has creado todo el código. Vamos a revisarlo, crear la PR y dejar que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo del agente introduciendo `/agent`.
3. Selecciona **Default** en la lista de opciones y pulsa <kbd>Enter</kbd>.
4. Crea una PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todas las comprobaciones se superen, realizará la fusión.
7. Asegúrate de ver un mensaje de Copilot parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no elude las aprobaciones obligatorias, la protección de ramas, las colas de fusión, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo que se indica y completa manualmente la fusión revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: la generación y validación del código, y ahora el proceso de la pull request. En esta lección, has:

- aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- inspeccionado el cambio completo de la funcionalidad y las pruebas de verificación.
- creado la PR del filtrado.
- activado Agent Merge solo después de la revisión y confirmado que la PR se ha fusionado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Recursos

- [Gestionar pull requests con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
