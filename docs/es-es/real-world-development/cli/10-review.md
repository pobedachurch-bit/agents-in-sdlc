---
title: "Lección 10 - Recapitulación y siguientes pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos pull requests, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI en un flujo de trabajo continuo con Tailspin Toys. Has:

- preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado inicial.
- añadido las valoraciones con estrellas, revisado el resultado en un navegador con un puerto reenviado y fusionado manualmente tu primera pull request (PR).
- partido de la incidencia de filtrado, definido el enfoque en el modo Plan, creado la funcionalidad en el modo Autopilot y revisado el resultado en el modo Interactive.
- guiado al agente con instrucciones personalizadas, personalizado la habilidad existente `quality-checks` y usado esa habilidad para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- añadido el servidor Model Context Protocol (MCP) de Playwright y usado el servidor para explorar el filtrado en un navegador real.
- creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas en el navegador.
- revisado todo el cambio de filtrado y autorizado Agent Merge para la PR correspondiente.
- explorado comandos de barra para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Lo que has publicado

El taller incluye dos hitos de PR, cada uno en su propia rama creada a partir de una versión actualizada de `main`:

1. **Valoraciones con estrellas:** mostrar el valor existente de `starRating` y un estado explícito para los juegos sin valorar en las tarjetas.
2. **Filtrado y flujo de calidad:** implementar el filtrado, actualizar las instrucciones y aplicarlas a la funcionalidad, personalizar el informe de `quality-checks`, crear un perfil de QA e incluir las pruebas asociadas.

Desde la planificación del filtrado hasta la apertura de su PR, has usado la misma conversación y rama. Para agilizar el taller, hemos incluido todo ese trabajo en una sola PR.

## Distintos tipos de verificación

Has comprobado el código de varias formas: con pruebas automatizadas, examinándolo tú mismo en el navegador y pidiendo a Copilot que lo explorara en un navegador mediante MCP. La habilidad `quality-checks` ha ejecutado pruebas unitarias, lint y comprobaciones de tipos, y ha incluido los resultados en el nuevo formato. QA ha reunido esos resultados con una revisión de los requisitos y de la cobertura de las pruebas antes de abrir la PR.

Las pruebas nuevas deben cubrir carencias reales. Si una ejecución de QA no requiere pruebas nuevas, puede ser correcta. Revisa el código y las evidencias antes de autorizar la fusión, y actualiza las evidencias afectadas después de los cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas a Copilot influyen en su trabajo. En este taller, has actualizado las instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones en distintas conversaciones y ajústalas a medida que cambien las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios reales y los resultados de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para estudiar un enfoque antes de empezar a implementarlo, **Interactive** para seguir participando en cambios concretos y **Autopilot** para tareas bien delimitadas. Elige un modelo más rápido para los cambios rutinarios y otro más capaz para el trabajo complejo.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* influye de forma significativa en el resultado.

## Más cosas por explorar

Ya has completado el flujo principal. Aquí tienes otras funciones de CLI que puedes explorar:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otro punto de vista.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar una tarea distinta.
- `/delegate` para enviar una tarea a Copilot cloud agent.

## Siguientes pasos

La mejor manera de mejorar con cualquier herramienta es seguir usándola. Utilízala para código de producción, proyectos personales o esa pequeña aplicación que llevas años pensando en crear. Comparte lo que aprendas con tu equipo y aprende de lo que descubran los demás. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la [serie opcional Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller de Copilot cloud agent][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar pull requests con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
