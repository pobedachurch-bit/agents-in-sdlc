---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisar el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le has pedido a Copilot que añada documentación al código, cree una funcionalidad de juegos relacionados para que el equipo de diseño pueda iterar sobre ella e implemente funciones de accesibilidad, incluidos los modos de alto contraste y claro. Vamos a explorar los cambios en el código y, si hace falta, a enviar comentarios a Copilot para que mejore su trabajo.

### Escenario

Como ya hemos señalado varias veces, añadir IA generativa no cambia los fundamentos del diseño de software ni de DevOps. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Con esto en mente, revisemos las propuestas de GitHub Copilot para crear la documentación, la funcionalidad de juegos relacionados y las funciones de accesibilidad antes de activar la revisión para el resto del equipo.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube de Copilot realiza tareas de forma asíncrona y sin supervisión, se han establecido ciertas restricciones de seguridad para garantizar que todo se mantenga protegido. Entre ellas se incluyen las siguientes:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usará para su código.
- El agente en la nube se ejecuta en GitHub Actions y crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar la ejecución de todos los flujos de trabajo de GitHub Actions.
- [El acceso a los recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores de Model Context Protocol (MCP).

## Revisar la documentación generada

Empezaremos por explorar la primera pull request (PR) que ha generado el agente en la nube de GitHub Copilot: la que añade documentación al código. Para ello, usarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Al explorar la PR, es posible que veas una advertencia que indica que el cortafuegos ha bloqueado GitHub Copilot. Es algo **esperado**, ya que Copilot tiene acceso limitado a los recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o desactivar el cortafuegos del agente en la nube de Copilot][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de pull requests.
3. Abre la pull request titulada algo parecido a **Add missing documentation** u otro título más descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la pull request tendrá la etiqueta **[WIP]**. Si es así, espera a que Copilot termine. Puede que tarde unos minutos; puedes aprovechar para descansar o repasar lo que has aprendido hasta ahora.

4. Cuando la pull request esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los nuevos comentarios de documentación TSDoc y otra documentación. Los cambios concretos pueden variar.

    Al revisar los cambios, busca comentarios de documentación TSDoc, convenciones de TypeScript y encabezados de comentarios. Estos proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que algunos flujos de trabajo están pendientes de aprobación.
8. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Deberías ver que los flujos de trabajo se ponen en cola en la sección de comprobaciones de la pull request. Si todo va bien, las comprobaciones del proyecto deberían superarse en la aplicación Astro. El proceso puede tardar unos minutos.

## Solicitar cambios a GitHub Copilot

Trabajar con Copilot en una pull request no es una interacción unidireccional. También puedes mencionar a Copilot en comentarios, como harías con otros miembros del equipo, tanto en la pull request como en comentarios insertados en el código. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos darte un texto exacto que debas usar. Algunas ideas de cambios que puedes pedir a Copilot:

- Añadir encabezados de comentario al principio de cada archivo de código con una breve descripción de su contenido.
- Añadir comentarios de documentación TSDoc a los archivos TypeScript y Astro.
- Crear un archivo README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier usuario. Usa una de las ideas anteriores u otra sugerencia sobre la documentación que quieras incluir en el código.
2. Selecciona **View Session** para observar cómo trabaja Copilot. Fíjate en que inicia una sesión nueva para realizar las actualizaciones.
3. Puedes seleccionar **Back to pull request** para volver a la pull request.

    ![Volver a la pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine los cambios, debería aparecer un commit nuevo en la pull request.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Cuando lo estés, puedes marcar la PR como lista para revisión y fusionarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la funcionalidad de juegos relacionados

Volvamos a la PR que Copilot ha generado para resolver la incidencia sobre cómo mostrar juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Show related games on the game details page** u otro título más descriptivo.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que señala que algunos flujos de trabajo están pendientes de aprobación.
7. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Deberías ver que los flujos de trabajo se ponen en cola en la sección de comprobaciones de la pull request. Si todo va bien, las comprobaciones del proyecto deberían superarse en la aplicación Astro. El proceso puede tardar unos minutos.
9. **Opcional:** también puedes cambiar a esta rama en tu Codespace para probar manualmente la funcionalidad de juegos relacionados. Ve a tu Codespace, abre el terminal y ejecuta los siguientes comandos (sustituye `<branch-name>` por el nombre de la rama que ha creado Copilot, por ejemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la funcionalidad de juegos relacionados. Como antes, puedes trabajar de forma iterativa con el agente en la nube de Copilot para solicitar actualizaciones. Por ejemplo, podrías pedirle que cambie el número de juegos relacionados que se muestran o que añada encabezados de comentario y comentarios de documentación TSDoc (recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas). Al igual que antes, puedes hacer estas solicitudes con un comentario nuevo en la pestaña **Conversation**. Copilot lo verá e iniciará una sesión nueva.

## Revisar las funciones de accesibilidad

Por último, revisaremos las funciones de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta PR debería incluir tanto el modo de alto contraste asignado en el ejercicio 3 como el modo claro solicitado desde mission control en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Add high contrast mode to website** u otro título más descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la pull request tendrá la etiqueta **[WIP]**. Si es así, espera a que Copilot termine. Puede que tarde unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a los aspectos siguientes:
   - los componentes de interfaz de usuario para cambiar de modo.
   - el uso del almacenamiento local para conservar las preferencias del usuario.
   - los cambios de CSS o de estilo correspondientes a los modos de alto contraste y claro.
   - los atributos de accesibilidad (etiquetas ARIA, navegación con el teclado, etc.).
   - el código JavaScript/TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que algunos flujos de trabajo están pendientes de aprobación.
8. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Deberías ver que los flujos de trabajo se ponen en cola en la sección de comprobaciones de la pull request. Si todo va bien, las comprobaciones del proyecto deberían superarse en la aplicación Astro. El proceso puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente las funciones de accesibilidad. Ve a tu Codespace, abre el terminal y ejecuta los siguientes comandos (sustituye `<branch-name>` por el nombre de la rama que ha creado Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los modos de alto contraste y claro en el navegador para asegurarte de que funcionan como se espera y se mantienen después de volver a cargar la página.

Fíjate en cómo el agente personalizado de accesibilidad ha ayudado a orientar a Copilot para que implemente estas funciones siguiendo las prácticas recomendadas de accesibilidad. Si detectas algún problema o mejora de accesibilidad, puedes mencionar a **@copilot** en un comentario para solicitar actualizaciones, como hiciste con las PR anteriores.

## Ejercicio opcional: seguir delegando

El agente en la nube funciona mejor cuando puedes asignarle incidencias reales del backlog y ocuparte de otra cosa. Para adquirir esa costumbre, crea varias incidencias más en el repositorio y asígnaselas a Copilot. Algunas ideas:

- Crear un formulario para que los patrocinadores expresen su interés en la página de detalles del juego.
- Implementar la paginación en la página con la lista de juegos.
- Añadir validación de entradas y gestión de errores a las funciones auxiliares de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizadas incluidos en el repositorio** para ver más adelante su efecto en el resultado del agente en la nube.
- **Has asignado incidencias al agente en la nube de Copilot** y has visto cómo configura el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y usado un agente personalizado** de accesibilidad para añadir los modos de alto contraste y claro.
- **Has usado la página de agentes de Copilot como centro de control** para supervisar y dirigir la sesión de accesibilidad mientras estaba en curso.
- **Has revisado e iterado sobre las pull requests del agente en la nube**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Revisión y siguientes pasos

Has completado el recorrido del agente en la nube. Si quieres seguir explorando, los otros recorridos complementan lo que has practicado aquí:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración con Model Context Protocol (MCP) directamente desde el entorno de desarrollo integrado (IDE).
- 💻 **[Recorrido de la interfaz de línea de comandos (CLI)](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y un puente opcional de `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente en la nube una incidencia de refactorización o de cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]
- [Configurar el cortafuegos del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
