---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores has pedido al agente en la nube de Copilot que se ocupe de tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente en la nube está diseñado para trabajar de forma autónoma y asíncrona, es importante poder supervisar estas tareas.

Tienes varias herramientas para gestionar las tareas asignadas al agente en la nube, como la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas del agente que tienen pull requests (PR) abiertas. También puedes explorar las operaciones realizadas e incluso dirigir una sesión en curso.

En esta lección, aprenderás a:

- explorar la página de agentes para supervisar las tareas del agente en la nube.
- dirigir una sesión en curso para pedir que se añada otra funcionalidad.

## Escenario

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo se dio cuenta de que también sería un buen momento para añadir un modo claro. Como ya se estaba trabajando en actualizar el estilo del sitio y añadir la opción para cambiar de modo, incluir esa funcionalidad parecía lógico. Quieres dirigir el trabajo del agente para asegurarte de que añada el modo claro además del modo de alto contraste.

## Revisar las tareas del agente en la nube de Copilot

Vamos a consultar el estado actual de todas las tareas asignadas al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Fíjate en la lista de tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones que ha realizado Copilot. Pueden incluir:
   - descargar el código del repositorio.
   - crear el entorno de trabajo de Copilot.
   - configurar servidores MCP.
   - realizar varios pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos concretos que se muestran dependerán del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Fíjate también en el panel de pull requests (PR) que aparece a la derecha. Desde ahí puedes consultar la PR y los archivos modificados para seguir el trabajo.

## Dirigir el agente en la nube

Ahora que has visto las tareas activas, vamos a pedir a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que Copilot haya usado y del estado actual del trabajo.

    ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de configurar el entorno y ha empezado a trabajar. Sabrás que esto ha ocurrido cuando empiecen a aparecer mensajes parecidos a los que se muestran a continuación.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, añade el siguiente prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de pantalla de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para dirigir una sesión activa de Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que ha recibido el prompt y lo incorpora a su flujo de trabajo.

## Dejar que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud a su flujo de trabajo cuando termine el paso que estaba realizando en el momento en que enviaste el mensaje.

Como antes, el proceso tardará varios minutos. Es un buen momento para hacer una pausa y reflexionar sobre todo lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro principal para supervisar y dirigir las tareas del agente en la nube de GitHub Copilot. Desde este centro de control puedes seguir las tareas activas y completadas, revisar el trabajo que se está realizando e incluso redirigir las tareas en curso para ajustar su alcance o proporcionar más indicaciones.

Has explorado estos conceptos:

- explorar mission control y la página de agentes para supervisar las tareas del agente en la nube.
- redirigir una sesión en curso para solicitar otra funcionalidad.

Mientras Copilot termina su trabajo en las funciones de accesibilidad, puedes [iterar sobre las pull requests que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
