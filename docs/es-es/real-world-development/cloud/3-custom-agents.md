---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot te permiten crear asistentes de inteligencia artificial (IA) especializados para tareas o ámbitos concretos de tu flujo de desarrollo. Al definir agentes mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar a Copilot instrucciones específicas, prácticas recomendadas, patrones de programación y conocimientos del dominio para que realice ciertos tipos de trabajo de forma más eficaz. Los equipos pueden plasmar su experiencia en agentes reutilizables: un agente de accesibilidad que garantice el cumplimiento de las [pautas de accesibilidad para el contenido web (WCAG, por sus siglas en inglés)][wcag], un agente de seguridad que siga prácticas de programación segura o un agente de pruebas que mantenga patrones de pruebas coherentes.

Los agentes personalizados se definen mediante archivos Markdown en la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML que incluye al menos `name` y `description`, seguido de un prompt en Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Diferencias entre los agentes personalizados y las habilidades de agente

Hay cierto solapamiento entre los agentes personalizados y las [habilidades de agente][agent-skills-concept]. Ambos se definen principalmente mediante archivos Markdown y explican a una IA cómo realizar tareas. La manera más sencilla de distinguirlos es esta: un **agente personalizado** es el trabajador y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar habilidades (e incluso otros agentes) durante el trabajo. En este taller, el agente personalizado de accesibilidad revisa y actualiza el sitio según las directrices de accesibilidad. Como parte de su trabajo, podría usar habilidades como una dedicada al flujo de trabajo de pull requests o a ejecutar y gestionar pruebas.

> [!NOTE]
> No hay una única manera correcta de crear un agente personalizado. Como ocurre con cualquier herramienta de IA, prueba distintas opciones e itera para encontrar la que mejor se adapte a tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Con los agentes personalizados, explorarás lo siguiente:

- cómo se definen los agentes personalizados.
- cómo asignar una tarea a un agente personalizado.

## Escenario

Tailspin Toys se ha comprometido a que su plataforma de micromecenazgo sea accesible para todos los usuarios, independientemente de sus capacidades o preferencias visuales. Los comentarios recientes de los usuarios indican que algunas personas tienen dificultades para leer el tema oscuro actual debido al contraste insuficiente entre los colores del texto y del fondo. Para resolver este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que los usuarios puedan activar y desactivar.

Como la accesibilidad es fundamental, quieres implementar esta funcionalidad lo antes posible. Para generar la funcionalidad, usarás un agente personalizado.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Vamos a revisar su contenido para entender cómo orientará a Copilot.

Vuelve a tu codespace y revisa el archivo del agente personalizado de accesibilidad:

1. Abre `.github/agents/accessibility.md`.
2. Fíjate en el frontmatter YAML, que incluye los campos `name` y `description`.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. A continuación, examina las secciones siguientes, que destacan:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores y problemas habituales.

## Crear y asignar una incidencia

Mission control es el centro desde el que puedes trabajar con todos los agentes de tu entorno. Puedes asignar tareas al agente en la nube de Copilot, supervisarlas e incluso redirigir una sesión en curso o proporcionarle indicaciones adicionales. Vamos a asignar a Copilot la tarea de crear el modo de alto contraste.

1. Ve a tu repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue** para crear la incidencia.
5. Escribe `Add high contrast mode to website` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.
9. Selecciona **Accessibility agent** en la lista de agentes personalizados.

    ![Captura de pantalla de la asignación al agente en la nube, con el agente personalizado y Accessibility resaltados](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en la tarea en segundo plano.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes de IA especializados para tareas y ámbitos concretos. Los agentes personalizados te permiten plasmar las prácticas y los estándares del equipo en agentes reutilizables que orientan a Copilot para que realice ciertos tipos de trabajo de forma más eficaz.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- cómo asignar una tarea a un agente personalizado.

Mientras Copilot trabaja en el modo de alto contraste, puedes [supervisar y dirigir la sesión del agente][next-lesson] desde mission control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparar la organización para usar agentes personalizados][org-custom-agents]
- [Preparar la empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lección anterior: Agente en la nube de GitHub Copilot][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
