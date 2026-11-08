---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando usas el modo agente de GitHub Copilot, este trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tiempo real en tu equipo, Copilot Chat en VS Code muestra cada llamada a una herramienta y cada edición de archivo a medida que se producen. Puedes revisar cada diff en línea, aceptar o rechazar cambios concretos y orientar la conversación con prompts de seguimiento para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio, aprenderás a:

- supervisar en tiempo real el trabajo del agente de accesibilidad.
- revisar y aceptar las ediciones de archivos propuestas.
- dirigir al agente a mitad de una sesión para añadir un selector del modo claro junto al modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que has usado en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha pedido otra función: un selector del modo claro para los usuarios a quienes les resulte demasiado oscuro el tema predeterminado. En vez de empezar otra conversación desde cero, orientarás la sesión existente para ampliar el trabajo.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en línea en el chat con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a una herramienta. Verás lecturas de archivos, ediciones y los comandos de terminal que solicita ejecutar.
3. Cuando el agente proponga una edición, aparecerá un diff en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertir el cambio y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos modificados por el agente durante esta sesión.

> [!NOTE]
> Si una llamada a una herramienta solicita permiso (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos con los que estés de acuerdo.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías ver un mensaje final que resuma los cambios realizados.

## Pedir al agente que añada el modo claro

Ahora que ya está implementado el modo de alto contraste, ampliarás la misma conversación para añadir un selector del modo claro. Como continuarás en la misma sesión de chat, el agente conservará todo el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilo y el patrón de almacenamiento local.

1. En el cuadro de entrada de Copilot Chat, envía el siguiente prompt:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente continuará desde donde lo dejó, explorará los cambios realizados y propondrá modificaciones para añadir el modo claro.
3. Revisa cada nueva edición a medida que aparezca, igual que hiciste con el modo de alto contraste. Usa **Keep** o **Undo** para cada diff.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, si intenta refactorizar la interfaz de los selectores de una forma que no te convence—, puedes detenerlo con el botón **Stop** junto al cuadro de entrada del chat y enviar un mensaje de seguimiento para aclarar lo que necesitas.

> [!TIP]
> Dirigir una sesión en curso es más rápido que empezar de nuevo. Usa prompts de seguimiento para pedir ajustes («simplifica el CSS: usa variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer selector; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos selectores»).

## Revisar el conjunto de cambios

Antes de crear un commit con el trabajo, revisa rápidamente todos los cambios realizados por el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre algunos de los archivos modificados y examina los diff. Confirma que siguen los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación con el teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la lección siguiente][next-lesson].

## Resumen y siguientes pasos

Has usado el modo agente de Copilot Chat para supervisar y dirigir una sesión en curso: has revisado cada edición de archivo a medida que aparecía, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección has explorado estos conceptos:

- supervisar el trabajo de un agente en tiempo real.
- aceptar y rechazar ediciones de archivos concretas.
- dirigir una sesión en curso mediante prompts de seguimiento.

En [la lección siguiente][next-lesson], publicarás tu trabajo de accesibilidad en una pull request y revisarás los cambios que ha generado el agente personalizado local durante este taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
